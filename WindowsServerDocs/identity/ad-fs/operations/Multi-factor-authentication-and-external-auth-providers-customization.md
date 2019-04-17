---
title: "L’authentification multifacteur et la personnalisation des fournisseurs d’authentification externe"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 08724d45-9be4-4c56-a5f1-2cf40864e136
ms.technology: identity-adfs
ms.openlocfilehash: 6d06c017601003e3b93df32f5fa50190ce54541d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="multi-factor-authentication-and-external-authentication-providers-customization"></a>L’authentification multifacteur et la personnalisation des fournisseurs d’authentification externe 

>S’applique à: Windows Server2016, Windows Server2012R2

Dans AD FS, la prise en charge pour l’authentification multifacteur est fourni à out-maximum\-programme-box. Par exemple, vous pouvez configurer AD FS pour utiliser l’authentification du certificat intégrée comme authentification de second facteur. Vous pouvez également utiliser des fournisseurs d’authentification externe. Cette approche peut activer AD FS intégrer des services supplémentaires, telles que l’authentification multifacteur Azure, ou vous pouvez développer votre propre fournisseur. Voir [Guide de Solution: gérer les risques avec contrôle d’accès prennent facteurs](https://technet.microsoft.com/library/dn280937.aspx) pour plus d’informations sur l’inscription du fournisseur d’authentification externe à l’aide d’AD FS.  
  
Nous recommandons d’utiliser les classes définies dans le fichier .css qui fournit des services AD FS pour créer l’interface utilisateur d’authentification un fournisseur d’authentification externe. Vous pouvez utiliser l’applet de commande suivante pour exporter le thème web par défaut et inspecter les classes d’interface utilisateur et les éléments qui sont définies dans le fichier .css. Le fichier .css peut être utilisé dans le développement de l’interface utilisateur de connexion d’un fournisseur d’authentification externe.  
  

    Export-AdfsWebTheme -Name default -DirectoryPath C:\theme  
 
  
Voici un exemple de l’interface utilisateur de connexion, qui est mis en surbrillance en rouge, par un fournisseur d’authentification externe. L’interface utilisateur emploie les classes de l’interface utilisateur dans le fichier .css AD FS.  
  
![ADFS et l’authentification Multifacteur](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom8.png)  
  
Avant d’écrire une nouvelle méthode d’authentification personnalisée, nous vous recommandons de temps d’étudier les définitions de thème et de style AD FS pour comprendre les exigences de création de contenu.  
  
-   Une méthode d’authentification personnalisée auteurs uniquement un segment HTML dans la page de connexion AD FS et pas la page entière. Vous devez utiliser la définition de style d’AD FS pour obtenir l’apparence et le comportement.  
  
![ADFS et l’authentification Multifacteur](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom9.png)  
  
-   N’oubliez pas que les administrateurs de services AD FS peuvent personnaliser les styles AD FS. . Nous déconseillons de coder en dur vos propres styles. Au lieu de cela, nous vous recommandons d’utiliser des styles AD FS chaque fois que possible.  
  
-   Out-maximum\ la zone, styles AD FS sont créés avec un style \(LTR\) fait à droite et un \(RTL\) celle-ci droit à gauche. Les administrateurs peuvent personnaliser les deux et peuvent fournir des styles de langue spécifique par le biais de la définition du thème web. Chaque feuille de style possède trois sections avec des commentaires respectifs:  
  
    -   **styles de thème** \-ces styles ne doivent pas et ne peut pas être utilisés. Ils servent à définir un thème appliqué dans toutes les pages. Ils sont définis par un ID d’élément délibérément afin qu’ils ne sont pas réutilisés.  
  
    -   **styles courants** \-ce sont les styles qui doivent être utilisées pour votre contenu.  
  
    -   **styles de facteur de former** \-il s’agit des styles pour différents facteurs de forme. Vous devez comprendre cette section pour vous assurer que votre contenu fonctionne avec différents facteurs de forme, par exemple, téléphones portables et tablettes.  
  
Pour plus d’informations, voir [Guide de Solution: gérer les risques avec contrôle d’accès prennent facteurs](https://technet.microsoft.com/library/dn280937.aspx) et [Guide de Solution: gérer les risques avec une authentification prennent-facteur supplémentaire pour les Applications sensibles](https://tnstage.redmond.corp.microsoft.com/library/dn280949.aspx).  

## <a name="additional-references"></a>Références supplémentaires 
[ADFS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md) 
