---
title: Nouveautés de la console Windows dans Windows Server2016
description: Répertorie les nouvelles fonctionnalités importantes de la console Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: server-general
ms.reviewer: na
ms.suite: na
ms.date: 10/04/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da9fc582-033b-4973-84e7-0c6024ecfcbc
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 2f05bcffa7c8c4f9e74f3699b9838b8a627af1b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837720"
---
# <a name="whats-new-in-the-windows-console-in-windows-server-2016"></a>Nouveautés de la console Windows dans Windows Server2016
>S'applique à : Windows Server 2016

L’hôte de la console (le code sous-jacent qui prend en charge toutes les applications en mode caractère, notamment l’invite de commandes Windows et l’invite Windows PowerShell, entre autres) a été mis à jour de plusieurs manières pour ajouter un grand nombre de nouvelles fonctionnalités.  

## <a name="controlling-the-new-features"></a>Contrôle des nouvelles fonctionnalités  
Les nouvelles fonctionnalités sont activées par défaut, mais vous pouvez activer et désactiver chacune d’elles ou revenir à l’hôte de la console précédent via l’interface de propriétés (principalement sous l’onglet **Options**) ou avec les clés de Registre suivantes (toutes les clés sont des valeurs DWORD sous **HKEY_CURRENT_USER\Console**) :  

|Clé de Registre|Description|  
|----------------|---------------|  
|ForceV2|1 active toutes les nouvelles fonctionnalités de la console ; 0 désactive toutes les nouvelles fonctionnalités. Remarque : Cette valeur n’est pas stockée dans les raccourcis, mais uniquement dans cette clé de Registre.|  
|LineSelection|1 active la sélection de ligne ; 0 pour utiliser uniquement le mode bloc|  
|FilterOnPaste|1 active le nouveau comportement de collage|  
|LineWrap|1 renvoie le texte à la ligne quand vous redimensionnez les fenêtres de la console|  
|CtrlKeyShortcutsDisabled|0 active de nouveaux raccourcis ; 1 les désactive|  
|ExtendedEdit Keys|1 active l’ensemble des touches de sélection ; 0 les désactive|  
|TrimLeadingZeros|1 supprime les zéros non significatifs dans les sélections effectuées par un double-clic ; 0 conserve les zéros non significatifs|  
|WindowsAlpha|Définit la valeur d’opacité entre 30 % et 100 %. Utilisez 0x4C à 0xFF ou 76 à 255 pour spécifier la valeur|  
|WordDelimiters|Définit le caractère qui est utilisé pour passer d’un mot à l’autre quand vous sélectionnez un texte par mot entier à l’aide de Ctrl+Maj+touche de direction (la valeur par défaut est l’espace). Définissez cette valeur REG_SZ pour contenir tous les caractères qui doivent être traités comme délimiteurs. Remarque : Cette valeur n’est pas stockée dans les raccourcis, mais uniquement dans cette clé de Registre.|  

Ces paramètres sont stockés par titre de chaque fenêtre dans le Registre sous HKCU\Console. Les fenêtres de la console ouvertes par un raccourci ont ces paramètres stockés dans le raccourci ; si le raccourci est copié vers un autre ordinateur, les paramètres sont aussi déplacés vers le nouvel ordinateur. Les paramètres des raccourcis remplacent tous les autres paramètres, dont les valeurs par défaut et les paramètres globaux. Toutefois, si vous revenez à la console d’origine à l’aide de l’option **Utiliser l’ancienne console**  sous l’onglet **Options**, ce paramètre est global et conservé pour toutes les fenêtres par la suite, notamment après le redémarrage de l’ordinateur.  

Vous pouvez préconfigurer ces paramètres ou créer des scripts pour ceux-ci en configurant le Registre de manière appropriée dans un fichier d’installation sans assistance ou avec Windows PowerShell.  

Les applications 16 bits NTVDM reviennent toujours à l’hôte de la console plus ancien.  

> [!NOTE]  
> Si vous rencontrez des problèmes avec les nouveaux paramètres de la console et que vous ne pouvez pas les résoudre avec les options spécifiques répertoriées ici, vous pouvez toujours revenir à la console d’origine en affectant la valeur 0 à ForceV2 ou en utilisant **Utiliser l’ancienne console**  sous l’onglet **Options**.  

