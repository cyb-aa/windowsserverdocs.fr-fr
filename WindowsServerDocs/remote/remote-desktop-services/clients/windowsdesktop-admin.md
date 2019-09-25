---
title: Client Windows Desktop pour les administrateurs
description: Informations sur le client Windows Desktop principalement utiles aux administrateurs.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: daveba
ms.author: helohr
ms.date: 09/16/2019
ms.localizationpriority: medium
ms.openlocfilehash: 86cadefb9733ca7293c4fd99c270b35a41799e1a
ms.sourcegitcommit: 081661f50d6dafb77180149956a02e679270c710
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71038707"
---
# <a name="windows-desktop-client-for-admins"></a>Client Windows Desktop pour les administrateurs

>S’applique à : Windows 10 et Windows 7

Cette rubrique contient des informations supplémentaires sur le client Windows Desktop que les administrateurs peuvent trouver utiles. Pour plus d’informations de base sur l’utilisation, consultez [Bien démarrer avec le client Windows Desktop](windowsdesktop.md).

## <a name="installation-options"></a>Options d’installation

Bien que les utilisateurs puissent installer le client directement après l’avoir téléchargé, si vous effectuez un déploiement sur plusieurs appareils, vous souhaiterez peut-être également y déployer le client par d’autres moyens. Le déploiement à l’aide de stratégies de groupe ou de System Center Configuration Manager vous permet d’exécuter le programme d’installation en mode silencieux à l’aide d’une ligne de commande. Exécutez les commandes suivantes pour déployer le client par appareil ou par utilisateur.

### <a name="per-device-installation"></a>Installation par appareil

```
msiexec.exe /I <path to the MSI> /qn ALLUSERS=1
```

### <a name="per-user-installation"></a>Installation par utilisateur

```
msiexec.exe /i `<path to the MSI>` /qn ALLUSERS=2 MSIINSTALLPERUSER=1
```

## <a name="configuration-options"></a>Options de configuration

Cette section décrit les nouvelles options de configuration pour ce client.

### <a name="configure-update-notifications"></a>Configurer les notifications de mise à jour

Par défaut, le client vous avertit chaque fois qu’une mise à jour existe. Pour désactiver les notifications, définissez les informations de Registre suivantes :

- **Clé :** HKLM\Software\Microsoft\MSRDC\Policies
- **Type :** REG_DWORD
- **Nom :** AutomaticUpdates
- **Données :** 0 = Désactiver les notifications. 1 = Afficher les notifications.

### <a name="configure-user-groups"></a>Configurer des groupes d’utilisateurs

Vous pouvez configurer le client pour l’un des types suivants de groupes d’utilisateurs, ce qui détermine le moment où le client reçoit les mises à jour.

#### <a name="insider-group"></a>Groupe Insider

Le groupe Insider est utilisé pour effectuer une validation précoce et se compose d’administrateurs et de leurs utilisateurs sélectionnés. Le groupe Insider sert de série de tests pour détecter les problèmes de la mise à jour qui peuvent impacter les performances avant sa publication dans le groupe Public.

> [!NOTE]
> Nous recommandons à chaque organisation d’avoir des utilisateurs dans le groupe Insider pour tester les mises à jour et détecter les problèmes de manière précoce.

Dans le groupe Insider, une nouvelle version du client est publiée pour les utilisateurs le deuxième mardi de chaque mois à des fins de validation anticipée. Si la mise à jour ne présente pas de problèmes, elle est publiée dans le groupe Public deux semaines plus tard. Les utilisateurs du groupe Insider reçoivent automatiquement des notifications de mise à jour chaque fois que des mises à jour sont prêtes. Vous trouverez des informations plus détaillées sur les modifications apportées au client dans la page [Nouveautés du client Windows Desktop](windowsdesktop-whatsnew.md).

Pour configurer le client pour le groupe Insider, définissez les informations de Registre suivantes :

- **Clé :** HKLM\Software\Microsoft\MSRDC\Policies
- **Type :** REG_SZ
- **Nom :** ReleaseRing
- **Données :** insider

#### <a name="public-group"></a>Groupe Public

Ce groupe est destiné à tous les utilisateurs et est la version la plus stable. Vous n’avez rien à faire pour configurer ce groupe.

Le groupe public reçoit chaque quatrième mardi de chaque mois la version du client qui a été testée par le groupe Insider. Tous les utilisateurs du groupe Public reçoivent une notification de mise à jour si ce paramètre est activé.
