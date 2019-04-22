---
ms.assetid: 75cc1d24-fa2f-45bd-8f3b-1bbd4a1aead0
title: Configuration requise de la mise à jour adaptée aux clusters et les meilleures pratiques
ms.prod: windows-server-threshold
ms.topic: article
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 08/06/2018
description: Conditions d’utilisation de la mise à jour adaptée aux clusters pour installer les mises à jour sur les clusters exécutant Windows Server.
ms.openlocfilehash: 379c3caa39b09e8a912150f2423190e143991c05
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819730"
---
# <a name="cluster-aware-updating-requirements-and-best-practices"></a>Configuration requise de la mise à jour adaptée aux clusters et les meilleures pratiques

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette section décrit la configuration requise et les dépendances qui sont nécessaires pour utiliser [la mise à jour adaptée aux clusters](cluster-aware-updating.md) (adaptée aux clusters) à appliquer les mises à jour vers un cluster de basculement exécutant Windows Server.
  
> [!NOTE]  
> Vous devrez peut-être valider indépendamment que votre environnement de cluster est prêt à appliquer les mises à jour si vous utilisez un plug\-dans d’autres que **Microsoft.WindowsUpdatePlugin**. Si vous utilisez un non\-Microsoft plug\-, contacter le serveur de publication pour plus d’informations. Pour plus d’informations sur plug-\-ins, consultez [comment brancher\-ins fonctionnent](cluster-aware-updating-plug-ins.md).   
  
