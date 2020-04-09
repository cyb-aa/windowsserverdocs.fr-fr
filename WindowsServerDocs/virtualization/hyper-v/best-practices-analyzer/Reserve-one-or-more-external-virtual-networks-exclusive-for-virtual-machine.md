---
title: Réserver un ou plusieurs réseaux virtuels externes pour une utilisation exclusive par les machines virtuelles
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: f7732258-93f1-44e8-835b-5ad2d1c45cd9
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: c371743f20f8192b682ff68045c5d72e9e0f7e8d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861802"
---
# <a name="reserve-one-or-more-external-virtual-networks-for-exclusive-use-by-virtual-machines"></a>Réserver un ou plusieurs réseaux virtuels externes pour une utilisation exclusive par les machines virtuelles

>S’applique à Windows Server 2016

Pour plus d'informations sur les meilleures pratiques et les analyses, consultez [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Error|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Tous les réseaux virtuels externes sont configurés pour être utilisés par le système d’exploitation de gestion et les ordinateurs virtuels.*  
  
## <a name="impact"></a>Impact  
  
*Les performances de mise en réseau peuvent être détériorées dans le système d’exploitation de gestion.*  
  
## <a name="resolution"></a>Résolution  
  
*Utilisez le gestionnaire de commutateur virtuel pour arrêter le partage d’un réseau virtuel externe avec le système d’exploitation de gestion.*  
  
#### <a name="to-stop-sharing-the-external-virtual-network-with-the-management-operating-system"></a>Pour cesser de partager le réseau virtuel externe avec le système d’exploitation de gestion  
  
1.  Ouvrez le Gestionnaire Hyper-V. Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestionnaire Hyper-V**.  
  
2.  Dans le menu **Actions**, cliquez sur **Gestionnaire de commutateur virtuel**.  
  
3.  Sous **commutateurs virtuels**, cliquez sur le nom du commutateur virtuel externe.  
  
4.  Dans la zone **type de connexion** , sous le nom de la carte réseau physique, désactivez la case à cocher **autoriser le système d’exploitation de gestion à partager cette carte réseau** .  
  
5.  Cliquez sur **OK**.  
  


