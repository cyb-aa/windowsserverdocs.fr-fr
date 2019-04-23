---
title: Bureau à distance - comparer les applications clientes
description: Découvrez comment les différentes applications de bureau à distance comparer lorsqu’il s’agit de fonctions et fonctionnalités prises en charge.
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
ms.date: 06/22/2018
ms.localizationpriority: medium
ms.openlocfilehash: 79a6b264c38b4b843c2887c6a3eb6f236480243d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828960"
---
# <a name="compare-the-client-apps"></a>Comparer les applications clientes

>S'applique à : Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2016

Nous mettons souvent demandé comment les différentes applications de client de bureau à distance par rapport à l’autre. Tous font la même chose ? Voici les réponses à ces questions.

## <a name="redirection-support"></a>Prise en charge la redirection

Le tableau suivant compare la prise en charge des appareils et autres redirections sur l’application de connexion Bureau à distance application universelle, application Android, application iOS, les clients web et application macOS. Ces tables couvrent les redirections que vous pouvez accéder à la fois dans une session à distance. 

Si vous à distance dans votre bureau personnel, il existe des redirections supplémentaires que vous pouvez configurer dans le **des paramètres supplémentaires** pour la session. Si vos applications ou le Bureau à distance sont gérées par votre organisation, votre administrateur peut activer ou désactiver des redirections via les paramètres de stratégie de groupe.

### <a name="input-redirection"></a>Redirection d’entrée

| Redirection | Bureau à distance<br> Connexion | Universel | Android | iOS | macOS | client Web |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| Clavier    | X                             | X         | X       | X   | X     | X          |
| Souris       | X                             | X         | X       | X*    | X     | X          |
| Touch       | X                             | X         | X       | X   |       |            |
| Autre       | Stylet                           |           |         |     |       |            |
* Afficher les [liste des périphériques d’entrée pris en charge pour le client de bêta de bureau à distance iOS](remote-desktop-ios.md#supported-input-devices).

### <a name="port-redirection"></a>Redirection de port   

| Redirection | Bureau à distance <br>Connexion | Universel | Android | iOS | macOS | client Web |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| Port série | X                             |           |         |     |       |            |
| USB         | X                             |           |         |     |       |            |

Lorsque vous activez la redirection de port USB, tous les périphériques USB au port USB sont automatiquement reconnus dans la session à distance.

### <a name="other-redirection-devices-etc"></a>Autre redirection (périphériques, etc.)



| Redirection         | Connexion Bureau à distance | Universel   | Android | iOS         | macOS                                    | client Web    |
|---------------------|---------------------------|-------------|---------|-------------|------------------------------------------|---------------|
| Appareils photo             | X                         |             |         |             |                                          |               |
| Presse-papiers           | X                         | texte, image | text    | texte, image | X                                        | text          |
| Lecteur/stockage local | X                         |             | X       |             | x                                        |               |
| Location            | X                         |             |         |             |                                          |               |
| Microphones         | X                         |X            |         |             | X                                        |               |
| Imprimantes            | X                         |             |         |             | X (tasses uniquement)                            | Impression PDF     |
| Scanneurs            | X                         |             |         |             |                                          |               |
| Cartes à puce         | X                         |             |         |             | X (ne pas pris en charge l’authentification Windows) |               |
| Haut-parleurs            | X                         | X           | X       | X           | X                                        | X (à l’exception d’Internet Explorer) |

* La redirection d’imprimante - l’application macOS prend en charge le pilote d’imprimante photocomposeuse de serveur de publication par défaut. Ils ne gèrent pas la redirection des pilotes d’imprimante natif.
