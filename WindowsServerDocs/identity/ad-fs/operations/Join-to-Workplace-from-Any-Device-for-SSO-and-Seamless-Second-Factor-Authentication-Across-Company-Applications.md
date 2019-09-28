---
ms.assetid: e22d84a5-113d-4bec-b484-036ed29f0c28
title: joindre un espace de travail à partir de n'importe quel appareil en utilisant l'authentification unique et l'authentification de second facteur transparente pour accéder aux applications de l'entreprise
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 12/05/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 05a8b8bb5685c31ece29ed8eb38d37a424e478b8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357937"
---
# <a name="join-to-workplace-from-any-device-for-sso-and-seamless-second-factor-authentication-across-company-applications"></a>joindre un espace de travail à partir de n'importe quel appareil en utilisant l'authentification unique et l'authentification de second facteur transparente pour accéder aux applications de l'entreprise



L'augmentation rapide du nombre d'appareils grand public et l'accès omniprésent aux informations modifient la façon dont les utilisateurs perçoivent leur technologie. L'utilisation constante de la technologie des informations tout au long de la journée, ainsi que la facilité d'accès aux informations, brouillent les barrières traditionnelles entre la vie professionnelle et la vie familiale. Ces limites de décalage sont accompagnées d’une conviction que les technologies personnelles sélectionnées et personnalisées en fonction des personnalités, des activités et des planifications des utilisateurs doivent s’étendre à l’espace de travail. Pour prendre en compte le besoin croissant de pouvoir connecter les appareils grand public personnels aux réseaux d’entreprise, nous introduisons les propositions de valeur suivantes :

-   Les administrateurs peuvent contrôler qui a accès aux ressources de l'entreprise en fonction de l'application, de l'utilisateur, de l'appareil et de l'emplacement.

-   Les employés peuvent accéder aux applications et aux données en tout point, depuis n'importe quel appareil. Les employés peuvent utiliser l'authentification unique dans les applications de navigateur ou d'entreprise.

## <a name="key-concepts-introduced-in-the-solution"></a>Concepts clés introduits dans la solution

### <a name="workplace-join"></a>Jonction à l'espace de travail
Grâce à la jonction d'espace de travail, les travailleurs de l'information peuvent établir une connexion entre leurs appareils personnels et les ordinateurs de leur espace de travail au sein d'entreprise pour accéder aux ressources et services de celle-ci. Quand vous joignez votre appareil personnel à votre espace de travail, il devient un appareil connu et offre un accès aux ressources et applications de l'espace de travail via l'authentification de second facteur transparente et l'authentification unique. Quand un appareil est rattaché au moyen de la jonction d'espace de travail, ses attributs peuvent être récupérés de l'annuaire pour déterminer les conditions d'accès aux applications et l'émission correspondante des jetons de sécurité. Les appareils Windows 8.1, iOS 6.0 (et versions ultérieures) et Android 4.0 (et versions ultérieures) peuvent être rattachés au moyen de la jonction d’espace de travail.

### <a name="BKMK_DRS"></a>Service Azure Active Directory Device Registration
Le service DRS (Device Registration Service) Azure Active Directory permet de mettre en œuvre une jonction d’espace de travail. Quand un appareil grand public est rattaché au moyen de la jonction d’espace de travail, le service fournit un objet d’appareil dans Azure Active Directory et définit sur l’appareil local une clé représentant son identité. Cette identité d’appareil peut ensuite être utilisée avec des règles de contrôle d’accès pour les applications hébergées localement et dans le cloud.

Pour plus d’informations, consultez [Présentation de la gestion des appareils dans Azure Active Directory](https://docs.microsoft.com/azure/active-directory/device-management-introduction).

### <a name="workplace-join-as-a-seamless-second-factor-authentication"></a>Jonction d'espace de travail avec l'authentification de second facteur transparente
Les entreprises peuvent gérer les risques liés à l'accès aux informations et établir des règles de gouvernance et de conformité tout en permettant aux appareils grand public d'accéder aux ressources de l'entreprise. La jonction d'espace de travail sur les appareils offre aux administrateurs les fonctionnalités suivantes :

-   Elle identifie les appareils connus avec une authentification d'appareil. Les administrateurs peuvent utiliser ces informations pour déterminer les conditions d'accès et contrôler l'accès aux ressources.

-   Elle procure une expérience de connexion plus transparente aux utilisateurs qui accèdent aux ressources de l'entreprise depuis des appareils approuvés.

### <a name="single-sign-on"></a>Authentification unique
Dans le contexte de ce scénario, l'authentification unique permet de réduire le nombre d'invites de mot de passe que l'utilisateur final doit renseigner pour accéder aux ressources de l'entreprise depuis un appareil connu. Cette fonctionnalité implique que l'utilisateur n'est invité à s'identifier qu'une seule fois pendant la durée de vie de l'authentification unique pour accéder aux applications et ressources de l'entreprise depuis cet appareil. Si un appareil utilise la jonction d'espace de travail, l'utilisateur qui est inscrit pour utiliser cet appareil bénéficie d'une authentification unique persistante pendant une durée par défaut de sept jours. Cet utilisateur profite d'une expérience de connexion transparente dans la même session ou dans de nouvelles sessions.

## <a name="solution-overview"></a>Vue d'ensemble de la solution
Dans le cadre de cette solution, vous apprenez à utiliser la jonction d’espace de travail sur un appareil pris en charge, et vous expérimentez l’authentification unique pour accéder à une ressource d’entreprise.

> [!NOTE]
> Pour prendre en charge les appareils Windows 8.1, iOS 6.0 (et versions ultérieures) et Android 4.0 (et versions ultérieures), vous devez configurer le service DRS (Device Registration Service) Azure Active Directory avec l’écriture différée de l’objet d’appareil ; voir [Guide pas à pas sur l’accès conditionnel local avec le service DRS (Device Registration Service) Azure Active Directory](https://msdn.microsoft.com/library/azure/dn788908.aspx)

Cette solution comprend les procédures pas à pas suivantes :

1.  [Procédure pas à pas : Joindre un espace de travail avec un appareil Windows](../../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)

2.  [Procédure pas à pas : Joindre un espace de travail avec un appareil iOS](../../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

3.  [Procédure pas à pas : Rattacher à l’espace de travail avec un appareil Android](../../ad-fs/operations/walkthrough--workplace-join-to-an-android-device.md)

## <a name="see-also"></a>Voir aussi
[Configurer un serveur de Fédération avec Device Registration service](../deployment/configure-a-federation-server-with-device-registration-service.md)



