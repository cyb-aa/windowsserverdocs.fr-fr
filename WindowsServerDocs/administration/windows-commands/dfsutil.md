---
title: Dfsutil
description: La rubrique de référence pour dfsutil, qui gère les espaces de noms DFS, les serveurs et les clients. les commandes dfsutil utilisent la terminologie d’origine système de fichiers DFS, avec la terminologie mise à jour des espaces de noms DFS fournie comme explication pour la plupart des commandes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef5093a4-0d24-4b21-9d04-59933ad98e2c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 999eef79227d4531ba724c9cac40127297ea38a0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719516"
---
# <a name="dfsutil"></a>Dfsutil

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La commande dfsutil gère les espaces de noms, les serveurs et les clients DFS.

>[!NOTE]
>Le **module PowerShell d’espaces de noms DFS** fournit des remplacements pour certains des paramètres dfsutil, tandis que d’autres nécessitent encore que vous utilisiez Dfsutil. Pour plus d’informations sur les équivalents PowerShell mis à jour, consultez [DFSN](https://docs.microsoft.com/powershell/module/dfsn/?view=win10-ps).

## <a name="parameters-available-in-powershell"></a>Paramètres disponibles dans PowerShell

Vous pouvez utiliser les paramètres suivants à partir de PowerShell :

| Paramètre | Description |
| --------- | ----------- |
| root | Affiche, crée, supprime, importe, exporte des racines d’espace de noms. |
| link | Affiche, crée, supprime ou déplace des dossiers (liens). |
| target | Affiche, crée, supprime une cible de dossier ou un serveur d’espaces de noms. |
| propriété | Affiche ou modifie une cible de dossier ou un serveur d’espaces de noms. |
| server | Affiche ou modifie la configuration de l’espace de noms. |
| domaine | Affiche tous les espaces de noms basés sur un domaine dans un domaine. |

## <a name="parameters-only-available-in-dfsutil"></a>Paramètres uniquement disponibles dans Dfsutil

Vous ne pouvez utiliser les paramètres suivants qu’à partir de Dfsutil.

| Paramètre | Description |
| --------- | ----------- |
| client | Affiche ou modifie des informations sur le client ou des clés de registre. |
| diag | Effectuez des diagnostics ou affichez dfsdirs/dfspath. |
| cache | Affiche ou vide le cache du client. |

Pour plus d’informations sur chacune de ces commandes, ouvrez une invite de commandes sur un serveur sur lequel les outils de gestion des espaces de noms `dfsutil client /?`DFS `dfsutil diag /?`sont installés `dfsutil cache /?`, puis tapez, ou.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)