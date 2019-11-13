---
title: Installer Active Directory Domain Services sur une machine virtuelle Azure
description: Comment créer une nouvelle forêt Active Directory sur une machine virtuelle sur une machine virtuelle Azure.
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 04/11/2019
ms.technology: identity-adds
ms.topic: article
ms.prod: windows-server
ms.openlocfilehash: e0a10e27e044fd1df7cbdb943964440983ce494c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390650"
---
# <a name="install-a-new-active-directory-forest-using-azure-cli"></a>Installer une nouvelle forêt Active Directory à l’aide de Azure CLI

AD DS peuvent s’exécuter sur une machine virtuelle Azure de la même façon que dans de nombreuses instances locales. Cet article vous guide dans le déploiement d’une nouvelle forêt AD DS, sur deux nouveaux contrôleurs de domaine, dans un groupe à haute disponibilité Azure à l’aide du Portail Azure et de Azure CLI. De nombreux clients trouvent ce guide utile lors de la création d’un laboratoire ou de la préparation du déploiement de contrôleurs de domaine dans Azure.

## <a name="components"></a>Composants

* Un groupe de ressources pour y placer tout.
* Un [réseau virtuel Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview.md), un sous-réseau, un groupe de sécurité réseau et une règle pour autoriser l’accès RDP aux machines virtuelles.
* Un [groupe à haute disponibilité](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability#availability-sets) de machines virtuelles Azure pour placer deux contrôleurs de domaine Active Directory Domain Services (AD DS) dans.
* Deux machines virtuelles Azure pour exécuter AD DS et DNS.

### <a name="items-that-are-not-covered"></a>Éléments non couverts

* [Création d’une connexion VPN de site à site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) à partir d’un emplacement local
* [Sécurisation du trafic réseau dans Azure](https://docs.microsoft.com/azure/security/azure-security-network-security-best-practices.md)
* [Conception de la topologie de site](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/designing-the-site-topology)
* [Planification du positionnement du rôle de maître d’opérations](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/planning-operations-master-role-placement)
* [Déploiement de Azure AD Connect pour synchroniser les identités sur Azure AD](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-express)

## <a name="build-the-test-environment"></a>Créer l’environnement de test

Nous utilisons les [portail Azure](https://portal.azure.com) et [Azure CLI](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest) pour créer l’environnement.

Le Azure CLI est utilisé pour créer et gérer des ressources Azure à partir de la ligne de commande ou dans des scripts. Ce didacticiel décrit en détail l’utilisation de la Azure CLI pour déployer des machines virtuelles exécutant Windows Server 2019. Une fois le déploiement terminé, nous nous connectons aux serveurs et installons AD DS.

Si vous n’avez pas d’abonnement Azure, [créez un compte gratuit](https://azure.microsoft.com/free) avant de commencer.

### <a name="using-azure-cli"></a>Utilisation de Azure CLI

Le script suivant automatise le processus de création de deux machines virtuelles Windows Server 2019, dans le but de créer des contrôleurs de domaine pour une nouvelle forêt Active Directory dans Azure. Un administrateur peut modifier les variables ci-dessous, en fonction de leurs besoins, puis se terminer comme une seule opération. Le script crée le groupe de ressources nécessaire, le groupe de sécurité réseau avec une règle de trafic pour Bureau à distance, le réseau virtuel et le sous-réseau et le groupe de disponibilité. Les machines virtuelles sont ensuite générées avec un disque de données de 20 Go avec mise en cache désactivée pour l’installation de AD DS sur.

Le script ci-dessous peut être exécuté directement à partir du Portail Azure. Si vous choisissez d’installer et d’utiliser l’interface CLI localement, ce guide de démarrage rapide nécessite que vous exécutiez le Azure CLI version 2.0.4 ou ultérieure. Exécutez `az --version` pour trouver la version. Si vous devez installer ou mettre à niveau, consultez [installer Azure CLI 2,0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

| Nom de la variable : | Objectif |
| :---: | :--- |
| AdminUsername | Nom d’utilisateur à configurer sur chaque machine virtuelle en tant qu’administrateur local. |
| AdminPassword | Mot de passe en texte clair à configurer sur chaque machine virtuelle en tant que mot de passe de l’administrateur local. |
| ResourceGroupName | Nom à utiliser pour le groupe de ressources. Ne doit pas dupliquer un nom existant. |
| Emplacement | Nom de l’emplacement Azure sur lequel vous souhaitez effectuer le déploiement. Répertorie les régions prises en charge pour l’abonnement actuel à l’aide de `az account list-locations`. |
| VNetName | Le nom à attribuer au réseau virtuel Azure ne doit pas dupliquer un nom existant. |
| VNetAddress | Étendue IP à utiliser pour la mise en réseau Azure. Ne doit pas dupliquer une plage existante. |
| SubnetName | Nom à attribuer au sous-réseau IP. Ne doit pas dupliquer un nom existant. |
| SubnetAddress | Adresse de sous-réseau pour les contrôleurs de domaine. Doit être un sous-réseau dans le réseau virtuel. |
| Lesquelles | Nom du groupe à haute disponibilité auquel les machines virtuelles du contrôleur de domaine se connectent. |
| VMSize | Taille de machine virtuelle Azure standard disponible à l’emplacement de déploiement. |
| DataDiskSize | Taille en Go du disque de données sur lequel AD DS s’installe. |
| DomainController1 | Nom du premier contrôleur de domaine. |
| DC1IP | Adresse IP du premier contrôleur de domaine. |
| DomainController2 | Nom du second contrôleur de domaine. |
| DC2IP | Adresse IP du second contrôleur de domaine. |

```azurecli
#Update based on your organizational requirements
Location=westus2
ResourceGroupName=ADonAzureVMs
NetworkSecurityGroup=NSG-DomainControllers
VNetName=VNet-AzureVMsWestUS2
VNetAddress=10.10.0.0/16
SubnetName=Subnet-AzureDCsWestUS2
SubnetAddress=10.10.10.0/24
AvailabilitySet=DomainControllers
VMSize=Standard_DS1_v2
DataDiskSize=20
AdminUsername=azureuser
AdminPassword=ChangeMe123456
DomainController1=AZDC01
DC1IP=10.10.10.11
DomainController2=AZDC02
DC2IP=10.10.10.12

# Create a resource group.
az group create --name $ResourceGroupName \
                --location $Location

# Create a network security group
az network nsg create --name $NetworkSecurityGroup \
                      --resource-group $ResourceGroupName \
                      --location $Location

# Create a network security group rule for port 3389.
az network nsg rule create --name PermitRDP \
                           --nsg-name $NetworkSecurityGroup \
                           --priority 1000 \
                           --resource-group $ResourceGroupName \
                           --access Allow \
                           --source-address-prefixes "*" \
                           --source-port-ranges "*" \
                           --direction Inbound \
                           --destination-port-ranges 3389

# Create a virtual network.
az network vnet create --name $VNetName \
                       --resource-group $ResourceGroupName \
                       --address-prefixes $VNetAddress \
                       --location $Location \

# Create a subnet
az network vnet subnet create --address-prefix $SubnetAddress \
                              --name $SubnetName \
                              --resource-group $ResourceGroupName \
                              --vnet-name $VNetName \
                              --network-security-group $NetworkSecurityGroup

# Create an availability set.
az vm availability-set create --name $AvailabilitySet \
                              --resource-group $ResourceGroupName \
                              --location $Location

# Create two virtual machines.
az vm create \
    --resource-group $ResourceGroupName \
    --availability-set $AvailabilitySet \
    --name $DomainController1 \
    --size $VMSize \
    --image Win2019Datacenter \
    --admin-username $AdminUsername \
    --admin-password $AdminPassword \
    --data-disk-sizes-gb $DataDiskSize \
    --data-disk-caching None \
    --nsg $NetworkSecurityGroup \
    --private-ip-address $DC1IP \
    --no-wait

az vm create \
    --resource-group $ResourceGroupName \
    --availability-set $AvailabilitySet \
    --name $DomainController2 \
    --size $VMSize \
    --image Win2019Datacenter \
    --admin-username $AdminUsername \
    --admin-password $AdminPassword \
    --data-disk-sizes-gb $DataDiskSize \
    --data-disk-caching None \
    --nsg $NetworkSecurityGroup \
    --private-ip-address $DC2IP

```

## <a name="dns-and-active-directory"></a>DNS et Active Directory

Si les machines virtuelles Azure créées dans le cadre de ce processus seront une extension d’une infrastructure Active Directory locale existante, les paramètres DNS du réseau virtuel doivent être modifiés pour inclure vos serveurs DNS locaux avant le déploiement. Cette étape est importante pour permettre aux contrôleurs de domaine qui viennent d’être créés dans Azure de résoudre les ressources locales et de permettre la réplication. Vous trouverez plus d’informations sur DNS, Azure et sur la configuration des paramètres dans la section [résolution de noms qui utilise votre propre serveur DNS](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#name-resolution-that-uses-your-own-dns-server).

Une fois les nouveaux contrôleurs de domaine promus dans Azure, ils doivent être définis sur les serveurs DNS principal et secondaire pour le réseau virtuel, et tous les serveurs DNS locaux sont rétrogradés au tertiaire et au-delà. Pour plus d’informations sur la modification des serveurs DNS, consultez l’article [créer, modifier ou supprimer un réseau virtuel](https://docs.microsoft.com/azure/virtual-network/manage-virtual-network#change-dns-servers).

Pour plus d’informations sur l’extension d’un réseau local à Azure, consultez l’article [création d’une connexion VPN de site à site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal
).

## <a name="configure-the-vms-and-install-active-directory-domain-services"></a>Configurer les machines virtuelles et installer Active Directory Domain Services

Une fois le script terminé, accédez au [portail Azure](https://portal.azure.com), puis à **machines virtuelles**.

### <a name="configure-the-first-domain-controller"></a>Configurer le premier contrôleur de domaine

Connectez-vous à AZDC01 à l’aide des informations d’identification que vous avez fournies dans le script.

* Initialisez et formatez le disque de données en tant que F :
   * Ouvrez le menu Démarrer et accédez à **gestion** de l’ordinateur.
   * Accédez à **stockage** > **gestion des disques**
   * Initialiser le disque en tant que MBR
   * Créer un volume simple et affecter la lettre de lecteur F : vous pouvez fournir un nom de volume si vous le souhaitez.
* Installer Active Directory Domain Services à l’aide de Gestionnaire de serveur
* Promouvoir le contrôleur de domaine en tant que premier dans une nouvelle forêt
   * Laissez le serveur DNS (Domain Name System) et le catalogue global (GC) activés sur la page Options du contrôleur de domaine.
   * Spécifier un mot de passe du mode de restauration des services d’annuaire en fonction des besoins de votre organisation
   * Modifiez les chemins d’accès à partir de C : pour qu’ils pointent vers le lecteur F : que nous avons créé lorsque vous êtes invité à entrer leur emplacement.
   * Passez en revue les sélections effectuées dans l’Assistant et choisissez **suivant** .

> [!NOTE]
> La vérification de la configuration requise vous avertit que la carte réseau physique n’a pas d’adresses IP statiques affectées, mais vous pouvez l’ignorer en toute sécurité, car des adresses IP statiques sont affectées dans le réseau virtuel Azure.

* Choisissez **installer**

Lorsque l’Assistant a terminé le processus d’installation, la machine virtuelle redémarre.

Lorsque la machine virtuelle redémarre la connexion avec les informations d’identification utilisées, mais cette fois en tant que membre du domaine que vous avez créé.

   > [!NOTE]
   > La première ouverture de session après la promotion sur un contrôleur de domaine peut prendre plus de temps que la normale. Attrapez une tasse de thé, de café, d’eau ou d’autres boissons de choix.

Étant donné que les [réseaux virtuels Azure ne prennent pas en charge IPv6](https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq#do-vnets-support-ipv6) , vous souhaiterez configurer vos machines virtuelles pour qu’elles préfèrent IPv4 sur IPv6. Pour plus d’informations sur la façon d’effectuer cette tâche, consultez l’article de la base de connaissances sur la [configuration d’IPv6 dans Windows pour les utilisateurs expérimentés](https://support.microsoft.com/help/929852/guidance-for-configuring-ipv6-in-windows-for-advanced-users).

### <a name="configure-the-second-domain-controller"></a>Configurer le deuxième contrôleur de domaine

Connectez-vous à AZDC02 à l’aide des informations d’identification que vous avez fournies dans le script.

* Initialisez et formatez le disque de données en tant que F :
   * Ouvrez le menu Démarrer et accédez à **gestion** de l’ordinateur.
   * Accédez à **stockage** > **gestion des disques**
   * Initialiser le disque en tant que MBR
   * Créer un volume simple et affecter la lettre de lecteur F : vous pouvez fournir un nom de volume si vous le souhaitez.
* Installer Active Directory Domain Services à l’aide de Gestionnaire de serveur
* Promouvoir le contrôleur de domaine
   * Ajouter un contrôleur de domaine à un domaine existant-CONTOSO.com
   * Fournir les informations d’identification pour effectuer l’opération
   * Modifiez les chemins d’accès à partir de C : pour qu’ils pointent vers le lecteur F : que nous avons créé lorsque vous êtes invité à entrer leur emplacement.
   * Vérifier que le serveur DNS (Domain Name System) et le catalogue global (GC) sont activés sur la page Options du contrôleur de domaine
   * Spécifier un mot de passe du mode de restauration des services d’annuaire en fonction des besoins de votre organisation
   * Passez en revue les sélections effectuées dans l’Assistant et choisissez **suivant** .

> [!NOTE]
> La vérification de la configuration requise vous avertit que la carte réseau physique n’a pas d’adresses IP statiques affectées, mais vous pouvez l’ignorer en toute sécurité, car des adresses IP statiques sont affectées dans le réseau virtuel Azure.

* Choisissez **installer**

Lorsque l’Assistant a terminé le processus d’installation, la machine virtuelle redémarre.

Lorsque la machine virtuelle redémarre la connexion avec les informations d’identification utilisées, mais cette fois en tant que membre du domaine CONTOSO.com

Étant donné que les [réseaux virtuels Azure ne prennent pas en charge IPv6](https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq#do-vnets-support-ipv6) , vous souhaiterez configurer vos machines virtuelles pour qu’elles préfèrent IPv4 sur IPv6. Pour plus d’informations sur la façon d’effectuer cette tâche, consultez l’article de la base de connaissances sur la [configuration d’IPv6 dans Windows pour les utilisateurs expérimentés](https://support.microsoft.com/help/929852/guidance-for-configuring-ipv6-in-windows-for-advanced-users).

### <a name="configure-dns"></a>Configurer DNS

Une fois les nouveaux contrôleurs de domaine promus dans Azure, ils doivent être définis sur les serveurs DNS principal et secondaire pour le réseau virtuel et tous les serveurs DNS locaux sont rétrogradés au tertiaire et au-delà. Pour plus d’informations sur la modification des serveurs DNS, consultez l’article [créer, modifier ou supprimer un réseau virtuel](https://docs.microsoft.com/azure/virtual-network/manage-virtual-network#change-dns-servers).

### <a name="wrap-up"></a>Synthèse

À ce stade, l’environnement a une paire de contrôleurs de domaine et nous avons configuré le réseau virtuel Azure pour que des serveurs supplémentaires puissent être ajoutés à l’environnement. Les tâches postérieures à l’installation pour Active Directory Domain Services telles que la configuration de sites et de services, l’audit, la sauvegarde et la sécurisation du compte administrateur intégré doivent être effectuées à ce stade.

## <a name="removing-the-environment"></a>Suppression de l’environnement

Pour supprimer l’environnement lorsque vous avez terminé le test, vous pouvez supprimer le groupe de ressources créé ci-dessus. cette étape supprime tous les composants qui font partie de ce groupe de ressources.

### <a name="remove-using-the-azure-portal"></a>Supprimer à l’aide de l’Portail Azure

À partir de la Portail Azure, accédez à **groupes de ressources** et choisissez le groupe de ressources que nous avons créé dans cet exemple ADonAzureVMs, puis sélectionnez Supprimer le **groupe de ressources**. Le processus demande une confirmation avant de supprimer toutes les ressources contenues dans le groupe de ressources.

### <a name="remove-using-the-azure-cli"></a>Supprimer à l’aide de l’Azure CLI

À partir de Azure CLI exécutez la commande suivante :

```azurecli
az group delete --name ADonAzureVMs
```

## <a name="next-steps"></a>Étapes suivantes

* [Virtualisation Active Directory Domain Services en toute sécurité (AD DS)](../../Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md)
* [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express)
* [Sauvegarde et récupération](https://docs.microsoft.com/azure/virtual-machines/windows/backup-recovery)
* [Connectivité VPN de site à site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)
* [Surveillance](https://docs.microsoft.com/azure/virtual-machines/windows/monitor)
* [Sécurité et stratégie](https://docs.microsoft.com/azure/virtual-machines/windows/security-policy)
* [Maintenance et mises à jour](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates)
