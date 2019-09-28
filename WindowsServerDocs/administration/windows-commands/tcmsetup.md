---
title: tcmsetup
description: Découvrez comment configurer et désactiver le client TAPI.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 15e0c10f-996f-4301-92e5-943f7ee8212d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0c646acef51f06c57f16ec7e5310e3319a11383f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370680"
---
# <a name="tcmsetup"></a>tcmsetup



Configure ou désactive le client TAPI.

## <a name="syntax"></a>Syntaxe

```
tcmsetup [/q] [/x] /c <Server1> [<Server2> …] 
tcmsetup  [/q] /c /d
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/q|Empêche l’affichage des boîtes de message.|
|/x|Spécifie que les rappels orientés connexion seront utilisés pour les réseaux à trafic intense où la perte de paquets est élevée. Lorsque ce paramètre est omis, les rappels sans connexion sont utilisés.|
|/c|Obligatoire. Spécifie l’installation du client.|
|@no__t 0Server1 >|Obligatoire. Spécifie le nom du serveur distant disposant des fournisseurs de services TAPI que le client utilisera. Le client utilisera les lignes et les téléphones du fournisseur de services. Le client doit se trouver dans le même domaine que le serveur ou dans un domaine qui a une relation d’approbation bidirectionnelle avec le domaine qui contient le serveur.|
|\<Server2 >...|Spécifie tout serveur ou serveur supplémentaire qui sera disponible pour ce client. Si vous spécifiez une liste de serveurs, utilisez un espace pour séparer les noms de serveurs.|
|/d|Efface la liste des serveurs distants. Désactive le client TAPI en l’empêchant d’utiliser les fournisseurs de services TAPI qui se trouvent sur les serveurs distants.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs sur l’ordinateur local, ou l’autorité appropriée doit vous avoir été déléguée. Si l'ordinateur est joint à un domaine, les membres du groupe Admins du domaine peuvent être en mesure d'effectuer cette procédure. Pour des raisons de sécurité, il est conseillé d'utiliser la fonction **Exécuter en tant que** pour effectuer cette procédure.
-   Pour que l’interface TAPI fonctionne correctement, vous devez exécuter la commande **tcmsetup** pour spécifier les serveurs distants qui seront utilisés par les clients TAPI.
-   Pour qu’un utilisateur client puisse utiliser un téléphone ou une ligne sur un serveur TAPI, l’administrateur du serveur de téléphonie doit attribuer l’utilisateur au téléphone ou à la ligne.
-   La liste des serveurs de téléphonie créée par cette commande remplace toute liste existante de serveurs de téléphonie disponibles pour le client. Vous ne pouvez pas utiliser cette commande pour ajouter à la liste existante.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Vue d’ensemble de l’interface de commande](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)

[Spécifier les serveurs de téléphonie sur un ordinateur client](https://technet.microsoft.com/library/cc759226(v=ws.10).aspx)

[Affecter un utilisateur de téléphonie à une ligne ou à un téléphone](https://technet.microsoft.com/library/cc736875(v=ws.10).aspx)

