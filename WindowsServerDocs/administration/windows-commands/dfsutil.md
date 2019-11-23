---
title: Dfsutil
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef5093a4-0d24-4b21-9d04-59933ad98e2c
robots: noindex,nofollow
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1a06806b109bbd324213f935892bbbab415362df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377978"
---
# <a name="dfsutil"></a>Dfsutil

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La commande dfsutil gère les espaces de noms, les serveurs et les clients DFS. les commandes dfsutil utilisent la terminologie d’origine système de fichiers DFS, avec la terminologie mise à jour des espaces de noms DFS fournie comme explication pour la plupart des commandes.

pour obtenir des exemples d’utilisation de cette commande, consultez. 

## <a name="syntax"></a>Syntaxe

```
command </parameter> </param2>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|[dfsutil root](dfsutil-root.md)|Affiche, crée, supprime, importe, exporte des racines d’espace de noms.|
|[Lien Dfsutil](dfsutil-link.md)|Affiche, crée, supprime ou déplace des dossiers \(des liens\).|
|[dfsutil target](dfsutil-target.md)|Affiche, crée, supprime une cible de dossier ou un serveur d’espaces de noms.|
|[dfsutil, propriété](dfsutil-property.md)|Affiche ou modifie une cible de dossier ou un serveur d’espaces de noms.|
|[dfsutil client](dfsutil-client.md)|Affiche ou modifie des informations sur le client ou des clés de registre.|
|[dfsutil Server](dfsutil-server.md)|Affiche ou modifie la configuration de l’espace de noms.|
|[dfsutil diag](dfsutil-diag.md)|Effectuez des diagnostics ou affichez dfsdirs\/dfspath.|
|[dfsutil Domain](dfsutil-domain.md)|Affiche tous les espaces de noms basés sur les\-de domaine dans un domaine.|
|[dfsutil cache](dfsutil-cache.md)|Affiche ou vide le cache du client.|
|[dfsutil oldcli](dfsutil-oldcli.md)|Utilisez la commande dfsutil \/oldcli pour utiliser la syntaxe dfsutil d’origine.|

## <a name="remarks-optional-section"></a>Remarques <optional section>
Si vous spécifiez un objet \(comme un serveur d’espaces de noms\) à la fin d’une commande, la plupart des commandes affichent des informations sur l’objet sans nécessiter de paramètres ou de commandes supplémentaires. Par exemple, lors de l’utilisation de la commande dfsutil root, vous pouvez ajouter une racine d’espace de noms à la commande pour afficher des informations sur la racine.

## <a name="BKMK_Examples"></a>Illustre
&lt;ici, vous trouverez une description détaillée de votre exemple.&gt;

```
This /is /the /example /of /calling /command /with /parameters
```

&lt;ici, vous trouverez une description détaillée d’un autre exemple.&gt;

```
This /is /a:different /example
```

## <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)


