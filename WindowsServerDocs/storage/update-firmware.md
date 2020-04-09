---
ms.assetid: e5945bae-4a33-487c-a019-92a69db8cf6c
title: Mise à jour du microprogramme des lecteurs
ms.prod: windows-server
ms.author: toklima
manager: dmoss
ms.technology: storage-spaces
ms.topic: article
author: toklima
ms.date: 10/04/2016
ms.openlocfilehash: 55a4fc94440b763c48735ffe44099da702857489
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820872"
---
# <a name="updating-drive-firmware"></a>Mise à jour du microprogramme des lecteurs
>S’applique à : Windows Server 2019, Windows Server 2016, Windows 10

La mise à jour du microprogramme des lecteurs a toujours été une tâche fastidieuse associée à un risque de temps d’arrêt potentiel. C’est pour cette raison que nous apportons des améliorations aux espaces de stockage, à Windows Server et Windows 10, version 1703 et versions ultérieures. Si vos lecteurs prennent en charge le nouveau mécanisme de mise à jour de microprogramme fourni par Windows, vous pouvez mettre à jour le microprogramme de vos lecteurs de production sans temps d’arrêt. Cependant, si vous prévoyez de mettre à jour le microprogramme d’un lecteur de production, veillez à lire nos conseils sur la façon de limiter autant que possible les risques en utilisant cette nouvelle et puissante fonctionnalité.

  > [!Warning]
  > La mise à jour de microprogramme est une opération de maintenance qui peut présenter des risques. Vous ne devez l’appliquer qu’après avoir testé de manière approfondie la nouvelle image de microprogramme. Le nouveau microprogramme peut nuire à la fiabilité et à la stabilité d’un matériel non pris en charge, voire entraîner une perte de données. Les administrateurs ont tout intérêt à lire les notes de publication qui accompagnent une mise à jour donnée pour déterminer son impact et ses conditions d’application.

## <a name="drive-compatibility"></a>Compatibilité des lecteurs

Pour mettre à jour le microprogramme des lecteurs avec Windows Server, ces lecteurs doivent être pris en charge. Pour garantir un comportement normal des appareils, nous avons commencé par définir de nouvelles spécifications (pour Windows Server 2016 et Windows 10) et des spécifications facultatives pour les appareils SAS, SATA et NVMe à travers notre Kit d’évaluation de matériel en laboratoire (HLK). Ces spécifications décrivent les commandes qu’un périphérique SATA, SAS ou NVMe doit prendre en charge pour envisager une mise à jour de microprogramme à l’aide de ces nouvelles applets de commande PowerShell natives Windows. Parallèlement à ces spécifications, le nouveau test HLK vise à vérifier si les produits des fournisseurs prennent en charge les commandes adéquates et à les faire implémenter dans leurs futures révisions. 

Pour savoir si votre matériel prend en charge la mise à jour Windows du microprogramme des lecteurs, contactez votre fournisseur de solutions.
Voici des liens vers les différentes spécifications :

