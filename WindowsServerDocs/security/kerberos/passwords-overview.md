---
title: Vue d’ensemble sur les mots de passe
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f608960e-2039-4c91-9c8c-9b81053c675e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 6c1b8d56b5c0da738e7dae5c0072be81040f90d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869600"
---
# <a name="passwords-overview"></a>Vue d’ensemble sur les mots de passe

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique destinée aux professionnels de l’informatique décrit les mots de passe utilisés dans les systèmes d’exploitation de Windows et des liens vers la documentation et des discussions sur l’utilisation de mots de passe dans une stratégie de gestion des informations d’identification.

## <a name="BKMK_OVER"></a>Description de la fonctionnalité
Systèmes d’exploitation et les applications sont conçues autour des mots de passe et même si vous utilisez des cartes à puce ou des systèmes biométriques, tous les comptes ont toujours des mots de passe et ils peuvent toujours être utilisés dans certaines circonstances. Certains comptes, notamment les comptes utilisés pour exécuter les services, ne peut pas même utiliser des cartes à puce et les jetons biométriques et doivent donc un mot de passe pour s’authentifier. Windows protège les mots de passe à l’aide des hachages de chiffrement.

Pour plus d’informations sur les mots de passe Windows, consultez [vue d’ensemble technique des mots de passe](https://technet.microsoft.com/library/hh994558(WS.10).aspx).

## <a name="BKMK_APP"></a>Applications pratiques
Dans Windows et de nombreux autres systèmes d’exploitation, la méthode la plus courante pour authentifier l’identité d’un utilisateur est d’utiliser une phrase secrète ou un mot de passe. Sécurisation de votre environnement réseau nécessite l’utilisation de mots de passe forts à tous les utilisateurs. Cela permet d’éviter les menaces d’un utilisateur malveillant de deviner un mot de passe faible, si par le biais des méthodes manuelles ou à l’aide d’outils, d’acquérir les informations d’identification d’un compte d’utilisateur compromis. Cela est particulièrement vrai pour les comptes administratifs. Lorsque vous modifiez un mot de passe complexe régulièrement, il réduit la probabilité d’une attaque de mot de passe compromettre ce compte.

## <a name="BKMK_NEW"></a>Fonctionnalités nouvelles et modifiées
Dans Windows Server 2012 et Windows 8, les mots de passe image sont nouvelles. Mots de passe image sont une combinaison d’une image sélectionnée utilisateur associée à une série de mouvements. La fonctionnalité de mot de passe image est désactivée sur domaine\-ordinateurs joints à un. Des liens vers plus d’informations sur les mots de passe image sont répertoriés dans [Voir aussi](#BKMK_LINKS) ci-dessous.

Il n’y a eu aucune modification à la fonctionnalité de mot de passe dans Windows Server 2012 et Windows 8. Aucun nouveau paramètre de stratégie de groupe n’ont été ajoutées. Toutefois, des améliorations ont été apportées dans les informations d’identification \(et mot de passe\) gestion, comme avec les mots de passe image, le stockage sécurisé des informations d’identification et la connexion à Windows 8 avec un compte Microsoft, anciennement appelé identifiant Windows Live ID .

## <a name="BKMK_DEP"></a>Fonctionnalités déconseillées
Aucune fonctionnalité de mot de passe a été déconseillée dans Windows Server 2012 et Windows 8.

## <a name="BKMK_SOFT"></a>Configuration logicielle requise
Dans les environnements d’entreprise, les mots de passe sont généralement gérées avec les Services de domaine Active Directory. Les mots de passe peuvent également être gérés sur l’ordinateur local en utilisant les paramètres de stratégie de mot de passe de paramètres de sécurité, stratégies de compte local.

## <a name="BKMK_LINKS"></a>Voir aussi
Ce tableau répertorie les ressources supplémentaires pour les fonctionnalités de mot de passe, gestion de la technologie et les informations d’identification.

|Type de contenu|Références|
|--------|-------|
|**Documentation des scénarios**|[Protection de votre identité numérique](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)|
|**Opérations**|[Utilisateurs et ordinateurs active Directory](https://technet.microsoft.com/library/cc754217.aspx)|
|**Résolution des problèmes**|[Découvrez quand votre mot de passe expire \- Blog de Active Directory PowerShell](http://blogs.msdn.com/b/adpowershell/archive/2010/08/09/9970198.aspx)|
|**Sécurité**| Windows Server 2008 R2 et Windows 7 [Guide des menaces et contre-mesures : Stratégies de compte](https://technet.microsoft.com/library/hh125920(v=ws.10).aspx)<br /><br />Conseils pour [modifier et créer des mots de passe forts](https://www.microsoft.com/security/online-privacy/passwords-create.aspx)|
|**Outils et paramètres**|[Référence de paramètres de stratégie de groupe pour Windows et Windows Server sur le centre de téléchargement Microsoft](https://www.microsoft.com/download/en/details.aspx?amp;displaylang=en&displaylang=en&id=25250)|
|**Ressources de la communauté**|[Protection de votre identité numérique](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)<br /><br />[Connexion à Windows 8 avec un Windows Live ID](http://blogs.msdn.com/b/b8/archive/2011/09/26/signing-in-to-windows-8-with-a-windows-live-id.aspx)<br /><br />[Vous connecter avec un mot de passe image](http://blogs.msdn.com/b/b8/archive/2011/12/16/signing-in-with-a-picture-password.aspx)<br /><br />[Optimisation de la sécurité de mot de passe image](http://blogs.msdn.com/b/b8/archive/2011/12/19/optimizing-picture-password-security.aspx)|


