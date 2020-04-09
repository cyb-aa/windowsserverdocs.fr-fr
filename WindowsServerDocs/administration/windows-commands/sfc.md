---
title: sfc
description: La rubrique commandes Windows pour SFC, qui analyse et vérifie l’intégrité de tous les fichiers système protégés et remplace les versions incorrectes par les versions appropriées.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c58c25da-e028-42a6-9e10-973484a4b953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7663c8e3527995e2d3ec874dff6fa972e7e83ddd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834322"
---
# <a name="sfc"></a>sfc

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Analyse et vérifie l’intégrité de tous les fichiers système protégés et remplace les versions incorrectes par les versions appropriées.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe
```
sfc [/scannow] [/verifyonly] [/scanfile=<file>] [/verifyfile=<file>] [/offwindir=<offline windows directory> /offbootdir=<offline boot directory>]
```

#### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/scannow|Analyse l’intégrité de tous les fichiers système protégés et répare les fichiers avec des problèmes lorsque cela est possible.|
|/verifyonly|Analyse l’intégrité de tous les fichiers système protégés. Aucune opération de réparation n’est effectuée.|
|/scanfile|Analyse l’intégrité du fichier spécifié et répare le fichier si des problèmes sont détectés, dans la mesure du possible.|
|fichier de \<>|Chemin d’accès complet et nom de fichier spécifiés|
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

## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour vérifier le **fichier Kernel32. dll**, tapez :
```
sfc /verifyfile=c:\windows\system32\kernel32.dll
```
Pour configurer la réparation hors connexion du fichier **Kernel32. dll** avec un répertoire de démarrage hors connexion défini sur **d :** et le répertoire Windows hors connexion défini sur **d:\Windows**, tapez :
```
sfc /scanfile=d:\windows\system32\kernel32.dll /offbootdir=d:\ /offwindir=d:\windows
```

## <a name="additional-references"></a>Références supplémentaires
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

