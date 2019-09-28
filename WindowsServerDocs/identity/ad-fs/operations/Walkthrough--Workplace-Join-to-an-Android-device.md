---
ms.assetid: a33bd54c-e6db-4b58-8264-c0f34bd8ba39
title: 'Procédure pas à pas : Workplace Join à un appareil Android'
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c021e8b67963df4f885059c75eff5e94d6dcdafb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407472"
---
# <a name="walkthrough-workplace-join-to-an-android-device"></a>Procédure pas à pas : Workplace Join à un appareil Android



## <a name="join-your-device-with-workplace-join"></a>Rattacher votre appareil au moyen de la jonction d'espace de travail

> [!NOTE]
> La jonction d’espace de travail Android nécessite Azure Active Directory Device Registration service. Pour appliquer des stratégies d’appareil conditionnelles sur site, l’outil de synchronisation d’annuaires (DirSync) doit être déployé avec l’option d’écriture différée de l’objet appareil activée. À l’heure actuelle, la réécriture de l’appareil pour Active Directory depuis Azure Active Directory peut prendre jusqu’à 3 heures. Par conséquent, les utilisateurs doivent attendre 3 heures pour accéder aux applications Web locales, après avoir créé un compte professionnel. Pour plus d’informations sur le déploiement de Azure Active Directory Device Registration service, consultez la page [vue d’ensemble du service Azure Active Directory Device Registration](https://msdn.microsoft.com/library/azure/dn788908.aspx)

#### <a name="create-a-work-account-that-joins-your-device-with-workplace-join"></a>Créer un compte professionnel qui joint votre appareil à la jonction d’espace de travail

1.  Vous devez installer Azure Authenticator application sur votre appareil pour créer un compte professionnel qui joint votre appareil à la jonction d’espace de travail. L’URL suivante contient des instructions sur l’installation de l’application Azure Authenticator sur votre appareil Android et l’ajout d’un compte professionnel. Le compte professionnel fait de votre appareil Android un appareil de confiance et fournit une authentification unique (SSO) aux applications sur l’appareil. Vous pouvez utiliser l’appareil de confiance pour accéder aux applications Web et aux applications métier modernes, comme recommandé par votre administrateur informatique. Pour plus d’informations, consultez [Azure Authenticator pour Android](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to).

## <a name="see-also"></a>Voir aussi
[Joindre à l’espace de travail à partir de n’importe quel appareil pour l’authentification unique et l’authentification de second facteur transparente entre les applications d’entreprise](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[configuration de l’accès conditionnel local à l’aide de Azure Active Directory Device Registration service](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)


