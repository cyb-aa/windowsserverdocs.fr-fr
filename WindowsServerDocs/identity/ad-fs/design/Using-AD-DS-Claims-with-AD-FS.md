---
ms.assetid: 460792e4-9f1d-4e7b-b6b2-53e057f839df
title: "Considérations sur la topologie déploiement ADFS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 46692653ba10558a9236bd321127591bc7c8a275
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="using-ad-ds-claims-with-ad-fs"></a>À l’aide des services AD DS des revendications avec AD FS
  
>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012
  
Vous pouvez activer le contrôle d’accès plus riche pour les applications fédérées à l’aide des Services de domaine Active Directory \(AD DS\)\-issued revendications d’utilisateur et périphérique ainsi que les Services de fédération Active Directory \(AD FS\).  
  
## <a name="about-dynamic-access-control"></a>Sur le contrôle d’accès dynamique  
Dans Windows Server® 2012, la fonctionnalité de contrôle d’accès dynamique permet aux organisations d’accorder l’accès aux fichiers en fonction des revendications d’utilisateur \ (qui sont généré par attributes\ de compte d’utilisateur) et les revendications de périphérique \ (qui sont généré par attributes\ de compte d’ordinateur) qui sont émis par les Services de domaine Active Directory \(AD DS\). Les services AD DS reçoit des revendications sont intégrés à l’authentification intégrée Windows via le protocole d’authentification Kerberos.  
  
Pour plus d’informations sur le contrôle d’accès dynamique, voir [dynamique contrôle contenu feuille de route accès](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP).  
  
### <a name="whats-new-in-ad-fs"></a>Quelles sont les nouveautés dans AD FS?  
En tant qu’extension pour le scénario de contrôle d’accès dynamique, AD FS dans Windows Server 2012 peuvent désormais:  
  
-   Attributs de compte accès ordinateur en plus des attributs de compte d’utilisateur de dans les services AD DS. Dans les versions antérieures d’AD FS, le Service de fédération pas pu accéder attributs de compte d’ordinateur du tout à partir des services AD DS.  
  
-   Utiliser les services AD DS, reçoit des revendications d’utilisateur ou un périphérique qui se trouvent dans un ticket d’authentification Kerberos. Dans les versions antérieures d’AD FS, le moteur de revendications a été en mesure de lire l’utilisateur et groupe sécurité ID \(SIDs\) Kerberos, mais n’a pas pu lire les informations contenues dans un ticket Kerberos des revendications.  
  
-   Transformer les services AD DS émis utilisateur ou des revendications de périphérique en jetons SAML applications partie de confiance peuvent utiliser pour effectuer le contrôle d’accès plus riche.  
  
## <a name="benefits-of-using-ad-ds-claims-with-ad-fs"></a>Avantages de l’utilisation des services AD DS revendications avec AD FS  
Ces services AD DS reçoit des revendications peuvent être insérées dans les tickets de l’authentification Kerberos et utilisés avec AD FS pour fournir les avantages suivants:  
  
-   Les organisations qui nécessitent des stratégies de contrôle d’accès plus riches peuvent activer claims\ accès aux applications et ressources à l’aide des services AD DS reçoit des revendications qui sont basées sur les valeurs d’attribut stockées dans AD DS pour un utilisateur donné ou un compte d’ordinateur. Cela peut aider les administrateurs à réduire la surcharge supplémentaire associée à la création et la gestion:  
  
    -   Groupes de sécurité du service d’annuaire Active Directory qui seraient sinon utilisées pour contrôler l’accès aux applications et ressources qui sont accessibles via l’authentification intégrée de Windows.  
  
    -   Forêt d’approbation qui serait sinon utilisées pour contrôler l’accès à celle-ci Business\-Business \(B2B\) \ / applications accessibles Internet et les ressources.  
  
