---
ms.assetid: 03c82f43-ae2d-4038-b286-ae3858aed35a
title: Configurer AD FS pour envoyer les revendications d’expiration de mot de passe
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 080e8cc81949df3bf74ae846eee7f32c5e145f53
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834360"
---
# <a name="configure-ad-fs-to-send-password-expiry-claims"></a>Configurer AD FS pour envoyer les revendications d’expiration de mot de passe

>S'applique à : Windows Server 2016, Windows Server 2012 R2

Vous pouvez configurer Active Directory Federation Services (ADFS) pour envoyer des revendications d’expiration de mot de passe à la confiance (applications) qui est protégés par AD FS. Comment ces revendications sont utilisées dépendent de l’application. Par exemple, avec Office 365 en tant que votre partie de confiance, les mises à jour ont été implémentées pour Exchange et Outlook pour informer les utilisateurs fédérés de leurs mots de passe bientôt expirer.

Pour configurer AD FS pour envoyer le mot de passe de revendications d’expiration pour une partie de confiance, vous devez ajouter les règles de revendication suivante à cette partie de confiance :

```
@RuleName = "Issue Password Expiry Claims"
c1:[Type == "http://schemas.microsoft.com/ws/2012/01/passwordexpirationtime"]
 => issue(store = "_PasswordExpiryStore", types = ("http://schemas.microsoft.com/ws/2012/01/passwordexpirationtime", "http://schemas.microsoft.com/ws/2012/01/passwordexpirationdays", "http://schemas.microsoft.com/ws/2012/01/passwordchangeurl"), query = "{0};", param = c1.Value);
```

> [!NOTE]
> Revendications d’expiration de mot de passe sont uniquement disponibles pour nom d’utilisateur et mot de passe et Microsoft Passport pour les types d’authentification de travail.  Si l’utilisateur s’authentifie à l’aide de l’authentification intégrée Windows et Passport n’est pas configuré, les revendications ne seront pas disponibles et les utilisateurs ne voient pas de notifications d’expiration de mot de passe.

> [!NOTE]
> Une fenêtre de 14 jours étant les revendications envoyées seront remplies uniquement si le mot de passe arrive à expiration dans les 14 jours.

## <a name="see-also"></a>Voir aussi
[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md)
