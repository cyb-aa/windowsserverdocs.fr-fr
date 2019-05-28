---
title: L’authentification multifacteur et personnalisation des fournisseurs d’authentification externe
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 08724d45-9be4-4c56-a5f1-2cf40864e136
ms.technology: identity-adfs
ms.openlocfilehash: 347b4783e82a6561334f8757029b1fddec6a85a3
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189083"
---
# <a name="multi-factor-authentication-and-external-authentication-providers-customization"></a>L’authentification multifacteur et personnalisation des fournisseurs d’authentification externe 



Dans AD FS, la prise en charge pour l’authentification multifacteur est fournie\-de\-le\-boîte. Par exemple, vous pouvez configurer les services AD FS pour utiliser généré\-dans l’authentification par certificat comme authentification de second facteur. Vous pouvez également utiliser des fournisseurs d'authentification externes. Cette approche peut activer AD FS à intégrer à des services supplémentaires, telles que l’authentification multifacteur Azure, ou vous pouvez développer votre propre fournisseur. Consultez [Guide de Solution : Gérer les risques avec Multi\-factoriser le contrôle d’accès](https://technet.microsoft.com/library/dn280937.aspx) pour plus d’informations sur l’inscription du fournisseur d’authentification externe à l’aide d’AD FS.  
  
Nous recommandons d’utiliser les classes qui sont définis dans le fichier .css qui fournit des services AD FS pour l’authentification de l’interface utilisateur de créer un fournisseur d’authentification externe. Vous pouvez utiliser l'applet de commande suivante pour exporter le thème web par défaut et inspecter les classes et les éléments d'interface utilisateur définis dans le fichier .css. Le fichier .css peut être utilisé dans le développement de la connexion\-dans l’interface utilisateur d’un fournisseur d’authentification externe.  
  

    Export-AdfsWebTheme -Name default -DirectoryPath C:\theme  
 
  
Voici un exemple de la connexion\-dans l’interface utilisateur, qui est mis en surbrillance en rouge, par un fournisseur d’authentification externe. L’interface utilisateur utilise les classes de l’interface utilisateur dans le fichier .css AD FS.  
  
![AD FS et l’authentification Multifacteur](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom8.png)  
  
Avant d’écrire une méthode d’authentification personnalisée, nous vous recommandons de temps d’étudier les définitions de thème et de style AD FS pour comprendre les exigences de création de contenu.  
  
-   Une méthode d’authentification personnalisée crée uniquement un segment HTML sur la connexion AD FS\-dans la page et pas la page entière. Vous devez utiliser la définition de style d’AD FS pour obtenir une apparence cohérente et son comportement.  
  
![AD FS et l’authentification Multifacteur](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom9.png)  
  
-   N’oubliez pas que les administrateurs AD FS peuvent personnaliser les styles d’AD FS. . Nous vous déconseillons de coder en dur vos propres styles. Au lieu de cela, nous vous recommandons d’utiliser les styles d’AD FS autant que possible.  
  
-   Out\-de\-la zone, styles d’AD FS sont créés avec la gauche d’une\-à\-droit \(LTR\) style et la droite d’une\-à\-gauche \(RTL\). Les administrateurs peuvent personnaliser les deux et peut fournir de langage\-styles spécifiques à la définition de thème web. Chaque feuille de style possède trois sections avec des commentaires respectifs :  
  
    -   **styles de thème** \- ces styles ne doivent pas et ne peut pas être utilisés. Ils servent à définir un thème appliqué dans toutes les pages. Ils sont délibérément définis par un ID d'élément qui empêche leur réutilisation.  
  
    -   **styles courants** \- ce sont les styles qui doivent être utilisés pour votre contenu.  
  
    -   **styles de facteur de forme** \- il s’agit de styles pour différents facteurs de forme. Vous devez comprendre cette section pour vous assurer que votre contenu fonctionne avec différents facteurs de forme, par exemple, les téléphones et les tablettes.  
  
Pour plus d’informations, consultez [Guide de Solution : Gérer les risques avec Multi\-factoriser le contrôle d’accès](https://technet.microsoft.com/library/dn280937.aspx) et [Guide de Solution : Gérer les risques avec Multi supplémentaire\-facteur d’authentification pour les Applications sensibles](https://tnstage.redmond.corp.microsoft.com/library/dn280949.aspx).  

## <a name="additional-references"></a>Références supplémentaires 
[AD FS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md) 
