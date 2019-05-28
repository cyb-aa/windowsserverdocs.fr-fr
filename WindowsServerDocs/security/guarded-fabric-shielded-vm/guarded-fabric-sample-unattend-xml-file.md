---
title: Créer un fichier de réponses de spécialisation du système d'exploitation
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 299aa38e-28d2-4cbe-af16-5b8c533eba1f
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 5717fcc9e1732b6273620e633c140c6df58ec8b7
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624653"
---
# <a name="create-os-specialization-answer-file"></a>Créer un fichier de réponses de spécialisation du système d'exploitation

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

En vue de déployer des machines virtuelles protégées, vous devrez peut-être créer un fichier de réponses de spécialisation de système d’exploitation. Sur Windows, cela est communément appelé le fichier « unattend.xml ». Le **New-ShieldingDataAnswerFile** fonction Windows PowerShell vous permet de faire cela. Vous pouvez ensuite utiliser le fichier de réponses lors de la création des machines virtuelles protégées à partir d’un modèle à l’aide de System Center Virtual Machine Manager (ou tout autre contrôleur de tissu).

Pour obtenir des recommandations générales pour les fichiers d’installation sans assistance pour les machines virtuelles protégées, consultez [créer un fichier de réponses](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file).
 
## <a name="downloading-the-new-shieldingdataanswerfile-function"></a>Téléchargement de la fonction New-ShieldingDataAnswerFile