## <a name="console-behavior"></a>Comportement de la console  
Vous pouvez désormais redimensionner la fenêtre de console à volonté en saisissant un bord à l’aide de la souris et en le faisant glisser. Les barres de défilement apparaissent uniquement si vous définissez manuellement les dimensions de la fenêtre (à l’aide de l’onglet **Disposition** dans **Propriétés**) ou si la ligne de texte la plus longue dans la mémoire tampon est plus large que la taille actuelle de la fenêtre.  

La nouvelle fenêtre de console prend désormais en charge le retour automatique à la ligne. Toutefois, si vous avez utilisé les API de console pour modifier le texte dans une mémoire tampon, la console laisse le texte tel qu’il a été inséré.  

Les fenêtres de console peuvent maintenant être semi-transparentes (pour une transparence minimale de 30 %). Vous pouvez ajuster la transparence dans le menu Propriétés ou avec les commandes de clavier suivantes :  

|Pour ce faire :|Utilisez cette combinaison de touches :|  
|---------------|-----------------------------|  
|Augmenter la transparence|Ctrl+Maj+signe plus (+) ou Ctrl+Maj+défilement vers le haut avec la souris|  
|Diminuer la transparence|Ctrl+Maj+signe moins (-) ou Ctrl+Maj+défilement vers le bas avec la souris|  
|Activer/désactiver le mode plein écran|Alt+Entrée|  

## <a name="selection"></a>d’un certificat SSTP  
Il existe de nombreuses nouvelles options pour la sélection de texte et des lignes, ainsi que pour le marquage de texte et l’utilisation de l’historique de la mémoire tampon. La console tente d’éviter les conflits avec les applications qui peuvent utiliser les mêmes touches.  

**Pour les développeurs :** Si un conflit se produit, vous pouvez généralement contrôler le comportement d’utilisation de l’application de la ligne d’entrée, entrée traitée et modes d’écho d’entrée avec l’API SetConsoleMode(). Si le mode d’exécution est l’entrée traitée, les raccourcis ci-dessous s’appliquent mais, dans d’autres modes, votre application doit les gérer. Les combinaisons de touches non répertoriées ici fonctionnent comme dans les versions précédentes de la console. Vous pouvez également tenter de résoudre les conflits avec différents paramètres sous l’onglet **Options**. Si toutes les autres possibilités échouent, vous pouvez toujours revenir à la console d’origine.  

Vous pouvez maintenant utiliser la sélection « cliquer et faire glisser » en dehors du mode d’édition rapide et cette sélection permet de sélectionner du texte sur plusieurs lignes, comme dans le Bloc-notes, et non juste un bloc rectangulaire. Les opérations de copie ne nécessitent plus de supprimer les sauts de ligne. En plus de la sélection « cliquer et faire glisser », les combinaisons de touches suivantes sont disponibles :  

**Sélection de texte**  

|Pour ce faire :|Utilisez cette combinaison de touches :|  
|---------------|-----------------------------|  
|Déplacer le curseur d’un caractère vers la gauche, ce qui étend la sélection|Maj+Gauche|  
|Déplacer le curseur d’un caractère vers la droite, ce qui étend la sélection|Maj+Droite|  
|Sélectionner le texte ligne par ligne vers le haut à partir du point d’insertion|Maj+Haut|  
|Étendre la sélection de texte d’une ligne vers le bas à partir du point d’insertion|Maj+Bas|  
|Si le curseur se trouve sur la ligne en cours de modification, utilisez une fois cette commande pour étendre la sélection jusqu’au dernier caractère de la ligne d’entrée. Utilisez-la une deuxième fois pour étendre la sélection jusqu’à la marge droite.|Maj+Fin|  
|Si le curseur ne se trouve **pas** sur la ligne en cours de modification, utilisez cette commande pour sélectionner tout le texte à partir du point d’insertion jusqu’à la marge droite.|Maj+Fin|  
|Si le curseur se trouve sur la ligne en cours de modification, utilisez une fois cette commande pour étendre la sélection jusqu’au caractère immédiatement après l’invite de commandes. Utilisez-la une deuxième fois pour étendre la sélection jusqu’à la marge droite.|Maj+Début|  
|Si le curseur ne se trouve **pas** sur la ligne en cours de modification, utilisez cette commande pour étendre la sélection jusqu’à la marge gauche.|Maj+Début|  
|Étendre la sélection d’un écran vers le bas|Maj+Page suivante|  
|Étendre la sélection d’un écran vers le haut|Maj+Page précédente|  
|Étendre la sélection d’un mot vers la droite. (Vous pouvez définir les délimiteurs de « mot » avec la clé de Registre WordDelimiters.)|Ctrl+Maj+Droite|  
|Étendre la sélection d’un mot vers la gauche|Ctrl+Maj+Début|  
|Étendre la sélection au début de la mémoire tampon d’écran|Ctrl+Maj+Fin|  
|Sélectionner tout le texte après l’invite, si le curseur se trouve sur la ligne active et que la ligne n’est pas vide|Ctrl+A|  
|Sélectionner la mémoire tampon entière, si le curseur ne se trouve **pas** sur la ligne active|Ctrl+A|  

