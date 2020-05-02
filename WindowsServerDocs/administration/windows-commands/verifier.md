---
title: verifier
description: Rubrique de référence pour le vérificateur, qui exécute le gestionnaire du vérificateur de pilote.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 782173d6-f196-4bc4-a547-76109829453c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0fda0caec9a0a3ea874c815d0e37c1d1a3ea9ce7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720296"
---
# <a name="verifier"></a>verifier

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
#### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|\<indicateurs>|Doit être un nombre en décimal ou hexadécimal, combinaison de bits :<p>-   **Valeur : description**<br />-   **bit 0 :** vérification de pool spécial<br />-   **bit 1 :** forcer la vérification IRQL<br />-   **bit 2 :** simulation de ressources faibles<br />-   **bit 3 :** suivi des pools<br />-   **bit 4 :** Vérification des e/s<br />-   **bit 5 :** détection des verrous mortels<br />-   **bit 6 :** inutilisé<br />-   **bit 7 :** Vérification de DMA<br />-   **bit 8 :** vérifications de sécurité<br />-   **bit 9 :** forcer les demandes d’e/s en attente<br />-   **bit 10 :** Journalisation IRP<br />-   **bit 11 :** vérifications diverses<p>par exemple, **/Flags 27** est équivalent à **/Flags 0x1B**|  
|/volatile|Utilisé pour modifier dynamiquement les paramètres du vérificateur sans redémarrer le système. Les nouveaux paramètres seront perdus au redémarrage du système.|  
|\<probabilité>|Nombre compris entre 1 et 10 000 spécifiant la probabilité d’injection d’erreurs. Par exemple, la spécification de 100 signifie une probabilité d’injection d’erreurs de 1% (100/10000).<p>Si ce paramètre n’est pas spécifié, la probabilité par défaut de 6% sera utilisée.|  
|\<balises>|Spécifie les balises de pool qui seront injectées avec des erreurs, séparées par des espaces. Si ce paramètre n’est pas spécifié, toute allocation de pool peut être injectée avec des erreurs.|  
|\<applications>|Spécifie le nom du fichier image des applications qui seront injectées avec des erreurs, séparées par des espaces. Si ce paramètre n’est pas spécifié, une simulation de ressources faibles peut avoir lieu dans n’importe quelle application.|  
|\<minutes>|Nombre positif spécifiant la longueur de la période après le redémarrage, en minutes, pendant laquelle aucune injection d’erreur ne se produit. Si ce paramètre n’est pas spécifié, la longueur par défaut de 8 minutes sera utilisée.|  
|/?|Affiche l'aide à l'invite de commandes.|  

## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  