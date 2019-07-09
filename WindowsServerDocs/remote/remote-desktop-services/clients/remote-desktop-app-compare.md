---
title: Bureau à distance - Comparer les applications clientes
description: Comparez les fonctions et fonctionnalités prises en charge dans les différentes applications Bureau à distance.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 12efe858-6b76-4e08-9f72-b9603aceb0fc
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 05/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 0e001b590f524711185e3dd70db3bc52a9b8d9af
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66447125"
---
# <a name="compare-the-client-apps"></a>Comparer les applications clientes

>S’applique à : Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

On nous demande souvent de comparer les différentes applications clientes Bureau à distance entre elles. Font-elles toutes la même chose ? Voici les réponses à ces questions.

## <a name="redirection-support"></a>Prise en charge de la redirection

Les tableaux suivants comparent la prise en charge de la redirection d’appareil et d’autres redirections sur l’application Connexion Bureau à distance, l’application universelle, l’application Android, l’application iOS, l’application macOS et le client web. Ces tableaux couvrent les redirections auxquelles vous pouvez accéder dans une session à distance. 

Si vous vous connectez à distance à votre bureau personnel, il existe des redirections supplémentaires que vous pouvez configurer dans les **paramètres supplémentaires** pour la session. Si votre bureau à distance ou vos applications sont gérés par votre organisation, votre administrateur peut activer ou désactiver des redirections à l’aide des paramètres de stratégie de groupe.

### <a name="input-redirection"></a>Redirection d’entrée

| Redirection | Bureau à distance<br> Connexion | Universel | Android | iOS | macOS |          Client web           |
|-------------|-------------------------------|-----------|---------|-----|-------|-------------------------------|
|  Clavier   |               X               |     X     |    X    |  X  |   X   |               X               |
|    Souris    |               X               |     X     |    X    | X\* |   X   |               X               |
|    Touch    |               X               |     X     |    X    |  X  |       | X (Edge et IE non pris en charge) |
|    Autre    |              Stylet              |           |         |     |       |                               |

*Consultez la [liste des périphériques d’entrée pris en charge pour le client bêta Bureau à distance iOS](remote-desktop-ios.md#supported-input-devices).

### <a name="port-redirection"></a>Redirection de port   

| Redirection | Bureau à distance <br>Connexion | Universel | Android | iOS | macOS | Client web |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| Port série | X                             |           |         |     |       |            |
| USB         | X                             |           |         |     |       |            |

Quand vous activez la redirection de port USB, tous les périphériques USB attachés au port USB sont automatiquement reconnus dans la session à distance.

### <a name="other-redirection-devices-etc"></a>Autre redirection (périphériques, etc.)



| Redirection         | Connexion Bureau à distance | Universel   | Android | iOS         | macOS                                    | Client web    |
|---------------------|---------------------------|-------------|---------|-------------|------------------------------------------|---------------|
| Appareils photo             | X                         |             |         |             |                                          |               |
| Presse-papiers           | X                         | texte, image | texte    | texte, image | X                                        | texte          |
| Disque/stockage local | X                         |             | X       |             | x                                        |               |
| Emplacement            | X                         |             |         |             |                                          |               |
| Microphones         | X                         |X            |         |             | X                                        |               |
| Imprimantes            | X                         |             |         |             | X (CUPS uniquement)                            | Impression PDF     |
| Scanneurs            | X                         |             |         |             |                                          |               |
| Cartes à puce         | X                         |             |         |             | X (Authentification Windows non prise en charge) |               |
| Haut-parleurs            | X                         | X           | X       | X           | X                                        | X (sauf IE) |

*Pour la redirection d’imprimante, l’application macOS prend en charge le pilote d’imprimante Photocomposeuse Publisher par défaut. La redirection pour les pilotes d’imprimante natifs n’est pas prise en charge.
