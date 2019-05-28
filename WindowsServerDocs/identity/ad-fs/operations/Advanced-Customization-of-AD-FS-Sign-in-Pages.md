---
ms.assetid: 882abec8-0189-4f73-99c5-792987168080
title: Personnalisation d’AD FS Sign-in Pages avancée
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 01/16/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 73ff3fc6df872edd29735ee96c0918144250d5f1
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190040"
---
# <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>Personnalisation d’AD FS Sign-in Pages avancée

  
## <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>Personnalisation de l’authentification AD FS avancée\-dans les Pages  
AD FS dans Windows Server 2012 R2 fournit intégré\-prise en charge pour la personnalisation de la connexion\-dans l’expérience. Pour la plupart de ces scénarios, intégrées\-dans Windows PowerShell, applets de commande sont tout ce qui est requis.  Il est recommandé d’utiliser intégrée\-dans les commandes Windows PowerShell pour personnaliser les éléments standards pour AD FS signer\-expérience autant que possible.  Consultez [AD FS sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md) pour plus d’informations.  
  
Dans certains cas, les administrateurs AD FS souhaiterez peut-être fournir une authentification supplémentaire\-dans des expériences qui ne sont pas possibles via existant commandes PowerShell fournis dans\-boîte avec AD FS. Dans certains cas, il est possible \(dans les instructions fournies ci-dessous\) pour les administrateurs de personnaliser la connexion\-dans une expérience plus loin en ajoutant une logique supplémentaire à **onload.js** qui est fourni par AD FS et sera exécuté sur toutes les pages d’AD FS.  
  
## <a name="things-to-know-before-you-start"></a>Choses à savoir avant de commencer  
  
-   Toute modification qui a un impact sur les flux de redirection ou modifie les paramètres de protocole ADFS fonctionne avec n’est pas pris en charge.
  
-   Le onload.js d’origine, celle qui est fourni avec le thème web par défaut, contient le code qui gère le rendu de page pour différents facteurs de forme. Il est recommandé de ne pas modifier le contenu de onload.js d’origine, mais uniquement ajouter votre code à l’onload.js existante qui gère la logique personnalisée.  
  
-   AD FS est livré avec un\-dans le thème web qui est appelé par défaut. Vous ne pouvez pas modifier le onload.js du thème web par défaut. Pour mettre à jour onload.js, vous devez créer et utiliser un thème web personnalisé pour l’authentification AD FS\-dans les pages.  Consultez [AD FS sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md) pour plus d’informations sur la création d’un thème web personnalisé.  
  
-   Le même onload.js s’exécutera sur toutes les pages d’ADFS \(ex. formulaire\-en fonction de la page de connexion, page de découverte du domaine d’accueil et etc.\). Vous devez vous assurer que le code dans votre script obtient uniquement exécuté comme il est conçu et ne sont pas exécutée inattendu.  
  
-   Lors du référencement d’un élément HTML, assurez-vous de toujours vérifier l’existence de l’élément avant d’agir sur l’élément. Cela fournit la robustesse et garantit que la logique personnalisée ne sera pas exécutée sur les pages qui ne contiennent pas de cet élément. Vous pouvez simplement afficher la source HTML sur la connexion AD FS\-dans les pages pour afficher les éléments existants.  
  
-   Il est fortement recommandé de valider vos personnalisations dans un autre environnement et de les tester avant de la rendre son évolution horizontale sur production serveurs AD FS. Cela réduit les risques des utilisateurs finaux ne soient exposées à ces personnalisations avant la validation.  
  
## <a name="customizing-the-ad-fs-sign-in-experience-by-using-onloadjs"></a>Personnalisation de l’authentification AD FS\-dans l’expérience à l’aide de onload.js  
Procédez comme suit lors de la personnalisation de la onload.js pour le service AD FS.  
  
