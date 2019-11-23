---
title: Gérer les Integration Services Hyper-V
description: Décrit comment activer et désactiver les services d’intégration et les installer si nécessaire.
ms.technology: compute-hyper-v
author: KBDAzure
ms.author: kathydav
manager: dongill
ms.date: 12/20/2016
ms.topic: article
ms.prod: windows-server
ms.service: na
ms.assetid: 9cafd6cb-dbbe-4b91-b26c-dee1c18fd8c2
ms.openlocfilehash: 39d57afbd8c4df78764c5975d4cc3d48848475c1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392764"
---
>S’applique à : Windows 10, Windows Server 2016, Windows Server 2019

# <a name="manage-hyper-v-integration-services"></a>Gérer les Integration Services Hyper-V

Hyper-V Integration Services améliorer les performances des machines virtuelles et fournir des fonctionnalités pratiques en tirant parti de la communication bidirectionnelle avec l’hôte Hyper-V. La plupart de ces services sont pratiques, tels que la copie de fichiers invités, tandis que d’autres sont importants pour les fonctionnalités de la machine virtuelle, telles que les pilotes de périphériques synthétiques. Cet ensemble de services et de pilotes est parfois appelé « composants d’intégration ». Vous pouvez contrôler si des services pratiques individuels fonctionnent pour une machine virtuelle donnée. Les composants du pilote ne sont pas destinés à être desservis manuellement.

Pour plus d’informations sur chaque service d’intégration, consultez [Hyper-V Integration Services](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services).

> [!IMPORTANT]
> Chaque service que vous souhaitez utiliser doit être activé à la fois dans l’hôte et l’invité pour fonctionner. Tous les services d’intégration, à l’exception de « Interface de services d’invité Hyper-V », sont activés par défaut sur les systèmes d’exploitation invités Windows. Les services peuvent être activés et désactivés individuellement. Les sections suivantes vous montrent comment procéder.

## <a name="turn-an-integration-service-on-or-off-using-hyper-v-manager"></a>Activer ou désactiver un service d’intégration à l’aide du Gestionnaire Hyper-V

1. Dans le volet central, cliquez avec le bouton droit sur l’ordinateur virtuel, puis cliquez sur **paramètres**.
  
2. Dans le volet gauche de la fenêtre **paramètres** , sous **gestion**, cliquez sur **Integration Services**.
  
Le volet Integration Services répertorie tous les services d’intégration disponibles sur l’hôte Hyper-V, et indique si l’ordinateur hôte a activé l’ordinateur virtuel pour les utiliser.

### <a name="turn-an-integration-service-on-or-off-using-powershell"></a>Activer ou désactiver un service d’intégration à l’aide de PowerShell

