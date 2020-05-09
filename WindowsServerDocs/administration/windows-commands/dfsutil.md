---
title: Dfsutil
description: Rubrique de référence pour la commande dfsutil, qui gère les espaces de noms DFS, les serveurs et les clients.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef5093a4-0d24-4b21-9d04-59933ad98e2c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6905d90ee42958e47dfed4869520000a4fd3ddf
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992622"
---
# <a name="dfsutil"></a>Dfsutil

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La commande dfsutil gère les espaces de noms, les serveurs et les clients DFS.

## <a name="functionality-available-in-powershell"></a>Fonctionnalités disponibles dans PowerShell

Le module PowerShell [DFSN](https://docs.microsoft.com/powershell/module/dfsn/?view=win10-ps) offre des fonctionnalités équivalentes aux paramètres dfsutil suivants.

| Paramètre | Description |
| --------- | ----------- |
| root | Affiche, crée, supprime, importe, exporte des racines d’espace de noms. |
| link | Affiche, crée, supprime ou déplace des dossiers (liens). |
| target | Affiche, crée, supprime une cible de dossier ou un serveur d’espaces de noms. |
| propriété | Affiche ou modifie une cible de dossier ou un serveur d’espaces de noms. |
| server | Affiche ou modifie la configuration de l’espace de noms. |
| domaine | Affiche tous les espaces de noms basés sur un domaine dans un domaine. |

## <a name="functionality-available-only-in-dfsutil"></a>Fonctionnalités disponibles uniquement dans Dfsutil

Les fonctionnalités suivantes sont uniquement disponibles en tant que paramètres dfsutil :

| Paramètre | Description |
| --------- | ----------- |
| client | Affiche ou modifie des informations sur le client ou des clés de registre. |
| diag | Effectuez des diagnostics ou affichez dfsdirs/dfspath. |
| cache | Affiche ou vide le cache du client. |

Pour plus d’informations sur chacune de ces commandes, ouvrez une invite de commandes sur un serveur sur lequel les outils de gestion des espaces de noms `dfsutil client /?`DFS `dfsutil diag /?`sont installés `dfsutil cache /?`, puis tapez, ou.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)