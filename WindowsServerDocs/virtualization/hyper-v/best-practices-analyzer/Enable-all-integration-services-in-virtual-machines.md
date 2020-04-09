---
title: Activer tous les services d’intégration sur les ordinateurs virtuels
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 16e202ad-3795-40c9-8176-7ca319e56d26
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 2497755185ba1971130b571ce654e0019df18e0a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861972"
---
# <a name="enable-all-integration-services-in-virtual-machines"></a>Activer tous les services d’intégration sur les ordinateurs virtuels

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
  
*Un ou plusieurs services d’intégration sont désactivés ou ne fonctionnent pas sur un ordinateur virtuel.*  
  
## <a name="impact"></a>Impact  
  
*La fonctionnalité de service ou d’intégration peut ne pas fonctionner correctement pour les machines virtuelles suivantes :*  
  
\<liste des noms de machine virtuelle >  
  
## <a name="resolution"></a>Résolution  
  
*Utilisez le composant logiciel enfichable Services ou l’outil de ligne de commande sc config pour vérifier que le service est configuré pour démarrer automatiquement et qu’il n’est pas arrêté.*  
  
#### <a name="to-configure-how-a-service-is-started-using-the-services-snap-in"></a>Pour configurer le mode de démarrage d’un service à l’aide du composant logiciel enfichable Services  
  
1.  Utilisez Services Bureau à distance ou la connexion à un ordinateur virtuel pour vous connecter à la machine virtuelle et ouvrir une session sur le système d’exploitation invité.  
  
2.  Ouvrez Services. (Cliquez sur **Démarrer**, cliquez dans la zone **Rechercher** , tapez **services. msc**, puis appuyez sur entrée.)  
  
3.  Dans le volet d'informations, cliquez avec le bouton droit sur le service à configurer, puis cliquez sur **Propriétés**.  
  
4.  Sous l’onglet **général** , dans type de **démarrage** , cliquez sur **automatique**.  
  
#### <a name="to-configure-how-a-service-is-started-using-sc-config"></a>Pour configurer le démarrage d’un service à l’aide de SC config  
  
1.  Ouvrez Windows PowerShell. (À partir du bureau, cliquez sur **Démarrer** et commencez à taper **Windows PowerShell**.)  
  
2.  Cliquez avec le bouton droit sur **Windows PowerShell** , puis cliquez sur **exécuter en tant qu’administrateur**.  
  
3.  Remplacez < service-name > par le nom du service, puis tapez :  
  
    ```  
    sc config <service-name> start=auto  
    ```  
  