Pour effectuer cette opération dans PowerShell, utilisez [Enable-VMIntegrationService](https://technet.microsoft.com/library/hh848500.aspx) et [Disable-VMIntegrationService](https://technet.microsoft.com/library/hh848488.aspx).

Les exemples suivants montrent comment activer et désactiver le service d’intégration de copie de fichiers invités pour un ordinateur virtuel nommé « demovm ».

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

1. Activer l’interface de service invité :

   ``` PowerShell
   Enable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
   ```

1. Vérifiez que l’interface de service invité est activée :

   ```
   Get-VMIntegrationService -VMName "DemoVM" 
   ``` 

1. Désactiver l’interface de service invité :

    ```
    Disable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
    ```
   
## <a name="checking-the-guests-integration-services-version"></a>Vérification de la version d’Integration Services de l’invité
Certaines fonctionnalités peuvent ne pas fonctionner correctement ou du tout si les services d’intégration de l’invité ne sont pas à jour. Pour obtenir les informations de version d’une fenêtre, connectez-vous au système d’exploitation invité, ouvrez une invite de commandes, puis exécutez la commande suivante :

```
REG QUERY "HKLM\Software\Microsoft\Virtual Machine\Auto" /v IntegrationServicesVersion
```

Les systèmes d’exploitation invités antérieurs ne disposent pas de tous les services disponibles. Par exemple, les invités Windows Server 2008 R2 ne peuvent pas avoir le « Interface de services d’invité Hyper-V ».

## <a name="start-and-stop-an-integration-service-from-a-windows-guest"></a>Démarrer et arrêter un service d’intégration à partir d’un invité Windows
Pour qu’un service d’intégration soit entièrement fonctionnel, son service correspondant doit être en cours d’exécution au sein de l’invité, en plus d’être activé sur l’ordinateur hôte. Dans les invités Windows, chaque service d’intégration est listé comme un service Windows standard. Vous pouvez utiliser l’applet services du panneau de configuration ou PowerShell pour arrêter et démarrer ces services.

> [!IMPORTANT]
> L’arrêt d’un service d’intégration peut affecter gravement la capacité de l’hôte à gérer votre machine virtuelle. Pour fonctionner correctement, chaque service d’intégration que vous souhaitez utiliser doit être activé sur l’hôte et l’invité.
> En guise de meilleure pratique, vous devez uniquement contrôler les services d’intégration à partir d’Hyper-V à l’aide des instructions ci-dessus. Le service de correspondance dans le système d’exploitation invité s’arrête ou démarre automatiquement lorsque vous modifiez son état dans Hyper-V.
> Si vous démarrez un service dans le système d’exploitation invité, mais que celui-ci est désactivé dans Hyper-V, le service s’arrête. Si vous arrêtez un service dans le système d’exploitation invité qui est activé dans Hyper-V, Hyper-V le redémarre finalement. Si vous désactivez le service dans l’invité, Hyper-V ne pourra pas le démarrer.

### <a name="use-windows-services-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>Utiliser les services Windows pour démarrer ou arrêter un service d’intégration au sein d’un invité Windows

1. Ouvrez le gestionnaire de services en exécutant ```services.msc``` en tant qu’administrateur ou en double-cliquant sur l’icône services dans le panneau de configuration.

    ![Capture d’écran qui montre le volet services Windows](media/HVServices.png) 

1. Recherchez les services qui commencent par « Hyper-V ». 

1. Cliquez avec le bouton droit sur le service que vous voulez démarrer ou arrêter. Cliquez sur l’action souhaitée.

### <a name="use-windows-powershell-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>Utiliser Windows PowerShell pour démarrer ou arrêter un service d’intégration au sein d’un invité Windows

1. Pour obtenir la liste des services d’intégration, exécutez :

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

1. Exécutez [Start-Service](https://technet.microsoft.com/library/hh849825.aspx) ou [Stop-service](https://technet.microsoft.com/library/hh849790.aspx). Par exemple, pour désactiver Windows PowerShell direct, exécutez :

    ```
    Stop-Service -Name vmicvmsession
    ```

## <a name="start-and-stop-an-integration-service-from-a-linux-guest"></a>Démarrer et arrêter un service d’intégration à partir d’un invité Linux 

Les services d’intégration Linux sont généralement fournis par le noyau Linux. Le pilote des services d’intégration Linux est nommé **hv_utils**.

1. Pour déterminer si **hv_utils** est chargé, utilisez la commande suivante :

   ``` BASH
   lsmod | grep hv_utils
   ``` 
  
2. La sortie doit ressembler à ceci :  
  
    ``` BASH
    Module                  Size   Used by
    hv_utils               20480   0
    hv_vmbus               61440   8 hv_balloon,hyperv_keyboard,hv_netvsc,hid_hyperv,hv_utils,hyperv_fb,hv_storvsc
    ```

3. Pour déterminer si les démons nécessaires sont en cours d’exécution, utilisez cette commande.
  
    ``` BASH
    ps -ef | grep hv
    ```
  
4. La sortie doit ressembler à ceci : 
  
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

5. Pour afficher les démons disponibles, exécutez la commande suivante :

    ``` BASH
    compgen -c hv_
    ```
  
6. La sortie doit ressembler à ceci :
  
    ``` BASH
    hv_vss_daemon
    hv_get_dhcp_info
    hv_get_dns_info
    hv_set_ifconfig
    hv_kvp_daemon
    hv_fcopy_daemon     
    ```
  
   Les démons de service d’intégration qui peuvent être listés sont les suivants : S’ils sont manquants, ils ne sont peut-être pas pris en charge sur votre système ou ne sont peut-être pas installés. Pour en savoir plus, consultez [machines virtuelles Linux et FreeBSD prises en charge pour Hyper-V sur Windows](https://technet.microsoft.com/library/dn531030.aspx).  
   - **hv_vss_daemon**: ce démon est nécessaire pour créer des sauvegardes de machines virtuelles Linux en temps réel.
   - **hv_kvp_daemon**: ce démon permet de définir et d’interroger des paires clé-valeur intrinsèques et extrinsèques.
   - **hv_fcopy_daemon**: ce démon implémente un service de copie de fichiers entre l’hôte et l’invité.  

### <a name="examples"></a>Exemples

Ces exemples montrent comment arrêter et démarrer le démon KVP, nommé `hv_kvp_daemon`.

1. Utilisez l’ID de processus \(PID\) pour arrêter le processus du démon. Pour rechercher le PID, examinez la deuxième colonne de la sortie ou utilisez `pidof`. Les démons Hyper-V s’exécutent en tant qu’utilisateur racine. vous aurez donc besoin d’autorisations racine.

    ``` BASH
    sudo kill -15 `pidof hv_kvp_daemon`
    ```

1. Pour vérifier que tous les processus de `hv_kvp_daemon` ont disparu, exécutez :

    ```
    ps -ef | hv
    ```

1. Pour redémarrer le démon, exécutez le démon en tant que racine :

    ``` BASH
    sudo hv_kvp_daemon
    ``` 

1. Pour vérifier que le processus de `hv_kvp_daemon` est listé avec un nouvel ID de processus, exécutez :

    ```
    ps -ef | hv
    ```

## <a name="keep-integration-services-up-to-date"></a>Tenir à jour les services d’intégration

Nous vous recommandons de tenir à jour les services d’intégration pour obtenir les meilleures performances et les fonctionnalités les plus récentes pour vos machines virtuelles. Cela se produit pour la plupart des invités Windows par défaut s’ils sont configurés pour obtenir des mises à jour importantes de Windows Update. Les invités Linux utilisant les noyaux actuels recevront les composants d’intégration les plus récents lors de la mise à jour du noyau.

**Pour les machines virtuelles qui s’exécutent sur des hôtes Windows 10 :**

> [!NOTE]
> Le fichier image vmguest. ISO n’est pas inclus dans Hyper-V sur Windows 10, car il n’est plus nécessaire.

| Invité  | Mécanisme de mise à jour | Remarques |
|:---------|:---------|:---------|
| Windows 10 | Windows Update | |
| Windows 8.1 | Windows Update | |
| Windows 8 | Windows Update | Nécessite le service d’intégration Échange de données.* |
| Windows 7 | Windows Update | Nécessite le service d’intégration Échange de données.* |
| Windows Vista (SP2) | Windows Update | Nécessite le service d’intégration Échange de données.* |
| - | | |
| Windows Server 2016 | Windows Update | |
| Windows Server, canal semi-annuel | Windows Update | |
| Windows Server 2012 R2 | Windows Update | |
| Windows Server 2012 | Windows Update | Nécessite le service d’intégration Échange de données.* |
| Windows Server 2008 R2 (SP1) | Windows Update | Nécessite le service d’intégration Échange de données.* |
| Windows Server 2008 (SP2) | Windows Update | Prise en charge étendue uniquement dans Windows Server 2016 ([en savoir plus](https://support.microsoft.com/lifecycle?p1=12925)). |
| Windows Home Server 2011 | Windows Update | Ne sera pas pris en charge dans Windows Server 2016 ([en savoir plus](https://support.microsoft.com/lifecycle?p1=15820)). |
| Windows Small Business Server 2011 | Windows Update | Pas de support standard ([en savoir plus](https://support.microsoft.com/lifecycle?p1=15817)). |
| - | | |
| Invités Linux | gestionnaire de package | Integration Services pour Linux est intégré à distribution, mais des mises à jour facultatives peuvent être disponibles. ******** |

\* si le service d’intégration échange de données ne peut pas être activé, les services d’intégration pour ces invités sont disponibles dans le [Centre de téléchargement](https://support.microsoft.com/kb/3071740) en tant que fichier CAB. Les instructions relatives à l’application d’un fichier CAB sont disponibles dans ce billet de [blog](https://blogs.technet.com/b/virtualization/archive/2015/07/24/integration-components-available-for-virtual-machines-not-connected-to-windows-update.aspx).

**Pour les machines virtuelles qui s’exécutent sur des hôtes Windows 8.1 :**

| Invité  | Mécanisme de mise à jour | Remarques |
|:---------|:---------|:---------|
| Windows 10 | Windows Update | |
| Windows 8.1 | Windows Update | |
| Windows 8 | Disque des services d’intégration | Consultez les [instructions](#install-or-update-integration-services)ci-dessous. |
| Windows 7 | Disque des services d’intégration | Consultez les [instructions](#install-or-update-integration-services)ci-dessous. |
| Windows Vista (SP2) | Disque des services d’intégration | Consultez les [instructions](#install-or-update-integration-services)ci-dessous. |
| Windows XP (SP2, SP3) | Disque des services d’intégration | Consultez les [instructions](#install-or-update-integration-services)ci-dessous. |
| - | | |
| Windows Server 2016 | Windows Update | |
| Windows Server, canal semi-annuel | Windows Update | |
| Windows Server 2012 R2 | Windows Update | |
| Windows Server 2012 | Disque des services d’intégration | Consultez les [instructions](#install-or-update-integration-services)ci-dessous. |
| Windows Server 2008 R2 | Disque des services d’intégration | Consultez les [instructions](#install-or-update-integration-services)ci-dessous. |
| Windows Server 2008 (SP2) | Disque des services d’intégration | Consultez les [instructions](#install-or-update-integration-services)ci-dessous. |
| Windows Home Server 2011 | Disque des services d’intégration | Consultez les [instructions](#install-or-update-integration-services)ci-dessous. |
| Windows Small Business Server 2011 | Disque des services d’intégration | Consultez les [instructions](#install-or-update-integration-services)ci-dessous. |
| Windows Server 2003 R2 (SP2) | Disque des services d’intégration | Consultez les [instructions](#install-or-update-integration-services)ci-dessous. |
| Windows Server 2003 (SP2) | Disque des services d’intégration | Consultez les [instructions](#install-or-update-integration-services)ci-dessous. |
| - | | |
| Invités Linux | gestionnaire de package | Integration Services pour Linux est intégré à distribution, mais des mises à jour facultatives peuvent être disponibles. ** |


**Pour les machines virtuelles qui s’exécutent sur des hôtes Windows 8 :**

| Invité  | Mécanisme de mise à jour | Remarques |
|:---------|:---------|:---------|
| Windows 8.1 | Windows Update | |
| Windows 8 | Disque des services d’intégration | Consultez les [instructions](#install-or-update-integration-services)ci-dessous. |
| Windows 7 | Disque des services d’intégration | Consultez les [instructions](#install-or-update-integration-services)ci-dessous. |
| Windows Vista (SP2) | Disque des services d’intégration | Consultez les [instructions](#install-or-update-integration-services)ci-dessous. |
| Windows XP (SP2, SP3) | Disque des services d’intégration | Consultez les [instructions](#install-or-update-integration-services)ci-dessous. |
| - | | |
| Windows Server 2012 R2 | Windows Update | |
| Windows Server 2012 | Disque des services d’intégration | Consultez les [instructions](#install-or-update-integration-services)ci-dessous. |
| Windows Server 2008 R2 | Disque des services d’intégration | Consultez les [instructions](#install-or-update-integration-services)ci-dessous.|
| Windows Server 2008 (SP2) | Disque des services d’intégration | Consultez les [instructions](#install-or-update-integration-services)ci-dessous. |
| Windows Home Server 2011 | Disque des services d’intégration | Consultez les [instructions](#install-or-update-integration-services)ci-dessous. |
| Windows Small Business Server 2011 | Disque des services d’intégration | Consultez les [instructions](#install-or-update-integration-services)ci-dessous. |
| Windows Server 2003 R2 (SP2) | Disque des services d’intégration | Consultez les [instructions](#install-or-update-integration-services)ci-dessous. |
| Windows Server 2003 (SP2) | Disque des services d’intégration | Consultez les [instructions](#install-or-update-integration-services)ci-dessous. |
| - | | |
| Invités Linux | gestionnaire de package | Integration Services pour Linux est intégré à distribution, mais des mises à jour facultatives peuvent être disponibles. ** |

Pour plus d’informations sur les invités Linux, consultez [machines virtuelles Linux et FreeBSD prises en charge pour Hyper-V sur Windows](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows).

## <a name="install-or-update-integration-services"></a>Installer ou mettre à jour les services d’intégration

Pour les ordinateurs hôtes antérieurs à Windows Server 2016 et Windows 10, vous devez installer ou mettre à jour manuellement les services d’intégration dans les systèmes d’exploitation invités. 
  
1.  Ouvrez le Gestionnaire Hyper-V. Dans le menu outils de Gestionnaire de serveur, cliquez sur **Gestionnaire Hyper-V**.  
  
2.  Connectez-vous à l’ordinateur virtuel. Cliquez avec le bouton droit sur l’ordinateur virtuel, puis cliquez sur **se connecter**.  
  
3.  Dans le menu Action de l’outil Connexion à un ordinateur virtuel, cliquez sur **Insérer le disque d’installation des services d’intégration**. Cette action charge le disque d’installation dans le lecteur de DVD virtuel. Selon le système d’exploitation invité, vous devrez peut-être démarrer manuellement l’installation.  
  
4.  Quand l’installation est terminée, tous les services d’intégration peuvent être utilisés.

Ces étapes ne peuvent pas être automatisées ou effectuées dans une session Windows PowerShell pour les machines virtuelles en ligne. Vous pouvez les appliquer à des images VHDX hors connexion ; [consultez ce](https://blogs.technet.microsoft.com/virtualization/2013/04/18/how-to-install-integration-services-when-the-virtual-machine-is-not-running/)billet de blog.
