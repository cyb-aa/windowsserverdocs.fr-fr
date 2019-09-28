---
title: Démarrage rapide de Nano Server
description: Étapes pour déployer rapidement un serveur Nano Server de base sur des machines virtuelles ou physiques
ms.prod: windows-server
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/05/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 5de589d9da1c7d4fc9eb116e6ea1f6a326d1ad7c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391721"
---
# <a name="nano-server-quick-start"></a>Démarrage rapide de Nano Server

>S'applique à : Windows Server 2016

> [!IMPORTANT]
> À compter de Windows Server, version 1709, Nano Server sera uniquement disponible sous forme d’[image de système d’exploitation de base du conteneur](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Pour en savoir plus, consultez [Modifications apportées à Nano Server](nano-in-semi-annual-channel.md). 

Suivez les étapes décrites dans cette section pour effectuer rapidement un déploiement de base de Nano Server, en utilisant DHCP pour obtenir une adresse IP. Vous pouvez exécuter un disque dur virtuel Nano Server dans une machine virtuelle ou le démarrer sur un ordinateur physique. La procédure est légèrement différente.

Une fois que vous avez mis en œuvre les principes fondamentaux avec le démarrage rapide, vous pouvez consulter la rubrique [Deploy Nano Server (Déployer Nano Server)](Deploy-Nano-Server.md) qui explique plus en détail comment créer des images personnalisées, gérer les packages selon plusieurs méthodes, effectuer des opérations de domaine et bien plus encore .
  
**Nano Server dans une machine virtuelle**  
  
Suivez ces étapes pour créer un disque dur virtuel Nano Server qui s’exécute sur une machine virtuelle.  
  
## <a name="to-quickly-deploy-nano-server-in-a-virtual-machine"></a>Pour déployer rapidement Nano Server sur une machine virtuelle  
  
1. Copiez le sous-dossier *NanoServerImageGenerator* du dossier \NanoServer du fichier ISO de Windows Server 2016 dans un dossier sur votre disque dur.  
  
2. Démarrez Windows PowerShell en tant qu'administrateur, accédez au répertoire où vous avez placé le dossier NanoServerImageGenerator, puis importez le module avec `Import-Module .\NanoServerImageGenerator -Verbose`  
   >[!NOTE]  
   >Il se peut que vous deviez modifier la stratégie d’exécution de Windows PowerShell. `Set-ExecutionPolicy RemoteSigned` devrait bien fonctionner.  
  
3. Créez un disque dur virtuel pour l’édition Standard, qui définit un nom d’ordinateur et comprend les **pilotes invités** de Hyper-V en exécutant la commande suivante qui vous demandera un mot de passe d’administrateur pour le nouveau disque dur virtuel :  
  
   `New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath <path to root of media> -BasePath .\Base -TargetPath .\NanoServerVM\NanoServerVM.vhd -ComputerName <computer name>` où  
  
   -   **-MediaPath <chemin d’accès à la racine du support\>** spécifie un chemin d’accès à la racine du contenu de l’image ISO de Windows Server 2016. Par exemple si vous avez copié le contenu de l’image ISO dans d:\TP5ISO, utilisez ce chemin d’accès.  
  
   -   **-BasePath** (facultatif) spécifie le dossier à créer et dans lequel copier les packages et le fichier WIM de Nano Server.  
  
   -   **-TargetPath** spécifie un chemin d’accès, avec le nom de fichier et l’extension, où le fichier VHD ou VHDX sera créé.  
  
   -   **Computer_name** indique le nom d’ordinateur attribué à la machine virtuelle Nano Server que vous créez.  
  
   **Exemple :** `New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath f:\ -BasePath .\Base -TargetPath .\Nano1\Nano.vhd -ComputerName Nano1`  
  
   Cet exemple crée un disque dur virtuel à partir d’un fichier ISO monté sur f:\\. Lors de sa création, le disque dur virtuel utilisera un dossier appelé Base situé dans le même répertoire que celui où vous avez exécuté New-NanoServerImage. Il placera le fichier VHD (nommé Nano.vhd) dans le sous-dossier Nano1 du dossier à partir duquel la commande est exécutée. Le nom d’ordinateur sera Nano1. Le disque dur virtuel obtenu contiendra l’édition Standard de Windows Server 2016 et autorisera le déploiement de machines virtuelles Hyper-V. Si vous souhaitez une machine virtuelle de génération 1, créez une image VHD en spécifiant une extension **.vhd** dans -TargetPath. Si vous souhaitez une machine virtuelle de génération 2, créez une image VHDX en spécifiant une extension **.vhdx** dans -TargetPath. Vous pouvez générer directement un fichier WIM en spécifiant une extension **.wim** dans -TargetPath.  
  
   > [!NOTE]  
   > Windows 8.1, Windows 10, Windows Server 2012 R2 et Windows Server 2016 prennent en charge l’image New-NanoServerImage.  
  
4. Dans le Gestionnaire Hyper-V, créez une machine virtuelle et utilisez le fichier VHD créé à l’étape 3.  
  
5. Démarrez la machine virtuelle puis, dans le Gestionnaire Hyper-V, connectez-vous à la machine virtuelle.  
  
6. Ouvrez une session sur la Console de récupération (consultez la section « Console de récupération de Nano Server » dans ce guide), en utilisant le nom d’administrateur et le mot de passe fournis lors de l’exécution du script à l’étape 3.  
   > [!NOTE]  
   > La Console de récupération ne prend en charge que les fonctions de base du clavier. Les voyants du clavier, le pavé numérique et les touches de changement de la disposition du clavier, comme Verr Maj et Verr Num, ne sont pas pris en charge.
  
7. Obtenez l’adresse IP de la machine virtuelle Nano Server et utilisez l’accès distant Windows PowerShell ou un autre outil d’administration à distance pour vous connecter à la machine virtuelle et la gérer à distance.  
  
**Nano Server sur un ordinateur physique**  
  
Vous pouvez également créer un disque dur virtuel qui exécutera Nano Server sur un ordinateur physique, à l’aide des pilotes de périphérique préinstallés. Si votre matériel requiert un pilote non fourni pour démarrer ou pour vous connecter à un réseau, suivez la procédure décrite dans la section « Ajout de pilotes supplémentaires » de ce guide.  
  
## <a name="to-quickly-deploy-nano-server-on-a-physical-computer"></a>Pour déployer rapidement Nano Server sur un ordinateur physique  
  
1.  Copiez le sous-dossier *NanoServerImageGenerator* du dossier \NanoServer du fichier ISO de Windows Server 2016 dans un dossier sur votre disque dur.  
  
2.  Démarrez Windows PowerShell en tant qu'administrateur, accédez au répertoire où vous avez placé le dossier NanoServerImageGenerator, puis importez le module avec `Import-Module .\NanoServerImageGenerator -Verbose`  
  
>[!NOTE]  
>Il se peut que vous deviez modifier la stratégie d’exécution de Windows PowerShell. `Set-ExecutionPolicy RemoteSigned` devrait bien fonctionner.  
  
3. Créez un disque dur virtuel qui définit un nom d’ordinateur et comprend les pilotes OEM ainsi que Hyper-V, en exécutant la commande suivante qui vous demandera un mot de passe d’administrateur pour le nouveau disque dur virtuel :  
  
   `New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath <path to root of media> -BasePath .\Base -TargetPath .\NanoServerPhysical\NanoServer.vhd -ComputerName <computer name> -OEMDrivers -Compute -Clustering` où  
  
   -   **-MediaPath <chemin d’accès à la racine du support\>** spécifie un chemin d’accès à la racine du contenu de l’image ISO de Windows Server 2016. Par exemple si vous avez copié le contenu de l’image ISO dans d:\TP5ISO, utilisez ce chemin d’accès.  
  
   -   **BasePath** spécifie le dossier à créer et dans lequel copier les packages et le fichier WIM de Nano Server. (Ce paramètre est facultatif.)  
  
   -   **-TargetPath** spécifie un chemin d’accès, avec le nom de fichier et l’extension, où le fichier VHD ou VHDX sera créé.  
  
   -   **Computer_name** est le nom d’ordinateur de l’instance Nano Serveur que vous créez.  
  
   **Exemple :** `New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath F:\ -BasePath .\Base -TargetPath .\Nano1\NanoServer.vhd -ComputerName Nano-srv1 -OEMDrivers -Compute -Clustering`  
  
   Cet exemple crée un disque dur virtuel à partir d’une image ISO montée sur F:\\. Lors de sa création, le disque dur virtuel utilisera un dossier appelé Base situé dans le même répertoire que celui où vous avez exécuté New-NanoServerImage. Il placera le fichier VHD dans le sous-dossier Nano1 du dossier à partir duquel la commande est exécutée. Le nom de l’ordinateur sera Nano-srv1. Des pilotes OEM seront installés pour le matériel le plus courant. Le rôle Hyper-V et la fonctionnalité de clustering seront activés. L’édition Nano Standard est utilisée.  
  
4. Connectez-vous en tant qu’administrateur au serveur physique sur lequel vous souhaitez exécuter le disque dur virtuel Nano Server.  
  
5. Copiez le fichier VHD créé par ce script sur l’ordinateur physique, puis configurez-le pour qu’il démarre à partir de ce nouveau disque dur virtuel. Pour ce faire, procédez comme suit :  
  
   1.  Montez le disque dur virtuel généré. Dans cet exemple, il est monté sur D:\\.  
  
   2.  Exécutez **bcdboot d:\windows**.  
  
   3.  Démontez le disque dur virtuel.  
  
6. Démarrez l’ordinateur physique dans le disque dur virtuel Nano Server.  
  
7. Ouvrez une session sur la Console de récupération (consultez la section « Console de récupération de Nano Server » dans ce guide), en utilisant le nom d’administrateur et le mot de passe fournis lors de l’exécution du script à l’étape 3.
   > [!NOTE]  
   > La Console de récupération ne prend en charge que les fonctions de base du clavier. Les voyants du clavier, le pavé numérique et les touches de changement de la disposition du clavier, comme Verr. maj. et Verr. num., ne sont pas pris en charge. 
  
8. Obtenez l’adresse IP de l’ordinateur Nano Server et utilisez l’accès distant Windows PowerShell ou un autre outil d’administration à distance pour vous connecter à la machine virtuelle et la gérer à distance.  
