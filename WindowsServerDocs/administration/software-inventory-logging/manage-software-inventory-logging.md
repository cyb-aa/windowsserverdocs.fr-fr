---
title: Gérer la journalisation de l'inventaire logiciel
description: Décrit comment gérer la journalisation de l’inventaire logiciel
ms.custom: na
ms.prod: windows-server
ms.technology: manage-software-inventory-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 812173d1-2904-42f4-a9e2-de19effec201
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a14233e01c19df650d1059e1b60cd5398b05709a
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75946991"
---
# <a name="manage-software-inventory-logging"></a>Gérer la journalisation de l'inventaire logiciel

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Ce document explique comment gérer la journalisation de l’inventaire logiciel, une fonctionnalité qui permet aux administrateurs de centre de données de consigner facilement les données de gestion des ressources logicielles Microsoft pour leurs déploiements dans le temps. Ce document explique comment gérer la journalisation de l’inventaire logiciel. Avant d’utiliser la journalisation de l’inventaire logiciel avec Windows Server 2012 R2, assurez-vous que Windows Update [kb 3000850](https://support.microsoft.com/kb/3000850) et [KB 3060681](https://support.microsoft.com/kb/3060681) sont installés sur chaque système devant être inventorié. Aucune mise à jour service n’est requise pour Windows Server 2016. Cette fonctionnalité s’exécute localement sur chaque serveur à inventorier. Elle ne collecte pas de données depuis des serveurs distants.  

La fonctionnalité de journalisation de l’inventaire logiciel peut également être ajoutée à deux versions de Windows Server antérieures à Windows Server 2012 R2. Vous pouvez installer les mises à jour suivantes pour ajouter la fonctionnalité de journalisation de l’inventaire logiciel à Windows Server 2012 et Windows Server 2008 R2 SP1 :

- **Windows Server 2012 (édition standard ou Datacenter)** 

> [!NOTE] 
> Assurez-vous d’avoir installé [WMF 4.0](https://www.microsoft.com/download/details.aspx?id=40855) avant d’appliquer le package de mise à jour ci-dessous.

-  Package de mise à jour de WMF 4.0 pour Windows Server 2012 : [KB 3119938](https://support.microsoft.com/kb/3119938)

- **Windows Server 2008 R2 SP1**

> [!NOTE] 
> Assurez-vous d’avoir installé [WMF 4.0](https://www.microsoft.com/download/details.aspx?id=40855) avant d’appliquer le package de mise à jour ci-dessous.


- Nécessite [.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)


- Package de mise à jour de WMF 4.0 pour Windows Server 2008 R2 : [KB 3109118](https://support.microsoft.com/kb/3109118)


Il existe deux méthodes principales pour l’inventaire à l’aide de cette fonctionnalité :  
  
1.  Démarrage de la fonctionnalité de journalisation SIL pour collecter à partir de sources de données SIL et envoyer la charge sur le réseau vers une cible spécifiée (URI) toutes les heures.  
  
2.  Interrogation manuelle des données SIL à l’aide de PowerShell ou WMI à n’importe quel intervalle.  
  
Le démarrage de la journalisation SIL implique une planification et une prévoyance, mais présente des avantages importants par rapport à l’interrogation manuelle des données. L’utilisation de la journalisation SIL présente trois principaux avantages pour les administrateurs de centre de données :  
  
-   Un historique permanent (journal) peut être collecté au fil du temps, générant des rapports flexibles et complets à partir d’une source unique.  
  
-   Les défis relatifs à la découverte des ordinateurs typiques de nombreux outils d’inventaire peuvent être surmontés.  
  
-   Les défis relatifs aux délimitations d’approbation et la nécessité de privilèges utilisateur élevés typiques de nombreux outils d’inventaire peuvent être surmontés tout en conservant un niveau de sécurité, dans la mesure où les données sont chiffrées sur HTTPS avec SSL.  
  
La journalisation de l’inventaire logiciel est installée par défaut, mais elle ne démarre par défaut. La configuration de la journalisation de l’inventaire logiciel est effectuée avec les applets de commande PowerShell. Seules quelques options de configuration sont disponibles pour la journalisation de l’inventaire logiciel. Ce document décrit ces options et leur objectif, ainsi que les applets de commande destinées à la collection de données (si vous utilisez la deuxième méthode ci-dessus).  
  
**Dans ce document**  
  
Les options de configuration traitées dans ce document incluent :  
  
-   [Démarrage et arrêt de la journalisation de l’inventaire logiciel](manage-software-inventory-logging.md#BKMK_Step1)  
  
-   [Journalisation de l’inventaire logiciel au fil du temps](manage-software-inventory-logging.md#BKMK_Step2)  
  
-   [Affichage des données de journalisation de l’inventaire logiciel](manage-software-inventory-logging.md#BKMK_Step3)  
  
-   [Suppression des données consignées par la journalisation de l’inventaire logiciel](manage-software-inventory-logging.md#BKMK_Step4)  
  
-   [Sauvegarde et restauration des données consignées par la journalisation de l’inventaire logiciel] Manage-Software-Inventory-Logging. MD # BKMK_Step5)  
  
-   [Lecture des données consignées et publiées par la journalisation de l’inventaire logiciel](manage-software-inventory-logging.md#BKMK_Step6)  
  
-   [Sécurité de la journalisation de l’inventaire logiciel](manage-software-inventory-logging.md#BKMK_Step7)  
  
-   [Utilisation des paramètres de date et d’heure dans la journalisation de l’inventaire logiciel Windows Server](manage-software-inventory-logging.md#BKMK_Step8)  
  
-   [Activation et configuration de la journalisation de l’inventaire logiciel dans un disque dur virtuel monté](manage-software-inventory-logging.md#BKMK_Step10)  
  
-   [Présentation de l’utilisation de la journalisation de l’inventaire logiciel dans Windows Server 2012 R2 sans KB 3000850](manage-software-inventory-logging.md#BKMK_Step11)  
  
-   [Utilisation de la journalisation de l’inventaire logiciel dans un environnement Hyper-V Windows Server 2012 R2 sans KB 3000850](manage-software-inventory-logging.md#BKMK_Step12)  
  
> [!NOTE]  
> Cette rubrique inclut des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d'informations, consultez Utilisation des applets de commande.

  
## <a name="BKMK_Step1"></a>Démarrage et arrêt de la journalisation de l’inventaire logiciel  
La collecte quotidienne de la journalisation de l’inventaire logiciel et sa transmission sur le réseau doivent être activées sur un ordinateur exécutant Windows Server 2012 R2 pour consigner l’inventaire logiciel.  
  
> [!NOTE]  
> Vous pouvez utiliser l’applet de commande PowerShell **[Get-SilLogging](https://technet.microsoft.com/library/dn283396.aspx)** pour récupérer des informations sur le service de journalisation de l’inventaire logiciel, notamment s’il est en cours d’exécution ou arrêté.  
  
#### <a name="to-start-software-inventory-logging"></a>Pour démarrer la journalisation de l’inventaire logiciel  
  
1.  Connectez-vous au serveur avec un compte disposant des privilèges d’administrateur local.  
  
2.  Ouvrez PowerShell en tant qu’administrateur.  
  
3.  À l’invite de PowerShell, tapez **[Start-SilLogging](https://technet.microsoft.com/library/dn283391.aspx)**  
  
> [!NOTE]  
> Il est possible de définir la cible sans définir d’empreinte de certificat, mais si vous le faites, les transferts échoueront et les données seront stockées localement pour une durée par défaut maximale de 30 jours (après quoi elles sont supprimées). Une fois qu’un hachage de certificat valide a été défini pour la cible (et qu’un certificat valide correspondant a été installé dans le magasin LocalMachine/Personal), les données stockées localement sont transmises à la cible, tant que celle-ci est configurée pour accepter ces données avec ce certificat (pour plus d’informations, voir [Software Inventory Logging Aggregator](Software-Inventory-Logging-Aggregator.md) ).  
  
#### <a name="to-stop-software-inventory-logging"></a>Pour arrêter la journalisation de l’inventaire logiciel  
  
1.  Connectez-vous au serveur avec un compte disposant des privilèges d’administrateur local.  
  
2.  Ouvrez PowerShell en tant qu’administrateur.  
  
3.  À l’invite de PowerShell, tapez **[Stop-SilLogging](https://technet.microsoft.com/library/dn283394.aspx)**  
  
## <a name="configuring-software-inventory-logging"></a>Configuration de la journalisation de l’inventaire logiciel  
Il existe trois étapes de configuration de la journalisation de l’inventaire logiciel pour transmettre des données à un serveur d’agrégation dans le temps :  
  
1.  Utilisez **Set-SilLogging – targetUri** pour spécifier l’adresse Web de votre serveur d’agrégation (doit commencer par « https:// »).  
  
2.  Utilisez **Set-SilLogging –CertificateThumbprint** pour spécifier le hachage d’empreinte de votre certificat SSL valide à utiliser pour authentifier les transmissions de données vers votre serveur d’agrégation (celui-ci devra être configuré pour accepter le hachage).  
  
3.  Installez un certificat SSL valide (pour votre réseau) dans le **magasin Local Machine/Personal** (ou **/LocalMachine/MY**) du serveur local à partir duquel transférer des données.  
  
Il est préférable d’effectuer ces étapes avant d’utiliser **Start-SilLogging**.  Si vous souhaitez les utiliser après l’utilisation de **Start-SilLogging**, vous devez simplement arrêter et redémarrer SIL.  Vous pouvez également utiliser l’applet de commande Publish-SilData pour vérifier que le serveur d’agrégation dispose d’un ensemble complet des données pour ce serveur.  
  
Pour obtenir un guide complet sur la configuration de l’infrastructure SIL dans son ensemble, voir [Software Inventory Logging Aggregator](software-inventory-logging-aggregator.md).  En particulier, si **Publish-SilData** génère une erreur ou que la journalisation SIL échoue par ailleurs, voir la section de dépannage.  
  
## <a name="BKMK_Step2"></a>Journalisation de l’inventaire logiciel au fil du temps  
Si un administrateur a démarré la journalisation de l’inventaire logiciel, la collection toutes les heures et le transfert des données vers le serveur d’agrégation (URI cible) commencent. Le premier transfert est un jeu de données complet des mêmes données que [Get-SilData](https://technet.microsoft.com/library/dn283388.aspx) récupère et affiche sur la console à un moment donné. Par la suite, à chaque intervalle, SIL crée une vérification des données et transfère uniquement un petit accusé de réception d’identification au serveur cible d’agrégation si les données n’ont pas été modifiées depuis la dernière collection. Si une valeur a changé, SIL envoie à nouveau un jeu de données complet.  
  
> [!IMPORTANT]  
> Si, à n’importe quel intervalle, l’URI cible est inaccessible ou le transfert des données sur le réseau a échoué pour une raison quelconque, les données collectées sont stockées localement pour une durée par défaut maximale de 30 jours (après quoi elles sont supprimées). Lors du transfert suivant des données au serveur cible d’agrégation, toutes les données stockées localement sont transférées et les données mises en cache locales sont supprimées.  
  
## <a name="BKMK_Step3"></a>Affichage des données de journalisation de l’inventaire logiciel  
Outres les applets de commande PowerShell décrites dans la section précédente, six autres applets de commande peuvent être utilisées pour collecter des données de journalisation de l’inventaire logiciel :  
  
-   **[Obtenir-SilComputer](https://technet.microsoft.com/library/dn283392.aspx)** : affiche les valeurs à un moment donné pour des données spécifiques relatives au serveur et au système d’exploitation, ainsi que le nom de domaine complet ou le nom d’hôte de l’hôte physique, s’il est disponible.  
  
-   **[Obtenir-SilComputerIdentity (KB 3000850)](https://technet.microsoft.com/library/dn858074.aspx)** : affiche les identificateurs utilisés par sil pour des serveurs individuels.  
  
-   **[Obtenir-SilData](https://technet.microsoft.com/library/dn283388.aspx)** : affiche une collection à un moment donné de toutes les données de journalisation de l’inventaire logiciel.  
  
-   **[Obtient-SilSoftware](https://technet.microsoft.com/library/dn283397.aspx)** : affiche l’identité à un moment donné de tous les logiciels installés sur l’ordinateur.  
  
-   **[SilUalAccess](https://technet.microsoft.com/library/dn283389.aspx)** : affiche le nombre total de demandes d’appareils client uniques et de demandes d’utilisateur client du serveur de deux jours avant.  
  
-   **[Obtient-SilWindowsUpdate](https://technet.microsoft.com/library/dn283393.aspx)** : affiche la liste à un moment donné de toutes les mises à jour de Windows installées sur l’ordinateur.  
  
Un scénario typique d’utilisation d’applets de commande de la journalisation de l’inventaire logiciel consisterait en un administrateur qui interrogerait la journalisation de l’inventaire logiciel à un moment donné de la collection de toutes les données de journalisation de l’inventaire logiciel à l’aide de [Get-SilSoftware](https://technet.microsoft.com/library/dn283397.aspx).  
  
**Exemple de sortie**  
  
```  
PS C:\> Get-SilData   
  
ID                 : 961FF8A1-8549-4BEC-8DF6-3B3E32C26FFA  
UUID               : B49ACB4C-7D9C-4806-9917-AE750BB3DA84  
VMGUID             : E84CCCBD-0D0F-486B-A424-9780C7CF92E4  
Name               : Server01Guest.Test.Contoso.com  
HypervisorHostName : Server01.Test.Contoso.com  
  
ID          : {F0C3E5D1-1ADE-321E-8167-68EF0DE699A5}  
Name        : Microsoft Visual C++ 2010  x86 Redistributable - 10.0.40219  
InstallDate : 12/5/2013  
Publisher   : Microsoft Corporation  
Version     : 10.0.40219  
  
ID          : {89F4137D-6C26-4A84-BDB8-2E5A4BB71E00}  
Name        : Microsoft Silverlight  
InstallDate : 3/20/2014  
Publisher   : Microsoft Corporation  
Version     : 5.1.30214.0  
  
ChassisSerialNumber       : 4452-0564-0284-2290-0113-6804-05  
CollectedDateTime         : 10/27/2014 4:01:33 PM  
Model                     : Virtual Machine  
Name                      : Server01Guest.Test.Contoso.com  
NumberOfCores             : 1  
NumberOfLogicalProcessors : 1  
NumberOfProcessors        : 1  
OSName                    : Microsoft Windows Server 2012 R2 Datacenter  
OSSku                     : 8  
OSSuite                   : 400  
OSSuiteMask               : 400  
OSVersion                 : 6.3.9600  
ProcessorFamily           : 179  
ProcessorManufacturer     : GenuineIntel  
ProcessorName             : Intel(R) Xeon(R) CPU           E5440  @ 2.83GHz  
SystemManufacturer        : Microsoft Corporation  
  
```  
  
> [!NOTE]  
> La sortie de cette applet de commande est la même que celle de toutes les autres applets de commande **Get-Sil** pour cette fonctionnalité combinée, mais elle est fournie à la console de façon asynchrone. De ce fait, l’ordre des objets peut ne pas toujours être le même.  
>   
> Il n’est pas nécessaire d’avoir commencé à utiliser la journalisation de l’inventaire logiciel pour utiliser les applets de commande **Get-Sil** .  
  
## <a name="BKMK_Step4"></a>Suppression des données consignées par la journalisation de l’inventaire logiciel  
La journalisation de l’inventaire logiciel n’est pas destinée à être un composant essentiel. Elle a été conçue pour affecter le moins possible les opérations système locales tout en maintenant un haut niveau de fiabilité. Cela permet également à l’administrateur de supprimer manuellement la base de données de journalisation de l’inventaire logiciel et les fichiers de prise en charge (chaque fichier dans le répertoire \Windows\System32\LogFiles\SIL) pour répondre aux besoins opérationnels.  
  
#### <a name="to-delete-data-logged-by-software-inventory-logging"></a>Pour supprimer les données consignées par la journalisation de l’inventaire logiciel  
  
1. Dans PowerShell, arrêtez la journalisation de l’inventaire logiciel avec la commande **[Stop-SilLogging](https://technet.microsoft.com/library/dn283394.aspx)** .  
  
2. Ouvrez l’Explorateur Windows.  
  
3. Accédez à **\Windows\System32\Logfiles\SIL\\**  
  
4. Supprimez tous les fichiers de ce dossier.  
  
## <a name="BKMK_Step5"></a>Sauvegarde et restauration des données consignées par la journalisation de l’inventaire logiciel  
La journalisation de l’inventaire logiciel stocke temporairement les collections de données effectuées toutes les heures si le transfert sur le réseau échoue. Les fichiers journaux sont stockés dans le répertoire \Windows\System32\LogFiles\SIL\. Les sauvegardes des données de journalisation de l’inventaire logiciel peuvent être effectuées avec vos sauvegardes planifiées du serveur.  
  
> [!IMPORTANT]  
> Si, pour une raison quelconque, une réparation de l’installation ou une mise à niveau du système d’exploitation est nécessaire, tous les fichiers journaux stockés localement seront perdus.  Si ces données sont essentielles pour les opérations, il est recommandé de les sauvegarder avant l’installation du nouveau système d’exploitation. Après la réparation ou la mise à niveau, restaurez simplement les données au même emplacement.  
  
> [!NOTE]  
> Si, pour une raison quelconque, la gestion de la durée de rétention des données enregistrées localement par SIL devient importante, vous pouvez configurer cette valeur en modifiant la valeur de Registre ici : \ HKEY_LOCAL_MACHINE\\SOFTWARE\Microsoft\Windows\SoftwareInventoryLogging. La valeur par défaut est « 30 » pendant 30 jours.  
  
## <a name="BKMK_Step6"></a>Lecture des données consignées et publiées par la journalisation de l’inventaire logiciel  
Les données enregistrées par SIL, mais stockées localement (si le transfert vers l’URI cible échoue) ou les données qui sont transférées avec succès au serveur d’agrégation cible, sont stockées dans un fichier binaire (pour les données de chaque jour). Pour afficher ces données dans PowerShell, utilisez l’applet de commande [Import-BinaryMiLog](https://technet.microsoft.com/library/dn262592.aspx) .  
  
## <a name="BKMK_Step7"></a>Sécurité de la journalisation de l’inventaire logiciel  
Des privilèges d’administrateur sur le serveur local sont requis pour récupérer les données à partir des API WMI et PowerShell de la journalisation de l’inventaire logiciel.  
  
Pour exploiter correctement la fonctionnalité complète de journalisation de l’inventaire logiciel afin de transférer des données vers un point d’agrégation en permanence au fil du temps (à des intervalles d’une heure), un administrateur doit utiliser des certificats clients pour garantir des sessions SSL sécurisées pour le transfert de données via le protocole HTTPS. Vous trouverez ici une vue d’ensemble de l’authentification HTTPS : [Authentification HTTPS](https://technet.microsoft.com/library/cc736680(v=WS.10).aspx).  
  
Toutes les données stockées localement sur un serveur Windows (se produit uniquement si la fonctionnalité est démarrée mais la cible est inaccessible pour une raison quelconque) ne sont accessibles qu’avec des privilèges d’administrateur sur le serveur local.  
  
## <a name="BKMK_Step8"></a>Utilisation des paramètres de date et d’heure dans la journalisation de l’inventaire logiciel de Windows Server 2012 R2  
  
-   Lorsque vous utilisez [Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) - TimeOfDay pour définir le moment auquel la journalisation SIL s’exécute, vous devez spécifier une date et une heure. La date du calendrier est définie et la journalisation n’a pas lieu tant que la date n’est pas atteinte, en heure système locale.  
  
-   Lorsque vous utilisez la [SilSoftware](https://technet.microsoft.com/library/dn283397.aspx)ou la [SilWindowsUpdate](https://technet.microsoft.com/library/dn283393.aspx), « InstallDate » affiche toujours 12:00:00, une valeur sans signification.  
  
-   Lorsque vous utilisez la [SilUalAccess](https://technet.microsoft.com/library/dn283389.aspx), « SampleDate » affiche toujours 11:59:14:00, une valeur sans signification.  La date est la donnée pertinente pour ces requêtes d’applet de commande.  
  
## <a name="BKMK_Step10"></a>Activation et configuration de la journalisation de l’inventaire logiciel dans un disque dur virtuel monté  
La journalisation de l’inventaire logiciel prend également en charge la configuration et l’activation sur des ordinateurs virtuels hors connexion. Ces utilisations pratiques sont destinées à couvrir la configuration de l’image Gold pour un déploiement à grande échelle entre les centres de données, ainsi que la configuration des images des utilisateurs finaux allant d’un site local vers un déploiement Cloud.  
  
Pour prendre en charge ces utilisations, la journalisation de l’inventaire logiciel comporte des entrées de Registre associées à chaque option configurable.  Ces valeurs de Registre se trouvent sous \ HKEY_LOCAL_MACHINE\\SOFTWARE\Microsoft\Windows\SoftwareInventoryLogging.  
  
|||||  
|-|-|-|-|  
|**Fonction**|**Nom de la valeur**|**Données**|**Applet de commande correspondante (disponible uniquement dans le système d’exploitation en cours d’exécution)**|  
|Fonctionnalité de démarrage/d’arrêt|CollectionState|1 ou 0|[Start-SilLogging](https://technet.microsoft.com/library/dn283391.aspx), [Stop-SilLogging](https://technet.microsoft.com/library/dn283394.aspx)|  
|Spécifie le point d’agrégation cible sur le réseau|TargetUri|chaîne|[Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) - TargetURI|  
|Spécifie l’empreinte du certificat ou le hachage du certificat utilisé pour l’authentification SSL pour le serveur web cible|CertificateThumbprint|chaîne|[Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) -CertificateThumbprint|  
|Spécifie la date et l’heure auxquelles la fonctionnalité débute (si la valeur définie est dans le futur selon l’heure système locale)|CollectionTime|Par défaut :  2000-01-01T03:00:00|[Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) -TimeOfDay|  
  
Pour modifier ces valeurs sur un disque dur virtuel hors connexion (système d’exploitation de l’ordinateur inactif), un disque dur virtuel doit tout d’abord être monté, et ensuite les commandes suivantes peuvent être utilisées pour apporter des modifications :  
  
-   [Reg load](https://technet.microsoft.com/library/cc742053.aspx)  
  
-   [Reg delete](https://technet.microsoft.com/library/cc742145.aspx)  
  
-   [Reg add](https://technet.microsoft.com/library/cc742162.aspx)  
  
-   [Reg unload](https://technet.microsoft.com/library/cc742043.aspx)  
  
La journalisation de l’inventaire logiciel vérifie ces valeurs lorsque le système d’exploitation démarre et s’exécute en conséquence.  
  
## <a name="BKMK_Step11"></a>Présentation de l’utilisation de la journalisation de l’inventaire logiciel dans Windows Server 2012 R2 sans KB 3000850  
Les modifications suivantes des paramètres par défaut et de la fonctionnalité de journalisation de l’inventaire logiciel ont été effectuées avec [KB 3000850](https://support.microsoft.com/kb/3000850):  
  
-   L’intervalle par défaut pour la collection et le transfert sur le réseau au démarrage de la journalisation SIL a été remplacé par toutes les heures à la place de quotidien (au hasard dans chaque heure).  
  
-   La charge utile de données par défaut a été réduite pour inclure uniquement les objets provenant de SilComputer, Get-SilComputerIdentity et Get-SilSoftware.  
  
-   L’invité pour la communication de canal hôte dans les environnements Hyper-V a été supprimé.  
  
## <a name="BKMK_Step12"></a>Utilisation de la journalisation de l’inventaire logiciel dans un environnement Hyper-V Windows Server 2012 R2 sans KB 3000850  
  
> [!NOTE]  
> Cette fonctionnalité est supprimée avec l’installation de la mise à jour [KB 3000850](https://support.microsoft.com/kb/3000850) .  
  
Lors de l’utilisation de la journalisation de l’inventaire logiciel sur un ordinateur hôte Windows Server 2012 R2 Hyper-V, il est possible d’extraire des données SIL des invités Windows Server 2012 R2 qui s’exécutent localement, si la journalisation SIL a été démarrée dans le ou les invités. Toutefois, cela n’est possible que si vous utilisez les applets de commande PowerShell SilData et Publish-SilData, et uniquement avec WIndows Server 2012 R2 dans l’hôte et l’invité.  L’objectif de cette fonctionnalité est de permettre aux administrateurs de centre de données qui fournissent des machines virtuelles invitées à des locataires, ou à d’autres entités d’une grande entreprise, de capturer les données d’inventaire logiciel sur l’hôte hyperviseur, puis de transférer toutes ces données à un agrégateur (ou URI cible).  
  
Voici deux exemples de l’aspect de la sortie sur la console PowerShell (très abrégé) sur un hôte Hyper-V Windows Server 2012 R2 exécutant une machine virtuelle invitée Windows Server 2012 R2 avec la journalisation SIL démarrée.  Vous remarquerez que le premier exemple, qui utilise SilData seul, génère toutes les données de l’hôte comme prévu.  Toutes les données SIL de l’invité sont également incluses, mais dans un format réduit.  Pour développer et afficher ces données à partir de l’invité, coupez et collez simplement l’extrait de code utilisé dans le deuxième exemple ci-dessous.  Les objets de données SIL de l’invité auront toujours le GUID de l’ordinateur virtuel associé au sein de l’objet.  
  
> [!NOTE]  
> Les données SIL étant une sortie sur la console, lors de l’utilisation de l’applet de commande Get-SilData, dans des flux de données, les objets ne sortiront pas systématiquement dans l’ordre prévu.  Dans les deux exemples ci-dessous, le texte a été codé par couleur (bleu pour les données de l’hôte physique et vert pour les données de l’invité virtuel) uniquement comme outil d’illustration pour ce document.  
  
**Exemple de sortie 1**  
  
![](../media/software-inventory-logging/SILHyper-VExample1.png)  
  
**Exemple de sortie 2** (fonction w/Expand-SilData)  
  
![](../media/software-inventory-logging/SILHyper-VExample2.png)  
  
## <a name="see-also"></a>Articles associés  
[Prise en main de la journalisation de l’inventaire logiciel](get-started-with-software-inventory-logging.md)  
[Agrégateur de journalisation de l’inventaire logiciel](software-inventory-logging-aggregator.md)  
[Applets de commande de la journalisation de l’inventaire logiciel dans Windows PowerShell](https://technet.microsoft.com/library/dn283390.aspx)  
[Import-BinaryMiLog](https://technet.microsoft.com/library/dn262592.aspx)  
[Export-BinaryMiLog](https://technet.microsoft.com/library/dn262591.aspx)  
  

