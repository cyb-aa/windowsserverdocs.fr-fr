---
title: ntfrsutl
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e94d2b0a9ca764a6e8a25a087817598e3f158581
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436402"
---
# <a name="ntfrsutl"></a>ntfrsutl

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exporte les tables internes, de thread et informations de mémoire pour le Service de réplication de fichiers NT \(NTFRS\). Il s’exécute sur les serveurs locaux et distants. Le paramètre de NTFRS dans le Gestionnaire de contrôle de Service de récupération \(SCM\) peut s’avérer primordiale pour la localisation et de conservation des événements du journal IMPORTANT sur l’ordinateur. Cet outil fournit une méthode pratique consistant à vérifier ces paramètres.   
  
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
|   IDTable   |                                                                                                                                                                                                                                                                                                                                          Tableau d’ID                                                                                                                                                                                                                                                                                                                                          |
| configtable |                                                                                                                                                                                                                                                                                                                                  Table de configuration FRS                                                                                                                                                                                                                                                                                                                                   |
|    inlog    |                                                                                                                                                                                                                                                                                                                                        Journaux de trafic entrant                                                                                                                                                                                                                                                                                                                                         |
|   outlog    |                                                                                                                                                                                                                                                                                                                                        Taille du journal                                                                                                                                                                                                                                                                                                                                        |
| <computer>  |                                                                                                                                                                                                                                                                                                                                  Spécifie l’ordinateur.                                                                                                                                                                                                                                                                                                                                   |
|   memory    |                                                                                                                                                                                                                                                                                                                                        Utilisation de la mémoire                                                                                                                                                                                                                                                                                                                                        |
|   Threads   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|    étape    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|     ds      |                                                                                                                                                                                                                                                                                                                         Répertorie la vue du service NTFRS du service d’annuaire.                                                                                                                                                                                                                                                                                                                          |
|    Jeux     |                                                                                                                                                                                                                                                                                                                             Spécifie les jeux de réplicas actifs                                                                                                                                                                                                                                                                                                                              |
|   version   |                                                                                                                                                                                                                                                                                                                       Spécifie les versions de service NTFRS et API.                                                                                                                                                                                                                                                                                                                        |
|    Interrogation     | Spécifie les intervalles d’interrogation actuelle.<br /><br />Paramètres :<br /><br /><ul><li>**\/rapidement** \[ **\=** \[ <N> \] \] \(interroge rapidement\)<br /><br /><ul><li>**rapidement** \- interroge rapidement jusqu'à ce que configuration stable est rectrieved</li><li>**rapidement\=**  \- interroge rapidement toutes les minutes par défaut.</li><li>**rapidement\=**  <N> \- interroge rapidement chaque *N* minutes</li></ul></li><li>**\/lentement** \[ **\=** \[ <N> \] \] \(interroge lentement\)<br /><br /><ul><li>**lentement** \- interroge lentement jusqu'à ce que configuration stable est récupérée.</li><li>**lentement\=**  \- interroge lentement toutes les minutes par défaut</li><li>**lentement\=**  <N> \- interroge rapidement chaque *N* minutes</li></ul></li><li>**\/maintenant** \(interroge maintenant\)</li></ul> |
|     \/ ?     |                                                                                                                                                                                                                                                                                                                            Affiche l'aide à l'invite de commandes.                                                                                                                                                                                                                                                                                                                            |
  
## <a name="BKMK_Examples"></a>Exemples  
Pour déterminer la fréquence d’interrogation pour la réplication de fichier :  
  
```  
C:\Program Files\SupportTools>ntfrsutl poll wrkstn-1  
```  
  
Pour déterminer l’interface de programme d’application NTFRS actuelle \(API\) version :  
  
```  
C:\Program Files\SupportTools>ntfrsutl version  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  
  
  

