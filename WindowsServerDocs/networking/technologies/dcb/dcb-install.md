---
title: Installer le Data Center Bridging (DCB) dans le client ou le serveur Windows
description: Cette rubrique fournit des instructions sur l’installation de la fonction de pontage de centre de données dans Windows Server ou le client Windows.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: b89213d8-143a-45f3-a609-bc6a7027204c
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5ecb6ef072dd2328a0a45d57d181dca9c2928a30
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405786"
---
# <a name="install-data-center-bridging-dcb-in-windows-server-2016-or-windows-10"></a>Installer Data Center Bridging \(DCB @ no__t-1 dans Windows Server 2016 ou Windows 10

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour apprendre à installer DCB dans Windows Server 2016 ou Windows 10.

## <a name="prerequisites-for-using-dcb"></a>Conditions préalables à l’utilisation de DCB

Vous trouverez ci-dessous les conditions préalables à la configuration et à la gestion de DCB.

### <a name="install-a-compatible-operating-system"></a>Installer un système d’exploitation compatible

Vous pouvez utiliser les commandes DCB de ce guide dans les systèmes d’exploitation suivants.

- Windows Server (canal semi-annuel)
- Windows Server 2016
- Windows 10 @no__t-versions 0tous @ no__t-1

Les systèmes d’exploitation suivants incluent des versions précédentes de DCB qui ne sont pas compatibles avec les commandes utilisées dans la documentation DCB pour Windows Server 2016 et Windows 10.

- Windows Server 2012 R2
- Windows Server 2012

###  <a name="hardware-requirements"></a>Configuration matérielle requise

La liste suivante répertorie la configuration matérielle requise pour DCB.

- La carte réseau Ethernet DCB @ no__t-0capable @ no__t-1s @ no__t-2 doit être installée sur les ordinateurs qui fournissent Windows Server 2016 DCB.
- Les commutateurs matériels DCB @ no__t-0capable doivent être déployés sur votre réseau.


## <a name="install-dcb-in-windows-server-2016"></a>Installer DCB dans Windows Server 2016

Vous pouvez utiliser les sections suivantes pour installer DCB sur un ordinateur exécutant Windows Server 2016.

**Informations d’identification d’administration**

Pour exécuter ces procédures, vous devez être membre des **administrateurs**.

### <a name="install-dcb-using-windows-powershell"></a>Installer DCB à l’aide de Windows PowerShell

Vous pouvez utiliser la procédure suivante pour installer DCB à l’aide de Windows PowerShell.

1. Sur un ordinateur exécutant Windows Server 2016, cliquez sur **Démarrer**, puis cliquez avec le bouton droit sur l’icône Windows PowerShell. Un menu s’affiche. Dans le menu, cliquez sur **plus**, puis sur **exécuter en tant qu’administrateur**. Si vous y êtes invité, tapez les informations d’identification d’un compte disposant de privilèges d’administrateur sur l’ordinateur. Windows PowerShell s’ouvre avec des privilèges d’administrateur.
2. Tapez la commande suivante et appuyez sur ENTRÉE.

````
    Install-WindowsFeature -Name Data-Center-Bridging -IncludeManagementTools
````

### <a name="install-dcb-using-server-manager"></a>Installer DCB à l’aide de Gestionnaire de serveur

Vous pouvez utiliser la procédure suivante pour installer DCB à l’aide de Gestionnaire de serveur.

>[!NOTE]
>Une fois que vous avez effectué la première étape de cette procédure, la page **avant de commencer** de l’Assistant Ajout de rôles et de fonctionnalités ne s’affiche pas si vous avez déjà sélectionné **ignorer cette page par défaut** lors de l’exécution de l’Assistant Ajout de rôles et de fonctionnalités. Si la page **avant de commencer** n’est pas affichée, passez de l’étape 1 à l’étape 3.

1. Sur DC1, dans Gestionnaire de serveur, cliquez sur **gérer**, puis sur **Ajouter des rôles et des fonctionnalités**. L’Assistant Ajout de rôles et de fonctionnalités s’ouvre.
2. Dans **Avant de commencer**, cliquez sur **Suivant**.
3. Dans **Sélectionner le type d’installation**, vérifiez que **Installation basée sur un rôle ou une fonctionnalité** est sélectionné, puis cliquez sur **Suivant**.
4. Dans **Sélectionner le serveur de destination**, vérifiez que **Sélectionner un serveur du pool de serveurs** est sélectionné. Dans **Pool de serveurs**, vérifiez que l’ordinateur local est sélectionné. Cliquez sur **Suivant**.
5. Dans **Sélectionner des rôles de serveurs**, cliquez sur **Suivant**.
6. Dans **Sélectionner des fonctionnalités**, dans **fonctionnalités**, cliquez sur pontage du **Centre de données**. Une boîte de dialogue s’ouvre pour vous demander si vous souhaitez ajouter des fonctionnalités DCB requises. Cliquez sur **Ajouter des fonctionnalités**.
7. Dans **Sélectionner des fonctionnalités**, cliquez sur **suivant**. 
8. 7.In **confirmer les sélections d’installation**, cliquez sur **installer**. La page progression de l' **installation** affiche l’État pendant le processus d’installation. Après l’affichage du message indiquant que l’installation a réussi, cliquez sur **Fermer**.

### <a name="configure-the-kernel-debugger-to-allow-qos-optional"></a>Configurer le débogueur du noyau pour autoriser la QoS \(Optional @ no__t-1

 Par défaut, les débogueurs de noyau bloquent NetQos. Quelle que soit la méthode que vous avez utilisée pour installer DCB, si vous avez un débogueur de noyau installé sur l’ordinateur, vous devez configurer le débogueur pour permettre l’activation et la configuration de la qualité de service (QoS) à l’aide de la commande suivante.

````
Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force
````

## <a name="install-dcb-in-windows-10"></a>Installer DCB dans Windows 10

Vous pouvez effectuer la procédure suivante sur un ordinateur Windows 10.

Pour effectuer cette procédure, vous devez être membre des **administrateurs**.

### <a name="install-dcb"></a>Installer DCB

1. Cliquez sur **Démarrer**, puis faites défiler jusqu’à, puis cliquez sur **système Windows**.
2. Cliquez sur **Panneau de configuration**. La boîte de dialogue **panneau de configuration** s’ouvre.
3. Dans **le panneau de configuration**, cliquez sur **afficher par**, puis cliquez sur **grandes icônes** ou **petites icônes**.
4. Cliquez sur **programmes et fonctionnalités**. La boîte de dialogue programmes et fonctionnalités s’ouvre.
5. Dans **programmes et fonctionnalités**, dans le volet gauche, cliquez sur **activer ou désactiver des fonctionnalités Windows**. La boîte de dialogue **fonctionnalités de Windows** s’ouvre.
6. Dans **fonctionnalités de Windows**, cliquez sur **pontage du centre de données**, puis cliquez sur **OK**.

![Boîte de dialogue Activer ou désactiver des fonctionnalités Windows](../../media/Dcb-Scripting/Dcb-Scripting.jpg)


