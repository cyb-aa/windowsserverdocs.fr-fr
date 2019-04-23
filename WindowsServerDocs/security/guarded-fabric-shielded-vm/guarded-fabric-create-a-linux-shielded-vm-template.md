---
title: Créer une Linux protégée disque de modèle de machine virtuelle
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: d0e1d4fb-97fc-4389-9421-c869ba532944
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: e791d18e027e5e3e1c5b9c52659641ff588a96a2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858670"
---
# <a name="create-a-linux-shielded-vm-template-disk"></a>Créer une Linux protégée disque de modèle de machine virtuelle

> S'applique à : Windows Server 2019, Windows Server (canal semi-annuel), 

Cette rubrique explique comment préparer un disque de modèle pour les machines virtuelles qui peuvent être utilisées pour instancier un ou plusieurs machines virtuelles du locataire Linux protégées.

## <a name="prerequisites"></a>Prérequis

Pour préparer et tester une Linux de machine virtuelle protégée, vous devez les ressources suivantes disponibles :

- Un serveur avec capababilities de virtualisation exécutant Windows Server, version 1709 ou ultérieure
- Un second ordinateur (Windows 10 ou Windows Server 2016) capable d’exécuter le Gestionnaire Hyper-V pour vous connecter à la console de l’exécution de la machine virtuelle
- Une image ISO pour l’une des prises en charge Linux protégées des systèmes d’exploitation de la machine virtuelle :
    - Ubuntu 16.04 LTS avec noyau 4.4
    - Red Hat Enterprise Linux 7.3
    - SUSE Linux Enterprise Server 12 Service Pack 2
- Accès à Internet pour télécharger le package de lsvmtools et les mises à jour du système d’exploitation

> [!IMPORTANT]
> Les versions plus récentes de l’exemple précédent que Linux OSes peut inclure un bogue connu du pilote TPM qui les empêchera d’approvisionnement avec succès en tant que les machines virtuelles protégées.
> Il est déconseillé de mettre à jour vos modèles ou de machines virtuelles protégées à une version plus récente jusqu'à ce qu’un correctif est disponible.
> La liste des systèmes d’exploitation pris en charge ci-dessus sera mis à jour lorsque les mises à jour sont rendues publiques.

## <a name="prepare-a-linux-vm"></a>Préparer une machine virtuelle Linux

Machines virtuelles protégées sont créés à partir de disques de modèle sécurisées.
Disques de modèle contiennent le système d’exploitation pour la machine virtuelle et les métadonnées, y compris une signature numérique des partitions /boot et/root, pour vous assurer de composants de système d’exploitation de base ne sont pas modifiés avant le déploiement.

Pour créer un disque de modèle, vous devez d’abord créer une machine virtuelle (non blindée) de standard que vous allez préparer en tant que l’image de base pour les futures machines virtuelles protégées.
Le logiciel installé et les modifications de configuration apportées à cette machine virtuelle seront applique à toutes les machines virtuelles protégées créés à partir de ce disque de modèle.
Ces étapes vous guidera à travers les conditions minimales requises pour préparer une VM Linux templatization.

> [!NOTE]
> Chiffrement de disque Linux est configuré lorsque le partitionnement du disque.
> Cela signifie que vous devez créer une nouvelle machine virtuelle déjà chiffrée à l’aide dm-crypt pour créer une Linux protégées disque de modèle de machine virtuelle.


1.  Sur le serveur de virtualisation, vérifiez que Hyper-V et les fonctions d’assistance de Hyper-V Guardian hôte sont installées en exécutant les commandes suivantes dans une console PowerShell avec élévation de privilèges :

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

2.  Télécharger l’image ISO à partir d’une source digne de confiance et le stocker sur votre serveur de virtualisation, ou sur un partage de fichiers accessible à votre serveur de virtualisation.

3.  Sur votre ordinateur de gestion exécutant Windows Server version 1709, installez protégées VM Server Outils d’Administration distant en exécutant la commande suivante :

    ```powershell
    Install-WindowsFeature RSAT-Shielded-VM-Tools
    ```

4.  Ouvrez **Gestionnaire Hyper-V** sur votre ordinateur de gestion et de se connecter à votre serveur de virtualisation.
    Vous pouvez le faire en cliquant sur « Se connecter au serveur... » dans le volet Actions ou à droite en cliquant sur Gestionnaire Hyper-V et en choisissant « se connecter au serveur... » Indiquez le nom DNS pour votre serveur Hyper-V et, si nécessaire, les informations d’identification nécessaires pour s’y connecter.

5.  À l’aide du Gestionnaire Hyper-V, [configurer un commutateur externe](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines) sur votre serveur de virtualisation pour que la VM Linux puissent accéder à Internet pour obtenir des mises à jour.

6.  Ensuite, créez une machine virtuelle pour installer le système d’exploitation Linux sur.
    Dans le volet Actions, cliquez sur **New** > **Machine virtuelle** pour ouvrir l’Assistant.
    Fournissez un nom convivial pour votre machine virtuelle, tels que « Pre-templatized Linux » et cliquez sur **suivant**.

