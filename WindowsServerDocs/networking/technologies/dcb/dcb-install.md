---
title: Installer Data Center Bridging (DCB) dans Windows Server ou Client
description: Cette rubrique fournit des instructions sur l’installation de Data Center Bridging dans Windows Server ou Client Windows.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: b89213d8-143a-45f3-a609-bc6a7027204c
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 491bdeb1a7458be1f991be68724e7a7b51f67ecf
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="install-data-center-bridging-dcb-in-windows-server-2016-or-windows-10"></a>Installer Data Center Bridging \(DCB\) dans Windows Server2016 ou Windows10

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour savoir comment installer DCB dans Windows Server2016 ou Windows10.

## <a name="prerequisites-for-using-dcb"></a>Configuration requise pour l’utilisation de DCB

Voici les conditions préalables pour la configuration et la gestion DCB.

### <a name="install-a-compatible-operating-system"></a>Installer un système d’exploitation compatible

Vous pouvez utiliser les commandes DCB à partir de ce guide dans les systèmes d’exploitation suivants.

- Windows Server (canal annuel un point-virgule)
- Windows Server2016
- Windows10 \(all versions\)

Les systèmes d’exploitation suivants comprennent les versions précédentes de DCB qui ne sont pas compatibles avec les commandes qui sont utilisés dans la documentation de DCB pour Windows Server2016 et Windows10.

- Windows Server2012R2
- Windows Server2012

###  <a name="hardware-requirements"></a>Configuration matérielle requise

Voici une liste de la configuration matérielle requise pour DCB.

- Adapter\(s\) de réseau Ethernet compatibles DCB\ doit être installé sur les ordinateurs offrant DCB de Windows Server2016.
- Commutateurs matériels compatibles DCB\ doivent être déployés sur votre réseau.


## <a name="install-dcb-in-windows-server-2016"></a>Installer DCB dans Windows Server2016

Vous pouvez utiliser les sections suivantes pour installer DCB sur un ordinateur exécutant Windows Server2016.

**Informations d’identification d’administration**

Pour effectuer ces procédures, vous devez être membre du **administrateurs**.

### <a name="install-dcb-using-windows-powershell"></a>Installer DCB à l’aide de Windows PowerShell

Vous pouvez utiliser la procédure suivante pour installer DCB à l’aide de Windows PowerShell.

1. Sur un ordinateur exécutant Windows Server2016, cliquez sur **Démarrer**, puis cliquez sur l’icône Windows PowerShell. Un menu s’affiche. Dans le menu, cliquez sur **plus**, puis cliquez sur **exécuter en tant qu’administrateur**. Si vous y êtes invité, tapez les informations d’identification d’un compte qui possède des privilèges d’administrateur sur l’ordinateur. Windows PowerShell s’ouvre avec des privilèges d’administrateur.
2. Tapez la commande suivante et appuyez sur ENTRÉE.

````
    Install-WindowsFeature -Name Data-Center-Bridging -IncludeManagementTools
````

### <a name="install-dcb-using-server-manager"></a>Installer DCB à l’aide du Gestionnaire de serveur

Vous pouvez utiliser la procédure suivante pour installer DCB à l’aide du Gestionnaire de serveur.

>[!NOTE]
>Après avoir effectué la première étape de cette procédure, le **avant de commencer** page de l’ajout de rôles et fonctionnalités Assistant ne s’affiche pas si vous avez sélectionné précédemment **ignorer cette page par défaut** lorsque l’ajout de rôles et fonctionnalités Assistant a été exécuté. Si le **avant de commencer** page n’apparaît pas, passez à l’étape1 à l’étape3.

1. Sur DC1, dans le Gestionnaire de serveur, cliquez sur **gérer**, puis cliquez sur **Ajout de rôles et fonctionnalités**. L’Assistant Ajouter des rôles et fonctionnalités s’ouvre.
2. Dans **avant de commencer**, cliquez sur **suivant**.
3. Dans **sélectionner le Type d’Installation**, assurez-vous que **installation basée sur un rôle ou une fonctionnalité** est sélectionné, puis cliquez sur **suivant**.
4. Dans **serveur de destination sélectionnez**, assurez-vous que **sélectionner un serveur du pool de serveurs** est sélectionné. Dans **Pool de serveurs**, vérifiez que l’ordinateur local est sélectionné. Cliquez sur **suivant**.
5. Dans **sélectionner des rôles de serveur**, cliquez sur **suivant**.
6. Dans **sélectionner des fonctionnalités**, dans **fonctionnalités**, cliquez sur **Data Center Bridging**. Une boîte de dialogue s’ouvre pour vous demander si vous souhaitez ajouter des fonctionnalités DCB requis. Cliquez sur **ajouter des fonctionnalités**.
7. Dans **sélectionner des fonctionnalités**, cliquez sur **suivant**. 
8. 7. En **confirmer les sélections d’installation**, cliquez sur **installer**. Le **progression de l’Installation** page affiche l’état pendant le processus d’installation. Une fois que le message s’affiche indiquant que l’installation a réussi, cliquez sur **fermer**.

### <a name="configure-the-kernel-debugger-to-allow-qos-optional"></a>Configurer le débogueur de noyau pour permettre la qualité de service \(Optional\)

 Par défaut, le noyau débogueurs bloquent NetQos. Quelle que soit la méthode que vous avez utilisé pour installer DCB, si vous disposez d’un débogueur du noyau installé sur l’ordinateur, vous devez configurer le débogueur pour permettre la qualité de service soit activé et configuré en exécutant la commande suivante.

````
Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force
````

## <a name="install-dcb-in-windows-10"></a>Installer DCB dans Windows10

Vous pouvez effectuer la procédure suivante sur un ordinateur Windows10.

Pour effectuer cette procédure, vous devez être membre du **administrateurs **.

### <a name="install-dcb"></a>Installer DCB

1. Cliquez sur **Démarrer**, puis faites défiler vers le bas et cliquez sur **système Windows **.
2. Cliquez sur **Panneau de configuration **. Le **le panneau de configuration** boîte de dialogue s’ouvre.
3. Dans **le panneau de configuration**, cliquez sur **afficher en**, puis cliquez sur **grandes icônes** ou **petites icônes **.
4. Cliquez sur **programmes et fonctionnalités **. La boîte de dialogue programmes et fonctionnalités s’ouvre.
5. Dans **programmes et fonctionnalités**, dans le volet gauche, cliquez sur **ou désactiver des fonctionnalités Windows activer **. Le **fonctionnalités Windows** boîte de dialogue s’ouvre.
6. Dans **fonctionnalités Windows**, cliquez sur **Data Center Bridging**, puis cliquez sur **OK **.

![Activer ou désactiver la boîte de dialogue des fonctionnalités de Windows](../../media/Dcb-Scripting/Dcb-Scripting.jpg)


