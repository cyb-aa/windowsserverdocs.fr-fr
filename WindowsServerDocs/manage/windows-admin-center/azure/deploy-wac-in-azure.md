---
title: Déployer une passerelle Windows Admin Center dans Azure
description: Comment déployer une passerelle Windows Admin Center dans Azure
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a3fa1838096d910505faf9a2c5bd819b3a256fe2
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280418"
---
# <a name="deploy-windows-admin-center-in-azure"></a>Déployer Windows Admin Center dans Azure

## <a name="deploy-using-script"></a>Déployer à l’aide du script

Vous pouvez télécharger [déployer-WACAzVM.ps1](https://aka.ms/deploy-wacazvm) que vous allez exécuter à partir de [Azure Cloud Shell](https://shell.azure.com) pour configurer une passerelle Windows Admin Center dans Azure. Ce script peut créer l’environnement entier, y compris le groupe de ressources.

[Passer à des étapes de déploiement manuel](#deploy-manually-on-an-existing-azure-virtual-machine)

### <a name="prerequisites"></a>Prérequis

* Configurer votre compte dans [Azure Cloud Shell](https://shell.azure.com). S’il s’agit de la première fois à l’aide de Cloud Shell, vous devez vous permettent d’associer ou créer un compte de stockage Azure avec Cloud Shell.
* Dans un **PowerShell** Cloud Shell, accédez à votre répertoire de base : ```PS Azure:\> cd ~```
* Pour télécharger le ```Deploy-WACAzVM.ps1``` de fichiers, faites glisser à partir de votre ordinateur local à n’importe où sur la fenêtre Cloud Shell.

Si vous spécifiez votre propre certificat :

* Téléchargez le certificat à [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis). Tout d’abord, créez un coffre de clés dans le portail Azure, puis charger le certificat dans le coffre de clés. Ou bien, vous pouvez utiliser le portail Azure pour générer un certificat pour vous.

### <a name="script-parameters"></a>Paramètres de script

* **ResourceGroupName** -[chaîne] Spécifie le nom du groupe de ressources où la machine virtuelle doit être créée.

* **Nom** -[chaîne] Spécifie le nom de la machine virtuelle.

* **Informations d’identification** -[PSCredential] Spécifie les informations d’identification pour la machine virtuelle.

* **Chemin_msi** -[chaîne] Spécifie le chemin d’accès local de Windows Admin Center MSI lors du déploiement de Windows Admin Center sur une machine virtuelle existante. Valeur par défaut est la version à partir de http://aka.ms/WACDownload si omise.

* **VaultName** -[chaîne] Spécifie le nom du coffre de clés qui contient le certificat.

* **CertName** -[chaîne] Spécifie le nom du certificat à utiliser pour l’installation de MSI.

* **GenerateSslCert** -[commutateur] True si le fichier MSI doit générer un certificat ssl signé personnel.

* **Numéro de port** -[int] Spécifie le numéro de port ssl pour le service Windows Admin Center. La valeur par défaut est 443 si omise.

* **OpenPorts** -[int []] Spécifie les ports ouverts pour la machine virtuelle.

* **Emplacement** -[chaîne] Spécifie l’emplacement de la machine virtuelle.

* **Taille** -[chaîne] Spécifie la taille de la machine virtuelle. La valeur par défaut est « Standard_DS1_v2 » dans le cas d’omission.

* **Image** -[chaîne] Spécifie l’image de la machine virtuelle. La valeur par défaut est « Win2016Datacenter » dans le cas d’omission.

* **VirtualNetworkName** -[chaîne] Spécifie le nom du réseau virtuel pour la machine virtuelle.

* **SubnetName** -[chaîne] Spécifie le nom du sous-réseau pour la machine virtuelle.

* **SecurityGroupName** -[chaîne] Spécifie le nom du groupe de sécurité pour la machine virtuelle.

* **PublicIpAddressName** -[chaîne] Spécifie le nom de l’adresse IP publique pour la machine virtuelle.

* **InstallWACOnly** -[commutateur] défini sur True si WAC doit être installé sur une machine virtuelle Azure déjà existant.

Il existe 2 options différentes pour le fichier MSI à déployer et le certificat utilisé pour l’installation de MSI. Le fichier MSI peut être téléchargé à partir de aka.ms/WACDownload ou, si le déploiement sur une machine virtuelle existante, le chemin d’accès d’un fichier MSI localement sur la machine virtuelle peut être donné. Le certificat peut être trouvé dans un Azure Key Vault ou un certificat auto-signé sera généré par le fichier MSI.

### <a name="script-examples"></a>Exemples de script

Tout d’abord, définir des variables communes pour les paramètres du script.

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

#### <a name="example-1-use-the-script-to-deploy-wac-gateway-on-a-new-vm-in-a-new-virtual-network-and-resource-group-use-the-msi-from-akamswacdownload-and-a-self-signed-cert-from-the-msi"></a>Exemple 1 : Utilisez le script pour déployer la passerelle WAC sur une machine virtuelle dans un groupe de ressources et de réseau virtuel nouveau. Utilisez le fichier MSI à partir de aka.ms/WACDownload et un certificat auto-signé à partir de la MSI.

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

#### <a name="example-2-same-as-1-but-using-a-certificate-from-azure-key-vault"></a>Exemple 2 : Identique à #1, mais à l’aide d’un certificat à partir d’Azure Key Vault.

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

#### <a name="example-3-using-a-local-msi-on-an-existing-vm-to-deploy-wac"></a>Exemple 3 : Utilisation d’une MSI locale sur une machine virtuelle existante pour déployer WAC.

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

### <a name="requirements-for-vm-running-the-windows-admin-center-gateway"></a>Configuration requise pour la machine virtuelle exécutant la passerelle Windows Admin Center

Le port 443 (HTTPS) doit être ouvert.
Utilisez les mêmes variables définies pour le script, vous pouvez utiliser le code ci-dessous dans Azure Cloud Shell pour mettre à jour le groupe de sécurité réseau :

```powershell
$nsg = Get-AzNetworkSecurityGroup -Name $SecurityGroupName -ResourceGroupName $ResourceGroupName
$newNSG = Add-AzNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name ssl-rule -Description "Allow SSL" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 443
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $newNSG
```

### <a name="requirements-for-managed-azure-vms"></a>Configuration requise pour la du machine virtuelle Azure gérée

Port 5985 (WinRM sur HTTP) doit être ouvert et avez un écouteur actif.
Vous pouvez utiliser le code ci-dessous dans Azure Cloud Shell pour mettre à jour les nœuds gérés. ```$ResourceGroupName``` et ```$Name``` utiliser les mêmes variables en tant que le script de déploiement, mais vous devez utiliser le ```$Credential``` spécifique à la machine virtuelle que vous gérez.

```powershell
Enable-AzVMPSRemoting -ResourceGroupName $ResourceGroupName -Name $Name
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any} -Credential $Credential
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {winrm create winrm/config/Listener?Address=*+Transport=HTTP} -Credential $Credential
```

## <a name="deploy-manually-on-an-existing-azure-virtual-machine"></a>Déployer manuellement sur une machine virtuelle Azure existante

Avant d’installer Windows Admin Center sur votre machine virtuelle de la passerelle de votre choix, installez un certificat SSL à utiliser pour la communication HTTPS, ou vous pouvez choisir d’utiliser un certificat auto-signé généré par Windows Admin Center. Toutefois, vous obtiendrez un avertissement lorsque vous tentez de vous connecter à partir d’un navigateur si vous choisissez cette dernière option. Vous pouvez ignorer cet avertissement dans Edge en cliquant sur **détails > Atteindre la page Web** ou, dans Chrome, en sélectionnant **Avancé > passez à la page [Web]** . Nous vous recommandons de qu'uniquement utiliser des certificats auto-signés pour les environnements de test.

> [!NOTE]
> Ces instructions concernent l’installation sur Windows Server avec expérience utilisateur et non sur une installation Server Core. 

1. [Téléchargement Windows Admin Center](https://aka.ms/windowsadmincenter) sur votre ordinateur local.

2. Établir une connexion Bureau à distance à la machine virtuelle, puis copiez le fichier MSI à partir de votre ordinateur local et collez dans la machine virtuelle.

3. Double-cliquez sur le fichier MSI pour lancer l’installation et suivez les instructions de l’Assistant. Tenez compte des éléments suivants :

   - Par défaut, le programme d’installation utilise le port recommandé 443 (HTTPS). Si vous souhaitez sélectionner un port différent, notez que vous devez ouvrir ce port dans votre pare-feu ainsi. 

   - Si vous avez déjà installé un certificat SSL sur la machine virtuelle, veillez à sélectionner cette option et entrez l’empreinte numérique.

4. Démarrer le service Windows Admin Center (C:/Program Files/Windows Admin Center/sme.exe d’exécution)

[En savoir plus sur le déploiement de Windows Admin Center.](../deploy/install.md)

### <a name="configure-the-gateway-vm-to-enable-https-port-access"></a>Configurer la passerelle de machine virtuelle pour activer l’accès de port HTTPS : 

1. Accédez à votre machine virtuelle dans le portail Azure et sélectionnez **mise en réseau**. 

2. Sélectionnez **ajouter une règle de port entrant** et sélectionnez **HTTPS** sous **Service**. 

> [!NOTE]
> Si vous avez choisi un port autre que la valeur par défaut 443, choisissez **personnalisé** sous Service et entrez le port que vous avez choisi à l’étape 3 sous **plages de Port**. 

### <a name="accessing-a-windows-admin-center-gateway-installed-on-an-azure-vm"></a>L’accès à une passerelle Windows Admin Center installée sur une machine virtuelle Azure

À ce stade, vous devez pouvoir pour accéder à Windows Admin Center à partir d’un navigateur moderne (Edge ou Chrome) sur votre ordinateur local en accédant au nom DNS de votre machine virtuelle de passerelle. 

> [!NOTE]
> Si vous avez sélectionné un port autre que 443, vous pouvez accéder à Windows Admin Center en accédant à https://\<nom DNS de votre machine virtuelle\>:\<port personnalisé\>

Lorsque vous tentez d’accéder à Windows Admin Center, le navigateur vous invite d’identification pour accéder à la machine virtuelle sur lequel Windows Admin Center est installé. Ici, vous devez entrer les informations d’identification qui se trouvent dans des utilisateurs locaux ou du groupe Administrateurs Local de l’ordinateur virtuel. 

Pour ajouter d’autres machines virtuelles dans le réseau virtuel, vérifiez que WinRM est en cours d’exécution sur la cible de machines virtuelles en exécutant la commande suivante dans PowerShell ou l’invite de commandes sur la machine virtuelle cible : `winrm quickconfig`

Si vous n’avez pas joints au domaine la machine virtuelle Azure, la machine virtuelle se comporte comme un serveur dans le groupe de travail, vous devez vous assurer de tenir compte de la [à l’aide de Windows Admin Center dans un groupe de travail](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).