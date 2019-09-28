---
title: Les serveurs de réplication doivent être configurés pour identifier les serveurs principaux autorisés à envoyer le trafic de réplication
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 0aeb1f4b-2e75-430b-9557-fe64738c4992
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 567b20d00d2f245ae7e9577d9d200dca116a9b4d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364738"
---
# <a name="replica-servers-should-be-configured-to-identify-specific-primary-servers-authorized-to-send-replication-traffic"></a>Les serveurs de réplication doivent être configurés pour identifier les serveurs principaux autorisés à envoyer le trafic de réplication

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Comme configuré, ce serveur réplica accepte le trafic de réplication de tous les serveurs principaux et les stocke dans un emplacement unique.*  
  
### <a name="impact"></a>Impact  
*Toutes les réplications à partir de tous les serveurs principaux sont stockées à un seul emplacement, ce qui peut entraîner des problèmes de confidentialité ou de sécurité.*  
  
## <a name="resolution"></a>Résolution :  
le Gestionnaire Hyper-V @no__t 0Use pour créer de nouvelles entrées d’autorisation pour les serveurs principaux spécifiques et pour spécifier des emplacements de stockage distincts pour chacun d’entre eux. Vous pouvez utiliser des caractères génériques pour regrouper les serveurs principaux en jeux pour chaque entrée d’autorisation. *  
  
#### <a name="create-authorization-entries-using-hyper-v-manager"></a>Créer des entrées d’autorisation à l’aide du Gestionnaire Hyper-V  
  
1.  Ouvrez le Gestionnaire Hyper-V. (À partir de Gestionnaire de serveur, cliquez sur **outils** > **Gestionnaire Hyper-V**.)  
  
2.  Dans la liste des ordinateurs hôtes, cliquez avec le bouton droit sur celui que vous souhaitez, puis cliquez sur **paramètres Hyper-V**.  
  
3.  Dans le volet de navigation, cliquez sur **configuration de la réplication**.  
  
4.  Sous **autorisation et stockage**, cliquez sur **autoriser la réplication à partir des serveurs spécifiés**.  
  
5.  Sous la liste des serveurs, cliquez sur **Ajouter**.  
  
6.  Sous **Ajouter une entrée d’autorisation**:  
  
    -   Tapez le nom complet du premier serveur.  
  
    -   Spécifiez un emplacement dédié pour stocker uniquement les fichiers de ce serveur.  
  
7.  Cliquez sur **OK**.  
  
8.  Répétez cette opération pour chaque serveur principal.  
  
9. Cliquez à nouveau sur **OK** pour terminer et fermer la fenêtre.  
  
### <a name="create-authorization-entries-using-windows-powershell"></a>Créer des entrées d’autorisation à l’aide de Windows PowerShell  
  
1.  Ouvrez Windows PowerShell. (À partir du bureau, cliquez sur Démarrer et commencez à taper **Windows PowerShell**.)  
  
2.  Cliquez avec le bouton droit sur **Windows PowerShell** , puis cliquez sur **exécuter en tant qu’administrateur**.  
  
3.  Exécutez une commande semblable à la suivante, en remplaçant :  
  
    -   Nom du serveur principal de server01.domain01.contoso.com avec le nom de domaine complet de votre serveur.  
  
    -   Emplacement de D:\ReplicaVMStorage avec votre emplacement.  
  
    -   Le groupe de confiance nommé par défaut avec le nom de votre groupe, si vous en avez créé un. Dans le cas contraire, utilisez la valeur par défaut.  
  
```  
New-VMReplicationAuthorizationEntry server01.domain01.contoso.com D:\ReplicaVMStorage DEFAULT  
```  
  
## <a name="see-also"></a>Voir aussi  
[New-VMReplicationAuthorizationEntry](https://technet.microsoft.com/library/hh848606.aspx)  
  


