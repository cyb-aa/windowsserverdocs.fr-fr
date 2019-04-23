---
title: Déployer plusieurs serveurs d’accès à distance dans un déploiement multisite
description: Cette rubrique fait partie du guide de déploiement de plusieurs serveurs d’accès distant dans un déploiement Multisite dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac2f6015-50a5-4909-8f67-8565f9d332a2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3091c770fb9b207d82deaa571970bfeb44d17ee3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875880"
---
# <a name="deploy-multiple-remote-access-servers-in-a-multisite-deployment"></a>Déployer plusieurs serveurs d’accès à distance dans un déploiement multisite

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

 Permet de combiner DirectAccess et accès distant (RAS) VPN dans un seul rôle accès à distance Windows Server 2016 et Windows Server 2012. L’accès à distance peut être déployé dans plusieurs scénarios d’entreprise. Cette présentation fournit une introduction au scénario d’entreprise pour déployer des serveurs d’accès à distance dans une configuration multisite.  
  
## <a name="BKMK_OVER"></a>Description du scénario  
Dans un déploiement multisite deux ou plusieurs serveurs d’accès à distance ou les clusters de serveurs sont déployés et configurés comme points d’entrée différents dans un emplacement unique, ou dans des emplacements géographiques disparates. Déploiement de plusieurs points d’entrée dans un emplacement unique permet la redondance de serveur, ou pour l’alignement des serveurs d’accès à distance avec une architecture réseau existante. Déploiement par emplacement géographique garantit une utilisation efficace des ressources, comme les ordinateurs clients distants peuvent se connecter aux ressources réseau internes à l’aide d’un point d’entrée le plus proche pour les. Le trafic via un déploiement multisite peut être distribué et équilibré avec un équilibreur de charge global externe.  
  
Un déploiement multisite prend en charge les ordinateurs clients qui exécutent Windows 10, Windows 8 ou Windows 7. Identifient les ordinateurs clients qui exécutent Windows 10 ou Windows 8 automatiquement un point d’entrée, ou l’utilisateur peut sélectionner manuellement un point d’entrée. L’attribution automatique se produit dans l’ordre de priorité suivant :  
  
1.  Utiliser un point d’entrée sélectionné manuellement par l’utilisateur.  
  
2.  Utiliser un point d’entrée identifié par un équilibreur de charge global externe si une est déployée.  
  
3.  Utilisez le point d’entrée le plus proche identifié par l’ordinateur client à l’aide d’un mécanisme de détection automatique.  
  
Prise en charge pour les clients qui exécutent Windows 7 doit être activée manuellement sur chaque point d’entrée, et la sélection d’un point d’entrée par ces clients n’est pas pris en charge.  
  
## <a name="prerequisites"></a>Prérequis  
Avant de déployer ce scénario, prenez connaissance des conditions requises suivantes qui ont leur importance :  
  
-   [Déployer un serveur DirectAccess unique avec des paramètres avancés](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) doit être déployé avant un déploiement multisite.  
  
-   Les clients Windows 7 seront connecte toujours à un site spécifique. Ils ne seront pas en mesure de se connecter au site plus proche en fonction de l’emplacement du client (contrairement aux clients Windows 10, 8 ou 8.1).  
  
-   La modification des stratégies en dehors de la console de gestion DirectAccess ou des applets de commande PowerShell n’est pas prise en charge.  
  
-   Une infrastructure à clé publique doit être déployée.  
  
    Pour plus d’informations, voir : [Mini-module de Guide de laboratoire de test : Infrastructure à clé publique base pour Windows Server 2012.](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)  
  
-   Le réseau d’entreprise doit être compatible IPv6. Si vous utilisez le protocole ISATAP, vous devez le supprimer et utiliser le protocole IPv6 natif.  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Le scénario de déploiement multisite comprend plusieurs étapes :  
  
