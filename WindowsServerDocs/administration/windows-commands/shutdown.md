---
title: shutdown
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c432f5cf-c5aa-4665-83af-0ec52c87112e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e8a5170fa214d4ed639ff3b817cf949a9f44ebd6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383889"
---
# <a name="shutdown"></a>shutdown



Permet d’arrêter ou de redémarrer des ordinateurs locaux ou distants, un par un.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
shutdown [/i | /l | /s | /r | /a | /p | /h | /e] [/f] [/m \\<ComputerName>] [/t <XXX>] [/d [p|u:]<XX>:<YY> [/c "comment"]] 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/i|Affiche la boîte de **dialogue arrêt à distance** . L’option **/i** doit être le premier paramètre à la suite de la commande. Si l’option **/i** est spécifiée, toutes les autres options sont ignorées.|
|/l|Déconnecte immédiatement l’utilisateur actuel, sans délai d’attente. Vous ne pouvez pas utiliser **/l** avec **/m** ou **/t**.|
|/s|Arrête l’ordinateur.|
|/r|Redémarre l’ordinateur après l’arrêt.|
|/a|Abandonne un arrêt du système. Effectif uniquement pendant le délai d’expiration. Pour utiliser **/a**, vous devez également utiliser l’option **/m** .|
|/p|Désactive l’ordinateur local uniquement (pas un ordinateur distant), sans délai d’attente ni avertissement. Vous pouvez utiliser **/p** uniquement avec **/d** ou **/f**. Si votre ordinateur ne prend pas en charge la fonctionnalité de mise hors tension, il s’arrête lorsque vous utilisez **/p**, mais la puissance de l’ordinateur reste active.|
|/h|Met l’ordinateur local en veille prolongée, si la mise en veille prolongée est activée. Vous pouvez utiliser **/h** uniquement avec **/f**.|
|/e|Vous permet de documenter la raison de l’arrêt inattendu sur l’ordinateur cible.|
|/f|Force la fermeture des applications en cours d’exécution sans prévenir les utilisateurs.</br>ATTENTION : l’utilisation de l’option **/f** peut entraîner la perte de données non enregistrées.|
|/m \\\\\<ComputerName >|Spécifie l’ordinateur cible. Ne peut pas être utilisé avec l’option **/l** .|
|/t \<XXX >|Définit le délai d’attente ou le délai sur *xxx* secondes avant un redémarrage ou un arrêt. Cela provoque l’affichage d’un avertissement sur la console locale. Vous pouvez spécifier 0-600 secondes. Si vous n’utilisez pas **/t**, le délai d’attente est de 30 secondes par défaut.|
|/d [p\|u :]\<XX >:\<YY >|Indique la raison du redémarrage ou de l’arrêt du système. Les valeurs de paramètre sont les suivantes :</br>**p** indique que le redémarrage ou l’arrêt est planifié.</br>**u** indique que la raison est définie par l’utilisateur.</br>Remarque : si **p** ou **u** ne sont pas spécifiés, le redémarrage ou l’arrêt n’est pas planifié.</br>*XX* spécifie le numéro de raison principale (entier positif inférieur à 256).</br>*YY* Spécifie le numéro de raison secondaire (entier positif inférieur à 65536).|
|/c «\<le commentaire > »|Vous permet de commenter en détail la raison de l’arrêt. Vous devez d’abord fournir une raison à l’aide de l’option **/d** . Vous devez placer les commentaires entre guillemets. Vous disposez de 511 caractères au maximum.|
|/?|Affiche l’aide à l’invite de commandes, y compris une liste des raisons majeures et mineures définies sur votre ordinateur local.|

## <a name="remarks"></a>Notes

-   Le droit d’utilisateur **arrêter le système** doit être affecté aux utilisateurs pour arrêter un ordinateur local ou administré à distance qui utilise la commande **Shutdown** .
-   Les utilisateurs doivent être membres du groupe administrateurs pour annoter un arrêt inattendu d’un ordinateur local ou administré à distance. Si l’ordinateur cible est joint à un domaine, les membres du groupe Admins du domaine peuvent être en mesure d’effectuer cette procédure. Pour plus d’informations, consultez :  
    -   [Groupes locaux par défaut](https://technet.microsoft.com/library/cc785098(v=ws.10).aspx)
    -   [Groupes par défaut](https://technet.microsoft.com/library/cc756898(v=ws.10).aspx)
-   Si vous souhaitez arrêter plusieurs ordinateurs à la fois, vous pouvez appeler **Shutdown** pour chaque ordinateur à l’aide d’un script, ou vous pouvez utiliser **Shutdown** **/i** pour afficher la boîte de dialogue arrêt à distance.
-   Si vous spécifiez des codes de raison majeurs et mineurs, vous devez d’abord définir ces codes de raison sur chaque ordinateur sur lequel vous envisagez d’utiliser les raisons. Si les codes de raison ne sont pas définis sur l’ordinateur cible, le moniteur d’événements de mise hors tension ne peut pas consigner le texte de raison correct.
-   N’oubliez pas d’indiquer qu’un arrêt est planifié à l’aide du paramètre **p :** . L’omission de **p :** indique qu’un arrêt n’est pas planifié. Si vous tapez **p :** suivi du code de raison d’un arrêt non planifié, la commande n’effectue pas l’arrêt. À l’inverse, si vous omettez **p :** et que vous tapez le code de raison d’un arrêt planifié, la commande n’effectue pas l’arrêt.

## <a name="BKMK_examples"></a>Illustre

Pour forcer les applications à fermer et redémarrer l’ordinateur local après un délai d’une minute avec la raison « application : maintenance (planifiée) » et le commentaire « reconfiguration de MyApp. exe », tapez :
```
shutdown /r /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```
Pour redémarrer l’ordinateur distant \\\\ServerName avec les mêmes paramètres, tapez :
```
shutdown /r /m \\servername /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
