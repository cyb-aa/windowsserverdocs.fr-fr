---
ms.assetid: 75cc1d24-fa2f-45bd-8f3b-1bbd4a1aead0
title: "Meilleures pratiques et les exigences de mise à jour adaptée aux clusters"
ms.prod: windows-server-threshold
ms.topic: article
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 4/28/2017
description: "Conditions d’utilisation de la mise à jour adaptée aux clusters pour installer les mises à jour sur les clusters exécutant Windows Server."
ms.openlocfilehash: 8517466d58345077af446c1b2c1e2aeb3b17aaa1
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-requirements-and-best-practices"></a>Meilleures pratiques et les exigences de mise à jour adaptée aux clusters

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016, Windows Server2012R2, Windows Server2012

Cette section décrit la configuration requise et les dépendances à satisfaire pour utiliser [mise à jour adaptée aux clusters](cluster-aware-updating.md) (adaptée aux clusters) pour appliquer des mises à jour vers un cluster de basculement exécutant Windows Server.
  
> [!NOTE]  
> Vous devrez peut-être valider indépendamment que votre environnement de cluster est prêt à appliquer les mises à jour si vous utilisez un plug-in autre que **Microsoft.WindowsUpdatePlugin**. Si vous utilisez une autre que celle Microsoft un plug-in, contactez l’éditeur pour plus d’informations. Pour plus d’informations sur l’installation d’un plug-ins, voir [fonctionnement un plug-ins](cluster-aware-updating-plug-ins.md).   
  
