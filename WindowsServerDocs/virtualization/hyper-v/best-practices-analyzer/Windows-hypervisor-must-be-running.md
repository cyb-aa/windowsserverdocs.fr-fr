---
title: Hyperviseur de Windows doit être en cours d’exécution
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 501a9beb-c464-46c0-88c5-e3e7e3e70101
author: KBDAzure
ms.date: 10/03/2016
ms.openlocfilehash: f34e10918c60fb602c3a88ef3434cda619b11d8d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884140"
---
# <a name="windows-hypervisor-must-be-running"></a>Hyperviseur de Windows doit être en cours d’exécution

>S'applique à : Windows Server 2016
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Prérequis|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Hyperviseur de Windows n’est pas en cours d’exécution.*  
  
## <a name="impact"></a>Impact  
  
*Impossible de démarrer les ordinateurs virtuels jusqu'à ce que l’hyperviseur de Windows est en cours d’exécution.*  
  
## <a name="resolution"></a>Résolution  
  
*Vérifiez le catalogue Windows Server pour voir si ce serveur est qualifié pour exécuter Hyper-V. Ensuite, vérifiez que le BIOS est activé pour la virtualisation assistée par matériel et de prévention de l’exécution matérielle des données. Ensuite, vérifiez le journal des événements hyperviseur Hyper-V.*  
  
Pour vérifier le catalogue, consultez [catalogue Windows Server](https://go.microsoft.com/fwlink/?LinkId=111228) (https://go.microsoft.com/fwlink/?LinkId=111228).  
  
> [!CAUTION]  
> Modification de certains paramètres dans le BIOS d’un ordinateur peut provoquer cet ordinateur arrêter le chargement du système d’exploitation, ou elle peut rendre les périphériques matériels, tels que les lecteurs de disque dur, non disponible. Toujours consulter le manuel de l’utilisateur pour l’ordinateur déterminer la meilleure façon de configurer le BIOS du système. En outre, il est toujours judicieux de suivre les paramètres que vous modifiez et la valeur de leur origine afin que vous pouvez les restaurer ultérieurement si nécessaire. Si vous rencontrez des problèmes après avoir modifié les paramètres dans le BIOS système, essayez de charger les paramètres par défaut (une option est généralement disponible dans l’utilitaire de configuration du BIOS) ou contactez le fabricant pour obtenir une assistance.  
  
#### <a name="to-verify-virtualization-support-in-the-bios-or-uefi"></a>Pour vérifier la prise en charge de la virtualisation dans le BIOS ou UEFI  
  
1.  Redémarrez l’ordinateur et y accèdent le BIOS ou UEFI via l’outil de configuration. Accès à cet outil est généralement disponible lorsque l’ordinateur passe par un processus de démarrage. Immédiatement après avoir activé la plupart des ordinateurs, un message s’affiche pendant quelques secondes qui répertorie la clé ou une combinaison de touches à appuyer sur pour ouvrir l’outil de configuration.  
  
2.  Rechercher les paramètres pour la virtualisation et la prévention de l’exécution de données-appliquée par le matériel (PED) et vérifiez qu’ils sont sur. Voici les emplacements de menu courants pour ces paramètres dans l’outil de configuration et des exemples de qu’elles peuvent être nommées :  
  
    -   Prise en charge de la virtualisation :  
  
        -   Généralement disponible sous les paramètres pour le processeur principal ou les performances. Il est parfois sous les paramètres de sécurité.  
  
        -   Rechercher des noms de paramètre qui incluent « virtualisation » ou « technologie de virtualisation ».  
  
    -   DEP matérielle :  
  
        -   Généralement disponible sous les paramètres de sécurité ou de la mémoire.  
  
        -   Rechercher des noms de paramètre qui incluent « exécution », « exécuter » ou « prévention ».  
  
3.  Si nécessaire, activez les paramètres en suivant les instructions fournies dans l’outil de configuration. Enregistrer les modifications et quitter.  
  
4.  Si vous avez apporté des modifications, mettre hors tension, puis sur Terminer.  
  
    > [!IMPORTANT]  
    > Nous recommandons que vous mettez hors tension puis de le rallumer (parfois appelé un cycle d’alimentation), car les modifications ne sont pas appliquées sur certains ordinateurs jusqu'à ce que cela se produit.  
  
Ensuite, vérifiez le journal des événements hyperviseur Hyper-V. S’il existe des problèmes, vous devez également vérifier le journal système.  
  
#### <a name="to-check-the-event-logs"></a>Pour vérifier les journaux des événements  
  
1.  Ouvrez l’observateur d’événements. Cliquez sur **Démarrer**, cliquez sur **outils d’administration**, puis cliquez sur **Observateur d’événements**.  
  
2.  Ouvrez le journal des événements hyperviseur Hyper-V. Dans le volet de navigation, développez **journaux des Applications et Services** >> **Microsoft** >> **Windows**  >>   **Hyperviseur Hyper-V**, puis cliquez sur **opérationnel**.  
  
3.  Si l’hyperviseur de Windows est en cours d’exécution, aucune action supplémentaire n’est nécessaire. Si l’hyperviseur de Windows n’est pas en cours d’exécution, cela :  
  
4.  Ouvrez le journal système. (Dans le volet de navigation, développez **les journaux Windows** , puis sélectionnez **système**.)  
  
5.  Utilisez un filtre pour rechercher des événements de l’hyperviseur Hyper-V :   
    1. Dans le **Actions** volet, cliquez sur **filtrer le journal actuel**. Pour **sources d’événements**, spécifiez « Hyperviseur Hyper-V ».   
    2. Recherchez les événements qui signalent des problèmes. Par exemple, ID d’événement 41 indique un problème avec la configuration du BIOS : « Échec du lancement de Hyper-V ; Soit VMX absent ou n’est pas activée dans le BIOS. »  
  
### <a name="see-also"></a>Voir aussi  
Pour plus d’informations sur l’utilisation de Hyper-V sur Windows 10, notamment comment vérifier que votre ordinateur peut exécuter Hyper-V, consultez [requise pour Windows 10 Hyper-V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_compatibility). 


