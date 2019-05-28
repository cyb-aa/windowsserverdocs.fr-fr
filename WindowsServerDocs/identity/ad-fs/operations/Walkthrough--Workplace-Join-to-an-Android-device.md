---
ms.assetid: a33bd54c-e6db-4b58-8264-c0f34bd8ba39
title: Procédure pas à pas - jonction à un appareil Android
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 73dbe4d62d460f9487467c7d4198d62b3b6af539
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188929"
---
# <a name="walkthrough-workplace-join-to-an-android-device"></a>Démonstration : Jonction à un appareil Android



## <a name="join-your-device-with-workplace-join"></a>Rattacher votre appareil au moyen de la jonction d'espace de travail

> [!NOTE]
> Jonction d’Android nécessite le Service Azure Active Directory Device Registration. Pour appliquer conditionnel appareil stratégies locales, outil de synchronisation d’annuaires (DirSync) doit être déployé avec l’option d’écriture différée objet appareil activée. À l’heure actuelle, écriture différée des appareils à Active Directory à partir d’Azure Active Directory peut prendre jusqu'à 3 heures. Par conséquent, les utilisateurs doivent attendre de 3 heures accéder à des applications web sur site, après avoir créé un compte professionnel. Pour plus d’informations sur le déploiement d’Azure Active Directory Device Registration service, consultez, [présentation du Service Azure Active Directory périphérique d’enregistrement](https://msdn.microsoft.com/library/azure/dn788908.aspx)

#### <a name="create-a-work-account-that-joins-your-device-with-workplace-join"></a>Créer un compte professionnel qui joint votre appareil avec workplace Join

1.  Vous devez installer l’application Azure Authenticator sur votre appareil pour créer un compte professionnel qui joint votre appareil avec Workplace join. L’URL suivante comporte des instructions sur la façon d’installer l’application Azure authenticator sur votre appareil Android et ajouter un compte professionnel. Le compte de travail rend votre appareil Android dans un appareil de confiance et fournit l’authentification unique (SSO) aux applications sur l’appareil. Vous pouvez utiliser l’appareil de confiance pour l’accès des applications web et des applications line of business modernes comme recommandé par votre administrateur informatique. Pour plus d’informations, consultez [Azure Authenticator pour Android](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to).

## <a name="see-also"></a>Voir aussi
[Rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et transparente deuxième facteur Authentication Across Company Applications](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[configuration d’accès conditionnel en local à l’aide du Service Azure Active Directory Device Registration](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)


