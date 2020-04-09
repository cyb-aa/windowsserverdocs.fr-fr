---
ms.assetid: f2eefaaf-2817-4ac7-abac-d2b65fa971dc
title: Fsutil transaction
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: aa6692dbb8af1ec832650971c6723c060fc2cd56
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844012"
---
# <a name="fsutil-transaction"></a>Fsutil transaction
>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows 2008, Windows Vista

Gère les transactions NTFS.

Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_examples) .

## <a name="syntax"></a>Syntaxe

```
fsutil transaction [commit] <GUID>
fsutil transaction [fileinfo] <Filename>
fsutil transaction [list]
fsutil transaction [query] [{Files|All}] <GUID>
fsutil transaction [rollback] <GUID>
```

#### <a name="parameters"></a>Paramètres

| Paramètre  |                                                                                                                                                     Description                                                                                                                                                     |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   validation   |                                                                                                                      Marque la fin d’une transaction spécifiée implicite ou explicite.                                                                                                                      |
|   <GUID>   |                                                                                                                               Spécifie la valeur GUID qui représente une transaction.                                                                                                                               |
|  FileInfo  |                                                                                                                              Affiche des informations sur les transactions pour le fichier spécifié.                                                                                                                               |
| <Filename> |                                                                                                                                         Spécifie le chemin d’accès complet et le nom de fichier.                                                                                                                                          |
|    list    |                                                                                                                                 Affiche la liste des transactions en cours d’exécution.                                                                                                                                  |
|   query    | Affiche des informations pour la transaction spécifiée.<p>-Si les **fichiers de requête fsutil transaction** sont spécifiés, les informations de fichier s’affichent uniquement pour la transaction spécifiée.<br />-Si l’ensemble de la **requête fsutil transaction** est spécifié, toutes les informations relatives à la transaction sont affichées. |
|  restaurer  |                                                                                                                                Restaure une transaction spécifiée au début.                                                                                                                                 |

### <a name="remarks"></a>Notes

-   Le NTFS transactionnel a été introduit dans Windows Server 2008.

### <a name="examples"></a><a name="BKMK_examples"></a>Illustre
Pour afficher les informations de transaction pour le fichier c:\test.txt, tapez :

```
fsutil transaction fileinfo c:\test.txt  
```

### <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Fsutil](Fsutil.md)

[NTFS transactionnel](https://go.microsoft.com/fwlink/?LinkID=165402)


