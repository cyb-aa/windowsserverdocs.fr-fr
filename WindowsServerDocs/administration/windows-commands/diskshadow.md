---
title: diskshadow
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8d9f34377473608d71ce7753972e5312d9eea0f6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377769"
---
# <a name="diskshadow"></a>diskshadow

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

DiskShadow. exe est un outil qui expose les fonctionnalités offertes par le service de cliché instantané des volumes \(VSS\). Par défaut, DiskShadow utilise un interpréteur de commandes interactif semblable à celui de DiskRAID ou DiskPart. DiskShadow comprend également un mode scriptable.  
  
> [!NOTE]  
> L’appartenance au groupe Administrateurs local, ou équivalent, est la condition minimale requise pour exécuter DiskShadow.  
  
pour obtenir des exemples d’utilisation de commandes DiskShadow, consultez [exemples](#BKMK_examples).  
  
## <a name="syntax"></a>Syntaxe  
pour le mode interactif, tapez la commande suivante à l’invite de commandes pour démarrer l’interpréteur de commandes DiskShadow :  
  
```  
diskshadow  
```  
  
pour le mode script, tapez la commande suivante, où *script. txt* est un fichier de script contenant les commandes DiskShadow :  
  
```  
diskshadow -s script.txt  
```  
  
## <a name="diskshadow-commands"></a>commandes DiskShadow  
Vous pouvez exécuter les commandes suivantes dans l’interpréteur de commandes DiskShadow ou à l’aide d’un fichier de script :  
  
|Paramètre|Description|  
|-------|--------|  
|[set_2](set_2.md)|Définit le contexte, les options, le mode détaillé et le fichier de métadonnées pour la création de clichés instantanés.|  
|[Simuler la restauration](simulate-restore.md)|Teste l’implication de l’enregistreur dans les sessions de restauration sur l’ordinateur sans émettre des événements **prerestauration** ou **postRestore** aux rédacteurs.|  
|[Charger les métadonnées](load-metadata.md)|Charge un fichier de métadonnées. cab avant d’importer un cliché instantané transportable ou charge les métadonnées de l’enregistreur dans le cas d’une restauration.|  
|[écrit](writer.md)|Vérifie qu’un writer ou un composant est inclus ou exclut un enregistreur ou un composant de la procédure de sauvegarde ou de restauration.|  
|[add_1](add_1.md)|Ajoute des volumes à l’ensemble des volumes qui doivent être des clichés instantanés ou ajoute des alias à l’environnement d’alias.|  
|[create_1](create_1.md)|démarre le processus de création de clichés instantanés à l’aide du contexte et des paramètres d’option actuels.|  
|[exécutable](exec.md)|exécute un fichier sur l’ordinateur local.|  
|[Commencer la sauvegarde](begin-backup.md)|démarre une session de sauvegarde complète.|  
|[Terminer la sauvegarde](end-backup.md)|Met fin à une session de sauvegarde complète et émet un événement **BackupComplete** avec l’état du writer approprié, si nécessaire.|  
|[Commencer la restauration](begin-restore.md)|démarre une session de restauration et émet un événement de **prérestauration** pour les Writers impliqués.|  
|[Terminer la restauration](end-restore.md)|Met fin à une session de restauration et émet un événement **postRestore** pour les Writers impliqués.|  
|[initialisation](reset.md)|rétablit l’État par défaut de DiskShadow.|  
|[tarifs](list.md)|répertorie les writers, les clichés instantanés ou les fournisseurs de clichés instantanés actuellement enregistrés qui se trouvent sur le système.|  
|[supprimer les ombres](delete-shadows.md)|supprime les clichés instantanés.|  
|[import](import.md)|importe un cliché instantané transportable à partir d’un fichier de métadonnées chargé dans le système.|  
|[mask](mask.md)|supprime les clichés instantanés matériels importés à l’aide de la commande **Importer** .|  
|[poser](expose.md)|Expose un cliché instantané persistant sous la forme d’une lettre de lecteur, d’un partage ou d’un point de montage.|  
|[cessez](unexpose.md)|N’expose pas de cliché instantané qui a été exposé à l’aide de la commande **exposer** .|  
|[break_2](break_2.md)|Dissocie un volume de clichés instantanés de VSS.|  
|[rétablir](revert.md)|ramène un volume à un cliché instantané spécifié.|  
|[exit_1](exit_1.md)|quitte DiskShadow.|  
  
## <a name="remarks"></a>Notes  
  
-   au minimum, il vous suffit d' **Ajouter** et de **créer** pour créer un cliché instantané. Toutefois, cela perdra les paramètres de contexte et d’option, sera une sauvegarde de copie et créera uniquement un cliché instantané sans script d’exécution de sauvegarde.  
  
## <a name="BKMK_examples"></a>Illustre  
Il s’agit d’un exemple de séquence de commandes qui créera un cliché instantané pour la sauvegarde. Il peut être enregistré dans le fichier en tant que script. DSH, et exécuté avec DiskShadow \/s script. DSH  
  
Supposons les éléments suivants :  
  
-   Vous avez un répertoire existant appelé c :\\diskshadowdata.  
  
-   Votre volume système est C : et votre volume de données est D :.  
  
-   Vous avez un fichier backupscript. cmd en c :\\diskshadowdata.  
  
-   Votre fichier backupscript. cmd effectuera la copie des données Shadow p : et q : sur votre lecteur de sauvegarde.  
  
Vous pouvez entrer ces commandes manuellement ou les créer dans un script :  
  
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
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

