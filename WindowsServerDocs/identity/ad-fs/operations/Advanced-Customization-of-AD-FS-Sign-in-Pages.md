---
ms.assetid: 882abec8-0189-4f73-99c5-792987168080
title: Personnalisation avancée des pages de connexion AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 01/16/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ea149e6b9a5fbf5c0671991a61f9bcda35656022
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859982"
---
# <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>Personnalisation avancée des pages de connexion AD FS

  
## <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>Personnalisation avancée des\-de signature de AD FS dans les pages  
AD FS dans Windows Server 2012 R2 offre une\-intégrée pour la personnalisation de l’expérience de signature\-. Pour la plupart de ces scénarios, le\-généré dans les applets de commande Windows PowerShell est tout ce qui est nécessaire.  Nous vous recommandons d’utiliser la\-générée dans les commandes Windows PowerShell pour personnaliser les éléments standard pour la AD FS de signe\-dans l’expérience autant que possible.  Pour plus d’informations, consultez Personnalisation de la [connexion utilisateur AD-FS](AD-FS-user-sign-in-customization.md) .  
  
Dans certains cas, AD FS administrateurs peuvent souhaiter fournir des\-de signe supplémentaires dans les expériences qui ne sont pas possibles via les commandes PowerShell existantes fournies dans\-Box avec AD FS. Dans certains cas, il est possible \(dans les instructions fournies ci-dessous\) pour permettre aux administrateurs de personnaliser davantage la signature\-dans l’expérience en ajoutant une logique supplémentaire à **OnLoad. js** qui est fournie par AD FS et sera exécutée sur toutes les pages de AD FS.  
  
## <a name="things-to-know-before-you-start"></a>Éléments à connaître avant de commencer  
  
-   Toute modification qui a un impact sur les flux de redirection ou modifie les paramètres de protocole avec lesquels AD FS fonctionne n’est pas prise en charge.
  
-   Le fichier OnLoad. js d’origine, qui est fourni avec le thème Web par défaut, contient du code qui gère le rendu des pages pour différents facteurs de forme. Il est recommandé de ne pas modifier le contenu OnLoad. js d’origine, mais d’ajouter votre code au fichier OnLoad. js existant qui gère la logique personnalisée.  
  
-   AD FS est fourni avec un\-généré dans le thème Web, qui est appelé default. Vous ne pouvez pas modifier le OnLoad. js du thème Web par défaut. Pour mettre à jour OnLoad. js, vous devez créer et utiliser un thème Web personnalisé pour AD FS signer\-dans les pages.  Pour plus d’informations sur la création d’un thème Web personnalisé, consultez Personnalisation de la [connexion utilisateur AD-FS](AD-FS-user-sign-in-customization.md) .  
  
-   Le même OnLoad. js s’exécutera sur toutes les pages ADFS \(ex. formulaire\-page d’ouverture de session, page de découverte de domaine d’hébergement et autres\). Vous devez vous assurer que le code de votre script est exécuté uniquement lorsqu’il est conçu et qu’il ne s’exécute pas de manière inattendue.  
  
-   Lors du référencement d’un élément HTML, vérifiez que vous recherchez toujours l’existence de l’élément avant d’agir sur l’élément. Cela fournit une robustesse et garantit que la logique personnalisée n’est pas exécutée sur les pages qui ne contiennent pas cet élément. Vous pouvez simplement afficher la source HTML sur le AD FS\-dans pages pour afficher les éléments existants.  
  
-   Il est fortement recommandé de valider vos personnalisations dans un autre environnement et de les tester avant de les déployer sur des serveurs de AD FS de production. Cela réduit le risque que les utilisateurs finaux soient exposés à ces personnalisations avant la validation.  
  
## <a name="customizing-the-ad-fs-sign-in-experience-by-using-onloadjs"></a>Personnalisation du\-de signe AD FS en cours d’utilisation à l’aide de OnLoad. js  
Utilisez les étapes suivantes lors de la personnalisation de OnLoad. js pour le service AD FS.  
  
#### <a name="customizing-onloadjs-for-the-ad-fs-service"></a>Personnalisation de OnLoad. js pour le service AD FS  
  
