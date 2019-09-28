---
title: rdpsign
description: Découvrez comment signer numériquement un fichier RDP.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4a6fa8ce-3d32-49a5-b056-bcc1a23391f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: aa1f8f8f31abd85a1ad106a3c4764fc4ccf74258
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384768"
---
# <a name="rdpsign"></a>rdpsign

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Vous permet de signer numériquement un fichier protocole RDP (Remote Desktop Protocol) (. RDP).
Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_examples).

> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.

## <a name="syntax"></a>Syntaxe
```
rdpsign /sha1 <hash> [/q | /v |] [/l] <file_name.rdp>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|/SHA1 \<hash >|Spécifie l’empreinte, qui est le hachage Secure Hash Algorithm 1 (SHA1) du certificat de signature inclus dans le magasin de certificats.|
|/q|Mode silencieux. Aucune sortie lorsque la commande réussit et une sortie minimale si la commande échoue.|
|/v|Mode détaillé. Affiche tous les avertissements, messages et États.|
|/l|Teste les résultats de la signature et de la sortie sans remplacer les fichiers d’entrée.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
-   L’empreinte numérique du certificat SHA1 doit représenter un éditeur de fichiers. RDP approuvé. Pour obtenir l’empreinte numérique du certificat, ouvrez le composant logiciel enfichable Certificats, double-cliquez sur le certificat que vous souhaitez utiliser (dans le magasin de certificats de l’ordinateur local ou dans votre magasin de certificats personnel), cliquez sur l’onglet **Détails** , puis dans laListe de champs, cliquez sur **empreinte numérique**.

    > [!NOTE]
    > Lorsque vous copiez l’empreinte numérique à utiliser avec l’outil rdpsign. exe, vous devez supprimer tous les espaces.

-   Vous devez spécifier le ou les fichiers. rdp à signer à l’aide du nom de fichier complet. Les caractères génériques ne sont pas acceptés.
-   Les fichiers de sortie signés remplacent les fichiers d’entrée.
-   Si des fichiers. RDP ne peuvent pas être lus ou écrits dans, l’outil continue le fichier suivant si plusieurs fichiers sont spécifiés.

## <a name="BKMK_examples"></a>Illustre
- Pour signer un fichier. RDP nommé fichier1. RDP, accédez au dossier où vous avez enregistré le fichier. RDP, puis tapez ce qui suit :
  ```
  rdpsign /sha1 hash file1.rdp
  ```
  > [!NOTE]
  > La valeur de *hachage* représente l’empreinte numérique du certificat SHA1, sans espace.
- Pour vérifier si la signature numérique est réussie pour un fichier. RDP sans signer réellement le fichier, tapez ce qui suit :
  ```
  rdpsign /sha1 hash /l file1.rdp
  ```
- Pour signer plusieurs fichiers. RDP, séparez les noms de fichiers à l’aide d’espaces. Par exemple, pour signer plusieurs fichiers. RDP nommés fichier1. RDP, fichier2. RDP et fichier3. RDP, tapez la commande suivante :
  ```
  rdpsign /sha1 hash file1.rdp file2.rdp file3.rdp
  ```
  ## <a name="see-also"></a>Voir aussi
  [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [ &#40;services Bureau à distance&#41; référence des commandes des services Terminal Server](remote-desktop-services-terminal-services-command-reference.md)
