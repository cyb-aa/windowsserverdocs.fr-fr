---
title: verifier
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 782173d6-f196-4bc4-a547-76109829453c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cc2482fa25d0236991889c3951cb522e27bf520d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362687"
---
# <a name="verifier"></a>verifier

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gestionnaire du vérificateur de pilotes.  

## <a name="syntax"></a>Syntaxe  
```  
verifier /standard /driver <name> [<name> ...]  
verifier /standard /all  
verifier [/flags <flags>] [/faults [<probability> [<tags> [<applications> [<minutes>]]]] /driver <name> [<name>...]  
verifier [/flags FLAGS] [/faults [<probability> [<tags> [<applications> [<minutes>]]]] /all  
verifier /querysettings  
verifier /volatile /flags <flags>  
verifier /volatile /adddriver <name> [<name>...]  
verifier /volatile /removedriver <name> [<name>...]  
verifier /volatile /faults [<probability> [<tags> [<applications>]]  
verifier /reset  
verifier /query  
verifier /log <LogFileName> [/interval <seconds>]  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|indicateurs de \<>|Doit être un nombre en décimal ou hexadécimal, combinaison de bits :<br /><br />**valeur -   : description**<br />-   **bit 0 :** vérification du pool spécial<br />-   **bit 1 :** forcer la vérification IRQL<br />-   **bit 2 :** simulation de ressources faibles<br />-   **bit 3 :** suivi des pools<br />-   **bit 4 :** vérification des e/s<br />-   **bit 5 :** détection des verrous mortels<br />-   **bit 6 :** inutilisé<br />-   **bit 7 :** vérification DMA<br />-   **bit 8 :** vérifications de sécurité<br />-   **bit 9 :** forcer les demandes d’e/s en attente<br />-   **bit 10 :** journalisation IRP<br />-   **bit 11 :** vérifications diverses<br /><br />par exemple, **/Flags 27** est équivalent à **/Flags 0x1B**|  
|/volatile|Utilisé pour modifier dynamiquement les paramètres du vérificateur sans redémarrer le système. Les nouveaux paramètres seront perdus au redémarrage du système.|  
|probabilité de \<>|Nombre compris entre 1 et 10 000 spécifiant la probabilité d’injection d’erreurs. Par exemple, la spécification de 100 signifie une probabilité d’injection d’erreurs de 1% (100/10000).<br /><br />Si ce paramètre n’est pas spécifié, la probabilité par défaut de 6% sera utilisée.|  
|balises \<>|Spécifie les balises de pool qui seront injectées avec des erreurs, séparées par des espaces. Si ce paramètre n’est pas spécifié, toute allocation de pool peut être injectée avec des erreurs.|  
|applications \<>|Spécifie le nom du fichier image des applications qui seront injectées avec des erreurs, séparées par des espaces. Si ce paramètre n’est pas spécifié, une simulation de ressources faibles peut avoir lieu dans n’importe quelle application.|  
|\<minutes >|Nombre positif spécifiant la longueur de la période après le redémarrage, en minutes, pendant laquelle aucune injection d’erreur ne se produit. Si ce paramètre n’est pas spécifié, la longueur par défaut de 8 minutes sera utilisée.|  
|/?|Affiche l'aide à l'invite de commandes.|  

## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  