---
title: Personnalisation de Multi-Factor Authentication et des fournisseurs d’authentification externes
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 08724d45-9be4-4c56-a5f1-2cf40864e136
ms.technology: identity-adfs
ms.openlocfilehash: 23ec5acbe442527b4eb44c4b857e183b5e0c37ea
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865721"
---
# <a name="multi-factor-authentication-and-external-authentication-providers-customization"></a>Personnalisation de Multi-Factor Authentication et des fournisseurs d’authentification externes 



Dans AD FS, la prise en charge de l’authentification multifacteur est\-prête\-à\-l’emploi. Par exemple, vous pouvez configurer AD FS pour utiliser l'\-authentification par certificat intégrée comme authentification de second facteur. Vous pouvez également utiliser des fournisseurs d'authentification externes. Cette approche peut permettre l’intégration d’AD FS à des services supplémentaires, tels qu’Azure Multi-Factor Authentication, ou vous pouvez développer votre propre fournisseur. Consultez [le Guide de solution : Gérer les risques avec\-multifacteur](https://technet.microsoft.com/library/dn280937.aspx) Access Control pour plus d’informations sur l’inscription d’un fournisseur d’authentification externe à l’aide de AD FS.  
  
Il est recommandé qu’un fournisseur d’authentification externe utilise les classes définies dans le fichier. CSS que AD FS fournit pour créer l’interface utilisateur d’authentification. Vous pouvez utiliser l'applet de commande suivante pour exporter le thème web par défaut et inspecter les classes et les éléments d'interface utilisateur définis dans le fichier .css. Le fichier. CSS peut être utilisé dans le développement de l’interface\-utilisateur de connexion d’un fournisseur d’authentification externe.  
  

    Export-AdfsWebTheme -Name default -DirectoryPath C:\theme  
 
  
Voici un exemple de l’interface utilisateur de\-connexion, mise en surbrillance en rouge, par un fournisseur d’authentification externe. L’interface utilisateur utilise les classes d’interface utilisateur dans le fichier AD FS. css.  
  
![AD FS et MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom8.png)  
  
Avant d’écrire une nouvelle méthode d’authentification personnalisée, nous vous recommandons d’étudier les définitions de thème et de style de AD FS pour comprendre les exigences de création de contenu.  
  
-   Une méthode d’authentification personnalisée crée uniquement un segment html sur la page\-de connexion AD FS et non sur la page complète. Vous devez utiliser la définition de style de AD FS pour avoir une apparence et un comportement cohérents.  
  
![AD FS et MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom9.png)  
  
-   N’oubliez pas que AD FS administrateurs peuvent personnaliser les styles de AD FS. . Nous vous déconseillons de coder en dur vos propres styles. Au lieu de cela, nous vous recommandons d’utiliser AD FS styles chaque fois que cela est possible.  
  
-   \-\- \(\-\-\- \(AD FS styles prêts àl’emploi\) sont créés avec un style de gauche à droite et un style de droite à gauche (RTL)\-\). Les administrateurs peuvent personnaliser les deux et peuvent fournir\-des styles spécifiques à la langue par le biais de la définition du thème Web. Chaque feuille de style possède trois sections avec des commentaires respectifs :  
  
    -   **styles de thème** \- Ces styles ne doivent pas et ne peuvent pas être utilisés. Ils servent à définir un thème appliqué dans toutes les pages. Ils sont délibérément définis par un ID d'élément qui empêche leur réutilisation.  
  
    -   **styles courants** \- Ce sont les styles qui doivent être utilisés pour votre contenu.  
  
    -   **styles de facteur de forme** \- Il s’agit de styles pour différents facteurs de forme. Vous devez comprendre cette section pour vous assurer que votre contenu fonctionne avec différents facteurs de forme, par exemple, les téléphones et les tablettes.  
  
Pour plus d’informations, [consultez Guide de solution : Gérer les risques avec\-Multi Factor](https://technet.microsoft.com/library/dn280937.aspx) Access Control [et guide de solution : Gérer les risques avec une\-authentification multifacteur supplémentaire pour](https://tnstage.redmond.corp.microsoft.com/library/dn280949.aspx)les applications sensibles.  

## <a name="additional-references"></a>Références supplémentaires 
[Personnalisation de la connexion de l’utilisateur AD FS](AD-FS-user-sign-in-customization.md) 
