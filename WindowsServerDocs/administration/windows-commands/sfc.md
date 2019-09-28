---
title: sfc
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c58c25da-e028-42a6-9e10-973484a4b953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca470d519e9f3425c0c58fd0070a76c7038ec9b5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384018"
---
# <a name="sfc"></a>sfc

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Analyse et vérifie l’intégrité de tous les fichiers système protégés et remplace les versions incorrectes par les versions appropriées.
Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe
```
sfc [/scannow] [/verifyonly] [/scanfile=<file>] [/verifyfile=<file>] [/offwindir=<offline windows directory> /offbootdir=<offline boot directory>]
```

### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/scannow|Analyse l’intégrité de tous les fichiers système protégés et répare les fichiers avec des problèmes lorsque cela est possible.|
|/verifyonly|Analyse l’intégrité de tous les fichiers système protégés. Aucune opération de réparation n’est effectuée.|
|/scanfile|Analyse l’intégrité du fichier spécifié et répare le fichier si des problèmes sont détectés, dans la mesure du possible.|
|\<file>|Chemin d’accès complet et nom de fichier spécifiés|
|/verifyfile|vérifie l’intégrité du fichier spécifié. Aucune opération de réparation n’est effectuée.|
|/offwindir|Spécifie l’emplacement du répertoire Windows hors connexion, pour la réparation hors connexion.|
|/offbootdir|Spécifie l’emplacement du répertoire de démarrage hors connexion pour Offline|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
-   Vous devez avoir ouvert une session en tant que membre du groupe administrateurs pour exécuter **SFC. exe**.
-   Si **SFC** découvre qu’un fichier protégé a été remplacé, il récupère la version correcte du fichier à partir du dossier **systemroot\system32\dllcache** , puis remplace le fichier incorrect.
-   Il existe des différences fonctionnelles entre **SFC** sur windows Server 2003, windows Server 2008 et windows Server 2008 R2 :
-   Pour plus d’informations sur **SFC** sur Windows Server 2003, voir l' [article 310747](https://go.microsoft.com/fwlink/?LinkId=227069) de la base de connaissances Microsoft.
-   Pour plus d’informations sur **SFC** sur windows Server 2008 et windows Server 2008 R2, consultez l' [outil de vérification des fichiers système](https://go.microsoft.com/fwlink/?LinkId=227071).

## <a name="BKMK_examples"></a>Illustre
Pour vérifier le **fichier Kernel32. dll**, tapez :
```
sfc /verifyfile=c:\windows\system32\kernel32.dll
```
Pour configurer la réparation hors connexion du fichier **Kernel32. dll** avec un répertoire de démarrage hors connexion défini sur **d :** et le répertoire Windows hors connexion défini sur **d:\Windows**, tapez :
```
sfc /scanfile=d:\windows\system32\kernel32.dll /offbootdir=d:\ /offwindir=d:\windows
```

## <a name="additional-references"></a>Références supplémentaires
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

