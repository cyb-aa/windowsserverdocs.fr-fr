---
title: tcmsetup
description: Découvrez comment configurer et désactiver le client de l’interface TAPI.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ac92c7b793274227bd20e6fa90a4106a32ea0446
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862000"
---
# <a name="tcmsetup"></a>tcmsetup



Définit ou désactive le client TAPI.

## <a name="syntax"></a>Syntaxe

```
tcmsetup [/q] [/x] /c <Server1> [<Server2> …] 
tcmsetup  [/q] /c /d
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/q|Empêche l’affichage de boîtes de message.|
|/x|Spécifie que rappels orientés connexion seront utilisés pour les réseaux de trafic dense où la perte de paquets est élevée. Lorsque ce paramètre est omis, les rappels sans connexion seront utilisés.|
|/c|Obligatoire. Spécifie le programme d’installation du client.|
|\<Serveur1 >|Obligatoire. Spécifie le nom du serveur distant qui a les fournisseurs de services TAPI que le client utilisera. Le client utilisera les lignes des fournisseurs de services et les téléphones. Le client doit être dans le même domaine que le serveur ou dans un domaine qui entretient une relation d’approbation bidirectionnelle avec le domaine qui contient le serveur.|
|\<Serveur2 >...|Spécifie un ou les serveurs qui seront disponibles pour ce client supplémentaires. Si vous spécifiez une liste de serveurs, utilisez un espace pour séparer les noms de serveur.|
|/d|Efface la liste des serveurs distants. Désactive le client TAPI en empêchant d’utiliser les fournisseurs de services TAPI qui se trouvent sur les serveurs distants.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs sur l’ordinateur local, ou l’autorité appropriée doit vous avoir été déléguée. Si l'ordinateur est joint à un domaine, les membres du groupe Admins du domaine peuvent être en mesure d'effectuer cette procédure. Pour des raisons de sécurité, il est conseillé d'utiliser la fonction **Exécuter en tant que** pour effectuer cette procédure.
-   Afin que TAPI fonctionne correctement, vous devez exécuter **tcmsetup** pour spécifier les serveurs distants qui seront utilisées par les clients TAPI.
-   Avant de l’utilisateur d’un client peut utiliser un téléphone ou une ligne sur un serveur TAPI, l’administrateur de serveur de téléphonie doit affecter l’utilisateur à la ligne ou le téléphone.
-   La liste des serveurs de téléphonie qui est créée par cette commande remplace toute liste existante des serveurs de téléphonie disponibles pour le client. Vous ne pouvez pas utiliser cette commande pour ajouter à la liste existante.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

[Vue d’ensemble du shell de commande](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)

[Spécifiez les serveurs de téléphonie sur un ordinateur client](https://technet.microsoft.com/library/cc759226(v=ws.10).aspx)

[Affecter un utilisateur à une ligne ou un téléphone](https://technet.microsoft.com/library/cc736875(v=ws.10).aspx)

