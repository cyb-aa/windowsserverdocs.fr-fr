---
title: bdehdcfg
description: Rubrique de référence pour la commande BdeHdCfg, qui prépare un disque dur avec les partitions nécessaires pour Chiffrement de lecteur BitLocker.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4c92cd74-188e-4fec-b7c4-fe4e8903e032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3bcc901847bb8d687d59bc3270dab39de0af8d60
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718569"
---
# <a name="bdehdcfg"></a>bdehdcfg

Prépare un disque dur avec les partitions nécessaires pour Chiffrement de lecteur BitLocker. La plupart des installations de Windows 7 n’ont pas besoin d’utiliser cet outil, car le programme d’installation de BitLocker permet de préparer et de repartitionner les lecteurs en fonction des besoins.

> [!WARNING]
> Il existe un conflit connu avec le paramètre de stratégie de groupe **Refuser l'accès en écriture aux lecteurs fixes non protégés par BitLocker** situé dans **Configuration ordinateur\Modèles d'administration\Composants Windows\Chiffrement de lecteur BitLocker\Lecteurs de données fixes**.
>
>Si BdeHdCfg est exécuté sur un ordinateur lorsque ce paramètre de stratégie est activé, vous pouvez rencontrer les problèmes suivants :
>
>- Si vous tentez de réduire le lecteur et si vous créez le lecteur système, le volume du lecteur sera réduit avec succès et une partition brute sera créée. Toutefois, la partition brute ne sera pas formatée. Le message d’erreur suivant s’affiche : impossible de formater le nouveau lecteur actif. Vous devrez peut-être préparer manuellement votre lecteur pour BitLocker.
>
>- Si vous tentez d'utiliser un espace non alloué pour créer le lecteur système, une partition brute sera créée. Toutefois, la partition brute ne sera pas formatée. Le message d’erreur suivant s’affiche : impossible de formater le nouveau lecteur actif. Vous devrez peut-être préparer manuellement votre lecteur pour BitLocker.
>
>- Si vous tentez de fusionner un lecteur existant dans le lecteur système, l'outil n'arrivera pas à copier le fichier de démarrage requis sur le lecteur cible pour créer le lecteur système. Le message d’erreur suivant s’affiche : le programme d’installation de BitLocker n’a pas pu copier les fichiers de démarrage. Vous devrez peut-être préparer manuellement votre lecteur pour BitLocker.
>
>- Lorsque ce paramètre de stratégie est appliqué, un disque dur ne peut pas être repartitionné car le lecteur est protégé. Si vous mettez à niveau des ordinateurs de votre organisation d'une version précédente de Windows et ces ordinateurs ont été configurés avec une partition unique, vous devez créer la partition de système BitLocker requise avant d'appliquer le paramètre de stratégie aux ordinateurs.

## <a name="syntax"></a>Syntaxe

```
bdehdcfg [–driveinfo <drive_letter>] [-target {default|unallocated|<drive_letter> shrink|<drive_letter> merge}] [–newdriveletter] [–size <size_in_mb>] [-quiet]
```

#### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- |----------- |
| [BdeHdCfg : DriveInfo](bdehdcfg-driveinfo.md) | Affiche la lettre de lecteur, la taille totale, l’espace libre maximal et les caractéristiques de partition des partitions sur le lecteur spécifié. Seules les partitions valides apparaissent. L'espace non alloué n'apparaît pas si quatre partitions principales ou étendues existent déjà. |
| [BdeHdCfg : cible](bdehdcfg-target.md) | Définit la partie d’un lecteur à utiliser comme lecteur système et rend la partie active. |
| [BdeHdCfg : newdriveletter](bdehdcfg-newdriveletter.md) | Affecte une nouvelle lettre de lecteur à la partie d’un lecteur utilisée comme lecteur système. |
| [BdeHdCfg : taille](bdehdcfg-size.md) | Détermine la taille de la partition système lors de la création d’un nouveau lecteur système. |
| [BdeHdCfg : quiet](bdehdcfg-quiet.md) | Empêche l’affichage de toutes les actions et erreurs dans l’interface de ligne de commande et indique à BdeHdCfg d’utiliser la réponse oui à toute invite de oui/non qui peut se produire lors de la préparation du lecteur suivant. |
| [BdeHdCfg : redémarrer](bdehdcfg-restart.md) | Indique à l’ordinateur de redémarrer une fois la préparation du lecteur terminée. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
