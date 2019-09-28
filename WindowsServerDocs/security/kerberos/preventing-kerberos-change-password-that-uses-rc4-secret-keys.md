---
title: Empêcher Kerberos modifier le mot de passe qui utilise des clés secrètes RC4
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: de207d55-aa3d-4c16-bd3b-496db43663a4
manager: alanth
author: justinha
ms.technology: security-crdential-protection-and-management
ms.date: 11/09/2016
ms.openlocfilehash: 64018f7f118086f3d290cb1ffa9b8d2b3e81c27c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386267"
---
# <a name="preventing-kerberos-change-password-that-uses-rc4-secret-keys"></a>Empêcher Kerberos de modifier le mot de passe utilisant des clés secrètes RC4

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2008 R2 et Windows Server 2008

Cette rubrique destinée aux professionnels de l’informatique présente certaines limitations dans le protocole Kerberos qui pourraient permettre à un utilisateur malveillant de contrôler le compte d’un utilisateur. Il existe une limitation dans le service d’authentification réseau Kerberos (v5) standard (RFC 4120), qui est bien connu dans le secteur, dans lequel une personne malveillante peut s’authentifier en tant qu’utilisateur ou modifier le mot de passe de cet utilisateur si ce dernier connaît la clé secrète de l’utilisateur.

La possession des clés secrètes Kerberos dérivées du mot de passe d’un utilisateur (RC4 et Advanced Encryption Standard [AES] par défaut) est validée pendant l’échange de modification de mot de passe Kerberos par RFC 4757. Le mot de passe en texte clair de l’utilisateur n’est jamais fourni au centre de distribution de clés (KDC) et, par défaut, les contrôleurs de domaine Active Directory ne possèdent pas de copie de mots de passe en texte clair pour les comptes. Si le contrôleur de domaine ne prend pas en charge un type de chiffrement Kerberos, cette clé secrète ne peut pas être utilisée pour modifier le mot de passe. 

Dans les systèmes d’exploitation Windows désignés dans la liste s’applique à au début de cette rubrique, il existe trois façons de bloquer la possibilité de modifier les mots de passe à l’aide de Kerberos avec des clés secrètes RC4 :

- Configurez le compte d’utilisateur pour inclure la carte à puce de l’option de compte est nécessaire pour l’ouverture de session interactive. Cela limite l’utilisateur à se connecter uniquement à l’aide d’une carte à puce valide afin que les demandes du service d’authentification RC4 (en tant que demandes) soient rejetées. Pour définir les options de compte d’un compte, cliquez avec le bouton droit sur le compte, cliquez sur Propriétés, puis sur l’onglet compte. 

- Désactivez la prise en charge de RC4 pour Kerberos sur tous les contrôleurs de domaine. Cela nécessite au minimum un niveau fonctionnel de domaine Windows Server 2008 et un environnement dans lequel tous les clients Kerberos, les serveurs d’applications et les relations d’approbation vers et depuis le domaine doivent prendre en charge AES. La prise en charge d’AES a été introduite dans Windows Server 2008 et Windows Vista.

    [!NOTE]
    Il existe un problème connu avec la désactivation de RC4, ce qui peut entraîner le redémarrage du système. Consultez les correctifs suivants :
    - [Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/3038261)
    - [Windows Server 2012](https://support.microsoft.com/en-us/kb/3086213)
    - Aucun correctif logiciel n’est disponible pour les versions antérieures de Windows Server

- Déployez les domaines définis sur le niveau fonctionnel du domaine Windows Server 2012 R2 ou supérieur, et configurez les utilisateurs en tant que membres du groupe de sécurité utilisateurs protégés. Étant donné que cette fonctionnalité perturbe plus que la simple utilisation de RC4 dans le protocole Kerberos, consultez les ressources dans la section suivante, [Voir aussi](#see-also) .

## <a name="see-also"></a>Voir aussi

- Pour plus d’informations sur la façon d’empêcher l’utilisation du type de chiffrement RC4 dans les domaines Windows Server 2012 R2, consultez [groupe de sécurité utilisateurs protégés](/../credentials-protection-and-management/protected-users-security-group.md)et [Comment configurer des comptes protégés](/../credentials-protection-and-management/how-to-configure-protected-accounts.md).

- Pour plus d’informations sur les RFC 4120 et RFC 4757, consultez les documents de l' [IETF](http://tools.ietf.org/html/).
