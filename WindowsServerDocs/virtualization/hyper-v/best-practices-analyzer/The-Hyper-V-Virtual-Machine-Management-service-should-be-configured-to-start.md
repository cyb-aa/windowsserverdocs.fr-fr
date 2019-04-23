---
title: Le service de gestion d’ordinateurs virtuels Hyper-V doit être configuré pour démarrer automatiquement
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 222bbe76-c514-4a3f-b61b-860a4dc2826a
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c33f81678d7fdc71e81834a002fd3d7917a6f632
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833250"
---
# <a name="the-hyper-v-virtual-machine-management-service-should-be-configured-to-start-automatically"></a>Le service de gestion d’ordinateurs virtuels Hyper-V doit être configuré pour démarrer automatiquement

>S'applique à : Windows Server 2016

Pour plus d'informations sur les meilleures pratiques et les analyses, consultez [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  

Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.

## <a name="issue"></a>Problème  
  
*Le Service de gestion d’ordinateurs virtuels Hyper-V n’est pas configuré pour démarrer automatiquement.*  
  
## <a name="impact"></a>Impact  
  
*Machines virtuelles ne peuvent pas être gérés jusqu'à ce que le service est démarré.*  
  
Les machines virtuelles qui sont en cours d’exécution continue de s’exécuter. Toutefois, vous ne pourrez gérer des machines virtuelles, ou créer ou supprimez-les jusqu'à ce que le service est en cours d’exécution.  
  
## <a name="resolution"></a>Résolution  
  
*Utilisez l’outil Services de composant logiciel enfichable ou sc config de ligne de commande pour reconfigurer le service pour démarrer automatiquement.*  
  
> [!TIP]  
> Si vous ne trouvez pas le service dans l’application de bureau ou de l’outil de ligne de commande indique que le service n’existe pas, les outils de gestion Hyper-V probablement ne sont pas installés. Pour les installer :  
>   
> - Sur Windows Server, ouvrez le Gestionnaire de serveur et utilisez l’Assistant Ajout de rôles et fonctionnalités. Pour plus d’informations, consultez [installer le rôle Hyper-V sur Windows Server 2016](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).  
> - Sur Windows, à partir du bureau, commencez à taper **programmes**, cliquez sur **programmes et fonctionnalités** (panneau) > **ou désactiver des fonctionnalités Windows activer**  >   **Hyper-V** > **les outils de gestion Hyper-V**. Cliquez ensuite sur **OK**.  
  
#### <a name="to-reconfigure-the-service-to-start-automatically-using-the-services-desktop-app"></a>Pour reconfigurer le service pour démarrer automatiquement à l’aide de l’application de bureau de Services  
  
1.  Ouvrez l’application de bureau de Services. (Cliquez sur **Démarrer**, cliquez dans la zone de recherche, commencez à taper **Services**, puis cliquez sur Services dans la liste des résultats.  
  
2.  Dans le volet détails, cliquez sur **gestion d’ordinateurs virtuels Hyper-V**, puis cliquez sur **propriétés**.  
  
3.  Sur le **général** sous l’onglet **démarrage** tapez, cliquez sur **automatique**.  
  
#### <a name="to-reconfigure-the-service-to-start-automatically-using-the-sc-config-command"></a>Pour reconfigurer le service pour démarrer automatiquement à l’aide de la commande SC Config  
  
1.  Ouvrez Windows PowerShell.  
  
2.  Tapez :  
  
    ```  
    set-service  vmms -startuptype automatic  
    ```  
  
3.  Si le service n’est pas déjà en cours d’exécution, tapez :  
  
    ```  
    start-service -name vmms  
    ```  
  