#### <a name="customizing-onloadjs-for-the-ad-fs-service"></a>Personnalisation de onload.js pour le Service AD FS  
  
1.  Pour ajouter votre logique personnalisée à onload.js, vous devez d’abord créer un thème web personnalisé. Le thème qui est livré out\-de\-le\-est appelée par défaut. Vous pouvez exporter le thème par défaut de manière à démarrer rapidement. L’applet de commande suivante crée un thème web personnalisé, qui duplique le thème web par défaut :  
  
    ```  
    New-AdfsWebTheme –Name custom –SourceName default  
  
    ```  
  
2.  Vous pouvez ensuite exporter personnalisé ou par défaut de thème web pour obtenir le fichier de onload.js. Pour exporter un thème web, utilisez l’applet de commande suivante :  
  
    ```  
    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  
  
    ```  
  
    Vous trouverez onload.js sous le dossier de script dans le répertoire que vous spécifiez dans l’applet de commande export ci-dessus et que vous ajoutez votre logique personnalisée au script \(voir dans la section exemple ci-dessous, les cas d’usage\).  
  
3.  Apporter la modification nécessaire pour personnaliser onload.js selon vos besoins.  
  
4.  Mettre à jour le thème avec le onload.js modifié. Pour appliquer la mise à jour onload.js à thème web personnalisé, utilisez l’applet de commande suivante :  

     Pour AD FS sur Windows Server 2012 R2 :  

    ```  
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=’/adfs/portal/script/onload.js’;path="c:\theme\script\onload.js"}  
  
    ```  
    Pour AD FS sur Windows Server 2016 :

     ```  
    Set-AdfsWebTheme -TargetName custom -OnLoadScriptPath "c:\ADFStheme\script\onload.js"   
  
    ```  
  
5.  Pour appliquer le thème web personnalisé à AD FS, utilisez l’applet de commande suivante :  
  
    ```  
    Set-AdfsWebConfig -ActiveThemeName custom  
    ```  
  
## <a name="additional-customization-examples"></a>Exemples de personnalisation supplémentaires  
Les éléments suivants sont des exemples de code personnalisé ajouté à onload.js pour différents fine\-à des fins de paramétrer. Lorsque vous ajoutez le code personnalisé, veuillez toujours ajouter votre code personnalisé au bas de la onload.js.  
  
### <a name="example-1-change-sign-in-with-organizational-account-string"></a>Exemple 1 : modification de la chaîne « Connectez-vous avec un compte professionnel »  
La valeur par défaut du formulaire d’AD FS\-l’authentification par\-dans la page a un titre de « Se connecter avec votre compte professionnel » au-dessus des zones d’entrée utilisateur.  
  
Si vous souhaitez remplacer cette chaîne par vos propres, vous pouvez ajouter le code suivant à onload.js.  
  
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
  
### <a name="example-2-accept-sam-account-name-as-a-login-format-on-an-ad-fs-form-based-sign-in-page"></a>Exemple 2 : accepter SAM\-nom du compte en tant que connexion format sur un formulaire d’AD FS\-l’authentification par\-dans la page  
La valeur par défaut du formulaire d’AD FS\-l’authentification par\-dans la page prend en charge le format de connexion des noms d’utilisateur Principal \(UPN\) \(, par exemple, **johndoe@contoso.com** \) domaine qualifié sam ou\-noms de comptes \( **contoso\\johndoe** ou **contoso.com\\johndoe**\). En cas de tous vos utilisateurs proviennent du même domaine et ils ont uniquement connaissent sam\-noms de compte, vous souhaiterez prendre en charge le scénario où les utilisateurs peuvent se connecter à leur utilisation sam\-uniquement des noms de compte. Vous pouvez ajouter le code suivant à onload.js pour prendre en charge ce scénario, remplacez simplement le domaine « contoso.com » dans l’exemple ci-dessous avec le domaine que vous souhaitez utiliser.  
  
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
[AD FS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)  
  

