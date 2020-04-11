---
title: Dfsutil
description: La rubrique commandes Windows pour dfsutil, qui gère les espaces de noms DFS, les serveurs et les clients. les commandes dfsutil utilisent la terminologie d’origine système de fichiers DFS, avec la terminologie mise à jour des espaces de noms DFS fournie comme explication pour la plupart des commandes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef5093a4-0d24-4b21-9d04-59933ad98e2c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47d468ee122dc78cc880f4a9bc0705354e0b5214
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122557"
---
# <a name="dfsutil"></a>Dfsutil

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La commande dfsutil gère les espaces de noms, les serveurs et les clients DFS.

>[!NOTE]
>Le **module PowerShell d’espaces de noms DFS** fournit des remplacements pour certains des paramètres dfsutil, tandis que d’autres nécessitent encore que vous utilisiez Dfsutil. Pour plus d’informations sur les équivalents PowerShell mis à jour, consultez [DFSN](https://docs.microsoft.com/powershell/module/dfsn/?view=win10-ps).

## <a name="parameters-available-in-powershell"></a>Paramètres disponibles dans PowerShell

Vous pouvez utiliser les paramètres suivants à partir de PowerShell :

| Paramètre | Description |
| --------- | ----------- |
| root | Affiche, crée, supprime, importe, exporte des racines d’espace de noms. |
| lien | Affiche, crée, supprime ou déplace des dossiers (liens). |
| target | Affiche, crée, supprime une cible de dossier ou un serveur d’espaces de noms. |
| propriété | Affiche ou modifie une cible de dossier ou un serveur d’espaces de noms. |
| serveur | Affiche ou modifie la configuration de l’espace de noms. |
| domain | Affiche tous les espaces de noms basés sur un domaine dans un domaine. |

## <a name="parameters-only-available-in-dfsutil"></a>Paramètres uniquement disponibles dans Dfsutil

Vous ne pouvez utiliser les paramètres suivants qu’à partir de Dfsutil.

| Paramètre | Description |
| --------- | ----------- |
| client | Affiche ou modifie des informations sur le client ou des clés de registre. |
| diag | Effectuez des diagnostics ou affichez dfsdirs/dfspath. |
| en | Affiche ou vide le cache du client. |

Pour plus d’informations sur chacune de ces commandes, ouvrez une invite de commandes sur un serveur sur lequel sont installés les outils de gestion des espaces de noms DFS, puis tapez `dfsutil client /?`, `dfsutil diag /?`ou `dfsutil cache /?`.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)