1. [Déployer un serveur DirectAccess unique avec des paramètres avancés](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Un seul serveur d’accès à distance avec des paramètres avancés doit être déployé avant de configurer un déploiement multisite.  
  
2. [Planifier un déploiement Multisite](plan/Plan-a-Multisite-Deployment.md). Pour créer un déploiement multisite à partir d’un seul serveur à un nombre de planification supplémentaires étapes sont nécessaires, notamment le respect des conditions préalables multisite et la planification de groupes de sécurité Active Directory, les objets de stratégie de groupe (GPO), les DNS et les paramètres client.  
  
3. [Configurer un déploiement Multisite](configure/Configure-a-Multisite-Deployment.md). Il s’agit d’un nombre d’étapes de configuration, y compris la préparation de l’infrastructure Active Directory, la configuration du serveur d’accès à distance existant et l’ajout de plusieurs serveurs d’accès à distance en tant que points d’entrée pour le déploiement multisite.  
  
4. [Résoudre les problèmes d’un déploiement Multisite](troubleshoot/Troubleshoot-a-Multisite-Deployment.md). Cette section décrit un nombre d’erreurs les plus courantes qui peuvent se produire lors du déploiement d’accès à distance dans un déploiement multisite.
  
## <a name="BKMK_APP"></a>Applications pratiques  
Un déploiement multisite offre les avantages suivants :  
  
-   Un déploiement multisite performances-A amélioré permet aux ordinateurs l’accès aux ressources internes à l’aide de l’accès à distance pour se connecter à l’aide du point d’entrée le plus proche et convenant clients. Client accéder efficacement aux ressources internes, et l’amélioration de la vitesse du client Qu'internet demande acheminé par le biais de DirectAccess. Le trafic entre les points d’entrée peut être équilibré à l’aide d’un équilibreur de charge global externe.  
  
-   Facilité de gestion Multisite permet aux administrateurs d’aligner le déploiement de l’accès à distance à un déploiement de sites Active Directory, fournissant une architecture simplifiée. Paramètres partagés peuvent aisément être définis entre serveurs de point d’entrée ou de clusters. Paramètres d’accès à distance peuvent être gérés à partir de tous les serveurs dans le déploiement, ou à distance à l’aide des outils d’Administration de serveur distant (RSAT). En outre, la totalité du déploiement multisite peut être surveillé à partir d’une seule console de gestion de l’accès à distance.  
  
## <a name="BKMK_NEW"></a>Fonctionnalités et rôles inclus dans ce scénario  
Le tableau suivant répertorie les rôles et les fonctionnalités utilisées dans ce scénario.  
  
|Rôle/fonctionnalité|Prise en charge de ce scénario|  
|---------|-----------------|  
|Rôle Accès à distance|Ce rôle est installé et désinstallé à l’aide de la console du Gestionnaire de serveur. Il englobe à la fois DirectAccess, qui était auparavant une fonctionnalité de Windows Server 2008 R2, et le service de routage et d’accès distant (RRAS) qui était auparavant un service de rôle sous le rôle de serveur Services de stratégie et d’accès réseau. Le rôle Accès à distance est constitué de deux composants :<br /><br />-DirectAccess, routage et accès distant (RRAS) VPN DirectAccess et VPN sont gérés ensemble dans la console de gestion de l’accès à distance.<br />-Les fonctionnalités de routage de RRAS routage-RRAS sont gérées dans la console Routage et accès distant héritée.<br /><br />Les dépendances sont les suivantes :<br /><br />-Internet Information Services (IIS) serveur Web - cette fonctionnalité est nécessaire pour configurer le serveur emplacement réseau et la sonde web par défaut.<br />-Windows Database-Used interne pour la gestion des comptes locale sur le serveur d’accès à distance.|  
|Fonctionnalité des outils de gestion de l’accès à distance|Cette fonctionnalité est installée comme suit :<br /><br />-Il est installé par défaut sur un serveur d’accès à distance lorsque le rôle accès à distance est installé et prend en charge de l’interface utilisateur de console Administration à distance.<br />-Il peut éventuellement être installé sur un serveur n'exécutant pas le rôle de serveur d’accès à distance. Dans ce cas, elle est utilisée pour la gestion à distance d’un ordinateur d’accès à distance qui exécute DirectAccess et le réseau privé virtuel (VPN).<br /><br />La fonctionnalité des outils de gestion de l’accès à distance est constituée des éléments suivants :<br /><br />-Accès à distance GUI et outils de ligne de commande<br />-Module d’accès distant pour Windows PowerShell<br /><br />Les dépendances incluent :<br /><br />-Console de gestion de stratégie de groupe<br />-Kit d’Administration RAS Gestionnaire de connexions (CMAK)<br />-Windows PowerShell 3.0<br />-Infrastructure et des outils de gestion graphique|  
  
## <a name="BKMK_HARD"></a>Configuration matérielle requise  
La configuration matérielle requise pour ce scénario comprend les éléments suivants :  
  
-   Au moins deux ordinateurs de l’accès à distance dans un déploiement multisite.   
  
-   Pour tester le scénario, au moins un ordinateur exécutant Windows 8 et configuré comme un client DirectAccess est requis. Pour tester le scénario pour les clients qui exécutent Windows 7, au moins un ordinateur exécutant Windows 7 est nécessaire.  
  
-   Pour équilibrer le trafic entre les serveurs de point d’entrée, un équilibreur de charge global externe tiers est requis.  
  
## <a name="BKMK_SOFT"></a>Configuration logicielle requise  
La configuration logicielle requise pour ce scénario comprend les éléments suivants :  
  
-   Configuration logicielle requise pour un déploiement sur un seul serveur.  
  
-   En plus de la configuration logicielle requise pour un seul serveur, il existe de nombreuses exigences multisite-spécifiques :  
  
    -   Authentification IPsec exigences dans qu'un déploiement multisite DirectAccess doit être déployé à l’aide de l’authentification de certificat IPsec machine. L’option permettant d’effectuer l’authentification IPsec utilisant le serveur d’accès à distance comme proxy Kerberos n’est pas pris en charge. Une autorité de certification interne est requis pour déployer les certificats IPsec.  
  
    -   IP-HTTPS et réseau emplacement exigences-certificats de serveur requis pour IP-HTTPS et le serveur emplacement réseau doivent être émis par une autorité de certification. La possibilité d’utiliser des certificats émis automatiquement et auto-signés par le serveur d’accès à distance n’est pas pris en charge. Certificats peuvent être émis par une autorité de certification interne ou par une autorité de certification tiers externe.  
  
    -   Configuration requise d’Active Directory-au moins un site Active Directory est requis. Le serveur d’accès à distance doit se trouver dans le site. Pour la mise à jour plus rapide, il est recommandé que chaque site dispose d’un contrôleur de domaine accessible en écriture, même si ce n’est pas obligatoire.  
  
    -   Configuration requise-exigences de groupe de sécurité sont les suivantes :  
  
        -   Un seul groupe de sécurité est requis pour tous les ordinateurs clients de Windows 8 à partir de tous les domaines. Il est recommandé de créer un groupe de sécurité unique de ces clients pour chaque domaine.  
  
        -   Un groupe de sécurité unique contenant des ordinateurs Windows 7 est requis pour chaque point d’entrée configuré pour prendre en charge les clients Windows 7. Il est recommandé d’avoir un groupe de sécurité unique pour chaque point d’entrée dans chaque domaine.  
  
        -   Les ordinateurs ne doivent pas être inclus dans plus d’un groupe de sécurité contenant des clients DirectAccess. Si les clients sont inclus dans plusieurs groupes, la résolution de noms des demandes client ne fonctionnera pas comme prévu.  
  
    -   Configuration requise-stratégie de groupe de stratégie de groupe peut être créé manuellement avant de configurer l’accès à distance ou créé automatiquement pendant le déploiement de l’accès à distance. Spécifications sont les suivantes :  
  
        -   Un objet de stratégie de groupe client unique est requis pour chaque domaine.  
  
        -   Un serveur de stratégie de groupe est requis pour chaque point d’entrée, dans le domaine dans lequel se trouve le point d’entrée. Si plusieurs points d’entrée sont situés dans le même domaine, il y aura plusieurs serveurs de stratégie de groupe (une pour chaque point d’entrée) dans le domaine.  
  
        -   Une stratégie de groupe du client Windows 7 unique est requise pour chaque point d’entrée est activée pour la prise en charge du client Windows 7, pour chaque domaine.  
  
## <a name="KnownIssues"></a>Problèmes connus  
Les éléments suivants sont des problèmes connus lors de la configuration d’un scénario multisite :  
  
-   **Plusieurs points d’entrée dans le même sous-réseau IPv4**. Ajout de plusieurs points d’entrée dans le même sous-réseau IPv4 entraîne un message de conflit d’adresse IP et l’adresse DNS64 pour le point d’entrée ne sera pas configuré comme prévu. Ce problème se produit lorsque IPv6 n’a pas été déployée sur les interfaces internes des serveurs sur le réseau d’entreprise. Pour éviter ce problème, exécutez la commande Windows PowerShell suivante sur tous les serveurs d’accès à distance actuelles et futures :  
  
    ```  
    Set-NetIPInterface -InterfaceAlias <InternalInterfaceName> -AddressFamily IPv6 -DadTransmits 0  
    ```  
  
-   Si l’adresse publique spécifiée pour les clients DirectAccess pour se connecter au serveur d’accès à distance comporte un suffixe inclus dans la table NRPT, DirectAccess ne peut pas fonctionner comme prévu. Assurez-vous que la table NRPT comporte une exemption pour le nom public. Dans un déploiement multisite, les exemptions doivent être ajoutées pour les noms de tous les points d’entrée publics. Notez qu’en cas de le tunneling forcé est activé ces exemptions sont ajoutées automatiquement. Ils sont supprimés si le tunneling forcé est désactivé.  
  
-   Lorsque vous utilisez l’applet de commande Windows PowerShell **Disable-DAMultiSite**, les paramètres WhatIf et Confirm n’ont aucun effet et Multisite va être désactivé et stratégie de groupe Windows 7 seront supprimés.  
  
-   Lorsque les clients Windows 7 à l’aide de DCA dans un déploiement Multisite sont mis à niveau vers Windows 8, l’Assistant connectivité réseau ne fonctionnera pas. Ce problème peut être résolu avant la mise à niveau du client en modifiant la stratégie de groupe Windows 7 à l’aide des applets de commande Windows PowerShell suivantes :  
  
    ```  
    Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1  
    Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"  
    ```  
  
    Dans le cas où le client a déjà été mis à niveau, vous devez ensuite déplacer l’ordinateur client vers le groupe de sécurité de Windows 8.  
  
-   Lorsque vous modifiez les paramètres de contrôleur de domaine à l’aide de l’applet de commande Windows PowerShell **Set-DAEntryPointDC**, si le paramètre ComputerName spécifié est un serveur d’accès à distance dans un point d’entrée autre que le dernier ajouté à la fonctionnalité Multisite déploiement, un avertissement sera affiché pour indiquer que le serveur spécifié ne sera pas mis à jour jusqu'à la prochaine actualisation de stratégie. L’ou les serveurs réels qui n’a pas été mis à jour peut être consulté dans le **état de la Configuration** dans le **tableau de bord** de la **Console de gestion des accès à distance**. Cela n’entraîne pas de problèmes fonctionnels, toutefois, vous pouvez exécuter **gpupdate /force** sur l’ou les serveurs qui n’a pas été mis à jour pour obtenir l’état de configuration mis à jour immédiatement.  
  
-   Lorsque multisite est déployé dans un réseau de d’entreprise IPv4 uniquement, modification interne modifications le DNS64 adresse IPv6 préfixe réseau également, mais ne met pas à jour l’adresse sur les règles de pare-feu qui autorise les requêtes DNS au service DNS64. Pour résoudre ce problème, exécutez les commandes Windows PowerShell suivantes après avoir modifié le préfixe IPv6 de réseau interne :  
  
    ```  
    $dns64Address = (Get-DAClientDnsConfiguration).NrptEntry | ?{ $_.DirectAccessDnsServers -match ':3333::1' } | Select-Object -First 1 -ExpandProperty DirectAccessDnsServers  
  
    $serverGpoName = (Get-RemoteAccess).ServerGpoName  
  
    $serverGpoDc = (Get-DAEntryPointDC).DomainControllerName  
  
    $gpoSession = Open-NetGPO -PolicyStore $serverGpoName -DomainController $serverGpoDc  
  
    Get-NetFirewallRule -GPOSession $gpoSession | ? {$_.Name -in @('0FDEEC95-1EA6-4042-8BA6-6EF5336DE91A', '24FD98AA-178E-4B01-9220-D0DADA9C8503')} |  Set-NetFirewallRule -LocalAddress $dns64Address  
  
    Save-NetGPO -GPOSession $gpoSession  
    ```  
  
-   Si DirectAccess a été déployée lors d’une infrastructure ISATAP existante existait lors de la suppression d’un point d’entrée qui a été un hôte ISATAP, l’adresse IPv6 du service DNS64 sera retiré de l’adresse de serveur DNS de tous les suffixes DNS dans la table NRPT.  
  
    Pour résoudre ce problème, dans le **le programme d’installation serveur d’Infrastructure** Assistant, sur le **DNS** page, supprimez les suffixes DNS qui ont été modifiés et les ajouter à nouveau avec les adresses de serveur DNS corrects, en cliquant sur  **Détecter** sur le **adresse de serveur DNS** boîte de dialogue.  
  


