---
ms.assetid: e22d84a5-113d-4bec-b484-036ed29f0c28
title: Rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et transparente à deux facteurs d’authentification entre les Applications d’entreprise
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 12/05/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4926eb32a0bbffb092ec02ca2508fe97d52d1466
ms.sourcegitcommit: 46439194e5deb0fa5f338b428f95dd6b5b799337
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2018
---
# <a name="join-to-workplace-from-any-device-for-sso-and-seamless-second-factor-authentication-across-company-applications"></a>Rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et transparente à deux facteurs d’authentification entre les Applications d’entreprise

>S’applique à: Windows Server2012R2

L’augmentation rapide du nombre d’appareils grand public et l’accès omniprésent aux informations modifient la façon dont les utilisateurs perçoivent leur technologie. L’utilisation constante de l’informatique tout au long de la journée, ainsi que d’un accès facile d’informations, brouillent les barrières traditionnelles entre vie professionnelle et privée. Ces changements sont accompagnés d’une croyance ce personnel sélectionné à la technologie et personnalisée pour les adapter à la croyance des utilisateurs, les activités et les planifications-doit s’étendre à l’espace de travail. Pour prendre en compte le besoin croissant d’appareils grand public personnels d’être connecté aux réseaux d’entreprise, nous introduisons les propositions de valeur suivantes:

-   Les administrateurs peuvent contrôler qui a accès aux ressources d’entreprise qui sont basées sur l’application, utilisateur, d’appareil et emplacement.

-   Les employés peuvent accéder des applications et des données partout, sur n’importe quel appareil. Les employés peuvent utiliser Single Sign-On dans les applications de navigateur ou des applications d’entreprise.

## <a name="key-concepts-introduced-in-the-solution"></a>Concepts clés introduits dans la solution

### <a name="workplace-join"></a>Jonction d’espace
À l’aide de la jonction, travailleurs de l’information peuvent rejoindre leurs appareils personnels et les ordinateurs d’espace de travail de leur entreprise pour accéder aux services et ressources de l’entreprise. Lorsque vous joignez votre appareil personnel à votre espace de travail, il devient un appareil connu et fournit transparente à deux facteurs d’authentification et de l’authentification unique aux ressources de l’espace de travail et applications. Lorsqu’un appareil est joint à l’espace de travail, les attributs de l’appareil peuvent être récupérés à partir du répertoire de l’accès conditionnel lecteur afin d’autoriser l’émission de jetons de sécurité pour les applications. Windows8.1 et les périphériques iOS 6.0 + et Android 4.0 + peuvent être associés à l’aide de jonction.

### <a name="BKMK_DRS"></a>Service d’inscription de l’appareil ActiveDirectory Azure
Jonction d’espace de travail est rendue possible par le service d’inscription de l’appareil Azure ActiveDirectory. Lorsqu’un appareil est joint à l’espace de travail, le service configure un objet d’appareil dans Azure ActiveDirectory, puis définit une clé sur l’appareil local qui est utilisée pour représenter l’identité de l’appareil. Cette identité d’appareil puis utilisable avec des règles de contrôle d’accès pour les applications qui sont hébergées dans le cloud et locaux.

Pour plus d’informations sur l’activation du service d’inscription Azure ActiveDirectory appareils, voir [Azure ActiveDirectory Device Registration Service Overview](https://msdn.microsoft.com/6a14cb1f-a058-4453-8ede-d9f4a66a7073.aspx).

### <a name="workplace-join-as-a-seamless-second-factor-authentication"></a>Jonction transparente l’authentification facteur
Les entreprises peuvent gérer les risques qui sont associées à l’accès aux informations et gouvernance de lecteur et la conformité lors de l’octroi consommateur devices d’accéder aux ressources d’entreprise. Jonction d’espace sur les appareils offre les fonctionnalités suivantes pour les administrateurs:

-   Identifie les appareils connus avec l’authentification des appareils. Les administrateurs peuvent utiliser ces informations pour déterminer les accès conditionnel et contrôler l’accès aux ressources.

-   Fournit une expérience de connexion plus transparente pour les utilisateurs d’accéder aux ressources d’entreprise à partir d’appareils de confiance.

### <a name="single-sign-on"></a>Single Sign-On
Single Sign-On (SSO) dans le cadre de ce scénario est la fonctionnalité qui permet de réduire le nombre d’invites de mot de passe que l’utilisateur final doit entrer pour accéder aux ressources de société à partir de périphériques connus. Cette fonctionnalité implique que les utilisateurs sont invités qu’une seule fois pendant la durée de vie de l’authentification unique pour l’accès aux applications d’entreprise et de ressources à partir de cet appareil. Si un périphérique utilise l’espace de travail, l’utilisateur qui est inscrit pour utiliser cet appareil Obtient une authentification unique persistante par défaut pour les sept jours. Cet utilisateur a une transparente expérience de connexion dans la même session ou dans les nouvelles sessions.

## <a name="solution-overview"></a>Vue d’ensemble de la solution
Dans le cadre de cette solution, vous allez apprendre à utiliser la jonction d’espace sur un appareil pris en charge et de l’authentification unique pour accéder à une ressource d’entreprise.

> [!NOTE]
> Pour prendre en charge Windows8.1, les périphériques iOS 6.0 + et Android 4.0 +, vous devez configurer l’inscription de l’appareil Azure ActiveDirectory, ainsi que le périphérique objet écriture différée, voir [Guide pas à pas pour l’accès conditionnel local avec Azure ActiveDirectory Device Registration Service](https://msdn.microsoft.com/library/azure/dn788908.aspx)

Cette solution comprend vous à travers les étapes suivantes de la procédure pas à pas:

1.  [Procédure pas à pas: Joindre un espace de travail avec un appareil Windows](../../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)

2.  [Procédure pas à pas: Jonction avec un appareil iOS](../../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

3.  [Procédure pas à pas: Joindre un espace de travail avec un périphérique Android](../../ad-fs/operations/walkthrough--workplace-join-to-an-android-device.md)

## <a name="see-also"></a>Voir aussi
[Configurer un serveur de fédération avec Device Registration Service](../deployment/configure-a-federation-server-with-device-registration-service.md)



