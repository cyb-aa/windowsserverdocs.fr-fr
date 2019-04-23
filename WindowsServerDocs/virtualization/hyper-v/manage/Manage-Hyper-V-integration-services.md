---
title: Gérer les Services d’intégration Hyper-V
description: Décrit comment activer et désactiver les services d’intégration et de les installer si nécessaire
ms.technology: compute-hyper-v
author: KBDAzure
ms.author: kathydav
manager: dongill
ms.date: 12/20/2016
ms.topic: article
ms.prod: windows-server-threshold
ms.service: na
ms.assetid: 9cafd6cb-dbbe-4b91-b26c-dee1c18fd8c2
ms.openlocfilehash: b049efc61d5060791574f20fcdd8b369a26f0507
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890250"
---
>S'applique à : Windows 10, Windows Server 2016, Windows Server 2019

# <a name="manage-hyper-v-integration-services"></a>Gérer les Services d’intégration Hyper-V

Services d’intégration Hyper-V améliorer les performances des machines virtuelles et fournissent des fonctionnalités pratiques en tirant parti d’une communication bidirectionnelle avec l’hôte Hyper-V. Plusieurs de ces services sont pratiques, comme la copie de fichiers invité, tandis que d’autres sont importantes pour les fonctionnalités de la machine virtuelle, tels que les pilotes de périphérique synthétique. Cet ensemble de services et pilotes sont parfois appelés « composants d’intégration ». Vous pouvez contrôler ou non les services de commodité individuels fonctionnent pour une machine virtuelle donnée. Les composants de pilote ne sont pas destinées à être traitées manuellement.

Pour plus d’informations sur chaque service d’intégration, consultez [Services d’intégration Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services).

>[!IMPORTANT]
>Chaque service que vous souhaitez utiliser doit être activé dans l’hôte et l’invité pour fonctionner. Tous les services d’intégration à l’exception de « Interface de Service invité de Hyper-V » sont activées par défaut sur les systèmes d’exploitation invités Windows. Les services peuvent être activées et désactivée individuellement. Les sections suivantes vous montrent comment.

## <a name="turn-an-integration-service-on-or-off-using-hyper-v-manager"></a>Activer un service d’intégration ou désactivé à l’aide du Gestionnaire Hyper-V

1. Dans le volet central, cliquez sur la machine virtuelle, puis cliquez sur **paramètres**.
  
2. Dans le volet gauche de la **paramètres** fenêtre, sous **gestion**, cliquez sur **Integration Services**.
  
Le volet d’Integration Services répertorie tous les services d’intégration disponibles sur l’hôte Hyper-V, et si l’hôte a activé la machine virtuelle pour les utiliser.

### <a name="turn-an-integration-service-on-or-off-using-powershell"></a>Activer un service d’intégration ou désactivé à l’aide de PowerShell

