---
title: Déployer une passerelle du centre d’administration Windows dans Azure
description: Comment déployer une passerelle du centre d’administration Windows dans Azure
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 42216375d1784a5bc853994a9de7cff72920088d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357321"
---
# <a name="deploy-windows-admin-center-in-azure"></a>Déployer le centre d’administration Windows dans Azure

## <a name="deploy-using-script"></a>Déployer à l’aide d’un script

Vous pouvez télécharger [Deploy-WACAzVM. ps1](https://aka.ms/deploy-wacazvm) que vous allez exécuter à partir de [Azure Cloud Shell](https://shell.azure.com) pour configurer une passerelle du centre d’administration Windows dans Azure. Ce script peut créer l’intégralité de l’environnement, y compris le groupe de ressources.

[Passer aux étapes de déploiement manuel](#deploy-manually-on-an-existing-azure-virtual-machine)

### <a name="prerequisites"></a>Prérequis

* Configurez votre compte dans [Azure Cloud Shell](https://shell.azure.com). Si c’est la première fois que vous utilisez Cloud Shell, vous êtes invité à associer ou à créer un compte de stockage Azure avec Cloud Shell.
* Dans un Cloud Shell **PowerShell** , accédez à votre répertoire de départ :```PS Azure:\> cd ~```
* Pour télécharger le ```Deploy-WACAzVM.ps1``` fichier, faites-le glisser et déposez-le de votre ordinateur local vers n’importe quel emplacement de la fenêtre de Cloud Shell.

Si vous spécifiez votre propre certificat :

* Téléchargez le certificat sur [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis). Tout d’abord, créez un coffre de clés dans le portail Azure, puis chargez le certificat dans le coffre de clés. Vous pouvez également utiliser le portail Azure pour générer un certificat à votre place.

### <a name="script-parameters"></a>Paramètres de script

* **ResourceGroupName** -[String] spécifie le nom du groupe de ressources dans lequel la machine virtuelle sera créée.

* **Name** -[String] spécifie le nom de la machine virtuelle.

* **Credential** -[PSCredential] spécifie les informations d’identification pour la machine virtuelle.

* **MsiPath** -[String] spécifie le chemin d’accès local du MSI du centre d’administration Windows lors du déploiement du centre d’administration Windows sur une machine virtuelle existante. La valeur par défaut est la http://aka.ms/WACDownload version de si elle est omise.

* **VaultName** -[String] spécifie le nom du coffre de clés qui contient le certificat.

* **CertName** -[String] spécifie le nom du certificat à utiliser pour l’installation de MSI.

* **GenerateSslCert** -[commutateur] true si le MSI doit générer un certificat SSL auto-signé.

* **Numéro_port** -[int] spécifie le numéro de port SSL pour le service Centre d’administration Windows. La valeur par défaut est 443 en cas d’omission.

* **OpenPorts** -[int []] spécifie les ports ouverts pour la machine virtuelle.

* **Location** -[String] spécifie l’emplacement de la machine virtuelle.

* **Size** -[String] spécifie la taille de la machine virtuelle. La valeur par défaut est « Standard_DS1_v2 » en cas d’omission.

* **Image** -[String] spécifie l’image de la machine virtuelle. La valeur par défaut est « Win2016Datacenter » en cas d’omission.

* **VirtualNetworkName** -[chaîne] spécifie le nom du réseau virtuel pour la machine virtuelle.

* **SubnetName** -[String] spécifie le nom du sous-réseau pour la machine virtuelle.

* **SecurityGroupName** -[String] spécifie le nom du groupe de sécurité pour la machine virtuelle.

* **PublicIpAddressName** -[String] spécifie le nom de l’adresse IP publique de la machine virtuelle.

* **InstallWACOnly** -[commutateur] a la valeur true si WAC doit être installé sur une machine virtuelle Azure préexistante.

Il existe deux options différentes pour le MSI à déployer et le certificat utilisé pour l’installation de MSI. Le MSI peut être téléchargé à partir de aka.ms/WACDownload ou, si vous déployez sur une machine virtuelle existante, le chemin d’un fichier MSI local sur la machine virtuelle peut être donné. Le certificat se trouve dans Azure Key Vault ou un certificat auto-signé est généré par le MSI.

### <a name="script-examples"></a>Exemples de scripts

Tout d’abord, définissez les variables communes nécessaires pour les paramètres du script.

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

#### <a name="example-1-use-the-script-to-deploy-wac-gateway-on-a-new-vm-in-a-new-virtual-network-and-resource-group-use-the-msi-from-akamswacdownload-and-a-self-signed-cert-from-the-msi"></a>Exemple 1 : Utilisez le script pour déployer la passerelle WAC sur une nouvelle machine virtuelle dans un nouveau réseau virtuel et un groupe de ressources. Utilisez le MSI de aka.ms/WACDownload et un certificat auto-signé à partir du MSI.

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

#### <a name="example-2-same-as-1-but-using-a-certificate-from-azure-key-vault"></a>Exemple 2 : Identique à #1, mais en utilisant un certificat de Azure Key Vault.

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

#### <a name="example-3-using-a-local-msi-on-an-existing-vm-to-deploy-wac"></a>Exemple 3 : Utilisation d’un MSI local sur une machine virtuelle existante pour déployer WAC.

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

### <a name="requirements-for-vm-running-the-windows-admin-center-gateway"></a>Configuration requise pour la machine virtuelle exécutant la passerelle du centre d’administration Windows

Le port 443 (HTTPs) doit être ouvert.
À l’aide des mêmes variables définies pour le script, vous pouvez utiliser le code ci-dessous dans Azure Cloud Shell pour mettre à jour le groupe de sécurité réseau :

```powershell
$nsg = Get-AzNetworkSecurityGroup -Name $SecurityGroupName -ResourceGroupName $ResourceGroupName
$newNSG = Add-AzNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name ssl-rule -Description "Allow SSL" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 443
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $newNSG
```

### <a name="requirements-for-managed-azure-vms"></a>Configuration requise pour les machines virtuelles Azure gérées

Le port 5985 (WinRM sur HTTP) doit être ouvert et disposer d’un écouteur actif.
Vous pouvez utiliser le code ci-dessous dans Azure Cloud Shell pour mettre à jour les nœuds gérés. ```$ResourceGroupName```et ```$Name``` utilisent les mêmes variables que le script de déploiement, mais vous devrez utiliser le ```$Credential``` spécifique à la machine virtuelle que vous gérez.

```powershell
Enable-AzVMPSRemoting -ResourceGroupName $ResourceGroupName -Name $Name
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any} -Credential $Credential
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {winrm create winrm/config/Listener?Address=*+Transport=HTTP} -Credential $Credential
```

## <a name="deploy-manually-on-an-existing-azure-virtual-machine"></a>Déployer manuellement sur une machine virtuelle Azure existante

Avant d’installer le centre d’administration Windows sur la machine virtuelle de votre passerelle souhaitée, installez un certificat SSL à utiliser pour la communication HTTPs, ou vous pouvez choisir d’utiliser un certificat auto-signé généré par le centre d’administration Windows. Toutefois, vous recevrez un avertissement lorsque vous tenterez de vous connecter à partir d’un navigateur si vous choisissez cette dernière option. Vous pouvez ignorer cet avertissement dans Edge en cliquant sur **détails > accéder à la page Web** ou, dans Chrome, en sélectionnant **> avancé, passer à la page [page Web]** . Nous vous recommandons d’utiliser uniquement des certificats auto-signés pour les environnements de test.

> [!NOTE]
> Ces instructions concernent l’installation de sur Windows Server avec expérience utilisateur, et non sur une installation Server Core. 

1. [Téléchargez le centre d’administration Windows](https://aka.ms/windowsadmincenter) sur votre ordinateur local.

2. Établissez une connexion Bureau à distance à la machine virtuelle, puis copiez le fichier MSI à partir de votre ordinateur local et collez-le dans la machine virtuelle.

3. Double-cliquez sur le fichier MSI pour commencer l’installation, puis suivez les instructions de l’Assistant. Tenez compte des éléments suivants :

   - Par défaut, le programme d’installation utilise le port recommandé 443 (HTTPs). Si vous souhaitez sélectionner un autre port, Notez que vous devez également ouvrir ce port dans votre pare-feu. 

   - Si vous avez déjà installé un certificat SSL sur la machine virtuelle, veillez à sélectionner cette option et à entrer l’empreinte numérique.

4. Démarrer le service Centre d’administration Windows (exécutez C:/Program Files/Windows Admin Center/SME. exe)

[En savoir plus sur le déploiement du centre d’administration Windows.](../deploy/install.md)

### <a name="configure-the-gateway-vm-to-enable-https-port-access"></a>Configurez la machine virtuelle de la passerelle pour activer l’accès au port HTTPs : 

1. Accédez à votre machine virtuelle dans le Portail Azure et sélectionnez **mise en réseau**. 

2. Sélectionnez **Ajouter une règle de port de trafic entrant** et sélectionnez **https** sous **service**. 

> [!NOTE]
> Si vous avez choisi un port autre que le port par défaut 443, choisissez **personnalisé** sous service et entrez le port que vous avez choisi à l’étape 3 sous **plages de ports**. 

### <a name="accessing-a-windows-admin-center-gateway-installed-on-an-azure-vm"></a>Accès à une passerelle du centre d’administration Windows installée sur une machine virtuelle Azure

À ce stade, vous devez être en mesure d’accéder au centre d’administration Windows à partir d’un navigateur moderne (Edge ou chrome) sur votre ordinateur local en accédant au nom DNS de votre machine virtuelle de passerelle. 

> [!NOTE]
> Si vous avez sélectionné un port autre que 443, vous pouvez accéder au centre d’administration Windows en accédant au nom DNS https://\<de\<votre machine virtuelle\>: port personnalisé\>

Lorsque vous tentez d’accéder au centre d’administration Windows, le navigateur vous invite à entrer les informations d’identification pour accéder à la machine virtuelle sur laquelle le centre d’administration Windows est installé. Ici, vous devez entrer les informations d’identification qui se trouvent dans le groupe utilisateurs locaux ou administrateurs locaux de la machine virtuelle. 

Pour ajouter d’autres machines virtuelles au réseau virtuel, assurez-vous que WinRM est en cours d’exécution sur les machines virtuelles cibles en exécutant la commande suivante dans PowerShell ou l’invite de commandes sur la machine virtuelle cible :`winrm quickconfig`

Si vous n’avez pas joint le domaine à la machine virtuelle Azure, celle-ci se comporte comme un serveur dans le groupe de travail. vous devez donc vous assurer de prendre en compte [l’utilisation du centre d’administration Windows dans un groupe de travail](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).