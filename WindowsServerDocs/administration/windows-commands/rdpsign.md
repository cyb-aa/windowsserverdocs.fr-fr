---
title: rdpsign
description: Découvrez comment signer numériquement un fichier RDP.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4a6fa8ce-3d32-49a5-b056-bcc1a23391f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 4245ea533238d31457563f4d3521fdb09ff1f255
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722627"
---
# <a name="rdpsign"></a>rdpsign

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous permet de signer numériquement un fichier protocole RDP (Remote Desktop Protocol) (. RDP).


> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.

## <a name="syntax"></a>Syntaxe
```
rdpsign /sha1 <hash> [/q | /v |] [/l] <file_name.rdp>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|> \<de hachage/SHA1|Spécifie l’empreinte, qui est le hachage Secure Hash Algorithm 1 (SHA1) du certificat de signature inclus dans le magasin de certificats. Utilisé dans Windows Server 2012 R2 et versions antérieures.|
|> \<de hachage/SHA256|Spécifie l’empreinte numérique, qui est le hachage de l’algorithme de hachage sécurisé 256 (SHA256) du certificat de signature inclus dans le magasin de certificats. Remplace/SHA1 dans Windows Server 2016 et versions ultérieures.|
|/q|Mode silencieux. Aucune sortie lorsque la commande réussit et une sortie minimale si la commande échoue.|
|/v|mode détaillé. Affiche tous les avertissements, messages et États.|
|/l|Teste les résultats de la signature et de la sortie sans remplacer les fichiers d’entrée.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 
-   L’empreinte numérique du certificat SHA1 ou SHA256 doit représenter un éditeur de fichiers. RDP approuvé. Pour obtenir l’empreinte numérique du certificat, ouvrez le composant logiciel enfichable Certificats, double-cliquez sur le certificat que vous souhaitez utiliser (dans le magasin de certificats de l’ordinateur local ou dans votre magasin de certificats personnel), cliquez sur l’onglet **Détails** , puis dans la liste **champ** , cliquez sur **empreinte numérique**.

    > [!NOTE]
    > Lorsque vous copiez l’empreinte numérique à utiliser avec l’outil rdpsign. exe, vous devez supprimer tous les espaces.

-   Vous devez spécifier le ou les fichiers. rdp à signer à l’aide du nom de fichier complet. Les caractères génériques ne sont pas acceptés.
-   Les fichiers de sortie signés remplacent les fichiers d’entrée.
-   Si des fichiers. RDP ne peuvent pas être lus ou écrits dans, l’outil continue le fichier suivant si plusieurs fichiers sont spécifiés.

## <a name="examples"></a><a name="BKMK_examples"></a>Illustre
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
  ## <a name="see-also"></a> Voir aussi
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [services Bureau à distance la référence de commande (services Terminal Server)](remote-desktop-services-terminal-services-command-reference.md)
