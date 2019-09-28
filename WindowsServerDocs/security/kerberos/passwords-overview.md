---
title: Vue d’ensemble sur les mots de passe
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 27d456dd274b917233f0484f055b679dc8c73214
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403510"
---
# <a name="passwords-overview"></a>Vue d’ensemble sur les mots de passe

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique destinée aux professionnels de l’informatique décrit les mots de passe utilisés dans les systèmes d’exploitation Windows, ainsi que des liens vers la documentation et les discussions relatives à l’utilisation de mots de passe dans une stratégie de gestion des informations d’identification.

## <a name="BKMK_OVER"></a>Description de la fonctionnalité
Les systèmes d’exploitation et les applications actuels sont conçus autour des mots de passe, et même si vous utilisez des cartes à puce ou des systèmes biométriques, tous les comptes ont encore des mots de passe et ils peuvent toujours être utilisés dans certaines circonstances. Certains comptes, notamment les comptes utilisés pour exécuter des services, ne peuvent même pas utiliser des cartes à puce et des jetons biométriques, et doivent donc utiliser un mot de passe pour s’authentifier. Windows protège les mots de passe à l’aide de hachages de chiffrement.

Pour plus d’informations sur les mots de passe Windows, consultez [vue d’ensemble technique des mots de passe](https://technet.microsoft.com/library/hh994558(WS.10).aspx).

## <a name="BKMK_APP"></a>Applications pratiques
Dans Windows et de nombreux autres systèmes d’exploitation, la méthode la plus courante pour authentifier l’identité d’un utilisateur consiste à utiliser une phrase secrète secrète ou un mot de passe. La sécurisation de votre environnement réseau exige que tous les utilisateurs utilisent des mots de passe forts. Cela permet d’éviter la menace qu’un utilisateur malveillant devine un mot de passe faible, que ce soit via des méthodes manuelles ou à l’aide d’outils, pour obtenir les informations d’identification d’un compte d’utilisateur compromis. Cela est particulièrement vrai pour les comptes d’administration. Lorsque vous modifiez un mot de passe complexe régulièrement, cela réduit le risque qu’une attaque de mot de passe compromette ce compte.

## <a name="BKMK_NEW"></a>Fonctionnalités nouvelles et modifiées
Dans Windows Server 2012 et Windows 8, les mots de passe image sont nouveaux. Les mots de passe image sont une combinaison d’une image sélectionnée par l’utilisateur et associée à une série de gestes. La fonctionnalité de mot de passe image est désactivée sur les ordinateurs Domain @ no__t-0joined. Des liens vers des informations supplémentaires sur les mots de passe d’image sont répertoriés dans la [section Voir aussi](#BKMK_LINKS) ci-dessous.

Les fonctionnalités de mot de passe de Windows Server 2012 et Windows 8 n’ont pas été modifiées. Aucun nouveau paramètre de stratégie de groupe n’a été ajouté. Toutefois, des améliorations et des améliorations ont été apportées à la gestion des informations d’identification \(and @ no__t-1, comme avec les mots de passe image, le verrouillage des informations d’identification et la connexion à Windows 8 avec un compte Microsoft, anciennement appelé Windows Live ID.

## <a name="BKMK_DEP"></a>Fonctionnalités déconseillées
Aucune fonctionnalité de mot de passe n’a été dépréciée dans Windows Server 2012 et Windows 8.

## <a name="BKMK_SOFT"></a>Configuration logicielle requise
Dans les environnements d’entreprise, les mots de passe sont généralement gérés avec Active Directory Domain Services. Les mots de passe peuvent également être gérés sur l’ordinateur local à l’aide des paramètres dans paramètres de sécurité locale, stratégies de compte, stratégie de mot de passe.

## <a name="BKMK_LINKS"></a>Voir aussi
Ce tableau répertorie des ressources supplémentaires pour les fonctionnalités de mot de passe, la gestion des technologies et des informations d’identification.

|Type de contenu|Références|
|--------|-------|
|**Documentation du scénario**|[Protection de votre identité numérique](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)|
|**Opérations**|[Active Directory les utilisateurs et les ordinateurs](https://technet.microsoft.com/library/cc754217.aspx)|
|**Résolution des problèmes**|[Déterminer à quel moment votre mot de passe expire \- Active Directory blog PowerShell](http://blogs.msdn.com/b/adpowershell/archive/2010/08/09/9970198.aspx)|
|**Sécurité**| Guide de 0Threats et de contre-mesures Windows Server 2008 R2 et Windows @no__t 7 : Stratégies de compte @ no__t-0<br /><br />Conseils pour [modifier et créer des mots de passe forts](https://www.microsoft.com/security/online-privacy/passwords-create.aspx)|
|**Outils et paramètres**|[Informations de référence sur les paramètres de stratégie de groupe pour Windows et Windows Server dans le centre de téléchargement Microsoft](https://www.microsoft.com/download/en/details.aspx?amp;displaylang=en&displaylang=en&id=25250)|
|**Ressources de la communauté**|[Protection de votre identité numérique](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)<br /><br />[Connexion à Windows 8 avec un identifiant Windows Live ID](http://blogs.msdn.com/b/b8/archive/2011/09/26/signing-in-to-windows-8-with-a-windows-live-id.aspx)<br /><br />[Connexion avec un mot de passe image](http://blogs.msdn.com/b/b8/archive/2011/12/16/signing-in-with-a-picture-password.aspx)<br /><br />[Optimisation de la sécurité des mots de passe image](http://blogs.msdn.com/b/b8/archive/2011/12/19/optimizing-picture-password-security.aspx)|