## <a name="BKMK_REQ_CLUS"></a>Installer la fonctionnalité de Clustering de basculement et les outils de Clustering de basculement  
Adaptée aux clusters nécessite une installation de la fonctionnalité de Clustering de basculement et les outils de Clustering de basculement. Les outils de Clustering de basculement incluent le \(clusterawareupdating.dll\) outils adaptée aux clusters, les applets de commande de Clustering de basculement et d’autres composants nécessaires aux opérations d’adaptée aux clusters. Pour savoir comment installer la fonctionnalité Clustering avec basculement, consultez [l’installation de la fonctionnalité de Clustering de basculement et des outils](http://go.microsoft.com/fwlink/p/?LinkId=253342).  
  
La configuration requise exacte pour les outils de Clustering de basculement varient selon si adaptée aux clusters coordonne les mises à jour comme un rôle en cluster sur le cluster de basculement \ (en utilisant mode\ mise à jour intégrée) ou à partir d’un ordinateur distant. En outre, le mode de mise à jour intégrée d’adaptée aux clusters nécessite l’installation du rôle en cluster adaptée aux clusters sur le cluster de basculement en utilisant les outils adaptée aux clusters.    
  
Le tableau suivant récapitule les spécifications de l’installation de fonctionnalité adaptée aux clusters pour les deux modes de mise à jour adaptée aux clusters.  
  
|Composant installé|Mode de mise à jour intégrée|Mode de mise à jour Remote\|  
|-----------------------|-----------------------|-------------------------|  
|Fonctionnalité de Clustering de basculement|Obligatoire sur tous les nœuds de cluster|Obligatoire sur tous les nœuds de cluster|  
|Outils de Clustering de basculement|Obligatoire sur tous les nœuds de cluster|-Requises sur l’ordinateur de mise à jour remote\<br />-Obligatoire sur tous les nœuds de cluster pour exécuter le [Save-CauDebugTrace](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/save-caudebugtrace) applet de commande|  
|Rôle en cluster adaptée aux clusters|Obligatoire|Non requis|  
  
## <a name="obtain-an-administrator-account"></a>Obtenir un compte d’administrateur  
Les exigences d’administrateur suivantes sont nécessaires pour utiliser les fonctionnalités adaptée aux clusters.  
  
-   Pour obtenir un aperçu ou appliquer des actions de mise à jour à l’aide de l’interface utilisateur adaptée aux clusters \(UI\) ou les applets de commande mise à jour adaptée aux clusters, vous devez utiliser un compte de domaine disposant des autorisations et droits d’administrateur local sur tous les nœuds de cluster. Si le compte ne dispose pas des privilèges suffisants sur chaque nœud, vous êtes invité à fournir les informations d’identification nécessaires lorsque vous effectuez ces actions dans la fenêtre de la mise à jour adaptée aux clusters. Pour utiliser le [mise à jour adaptée aux clusters](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating) applets de commande, vous pouvez fournir les informations d’identification nécessaires sous la forme d’un paramètre d’applet de commande.  
  
-   Si vous utilisez adaptée aux clusters en mode de mise à jour remote\ lorsque vous êtes connecté avec un compte qui n’a pas les autorisations et les droits d’administrateur local sur les nœuds de cluster, vous devez exécuter les outils adaptée aux clusters en tant qu’administrateur à l’aide d’un compte d’administrateur local sur l’ordinateur coordinateur de mise à jour ou à l’aide d’un compte qui possède le **emprunter l’identité d’un client après l’authentification** droit d’utilisateur. 
  
-   Pour exécuter le Best Practices Analyzer adaptée aux clusters, vous devez utiliser un compte qui dispose des privilèges d’administrateur sur les nœuds de cluster et des privilèges d’administrateur local sur l’ordinateur qui est utilisé pour exécuter le [Test-CauSetup](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/test-causetup) applet de commande ou d’analyser la disponibilité à l’aide de la fenêtre de la mise à jour adaptée aux clusters de mise à jour de cluster. Pour plus d’informations, voir [mise à jour de la préparation du cluster de Test](#BKMK_BPA).  
  
## <a name="verify-the-cluster-configuration"></a>Vérifiez la configuration du cluster  
Voici les conditions générales requises pour un cluster de basculement prendre en charge les mises à jour à l’aide d’adaptée aux clusters. Configuration supplémentaire requise pour la gestion à distance sur les nœuds est répertoriés dans [configurer les nœuds pour la gestion à distance](#BKMK_NODE_CONFIG) plus loin dans cette rubrique.  
  
-   Les nœuds de cluster suffisant doivent être en ligne afin que le cluster a un quorum.  
  
-   Tous les nœuds de cluster doivent être dans le même domaine ActiveDirectory.  
  
-   Le nom du cluster doit être résolu sur le réseau à l’aide de DNS.  
  
-   Si adaptée aux clusters est utilisée en mode de mise à jour remote\, l’ordinateur coordinateur de mise à jour doit être connecté aux nœuds du cluster de basculement, et il doit être dans le même domaine ActiveDirectory en tant que le cluster de basculement.  
  
-   Le service de Cluster doit s’exécuter sur tous les nœuds de cluster. Par défaut, ce service est installé sur tous les nœuds de cluster et est configuré pour démarrer automatiquement.  
  
-   Pour utiliser les scripts PowerShell pre\-mise à jour ou post\-mise à jour durant une exécution de mise à jour adaptée aux clusters, assurez-vous que les scripts sont installés sur tous les nœuds de cluster ou qu’ils sont accessibles à tous les nœuds, par exemple, sur un partage de fichiers réseau hautement disponible. Si les scripts sont enregistrées dans un partage de fichiers réseau, configurez le dossier d’autorisation de lecture pour tout le monde groupe.  
  
## <a name="BKMK_NODE_CONFIG"></a>Configurer les nœuds pour la gestion à distance  
Pour utiliser la mise à jour adaptée aux clusters, tous les nœuds du cluster doivent être configurés pour la gestion à distance. Par défaut, la seule tâche à effectuer pour configurer les nœuds pour la gestion à distance est à [activer une règle de pare-feu pour permettre les redémarrages automatiques](#BKMK_FW). 

Le tableau suivant répertorie les exigences de gestion à distance terminée, en cas de votre environnement diffère les valeurs par défaut.

Ces conditions requises s’ajoutent aux exigences d’installation pour le [installer la fonctionnalité de Clustering de basculement et les outils de Clustering de basculement](#BKMK_REQ_CLUS) et aux prescriptions générales clustering configuration requise décrite dans les sections précédentes de cette rubrique.  
  
|Configuration requise|État par défaut|Mode de mise à jour intégrée|Mode de mise à jour Remote\|  
|---------------|---|-----------------------|-------------------------|  
|[Activer une règle de pare-feu pour permettre les redémarrages automatiques](#BKMK_FW)|Désactivé|Obligatoire sur tous les nœuds de cluster si un pare-feu est en cours d’utilisation|Obligatoire sur tous les nœuds de cluster si un pare-feu est en cours d’utilisation|  
|[Activer WindowsManagementInstrumentation](#BKMK_WMI)|Activé|Obligatoire sur tous les nœuds de cluster|Obligatoire sur tous les nœuds de cluster|  
|[Activer WindowsPowerShell3.0 ou 4.0 et la communication à distance de Windows PowerShell](#BKMK_PS)|Activé|Obligatoire sur tous les nœuds de cluster|Obligatoire sur tous les nœuds de cluster pour exécuter les éléments suivants:<br /><br />-La [Save-CauDebugTrace](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/save-caudebugtrace) applet de commande<br />-Pre\-scripts PowerShell et post\-mise à jour durant une exécution de mise à jour<br />-Tests de mise à jour de disponibilité à l’aide de la fenêtre de la mise à jour adaptée aux clusters cluster ou le [test-CauSetup](http://go.microsoft.com/fwlink/p/?LinkId=242384) applet de commande Windows PowerShell|  
|[Installer .NETFramework 4.6 ou 4.5](#BKMK_NET)|Activé|Obligatoire sur tous les nœuds de cluster|Obligatoire sur tous les nœuds de cluster pour exécuter les éléments suivants:<br /><br />-La [Save-CauDebugTrace](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/save-caudebugtrace) applet de commande<br />-Pre\-scripts PowerShell et post\-mise à jour durant une exécution de mise à jour<br />-Tests de mise à jour de disponibilité à l’aide de la fenêtre de la mise à jour adaptée aux clusters cluster ou le [test-CauSetup](http://go.microsoft.com/fwlink/p/?LinkId=242384) applet de commande Windows PowerShell|  

### <a name="BKMK_FW"></a>Activer une règle de pare-feu pour permettre les redémarrages automatiques  
Pour permettre le redémarrage automatique après l’application des mises à jour \ (si l’installation d’une mise à jour nécessite un Restart), si le pare-feu Windows ou un pare-feu autre que celle Microsoft est en cours d’utilisation sur les nœuds de cluster, une règle de pare-feu doit être activée sur chaque nœud qui autorise le trafic suivant:  
  
-   Protocole: TCP  
  
-   Sens: entrant  
  
-   Programme: wininit.exe  
  
-   Ports: Les Ports dynamiques RPC  
  
-   Profil: domaine  
  
Si le pare-feu Windows est utilisé sur les nœuds de cluster, cela en activant le **arrêt à distance** groupe de règles de pare-feu Windows sur chaque nœud du cluster. Lorsque vous utilisez la fenêtre de la mise à jour adaptée aux clusters pour appliquer des mises à jour et configurer les options de mise à jour intégrée, le **arrêt à distance** groupe de règles de pare-feu Windows est activé automatiquement sur chaque nœud du cluster.  
  
> [!NOTE]  
> Le **arrêt à distance** groupe de règles de pare-feu Windows ne peut pas être activé quand il est en conflit avec les paramètres de stratégie de groupe qui sont configurés pour le pare-feu Windows.    
  
Le **arrêt à distance** groupe de règles de pare-feu est également activé en spécifiant le **– EnableFirewallRules** paramètre lors de l’exécution des applets de commande suivantes adaptée aux clusters: [Add-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole), [Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-causcan), et [SetCauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole).  
  
L’exemple de PowerShell suivant montre une méthode supplémentaire pour permettre le redémarrage automatique sur un nœud de cluster.  
  
```PowerShell  
Set-NetFirewallRule -Group "@firewallapi.dll,-36751" -Profile Domain -Enabled true  
```  

### <a name="BKMK_WMI"></a>Activer WindowsManagementInstrumentation (WMI) 
Tous les nœuds de cluster doivent être configurés pour la gestion à distance à l’aide de WindowsManagementInstrumentation \(WMI\). Ceci est activé par défaut.  
  
Pour activer manuellement la gestion à distance, procédez comme suit:  
  
1.  Dans la console Services, démarrez le **WindowsRemoteManagement** de service et définir le type de démarrage sur **automatique**.  
  
2.  Exécutez le [Set-WSManQuickConfig](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.wsman.management/set-wsmanquickconfig) invite applet de commande, ou exécutez la commande suivante à partir d’une commande avec élévation de privilèges:  
  
    ```PowerShell  
    winrm quickconfig -q  
    ```  
  
Pour prendre en charge la communication à distance WMI, si le pare-feu Windows est en cours d’utilisation sur les nœuds de cluster, le pare-feu entrant règle pour **WindowsRemoteManagement \(HTTP\-In\)** doit être activée sur chaque nœud.  Par défaut, cette règle est activée.  
  
### <a name="BKMK_PS"></a>Activer Windows PowerShell et la communication à distance de Windows PowerShell  
Pour activer le mode de mise à jour intégrée et certaines fonctionnalités d’adaptée aux clusters en mode de mise à jour remote\, PowerShell doit être installé et activé pour exécuter des commandes à distance sur tous les nœuds de cluster. Par défaut, PowerShell est installé et activé pour la communication à distance.  
  
Pour activer la communication à distance PowerShell, utilisez une des méthodes suivantes:  
  
-   Exécutez le [Enable-PSRemoting](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/enable-psremoting) applet de commande.  
  
-   Configurer un paramètre de stratégie de groupe au niveau domain\ pour la gestion à distance de Windows \(WinRM\).  
  
Pour plus d’informations sur l’activation de la communication à distance PowerShell, voir [about\_Remote\_Requirements](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/about/about_remote_requirements).  
  
### <a name="BKMK_NET"></a>Installer .NETFramework 4.6 ou 4.5  
Pour activer le mode de mise à jour intégrée et certaines fonctionnalités d’adaptée aux clusters en mode de mise à jour remote\, .NETFramework 4.6 ou .NETFramework 4.5 (sur Windows Server2012R2) doit être installé sur tous les nœuds de cluster. Par défaut, .NETFramework est installé.  

Pour installer .NETFramework 4.6 (ou 4.5) à l’aide de PowerShell s’il n’est pas déjà installé, utilisez la commande suivante:

```PowerShell
Install-WindowsFeature -Name NET-Framework-45-Core
```

## <a name="BKMK_BEST_PRAC"></a>Meilleures pratiques recommandées pour l’utilisation de la mise à jour adaptée aux clusters 
  
### <a name="BKMK_BP_WUA"></a>Recommandations pour l’application des mises à jour Microsoft

Nous recommandons que quand vous commencez à utiliser adaptée aux clusters pour appliquer des mises à jour avec la valeur par défaut **Microsoft.WindowsUpdatePlugin** un plug-in sur un cluster, vous arrêtez d’utiliser d’autres méthodes pour installer les mises à jour logicielles à partir de Microsoft sur les nœuds de cluster.  
  
> [!CAUTION]  
> Combinaison adaptée aux clusters avec des méthodes qui mettent à jour des nœuds individuels automatiquement \ (sur un temps de temps fixe planifié\) peut provoquer des résultats imprévisibles, notamment des interruptions de service et les temps d’arrêt non planifié.  
  
Nous vous recommandons de suivre ces recommandations:  
  
-   Pour obtenir des résultats optimaux, nous vous recommandons de désactiver paramètres sur les nœuds de cluster pour la mise à jour automatique, par exemple, via les paramètres de mises à jour automatiques dans le panneau de configuration ou dans les paramètres qui sont configurés à l’aide de la stratégie de groupe.  
  
    > [!CAUTION]  
    > Installation automatique des mises à jour sur les nœuds de cluster peut interférer avec l’installation de mises à jour adaptée aux clusters et peut entraîner des échecs adaptée aux clusters.  
  
    Si elles sont nécessaires, les paramètres suivants de mises à jour automatiques sont compatibles avec adaptée aux clusters, étant donné que l’administrateur peut contrôler le moment de l’installation de la mise à jour:  
  
    -   Paramètres de notification avant le téléchargement des mises à jour et de notification avant l’installation  
  
    -   Paramètres de téléchargement automatique des mises à jour et de notification avant l’installation  
  
    Toutefois, si les mises à jour automatiques télécharge les mises à jour en même temps que d’une exécution de mise à jour adaptée aux clusters, l’exécution de mise à jour peut prendre plu de temps.  
  
-   Ne configurez pas un système de mise à jour comme \(WSUS\) WindowsServerUpdateServices pour appliquer des mises à jour automatiquement \ (sur un temps de temps fixe planifié\) pour les nœuds de cluster.  
  
-   Tous les nœuds de cluster doivent être uniformément configurés pour utiliser la même source de mise à jour, par exemple, un WSUS server, Windows Update ou MicrosoftUpdate.  
  
-   Si vous utilisez un système de gestion de configuration pour appliquer des mises à jour logicielles aux ordinateurs sur le réseau, excluez les nœuds de cluster de toutes les mises à jour requises ou automatiques. MicrosoftSystemCenter ConfigurationManager2007 et MicrosoftSystemCenter Virtual Machine Manager2008 constituent des exemples de systèmes de gestion de configuration.  
  
-   Si les serveurs de distribution de logiciels internes \ (par exemple, WSUS servers\) sont utilisés pour contenir et déployer les mises à jour, assurez-vous que ces serveurs identifient correctement les mises à jour approuvées pour les nœuds de cluster.  
  
#### <a name="BKMK_PROXY"></a>Appliquer des mises à jour Microsoft dans les scénarios de succursale  
Pour télécharger les mises à jour Microsoft à partir de MicrosoftUpdate ou Windows Update sur les nœuds de cluster dans certains scénarios de succursale, vous devrez configurer les paramètres de proxy pour le compte système Local sur chaque nœud. Par exemple, vous devrez peut-être procéder ainsi si les clusters de vos filiales accéder à MicrosoftUpdate ou Windows Update pour télécharger les mises à jour à l’aide d’un serveur proxy local.  
  
Si nécessaire, configurez les paramètres de proxy WinHTTP sur chaque nœud pour spécifier un serveur proxy local et configurer des exceptions d’adresses locales \ (autrement dit, une liste de contournement pour addresses\ local). Pour ce faire, vous pouvez exécuter la commande suivante sur chaque nœud de cluster à partir d’une invite de commandes avec élévation de privilèges:  
  
```  
netsh winhttp set proxy <ProxyServerFQDN >:<port> "<local>"  
```  
  
Où <*ProxyServerFQDN*> est le nom de domaine complet du serveur proxy et <*port*> est le port sur lequel vous souhaitez communiquer (généralement le port 443).  
  
Par exemple, pour configurer les paramètres de proxy WinHTTP pour le compte système Local en spécifiant le serveur proxy *MyProxy.CONTOSO.com*avec le port 443 et exceptions d’adresses locales, tapez la commande suivante:  
  
```  
netsh winhttp set proxy MyProxy.CONTOSO.com:443 "<local>"  
```  
  
### <a name="BKMK_BP_HF"></a>Recommandations pour l’utilisation de Microsoft.HotfixPlugin  
  
-   Nous vous recommandons de configurer des autorisations dans le dossier racine de correctifs logiciels et le fichier de configuration de correctif logiciel pour restreindre l’accès en écriture aux administrateurs locaux sur les ordinateurs qui sont utilisés pour stocker ces fichiers. Cela permet d’empêcher la falsification de ces fichiers par des utilisateurs non autorisés qui pourraient compromettre la fonctionnalité du cluster de basculement lors de l’application des correctifs logiciels.  
  
-   Pour garantir l’intégrité des données pour les connexions server message block \(SMB\) qui sont utilisés pour accéder au dossier racine de correctif logiciel, vous devez configurer le chiffrement SMB dans le dossier partagé SMB, s’il est possible de le configurer. Le **Microsoft.HotfixPlugin** nécessite que la signature SMB ou chiffrement SMB soit configuré pour garantir l’intégrité des données pour les connexions SMB. 
  
    Pour plus d’informations, voir [restreindre l’accès pour le dossier racine de correctifs logiciels et le fichier de configuration de correctif](cluster-aware-updating-plug-ins.md#BKMK_ACL).
  
### <a name="additional-recommendations"></a>Recommandations supplémentaires  
  
-   Pour éviter toute interférence avec une exécution adaptée aux clusters mise à jour qui peuvent être planifiés en même temps, ne planifiez pas les modifications de mot de passe pour les objets de nom de cluster et des objets d’ordinateur virtuel au cours des fenêtres de maintenance planifiée.  
  
-   Vous devez définir des autorisations appropriées sur les scripts pre\- et post\-mises à jour qui sont enregistrés sur les dossiers réseau partagés pour empêcher toute falsification de ces fichiers par des utilisateurs non autorisés.  
  
-   Pour configurer adaptée aux clusters en mode de mise à jour intégrée, un objet ordinateur virtuel \(VCO\) pour le rôle en cluster adaptée aux clusters doit être créé dans ActiveDirectory. Adaptée aux clusters peut créer cet objet automatiquement au moment où le rôle en cluster adaptée aux clusters est ajouté, si le cluster de basculement dispose des autorisations suffisantes. Toutefois, en raison des stratégies de sécurité dans certaines organisations, il peut être nécessaire de prédéfinir l’objet dans ActiveDirectory. Pour la procédure à suivre, voir [étapes de prédéfinition d’un compte pour un rôle en cluster](http://go.microsoft.com/fwlink/p/?LinkId=237624).  
  
-   Pour enregistrer et réutiliser des paramètres d’exécution de mise à jour sur les clusters de basculement avec similaire de mise à jour des besoins de l’organisation informatique, vous pouvez créer profils d’exécution de mise à jour. En outre, selon le mode de mise à jour, vous pouvez enregistrer et gérer les profils d’exécution de mise à jour sur un partage de fichiers qui est accessible à tous les ordinateurs coordinateur de mise à jour distants ou clusters de basculement. Pour plus d’informations, voir [Options avancées et mise à jour des profils d’exécution pour adaptée aux clusters](cluster-aware-updating-options.md).  
  
## <a name="BKMK_BPA"></a>Mise à jour de la préparation du cluster de test  
Vous pouvez exécuter le modèle \(BPA\) adaptée aux clusters Best Practices Analyzer pour tester si un cluster de basculement et l’environnement réseau répondent à la plupart de la configuration requise pour les mises à jour logicielles appliquées par adaptée aux clusters. Bon nombre des tests de vérifier l’environnement de préparation à appliquer les mises à jour Microsoft à l’aide de la valeur par défaut, un plug-in, **Microsoft.WindowsUpdatePlugin**.  
  
> [!NOTE]  
> Vous devrez peut-être valider indépendamment que votre environnement de cluster est prêt à appliquer les mises à jour logicielles en utilisant un plug-in autre que **Microsoft.WindowsUpdatePlugin**. Si vous utilisez une autre que celle Microsoft un plug-in, tel que celui fourni par le fabricant de votre matériel, contactez l’éditeur pour plus d’informations.  
  
Vous pouvez exécuter l’outil BPA des deux manières suivantes:  
  
1.  Sélectionnez **analyser la mise à jour de la préparation du cluster** dans la console adaptée aux clusters. L’outil BPA après les tests de préparation, un rapport de test s’affiche. Si des problèmes sont détectés sur les nœuds de cluster, les problèmes et les nœuds où ils apparaissent sont identifiés pour que vous pouvez prendre des mesures correctives. Les tests peuvent prendre plusieurs minutes.  
  
2.  Exécutez le [Test-CauSetup](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/test-causetup) applet de commande. Vous pouvez exécuter l’applet de commande sur un ordinateur local ou distant sur lequel est installé le basculement Clustering Module pour Windows PowerShell (partie des outils de Clustering de basculement). Vous pouvez également exécuter l’applet de commande sur un nœud du cluster de basculement.  
  
> [!NOTE]  
> -   Vous devez utiliser un compte qui dispose des privilèges d’administrateur sur les nœuds de cluster et des privilèges d’administrateur local sur l’ordinateur qui est utilisé pour exécuter le **test-CauSetup** applet de commande ou d’analyser la disponibilité à l’aide de la fenêtre de la mise à jour adaptée aux clusters de mise à jour de cluster. Pour exécuter les tests à l’aide de la fenêtre de la mise à jour adaptée aux clusters, vous devez être connecté à l’ordinateur avec les informations d’identification nécessaires.  
> -   Les tests partent du principe que les outils adaptée aux clusters qui sont utilisés pour afficher un aperçu et appliquer des mises à jour logicielles s’exécuter depuis le même ordinateur et avec les mêmes informations d’identification de l’utilisateur comme sont utilisés pour tester la mise à jour de la disponibilité du cluster.  
  
> [!IMPORTANT]  
> Nous vous recommandons vivement de tester le cluster de mise à jour de préparation dans les situations suivantes:  
>   
> -   Avant d’utiliser adaptée aux clusters pour la première fois pour appliquer les mises à jour logicielles.  
> -   Une fois que vous ajoutez un nœud au cluster ou effectuez d’autres modifications sur le matériel dans le cluster qui nécessitent l’exécution de la validation d’un Assistant de Cluster.  
> -   Une fois que vous modifiez une source de mise à jour, ou les paramètres de mise à jour ou configurations \(other than CAU\) qui peuvent affecter l’application des mises à jour sur les nœuds.  
  
### <a name="tests-for-cluster-updating-readiness"></a>Tests de mise à jour de la disponibilité du cluster  
Le tableau suivant répertorie la mise à jour des tests de préparation, certains problèmes courants et les étapes de résolution de cluster.  
  
|Test|Problèmes et impacts possibles|Étapes de résolution|  
|--------|-------------------------------|--------------------|  
|Le cluster de basculement doit être disponible|Impossible de résoudre que le nom de cluster de basculement, ou un ou plusieurs nœuds de cluster ne sont pas accessibles. L’outil BPA ne peut pas exécuter les tests de préparation du cluster.|-Vérifier l’orthographe du nom de cluster spécifié durant l’exécution de l’outil BPA.<br />-Assurez-vous que tous les nœuds du cluster sont en ligne et en cours d’exécution.<br />-Vérifier que la validation d’un Assistant de Configuration peut s’exécuter sur le cluster de basculement.|  
|Les nœuds de cluster de basculement doivent être activés pour la gestion à distance via WMI|Un ou plusieurs nœuds de cluster de basculement ne sont pas activés pour la gestion à distance à l’aide de WindowsManagementInstrumentation \(WMI\). Adaptée aux clusters ne peut pas mettre à jour les nœuds de cluster si les nœuds ne sont pas configurés pour la gestion à distance.|Assurez-vous que tous les nœuds de cluster de basculement sont activés pour la gestion à distance via WMI. Pour plus d’informations, voir [configurer les nœuds pour la gestion à distance](#BKMK_NODE_CONFIG) dans cette rubrique.|  
|PowerShell doit être activée sur chaque nœud de cluster de basculement|PowerShell n’est pas installé ou n’est pas activé pour la communication à distance sur un ou plusieurs nœuds de cluster de basculement. Adaptée aux clusters ne peut pas être configuré pour le mode de mise à jour intégrée ou utiliser certaines fonctionnalités du mode de mise à jour remote\.|Assurez-vous que PowerShell est installé sur tous les nœuds de cluster et est activé pour la communication à distance.<br /><br />Pour plus d’informations, voir [configurer les nœuds pour la gestion à distance](#BKMK_NODE_CONFIG) dans cette rubrique.|  
|Version du cluster de basculement|Un ou plusieurs nœuds du cluster de basculement ne s’exécutent Windows Server2016, Windows Server2012R2 ou Windows Server2012. Adaptée aux clusters ne peut pas mettre à jour le cluster de basculement.|Vérifiez que le cluster de basculement qui est spécifié pendant l’exécution de l’outil BPA exécute Windows Server2016, Windows Server2012R2 ou Windows Server2012.<br /><br />Pour plus d’informations, voir [vérifier la configuration de cluster](#BKMK_REQ_CLUS) dans cette rubrique.|  
|Les versions requises de .NETFramework et Windows PowerShell doivent être installées sur tous les nœuds de cluster de basculement|.NETFramework 4.6 4.5 ou Windows PowerShell n’est pas installé sur un ou plusieurs nœuds de cluster. Certaines fonctionnalités adaptée aux clusters peut ne pas fonctionnent.|Assurez-vous que .NETFramework 4.6 ou 4.5 et Windows PowerShell sont installés sur tous les nœuds de cluster, s’ils sont requis.<br /><br />Pour plus d’informations, voir [configurer les nœuds pour la gestion à distance](#BKMK_NODE_CONFIG) dans cette rubrique.|  
|Le service de Cluster doit s’exécuter sur tous les nœuds de cluster|Le service de Cluster n’est pas en cours d’exécution sur un ou plusieurs nœuds. Adaptée aux clusters ne peut pas mettre à jour le cluster de basculement.|-S’assurer que le \(clussvc\) service Cluster a démarré sur tous les nœuds du cluster, et il est configuré pour démarrer automatiquement.<br />-Vérifier que la validation d’un Assistant de Configuration peut s’exécuter sur le cluster de basculement.<br /><br />Pour plus d’informations, voir [vérifier la configuration de cluster](#BKMK_REQ_CLUS) dans cette rubrique.|  
|Mises à jour automatiques ne doivent pas être configurés pour installer automatiquement les mises à jour sur n’importe quel nœud de cluster de basculement|Sur au moins un nœud de cluster de basculement, les mises à jour automatiques est configuré pour installer automatiquement les mises à jour Microsoft sur ce nœud. Combinaison adaptée aux clusters avec d’autres méthodes de mise à jour peut entraîner des temps d’arrêt non planifié ou des résultats imprévisibles.|Si la fonctionnalité mise à jour de Windows est configurée pour les mises à jour automatiques sur un ou plusieurs nœuds de cluster, assurez-vous que les mises à jour automatique n’est pas configuré pour installer automatiquement les mises à jour.<br /><br />Pour plus d’informations, voir [recommandations pour l’application des mises à jour Microsoft](#BKMK_BP_WUA).|  
|Les nœuds de cluster de basculement doivent utiliser la même source de mise à jour|Un ou plusieurs nœuds de cluster de basculement sont configurés pour utiliser une source de mise à jour pour les mises à jour de Microsoft est différent du reste des nœuds. Mises à jour peuvent ne pas être appliqués uniformément sur les nœuds de cluster par adaptée aux clusters.|Assurez-vous que chaque nœud de cluster est configuré pour utiliser la même source de mise à jour, par exemple, un serveur WSUS, Windows Update ou MicrosoftUpdate.<br /><br />Pour plus d’informations, voir [recommandations pour l’application des mises à jour Microsoft](#BKMK_BP_WUA).|  
|Une règle de pare-feu qui permet l’arrêt à distance doit être activée sur chaque nœud du cluster de basculement|Un ou plusieurs nœuds de cluster de basculement ne disposent pas d’une règle de pare-feu activée qui permet l’arrêt à distance, ou un paramètre de stratégie de groupe empêche cette règle d’être activée. Une mise à jour d’exécution qui applique des mises à jour nécessitant un redémarrage automatique des nœuds ne peut pas effectuer correctement.|Si le pare-feu Windows ou un pare-feu autre que celle Microsoft est en cours d’utilisation sur les nœuds de cluster, configurez une règle de pare-feu qui permet l’arrêt à distance.<br /><br />Pour plus d’informations, voir [activer une règle de pare-feu pour permettre les redémarrages automatiques](#BKMK_FW) dans cette rubrique.|  
|Le paramètre de serveur proxy sur chaque nœud de cluster de basculement doit être défini sur un serveur proxy local|Un ou plusieurs nœuds de cluster de basculement ont une configuration du serveur proxy incorrecte.<br /><br />Si un serveur proxy local est en cours d’utilisation, le paramètre de serveur proxy sur chaque nœud doit être configuré correctement pour le cluster d’accéder à MicrosoftUpdate ou Windows Update.|Assurez-vous que les paramètres de proxy WinHTTP sur chaque nœud de cluster sont définies pour un serveur proxy local s’il est nécessaire. Si un serveur proxy n’est pas en cours d’utilisation dans votre environnement, cet avertissement peut être ignoré.<br /><br />Pour plus d’informations, voir [appliquer les mises à jour dans les scénarios de succursale](#BKMK_PROXY) dans cette rubrique.|  
|Le rôle en cluster adaptée aux clusters doit être installé sur le cluster de basculement pour activer le mode de mise à jour intégrée|Le rôle en cluster adaptée aux clusters n’est pas installé sur ce cluster de basculement. Ce rôle est nécessaire pour la mise à jour intégrée du cluster.|Pour utiliser adaptée aux clusters en mode de mise à jour intégrée, ajoutez le rôle en cluster adaptée aux clusters sur le cluster de basculement dans une des manières suivantes:<br /><br />-Exécuter le [Add-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole) applet de commande PowerShell.<br />-Sélectionnez la **configurer les options de mise à jour intégrée cluster** action dans la fenêtre de la mise à jour adaptée aux clusters.|  
|Le rôle en cluster adaptée aux clusters doit être activé sur le cluster de basculement pour activer le mode de mise à jour intégrée|Le rôle en cluster adaptée aux clusters est désactivé. Par exemple, le rôle en cluster adaptée aux clusters n’est pas installé, ou il a été désactivé à l’aide de la [Disable-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/disable-cauclusterrole) applet de commande PowerShell. Ce rôle est nécessaire pour la mise à jour intégrée du cluster.|Pour utiliser adaptée aux clusters en mode de mise à jour intégrée, activer le rôle en cluster adaptée aux clusters sur ce cluster de basculement dans une des manières suivantes:<br /><br />-Exécuter le [Enable-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/enable-cauclusterrole) applet de commande PowerShell.<br />-Sélectionnez la **configurer les options de mise à jour intégrée cluster** action dans la fenêtre de la mise à jour adaptée aux clusters.|  
|L’adaptée aux clusters configurée un plug-in pour le mode de mise à jour intégrée doit être enregistrée sur tous les nœuds de cluster de basculement|Le rôle en cluster adaptée aux clusters sur un ou plusieurs nœuds de ce cluster de basculement ne peut pas accéder au module d’un plug-in adaptée aux clusters qui est configuré dans les options de mise à jour intégrée. Un intégrée mise à jour exécuter risque d’échouer.|-S’assurer que l’adaptée aux clusters configurée un plug-in est installée sur tous les nœuds de cluster en suivant la procédure d’installation pour le produit qui fournit l’adaptée aux clusters un plug-in.<br />-Exécuter le [Register-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) applet de commande PowerShell pour inscrire le plug-in sur les nœuds de cluster nécessaires.|  
|Tous les nœuds de cluster de basculement doivent avoir le même ensemble d’inscrit un plug-ins d’adaptée aux clusters|Un intégrée mise à jour exécuter risque d’échouer si le plug-in qui est configuré pour être utilisé dans une exécution de mise à jour est remplacé par un qui n’est pas disponible sur tous les nœuds de cluster.|-S’assurer que l’adaptée aux clusters configurée un plug-in est installée sur tous les nœuds de cluster en suivant la procédure d’installation pour le produit qui fournit l’adaptée aux clusters un plug-in.<br />-Exécuter le [Register-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) applet de commande PowerShell pour inscrire le plug-in sur les nœuds de cluster nécessaires.|  
|Les options de mise à jour configurées doivent être valides|Le calendrier de mise à jour intégrée et les options d’exécution de mise à jour qui sont configurées pour ce cluster de basculement sont incomplètes ou non valides. Un intégrée mise à jour exécuter risque d’échouer.|Configurer un calendrier de mise à jour intégrée valide et définir des options d’exécution de mise à jour. Par exemple, vous pouvez utiliser le [Set-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole) rôle en cluster l’applet de commande PowerShell pour configurer l’adaptée aux clusters.|  
|Au moins deux nœuds de cluster de basculement doivent être propriétaires du rôle en cluster adaptée aux clusters|Une mise à jour exécution lancée en mode de mise à jour intégrée échoue, car le rôle en cluster adaptée aux clusters n’a pas les déplacer vers un nœud propriétaire.|Utilisez les outils de Clustering de basculement pour vous assurer que tous les nœuds de cluster sont configurés comme rôle en cluster propriétaires possibles de cette dernière. Il s’agit de la configuration par défaut.||  
|Tous les nœuds de cluster de basculement doivent être en mesure d’accéder aux scripts Windows PowerShell|Pas tous les nœuds propriétaires possibles du rôle en cluster adaptée aux clusters peuvent accéder aux scripts de pre\- et post\-mises à jour de Windows PowerShell configurés. Un intégrée mise à jour exécuter échouera.|Assurez-vous que tous les nœuds propriétaires possibles du rôle en cluster adaptée aux clusters disposent des autorisations pour accéder aux scripts pre\-mise à jour et mise à jour post\ PowerShell configurés.|  
|Tous les nœuds de cluster de basculement doivent utiliser des scripts Windows PowerShell identiques|Pas de tous les nœuds propriétaires possibles du rôle en cluster adaptée aux clusters utilisent la même copie des scripts pre\- et post\-mises à jour de Windows PowerShell spécifiés. Une exécution de mise à jour intégrée peut échouer ou un comportement inattendu.|Assurez-vous que tous les nœuds propriétaires possibles du rôle en cluster adaptée aux clusters utilisent les mêmes scripts PowerShell pre\- et post\-mises à jour.|  
|Le paramètre WarnAfter spécifié pour l’exécution de mise à jour doit être inférieure au paramètre StopAfter|Les valeurs de délai d’exécution de mise à jour adaptée aux clusters spécifiés Assurez-vous que le délai d’expiration de l’avertissement inefficace. Une exécution de mise à jour peut être annulée avant un journal des événements avertissement peut être généré.|Dans les options de mise à jour, vous devez configurer un **WarnAfter** option la valeur est inférieure à celle de **StopAfter** valeur d’option.|  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Vue d’ensemble de la mise à jour adaptée aux clusters](cluster-aware-updating.md)
  
