---
title: Activer vRSS sur une carte réseau virtuelle
description: Dans cette rubrique, vous allez apprendre à activer vRSS dans Windows Server en utilisant le Gestionnaire de périphériques ou Windows PowerShell.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cb48315c-0204-4927-aa24-64f6789c2e20
manager: dougkim
ms.localizationpriority: medium
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 19e8011fb98b84c20e8237792664551d2362d589
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133415"
---
# Activer vRSS sur une carte réseau virtuelle

>S’applique à: Windows Server (canal semi-annuel), WindowsServer2016

Virtuel \(vRSS\) RSS nécessite la file d’attente de la Machine virtuelle \(VMQ\) prennent en charge à partir de la carte physique. Si VMQ est désactivée ou n’est pas pris en charge côté virtuelle réception d’échelle est désactivée. 

Pour plus d’informations, voir [planifier l’utilisation de vRSS](vrss-plan.md).

## Activer vRSS sur un ordinateur virtuel
 
Utilisez les procédures suivantes pour activer vRSS à l’aide de Windows PowerShell ou le Gestionnaire de périphériques.

-   Gestionnaire de périphériques
-   Windows PowerShell
  
### Gestionnaire de périphériques

Vous pouvez utiliser cette procédure pour activer vRSS à l’aide du Gestionnaire de périphériques.

>[!NOTE]
>La première étape de cette procédure est spécifique aux ordinateurs virtuels qui exécutent Windows 10 ou Windows Server 2016. Si votre ordinateur virtuel exécute un autre système d’exploitation, vous pouvez ouvrir le Gestionnaire de périphériques première ouverture du Panneau de configuration, puis localisez et ouvrez le Gestionnaire de périphériques.
  
1.  Sur la barre des tâches de l’ordinateur virtuel, dans le **Type ici pour effectuer une recherche**, tapez **appareil**. 

2.  Dans les résultats de recherche, cliquez sur **Le Gestionnaire de périphériques**.

3.  Dans le Gestionnaire de périphériques, cliquez pour développer des **cartes réseau**. 

4.  Avec le bouton droit de la carte réseau que vous souhaitez configurer, puis cliquez sur **Propriétés**.<p>La boîte de dialogue de **Propriétés** de carte réseau s’ouvre.

5.  Dans la carte réseau de **Propriétés**, cliquez sur l’onglet **Options avancées** . 

6.  Dans la **propriété**, faites défiler vers le bas, puis cliquez sur **mise à l’échelle côté réception**. 

7.  Assurez-vous que la sélection de la **valeur** est **activé**. 

8.  Cliquez sur **OK**.
  
> [!NOTE]
> Sous l’onglet **Avancé** , certaines cartes réseau affichent également le nombre de files d’attente RSS qui sont pris en charge par la carte.

---

### Windows PowerShell

Utilisez la procédure suivante pour activer vRSS à l’aide de Windows PowerShell.

1. Sur la machine virtuelle, ouvrez **Windows PowerShell**.

2. Tapez la commande suivante, en vous assurant que vous remplacez la valeur *AdapterName* pour la **-Name** paramètre portant le nom de la carte réseau que vous souhaitez configurer et appuyez sur ENTRÉE. 
  
   ```PowerShell
   Enable-NetAdapterRSS -Name "AdapterName"
   ```

   >[!TIP]
   >Sinon, vous pouvez utiliser la commande suivante pour activer vRSS.
   >```PowerShell
   >Set-NetAdapterRSS -Name "AdapterName" -Enabled $True  
   >```

Pour plus d’informations, voir [Les commandes Windows PowerShell pour RSS et vRSS](vrss-wps.md).

---