-   Les organisations peuvent maintenant empêcher tout accès non autorisé aux ressources réseau des ordinateurs clients basés sur stockées dans AD DS de valeur d’attribut si un compte d’ordinateur spécifique \ (par exemple, d’un ordinateur DNS nom\) correspond à la stratégie de contrôle d’accès de la ressource \ (par exemple, un serveur de fichiers qui a été ACLd avec claims\) ou la stratégie de partie de confiance \ (par exemple, un prenant en charge claims\ Web appel à leurs). Cela peut aider les administrateurs de définir des stratégies de contrôle d’accès plus précis des ressources ou des applications qui sont:  
  
    -   Accessible uniquement via l’authentification intégrée de Windows.  
  
    -   Internet accessible via des mécanismes d’authentification AD FS. AD FS peut être utilisée pour transformer les services AD DS reçoit des revendications de périphérique dans les revendications AD FS qui peuvent être encapsulées dans des jetons SAML qui peuvent être consommées par une ressource accessible par Internet ou par une application tierce partie de confiance.  
  
## <a name="differences-between-ad-ds-and-ad-fs-issued-claims"></a>Différences entre les services AD DS et AD FS reçoit des revendications  
Il existe deux facteurs de différenciation qui sont importantes pour comprendre les revendications qui sont publiées à partir des services AD DS vs. AD FS. Ces différences sont les suivantes:  
  
-   Les services AD DS peuvent émettre uniquement des revendications qui sont encapsulées dans les tickets Kerberos, pas les jetons SAML. Pour plus d’informations sur la façon dont les services AD DS émet des revendications, voir [dynamique contrôle contenu feuille de route accès](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP).  
  
-   AD FS peut émettre uniquement des revendications encapsulées dans des jetons SAML, pas les tickets Kerberos. Pour plus d’informations sur comment problèmes les revendications AD FS, voir [The Role of the Claims Engine](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md).  
  
## <a name="how-ad-ds-issued-claims-work-with-ad-fs"></a>Comment les services AD DS des revendications émises avec AD FS  
Les services AD DS reçoit des revendications utilisable avec AD FS pour accéder aux revendications d’utilisateur et périphérique directement à partir de l’utilisateur contexte d’authentification, plutôt que d’effectuer un appel LDAP séparé à Active Directory. L’illustration suivante et les étapes correspondantes traite du fonctionnement de ce processus plus en détail pour activer le contrôle d’accès basé sur claims\ pour le scénario de contrôle d’accès dynamique.  
  
![l’utilisation de revendications](media/UsingADDSClaimswithADFS.gif)  
  
1.  Un administrateur de domaine Active Directory utilise la console du centre d’administration Active Directory ou les applets de commande PowerShell pour des objets de type de revendication spécifique permet dans le schéma AD DS.  
  
2.  Un administrateur AD FS utilise la console de gestion AD FS pour créer et configurer le fournisseur de revendications et de la partie de confiance avec une des approbations via pass\ ou règles de revendication de transformation.  
  
3.  Un client Windows tente d’accéder au réseau. Dans le cadre du processus d’authentification Kerberos, le client présente son utilisateur et ordinateur ticket\-granting ticket \(TGT\) pas encore contiennent les revendications, au contrôleur de domaine. Le contrôleur de domaine puis recherche dans les services AD DS pour les types de revendication activé et inclut toutes les revendications qui en résulte dans le ticket Kerberos retourné.  
  
4.  Lorsque le par l’utilisateur/client tente d’accéder à une ressource de fichier est ACLd pour exiger les revendications, ils peuvent accéder à la ressource, car l’ID composée qui a été exposé à partir de Kerberos a ces revendications.  
  
5.  Lorsque le même client tente d’accéder à une site Web ou une application Web qui est configurée pour l’authentification AD FS, l’utilisateur est redirigé vers un serveur de fédération AD FS qui est configuré pour l’authentification Windows intégrée. Le client envoie une demande au contrôleur de domaine à l’aide de Kerberos. Le contrôleur de domaine émet un ticket Kerberos contenant les revendications demandées auquel le client peut présenter ensuite au serveur de fédération.  
  
6.  Selon la manière dont les règles de revendications ont été configurés sur le fournisseur de revendications et approbations que l’administrateur configuré précédemment, AD FS lit les revendications dans le ticket Kerberos et les inclut un jeton SAML qui qu’elle émet pour le client de partie de confiance.  
  
7.  Le client reçoit le jeton SAML contenant les revendications correctes et est alors redirigé vers le site Web.  
  
Pour plus d’informations sur comment créer les règles de revendication requis pour les services AD DS émis de revendications pour travailler avec AD FS, voir [créer une règle pour transformer une revendication entrante](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
