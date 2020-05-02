---
title: mqbkup
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7bdd41c4-75ef-455f-b241-1d64a4c7acf5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 25bc5a0b5da0c78ccc68cf7b8fc70a5b4ec11a1f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723907"
---
# <a name="mqbkup"></a>mqbkup

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sauvegarde les fichiers de messages et les paramètres de Registre MSMQ sur un dispositif de stockage et restaure les messages et paramètres précédemment stockés.   
La sauvegarde et l’opération de restauration interrompent le service MSMQ local. Si le service MSMQ a été démarré au préalable, l’utilitaire tente de redémarrer le service MSMQ à la fin de la sauvegarde ou de l’opération de restauration. Si le service a déjà été arrêté avant l’exécution de l’utilitaire, aucune tentative de redémarrage du service n’est effectuée.  
Avant d’utiliser l’utilitaire de sauvegarde/restauration des messages MSMQ, vous devez fermer toutes les applications locales qui utilisent MSMQ.  
## <a name="syntax"></a>Syntaxe  
```  
mqbkup {/b | /r} <folder path_to_storage_device>  
```  
#### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|/b|Spécifie l’opération de sauvegarde|  
|/r|Spécifie l’opération de restauration|  
|Dossier <path_to_storage\_de l’appareil>|Spécifie le chemin d’accès de stockage des fichiers de messages et des paramètres de Registre MSMQ|  
|/?|Affiche l'aide à l'invite de commandes.|  
## <a name="examples"></a>Exemples  
Pour sauvegarder tous les fichiers de messages MSMQ et les paramètres du Registre, et les stocker dans le dossier *Msmqbkup* sur votre lecteur C :.  
```  
mqbkup /b c:\msmqbkup  
```  
Si le dossier spécifié n’existe pas, l’utilitaire en crée un automatiquement. Si vous choisissez de spécifier un dossier existant, ce dossier doit être vide. Si vous spécifiez un dossier non vide, l’utilitaire supprimera tous les fichiers et sous-dossiers qu’il contient. Dans ce cas, vous êtes invité à autoriser la suppression des fichiers et sous-dossiers existants. Vous pouvez utiliser le paramètre **/y** pour indiquer que vous acceptez au préalable la suppression de tous les fichiers et sous-dossiers existants dans le dossier spécifié.  
Pour supprimer tous les fichiers et sous-dossiers du dossier *Oldbkup* sur votre lecteur C : et stocker les fichiers de message et les paramètres de Registre MSMQ dans ce dossier.  
```  
mqbkup /b /y c:\oldbkup  
```  
Pour restaurer les messages MSMQ et les paramètres du Registre :  
```  
mqbkup /r c:\msmqbkup  
```  
Les emplacements des dossiers utilisés pour stocker les fichiers de messages MSMQ sont stockés dans le registre. Ainsi, l’utilitaire restaure les fichiers de messages MSMQ dans les dossiers spécifiés dans le registre, et non dans les dossiers de stockage utilisés avant l’opération de restauration. Si les dossiers spécifiés dans le registre n’existent pas, l’opération de restauration les crée automatiquement. Si des répertoires de dossiers existent et ne sont pas vides, l’utilitaire vous invite à entrer l’autorisation de supprimer le contenu actuel de ces dossiers.  
## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