7.  Dans la deuxième page de l’Assistant, sélectionnez **2e** pour vous assurer de la machine virtuelle est configurée avec un profil de microprogramme UEFI.

8.  Terminez l’Assistant en fonction de vos préférences.
    N’utilisez pas un disque de différenciation pour cette machine virtuelle ; disques de modèle de machine virtuelle protégées ne peut pas utiliser des disques de différenciation.
    Enfin, connectez l’image ISO que vous avez téléchargé précédemment au lecteur de DVD virtuel pour cette machine virtuelle afin que vous pouvez installer le système d’exploitation.

9.  Dans le Gestionnaire Hyper-V, sélectionnez votre machine virtuelle qui vient d’être créé et cliquez sur **Connect...**  dans le volet Actions pour attacher à une console virtuelle de la machine virtuelle.
    Dans la fenêtre qui s’affiche, cliquez sur **Démarrer** pour allumer l’ordinateur virtuel.

10. Parcourez le processus d’installation de votre distribution Linux sélectionnée.
    Bien que chaque distribution Linux utilise un Assistant de configuration différents, les conditions suivantes sont requises pour les machines virtuelles qui deviendront Linux protégées des disques de modèle de machine virtuelle :

    - Le disque doit être partitionné à l’aide de la disposition de la Table de partitionnement de GUID (GPT)
    - La partition racine doit être chiffrée avec dm-crypt. La phrase secrète doit être définie sur **phrase secrète** (tout en minuscules). Cette phrase secrète est aléatoire et la partition rechiffrée quand une machine virtuelle protégée est approvisionnée.
    - La partition de démarrage doit utiliser le **ext2** système de fichiers

11. Une fois que votre système d’exploitation Linux ait démarré complètement et vous êtes connecté, il est recommandé d’installer le noyau linux-virtuel et associé à des packages integration services Hyper-V.
    En outre, vous devez installer un serveur SSH ou autre outil de gestion à distance pour accéder à la machine virtuelle une fois qu’il est protégé.

    Sur Ubuntu, exécutez la commande suivante pour installer ces composants :

    ```bash
    sudo apt-get install linux-virtual linux-tools-virtual linux-cloud-tools-virtual linux-image-extra-virtual openssh-server
    ```

    Sur RHEL, exécutez la commande suivante à la place :

    ```bash
    sudo yum install hyperv-daemons openssh-server
    sudo service sshd start
    ```

    Et sur SLES, exécutez la commande suivante :

    ```bash
    sudo zypper install hyper-v
    sudo chkconfig hv_kvp_daemon on
    sudo systemctl enable sshd
    ```

12. Configurez votre système d’exploitation Linux selon vos besoins.
    Tout logiciel vous installez, vous ajoutez, comptes d’utilisateur et les modifications de configuration du système que vous apportez seront applique à toutes les futures machines virtuelles créées à partir de ce disque de modèle.
    Vous devez éviter l’enregistrement des secrets ou les packages inutiles sur le disque.

