---
ms.assetid: 03c82f43-ae2d-4038-b286-ae3858aed35a
title: Configurer AD FS pour envoyer les revendications d’expiration de mot de passe
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a0c04f42cbe29b1ea36d09f1f148fb28280d7fec
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859892"
---
# <a name="configure-ad-fs-to-send-password-expiry-claims"></a>Configurer AD FS pour envoyer les revendications d’expiration de mot de passe


Vous pouvez configurer Services ADFS (AD FS) pour envoyer des revendications d’expiration de mot de passe aux approbations de partie de confiance (applications) protégées par ADFS. Le mode d’utilisation de ces revendications dépend de l’application. Par exemple, avec Office 365 comme partie de confiance, des mises à jour ont été implémentées dans Exchange et Outlook pour informer les utilisateurs fédérés de l’expiration prochaine de leurs mots de passe.

Pour configurer AD FS pour envoyer des revendications d’expiration de mot de passe à une approbation de partie de confiance, vous devez ajouter les règles de revendication suivantes à cette approbation de partie de confiance :

```
@RuleName = "Issue Password Expiry Claims"
c1:[Type == "https://schemas.microsoft.com/ws/2012/01/passwordexpirationtime"]
 => issue(store = "_PasswordExpiryStore", types = ("https://schemas.microsoft.com/ws/2012/01/passwordexpirationtime", "https://schemas.microsoft.com/ws/2012/01/passwordexpirationdays", "https://schemas.microsoft.com/ws/2012/01/passwordchangeurl"), query = "{0};", param = c1.Value);
```

> [!NOTE]
> Les revendications d’expiration de mot de passe sont uniquement disponibles pour le nom d’utilisateur et le mot de passe et les types d’authentification Microsoft Passport for Work.  Si l’utilisateur s’authentifie à l’aide de l’authentification intégrée de Windows et que Passport n’est pas configuré, les revendications ne sont pas disponibles et les utilisateurs ne voient pas les notifications d’expiration de mot de passe.

> [!NOTE]
> Il existe une fenêtre de 14 jours pour que les revendications envoyées soient remplies uniquement si le mot de passe expire dans un délai de 14 jours.

## <a name="see-also"></a>Voir aussi
[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md)
