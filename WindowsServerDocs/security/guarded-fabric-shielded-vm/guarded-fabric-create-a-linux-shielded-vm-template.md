---
title: Créer un disque de modèle d’ordinateur virtuel protégé Linux
ms.prod: windows-server
ms.topic: article
ms.assetid: d0e1d4fb-97fc-4389-9421-c869ba532944
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 1a6325a5d8e931f1e62c83ba4013d94760e39f86
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856792"
---
# <a name="create-a-linux-shielded-vm-template-disk"></a>Créer un disque de modèle d’ordinateur virtuel protégé Linux

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), 

Cette rubrique explique comment préparer un disque de modèle pour les machines virtuelles protégées Linux qui peuvent être utilisées pour instancier une ou plusieurs machines virtuelles clientes.

## <a name="prerequisites"></a>Composants requis

Pour préparer et tester une machine virtuelle Linux protégée, vous devez disposer des ressources suivantes :

- Un serveur avec la virtualisation capababilities exécutant Windows Server, version 1709 ou ultérieure
- Un deuxième ordinateur (Windows 10 ou Windows Server 2016) permettant d’exécuter le Gestionnaire Hyper-V pour se connecter à la console de la machine virtuelle en cours d’exécution
- Image ISO pour l’un des systèmes d’exploitation de machines virtuelles Linux protégés pris en charge :
    - Ubuntu 16,04 LTS avec le noyau 4,4
    - Red Hat Enterprise Linux 7,3
    - SUSE Linux Enterprise Server 12 Service Pack 2
- Accès à Internet pour télécharger le package lsvmtools et les mises à jour du système d’exploitation

> [!IMPORTANT]
> Les versions plus récentes des systèmes d’exploitation Linux précédents peuvent inclure un bogue de pilote TPM connu, ce qui les empêchera de provisionner correctement comme machines virtuelles protégées.
> Il n’est pas recommandé de mettre à jour vos modèles ou machines virtuelles protégées vers une version plus récente tant qu’un correctif n’est pas disponible.
> La liste des systèmes d’exploitation pris en charge ci-dessus sera mise à jour lorsque les mises à jour seront rendues publiques.

## <a name="prepare-a-linux-vm"></a>Préparer une machine virtuelle Linux

Les machines virtuelles protégées sont créées à partir de disques de modèles sécurisés.
Les disques de modèle contiennent le système d’exploitation de la machine virtuelle et des métadonnées, y compris une signature numérique des partitions/Boot et/root, pour s’assurer que les composants du système d’exploitation de base ne sont pas modifiés avant le déploiement.

Pour créer un disque de modèle, vous devez d’abord créer une machine virtuelle standard (non protégée) que vous préparerez comme image de base pour les futures machines virtuelles dotées d’une protection maximale.
Le logiciel que vous installez et les modifications de configuration apportées à cette machine virtuelle s’appliquent à toutes les machines virtuelles protégées créées à partir de ce disque de modèle.
Ces étapes vous guideront tout au long de la configuration minimale requise pour préparer une machine virtuelle Linux pour Templatization.

> [!NOTE]
> Le chiffrement de disque Linux est configuré lorsque le disque est partitionné.
> Cela signifie que vous devez créer une machine virtuelle qui est pré-chiffrée à l’aide de DM-crypt pour créer un disque de modèle d’ordinateur virtuel protégé Linux.


1.  Sur le serveur de virtualisation, assurez-vous que les fonctionnalités de prise en charge d’Hyper-V et de l’ordinateur hôte Guardian Hyper-V sont installées en exécutant les commandes suivantes dans une console PowerShell avec élévation de privilèges :

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

2.  Téléchargez l’image ISO à partir d’une source digne de confiance et stockez-la sur votre serveur de virtualisation, ou sur un partage de fichiers accessible à votre serveur de virtualisation.

3.  Sur votre ordinateur de gestion exécutant Windows Server version 1709, installez le Outils d’administration de serveur distant de la machine virtuelle protégée en exécutant la commande suivante :

    ```powershell
    Install-WindowsFeature RSAT-Shielded-VM-Tools
    ```

4.  Ouvrez le **Gestionnaire Hyper-V** sur votre ordinateur de gestion et connectez-vous à votre serveur de virtualisation.
    Pour ce faire, cliquez sur « se connecter au serveur... ». dans le volet actions ou en cliquant avec le bouton droit sur Gestionnaire Hyper-V et en choisissant « se connecter au serveur... » Fournissez le nom DNS de votre serveur Hyper-V et, si nécessaire, les informations d’identification nécessaires pour s’y connecter.

5.  À l’aide du Gestionnaire Hyper-V, [configurez un commutateur externe](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines) sur votre serveur de virtualisation pour que la machine virtuelle Linux puisse accéder à Internet afin d’obtenir des mises à jour.

