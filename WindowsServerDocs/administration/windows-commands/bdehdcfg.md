---
title: bdehdcfg
description: Rubrique de commandes de Windows pour **bdehdcfg** -prépare un disque dur avec les partitions nécessaires pour le chiffrement de lecteur BitLocker.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4c92cd74-188e-4fec-b7c4-fe4e8903e032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a4fda562f4aa6b8caa885fcd70155b7150c91d12
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869700"
---
# <a name="bdehdcfg"></a>bdehdcfg



Prépare un disque dur avec les partitions nécessaires pour le chiffrement de lecteur BitLocker. La plupart des installations de Windows 7 ne devrez pas utiliser cet outil, car le programme d’installation de BitLocker inclut la possibilité de préparer et repartitionner les disques en fonction des besoins.

> [!WARNING]
> Il existe un conflit connu avec le paramètre de stratégie de groupe **Refuser l'accès en écriture aux lecteurs fixes non protégés par BitLocker** situé dans **Configuration ordinateur\Modèles d'administration\Composants Windows\Chiffrement de lecteur BitLocker\Lecteurs de données fixes**.</br>> Si Bdehdcfg est exécuté sur un ordinateur lorsque ce paramètre de stratégie est activé, vous pouvez rencontrer les problèmes suivants :</br>> : Si vous avez tenté de réduire le lecteur et créer le lecteur système, la taille du disque sera réduite avec succès et une partition brute sera créée. Toutefois, la partition brute ne sera pas formatée. Le message d'erreur suivant s'affiche : « Impossible de reformater le nouveau lecteur actif. Vous devrez peut-être préparer manuellement votre lecteur pour BitLocker. »</br>>-Si vous avez tenté d’utiliser l’espace non alloué pour créer le lecteur système, une partition brute sera créée. Toutefois, la partition brute ne sera pas formatée. Le message d'erreur suivant s'affiche : « Impossible de reformater le nouveau lecteur actif. Vous devrez peut-être préparer manuellement votre lecteur pour BitLocker. »</br>>-Si vous avez tenté de fusionner un lecteur existant dans le lecteur du système, l’outil échouera copier le fichier de démarrage requis sur le lecteur cible pour créer le lecteur système. Le message d'erreur suivant s'affiche : « Le programme d’installation de BitLocker n’a pas pu copier les fichiers de démarrage. Vous devrez peut-être préparer manuellement votre lecteur pour BitLocker. »</br>> Si ce paramètre de stratégie est appliqué, un disque dur ne peut pas être repartitionné car le lecteur est protégé. Si vous mettez à niveau des ordinateurs de votre organisation d'une version précédente de Windows et ces ordinateurs ont été configurés avec une partition unique, vous devez créer la partition de système BitLocker requise avant d'appliquer le paramètre de stratégie aux ordinateurs.

Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
bdehdcfg [–driveinfo <DriveLetter>] [-target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}] [–newdriveletter] [–size <SizeinMB>] [-quiet]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[Bdehdcfg: driveinfo](bdehdcfg-driveinfo.md)|Affiche la lettre de lecteur, la taille totale, l’espace libre maximal et les caractéristiques de partition des partitions sur le lecteur spécifié. Seules les partitions valides apparaissent. L'espace non alloué n'apparaît pas si quatre partitions principales ou étendues existent déjà.|
|[Bdehdcfg: target](bdehdcfg-target.md)|Définit la partie d’un disque à utiliser en tant que le lecteur du système et rend la partie active.|
|[Bdehdcfg: newdriveletter](bdehdcfg-newdriveletter.md)|Affecte une nouvelle lettre de lecteur à la partie d’un lecteur utilisé comme lecteur système.|
|[Bdehdcfg: size](bdehdcfg-size.md)|Détermine la taille de la partition système lorsqu’un nouveau lecteur système est créé.|
|[Bdehdcfg: quiet](bdehdcfg-quiet.md)|Empêche l’affichage de toutes les actions et les erreurs dans l’interface de ligne de commande et dirige Bdehdcfg à utiliser la réponse « Oui » pour n’importe quel Oui/préparation de lecteur ni d’invites qui peut-être se produire pendant les.|
|[Bdehdcfg : redémarrer](bdehdcfg-restart.md)|Indique à l’ordinateur à redémarrer après que la préparation du lecteur a terminé.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_Examples"></a>Exemples

L’exemple suivant illustre Bdehdcfg utilisé avec le lecteur par défaut pour créer une partition de système de 500 Mo. Étant donné que sans lettre de lecteur n’est spécifié, la nouvelle partition système n’aura pas d’une lettre de lecteur.
```
bdehdcfg -target default -size 500
```
L’exemple suivant illustre Bdehdcfg utilisé avec le lecteur par défaut pour créer une partition système (p) de la taille par défaut de 300 Mo en dehors de l’espace non alloué sur le lecteur. L’outil n’invitera pas l’utilisateur pour une entrée supplémentaire ni seront affiche toutes les erreurs. Une fois que le lecteur du système a été créé, l’ordinateur redémarre automatiquement.
```
bdehdcfg -target unallocated –newdriveletter P: -quiet -restart
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)