13. Si vous envisagez d’utiliser System Center Virtual Machine Manager pour déployer vos machines virtuelles, installer l’agent invité VMM pour permettre à VMM spécialiser votre système d’exploitation lors de la configuration de la machine virtuelle.
    La spécialisation permet à chaque machine virtuelle à configurer en toute sécurité avec différents utilisateurs et clés SSH, des configurations de réseau et les étapes d’installation personnalisée.
    Découvrez comment [obtenir et installer l’agent invité VMM](https://docs.microsoft.com/system-center/vmm/vm-linux#install-the-vmm-guest-agent) dans la documentation de VMM.

14. Ensuite, [ajouter le référentiel Microsoft Linux à votre gestionnaire de package](../../administration/linux-package-repository-for-microsoft-software.md).

15. À l’aide de votre gestionnaire de package, installer le package lsvmtools qui contient le Linux protégées shim de chargeur de démarrage de machine virtuelle, l’approvisionnement de composants et outil de préparation de disque.

    ```bash
    # Ubuntu 16.04
    sudo apt-get install lsvmtools

    # SLES 12 SP2
    sudo zypper install lsvmtools

    # RHEL 7.3
    sudo yum install lsvmtools
    ```

14. Lorsque vous avez terminé personnaliser le système d’exploitation Linux, recherchez le programme d’installation lsvmprep sur votre système et l’exécuter.
    
    ```bash
    # The path below may change based on the version of lsvmprep installed
    # Run "find /opt -name lsvmprep" to locate the lsvmprep executable
    sudo /opt/lsvmtools-1.0.0-x86-64/lsvmprep
    ```

15. Arrêter votre machine virtuelle.

16. Si vous avez effectué des points de contrôle de votre machine virtuelle (y compris les points de contrôle automatiques créés par Hyper-V avec le Windows 10 Fall Creators Update), veillez à les supprimer avant de continuer.
    Points de contrôle créent des disques de différenciation (.avhdx) qui ne sont pas pris en charge par l’Assistant modèle de disque.
    
    Pour supprimer des points de contrôle, ouvrez **Gestionnaire Hyper-V**, sélectionnez votre machine virtuelle, cliquez avec le bouton droit sur le point de contrôle au premier plan dans le volet de points de contrôle, puis cliquez sur **supprimer la sous-arborescence de point de contrôle**.

    ![Supprimer tous les points de contrôle pour votre modèle de machine virtuelle dans le Gestionnaire Hyper-V](../media/Guarded-Fabric-Shielded-VM/delete-checkpoints-lsvm-template.png)

## <a name="protect-the-template-disk"></a>Protéger le disque de modèle

La machine virtuelle que vous avez préparé dans la section précédente est presque prête à être utilisée comme une Linux protégée disque de modèle de machine virtuelle.
La dernière étape consiste à exécuter le disque via l’Assistant de disque de modèle, ce qui permettra de hachage et signer numériquement l’état actuel des partitions de démarrage et de la racine.
Le hachage et la signature numérique sont vérifiés lorsqu’une machine virtuelle protégée est configurée pour s’assurer qu’aucune modification non autorisées ont été apportées pour les deux partitions entre la création du modèle et le déploiement.

### <a name="obtain-a-certificate-to-sign-the-disk"></a>Obtenir un certificat pour signer le disque

Pour signer numériquement les mesures de disque, vous devez obtenir un certificat sur l’ordinateur où vous exécuterez l’Assistant modèle de disque.
Le certificat doit remplir les conditions suivantes :

Propriété du certificat | Valeur requise
---------------------|---------------
Algorithme de clé | RSA
Taille de clé minimale | 2 048 bits
Algorithme de signature | SHA256 (recommandé)
Utilisation de la clé | Signature numérique

Plus d’informations sur ce certificat seront affichera pour les clients lorsqu’ils créent leurs fichiers de données de protection et que vous autorisant disques qu'ils font confiance.
Par conséquent, il est important obtenir ce certificat à partir d’une autorité de certification mutuellement approuvée par vous et vos clients.
Dans les scénarios d’entreprise où vous êtes l’hébergeur et le client, vous pouvez envisager d’émettre ce certificat à partir de votre autorité de certification d’entreprise.
Protéger ce certificat avec soin, comme toute personne en possession de ce certificat peut créer des disques de modèle qui sont approuvés identique à votre disque authentique.

Dans un environnement de laboratoire de test, vous pouvez créer un certificat auto-signé avec la commande PowerShell suivante :

```powershell
New-SelfSignedCertificate -Subject "CN=Linux Shielded VM Template Disk Signing Certificate"
```

### <a name="process-the-disk-with-the-template-disk-wizard-cmdlet"></a>Le disque avec l’applet de commande Assistant disque de modèle de processus

Copier votre disque de modèle et le certificat à un ordinateur exécutant Windows Server, version 1709, puis exécutez les commandes suivantes pour lancer le processus de signature.
Le VHDX que vous fournissez à la `-Path` paramètre sera remplacé par le disque de modèle mis à jour, par conséquent, veillez à faire une copie avant d’exécuter la commande.

> [!IMPORTANT]
> Les outils d’Administration serveur à distance disponible sur Windows Server 2016 ou Windows 10 ne peut pas être utilisés pour préparer une Linux protégée disque de modèle de machine virtuelle.
> Utilisez uniquement le [Protect-TemplateDisk](https://docs.microsoft.com/powershell/module/shieldedvmtemplate/protect-templatedisk?view=win10-ps) applet de commande disponible sur Windows Server, version 1709 ou les outils d’Administration serveur à distance disponible sur Windows Server 2019 pour préparer un Linux protégées de disque de modèle de machine virtuelle.

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Path 'C:\temp\MyLinuxTemplate.vhdx' -TemplateName 'Ubuntu 16.04' -Version 1.0.0.0 -Certificate $certificate -ProtectedTemplateTargetDiskType PreprocessedLinux
```

Votre disque de modèle est maintenant prêt à être utilisé pour approvisionner des machines virtuelles Linux protégées.
Si vous utilisez System Center Virtual Machine Manager pour déployer votre machine virtuelle, vous pouvez maintenant copier le disque VHDX à votre bibliothèque VMM.

Vous souhaiterez également extraire le catalogue de signatures de volume du disque VHDX.
Ce fichier est utilisé pour fournir des informations sur la version, le nom du disque et le certificat de signature pour les propriétaires d’ordinateurs virtuels qui souhaitent utiliser votre modèle.
Dont ils ont besoin importer ce fichier dans l’Assistant de fichier de données de protection pour vous autoriser, l’auteur du modèle en possession du certificat de signature, pour créer ce et disques de modèle futures pour eux.

Pour extraire le catalogue de signatures de volume, exécutez la commande suivante dans PowerShell :

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```