6.  Ensuite, créez une machine virtuelle pour installer le système d’exploitation Linux sur.
    Dans le volet Actions, cliquez sur **nouveau** > **machine virtuelle** pour afficher l’Assistant.
    Fournissez un nom convivial pour votre machine virtuelle, par exemple « Linux en préproduction », puis cliquez sur **suivant**.

7.  Sur la deuxième page de l’Assistant, sélectionnez **génération 2** pour vous assurer que la machine virtuelle est configurée avec un profil de microprogramme UEFI.

8.  Complétez le reste de l’Assistant en fonction de vos préférences.
    N’utilisez pas de disque de différenciation pour cette machine virtuelle. les disques de modèle d’ordinateur virtuel protégé ne peuvent pas utiliser des disques de différenciation.
    Enfin, connectez l’image ISO que vous avez téléchargée précédemment au lecteur de DVD virtuel pour cette machine virtuelle afin de pouvoir installer le système d’exploitation.

9.  Dans le Gestionnaire Hyper-V, sélectionnez la machine virtuelle que vous venez de créer, puis cliquez sur **se connecter...** dans le volet Actions pour l’attacher à une console virtuelle de la machine virtuelle.
    Dans la fenêtre qui s’affiche, cliquez sur **Démarrer** pour activer l’ordinateur virtuel.

10. Poursuivez le processus d’installation de votre distribution Linux sélectionnée.
    Tandis que chaque distribution Linux utilise un assistant d’installation différent, les conditions suivantes doivent être remplies pour les machines virtuelles qui deviendront des disques de modèle d’ordinateur virtuel protégé Linux :

    - Le disque doit être partitionné à l’aide de la disposition de la table partitionnement GUID (GPT)
    - La partition racine doit être chiffrée avec DM-Crypt. La phrase secrète doit être définie sur **phrase secrète** (tout en minuscules). Cette phrase secrète est aléatoire et la partition est chiffrée à nouveau lors de la configuration d’une machine virtuelle protégée.
    - La partition de démarrage doit utiliser le système de fichiers **ext2**

11. Une fois que votre système d’exploitation Linux a démarré et que vous vous êtes connecté, il est recommandé d’installer le noyau Linux-et les packages des services d’intégration Hyper-V associés.
    En outre, vous souhaiterez peut-être installer un serveur SSH ou un autre outil de gestion à distance pour accéder à la machine virtuelle une fois qu’elle est protégée.

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

12. Configurez votre système d’exploitation Linux comme vous le souhaitez.
    Tous les logiciels que vous installez, les comptes d’utilisateur que vous ajoutez et les modifications de configuration du système que vous apportez s’appliquent à toutes les futures machines virtuelles créées à partir de ce disque de modèle.
    Vous devez éviter d’enregistrer des secrets ou des packages inutiles sur le disque.