Vous pouvez obtenir le **New-ShieldingDataAnswerFile** fonction à partir de la [PowerShell Gallery](https://aka.ms/gftools). Si votre ordinateur est connecté à Internet, vous pouvez l’installer à partir de PowerShell avec la commande suivante :

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

Le `unattend.xml` sortie peut être empaquetée dans les données de protection, ainsi que des artefacts supplémentaires, afin qu’il peut être utilisé pour créer des machines virtuelles protégées à partir de modèles.

Les sections suivantes montrent comment vous pouvez utiliser les paramètres de fonction pour un `unattend.xml` fichier contenant les différentes options :

- [Fichier de réponses de base Windows](#basic-windows-answer-file)
- [Fichier avec la jonction de domaine de réponse de Windows](#windows-answer-file-with-domain-join)
- [Fichier de réponses Windows avec des adresses IPv4 statiques](#windows-answer-file-with-static-ipv4-addresses)
- [Fichier de réponses Windows avec des paramètres régionaux personnalisé](#windows-answer-file-with-a-custom-locale)
- [Fichier de réponses base Linux](#basic-linux-answer-file)

## <a name="basic-windows-answer-file"></a>Fichier de réponses de base Windows

Les commandes suivantes créent un fichier de réponses Windows qui se contente de définir le mot de passe de compte administrateur et le nom d’hôte.
Les cartes réseau de machine virtuelle utilise DHCP pour obtenir des adresses IP et la machine virtuelle ne sera pas joint à un domaine Active Directory.
Lorsque vous êtes invité à entrer les informations d’identification d’administrateur, spécifiez le nom d’utilisateur souhaité et le mot de passe.
Utilisez « Administrateur » pour le nom d’utilisateur si vous souhaitez configurer le compte administrateur intégré.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred
```

## <a name="windows-answer-file-with-domain-join"></a>Fichier avec la jonction de domaine de réponse de Windows

Les commandes suivantes créent un fichier de réponses Windows qui joint la machine virtuelle protégée à un domaine Active Directory.
Les cartes réseau de machine virtuelle utilise DHCP pour obtenir des adresses IP.

La première invite d’informations d’identification vous demandera les informations de compte d’administrateur local.
Utilisez « Administrateur » pour le nom d’utilisateur si vous souhaitez configurer le compte administrateur intégré.

La seconde invite d’informations d’identification vous demandera des informations d’identification que vous êtes autorisé à joindre l’ordinateur au domaine Active Directory.

Veillez à modifier la valeur de la «-DomainName « paramètre le nom de domaine complet de votre domaine Active Directory.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"
$domainCred = Get-Credential -Message "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -DomainName 'my.contoso.com' -DomainJoinCredentials $domainCred
```
## <a name="windows-answer-file-with-static-ipv4-addresses"></a>Fichier de réponses Windows avec des adresses IPv4 statiques

Les commandes suivantes créent un fichier de réponses Windows qui utilise des adresses IP statiques fournies au moment du déploiement par le Gestionnaire d’infrastructure, tels que System Center Virtual Machine Manager.

Virtual Machine Manager fournit trois composants à l’adresse IP statique à l’aide d’un pool d’adresses IP : Adresse IPv4, IPv6 adresse, adresse de la passerelle et adresse DNS. Si vous souhaitez que tous les champs supplémentaires à inclure ou nécessitent une configuration de réseau personnalisé, vous devez modifier manuellement le fichier de réponses produit par le script.

Les captures d’écran suivantes montrent les pools IP que vous pouvez configurer dans Virtual Machine Manager. Ces pools sont nécessaires si vous souhaitez utiliser une adresse IP statique.

Actuellement, la fonction prend en charge qu’un seul serveur DNS. Voici à quoi ressemblerait vos paramètres DNS :

![La configuration du serveur DNS avec le pool d’adresses IP statique](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-dns-settings.png)

Voici à quoi ressemblera votre résumé pour la création de pool d’adresses IP statiques. En bref, vous devez disposer d’un seul réseau route, une passerelle et un serveur DNS - et vous devez spécifier votre adresse IP.

![Résumé de la création du pool IP statique](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-summary.png)

Vous devez configurer votre carte réseau pour votre machine virtuelle. La capture d’écran suivante montre où définir cette configuration et basculez vers l’adresse IP statique.

![Configurer le matériel pour utiliser l’adresse IP statique](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-network-adapter-settings.png)

Ensuite, vous pouvez utiliser le `-StaticIPPool` paramètre pour inclure les éléments d’adresse IP statiques dans le fichier de réponses. Les paramètres `@IPAddr-1@`, `@NextHop-1-1@`, et `@DNSAddr-1-1@` dans la réponse fichier est alors remplacé par des valeurs réelles que vous avez spécifié dans Virtual Machine Manager au moment du déploiement.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -StaticIPPool IPv4Address
```

## <a name="windows-answer-file-with-a-custom-locale"></a>Fichier de réponses Windows avec des paramètres régionaux personnalisé

Les commandes suivantes créent un fichier de réponses Windows avec des paramètres régionaux personnalisé.

Lorsque vous êtes invité à entrer les informations d’identification d’administrateur, spécifiez le nom d’utilisateur souhaité et le mot de passe.
Utilisez « Administrateur » pour le nom d’utilisateur si vous souhaitez configurer le compte administrateur intégré.

```powershell
$adminCred = Get-Credential -Message "Local administrator account"
$domainCred = Get-Credential -Message "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -Locale es-ES
```

## <a name="basic-linux-answer-file"></a>Fichier de réponses base Linux

À compter de Windows Server version 1709, vous pouvez exécuter certains systèmes d’exploitation invités Linux dans des machines virtuelles protégées.
Si vous utilisez l’agent System Center Virtual Machine Manager Linux pour spécialiser ces machines virtuelles, l’applet de commande New-ShieldingDataAnswerFile pouvez créer des fichiers de réponses compatible pour elle.

Dans un fichier de réponses de Linux, vous inclurez généralement le mot de passe racine, clé SSH racine et informations sur le pool IP statiques si vous le souhaitez.
Remplacez le chemin d’accès à la moitié publique de votre clé SSH avant d’exécuter le script ci-dessous.

```powershell
$rootPassword = Read-Host -Prompt "Root password" -AsSecureString

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -RootPassword $rootPassword -RootSshKey '~\.ssh\id_rsa.pub'
```

## <a name="see-also"></a>Voir aussi

- [Déployer des machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Structure protégée et machines virtuelles dotées d’une protection maximale](guarded-fabric-and-shielded-vms-top-node.md)