-   SATA : [Device.Storage.Hd.Sata](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/device-storage#devicestoragehdsata) - dans la section **[If Implemented\] Firmware Download & Activate**
    
-   SAS : [Device.Storage.Hd.Sas](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/device-storage#devicestoragehdsas) - dans la section **[If Implemented\] Firmware Download & Activate**

-   NVMe : [Device.Storage.ControllerDrive.NVMe](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/device-storage#devicestoragecontrollerdrivenvme) - dans les sections **5.7** et **5.8**.

## <a name="powershell-cmdlets"></a>Applets de commande PowerShell

Les deux applets de commande ajoutées à Windows sont les suivantes :

-   Get-StorageFirmwareInformation
-   Update-StorageFirmware

La première applet de commande fournit des informations détaillées sur les fonctionnalités du périphérique, les images du microprogramme et les révisions. Dans ce cas, l’ordinateur contient un seul SSD SATA avec un emplacement de microprogramme. Voici un exemple :

   ```powershell
   Get-PhysicalDisk | Get-StorageFirmwareInformation

   SupportsUpdate        : True
   NumberOfSlots         : 1
   ActiveSlotNumber      : 0
   SlotNumber            : {0}
   IsSlotWritable        : {True}
   FirmwareVersionInSlot : {J3E16101}
   ```

Notez que le paramètre « SupportsUpdate » indique toujours « True » pour les périphériques SAS, car il n’existe aucun moyen d’interroger explicitement ces périphériques pour savoir s’ils prennent en charge ces commandes.

La deuxième applet de commande, Update-StorageFirmware, permet aux administrateurs de mettre à jour le microprogramme des lecteurs avec un fichier image, si les lecteurs prennent en charge le nouveau mécanisme de mise à jour de microprogramme. Vous devriez pouvoir vous procurer ce fichier image directement auprès du fabricant OEM ou du fournisseur des lecteurs.

  > [!Note]
  > Avant de mettre à jour du matériel de production, testez l’image de microprogramme en question sur du matériel identique dans un environnement de laboratoire.

Dans un premier temps, le lecteur charge la nouvelle image de microprogramme dans une zone intermédiaire interne. En règle générale, les E/S continuent pendant cette opération. Une fois le téléchargement terminé, l’image s’active. Le lecteur ne peut alors pas répondre aux commandes d’E/S, car une réinitialisation interne se produit. Autrement dit, le lecteur ne traite aucune donnée pendant l’activation. Les applications qui accèdent aux données de ce lecteur doivent ainsi attendre une réponse et donc la fin de l’activation du microprogramme. Voici un exemple de l’applet de commande à l’œuvre :

   ```powershell 
   $pd | Update-StorageFirmware -ImagePath C:\Firmware\J3E160@3.enc -SlotNumber 0
   $pd | Get-StorageFirmwareInformation

   SupportsUpdate        : True
   NumberOfSlots         : 1
   ActiveSlotNumber      : 0
   SlotNumber            : {0}
   IsSlotWritable        : {True}
   FirmwareVersionInSlot : {J3E160@3}
   ```

Généralement, les lecteurs ne traitent pas les demandes d’E/S pendant l’activation d’une nouvelle image de microprogramme. La durée d’activation d’un lecteur dépend de sa conception et du type de microprogramme que vous mettez à jour. D’après nos observations, la durée de la mise à jour varie d’un peu moins de 5 secondes à plus de 30 secondes.

Comme vous pouvez le constater ci-dessous, ce lecteur a eu besoin d’un peu moins de 5,8 secondes pour effectuer la mise à jour du microprogramme :

```powershell 
Measure-Command {$pd | Update-StorageFirmware -ImagePath C:\\Firmware\\J3E16101.enc -SlotNumber 0}

 Days : 0
 Hours : 0
 Minutes : 0
 Seconds : 5
 Milliseconds : 791
 Ticks : 57913910
 TotalDays : 6.70299884259259E-05
 TotalHours : 0.00160871972222222
 TotalMinutes : 0.0965231833333333
 TotalSeconds : 5.791391
 TotalMilliseconds : 5791.391
 ```

## <a name="updating-drives-in-production"></a>Mise à jour de lecteurs de production

Avant de passer un serveur en production, nous vous recommandons vivement de mettre à jour le microprogramme de vos lecteurs avec le microprogramme recommandé par le fournisseur de matériel ou le fabricant OEM qui a vendu et assure le support de votre solution (boîtiers de stockage, lecteurs et serveurs).

Une fois le serveur en production, il est préférable d’y apporter le moins de modifications possible. Pourtant, le fournisseur de votre solution peut parfois vous informer qu’une mise à jour de microprogramme extrêmement importante pour vos lecteurs existe. Si cela se produit, voici quelques bonnes pratiques à observer avant d’appliquer les mises à jour de microprogramme aux lecteurs :

1. Examinez les notes de publication du microprogramme. Vérifiez que la mise à jour résout les problèmes susceptibles d’affecter votre environnement et que le microprogramme ne présente pas de problèmes connus qui pourraient vous porter préjudice.

2. Installez le microprogramme sur un serveur de votre laboratoire équipé de lecteurs identiques (avec la dernière révision du lecteur, s’il en existe plusieurs), puis testez le lecteur en situation de charge avec le nouveau microprogramme. Pour plus d’informations sur l’exécution de tests de charge synthétique, consultez [Tester les performances des espaces de stockage à l’aide de charges de travail synthétiques dans Windows Server](https://technet.microsoft.com/library/dn894707.aspx).

## <a name="automated-firmware-updates-with-storage-spaces-direct"></a>Mises à jour de microprogramme automatisées avec les espaces de stockage direct

Windows Server 2016 propose un service de contrôle d’intégrité pour les déploiements d’espaces de stockage direct (notamment les solutions Microsoft Azure Stack). Le service de contrôle d’intégrité vise essentiellement à faciliter la surveillance et la gestion du déploiement de votre matériel. Ses fonctions de gestion permettent entre autres de déployer un microprogramme de lecteur sur un cluster entier sans mettre les charges de travail hors ligne ni risquer de temps morts. Cette fonctionnalité est pilotée par une stratégie et l’administrateur en a le contrôle total.

Le déploiement d’un microprogramme sur un cluster à l’aide du service de contrôle d’intégrité s’avère très simple et repose sur les étapes suivantes :

-   Identifiez les disques durs et les lecteurs SSD que vous voulez intégrer à votre cluster d’espaces de stockage direct, puis déterminez si les lecteurs prennent en charge les mises à jour de microprogramme assurées par Windows.
-   Dressez la liste de ces lecteurs dans le fichier .xml des composants pris en charge.
-   Identifiez les versions de microprogramme qui doivent figurer dans le fichier .xml des composants pris en charge (y compris le chemin de l’emplacement des images de microprogramme) pour ces lecteurs.
-   Chargez le fichier .xml dans la base de données du cluster.

À ce stade, le service de contrôle d’intégrité inspecte et analyse le fichier .xml, puis identifie les lecteurs dont la version de microprogramme souhaitée n’est pas déployée. Il redirige ensuite les E/S des lecteurs concernés (un nœud après l’autre) et met à jour le microprogramme sur ces derniers. Un cluster d’espaces de stockage direct assure la résilience en répartissant les données entre les différents nœuds du serveur. Le service de contrôle d’intégrité peut isoler les lecteurs d’un nœud entier pour les besoins de la mise à jour. Dès lors qu’un nœud est mis à jour, il lance une réparation dans les espaces de stockage, où toutes les copies de données sont resynchronisées les unes par rapport aux autres à l’échelle du cluster, avant de passer au nœud suivant. Il est normal et attendu que les espaces de stockage passent à un mode de fonctionnement « dégradé » pendant le déploiement du microprogramme.

Pour assurer un déploiement stable et un temps de validation suffisant pour une image de microprogramme, un délai important est observé entre les mises à jour des différents serveurs. Par défaut, le service de contrôle d’intégrité attend 7 jours avant de mettre à jour le 2 <sup>ème</sup> serveur. Les autres serveurs (3 <sup>ème</sup>, 4 <sup>ème</sup>,…) éventuels sont mis à jour dans un délai d’un jour. Si un administrateur juge que le microprogramme est instable voire indésirable, il peut à tout moment déprogrammer les autres déploiements via le service de contrôle d’intégrité. Si le microprogramme a déjà été validé et qu’un déploiement plus rapide est souhaité, ces valeurs par défaut peuvent être modifiées pour passer de quelques jours à quelques heures ou minutes.

Voici un exemple de fichier .xml de composants pris en charge pour un cluster d’espaces de stockage direct générique :

```xml
 <Components>
     <Disks>
        <Disk>
            <Manufacturer>Contoso</Manufacturer>
            <Model>XYZ9000</Model>
            <AllowedFirmware>
              <Version>2.0</Version>
              <Version>2.1>/Version>
              <Version>2.2</Version>
            </AllowedFirmware>
            <TargetFirmware>
              <Version>2.2</Version>
              <BinaryPath>\\path\to\image.bin</BinaryPath>
            </TargetFirmware>
        </Disk>
        ...
        ...
    </Disks>
 </Components>
```

Pour lancer le déploiement du nouveau microprogramme dans ce cluster d’espaces de stockage direct, il suffit de charger le fichier .xml dans la base de données du cluster DB :

```powershell
$SpacesDirect = Get-StorageSubSystem Clus*

$CurrentDoc = $SpacesDirect | Get-StorageHealthSetting -Name "System.Storage.SupportedComponents.Document"

$CurrentDoc.Value | Out-File <Path>
```

Modifiez le fichier dans l’éditeur de votre choix, par exemple Visual Studio Code ou le Bloc-notes, puis enregistrez-le.

```powershell
$NewDoc = Get-Content <Path> | Out-String

$SpacesDirect | Set-StorageHealthSetting -Name "System.Storage.SupportedComponents.Document" -Value $NewDoc
```

Si vous souhaitez voir le Service de contrôle d’intégrité en action et en savoir plus sur son mécanisme de déploiement, consultez cette vidéo : https://channel9.msdn.com/Blogs/windowsserver/Update-Drive-Firmware-Without-Downtime-in-Storage-Spaces-Direct

## <a name="frequently-asked-questions"></a>Forum aux questions

Consultez également [Résolution des problèmes liés aux mises à jour du microprogramme des lecteurs](troubleshoot-firmware-update.md).

### <a name="will-this-work-on-any-storage-device"></a>Cette fonctionnalité est-elle compatible avec n’importe quel dispositif de stockage ?

Cette fonctionnalité est compatible avec les dispositifs de stockage capables d’implémenter les commandes appropriées dans leur microprogramme. L’applet de commande Get-StorageFirmwareInformation permet de déterminer si le microprogramme d’un lecteur prend effectivement en charge les commandes appropriées (pour SATA/NVMe) tandis que le test HLK permet aux fournisseurs et aux fabricants OEM de tester ce comportement.

### <a name="after-i-update-a-sata-drive-it-reports-to-no-longer-support-the-update-mechanism-is-something-wrong-with-the-drive"></a>Après avoir été mis à jour, un lecteur SATA indique qu’il ne prend plus en charge le mécanisme de mise à jour. Ce lecteur aurait-il un problème ?

Non, le lecteur est parfait, à moins que le nouveau microprogramme n’autorise plus les mises à jour. Vous rencontrez un problème connu selon lequel une version mise en cache des fonctionnalités du lecteur est incorrecte. L’exécution de « Update-StorageProviderCache - DiscoveryLevel Full » aura pour effet d’énumérer à nouveau les fonctionnalités du lecteur et de mettre à jour la copie mise en cache. Comme solution de contournement, nous vous recommandons d’exécuter la commande ci-dessus une seule fois avant de lancer une mise à jour de microprogramme ou un déploiement complet sur un cluster d’espaces de stockage direct.

### <a name="can-i-update-firmware-on-my-san-through-this-mechanism"></a>La mise à jour d’un microprogramme sur un réseau SAN est-elle possible par le biais de ce mécanisme ?
Non. Les réseaux SAN ont généralement leurs propres utilitaires et interfaces pour mener à bien ce type d’opération de maintenance. Ce nouveau mécanisme s’adresse aux dispositifs de stockage directement connectés, tels que les appareils SATA, SAS ou NVMe.

### <a name="from-where-do-i-get-the-firmware-image"></a>Où se procurer l’image d’un microprogramme ?

Vous devez toujours obtenir les microprogrammes directement auprès de votre OEM, fournisseur de solutions ou fournisseur de lecteurs et éviter de les télécharger auprès de services tiers. Windows met à disposition le mécanisme permettant d’obtenir l’image sur le lecteur, mais ne peut pas en vérifier l’intégrité.

### <a name="will-this-work-on-clustered-drives"></a>Cette fonctionnalité est-elle compatible avec les lecteurs en cluster ?

Les applets de commande peuvent aussi remplir leur fonction sur les lecteurs en cluster, mais n’oubliez pas que l’orchestration du service de contrôle d’intégrité atténue l’impact des E/S sur les charges de travail en cours d’exécution. Si les applets de commande sont utilisées directement sur les lecteurs en cluster, les E/S risquent de se bloquer. En général, il est recommandé d’appliquer les mises à jour de microprogramme aux lecteurs sous-jacents qui n’ont pas de charge de travail en cours ou une charge de travail minime.

### <a name="what-happens-when-i-update-firmware-on-storage-spaces"></a>Que se passe-t-il pendant la mise à jour d’un microprogramme sur des espaces de stockage ?

Sur Windows Server 2016, après avoir déployé le service de contrôle d’intégrité sur les espaces de stockage direct, vous pouvez effectuer cette opération sans mettre vos charges de travail hors ligne, à condition que les lecteurs prennent en charge la mise à jour Windows Server du microprogramme.

### <a name="what-happens-if-the-update-fails"></a>Que se passe-t-il en cas d’échec de la mise à jour ?

La mise à jour peut échouer pour diverses raisons, dont certaines sont : 1) le lecteur ne prend pas en charge les commandes appropriées pour que Windows puisse mettre à jour son microprogramme. Dans ce cas, la nouvelle image de microprogramme ne s’active jamais et le lecteur continue de fonctionner avec l’ancienne image. 2) L’image ne peut pas être téléchargée ou appliquée sur le lecteur (incompatibilité de version, image endommagée,...). Dans ce cas, le lecteur fait échouer la commande d’activation. Là encore, l’ancienne image de microprogramme continue de fonctionner.

Si le lecteur ne répond pas du tout après une mise à jour de microprogramme, le microprogramme de lecteur présente probablement un bogue. Testez toutes les mises à jour de microprogramme dans un environnement de laboratoire avant de les mettre en production. La seule action corrective est peut-être de remplacer le disque.

Pour plus d’informations, consultez [Résolution des problèmes liés aux mises à jour du microprogramme des lecteurs](troubleshoot-firmware-update.md).

### <a name="how-do-i-stop-an-in-progress-firmware-roll-out"></a>Comment arrêter un déploiement de microprogramme en cours ?

Désactivez le déploiement dans PowerShell via :
```powershell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.Enabled" -Value false
```

### <a name="i-am-seeing-an-access-denied-or-path-not-found-error-during-roll-out-how-do-i-fix-this"></a>Une erreur indiquant un accès refusé ou un chemin introuvable s’affiche pendant le déploiement. Comment résoudre ce problème ?

Vérifiez que l’image de microprogramme que vous prévoyez d’utiliser pour la mise à jour est accessible à tous les nœuds du cluster. Le moyen le plus simple de s’en assurer est de la placer sur un volume partagé de cluster.
