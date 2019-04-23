---
title: Activez vRSS sur une carte réseau virtuelle
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882680"
---
# <a name="enable-vrss-on-a-virtual-network-adapter"></a>Activez vRSS sur une carte réseau virtuelle

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

RSS virtuel \(vRSS\) nécessite la file d’attente de la Machine virtuelle \(VMQ\) prennent en charge à partir de la carte physique. Si la file est désactivé ou n’est pas prise en charge mise à l’échelle côté réception virtuelle est désactivée. 

Pour plus d’informations, consultez [planifiez l’utilisation de vRSS](vrss-plan.md).

## <a name="enable-vrss-on-a-vm"></a>Activez vRSS sur une machine virtuelle
 
Utilisez les procédures suivantes pour activer vRSS à l’aide de Windows PowerShell ou le Gestionnaire de périphériques.

-   Gestionnaire de périphériques
-   Windows PowerShell
  
### <a name="device-manager"></a>Gestionnaire de périphériques

Vous pouvez utiliser cette procédure pour activer vRSS à l’aide du Gestionnaire de périphériques.

>[!NOTE]
>La première étape de cette procédure est spécifique aux machines virtuelles qui exécutent Windows 10 ou Windows Server 2016. Si votre machine virtuelle exécute un système d’exploitation différent, vous pouvez ouvrir le Gestionnaire de périphériques première ouvrant le panneau de configuration, puis localisez et ouvrez le Gestionnaire de périphériques.
  
1.  Dans la barre des tâches de la machine virtuelle, dans **tapez ici pour effectuer une recherche**, type **appareil**. 

2.  Dans les résultats de recherche, cliquez sur **Device Manager**.

3.  Dans le Gestionnaire de périphériques, cliquez pour développer **cartes réseau**. 

4.  Avec le bouton droit de la carte réseau que vous souhaitez configurer, puis cliquez sur **propriétés**.<p>La carte réseau **propriétés** boîte de dialogue s’ouvre.

5.  Dans la carte réseau **propriétés**, cliquez sur le **avancé** onglet. 

6.  Dans **propriété**, faites défiler jusqu'à, puis cliquez sur **mise à l’échelle côté réception**. 

7.  Vérifiez que la sélection dans **valeur** est **activé**. 

8.  Cliquez sur **OK**.
  
> [!NOTE]
> Sur le **avancé** onglet, certaines cartes réseau affichent également le nombre de files d’attente RSS qui sont pris en charge par l’adaptateur.

---

### <a name="windows-powershell"></a>Windows PowerShell

Utilisez la procédure suivante pour activer vRSS à l’aide de Windows PowerShell.

1. Sur la machine virtuelle, ouvrez **Windows PowerShell**.

2. Tapez la commande suivante, en garantissant que vous remplacez le *AdapterName* valeur pour le **-nom** paramètre portant le nom de la carte réseau que vous souhaitez configurer, puis appuyez sur ENTRÉE. 
  
   ```PowerShell
   Enable-NetAdapterRSS -Name "AdapterName"
   ```

   >[!TIP]
   >Alternativement, vous pouvez utiliser la commande suivante pour activer vRSS.
   >```PowerShell
   >Set-NetAdapterRSS -Name "AdapterName" -Enabled $True  
   >```

Pour plus d’informations, consultez [commandes PowerShell de Windows pour les flux RSS et vRSS](vrss-wps.md).

---