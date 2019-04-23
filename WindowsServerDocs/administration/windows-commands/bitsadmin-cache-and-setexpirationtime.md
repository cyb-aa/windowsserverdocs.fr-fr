---
titre : description de cache et setexpirationtime bitsadmin : « Rubrique commandes de Windows pour **bitsadmin cache et setexpirationtime** -définit le délai d’expiration de cache. »
ms.custom: na ms.prod: windows-server-threshold ms.reviewer: na ms.suite: na ms.technology: manage-windows-commands ms.tgt_pltfrm: na ms.topic: article ms.assetid: 00ea6e4e-b707-4b31-88dd-b61a78565c8d author: coreyp-at-msft ms.author: coreyp manager: dongill ms.date: 10/16/2017 --

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="bitsadmin-cache-and-setexpirationtime"></a>setexpirationtime et bitsadmin cache
Définit le délai d’expiration de cache.
## <a name="syntax"></a>Syntaxe
```
bitsadmin /Cache /SetExpirationtime secs
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|secondes|Le nombre de secondes avant l’expiration du cache.|
## <a name="BKMK_examples"></a>Exemples
L’exemple suivant l’expiration du cache en 60 secondes.
```
C:\>bitsadmin /Cache / SetExpirationtime 60
```
## <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
