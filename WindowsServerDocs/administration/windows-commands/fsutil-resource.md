---
ms.assetid: b198d8ca-a5b7-430f-8911-5cbb9f50484c
title: ressources de fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 38b0947171b7fd8afc44a95b2244a3fd2a0c9e73
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439047"
---
# <a name="fsutil-resource"></a>ressources de fsutil
>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows 2008, Windows Vista

Crée un gestionnaire de ressources secondaire transactionnelle, démarre ou arrête un gestionnaire de ressources transactionnelles, ou affiche des informations sur un gestionnaire de ressources transactionnelles et modifie le comportement suivant :

-   Indique si une valeur par défaut du Gestionnaire de ressources transactionnel nettoiera ses métadonnées transactionnelles sur le montage suivant

-   Le Gestionnaire de ressources transactionnelles spécifié à préférer la cohérence de la disponibilité

-   Le Gestionnaire de ressources de Transaction spécifié à la cohérence préférez la disponibilité

-   Les caractéristiques d’un gestionnaire de ressources transactionnelles en cours d’exécution

Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#BKMK_examples) .

## <a name="syntax"></a>Syntaxe

```
fsutil resource [create] <RmRootPathname>
fsutil resource [info] <RmRootPathname>
fsutil resource [setautoreset] {true|false} <DefaultRmRootPathname>
fsutil resource [setavailable] <RmRootPathname>
fsutil resource [setconsistent] <RmRootPathname>
fsutil resource [setlog] [growth {<Containers> containers|<Percent> percent} <RmRootPathname>] [maxextents <Containers> <RmRootPathname>] [minextents <Containers> <RmRootPathname>] [mode {full|undo} <RmRootPathname>] [rename <RmRootPathname>] [shrink <percent> <RmRootPathname>] [size <Containers> <RmRootPathname>]
fsutil resource [start] <RmRootPathname> [<RmLogPathname> <TmLogPathname>
fsutil resource [stop] <RmRootPathname>
```

### <a name="parameters"></a>Paramètres

|        Paramètre        |                                                                                                                                                                                                                                        Description                                                                                                                                                                                                                                         |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         créer          |                                                                                                                                                                                                                    Crée un gestionnaire de ressources transactionnelles secondaire.                                                                                                                                                                                                                     |
|    <RmRootPathname>     |                                                                                                                                                                                                        Spécifie le chemin d’accès complet à un répertoire racine de gestionnaire de ressources transactionnelles.                                                                                                                                                                                                         |
|          info           |                                                                                                                                                                                                            Affiche des informations spécifiées transactionnelle du Gestionnaire de ressources.                                                                                                                                                                                                            |
|      setautoreset       | Spécifie si une valeur par défaut du Gestionnaire de ressources transactionnel nettoiera les métadonnées transactionnelles sur le montage suivant.<br /><br />-Définissez la **setautoreset** paramètre **true** pour spécifier que le Gestionnaire de ressources de Transaction nettoiera les métadonnées transactionnelles sur le montage suivant, par défaut.<br />-Définissez la **setautoreset** paramètre **false** pour spécifier que le Gestionnaire de ressources de Transaction nettoiera pas les métadonnées transactionnelles sur le montage suivant, par défaut. |
| <DefaultRmRootPathname> |                                                                                                                                                                                                                       Spécifie le nom du lecteur suivi du signe deux-points.                                                                                                                                                                                                                        |
|      setavailable       |                                                                                                                                                                                                 Spécifie qu’un gestionnaire de ressources transactionnelles sera préférez la disponibilité cohérence.                                                                                                                                                                                                 |
|      setconsistent      |                                                                                                                                                                                                 Spécifie qu’un gestionnaire de ressources transactionnelles préféreront cohérence sur la disponibilité.                                                                                                                                                                                                 |
|         setlog          |                                                                                                                                                                                                  Modifie les caractéristiques d’un gestionnaire de ressources transactionnelles qui est déjà en cours d’exécution.                                                                                                                                                                                                  |
|         growth          |                                                                                                  Spécifie le montant par lequel le journal du Gestionnaire de ressources transactionnel peut atteindre.<br /><br />Le paramètre de croissance peut être spécifié comme suit :<br /><br />-Nombre de conteneurs en utilisant le format : *Conteneurs***conteneurs**<br />-   pourcentage en utilisant le format : *Pourcentage***%* *                                                                                                   |
|      <containers>       |                                                                                                                                                                                                      Spécifie les objets de données qui sont utilisées par le Gestionnaire de ressources transactionnelles.                                                                                                                                                                                                       |
|        maxextent        |                                                                                                                                                                                                Spécifie le nombre maximal de conteneurs pour le Gestionnaire de ressources transactionnel spécifié.                                                                                                                                                                                                |
|        minextent        |                                                                                                                                                                                                Spécifie le nombre minimal de conteneurs pour le Gestionnaire de ressources transactionnel spécifié.                                                                                                                                                                                                |
|  mode {complète&#124;Annuler}  |                                                                                                                                                                                        Spécifie si toutes les transactions sont enregistrées ( **complète**) ou uniquement restaurée les événements sont enregistrés (**Annuler**).                                                                                                                                                                                         |
|         rename          |                                                                                                                                                                                                                  Modifie le GUID pour le Gestionnaire de ressources transactionnelles.                                                                                                                                                                                                                  |
|         shrink          |                                                                                                                                                                                              Spécifie le pourcentage par lequel le journal du Gestionnaire de ressources transactionnel peut automatiquement diminuer.                                                                                                                                                                                              |
|          size           |                                                                                                                                                                                              Spécifie la taille du Gestionnaire de ressources transactionnelles comme un nombre spécifié de *conteneurs*.                                                                                                                                                                                               |
|          start          |                                                                                                                                                                                                                    Démarre le Gestionnaire de ressources transactionnel spécifié.                                                                                                                                                                                                                    |
|          stop           |                                                                                                                                                                                                                    Arrête le Gestionnaire de ressources transactionnel spécifié.                                                                                                                                                                                                                     |

### <a name="BKMK_examples"></a>Exemples
Pour définir le journal pour transactionnelle Resource Manager qui est spécifié par c:\test, pour avoir une croissance automatique de cinq conteneurs, tapez :

```
fsutil resource setlog growth 5 containers c:test
```

Pour définir le journal pour transactionnelle Resource Manager qui est spécifié par c:\test, pour avoir une croissance automatique de deux pour cent, tapez :

```
fsutil resource setlog growth 2 percent c:test
```

Pour spécifier que la valeur par défaut du Gestionnaire de ressources transactionnel nettoiera les métadonnées transactionnelles sur le montage suivant sur le lecteur C, tapez :

```
fsutil resource setautoreset true c:\  
```

### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[NTFS transactionnel](https://go.microsoft.com/fwlink/?LinkID=165402)


