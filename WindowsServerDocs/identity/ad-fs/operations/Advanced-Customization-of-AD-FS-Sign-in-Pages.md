---
ms.assetid: 882abec8-0189-4f73-99c5-792987168080
title: "Personnalisation avancée des services AD FS Sign-in Pages"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/13/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ea01d0ff2a38c4fef2f68091608d777d8412e91b
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>Personnalisation avancée des services AD FS Sign-in Pages

>S’applique à: Windows Server2016, Windows Server2012R2
  
## <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>Personnalisation avancée des Pages de connexion FS AD  
AD FS dans Windows Server 2012 R2 fournit une prise en charge intégrée pour personnaliser l’expérience de connexion. Pour la plupart de ces scénarios, les applets de commande intégrée dans Windows PowerShell sont tout ce qui est requis.  Il est recommandé d’utiliser les commandes Windows PowerShell intégrée pour personnaliser les éléments standards pour AD FS connexion expérience chaque fois que possible.  Voir [ADFS connexion personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md) pour plus d’informations.  
  
Dans certains cas, les administrateurs des services AD FS voudrez fournir la connexion d’expériences supplémentaires qui ne sont pas possibles via les commandes PowerShell existants livrés ou-box avec AD FS. Dans certains cas, il est possible de \ (dans les instructions fournies below\) pour les administrateurs de personnaliser l’expérience de connexion en ajoutant une logique supplémentaire à **onload.js** qui est fourni par AD FS et sera exécuté sur toutes les pages d’AD FS.  
  
## <a name="things-to-know-before-you-start"></a>Éléments à connaître avant de commencer  
  
-   Toute modification qui a une incidence sur les flux de redirection ou modifie les paramètres de protocole fonctionnant avec AD FS n’est pas pris en charge.
  
-   Le onload.js d’origine, celui qui est fourni avec le thème web par défaut, contient le code qui gère le rendu de page pour différents facteurs de forme. Il est recommandé de ne pas modifier le contenu onload.js d’origine, mais uniquement ajouter votre code à la onload.js existant qui gère une logique personnalisée.  
  
-   AD FS est fourni avec un thème web intégrée qui est appelé par défaut. Vous ne pouvez pas modifier le onload.js du thème web par défaut. Pour mettre à jour onload.js, vous devez créer et utiliser un thème web personnalisé pour les pages de connexion AD FS.  Voir [ADFS connexion personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md) pour plus d’informations sur la création d’un thème web personnalisé.  
  
-   Le même onload.js s’exécutera sur tous les ADFS pages \(ex. page d’ouverture de session basée sur des formulaires, page de découverte de domaine d’accueil et etc.. \). Vous devez vous assurer que le code dans votre script obtient uniquement exécuté comme il est conçu et ne sont pas exécutée inattendu.  
  
-   Lorsque vous référencez n’importe quel élément HTML, vérifiez toujours vérifier l’existence de l’élément avant agissant sur l’élément. Cela fournit la robustesse et permet de s’assurer que la logique personnalisée ne sera pas exécutée sur les pages qui ne contiennent pas cet élément. Vous pouvez simplement afficher la source HTML sur les pages de connexion AD FS pour afficher les éléments existants.  
  
-   Il est fortement recommandé de valider vos personnalisations dans un environnement de remplacement et de les tester avant de déployer sur production serveurs AD FS. Cela réduit les risques d’exposition à ces personnalisations avant la validation des utilisateurs.  
  
## <a name="customizing-the-ad-fs-sign-in-experience-by-using-onloadjs"></a>Personnalisation de l’expérience de connexion AD FS à l’aide de onload.js  
Utilisez les étapes suivantes lorsque vous personnalisez le onload.js pour le service AD FS.  
  
#### <a name="customizing-onloadjs-for-the-ad-fs-service"></a>Personnalisation de onload.js pour le Service AD FS  
  
1.  Pour ajouter une logique personnalisée à onload.js, vous devez d’abord créer un thème web personnalisé. Le thème est expédié out-maximum\-programme-box est appelé par défaut. Vous pouvez exporter le thème par défaut et l’utiliser afin que vous pouvez démarrer rapidement. L’applet de commande suivant crée un thème web personnalisé, qui duplique le thème web par défaut:  
  
    ```  
    New-AdfsWebTheme –Name custom –SourceName default  
  
    ```  
  