13. Si vous envisagez d’utiliser System Center Virtual Machine Manager pour déployer vos machines virtuelles, installez l’agent invité VMM pour permettre à VMM de spécialiser votre système d’exploitation pendant l’approvisionnement de la machine virtuelle.
    La spécialisation permet de configurer chaque machine virtuelle en toute sécurité avec différents utilisateurs et clés SSH, des configurations réseau et des étapes de configuration personnalisées.
    Découvrez comment [obtenir et installer l’agent invité VMM](https://docs.microsoft.com/system-center/vmm/vm-linux#install-the-vmm-guest-agent) dans la documentation VMM.

14. Ensuite, [Ajoutez le référentiel Microsoft Linux Software à votre gestionnaire de package](../../administration/linux-package-repository-for-microsoft-software.md).

15. À l’aide de votre gestionnaire de package, installez le package lsvmtools qui contient le shim de chargeur de machine virtuelle Linux protégé, les composants de configuration et l’outil de préparation de disque.

    ```bash
    # Ubuntu 16.04
    sudo apt-get install lsvmtools

    # SLES 12 SP2
    sudo zypper install lsvmtools

    # RHEL 7.3
    sudo yum install lsvmtools
    ```

14. Lorsque vous avez terminé de personnaliser le système d’exploitation Linux, recherchez le programme d’installation de lsvmprep sur votre système et exécutez-le.
    
    ```bash
    # The path below may change based on the version of lsvmprep installed
    # Run "find /opt -name lsvmprep" to locate the lsvmprep executable
    sudo /opt/lsvmtools-1.0.0-x86-64/lsvmprep
    ```

15. Arrêtez votre machine virtuelle.

16. Si vous avez effectué des points de contrôle de votre machine virtuelle (y compris des points de contrôle automatiques créés par Hyper-V avec la mise à jour de Windows 10 automne Creators), veillez à les supprimer avant de continuer.
    Les points de contrôle créent des disques de différenciation (. avhdx) qui ne sont pas pris en charge par l’Assistant disque de modèle.
    
    Pour supprimer des points de contrôle, ouvrez le **Gestionnaire Hyper-V**, sélectionnez votre machine virtuelle, cliquez avec le bouton droit sur le point de contrôle le plus élevé dans le volet points de contrôle, puis cliquez sur **Supprimer la sous-arborescence de point de contrôle**.

    ![Supprimer tous les points de contrôle de votre machine virtuelle de modèle dans le Gestionnaire Hyper-V](../media/Guarded-Fabric-Shielded-VM/delete-checkpoints-lsvm-template.png)

## <a name="protect-the-template-disk"></a>Protéger le disque de modèle

La machine virtuelle que vous avez préparée dans la section précédente est presque prête à être utilisée en tant que disque de modèle d’ordinateur virtuel protégé Linux.
La dernière étape consiste à exécuter le disque via l’Assistant disque de modèle, qui hache et signe numériquement l’état actuel des partitions racine et de démarrage.
Le hachage et la signature numérique sont vérifiés lorsqu’une machine virtuelle dotée d’une protection maximale est approvisionnée pour s’assurer qu’aucune modification non autorisée n’a été apportée aux deux partitions entre la création et le déploiement du modèle.

### <a name="obtain-a-certificate-to-sign-the-disk"></a>Obtenir un certificat pour signer le disque

Pour signer numériquement les mesures de disque, vous devez obtenir un certificat sur l’ordinateur sur lequel vous allez exécuter l’Assistant disque de modèle.
Le certificat doit remplir les conditions suivantes :

Propriété du certificat | Valeur requise
---------------------|---------------
Algorithme de clé | RSA
Taille de clé minimale | 2 048 bits
Algorithme de signature | SHA256 (recommandé)
Utilisation de la clé | Signature numérique

Les détails relatifs à ce certificat sont présentés aux locataires lorsqu’ils créent leurs fichiers de données de protection et qu’ils autorisent les disques auxquels ils font confiance.
Par conséquent, il est important d’obtenir ce certificat auprès d’une autorité de certification que vous et vos locataires approuvent de façon mutuelle.
Dans les scénarios d’entreprise où vous êtes à la fois l’hébergeur et le locataire, vous pouvez envisager d’émettre ce certificat à partir de votre autorité de certification d’entreprise.
Protégez ce certificat avec soin, car toute personne en possession de ce certificat peut créer de nouveaux disques de modèle qui sont approuvés comme votre disque authentique.

Dans un environnement de laboratoire de test, vous pouvez créer un certificat auto-signé avec la commande PowerShell suivante :

```powershell
New-SelfSignedCertificate -Subject "CN=Linux Shielded VM Template Disk Signing Certificate"
```

### <a name="process-the-disk-with-the-template-disk-wizard-cmdlet"></a>Traiter le disque avec l’applet de commande de l’Assistant disque de modèle

Copiez votre disque de modèle et votre certificat sur un ordinateur exécutant Windows Server, version 1709, puis exécutez les commandes suivantes pour initier le processus de signature.
Le VHDX que vous fournissez au paramètre `-Path` sera remplacé par le disque de modèle mis à jour. Assurez-vous de faire une copie avant d’exécuter la commande.

> [!IMPORTANT]
> Les Outils d’administration de serveur distant disponibles sur Windows Server 2016 ou Windows 10 ne peuvent pas être utilisées pour préparer un disque de modèle d’ordinateur virtuel protégé Linux.
> Utilisez uniquement l’applet de commande [Protect-TemplateDisk](https://docs.microsoft.com/powershell/module/shieldedvmtemplate/protect-templatedisk?view=win10-ps) disponible sur Windows Server, version 1709 ou le outils d’administration de serveur distant disponible sur windows server 2019 pour préparer un disque de modèle d’ordinateur virtuel protégé Linux.

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Path 'C:\temp\MyLinuxTemplate.vhdx' -TemplateName 'Ubuntu 16.04' -Version 1.0.0.0 -Certificate $certificate -ProtectedTemplateTargetDiskType PreprocessedLinux
```

Votre disque de modèle est maintenant prêt à être utilisé pour approvisionner des machines virtuelles Linux protégées.
Si vous utilisez System Center Virtual Machine Manager pour déployer votre machine virtuelle, vous pouvez maintenant copier le VHDX dans votre bibliothèque VMM.

Vous souhaiterez peut-être également extraire le catalogue de signatures de volume du VHDX.
Ce fichier est utilisé pour fournir des informations sur le certificat de signature, le nom du disque et la version aux propriétaires de machines virtuelles qui veulent utiliser votre modèle.
Ils doivent importer ce fichier dans l’Assistant fichier de données de protection pour vous autoriser, l’auteur du modèle en possession du certificat de signature, pour créer ce modèle et les disques de modèle à venir pour eux.

Pour extraire le catalogue de signatures de volume, exécutez la commande suivante dans PowerShell :

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```
