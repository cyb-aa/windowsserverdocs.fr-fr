---
title: "Personnaliser les revendications à émettre dans id_token lors de l’utilisation de se connecter OpenID ou OAuth avec ADFS2016"
description: "Une vue d’ensemble technique de la conecpts jeton id personnalisé dans ADFS2016"
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.reviewer: anandy
ms.technology: identity-adfs
ms.openlocfilehash: c17d50bb6d90726eafc3e585f16a7a8189a68392
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="customize-claims-to-be-emitted-in-idtoken-when-using-openid-connect-or-oauth-with-ad-fs-2016"></a>Personnaliser les revendications à émettre dans id_token lors de l’utilisation de se connecter OpenID ou OAuth avec ADFS2016

## <a name="overview"></a>Vue d’ensemble
L’article [ici](enabling-openId-connect-with-ad-fs.md) montre comment créer une application qui utilise les services ADFS pour se connecter OpenID ouverture de session. Toutefois, par défaut sont uniquement un ensemble fixe de revendications disponibles dans l’id_token. ADFS2016 a la possibilité de personnaliser l’id_token dans les scénarios de connexion OpenID.

## <a name="when-are-custom-id-token-used"></a>Lorsque sont ID personnalisé jeton utilisé?
Dans certains scénarios, il est possible que l’application cliente n’est qu’il essaie d’accéder à une ressource. Par conséquent, il n’a pas vraiment besoin d’un jeton d’accès. Dans ce cas, l’application cliente doit essentiellement uniquement un mais ID jeton avec certaines revendications supplémentaires pour aider à la fonctionnalité.

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>Quelles sont les restrictions sur l’obtention de revendications personnalisées dans l’ID de jeton?

### <a name="scenario-1"></a>Scénario 1

![Restreindre](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1.  response_mode est défini comme form_post
2.  Publics uniquement des clients peuvent obtenir des revendications personnalisées dans l’ID de jeton
3.  Identificateur de partie de confiance doit être identique en tant qu’identificateur de client

### <a name="scenario-2"></a>Scénario 2

![Restreindre](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

Avec [KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472) sur vos serveurs ADFS
1.  response_mode est défini comme form_post
2.  Attribuer allatclaims étendue au client – paire RP.
Vous pouvez affecter l’étendue à l’aide de l’applet de commande Grant-ADFSApplicationPermission comme indiqué dans l’exemple ci-dessous:

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-an-oauth-application-to-handle-custom-claims-in-id-token"></a>Création d’une application OAuth pour gérer des revendications personnalisées dans le jeton d’ID
Utilisez l’article [ici](Enabling-OpenId-Connect-with-AD-FS-2016.md) pour créer une application de l’application qui utilise ADFS pour OpenID Connect ouverture de session. Suivez les étapes ci-dessous pour configurer l’application dans ADFS pour la réception de jeton ID avec les revendications personnalisées.

### <a name="create-the-application-group-in-ad-fs-2016"></a>Créer le groupe d’applications dans ADFS2016

1.  Créer un groupe d’applications basé sur le nouveau modèle, illustré ci-dessous, appelé CustomTokenClient.

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

2. Ce modèle crée un client confidentiel. Notez l’identificateur et spécifiez l’URI de retour en tant que l’URL SSL du projet Visual Studio.

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

3.  Dans l’étape suivante, sélectionnez **générer un secret partagé** pour créer des informations d’identification de client et copier les informations d’identification client générées.

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

4. Cliquez sur **suivant** et continuez pour terminer l’Assistant.

![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

### <a name="create-the-relying-party"></a>Créer la partie de confiance
Afin d’ajouter des revendications personnalisées dans l’ID de jeton, vous devez créer un RP dont les revendications seront ajoutées dans le jeton d’ID. Utilisez l’Assistant Ajouter une approbation de partie de confiance pour créer une nouvelle partie de confiance, comme illustré ci-dessous:
 
![Partie de confiance](media/Custom-Id-Tokens-in-AD-FS/rpsnap1.png)

Une fois la partie de confiance est créée, cliquez avec le bouton droit sur l’entrée de partie de confiance et sélectionnez **stratégie d’émission de revendication modifier** pour ajouter des règles d’émission de revendications. Ajouter les revendications personnalisées requises pour le jeton d’ID, comme illustré ci-dessous:

![Partie de confiance](media/Custom-Id-Tokens-in-AD-FS/rpsnap2.png)

### <a name="assign-allatclaims-scope-to-the-pair-of-client-and-relying-party"></a>Attribuer étendue «allatclaims» à la paire de client et de la partie de confiance
À l’aide de PowerShell sur le serveur ADFS, attribuez la nouvelle étendue allatclaims selon les indications dans l’exemple ci-dessous (modifier l’identifiant du client et le serveur:

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "5db77ce4-cedf-4319-85f7-cc230b7022e0" -ServerRoleIdentifier "https://customidrp1/" -ScopeNames "allatclaims","openid"
```

>[!NOTE]
>Modifier le ClientRoleIdentifier et ServerRoleIdentifier en fonction de vos paramètres d’application

## <a name="test-the-custom-claims-in-id-token"></a>Tester les revendications personnalisées dans le jeton d’ID

Ensuite, à l’aide de la même bit de code que vous avez toujours utilisé pour accéder aux revendications, vous pouvez voir les revendications supplémentaires qui feront partie de l’id_token.
Par exemple, dans un exemple d’application .NET MVC, ouvrez un des fichiers du contrôleur et entrez le code comme le ci-dessous:


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
>Exemple incorrect:
>
>**HTTPS et #58; / / sts.contoso.com/adfs/oauth2/authorize?response_type=id_token&scope=openid&redirect_uri=https&#58;//myportal/auth&nonce=b3ceb943fc756d927777&client_id=6db3ec2a-075a-4c72-9b22-ca7ab60cb4e7&state=42c2c156aef47e8d0870&resource=6db3ec2a-075a-4c72-9b22-ca7ab60cb4e7**
>
>Bon exemple:
>
>**HTTPS et #58; / / sts.contoso.com/adfs/oauth2/authorize?response_type=id_token&scope=openid&redirect_uri=https&#58;//myportal/auth&nonce=b3ceb943fc756d927777&client_id=6db3ec2a-075a-4c72-9b22-ca7ab60cb4e7&state=42c2c156aef47e8d0870&resource=https&#58;//customidrp1/**
>
>Si le paramètre de la ressource n’est pas dans la demande que vous receviez une erreur ou un jeton sans les revendications personnalisées.

## <a name="next-steps"></a>Étapes suivantes
[Développement d’ADFS](../../ad-fs/AD-FS-Development.md)  