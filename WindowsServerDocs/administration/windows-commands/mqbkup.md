---
title: mqbkup
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7bdd41c4-75ef-455f-b241-1d64a4c7acf5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a809bfc788ba25afab280e80e5c6f0dc9c70dc86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842900"
---
# <a name="mqbkup"></a>mqbkup

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sauvegarde du Registre et les fichiers de messages MSMQ paramètres vers un dispositif de stockage et restaure les paramètres et des messages précédemment stocké.   
La sauvegarde et l’opération de restauration seront arrête le service MSMQ local. Si le service MSMQ a démarré au préalable, l’utilitaire tente de redémarrer le service MSMQ à la fin de la sauvegarde ou de l’opération de restauration. Si le service est déjà arrêté avant d’exécuter l’utilitaire, aucune tentative pour redémarrer le service est effectuée.  
Avant d’utiliser l’utilitaire de sauvegarde/restauration de Message MSMQ, vous devez fermer toutes les applications locales qui sont à l’aide de MSMQ.  
## <a name="syntax"></a>Syntaxe  
```  
mqbkup {/b | /r} <folder path_to_storage_device>  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|/b|Spécifie l’opération de sauvegarde|  
|/r|Spécifie l’opération de restauration|  
|< dossier path_to_storage\_appareil >|Spécifie le chemin d’accès où sont stockés les fichiers de messages MSMQ et les paramètres du Registre|  
|/?|Affiche l'aide à l'invite de commandes.|  
## <a name="BKMK_Examples"></a>Exemples  
Pour sauvegarder tous les paramètres de Registre et les fichiers de message MSMQ et de les stocker dans le *Msmqbkup* dossier sur votre lecteur C:.  
```  
mqbkup /b c:\msmqbkup  
```  
Si le dossier spécifié n’existe pas, l’utilitaire crée automatiquement un. Si vous choisissez de spécifier un dossier existant, ce dossier doit être vide. Si vous spécifiez un dossier vide, l’utilitaire supprimera tous les fichiers et sous-dossiers qu’il contient. Dans ce cas, vous devez accorder l’autorisation de supprimer les sous-dossiers et fichiers existants. Vous pouvez utiliser la **/y** paramètre pour indiquer que vous acceptez au préalable à la suppression de tous les fichiers et sous-dossiers existants dans le dossier spécifié.  
Pour supprimer tous les fichiers et sous-dossiers dans le *Oldbkup* dossier sur votre lecteur C: et les fichiers de messages MSMQ de magasin et les paramètres de Registre dans ce dossier.  
```  
mqbkup /b /y c:\oldbkup  
```  
Pour restaurer les paramètres de Registre et les messages MSMQ :  
```  
mqbkup /r c:\msmqbkup  
```  
Les emplacements des dossiers utilisés pour stocker les fichiers de messages MSMQ sont stockés dans le Registre. Par conséquent, l’utilitaire restaure les fichiers de messages MSMQ vers les dossiers spécifiés dans le Registre et non vers les dossiers de stockage utilisées avant l’opération de restauration. Si les dossiers spécifiés dans le Registre n’existent pas, l’opération de restauration crée automatiquement les. Si les répertoires de dossiers existent et ne sont pas vides, l’utilitaire vous invite d’autorisation de supprimer le contenu actuel de ces dossiers.  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
