---
title: Dfsdiag TestReferral
description: Rubrique de référence pour Dfsdiag TestReferral, qui vérifie les références système de fichiers DFS (DFS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 877c60dc-e993-4bd5-87dd-e892e3f98a1a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b4c616181d367a8a95efe6484f74af0ff88cc5f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719566"
---
# <a name="dfsdiag-testreferral"></a>Dfsdiag TestReferral

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vérifie les références système de fichiers DFS (DFS) en effectuant les tests suivants :

- Quand vous utilisez le paramètre DFSpath sans arguments, cette commande valide que la liste de références comprend tous les domaines approuvés.

- Lorsque vous spécifiez un domaine, la commande effectue un contrôle d’intégrité des contrôleurs de domaine (Dfsdiag/testdcs) et teste les associations de sites et le cache de domaine de l’hôte local.

- Lorsque vous spécifiez un domaine et \SYSvol ou \NETLOGON, en plus d’effectuer les mêmes vérifications d’intégrité que lorsque vous spécifiez un domaine, la commande vérifie que la durée de vie (\TTL) des références SYSvol ou NETLOGon correspond à la valeur par défaut de 900 secondes.

- Lorsque vous spécifiez une racine d’espace de noms, en plus d’effectuer les mêmes vérifications d’intégrité que lorsque vous spécifiez un domaine, la commande effectue une vérification de la configuration DFS (Dfsdiag/TestDFSConfig) et une vérification de l’intégrité de l’espace de noms (Dfsdiag/TestDFSIntegrity).

- Lorsque vous spécifiez un dossier DFS (lien), en plus d’effectuer les mêmes vérifications d’intégrité que lorsque vous spécifiez une racine d’espace de noms, la commande valide la configuration du site pour les cibles de dossier (Dfsdiag/testsites) et valide l’Association de site de l’hôte local.

## <a name="syntax"></a>Syntaxe

```
dfsdiag /TestReferral /DFSpath:<DFS path for getting referrals> [/Full]
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
| /DFSpath:<path for getting referrals>|Ce chemin d’accès DFS peut être l’un des suivants :<p>-   \(vide\): teste les domaines approuvés.<br />-   \\\\Domaine : références de contrôleur de domaine.<br />-   \\\\Domaine\\SYSVOL : références SYSVOL.<br />-   \\\\Doma dans\\Netlogon : références Netlogon.<br />-   \\\\<Domain or server>\\<Namespace Root>: Références de la racine de l’espace de noms.<br />-   \\\\<Domain or server>\\<Namespace root>\\<DFS folder>: Références de \(lien\) de dossier DFS.|
|/Full|S’applique uniquement aux références de domaine et racine. vérifie la cohérence des informations d’association de site entre le registre et les services \(de domaine\)Active Directory AD DS.|

## <a name="examples"></a>Exemples

```
dfsdiag /TestReferral /DFSpath:\\Contoso.com\MyNamespace
```

```
dfsdiag /TestReferral /DFSpath:
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)


