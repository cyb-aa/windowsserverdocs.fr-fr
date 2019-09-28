---
ms.assetid: 75cc1d24-fa2f-45bd-8f3b-1bbd4a1aead0
title: Exigences et meilleures pratiques en matière de mise à jour adaptée aux clusters
ms.prod: windows-server
ms.topic: article
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 08/06/2018
description: Configuration requise pour l’utilisation de la mise à jour adaptée aux clusters pour installer les mises à jour sur les clusters exécutant Windows Server.
ms.openlocfilehash: 501969fad2455195bca485bd8124911d6d75378e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361324"
---
# <a name="cluster-aware-updating-requirements-and-best-practices"></a>Exigences et meilleures pratiques en matière de mise à jour adaptée aux clusters

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette section décrit les exigences et les dépendances nécessaires à l’utilisation de la [mise à jour adaptée aux clusters](cluster-aware-updating.md) pour appliquer des mises à jour à un cluster de basculement exécutant Windows Server.

> [!NOTE]  
> Vous devrez peut-être vérifier indépendamment que votre environnement de cluster est prêt à appliquer les mises à jour si vous utilisez un plug-no__t-0in autre que **Microsoft. WindowsUpdatePlugin**. Si vous utilisez un plug-in non @ no__t-0Microsoft @ no__t-1Dans, contactez le serveur de publication pour plus d’informations. Pour plus d’informations sur plug @ no__t-0ins, consultez fonctionnement de [Plug @ no__t-2ins](cluster-aware-updating-plug-ins.md).   

