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

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vide les informations sur les tables, les threads et la mémoire internes du service de réplication de fichiers NT \(NTFRS\). Il s’exécute sur les serveurs locaux et distants. Le paramètre de récupération pour NTFRS dans le gestionnaire de contrôle des services \(\) SCM peut être essentiel pour localiser et conserver des événements de journal importants sur l’ordinateur. Cet outil fournit une méthode pratique pour passer en revue ces paramètres.   
  
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
|   thèmes   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|    étape    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|     SD      |                                                                                                                                                                                                                                                                                                                         répertorie la vue du service NTFRS du service d’annuaire.                                                                                                                                                                                                                                                                                                                          |
|    paramétr     |                                                                                                                                                                                                                                                                                                                             Spécifie les jeux de réplicas actifs                                                                                                                                                                                                                                                                                                                              |
|   version   |                                                                                                                                                                                                                                                                                                                       Spécifie les versions de service API et NTFRS.                                                                                                                                                                                                                                                                                                                        |
|    Sondages     | Spécifie les fréquences d’interrogation actuelles.<br /><br />Paramètres :<br /><br /><ul><li>**\/rapidement**\[ **\=** \[ <N>\]\]\(\)<br /><br /><ul><li>\- **rapidement** les interrogations jusqu’à ce que la configuration soit stable rectrieved</li><li>**\=rapidement** \- interroge rapidement chaque minute par défaut.</li><li>**\=rapidement**<N> \- interrogations rapidement toutes les *N* minutes</li></ul></li><li>**\/lentement**\[ **\=** \[ <N>\]\] \(\)<br /><br /><ul><li>\- **lentement** les interrogations jusqu’à la récupération de la configuration stable</li><li>**lent\=** \- interrogations lentement toutes les minutes par défaut</li><li>**\=lentement**<N> \- interrogations rapidement toutes les *N* minutes</li></ul></li><li>**\/maintenant** \(des interrogations maintenant\)</li></ul> |
|     \/ ?     |                                                                                                                                                                                                                                                                                                                            Affiche l'aide à l'invite de commandes.                                                                                                                                                                                                                                                                                                                            |
  
## <a name="BKMK_Examples"></a>Illustre  
Pour déterminer l’intervalle d’interrogation pour la réplication de fichiers :  
  
```  
C:\Program Files\SupportTools>ntfrsutl poll wrkstn-1  
```  
  
Pour déterminer l’interface du programme d’application NTFRS actuelle \(version\) API :  
  
```  
C:\Program Files\SupportTools>ntfrsutl version  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  
  
  

