---
title: verifier
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2ab0833d4fdb11c4962d4916ec2e32097e08ca04
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865870"
---
# <a name="verifier"></a>verifier

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gestionnaire du vérificateur de pilote.  

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
|\<flags>|Doit être un nombre décimal ou hexadécimal, combinaison de bits :<br /><br />-   **Valeur : description**<br />-   **bit 0 :** vérification du pool spécial<br />-   **bit 1 :** vérification irql forcée<br />-   **bit 2 :** faible de simulation de ressources<br />-   **bit 3 :** suivi de pool<br />-   **bit 4 :** Vérification d’e/s<br />-   **bit 5 :** détection de blocage<br />-   **bit 6 :** inutilisées<br />-   **bit 7 :** Vérification de DMA<br />-   **bit 8 :** vérifications de sécurité<br />-   **bit 9 :** forcer les demandes d’e/s en attente<br />-   **bit 10 :** Journalisation de l’IRP<br />-   **bit 11 :** chèques divers<br /><br />par exemple, **/Flags 27** équivaut avec **/Flags 0x1B**|  
|/volatile|Utilisé pour modifier les paramètres de vérificateur de manière dynamique sans avoir à redémarrer le système. Tous les nouveaux paramètres seront perdues lorsque le système est redémarré.|  
|\<probability>|Nombre compris entre 1 et 10 000 spécifiant la probabilité d’injection d’erreur. Par exemple, 100 indique une probabilité d’injection d’erreur de 1 % (100/10 000).<br /><br />Si ce paramètre n’est pas spécifié, puis la probabilité de défaut de 6 % sera utilisée.|  
|\<tags>|Spécifie les balises de pool qui seront injectées avec des erreurs, séparés par des espaces. Si ce paramètre n’est pas spécifié toute allocation de pool peut être injectée avec des erreurs.|  
|\<applications>|Spécifie le nom du fichier image des applications qui seront injectées avec des erreurs, séparés par des espaces. Si ce paramètre n’est pas spécifié la simulation de manque de ressources peut prendre place dans n’importe quelle application.|  
|\<minutes>|Un nombre positif indiquant la longueur de la période après le redémarrage, en minutes, durant l’aucune erreur, l’injection de code se produisent. Si ce paramètre n’est pas spécifié, puis la longueur par défaut de 8 minutes sera utilisée.|  
|/?|Affiche l'aide à l'invite de commandes.|  

## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  