Pour ce faire, dans PowerShell, utilisez [Enable-VMIntegrationService](https://technet.microsoft.com/library/hh848500.aspx) et [Disable-VMIntegrationService](https://technet.microsoft.com/library/hh848488.aspx).

Les exemples suivants montrent l’activation de l’invité fichier copie service d’intégration sur et hors tension pour un ordinateur virtuel nommé « demovm ».

1. Obtenir la liste des services d’intégration en cours d’exécution :
  
    ``` PowerShell
    Get-VMIntegrationService -VMName "DemoVM"
    ```
1. La sortie doit ressembler à ceci :

    ``` PowerShell
   VMName      Name                    Enabled PrimaryStatusDescription SecondaryStatusDescription
   ------      ----                    ------- ------------------------ --------------------------
   DemoVM      Guest Service Interface False   OK
   DemoVM      Heartbeat               True    OK                       OK
   DemoVM      Key-Value Pair Exchange True    OK
   DemoVM      Shutdown                True    OK
   DemoVM      Time Synchronization    True    OK
   DemoVM      VSS                     True    OK
   ```

1. Activer l’Interface de Service invité :

   ``` PowerShell
   Enable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
   ```

1. Vérifiez que l’Interface de Service invité est activé :

   ```
   Get-VMIntegrationService -VMName "DemoVM" 
   ``` 

1. Désactiver l’Interface de Service invité :

    ```
    Disable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
    ```
   
## <a name="checking-the-guests-integration-services-version"></a>Vérification de la version des services d’intégration de l’invité
Certaines fonctionnalités ne peuvent pas fonctionner correctement si les services d’intégration de l’invité ne sont pas en cours. Pour obtenir les informations de version pour un Windows, ouvrez une session sur le système d’exploitation invité, ouvrez une invite de commandes et exécutez la commande suivante :

```
REG QUERY "HKLM\Software\Microsoft\Virtual Machine\Auto" /v IntegrationServicesVersion
```
Les systèmes d’exploitation invité n’a pas de tous les services disponibles. Par exemple, les invités Windows Server 2008 R2 ne peut pas avoir « Interface de Service de Hyper-V invité ».

## <a name="start-and-stop-an-integration-service-from-a-windows-guest"></a>Démarrer et arrêter un service d’intégration à partir d’un invité de Windows
Dans l’ordre pour un service d’intégration soit pleinement opérationnelle, son service correspondante doit être exécuté dans l’invité en plus en cours d’activation sur l’ordinateur hôte. Dans les invités Windows, chaque service d’intégration est répertorié comme un service Windows standard. Vous pouvez utiliser l’applet Services dans le panneau de configuration ou de PowerShell pour arrêter et démarrer ces services.

>[!IMPORTANT]
> L’arrêt d’un service d’intégration peut avoir un impact considérable sur les capacité de l’hôte pour gérer votre machine virtuelle. Pour fonctionner correctement, chaque service d’intégration que vous souhaitez utiliser doit être activé sur l’hôte et l’invité.
> Comme meilleure pratique, vous devez uniquement contrôler integration services dans Hyper-V en suivant les instructions ci-dessus. Le service correspondant dans le système d’exploitation invité s’arrêter ou démarrer automatiquement lorsque vous modifiez son état dans Hyper-V.
> Si vous démarrez un service dans le système d’exploitation invité, mais il est désactivé dans Hyper-V, le service s’arrête. Si vous arrêtez un service dans le système d’exploitation invité prenant en charge dans Hyper-V, Hyper-V sera finalement démarrez-le à nouveau. Si vous désactivez le service dans l’invité, Hyper-V ne pourra pas démarrer.

### <a name="use-windows-services-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>Utiliser les Services Windows pour démarrer ou arrêter un service d’intégration au sein d’un invité de Windows

1. Ouvrez le gestionnaire Services en exécutant ```services.msc``` en tant qu’administrateur ou en double-cliquant sur l’icône Services dans le panneau de configuration.

    ![Capture d’écran montrant le volet des Services de Windows](media/HVServices.png) 

1. Recherchez les services qui commencent par « Hyper-V ». 

1. Cliquez sur le service de démarrage ou arrêt. Cliquez sur l’action souhaitée.


### <a name="use-windows-powershell-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>Utiliser Windows PowerShell pour démarrer ou arrêter un service d’intégration au sein d’un invité de Windows

1. Pour obtenir une liste des services d’intégration, exécutez :

    ```
    Get-Service -Name vm*
    ```
1.  La sortie doit ressembler à ceci :

    ```PowerShell
    Status   Name               DisplayName
    ------   ----               -----------
    Running  vmicguestinterface Hyper-V Guest Service Interface
    Running  vmicheartbeat      Hyper-V Heartbeat Service
    Running  vmickvpexchange    Hyper-V Data Exchange Service
    Running  vmicrdv            Hyper-V Remote Desktop Virtualizati...
    Running  vmicshutdown       Hyper-V Guest Shutdown Service
    Running  vmictimesync       Hyper-V Time Synchronization Service
    Stopped  vmicvmsession      Hyper-V VM Session Service
    Running  vmicvss            Hyper-V Volume Shadow Copy Requestor
    ```

1. Exécutez le [Start-Service](https://technet.microsoft.com/library/hh849825.aspx) ou [Stop-Service](https://technet.microsoft.com/library/hh849790.aspx). Par exemple, pour désactiver directe de Windows PowerShell, exécutez :

    ```
    Stop-Service -Name vmicvmsession
    ```

## <a name="start-and-stop-an-integration-service-from-a-linux-guest"></a>Démarrer et arrêter un service d’intégration à partir d’un invité Linux 

Les services d’intégration Linux sont généralement fournis par le noyau Linux. Le pilote de services d’intégration Linux est nommé **hv_utils**.

1.  Pour déterminer si **hv_utils** est chargé, utilisez la commande suivante :

    ``` BASH
    lsmod | grep hv_utils
    ``` 
  
1. La sortie doit ressembler à ceci :  
  
    ``` BASH
    Module                  Size   Used by
    hv_utils               20480   0
    hv_vmbus               61440   8 hv_balloon,hyperv_keyboard,hv_netvsc,hid_hyperv,hv_utils,hyperv_fb,hv_storvsc
    ```

1. Pour savoir si les démons nécessaires sont en cours d’exécution, utilisez cette commande.
  
    ``` BASH
    ps -ef | grep hv
    ```
  
1. La sortie doit ressembler à ceci : 
  
    ```BASH
    root       236     2  0 Jul11 ?        00:00:00 [hv_vmbus_con]
    root       237     2  0 Jul11 ?        00:00:00 [hv_vmbus_ctl]
    ...
    root       252     2  0 Jul11 ?        00:00:00 [hv_vmbus_ctl]
    root      1286     1  0 Jul11 ?        00:01:11 /usr/lib/linux-tools/3.13.0-32-generic/hv_kvp_daemon
    root      9333     1  0 Oct12 ?        00:00:00 /usr/lib/linux-tools/3.13.0-32-generic/hv_kvp_daemon
    root      9365     1  0 Oct12 ?        00:00:00 /usr/lib/linux-tools/3.13.0-32-generic/hv_vss_daemon
    scooley  43774 43755  0 21:20 pts/0    00:00:00 grep --color=auto hv          
    ```

1. Pour afficher les démons disponibles, exécutez la commande suivante :

    ``` BASH
    compgen -c hv_
    ```
  
1. La sortie doit ressembler à ceci :
  
    ``` BASH
    hv_vss_daemon
    hv_get_dhcp_info
    hv_get_dns_info
    hv_set_ifconfig
    hv_kvp_daemon
    hv_fcopy_daemon     
    ```
  
 Démons de service d’intégration qui peuvent être répertoriés sont les suivants : Si une est manquante, ils ne peuvent pas être pris en charge sur votre système, ou ils ne peuvent pas être installés. Plus d’informations, consultez la rubrique [pris en charge de Linux et FreeBSD machines virtuelles pour Hyper-V sur Windows](https://technet.microsoft.com/library/dn531030.aspx).  
  - **hv_vss_daemon**: Ce démon est nécessaire pour créer des sauvegardes dynamiques de la machine virtuelle Linux.
  - **hv_kvp_daemon**: Ce démon permet de définir et d’interroger les paires clé-valeur intrinsèques et extrinsèques.
  - **hv_fcopy_daemon**: Ce démon implémente un service entre l’hôte et l’invité de copie de fichiers.  

### <a name="examples"></a>Exemples

Ces exemples illustrent les arrêter et démarrer le démon KVP, nommé `hv_kvp_daemon`.

1. Utilisez l’ID de processus \(PID\) pour arrêter le processus du démon. Pour rechercher le PID, examinez la deuxième colonne de la sortie, ou utilisez `pidof`. Les démons Hyper-V exécutent en tant que racine, vous aurez besoin des autorisations de racine.

    ``` BASH
    sudo kill -15 `pidof hv_kvp_daemon`
    ```

1. Pour vérifier que tous les `hv_kvp_daemon` processus ont disparu, exécutez :

    ```
    ps -ef | hv
    ```

1. Pour démarrer le démon à nouveau, exécutez le démon en tant que racine :

    ``` BASH
    sudo hv_kvp_daemon
    ``` 

1. Pour vérifier que le `hv_kvp_daemon` processus est répertorié avec un nouvel ID de processus, exécutez :

    ```
    ps -ef | hv
    ```

## <a name="keep-integration-services-up-to-date"></a>Services d’intégration mise à jour

Nous vous recommandons de conserver les services d’intégration à jour pour obtenir de meilleures performances et des fonctionnalités les plus récentes pour vos machines virtuelles. Cela se produit pour la plupart des invités Windows. de la fenêtre par défaut si elles sont configurées pour obtenir des mises à jour importantes à partir de Windows Update. Invités Linux à l’aide de noyaux actuelles reçoit les composants d’intégration plus récente lorsque vous mettez à jour le noyau.

**Pour les machines virtuelles qui s’exécutent sur des hôtes Windows 10 :**

> [!NOTE]
> Le fichier image vmguest.iso n’est pas inclus avec Hyper-V sur Windows 10, car il n’est plus nécessaire.

| Invité  | Mécanisme de mise à jour | Notes |
|:---------|:---------|:---------|
| Windows 10 | Windows Update | |
| Windows 8.1 | Windows Update | |
| Windows 8 | Windows Update | Nécessite le service d’intégration Échange de données.* |
| Windows 7 | Windows Update | Nécessite le service d’intégration Échange de données.* |
| Windows Vista (SP2) | Windows Update | Nécessite le service d’intégration Échange de données.* |
| - | | |
| Windows Server 2016 | Windows Update | |
| Windows Server, canal semi-annuel | Windows Update | |
| Windows Server 2012 R2 | Windows Update | |
| Windows Server 2012 | Windows Update | Nécessite le service d’intégration Échange de données.* |
| Windows Server 2008 R2 (SP1) | Windows Update | Nécessite le service d’intégration Échange de données.* |
| Windows Server 2008 (SP2) | Windows Update | Support étendu uniquement dans Windows Server 2016 ([plus](https://support.microsoft.com/lifecycle?p1=12925)). |
| Windows Home Server 2011 | Windows Update | Ne sera pas pris en charge dans Windows Server 2016 ([plus](https://support.microsoft.com/lifecycle?p1=15820)). |
| Windows Small Business Server 2011 | Windows Update | Pas de support standard ([en savoir plus](https://support.microsoft.com/lifecycle?p1=15817)). |
| - | | |
| Invités Linux | gestionnaire de package | Services d’intégration pour Linux sont intégrés à la distribution, mais mises à jour facultatives peuvent être disponibles. ******** |

\* Si le service d’intégration échange de données ne peut pas être activé, les services d’intégration pour ces invités sont disponibles à partir de la [centre de téléchargement](https://support.microsoft.com/kb/3071740) comme un fichier cabinet (cab). Instructions pour appliquer un fichier cab sont disponibles dans ce [billet de blog](https://blogs.technet.com/b/virtualization/archive/2015/07/24/integration-components-available-for-virtual-machines-not-connected-to-windows-update.aspx).

**Pour les machines virtuelles qui s’exécutent sur des hôtes Windows 8.1 :**

| Invité  | Mécanisme de mise à jour | Notes |
|:---------|:---------|:---------|
| Windows 10 | Windows Update | |
| Windows 8.1 | Windows Update | |
| Windows 8 | Disque des services d’intégration | Consultez [instructions](#install-or-update-integration-services), ci-dessous. |
| Windows 7 | Disque des services d’intégration | Consultez [instructions](#install-or-update-integration-services), ci-dessous. |
| Windows Vista (SP2) | Disque des services d’intégration | Consultez [instructions](#install-or-update-integration-services), ci-dessous. |
| Windows XP (SP2, SP3) | Disque des services d’intégration | Consultez [instructions](#install-or-update-integration-services), ci-dessous. |
| - | | |
| Windows Server 2016 | Windows Update | |
| Windows Server, canal semi-annuel | Windows Update | |
| Windows Server 2012 R2 | Windows Update | |
| Windows Server 2012 | Disque des services d’intégration | Consultez [instructions](#install-or-update-integration-services), ci-dessous. |
| Windows Server 2008 R2 | Disque des services d’intégration | Consultez [instructions](#install-or-update-integration-services), ci-dessous. |
| Windows Server 2008 (SP2) | Disque des services d’intégration | Consultez [instructions](#install-or-update-integration-services), ci-dessous. |
| Windows Home Server 2011 | Disque des services d’intégration | Consultez [instructions](#install-or-update-integration-services), ci-dessous. |
| Windows Small Business Server 2011 | Disque des services d’intégration | Consultez [instructions](#install-or-update-integration-services), ci-dessous. |
| Windows Server 2003 R2 (SP2) | Disque des services d’intégration | Consultez [instructions](#install-or-update-integration-services), ci-dessous. |
| Windows Server 2003 (SP2) | Disque des services d’intégration | Consultez [instructions](#install-or-update-integration-services), ci-dessous. |
| - | | |
| Invités Linux | gestionnaire de package | Services d’intégration pour Linux sont intégrés à la distribution, mais mises à jour facultatives peuvent être disponibles. ** |


**Pour les machines virtuelles qui s’exécutent sur des hôtes Windows 8 :**

| Invité  | Mécanisme de mise à jour | Notes |
|:---------|:---------|:---------|
| Windows 8.1 | Windows Update | |
| Windows 8 | Disque des services d’intégration | Consultez [instructions](#install-or-update-integration-services), ci-dessous. |
| Windows 7 | Disque des services d’intégration | Consultez [instructions](#install-or-update-integration-services), ci-dessous. |
| Windows Vista (SP2) | Disque des services d’intégration | Consultez [instructions](#install-or-update-integration-services), ci-dessous. |
| Windows XP (SP2, SP3) | Disque des services d’intégration | Consultez [instructions](#install-or-update-integration-services), ci-dessous. |
| - | | |
| Windows Server 2012 R2 | Windows Update | |
| Windows Server 2012 | Disque des services d’intégration | Consultez [instructions](#install-or-update-integration-services), ci-dessous. |
| Windows Server 2008 R2 | Disque des services d’intégration | Consultez [instructions](#install-or-update-integration-services), ci-dessous.|
| Windows Server 2008 (SP2) | Disque des services d’intégration | Consultez [instructions](#install-or-update-integration-services), ci-dessous. |
| Windows Home Server 2011 | Disque des services d’intégration | Consultez [instructions](#install-or-update-integration-services), ci-dessous. |
| Windows Small Business Server 2011 | Disque des services d’intégration | Consultez [instructions](#install-or-update-integration-services), ci-dessous. |
| Windows Server 2003 R2 (SP2) | Disque des services d’intégration | Consultez [instructions](#install-or-update-integration-services), ci-dessous. |
| Windows Server 2003 (SP2) | Disque des services d’intégration | Consultez [instructions](#install-or-update-integration-services), ci-dessous. |
| - | | |
| Invités Linux | gestionnaire de package | Services d’intégration pour Linux sont intégrés à la distribution, mais mises à jour facultatives peuvent être disponibles. ** |

Pour plus d’informations sur les invités Linux sont disponibles, consultez [pris en charge de Linux et FreeBSD machines virtuelles pour Hyper-V sur Windows](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows).

## <a name="install-or-update-integration-services"></a>Installer ou mettre à jour des services d’intégration

Pour les ordinateurs hôtes antérieurs à Windows Server 2016 et Windows 10, vous devez installer ou mettre à jour les services d’intégration dans les systèmes d’exploitation invité manuellement. 
  
1.  Ouvrez le Gestionnaire Hyper-V. Dans le menu Outils du Gestionnaire de serveur, cliquez sur **Gestionnaire Hyper-V**.  
  
2.  Connectez-vous à l’ordinateur virtuel. Avec le bouton droit de la machine virtuelle et cliquez sur **Connect**.  
  
3.  Dans le menu Action de l’outil Connexion à un ordinateur virtuel, cliquez sur **Insérer le disque d’installation des services d’intégration**. Cette action charge le disque d’installation dans le lecteur de DVD virtuel. Selon le système d’exploitation invité, vous devrez peut-être démarrer l’installation manuellement.  
  
4.  Quand l’installation est terminée, tous les services d’intégration peuvent être utilisés.

Ces étapes ne peuvent pas être automatisés ou effectuées au sein d’une session Windows PowerShell pour les machines virtuelles en ligne. Vous pouvez les appliquer aux images VHDX hors connexion ; [consultez le blog](https://blogs.technet.microsoft.com/virtualization/2013/04/18/how-to-install-integration-services-when-the-virtual-machine-is-not-running/).
