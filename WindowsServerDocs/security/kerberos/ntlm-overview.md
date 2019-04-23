---
title: Vue d’ensemble de l’authentification NTLM
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 773909fd-c0bc-498a-95fc-bb452ec04d90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 523fb71304ae55d17203cab4d1c5a17551bf8fdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890640"
---
# <a name="ntlm-overview"></a>Vue d’ensemble de l’authentification NTLM

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique destinée aux professionnels de l’informatique décrit l’authentification NTLM, les modifications apportées aux fonctionnalités et fournit des liens vers des ressources techniques pour l’authentification Windows et NTLM pour Windows Server 2012 et les versions précédentes.

## <a name="BKMK_OVER"></a>Description de la fonctionnalité
L’authentification NTLM est une famille de protocoles d’authentification qui sont comprises dans le Windows Msv1\_0.dll. Les protocoles d’authentification NTLM comprennent les versions 1 et 2 de LAN Manager et les versions 1 et 2 de NTLM . Les protocoles d’authentification NTLM authentifient les utilisateurs et les ordinateurs basés sur une stimulation\/mécanisme de réponse destiné à prouver à un serveur ou un contrôleur de domaine qu’un utilisateur connaît le mot de passe associé à un compte. Lorsque vous faites appel au protocole NTLM, un serveur de ressources doit entreprendre l’une des actions suivantes afin de vérifier l’identité d’un ordinateur ou d’un utilisateur dès qu’un nouveau jeton d’accès est nécessaire :

-   Contactez un service d’authentification de domaine sur le contrôleur de domaine du compte de domaine de l’ordinateur ou de l’utilisateur si le compte en question est un compte de domaine.

-   Si le compte est un compte local, recherchez le compte de l’ordinateur ou de l’utilisateur dans la base de données du compte local.

## <a name="BKMK_APP"></a>Applications en cours
L’authentification NTLM est toujours prise en charge et doit servir à des fins d’authentification Windows avec des systèmes configurés en tant que membres d’un groupe de travail. L’authentification NTLM est également utilisée pour l’authentification d’ouverture de session locale sur non\-contrôleurs de domaine. L’authentification Kerberos version 5 est la méthode d’authentification recommandée pour les environnements Active Directory, mais un non\-Microsoft ou Microsoft application peut continuer d’utiliser NTLM.

Limiter l’utilisation du protocole NTLM dans un environnement informatique requiert de connaître les exigences liées à l’application déployée sur NTLM et les stratégies et étapes nécessaires à la configuration des environnements informatiques pour utiliser d’autres protocoles. De nouveaux paramètres et outils ont été ajoutés pour vous permettre de découvrir comment le protocole NTLM est utilisé pour limiter de manière sélective le trafic NTLM. Pour plus d’informations sur la façon d’analyser et de limiter l’utilisation de NTLM dans vos environnements, voir [Présentation des limites de l’authentification NTLM](https://technet.microsoft.com/library/dd560653(v=ws.10).aspx) pour accéder au guide relatif à l’audit et à la limitation de l’utilisation du protocole NTLM.

## <a name="BKMK_NEW"></a>Fonctionnalités nouvelles et modifiées
Il n’existe aucune modification de fonctionnalité pour NTLM pour Windows Server 2012.

## <a name="BKMK_DEP"></a>Fonctionnalité supprimée ou déconseillée
Il n’existe aucune fonctionnalité supprimée ou déconseillée pour NTLM pour Windows Server 2012.

## <a name="BKMK_INSTALL"></a>Informations de gestionnaire de serveur
L’authentification NTLM ne peut être configurée à partir du Gestionnaire de serveur. Vous pouvez faire appel aux paramètres de stratégie de sécurité ou aux stratégies de groupe pour gérer le mode d’application de l’authentification NTLM entre un système informatique et un autre. Dans un domaine, Kerberos constitue le protocole d’authentification par défaut.

## <a name="BKMK_LINKS"></a>Voir aussi
Le tableau qui suit répertorie les ressources adaptées à l’authentification NTLM et à d’autres technologies d’authentification Windows.

|Type de contenu|Références|
|--------|-------|
|**Évaluation du produit**|[Présentation de la Restriction de l’authentification NTLM](https://technet.microsoft.com/library/dd560653.aspx)<br /><br />[Modifications apportées à l’authentification NTLM](https://technet.microsoft.com/library/dd566199.aspx)|
|**Planification**|[Guide de modélisation de menaces Infrastructure informatique](https://technet.microsoft.com/library/dd941826.aspx)<br /><br />[Menaces et contre-mesures : Paramètres de sécurité dans Windows Server 2003 et Windows XP](https://technet.microsoft.com/library/dd162275.aspx)<br /><br />[Guide des menaces et des contre-mesures : Paramètres de sécurité dans Windows Server 2008 et Windows Vista](https://technet.microsoft.com/library/dd349791.aspx)<br /><br />[Guide des menaces et des contre-mesures : Paramètres de sécurité dans Windows Server 2008 R2 et Windows 7](https://technet.microsoft.com/library/hh125921.aspx)|
|**Déploiement**|[Protection étendue pour l’authentification](https://support.microsoft.com/kb/968389)<br /><br />[L’audit et en limitant le guide d’utilisation NTLM](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)<br /><br />[Demandez à l’équipe de Services d’annuaire : Blocage de NTLM et vous : Analyse de l’application méthodologies d’audit et dans Windows 7](https://blogs.technet.com/askds/archive/2009/10/08/ntlm-blocking-and-you-application-analysis-and-auditing-methodologies-in-windows-7.aspx)<br /><br />[Blog de l’authentification Windows](https://blogs.technet.com/authentication/)<br /><br />[Configuration de MaxConcurrentAPI pour passer NTLM\-via l’authentification](https://social.technet.microsoft.com/wiki/contents/articles/9759.configuring-maxconcurrentapi-for-ntlm-pass-through-authentication.aspx)|
|**Développement**|[Microsoft NTLM \(Windows\)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)<br /><br />[\[MS\-NLMP\]: NT LAN Manager \(NTLM\) spécification du protocole d’authentification](https://msdn.microsoft.com/library/cc236621(PROT.10).aspx)<br /><br />[\[MS\-NNTP\]: NT LAN Manager \(NTLM\) l’authentification : Network News Transfer Protocol \(NNTP\) Extension](https://msdn.microsoft.com/library/cc236774(PROT.10).aspx)<br /><br />[\[MS\-NTHT\]: NTLM sur la spécification du protocole HTTP](https://msdn.microsoft.com/library/cc237488(PROT.10).aspx)|
|**Résolution des problèmes**|Pas encore disponible|
|**Ressources de la communauté**|[Ce cheval n’est mort encore : Les goulots d’étranglement NTLM et le runtime RPC](http://blogs.technet.com/b/askds/archive/2011/09/15/is-this-horse-dead-yet-ntlm-bottlenecks-and-the-rpc-runtime.aspx)|



