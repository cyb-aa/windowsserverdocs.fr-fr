---
title: Configurer des nœuds SGH supplémentaires
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 227f723b-acb2-42a7-bbe3-44e82f930e35
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 10/22/2018
ms.openlocfilehash: c0741845bdbd8bfbea00df21d1fe810a27fc6a3b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443862"
---
# <a name="configure-additional-hgs-nodes"></a>Configurer des nœuds SGH supplémentaires

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Dans les environnements de production, SGH doit être configurés dans un cluster à haute disponibilité pour vous assurer que les machines virtuelles protégées peuvent être allumés même si un nœud SGH tombe en panne. Pour les environnements de test, les nœuds SGH secondaires ne sont pas nécessaires.

Utilisez une des méthodes suivantes pour ajouter des nœuds SGH, idéal pour votre environnement.

|                |                         |                              | 
|----------------|-------------------------|------------------------------|
|Nouvelle forêt SGH  | [À l’aide de fichiers PFX](#dedicated-hgs-forest-with-pfx-certificates) | [À l’aide d’empreintes numériques de certificat](#dedicated-hgs-forest-with-certificate-thumbprints) |
|Forêt bastion existante |  [À l’aide de fichiers PFX](#existing-bastion-forest-with-pfx-certificates) | [À l’aide d’empreintes numériques de certificat](#existing-bastion-forest-with-certificate-thumbprints) |

## <a name="prerequisites"></a>Prérequis

Assurez-vous que chaque nœud supplémentaire : 
- A la même configuration matérielle et logicielle que le nœud principal 
- Est connecté au même réseau que les autres serveurs SGH
- Peut résoudre les autres serveurs SGH par leurs noms DNS

## <a name="dedicated-hgs-forest-with-pfx-certificates"></a>Forêt SGH dédiée avec des certificats PFX

1. Promouvoir le nœud SGH pour un contrôleur de domaine
2. Initialiser le serveur SGH

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>Promouvoir le nœud SGH pour un contrôleur de domaine

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)] 

### <a name="initialize-the-hgs-server"></a>Initialiser le serveur SGH

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="dedicated-hgs-forest-with-certificate-thumbprints"></a>Forêt SGH dédiée avec empreintes numériques de certificat
 
1. Promouvoir le nœud SGH pour un contrôleur de domaine
2. Initialiser le serveur SGH
3. Installer les clés privées des certificats

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>Promouvoir le nœud SGH pour un contrôleur de domaine

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)] 

### <a name="initialize-the-hgs-server"></a>Initialiser le serveur SGH

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

### <a name="install-the-private-keys-for-the-certificates"></a>Installer les clés privées des certificats

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="existing-bastion-forest-with-pfx-certificates"></a>Forêt bastion existante avec des certificats PFX

1. Joindre le nœud au domaine existant
2. Accorder les droits de l’ordinateur pour récupérer le mot de passe de service administré de groupe et exécutez Install-ADServiceAccount
3. Initialiser le serveur SGH

### <a name="join-the-node-to-the-existing-domain"></a>Joindre le nœud au domaine existant

1. Vérifiez qu'au moins une carte réseau sur le nœud est configurée pour utiliser le serveur DNS sur votre premier serveur SGH.
2. Joindre le nouveau nœud SGH pour le même domaine que votre premier nœud SGH. 

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>Accorder les droits de l’ordinateur pour récupérer le mot de passe de service administré de groupe et exécutez Install-ADServiceAccount

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)] 

### <a name="initialize-the-hgs-server"></a>Initialiser le serveur SGH

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="existing-bastion-forest-with-certificate-thumbprints"></a>Forêt bastion existante avec les empreintes numériques de certificat

1. Joindre le nœud au domaine existant
2. Accorder les droits de l’ordinateur pour récupérer le mot de passe de service administré de groupe et exécutez Install-ADServiceAccount
3. Initialiser le serveur SGH
4. Installer les clés privées des certificats

### <a name="join-the-node-to-the-existing-domain"></a>Joindre le nœud au domaine existant

1. Vérifiez qu'au moins une carte réseau sur le nœud est configurée pour utiliser le serveur DNS sur votre premier serveur SGH.
2. Joindre le nouveau nœud SGH pour le même domaine que votre premier nœud SGH. 

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>Accorder les droits de l’ordinateur pour récupérer le mot de passe de service administré de groupe et exécutez Install-ADServiceAccount

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)] 

### <a name="initialize-the-hgs-server"></a>Initialiser le serveur SGH

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

Il prendra jusqu'à 10 minutes pour le chiffrement et les certificats de signature à partir du premier serveur SGH pour répliquer vers ce nœud.

### <a name="install-the-private-keys-for-the-certificates"></a>Installer les clés privées des certificats

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="configure-hgs-for-https-communications"></a>Configurer le service SGH pour les communications HTTPS

Si vous souhaitez sécuriser des points de terminaison de service SGH avec un certificat SSL, vous devez configurer le certificat SSL sur ce nœud, ainsi que tous les nœuds du cluster SGH.
Certificats SSL *ne sont pas* répliquées par SGH et n’avez pas besoin d’utiliser les mêmes clés pour chaque nœud (par exemple, vous pouvez avoir différents certificats SSL pour chaque nœud).

Lorsque vous demandez un certificat SSL, vérifiez le nom de domaine complet du cluster (comme indiqué dans la sortie de `Get-HgsServer`) est le nom commun sujet du certificat, ou inclus en tant qu’un autre DNS nom d’objet.
Lorsque vous avez obtenu un certificat à partir de votre autorité de certification, vous pouvez configurer le service SGH pour l’utiliser avec [Set-HgsServer](https://technet.microsoft.com/itpro/powershell/windows/hgsserver/set-hgsserver).

```powershell
$sslPassword = Read-Host -AsSecureString -Prompt "SSL Certificate Password"
Set-HgsServer -Http -Https -HttpsCertificatePath 'C:\temp\HgsSSLCertificate.pfx' -HttpsCertificatePassword $sslPassword
```

Si vous avez déjà installé le certificat dans le magasin de certificats local et que vous souhaitez faire référence par empreinte numérique, exécutez la commande suivante à la place :

```powershell
Set-HgsServer -Http -Https -HttpsCertificateThumbprint 'A1B2C3D4E5F6...'
```

SGH exposera toujours les ports HTTP et HTTPS pour la communication.
Il est non pris en charge pour supprimer la liaison HTTP dans IIS, mais vous pouvez utiliser le pare-feu Windows ou autres technologies de pare-feu de réseau pour bloquer les communications sur le port 80.

## <a name="decommission-an-hgs-node"></a>Retirer un nœud SGH

Pour retirer un nœud SGH :

1. [Effacer la configuration de SGH](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration).

   Cela supprime le nœud du cluster et désinstalle l’attestation et les services de protection de clé. 
   Si c’est le dernier nœud dans le cluster, - Force est nécessaire pour indiquer vous ne souhaitez pas supprimer le dernier nœud et de détruire le cluster dans Active Directory. 
   
   Si SGH est déployé dans une forêt bastion (valeur par défaut), qui est la seule étape. 
   Vous pouvez éventuellement disjoindre l’ordinateur à partir du domaine et supprimez le compte de service administré de groupe d’Active Directory.

1. Si SGH créé son propre domaine, vous devez également [désinstaller SGH](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration) visant à disjoindre le domaine et de rétrograder le contrôleur de domaine.



## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Valider la configuration de SGH](guarded-fabric-verify-hgs-configuration.md)

