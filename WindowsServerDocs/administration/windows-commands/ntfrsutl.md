---
title: ntfrsutl
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d7721a19-5a87-4ab6-b816-65d2da2c811f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1301b6876698e9eb552ae0ef9e70ed278319a7c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372622"
---
# <a name="ntfrsutl"></a>ntfrsutl

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Vide les informations sur les tables, les threads et la mémoire internes pour le service de réplication de fichiers NT \(NTFRS @ no__t-1. Il s’exécute sur les serveurs locaux et distants. Le paramètre de récupération pour NTFRS dans le gestionnaire de contrôle des services \(SCM @ no__t-1 peut être essentiel pour localiser et conserver des événements de journal importants sur l’ordinateur. Cet outil fournit une méthode pratique pour passer en revue ces paramètres.   
  
## <a name="syntax"></a>Syntaxe  
  
```  
ntfrsutl[idtable|configtable|inlog|outlog][<computer>]  
ntfrsutl[memory|threads|stage][<computer>]  
ntfrsutl ds[<computer>]  
ntfrsutl [sets][<computer>]  
ntfrsutl [version][<computer>]  
ntfrsutl poll[/quickly[=[<N>]]][/slowly[=[<N>]]][/now][<computer>]  
```  
  
### <a name="parameters"></a>Paramètres  
  
|  Paramètre  |                                                                                                                                                                                                                                                                                                                                        Description                                                                                                                                                                                                                                                                                                                                         |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   idtable   |                                                                                                                                                                                                                                                                                                                                          Table d’ID                                                                                                                                                                                                                                                                                                                                          |
| configtable |                                                                                                                                                                                                                                                                                                                                  Table de configuration FRS                                                                                                                                                                                                                                                                                                                                   |
|    inlog    |                                                                                                                                                                                                                                                                                                                                        Journal entrant                                                                                                                                                                                                                                                                                                                                         |
|   Outlog    |                                                                                                                                                                                                                                                                                                                                        Journal sortant                                                                                                                                                                                                                                                                                                                                        |
| <computer>  |                                                                                                                                                                                                                                                                                                                                  Spécifie l’ordinateur.                                                                                                                                                                                                                                                                                                                                   |
|   memory    |                                                                                                                                                                                                                                                                                                                                        Utilisation de la mémoire                                                                                                                                                                                                                                                                                                                                        |
|   Thèmes   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|    étape    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|     SD      |                                                                                                                                                                                                                                                                                                                         répertorie la vue du service NTFRS du service d’annuaire.                                                                                                                                                                                                                                                                                                                          |
|    Paramétr     |                                                                                                                                                                                                                                                                                                                             Spécifie les jeux de réplicas actifs                                                                                                                                                                                                                                                                                                                              |
|   version   |                                                                                                                                                                                                                                                                                                                       Spécifie les versions de service API et NTFRS.                                                                                                                                                                                                                                                                                                                        |
|    sondages     | Spécifie les fréquences d’interrogation actuelles.<br /><br />Paramètres :<br /><br /><ul><li>**\/quickly**\[ **\=** \[ <N> @ no__t-7 @ no__t-8 @ no__t-9Polls rapidement @ no__t-10<br /><br /><ul><li>interrogation **rapide @no__t-** 1 jusqu’à ce que la configuration stable soit rectrieved</li><li>**rapidement @ no__t-1** \- interroge rapidement toutes les minutes par défaut.</li><li>**rapidement @ no__t-1**<N> \- sondes rapidement toutes les *N* minutes</li></ul></li><li>**\/slowly**\[ **\=** \[ <N> @ no__t-7 @ no__t-8 \(Polls lentement @ no__t-10<br /><br /><ul><li>**lenteur** d’interrogation \- lentement jusqu’à la récupération de la configuration stable</li><li>**lentement @ no__t-1** \- interroge lentement toutes les minutes par défaut</li><li>**lenteur de @ no__t-1**<N> \- sondes rapidement toutes les *N* minutes</li></ul></li><li>**\/now** \(Polls maintenant @ no__t-3</li></ul> |
|     \/?     |                                                                                                                                                                                                                                                                                                                            Affiche l'aide à l'invite de commandes.                                                                                                                                                                                                                                                                                                                            |
  
## <a name="BKMK_Examples"></a>Illustre  
Pour déterminer l’intervalle d’interrogation pour la réplication de fichiers :  
  
```  
C:\Program Files\SupportTools>ntfrsutl poll wrkstn-1  
```  
  
Pour déterminer la version actuelle de l’interface du programme d’application NTFRS \(API @ no__t-1, procédez comme suit :  
  
```  
C:\Program Files\SupportTools>ntfrsutl version  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  
  
  