## <a name="BKMK_REQ_CLUS"></a>Installer la fonctionnalité Clustering avec basculement et les outils de Clustering de basculement  
La Mise à jour adaptée aux clusters demande une installation de la fonctionnalité et des outils de clustering de basculement. Les outils de Clustering de basculement incluent les outils adaptée aux clusters \(clusterawareupdating.dll\), les applets de commande de Clustering de basculement et d’autres composants nécessaires aux opérations d’adaptée aux clusters. Pour connaître les étapes d’installation de la fonctionnalité de clustering de basculement, voir la page relative à l’ [installation de la fonctionnalité et des outils de clustering de basculement](create-failover-cluster.md#install-the-failover-clustering-feature).  
  
La configuration requise exacte pour les outils de Clustering de basculement varie selon si adaptée aux clusters coordonne les mises à jour comme un rôle en cluster sur le cluster de basculement \(à l’aide de self\-la mise à jour en mode\) ou à partir d’un ordinateur distant. Self\-la mise à jour en outre de mode d’adaptée aux clusters nécessite l’installation du rôle en cluster adaptée aux clusters sur le cluster de basculement en utilisant les outils adaptée aux clusters.    
  
Le tableau suivant résume les conditions requises par l'installation de la fonctionnalité de Mise à jour adaptée aux clusters pour les deux modes de mise à jour possibles.  
  
|Composant installé|Self\-en mode de mise à jour|À distance\-en mode de mise à jour|  
|-----------------------|-----------------------|-------------------------|  
|Fonctionnalité de clustering de basculement|Obligatoire sur tous les nœuds de cluster|Obligatoire sur tous les nœuds de cluster|  
|Outils de clustering de basculement|Obligatoire sur tous les nœuds de cluster|-Required de télécommande\-la mise à jour d’ordinateur<br />-Obligatoire sur tous les nœuds de cluster pour exécuter le [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps) applet de commande|  
|Rôle en cluster de la Mise à jour adaptée aux clusters|Obligatoire|Non requis|  
  
## <a name="obtain-an-administrator-account"></a>Obtenir un compte d'administrateur  
Vous devez satisfaire aux exigences suivantes en matière d'administration pour pouvoir utiliser les fonctionnalités de la Mise à jour adaptée aux clusters.  
  
-   Pour afficher un aperçu ou appliquer des actions de mise à jour à l’aide de l’interface utilisateur d’adaptée aux clusters \(l’interface utilisateur\) ou les applets de commande mise à jour adaptée aux clusters, vous devez utiliser un compte de domaine disposant des droits d’administrateur local et les autorisations sur tous les nœuds de cluster. Si le compte ne dispose pas des privilèges suffisants sur chaque nœud, vous êtes invité dans la fenêtre de la mise à jour adaptée aux clusters pour fournir les informations d’identification nécessaires lorsque vous effectuez ces actions. Pour utiliser le [la mise à jour adaptée aux clusters](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps) applets de commande, vous pouvez fournir les informations d’identification nécessaires comme un paramètre d’applet de commande.  
  
-   Si vous utilisez adaptée aux clusters distants\-la mise à jour en mode lorsque vous êtes connecté avec un compte qui n’a pas les autorisations et les droits d’administrateur local sur les nœuds de cluster, vous devez exécuter les outils adaptée aux clusters en tant qu’administrateur à l’aide d’un compte d’administrateur local sur le Mettre à jour de l’ordinateur coordinateur ou en utilisant un compte qui possède le **emprunter l’identité d’un client après authentification** droit d’utilisateur. 
  
-   Pour exécuter le Best Practices Analyzer adaptée aux clusters, vous devez utiliser un compte disposant des privilèges d’administrateur sur les nœuds de cluster et des privilèges d’administrateur local sur l’ordinateur qui est utilisé pour exécuter le [Test-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) applet de commande ou à analyser mise à jour de la disponibilité à l’aide de la fenêtre de la mise à jour adaptée aux clusters de cluster. Pour plus d'informations, voir [Tester l'état de préparation aux mises à jour de cluster](#BKMK_BPA).  
  
## <a name="verify-the-cluster-configuration"></a>Vérifier la configuration du cluster  
Voici les conditions requises généralement pour qu'un cluster de basculement puisse prendre en charge les mises à jour à l'aide de la Mise à jour adaptée aux clusters. Des conditions requises supplémentaires en matière de configuration pour l’administration à distance des nœuds sont répertoriées dans [Configurer les nœuds pour l'administration à distance](#BKMK_NODE_CONFIG) plus loin dans cette rubrique.  
  
-   Suffisamment de nœuds de cluster doivent être en ligne pour permettre au cluster de disposer d'un quorum.  
  
-   Tous les nœuds de cluster doivent se trouver dans le même domaine Active Directory.  
  
-   Le nom du cluster doit être résolu sur le réseau via DNS.  
  
-   Si adaptée aux clusters est utilisée dans à distance\-la mise à jour en mode, l’ordinateur coordinateur de mise à jour doit être connecté aux nœuds du cluster de basculement, et il doit être dans le même domaine Active Directory en tant que le cluster de basculement.  
  
-   Le service de cluster doit s'exécuter sur tous les nœuds de cluster. Par défaut, ce service est installé sur tous les nœuds de cluster et est configuré pour démarrer automatiquement.  
  
-   Pour utiliser PowerShell pre\-mettre à jour ou valider\-mise à jour de scripts durant une exécution de la mise à jour adaptée aux clusters, assurez-vous que les scripts sont installés sur tous les nœuds de cluster ou qu’ils sont accessibles à tous les nœuds, par exemple, sur un partage de fichiers réseau hautement disponible. Si les scripts sont enregistrés sur un partage de fichiers réseau, configurez le dossier avec une autorisation d'accès en lecture pour le groupe Tout le monde.  
  
## <a name="BKMK_NODE_CONFIG"></a>Configurer les nœuds pour la gestion à distance  
Pour utiliser la mise à jour adaptée aux clusters, tous les nœuds du cluster doivent être configurés pour la gestion à distance. Par défaut, la seule tâche à effectuer pour configurer les nœuds pour la gestion à distance est à [activer une règle de pare-feu pour permettre les redémarrages automatiques](#BKMK_FW). 

Le tableau suivant répertorie les exigences de gestion à distance terminée, dans le cas où votre environnement diffère des valeurs par défaut.

Ces conditions requises s'ajoutent aux exigences d'[installation de la fonctionnalité et des outils de clustering de basculement](#BKMK_REQ_CLUS), ainsi qu'aux prescriptions générales en matière de clustering décrites dans les sections précédentes de cette rubrique.  
  
|Condition requise|État par défaut|Self\-en mode de mise à jour|À distance\-en mode de mise à jour|  
|---------------|---|-----------------------|-------------------------|  
|[Activer une règle de pare-feu pour permettre les redémarrages automatiques](#BKMK_FW)|Désactivée|Obligatoire sur tous les nœuds de cluster si un pare-feu est en cours d'utilisation|Obligatoire sur tous les nœuds de cluster si un pare-feu est en cours d'utilisation|  
|[Activer l’Instrumentation de gestion Windows](#BKMK_WMI)|Enabled|Obligatoire sur tous les nœuds de cluster|Obligatoire sur tous les nœuds de cluster|  
|[Activer Windows PowerShell 3.0 ou 4.0 et la communication à distance de Windows PowerShell](#BKMK_PS)|Enabled|Obligatoire sur tous les nœuds de cluster|Obligatoire sur tous les nœuds de cluster pour exécuter ce qui suit :<br /><br />-Le [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps) applet de commande<br />-PowerShell pre\-mettre à jour et valider\-mettre à jour les scripts durant une exécution de la mise à jour<br />-Tests de disponibilité à l’aide de la fenêtre de la mise à jour adaptée aux clusters de la mise à jour de cluster ou le [Test\-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) applet de commande Windows PowerShell|  
|[Installez .NET Framework 4.6 ou 4.5](#BKMK_NET)|Enabled|Obligatoire sur tous les nœuds de cluster|Obligatoire sur tous les nœuds de cluster pour exécuter ce qui suit :<br /><br />-Le [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps) applet de commande<br />-PowerShell pre\-mettre à jour et valider\-mettre à jour les scripts durant une exécution de la mise à jour<br />-Tests de disponibilité à l’aide de la fenêtre de la mise à jour adaptée aux clusters de la mise à jour de cluster ou le [Test\-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) applet de commande Windows PowerShell|  

### <a name="BKMK_FW"></a>Activer une règle de pare-feu pour permettre les redémarrages automatiques  
Pour permettre les redémarrages automatiques après application des mises à jour \(si l’installation d’une mise à jour nécessite un redémarrage\), si les pare-feu Windows ou un non\-Microsoft pare-feu est en cours d’utilisation sur les nœuds de cluster, une règle de pare-feu doit être activée sur chaque nœud autorisant le trafic suivant :  
  
-   Protocole : TCP  
  
-   Sens : entrant  
  
-   Programme : wininit.exe  
  
-   Ports : Ports dynamiques RPC  
  
-   Profil : domaine.  
  
Si le Pare-feu Windows est utilisé sur les nœuds de cluster, vous pouvez activer le groupe de règles du Pare-feu Windows **Arrêt à distance** sur chaque nœud de cluster. Lorsque vous utilisez la fenêtre de la mise à jour adaptée aux clusters pour appliquer les mises à jour et à configurer le\-options, mise à jour le **arrêt à distance** groupe de règles de pare-feu de Windows est automatiquement activé sur chaque nœud du cluster.  
  
> [!NOTE]  
> Le groupe de règles du Pare-feu Windows **Remote Shutdown** ne peut pas être activé quand il est en conflit avec des paramètres de stratégie de groupe configurés pour le Pare-feu Windows.    
  
Le **arrêt à distance** groupe de règles de pare-feu est également activée en spécifiant le **– EnableFirewallRules** paramètre lorsque vous exécutez les applets de commande suivantes : [Ajouter-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole?view=win10-ps), [Invoke-CauRun](https://docs.microsoft.com/powershell/module/clusterawareupdating/Invoke-CauRun?view=win10-ps), et [SetCauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Set-CauClusterRole?view=win10-ps).  
  
L’exemple PowerShell suivant montre une méthode supplémentaire pour permettre le redémarrage automatique sur un nœud de cluster.  
  
```PowerShell  
Set-NetFirewallRule -Group "@firewallapi.dll,-36751" -Profile Domain -Enabled true  
```  

### <a name="BKMK_WMI"></a>Activer Windows Management Instrumentation (WMI) 
Tous les nœuds de cluster doivent être configurés pour la gestion à distance à l’aide de Windows Management Instrumentation \(WMI\). Ceci est activé par défaut.  
  
Pour activer manuellement l'administration à distance, procédez comme suit :  
  
1.  Dans la console Services, démarrez le service **Gestion à distance de Windows** , puis définissez son type de démarrage comme étant **Automatique**.  
  
2.  Exécutez le [Set-WSManQuickConfig](https://docs.microsoft.com/powershell/module/Microsoft.WsMan.Management/Set-WSManQuickConfig?view=powershell-6) applet de commande, ou exécutez la commande suivante à partir d’une commande avec élévation de privilèges invite :  
  
    ```PowerShell  
    winrm quickconfig -q  
    ```  
  
Pour prendre en charge de communication à distance WMI, si le pare-feu Windows est en cours d’utilisation sur les nœuds de cluster, le pare-feu de trafic entrant de règle pour **Windows Remote Management \(HTTP\-dans\)**  doit être activé sur chaque nœud.  Par défaut, cette règle est activée.  
  
### <a name="BKMK_PS"></a>Activer Windows PowerShell et communication à distance de Windows PowerShell  
Pour activer auto\-mise à jour du mode et certaines fonctionnalités adaptée aux clusters distants\-la mise à jour en mode, PowerShell doit être installé et activé pour exécuter des commandes à distance sur tous les nœuds de cluster. Par défaut, PowerShell est installé et activé pour la communication à distance.  
  
Pour activer la communication à distance PowerShell, utilisez une des méthodes suivantes :  
  
-   Exécutez le [Enable-PSRemoting](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Enable-PSRemoting) applet de commande.  
  
-   Configurer un domaine\-paramètre de stratégie de groupe pour la gestion à distance de Windows de niveau \(WinRM\).  
  
Pour plus d’informations sur l’activation de communication à distance PowerShell, consultez [About_remote_requirements](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_remote_requirements?view=powershell-6).  
  
### <a name="BKMK_NET"></a>Installez .NET Framework 4.6 ou 4.5  
Pour activer auto\-mise à jour du mode et certaines fonctionnalités adaptée aux clusters distants\-la mise à jour à la mode, .NET Framework 4.6 ou .NET Framework 4.5 (sur Windows Server 2012 R2) doit être installé sur tous les nœuds de cluster. Par défaut, .NET Framework est installé.  

Pour installer .NET Framework 4.6 (ou 4.5) à l’aide de PowerShell s’il n’est pas déjà installé, utilisez la commande suivante :

```PowerShell
Install-WindowsFeature -Name NET-Framework-45-Core
```

## <a name="BKMK_BEST_PRAC"></a>Meilleures pratiques recommandées pour l’utilisation de la mise à jour adaptée aux clusters 
  
### <a name="BKMK_BP_WUA"></a>Recommandations pour l’application des mises à jour Microsoft

Nous vous recommandons quand vous commencez à utiliser adaptée aux clusters pour appliquer les mises à jour avec la valeur par défaut **Microsoft.WindowsUpdatePlugin** Branchez\-sur un cluster, permet d’arrêter à l’aide d’autres méthodes pour installer les mises à jour logicielles à partir de Microsoft sur le cluster nœuds.  
  
> [!CAUTION]  
> Combinaison adaptée aux clusters avec des méthodes qui mettent à jour automatiquement les nœuds individuels \(selon une planification fixe\) peut provoquer des résultats imprévisibles, notamment des interruptions de service et les temps d’arrêt non planifié.  
  
Nous vous conseillons de suivre les recommandations suivantes :  
  
-   Pour des résultats optimaux, nous vous recommandons de désactiver la mise à jour automatique sur les nœuds de cluster, par exemple, via les paramètres de l'application Mises à jour automatiques du Panneau de configuration, ou via les paramètres configurés à l'aide de la stratégie de groupe.  
  
    > [!CAUTION]  
    > L'installation automatique des mises à jour sur les nœuds de cluster peut interférer avec l'installation des mises à jour par la Mise à jour adaptée aux clusters et peut provoquer des défaillances de la Mise à jour adaptée aux clusters.  
  
    S'ils sont nécessaires, les paramètres suivants des mises à jour automatiques sont compatibles avec la Mise à jour adaptée aux clusters, car l'administrateur peut contrôler le moment de l'installation des mises à jour :  
  
    -   Paramètres de notification avant le téléchargement des mises à jour et de notification avant l'installation  
  
    -   Paramètres de téléchargement automatique des mises à jour et de notification avant l'installation  
  
    Toutefois, si les mises à jour automatiques téléchargent des mises à jour pendant qu'une Exécution de mise à jour est effectuée par la Mise à jour adaptée aux clusters, l'Exécution de mise à jour risque de durer plus longtemps.  
  
-   Ne configurez pas un système de mise à jour tels que Windows Server Update Services \(WSUS\) pour appliquer automatiquement les mises à jour \(selon une planification fixe\) aux nœuds de cluster.  
  
-   Tous les nœuds de cluster doivent être uniformément configurés pour utiliser la même source de mise à jour, par exemple, un serveur WSUS, Windows Update ou Microsoft Update.  
  
-   Si vous utilisez un système de gestion de la configuration sur des ordinateurs du réseau, excluez les nœuds de cluster de toutes les mises à jour obligatoires ou automatiques. Microsoft System Center Configuration Manager 2007 et Microsoft System Center Virtual Machine Manager 2008 constituent des exemples de systèmes de gestion de la configuration.  
  
-   Si les serveurs de distribution de logiciels internes \(, par exemple, les serveurs WSUS\) sont utilisés pour contenir et déployer les mises à jour, assurez-vous que ces serveurs identifient correctement les mises à jour approuvées pour les nœuds de cluster.  
  
#### <a name="BKMK_PROXY"></a>Appliquer des mises à jour de Microsoft dans les scénarios de succursale  
Pour télécharger des mises à jour Microsoft à partir de Microsoft Update ou Windows Update vers des nœuds de cluster dans certains scénarios impliquant des filiales, vous devrez peut-être configurer les paramètres de proxy du compte système local sur chaque nœud. Cela peut être le cas, par exemple, si les clusters de vos filiales ont accès à Microsoft Update ou Windows Update pour télécharger les mises à jour à l'aide d'un serveur proxy local.  
  
Si nécessaire, configurez les paramètres de proxy WinHTTP sur chaque nœud pour spécifier un serveur proxy local et configurer des exceptions d’adresses locales \(, autrement dit, une liste de contournement pour les adresses locales\). Pour ce faire, vous pouvez exécuter la commande suivante sur chaque nœud de cluster à partir d'une invite de commandes avec élévation de privilèges :  
  
```  
netsh winhttp set proxy <ProxyServerFQDN >:<port> "<local>"  
```  
  
où <*ProxyServerFQDN*> est le nom de domaine complet du serveur proxy et <*port*> est le port sur lequel communiquer (généralement le port 443).  
  
Par exemple, pour configurer les paramètres de proxy WinHTTP pour le compte système Local en spécifiant le serveur proxy *MyProxy.CONTOSO.com*, avec le port 443 et les exceptions d’adresses locales, tapez la commande suivante :  
  
```  
netsh winhttp set proxy MyProxy.CONTOSO.com:443 "<local>"  
```  
  
### <a name="BKMK_BP_HF"></a>Recommandations pour l’utilisation de Microsoft.HotfixPlugin  
  
-   Nous vous recommandons de configurer les autorisations du dossier racine de correctifs logiciels et du fichier de configuration de correctif logiciel pour restreindre l'accès en écriture aux administrateurs locaux sur les ordinateurs qui servent à stocker ces fichiers. Cela permet d'éviter la falsification de ces fichiers par des utilisateurs non autorisés qui peuvent compromettre la fonctionnalité du cluster de basculement durant l'application des correctifs logiciels.  
  
-   Pour garantir l’intégrité des données pour le bloc de message serveur \(SMB\) connexions qui sont utilisées pour accéder au dossier racine de correctif logiciel, vous devez configurer le chiffrement SMB dans le dossier partagé SMB, s’il est possible de le configurer. **Microsoft.HotfixPlugin** nécessite qu'une signature SMB ou un chiffrement SMB soit configuré pour garantir l'intégrité des données des connexions SMB. 
  
    Pour plus d’informations, consultez [restreindre l’accès au dossier racine de correctifs logiciels et au fichier de configuration correctif](cluster-aware-updating-plug-ins.md#BKMK_ACL).
  
### <a name="additional-recommendations"></a>Recommandations supplémentaires  
  
-   Pour éviter toute interférence avec une Exécution de mise à jour de la Mise à jour adaptée aux clusters, ne planifiez aucun changement de mot de passe pour les objets noms de cluster et les objets ordinateurs virtuels durant les fenêtres de maintenance planifiée.  
  
-   Vous devez définir les autorisations appropriées sur pre\-mettre à jour et valider\-les scripts de mise à jour qui sont enregistrés sur le réseau partagés dossiers afin d’éviter la falsification de ces fichiers par les utilisateurs non autorisés.  
  
-   Pour configurer adaptée aux clusters dans self\-mode, un objet ordinateur virtuel de la mise à jour \(VCO\) pour cette dernière rôle en cluster doit être créé dans Active Directory. La Mise à jour adaptée aux clusters peut créer cet objet automatiquement au moment où le rôle en cluster de la Mise à jour adaptée aux clusters est ajouté, si le cluster de basculement dispose des autorisations suffisantes. Toutefois, en raison des stratégies de sécurité de certaines organisations, il est parfois nécessaire de prédéfinir l'objet dans Active Directory. Pour connaître la procédure à suivre, voir les [étapes de prédéfinition d’un compte pour un rôle en cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731002\(v=ws.10\)#steps-for-prestaging-the-cluster-name-account).  
  
-   Pour enregistrer et réutiliser les paramètres d'Exécution de mise à jour sur les clusters de basculement ayant des besoins de mise à jour informatique similaires, vous pouvez créer des profils d'Exécution de mise à jour. En outre, selon le mode de mise à jour, vous pouvez enregistrer et gérer les profils d'Exécution de mise à jour sur un partage de fichiers accessible à l'ensemble des ordinateurs coordinateurs de mise à jour distants ou clusters de basculement. Pour plus d’informations, consultez [Options avancées et la mise à jour de profils d’exécution adaptée aux clusters](cluster-aware-updating-options.md).  
  
## <a name="BKMK_BPA"></a>Mise à jour de la préparation du cluster de test  
Vous pouvez exécuter le Best Practices Analyzer adaptée aux clusters \(BPA\) modèle pour tester si un cluster de basculement et de l’environnement réseau répondent à la plupart de la configuration requise pour les mises à jour logicielles appliquées par adaptée aux clusters. Nombre de ces tests vérifier l’environnement et pour la préparation à l’application des mises à jour Microsoft à l’aide de la prise par défaut\-in, **Microsoft.WindowsUpdatePlugin**.  
  
> [!NOTE]  
> Vous devrez peut-être valider indépendamment que votre environnement de cluster est prêt à appliquer les mises à jour logicielles à l’aide d’un plug\-dans d’autres que **Microsoft.WindowsUpdatePlugin**. Si vous utilisez un non\-Microsoft plug\-, tel que celui fourni par le fabricant de votre matériel, contactez l’éditeur pour plus d’informations.  
  
Vous pouvez exécuter l'outil BPA des deux façons suivantes :  
  
1.  Sélectionnez **Analyser la disponibilité de la mise à jour du cluster** dans la console de la Mise à jour adaptée aux clusters. L’outil BPA après les tests de disponibilité, un rapport de test s’affiche. Si des problèmes sont détectés sur des nœuds de cluster, les problèmes et les nœuds où ils apparaissent sont identifiés pour que vous puissiez prendre des mesures correctives. Les tests peuvent durer plusieurs minutes.  
  
2.  Exécutez le [Test-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup) applet de commande. Vous pouvez exécuter l’applet de commande sur un ordinateur local ou distant sur lequel le basculement Clustering Module pour Windows PowerShell (partie sur les outils de Clustering de basculement) est installé. Vous pouvez également exécuter l'applet de commande sur un nœud du cluster de basculement.  
  
> [!NOTE]  
> -   Vous devez utiliser un compte disposant des privilèges d’administrateur sur les nœuds de cluster et des privilèges d’administrateur local sur l’ordinateur qui est utilisé pour exécuter le **Test\-CauSetup** applet de commande ou pour analyser la mise à jour de la disponibilité du cluster à l’aide de la fenêtre de la mise à jour adaptée aux clusters. Pour exécuter les tests à l’aide de la fenêtre de la mise à jour adaptée aux clusters, vous devez être connecté à l’ordinateur avec les informations d’identification nécessaires.  
> -   Les tests partent du principe que les outils de la Mise à jour adaptée aux clusters utilisés pour obtenir un aperçu et appliquer les mises à jour de logiciels sont exécutés à partir du même ordinateur et avec les mêmes informations d'identification utilisateur que pour tester l'état de préparation aux mises à jour de cluster.  
  
> [!IMPORTANT]  
> Nous vous recommandons fortement de tester l'état de préparation aux mises à jour du cluster dans les situations suivantes :  
>   
> -   avant d'utiliser la Mise à jour adaptée aux clusters pour la première fois pour appliquer des mises à jour de logiciels ;  
> -   après avoir ajouté un nœud au cluster ou effectué d'autres changements sur le matériel du cluster, qui nécessitent l'exécution de l'Assistant Validation de cluster ;  
> -   Une fois que vous modifiez une source de mise à jour, ou les paramètres de mise à jour ou des configurations \(autre qu’adaptée aux clusters\) qui peuvent affecter l’application des mises à jour sur les nœuds.  
  
### <a name="tests-for-cluster-updating-readiness"></a>Tests de l'état de préparation aux mises à jour de cluster  
Le tableau ci-dessous présente les tests de l'état de préparation des mises à jour de cluster, certains problèmes fréquents et les étapes de résolution.  
  
|Tester|Problèmes et impacts possibles|Étapes de résolution|  
|--------|-------------------------------|--------------------|  
|Le cluster de basculement doit être disponible|Impossible de résoudre le nom du cluster de basculement, ou un ou plusieurs nœuds de cluster ne sont pas accessibles. L'outil BPA ne peut pas exécuter les tests de l'état de préparation du cluster.|-Vérifiez l’orthographe du nom du cluster spécifié pendant l’exécution de l’outil BPA.<br />-Assurez-vous que tous les nœuds du cluster sont en ligne et en cours d’exécution.<br />-Vérifiez que la validation d’un Assistant de Configuration s’exécute sous le cluster de basculement.|  
|Les nœuds de cluster de basculement doivent être activés pour permettre l'administration à distance via WMI|Un ou plusieurs nœuds de cluster de basculement ne sont pas activés pour la gestion à distance à l’aide de Windows Management Instrumentation \(WMI\). La Mise à jour adaptée aux clusters ne peut pas mettre à jour les nœuds de cluster s'ils ne sont pas configurés pour l'administration à distance.|Assurez-vous que tous les nœuds de cluster de basculement sont activés pour l'administration à distance via WMI. Pour plus d'informations, consultez [Configurer les nœuds pour l'administration à distance](#BKMK_NODE_CONFIG) dans cette rubrique.|  
|PowerShell doit être activée sur chaque nœud de cluster de basculement|PowerShell n’est pas installé ou n’est pas activé pour la communication à distance sur un ou plusieurs nœuds de cluster de basculement. Adaptée aux clusters ne peut pas être configurée pour self\-en mode de mise à jour ou utiliser certaines fonctionnalités distants\-en mode de mise à jour.|Vérifiez que PowerShell est installé sur tous les nœuds de cluster est activée pour la communication à distance.<br /><br />Pour plus d'informations, consultez [Configurer les nœuds pour l'administration à distance](#BKMK_NODE_CONFIG) dans cette rubrique.|  
|Version du cluster de basculement|N’exécutent pas un ou plusieurs nœuds du cluster de basculement Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012. La Mise à jour adaptée aux clusters ne peut pas mettre à jour le cluster de basculement.|Vérifiez que le cluster de basculement qui est spécifié pendant l’exécution de l’outil BPA exécute Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.<br /><br />Pour plus d'informations, consultez [Vérifier la configuration du cluster](#BKMK_REQ_CLUS) dans cette rubrique.|  
|Les versions nécessaires du .NET Framework et de Windows PowerShell doivent être installées sur tous les nœuds de cluster de basculement|.NET framework 4.6, 4.5 ou Windows PowerShell n’est pas installé sur un ou plusieurs nœuds de cluster. Certaines fonctionnalités de la Mise à jour adaptée aux clusters risquent de ne pas fonctionner.|Assurez-vous que .NET Framework 4.6 ou 4.5 et Windows PowerShell sont installés sur tous les nœuds de cluster, s’ils sont requis.<br /><br />Pour plus d'informations, consultez [Configurer les nœuds pour l'administration à distance](#BKMK_NODE_CONFIG) dans cette rubrique.|  
|Le service de cluster doit s'exécuter sur tous les nœuds de cluster|Le service de cluster ne s'exécute pas sur un ou plusieurs nœuds. La Mise à jour adaptée aux clusters ne peut pas mettre à jour le cluster de basculement.|-Vérifiez que le service de Cluster \(clussvc\) est démarré sur tous les nœuds du cluster, et il est configuré pour démarrer automatiquement.<br />-Vérifiez que la validation d’un Assistant de Configuration s’exécute sous le cluster de basculement.<br /><br />Pour plus d'informations, consultez [Vérifier la configuration du cluster](#BKMK_REQ_CLUS) dans cette rubrique.|  
|Les mises à jour automatiques ne doivent pas être configurées pour installer automatiquement les mises à jour sur un nœud de cluster de basculement|Sur un nœud de cluster de basculement au moins, les mises à jour automatiques sont configurées pour installer automatiquement les mises à jour Microsoft. L'association de la Mise à jour adaptée aux clusters à d'autres méthodes de mise à jour peut entraîner des temps d'arrêt imprévus ou des résultats imprévisibles.|Si la fonctionnalité Windows Update est configurée pour les mises à jour automatiques sur un ou plusieurs nœuds de cluster, assurez-vous que les mises à jour automatiques ne sont pas configurées pour installer automatiquement les mises à jour.<br /><br />Pour plus d'informations, voir [Recommandations pour l'application des mises à jour Microsoft](#BKMK_BP_WUA).|  
|Les nœuds de cluster de basculement doivent utiliser la même source de mise à jour|Un ou plusieurs nœuds de cluster de basculement sont configurés pour utiliser une autre source de mise à jour pour les mises à jour Microsoft que le reste des nœuds. Les mises à jour risquent de ne pas être appliquées de manière uniforme sur les nœuds de cluster par la Mise à jour adaptée aux clusters.|Assurez-vous que chaque nœud de cluster est configuré pour utiliser la même source de mise à jour, par exemple, un serveur WSUS, Windows Update ou Microsoft Update.<br /><br />Pour plus d'informations, voir [Recommandations pour l'application des mises à jour Microsoft](#BKMK_BP_WUA).|  
|Une règle de pare-feu qui permet l'arrêt à distance doit être activée sur chaque nœud du cluster de basculement|Un ou plusieurs nœuds de cluster de basculement n'ont pas de règle de pare-feu activée qui permet l'arrêt à distance, ou un paramètre de stratégie de groupe empêche cette règle d'être activée. Une Exécution de mise à jour qui applique des mises à jour nécessitant le redémarrage automatique des nœuds risque de ne pas s'effectuer correctement.|Si les pare-feu Windows ou un non\-Microsoft pare-feu est en cours d’utilisation sur les nœuds de cluster, configurer une règle de pare-feu qui permet l’arrêt à distance.<br /><br />Pour plus d'informations, voir [Activer une règle de pare-feu pour permettre les redémarrages automatiques](#BKMK_FW) dans cette rubrique.|  
|Le paramètre de serveur proxy de chaque nœud de cluster de basculement doit être défini en fonction d'un serveur proxy local|Un ou plusieurs nœuds de cluster de basculement ont une configuration de serveur proxy incorrecte.<br /><br />Si un serveur proxy local est en cours d'utilisation, le paramètre de serveur proxy de chaque nœud doit être configuré correctement pour permettre au cluster d'accéder à Microsoft Update ou Windows Update.|Assurez-vous que les paramètres de proxy WinHTTP de chaque nœud de cluster sont définis en fonction d'un serveur proxy local, le cas échéant. Si aucun serveur proxy n'est utilisé dans votre environnement, cet avertissement peut être ignoré.<br /><br />Pour plus d'informations, consultez [Apply updates in branch office scenarios](#BKMK_PROXY) dans cette rubrique.|  
|Le rôle en cluster adaptée aux clusters doit être installé sur le cluster de basculement pour activer auto\-en mode de mise à jour|Le rôle en cluster de la Mise à jour adaptée aux clusters n'est pas installé sur ce cluster de basculement. Ce rôle est nécessaire pour cluster self\-la mise à jour.|Pour utiliser adaptée aux clusters dans self\-mise à jour du mode, ajoutez le rôle en cluster adaptée aux clusters sur le cluster de basculement dans une des manières suivantes :<br /><br />-Exécuter le [Add-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole) applet de commande PowerShell.<br />-Sélectionnez la **configurer cluster self\-options mise à jour** action dans la fenêtre de la mise à jour adaptée aux clusters.|  
|Le rôle en cluster adaptée aux clusters doit être activé sur le cluster de basculement pour activer auto\-en mode de mise à jour|Le rôle en cluster de la Mise à jour adaptée aux clusters est désactivé. Par exemple, le rôle en cluster adaptée aux clusters n’est pas installé ou il a été désactivé à l’aide de la [désactiver\-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole) applet de commande PowerShell. Ce rôle est nécessaire pour cluster self\-la mise à jour.|Pour utiliser adaptée aux clusters dans self\-mise à jour du mode, activer le rôle en cluster adaptée aux clusters sur ce cluster de basculement dans une des manières suivantes :<br /><br />-Exécuter le [Enable-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole) applet de commande PowerShell.<br />-Sélectionnez la **configurer cluster self\-options mise à jour** action dans la fenêtre de la mise à jour adaptée aux clusters.|  
|Le plug adaptée aux clusters configuré\-dans automatique\-en mode de mise à jour doit être enregistré sur tous les nœuds de cluster de basculement|Le rôle en cluster adaptée aux clusters sur un ou plusieurs nœuds de ce cluster de basculement ne peut pas accéder à la prise d’adaptée aux clusters\-dans le module qui est configuré dans self\-options mise à jour. Un libre-service\-la mise à jour à l’exécution risque d’échouer.|-Vérifiez que le plug adaptée aux clusters configuré\-dans est installé sur tous les nœuds de cluster en suivant la procédure d’installation pour le produit qui fournit la prise d’adaptée aux clusters\-dans.<br />-Exécuter le [inscrire\-CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/Register-CauPlugin) applet de commande PowerShell pour inscrire le plug\-dans sur les nœuds de cluster requis.|  
|Tous les nœuds de cluster de basculement doivent avoir le même jeu du plug-in d’inscrits adaptée aux clusters\-ins|Un libre-service\-la mise à jour à exécuter peut échouer si le plug\-qui est configuré pour être utilisé dans une exécution de la mise à jour est modifié pour qu’il n’est pas disponible sur tous les nœuds de cluster.|-Vérifiez que le plug adaptée aux clusters configuré\-dans est installé sur tous les nœuds de cluster en suivant la procédure d’installation pour le produit qui fournit la prise d’adaptée aux clusters\-dans.<br />-Exécuter le [inscrire\-CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/Register-CauPlugin) applet de commande PowerShell pour inscrire le plug\-dans sur les nœuds de cluster requis.|  
|Les options configurées pour l'Exécution de mise à jour doivent être valides|Self\-mise à jour de la planification et exécution de la mise à jour des options qui sont configurées pour ce cluster de basculement sont incomplètes ou ne sont pas valides. Un libre-service\-la mise à jour à l’exécution risque d’échouer.|Configurer un libre-service valide\-mise à jour du calendrier et définir des options d’exécution de la mise à jour. Par exemple, vous pouvez utiliser la [définir\-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Set-CauClusterRole) rôle en cluster l’applet de commande PowerShell pour configurer cette dernière.|  
|Au moins deux nœuds de cluster de basculement doivent être propriétaires du rôle en cluster de la Mise à jour adaptée aux clusters|Une mise à jour exécution lancé en self\-en mode de mise à jour échoue, car le rôle en cluster adaptée aux clusters n’a pas un nœud propriétaire possible.|Utilisez les outils de clustering de basculement pour vous assurer que tous les nœuds de cluster sont configurés en tant que propriétaires possibles du rôle en cluster de la Mise à jour adaptée aux clusters. Il s'agit de la configuration par défaut.||  
|Tous les nœuds de cluster de basculement doivent avoir accès aux scripts Windows PowerShell|Pas tous les nœuds propriétaires possibles du rôle en cluster adaptée aux clusters peuvent accéder à la version antérieure de Windows PowerShell configurée\-mettre à jour et valider\-mettre à jour les scripts. Un libre-service\-la mise à jour à l’exécution échoue.|Assurez-vous que tous les nœuds propriétaires possibles du rôle en cluster adaptée aux clusters disposent des autorisations pour accéder à la version antérieure de PowerShell configurée\-mettre à jour et valider\-mettre à jour les scripts.|  
|Tous les nœuds de cluster de basculement doivent utiliser des scripts Windows PowerShell identiques|Pas tous les nœuds propriétaires possibles du rôle en cluster adaptée aux clusters utilisent la même copie de la version antérieure de Windows PowerShell spécifiée\-mettre à jour et valider\-mettre à jour les scripts. Un libre-service\-la mise à jour à exécuter peut échouer ou produire un comportement inattendu.|Assurez-vous que tous les nœuds propriétaires possibles du rôle en cluster adaptée aux clusters utilisent la même version antérieure de PowerShell\-mettre à jour et valider\-mettre à jour les scripts.|  
|Le paramètre WarnAfter spécifié pour l'Exécution de mise à jour doit être inférieur au paramètre StopAfter|Les délais d'expiration de l'Exécution de mise à jour pour la Mise à jour adaptée aux clusters rendent l'avertissement relatif au délai d'expiration inefficace. Une Exécution de mise à jour peut être annulée avant qu'un journal des événements d'avertissement puisse être généré.|Dans les options de l'Exécution de mise à jour, configurez une valeur pour l'option **WarnAfter** qui soit inférieure à la valeur de l'option **StopAfter** .|  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Vue d’ensemble de la mise à jour adaptée aux clusters](cluster-aware-updating.md)