---
title: Le service de gestion d’ordinateurs virtuels Hyper-V doit être en cours d’exécution
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: f44d6887-6458-4438-9d93-574587e3f7d1
author: kbdazure
ms.date: 10/03/2016
ms.openlocfilehash: 50f101f9dad824e13fa5827175cc1c944a96a91b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859322"
---
# <a name="the-hyper-v-virtual-machine-management-service-must-be-running"></a>Le service de gestion d’ordinateurs virtuels Hyper-V doit être en cours d’exécution

>S’applique à Windows Server 2016
  
Pour plus d'informations sur les meilleures pratiques et les analyses, consultez [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Error|  
|**Catégorie**|Composants requis|  

Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.

## <a name="issue"></a>Problème  
  
*Le service requis pour gérer les ordinateurs virtuels n’est pas en cours d’exécution.*  
  
## <a name="impact"></a>Impact  
  
*Aucune opération de gestion d’ordinateur virtuel ne peut être effectuée.*  
  
Les ordinateurs virtuels en cours d’exécution continuent à s’exécuter. Toutefois, vous ne pourrez pas gérer les machines virtuelles, ni les créer ou les supprimer tant que le service n’est pas en cours d’exécution.  
  
## <a name="resolution"></a>Résolution  
  
*Utilisez le composant logiciel enfichable Services ou l’outil de ligne de commande sc config pour reconfigurer le service pour qu’il démarre automatiquement.*  
  
> [!TIP]  
> Si vous ne trouvez pas le service dans l’application de bureau ou si l’outil en ligne de commande signale que le service n’existe pas, les outils de gestion Hyper-V ne sont probablement pas installés. Et si vous n’êtes pas en mesure de voir la console MMC Hyper-V dans le menu Démarrer, vous devez installer les outils de gestion Hyper-V.

Pour installer les outils de gestion Hyper-V :  
>   
> - Sur Windows Server, ouvrez Gestionnaire de serveur et utilisez l’Assistant Ajout de rôles et de fonctionnalités. Pour plus d’informations, consultez [installer le rôle Hyper-V sur Windows Server 2016](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).  Vous pouvez également utiliser PowerShell pour installer les outils (`Install-WindowsFeature -Name Hyper-V-Tools, Hyper-V-PowerShell`) 
> - Sur Windows, à partir du bureau, commencez à taper **programmes**, cliquez sur **programmes et fonctionnalités** (panneau de configuration) > **activer ou désactiver des fonctionnalités Windows** > **les** outils de gestion Hyper-v > **hyper-** v. Cliquez ensuite sur **OK**.  
  
### <a name="to-reconfigure-the-service-to-start-automatically-using-the-services-desktop-app"></a>Pour reconfigurer le service de façon à ce qu’il démarre automatiquement à l’aide de l’application de bureau services  
  
1.  Ouvrez l’application de bureau Services. (Cliquez sur **Démarrer**, cliquez dans la zone **Rechercher** , tapez **services. msc**, puis appuyez sur entrée.)  
  
2.  Dans le volet d’informations, cliquez avec le bouton droit sur **gestion des machines virtuelles Hyper-V**, puis cliquez sur **Propriétés**.  
  
3.  Sous l’onglet **général** , dans type de **démarrage** , cliquez sur **automatique**.  
  
4.  Pour démarrer le service, cliquez sur **Démarrer**.  
  
### <a name="to-reconfigure-the-service-to-start-automatically-using-sc-config"></a>Pour reconfigurer le service pour qu’il démarre automatiquement à l’aide de SC config  
  
1.  Ouvrez Windows PowerShell. (À partir du bureau, cliquez sur **Démarrer** et commencez à taper **Windows PowerShell**.)  
  
2.  Cliquez avec le bouton droit sur **Windows PowerShell** , puis cliquez sur **exécuter en tant qu’administrateur**.  
  
3.  Pour reconfigurer le service, tapez :  
  
    ```  
    sc config vmms start=auto  
    ```  
  
4.  Pour démarrer le service, tapez :  
  
    ```  
    sc start vmms  
    ```  
  
Si le service est déjà configuré pour démarrer automatiquement et que vous devez simplement redémarrer le service, vous pouvez le faire à partir du Gestionnaire Hyper-V ou de la commande sc start VMMS présentée ci-dessus.  
  
#### <a name="to-restart-the-service-from-hyper-v-manager"></a>Pour redémarrer le service à partir du Gestionnaire Hyper-V  
  
1.  Ouvrez le Gestionnaire Hyper-V. Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestionnaire Hyper-V**.  
  
2.  Dans le volet de navigation, cliquez sur le nom du serveur s’il n’est pas déjà sélectionné.  
  
3.  Dans le volet **actions** , cliquez sur **Démarrer le service**.  
  


