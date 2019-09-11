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
ms.openlocfilehash: fe9cd63f08da849c2cb002ab853501370d736f0f
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870588"
---
# <a name="configure-additional-hgs-nodes"></a>Configurer des nœuds SGH supplémentaires

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Dans les environnements de production, SGH doit être configuré dans un cluster à haute disponibilité pour s’assurer que les machines virtuelles protégées peuvent être alimentées même en cas de défaillance d’un nœud SGH. Pour les environnements de test, les nœuds SGH secondaires ne sont pas requis.

Utilisez l’une des méthodes suivantes pour ajouter des nœuds SGH, le plus adapté à votre environnement.

|                |                         |                              | 
|----------------|-------------------------|------------------------------|
|Nouvelle forêt SGH  | [Utilisation de fichiers PFX](#dedicated-hgs-forest-with-pfx-certificates) | [Utilisation d’empreintes numériques de certificat](#dedicated-hgs-forest-with-certificate-thumbprints) |
|Forêt bastion existante |  [Utilisation de fichiers PFX](#existing-bastion-forest-with-pfx-certificates) | [Utilisation d’empreintes numériques de certificat](#existing-bastion-forest-with-certificate-thumbprints) |

## <a name="prerequisites"></a>Prérequis

Assurez-vous que chaque nœud supplémentaire : 
- A la même configuration matérielle et logicielle que le nœud principal 
- Est connecté au même réseau que les autres serveurs SGH
- Peut résoudre les autres serveurs SGH par leurs noms DNS

## <a name="dedicated-hgs-forest-with-pfx-certificates"></a>Forêt SGH dédiée avec certificats PFX

1. Promouvoir le nœud SGH en contrôleur de domaine
2. Initialiser le serveur SGH

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>Promouvoir le nœud SGH en contrôleur de domaine

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)] 

### <a name="initialize-the-hgs-server"></a>Initialiser le serveur SGH

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="dedicated-hgs-forest-with-certificate-thumbprints"></a>Forêt SGH dédiée avec empreintes numériques de certificat
 
1. Promouvoir le nœud SGH en contrôleur de domaine
2. Initialiser le serveur SGH
3. Installer les clés privées pour les certificats

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>Promouvoir le nœud SGH en contrôleur de domaine

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)] 

### <a name="initialize-the-hgs-server"></a>Initialiser le serveur SGH

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

### <a name="install-the-private-keys-for-the-certificates"></a>Installer les clés privées pour les certificats

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="existing-bastion-forest-with-pfx-certificates"></a>Forêt bastion existante avec certificats PFX

1. Joindre le nœud au domaine existant
2. Accordez les droits de l’ordinateur pour récupérer le mot de passe gMSA et exécutez Install-ADServiceAccount
3. Initialiser le serveur SGH

### <a name="join-the-node-to-the-existing-domain"></a>Joindre le nœud au domaine existant

1. Assurez-vous qu’au moins une carte réseau sur le nœud est configurée pour utiliser le serveur DNS sur votre premier serveur SGH.
2. Joignez le nouveau nœud SGH au même domaine que votre premier nœud SGH. 

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>Accordez les droits de l’ordinateur pour récupérer le mot de passe gMSA et exécutez Install-ADServiceAccount

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)] 

### <a name="initialize-the-hgs-server"></a>Initialiser le serveur SGH

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="existing-bastion-forest-with-certificate-thumbprints"></a>Forêt bastion existante avec empreintes de certificat

1. Joindre le nœud au domaine existant
2. Accordez les droits de l’ordinateur pour récupérer le mot de passe gMSA et exécutez Install-ADServiceAccount
3. Initialiser le serveur SGH
4. Installer les clés privées pour les certificats

### <a name="join-the-node-to-the-existing-domain"></a>Joindre le nœud au domaine existant

1. Assurez-vous qu’au moins une carte réseau sur le nœud est configurée pour utiliser le serveur DNS sur votre premier serveur SGH.
2. Joignez le nouveau nœud SGH au même domaine que votre premier nœud SGH. 

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>Accordez les droits de l’ordinateur pour récupérer le mot de passe gMSA et exécutez Install-ADServiceAccount

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)] 

### <a name="initialize-the-hgs-server"></a>Initialiser le serveur SGH

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

La réplication des certificats de chiffrement et de signature du premier serveur SGH sur ce nœud prendra jusqu’à 10 minutes.

### <a name="install-the-private-keys-for-the-certificates"></a>Installer les clés privées pour les certificats

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="configure-hgs-for-https-communications"></a>Configurer SGH pour les communications HTTPs

Si vous souhaitez sécuriser des points de terminaison SGH avec un certificat SSL, vous devez configurer le certificat SSL sur ce nœud, ainsi que sur tous les autres nœuds du cluster SGH.
Les certificats SSL ne *sont pas* répliqués par SGH et n’ont pas besoin d’utiliser les mêmes clés pour chaque nœud (par exemple, vous pouvez avoir différents certificats SSL pour chaque nœud).

Lorsque vous demandez un certificat SSL, assurez-vous que le nom de domaine complet du cluster ( `Get-HgsServer`comme indiqué dans la sortie de) est soit le nom commun du sujet du certificat, soit il est inclus comme autre nom DNS de l’objet.
Une fois que vous avez obtenu un certificat auprès de votre autorité de certification, vous pouvez configurer le service SGH pour l’utiliser avec [Set-HgsServer](https://technet.microsoft.com/itpro/powershell/windows/hgsserver/set-hgsserver).

```powershell
$sslPassword = Read-Host -AsSecureString -Prompt "SSL Certificate Password"
Set-HgsServer -Http -Https -HttpsCertificatePath 'C:\temp\HgsSSLCertificate.pfx' -HttpsCertificatePassword $sslPassword
```

Si vous avez déjà installé le certificat dans le magasin de certificats local et que vous souhaitez le référencer par empreinte, exécutez la commande suivante à la place :

```powershell
Set-HgsServer -Http -Https -HttpsCertificateThumbprint 'A1B2C3D4E5F6...'
```

SGH exposera toujours les ports HTTP et HTTPs pour la communication.
La suppression de la liaison HTTP dans IIS n’est pas prise en charge. Toutefois, vous pouvez utiliser le pare-feu Windows ou d’autres technologies de pare-feu réseau pour bloquer les communications sur le port 80.

## <a name="decommission-an-hgs-node"></a>Désactiver un nœud SGH

Pour désactiver un nœud SGH :

1. [Désactivez la configuration SGH](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration).

   Cela permet de supprimer le nœud du cluster et de désinstaller les services d’attestation et de protection de clé. 
   S’il s’agit du dernier nœud dans le cluster,-force est nécessaire pour indiquer que vous souhaitez supprimer le dernier nœud et détruire le cluster dans Active Directory. 
   
   Si SGH est déployé dans une forêt bastion (par défaut), il s’agit de la seule étape. 
   Vous pouvez éventuellement annuler la jonction de l’ordinateur au domaine et supprimer le compte gMSA de Active Directory.

1. Si SGH a créé son propre domaine, vous devez également [désinstaller SGH pour supprimer](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration) la jonction au domaine et rétrograder le contrôleur de domaine.



## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Valider la configuration SGH](guarded-fabric-verify-hgs-configuration.md)

