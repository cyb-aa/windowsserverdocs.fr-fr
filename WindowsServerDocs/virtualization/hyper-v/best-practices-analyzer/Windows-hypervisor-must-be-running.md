---
title: L’hyperviseur Windows doit être en cours d’exécution
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 501a9beb-c464-46c0-88c5-e3e7e3e70101
author: KBDAzure
ms.date: 10/03/2016
ms.openlocfilehash: 51f863425bd1107894fb5e4d44ed7c742a806394
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393045"
---
# <a name="windows-hypervisor-must-be-running"></a>L’hyperviseur Windows doit être en cours d’exécution

>S'applique à : Windows Server 2016
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Warning|  
|**Catégorie**|Prérequis|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*L’hyperviseur Windows n’est pas en cours d’exécution.*  
  
## <a name="impact"></a>Impact  
  
*Impossible de démarrer les machines virtuelles tant que l’hyperviseur Windows n’est pas en cours d’exécution.*  
  
## <a name="resolution"></a>Résolution :  
  
*Check le catalogue Windows Server pour voir si ce serveur est qualifié pour exécuter Hyper-V. Ensuite, assurez-vous que le BIOS est activé pour la virtualisation d’assistance matérielle et la prévention de l’exécution des données appliquée par le matériel. Vérifiez ensuite le journal des événements hyperviseur Hyper-V.*  
  
Pour vérifier le catalogue, consultez le [Catalogue Windows Server](https://go.microsoft.com/fwlink/?LinkId=111228) (https://go.microsoft.com/fwlink/?LinkId=111228).  
  
> [!CAUTION]  
> La modification de certains paramètres dans le BIOS système d’un ordinateur peut entraîner l’arrêt du chargement du système d’exploitation par cet ordinateur, ou la création de périphériques matériels, tels que des lecteurs de disque dur, non disponibles. Consultez toujours le manuel de l’utilisateur de l’ordinateur pour déterminer la méthode appropriée pour configurer le BIOS du système. En outre, il est toujours judicieux de suivre les paramètres que vous modifiez et leur valeur d’origine afin de pouvoir les restaurer ultérieurement si nécessaire. Si vous rencontrez des problèmes après avoir modifié les paramètres dans le BIOS du système, essayez de charger les paramètres par défaut (une option est généralement disponible dans l’utilitaire de configuration du BIOS) ou contactez le fabricant de l’ordinateur pour obtenir de l’aide.  
  
#### <a name="to-verify-virtualization-support-in-the-bios-or-uefi"></a>Pour vérifier la prise en charge de la virtualisation dans le BIOS ou UEFI  
  
1.  Redémarrez l’ordinateur et accédez au BIOS ou à UEFI via l’outil de configuration de. L’accès à cet outil est généralement disponible lorsque l’ordinateur passe par un processus de démarrage. Immédiatement après avoir activé la plupart des ordinateurs, un message s’affiche pendant quelques secondes, qui répertorie la touche ou la combinaison de touches à appuyer pour ouvrir l’outil de configuration de.  
  
2.  Recherchez les paramètres pour la virtualisation et la prévention de l’exécution des données (DEP) appliquée par le matériel et vérifiez qu’ils sont activés. Voici les emplacements de menu courants de ces paramètres dans l’outil de configuration de, ainsi que des exemples de leur nom :  
  
    -   Prise en charge de la virtualisation :  
  
        -   Généralement disponible sous les paramètres du processeur principal ou des performances. Il se trouve parfois sous les paramètres de sécurité.  
  
        -   Recherchez les noms de paramètres qui incluent « virtualisation » ou « technologie de virtualisation ».  
  
    -   DEP appliquée par le matériel :  
  
        -   Généralement disponible sous les paramètres de sécurité ou de mémoire.  
  
        -   Recherchez les noms de paramètres qui incluent « exécution », « exécuter » ou « prévention ».  
  
3.  Si nécessaire, activez les paramètres en suivant les instructions de l’outil de configuration de. Enregistrez les modifications et quittez.  
  
4.  Si vous avez apporté des modifications, mettez l’éteindre, puis de nouveau sous tension pour terminer.  
  
    > [!IMPORTANT]  
    > Nous vous recommandons de mettre l’ordinateur hors tension puis de le rallumer (parfois appelé « cycle d’alimentation »), car les modifications ne sont pas appliquées sur certains ordinateurs jusqu’à ce que cela se produise.  
  
Ensuite, consultez le journal des événements hyperviseur Hyper-V. En cas de problème, vous allez également vérifier le journal système.  
  
#### <a name="to-check-the-event-logs"></a>Pour vérifier les journaux des événements  
  
1.  Ouvrez l’observateur d’événements. Cliquez sur **Démarrer**, sur **Outils d’administration**, puis sur **Observateur d’événements**.  
  
2.  Ouvrez le journal des événements hyperviseur Hyper-V. Dans le volet de navigation, développez **journaux des applications et des Services** >> **Microsoft** >> **Windows** >> **Hyper-V-hyperviseur**, puis cliquez sur **opérationnel**.  
  
3.  Si l’hyperviseur Windows est en cours d’exécution, aucune action supplémentaire n’est nécessaire. Si l’hyperviseur Windows n’est pas en cours d’exécution, procédez comme suit :  
  
4.  Ouvrez le journal système. (Dans le volet de navigation, développez **journaux Windows** , puis sélectionnez **système**.)  
  
5.  Utilisez un filtre pour rechercher les événements hyperviseur Hyper-V :   
    1. Dans le volet **actions** , cliquez sur **filtrer le journal actuel**. Pour les **sources d’événements**, spécifiez « Hyper-V-hyperviseur ».   
    2. Recherchez les événements qui signalent des problèmes. Par exemple, l’ID d’événement 41 indique un problème avec la configuration du BIOS : «Échec du lancement d’Hyper-V ; VMX n’est pas présent ou n’est pas activé dans le BIOS.»  
  
### <a name="see-also"></a>Voir aussi  
Pour plus d’informations sur l’utilisation d’Hyper-V sur Windows 10, notamment sur la façon de vérifier que votre ordinateur peut exécuter Hyper-V, voir [Configuration système requise pour Hyper](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_compatibility)-v dans Windows 10. 


