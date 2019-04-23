---
title: Réserver un ou plusieurs réseaux virtuels externes pour une utilisation exclusive par les ordinateurs virtuels
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: f7732258-93f1-44e8-835b-5ad2d1c45cd9
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c8c90a74352bae0b348608db0fc05107e4d09010
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884740"
---
# <a name="reserve-one-or-more-external-virtual-networks-for-exclusive-use-by-virtual-machines"></a>Réserver un ou plusieurs réseaux virtuels externes pour une utilisation exclusive par les ordinateurs virtuels

>S'applique à : Windows Server 2016

Pour plus d'informations sur les meilleures pratiques et les analyses, consultez [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Erreur|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Tous les réseaux virtuels externes sont configurés pour une utilisation par le système d’exploitation de gestion et les ordinateurs virtuels.*  
  
## <a name="impact"></a>Impact  
  
*Performances de mise en réseau peuvent être dégradées dans le système d’exploitation de gestion.*  
  
## <a name="resolution"></a>Résolution  
  
*Gestionnaire de commutateur virtuel permet d’arrêter le partage d’un réseau virtuel externe avec le système d’exploitation de gestion.*  
  
#### <a name="to-stop-sharing-the-external-virtual-network-with-the-management-operating-system"></a>Pour cesser de partager le réseau virtuel externe avec le système d’exploitation de gestion  
  
1.  Ouvrez le Gestionnaire Hyper-V. Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestionnaire Hyper-V**.  
  
2.  Dans le menu **Actions** , cliquez sur **Gestionnaire de commutateur virtuel**.  
  
3.  Sous **commutateurs virtuels**, cliquez sur le nom du commutateur virtuel externe.  
  
4.  Dans le **type de connexion** zone, sous le nom de la carte réseau physique, désactivez le **autoriser le système d’exploitation de gestion à partager cette carte réseau** case à cocher.  
  
5.  Cliquez sur **OK**.  
  


