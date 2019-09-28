---
title: Créer un fichier de réponses de spécialisation du système d'exploitation
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 299aa38e-28d2-4cbe-af16-5b8c533eba1f
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 4920f9a90bd0190d390a9d35b3d265023d69efac
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386503"
---
# <a name="create-os-specialization-answer-file"></a>Créer un fichier de réponses de spécialisation du système d'exploitation

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

En vue de déployer des machines virtuelles protégées, vous devrez peut-être créer un fichier de réponses de spécialisation du système d’exploitation. Sur Windows, il s’agit généralement du fichier « Unattend. Xml ». La fonction Windows PowerShell **New-ShieldingDataAnswerFile** vous aide à effectuer cette opération. Vous pouvez ensuite utiliser le fichier de réponses lorsque vous créez des machines virtuelles protégées à partir d’un modèle à l’aide de System Center Virtual Machine Manager (ou de tout autre contrôleur de structure).

Pour obtenir des instructions générales pour les fichiers d’installation sans assistance pour les machines virtuelles protégées, consultez [créer un fichier de réponses](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file).
 
## <a name="downloading-the-new-shieldingdataanswerfile-function"></a>Téléchargement de la fonction New-ShieldingDataAnswerFile

Vous pouvez obtenir la fonction **New-ShieldingDataAnswerFile** à partir de la [PowerShell Gallery](https://aka.ms/gftools). Si votre ordinateur dispose d’une connectivité Internet, vous pouvez l’installer à partir de PowerShell à l’aide de la commande suivante :

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

La sortie `unattend.xml` peut être empaquetée dans les données de protection, ainsi que des artefacts supplémentaires, afin qu’elle puisse être utilisée pour créer des machines virtuelles protégées à partir de modèles.

Les sections suivantes montrent comment vous pouvez utiliser les paramètres de fonction pour un fichier `unattend.xml` contenant diverses options :

- [Fichier de réponses Windows de base](#basic-windows-answer-file)
- [Fichier de réponses Windows avec jonction de domaine](#windows-answer-file-with-domain-join)
- [Fichier de réponses Windows avec adresses IPv4 statiques](#windows-answer-file-with-static-ipv4-addresses)
- [Fichier de réponses Windows avec paramètres régionaux personnalisés](#windows-answer-file-with-a-custom-locale)
- [Fichier de réponses Linux de base](#basic-linux-answer-file)

## <a name="basic-windows-answer-file"></a>Fichier de réponses Windows de base

Les commandes suivantes créent un fichier de réponses Windows qui définit simplement le mot de passe du compte administrateur et le nom d’hôte.
Les cartes réseau de machines virtuelles utilisent DHCP pour obtenir les adresses IP, et la machine virtuelle n’est pas jointe à un domaine Active Directory.
Lorsque vous êtes invité à entrer des informations d’identification d’administrateur, spécifiez le nom d’utilisateur et le mot de passe souhaités.
Utilisez « administrateur » pour le nom d’utilisateur si vous souhaitez configurer le compte administrateur intégré.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred
```

## <a name="windows-answer-file-with-domain-join"></a>Fichier de réponses Windows avec jonction de domaine

Les commandes suivantes créent un fichier de réponses Windows qui joint la machine virtuelle protégée à un domaine Active Directory.
Les cartes réseau de machines virtuelles utilisent DHCP pour obtenir les adresses IP.

La première invite d’informations d’identification vous demandera les informations du compte d’administrateur local.
Utilisez « administrateur » pour le nom d’utilisateur si vous souhaitez configurer le compte administrateur intégré.

La deuxième invite d’informations d’identification demande des informations d’identification qui ont le droit de joindre l’ordinateur au domaine Active Directory.

Veillez à remplacer la valeur du paramètre « -DomainName » par le nom de domaine complet de votre Active Directory domaine.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"
$domainCred = Get-Credential -Message "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -DomainName 'my.contoso.com' -DomainJoinCredentials $domainCred
```
## <a name="windows-answer-file-with-static-ipv4-addresses"></a>Fichier de réponses Windows avec adresses IPv4 statiques

Les commandes suivantes créent un fichier de réponses Windows qui utilise des adresses IP statiques fournies au moment du déploiement par Fabric Manager, par exemple System Center Virtual Machine Manager.

Virtual Machine Manager fournit trois composants à l’adresse IP statique à l’aide d’un pool d’adresses IP : Adresse IPv4, adresse IPv6, adresse de la passerelle et adresse DNS. Si vous souhaitez inclure des champs supplémentaires ou exiger une configuration réseau personnalisée, vous devez modifier manuellement le fichier de réponses produit par le script.

Les captures d’écran suivantes montrent les pools d’adresses IP que vous pouvez configurer dans Virtual Machine Manager. Ces pools sont nécessaires si vous souhaitez utiliser une adresse IP statique.

Actuellement, la fonction ne prend en charge qu’un seul serveur DNS. Voici à quoi ressemblerait vos paramètres DNS :

![Configuration du serveur DNS avec un pool d’adresses IP statiques](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-dns-settings.png)

Voici à quoi ressemble votre résumé pour créer le pool d’adresses IP statiques. En résumé, vous devez disposer d’un seul itinéraire réseau, d’une passerelle et d’un serveur DNS, et vous devez spécifier votre adresse IP.

![Résumé de la création du pool d’adresses IP statiques](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-summary.png)

Vous devez configurer votre carte réseau pour votre machine virtuelle. La capture d’écran suivante montre où définir cette configuration et comment la faire passer à une adresse IP statique.

![Configurer le matériel pour utiliser une adresse IP statique](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-network-adapter-settings.png)

Ensuite, vous pouvez utiliser le paramètre `-StaticIPPool` pour inclure les éléments IP statiques dans le fichier de réponses. Les paramètres `@IPAddr-1@`, `@NextHop-1-1@` et `@DNSAddr-1-1@` dans le fichier de réponses sont ensuite remplacés par les valeurs réelles que vous avez spécifiées dans Virtual Machine Manager au moment du déploiement.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -StaticIPPool IPv4Address
```

## <a name="windows-answer-file-with-a-custom-locale"></a>Fichier de réponses Windows avec paramètres régionaux personnalisés

Les commandes suivantes créent un fichier de réponses Windows avec des paramètres régionaux personnalisés.

Lorsque vous êtes invité à entrer des informations d’identification d’administrateur, spécifiez le nom d’utilisateur et le mot de passe souhaités.
Utilisez « administrateur » pour le nom d’utilisateur si vous souhaitez configurer le compte administrateur intégré.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"
$domainCred = Get-Credential -Message "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -Locale es-ES
```

## <a name="basic-linux-answer-file"></a>Fichier de réponses Linux de base

À compter de la version 1709 de Windows Server, vous pouvez exécuter certains systèmes d’exploitation invités Linux dans des machines virtuelles protégées.
Si vous utilisez l’agent System Center Virtual Machine Manager Linux pour spécialiser ces machines virtuelles, l’applet de commande New-ShieldingDataAnswerFile peut créer des fichiers de réponses compatibles pour celle-ci.

Dans un fichier de réponses Linux, vous incluez généralement le mot de passe racine, la clé SSH racine et éventuellement des informations de pool d’adresses IP statiques.
Remplacez le chemin d’accès à la moitié publique de votre clé SSH avant d’exécuter le script ci-dessous.

```powershell
$rootPassword = Read-Host -Prompt "Root password" -AsSecureString

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -RootPassword $rootPassword -RootSshKey '~\.ssh\id_rsa.pub'
```

## <a name="see-also"></a>Voir aussi

- [Déployer des machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Structure protégée et machines virtuelles dotées d’une protection maximale](guarded-fabric-and-shielded-vms-top-node.md)
