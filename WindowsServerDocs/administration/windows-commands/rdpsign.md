---
title: rdpsign
description: Découvrez comment signer numériquement un fichier RDP.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 648b179f5b2feb8a7585c815aee47804e3bf1532
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882270"
---
# <a name="rdpsign"></a>rdpsign

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous permet de signer numériquement un fichier Remote Desktop Protocol (.rdp).
Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#BKMK_examples).

> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour savoir quelles sont les nouveautés dans la version la plus récente, consultez [les nouveautés des Services Bureau à distance dans Windows Server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.

## <a name="syntax"></a>Syntaxe
```
rdpsign /sha1 <hash> [/q | /v |] [/l] <file_name.rdp>
```

## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/sha1 \<hash>|Spécifie l’empreinte, qui est le hachage Secure Hash Algorithm 1 (SHA1) du certificat de signature qui est inclus dans le magasin de certificats.|
|/q|Mode silencieux. Aucune sortie quand la commande aboutit et une sortie minimale si la commande échoue.|
|/v|Mode détaillé. Affiche tous les avertissements, messages et l’état.|
|/l|Les signatures et de sortie des résultats des tests sans réellement en remplaçant les fichiers d’entrée.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
-   L’empreinte de certificat SHA1 doit représenter un serveur de publication du fichier .rdp approuvés. Pour obtenir l’empreinte de certificat, ouvrez le composant logiciel enfichable Certificats, double-cliquez sur le certificat que vous souhaitez utiliser (dans le magasin de certificats de l’ordinateur local ou dans votre magasin de certificats personnels), cliquez sur le **détails** onglet, puis dans le **champ** , cliquez sur **empreinte**.

    > [!NOTE]
    > Lorsque vous copiez l’empreinte numérique pour une utilisation avec l’outil rdpsign.exe, vous devez supprimer les espaces.

-   Vous devez spécifier le fichier .rdp (ou fichiers) pour vous connecter en utilisant le nom de fichier complet. Caractères génériques ne sont pas acceptées.
-   Les fichiers de sortie signé remplacera les fichiers d’entrée.
-   Si les fichiers .rdp ne peut pas être lues ou écrites dans, l’outil continue au fichier suivant si plusieurs fichiers sont spécifiés.

## <a name="BKMK_examples"></a>Exemples
-   Pour signer un fichier .rdp qui est nommé File1.rdp, accédez au dossier où vous avez enregistré le fichier .rdp, puis tapez la commande suivante :
    ```
    rdpsign /sha1 hash file1.rdp
    ```
    > [!NOTE]
    > Le *hachage* valeur représente l’empreinte de certificat SHA1, sans espaces.
-   Pour tester si la signature numérique elle aboutit pour un fichier .rdp sans réellement ouvrir le fichier de session, tapez la commande suivante :
    ```
    rdpsign /sha1 hash /l file1.rdp
    ```
-   Pour signer les fichiers .rdp multiples, séparez les noms de fichiers à l’aide d’espaces. Par exemple, pour signer les fichiers .rdp plusieurs sont nommés File1.rdp, File2.rdp et File3.rdp, tapez la commande suivante :
    ```
    rdpsign /sha1 hash file1.rdp file2.rdp file3.rdp
    ```
## <a name="see-also"></a>Voir aussi
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
[des Services Bureau à distance &#40;Services Terminal Server&#41; référence de la commande](remote-desktop-services-terminal-services-command-reference.md)
