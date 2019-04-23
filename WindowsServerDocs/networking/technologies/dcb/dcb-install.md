---
title: Installer Data Center Bridging (DCB) dans Windows Server ou Client
description: Cette rubrique fournit des instructions sur l’installation de Data Center Bridging dans Windows Server ou Windows Client.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: b89213d8-143a-45f3-a609-bc6a7027204c
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7c20ef027279780181ff176afa39a19f2976c4c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845440"
---
# <a name="install-data-center-bridging-dcb-in-windows-server-2016-or-windows-10"></a>Pontage de centre de données d’installation \(DCB\) dans Windows Server 2016 ou Windows 10

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour savoir comment installer DCB dans Windows Server 2016 ou Windows 10.

## <a name="prerequisites-for-using-dcb"></a>Conditions préalables pour l’utilisation de DCB

Voici les conditions préalables pour configurer et gérer DCB.

### <a name="install-a-compatible-operating-system"></a>Installer un système d’exploitation compatible

Vous pouvez utiliser les commandes DCB à partir de ce guide dans les systèmes d’exploitation suivants.

- Windows Server (canal semi-annuel)
- Windows Server 2016
- Windows 10 \(toutes les versions\)

Les systèmes d’exploitation suivants incluent les versions précédentes de DCB qui ne sont pas compatibles avec les commandes qui sont utilisés dans la documentation de DCB pour Windows Server 2016 et Windows 10.

- Windows Server 2012 R2
- Windows Server 2012

###  <a name="hardware-requirements"></a>Configuration matérielle requise

Voici une liste de la configuration matérielle requise pour DCB.

- DCB\-carte de réseau Ethernet compatible\(s\) doit être installé sur les ordinateurs qui fournissent DCB de Windows Server 2016.
- DCB\-matériel compatible avec les commutateurs doivent être déployés sur votre réseau.


## <a name="install-dcb-in-windows-server-2016"></a>Installer DCB dans Windows Server 2016

Vous pouvez utiliser les sections suivantes pour installer DCB sur un ordinateur exécutant Windows Server 2016.

**Informations d’identification administratives**

Pour effectuer ces procédures, vous devez être membre du **administrateurs**.

### <a name="install-dcb-using-windows-powershell"></a>Installer DCB à l’aide de Windows PowerShell

Vous pouvez utiliser la procédure suivante pour installer DCB à l’aide de Windows PowerShell.

1. Sur un ordinateur exécutant Windows Server 2016, cliquez sur **Démarrer**, puis cliquez sur l’icône Windows PowerShell. Un menu s’affiche. Dans le menu, cliquez sur **plus**, puis cliquez sur **exécuter en tant qu’administrateur**. Si vous y êtes invité, tapez les informations d’identification pour un compte disposant des privilèges d’administrateur sur l’ordinateur. Windows PowerShell s’ouvre avec des privilèges d’administrateur.
2. Tapez la commande suivante et appuyez sur ENTRÉE.

````
    Install-WindowsFeature -Name Data-Center-Bridging -IncludeManagementTools
````

### <a name="install-dcb-using-server-manager"></a>Installer DCB à l’aide du Gestionnaire de serveur

Vous pouvez utiliser la procédure suivante pour installer DCB à l’aide du Gestionnaire de serveur.

>[!NOTE]
>Après avoir effectué la première étape dans cette procédure, le **avant de commencer** page d’ajout de rôles et fonctionnalités Assistant s’affiche pas si vous avez sélectionné précédemment **ignorer cette page par défaut** lors de l’ajout Assistant de rôles et fonctionnalités a été exécuté. Si le **avant de commencer** page s’affiche pas, passez à l’étape 1 à l’étape 3.

1. Sur DC1, dans le Gestionnaire de serveur, cliquez sur **gérer**, puis cliquez sur **Ajout de rôles et fonctionnalités**. L’Assistant Ajout de rôles et de fonctionnalités s’ouvre.
2. Dans **Avant de commencer**, cliquez sur **Suivant**.
3. Dans **Sélectionner le type d’installation**, vérifiez que **Installation basée sur un rôle ou une fonctionnalité** est sélectionné, puis cliquez sur **Suivant**.
4. Dans **Sélectionner le serveur de destination**, vérifiez que **Sélectionner un serveur du pool de serveurs** est sélectionné. Dans **Pool de serveurs**, vérifiez que l’ordinateur local est sélectionné. Cliquez sur **Suivant**.
5. Dans **Sélectionner des rôles de serveurs**, cliquez sur **Suivant**.
6. Dans **sélectionner des fonctionnalités**, dans **fonctionnalités**, cliquez sur **Data Center Bridging**. Une boîte de dialogue s’ouvre pour vous demander si vous souhaitez ajouter des fonctionnalités DCB requis. Cliquez sur **ajouter des fonctionnalités**.
7. Dans **sélectionner des fonctionnalités**, cliquez sur **suivant**. 
8. 7. dans **confirmer les sélections d’installation**, cliquez sur **installer**. Le **progression de l’Installation** page affiche l’état pendant le processus d’installation. Une fois que le message s’affiche et indique que l’installation a réussi, cliquez sur **fermer**.

### <a name="configure-the-kernel-debugger-to-allow-qos-optional"></a>Configurer le débogueur du noyau pour autoriser QoS \(facultatif\)

 Par défaut, les débogueurs de noyau bloquent NetQos. Quelle que soit la méthode que vous avez utilisé pour installer DCB, si vous avez un débogueur du noyau installé dans l’ordinateur, vous devez configurer le débogueur pour autoriser la qualité de service être activée et configurée en exécutant la commande suivante.

````
Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force
````

## <a name="install-dcb-in-windows-10"></a>Installer DCB dans Windows 10

Vous pouvez effectuer la procédure suivante sur un ordinateur Windows 10.

Pour effectuer cette procédure, vous devez être membre du **administrateurs**.

### <a name="install-dcb"></a>Installer DCB

1. Cliquez sur **Démarrer**, puis accédez à, cliquez sur **Windows System**.
2. Cliquez sur **Panneau de configuration**. Le **le panneau de configuration** boîte de dialogue s’ouvre.
3. Dans **le panneau de configuration**, cliquez sur **afficher par**, puis cliquez sur **grandes icônes** ou **petites icônes**.
4. Cliquez sur **programmes et fonctionnalités**. La boîte de dialogue programmes et fonctionnalités s’ouvre.
5. Dans **programmes et fonctionnalités**, dans le volet gauche, cliquez sur **ou désactiver des fonctionnalités Windows activer**. Le **les fonctionnalités de Windows** boîte de dialogue s’ouvre.
6. Dans **les fonctionnalités de Windows**, cliquez sur **Data Center Bridging**, puis cliquez sur **OK**.

![Activer ou désactiver la boîte de dialogue des fonctionnalités de Windows](../../media/Dcb-Scripting/Dcb-Scripting.jpg)


