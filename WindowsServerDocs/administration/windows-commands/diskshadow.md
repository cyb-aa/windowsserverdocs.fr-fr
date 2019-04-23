---
title: diskshadow
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e962537d-b759-4368-b6f1-e8391cf7b221
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b2c5648235a1c856c6aef09621e2381e74d08d70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869180"
---
# <a name="diskshadow"></a>diskshadow

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

DiskShadow.exe est un outil qui expose les fonctionnalités offertes par VSS Service \(VSS\). Par défaut, diskshadow utilise un interpréteur de commandes interactif similaire à celui de diskraid ou de DiskPart. DiskShadow inclut également un mode scriptable.  
  
> [!NOTE]  
> L’appartenance au groupe Administrateurs local, ou équivalent, est la condition minimale requise pour exécuter diskshadow.  
  
Pour obtenir des exemples montrant comment utiliser les commandes de diskshadow, voir [exemples](#BKMK_examples).  
  
## <a name="syntax"></a>Syntaxe  
pour le mode interactif, tapez ce qui suit à l’invite de commande pour démarrer l’interpréteur de commandes diskshadow :  
  
```  
diskshadow  
```  
  
pour le mode script, tapez la commande suivante, où *script.txt* est un fichier de script contenant des commandes de diskshadow :  
  
```  
diskshadow -s script.txt  
```  
  
## <a name="diskshadow-commands"></a>commandes de DiskShadow  
Vous pouvez exécuter les commandes suivantes dans l’interpréteur de commandes diskshadow ou via un fichier de script :  
  
|Paramètre|Description|  
|-------|--------|  
|[set_2](set_2.md)|Définit le contexte, les options, mode détaillé et fichier de métadonnées pour la création de clichés instantanés.|  
|[Simuler la restauration](simulate-restore.md)|Implication de l’enregistreur des tests dans des sessions de restauration sur l’ordinateur sans émettre de **PreRestore** ou **PostRestore** événements pour les writers.|  
|[Chargement des métadonnées](load-metadata.md)|Charge un fichier .cab de métadonnées avant d’importer une copie de clichés instantanés transportables ou charge les métadonnées de writer dans le cas d’une restauration.|  
|[writer](writer.md)|Vérifie qu’un enregistreur ou le composant est inclus ou exclut un enregistreur ou un composant à partir de la procédure de sauvegarde ou de restauration.|  
|[add_1](add_1.md)|Ajoute des volumes à l’ensemble de volumes qui doivent être des clichés instantanés, ou ajoute des alias à l’environnement de l’alias.|  
|[create_1](create_1.md)|démarre le processus de création de copie de clichés instantanés, en utilisant les paramètres de contexte et l’option actuelles.|  
|[exec](exec.md)|exécute un fichier sur l’ordinateur local.|  
|[Commencer la sauvegarde](begin-backup.md)|démarre une session de sauvegarde complète.|  
|[Terminer la sauvegarde](end-backup.md)|Met fin à une session de sauvegarde complète et les problèmes un **Backupcomplete** événement avec l’état du writer approprié, si nécessaire.|  
|[Début de restauration](begin-restore.md)|Démarre une session de restauration et les problèmes un **PreRestore** événement pour les writers impliqués.|  
|[Fin de restauration](end-restore.md)|Met fin à une session de restauration et les problèmes un **PostRestore** événement pour les writers impliqués.|  
|[reset](reset.md)|Rétablit l’état par défaut diskshadow.|  
|[list](list.md)|Répertorie les enregistreurs, clichés instantanés ou actuellement des fournisseurs qui se trouvent sur le système.|  
|[supprimer les ombres](delete-shadows.md)|Supprime les clichés instantanés.|  
|[import](import.md)|importe une copie de clichés instantanés transportables à partir d’un fichier de métadonnées chargées dans le système.|  
|[mask](mask.md)|Supprime les clichés instantanés de matériels qui ont été importés à l’aide de la **importer** commande.|  
|[expose](expose.md)|expose un cliché instantané persistant comme lettre de lecteur, le partage ou le point de montage.|  
|[unexpose](unexpose.md)|unexposes un cliché instantané qui a été exposé à l’aide de la **exposer** commande.|  
|[break_2](break_2.md)|Dissocie un volume de clichés instantanés de VSS.|  
|[revert](revert.md)|Rétablit un volume vers un cliché instantané spécifié.|  
|[exit_1](exit_1.md)|quitte diskshadow.|  
  
## <a name="remarks"></a>Notes  
  
-   au minimum, uniquement **ajouter** et **créer** sont nécessaires pour créer un cliché instantané. Toutefois, cela annulera les paramètres de contexte et option sera une sauvegarde de copie et créera uniquement un cliché instantané avec aucun script de l’exécution de sauvegardes.  
  
## <a name="BKMK_examples"></a>Exemples  
Il s’agit d’un exemple de séquence de commandes qui créera un cliché instantané pour la sauvegarde. Il peut être enregistré dans un fichier script.dsh et exécutée avec diskshadow \/s script.dsh  
  
Supposons que les éléments suivants :  
  
-   Vous avez un répertoire existant appelé c:\\diskshadowdata.  
  
-   Votre volume système est C: et votre volume de données est D:.  
  
-   Vous avez un fichier backupscript.cmd dans c:\\diskshadowdata.  
  
-   Votre fichier backupscript.cmd effectue la copie de clichés instantanés données p: et q : sur votre disque de sauvegarde.  
  
Vous pouvez entrer ces commandes manuellement ou écrire des scripts :  
  
```  
#diskshadow script file  
set context persistent nowriters  
set metadata c:\diskshadowdata\example.cab  
set verbose on  
begin backup  
add volume c: alias Systemvolumeshadow  
add volume d: alias Datavolumeshadow  
  
create  
  
expose %Systemvolumeshadow% p:  
expose %Datavolumeshadow% q:  
exec c:\diskshadowdata\backupscript.cmd  
end backup  
#End of script  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
  