## <a name="BKMK_REQ_CLUS"></a>Installer la fonctionnalité de clustering de basculement et les outils de clustering de basculement  
La Mise à jour adaptée aux clusters demande une installation de la fonctionnalité et des outils de clustering de basculement. Les outils de clustering avec basculement incluent les outils de la mise à jour adaptée aux clusters @no__t -0clusterawareupdating. dll @ no__t-1, les applets de commande de clustering de basculement et les autres composants nécessaires pour les opérations de mise à jour Pour connaître les étapes d’installation de la fonctionnalité de clustering de basculement, voir la page relative à l’ [installation de la fonctionnalité et des outils de clustering de basculement](create-failover-cluster.md#install-the-failover-clustering-feature).  

La configuration requise pour l’installation des outils de clustering avec basculement varie selon que la mise à jour adaptée aux clusters coordonne ou non les mises à jour en tant que rôle en cluster sur le cluster de basculement \(BY à l’aide du mode Self @ no__t-1updating @ no__t-2 ou d’un ordinateur distant. Le mode Self @ no__t-0updating de la mise à jour adaptée aux clusters nécessite également l’installation du rôle en cluster de la mise à jour adaptée aux clusters sur le cluster de basculement à l’aide des outils de la mise à jour    

Le tableau suivant résume les conditions requises par l'installation de la fonctionnalité de Mise à jour adaptée aux clusters pour les deux modes de mise à jour possibles.  

|Composant installé|Mode Self @ no__t-0updating|Mode @ no__t-0updating distant|  
|-----------------------|-----------------------|-------------------------|  
|Fonctionnalité de clustering de basculement|Obligatoire sur tous les nœuds de cluster|Obligatoire sur tous les nœuds de cluster|  
|Outils de clustering de basculement|Obligatoire sur tous les nœuds de cluster|-Requis sur l’ordinateur distant @ no__t-0updating<br />-Requis sur tous les nœuds de cluster pour exécuter l’applet de commande [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps)|  
|Rôle en cluster de la Mise à jour adaptée aux clusters|Obligatoire|Non requis|  

## <a name="obtain-an-administrator-account"></a>Obtenir un compte d'administrateur  
Vous devez satisfaire aux exigences suivantes en matière d'administration pour pouvoir utiliser les fonctionnalités de la Mise à jour adaptée aux clusters.  

-   Pour afficher un aperçu ou appliquer des actions de mise à jour à l’aide de l’interface utilisateur de la mise à jour adaptée aux clusters @no__t 0UI @ no__t-1 ou des applets de commande de mise à jour adaptée aux clusters, vous devez utiliser un compte de domaine disposant des droits d’administrateur local et des autorisations sur tous les nœuds de cluster Si le compte ne dispose pas de privilèges suffisants sur chaque nœud, la fenêtre mise à jour adaptée aux clusters vous invite à fournir les informations d’identification nécessaires quand vous effectuez ces actions. Pour utiliser les applets de commande de [mise à jour adaptée aux clusters](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps) , vous pouvez fournir les informations d’identification nécessaires sous la forme d’un paramètre d’applet de commande.  

-   Si vous utilisez la mise à jour adaptée aux clusters en mode no__t-0updating à distance lorsque vous êtes connecté avec un compte qui ne dispose pas de droits d’administrateur local et d’autorisations sur les nœuds du cluster, vous devez exécuter les outils de la mise à jour adaptée aux clusters en tant qu’administrateur en utilisant un compte d’administrateur local sur la mise à jour Ou en utilisant un compte disposant du droit d’utilisateur **emprunter l’identité d’un client après l’authentification** . 

-   Pour exécuter la mise à jour adaptée aux clusters Best Practices Analyzer, vous devez utiliser un compte disposant de privilèges d’administration sur les nœuds de cluster et des privilèges d’administrateur local sur l’ordinateur utilisé pour exécuter l’applet de commande [test-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) ou pour analyser la mise à jour de cluster. préparation à l’aide de la fenêtre mise à jour adaptée aux clusters. Pour plus d'informations, voir [Tester l'état de préparation aux mises à jour de cluster](#BKMK_BPA).  

## <a name="verify-the-cluster-configuration"></a>Vérifier la configuration du cluster  
Voici les conditions requises généralement pour qu'un cluster de basculement puisse prendre en charge les mises à jour à l'aide de la Mise à jour adaptée aux clusters. Des conditions requises supplémentaires en matière de configuration pour l’administration à distance des nœuds sont répertoriées dans [Configurer les nœuds pour l'administration à distance](#BKMK_NODE_CONFIG) plus loin dans cette rubrique.  

-   Suffisamment de nœuds de cluster doivent être en ligne pour permettre au cluster de disposer d'un quorum.  

-   Tous les nœuds de cluster doivent se trouver dans le même domaine Active Directory.  

-   Le nom du cluster doit être résolu sur le réseau via DNS.  

-   Si la mise à jour adaptée aux clusters est utilisée en mode @ no__t-0updating à distance, l’ordinateur coordinateur de mise à jour doit disposer d’une connectivité réseau avec les nœuds de cluster de basculement et il doit se trouver dans le même domaine de Active Directory que le cluster de basculement.  

-   Le service de cluster doit s'exécuter sur tous les nœuds de cluster. Par défaut, ce service est installé sur tous les nœuds de cluster et est configuré pour démarrer automatiquement.  

-   Pour utiliser des scripts PowerShell pre @ no__t-0update ou postérieurs à @ no__t-1Update pendant une exécution de mise à jour de la mise à jour adaptée aux clusters, vérifiez que les scripts sont installés sur tous les nœuds de cluster ou qu’ils sont accessibles à tous les nœuds, par exemple, sur un partage de fichiers réseau hautement disponible. Si les scripts sont enregistrés sur un partage de fichiers réseau, configurez le dossier avec une autorisation d'accès en lecture pour le groupe Tout le monde.  

## <a name="BKMK_NODE_CONFIG"></a>Configurer les nœuds pour la gestion à distance  
Pour utiliser la mise à jour adaptée aux clusters, tous les nœuds du cluster doivent être configurés pour la gestion à distance. Par défaut, la seule tâche que vous devez effectuer pour configurer les nœuds pour la gestion à distance consiste à [activer une règle de pare-feu pour autoriser les redémarrages automatiques](#BKMK_FW). 

Le tableau suivant répertorie les conditions requises pour la gestion à distance complète, au cas où votre environnement divergent des paramètres par défaut.

Ces conditions requises s'ajoutent aux exigences d'[installation de la fonctionnalité et des outils de clustering de basculement](#BKMK_REQ_CLUS), ainsi qu'aux prescriptions générales en matière de clustering décrites dans les sections précédentes de cette rubrique.  

|Condition requise|État par défaut|Mode Self @ no__t-0updating|Mode @ no__t-0updating distant|  
|---------------|---|-----------------------|-------------------------|  
|[Activer une règle de pare-feu pour autoriser les redémarrages automatiques](#BKMK_FW)|Désactivé|Obligatoire sur tous les nœuds de cluster si un pare-feu est en cours d'utilisation|Obligatoire sur tous les nœuds de cluster si un pare-feu est en cours d'utilisation|  
|[Activer Windows Management Instrumentation](#BKMK_WMI)|Enabled|Obligatoire sur tous les nœuds de cluster|Obligatoire sur tous les nœuds de cluster|  
|[Activer Windows PowerShell 3,0 ou 4,0 et la communication à distance Windows PowerShell](#BKMK_PS)|Enabled|Obligatoire sur tous les nœuds de cluster|Obligatoire sur tous les nœuds de cluster pour exécuter ce qui suit :<br /><br />-L’applet de commande [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps)<br />-PowerShell pre @ no__t-0update et après @ no__t-1Update scripts lors d’une exécution de mise à jour<br />-Tests de préparation à la mise à jour du cluster à l’aide de la fenêtre mise à jour adaptée aux clusters ou de l’applet de commande Windows PowerShell [test @ no__t-1CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps)|  
|[Installer .NET Framework 4,6 ou 4,5](#BKMK_NET)|Enabled|Obligatoire sur tous les nœuds de cluster|Obligatoire sur tous les nœuds de cluster pour exécuter ce qui suit :<br /><br />-L’applet de commande [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps)<br />-PowerShell pre @ no__t-0update et après @ no__t-1Update scripts lors d’une exécution de mise à jour<br />-Tests de préparation à la mise à jour du cluster à l’aide de la fenêtre mise à jour adaptée aux clusters ou de l’applet de commande Windows PowerShell [test @ no__t-1CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps)|  

### <a name="BKMK_FW"></a>Activer une règle de pare-feu pour autoriser les redémarrages automatiques  
Pour autoriser les redémarrages automatiques après l’application des mises à jour \(if l’installation d’une mise à jour nécessite un redémarrage de @ no__t-1, si le pare-feu Windows ou un pare-feu non-no__t-2Microsoft est utilisé sur les nœuds de cluster, une règle de pare-feu doit être activée sur chaque nœud qui autorise le trafic suivant :  

-   Protocole : TCP  

-   Sens : entrant  

-   Programme : wininit.exe  

-   Ports : Ports dynamiques RPC  

-   Profil : domaine.  

Si le Pare-feu Windows est utilisé sur les nœuds de cluster, vous pouvez activer le groupe de règles du Pare-feu Windows **Arrêt à distance** sur chaque nœud de cluster. Lorsque vous utilisez la fenêtre mise à jour adaptée aux clusters pour appliquer les mises à jour et configurer les options Self @ no__t-0updating, le groupe de règles du pare-feu Windows d' **arrêt à distance** est automatiquement activé sur chaque nœud de cluster.  

> [!NOTE]  
> Le groupe de règles du Pare-feu Windows **Remote Shutdown** ne peut pas être activé quand il est en conflit avec des paramètres de stratégie de groupe configurés pour le Pare-feu Windows.    

Le groupe de règles de pare-feu d' **arrêt à distance** est également activé en spécifiant le paramètre **– enablefirewallrules durant** lors de l’exécution des cmdlets de la mise à jour adaptée aux clusters [Add-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole?view=win10-ps), [Invoke-CauRun](https://docs.microsoft.com/powershell/module/clusterawareupdating/Invoke-CauRun?view=win10-ps)et [SetCauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Set-CauClusterRole?view=win10-ps).  

L’exemple PowerShell suivant montre une méthode supplémentaire pour activer les redémarrages automatiques sur un nœud de cluster.  

```PowerShell  
Set-NetFirewallRule -Group "@firewallapi.dll,-36751" -Profile Domain -Enabled true  
```  

### <a name="BKMK_WMI"></a>Activer les Windows Management Instrumentation (WMI) 
Tous les nœuds de cluster doivent être configurés pour la gestion à distance à l’aide de Windows Management Instrumentation \(WMI @ no__t-1. Ceci est activé par défaut.  

Pour activer manuellement l'administration à distance, procédez comme suit :  

1.  Dans la console Services, démarrez le service **Gestion à distance de Windows** , puis définissez son type de démarrage comme étant **Automatique**.  

2.  Exécutez l’applet de commande [Set-WSManQuickConfig](https://docs.microsoft.com/powershell/module/Microsoft.WsMan.Management/Set-WSManQuickConfig?view=powershell-6) ou exécutez la commande suivante à partir d’une invite de commandes avec élévation de privilèges :  

    ```PowerShell  
    winrm quickconfig -q  
    ```  

Pour prendre en charge la communication à distance WMI, si le pare-feu Windows est en cours d’utilisation sur les nœuds de cluster, la règle de pare-feu entrante pour **Windows Remote Management \(http @ no__t-2in @ no__t-3** doit être activée sur chaque nœud.  Par défaut, cette règle est activée.  

### <a name="BKMK_PS"></a>Activer Windows PowerShell et la communication à distance Windows PowerShell  
Pour activer le mode Self @ no__t-0updating et certaines fonctionnalités de la mise à jour adaptée aux clusters en mode @ no__t-1updating distant, PowerShell doit être installé et activé pour exécuter les commandes à distance sur tous les nœuds du cluster. Par défaut, PowerShell est installé et activé pour la communication à distance.  

Pour activer la communication à distance PowerShell, utilisez l’une des méthodes suivantes :  

-   Exécutez l’applet [de commande Enable-PSRemoting](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Enable-PSRemoting) .  

-   Configurez un paramètre de stratégie de groupe de domaine @ no__t-0level pour Windows Remote Management \(WinRM @ no__t-2.  

Pour plus d’informations sur l’activation de la communication [à distance PowerShell, consultez à propos des exigences à distance](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_remote_requirements?view=powershell-6).  

### <a name="BKMK_NET"></a>Installer .NET Framework 4,6 ou 4,5  
Pour activer le mode auto-no__t-0updating et certaines fonctionnalités de la mise à jour adaptée aux clusters en mode @ no__t-1updating distant, .NET Framework 4,6 ou .NET Framework 4,5 (sur Windows Server 2012 R2) doit être installé sur tous les nœuds du cluster. Par défaut, le .NET Framework est installé.  

Pour installer .NET Framework 4,6 (ou 4,5) à l’aide de PowerShell s’il n’est pas déjà installé, utilisez la commande suivante :

```PowerShell
Install-WindowsFeature -Name NET-Framework-45-Core
```

## <a name="BKMK_BEST_PRAC"></a>Recommandations relatives aux meilleures pratiques pour l’utilisation de la mise à jour adaptée aux clusters 

### <a name="BKMK_BP_WUA"></a>Recommandations pour l’application des mises à jour Microsoft

Lorsque vous commencez à utiliser la mise à jour adaptée aux clusters pour appliquer les mises à jour avec le plug-in **Microsoft. WindowsUpdatePlugin** par défaut, vous ne pouvez plus utiliser d’autres méthodes pour installer les mises à jour logicielles de Microsoft sur les nœuds du cluster.  

> [!CAUTION]  
> La combinaison de la mise à jour adaptée aux méthodes qui met à jour automatiquement des nœuds individuels @no__t 0ON une planification à temps fixe @ no__t-1 peut entraîner des résultats imprévisibles, notamment des interruptions de service et des temps d’arrêt non planifiés.  

Nous vous conseillons de suivre les recommandations suivantes :  

-   Pour des résultats optimaux, nous vous recommandons de désactiver la mise à jour automatique sur les nœuds de cluster, par exemple, via les paramètres de l'application Mises à jour automatiques du Panneau de configuration, ou via les paramètres configurés à l'aide de la stratégie de groupe.  

    > [!CAUTION]  
    > L'installation automatique des mises à jour sur les nœuds de cluster peut interférer avec l'installation des mises à jour par la Mise à jour adaptée aux clusters et peut provoquer des défaillances de la Mise à jour adaptée aux clusters.  

    S'ils sont nécessaires, les paramètres suivants des mises à jour automatiques sont compatibles avec la Mise à jour adaptée aux clusters, car l'administrateur peut contrôler le moment de l'installation des mises à jour :  

    -   Paramètres de notification avant le téléchargement des mises à jour et de notification avant l'installation  

    -   Paramètres de téléchargement automatique des mises à jour et de notification avant l'installation  

    Toutefois, si les mises à jour automatiques téléchargent des mises à jour pendant qu'une Exécution de mise à jour est effectuée par la Mise à jour adaptée aux clusters, l'Exécution de mise à jour risque de durer plus longtemps.  

-   Ne configurez pas un système de mise à jour tel que Windows Server Update Services \(WSUS @ no__t-1 pour appliquer automatiquement les mises à jour \(On une planification à temps fixe @ no__t-3 aux nœuds de cluster.  

-   Tous les nœuds de cluster doivent être uniformément configurés pour utiliser la même source de mise à jour, par exemple, un serveur WSUS, Windows Update ou Microsoft Update.  

-   Si vous utilisez un système de gestion de la configuration sur des ordinateurs du réseau, excluez les nœuds de cluster de toutes les mises à jour obligatoires ou automatiques. Microsoft System Center Configuration Manager 2007 et Microsoft System Center Virtual Machine Manager 2008 constituent des exemples de systèmes de gestion de la configuration.  

-   Si des serveurs de distribution de logiciels internes @no__t 0for-exemple, les serveurs WSUS @ no__t-1 sont utilisés pour contenir et déployer les mises à jour, assurez-vous que ces serveurs identifient correctement les mises à jour approuvées pour les nœuds de cluster.  

#### <a name="BKMK_PROXY"></a>Appliquer des mises à jour Microsoft dans les scénarios de succursale  
Pour télécharger des mises à jour Microsoft à partir de Microsoft Update ou Windows Update vers des nœuds de cluster dans certains scénarios impliquant des filiales, vous devrez peut-être configurer les paramètres de proxy du compte système local sur chaque nœud. Cela peut être le cas, par exemple, si les clusters de vos filiales ont accès à Microsoft Update ou Windows Update pour télécharger les mises à jour à l'aide d'un serveur proxy local.  

Si nécessaire, configurez les paramètres de proxy WinHTTP sur chaque nœud pour spécifier un serveur proxy local et configurer des exceptions d’adresses locales \(that est, une liste de contournement pour les adresses locales @ no__t-1. Pour ce faire, vous pouvez exécuter la commande suivante sur chaque nœud de cluster à partir d'une invite de commandes avec élévation de privilèges :  

```  
netsh winhttp set proxy <ProxyServerFQDN >:<port> "<local>"  
```  

où <*ProxyServerFQDN*> est le nom de domaine complet pour le serveur proxy et le*port*de < > est le port sur lequel communiquer (généralement le port 443).  

Par exemple, pour configurer les paramètres de proxy WinHTTP pour le compte système local en spécifiant le serveur proxy *MyProxy.contoso.com*, avec les exceptions de port 443 et d’adresse locale, tapez la commande suivante :  

```  
netsh winhttp set proxy MyProxy.CONTOSO.com:443 "<local>"  
```  

### <a name="BKMK_BP_HF"></a>Recommandations relatives à l’utilisation de Microsoft. HotfixPlugin  

-   Nous vous recommandons de configurer les autorisations du dossier racine de correctifs logiciels et du fichier de configuration de correctif logiciel pour restreindre l'accès en écriture aux administrateurs locaux sur les ordinateurs qui servent à stocker ces fichiers. Cela permet d'éviter la falsification de ces fichiers par des utilisateurs non autorisés qui peuvent compromettre la fonctionnalité du cluster de basculement durant l'application des correctifs logiciels.  

-   Pour garantir l’intégrité des données pour le bloc de message serveur \(SMB @ no__t-1 connexions utilisées pour accéder au dossier racine de correctifs logiciels, vous devez configurer le chiffrement SMB dans le dossier partagé SMB, s’il est possible de le configurer. **Microsoft.HotfixPlugin** nécessite qu'une signature SMB ou un chiffrement SMB soit configuré pour garantir l'intégrité des données des connexions SMB. 

    Pour plus d’informations, consultez [restreindre l’accès au dossier racine de correctifs logiciels et au fichier de configuration de correctif logiciel](cluster-aware-updating-plug-ins.md#BKMK_ACL).

### <a name="additional-recommendations"></a>Recommandations supplémentaires  

-   Pour éviter toute interférence avec une Exécution de mise à jour de la Mise à jour adaptée aux clusters, ne planifiez aucun changement de mot de passe pour les objets noms de cluster et les objets ordinateurs virtuels durant les fenêtres de maintenance planifiée.  

-   Vous devez définir les autorisations appropriées sur les scripts pre @ no__t-0update et postérieurs à @ no__t-1Update enregistrés sur les dossiers partagés sur le réseau afin d’éviter la falsification potentielle de ces fichiers par des utilisateurs non autorisés.  

-   Pour configurer la mise à jour adaptée aux clusters en mode auto @ no__t-0updating, un objet ordinateur virtuel \(VCO @ no__t-2 pour le rôle en cluster de la mise à jour adaptée aux clusters doit être créé dans Active Directory. La Mise à jour adaptée aux clusters peut créer cet objet automatiquement au moment où le rôle en cluster de la Mise à jour adaptée aux clusters est ajouté, si le cluster de basculement dispose des autorisations suffisantes. Toutefois, en raison des stratégies de sécurité de certaines organisations, il est parfois nécessaire de prédéfinir l'objet dans Active Directory. Pour connaître la procédure à suivre, voir les [étapes de prédéfinition d’un compte pour un rôle en cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731002\(v=ws.10\)#steps-for-prestaging-the-cluster-name-account).  

-   Pour enregistrer et réutiliser les paramètres d'Exécution de mise à jour sur les clusters de basculement ayant des besoins de mise à jour informatique similaires, vous pouvez créer des profils d'Exécution de mise à jour. En outre, selon le mode de mise à jour, vous pouvez enregistrer et gérer les profils d'Exécution de mise à jour sur un partage de fichiers accessible à l'ensemble des ordinateurs coordinateurs de mise à jour distants ou clusters de basculement. Pour plus d’informations, consultez [Options avancées et mise à jour des profils d’exécution pour la mise à jour adaptée aux clusters](cluster-aware-updating-options.md).  

## <a name="BKMK_BPA"></a>Tester la disponibilité de la mise à jour du cluster  
Vous pouvez exécuter la mise à jour adaptée aux clusters Best Practices Analyzer \(BPA @ no__t-1 pour tester si un cluster de basculement et l’environnement réseau répondent à la plupart des exigences d’application des mises à jour logicielles par la mise à jour adaptée aux clusters. La plupart des tests vérifient que l’environnement est prêt à appliquer les mises à jour Microsoft à l’aide du plug-in par défaut @ no__t-0in, **Microsoft. WindowsUpdatePlugin**.  

> [!NOTE]  
> Vous devrez peut-être valider indépendamment que votre environnement de cluster est prêt à appliquer les mises à jour logicielles à l’aide d’un plug @ no__t-0in autre que **Microsoft. WindowsUpdatePlugin**. Si vous utilisez un plug-in non-no__t-0Microsoft @ no__t-1Dans, tel que celui fourni par le fabricant de votre matériel, contactez le serveur de publication pour plus d’informations.  

Vous pouvez exécuter l'outil BPA des deux façons suivantes :  

1.  Sélectionnez **Analyser la disponibilité de la mise à jour du cluster** dans la console de la Mise à jour adaptée aux clusters. Une fois que l’outil BPA a effectué les tests de préparation, un rapport test s’affiche. Si des problèmes sont détectés sur des nœuds de cluster, les problèmes et les nœuds où ils apparaissent sont identifiés pour que vous puissiez prendre des mesures correctives. Les tests peuvent durer plusieurs minutes.  

2.  Exécutez l’applet de commande [test-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup) . Vous pouvez exécuter l’applet de commande sur un ordinateur local ou distant sur lequel le module clustering de basculement pour Windows PowerShell (qui fait partie des outils de clustering de basculement) est installé. Vous pouvez également exécuter l'applet de commande sur un nœud du cluster de basculement.  

> [!NOTE]  
> -   Vous devez utiliser un compte disposant de privilèges d’administration sur les nœuds de cluster et des privilèges d’administrateur local sur l’ordinateur utilisé pour exécuter l’applet de commande **test @ no__t-1CauSetup** ou pour analyser la disponibilité de la mise à jour de cluster à l’aide de la prise en charge des clusters Mise à jour de la fenêtre. Pour exécuter les tests à l’aide de la fenêtre mise à jour adaptée aux clusters, vous devez avoir ouvert une session sur l’ordinateur avec les informations d’identification nécessaires.  
> -   Les tests partent du principe que les outils de la Mise à jour adaptée aux clusters utilisés pour obtenir un aperçu et appliquer les mises à jour de logiciels sont exécutés à partir du même ordinateur et avec les mêmes informations d'identification utilisateur que pour tester l'état de préparation aux mises à jour de cluster.  

> [!IMPORTANT]  
> Nous vous recommandons fortement de tester l'état de préparation aux mises à jour du cluster dans les situations suivantes :  
>   
> -   avant d'utiliser la Mise à jour adaptée aux clusters pour la première fois pour appliquer des mises à jour de logiciels ;  
> -   après avoir ajouté un nœud au cluster ou effectué d'autres changements sur le matériel du cluster, qui nécessitent l'exécution de l'Assistant Validation de cluster ;  
> -   Après avoir modifié une source de mise à jour, ou modifié les paramètres ou configurations de mise à jour \(other que la mise à jour adaptée aux clusters @ no__t-1 qui peut affecter l’application des mises à jour sur les nœuds.  

### <a name="tests-for-cluster-updating-readiness"></a>Tests de l'état de préparation aux mises à jour de cluster  
Le tableau ci-dessous présente les tests de l'état de préparation des mises à jour de cluster, certains problèmes fréquents et les étapes de résolution.  


|                                                      Tester                                                      |                                                                                                                                               Problèmes et impacts possibles                                                                                                                                               |                                                                                                                                                                                         Étapes de résolution                                                                                                                                                                                         |
|----------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                     Le cluster de basculement doit être disponible                                     |                                                                                       Impossible de résoudre le nom du cluster de basculement, ou un ou plusieurs nœuds de cluster ne sont pas accessibles. L'outil BPA ne peut pas exécuter les tests de l'état de préparation du cluster.                                                                                        |                                                                   -Vérifiez l’orthographe du nom du cluster spécifié lors de l’exécution de l’outil BPA.<br />-Assurez-vous que tous les nœuds du cluster sont en ligne et en cours d’exécution.<br />-Vérifiez que l’Assistant validation d’une configuration peut s’exécuter correctement sur le cluster de basculement.                                                                    |
|                    Les nœuds de cluster de basculement doivent être activés pour permettre l'administration à distance via WMI                    |                                                Un ou plusieurs nœuds de cluster de basculement ne sont pas activés pour la gestion à distance à l’aide de Windows Management Instrumentation \(WMI @ no__t-1. La Mise à jour adaptée aux clusters ne peut pas mettre à jour les nœuds de cluster s'ils ne sont pas configurés pour l'administration à distance.                                                 |                                                                                                  Assurez-vous que tous les nœuds de cluster de basculement sont activés pour l'administration à distance via WMI. Pour plus d'informations, consultez [Configurer les nœuds pour l'administration à distance](#BKMK_NODE_CONFIG) dans cette rubrique.                                                                                                   |
|                      La communication à distance PowerShell doit être activée sur chaque nœud de cluster de basculement                       |                                                           PowerShell n’est pas installé ou n’est pas activé pour la communication à distance sur un ou plusieurs nœuds de cluster de basculement. La mise à jour adaptée aux clusters ne peut pas être configurée pour le mode Self @ no__t-0updating ou utiliser certaines fonctionnalités en mode @ no__t-1updating distant.                                                            |                                                                                             Vérifiez que PowerShell est installé sur tous les nœuds de cluster et qu’il est activé pour la communication à distance.<br /><br />Pour plus d'informations, consultez [Configurer les nœuds pour l'administration à distance](#BKMK_NODE_CONFIG) dans cette rubrique.                                                                                             |
|                                            Version du cluster de basculement                                            |                                                                            Un ou plusieurs nœuds du cluster de basculement n’exécutent pas Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012. La Mise à jour adaptée aux clusters ne peut pas mettre à jour le cluster de basculement.                                                                             |                                                                   Vérifiez que le cluster de basculement spécifié lors de l’exécution de l’outil BPA exécute Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.<br /><br />Pour plus d'informations, consultez [Vérifier la configuration du cluster](#BKMK_REQ_CLUS) dans cette rubrique.                                                                   |
| Les versions nécessaires du .NET Framework et de Windows PowerShell doivent être installées sur tous les nœuds de cluster de basculement |                                                                                              .NET Framework 4,6, 4,5 ou Windows PowerShell n’est pas installé sur un ou plusieurs nœuds de cluster. Certaines fonctionnalités de la Mise à jour adaptée aux clusters risquent de ne pas fonctionner.                                                                                              |                                                                            Assurez-vous que .NET Framework 4,6 ou 4,5 et Windows PowerShell sont installés sur tous les nœuds de cluster, si nécessaire.<br /><br />Pour plus d'informations, consultez [Configurer les nœuds pour l'administration à distance](#BKMK_NODE_CONFIG) dans cette rubrique.                                                                             |
|                           Le service de cluster doit s'exécuter sur tous les nœuds de cluster                           |                                                                                                            Le service de cluster ne s'exécute pas sur un ou plusieurs nœuds. La Mise à jour adaptée aux clusters ne peut pas mettre à jour le cluster de basculement.                                                                                                             |                        -Assurez-vous que le service de cluster \(clussvc @ no__t-1 est démarré sur tous les nœuds du cluster et qu’il est configuré pour démarrer automatiquement.<br />-Vérifiez que l’Assistant validation d’une configuration peut s’exécuter correctement sur le cluster de basculement.<br /><br />Pour plus d'informations, consultez [Vérifier la configuration du cluster](#BKMK_REQ_CLUS) dans cette rubrique.                         |
|     Les mises à jour automatiques ne doivent pas être configurées pour installer automatiquement les mises à jour sur un nœud de cluster de basculement     |                                           Sur un nœud de cluster de basculement au moins, les mises à jour automatiques sont configurées pour installer automatiquement les mises à jour Microsoft. L'association de la Mise à jour adaptée aux clusters à d'autres méthodes de mise à jour peut entraîner des temps d'arrêt imprévus ou des résultats imprévisibles.                                            |                                                     Si la fonctionnalité Windows Update est configurée pour les mises à jour automatiques sur un ou plusieurs nœuds de cluster, assurez-vous que les mises à jour automatiques ne sont pas configurées pour installer automatiquement les mises à jour.<br /><br />Pour plus d'informations, voir [Recommandations pour l'application des mises à jour Microsoft](#BKMK_BP_WUA).                                                     |
|                          Les nœuds de cluster de basculement doivent utiliser la même source de mise à jour                          |                                                    Un ou plusieurs nœuds de cluster de basculement sont configurés pour utiliser une autre source de mise à jour pour les mises à jour Microsoft que le reste des nœuds. Les mises à jour risquent de ne pas être appliquées de manière uniforme sur les nœuds de cluster par la Mise à jour adaptée aux clusters.                                                    |                                                                        Assurez-vous que chaque nœud de cluster est configuré pour utiliser la même source de mise à jour, par exemple, un serveur WSUS, Windows Update ou Microsoft Update.<br /><br />Pour plus d'informations, voir [Recommandations pour l'application des mises à jour Microsoft](#BKMK_BP_WUA).                                                                         |
|       Une règle de pare-feu qui permet l'arrêt à distance doit être activée sur chaque nœud du cluster de basculement       |                 Un ou plusieurs nœuds de cluster de basculement n'ont pas de règle de pare-feu activée qui permet l'arrêt à distance, ou un paramètre de stratégie de groupe empêche cette règle d'être activée. Une Exécution de mise à jour qui applique des mises à jour nécessitant le redémarrage automatique des nœuds risque de ne pas s'effectuer correctement.                  |                                                                    Si le pare-feu Windows ou un pare-feu non-no__t-0Microsoft est utilisé sur les nœuds de cluster, configurez une règle de pare-feu qui autorise l’arrêt à distance.<br /><br />Pour plus d'informations, voir [Activer une règle de pare-feu pour permettre les redémarrages automatiques](#BKMK_FW) dans cette rubrique.                                                                    |
|          Le paramètre de serveur proxy de chaque nœud de cluster de basculement doit être défini en fonction d'un serveur proxy local          |                             Un ou plusieurs nœuds de cluster de basculement ont une configuration de serveur proxy incorrecte.<br /><br />Si un serveur proxy local est en cours d'utilisation, le paramètre de serveur proxy de chaque nœud doit être configuré correctement pour permettre au cluster d'accéder à Microsoft Update ou Windows Update.                              |                                            Assurez-vous que les paramètres de proxy WinHTTP de chaque nœud de cluster sont définis en fonction d'un serveur proxy local, le cas échéant. Si aucun serveur proxy n'est utilisé dans votre environnement, cet avertissement peut être ignoré.<br /><br />Pour plus d'informations, consultez [Apply updates in branch office scenarios](#BKMK_PROXY) dans cette rubrique.                                            |
|        Le rôle en cluster de la mise à jour adaptée aux clusters doit être installé sur le cluster de basculement pour activer le mode auto-no__t-0updating        |                                                                                                   Le rôle en cluster de la Mise à jour adaptée aux clusters n'est pas installé sur ce cluster de basculement. Ce rôle est requis pour le cluster Self @ no__t-0updating.                                                                                                   |      Pour utiliser la mise à jour adaptée aux clusters en mode auto @ no__t-0updating, ajoutez le rôle en cluster de la mise à jour adaptée aux clusters sur le cluster de basculement de l’une des manières suivantes :<br /><br />-Exécutez l’applet de commande PowerShell [Add-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole) .<br />-Sélectionnez l’action **configurer les options no__t-1updating du cluster** dans la fenêtre mise à jour adaptée aux clusters.      |
|         Le rôle en cluster de la mise à jour adaptée aux clusters doit être activé sur le cluster de basculement pour activer le mode auto-no__t-0updating         | Le rôle en cluster de la Mise à jour adaptée aux clusters est désactivé. Par exemple, le rôle en cluster de la mise à jour adaptée aux clusters n’est pas installé ou a été désactivé à l’aide de l’applet de commande PowerShell [Disable @ no__t-1CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole) . Ce rôle est requis pour le cluster Self @ no__t-0updating. | Pour utiliser la mise à jour adaptée aux clusters en mode auto @ no__t-0updating, activez le rôle en cluster de la mise à jour adaptée aux clusters sur ce cluster de basculement de l’une des manières suivantes :<br /><br />-Exécutez l’applet de commande PowerShell [Enable-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole) .<br />-Sélectionnez l’action **configurer les options no__t-1updating du cluster** dans la fenêtre mise à jour adaptée aux clusters. |
|      Le plug @ no__t-0in configuré pour la mise à jour adaptée aux clusters pour le mode Self @ no__t-1updating doit être inscrit sur tous les nœuds de cluster de basculement      |                                                              Le rôle en cluster de la mise à jour adaptée aux clusters sur un ou plusieurs nœuds de ce cluster de basculement ne peut pas accéder au module plug @ no__t-0in de la mise à jour adaptée aux clusters configuré dans les options Self @ no__t-1updating. Une exécution Self @ no__t-0updating peut échouer.                                                              |           -Assurez-vous que le plug @ no__t-0in configuré pour la mise à jour adaptée aux clusters est installé sur tous les nœuds du cluster en suivant la procédure d’installation du produit qui fournit le plug @ no__t-1Dans.<br />-Exécutez l’applet de commande PowerShell [@ no__t-1CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/Register-CauPlugin) pour inscrire le plug-and-no__t-2in sur les nœuds de cluster requis.           |
|                Tous les nœuds de cluster de basculement doivent avoir le même ensemble de plug-in de mise à jour adaptée aux clusters no__t-0ins                 |                                                                             Une exécution Self @ no__t-0updating peut échouer si le plug @ no__t-1Dans qui est configuré pour être utilisé dans une exécution de mise à jour est remplacé par un plug-in qui n’est pas disponible sur tous les nœuds du cluster.                                                                              |           -Assurez-vous que le plug @ no__t-0in configuré pour la mise à jour adaptée aux clusters est installé sur tous les nœuds du cluster en suivant la procédure d’installation du produit qui fournit le plug @ no__t-1Dans.<br />-Exécutez l’applet de commande PowerShell [@ no__t-1CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/Register-CauPlugin) pour inscrire le plug-and-no__t-2in sur les nœuds de cluster requis.           |
|                               Les options configurées pour l'Exécution de mise à jour doivent être valides                                |                                                                          Les options d’exécution de la planification et de la mise à jour du no__t-0updating auto-qui sont configurées pour ce cluster de basculement sont incomplètes ou non valides. Une exécution Self @ no__t-0updating peut échouer.                                                                           |                                                            Configurez une planification @ no__t-0updating valide et un ensemble d’options d’exécution de mise à jour. Par exemple, vous pouvez utiliser l’applet de commande PowerShell [Set @ no__t-1CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Set-CauClusterRole) pour configurer le rôle en cluster de la mise à jour adaptée aux clusters.                                                            |
|                  Au moins deux nœuds de cluster de basculement doivent être propriétaires du rôle en cluster de la Mise à jour adaptée aux clusters                  |                                                                                        Une exécution de mise à jour lancée en mode Self @ no__t-0updating échoue, car le rôle en cluster de la mise à jour adaptée aux clusters n’a pas de nœud propriétaire possible vers lequel se déplacer.                                                                                         |                                                                                                                Utilisez les outils de clustering de basculement pour vous assurer que tous les nœuds de cluster sont configurés en tant que propriétaires possibles du rôle en cluster de la Mise à jour adaptée aux clusters. Il s'agit de la configuration par défaut.                                                                                                                |
|                  Tous les nœuds de cluster de basculement doivent avoir accès aux scripts Windows PowerShell                  |                                                                        Tous les nœuds propriétaires possibles du rôle en cluster de la mise à jour adaptée aux clusters ne peuvent pas accéder aux scripts Windows PowerShell pre @ no__t-0update et no__t-1Update configurés. Une exécution Self @ no__t-0updating échoue.                                                                        |                                                                                                                    Assurez-vous que tous les nœuds propriétaires possibles du rôle en cluster de la mise à jour adaptée aux clusters sont autorisés à accéder aux scripts PowerShell pre @ no__t-0update et postérieurs à no__t-1Update configurés.                                                                                                                     |
|                   Tous les nœuds de cluster de basculement doivent utiliser des scripts Windows PowerShell identiques                   |                                                     Tous les nœuds propriétaires possibles du rôle en cluster de la mise à jour adaptée aux clusters utilisent la même copie des scripts Windows PowerShell pre @ no__t-0update et postérieurs à @ no__t-1Update spécifiés. Une exécution Self @ no__t-0updating peut échouer ou afficher un comportement inattendu.                                                     |                                                                                                                                   Assurez-vous que tous les nœuds propriétaires possibles du rôle en cluster de la mise à jour adaptée aux clusters utilisent les mêmes scripts PowerShell pre @ no__t-0update et postérieurs à @ no__t-1Update.                                                                                                                                   |
|         Le paramètre WarnAfter spécifié pour l'Exécution de mise à jour doit être inférieur au paramètre StopAfter         |                                                                           Les délais d'expiration de l'Exécution de mise à jour pour la Mise à jour adaptée aux clusters rendent l'avertissement relatif au délai d'expiration inefficace. Une Exécution de mise à jour peut être annulée avant qu'un journal des événements d'avertissement puisse être généré.                                                                            |                                                                                                                                      Dans les options de l'Exécution de mise à jour, configurez une valeur pour l'option **WarnAfter** qui soit inférieure à la valeur de l'option **StopAfter** .                                                                                                                                       |

## <a name="see-also"></a>Voir aussi  

-   [Vue d’ensemble de la mise à jour adaptée aux clusters](cluster-aware-updating.md)