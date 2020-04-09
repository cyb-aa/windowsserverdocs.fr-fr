---
ms.assetid: 13210461-1e92-48a1-91a2-c251957ba256
title: Résolution des problèmes liés aux mises à jour du microprogramme des lecteurs
ms.prod: windows-server
ms.author: toklima
manager: masriniv
ms.technology: storage
ms.topic: article
author: toklima
ms.date: 04/18/2017
ms.openlocfilehash: b62fdfe64ea579f61239dc582c639fb10ec1371c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820882"
---
# <a name="troubleshooting-drive-firmware-updates"></a>Résolution des problèmes liés aux mises à jour du microprogramme des lecteurs

>S’applique à : Windows 10, Windows Server (canal semi-annuel)

Windows 10, version 1703 ou ultérieure, et Windows Server (canal semi-annuel) permettent de mettre à jour le microprogramme de disques durs et de disques SSD qui ont été certifiés à l’aide du qualificateur supplémentaire Firmware Upgradeable (microprogramme pouvant être mis à niveau) via PowerShell.

Vous trouverez plus d’informations sur cette fonctionnalité ici :

- [Mise à jour du microprogramme de lecteur dans Windows Server 2016](update-firmware.md)
- [Mettez à jour le microprogramme de lecteur sans temps d’arrêt dans espaces de stockage direct](https://channel9.msdn.com/Blogs/windowsserver/Update-Drive-Firmware-Without-Downtime-in-Storage-Spaces-Direct)

Les mises à jour du microprogramme peuvent échouer pour diverses raisons. Cet article vous aide à effectuer des procédures de dépannage avancé.

  > [!Note]
  > En fonction du problème rencontré, les informations fournies dans l’article peuvent ne pas résoudre toutes les défaillances éventuelles.

## <a name="common-issues"></a>Problèmes courants
D’un point de vue architectural, cette nouvelle fonctionnalité s’appuie sur les API implémentées dans la pile de stockage Windows appelée par PowerShell. La pile de stockage s’appuie sur les pilotes et le matériel pour mettre en œuvre correctement les commandes définies par l’industrie. Pour cette raison, des défaillances peuvent se produire à plusieurs niveaux. Les problèmes les plus souvent observés sont :

1.  Un lecteur donné n’implémente pas correctement les commandes standard de l’industrie (pas de qualificateur supplémentaire)
2.  Les API nécessaires pour effectuer la mise à jour ne sont pas implémentées ou sont défectueuses (en cas d’utilisation de pilotes tiers)
3.  Les API fonctionnent, mais il existe un problème au niveau du microprogramme (image non valide/endommagée, etc.)

Les sections suivantes proposent diverses informations de dépannage, selon que des pilotes Microsoft ou tiers sont utilisés.

## <a name="identifying-inappropriate-hardware"></a>Identification de matériel inapproprié
La méthode la plus rapide pour déterminer si un périphérique prend en charge le jeu de commandes approprié consiste à simplement lancer PowerShell et à passer un disque représentant l’objet PhysicalDisk dans l’applet de commande Get-StorageFirmwareInfo. Exemple :

```powershell
Get-PhysicalDisk -SerialNumber 15140F55976D | Get-StorageFirmwareInformation
```
Et voici un exemple de sortie :

```
PhysicalDisk          : MSFT_PhysicalDisk (ObjectId = "{1}\\TOKLIMA-DL380\root/Microsoft/Windo...)
SupportsUpdate        : True
NumberOfSlots         : 1
ActiveSlotNumber      : 0
SlotNumber            : {0}
IsSlotWritable        : {True}
FirmwareVersionInSlot : {0013}
```

Le champ SupportsUpdate, du moins pour les périphériques SATA et NVMe, indique si la fonctionnalité intégrée de PowerShell peut être utilisée pour mettre à jour le microprogramme.

Le champ SupportsUpdate indique toujours « True » pour les périphériques SAS, car il n’est pas possible d’interroger le périphérique pour vérifier s’il prend en charge la commande standard de l’industrie appropriée.

Pour vérifier si un périphérique SAS prend en charge le jeu de commandes requis, il existe deux options :
1.  Faire un essai à l’aide de l’applet de commande Update-StorageFirmware avec une image de microprogramme appropriée, ou
2.  Consultez le catalogue Windows Server pour identifier les appareils SAS qui ont obtenu avec succès la mise à jour du pare-feu AQ (https://www.windowsservercatalog.com/)

### <a name="remediation-options"></a>Options de correction
Si le périphérique que vous testez ne prend pas en charge le jeu de commandes approprié, vérifiez si votre fournisseur peut vous fournir un microprogramme mis à jour, comportant le jeu de commandes requis ou, dans le catalogue Windows Server, recherchez les périphériques qui implémentent le jeu de commandes approprié.

## <a name="troubleshooting-with-3rd-party-drivers-sas"></a>Résolution des problèmes liés aux pilotes tiers (SAS)
Les composants logiciels qui interagissent le plus avec le matériel sont les pilotes de miniport dans la pile de stockage Windows. Pour certains protocoles de stockage tels que SATA et NVMe, Microsoft fournit des pilotes natifs Windows. Ces pilotes permettent de disposer d’informations de débogage supplémentaires. Toutefois, les fournisseurs de logiciels et de matériels tiers sont libres d’écrire leurs propres pilotes de miniport pour leurs périphériques et le niveau de prise en charge offert en matière d’informations de débogage est susceptible de varier.

Pour déterminer ce qu’il est advenu du téléchargement du microprogramme et activer les API envoyées vers la pile de stockage, quel que soit le pilote de miniport, consultez le canal de journal des événements suivant :

Observateur d’événements - Journaux des applications et des services - Microsoft - Windows - StorDiag - **Microsoft-Windows-Storage-ClassPnP/Operational**

Ce canal enregistre des informations sur les API Windows envoyées vers les pilotes de miniport et leurs réponses. Par exemple, la condition d’erreur indiquée au-dessous s’affiche lorsque vous tentez de télécharger une image de microprogramme sur un périphérique SATA qui est connecté par le biais d’un adaptateur de bus hôte (HBA) SAS qui n’implémente pas correctement la traduction nécessaire de SAS vers SATA :

```powershell
Get-PhysicalDisk -SerialNumber 44GS103UT5EW | Update-StorageFirmware -ImagePath C:\Firmware\J3E160@3.enc -SlotNumber 0
```
Voici un exemple de sortie :

```
Update-StorageFirmware : Failed

Extended information:
A warning or error has been encountered during storage firmware update.
Incorrect function.

Activity ID: {1224482b-2315-4a38-81eb-27bb7de19c00}
At line:1 char:47
+ ... S103UT5EW | Update-StorageFirmware -ImagePath C:\Firmware\J3E160@3.en ...
+                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : NotSpecified: (:) [Update-StorageFirmware], CimException
+ FullyQualifiedErrorId : StorageWMI 4,Microsoft.Management.Infrastructure.CimCmdlets.InvokeCimMethodCommand,Update-StorageFirmware
```
    
PowerShell génère une erreur et reçoit notification que les informations appelées par la fonction (autrement dit, l’API du noyau) sont incorrectes. Cela peut signifier que l’API n’a pas été implémentée par le pilote de miniport SAS tiers (ce qui est le cas ici), ou que l’API a échoué pour une autre raison, par exemple, un mauvais alignement des segments de téléchargement.

```
EventData
DeviceGUID  {132EDB55-6BAC-A3A0-C2D5-203C7551D700}
DeviceNumber    1
Vendor  ATA 
Model   TOSHIBA THNSNJ12
FirmwareVersion 6101
SerialNumber    44GS103UT5EW
DownLevelIrpStatus  0xc0000185
SrbStatus   132
ScsiStatus  2
SenseKey    5
AdditionalSenseCode 36
AdditionalSenseCodeQualifier    0
CdbByteCount    10
CdbBytes    3B0E0000000001000000
NumberOfRetriesDone 0
```

L’événement ETW 507 du canal indique qu’une demande SCSI SRB a échoué et fournit les informations supplémentaires que SenseKey était « 5 » (demande non conforme) et que AdditionalSense informations était « 36 » (champ non conforme dans CDB).

   > [!Note]
   > Ces informations sont fournies directement par le miniport en question et leur exactitude dépend de l’implémentation et du niveau technique du pilote du miniport.

Des conditions d’erreur différentes peuvent générer les mêmes codes d’erreur, si le pilote de miniport ne les distingue pas. Par exemple, la tentative de téléchargement d’une image de microprogramme non valide par le biais d’un adaptateur de bus hôte (HBA) sur un périphérique SATA (avec échec anticipé sur le périphérique) peut être traduite par les mêmes codes d’erreur.

Lorsque plusieurs protocoles sont utilisés et que les traductions ont lieu, par exemple SATA derrière SAS, il est recommandé de tester le périphérique SATA en le reliant directement à un contrôleur SATA pour s’assurer qu’il ne représente pas un problème potentiel.

### <a name="remediation-options"></a>Options de correction
S’il apparaît que le pilote tiers n’implémente pas les API ou les traductions requises, il est possible d’utiliser les solutions alternatives de Microsoft pour SATA (StorAHCI.sys) et NVMe (StorNVMe.sys). Vous pouvez également contacter le fournisseur OEM ou HBA du pilote SAS fourni pour vérifier s’il existe une version plus récente, avec la prise en charge appropriée.

## <a name="additional-troubleshooting-with-microsoft-drivers-satanvme"></a>Dépannage supplémentaire pour les pilotes Microsoft (SATA/NVMe)
Lorsque des pilotes natifs Windows tels que que StorAHCI.sys ou StorNVMe.sys sont utilisés pour des périphériques de stockage, il est possible d’obtenir des informations supplémentaires sur les défaillances potentielles pendant les opérations de mise à jour du microprogramme.

Au-delà du canal opérationnel ClassPnP, StorAHCI et StorNVMe journalisent les codes de retour spécifiques au protocole du périphérique dans le canal ETW suivant :

Observateur d’événements - Journaux des applications et des services - Microsoft - Windows - StorDiag - **Microsoft-Windows-Storage-StorPort/Diagnose**

Par défaut, les journaux de diagnostic ne sont pas affichés et vous pouvez les activer/afficher en cliquant sur « View » dans l’Observateur d’événements, puis en sélectionnant « Show Analytics and Debug Logs » dans le menu déroulant.

Pour collecter ces entrées de journal avancées, activez le journal, reproduisez l’échec de mise à jour du microprogramme et enregistrez le journal de diagnostic.

Voici l’exemple d’une mise à jour du microprogramme sur un périphérique SATA ayant échoué parce que l’image à télécharger n’était pas valide (ID d’événement : 258) :

``` 
EventData
MiniportName    storahci
MiniportEventId 19
MiniportEventDescription    Firmware Activate Completion
PortNumber  0
Bus 2
Target  0
LUN 0
Irp 0xffff8c84cd45aca0
Srb 0xffffab0024030bc0
Parameter1Name  SrbStatus
Parameter1Value 130
Parameter2Name  ReturnCode
Parameter2Value 0
Parameter3Name  FeaturesReg
Parameter3Value 15
Parameter4Name  SectorCountReg
Parameter4Value 0
Parameter5Name  DriveHeadReg
Parameter5Value 160
Parameter6Name  CommandReg
Parameter6Value 146
Parameter7Name  NULL
Parameter7Value 0
Parameter8Name  NULL
Parameter8Value 0
```

L’événement ci-dessus contient des informations détaillées sur le périphérique sous forme de valeurs de paramètre comprises entre 2 et 6. Dans ce cas, il s’agit de valeurs de registre ATA. La spécification ACS ATA peut être utilisée pour décoder les valeurs suivantes pour l’échec d’une commande de téléchargement de commande :
- Return Code: 0 (0000 0000) (N/A - sans signification dans la mesure où aucune charge utile n’a été transférée)
- Fonctionnalités : 15 (0000 1111) (le bit 1 est défini sur « 1 » et indique « abandon »)
- SectorCount: 0 (0000 0000) (N/A)
- DriveHead: 160 (1010 0000) (N/A – seuls les bits obsolètes sont définis)
- Commande : 146 (1001 0010) (le bit 1 est défini sur « 1 » indiquant la disponibilité des données de sens)

Ceci indique que l’opération de mise à jour du microprogramme a été abandonnée par le périphérique.

Un niveau similaire d’informations de débogage est disponible dans ce canal lors de l’utilisation de périphériques NVMe avec le pilote NVMe natif Windows(StorNVMe.sys).
