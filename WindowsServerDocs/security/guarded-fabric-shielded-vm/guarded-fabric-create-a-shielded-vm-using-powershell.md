---
title: Créer une machine virtuelle protégée à l’aide de PowerShell
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 09/25/2019
ms.openlocfilehash: 317da0ae3c41d142db6f5a076fd3004d9970b815
ms.sourcegitcommit: de71970be7d81b95610a0977c12d456c3917c331
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71940742"
---
# <a name="create-a-shielded-vm-using-powershell"></a>Créer une machine virtuelle protégée à l’aide de PowerShell

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

En production, vous utiliseriez généralement un gestionnaire de Fabric (par exemple, VMM) pour déployer des machines virtuelles protégées. Toutefois, les étapes illustrées ci-dessous vous permettent de déployer et de valider la totalité du scénario sans structure Manager.

En résumé, vous allez créer un disque de modèle, un fichier de données de protection, un fichier de réponses d’installation sans assistance et d’autres artefacts de sécurité sur n’importe quel ordinateur, puis copier ces fichiers sur un hôte service Guardian et approvisionner la machine virtuelle protégée.

## <a name="create-a-signed-template-disk"></a>Créer un disque de modèle signé

Pour créer une nouvelle machine virtuelle protégée, vous avez d’abord besoin d’un disque de modèle d’ordinateur virtuel protégé qui est pré-chiffré avec le volume du système d’exploitation (ou les partitions de démarrage et racine sur Linux) signé.
Pour plus d’informations sur la création d’un disque de modèle, suivez les liens ci-dessous.

- [Préparer un disque de modèle Windows](guarded-fabric-create-a-shielded-vm-template.md)
- [Préparer un disque de modèle Linux](guarded-fabric-create-a-linux-shielded-vm-template.md)

Vous aurez également besoin d’une copie du catalogue de signatures de volume du disque pour créer le fichier de données de protection.
Pour enregistrer ce fichier, exécutez la commande suivante sur l’ordinateur sur lequel vous avez créé le disque de modèle :

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath "C:\temp\MyTemplateDisk.vhdx" -VolumeSignatureCatalogPath "C:\temp\MyTemplateDiskCatalog.vsc"
```

## <a name="download-guardian-metadata"></a>Télécharger les métadonnées Guardian

Pour chacune des infrastructures de virtualisation où vous souhaitez exécuter votre machine virtuelle protégée, vous devez obtenir les métadonnées du gardien pour les clusters SGH de l’étoffe.
Votre fournisseur d’hébergement doit être en mesure de fournir ces informations pour vous.

Si vous êtes dans un environnement d’entreprise et que vous pouvez communiquer avec le serveur SGH, les métadonnées Guardian sont disponibles sur *http://\<HGSCLUSTERNAME\>/KeyProtection/service/Metadata/2014-07/Metadata.xml*

## <a name="create-shielding-data-pdk-file"></a>Créer un fichier de données de protection (PDK)

Les données de protection sont créées et détenues par les propriétaires de machines virtuelles clientes et contiennent des secrets nécessaires pour créer des machines virtuelles protégées qui doivent être protégées de l’administrateur de l’infrastructure, telles que le mot de passe administrateur de la machine virtuelle protégée.
Les données de protection sont chiffrées afin que seuls les serveurs SGH et le locataire puissent les déchiffrer.
Une fois créé par le propriétaire du client ou de la machine virtuelle, le fichier PDK résultant doit être copié dans l’infrastructure protégée.
Pour plus d’informations, consultez [qu’est-ce que les données de protection et pourquoi est-il nécessaire ?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

En outre, vous aurez besoin d’un fichier de réponses d’installation sans assistance (Unattend. xml pour Windows, qui varie pour Linux). Consultez [créer un fichier de réponses](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file) pour obtenir des conseils sur les éléments à inclure dans le fichier de réponses.

Exécutez les applets de commande suivantes sur un ordinateur avec le Outils d’administration de serveur distant pour les machines virtuelles protégées installées.
Si vous créez un PDK pour une machine virtuelle Linux, vous devez le faire sur un serveur exécutant Windows Server, version 1709 ou ultérieure.

 
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
    
## <a name="provision-shielded-vm-on-a-guarded-host"></a>Approvisionner une machine virtuelle protégée sur un hôte service Guardian
Copiez le fichier de modèle de disque (Serveros. vhdx) et le fichier PDK (contoso. PDK) sur l’hôte service Guardian pour préparer le déploiement.

Sur l’hôte service Guardian, installez le module PowerShell outils d’infrastructure protégée, qui contient l’applet de commande New-ShieldedVM pour simplifier le processus de configuration. Si votre hôte service Guardian a accès à Internet, exécutez la commande suivante :

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

Vous pouvez également télécharger le module sur un autre ordinateur disposant d’un accès à Internet et copier le module obtenu vers `C:\Program Files\WindowsPowerShell\Modules` sur l’hôte service Guardian.

```powershell
Save-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0 -Path C:\temp\
```

Une fois le module installé, vous êtes prêt à configurer votre machine virtuelle protégée.

```powershell
New-ShieldedVM -Name 'MyShieldedVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait
```

Si votre fichier de réponses aux données de protection contient des valeurs de spécialisation, vous pouvez fournir les valeurs de remplacement à New-ShieldedVM. Dans cet exemple, le fichier de réponses est configuré avec des valeurs d’espace réservé pour une adresse IPv4 statique.

```powershell
$specializationValues = @{
    "@IP4Addr-1@" = "192.168.1.10"
    "@MacAddr-1@" = "Ethernet"
    "@Prefix-1-1@" = "192.168.1.0/24"
    "@NextHop-1-1@" = "192.168.1.254"
}
New-ShieldedVM -Name 'MyStaticIPVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -SpecializationValues $specializationValues -Wait

```

Si votre disque de modèle contient un système d’exploitation Linux, incluez l’indicateur `-Linux` lors de l’exécution de la commande :

```powershell
New-ShieldedVM -Name 'MyLinuxVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait -Linux
```

Consultez le contenu de l’aide à l’aide de `Get-Help New-ShieldedVM -Full` pour en savoir plus sur les autres options que vous pouvez passer à l’applet de commande.

Une fois la configuration terminée, la machine virtuelle passe à la phase de spécialisation propre au système d’exploitation, après quoi elle est prête à être utilisée.
Veillez à connecter la machine virtuelle à un réseau valide pour pouvoir vous y connecter une fois qu’elle est en cours d’exécution (à l’aide de RDP, de PowerShell, de SSH ou de votre outil de gestion préféré).

## <a name="running-shielded-vms-on-a-hyper-v-cluster"></a>Exécution de machines virtuelles protégées sur un cluster Hyper-V

Si vous essayez de déployer des machines virtuelles protégées sur des hôtes service Guardian en cluster (à l’aide d’un cluster de basculement Windows), vous pouvez configurer la haute disponibilité de la machine virtuelle protégée à l’aide de l’applet de commande suivante :

```powershell
Add-ClusterVirtualMachineRole -VMName 'MyShieldedVM' -Cluster <Hyper-V cluster name>
```

La machine virtuelle protégée peut désormais être migrée dynamiquement au sein du cluster.

## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Déployer une protection blindée à l’aide de VMM](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)