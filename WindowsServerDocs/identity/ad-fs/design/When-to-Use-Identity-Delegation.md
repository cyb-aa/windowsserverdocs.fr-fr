---
ms.assetid: 6e711a96-9055-4508-b6d4-318d6aa95fd1
title: Quand utiliser la délégation d’identité
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2544001b871a1eda2c03005c384a99d5209e7282
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190553"
---
# <a name="when-to-use-identity-delegation"></a>Quand utiliser la délégation d’identité
  
## <a name="what-is-identity-delegation"></a>Qu’est-ce que la délégation d’identité ?  
Délégation d’identité est une fonctionnalité des Services de fédération Active Directory \(AD FS\) qui permet à administrateur\-spécifié des comptes pour emprunter l’identité des utilisateurs. Le compte qui emprunte l’identité de l’utilisateur est appelé le *délégué*. Cette fonctionnalité de délégation est essentielle pour de nombreuses applications distribuées pour lesquelles une série de vérifications de contrôle d’accès doit être effectuée de façon séquentielle pour chaque application, base de données ou service qui se trouve dans la chaîne d’autorisation de la demande d’origine. Nombre réel\-scénarios du monde existent dans lequel une application « frontal » Web doit récupérer des données à partir d’une plus sécurisée « principale », tel qu’un service Web qui est connecté à une base de données Microsoft SQL Server.  
  
Par exemple, des parties existantes\-classement du site Web peut être amélioré par programmation afin qu’elle autorise des partenaires aux organisations d’afficher leur propre état de l’historique et compte achat. Pour des raisons de sécurité, toutes les données financières des partenaires sont stockées dans une base de données sécurisé sur un langage de requête structuré dédié \(SQL\) server. Dans ce cas, le code devant\-fermer l’application ne sait rien sur les données financières de l’organisation partenaire. Par conséquent, il doit récupérer ces données à partir d’un autre ordinateur ailleurs sur le réseau qui héberge \(dans ce cas\) le service Web pour la base de données des parties \(le serveur principal\).  
  
Pour ces données\-des processus de récupération réussisse, une série de négociations d’autorisation « main\-secouant » doit intervenir entre l’application Web et le service Web pour la base de données de parties, comme indiqué dans l’illustration suivante.  
  
![délégation d’identité](media/adfs2_identitydelegationconcept.gif)  
  
Étant donné que la demande d’origine a été effectuée sur le serveur web lui-même, qui est probablement situé dans une organisation complètement différente de celle de l’utilisateur qui tente d’accéder au serveur web, le jeton de sécurité qui est envoyé avec la demande ne répond pas aux critères d’autorisation requis pour accéder à un ordinateur, au-delà du serveur web. Par conséquent, la demande d’origine de l’utilisateur peut être satisfaite uniquement en plaçant un serveur de fédération intermédiaire dans l’organisation partenaire de ressource pour aider à émettre à nouveau un jeton de sécurité qui dispose des privilèges d’accès appropriés.  
  
## <a name="how-does-identity-delegation-work"></a>Fonctionnement de la délégation d’identité  
Les applications web des architectures d’application intermédiaires appellent souvent des services web pour accéder à des données ou des fonctionnalités communes. Il est important pour ces services web de connaître l’identité de l’utilisateur d’origine afin que le service puisse prendre des décisions d’autorisation et faciliter les audits. Dans ce cas, le front\-fin application Web représente l’utilisateur au service Web en tant que délégué. AD FS facilite ce scénario en permettant aux comptes Active Directory d’agir en tant qu’utilisateur à une autre partie de confiance. Un scénario de délégation d’identité est illustré ci-après.  
  
![délégation d’identité](media/adfs2_identitydelegationsteps.gif)  
  
1.  Frank tente d’accéder à la partie\-le classement de l’historique à partir d’une application Web dans une autre organisation. Son ordinateur client demande et reçoit un jeton à partir d’AD FS pour l’avant\-fin partie\-ordre d’application Web.  
  
2.  L’ordinateur client envoie une demande à l’application web, incluant le jeton obtenu à l’étape 1, pour prouver l’identité du client.  
  
3.  L’application web doit communiquer avec le service web pour terminer sa transaction pour le client. L’application Web contacte AD FS pour obtenir un jeton de délégation pour interagir avec le service Web. Les jetons de délégation sont des jetons de sécurité émis à un délégué de sorte que ce dernier puisse agir en tant qu’utilisateur. AD FS renvoie un jeton de délégation avec des revendications relatives au client, ciblé pour le service Web.  
  
4.  L’application Web utilise le jeton qui a été obtenu à partir d’AD FS à l’étape 3 pour accéder au service Web qui agit en tant que le client. En examinant le jeton de délégation, le service web peut déterminer que l’application web agit en tant que client. Le service web exécute sa stratégie d’autorisation, enregistre la demande et fournit les données d’historique de pièces requises, initialement demandées par Frank à l’application web, donc à Frank.  
  
Pour un délégué particulier, AD FS peut limiter les services Web pour lequel l’application Web peut demander un jeton de délégation. L’ordinateur client ne doit pas nécessairement posséder un compte Active Directory pour que cette opération réussisse. Enfin, comme indiqué précédemment, le service web peut déterminer facilement l’identité du délégué qui agit en tant qu’utilisateur. Cela permet aux services web de présenter un comportement différent selon qu’ils s’adressent à l’ordinateur client directement ou via un délégué.  
  
## <a name="configuring-ad-fs-for-identity-delegation"></a>Configuration d’AD FS pour la délégation d’identité  
Vous pouvez utiliser le composant logiciel enfichable Gestion AD FS\-pour configurer AD FS pour la délégation d’identité à chaque fois que vous avez besoin faciliter le processus de récupération de données. Une fois que vous configurez, AD FS peut générer de nouveaux jetons de sécurité qui incluent le contexte d’autorisation qui l’arrière\-service principal peut nécessiter avant de fournir l’accès aux données protégées.  
  
AD FS ne limite pas les utilisateurs qui peuvent être empruntées. Après avoir configuré les services AD FS pour la délégation d’identité, il effectue les opérations suivantes :  
  
-   Il détermine à quels serveurs l’autorité de demande de jetons peut être déléguée pour emprunter l’identité d’un utilisateur.  
  
-   Il établit et garde séparés le contexte d’identité pour le compte du client délégué et le serveur qui agit en tant que délégué.  
  
Vous pouvez configurer la délégation d’identité en ajoutant des règles d’autorisation de délégation à une partie de confiance approuver dans le composant logiciel enfichable Gestion AD FS\-dans. Pour plus d’informations sur la procédure à suivre, consultez [liste de vérification : Création de règles de revendication pour une partie de confiance](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Relying-Party-Trust.md).  
  
## <a name="configuring-the-front-end-web-application-for-identity-delegation"></a>Configuration de l’avant\-terminer l’application Web pour la délégation d’identité  
Les développeurs disposent de plusieurs options qu’ils peuvent utiliser pour programmer correctement le serveur Web frontal\-fin application ou service pour rediriger les demandes de délégation sur un ordinateur AD FS. Pour plus d’informations sur la personnalisation d’une application web de sorte qu’elle fonctionne avec la délégation d’identité, consultez [Kit de développement logiciel (SDK) de Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