2.  Vous pouvez exporter personnalisé ou thème web pour obtenir le fichier onload.js par défaut. Pour exporter un thème web, utilisez l’applet de commande suivante:  
  
    ```  
    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  
  
    ```  
  
    Vous trouverez onload.js sous le dossier de script dans le répertoire que vous spécifiez dans l’applet de commande export ci-dessus et ajoutez votre logique personnalisée pour le script \ (voir cas d’utilisation dans la below\ section exemple).  
  
3.  Effectuer cette modification nécessaire pour personnaliser onload.js en fonction de vos besoins.  
  
4.  Mettre à jour le thème avec le onload.js modifié. Pour appliquer la mise à jour onload.js à thème web personnalisé, utilisez l’applet de commande suivante:  
  
    ```  
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=’/adfs/portal/script/onload.js’;path="c:\theme\script\onload.js"}  
  
    ```  
  
5.  Pour appliquer le thème web personnalisé pour AD FS, utilisez l’applet de commande suivante:  
  
    ```  
    Set-AdfsWebConfig -ActiveThemeName custom  
    ```  
  
## <a name="additional-customization-examples"></a>Exemples de personnalisation supplémentaires  
Voici les exemples de code personnalisé ajouté à onload.js à des fins de régler les fine\ différents. Lorsque vous ajoutez le code personnalisé, veuillez toujours ajouter votre code personnalisé vers le bas de la onload.js.  
  
### <a name="example-1-change-sign-in-with-organizational-account-string"></a>Exemple 1: modifier la chaîne «Connectez-vous avec le compte d’organisation»  
La page par défaut AD FS basée sur des formulaires dans connexion possède un titre de «Connectez-vous avec votre compte d’organisation» au-dessus des zones de l’entrée utilisateur.  
  
Si vous souhaitez remplacer cette chaîne par votre propre, vous pouvez ajouter le code suivant à onload.js.  
  
```  
// Sample code to change “Sign in with organizational account” string.  
  
// Check whether the loginMessage element is present on this page.  
var loginMessage = document.getElementById('loginMessage');  
if (loginMessage)  
{  
       // loginMessage element is present, modify its properties.  
       loginMessage.innerHTML = 'Your company description text';  
}  
  
```  
  
### <a name="example-2-accept-sam-account-name-as-a-login-format-on-an-ad-fs-form-based-sign-in-page"></a>Exemple 2: accepter le nom du compte de SAM\ comme format de connexion sur une page AD FS basée sur des formulaires connexion  
Le format de connexion AD FS basée sur des formulaires page de connexion prend en charge des noms d’utilisateur Principal \(UPNs\) \(for example, ** johndoe@contoso.com **\) ou de domaine par défaut des noms de compte sam\ qualifiés \ (**contoso\\johndoe** ou **contoso.com\\johndoe**\). En cas de tous les utilisateurs proviennent du même domaine et ils savent uniquement sur les noms de compte sam\, vous voudrez peut-être prendre en charge le scénario où les utilisateurs peuvent se connecter à leur utilisation uniquement les noms de compte sam\. Vous pouvez ajouter le code suivant à onload.js pour prendre en charge ce scénario, remplacez simplement le domaine «contoso.com» dans l’exemple ci-dessous avec le domaine que vous souhaitez utiliser.  
  
```  
if (typeof Login != 'undefined'){  
    Login.submitLoginRequest = function () {   
    var u = new InputUtil();  
    var e = new LoginErrors();  
    var userName = document.getElementById(Login.userNameInput);  
    var password = document.getElementById(Login.passwordInput);  
    if (userName.value && !userName.value.match('[@\\\\]'))   
    {  
        var userNameValue = 'contoso.com\\' + userName.value;  
        document.forms['loginForm'].UserName.value = userNameValue;  
    }  
  
    if (!userName.value) {  
       u.setError(userName, e.userNameFormatError);  
       return false;  
    }  
  
    if (!password.value)   
    {  
        u.setError(password, e.passwordEmpty);  
        return false;  
    }  
    document.forms['loginForm'].submit();  
    return false;  
};  
}  
  
```  
  
## <a name="additional-references"></a>Références supplémentaires 
[ADFS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)  
  

