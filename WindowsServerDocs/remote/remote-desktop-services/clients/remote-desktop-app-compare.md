---
title: Bureau à distance - Comparer les clients
description: Comparez les fonctions et fonctionnalités prises en charge dans les différents clients Bureau à distance.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 12efe858-6b76-4e08-9f72-b9603aceb0fc
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 03/28/2020
ms.localizationpriority: medium
ms.openlocfilehash: 11c91ac951db27915d9313f7f98e5e2cfc56b726
ms.sourcegitcommit: 78c00944b6990702d28bdcc4a9215927ca901bfb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80440370"
---
# <a name="compare-the-clients"></a>Comparer les clients

>S'applique à : Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Il nous est souvent demandé de comparer les différents clients Bureau à distance. Font-ils tous la même chose ? Voici les réponses à ces questions.

## <a name="redirection-support"></a>Prise en charge de la redirection

Les tableaux suivants comparent la prise en charge des appareils et autres redirections sur les différents clients. Ces tableaux couvrent les redirections auxquelles vous pouvez accéder dans une session à distance.

Si vous vous connectez à distance à votre bureau personnel, il existe des redirections supplémentaires que vous pouvez configurer dans les **paramètres supplémentaires** pour la session. Si votre bureau à distance ou vos applications sont gérés par votre organisation, votre administrateur peut activer ou désactiver des redirections à l’aide des paramètres de stratégie de groupe ou des propriétés RDP.

### <a name="input-redirection"></a>Redirection d’entrée

| Redirection | Boîte de réception Windows</br>(MSTSC) | Bureau Windows</br>(MSRDC) | Windows Store | Android | iOS | macOS | Client web    |
|-------------|---------------------------|-----------------------------|---------------|---------|-----|-------|---------------|
| Clavier    | X                         | X                           | X             | X       | X   | X     | X             |
| Souris       | X                         | X                           | X             | X       | X\* | X     | X             |
| Toucher       | X                         | X                           | X             | X       | X   |       | X (sauf IE) |
| Stylet         | X                         | X                           |               |         |     |       |               |

*Consultez la [liste des périphériques d’entrée pris en charge pour le client Bureau à distance iOS](remote-desktop-ios.md#supported-input-devices).

### <a name="port-redirection"></a>Redirection de port

| Redirection | Boîte de réception Windows</br>(MSTSC) | Bureau Windows</br>(MSRDC) | Windows Store | Android | iOS | macOS | Client web |
|-------------|---------------------------|-----------------------------|---------------|---------|-----|-------|------------|
| Port série | X                         | X                           |               |         |     |       |            |
| USB         | X                         | X                           |               |         |     |       |            |

Quand vous activez la redirection de port USB, tous les périphériques USB attachés au port USB sont automatiquement reconnus dans la session à distance.

### <a name="other-redirection-devices-etc"></a>Autre redirection (périphériques, etc.)

| Redirection         | Boîte de réception Windows</br>(MSTSC) | Bureau Windows</br>(MSRDC) | Windows Store | Android | iOS         | macOS                           | Client web    |
|---------------------|---------------------------|-----------------------------|---------------|---------|-------------|---------------------------------|---------------|
| Appareils photo             | X                         | X                           |               |         |             | X                               |               |
| Presse-papiers           | X                         | X                           | X             | texte    | texte, image | X                               | texte          |
| Disque/stockage local | X                         | X                           |               | X       |             | X                               |               |
| Emplacement            | X                         | X                           |               |         |             |                                 |               |
| Microphones         | X                         | X                           | X             |         |             | X                               |               |
| Imprimantes            | X                         | X                           |               |         |             | X (CUPS uniquement)                   | Impression PDF     |
| Scanneurs            | X                         | X                           |               |         |             |                                 |               |
| Smart Cards         | X                         | X                           |               |         |             | X (ouverture de session Windows non prise en charge) |               |
| Haut-parleurs            | X                         | X                           | X             | X       | X           | X                               | X (sauf IE) |

*Pour la redirection d’imprimante, l’application macOS prend en charge le pilote d’imprimante Photocomposeuse Publisher par défaut. La redirection pour les pilotes d’imprimante natifs n’est pas prise en charge.
