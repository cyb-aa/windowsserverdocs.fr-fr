---
title: Gérer la journalisation de l'inventaire logiciel
description: Décrit comment gérer la journalisation de l’inventaire logiciel
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 506072251b77362f3dc35faa0c976f396f7f6034
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435483"
---
# <a name="manage-software-inventory-logging"></a>Gérer la journalisation de l'inventaire logiciel

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Ce document décrit comment gérer la journalisation de l’inventaire logiciel, une fonctionnalité qui permet aux administrateurs de centre de données facilement consigner les données de gestion de licences de logiciel Microsoft pour leurs déploiements au fil du temps. Ce document explique comment gérer la journalisation de l’inventaire logiciel. Avant d’utiliser la journalisation de l’inventaire logiciel avec Windows Server 2012 R2, assurez-vous que cette mise à jour Windows [KB 3000850](https://support.microsoft.com/kb/3000850) et [KB 3060681](https://support.microsoft.com/kb/3060681) sont installés sur chaque système à inventorier. Aucune mise à jour Windows ne sont requis pour Windows Server 2016. Cette fonctionnalité s’exécute localement sur chaque serveur à inventorier. Elle ne collecte pas de données depuis des serveurs distants.  

La fonctionnalité de journalisation de l’inventaire logiciel peut également être ajoutée aux deux versions de Windows Server antérieures à Windows Server 2012 R2. Vous pouvez installer les mises à jour suivantes pour ajouter des fonctionnalités de journalisation de l’inventaire logiciel pour Windows Server 2012 et Windows Server 2008 R2 SP1 :

- **Windows Server 2012 (Édition Standard ou Datacenter)** 

> [!NOTE] 
> Assurez-vous d’avoir installé [WMF 4.0](https://www.microsoft.com/en-us/download/details.aspx?id=40855) avant d’appliquer le package de mise à jour ci-dessous.

-  Package de mise à jour de WMF 4.0 pour Windows Server 2012 : [KB 3119938](https://support.microsoft.com/en-us/kb/3119938)

- **Windows Server 2008 R2 SP1**

> [!NOTE] 
> Assurez-vous d’avoir installé [WMF 4.0](https://www.microsoft.com/en-us/download/details.aspx?id=40855) avant d’appliquer le package de mise à jour ci-dessous.


- Nécessite [.NET Framework 4.5](https://www.microsoft.com/en-us/download/details.aspx?id=30653)


- Package de mise à jour de WMF 4.0 pour Windows Server 2008 R2 : [KB 3109118](https://support.microsoft.com/en-us/kb/3109118)


Il existe deux méthodes principales pour l’inventaire à l’aide de cette fonctionnalité :  
  
1.  Démarrage de la fonctionnalité de journalisation SIL pour collecter à partir de sources de données SIL et envoyer la charge sur le réseau vers une cible spécifiée (URI) toutes les heures.  
  
2.  Interrogation manuelle des données SIL à l’aide de PowerShell ou WMI à n’importe quel intervalle.  
  
Le démarrage de la journalisation SIL implique une planification et une prévoyance, mais présente des avantages importants par rapport à l’interrogation manuelle des données. L’utilisation de la journalisation SIL présente trois principaux avantages pour les administrateurs de centre de données :  
  
-   Un historique permanent (journal) peut être collecté au fil du temps, générant des rapports flexibles et complets à partir d’une source unique.  
  
-   Les défis relatifs à la découverte des ordinateurs typiques de nombreux outils d’inventaire peuvent être surmontés.  
  
-   Les défis relatifs aux délimitations d’approbation et la nécessité de privilèges utilisateur élevés typiques de nombreux outils d’inventaire peuvent être surmontés tout en conservant un niveau de sécurité, dans la mesure où les données sont chiffrées sur HTTPS avec SSL.  
  
La journalisation de l’inventaire logiciel est installée par défaut, mais elle ne démarre par défaut. La configuration de la journalisation de l’inventaire logiciel est effectuée avec les applets de commande PowerShell. Seules quelques options de configuration sont disponibles pour la journalisation de l’inventaire logiciel. Ce document décrit ces options et leur objectif, ainsi que les applets de commande destinées à la collection de données (si vous utilisez la deuxième méthode ci-dessus).  
  
**Dans ce document**  
  
Les options de configuration traitées dans ce document incluent :  
  
-   [Démarrage et arrêt de logiciel de journalisation de l’inventaire](manage-software-inventory-logging.md#BKMK_Step1)  
  
-   [Journalisation de l’inventaire logiciel au fil du temps](manage-software-inventory-logging.md#BKMK_Step2)  
  
-   [Affichage des données de journalisation de l’inventaire logiciel](manage-software-inventory-logging.md#BKMK_Step3)  
  
-   [Suppression des données consignées par la journalisation de l’inventaire logiciel](manage-software-inventory-logging.md#BKMK_Step4)  
  
-   [Sauvegarde et restauration des données consignées par la journalisation de l’inventaire logiciel] gérer-logiciel-inventaire-logging.md #BKMK_Step5)  
  
-   [Lecture des données consignées et publiées par la journalisation de l’inventaire logiciel](manage-software-inventory-logging.md#BKMK_Step6)  
  
-   [Sécurité de journalisation d’inventaire logiciel](manage-software-inventory-logging.md#BKMK_Step7)  
  
-   [Utilisation des paramètres de date et d’heure dans la journalisation de l’inventaire des logiciels du serveur de Windows](manage-software-inventory-logging.md#BKMK_Step8)  
  
-   [Activation et configuration de journalisation dans un disque dur virtuel monté de l’inventaire logiciel](manage-software-inventory-logging.md#BKMK_Step10)  
  
-   [Vue d’ensemble de l’utilisation de Software Inventory Logging dans Windows Server 2012 R2 sans KB 3000850](manage-software-inventory-logging.md#BKMK_Step11)  
  
-   [À l’aide de journalisation de l’inventaire logiciel dans un environnement Windows Server 2012 R2 Hyper-V sans KB 3000850](manage-software-inventory-logging.md#BKMK_Step12)  
  
> [!NOTE]  
> Cette rubrique inclut des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez les applets de commande à l’aide.

  
## <a name="BKMK_Step1"></a>Démarrage et arrêt de logiciel de journalisation de l’inventaire  
Collection quotidienne de journalisation de l’inventaire logiciel et de transfert sur le réseau doivent être activés sur un ordinateur exécutant Windows Server 2012 R2 pour journaliser l’inventaire logiciel.  
  
> [!NOTE]  
> Vous pouvez utiliser l’applet de commande PowerShell **[Get-SilLogging](https://technet.microsoft.com/library/dn283396.aspx)** pour récupérer des informations sur le service de journalisation de l’inventaire logiciel, notamment s’il est en cours d’exécution ou arrêté.  
  
#### <a name="to-start-software-inventory-logging"></a>Pour démarrer la journalisation de l’inventaire logiciel  
  
1.  Connectez-vous au serveur avec un compte disposant des privilèges d’administrateur local.  
  
2.  Ouvrez PowerShell en tant qu’administrateur.  
  
3.  À l’invite de PowerShell, tapez **[Start-SilLogging](https://technet.microsoft.com/library/dn283391.aspx)**  
  
> [!NOTE]  
> Il est possible de définir la cible sans définir d’empreinte de certificat, mais si vous le faites, les transferts échoueront et les données seront stockées localement pour une durée par défaut maximale de 30 jours (après quoi elles sont supprimées). Une fois qu’un hachage de certificat valide a été défini pour la cible (et qu’un certificat valide correspondant a été installé dans le magasin LocalMachine/Personal), les données stockées localement sont transmises à la cible, tant que celle-ci est configurée pour accepter ces données avec ce certificat (pour plus d’informations, voir [Software Inventory Logging Aggregator](Software-Inventory-Logging-Aggregator.md) ).  
  
#### <a name="to-stop-software-inventory-logging"></a>Pour arrêter la journalisation de l’inventaire logiciel  
  
1.  Connectez-vous au serveur avec un compte disposant des privilèges d’administrateur local.  
  
2.  Ouvrez PowerShell en tant qu’administrateur.  
  
3.  À l’invite de PowerShell, tapez **[Stop-SilLogging](https://technet.microsoft.com/library/dn283394.aspx)**  
  
## <a name="configuring-software-inventory-logging"></a>Configuration de la journalisation de l’inventaire logiciel  
Il existe trois étapes de configuration de la journalisation de l’inventaire logiciel pour transmettre des données à un serveur d’agrégation dans le temps :  
  
1.  Utilisez **Set-SilLogging – TargetUri** pour spécifier l’adresse web de votre serveur d’agrégation (elle doit commencer par « https:// »).  
  
2.  Utilisez **Set-SilLogging –CertificateThumbprint** pour spécifier le hachage d’empreinte de votre certificat SSL valide à utiliser pour authentifier les transmissions de données vers votre serveur d’agrégation (celui-ci devra être configuré pour accepter le hachage).  
  
3.  Installez un certificat SSL valide (pour votre réseau) dans le **magasin Local Machine/Personal** (ou **/LocalMachine/MY**) du serveur local à partir duquel transférer des données.  
  
Il est préférable d’effectuer ces étapes avant d’utiliser **Start-SilLogging**.  Si vous souhaitez les utiliser après l’utilisation de **Start-SilLogging**, vous devez simplement arrêter et redémarrer SIL.  Vous pouvez également utiliser l’applet de commande Publish-SilData pour vérifier que le serveur d’agrégation dispose d’un ensemble complet des données pour ce serveur.  
  
Pour obtenir un guide complet sur la configuration de l’infrastructure SIL dans son ensemble, voir [Software Inventory Logging Aggregator](software-inventory-logging-aggregator.md).  En particulier, si **Publish-SilData** génère une erreur ou que la journalisation SIL échoue par ailleurs, voir la section de dépannage.  
  
## <a name="BKMK_Step2"></a>Journalisation de l’inventaire logiciel au fil du temps  
Si un administrateur a démarré la journalisation de l’inventaire logiciel, la collection toutes les heures et le transfert des données vers le serveur d’agrégation (URI cible) commencent. Le premier transfert est un jeu de données complet des mêmes données que [Get-SilData](https://technet.microsoft.com/library/dn283388.aspx) récupère et affiche sur la console à un moment donné. Par la suite, à chaque intervalle, SIL crée une vérification des données et transfère uniquement un petit accusé de réception d’identification au serveur cible d’agrégation si les données n’ont pas été modifiées depuis la dernière collection. Si une valeur a changé, SIL envoie à nouveau un jeu de données complet.  
  
> [!IMPORTANT]  
> Si, à n’importe quel intervalle, l’URI cible est inaccessible ou le transfert des données sur le réseau a échoué pour une raison quelconque, les données collectées sont stockées localement pour une durée par défaut maximale de 30 jours (après quoi elles sont supprimées). Lors du transfert suivant des données au serveur cible d’agrégation, toutes les données stockées localement sont transférées et les données mises en cache locales sont supprimées.  
  
## <a name="BKMK_Step3"></a>Affichage des données de journalisation de l’inventaire logiciel  
Outres les applets de commande PowerShell décrites dans la section précédente, six autres applets de commande peuvent être utilisées pour collecter des données de journalisation de l’inventaire logiciel :  
  
-   **[Get-SilComputer](https://technet.microsoft.com/library/dn283392.aspx)** : affiche les valeurs à un moment donné pour des données relatives à un serveur et un système d’exploitation spécifiques, ainsi que le nom de domaine complet ou le nom de l’hôte physique, si disponible.  
  
-   **[Get-SilComputerIdentity (KB 3000850)](https://technet.microsoft.com/library/dn858074.aspx)** : affiche les identificateurs utilisés par SIL pour des serveurs individuels.  
  
-   **[Get-SilData](https://technet.microsoft.com/library/dn283388.aspx)** : affiche une collection à un moment donné de toutes les données de journalisation de l’inventaire logiciel.  
  
-   **[Get-SilSoftware](https://technet.microsoft.com/library/dn283397.aspx)** : affiche l’identité à un moment donné de tous les logiciels installés sur l’ordinateur.  
  
-   **[Get-SilUalAccess](https://technet.microsoft.com/library/dn283389.aspx)** : affiche le nombre total de demandes de périphérique client et de demandes d’utilisateur client uniques au serveur à partir de deux jours antérieurs.  
  
-   **[Get-SilWindowsUpdate](https://technet.microsoft.com/library/dn283393.aspx)** : affiche la liste à un moment donné de toutes les mises à jour de Windows installées sur l’ordinateur.  
  
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
La journalisation de l’inventaire logiciel n’est pas destinée à être un composant essentiel. Elle a été conçue pour affecter le moins possible les opérations système locales tout en maintenant un haut niveau de fiabilité. Cela permet également l’administrateur de supprimer manuellement la base de données de journalisation de l’inventaire logiciel et les fichiers associés (chaque fichier dans le répertoire de \Windows\System32\LogFiles\SIL) pour répondre aux besoins opérationnels.  
  
#### <a name="to-delete-data-logged-by-software-inventory-logging"></a>Pour supprimer les données consignées par la journalisation de l’inventaire logiciel  
  
1. Dans PowerShell, arrêtez la journalisation de l’inventaire logiciel avec la commande **[Stop-SilLogging](https://technet.microsoft.com/library/dn283394.aspx)** .  
  
2. Ouvrez l’Explorateur Windows.  
  
3. Accédez à **\Windows\System32\Logfiles\SIL\\**  
  
4. Supprimez tous les fichiers de ce dossier.  
  
## <a name="BKMK_Step5"></a>Sauvegarde et restauration des données consignées par la journalisation de l’inventaire logiciel  
La journalisation de l’inventaire logiciel stocke temporairement les collections de données effectuées toutes les heures si le transfert sur le réseau échoue. Les fichiers journaux sont stockés dans le répertoire \Windows\System32\LogFiles\SIL\. Les sauvegardes des données de journalisation de l’inventaire logiciel peuvent être effectuées avec vos sauvegardes planifiées du serveur.  
  
> [!IMPORTANT]  
> Si, pour une raison quelconque, une réparation de l’installation ou une mise à niveau du système d’exploitation est nécessaire, tous les fichiers journaux stockés localement seront perdus.  Si ces données sont importantes pour les opérations, il est recommandé de les sauvegarder avant l’installation du nouveau système d’exploitation. Après la réparation ou la mise à niveau, restaurez simplement les données au même emplacement.  
  
> [!NOTE]  
> Si pour une raison quelconque la gestion de la rétention des données enregistrées localement par SIL devient importante, cela peut être configuré en modifiant la valeur de Registre : \HKEY_LOCAL_MACHINE\\SOFTWARE\Microsoft\Windows\SoftwareInventoryLogging. La valeur par défaut est « 30 » pour 30 jours.  
  
## <a name="BKMK_Step6"></a>Lecture des données consignées et publiées par la journalisation de l’inventaire logiciel  
Les données enregistrées par SIL mais stockées localement (si le transfert vers l’URI cible échoue) ou les données transférées au serveur cible d’agrégation sont stockées dans un fichier binaire (pour les données de chaque jour). Pour afficher ces données dans PowerShell, utilisez l’applet de commande [Import-BinaryMiLog](https://technet.microsoft.com/library/dn262592.aspx) .  
  
## <a name="BKMK_Step7"></a>Sécurité de journalisation d’inventaire logiciel  
Des privilèges d’administrateur sur le serveur local sont requis pour récupérer les données à partir des API WMI et PowerShell de la journalisation de l’inventaire logiciel.  
  
Pour exploiter correctement la fonctionnalité complète de journalisation de l’inventaire logiciel afin de transférer des données vers un point d’agrégation en permanence au fil du temps (à des intervalles d’une heure), un administrateur doit utiliser des certificats clients pour garantir des sessions SSL sécurisées pour le transfert de données via le protocole HTTPS. Vous trouverez ci-dessous une vue d’ensemble de l’authentification HTTPS : [Authentification HTTPS](https://technet.microsoft.com/library/cc736680(v=WS.10).aspx).  
  
Toutes les données stockées localement sur un serveur Windows (se produit uniquement si la fonctionnalité est démarrée mais la cible est inaccessible pour une raison quelconque) ne sont accessibles qu’avec des privilèges d’administrateur sur le serveur local.  
  
## <a name="BKMK_Step8"></a>Utilisation des paramètres de date et d’heure dans Windows Server 2012 R2 logiciel de journalisation de l’inventaire  
  
-   Lorsque vous utilisez [Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) - TimeOfDay pour définir le moment auquel la journalisation SIL s’exécute, vous devez spécifier une date et une heure. La date du calendrier est définie et la journalisation n’a pas lieu tant que la date n’est pas atteinte, en heure locale du système.  
  
-   Lorsque vous utilisez [Get-SilSoftware](https://technet.microsoft.com/library/dn283397.aspx), ou [Get-SilWindowsUpdate](https://technet.microsoft.com/library/dn283393.aspx), « InstallDate » affichera toujours les 12:00:00 AM, une valeur sans signification.  
  
-   Lorsque vous utilisez [Get-SilUalAccess](https://technet.microsoft.com/library/dn283389.aspx), « SampleDate » affichera toujours 11:59:00 PM, une valeur sans signification.  La date est la donnée pertinente pour ces requêtes d’applet de commande.  
  
## <a name="BKMK_Step10"></a>Activation et configuration de journalisation dans un disque dur virtuel monté de l’inventaire logiciel  
La journalisation de l’inventaire logiciel prend également en charge la configuration et l’activation sur des ordinateurs virtuels hors connexion. Les utilisations pratiques couvrent les paramètres « d’image gold » pour déploiement à grande échelle entre les centres de données, ainsi que la configuration des images des utilisateurs finaux allant d’un site à un déploiement de cloud.  
  
Pour prendre en charge ces utilisations, la journalisation de l’inventaire logiciel comporte des entrées de Registre associées à chaque option configurable.  Vous trouverez ces valeurs de Registre sous \HKEY_LOCAL_MACHINE\\SOFTWARE\Microsoft\Windows\SoftwareInventoryLogging.  
  
|||||  
|-|-|-|-|  
|**(Fonction)**|**Nom de la valeur**|**Données**|**Applet de commande correspondante (disponible uniquement dans le système d’exploitation en cours d’exécution)**|  
|Fonctionnalité de démarrage/d’arrêt|CollectionState|1 ou 0|[Start-SilLogging](https://technet.microsoft.com/library/dn283391.aspx), [Stop-SilLogging](https://technet.microsoft.com/library/dn283394.aspx)|  
|Spécifie le point d’agrégation cible sur le réseau|TargetUri|chaîne|[Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) - TargetURI|  
|Spécifie l’empreinte du certificat ou le hachage du certificat utilisé pour l’authentification SSL pour le serveur web cible|CertificateThumbprint|chaîne|[Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) -CertificateThumbprint|  
|Spécifie la date et l’heure auxquelles la fonctionnalité débute (si la valeur définie est dans le futur selon l’heure système locale)|CollectionTime|Default :  2000-01-01T03:00:00|[Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) -TimeOfDay|  
  
Pour modifier ces valeurs sur un disque dur virtuel hors connexion (système d’exploitation de l’ordinateur inactif), un disque dur virtuel doit tout d’abord être monté, et ensuite les commandes suivantes peuvent être utilisées pour apporter des modifications :  
  
-   [Reg load](https://technet.microsoft.com/library/cc742053.aspx)  
  
-   [Reg delete](https://technet.microsoft.com/library/cc742145.aspx)  
  
-   [Reg add](https://technet.microsoft.com/library/cc742162.aspx)  
  
-   [Reg unload](https://technet.microsoft.com/library/cc742043.aspx)  
  
La journalisation de l’inventaire logiciel vérifie ces valeurs lorsque le système d’exploitation démarre et s’exécute en conséquence.  
  
## <a name="BKMK_Step11"></a>Vue d’ensemble de l’utilisation de Software Inventory Logging dans Windows Server 2012 R2 sans KB 3000850  
Les modifications suivantes des paramètres par défaut et de la fonctionnalité de journalisation de l’inventaire logiciel ont été effectuées avec [KB 3000850](https://support.microsoft.com/kb/3000850):  
  
-   L’intervalle par défaut pour la collection et le transfert sur le réseau au démarrage de la journalisation SIL a été remplacé par toutes les heures à la place de quotidien (au hasard dans chaque heure).  
  
-   La charge utile de données par défaut a été réduite pour inclure uniquement les objets provenant de SilComputer, Get-SilComputerIdentity et Get-SilSoftware.  
  
-   L’invité pour la communication de canal hôte dans les environnements Hyper-V a été supprimé.  
  
## <a name="BKMK_Step12"></a>À l’aide de journalisation de l’inventaire logiciel dans un environnement Windows Server 2012 R2 Hyper-V sans KB 3000850  
  
> [!NOTE]  
> Cette fonctionnalité est supprimée avec l’installation de la mise à jour [KB 3000850](https://support.microsoft.com/kb/3000850).  
  
Lorsque vous utilisez la journalisation de l’inventaire logiciel sur un ordinateur hôte Windows Server 2012 R2 Hyper-V, il est possible de récupérer des données SIL à partir de Windows Server 2012 R2 invités qui sont exécutent localement, si la journalisation SIL a été démarrée dans l’ou les invités. Cependant, ceci est uniquement possible lorsque vous utilisez les applets de commande Get-SilData et Publish-SilData Powershell et n’est possible avec WIndows Server 2012 R2 dans l’hôte et invité.  Cette fonctionnalité vise à permettre aux administrateurs de centre de données qui fournissent des ordinateurs virtuels invités aux locataires ou autres entités d’une grande entreprise, de capturer des données d’inventaire logiciel sur l’hôte hyperviseur et, par la suite, de transférer toutes ces données à un agrégateur (ou un URI cible).  
  
Voici deux exemples de démarrage de machine virtuelle avec la journalisation SIL quelles la sortie dans la console ressemble (très abrégés) sur un hôte Windows Server 2012 R2 Hyper-V un invité de Windows Server 2012 R2 en cours d’exécution de PowerShell.  Vous remarquerez que le premier exemple, qui utilise Get-SilData seul, affiche toutes les données de l’hôte comme prévu.  Toutes les données SIL de l’invité sont également incluses, mais dans un format réduit.  Pour développer et afficher ces données à partir de l’invité, coupez et collez simplement l’extrait de code utilisé dans le deuxième exemple ci-dessous.  Les objets de données SIL de l’invité auront toujours le GUID de l’ordinateur virtuel associé au sein de l’objet.  
  
> [!NOTE]  
> Les données SIL étant une sortie sur la console, lors de l’utilisation de l’applet de commande Get-SilData, dans des flux de données, les objets ne sortiront pas systématiquement dans l’ordre prévu.  Dans les deux exemples ci-dessous, le texte a été codé par couleur (bleu pour les données de l’hôte physique et vert pour les données de l’invité virtuel) uniquement comme outil d’illustration de ce document.  
  
**Exemple de sortie 1**  
  
![](../media/software-inventory-logging/SILHyper-VExample1.png)  
  
**Exemple de sortie 2** (avec la fonction Expand-SilData)  
  
![](../media/software-inventory-logging/SILHyper-VExample2.png)  
  
## <a name="see-also"></a>Voir aussi  
[Bien démarrer avec le logiciel de journalisation de l’inventaire](get-started-with-software-inventory-logging.md)  
[Agrégateur de journalisation de l’inventaire logiciel](software-inventory-logging-aggregator.md)  
[Applets de commande journalisation de l’inventaire logiciel dans Windows PowerShell](https://technet.microsoft.com/library/dn283390.aspx)  
[Import-BinaryMiLog](https://technet.microsoft.com/library/dn262592.aspx)  
[Export-BinaryMiLog](https://technet.microsoft.com/library/dn262591.aspx)  
  

