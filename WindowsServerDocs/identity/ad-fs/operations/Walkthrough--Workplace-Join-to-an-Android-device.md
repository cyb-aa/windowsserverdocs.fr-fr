---
ms.assetid: a33bd54c-e6db-4b58-8264-c0f34bd8ba39
title: "Procédure pas à pas: la jonction à un périphérique Android"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cfe26947b6b0de28ea50367f82d52815fff8f323
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="walkthrough-workplace-join-to-an-android-device"></a>Procédure pas à pas: La jonction à un périphérique Android

>S’applique à: Windows Server2016, Windows Server2012R2


## <a name="join-your-device-with-workplace-join"></a>Joindre votre appareil avec l’espace de travail

> [!NOTE]
> Jonction Android nécessite Azure Active Directory Device Registration Service. Pour appliquer l’appareil conditionnel le stratégies locales, outil de synchronisation d’annuaires (DirSync) doit être déployé avec périphérique objet en écriture différée option est activée. À l’heure actuelle, appareil en écriture différée dans Active Directory à partir d’Azure Active Directory peut prendre jusqu'à 3 heures. Par conséquent, les utilisateurs doivent attendre 3 heures accéder aux applications web sur site après la création d’un compte professionnel. Pour plus d’informations sur le déploiement d’Azure Active Directory Device Registration service, voir, [Azure Active Directory Device Registration Service Overview](https://msdn.microsoft.com/library/azure/dn788908.aspx)

#### <a name="create-a-work-account-that-joins-your-device-with-workplace-join"></a>Créer un compte professionnel qui rejoint votre appareil avec la jonction d’espace de travail

1.  Vous devrez installer l’application Azure Authenticator sur votre appareil pour créer un compte professionnel qui rejoint votre appareil avec la jonction. L’URL suivante comporte des instructions sur la façon d’installer l’application Azure authenticator sur votre périphérique Android et ajouter un compte professionnel. Le compte rend votre périphérique Android dans un appareil de confiance et fournit Single Sign-On (SSO) pour les applications sur l’appareil. Vous pouvez utiliser l’appareil de confiance pour les applications web accès et les applications métier de modernes comme recommandé par votre administrateur informatique. Pour plus d’informations, voir [Azure Authenticator pour Android](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to).

## <a name="see-also"></a>Voir aussi
[Rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et transparente deuxième facteur d’authentification entre les Applications d’entreprise](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[configuration d’accès conditionnel local avec Azure Active Directory Device Registration Service](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)


