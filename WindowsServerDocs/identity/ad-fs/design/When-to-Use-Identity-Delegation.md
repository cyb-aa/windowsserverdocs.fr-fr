---
ms.assetid: 6e711a96-9055-4508-b6d4-318d6aa95fd1
title: "Quand utiliser la délégation d’identité"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: af227d9e87ddb73f194dd46c8ce45fcdf12a34cf
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="when-to-use-identity-delegation"></a>Quand utiliser la délégation d’identité

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012
  
## <a name="what-is-identity-delegation"></a>Quelle est la délégation d’identité?  
Délégation d’identité est une fonctionnalité d’Active Directory Federation Services \(AD FS\) qui permet de comptes spécifiés administrator\ emprunter l’identité des utilisateurs. Le compte qui emprunte l’identité de l’utilisateur est appelé le *déléguer*. Cette fonctionnalité de délégation est essentielle pour de nombreuses applications distribuées pour lesquels il existe une série d’accès des vérifications de contrôle doivent être effectuées de façon séquentielle pour chaque application, une base de données ou un service qui se trouve dans la chaîne d’autorisation pour la demande d’origine. Nombreux scénarios réels real\ existent dans lequel un application «frontal» Web doit récupérer des données à partir d’un principal plus sécurisé «précédent», par exemple, un service Web qui est connecté à une base de données Microsoft SQL Server.  
  
Par exemple, un site Web de commande de pièces parts\ existant peut être amélioré par programme afin qu’elle autorise partenaire aux organisations d’afficher leur propre compte et l’historique de l’état achat. Pour des raisons de sécurité, toutes les données financières des partenaires sont stockées dans une base de données sécurisé sur un serveur \(SQL\) Structured Query Language dédié. Dans ce cas, le code de l’application de bout en bout front\ ne sait rien sur les données financières de l’organisation partenaire. Par conséquent, il doit récupérer ces données à partir d’un autre ordinateur sur le réseau qui héberge \ (dans ce respectent) le service Web pour la base de données de pièces \(the back end\).  
  
Pour ce processus de récupération data\ réussisse, certains suite de l’autorisation «hand\-fait de secouer» doit prendre placer entre l’application Web et le service Web pour la base de données de pièces, comme illustré dans l’illustration suivante.  
  
![délégation d’identité](media/adfs2_identitydelegationconcept.gif)  
  
Étant donné que la demande d’origine a été effectuée sur le serveur Web lui-même, qui est probablement situé dans une organisation complètement différente de l’organisation de l’utilisateur qui tente d’accéder au serveur Web, le jeton de sécurité qui est envoyé avec la demande ne respecte pas les critères d’autorisation requis pour accéder à n’importe quel autre ordinateur au-delà du serveur Web. Par conséquent, la seule façon que la demande utilisateur d’origine peut être satisfaite est en plaçant un serveur de fédération intermédiaire dans l’organisation partenaire de ressource pour aider à émettre un jeton de sécurité qui possède les droits d’accès appropriés à.  
  
## <a name="how-does-identity-delegation-work"></a>Comment fonctionne la délégation d’identité?  
Les applications Web dans des architectures d’application appellent souvent des services Web pour accéder aux données ou fonctionnalités communes. Il est important pour ces services Web de connaître l’identité de l’utilisateur d’origine afin que le service peut prendre des décisions d’autorisation et faciliter les audits. Dans ce cas, l’application Web de bout en bout front\ représente l’utilisateur au service Web en tant que délégué. AD FS facilite ce scénario en autorisant les comptes Active Directory d’agir en tant qu’utilisateur à une autre partie de confiance. Un scénario de délégation d’identité est illustré dans l’illustration suivante.  
  
![délégation d’identité](media/adfs2_identitydelegationsteps.gif)  
  
1.  Frank tente d’accéder à l’historique de commande de pièces part\ à partir d’une application Web dans une autre organisation. Son ordinateur client demande et reçoit un jeton d’AD FS pour l’application Web de bout en bout front\ part\ ordre.  
  
2.  L’ordinateur client envoie une demande à l’application Web, incluant le jeton obtenu à l’étape 1, pour prouver l’identité du client.  
  
3.  L’application Web doit communiquer avec le service Web pour terminer sa transaction pour le client. L’application Web contacte AD FS pour obtenir un jeton de délégation pour interagir avec le service Web. Les jetons de délégation sont des jetons de sécurité émis à un délégué d’agir en tant qu’utilisateur. AD FS retourne un jeton de délégation avec des revendications relatives au client, ciblé pour le service Web.  
  
4.  L’application Web utilise le jeton obtenu à partir d’AD FS à l’étape 3 pour accéder au service Web qui agit en tant que le client. Examiner le jeton de délégation, le service Web peut déterminer que l’application Web agit en tant que le client. Le service Web exécute sa stratégie d’autorisation, enregistre la demande et fournit des données d’historique a été initialement demandées par Frank à l’application Web les parties nécessaires et donc à Frank.  
  
Pour un délégué particulier, AD FS peut limiter les services Web pour lesquels l’application Web peut demander un jeton de délégation. L’ordinateur client ne doit pas nécessairement posséder un compte Active Directory pour cette opération réussisse. Enfin, comme indiqué précédemment, le service Web peut déterminer facilement l’identité du délégué qui agit en tant que l’utilisateur. Ainsi, les services Web présenter un comportement différent selon qu’ils s’adressent directement à l’ordinateur client ou via un délégué.  
  
## <a name="configuring-ad-fs-for-identity-delegation"></a>La configuration AD FS pour la délégation d’identité  
Vous pouvez utiliser la gestion AD FS de composants pour configurer AD FS pour la délégation d’identité à chaque fois que vous avez besoin faciliter le processus de récupération de données. Une fois que vous configurez, AD FS peut générer de nouveaux jetons de sécurité qui incluent le contexte d’autorisation que le service de bout en bout back\ peut exiger avant de fournir l’accès aux données protégées.  
  
AD FS ne limite pas les utilisateurs qui peuvent être empruntées. Après avoir configuré les services AD FS pour la délégation d’identité, il effectue les opérations suivantes:  
  
-   Elle détermine quels serveurs peuvent être déléguées l’autorité de demande de jetons pour emprunter l’identité d’un utilisateur.  
  
-   Il établit et garde séparés le contexte d’identité pour le compte client qui est délégué et le serveur qui agit en tant que délégué.  
  
Vous pouvez configurer la délégation d’identité en ajoutant des règles d’autorisation de délégation pour une approbation de partie de confiance dans la gestion AD FS de composants. Pour plus d’informations sur la procédure à suivre, voir [liste de vérification: Creating Claim Rules for a Relying Party Trust](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Relying-Party-Trust.md).  
  
## <a name="configuring-the-front-end-web-application-for-identity-delegation"></a>Configuration de l’application Web de bout en bout front\ pour la délégation d’identité  
Les développeurs ont plusieurs options qu’ils peuvent utiliser pour programmer correctement l’application de bout en bout front\ Web ou le service pour rediriger les demandes de délégation à un ordinateur d’AD FS. Pour plus d’informations sur la personnalisation d’une application Web fonctionne avec la délégation d’identité, consultez le [SDK de Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
