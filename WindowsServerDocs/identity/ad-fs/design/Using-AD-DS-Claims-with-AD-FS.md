---
ms.assetid: 460792e4-9f1d-4e7b-b6b2-53e057f839df
title: Considérations sur la topologie du déploiement d'AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 881cdc02d06ce5afd3c0706f9c1ea5fa2576799f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358918"
---
# <a name="using-ad-ds-claims-with-ad-fs"></a>Utilisation de revendications AD DS avec les services AD FS
  
  
Vous pouvez activer le contrôle d’accès plus riche pour les applications fédérées\)à l’aide de Active Directory Domain Services \(AD DS\-revendications d' \(utilisateur et de périphérique émises avec services ADFS AD FS \).  
  
## <a name="about-dynamic-access-control"></a>À propos des Access Control dynamiques  
Dans Windows Server® 2012, la fonctionnalité de Access Control dynamique permet aux organisations d’accorder l’accès aux fichiers en \(fonction des revendications d’utilisateur qui sont basées\) sur les attributs \(de compte d’utilisateur et les revendications de périphérique qui sont émises par les attributs\) de compte d’ordinateur émis par \(Active Directory Domain Services\)AD DS. AD DS revendications émises sont intégrées à l’authentification intégrée de Windows par le biais du protocole d’authentification Kerberos.  
  
Pour plus d’informations sur les Access Control dynamiques, consultez feuille de route pour le [contenu dynamique Access Control](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP).  
  
### <a name="whats-new-in-ad-fs"></a>Quelles sont les nouveautés de AD FS ?  
En tant qu’extension du scénario de Access Control dynamique, AD FS dans Windows Server 2012 peut désormais :  
  
-   Accédez aux attributs de compte d’ordinateur en plus des attributs de compte d’utilisateur dans AD DS. Dans les versions précédentes de AD FS, le service FS (Federation Service) n’a pas pu accéder aux attributs de compte d’ordinateur à partir de AD DS.  
  
-   Consomme AD DS revendications d’utilisateur ou de périphérique émises qui résident dans un ticket d’authentification Kerberos. Dans les versions précédentes de AD FS, le moteur de revendications était en mesure de lire les SID \(\) des ID de sécurité d’utilisateur et de groupe à partir de Kerberos, mais n’a pas pu lire les informations de revendications contenues dans un ticket Kerberos.  
  
-   Transformez AD DS revendications d’utilisateur ou d’appareil émises en jetons SAML que les applications qui utilisent peuvent utiliser pour effectuer un contrôle d’accès plus riche.  
  
## <a name="benefits-of-using-ad-ds-claims-with-ad-fs"></a>Avantages de l’utilisation de AD DS revendications avec AD FS  
Ces AD DS des revendications émises peuvent être insérées dans des tickets d’authentification Kerberos et utilisées avec AD FS pour offrir les avantages suivants :  
  
-   Les organisations qui requièrent des stratégies de contrôle d’accès\-plus riches peuvent activer l’accès basé sur les revendications aux applications et aux ressources à l’aide de AD DS revendications émises basées sur les valeurs d’attribut stockées dans AD DS pour un compte d’utilisateur ou d’ordinateur donné. Cela peut aider les administrateurs à réduire la surcharge supplémentaire associée à la création et à la gestion des éléments suivants :  
  
    -   AD DS les groupes de sécurité qui autrement seraient utilisés pour contrôler l’accès aux applications et aux ressources accessibles via l’authentification intégrée de Windows.  
  
    -   Les approbations de forêt qui, autrement, seraient utilisées pour contrôler\-l’accès aux\) applications et aux ressources de l’entreprise\- \(B2B \/ accessibles via Internet.  
  
-   Les organisations peuvent désormais empêcher tout accès non autorisé aux ressources réseau à partir d’ordinateurs clients selon qu’une valeur d’attribut de compte d' \(ordinateur spécifique stockée dans AD DS par exemple\) , le nom DNS d’un ordinateur correspond au contrôle d’accès. stratégie de la ressource \(, par exemple, un serveur de fichiers qui a été ACLd\) avec des revendications ou la stratégie \(de partie de confiance,\-par exemple,\)une application Web prenant en charge les revendications. Cela peut aider les administrateurs à définir des stratégies de contrôle d’accès plus fines pour les ressources ou les applications suivantes :  
  
    -   Accessible uniquement via l’authentification intégrée de Windows.  
  
    -   Accessible via Internet via des mécanismes d’authentification AD FS. AD FS peut être utilisé pour transformer AD DS revendications de périphérique émises en AD FS revendications qui peuvent être encapsulées dans des jetons SAML qui peuvent être consommées par une ressource accessible sur Internet ou une application de partie de confiance.  
  
## <a name="differences-between-ad-ds-and-ad-fs-issued-claims"></a>Différences entre AD DS et AD FS revendications émises  
Il existe deux facteurs de différenciation qui sont importants pour comprendre les revendications émises à partir de AD DS et AD FS. Ces différences sont les suivantes :  
  
-   AD DS pouvez émettre uniquement des revendications encapsulées dans des tickets Kerberos, et non des jetons SAML. Pour plus d’informations sur la façon dont AD DS émet des revendications, consultez feuille de route pour le [contenu dynamique Access Control](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP).  
  
-   AD FS pouvez émettre uniquement des revendications encapsulées dans des jetons SAML, et non des tickets Kerberos. Pour plus d’informations sur la façon dont AD FS émet des revendications, consultez [le rôle du moteur de revendications](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md).  
  
## <a name="how-ad-ds-issued-claims-work-with-ad-fs"></a>Fonctionnement des revendications émises par les services AD DS avec les services AD FS  
AD DS revendications émises peuvent être utilisées avec AD FS pour accéder aux revendications d’utilisateur et d’appareil directement à partir du contexte d’authentification de l’utilisateur, plutôt que d’effectuer un appel LDAP distinct à Active Directory. L’illustration suivante et les étapes correspondantes décrivent le fonctionnement de ce processus plus en détail\-pour activer le contrôle d’accès basé sur les revendications pour le scénario de Access Control dynamique.  
  
![utilisation des revendications](media/UsingADDSClaimswithADFS.gif)  
  
1.  Un administrateur AD DS utilise la console Centre d’administration Active Directory ou des applets de commande PowerShell pour activer des objets de type de revendication spécifiques dans le schéma AD DS.  
  
2.  Un administrateur AD FS utilise la console de gestion de AD FS pour créer et configurer le fournisseur de revendications et les approbations de partie\-de confiance avec les règles de réussite ou de revendication.  
  
3.  Un client Windows tente d’accéder au réseau. Dans le cadre du processus d’authentification Kerberos, le client présente son ticket d'\- \(accord\) de ticket d’utilisateur et d’ordinateur qui ne contient pas encore de revendications au contrôleur de domaine. Le contrôleur de domaine recherche ensuite dans AD DS pour les types de revendication activés, et comprend toutes les revendications résultantes dans le ticket Kerberos retourné.  
  
4.  Lorsque le client\/utilisateur tente d’accéder à une ressource de fichier qui est ACLd pour exiger les revendications, il peut accéder à la ressource parce que l’ID composé qui a été exposé à partir de Kerberos a ces revendications.  
  
5.  Lorsque le même client tente d’accéder à un site Web ou une application Web configurée pour l’authentification AD FS, l’utilisateur est redirigé vers un serveur de fédération AD FS configuré pour l’authentification intégrée de Windows. Le client envoie une demande au contrôleur de domaine à l’aide de Kerberos. Le contrôleur de domaine émet un ticket Kerberos contenant les revendications demandées que le client peut ensuite présenter au serveur de Fédération.  
  
6.  En fonction de la façon dont les règles de revendication ont été configurées sur le fournisseur de revendications et les approbations de partie de confiance que l’administrateur a configurées précédemment, AD FS lit les revendications à partir du ticket Kerberos et les comprend dans un jeton SAML qu’il émet pour le client.  
  
7.  Le client reçoit le jeton SAML contenant les revendications correctes et est ensuite redirigé vers le site Web.  
  
Pour plus d’informations sur la création des règles de revendication requises pour AD DS les revendications émises pour fonctionner avec AD FS, consultez [créer une règle pour transformer une revendication entrante](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
