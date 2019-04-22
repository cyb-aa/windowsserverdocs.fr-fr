---
ms.assetid: f2eefaaf-2817-4ac7-abac-d2b65fa971dc
title: transaction de fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 93c981d077dbb027400a1eb2e2c662f72c14cc44
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825210"
---
# <a name="fsutil-transaction"></a>transaction de fsutil
>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows 2008, Windows Vista

Gère les transactions de NTFS.

Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#BKMK_examples) .

## <a name="syntax"></a>Syntaxe

```
fsutil transaction [commit] <GUID>
fsutil transaction [fileinfo] <Filename>
fsutil transaction [list]
fsutil transaction [query] [{Files|All}] <GUID>
fsutil transaction [rollback] <GUID>

```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------------|---------------|
|validation|Marque la fin d’une transaction réussie implicite ou explicite spécifiée.|
|<GUID>|Spécifie la valeur GUID qui représente une transaction.|
|fileinfo|Affiche des informations de transaction pour le fichier spécifié.|
|<Filename>|Spécifie le chemin d’accès complet et nom de fichier.|
|list|Affiche une liste de transactions en cours d’exécution.|
|requête|Affiche des informations pour la transaction spécifiée.<br /><br />-If **fsutil transaction des fichiers de requête** est spécifié, les informations de fichier seront affichera uniquement pour la transaction spécifiée.<br />-If **fsutil transaction interroger tous les** est spécifié, toutes les informations de la transaction seront affichera.|
|rollback|Restaure une transaction spécifiée au début.|

### <a name="remarks"></a>Notes

-   NTFS transactionnel a été introduit dans Windows Server 2008.

### <a name="BKMK_examples"></a>Exemples
Pour afficher les informations de transaction pour le fichier c:\test.txt, tapez :

```
fsutil transaction fileinfo c:\test.txt  
```

### <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[NTFS transactionnel](https://go.microsoft.com/fwlink/?LinkID=165402)


