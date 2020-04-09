---
title: Dfsdiag TestReferral
description: La rubrique commandes Windows pour Dfsdiag TestReferral, qui vérifie les références système de fichiers DFS (DFS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 877c60dc-e993-4bd5-87dd-e892e3f98a1a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d5c0a75d557d816ac9e19a1e22b3273195b93f53
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846243"
---
# <a name="dfsdiag-testreferral"></a>Dfsdiag TestReferral

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
| /DFSpath :<path for getting referrals>|Ce chemin d’accès DFS peut être l’un des suivants :<p>-   \(\)vide : teste les domaines approuvés.<br />-   \\domaine \\: références de contrôleur de domaine.<br />-   \\\\domaine\\SYSvol : références SYSvol.<br />-   \\\\Doma dans\\références Netlogon : NETLOGon.<br />-   \\\\<Domain or server>\\<Namespace Root>: références de la racine de l’espace de noms.<br />-   \\\\<Domain or server>\\<Namespace root>\\<DFS folder>: dossier DFS \(des références\) des références.|
|/Full|S’applique uniquement aux références de domaine et racine. vérifie la cohérence des informations d’association de site entre le registre et les services de domaine Active Directory \(AD DS\).|

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

```
dfsdiag /TestReferral /DFSpath:\\Contoso.com\MyNamespace
```

```
dfsdiag /TestReferral /DFSpath:
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)


