---
title: Déploiement d’une passerelle Windows Admin Center dans Azure
description: Comment déployer une passerelle Windows Admin Center dans Azure
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: af766c2e0502d9fe633cae42d999db5cbffc32c8
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296920"
---
# Déployer Windows Admin Center dans Azure

## Déployer à l’aide de script

Vous pouvez télécharger [Déployer-WACAzVM.ps1](https://aka.ms/deploy-wacazvm) lequel vous allez exécuter à partir de l' [Interpréteur de commandes Azure Cloud](https://shell.azure.com) pour configurer une passerelle Windows Admin Center dans Azure. Ce script peut créer l’environnement entière, y compris le groupe de ressources.

[Accéder aux étapes de déploiement manuel](#deploy-manually-on-an-existing-azure-virtual-machine)

### Prérequis

* Configurer votre compte dans [Azure Cloud Shell](https://shell.azure.com). S’il s’agit de votre première fois à l’aide d’interpréteur de commandes Cloud, vous serez invité vous permet d’associer ou de créer un compte de stockage Azure avec l’interpréteur de commandes de Cloud.
* Dans un interpréteur de commandes **PowerShell** Cloud, accédez à votre répertoire de base: ```PS Azure:\> cd ~```
* Pour charger les ```Deploy-WACAzVM.ps1``` de fichiers, faites glisser et déposez-le à partir de votre ordinateur local à n’importe quel emplacement dans la fenêtre de l’interpréteur de commandes de Cloud.

Si vous spécifiez votre propre certificat:

* Télécharger le certificat vers le [Coffre de clés Azure](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis). Tout d’abord, créez un coffre de clés dans le portail Azure, puis téléchargez le certificat dans le coffre de clés. Par ailleurs, vous pouvez utiliser le portail Azure pour générer un certificat pour vous.

### Paramètres de script

* **ResourceGroupName** - [String] Spécifie le nom du groupe de ressources où l’ordinateur virtuel sera créé.

* **Nom** : [chaîne] Spécifie le nom de la machine virtuelle.

* **Informations d’identification** - [PSCredential] Spécifie les informations d’identification pour l’ordinateur virtuel.

* **Chemin_msi** - [String] Spécifie le chemin d’accès local de Windows Admin Center MSI lors du déploiement de Windows Admin Center sur un ordinateur virtuel existant. Par défaut, la version à partir de http://aka.ms/WACDownload s’il est omis.

* **VaultName** - [String] Spécifie le nom de la clé coffre qui contient le certificat.

* **CertName** - [String] Spécifie le nom du certificat à utiliser pour une installation MSI.

* **GenerateSslCert** - [commutateur] True si le fichier MSI doit générer un certificat auto-signé ssl signé.

* **Numéro de port** - [int] Spécifie le numéro de port ssl pour le service Windows Admin Center. Par défaut, 443 s’il est omis.

* **OpenPorts** - [int []] Spécifie les ports ouverts pour la machine virtuelle.

* **Emplacement** : [chaîne] Spécifie l’emplacement de la machine virtuelle.

* **Taille** : [chaîne] Spécifie la taille de la machine virtuelle. Valeur par défaut est «Standard_DS1_v2» s’il est omis.

* **Image** - [String] Spécifie l’image de la machine virtuelle. Valeur par défaut est «Win2016Datacenter» s’il est omis.

* **VirtualNetworkName** - [String] Spécifie le nom du réseau virtuel pour la machine virtuelle.

* **SubnetName** - [String] Spécifie le nom du sous-réseau pour la machine virtuelle.

* **SecurityGroupName** - [String] Spécifie le nom du groupe de sécurité pour la machine virtuelle.

* **PublicIpAddressName** - [String] Spécifie le nom de l’adresse IP publique pour l’ordinateur virtuel.

* **InstallWACOnly** - [commutateur] est défini sur True si WAC doit être installée sur un ordinateur virtuel Azure existants.

Il existe 2 options différentes pour le MSI pour déployer et le certificat utilisé pour l’installation MSI. Le fichier MSI soit peut être téléchargé à partir de aka.ms/WACDownload ou, si le déploiement à un ordinateur virtuel existant, le chemin d’accès d’un fichier MSI localement sur l’ordinateur virtuel peut être fournie. Vous trouverez le certificat dans le coffre de clés soit Azure ou un certificat auto-signé sera généré par le fichier MSI.

### Exemples de scripts

Tout d’abord, définissez des variables courantes nécessaires pour les paramètres du script.

```PowerShell
$ResourceGroupName = "wac-rg1" 
$VirtualNetworkName = "wac-vnet"
$SecurityGroupName = "wac-nsg"
$SubnetName = "wac-subnet"
$VaultName = "wac-key-vault"
$CertName = "wac-cert"
$Location = "westus"
$PublicIpAddressName = "wac-public-ip"
$Size = "Standard_D4s_v3"
$Image = "Win2016Datacenter"
$Credential = Get-Credential
```

#### Exemple 1: Utiliser le script pour déployer la passerelle WAC sur un nouvel ordinateur virtuel dans un nouveau groupe réseau virtuel de ressource. Utilisez le fichier MSI à partir de aka.ms/WACDownload et un certificat auto-signé du MSI.

```PowerShell
$scriptParams = @{
    ResourceGroupName = $ResourceGroupName
    Name = "wac-vm1"
    Credential = $Credential
    VirtualNetworkName = $VirtualNetworkName
    SubnetName = $SubnetName
    GenerateSslCert = $true
}
./Deploy-WACAzVM.ps1 @scriptParams
```

#### Exemple 2: Identique #1, mais à l’aide d’un certificat à partir de coffre de clés Azure.

```PowerShell
$scriptParams = @{
    ResourceGroupName = $ResourceGroupName
    Name = "wac-vm2"
    Credential = $Credential
    VirtualNetworkName = $VirtualNetworkName
    SubnetName = $SubnetName
    VaultName = $VaultName
    CertName = $CertName
}
./Deploy-WACAzVM.ps1 @scriptParams
```

#### Exemple 3: À l’aide un MSI local sur une machine virtuelle existante pour déployer WAC.

```PowerShell
$MsiPath = "C:\Users\<username>\Downloads\WindowsAdminCenter<version>.msi"
$scriptParams = @{
    ResourceGroupName = $ResourceGroupName
    Name = "wac-vm3"
    Credential = $Credential
    MsiPath = $MsiPath
    InstallWACOnly = $true
    GenerateSslCert = $true
}
./Deploy-WACAzVM.ps1 @scriptParams
```

### Configuration requise pour la machine virtuelle exécutant la passerelle Windows Admin Center

Port 443 (HTTPS) doit être ouvert.
À l’aide des mêmes variables définies pour le script, vous pouvez utiliser le code suivant dans l’interpréteur de commandes Azure Cloud pour mettre à jour le groupe de sécurité réseau:

```powershell
$nsg = Get-AzNetworkSecurityGroup -Name $SecurityGroupName -ResourceGroupName $ResourceGroupName
$newNSG = Add-AzNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name ssl-rule -Description "Allow SSL" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 443
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $newNSG
```

### Configuration requise pour Azure VM managé

Port 5985 (WinRM sur HTTP) doit être ouverts et ont un écouteur actif.
Vous pouvez utiliser le code suivant dans l’interpréteur de commandes Azure Cloud pour mettre à jour les nœuds gérés. ```$ResourceGroupName``` et ```$Name``` utilisent les mêmes variables en tant que le script de déploiement, mais vous devez utiliser le ```$Credential``` spécifique à la machine virtuelle que vous gérez.

```powershell
Enable-AzVMPSRemoting -ResourceGroupName $ResourceGroupName -Name $Name
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any} -Credential $Credential
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {winrm create winrm/config/Listener?Address=*+Transport=HTTP} -Credential $Credential
```

## Déployer manuellement sur une machine virtuelle Azure existante

Avant d’installer Windows Admin Center sur votre ordinateur virtuel de passerelle souhaité, installer un certificat SSL à utiliser pour la communication HTTPS, ou vous pouvez choisir d’utiliser un certificat auto-signé généré par Windows Admin Center. Toutefois, vous obtiendrez un avertissement lorsque vous essayez de vous connecter à partir d’un navigateur si vous choisissez l’option de ce dernier. Vous pouvez ignorer cet avertissement dans Edge en cliquant sur **Détails > passer à la page Web** ou, dans le Chrome, en sélectionnant **> avancées continuer à [page Web]**. Nous vous recommandons de qu'uniquement utiliser des certificats auto-signés pour les environnements de test.

> [!NOTE]
> Ces instructions sont pour l’installation sur Windows Server avec expérience utilisateur, pas sur une installation minimale. 

1. [Télécharger Windows Admin Center](https://aka.ms/windowsadmincenter) sur votre ordinateur local.

2. Établir une connexion Bureau à distance à la machine virtuelle, puis copiez le fichier MSI à partir de votre ordinateur local et collez dans la machine virtuelle.

3. Double-cliquez sur le fichier MSI pour commencer l’installation, puis suivez les instructions de l’Assistant. Tenez compte des éléments suivants:

   - Par défaut, le programme d’installation utilise le port 443 (HTTPS) recommandé. Si vous souhaitez sélectionner un autre port, notez que vous devrez peut-être ouvrir ce port dans votre pare-feu également. 

   - Si vous avez déjà installé un certificat SSL sur l’ordinateur virtuel, assurez-vous que vous sélectionnez cette option et entrez l’empreinte numérique.

4. Démarrez le service Windows Admin Center (C:/Program Files/Windows Admin Center/sme.exe d’exécution)

[En savoir plus sur le déploiement de Windows Admin Center.](../deploy/install.md)

### Configurer la passerelle VM pour activer l’accès de port HTTPS: 

1. Accédez à votre machine virtuelle dans le portail Azure et sélectionnez la **mise en réseau**. 

2. Sélectionnez **Ajouter une règle port d’entrée** et **HTTPS** sous **Service**. 

> [!NOTE]
> Si vous avez choisi un port autre que la valeur par défaut 443, choisissez l’option **personnalisée** sous Service et entrez le port que vous avez choisi à l’étape 3 sous les **plages de ports**. 

### Accéder à une passerelle Windows Admin Center installée sur une machine virtuelle Azure

À ce stade, vous devez être en mesure d’accéder à Windows Admin Center à partir d’un navigateur modern (Edge ou Chrome) sur votre ordinateur local en accédant au nom DNS de votre ordinateur virtuel de passerelle. 

> [!NOTE]
> Si vous avez sélectionné un port autre que 443, vous pouvez accéder à Windows Admin Center en accédant à https://\<DNS nom de votre VM\>:\<custom port\>

Lorsque vous tentez d’accéder à Windows Admin Center, le navigateur invite d’identification pour accéder à la machine virtuelle sur lequel Windows Admin Center est installé. Ici, vous devrez entrer les informations d’identification qui se trouvent dans les utilisateurs locaux ou d’un groupe d’administrateurs locaux de la machine virtuelle. 

Afin d’ajouter d’autres ordinateurs virtuels dans la basculée, vérifiez que WinRM s’exécute sur la cible de machines virtuelles en exécutant la commande suivante dans PowerShell ou l’invite de commandes sur la cible de machine virtuelle: `winrm quickconfig`

Si vous n’avez pas encore joint au domaine la machine virtuelle Azure, l’ordinateur virtuel se comporte comme un serveur de groupe de travail, vous devez donc pour vous assurer que vous prendre en compte pour [l’utilisation de Windows Admin Center dans un groupe de travail](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).