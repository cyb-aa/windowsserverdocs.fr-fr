---
ms.assetid: 460792e4-9f1d-4e7b-b6b2-53e057f839df
title: Considérations sur la topologie du déploiement d'AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 46692653ba10558a9236bd321127591bc7c8a275
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838380"
---
# <a name="using-ad-ds-claims-with-ad-fs"></a>Utilisation de revendications AD DS avec les services AD FS
  
>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
Vous pouvez activer le contrôle d’accès plus riche pour les applications fédérées à l’aide des Services de domaine Active Directory \(AD DS\)\-reçoit des revendications utilisateur et appareil avec Active Directory Federation Services \(AD FS \).  
  
## <a name="about-dynamic-access-control"></a>À propos du contrôle d’accès dynamique  
Dans Windows Server® 2012, la fonctionnalité de contrôle d’accès dynamique permet aux organisations d’accorder l’accès aux fichiers basé sur les revendications d’utilisateur \(qui sont généré par des attributs de compte utilisateur\) et revendications de périphérique \(qui sont généré par des attributs de compte d’ordinateur\) qui sont émis par les Services de domaine Active Directory \(AD DS\). AD DS, reçoit des revendications sont intégrés à l’authentification intégrée de Windows via le protocole d’authentification Kerberos.  
  
Pour plus d’informations sur le contrôle d’accès dynamique, consultez [dynamique plan du contenu contrôle accès](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP).  
  
### <a name="whats-new-in-ad-fs"></a>Nouveautés dans AD FS ?  
En tant qu’extension pour le scénario de contrôle d’accès dynamique, AD FS dans Windows Server 2012 peuvent désormais :  
  
-   Attributs de compte accès ordinateur en plus des attributs de compte d’utilisateur à partir de dans AD DS. Dans les versions précédentes d’AD FS, le Service de fédération pas pu accéder attributs de compte d’ordinateur du tout à partir d’AD DS.  
  
-   Consommer les services AD DS, reçoit des revendications utilisateur ou périphérique qui résident dans un ticket d’authentification Kerberos. Dans les versions précédentes d’AD FS, le moteur de revendications a été en mesure de lire l’ID de sécurité utilisateur et groupe \(SID\) de Kerberos, mais n’a pas pu lire les informations contenues dans un ticket Kerberos des revendications.  
  
-   Transformer les services AD DS émis utilisateur ou revendications de périphérique dans les jetons SAML qui applications par partie de confiance peuvent utiliser pour effectuer un contrôle d’accès plus riche.  
  
## <a name="benefits-of-using-ad-ds-claims-with-ad-fs"></a>Avantages de l’utilisation des services AD DS revendications avec AD FS  
Ces services AD DS reçoit des revendications peuvent être insérées dans les tickets d’authentification Kerberos et utilisés avec AD FS pour offrent les avantages suivants :  
  
-   Les organisations qui nécessitent des stratégies de contrôle d’accès plus riches peuvent activer des revendications\-d ' accès aux applications et ressources à l’aide des services AD DS reçoit des revendications qui sont basées sur les valeurs d’attribut stockées dans AD DS pour un compte d’utilisateur ou un ordinateur donné. Cela peut aider les administrateurs à réduire les frais supplémentaires associés à la création et la gestion :  
  
    -   Groupes de sécurité du service d’annuaire AD qui seraient utilisées sinon pour contrôler l’accès aux applications et ressources qui sont accessibles via l’authentification Windows intégrée.  
  
    -   Les approbations qui seraient utilisées sinon pour contrôler l’accès à l’entreprise de forêt\-à\-Business \(B2B\) \/ applications accessibles Internet et les ressources.  
  
-   Les organisations peuvent maintenant empêcher tout accès non autorisé aux ressources réseau à partir d’ordinateurs clients en fonction de la valeur stockée dans AD DS d’attribut si un compte d’ordinateur spécifique \(, par exemple, un nom d’ordinateur DNS\) correspond au contrôle d’accès stratégie de la ressource \(, par exemple, un serveur de fichiers a été ACLd avec les revendications\) ou la stratégie de confiance \(, par exemple, un revendications\-application Web prenant en charge\). Cela peut aider les administrateurs de définir des stratégies de contrôle d’accès plus précis des ressources ou des applications qui sont :  
  
    -   Accessible uniquement via l’authentification Windows intégrée.  
  
    -   Internet accessible par le biais de mécanismes d’authentification AD FS. AD FS peut être utilisée pour transformer les services AD DS a émis des revendications de périphérique dans les revendications AD FS qui peuvent être encapsulées dans des jetons SAML qui peuvent être consommés par une ressource accessible par Internet ou d’une application de confiance.  
  
