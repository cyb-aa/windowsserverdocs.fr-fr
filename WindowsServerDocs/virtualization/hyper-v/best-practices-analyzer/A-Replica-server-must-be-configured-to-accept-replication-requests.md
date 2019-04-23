---
title: Un serveur de réplica doit être configuré pour accepter les demandes de réplication
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c5e30ddc50b176b83db081a29c6427356ab946c8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827520"
---
# <a name="a-replica-server-must-be-configured-to-accept-replication-requests"></a>Un serveur de réplica doit être configuré pour accepter les demandes de réplication

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Erreur|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.
  
## <a name="issue"></a>Problème  
*Cet ordinateur est désigné comme serveur réplica Hyper-V, mais n’est pas configuré pour accepter les données de réplication entrant à partir de serveurs principaux.*  
  
## <a name="impact"></a>Impact  
*Ce serveur ne peut pas accepter le trafic de réplication à partir de serveurs principaux.*  
  
## <a name="resolution"></a>Résolution  
*Utilisez le Gestionnaire Hyper-V pour spécifier les serveurs principaux qui ce serveur de réplication doit accepter des données de réplication à partir de.*  
  
#### <a name="create-authorization-entries-using-hyper-v-manager"></a>Créer des entrées d’autorisation à l’aide du Gestionnaire Hyper-V  
  
1.  Ouvrez le Gestionnaire Hyper-V. (Dans le Gestionnaire de serveur, cliquez sur **outils** > **Gestionnaire Hyper-V**.)  
  
2.  Dans la liste des ordinateurs hôtes, cliquez sur celui de votre choix, puis cliquez sur **paramètres Hyper-V**.  
  
3.  Dans le volet de navigation, cliquez sur **Configuration de la réplication**.  
  
4.  Sous **autorisation et stockage**, cliquez sur **autoriser la réplication des serveurs spécifiés**.  
  
5.  Sous la liste des serveurs, cliquez sur **ajouter**.  
  
6.  Sous **ajouter l’entrée d’autorisation**:  
  
    -   Tapez le nom qualifié complet du premier serveur.  
  
    -   Spécifiez un emplacement dédié pour stocker uniquement les fichiers de ce serveur.  
  
7.  Cliquez sur **OK**.  
  
8.  Répétez pour chaque serveur principal.  
  
9. Cliquez sur **OK** à nouveau pour terminer et fermer la fenêtre.  
  
### <a name="create-authorization-entries-using-windows-powershell"></a>Créer des entrées d’autorisation à l’aide de Windows PowerShell  
  
1.  Ouvrez Windows PowerShell. (À partir du bureau, cliquez sur Démarrer et commencez à taper **Windows PowerShell**.)  
  
2.  Avec le bouton droit **Windows PowerShell** et cliquez sur **exécuter en tant qu’administrateur**.  
  
3.  Exécutez une commande semblable à ce qui suit, en remplaçant :  
  
    -   Nom du serveur principal de server01.domain01.contoso.com avec le nom de domaine complet de votre serveur.  
  
    -   L’emplacement du D:\ReplicaVMStorage avec votre emplacement.  
  
    -   Le groupe de confiance nommé par défaut avec le nom de votre groupe, si vous avez créé un. Si ce n’est pas le cas, utilisez la valeur par défaut.  
  
```  
New-VMReplicationAuthorizationEntry server01.domain01.contoso.com D:\ReplicaVMStorage DEFAULT  
```  
  


