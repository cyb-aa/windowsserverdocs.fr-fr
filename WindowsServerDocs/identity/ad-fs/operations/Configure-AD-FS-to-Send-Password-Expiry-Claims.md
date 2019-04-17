---
ms.assetid: 03c82f43-ae2d-4038-b286-ae3858aed35a
title: "Configurer ADFS pour envoyer des revendications d’expiration de mot de passe"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 386a5ac921ba609c371121b8657351667628951b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="configure-ad-fs-to-send-password-expiry-claims"></a>Configurer ADFS pour envoyer des revendications d’expiration de mot de passe

>S’applique à: Windows Server2016, Windows Server2012R2

Vous pouvez configurer les Services de fédération ActiveDirectory (ADFS) pour envoyer des revendications d’expiration de mot de passe pour les approbations de confiance (applications) qui sont protégées par ADFS. Comment ces revendications sont utilisées dépendent de l’application. Par exemple, avec Office 365 comme votre partie de confiance, les mises à jour ont été implémentés à Exchange et Outlook pour informer les utilisateurs fédérés de leurs mots de passe dès expiré.

Pour configurer ADFS pour envoyer le mot de passe de revendications d’expiration pour une approbation de partie de confiance, vous devez ajouter des règles de revendication suivantes à cette partie de confiance:

```
c1:[Type == "https://schemas.microsoft.com/ws/2012/01/passwordexpirationtime"]
=> issue(store = "_PasswordExpiryStore", types = ("https://schemas.microsoft.com/ws/2012/01/passwordexpirationtime", "https://schemas.microsoft.com/ws/2012/01/passwordexpirationdays", "https://schemas.microsoft.com/ws/2012/01/passwordchangeurl"), query = "{0};", param = c1.Value);
```

> [!NOTE]
> Revendications d’expiration de mot de passe sont uniquement disponibles pour le nom d’utilisateur et mot de passe et MicrosoftPassport pour les types d’authentification de travail.  Si l’utilisateur s’authentifie à l’aide de l’authentification Windows intégrée et Passport n’est pas configuré, les revendications ne sera pas disponibles et les utilisateurs ne verront pas de notifications d’expiration de mot de passe.

> [!NOTE]
> Il existe une fenêtre de 14jours pour les revendications envoyées seront remplies uniquement si le mot de passe arrive à expiration dans les 14jours.

## <a name="see-also"></a>Voir aussi
[Opérations ADFS](../../ad-fs/AD-FS-2016-Operations.md)
