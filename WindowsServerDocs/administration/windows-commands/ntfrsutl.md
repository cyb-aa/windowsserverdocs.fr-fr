---
title: ntfrsutl
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d7721a19-5a87-4ab6-b816-65d2da2c811f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 14e718550247b8854073407146456d366d562d2b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837992"
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
  
#### <a name="parameters"></a>Paramètres  
  
|  Paramètre  |                                                                                                                                                                                                                                                                                                                                        Description                                                                                                                                                                                                                                                                                                                                         |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   idtable   |                                                                                                                                                                                                                                                                                                                                          Table d’ID                                                                                                                                                                                                                                                                                                                                          |
| configtable |                                                                                                                                                                                                                                                                                                                                  Table de configuration FRS                                                                                                                                                                                                                                                                                                                                   |
|    inlog    |                                                                                                                                                                                                                                                                                                                                        Journal entrant                                                                                                                                                                                                                                                                                                                                         |
|   Outlog    |                                                                                                                                                                                                                                                                                                                                        Journal sortant                                                                                                                                                                                                                                                                                                                                        |
| <computer>  |                                                                                                                                                                                                                                                                                                                                  Spécifie l’ordinateur.                                                                                                                                                                                                                                                                                                                                   |
|   mémoire    |                                                                                                                                                                                                                                                                                                                                        Utilisation de la mémoire                                                                                                                                                                                                                                                                                                                                        |
|   threads   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|    étape    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|     SD      |                                                                                                                                                                                                                                                                                                                         répertorie la vue du service NTFRS du service d’annuaire.                                                                                                                                                                                                                                                                                                                          |
|    jeux     |                                                                                                                                                                                                                                                                                                                             Spécifie les jeux de réplicas actifs                                                                                                                                                                                                                                                                                                                              |
|   version   |                                                                                                                                                                                                                                                                                                                       Spécifie les versions de service API et NTFRS.                                                                                                                                                                                                                                                                                                                        |
|    sondages     | Spécifie les fréquences d’interrogation actuelles.<p>Paramètres :<p><ul><li>**\/rapidement**\[ **\=** \[ <N>\]\]\(\)<p><ul><li>\- **rapidement** les interrogations jusqu’à ce que la configuration soit stable rectrieved</li><li>**\=rapidement** \- interroge rapidement chaque minute par défaut.</li><li>**\=rapidement**<N> \- interrogations rapidement toutes les *N* minutes</li></ul></li><li>**\/lentement**\[ **\=** \[ <N>\]\] \(\)<p><ul><li>\- **lentement** les interrogations jusqu’à la récupération de la configuration stable</li><li>**lent\=** \- interrogations lentement toutes les minutes par défaut</li><li>**\=lentement**<N> \- interrogations rapidement toutes les *N* minutes</li></ul></li><li>**\/maintenant** \(des interrogations maintenant\)</li></ul> |
|     \/ ?     |                                                                                                                                                                                                                                                                                                                            Affiche l'aide à l'invite de commandes.                                                                                                                                                                                                                                                                                                                            |
  
## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
Pour déterminer l’intervalle d’interrogation pour la réplication de fichiers :  
  
```  
C:\Program Files\SupportTools>ntfrsutl poll wrkstn-1  
```  
  
Pour déterminer l’interface du programme d’application NTFRS actuelle \(version\) API :  
  
```  
C:\Program Files\SupportTools>ntfrsutl version  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  
  
  