## <a name="differences-between-ad-ds-and-ad-fs-issued-claims"></a>Différences entre les services AD DS et AD FS reçoit des revendications  
Il existe deux facteurs de différenciation sont importants à comprendre concernant les revendications émises à partir de vs d’AD DS. AD FS. Ces différences sont les suivantes :  
  
-   Les services AD DS peut uniquement émettre les revendications qui sont encapsulées dans les tickets Kerberos, pas les jetons SAML. Pour plus d’informations sur la façon dont les services AD DS émet des revendications, consultez [dynamique plan du contenu contrôle accès](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP).  
  
-   AD FS peut émettre uniquement des revendications qui sont encapsulées dans les jetons SAML, pas les tickets Kerberos. Pour plus d’informations sur la façon dont ADFS émet des revendications, consultez [The Role of the Claims Engine](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md).  
  
## <a name="how-ad-ds-issued-claims-work-with-ad-fs"></a>Fonctionnement des revendications émises par les services AD DS avec les services AD FS  
AD DS, reçoit des revendications peut être utilisés pour accéder aux revendications d’utilisateur et appareil directement à partir de l’utilisateur contexte d’authentification, plutôt que d’effectuer un appel LDAP séparé à Active Directory avec AD FS. L’illustration suivante et les étapes correspondantes traite du fonctionnement de ce processus plus en détail pour activer les revendications\-en fonction de contrôle d’accès pour le scénario de contrôle d’accès dynamique.  
  
![l’utilisation de revendications](media/UsingADDSClaimswithADFS.gif)  
  
1.  Un administrateur AD DS utilise la console de centre d’administration Active Directory ou des applets de commande PowerShell Active les objets de type de revendication spécifique dans le schéma AD DS.  
  
2.  Un administrateur AD FS utilise la console de gestion AD FS pour créer et configurer le fournisseur de revendications et de la partie de confiance approbations avec pass\-via ou règles de revendication de transformation.  
  
3.  Un client Windows tente d’accéder au réseau. Dans le cadre du processus d’authentification Kerberos, le client présente son ticket d’utilisateur et ordinateur\-accord de ticket \(TGT\) qui ne contient pas encore de toute réclamation, au contrôleur de domaine. Le contrôleur de domaine recherche dans les services AD DS pour les types de revendication est activée, puis inclut toutes les revendications qui en résulte dans le ticket Kerberos retourné.  
  
4.  Lorsque l’utilisateur\/client tente d’accéder à une ressource de fichier est ACLd pour exiger les revendications, ils peuvent accéder à la ressource, car l’ID composée qui a été exposé à partir de Kerberos a ces revendications.  
  
5.  Lorsque le même client tente d’accéder à une site Web ou une application Web qui est configurée pour l’authentification AD FS, l’utilisateur est redirigé vers un serveur de fédération AD FS est configuré pour l’authentification intégrée Windows. Le client envoie une demande au contrôleur de domaine à l’aide de Kerberos. Le contrôleur de domaine émet un ticket Kerberos contenant les revendications demandées dans lequel le client peut ensuite présenter au serveur de fédération.  
  
6.  Selon la façon dont les règles de revendications ont été configurés sur le fournisseur de revendications et approbations que l’administrateur configurée précédemment, AD FS lit les revendications dans le ticket Kerberos et les inclut dans un jeton SAML émis pour le client de partie de confiance.  
  
7.  Le client reçoit le jeton SAML contenant les revendications correctes et est ensuite redirigé vers le site Web.  
  
Pour plus d’informations sur la façon de créer les règles de revendication requis pour les services AD DS émis de revendications pour travailler avec AD FS, consultez [créer une règle pour transformer une revendication entrante](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
