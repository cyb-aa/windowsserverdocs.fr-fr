---
title: Le service de gestion d’ordinateurs virtuels Hyper-V doit être configuré pour démarrer automatiquement
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 222bbe76-c514-4a3f-b61b-860a4dc2826a
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 26122d40b3fbdbdc40a94801d5e3ff8fcf4fa646
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859312"
---
# <a name="the-hyper-v-virtual-machine-management-service-should-be-configured-to-start-automatically"></a>Le service de gestion d’ordinateurs virtuels Hyper-V doit être configuré pour démarrer automatiquement

>S’applique à Windows Server 2016

Pour plus d'informations sur les meilleures pratiques et les analyses, consultez [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  

Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.

## <a name="issue"></a>Problème  
  
*Le service de gestion d’ordinateurs virtuels Hyper-V n’est pas configuré pour démarrer automatiquement.*  
  
## <a name="impact"></a>Impact  
  
*Les machines virtuelles ne peuvent pas être gérées tant que le service n’est pas démarré.*  
  
Les ordinateurs virtuels en cours d’exécution continuent à s’exécuter. Toutefois, vous ne pourrez pas gérer les machines virtuelles, ni les créer ou les supprimer tant que le service n’est pas en cours d’exécution.  
  
## <a name="resolution"></a>Résolution  
  
*Utilisez le composant logiciel enfichable Services ou l’outil de ligne de commande sc config pour reconfigurer le service pour qu’il démarre automatiquement.*  
  
> [!TIP]  
> Si vous ne trouvez pas le service dans l’application de bureau ou si l’outil en ligne de commande signale que le service n’existe pas, les outils de gestion Hyper-V ne sont probablement pas installés. Pour les installer :  
>   
> - Sur Windows Server, ouvrez Gestionnaire de serveur et utilisez l’Assistant Ajout de rôles et de fonctionnalités. Pour plus d’informations, consultez [installer le rôle Hyper-V sur Windows Server 2016](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).  
> - Sur Windows, à partir du bureau, commencez à taper **programmes**, cliquez sur **programmes et fonctionnalités** (panneau de configuration) > **activer ou désactiver des fonctionnalités Windows** > **les** outils de gestion Hyper-v > **hyper-** v. Cliquez ensuite sur **OK**.  
  
#### <a name="to-reconfigure-the-service-to-start-automatically-using-the-services-desktop-app"></a>Pour reconfigurer le service de façon à ce qu’il démarre automatiquement à l’aide de l’application de bureau services  
  
1.  Ouvrez l’application de bureau Services. (Cliquez sur **Démarrer**, cliquez dans la zone de recherche, commencez à taper **services**, puis cliquez sur services dans la liste des résultats.  
  
2.  Dans le volet d’informations, cliquez avec le bouton droit sur **gestion des machines virtuelles Hyper-V**, puis cliquez sur **Propriétés**.  
  
3.  Sous l’onglet **général** , dans type de **démarrage** , cliquez sur **automatique**.  
  
#### <a name="to-reconfigure-the-service-to-start-automatically-using-the-sc-config-command"></a>Pour reconfigurer le service pour qu’il démarre automatiquement à l’aide de la commande SC config  
  
1.  Ouvrez Windows PowerShell.  
  
2.  Type :  
  
    ```  
    set-service  vmms -startuptype automatic  
    ```  
  
3.  Si le service n’est pas déjà en cours d’exécution, tapez :  
  
    ```  
    start-service -name vmms  
    ```  
  