**Modification de texte**  

Vous pouvez copier et coller du texte dans la console à l’aide des commandes du clavier. Ctrl+C remplit maintenant deux fonctions. Si aucun texte n’est sélectionné quand vous l’utilisez, cette combinaison envoie la commande BREAK comme d’habitude. Si du texte est sélectionné, la première utilisation copie le texte et efface la sélection ; la deuxième utilisation envoie BREAK. Voici les autres commandes de modification :  

|Pour ce faire :|Utilisez cette combinaison de touches :|  
|---------------|-----------------------------|  
|Coller du texte dans la ligne de commande|Ctrl+V|  
|Copier le texte sélectionné dans le Presse-papiers|Ctrl+Ins|  
|Copier le texte sélectionné dans le Presse-papiers ; envoyer BREAK|Ctrl+C|  
|Coller du texte dans la ligne de commande|Maj+Ins|  

**Mode marque**  

Pour passer en mode marquage à tout moment, cliquez avec le bouton droit n’importe où dans la barre de titre de la console, pointez sur **Modifier**, puis sélectionnez **Mark** (Marquage) dans le menu qui s’ouvre. Vous pouvez également utiliser Ctrl+M. En mode marquage, utilisez la touche Alt pour identifier le début d’une sélection du retour automatique à la ligne. (Si l’option **Activer la sélection du retour automatique à la ligne** est désactivée, le mode marquage sélectionne le texte dans un bloc.) En mode marquage, utilisez Ctrl+Maj+touche de direction pour une sélection par caractère et non par mot comme en mode normal. En plus des touches de sélection de la section **Modification de texte**, les combinaisons suivantes sont disponibles en mode marquage :  

|Pour ce faire :|Utilisez cette combinaison de touches :|  
|---------------|-----------------------------|  
|Passer en mode marquage pour déplacer le curseur dans la fenêtre|Ctrl+M|  
|Commencer la sélection du retour automatique à la ligne en mode marquage, avec d’autres combinaisons de touches|Alt|  
|Déplacer le curseur dans la direction spécifiée|Touches de direction|  
|Déplacer le curseur d’une page dans la direction spécifiée|Touches Page|  
|Déplacer le curseur au début de la mémoire tampon|Ctrl+Début|  
|Déplacer le curseur à la fin de la mémoire tampon|Ctrl+Fin|  

**Historique de navigation**  

|Pour ce faire :|Utilisez cette combinaison de touches :|  
|---------------|-----------------------------|  
|Monter d’une ligne dans l’historique de sortie|Ctrl+Haut|  
|Descendre d’une ligne dans l’historique de sortie|Ctrl+Bas|  
|Déplacer la fenêtre d’affichage en haut de la mémoire tampon (si la ligne de commande est vide) ou supprimer tous les caractères à gauche du curseur (si la ligne de commande n’est pas vide)|Ctrl+Début|  
|Déplacer la fenêtre d’affichage vers la ligne de commande (si la ligne de commande est vide) ou supprimer tous les caractères à droite du curseur (si la ligne de commande n’est pas vide)|Ctrl+Fin|  

**Commandes de clavier supplémentaires**  

|Pour ce faire :|Utilisez cette combinaison de touches :|  
|---------------|-----------------------------|  
|Ouvrir la boîte de dialogue Rechercher|Ctrl+F|  
|Fermer la fenêtre de la console|ALT+F4|  