1.  Pour ajouter votre logique personnalisée à OnLoad. js, vous devez d’abord créer un thème Web personnalisé. Le thème qui est expédié\-de\-la boîte de\-est appelée valeur par défaut. Vous pouvez exporter le thème par défaut de manière à démarrer rapidement. L’applet de commande suivante crée un thème Web personnalisé, qui duplique le thème Web par défaut :  
  
    ```  
    New-AdfsWebTheme –Name custom –SourceName default  
  
    ```  
  
2.  Vous pouvez ensuite exporter le thème Web personnalisé ou par défaut pour accéder au fichier OnLoad. js. Pour exporter un thème Web, utilisez l’applet de commande suivante :  
  
    ```  
    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  
  
    ```  
  
    Vous trouverez OnLoad. js sous le dossier script dans le répertoire que vous spécifiez dans l’applet de commande Export ci-dessus et vous ajouterez votre logique personnalisée au script \(consultez les cas d’utilisation dans la section exemple ci-dessous\).  
  
3.  Apportez les modifications nécessaires pour personnaliser OnLoad. js en fonction de vos besoins.  
  
4.  Mettez à jour le thème avec l’OnLoad. js modifié. Utilisez l’applet de commande suivante pour appliquer la mise à jour de OnLoad. js au thème Web personnalisé :  

     Pour AD FS sur Windows Server 2012 R2 :  

    ```  
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';path="c:\theme\script\onload.js"}  
  
    ```  
    Pour AD FS sur Windows Server 2016 :

     ```  
    Set-AdfsWebTheme -TargetName custom -OnLoadScriptPath "c:\ADFStheme\script\onload.js"   
  
    ```  
  
5.  Pour appliquer le thème Web personnalisé à AD FS, utilisez l’applet de commande suivante :  
  
    ```  
    Set-AdfsWebConfig -ActiveThemeName custom  
    ```  
  
## <a name="additional-customization-examples"></a>Exemples supplémentaires de personnalisation  
Vous trouverez ci-dessous des exemples de code personnalisé ajoutés à OnLoad. js à des fins de réglage\-. Quand vous ajoutez le code personnalisé, ajoutez toujours votre code personnalisé en bas de OnLoad. js.  
  
### <a name="example-1-change-sign-in-with-organizational-account-string"></a>Exemple 1 : modifier la chaîne « connexion avec un compte d’organisation »  
Le formulaire AD FS par défaut\-Sign\-sur la page a le titre « Connectez-vous avec votre compte professionnel » au-dessus des zones de saisie de l’utilisateur.  
  
Si vous souhaitez remplacer cette chaîne par votre propre chaîne, vous pouvez ajouter le code suivant à OnLoad. js.  
  
```  
// Sample code to change "Sign in with organizational account" string.  
  
// Check whether the loginMessage element is present on this page.  
var loginMessage = document.getElementById('loginMessage');  
if (loginMessage)  
{  
       // loginMessage element is present, modify its properties.  
       loginMessage.innerHTML = 'Your company description text';  
}  
  
```  
  
### <a name="example-2-accept-sam-account-name-as-a-login-format-on-an-ad-fs-form-based-sign-in-page"></a>Exemple 2 : accepter le nom de compte\-SAM en tant que format de connexion sur un formulaire de AD FS\-\-de signature sur la page  
La forme AD FS par défaut\-Sign\-dans la page prend en charge le format de connexion des noms d’utilisateur principaux \(UPN\) \(<strong>johndoe@contoso.com, par exemple\)\-ou</strong> des noms de compte Sam \(de domaine\\**contoso\\johndoe** ou **contoso.com\)JohnDoe** . Si tous vos utilisateurs proviennent du même domaine et qu’ils connaissent uniquement les noms de compte Sam\-, vous pouvez prendre en charge le scénario dans lequel les utilisateurs peuvent se connecter à l’aide des noms de compte Sam\-uniquement. Vous pouvez ajouter le code suivant à OnLoad. js pour prendre en charge ce scénario, remplacez simplement le domaine « contoso.com » dans l’exemple ci-dessous par le domaine que vous souhaitez utiliser.  
  
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
[Personnalisation de la connexion de l’utilisateur AD FS](AD-FS-user-sign-in-customization.md)  
  

