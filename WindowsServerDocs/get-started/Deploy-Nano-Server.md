---
title: Déployer Nano Server
description: Explique la création et le déploiement des images personnalisées, packages, pilotes, domaines, rôles, fonctionnalités
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9f109c91-7c2e-4065-856c-ce9e2e9ce558
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 8953627b7240bdb6bd0ea76b09c5af8317bde186
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830760"
---
# <a name="deploy-nano-server"></a>Déployer Nano Server

>S'applique à : Windows Server 2016

> [!IMPORTANT]
> À compter de Windows Server, version 1709, Nano Server sera uniquement disponible sous forme d’[image du système d’exploitation de base du conteneur](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Consultez [Modifications apportées à Nano Server](nano-in-semi-annual-channel.md) pour en savoir plus. 

Cette rubrique contient les informations nécessaires au déploiement des images Nano Server plus personnalisées en fonction de vos besoins que les exemples simples décrits dans la rubrique sur le démarrage rapide avec Nano Server. Vous trouverez également des informations sur la création d’une image Nano Server personnalisée avec exactement les fonctionnalités souhaitées, l’installation des images Nano Server à partir de fichiers VHD ou WIM, la modification des fichiers, l’utilisation des domaines, la gestion des packages grâce à plusieurs méthodes et l’utilisation des rôles serveur.

## <a name="nano-server-image-builder"></a>Générateur d’images Nano Server

Le générateur d’images Nano Server est un outil qui vous permet de créer une image Nano Server personnalisée et un support USB démarrable à l’aide d’une interface graphique. En fonction des entrées que vous fournissez, il génère des scripts PowerShell réutilisables qui vous permettent d’automatiser facilement les installations cohérentes de Nano Server exécutant les éditions Standard ou Datacenter de Windows Server 2016.

Procurez-vous l’outil auprès du [Centre de téléchargement](https://www.microsoft.com/en-us/download/details.aspx?id=54065). 

Cet outil nécessite également le [Kit de déploiement et d’évaluation Windows](https://developer.microsoft.com/en-us/windows/hardware/windows-assessment-deployment-kit).


Le générateur d’images Nano Server crée des images Nano Server personnalisées aux formats VHD, VHDX ou ISO et peut créer un support USB démarrable pour déployer Nano Server ou détecter la configuration matérielle d’un serveur. Il peut également effectuer les opérations suivantes :

- Accepter les termes du contrat de licence 
- Créer des formats VHD, VHDX ou ISO
- Ajouter des rôles serveur
- Ajouter des pilotes de périphérique
- Définir le nom de l’ordinateur, le mot de passe administrateur, le chemin d’accès du fichier journal et le fuseau horaire
- Joindre un domaine à l’aide d’un compte Active Directory existant ou d’un objet blob de jonction de domaine collecté
- Activer WinRM pour la communication en dehors du sous-réseau local
- Activer les ID de réseau local virtuel et configurer des adresses IP statiques
- Injecter de nouveaux packages de maintenance à la volée
- Ajouter un setupcomplete.cmd ou d’autres scripts clients à exécuter une fois le fichier unattend.xml traité
- Activer les services de gestion d’urgence pour l’accès à la console avec port série
- Activer les services de développement pour tester les pilotes signés et les applications non signées, l’interpréteur de commandes par défaut PowerShell
- Activer le débogage sur les protocoles IEEE 1394 ou série, USB, TCP/IP
- Créer un support USB à l’aide de WinPE qui partitionnera le serveur et installera l’image Nano
- Créer un support USB à l’aide de WinPE qui détectera votre configuration matérielle Nano Server existante et signalera tous les détails dans un journal et à l’écran. Cela inclut les cartes réseau, les adresses MAC et le type du microprogramme (BIOS ou UEFI). Le processus de détection va également répertorier tous les volumes sur le système et les périphériques dépourvus de pilote dans le package de pilotes Server Core.

Si vous n’êtes pas familier avec une ou plusieurs des actions susmentionnées, consultez le reste de cette rubrique, ainsi que les autres rubriques relatives à Nano Server pour être prêt à fournir à l’outil les informations nécessaires.

## <a name="BKMK_CreateImage"></a>Création d’une image Nano Server personnalisée  
Pour Windows Server 2016, Nano Server est distribué sur le support physique dans lequel vous trouverez un dossier **NanoServer** contenant une image .wim et un sous-dossier appelé **Packages**. Ce sont ces fichiers de package que vous utilisez pour ajouter des rôles serveur et des fonctionnalités à l’image VHD, à partir de laquelle vous démarrerez ensuite.  
  
Vous pouvez également rechercher et installer ces packages avec le fournisseur NanoServerPackage du module PowerShell PackageManagement (OneGet). Consultez la section «Installation des rôles et des fonctionnalités en ligne» de cette rubrique.  
  
Ce tableau présente les rôles et les fonctionnalités disponibles dans cette version de Nano Server, ainsi que les options Windows PowerShell qui installeront les packages pour ces derniers. Certains packages sont directement installés avec leurs propres commutateurs Windows PowerShell (par exemple, -Compute). D’autres devront être installés en transmettant les noms de package au paramètre -Package que vous pouvez combiner dans une liste séparée par des virgules. Vous pouvez répertorier dynamiquement les packages disponibles à l’aide de l’applet de commande Get-NanoServerPackage.  
  
|Rôle ou fonctionnalité|Option|
|-------------------|----------|
|Rôle Hyper-V (y compris NetQoS)|-Compute|
|Clustering de basculement et autres éléments détaillés à la suite de ce tableau|-Clustering|
|Pilotes de base pour plusieurs cartes réseau et contrôleurs de stockage. Il s’agit du même jeu de pilotes inclus dans une installation minimale de Windows Server 2016.|-OEMDrivers|
|Rôle de serveur de fichiers et autres éléments de stockage détaillés à la suite de ce tableau|-Storage|
|Windows Defender, y compris un fichier de signature par défaut|-Defender|
|Redirecteurs inversés pour la compatibilité des applications, par exemple les infrastructures d’application courantes telles que Ruby, Node.js, etc.|Désormais inclus par défaut|
|Rôle de serveur DNS|-Package Microsoft-NanoServer-DNS-Package|
|Configuration de l’état souhaité de PowerShell (DSC)|-Package Microsoft-NanoServer-DSC-Package<br />**Remarque :** Pour plus d’informations, consultez [Utilisation de DSC sur Nano Server](https://msdn.microsoft.com/powershell/dsc/nanoDsc).|
|Internet Information Server (IIS)|-Package Microsoft-NanoServer-IIS-Package<br />**Remarque :** Consultez [IIS sur Nano Server](IIS-on-Nano-Server.md) pour plus d’informations sur l’utilisation d’IIS.|
|Prise en charge hôte pour les conteneurs Windows|-Containers|
|Agent System Center Virtual Machine Manager|-Package Microsoft-NanoServer-SCVMM-Package<br />-Package Microsoft-NanoServer-SCVMM-Compute-Package<br />**Remarque :** Utilisez le package SCVMM Compute uniquement si vous analysez Hyper-V. Pour les déploiements hyperconvergés dans VMM, vous devez également spécifier le paramètre - Storage. Pour plus d’informations, voir la [documentation VMM](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-compute-add-nano-hyper-v).| 
|Agent System Center Operations Manager| Installé séparément. Consultez la documentation de System Center Operations Manager pour plus d’informations sur https://technet.microsoft.com/system-center-docs/om/manage/install-agent-on-nano-server.|
|Data Center Bridging (y compris DCBQoS)|-Package Microsoft-NanoServer-DCB-Package|
|Déploiement sur une machine virtuelle|-Package Microsoft-NanoServer-Guest-Package|
|Déploiement sur un ordinateur physique|-Package Microsoft-NanoServer-Host-Package|
|BitLocker, module de plateforme sécurisée (TPM), chiffrement de volume, identification de plateforme, fournisseurs de chiffrement et autres fonctionnalités pour un démarrage sécurisé|-Package Microsoft-NanoServer-SecureStartup-Package|
|Prise en charge Hyper-V des machines virtuelles dotées d’une protection maximale|-Package Microsoft-NanoServer-ShieldedVM-Package<br />**Remarque :** Ce package est uniquement disponible pour l’Édition Datacenter de Nano Server.|
|Agent SNMP (Simple Network Management Protocol)|-Package Microsoft-NanoServer-SNMP-Agent-Package.cab<br />**Remarque :** Pas inclus avec le support d’installation de Windows Server 2016. Disponible en ligne uniquement. Voir [Installation des rôles et des fonctionnalités en ligne](https://technet.microsoft.com/windows-server-docs/get-started/deploy-nano-server#a-namebkmkonlineainstalling-roles-and-features-online) pour plus d’informations.|
|Service IPHelper qui fournit une connectivité tunnel avec des technologies de transition IPv6 (6to4, ISATAP, Port Proxy et Teredo) et IP-HTTPS|-Package Microsoft-NanoServer-IPHelper-Service-Package.cab<br />**Remarque :** Pas inclus avec le support d’installation de Windows Server 2016. Disponible en ligne uniquement. Voir [Installation des rôles et des fonctionnalités en ligne](https://technet.microsoft.com/windows-server-docs/get-started/deploy-nano-server#a-namebkmkonlineainstalling-roles-and-features-online) pour plus d’informations.|

> [!NOTE]  
> Lorsque vous installez les packages avec ces options, un module linguistique correspondant est également installé en fonction des paramètres régionaux du support de serveur sélectionné. Vous pouvez trouver les modules linguistiques disponibles et leurs abréviations de paramètres régionaux dans le support d’installation dans des sous-dossiers nommés en fonction du paramètre régional de l’image.  
  
> [!NOTE]  
> Lorsque vous utilisez le paramètre -Storage pour installer les services de fichiers, ceux-ci ne sont pas réellement activés. Activez cette fonctionnalité à partir d’un ordinateur distant avec le Gestionnaire de serveur. 

### <a name="failover-clustering-items-installed-by-the--clustering-parameter"></a>Éléments de clustering de basculement installés avec le paramètre -Clustering

- Rôle de clustering de basculement
- Clustering de basculement des machines virtuelles
- Espaces de stockage directs (S2D)
- Qualité de service de stockage
- Clustering de réplication de volume
- Service Témoin SMB


### <a name="file-and-storage-items-installed-by-the--storage-parameter"></a>Éléments de stockage et de fichier installés avec le paramètre -Storage

- Rôle de serveur de fichiers
- Déduplication des données
- MPIO (Multipath I/O), y compris un pilote pour le module spécifique de périphériques Microsoft (MSDSM)
- ReFS (v1 et v2)
- Initiateur iSCSI (mais pas Cible iSCSI)
- Réplica de stockage
- Service de gestion du stockage avec prise en charge de SMI-S
- Service Témoin SMB
- Volumes dynamiques
- Fournisseurs de stockage Windows de base (pour la gestion de stockage Windows)
 
  
  
  
### <a name="installing-a-nano-server-vhd"></a>Installation d’un fichier VHD Nano Server  
Cet exemple crée une image VHDX basée sur GPT avec un nom d’ordinateur donné et comprenant des pilotes invités Hyper-V, en commençant par le support d’installation Nano Server sur un partage réseau. Dans une invite Windows PowerShell avec élévation de privilèges, commencez avec cette applet de commande :  
  
`Import-Module <Server media location>\NanoServer\NanoServerImageGenerator; New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\server_en-us -BasePath .\Base -TargetPath .\FirstStepsNano.vhdx -ComputerName FirstStepsNano`  
  
L’applet de commande exécutera toutes les tâches suivantes :  
  
1.  Sélectionner Standard comme édition de base  
  
2.  Vous inviter à indiquer le mot de passe administrateur  
  
3.  Copier le support d’installation à partir de \\\Path\To\Media\server_en-us en. \Base  
  
4.  Convertir l’image WIM en image VHD (L’extension de fichier de l’argument de chemin d’accès cible détermine si elle crée un fichier VHD basé sur MBR pour les ordinateurs virtuels de génération1 ou un fichier VHDX basé sur GPT pour les ordinateurs virtuels de génération2).  
  
5.  Copier le fichier VHD résultant dans .\FirstStepsNano.vhdx  
  
6.  Définir le mot de passe administrateur pour l’image comme spécifié  
  
7.  Définir le nom d’ordinateur de l’image sur FirstStepsNano  
  
8.  Installer les pilotes invités Hyper-V  
  
Tout cela permet de créer une image de .\FirstStepsNano.vhdx.  
  
L’applet de commande génère un journal à mesure de l’exécution et vous indiquera l’emplacement de celui-ci une fois l’exécution terminée. La conversion WIM-VHD effectuée par le script d’accompagnement génère son propre journal dans %TEMP%\Convert-WindowsImage\\\<GUID&gt; (où \<GUID&gt; est un identificateur unique par session de conversion).  
  
Tant que vous utilisez le même chemin d’accès de base, vous pouvez omettre le paramètre de chemin d’accès du support chaque fois que vous exécutez cette applet de commande, car il utilisera les fichiers mis en cache dans le chemin d’accès de base. Si vous ne spécifiez pas de chemin d’accès de base, l’applet de commande en génèrera un par défaut dans le dossier TEMP. Si vous souhaitez utiliser un support source différent, mais le même chemin d’accès de base, vous devez toutefois spécifier le paramètre de chemin d’accès du support.  
  
>[!NOTE]  
>Vous pouvez maintenant spécifier l’édition de Nano Server à générer, soit Standard soit Datacenter. Utilisez le paramètre -Edition pour spécifier l’édition *Standard* ou *Datacenter*.  
  
Une fois que vous disposez d’une image, vous pouvez la modifier comme nécessaire à l’aide de l’applet de commande *Edit-NanoServerImage*.  
  
Si vous ne spécifiez pas de nom d’ordinateur, un nom aléatoire sera généré.  
  
### <a name="installing-a-nano-server-wim"></a>Installation d’un fichier WIM Nano Server  
  
1.  Copiez le dossier *NanoServerImageGenerator* du dossier \NanoServer du fichier ISO de Windows Server 2016 dans un dossier local sur votre ordinateur.  
2. Démarrez Windows PowerShell en tant qu’administrateur, accédez au répertoire où vous avez placé le dossier NanoServerImageGenerator, puis importez le module avec `Import-Module .\NanoServerImageGenerator -Verbose`.  
  
 >[!NOTE]  
>Il se peut que vous deviez modifier la stratégie d’exécution de Windows PowerShell. `Set-ExecutionPolicy RemoteSigned` devrait bien fonctionner.  
  
Pour créer une image Nano Server et l’utiliser en tant qu’ordinateur hôte Hyper-V, exécutez ce qui suit:  
  
`New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath <path to root of media> -BasePath .\Base -TargetPath .\NanoServerPhysical\NanoServer.wim -ComputerName <computer name> -OEMDrivers -Compute -Clustering`  
  
Où  
-   -MediaPath correspond à la racine du support DVD ou à l’image ISO contenant Windows Server 2016.  
-   -BasePath contiendra une copie des fichiers binaires de Nano Server, afin que vous puissiez utiliser New-NanoServerImage -BasePath sans avoir besoin de spécifier -MediaPath lors des exécutions futures.  
-   -TargetPath contiendra le fichier .wim obtenu contenant les rôles et fonctionnalités que vous avez sélectionnés. Veillez à spécifier l’extension .wim.  
-   -Compute ajoute le rôle Hyper-V.  
-   -OemDrivers ajoute un nombre de pilotes courants.  
  
Vous serez invité à entrer un mot de passe administrateur.  
  
Pour plus d’informations, exécuter `Get-Help New-NanoServerImage -Full`.  
   
Démarrez dans WinPE et assurez-vous que le fichier .wim tout juste créé est accessible à partir de WinPE. (Vous pouvez, par exemple, copier le fichier .wim sur une image WinPE démarrable sur un lecteur flash USB).  
  
Au démarrage de WinPE, utilisez Diskpart.exe pour préparer le disque dur de l’ordinateur cible. Exécutez les commandes Diskpart suivantes (modifiez en conséquence si vous n’utilisez pas UEFI et GPT):  
   
> [!WARNING]  
> Ces commandes supprimeront toutes les données du disque dur.  
  
**Taille d’efi de partition GPT de convertir propre créer DiskPart.exe sélectionnez le disque 0 = 100 FS rapides Format = FAT32 label = « Système » Assign lettre = « s » Create partition msr size = 128 créer FS rapide de Format principal de la partition = NTFS label = « NanoServer » Assign lettre = volume de liste « n » Sortie**  
   
Appliquez l’image Nano Server (ajustez le chemin d’accès du fichier .wim) :  
  
**DISM.exe /apply-image /imagefile:.\NanoServer.wim /index:1 /applydir:n : \ BCDboot.exe n:\Windows /s s:**  
   
Retirez le support DVD ou le lecteur USB et redémarrez votre système avec **Wpeutil.exe Reboot**  
  
  
### <a name="editing-files-on-nano-server-locally-and-remotely"></a>Modification des fichiers sur Nano Server localement et à distance  
 Dans les deux cas, connectez-vous à Nano Server, comme avec la communication à distance Windows PowerShell.  
   
 Une fois que vous êtes connecté à Nano Server, vous pouvez modifier un fichier résidant sur votre ordinateur local en transmettant le chemin d’accès relatif ou absolu du fichier à la commande psEdit, par exemple:   
`psEdit C:\Windows\Logs\DISM\dism.log` ou `psEdit .\myScript.ps1`  
  
Modifiez un fichier résidant sur Nano Server à distance en démarrant une session à distance avec `Enter-PSSession -ComputerName "192.168.0.100" -Credential ~\Administrator`, puis en transmettant le chemin d’accès relatif ou absolu du fichier à la commande psEdit comme suit :   
`psEdit C:\Windows\Logs\DISM\dism.log`  
  
## <a name="BKMK_online"></a>Installez des rôles et fonctionnalités en ligne  
> [!NOTE]
> Si vous installez un package Nano Serveur facultatif à partir d’un support ou d’un référentiel en ligne, aucun correctif de sécurité récent n’est inclus. Pour éviter une incompatibilité de version entre les packages facultatifs et le système d’exploitation, vous devez installer la [dernière mise à jour cumulative](https://technet.microsoft.com/windows-server-docs/get-started/update-nano-server) immédiatement après l’installation des packages facultatifs et **avant** le redémarrage du serveur.

### <a name="installing-roles-and-features-from-a-package-repository"></a>Installation des rôles et des fonctionnalités à partir d’un référentiel de packages  
Vous pouvez rechercher et installer des packages Nano Server à partir du référentiel de packages en ligne à l’aide du fournisseur NanoServerPackage du module PowerShell PackageManagement. Pour installer ce fournisseur, utilisez ces applets de commande :

```powershell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
```

>[!NOTE]
>Si vous rencontrez des erreurs lors de l’exécution de Install-PackageProvider, vérifiez que vous avez installé la [dernière mise à jour cumulative](https://technet.microsoft.com/windows-server-docs/get-started/update-nano-server) ([KB3206632](https://support.microsoft.com/kb/3206632) ou version ultérieure), ou utilisez Save-Module comme suit : 

```powershell
Save-Module -Path "$Env:ProgramFiles\WindowsPowerShell\Modules\" -Name NanoServerPackage -MinimumVersion 1.0.1.0
Import-PackageProvider NanoServerPackage
```

Une fois que ce fournisseur a été installé et importé, vous pouvez rechercher, télécharger et installer des packages Nano Server à l’aide des applets de commande conçues spécialement à cet effet.

```powershell
Find-NanoServerPackage  
Save-NanoServerPackage  
Install-NanoServerPackage
```  
  
Vous pouvez également utiliser les applets de commande PackageManagement génériques pour spécifier le fournisseur NanoServerPackage :

```powershell
Find-Package -ProviderName NanoServerPackage
Save-Package -ProviderName NanoServerPackage
Install-Package -ProviderName NanoServerPackage
Get-Package -ProviderName NanoServerPackage
```
  
Pour utiliser ces applets de commande avec les packages Nano Server sur Nano Server, ajoutez `-ProviderName NanoServerPackage`. Si vous n’ajoutez pas le paramètre -ProviderName, PackageManagement effectuera une itération de tous les fournisseurs. Pour plus d’informations sur ces applets de commande, exécutez `Get-Help <cmdlet>`. Voici quelques exemples d’utilisations courantes :
    
### <a name="searching-for-nano-server-packages"></a>Recherche des packages Nano Server  
Vous pouvez utiliser `Find-NanoServerPackage` ou `Find-Package -ProviderName NanoServerPackage` pour rechercher et renvoyer une liste des packages Nano Server qui sont disponibles dans le référentiel en ligne. Par exemple, vous pouvez obtenir une liste de tous les packages les plus récents :

```powershell
Find-NanoServerPackage
```
  
Exécutez `Find-Package -ProviderName NanoServerPackage -DisplayCulture` pour afficher toutes les cultures disponibles.

Si vous avez besoin d’une version de paramètres régionaux spécifique, tels que l’anglais des États-Unis, vous pouvez utiliser `Find-NanoServerPackage -Culture en-us` ou  
`Find-Package -ProviderName NanoServerPackage -Culture en-us` ou `Find-Package -Culture en-us -DisplayCulture`.

Pour rechercher un package spécifique par nom de package, utilisez le paramètre -Name. Ce paramètre accepte également les caractères génériques. Par exemple, pour rechercher tous les packages dont le nom inclut VMM, utilisez `Find-NanoServerPackage -Name *VMM*` ou `Find-Package -ProviderName NanoServerPackage -Name *VMM*`.

Vous pouvez rechercher une version particulière avec les paramètres -RequiredVersion, -MinimumVersion ou -MaximumVersion. Pour rechercher toutes les versions disponibles, utilisez -AllVersions. Sinon, seule la version la plus récente est renvoyée. Par exemple : `Find-NanoServerPackage -Name *VMM* -RequiredVersion 10.0.14393.0`. Ou, pour toutes les versions : `Find-Package -ProviderName NanoServerPackage -Name *VMM* -AllVersions`

### <a name="installing-nano-server-packages"></a>Installation de packages Nano Server  
Vous pouvez installer un package Nano Server (y compris ses packages de dépendance, le cas échéant) sur Nano Server, localement ou sur une image hors connexion avec `Install-NanoServerPackage` ou `Install-Package -ProviderName NanoServerPackage`. Ces deux applets de commande acceptent des entrées provenant du pipeline.

Pour installer la version la plus récente d’un package Nano Server sur un Nano Server en ligne, utilisez `Install-NanoServerPackage -Name Microsoft-NanoServer-Containers-Package` ou `Install-Package -Name Microsoft-NanoServer-Containers-Package`. PackageManagement utilisera la culture du Nano Server.

Vous pouvez installer un package Nano Server sur une image hors connexion, et spécifier une version et une culture particulières, comme suit :

`Install-NanoServerPackage -Name Microsoft-NanoServer-DCB-Package -Culture de-de -RequiredVersion 10.0.14393.0 -ToVhd C:\MyNanoVhd.vhd`

ou:

`Install-Package -Name Microsoft-NanoServer-DCB-Package -Culture de-de -RequiredVersion 10.0.14393.0 -ToVhd C:\MyNanoVhd.vhd`

Voici quelques exemples de traitement en pipeline des résultats de recherche de package sur l’applet de commande d’installation :  

`Find-NanoServerPackage *dcb* | Install-NanoServerPackage` recherche tous les packages avec « dcb » dans le nom, puis les installe.

`Find-Package *nanoserver-compute-* | Install-Package` recherche les packages avec « nanoserver - compute- » dans le nom et les installe.
  
`Find-NanoServerPackage -Name *nanoserver-compute* | Install-NanoServerPackage -ToVhd C:\MyNanoVhd.vhd` recherche les packages avec « compute » dans le nom et les installe sur une image hors connexion.

`Find-Package -ProviderName NanoserverPackage *nanoserver-compute-* | Install-Package -ToVhd C:\MyNanoVhd.vhd` fait la même chose avec les packages dont le nom contient « nanoserver - compute- ».

### <a name="downloading-nano-server-packages"></a>Téléchargement de packages Nano Server  

`Save-NanoServerPackage` ou `Save-Package` vous permettent de télécharger les packages et les enregistrer sans les installer. Ces deux applets de commande acceptent des entrées provenant du pipeline.

Par exemple, pour télécharger et enregistrer un package Nano Server dans un répertoire qui correspond au chemin d’accès générique, utilisez `Save-NanoServerPackage -Name Microsoft-NanoServer-DNS-Package -Path C:\`. Dans cet exemple, la culture de la machine locale sera utilisée, car -Culture n’a pas été spécifié. Comme aucune version n’a été spécifiée, c’est la version la plus récente qui sera enregistrée.

`Save-Package -ProviderName NanoServerPackage -Name Microsoft-NanoServer-IIS-Package -Path C:\ -Culture it-IT -MinimumVersion 10.0.14393.0` enregistre une version particulière et pour les paramètres régionaux et la langue italienne.

Vous pouvez envoyer des résultats de recherche via le pipeline, comme dans les exemples suivants :

`Find-NanoServerPackage -Name *containers* -MaximumVersion 10.2 -MinimumVersion 1.0 -Culture es-ES | Save-NanoServerPackage -Path C:\`

ou

`Find-Package -ProviderName NanoServerPackage -Name *shield* -Culture es-ES | Save-Package -Path`

### <a name="inventory-installed-packages"></a>Inventaire des packages installés
Vous pouvez détecter quels sont les packages Nano Server installés avec `Get-Package`. Par exemple, découvrez quels sont les packages installés sur Nano Server avec `Get-Package -ProviderName NanoserverPackage`.

Pour vérifier quels sont les packages Nano Server installés sur une image hors connexion, exécutez `Get-Package -ProviderName NanoserverPackage -FromVhd C:\MyNanoVhd.vhd`.


### <a name="installing-roles-and-features-from-local-source"></a>Installation des rôles et des fonctionnalités à partir d’une source locale  
Bien que l’installation hors connexion des rôles serveur et des autres packages soit recommandée, vous devrez peut-être les installer en ligne (avec Nano Server en cours d’exécution) dans les scénarios avec conteneurs. Pour cela, procédez comme suit:  
  
1.  Copiez le dossier Packages du support d’installation localement sur le Nano Server en cours d’exécution (par exemple, sur C:\packages).  
  
2.  Créez un fichier Unattend.xml sur un autre ordinateur et copiez-le sur Nano Server. Vous pouvez copier et coller ce contenu XML dans le fichier XML que vous avez créé (cet exemple illustre l’installation du package IIS) :  
  
   
     
```  
<?xml version="1.0" encoding="utf-8"?>
    <unattend xmlns="urn:schemas-microsoft-com:unattend">  
    <servicing>  
        <package action="install">  
            <assemblyIdentity name="Microsoft-NanoServer-IIS-Feature-Package" version="10.0.14393.0" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" />  
            <source location="c:\packages\Microsoft-NanoServer-IIS-Package.cab" />  
        </package>  
        <package action="install">  
            <assemblyIdentity name="Microsoft-NanoServer-IIS-Feature-Package" version="10.0.14393.0" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="en-US" />  
            <source location="c:\packages\en-us\Microsoft-NanoServer-IIS-Package_en-us.cab" />  
        </package>  
    </servicing>  
    <cpi:offlineImage cpi:source="" xmlns:cpi="urn:schemas-microsoft-com:cpi" />  
</unattend>  
```  
  
  
  
  
3.  Dans le nouveau fichier XML que vous avez créé (ou copié), remplacez C:\packages par le répertoire dans lequel vous avez copié le contenu de Packages.  
  
4.  Accédez au répertoire contenant le fichier XML créé et exécutez  
  
    **dism /online /apply-unattend:.\unattend.xml**  
  
     
  
5.  Vérifiez que le package et son module linguistique sont correctement installés en exécutant:  
  
    **dism /online /get-packages**  
  
    Vous devez voir « identité du Package : Microsoft-NanoServer-IIS-Package ~ 31bf3856ad364e35 ~ amd64 ~ en-US ~ 10.0.10586.0" répertorié deux fois, une fois pour Type de version : Module linguistique et une fois pour le Type de version : Feature Pack.  
  
## <a name="customizing-an-existing-nano-server-vhd"></a>Personnalisation d’un fichier VHD Nano Server existant  
Vous pouvez modifier les détails d’un fichier VHD existant à l’aide de l’applet de commande Edit-NanoServerImage, comme dans cet exemple:  
  
`Edit-NanoServerImage   -BasePath .\Base   -TargetPath .\BYOVHD.vhd`  
  
Cette applet de commande effectue les mêmes opérations que New-NanoServerImage, mais modifie l’image existante plutôt que de la convertir du format WIM au format VHD. Elle prend en charge les mêmes paramètres que New-NanoServerImage à l’exception de -MediaPath et -MaxSize ; le fichier VHD initial doit donc avoir été créé avec ces paramètres avant d’y apporter des modifications avec Edit-NanoServerImage.  

## <a name="additional-tasks-you-can-accomplish-with-new-nanoserverimage-and-edit-nanoserverimage"></a>Tâches supplémentaires que vous pouvez accomplir avec New-NanoServerImage et Edit-NanoServerImage  
  
### <a name="joining-domains"></a>Jonction de domaines  
New-NanoServerImage propose deux méthodes de jonction à un domaine ; les deux s’appuient sur l’approvisionnement du domaine hors connexion, mais l’une d’entre elles collecte un objet blob pour effectuer la jonction. Dans cet exemple, l’applet de commande collecte un objet blob de domaine pour le domaine Contoso à partir de l’ordinateur local (qui, bien entendu, doit faire partie du domaine Contoso), puis effectue un approvisionnement hors connexion de l’image à l’aide de l’objet blob:  
  
`New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\JoinDomHarvest.vhdx -ComputerName JoinDomHarvest -DomainName Contoso`  
  
Lorsque cette applet de commande se termine, vous devez rechercher un ordinateur nommé «JoinDomHarvest» dans la liste des ordinateurs Active Directory.  
  
Vous pouvez également utiliser cette applet de commande sur un ordinateur qui n’est pas joint à un domaine. Pour ce faire, collectez un objet blob à partir de n’importe quel ordinateur joint au domaine, puis fournissez vous-même l’objet blob à l’applet de commande. Notez que lorsque vous collectez un objet blob à partir d’un autre ordinateur, l’objet blob contient déjà le nom de l’ordinateur. Ainsi, si vous essayez d’ajouter le paramètre *-ComputerName*, une erreur se produit.  
  
Vous pouvez collecter l’objet blob avec cette commande:  
  
**djoin**  
 **/Provision**  
 **Ou du domaine Contoso**  
 **/ Machine JoiningDomainsNoHarvest**  
 **/SaveFile JoiningDomainsNoHarvest.djoin**  
  
Exécutez New-NanoServerImage à l’aide de l’objet blob collecté :  
  
`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\JoinDomNoHrvest.vhd -DomainBlobPath .\Path\To\Domain\Blob\JoinDomNoHrvestContoso.djoin`  
  
Si vous disposez déjà d’un nœud dans le domaine avec le même nom d’ordinateur que votre futur Nano Server, vous pouvez réutiliser le nom d’ordinateur en ajoutant le paramètre `-ReuseDomainNode`.  

### <a name="adding-additional-drivers"></a>Ajout de pilotes supplémentaires
Nano Server propose un package qui comprend un jeu de pilotes de base pour plusieurs cartes réseau et contrôleurs de stockage; il est possible que les pilotes de vos cartes réseau ne soient pas inclus. Vous pouvez utiliser ces étapes pour chercher des pilotes sur un système en fonctionnement, les extraire, puis les ajouter à l’image Nano Server.

1.  Installez Windows Server 2016 sur l’ordinateur physique sur lequel vous exécuterez Nano Server.
2.  Ouvrez le Gestionnaire de périphériques et identifiez les périphériques dans les catégories suivantes:
* Cartes réseau
* Contrôleurs de stockage
* Lecteurs de disque
3.  Pour chaque périphérique de ces catégories, cliquez avec le bouton droit sur le nom du périphérique, puis cliquez sur **Propriétés**. Dans la boîte de dialogue qui s’affiche, cliquez sur l’onglet **Pilote**, puis sur **Détails du pilote**.
4.  Notez le nom de fichier et le chemin d’accès du fichier de pilote qui s’affiche. Par exemple, supposons que le fichier de pilote soit e1i63x64.sys, qui se trouve dans C:\Windows\System32\Drivers.
5.  Dans une invite de commandes, recherchez le fichier de pilote et toutes les instances avec dir e1i*.sys /s /b. Dans cet exemple, le fichier de pilote est également présent dans le chemin d’accès C:\Windows\System32\DriverStore\FileRepository\net1ic64.inf_amd64_fafa7441408bbecd\e1i63x64.sys.
6.  Dans une invite de commandes avec élévation de privilèges, accédez au répertoire dans lequel le fichier VHD Nano Server se trouve et exécutez les commandes suivantes : **md mountdir**
     
     **dism\dism /Mount-Image /ImageFile:.\NanoServer.vhd /Index:1 /MountDir:.\mountdir**
     
     **image de /image:.\mountdir dism\dism Add : C:\Windows\System32\DriverStore\FileRepository\net1ic64.inf_amd64_fafa7441408bbecd**
     
     **dism\dism /Unmount-Image /MountDir:.\MountDir /Commit**
7.  Répétez ces étapes pour chaque fichier de pilote dont vous avez besoin.

> [!NOTE]  
> Dans le dossier dans lequel se trouvent vos pilotes, les fichiers SYS et les fichiers INF correspondants doivent être présents. En outre, Nano Server prend uniquement en charge les pilotes signés, 64\- bits. 
  
### <a name="injecting-drivers"></a>Injection de pilotes  
Nano Server propose un package qui comprend un jeu de pilotes de base pour plusieurs cartes réseau et contrôleurs de stockage; il est possible que les pilotes de vos cartes réseau ne soient pas inclus. Vous pouvez utiliser cette syntaxe pour que New-NanoServerImage recherche les pilotes disponibles dans le répertoire et les injecte dans l’image de Nano Server :  
  
`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\InjectingDrivers.vhdx -DriverPath .\Extra\Drivers`  
  
> [!NOTE]
> Dans le dossier dans lequel se trouvent vos pilotes, les fichiers SYS et les fichiers INF correspondants doivent être présents. En outre, Nano Server prend uniquement en charge les pilotes signés 64 bits.

Avec le paramètre -DriverPath, vous pouvez également passer un tableau de chemins aux fichiers .inf des pilotes :

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\InjectingDrivers.vhdx -DriverPath .\Extra\Drivers\netcard64.inf`

### <a name="connecting-with-winrm"></a>Connexion avec WinRM  
Pour pouvoir se connecter à un ordinateur Nano Server à l’aide de Windows Remote Management (WinRM) (à partir d’un autre ordinateur qui n’est pas sur le même sous-réseau), ouvrez le port 5985 pour le trafic TCP entrant sur l’image Nano Server. Utilisez cette applet de commande:  
  
`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\ConnectingOverWinRM.vhd -EnableRemoteManagementPort`  
  
### <a name="setting-static-ip-addresses"></a>Définition des adresses IP statiques  
Pour configurer une image Nano Server afin d’utiliser des adresses IP statiques, commencez par rechercher le nom ou l’index de l’interface que vous souhaitez modifier à l’aide de Get-NetAdapter, netsh ou de la Console de récupération Nano Server. Utilisez les paramètres -Ipv6Address, -Ipv6Dns, -Ipv4Address, -Ipv4SubnetMask, -Ipv4Gateway et -Ipv4Dns pour spécifier la configuration, comme dans cet exemple :  
  
`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\StaticIpv4.vhd -InterfaceNameOrIndex Ethernet -Ipv4Address 192.168.1.2 -Ipv4SubnetMask 255.255.255.0 -Ipv4Gateway 192.168.1.1 -Ipv4Dns 192.168.1.1`  
  
### <a name="custom-image-size"></a>Taille d’image personnalisée  
Vous pouvez configurer l’image Nano Server en tant que fichier VHD ou VHDX de taille dynamique avec le paramètre -MaxSize, comme dans cet exemple :  
  
`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\BigBoss.vhd -MaxSize 100GB`  
  
### <a name="embedding-custom-data"></a>Intégration de données personnalisées  
Pour intégrer vos propres scripts ou fichiers binaires dans l’image Nano Server, utilisez le paramètre -CopyPath pour passer un tableau de fichiers et de répertoires à copier. Avec le paramètre -CopyPath, vous pouvez également utiliser une table de hachage pour spécifier le chemin de destination des fichiers et des répertoires.

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\BigBoss.vhd -CopyPath .\tools`

### <a name="running-custom-commands-after-the-first-boot"></a>Exécution de commandes personnalisées après le premier démarrage
Pour exécuter des commandes personnalisées dans le cadre de setupcomplete.cmd, utilisez le paramètre -SetupCompleteCommand pour passer un tableau de commandes :

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -SetupCompleteCommand @("echo foo", "echo bar")`


### <a name="running-custom-powershell-scripts-as-part-of-image-creation"></a>Exécution de scripts PowerShell personnalisés dans le cadre du processus de création d’image
Pour exécuter des scripts PowerShell personnalisés dans le cadre du processus de création d’image, utilisez le paramètre -OfflineScriptPath pour passer un tableau de chemins aux scripts .ps1. Si ces scripts acceptent les arguments, utilisez l’argument -OfflineScriptArgument pour passer une table de hachage d’arguments supplémentaires aux scripts.

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -OfflineScriptPath C:\MyScripts\custom.ps1 -OfflineScriptArgument @{Param1="Value1"; Param2="Value2"}`


### <a name="support-for-development-scenarios"></a>Prise en charge des scénarios de développement
Si vous souhaitez développer et tester sur Nano Server, vous pouvez utiliser le paramètre -Development. Cela définit PowerShell comme interpréteur de commandes local par défaut. Cela permet aussi l’installation de pilotes non signés, la copie des fichiers binaires du débogueur, l’ouverture d’un port pour le débogage, la signature de test et l’installation des packages AppX sans licence de développeur :

`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -Development`


### <a name="custom-unattend-file"></a>Fichier d’installation sans assistance personnalisé  
Si vous souhaitez utiliser votre propre fichier d’installation sans assistance, utilisez le paramètre -UnattendPath :  
  
`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -UnattendPath \\path\to\unattend.xml`  
  
En spécifiant un nom d’ordinateur ou un mot de passe administrateur dans ce fichier d’installation sans assistance, ils remplaceront les valeurs définies par -AdministratorPassword et -ComputerName. 

> [!NOTE]
> NANO Server ne prend pas en charge la définition des paramètres TCP/IP via des fichiers d’installation sans assistance. Vous pouvez utiliser Setupcomplete.cmd pour configurer les paramètres TCP/IP.

### <a name="collecting-log-files"></a>Collecte de fichiers journaux
Si vous souhaitez collecter les fichiers journaux pendant le processus de création d’image, utilisez le paramètre -LogPath pour spécifier un répertoire où copier tous les fichiers journaux.

`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -LogPath C:\Logs`


> [!NOTE]
> Certains paramètres définis sur New-NanoServerImage et Edit-NanoServerImage sont réservés à un usage interne et peuvent être ignorés. C’est notamment le cas des paramètres -SetupUI et -Internal.


## <a name="installing-apps-and-drivers"></a>Installation d’applications et de pilotes
[comment]: # (À partir de Xumin Sun, bogues #68620.)  

### <a name="windows-server-app-installer"></a>Programme d’installation de Windows Server App
Le programme d’installation de Windows Server App (WSA) fournit une option d’installation fiable pour Nano Server. Étant donné que Microsoft Windows Installer (MSI) n’est pas pris en charge sur Nano Server, WSA est également la seule technologie d’installation disponible pour les produits autres que Microsoft. WSA utilise la technologie de package d’application Windows conçue pour installer et maintenir des applications de manière sûre et fiable à l’aide d’un manifeste déclaratif. Il étend le programme d’installation de package d’application Windows pour prendre en charge les extensions spécifiques à Windows Server, mais ne prend pas en charge l’installation des pilotes.

La création et l’installation d’un package WSA sur Nano Server impliquent des étapes pour l’éditeur et le consommateur du package.

L’éditeur de package doit effectuer les opérations suivantes :

1. Installer [SDK Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk), qui inclut les outils nécessaires pour créer un package WSA : MakeAppx, MakeCert, Pvk2Pfx, SignTool.
2. Déclarez un manifeste : Suivez le [schéma d’extension de manifeste WSA](https://msdn.microsoft.com/library/windows/apps/mt670653.aspx) pour créer le fichier manifest, AppxManifest.xml.
3. Utilisez l’outil **MakeAppx** pour créer un package WSA.
4. Utilisez les outils **MakeCert** et **Pvk2Pfx** pour créer le certificat, puis utilisez **Signtool** pour signer le package.

Ensuite, le consommateur de package devra suivre les étapes suivantes :

1. Exécutez l’applet de commande PowerShell [*Import-Certificate*](https://technet.microsoft.com/library/hh848630) pour importer le certificat de l’éditeur de l’étape 4 ci-dessus dans Nano Server avec certStoreLocation à l’emplacement « Cert: \LocalMachine\TrustedPeople ». Par exemple : `Import-Certificate -FilePath ".\xyz.cer" -CertStoreLocation "Cert:\LocalMachine\TrustedPeople"`
2. Installez l’application sur Nano Server en exécutant l’applet de commande PowerShell [**Add-AppxPackage**](https://technet.microsoft.com/library/mt575516(v=wps.620).aspx) pour installer un package WSA sur Nano Server. Par exemple : `Add-AppxPackage wsaSample.appx`

#### <a name="additional-resources-for-creating-apps"></a>Ressources supplémentaires pour créer des applications
WSA est l’extension de serveur de la technologie de package d’application Windows (même si elle n’est pas hébergée dans le Microsoft Store). Si vous souhaitez publier des applications avec WSA, ces rubriques vous aideront à vous familiariser avec le pipeline de package d’application:

- [Comment créer un manifeste de package de base](https://msdn.microsoft.com/library/windows/desktop/br211475.aspx)
- [Application Packager (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx)
- [Comment créer un package d’application certificat de signature](https://msdn.microsoft.com/library/windows/desktop/jj835832(v=vs.85).aspx)
- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764(v=vs.85).aspx)

### <a name="installing-drivers-on-nano-server"></a>Installation de pilotes sur Nano Server
Vous pouvez installer des pilotes autres que Microsoft sur Nano Server à l’aide de packages de pilotes INF. Ceux-ci incluent des packages de pilotes Plug-and-Play (PnP) et des packages de pilotes de filtre du système de fichiers. Les pilotes de filtre réseau ne sont actuellement pas pris en charge sur Nano Server.

Les packages de pilotes PnP et de pilotes de filtre du système de fichiers doivent respecter les exigences du pilote universel et le processus d’installation, ainsi que les instructions du package de pilotes général, comme la signature. Ils sont documentés à ces emplacements:

- [Signature du pilote](https://msdn.microsoft.com/windows/hardware/drivers/install/driver-signing)
- [À l’aide d’un fichier INF universel](https://msdn.microsoft.com/windows/hardware/drivers/install/using-a-configurable-inf-file)

#### <a name="installing-driver-packages-offline"></a>Installation des packages de pilotes hors connexion

Les packages de pilotes pris en charge peuvent être installés sur Nano Server hors connexion via les applets de commande [DISM.exe](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/dism-driver-servicing-command-line-options-s14) ou [DISM PowerShell](https://technet.microsoft.com/library/dn376497.aspx).

#### <a name="installing-driver-packages-online"></a>Installation des packages de pilotes en ligne
Les packages de pilotes PnP peuvent être installés sur Nano Server en ligne à l’aide de [PnpUtil](https://msdn.microsoft.com/library/windows/hardware/ff550419(v=vs.85).aspx). L’installation du pilote en ligne pour les packages de pilotes autres que PnP n’est pas prise en charge sur Nano Server.





 
--------------------------------------------------  
  
  
## <a name="BKMK_JoinDomain"></a>Jonction de Nano Server à un domaine  
  
### <a name="to-add-nano-server-to-a-domain-online"></a>Pour ajouter Nano Server à un domaine en ligne  
  
1.  Collectez un objet blob de données à partir d’un ordinateur dans le domaine qui exécute déjà Windows Threshold Server à l’aide de cette commande:  
  
    `djoin.exe /provision /domain <domain-name> /machine <machine-name> /savefile .\odjblob`  
  
    Cela permet d’enregistrer l’objet blob de données dans un fichier appelé « odjblob ».  
  
2.  Copiez le fichier «odjblob» sur l’ordinateur Nano Server avec les commandes suivantes:  
  
    **net use z: \\ \\ \<adresse ip de Nano Server > \c$**  
  
    > [!NOTE]  
    > Si la commande net use échoue, vous devrez probablement modifier les règles de pare-feu Windows. Pour ce faire, commencez par ouvrir une invite de commandes avec élévation de privilèges, démarrez Windows PowerShell, puis connectez-vous à l’ordinateur Nano Server avec la communication à distance Windows PowerShell à l’aide des commandes suivantes:  
    >   
    > `Set-Item WSMan:\localhost\Client\TrustedHosts "<IP address of Nano Server>"`  
    >   
    > `$ip = "<ip address of Nano Server>"`  
    >   
    > `Enter-PSSession -ComputerName $ip -Credential $ip\Administrator`  
    >   
    > Lorsque vous y êtes invité, fournissez le mot de passe administrateur, puis exécutez la commande suivante pour définir la règle de pare-feu :  
    >   
    > **netsh advfirewall firewall définie le groupe de règles = activer de nouveau « Partage de fichiers et imprimantes » = yes**  
    >   
    > Quittez Windows PowerShell avec `Exit-PSSession`, puis réessayez la commande net use. Si celle-ci s’exécute correctement, continuez en copiant le contenu du fichier «odjblob» sur Nano Server.  
  
    **MD z:\Temp**  
  
    **copie odjblob z:\Temp**  
  
3.  Vérifiez le domaine auquel vous voulez joindre Nano Server et assurez-vous que le DNS est configuré. En outre, vérifiez que la résolution de noms du domaine ou qu’un contrôleur de domaine fonctionne comme prévu. Pour ce faire, ouvrez une invite de commandes avec élévation de privilèges, démarrez Windows PowerShell, puis connectez-vous à l’ordinateur Nano Server avec la communication à distance Windows PowerShell à l’aide des commandes suivantes :  
  
    `Set-Item WSMan:\localhost\Client\TrustedHosts "<IP address of Nano Server>"`  
  
    `$ip = "<ip address of Nano Server>"`  
  
    `Enter-PSSession -ComputerName $ip -Credential $ip\Administrator`  
  
    Lorsque vous y êtes invité, fournissez le mot de passe administrateur. Nslookup n’est pas disponible sur Nano Server; utilisez Resolve-DNSName pour vérifier la résolution de noms.

4. Si la résolution de noms s’effectue correctement, dans la même session Windows PowerShell, exécutez cette commande pour joindre le domaine :  
  
    `djoin /requestodj /loadfile c:\Temp\odjblob /windowspath c:\windows /localos`  
  
5.  Redémarrez l’ordinateur Nano Server et quittez la session Windows PowerShell :  
  
    `shutdown /r /t 5`  
  
    `Exit-PSSession`  
  
6.  Une fois que vous avez joint Nano Server à un domaine, ajoutez le compte d’utilisateur de domaine au groupe Administrateurs sur Nano Server.

7. Pour la sécurité, retirez Nano Server à partir de la liste des hôtes approuvés avec cette commande : `Set-Item WSMan:\localhost\client\TrustedHosts ""` 
  
**Autre méthode pour joindre un domaine en une seule étape**  
  
Commencez par collecter un objet blob de données à partir d’un autre ordinateur existant dans le domaine et exécutant Windows Threshold Server à l’aide de cette commande:  
  
`djoin.exe /provision /domain <domain-name> /machine <machine-name> /savefile .\odjblob`  
  
Ouvrez le fichier «odjblob» (éventuellement dans le Bloc-notes), copiez son contenu, puis collez le contenu dans la section \<AccountData&gt; du fichier Unattend.xml ci-dessous.  
  
Placez ce fichier Unattend.xml dans le dossier C:\NanoServer, puis utilisez les commandes suivantes pour installer le VHD et appliquer les paramètres dans la section `offlineServicing` :  
  
**dism\dism /Mount-ImagemediaFile:.\NanoServer.vhd /Index:1 /MountDir:.\mountdir**  
  
**dism\dismmedia:.\mountdir /Apply-Unattend:.\unattend.xml**  
  
Créez un dossier « Panther » (utilisé par les systèmes Windows pour stocker les fichiers pendant l’installation ; consultez [Windows 7, Windows Server 2008 R2, and Windows Vista setup log file locations](https://support.microsoft.com/en-us/kb/927521) (Emplacements du fichier journal d’installation de Windows 7, Windows Server 2008 R2 et Windows Vista) si vous cela vous intéresse), copiez le fichier Unattend.xml dedans, puis désinstallez le VHD avec les commandes suivantes :  
  
**md .\mountdir\windows\panther**  
  
**copy .\unattend.xml .\mountdir\windows\panther**  
  
**dism\dism /Unmount-Image /MountDir:.\mountdir /Commit**  
  
La première fois que vous démarrerez Nano Server à partir de ce VHD, les autres paramètres s’appliqueront.  
  
Une fois que vous avez joint Nano Server à un domaine, ajoutez le compte d’utilisateur de domaine au groupe Administrateurs sur Nano Server.  
 
## <a name="working-with-server-roles-on-nano-server"></a>Utilisation des rôles serveur sur Nano Server

###<a name="using-hyper-v-on-nano-server"></a>Utilisation d’Hyper-V sur Nano Server  
Hyper-V fonctionne de la même manière sur Nano Server que sur Windows Server en mode Server Core, à deux exceptions près:  
  
-   Vous devez effectuer toute l’administration à distance et l’ordinateur d’administration doit exécuter la même version de Windows Server que le Nano Server. Les versions antérieures des applets de commande Windows PowerShell Hyper-V ou de Gestionnaire Hyper-V ne fonctionnent pas.  
  
-   RemoteFX n’est pas disponible.  
  
Dans cette version, ces fonctionnalités d’Hyper-V ont été vérifiées:  
  
-   Activation d’Hyper-V  
  
-   Création d’ordinateurs virtuels de génération 1 et de génération 2  
  
-   Création de commutateurs virtuels  
  
-   Démarrage d’ordinateurs virtuels et exécution de systèmes d’exploitation invités Windows  
- Réplication Hyper-V  
  
  
  
Si vous souhaitez effectuer une migration dynamique d’ordinateurs virtuels, créer un ordinateur virtuel sur un partage SMB ou connecter des ressources sur un partage SMB existant à un ordinateur virtuel existant, il est essentiel que vous configuriez l’authentification correctement. Pour ce faire, deux options s’offrent à vous:  
  
**La délégation contrainte**  
  
La délégation contrainte fonctionne exactement de la même manière que dans les versions antérieures. Pour plus d’informations, consultez les articles ci-dessous :  
  
-   [Activation de la gestion à distance d’Hyper-V - configuration de la délégation contrainte pour SMB et SMB à haute disponibilité](http://blogs.msdn.com/b/taylorb/archive/2012/03/20/enabling-hyper-v-remote-management-configuring-constrained-delegation-for-smb-and-highly-available-smb.aspx)  
  
-   [Activation de la gestion à distance d’Hyper-V - configuration de la délégation pour la Migration en direct Non cluster](http://blogs.msdn.com/b/taylorb/archive/2012/03/20/enabling-hyper-v-remote-management-configuring-constrained-delegation-for-non-clustered-live-migration.aspx)  
  
**CredSSP**  
  
Tout d’abord, reportez-vous à la section «Utilisation de la communication à distance Windows PowerShell» de cette rubrique pour activer et tester CredSSP. Ensuite, sur l’ordinateur d’administration, vous pouvez utiliser le Gestionnaire Hyper-V et sélectionner l’option «Se connecter en tant qu’autre utilisateur». Le Gestionnaire Hyper-V utilisera CredSSP. Vous devez le faire même si vous utilisez votre compte actuel.  
  
Les applets de commande Windows PowerShell pour Hyper-V peuvent utiliser les paramètres CimSession ou Credential, chacun d’eux pouvant fonctionner avec CredSSP.  
  
### <a name="BKMK_Failover"></a>À l’aide du Clustering de basculement sur Nano Server  
Le clustering de basculement fonctionne de la même manière sur Nano Server que sur Windows Server en mode Server Core, mais gardez ces mises en garde à l’esprit :  
  
-   Les clusters doivent être gérés à distance avec le Gestionnaire du cluster de basculement ou Windows PowerShell.  
  
-   Tous les nœuds de cluster de Nano Server doivent être joints au même domaine, similaire aux nœuds de cluster dans Windows Server.  
  
-   Le compte de domaine doit disposer des privilèges d’administrateur sur tous les nœuds Nano Server, comme avec les nœuds de cluster dans Windows Server.  
  
-   Toutes les commandes doivent être exécutées dans une invite de commandes avec élévation de privilèges.  
  
> [!NOTE]  
> En outre, certaines fonctionnalités ne sont pas prises en charge dans cette version :  
>   
> -   Vous ne pouvez pas exécuter les applets de commande de clustering de basculement sur un Nano Server local via Windows PowerShell.  
> -   Les rôles de clustering autres que Hyper-V et de serveur de fichiers.  
  
Ces applets de commande Windows PowerShell peuvent être utiles pour la gestion des clusters de basculement:  
  
Vous pouvez créer un nouveau cluster avec `New-Cluster -Name <clustername> -Node <comma-separated cluster node list>`  
  
Une fois que vous avez créé un cluster, vous devez exécuter `Set-StorageSetting -NewDiskPolicy OfflineShared` sur tous les nœuds.  
  
Ajouter un nœud supplémentaire au cluster avec `Add-ClusterNode -Name <comma-separated cluster node list>  -Cluster <clustername>`  
  
Supprimer un nœud du cluster avec  `Remove-ClusterNode -Name <comma-separated cluster node list>  -Cluster <clustername>`  
  
Créer un serveur de fichiers de montée en puissance avec `Add-ClusterScaleoutFileServerRole -name <sofsname> -cluster <clustername>`  
  
Vous pouvez rechercher des applets de commande supplémentaires pour le clustering de basculement à l’emplacement [Microsoft.FailoverClusters.PowerShell](https://technet.microsoft.com/library/ee461009.aspx).  
  
### <a name="BKMK_DNS"></a>À l’aide du serveur DNS sur Nano Server  
Pour fournir le rôle de serveur DNS à Nano Server, ajoutez Microsoft-NanoServer-DNS-Package à l’image (consultez la section «Création d’une image Nano Server personnalisée» de cette rubrique). Une fois que Nano Server est en cours d’exécution, connectez-vous à celui-ci et exécutez cette commande à partir d’une console Windows PowerShell avec élévation de privilèges pour activer la fonctionnalité :  
  
`Enable-WindowsOptionalFeature -Online -FeatureName DNS-Server-Full-Role`  
  
### <a name="BKMK_IIS"></a>À l’aide d’IIS sur Nano Server  
Pour savoir comment utiliser le rôle Internet Information Services (IIS), consultez [IIS on Nano Server](IIS-on-Nano-Server.md) (IIS sur Nano Server). 

### <a name="using-mpio-on-nano-server"></a>Utilisation de MPIO sur Nano Server
Pour savoir comment utiliser MPIO, consultez [MPIO sur Nano Server](MPIO-on-Nano-Server.md). 

### <a name="BKMK_SSH"></a>À l’aide de SSH sur Nano Server
Pour savoir comment installer et utiliser SSH sur Nano Server avec le projet OpenSSH, consultez le [wiki Win32-OpenSS](https://github.com/PowerShell/Win32-OpenSSH/wiki).

## <a name="appendix-sample-unattendxml-file-that-joins-nano-server-to-a-domain"></a>Annexe : Exemple de fichier Unattend.xml qui joint Nano Server à un domaine  
  
> [!NOTE]  
> Veillez à supprimer l’espace de fin dans le contenu de « odjblob » après l’avoir collé dans le fichier d’installation sans assistance.  
  
```  
<?xml version='1.0' encoding='utf-8'?>  
<unattend xmlns="urn:schemas-microsoft-com:unattend" xmlns:wcm="https://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  
  <settings pass="offlineServicing">  
    <component name="Microsoft-Windows-UnattendedJoin" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">  
        <OfflineIdentification>                
           <Provisioning>    
             <AccountData> AAAAAAARUABLEABLEABAoAAAAAAAMABSUABLEABLEABAwAAAAAAAAABbMAAdYABc8ABYkABLAABbMAAEAAAAMAAA0ABY4ABZ8ABbIABa0AAcIABY4ABb8ABZUABAsAAAAAAAQAAZoABNUABOYABZYAANQABMoAAOEAAMIAAOkAANoAAMAAAXwAAJAAAAYAAA0ABY4ABZ8ABbIABa0AAcIABY4ABb8ABZUABLEAALMABLQABU0AATMABXAAAAAAAKdf/mhfXoAAUAAAQAAAAb8ABLQABbMABcMABb4ABc8ABAIAAAAAAb8ABLQABbMABcMABb4ABc8ABLQABb0ABZIAAGAAAAsAAR4ABTQABUAAAAAAACAAAQwABZMAAZcAAUgABVcAAegAARcABKkABVIAASwAAY4ABbcABW8ABQoAAT0ABN8AAO8ABekAAJMAAVkAAZUABckABXEABJUAAQ8AAJ4AAIsABZMABdoAAOsABIsABKkABQEABUEABIwABKoAAaAABXgABNwAAegAAAkAAAAABAMABLIABdIABc8ABY4AADAAAA4AAZ4ABbQABcAAAAAAACAAkKBW0ID8nJDWYAHnBAXE77j7BAEWEkl+lKB98XC2G0/9+Wd1DJQW4IYAkKBAADhAnKBWEwhiDAAAM2zzDCEAM6IAAAgAAAAAAAQAAAAAAAAAAAABwzzAAA  
</AccountData>  
           </Provisioning>    
         </OfflineIdentification>    
    </component>  
  </settings>  
  
  <settings pass="oobeSystem">  
    <component name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">  
      <UserAccounts>  
        <AdministratorPassword>  
           <Value>Tuva</Value>  
           <PlainText>true</PlainText>  
        </AdministratorPassword>  
      </UserAccounts>  
      <TimeZone>Pacific Standard Time</TimeZone>  
    </component>  
  </settings>  
  
  <settings pass="specialize">  
    <component name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">  
      <RegisteredOwner>My Team</RegisteredOwner>  
      <RegisteredOrganization>My Corporation</RegisteredOrganization>  
    </component>  
  </settings>  
</unattend>  
```  
  
