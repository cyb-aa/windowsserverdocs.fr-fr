---
title: Activer vRSS sur une carte réseau virtuelle
description: Dans cette rubrique, vous allez apprendre à activer vRSS dans Windows Server à l’aide de Device Manager ou de Windows PowerShell.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: cb48315c-0204-4927-aa24-64f6789c2e20
manager: dougkim
ms.localizationpriority: medium
ms.date: 09/05/2018
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e4be9060da4a738e3ad8e4976d037f3a05467da3
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315373"
---
# <a name="enable-vrss-on-a-virtual-network-adapter"></a>Activer vRSS sur une carte réseau virtuelle

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Virtual RSS \(vRSS\) nécessite la prise en charge de l’File d’attente d’ordinateurs virtuels \(d’un\) d’ordinateurs virtuels à partir de la carte physique. Si l’extension d’ordinateurs virtuels est désactivée ou n’est pas prise en charge, la mise à l’échelle côté réception virtuelle est désactivée. 

Pour plus d’informations, consultez [planifier l’utilisation de vRSS](vrss-plan.md).

## <a name="enable-vrss-on-a-vm"></a>Activer vRSS sur une machine virtuelle
 
Utilisez les procédures suivantes pour activer vRSS à l’aide de Windows PowerShell ou Device Manager.

-   Gestionnaire de périphérique
-   Windows PowerShell
  
### <a name="device-manager"></a>Gestionnaire de périphérique

Vous pouvez utiliser cette procédure pour activer vRSS à l’aide de Device Manager.

>[!NOTE]
>La première étape de cette procédure est spécifique aux machines virtuelles qui exécutent Windows 10 ou Windows Server 2016. Si votre machine virtuelle exécute un système d’exploitation différent, vous pouvez ouvrir Device Manager en ouvrant le panneau de configuration, puis en recherchant et en ouvrant Device Manager.
  
1.  Dans la barre des tâches de la machine virtuelle, dans **type here to Search**, tapez **Device**. 

2.  Dans les résultats de la recherche, cliquez sur **Device Manager**.

3.  Dans Device Manager, cliquez pour développer **cartes réseau**. 

4.  Cliquez avec le bouton droit sur la carte réseau que vous souhaitez configurer, puis cliquez sur **Propriétés**.<p>La boîte de dialogue **Propriétés** de la carte réseau s’ouvre.

5.  Dans les **Propriétés**de la carte réseau, cliquez sur l’onglet **avancé** . 

6.  Dans **propriété**, faites défiler la liste jusqu’à et cliquez sur **mise à l’échelle côté réception**. 

7.  Assurez-vous que la sélection dans **valeur** est **activée**. 

8.  Cliquez sur **OK**.
  
> [!NOTE]
> Sous l’onglet **avancé** , certaines cartes réseau affichent également le nombre de files d’attente RSS prises en charge par l’adaptateur.

---

### <a name="windows-powershell"></a>Windows PowerShell

Utilisez la procédure suivante pour activer vRSS à l’aide de Windows PowerShell.

1. Sur la machine virtuelle, ouvrez **Windows PowerShell**.

2. Tapez la commande suivante, en veillant à remplacer la valeur *AdapterName* pour le paramètre **-Name** par le nom de la carte réseau que vous souhaitez configurer, puis appuyez sur entrée. 
  
   ```PowerShell
   Enable-NetAdapterRSS -Name "AdapterName"
   ```

   >[!TIP]
   >Vous pouvez également utiliser la commande suivante pour activer vRSS.
   >```PowerShell
   >Set-NetAdapterRSS -Name "AdapterName" -Enabled $True  
   >```

Pour plus d’informations, consultez [commandes Windows PowerShell pour RSS et vRSS](vrss-wps.md).

---