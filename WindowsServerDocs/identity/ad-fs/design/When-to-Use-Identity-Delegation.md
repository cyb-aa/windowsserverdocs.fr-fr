---
ms.assetid: 6e711a96-9055-4508-b6d4-318d6aa95fd1
title: Quand utiliser la délégation d’identité
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7a594332900c8b3afb95c139bcde8458a10f186b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858462"
---
# <a name="when-to-use-identity-delegation"></a>Quand utiliser la délégation d’identité
  
## <a name="what-is-identity-delegation"></a>Qu’est-ce que la délégation d’identité ?  
La délégation d’identité est une fonctionnalité de Services ADFS \(AD FS\) qui permet à l’administrateur\-comptes spécifiés d’emprunter l’identité des utilisateurs. Le compte qui emprunte l’identité de l’utilisateur est appelé le *délégué*. Cette fonctionnalité de délégation est essentielle pour de nombreuses applications distribuées pour lesquelles une série de vérifications de contrôle d’accès doit être effectuée de façon séquentielle pour chaque application, base de données ou service qui se trouve dans la chaîne d’autorisation de la demande d’origine. Il existe de nombreux scénarios réels\-dans lesquels une application Web doit récupérer les données à partir d’un « back end » plus sécurisé, tel qu’un service Web connecté à une base de données Microsoft SQL Server.  
  
Par exemple, il est possible d’améliorer par programme un site Web de commande de pièces\-existant pour permettre aux organisations partenaires d’afficher leur historique d’achat et l’état de leur compte. Pour des raisons de sécurité, toutes les données financières des partenaires sont stockées dans une base de données sécurisée sur un langage SQL \(SQL\) Server dédié. Dans ce cas, le code de l’application front\-end ne sait rien sur les données financières de l’organisation partenaire. Par conséquent, il doit récupérer ces données à partir d’un autre ordinateur situé ailleurs sur le réseau qui héberge \(dans ce cas\) le service Web pour la base de données des composants \(le back end\).  
  
Pour que ce processus de récupération de\-de données aboutisse, une succession d’autorisation « main\-agitation » doit avoir lieu entre l’application Web et le service Web pour la base de données de pièces, comme indiqué dans l’illustration suivante.  
  
![délégation d’identité](media/adfs2_identitydelegationconcept.gif)  
  
Étant donné que la demande d’origine a été effectuée sur le serveur web lui-même, qui est probablement situé dans une organisation complètement différente de celle de l’utilisateur qui tente d’accéder au serveur web, le jeton de sécurité qui est envoyé avec la demande ne répond pas aux critères d’autorisation requis pour accéder à un ordinateur, au-delà du serveur web. Par conséquent, la demande d’origine de l’utilisateur peut être satisfaite uniquement en plaçant un serveur de fédération intermédiaire dans l’organisation partenaire de ressource pour aider à émettre à nouveau un jeton de sécurité qui dispose des privilèges d’accès appropriés.  
  
## <a name="how-does-identity-delegation-work"></a>Fonctionnement de la délégation d’identité  
Les applications web des architectures d’application intermédiaires appellent souvent des services web pour accéder à des données ou des fonctionnalités communes. Il est important pour ces services web de connaître l’identité de l’utilisateur d’origine afin que le service puisse prendre des décisions d’autorisation et faciliter les audits. Dans ce cas, l’application Web front\-end représente l’utilisateur du service Web en tant que délégué. AD FS facilite ce scénario en permettant à Active Directory comptes d’agir en tant qu’utilisateur pour une autre partie de confiance. Un scénario de délégation d’identité est illustré ci-après.  
  
![délégation d’identité](media/adfs2_identitydelegationsteps.gif)  
  
1.  Frank tente d’accéder à la partie\-historique des commandes à partir d’une application Web dans une autre organisation. Son ordinateur client demande et reçoit un jeton de AD FS pour la partie\-final\-commander l’application Web.  
  
2.  L’ordinateur client envoie une demande à l’application Web, y compris le jeton obtenu à l’étape 1, pour prouver l’identité du client.  
  
3.  L’application web doit communiquer avec le service web pour terminer sa transaction pour le client. L’application Web contacte AD FS pour obtenir un jeton de délégation pour interagir avec le service Web. Les jetons de délégation sont des jetons de sécurité émis à un délégué de sorte que ce dernier puisse agir en tant qu’utilisateur. AD FS retourne un jeton de délégation avec des revendications relatives au client, ciblé pour le service Web.  
  
4.  L’application Web utilise le jeton obtenu à partir de AD FS à l’étape 3 pour accéder au service Web qui agit en tant que client. En examinant le jeton de délégation, le service web peut déterminer que l’application web agit en tant que client. Le service web exécute sa stratégie d’autorisation, enregistre la demande et fournit les données d’historique de pièces requises, initialement demandées par Frank à l’application web, donc à Frank.  
  
Pour un délégué particulier, AD FS pouvez limiter les services Web pour lesquels l’application Web peut demander un jeton de délégation. L’ordinateur client ne doit pas nécessairement posséder un compte Active Directory pour que cette opération réussisse. Enfin, comme indiqué précédemment, le service web peut déterminer facilement l’identité du délégué qui agit en tant qu’utilisateur. Cela permet aux services web de présenter un comportement différent selon qu’ils s’adressent à l’ordinateur client directement ou via un délégué.  
  
## <a name="configuring-ad-fs-for-identity-delegation"></a>Configuration d’AD FS pour la délégation d’identité  
Vous pouvez utiliser le\-du composant logiciel enfichable de gestion de AD FS dans pour configurer AD FS pour la délégation d’identité chaque fois que vous avez besoin de faciliter le processus de récupération des données. Une fois que vous l’avez configurée, AD FS pouvez générer de nouveaux jetons de sécurité qui incluent le contexte d’autorisation que le service final\-peut exiger avant de pouvoir fournir l’accès aux données protégées.  
  
AD FS ne limite pas les utilisateurs qui peuvent faire l’emprunt d’identité. Une fois que vous avez configuré AD FS pour la délégation d’identité, elle effectue les opérations suivantes :  
  
-   Il détermine à quels serveurs l’autorité de demande de jetons peut être déléguée pour emprunter l’identité d’un utilisateur.  
  
-   Il établit et garde séparés le contexte d’identité pour le compte du client délégué et le serveur qui agit en tant que délégué.  
  
Vous pouvez configurer la délégation d’identité en ajoutant des règles d’autorisation de délégation à une approbation de partie de confiance dans le\-du composant logiciel enfichable de gestion AD FS dans. Pour plus d’informations sur la procédure à suivre, consultez [Checklist: Creating Claim Rules for a Relying Party Trust](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Relying-Party-Trust.md).  
  
## <a name="configuring-the-front-end-web-application-for-identity-delegation"></a>Configuration de l’application Web frontale\-end pour la délégation d’identité  
Les développeurs disposent de plusieurs options qu’ils peuvent utiliser pour programmer correctement l’application ou le service Web front\-end pour rediriger les demandes de délégation vers un ordinateur AD FS. Pour plus d’informations sur la personnalisation d’une application web de sorte qu’elle fonctionne avec la délégation d’identité, consultez [Kit de développement logiciel (SDK) de Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
