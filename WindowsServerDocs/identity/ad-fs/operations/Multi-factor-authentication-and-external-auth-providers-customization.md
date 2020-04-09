---
title: Personnalisation de Multi-Factor Authentication et des fournisseurs d’authentification externes
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.assetid: 08724d45-9be4-4c56-a5f1-2cf40864e136
ms.technology: identity-adfs
ms.openlocfilehash: 8252244738d59f11a07c3bebadbbf2a5f4818845
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816232"
---
# <a name="multi-factor-authentication-and-external-authentication-providers-customization"></a>Personnalisation de Multi-Factor Authentication et des fournisseurs d’authentification externes 



Dans AD FS, la prise en charge de l’authentification multifacteur est fournie\-de\-la boîte de\-. Par exemple, vous pouvez configurer AD FS pour utiliser des\-générés dans l’authentification par certificat comme authentification de second facteur. Vous pouvez également utiliser des fournisseurs d'authentification externes. Cette approche peut permettre l’intégration d’AD FS à des services supplémentaires, tels qu’Azure Multi-Factor Authentication, ou vous pouvez développer votre propre fournisseur. Pour plus d’informations sur l’inscription d’un fournisseur d’authentification externe à l’aide d’AD FS, consultez [Guide de solution : gérer les risques avec plusieurs\-Access Control](https://technet.microsoft.com/library/dn280937.aspx) .  
  
Il est recommandé qu’un fournisseur d’authentification externe utilise les classes définies dans le fichier. CSS que AD FS fournit pour créer l’interface utilisateur d’authentification. Vous pouvez utiliser l'applet de commande suivante pour exporter le thème web par défaut et inspecter les classes et les éléments d'interface utilisateur définis dans le fichier .css. Le fichier. CSS peut être utilisé dans le développement du\-de signe dans l’interface utilisateur d’un fournisseur d’authentification externe.  
  

    Export-AdfsWebTheme -Name default -DirectoryPath C:\theme  
 
  
Voici un exemple de signe\-dans l’interface utilisateur, mis en surbrillance en rouge, par un fournisseur d’authentification externe. L’interface utilisateur utilise les classes d’interface utilisateur dans le fichier AD FS. css.  
  
![AD FS et MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom8.png)  
  
Avant d’écrire une nouvelle méthode d’authentification personnalisée, nous vous recommandons d’étudier les définitions de thème et de style de AD FS pour comprendre les exigences de création de contenu.  
  
-   Une méthode d’authentification personnalisée crée uniquement un segment HTML sur le AD FS\-dans la page et non dans la page entière. Vous devez utiliser la définition de style de AD FS pour avoir une apparence et un comportement cohérents.  
  
![AD FS et MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom9.png)  
  
-   N’oubliez pas que AD FS administrateurs peuvent personnaliser les styles de AD FS. . Nous vous déconseillons de coder en dur vos propres styles. Au lieu de cela, nous vous recommandons d’utiliser AD FS styles chaque fois que cela est possible.  
  
-   En dehors de la\-de la boîte de\-, les styles de AD FS sont créés avec un\-de gauche pour\-\(\) RTL\-de droite\-.\(\) Les administrateurs peuvent personnaliser les deux types de langage et fournir des\-des styles spécifiques par le biais de la définition du thème Web. Chaque feuille de style possède trois sections avec des commentaires respectifs :  
  
    -   Les **styles de thème** \- ces styles ne doivent pas et ne peuvent pas être utilisés. Ils servent à définir un thème appliqué dans toutes les pages. Ils sont délibérément définis par un ID d'élément qui empêche leur réutilisation.  
  
    -   les **styles courants** \- ce sont les styles qui doivent être utilisés pour votre contenu.  
  
    -   Les **styles de facteur de forme** \- il s’agit de styles pour différents facteurs de forme. Vous devez comprendre cette section pour vous assurer que votre contenu fonctionne avec différents facteurs de forme, par exemple, les téléphones et les tablettes.  
  
Pour plus d’informations, consultez [Guide de solution : gérer les risques avec plusieurs\-facteurs Access Control](https://technet.microsoft.com/library/dn280937.aspx) et [Guide de solution : gérer les risques avec une authentification multifacteur\-supplémentaire pour les applications sensibles](https://tnstage.redmond.corp.microsoft.com/library/dn280949.aspx).  

## <a name="additional-references"></a>Références supplémentaires 
[Personnalisation de la connexion de l’utilisateur AD FS](AD-FS-user-sign-in-customization.md) 
