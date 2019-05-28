---
title: Nouveautés de Windows Server2016
description: Présentation des nouvelles fonctionnalités de calcul, d’identité, de gestion, d’automatisation, de mise en réseau, de sécurité et de stockage.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 05/21/2019
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2827f332-44d4-4785-8b13-98429087dcc7
author: jasongerend
ms.author: jgerend
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 8d36961558066197a54f42d27a3560d653bd81f2
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976634"
---
# <a name="whats-new-in-windows-server-2016"></a>Nouveautés de Windows Server2016

>S’applique à : Windows Server 2016

<img src="media/whats-new.png" style='float:left; padding:.5em;' alt="Icon showing a newspaper">Pour en savoir plus sur les dernières fonctionnalités dans Windows, consultez [What ' s New in Windows Server](whats-new-in-windows-server.md). Le contenu de cette section décrit les nouveautés et les modifications de Windows Server&reg; 2016. Les nouvelles fonctionnalités et modifications répertoriées ici sont celles qui sont susceptibles d'avoir l'impact le plus important quand vous utilisez cette version.  

## <a name="computevirtualizationvirtualizationmd"></a>[Compute](../virtualization/virtualization.md)

Le domaine de virtualisation contient des fonctionnalités et produits de virtualisation que les professionnels de l’informatique peuvent utiliser pour la conception, le déploiement et la maintenance de Windows Server.  

### <a name="general"></a>Général  
Les machines physiques et virtuelles bénéficient d’une plus grande précision du temps en raison des améliorations apportées aux services Synchronisation date/heure de Hyper-V et de Win32. Windows Server peut désormais héberger des services qui sont conformes aux réglementations à venir qui exigent une précision égale à 1ms en ce qui concerne l’heure UTC.  

### <a name="hyper-v"></a>Hyper-V  
-   [Nouveautés de Hyper-V sur Windows Server 2016](../virtualization/hyper-v/What-s-new-in-Hyper-V-on-Windows.md). Cette rubrique décrit les fonctionnalités nouvelles et modifiées du rôle Hyper-V dans Windows Server 2016, Hyper-V client s’exécutant sur Windows 10 et Microsoft Hyper-V Server 2016.  

-   [Les conteneurs Windows](https://msdn.microsoft.com/virtualization/windowscontainers/containers_welcome):  Prise en charge des conteneurs Windows Server 2016 ajoute des améliorations des performances, la gestion de réseau simplifiée et la prise en charge pour les conteneurs Windows sur Windows 10. Pour obtenir des informations supplémentaires sur les conteneurs, consultez [conteneurs : Docker, Windows et les tendances](https://azure.microsoft.com/blog/2015/08/17/containers-docker-windows-and-trends/).  

### <a name="nano-server"></a>Nano Server  
Nouveautés de [Nano Server](getting-started-with-nano-server.md). Nano Server possède maintenant un module mis à jour pour la création d’images Nano Server, et propose notamment une plus grande séparation des fonctionnalités de l’hôte physique et de la machine virtuelle invitée, ainsi que la prise en charge de différentes éditions de Windows Server.   

Des améliorations ont également été apportées à la console de récupération, notamment la séparation des règles de pare-feu entrantes et sortantes ainsi que la possibilité de réparer la configuration de WinRM.  

### <a name="shielded-virtual-machines"></a>Machines virtuelles dotées d’une protection maximale  
Windows Server 2016 fournit une nouvelle machine virtuelle Hyper-V dotée d’une protection maximale, pour protéger une machine virtuelle de deuxième génération contre une structure compromise. Les fonctionnalités introduites dans Windows Server2016 sont les suivantes:  

- Un nouveau mode « Chiffrement pris en charge » qui offre plus de protections que pour une machine virtuelle ordinaire, mais moins que le mode « Protection maximale », tout en prenant toujours en charge le module de plateforme sécurisée (TPM) virtuel, le chiffrement de disque, le chiffrement de trafic de migration dynamique et d’autres fonctionnalités, notamment les avantages d’administration de structure directe tels que les connexions de console de machine virtuelle et Powershell Direct.  

- Prise en charge complète de la conversion de machines virtuelles de génération2 non protégées existantes en machines virtuelles dotées d’une protection maximale, dont le chiffrement automatique du disque.

- Hyper-V Virtual Machine Manager peut désormais afficher les structures sur lesquelles une machine virtuelle dotée d’une protection maximale peut s’exécuter, ce qui permet à l’administrateur de la structure d’ouvrir le protecteur de clé d’une machine virtuelle dotée d’une protection maximale et d’afficher les structures sur lesquelles l’exécution est autorisée.  

- Vous pouvez changer de mode d’attestation sur un service Guardian hôte en cours d’exécution. Vous pouvez maintenant basculer sur-le-champ entre l’attestation Active Directory moins sécurisée, mais plus simple, et l’attestation basée sur le module de plateforme sécurisée.  

- Des outils de diagnostic de bout en bout basés sur Windows PowerShell qui sont en mesure de détecter des problèmes de configuration ou des erreurs à la fois dans les hôtes Hyper-V protégés et le service Guardian hôte.  

- Un environnement de récupération qui offre un moyen de résoudre les problèmes des machines virtuelles dotées d’une protection maximale et de les réparer en toute sécurité au sein de la structure dans laquelle elles s’exécutent normalement tout en offrant le même niveau de protection que la machine virtuelle dotée d’une protection maximale elle-même.

- Le service Guardian hôte prend en charge une instance Active Directory sécurisée: vous pouvez demander au service Guardian hôte d’utiliser une forêt Active Directory comme la sienne au lieu de créer sa propre instance Active Directory.

Pour obtenir plus d’informations et des instructions sur l’utilisation des machines virtuelles dotées d’une protection maximale, voir [Shielded VMs and Guarded Fabric Validation Guide for Windows Server 2016 (TPM)](https://aka.ms/shieldedvms) (Guide de validation de structure protégée et de machines virtuelles dotées d’une protection maximale pour Windows Server2016 [TPM]).  

## <a name="identity-and-accessidentityidentity-and-accessmd"></a>[Identité et accès](../identity/Identity-and-Access.md)  
De nouvelles fonctionnalités d’identité améliorent la capacité des organisations à sécuriser les environnements Active Directory et leur permettent de migrer vers les déploiements de cloud uniquement et les déploiements hybrides, où certains services et applications sont hébergés dans le cloud et d’autres sont hébergés en local.  

### <a name="active-directory-certificate-services"></a>Services de certificats Active Directory  
Services de certificats Active Directory (AD CS) dans Windows Server 2016 augmente la prise en charge pour l’attestation de clé TPM : Vous pouvez maintenant utiliser le KSP de carte à puce pour l’attestation de clé et les appareils qui ne sont pas joints au domaine peuvent maintenant utiliser l’inscription NDES pour obtenir des certificats qui peuvent être attesté par des clés en cours dans un module de plateforme sécurisée.  

### <a name="active-directory-domain-services"></a>Services de domaine Active Directory  
Les services de domaine Active Directory comprennent des améliorations pour aider les organisations à sécuriser les environnements Active Directory et fournir de meilleures expériences de gestion des identités pour les appareils d'entreprise et personnels. Pour plus d’informations, consultez [Quelles sont les nouveautés dans Active Directory Federation Services pour Windows Server 2016](../identity/whats-new-active-directory-domain-services.md).   

### <a name="active-directory-federation-services"></a>Services de fédération Active Directory (AD FS)  
Nouveautés des services de fédération Active Directory. Les services AD FS de Windows Server 2016 incluent de nouvelles fonctionnalités qui vous permettent de configurer AD FS pour authentifier les utilisateurs stockés dans les annuaires LDAP (Lightweight Directory Access Protocol). Pour plus d’informations, voir [Nouveautés des services ADFS pour Windows Server 2016](../identity/ad-fs/overview/whats-new-active-directory-federation-services-windows-server.md).  

### <a name="web-application-proxy"></a>Proxy d'application web  
La dernière version du proxy d’application web se concentre sur les nouvelles fonctionnalités qui permettent la publication et la préauthentification pour des applications supplémentaires et améliorent l’expérience utilisateur. Consultez la liste complète des nouvelles fonctionnalités qui inclut la pré-authentification pour les applications clientes riches comme Exchange ActiveSync et les domaines avec caractères génériques pour une publication plus facile des applications SharePoint. Pour plus d’informations, voir [Proxy d’application web dans Windows Server 2016](../remote/remote-access/web-application-proxy/web-application-proxy-windows-server.md).  

##  <a name="administrationadministrationmanage-windows-servermd"></a>[Administration](../administration/manage-windows-server.md)  
La section Gestion et automatisation fournit des informations sur les outils et références pour les informaticiens professionnels qui souhaitent exécuter et gérer Windows Server 2016, dont Windows PowerShell.

Windows PowerShell 5.1 inclut de nouvelles fonctionnalités importantes, notamment la prise en charge du développement avec les classes, et de nouvelles fonctionnalités de sécurité, qui étendent son utilisation, améliorent sa convivialité et vous permettent de contrôler et gérer des environnements Windows plus facilement et de façon plus globale. Voir [Nouveaux scénarios et fonctionnalités dans WMF 5.1](https://docs.microsoft.com/powershell/wmf/5.1/scenarios-features) pour plus d’informations.

Les nouveautés de Windows Server 2016 incluent la possibilité d’exécuter PowerShell.exe localement sur Nano Server (et non plus seulement à distance), de nouvelles applets de commande Utilisateurs et groupes locaux pour remplacer l’interface utilisateur graphique, la prise en charge du débogage de PowerShell et la prise en charge de Nano Server pour la transcription et la journalisation de la sécurité, ainsi que JEA.

Voici d’autres nouvelles fonctionnalités d’administration :

### <a name="powershell-desired-state-configuration-dsc-in-windows-management-framework-wmf-5"></a>Configuration d’état souhaité Windows PowerShell (DSC) dans Windows Management Framework (WMF) 5
Windows Management Framework 5 inclut des mises à jour de configuration d’état souhaité Windows PowerShell (DSC), Windows Remote Management (WinRM) et Windows Management Instrumentation (WMI).

Pour plus d’informations sur le test des fonctionnalités DSC de Windows Management Framework 5, consultez la série de billets de blog mentionnés dans [Validate features of PowerShell DSC](https://blogs.msdn.microsoft.com/powershell/2015/07/06/validate-features-of-powershell-dsc/) (Validation des fonctionnalités de PowerShell DSC). Pour les télécharger, consultez [Windows Management Framework 5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure).

### <a name="packagemanagement-unified-package-management-for-software-discovery-installation-and-inventory"></a>Gestion unifiée des packages PackageManagement pour la détection, l’installation et l’inventaire des logiciels
Windows Server 2016 et Windows 10 incluent une nouvelle fonctionnalité PackageManagement (auparavant appelée OneGet) qui permet aux professionnels de l’informatique ou développeurs d’automatiser la détection, l’installation et l’inventaire des logiciels, localement ou à distance, quels que soient la technologie d’installation utilisée et l’emplacement des logiciels. 

Pour plus d’informations, voir [https://github.com/OneGet/oneget/wiki](https://github.com/OneGet/oneget/wiki).

### <a name="powershell-enhancements-to-assist-digital-forensics-and-help-reduce-security-breaches"></a>Améliorations de PowerShell pour faciliter la forensique numérique et aider à réduire les violations de sécurité
Pour aider l’équipe responsable de l’examen des systèmes compromis, parfois appelée « l’équipe bleue », nous avons ajouté des fonctionnalités de forensique numérique et de journalisation PowerShell supplémentaires, ainsi que des fonctionnalités destinées à réduire les vulnérabilités dans les scripts, tels que PowerShell limité et à sécuriser les API CodeGeneration.

Pour plus d’informations, consultez [PowerShell ♥ the Blue Team](https://blogs.msdn.microsoft.com/powershell/2015/06/09/powershell-the-blue-team/).

## <a name="networkingnetworkingnetworkingmd"></a>[Mise en réseau](../networking/Networking.md)  
Cette section décrit les fonctionnalités et produits de mise en réseau que les informaticiens professionnels peuvent utiliser pour la conception, le déploiement et la maintenance de Windows Server 2016.  

### <a name="software-defined-networking"></a>Mise en réseau définie par logiciel
Vous pouvez désormais à la fois mettre en miroir et acheminer le trafic vers des équipements virtuels nouveaux ou existants. Avec un pare-feu distribué et des groupes de sécurité réseau, cela vous permet de segmenter et sécuriser de manière dynamique les charges de travail d’une manière similaire à Azure. Ensuite, vous pouvez déployer et gérer l’intégralité de la pile de mise en réseau SDN (Software Defined Networking) à l’aide de System Center Virtual Machine Manager. Enfin, vous pouvez utiliser Docker pour gérer la mise en réseau de conteneurs Windows Server et associer des stratégies de mise en réseau SDN non seulement à des machines virtuelles, mais aussi des conteneurs. Pour plus d’informations, voir [Planifier une mise en réseau SDN (Software Defined Networking)](../networking/sdn/plan/plan-a-software-defined-network-infrastructure.md).

### <a name="tcp-performance-improvements"></a>Améliorations des performances de TCP
La fenêtre de congestion initiale par défaut a été augmentée de 4 à 10 et TCP Fast Open (TFO) a été implémenté. TFO réduit le temps nécessaire pour établir une connexion TCP et la nouvelle fenêtre de congestion initiale permet de transférer des objets plus volumineux dans la rafale initiale. Cette combinaison réduit considérablement le temps nécessaire pour transférer un objet Internet entre le client et le cloud.

Pour améliorer le comportement de TCP en cas de récupération suite à une perte de paquets, nous avons implémenté TCP TLP (Tail Loss Probe) et RACK (Recent Acknowledgement). TLP permet de convertir des délais de retransmission en récupérations rapides, et RACK réduit le temps nécessaire à une récupération rapide pour retransmettre un paquet perdu. 

## <a name="security-and-assurancesecuritysecurity-and-assurancemd"></a>[Sécurité et assurance](../security/Security-and-Assurance.md)  
Contient des fonctionnalités et solutions de sécurité pour professionnels en informatique à déployer dans votre environnement de centre de données et cloud. Pour plus d’informations générales sur la sécurité dans Windows Server 2016, consultez [Sécurité et assurance](../security/Security-and-Assurance.md).  

### <a name="just-enough-administration"></a>Administration suffisante  
Dans Windows Server 2016, Just Enough Administration est une technologie de sécurité qui permet de déléguer l’administration de tout ce qui peut être géré avec Windows PowerShell. Les fonctionnalités incluent la prise en charge de l’exécution sous une identité réseau, la connexion via PowerShell Direct, la copie en toute sécurité de fichiers vers/depuis des points de terminaison JEA (Just Enough Administration) et la configuration de la console PowerShell pour un lancement dans un contexte JEA par défaut. Pour plus d’informations, voir [Administration suffisante sur GitHub](https://aka.ms/JEA).

### <a name="credential-guard"></a>Protection des informations d’identification
La protection des informations d’identification utilise une sécurité basée sur la virtualisation pour isoler les secrets, afin que seuls les logiciels système privilégiés puissent y accéder. Consultez [Protéger les informations d’identification de domaine dérivées avec CredentialGuard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard).

###  <a name="remote-credential-guard"></a>Protection des informations d’identification à distance
Protection des informations d’identification prend en charge les sessions RDP afin que les informations d’identification de l’utilisateur restent côté client et ne soient pas exposées côté serveur. Il fournit également l’authentification unique pour Bureau à distance. Voir [Protéger les informations d’identification de domaine dérivées avec Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard).   

### <a name="device-guard-code-integrity"></a>Protection de périphérique (intégrité du code)
Protection de périphérique assure l’intégrité du code en mode noyau (KMCI) et en mode utilisateur (UMCI), en créant des stratégies qui spécifient le code exécutable sur le serveur. Voir [Introduction à Windows Defender Device Guard : Stratégies d’intégrité du code et de sécurité basée sur la virtualisation](https://docs.microsoft.com/windows/device-security/device-guard/introduction-to-device-guard-virtualization-based-security-and-code-integrity-policies).


### <a name="windows-defender"></a>Windows Defender  
[Windows Defender Overview for Windows Server 2016](../security/windows-defender/windows-defender-overview-windows-server.md) (Présentation de Windows Defender pour Windows Server 2016. Windows Server Antimalware est installé et activé par défaut dans Windows Server 2016, mais pas son interface utilisateur. Toutefois, Windows Server Antimalware va mettre à jour les définitions de logiciel anti-programme malveillant et protéger l'ordinateur sans l'interface utilisateur. Si vous avez besoin de l'interface utilisateur pour Windows Server Antimalware, vous pouvez l'installer après l'installation du système d'exploitation à l'aide de l'Assistant Ajout de rôles et de fonctionnalités.

### <a name="control-flow-guard"></a>Protection du flux de contrôle
Protection de flux de contrôle est une fonctionnalité de sécurité de plateforme qui a été créée pour lutter contre les risques de corruption de mémoire. Pour plus d’informations, consultez [Control Flow Guard](https://msdn.microsoft.com/library/windows/desktop/mt637065(v=vs.85).aspx) (Protection du flux de contrôle).


## <a name="storagestoragestoragemd"></a>[Stockage](../storage/storage.md)

Dans Windows Server 2016, le stockage inclut des nouveautés et des améliorations pour le stockage par logiciel, ainsi que pour les serveurs de fichiers traditionnels. Voici quelques-unes des nouveautés. Pour d’autres améliorations et d’autres détails, consultez [What’s New in Storage in Windows Server 2016](../storage/whats-new-in-storage.md) (Nouveautés concernant le stockage dans Windows Server2016).

### <a name="storage-spaces-direct"></a>Espaces de stockage directs

Les espaces de stockage direct permettent de créer un stockage à haute disponibilité et scalable en utilisant des serveurs avec un stockage local. Le déploiement et la gestion des systèmes de stockage défini par un logiciel sont simplifiés, et il est maintenant possible d’utiliser de nouvelles classes de périphériques de disque, comme SSD SATA et NVMe, ce qui ne pouvait pas se faire auparavant avec les espaces de stockage en cluster avec des disques partagés.

Pour plus d’informations, consultez [Storage Spaces Direct](../storage/storage-spaces/storage-spaces-direct-overview.md) (Espaces de stockage direct).

### <a name="storage-replica"></a>Réplica de stockage

Le réplica de stockage permet une réplication synchrone indépendante du stockage, au niveau du bloc, entre des clusters ou des serveurs pour la récupération d’urgence, ainsi que l’extension d’un cluster de basculement entre des sites. La réplication synchrone permet la mise en miroir des données dans des sites physiques avec des volumes cohérents en cas d’incident, ce qui garantit aucune perte de données au niveau du système de fichiers. La réplication asynchrone permet l’extension de site au-delà de plages métropolitaines avec la possibilité de perte de données.

Pour plus d’informations, consultez [Storage Replica overview](../storage/storage-replica/storage-replica-overview.md) (Présentation du réplica de stockage).

### <a name="storage-quality-of-service-qos"></a>Qualité de service de stockage

Vous pouvez maintenant utiliser la qualité de service (QoS) de stockage pour analyser de manière centralisée les performances de stockage de bout en bout et créer des stratégies de gestion avec Hyper-V et des clusters CSV dans Windows Server 2016.

Pour plus d’informations, consultez [Storage Quality of Service](../storage/storage-qos/storage-qos-overview.md) (Qualité de service de stockage).

## <a name="failover-clusteringfailover-clusteringwhats-new-in-failover-clusteringmd"></a>[Clustering de basculement](../failover-clustering/whats-new-in-failover-clustering.md)

Windows Server2016 inclut plusieurs nouvelles fonctionnalités et améliorations pour plusieurs serveurs regroupés dans un cluster à tolérance de panne, à l’aide de la fonctionnalité Clustering avec basculement. Certains des ajouts sont indiqués ci-dessous. Pour une liste plus complète, consultez [What’s New in Failover Clustering in Windows Server 2016](../failover-clustering/whats-new-in-failover-clustering.md) (Nouveautés du clustering avec basculement dans Windows Server2016.

### <a name="cluster-operating-system-rolling-upgrade"></a>Mise à niveau propagée de système d’exploitation de cluster

La mise à niveau propagée de système d’exploitation de cluster permet à un administrateur de mettre à niveau le système d’exploitation des nœuds de cluster Windows Server 2012 R2 vers Windows Server 2016, sans arrêter les charges de travail du serveur de fichiers avec montée en puissance parallèle ni Hyper-V. Avec cette fonctionnalité, les pénalités de temps d’arrêt sur les contrats de niveau de service peuvent être évitées.

Pour plus d’informations, consultez [Cluster Operating System Rolling Upgrade (Mise à niveau propagée de système d’exploitation de cluster)](../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md).

### <a name="cloud-witness"></a>Témoin cloud

Dans Windows Server 2016, Témoin cloud est un nouveau type de témoin de quorum de cluster avec basculement, qui utilise Microsoft Azure comme point d’arbitrage. Comme les autres témoins de quorum, Témoin cloud obtient un vote et peut prendre part à des calculs de quorum. Vous pouvez le configurer comme témoin de quorum à l’aide de l’Assistant Configuration de quorum du cluster.

Pour plus d’informations, consultez [Deploy a cloud witness for a Failover Cluster](../failover-clustering/deploy-cloud-witness.md) (Déployer un témoin cloud pour un cluster avec basculement).

### <a name="health-service"></a>Service de contrôle d'intégrité

Le service de contrôle d’intégrité améliore la surveillance, les opérations et la maintenance quotidiennes des ressources sur un cluster d’espaces de stockage direct.

Pour plus d’informations, consultez [Health Service in Windows Server 2016](../failover-clustering/health-service-overview.md) (Service de contrôle d’intégrité dans Windows Server2016).

## <a name="application-development"></a>Développement d’applications

### <a name="internet-information-services-iis-100"></a>Services IIS (Internet Information Services) 10.0
Nouvelles fonctionnalités fournies par le serveur web IIS 10.0 dans Windows Server 2016 :

- Prise en charge du protocole HTTP/2 dans la pile Gestion de réseau et intégration avec IIS 10.0, ce qui permet aux sites web IIS 10.0 de servir automatiquement les demandes HTTP/2 pour les configurations prises en charge. Cette évolution apporte de nombreuses améliorations comparé à HTTP/1.1, telles qu'une réutilisation plus efficace des connexions et une moindre latence, ce qui améliore les temps de chargement des pages web. 
- Possibilité d’exécuter et de gérer IIS 10.0 dans Nano Server. Voir [IIS sur Nano Server](iis-on-nano-server.md).
- Prise en charge des en-têtes d’hôtes génériques, permettant aux administrateurs d’un serveur web pour un domaine de définir ensuite le serveur web traite les requêtes pour n’importe quel sous-domaine.
- Nouveau module PowerShell (IISAdministration) pour la gestion des services IIS. 

Pour plus d'informations, voir [IIS](https://iis.net/learn).

### <a name="distributed-transaction-coordinator-msdtc"></a>Distributed Transaction Coordinator (MSDTC)
Trois nouvelles fonctionnalités ont été ajoutées dans Microsoft Windows 10 et Windows Server 2016 :

- Une nouvelle interface Rejoin peut être utilisée par un gestionnaire de ressources pour déterminer le résultat d’une transaction incertaine après le redémarrage d’une base de données en raison d’une erreur. Voir [IResourceManagerRejoinable::Rejoin](https://msdn.microsoft.com/en-us/library/mt203799(v=vs.85).aspx) pour plus d’informations.

- La limite des noms de source de données (DSN) a été étendue de 256 octets à 3 072 octets. Voir [IDtcToXaHelperFactory::Create](https://msdn.microsoft.com/en-us/library/ms686861(v=vs.85).aspx), [IDtcToXaHelperSinglePipe::XARMCreate](https://msdn.microsoft.com/en-us/library/ms679248(v=vs.85).aspx) ou [IDtcToXaMapper::RequestNewResourceManager](https://msdn.microsoft.com/en-us/library/ms680310(v=vs.85).aspx) pour plus d’informations.

- Suivi amélioré, qui permet de définir une clé de Registre de manière à inclure un chemin de fichier image dans le nom de fichier du journal de suivi pour simplifier l'identification du journal de suivi à vérifier. Voir [Comment activer le suivi de diagnostic pour MSDTC sur un ordinateur fonctionnant sous Windows](https://support.microsoft.com/en-us/kb/926099) pour plus d’informations sur la configuration du suivi pour MSDTC.



## <a name="see-also"></a>Voir aussi  
-   [Notes de publication : problèmes importants sur Windows Server 2016](Windows-Server-2016-GA-Release-Notes.md)  

