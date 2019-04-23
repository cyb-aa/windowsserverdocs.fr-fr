---
title: Activer tous les services d’intégration dans les machines virtuelles
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 16e202ad-3795-40c9-8176-7ca319e56d26
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 307e2d407a0defa14a6b57bda95a2f3ab018406d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829430"
---
# <a name="enable-all-integration-services-in-virtual-machines"></a>Activer tous les services d’intégration dans les machines virtuelles

>S'applique à : Windows Server 2016

Pour plus d'informations sur les meilleures pratiques et les analyses, consultez [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Un ou plusieurs services d’intégration sont désactivés ou ne fonctionne ne pas dans une machine virtuelle.*  
  
## <a name="impact"></a>Impact  
  
*La fonctionnalité de service ou l’intégration ne peut-être pas fonctionner correctement pour les ordinateurs virtuels suivants :*  
  
\<liste des noms de machine virtuelle >  
  
## <a name="resolution"></a>Résolution  
  
*Utilisez l’outil Services de composant logiciel enfichable ou sc config de ligne de commande pour vérifier que le service est configuré pour démarrer automatiquement et n’est pas arrêté.*  
  
#### <a name="to-configure-how-a-service-is-started-using-the-services-snap-in"></a>Pour configurer un service est démarré à l’aide du composant logiciel enfichable Services  
  
1.  Utiliser des Services Bureau à distance ou connexion de Machine virtuelle pour se connecter à la machine virtuelle et le journal sur le système d’exploitation invité.  
  
2.  Ouvrez le dossier Services. (Cliquez sur **Démarrer**, cliquez dans le **rechercher** , tapez **services.msc**, puis appuyez sur ENTRÉE.)  
  
3.  Dans le volet d'informations, cliquez avec le bouton droit sur le service à configurer, puis cliquez sur **Propriétés**.  
  
4.  Sur le **général** sous l’onglet **démarrage** tapez, cliquez sur **automatique**.  
  
#### <a name="to-configure-how-a-service-is-started-using-sc-config"></a>Pour configurer un service est démarré à l’aide de SC Config  
  
1.  Ouvrez Windows PowerShell. (À partir du bureau, cliquez sur **Démarrer** et commencez à taper **Windows PowerShell**.)  
  
2.  Avec le bouton droit **Windows PowerShell** et cliquez sur **exécuter en tant qu’administrateur**.  
  
3.  Remplacez < service-name > avec le nom du service, puis tapez :  
  
    ```  
    sc config <service-name> start=auto  
    ```  
  


