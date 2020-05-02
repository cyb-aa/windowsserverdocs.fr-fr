---
title: DiskShadow
description: Rubrique de référence pour DiskShadow, qui est un outil qui expose les fonctionnalités offertes par le service de cliché instantané des volumes (VSS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e962537d-b759-4368-b6f1-e8391cf7b221
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: db1b1602bcbde41c2d92af925ff819ef390220e1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719424"
---
# <a name="diskshadow"></a>DiskShadow

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

DiskShadow. exe est un outil qui expose les fonctionnalités offertes par le service de cliché instantané des volumes (VSS). Par défaut, DiskShadow utilise un interpréteur de commandes interactif semblable à celui de DiskRAID ou DiskPart. DiskShadow comprend également un mode scriptable.  
  
> [!NOTE]  
> L’appartenance au groupe Administrateurs local, ou équivalent, est la condition minimale requise pour exécuter DiskShadow.  
  

## <a name="syntax"></a>Syntaxe  
pour le mode interactif, tapez la commande suivante à l’invite de commandes pour démarrer l’interpréteur de commandes DiskShadow :  
  
```  
diskshadow  
```  
  
pour le mode script, tapez la commande suivante, où *script. txt* est un fichier de script contenant les commandes DiskShadow :  
  
```  
diskshadow -s script.txt  
```  
  
### <a name="parameters"></a>Paramètres  
Vous pouvez exécuter les commandes suivantes dans l’interpréteur de commandes DiskShadow ou à l’aide d’un fichier de script :  
  
|Paramètre|Description|  
|-------|--------|  
|[set_2](set_2.md)|Définit le contexte, les options, le mode détaillé et le fichier de métadonnées pour la création de clichés instantanés.|  
|[Simuler la restauration](simulate-restore.md)|Teste l’implication de l’enregistreur dans les sessions de restauration sur l’ordinateur sans émettre des événements **prerestauration** ou **postRestore** aux rédacteurs.|  
|[Charger les métadonnées](load-metadata.md)|Charge un fichier de métadonnées. cab avant d’importer un cliché instantané transportable ou charge les métadonnées de l’enregistreur dans le cas d’une restauration.|  
|[writer](writer.md)|vérifie qu’un writer ou un composant est inclus ou exclut un enregistreur ou un composant de la procédure de sauvegarde ou de restauration.|  
|[add](add.md)|Ajoute des volumes à l’ensemble des volumes qui doivent être des clichés instantanés ou ajoute des alias à l’environnement d’alias.|  
|[create_1](create_1.md)|démarre le processus de création de clichés instantanés à l’aide du contexte et des paramètres d’option actuels.|  
|[exec](exec.md)|exécute un fichier sur l’ordinateur local.|  
|[Commencer la sauvegarde](begin-backup.md)|démarre une session de sauvegarde complète.|  
|[Terminer la sauvegarde](end-backup.md)|Met fin à une session de sauvegarde complète et émet un événement **BackupComplete** avec l’état du writer approprié, si nécessaire.|  
|[Commencer la restauration](begin-restore.md)|démarre une session de restauration et émet un événement de **prérestauration** pour les Writers impliqués.|  
|[Terminer la restauration](end-restore.md)|Met fin à une session de restauration et émet un événement **postRestore** pour les Writers impliqués.|  
|[reset](reset.md)|rétablit l’État par défaut de DiskShadow.|  
|[list](list.md)|répertorie les writers, les clichés instantanés ou les fournisseurs de clichés instantanés actuellement enregistrés qui se trouvent sur le système.|  
|[supprimer les ombres](delete-shadows.md)|supprime les clichés instantanés.|  
|[import](import.md)|importe un cliché instantané transportable à partir d’un fichier de métadonnées chargé dans le système.|  
|[masque](mask.md)|supprime les clichés instantanés matériels importés à l’aide de la commande **Importer** .|  
|[poser](expose.md)|expose un cliché instantané persistant sous la forme d’une lettre de lecteur, d’un partage ou d’un point de montage.|  
|[cessez](unexpose.md)|n’expose pas de cliché instantané qui a été exposé à l’aide de la commande **exposer** .|  
|[break_2](break_2.md)|Dissocie un volume de clichés instantanés de VSS.|  
|[rétablir](revert.md)|ramène un volume à un cliché instantané spécifié.|  
|[exit_1](exit_1.md)|quitte DiskShadow.|  
  
## <a name="remarks"></a>Notes   
  
-   Au minimum, il vous suffit d' **Ajouter** et de **créer** pour créer un cliché instantané. Toutefois, cela perdra les paramètres de contexte et d’option, sera une sauvegarde de copie et créera uniquement un cliché instantané sans script d’exécution de sauvegarde.  
  
## <a name="examples"></a>Exemples  
Il s’agit d’un exemple de séquence de commandes qui créera un cliché instantané pour la sauvegarde. Il peut être enregistré dans le fichier en tant que script. DSH et exécuté \/avec le script s de DiskShadow. DSH  
  
Supposons les éléments suivants :  
  
-   Vous avez un répertoire existant appelé c :\\diskshadowdata.  
  
-   Votre volume système est C : et votre volume de données est D :.  
  
-   Vous avez un fichier backupscript. cmd dans c :\\diskshadowdata.  
  
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
  
## <a name="additional-references"></a>Références supplémentaires  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

