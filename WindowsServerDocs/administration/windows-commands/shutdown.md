---
title: shutdown
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5d03a8d35f3e56ec7829bc51c1499fddd13b86e8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837250"
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
|/i|Affiche le **boîte de dialogue Arrêt à distance** boîte. Le **/i** option doit être le premier paramètre de la commande suivante. Si **/i** est spécifié, toutes les autres options sont ignorées.|
|/l|Déconnecte l’utilisateur actuel immédiatement, sans délai. Vous ne pouvez pas utiliser **/l** avec **/m** ou **/t**.|
|/s|Arrête l’ordinateur.|
|/r|Redémarre l’ordinateur après l’arrêt.|
|/a|Abandonne un arrêt du système. À compter du uniquement pendant la période de délai d’attente. Pour utiliser **/a**, vous devez également utiliser le **/m** option.|
|/p|Désactive l’ordinateur local (pas un ordinateur distant), sans délai d’attente ou un avertissement. Vous pouvez utiliser **/p** uniquement avec **/d** ou **/f**. Si votre ordinateur ne prend pas en charge la fonctionnalité de mise hors tension, il s’arrête lorsque vous utilisez **/p**, mais reste sur la puissance à l’ordinateur.|
|/h|Met l’ordinateur en veille prolongée, si la mise en veille prolongée est activée. Vous pouvez utiliser **/h** uniquement avec **/f**.|
|/e|Vous permet de documenter la raison de l’arrêt inattendu de l’ordinateur cible.|
|/f|Force la fermeture sans avertir l’utilisateur des applications en cours d’exécution.</br>Avertissement : À l’aide de la **/f** option peut entraîner une perte de données non enregistrées.|
|/m \\\\\<ComputerName>|Spécifie l’ordinateur cible. Ne peut pas être utilisé avec le **/l** option.|
|/t \<XXX>|Définit le délai d’attente ou un délai pour *XXX* secondes avant un redémarrage ou un arrêt. Ceci génère un avertissement à afficher sur la console locale. Vous pouvez spécifier 0-600 secondes. Si vous n’utilisez pas **/t**, le délai d’attente est de 30 secondes par défaut.|
|/d [p\|u:]\<XX>:\<YY>|Indique la raison pour le redémarrage du système ou l’arrêt. Voici les valeurs de paramètre :</br>**p** indique que le redémarrage ou l’arrêt est planifié.</br>**u** indique que la raison est défini par l’utilisateur.</br>Remarque: Si **p** ou **u** n’est pas spécifié, le redémarrage ou l’arrêt est non planifié.</br>*XX* Spécifie le nombre de raison majeur (entier positif inférieur à 256).</br>*YY* Spécifie le nombre de code de raison mineur (entier positif inférieur à 65 536).|
|/c «\<commentaire > »|Vous permet de commenter en détail la raison de l’arrêt. Vous devez tout d’abord fournir une raison à l’aide de la **/d** option. Vous devez placer des commentaires entre guillemets. Vous disposez de 511 caractères au maximum.|
|/?|Affiche l’aide à l’invite de commande, notamment une liste des raisons majeures et mineures qui sont définies sur votre ordinateur local.|

## <a name="remarks"></a>Notes

-   Les utilisateurs doivent être affectés le **arrêter le système** droit utilisateur adéquat pour arrêtés une variable locale ou à distance administré ordinateur qui utilise le **arrêt** commande.
-   Les utilisateurs doivent être membres du groupe Administrateurs pour annoter un arrêt inattendu d’un ordinateur local ou administré à distance. Si l’ordinateur cible est joint à un domaine, les membres du groupe Admins du domaine peuvent être en mesure d’effectuer cette procédure. Pour plus d'informations, consultez :  
    -   [Groupes locaux par défaut](https://technet.microsoft.com/library/cc785098(v=ws.10).aspx)
    -   [Groupes par défaut](https://technet.microsoft.com/library/cc756898(v=ws.10).aspx)
-   Si vous souhaitez arrêter de plusieurs ordinateurs à la fois, vous pouvez appeler **arrêt** pour chaque ordinateur à l’aide d’un script, ou vous pouvez utiliser **arrêt** **/i** pour afficher l’élément distant Boîte de dialogue d’arrêt.
-   Si vous spécifiez des codes de raison principale et secondaire, vous devez d’abord définir ces codes de raison sur chaque ordinateur où vous envisagez d’utiliser les raisons. Si les codes de raison ne sont pas définis sur l’ordinateur cible, Shutdown Event Tracker ne peut pas enregistrer le texte du motif correct.
-   N’oubliez pas d’indiquer qu’un arrêt est programmé à l’aide de la **p:** paramètre. En omettant **p:** indique qu’un arrêt est non planifié. Si vous tapez **p:** suivi par le code de raison d’un arrêt non planifié, la commande effectuera pas l’arrêt. À l’inverse, si vous omettez **p:** et type dans le code de raison d’un arrêt planifié, la commande effectuera pas l’arrêt.

## <a name="BKMK_examples"></a>Exemples

Pour forcer les applications à fermer et redémarrer l’ordinateur local après un délai d’une minute avec la raison « Application : Maintenance (planifiée) » et le commentaire « Reconfiguration de myapp.exe » type :
```
shutdown /r /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```
Pour redémarrer l’ordinateur distant \\ \\nom_serveur avec les mêmes paramètres, tapez :
```
shutdown /r /m \\servername /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
