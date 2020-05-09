---
title: Dfsdiag testreferral
description: Rubrique de référence pour la commande Dfsdiag testreferral, qui vérifie les références système de fichiers DFS (DFS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 877c60dc-e993-4bd5-87dd-e892e3f98a1a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 23abcd738170d5f53e12ae83c41d632d2d7ac738
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992933"
---
# <a name="dfsdiag-testreferral"></a>Dfsdiag testreferral

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vérifie les références système de fichiers DFS (DFS) en effectuant les tests suivants :

- Si vous utilisez le paramètre **DFSpath*** sans arguments, la commande valide que la liste de références comprend tous les domaines approuvés.

- Si vous spécifiez un domaine, la commande effectue un contrôle d’intégrité des contrôleurs de domaine (`dfsdiag /testdcs`) et teste les associations de sites et le cache de domaine de l’hôte local.

- Si vous spécifiez un domaine et \SYSvol ou \NETLOGON, la commande effectue les mêmes contrôles d’intégrité de contrôleur de domaine, ainsi que la vérification que la durée de vie **(TTL)** des références Sysvol ou Netlogon correspond à la valeur par défaut de 900 secondes.

- Si vous spécifiez une racine d’espace de noms, la commande effectue les mêmes contrôles d’intégrité de contrôleur de domaine, ainsi`dfsdiag /testdfsconfig`que l’exécution d’une vérification de`dfsdiag /testdfsintegrity`la configuration DFS () et d’une vérification de l’intégrité de l’espace de noms ().

- Si vous spécifiez un dossier DFS (lien), la commande effectue les mêmes contrôles d’intégrité de la racine de l’espace de noms, ainsi que la validation de la configuration du site pour les cibles de dossier (Dfsdiag/testsites) et la validation de l’Association de site de l’hôte local.

## <a name="syntax"></a>Syntaxe

```
dfsdiag /testreferral /DFSpath:<DFS path to get referrals> [/full]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| /DFSpath:`<path to get referrals>` | Il peut s'agir d'une des méthodes suivantes :<ul><li>**Vide :** Teste uniquement les domaines approuvés.</li><li>`\\Domain:`Teste uniquement les références de contrôleur de domaine.</li><li>`\\Domain\SYSvol:`Teste uniquement les références de SYSvol.</li><li>`\\Domain\NETLOGON:`Teste uniquement les références NETLOGon.</li><li>`\\<domain or server>\<namespace root>:`Teste uniquement les références à la racine de l’espace de noms.</li><li>`\\<domain or server>\<namespace root>\<DFS folder>:`Teste uniquement les références du dossier DFS (lien).</li></ul> |
| /Full | S’applique uniquement aux références de domaine et racine. Vérifie la cohérence des informations d’association de sites entre le registre et les services de domaine Active Directory (AD DS). |

## <a name="examples"></a>Exemples

Pour vérifier les références du système de fichiers DFS (DFS) dans *contoso. com\MyNamespace*, tapez :

```
dfsdiag /testreferral /DFSpath:\\contoso.com\MyNamespace
```

Pour vérifier les références du système de fichiers DFS (DFS) dans tous les domaines approuvés, tapez :

```
dfsdiag /testreferral /DFSpath:
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Dfsdiag](dfsdiag.md)
