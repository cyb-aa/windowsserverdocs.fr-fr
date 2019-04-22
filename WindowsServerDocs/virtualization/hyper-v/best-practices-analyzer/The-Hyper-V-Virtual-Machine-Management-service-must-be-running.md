---
title: Le service de gestion d’ordinateurs virtuels Hyper-V doit être en cours d’exécution
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: f44d6887-6458-4438-9d93-574587e3f7d1
author: KBDAzure
ms.date: 10/03/2016
ms.openlocfilehash: 58886b68ca30ddeb064fc12c6cb4c00183399715
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826110"
---
# <a name="the-hyper-v-virtual-machine-management-service-must-be-running"></a>Le service de gestion d’ordinateurs virtuels Hyper-V doit être en cours d’exécution

>S'applique à : Windows Server 2016
  
Pour plus d'informations sur les meilleures pratiques et les analyses, consultez [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Erreur|  
|**Catégorie**|Prérequis|  

Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.

## <a name="issue"></a>Problème  
  
*Le service requis pour gérer des machines virtuelles ne fonctionne pas.*  
  
## <a name="impact"></a>Impact  
  
*Aucune opération de gestion de machine virtuelle ne peut être effectuée.*  
  
Les machines virtuelles qui sont en cours d’exécution continue de s’exécuter. Toutefois, vous ne pourrez gérer des machines virtuelles, ou créer ou supprimez-les jusqu'à ce que le service est en cours d’exécution.  
  
## <a name="resolution"></a>Résolution  
  
*Utilisez le composant logiciel enfichable Services ou l’outil de ligne de commande Sc config pour reconfigurer le service pour démarrer automatiquement.*  
  
> [!TIP]  
> Si vous ne trouvez pas le service dans l’application de bureau ou de l’outil de ligne de commande indique que le service n’existe pas, les outils de gestion Hyper-V probablement ne sont pas installés. Et si vous n’êtes pas en mesure de voir la console MMC de Hyper-V dans le menu Démarrer, vous devez installer les outils de gestion Hyper-V.

Pour installer les outils de gestion Hyper-V :  
>   
> - Sur Windows Server, ouvrez le Gestionnaire de serveur et utilisez l’Assistant Ajout de rôles et fonctionnalités. Pour plus d’informations, consultez [installer le rôle Hyper-V sur Windows Server 2016](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).  Vous pouvez également utiliser PowerShell pour installer les outils (`Install-WindowsFeature -Name Hyper-V-Tools, Hyper-V-PowerShell`) 
> - Sur Windows, à partir du bureau, commencez à taper **programmes**, cliquez sur **programmes et fonctionnalités** (panneau) > **ou désactiver des fonctionnalités Windows activer**  >   **Hyper-V** > **les outils de gestion Hyper-V**. Cliquez ensuite sur **OK**.  
  
### <a name="to-reconfigure-the-service-to-start-automatically-using-the-services-desktop-app"></a>Pour reconfigurer le service pour démarrer automatiquement à l’aide de l’application de bureau de Services  
  
1.  Ouvrez l’application de bureau de Services. (Cliquez sur **Démarrer**, cliquez dans le **rechercher** , tapez **services.msc**, puis appuyez sur ENTRÉE.)  
  
2.  Dans le volet détails, cliquez sur **gestion d’ordinateurs virtuels Hyper-V**, puis cliquez sur **propriétés**.  
  
3.  Sur le **général** sous l’onglet **démarrage** tapez, cliquez sur **automatique**.  
  
4.  Pour démarrer le service, cliquez sur **Démarrer**.  
  
### <a name="to-reconfigure-the-service-to-start-automatically-using-sc-config"></a>Pour reconfigurer le service pour démarrer automatiquement à l’aide de SC Config  
  
1.  Ouvrez Windows PowerShell. (À partir du bureau, cliquez sur **Démarrer** et commencez à taper **Windows PowerShell**.)  
  
2.  Avec le bouton droit **Windows PowerShell** et cliquez sur **exécuter en tant qu’administrateur**.  
  
3.  Pour reconfigurer le service, tapez :  
  
    ```  
    sc config vmms start=auto  
    ```  
  
4.  Pour démarrer le service, tapez :  
  
    ```  
    sc start vmms  
    ```  
  
Si le service est déjà configuré pour démarrer automatiquement et vous souhaitez de redémarrer le service, vous pouvez le faire à partir du Gestionnaire Hyper-V ou à partir de la commande « sc start vmms » ci-dessus.  
  
#### <a name="to-restart-the-service-from-hyper-v-manager"></a>Pour redémarrer le service à partir du Gestionnaire Hyper-V  
  
1.  Ouvrez le Gestionnaire Hyper-V. Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestionnaire Hyper-V**.  
  
2.  Dans le volet de navigation, cliquez sur le nom du serveur s’il n’est pas déjà sélectionné.  
  
3.  Dans le **Actions** volet, cliquez sur **démarrer le Service**.  
  


