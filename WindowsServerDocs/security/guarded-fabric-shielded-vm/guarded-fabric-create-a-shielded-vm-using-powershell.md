---
title: Créer une machine virtuelle protégée, à l’aide de PowerShell
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 0086edb7781a604cc90b9e76d34e5a3dc2725547
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447524"
---
# <a name="create-a-shielded-vm-using-powershell"></a>Créer une machine virtuelle protégée, à l’aide de PowerShell

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

En production, vous utiliseriez généralement un gestionnaire de fabric (par exemple, VMM) pour déployer des machines virtuelles protégées. Toutefois, les étapes illustrées ci-dessous permettent de déployer et de valider l’ensemble du scénario sans gestionnaire de fabric.

En bref, vous créez un disque de modèle, d’un fichier de données de protection, d’un fichier de réponses d’installation sans assistance et d’autres artefacts de sécurité sur n’importe quel ordinateur, puis copiez ces fichiers dans un hôte service Guardian et approvisionner la machine virtuelle protégée.

## <a name="create-a-signed-template-disk"></a>Créer un disque de modèle signé

Pour créer une machine virtuelle protégée, vous devez tout d’abord un disque de modèle de machine virtuelle protégé qui est déjà chiffré avec son volume de système d’exploitation (ou partitions de démarrage et de racine sur Linux) signé.
Suivez les liens ci-dessous pour plus d’informations sur la création d’un disque de modèle.

- [Préparer un disque de modèle Windows](guarded-fabric-create-a-shielded-vm-template.md)
- [Préparer un disque de modèle de Linux](guarded-fabric-create-a-linux-shielded-vm-template.md)

Vous devez également une copie du catalogue des signatures de volume du disque pour créer le fichier de données de protection.
Pour enregistrer ce fichier, exécutez la commande suivante sur l’ordinateur où vous avez créé le disque de modèle :

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath "C:\temp\MyTemplateDisk.vhdx" -VolumeSignatureCatalogPath "C:\temp\MyTemplateDiskCatalog.vsc"
```

## <a name="download-guardian-metadata"></a>Télécharger les métadonnées de guardian

Pour chacune des structures de virtualisation dans lequel vous souhaitez exécuter votre machine virtuelle protégée, vous devrez obtenir les métadonnées pour les clusters de SGH des infrastructures.
Votre fournisseur d’hébergement doit être en mesure de fournir ces informations pour vous.

Si vous êtes dans un environnement d’entreprise et que vous pouvez communiquer avec le serveur SGH, les métadonnées sont disponible à l’adresse *http://\<HGSCLUSTERNAME\>/KeyProtection/service/metadata/2014-07/metadata.xml*

## <a name="create-shielding-data-pdk-file"></a>Créer le fichier de données de protection (PDK)

Les données de protection sont créée et détenus par les propriétaires de machine virtuelle de locataire et contient les clés secrètes nécessaires à la création de machines virtuelles protégées qui doivent être protégées à partir de l’administrateur de l’infrastructure, comme mot de passe administrateur de machine virtuelle protégée.
Les données de protection sont chiffrées telles que les serveurs de SGH et le client peuvent le déchiffrer.
Une fois créé par le propriétaire du locataire/la machine virtuelle, le fichier PDK résultant doit être copié à l’infrastructure service Guardian.
Pour plus d’informations, consultez [ce qui est des données de protection et pourquoi est-il nécessaire ?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

Vous devez en outre, un fichier de réponses d’installation sans assistance (unattend.xml pour Windows, varie pour Linux). Consultez [créer un fichier de réponses](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file) pour obtenir des conseils sur les éléments à inclure dans le fichier de réponses.

Exécuter les applets de commande suivantes sur un ordinateur avec les outils d’Administration de serveur distant pour les machines virtuelles protégées installé.
Si vous créez un PDK pour une VM Linux, vous devez le faire sur un serveur exécutant Windows Server, version 1709 ou ultérieure.

 
```powershell
# Create owner certificate, don't lose this!
# The certificate is stored at Cert:\LocalMachine\Shielded VM Local Certificates
$Owner = New-HgsGuardian –Name 'Owner' –GenerateCertificates
 
# Import the HGS guardian for each fabric you want to run your shielded VM
$Guardian = Import-HgsGuardian -Path C:\HGSGuardian.xml -Name 'TestFabric'
 
# Create the PDK file
# The "Policy" parameter describes whether the admin can see the VM's console or not
# Use "EncryptionSupported" if you are testing out shielded VMs and want to debug any issues during the specialization process
New-ShieldingDataFile -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Owner $Owner –Guardian $guardian –VolumeIDQualifier (New-VolumeIDQualifier -VolumeSignatureCatalogFilePath 'C:\temp\MyTemplateDiskCatalog.vsc' -VersionRule Equals) -WindowsUnattendFile 'C:\unattend.xml' -Policy Shielded
```
    
## <a name="provision-shielded-vm-on-a-guarded-host"></a>Approvisionner des machines virtuelles protégées sur un ordinateur hôte service Guardian
Copiez le fichier de disque de modèle (ServerOS.vhdx) et le fichier PDK (contoso.pdk) à l’hôte service Guardian pour se préparer pour le déploiement.

Sur l’hôte service Guardian, installez le module PowerShell des outils de Fabric service Guardian, qui contient l’applet de commande New-ShieldedVM pour simplifier le processus d’approvisionnement. Si votre service Guardian hôte a accès à Internet, exécutez la commande suivante :

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

Vous pouvez également télécharger le module sur un autre ordinateur qui a Internet accéder et de copier le module qui en résulte `C:\Program Files\WindowsPowerShell\Modules` sur l’hôte service Guardian.

```powershell
Save-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0 -Path C:\temp\
```

Une fois que le module est installé, vous êtes prêt à approvisionner votre machine virtuelle protégée.

```powershell
New-ShieldedVM -Name 'MyShieldedVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait
```

Si votre modèle de disque contient un système d’exploitation basé sur Linux, incluez le `-Linux` indicateur lors de l’exécution de la commande :

```powershell
New-ShieldedVM -Name 'MyLinuxVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait -Linux
```

Vérifiez le contenu aide `Get-Help New-ShieldedVM -Full` pour en savoir plus sur les autres options que vous pouvez passer à l’applet de commande.

Une fois que la machine virtuelle a terminé l’approvisionnement, il passe à la phase de spécialisation du système d’exploitation spécifiques, après quoi il sera prêt à être utilisé.
Veillez à connecter la machine virtuelle à un réseau valid pour pouvoir vous connecter à ce dernier une fois qu’il est en cours d’exécution (à l’aide de RDP, PowerShell, SSH ou votre outil de gestion préférés).

## <a name="running-shielded-vms-on-a-hyper-v-cluster"></a>Machines virtuelles protégées en cours d’exécution sur un cluster Hyper-V

Si vous essayez de déployer des machines virtuelles protégées sur les hôtes en cluster (à l’aide d’un Cluster de basculement Windows), vous pouvez configurer une machine virtuelle protégée pour être hautement disponible à l’aide de l’applet de commande suivante :

```powershell
Add-ClusterVirtualMachineRole -VMName 'MyShieldedVM' -Cluster <Hyper-V cluster name>
```

La machine virtuelle protégée peut désormais être migrées dynamiquement au sein du cluster.

## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Déployer un protégées à l’aide de VMM](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)