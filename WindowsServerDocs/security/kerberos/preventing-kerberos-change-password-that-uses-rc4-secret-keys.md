---
title: "Prévention Kerberos modifier le mot de passe qui utilisent des clés secrètes RC4"
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: de207d55-aa3d-4c16-bd3b-496db43663a4
manager: alanth
author: justinha
ms.technology: security-crdential-protection-and-management
ms.date: 11/09/2016
ms.openlocfilehash: 3cfa4ae2442f0cd1e3e4110a355da675fa2d61db
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="preventing-kerberos-change-password-that-uses-rc4-secret-keys"></a>Prévention Kerberos modifier le mot de passe qui utilise des clés secrètes RC4

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016, Windows Server2008R2 et Windows Server2008

Cette rubrique destinée aux professionnels de l’informatique décrit certaines limitations du protocole Kerberos qui pourrait conduire à un utilisateur malveillant prendre le contrôle de compte d’un utilisateur. Il existe une limitation dans la norme de Service d’authentification réseau Kerberos (V5) (RFC4120), qui est bien connue au sein de l’industrie, dans laquelle une personne malveillante peut s’authentifier en tant qu’utilisateur ou modifier le mot de passe de cet utilisateur si la personne malveillante connaît clé secrète de l’utilisateur.

En possession de dérivé de mot de passe clés secrètes Kerberos l’utilisateur (Advanced Encryption Standard [AES] par défaut et RC4) est validée lors de l’échange de modification de mot de passe Kerberos par RFC4757. Le mot de passe en texte en clair n’est jamais fournie pour le centre de Distribution de clés (KDC), et par défaut, les contrôleurs de domaine ActiveDirectory ne possèdent pas d’une copie de mots de passe en texte en clair pour les comptes. Si le contrôleur de domaine ne prend pas en charge un type de chiffrement Kerberos, cette clé secrète ne peut pas être utilisée pour modifier le mot de passe. 

Dans les systèmes d’exploitation Windows figurant dans la liste s’applique au début de cette rubrique, il existe trois façons de bloquer la possibilité de modifier les mots de passe à l’aide de Kerberos avec les clés secrètes RC4:

- Configurer l’utilisateur à inclure l’option de compte carte à puce est nécessaire pour l’ouverture de session interactive. Cela limite l’utilisateur à une session uniquement avec une carte à puce valide afin que les demandes de service RC4 d’authentification (AS-demandes) sont rejetées. Pour définir les options de compte sur un compte, avec le bouton droit sur le compte, cliquez sur Propriétés, puis cliquez sur l’onglet compte. 

- Désactivez la prise en charge de RC4 pour Kerberos sur tous les contrôleurs de domaine. Cela nécessite au moins un niveau fonctionnel du domaine Windows Server2008 et d’un environnement dans lequel tous les clients, serveurs d’applications et les relations d’approbation vers et depuis le domaine Kerberos doivent prendre en charge AES. Prise en charge du chiffrement AES a été introduite dans Windows Server2008 et WindowsVista.

    [!NOTE]
    Il existe un problème connu avec la désactivation de RC4 qui peut provoquer le redémarrage du système. Consultez les correctifs suivants:
    - [Windows Server2012R2](https://support.microsoft.com/en-us/kb/3038261)
    - [Windows Server2012](https://support.microsoft.com/en-us/kb/3086213)
    - Aucun correctif n’est disponible pour les versions antérieures de Windows Server

- Déployer l’ensemble des domaines au niveau fonctionnel de domaine Windows Server2012R2 ou version ultérieure et configurer les utilisateurs en tant que membres du groupe de sécurité utilisateurs protégés. Étant donné que cette fonctionnalité bloque plus que RC4 l’utilisation du protocole Kerberos, voir les ressources dans l’exemple suivant [Voir aussi](#see-also) section.

## <a name="see-also"></a>Voir aussi

- Pour plus d’informations sur comment empêcher l’utilisation du type de chiffrement RC4 dans des domaines Windows Server2012R2, consultez [groupe de sécurité utilisateurs protégés](/../credentials-protection-and-management/protected-users-security-group.md), et [comment configurer des comptes protégés](/../credentials-protection-and-management/how-to-configure-protected-accounts.md).

- Pour obtenir des explications sur les RFC4120 et RFC4757, consultez [Documents de l’IETF](http://tools.ietf.org/html/).
