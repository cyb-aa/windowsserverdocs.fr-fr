---
title: L’ordinateur portable distant se déconnecte du réseau sans fil
description: 'Résolution du problème suivant : l’ordinateur portable distant se déconnecte du réseau sans fil.'
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: df43a69fa4777a9286cbe27cfa2d241111f7edf6
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265871"
---
# <a name="remote-laptop-disconnects-from-wireless-network"></a>L’ordinateur portable distant se déconnecte du réseau sans fil

Ce problème peut se produire quand un client Bureau à distance se connecte à un ordinateur portable par le biais d’un réseau sans fil 802.1x. Par intermittence, l’ordinateur portable se déconnecte du réseau sans fil et ne se reconnecte pas automatiquement.

Il s’agit d’un problème connu qui se produit quand le paramètre d’authentification réseau pour la connexion réseau sans fil est **Authentification de l’utilisateur**.

Pour contourner ce problème, affectez la valeur **Authentification de l’utilisateur ou de l’ordinateur** ou **Authentification de l’ordinateur** au paramètre d’authentification réseau.

 > [!NOTE]  
> Pour modifier les paramètres d’authentification réseau sur un seul ordinateur, vous devrez peut-être utiliser le panneau de configuration Centre réseau et partage afin de créer une nouvelle connexion sans fil avec les nouveaux paramètres.

Pour obtenir une description complète de la façon de configurer les paramètres de réseau sans fil à l’aide d’objets de stratégie de groupe, consultez [Configurer des stratégies de réseau sans fil (IEEE 802.11)](../../../networking/core-network-guide/cncg/wireless/e-wireless-access-deployment.md#bkmk_policies).
