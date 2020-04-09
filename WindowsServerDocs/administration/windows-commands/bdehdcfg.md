---
title: bdehdcfg
description: La rubrique commandes Windows pour **BdeHdCfg**, qui prépare un disque dur avec les partitions nécessaires pour chiffrement de lecteur BitLocker.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4c92cd74-188e-4fec-b7c4-fe4e8903e032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c9adf8bbfb655e0820fcff6385d3663fc7abbd9a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851002"
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

Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
bdehdcfg [–driveinfo <DriveLetter>] [-target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}] [–newdriveletter] [–size <SizeinMB>] [-quiet]
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

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

L’exemple suivant illustre l’utilisation de BdeHdCfg avec le lecteur par défaut pour créer une partition système de 500 Mo. Étant donné qu’aucune lettre de lecteur n’est spécifiée, la nouvelle partition système n’a pas de lettre de lecteur.

```
bdehdcfg -target default -size 500
```

L’exemple suivant illustre l’utilisation de BdeHdCfg avec le lecteur par défaut pour créer une partition système (P :) de la taille par défaut de 300 Mo de l’espace non alloué sur le lecteur. L’outil ne demande pas à l’utilisateur d’entrer d’autres informations et aucune erreur ne s’affiche. Une fois le lecteur système créé, l’ordinateur redémarre automatiquement.

```
bdehdcfg -target unallocated –newdriveletter P: -quiet -restart
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)