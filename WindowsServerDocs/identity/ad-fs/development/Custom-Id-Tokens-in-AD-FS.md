---
title: Personnaliser des revendications d’être émis dans id_token lors de l’utilisation de OpenID Connect ou OAuth avec AD FS 2016
description: Présentation technique de la conecpts de jeton d’id personnalisé dans AD FS 2016
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.reviewer: anandy
ms.technology: identity-adfs
ms.openlocfilehash: 8c8e14f22a4bca5b6d32e841814a58a4b4ddca01
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820640"
---
# <a name="customize-claims-to-be-emitted-in-idtoken-when-using-openid-connect-or-oauth-with-ad-fs-2016"></a>Personnaliser des revendications d’être émis dans id_token lors de l’utilisation de OpenID Connect ou OAuth avec AD FS 2016

## <a name="overview"></a>Vue d'ensemble
L’article [ici](enabling-openId-connect-with-ad-fs.md) montre comment créer une application qui utilise ADFS pour OpenID Connect d’authentification. Toutefois, par défaut il existe uniquement un ensemble fixe de revendications disponibles dans le paramètre id_token. AD FS 2016 a la possibilité de personnaliser id_token dans les scénarios OpenID Connect.

## <a name="when-are-custom-id-token-used"></a>Lorsque sont ID personnalisé jeton utilisé ?
Dans certains scénarios, il est possible que l’application cliente n’a pas d’une ressource qu’il tente d’accéder. Par conséquent, il n’a pas vraiment besoin d’un jeton d’accès. Dans ce cas, l’application cliente doit essentiellement uniquement un ID jeton, mais certaines revendications supplémentaires pour vous aider dans la fonctionnalité.

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>Quelles sont les restrictions sur l’obtention des revendications personnalisées dans l’ID de jeton ?

### <a name="scenario-1"></a>Scénario 1

![restreindre](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1.  response_mode est défini comme form_post
2.  Uniquement les clients publics peuvent obtenir des revendications personnalisées dans l’ID de jeton
3.  Identificateur de partie de confiance doivent être identique en tant qu’identificateur de client

### <a name="scenario-2"></a>Scénario 2

![restreindre](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

Avec [KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472) installé sur vos serveurs AD FS
1.  response_mode est défini comme form_post
2.  Affecter allatclaims d’étendue pour le client : paire de partie de confiance.
Vous pouvez affecter l’étendue à l’aide de l’applet de commande Grant-ADFSApplicationPermission, comme indiqué dans l’exemple ci-dessous :

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-an-oauth-application-to-handle-custom-claims-in-id-token"></a>Création d’une application OAuth pour gérer les revendications personnalisées dans le jeton d’ID
Utilisez l’article [ici](Enabling-OpenId-Connect-with-AD-FS-2016.md) pour créer une application de l’application qui utilise ADFS pour OpenID Connect connectent. Puis suivez les étapes ci-dessous pour configurer l’application dans AD FS pour la réception du jeton d’ID avec des revendications personnalisées.

### <a name="create-the-application-group-in-ad-fs-2016"></a>Créer le groupe d’applications dans AD FS 2016

1.  Créer un groupe d’applications basé sur le nouveau modèle, vous trouverez ci-dessous, appelé CustomTokenClient.

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

2. Ce modèle crée un client confidentiel. Notez l’identificateur et spécifiez l’URI de retour en tant que l’URL SSL du projet Visual Studio.

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

3.  Dans l’étape suivante, sélectionnez **générer un secret partagé** pour créer des informations d’identification de client et de copier les informations d’identification de client générées.

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

4. Cliquez sur **suivant** et terminez l’Assistant.

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

### <a name="create-the-relying-party"></a>Créer la partie de confiance
Pour ajouter des revendications personnalisées dans l’ID de jeton, vous devez créer une partie de confiance dont les revendications seront ajoutées dans le jeton d’ID. Utilisez l’Assistant Ajout de confiance pour créer une nouvelle partie de confiance comme indiqué ci-dessous :
 
![Partie de confiance](media/Custom-Id-Tokens-in-AD-FS/rpsnap1.png)

Après la création de la partie de confiance, cliquez avec le bouton droit sur l’entrée de tiers de confiance et sélectionnez **stratégie d’émission de revendications de modification** pour ajouter des règles d’émission de revendications. Ajoutez les revendications personnalisées requises pour le jeton d’ID comme indiqué ci-dessous :

![Partie de confiance](media/Custom-Id-Tokens-in-AD-FS/rpsnap2.png)

### <a name="assign-allatclaims-scope-to-the-pair-of-client-and-relying-party"></a>Affecter l’étendue de « allatclaims » à la paire de client et de confiance
À l’aide de PowerShell sur le serveur AD FS, affectez à la nouvelle étendue allatclaims comme indiqué dans l’exemple ci-dessous (modifier l’ID client et le serveur :

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "5db77ce4-cedf-4319-85f7-cc230b7022e0" -ServerRoleIdentifier "https://customidrp1/" -ScopeNames "allatclaims","openid"
```

>[!NOTE]
>Modifier le ClientRoleIdentifier et ServerRoleIdentifier en fonction de vos paramètres d’application

## <a name="test-the-custom-claims-in-id-token"></a>Tester les revendications personnalisées dans le jeton d’ID

Ensuite, à l’aide de la même bit de code que vous avez toujours utilisé pour accéder aux revendications, vous pouvez voir les revendications supplémentaires qui feront partie du jeton id_token.
Par exemple, dans un exemple d’application MVC .NET, ouvrez un des fichiers de contrôleur et entrez le code tel que le ci-dessous :


``` code
    [Authorize]
    public ActionResult About()
    {

        ClaimsPrincipal cp = ClaimsPrincipal.Current;

        string userName = cp.FindFirst(ClaimTypes.GivenName).Value;
        ViewBag.Message = String.Format("Hello {0}!", userName);
        return View();

    }

```

>[!NOTE]
>N’oubliez pas que le paramètre de ressource est requis dans la demande Oauth2.
>
>Exemple incorrect :
>
>**HTTPS&#58;//sts.contoso.com/adfs/oauth2/authorize?response_type=id_token & scope = openid & redirect_uri = https&#58;//myportal/auth & valeur à usage unique = b3ceb943fc756d927777 & client_id = 72-9b22-ca7ab60cb4e7 6db3ec2a-075a - 4c & état = 42c2c156aef47e8d0870 & ressource = 72-9b22-ca7ab60cb4e7 6db3ec2a-075a - 4c**
>
>Bon exemple :
>
>**HTTPS&#58;//sts.contoso.com/adfs/oauth2/authorize?response_type=id_token & scope = openid & redirect_uri = https&#58;//myportal/auth & valeur à usage unique = b3ceb943fc756d927777 & client_id = 72-9b22-ca7ab60cb4e7 6db3ec2a-075a - 4c & état = 42c2c156aef47e8d0870 & ressource = https&#58;//customidrp1/ & response_mode = form_post**
>
>Si le paramètre de ressource n’est pas dans la demande, vous pouvez recevoir une erreur ou un jeton sans les revendications personnalisées.

## <a name="next-steps"></a>Étapes suivantes
[Développement de AD FS](../../ad-fs/AD-FS-Development.md)  
