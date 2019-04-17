---
title: "Vue d’ensemble NTLM"
description: "Sécurité de Windows Server"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="ntlm-overview"></a>Vue d’ensemble NTLM

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique destinée aux professionnels de l’informatique décrit NTLM, toutes les modifications apportées aux fonctionnalités et fournit des liens vers des ressources techniques pour l’authentification Windows et NTLM pour Windows Server2012 et les versions précédentes.

## <a name="BKMK_OVER"></a>Description de la fonctionnalité
L’authentification NTLM est une famille de protocoles d’authentification qui sont inclus dans le Msv1\_0.dll Windows. Les protocoles d’authentification NTLM comprennent les versions 1 et 2 de NTLM version1 et 2. Les protocoles d’authentification NTLM authentifient les utilisateurs et des ordinateurs basés sur un mécanisme challenge\/réponse destiné à prouver à un serveur ou un contrôleur de domaine qu’un utilisateur connaît le mot de passe associé à un compte. Lorsque le protocole NTLM est utilisé, un serveur de ressources doit prendre l’une des actions suivantes pour vérifier l’identité d’un ordinateur ou l’utilisateur chaque fois qu’un nouveau jeton d’accès est nécessaire:

-   Contactez un service d’authentification de domaine sur le contrôleur de domaine pour le domaine de compte de l’ordinateur ou de l’utilisateur, si le compte est un compte de domaine.

-   Recherchez le compte de l’ordinateur ou de l’utilisateur dans la base de données du compte local, si le compte est un compte local.

## <a name="BKMK_APP"></a>Applications actuelles
L’authentification NTLM est toujours pris en charge et doit être utilisée pour l’authentification Windows avec des systèmes configurés en tant que membre d’un groupe de travail. L’authentification NTLM est également utilisée pour l’authentification d’ouverture de session locale sur des contrôleurs de domaine autre que celle. L’authentification Kerberos version5 est la méthode d’authentification préféré pour les environnements ActiveDirectory, mais un autre que celle que Microsoft ou une application de Microsoft peut continuer d’utiliser NTLM.

Ce qui réduit l’utilisation du protocole NTLM dans un environnement informatique requiert les deux connaissance des exigences de l’application déployée sur NTLM et les stratégies et les étapes nécessaires à la configuration des environnements informatiques d’utiliser d’autres protocoles. Nouveaux paramètres et outils ont été ajoutés pour vous aider à découvrir comment le protocole NTLM est utilisé pour limiter de manière sélective le trafic NTLM. Pour plus d’informations sur la façon d’analyser et de limiter l’utilisation de NTLM dans vos environnements, voir [présentation de la Restriction de l’authentification NTLM](https://technet.microsoft.com/library/dd560653(v=ws.10).aspx) pour accéder à l’audit et la restriction de guide de l’utilisation NTLM.

## <a name="BKMK_NEW"></a>Fonctionnalités nouvelles et modifiées
Il n’existe aucune modification de fonctionnalité pour NTLM pour Windows Server2012.

## <a name="BKMK_DEP"></a>Fonctionnalité supprimée ou déconseillée
Il n’existe aucune fonctionnalité supprimée ou déconseillée pour NTLM pour Windows Server2012.

## <a name="BKMK_INSTALL"></a>Informations du Gestionnaire de serveur
NTLM ne peut pas être configuré à partir du Gestionnaire de serveur. Vous pouvez utiliser les paramètres de stratégie de sécurité ou des stratégies de groupe pour gérer l’utilisation de l’authentification NTLM entre des systèmes informatiques. Dans un domaine, Kerberos est le protocole d’authentification par défaut.

## <a name="BKMK_LINKS"></a>Voir aussi
Le tableau suivant répertorie les ressources appropriées pour NTLM et autres technologies d’authentification Windows.

|Type de contenu|Références|
|--------|-------|
|**Évaluation du produit**|[Présentation de la Restriction de l’authentification NTLM](https://technet.microsoft.com/library/dd560653.aspx)<br /><br />[Modifications apportées à l’authentification NTLM](https://technet.microsoft.com/library/dd566199.aspx)|
|**Planification**|[Guide de modélisation des menaces informatiques Infrastructure](https://technet.microsoft.com/library/dd941826.aspx)<br /><br />[Menaces et contre-mesures: paramètres de sécurité dans Windows Server2003 et WindowsXP](https://technet.microsoft.com/library/dd162275.aspx)<br /><br />[Guide des menaces et contre-mesures: paramètres de sécurité dans Windows Server2008 et WindowsVista](https://technet.microsoft.com/library/dd349791.aspx)<br /><br />[Guide des menaces et contre-mesures: paramètres de sécurité dans Windows Server2008R2 et Windows7](https://technet.microsoft.com/library/hh125921.aspx)|
|**Déploiement**|[Protection étendue pour l’authentification](https://support.microsoft.com/kb/968389)<br /><br />[L’audit et la restriction de guide de l’utilisation NTLM](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)<br /><br />[Demander à l’équipe de Services d’annuaire: blocage de NTLM et vous: méthodologies dans Windows7 d’audit et d’analyse](https://blogs.technet.com/askds/archive/2009/10/08/ntlm-blocking-and-you-application-analysis-and-auditing-methodologies-in-windows-7.aspx)<br /><br />[Blog de l’authentification Windows](https://blogs.technet.com/authentication/)<br /><br />[Configuration de MaxConcurrentAPI pour l’authentification via pass\ NTLM](https://social.technet.microsoft.com/wiki/contents/articles/9759.configuring-maxconcurrentapi-for-ntlm-pass-through-authentication.aspx)|
|**Développement**|[MicrosoftNTLM \(Windows\)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)<br /><br />[\[MS\-NLMP\]: spécification du protocole d’authentification NT LAN Manager \(NTLM\)](https://msdn.microsoft.com/library/cc236621(PROT.10).aspx)<br /><br />[\[MS\-NNTP\]: l’authentification NT LAN Manager \(NTLM\): Network News Transfer Protocol \(NNTP\) Extension](https://msdn.microsoft.com/library/cc236774(PROT.10).aspx)<br /><br />[\[MS\-NTHT\]: NTLM sur la spécification du protocole HTTP](https://msdn.microsoft.com/library/cc237488(PROT.10).aspx)|
|**Résolution des problèmes**|Pas encore disponible|
|**Ressources de la Communauté**|[Ce cheval n’est mort encore: les goulots d’étranglement NTLM et le runtime RPC](http://blogs.technet.com/b/askds/archive/2011/09/15/is-this-horse-dead-yet-ntlm-bottlenecks-and-the-rpc-runtime.aspx)|



