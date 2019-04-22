---
title: Kerberos empêchant modifier le mot de passe qui utilisent des clés secrètes RC4
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: de207d55-aa3d-4c16-bd3b-496db43663a4
manager: alanth
author: justinha
ms.technology: security-crdential-protection-and-management
ms.date: 11/09/2016
ms.openlocfilehash: 3cfa4ae2442f0cd1e3e4110a355da675fa2d61db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816270"
---
# <a name="preventing-kerberos-change-password-that-uses-rc4-secret-keys"></a>Empêcher Kerberos de modifier le mot de passe utilisant des clés secrètes RC4

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2008 R2 et Windows Server 2008

Cette rubrique destinée aux professionnels de l’informatique explique certaines limitations dans le protocole Kerberos qui peuvent aboutir à un utilisateur malveillant prenne le contrôle de compte d’un utilisateur. Il existe une limitation dans la norme de Service d’authentification réseau Kerberos (V5) (RFC 4120), qui est connue au sein de l’industrie, par laquelle une personne malveillante peut s’authentifier en tant qu’utilisateur ou modifier le mot de passe de cet utilisateur si l’attaquant connaît la clé secrète de l’utilisateur.

Possession de dérivée de mot de passe Kerberos clés secrètes l’utilisateur (RC4 et Advanced Encryption Standard [AES] par défaut) est validée au cours de l’échange de modification de mot de passe Kerberos par RFC 4757. Le mot de passe en texte clair n’est jamais fourni pour le centre de Distribution de clés (KDC), et par défaut, les contrôleurs de domaine Active Directory ne possèdent pas une copie des mots de passe en texte clair pour les comptes. Si le contrôleur de domaine ne prend pas en charge un type de chiffrement Kerberos, cette clé secrète ne peut pas être utilisée pour modifier le mot de passe. 

Dans les systèmes d’exploitation de Windows désignés dans la liste s’applique au début de cette rubrique, il existe trois façons de bloquer la possibilité de modifier les mots de passe à l’aide de Kerberos avec les clés secrètes RC4 :

- Configurez l’utilisateur du compte à inclure l’option de compte de carte à puce est nécessaire pour l’ouverture de session interactive. Cela limite l’utilisateur à connecter uniquement à une carte à puce valide afin que les demandes de service d’authentification RC4 (AS-impératifs) sont rejetées. Pour définir les options de compte sur un compte, avec le bouton droit sur le compte, cliquez sur Propriétés, puis cliquez sur l’onglet compte. 

- Désactiver la prise en charge RC4 pour Kerberos sur tous les contrôleurs de domaine. Cela nécessite au moins un niveau fonctionnel du domaine Windows Server 2008 et d’un environnement où tous les clients, serveurs d’applications et les relations d’approbation vers et depuis le domaine Kerberos doivent prendre en charge AES. Prise en charge d’AES a été introduite dans Windows Server 2008 et Windows Vista.

    [!NOTE]
    Il existe un problème connu avec la désactivation de RC4, ce qui peut entraîner le redémarrage du système. Consultez les correctifs suivants :
    - [Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/3038261)
    - [Windows Server 2012](https://support.microsoft.com/en-us/kb/3086213)
    - Aucun correctif n’est disponible pour les versions antérieures de Windows Server

- Déployer l’ensemble des domaines à un niveau fonctionnel du domaine Windows Server 2012 R2 ou version ultérieure et configurer les utilisateurs en tant que membres du groupe de sécurité utilisateurs protégés. Étant donné que cette fonctionnalité bloque plus que RC4 l’utilisation du protocole Kerberos, consultez les ressources dans l’exemple suivant [Voir aussi](#see-also) section.

## <a name="see-also"></a>Voir aussi

- Pour plus d’informations sur la façon d’empêcher l’utilisation du type de chiffrement RC4 dans des domaines Windows Server 2012 R2, consultez [Protected Users Security Group](/../credentials-protection-and-management/protected-users-security-group.md), et [comment configurer des comptes protégés](/../credentials-protection-and-management/how-to-configure-protected-accounts.md).

- Pour obtenir des explications sur les RFC 4120 et RFC 4757, consultez [Documents de l’IETF](http://tools.ietf.org/html/).
