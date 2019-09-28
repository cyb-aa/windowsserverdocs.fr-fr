---
title: Déployer plusieurs serveurs d’accès à distance dans un déploiement multisite
description: Cette rubrique fait partie du guide déployer plusieurs serveurs d’accès à distance dans un déploiement multisite dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac2f6015-50a5-4909-8f67-8565f9d332a2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: da23f3082e1d97f1bcfbee7365b863d29ba2d020
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404499"
---
# <a name="deploy-multiple-remote-access-servers-in-a-multisite-deployment"></a>Déployer plusieurs serveurs d’accès à distance dans un déploiement multisite

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

 Windows Server 2016 et Windows Server 2012 associent DirectAccess et le VPN RAS (Remote Access Service) à un rôle d’accès à distance unique. L’accès à distance peut être déployé dans plusieurs scénarios d’entreprise. Cette vue d’ensemble fournit une introduction au scénario d’entreprise pour le déploiement de serveurs d’accès à distance dans une configuration multisite.  
  
## <a name="BKMK_OVER"></a>Description du scénario  
Dans un déploiement multisite, deux ou plusieurs serveurs d’accès à distance ou clusters de serveurs sont déployés et configurés en tant que points d’entrée différents dans un emplacement unique ou dans des emplacements géographiques dispersés. Le déploiement de plusieurs points d’entrée dans un seul emplacement permet la redondance des serveurs ou l’alignement des serveurs d’accès à distance avec l’architecture réseau existante. Le déploiement par emplacement géographique garantit une utilisation efficace des ressources, car les ordinateurs clients distants peuvent se connecter aux ressources réseau internes à l’aide d’un point d’entrée le plus proche. Le trafic dans un déploiement multisite peut être distribué et équilibré avec un équilibreur de charge global externe.  
  
Un déploiement multisite prend en charge les ordinateurs clients exécutant Windows 10, Windows 8 ou Windows 7. Les ordinateurs clients qui exécutent Windows 10 ou Windows 8 identifient automatiquement un point d’entrée, ou l’utilisateur peut sélectionner manuellement un point d’entrée. L’attribution automatique se produit dans l’ordre de priorité suivant :  
  
1.  Utilisez un point d’entrée sélectionné manuellement par l’utilisateur.  
  
2.  Utilisez un point d’entrée identifié par un équilibreur de charge global externe s’il est déployé.  
  
3.  Utilisez le point d’entrée le plus proche identifié par l’ordinateur client à l’aide d’un mécanisme de sonde automatique.  
  
La prise en charge des clients exécutant Windows 7 doit être activée manuellement sur chaque point d’entrée, et la sélection d’un point d’entrée par ces clients n’est pas prise en charge.  
  
## <a name="prerequisites"></a>Prérequis  
Avant de déployer ce scénario, prenez connaissance des conditions requises suivantes qui ont leur importance :  
  
-   [Le déploiement d’un serveur DirectAccess unique avec des paramètres avancés](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) doit être déployé avant un déploiement multisite.  
  
-   Les clients Windows 7 se connectent toujours à un site spécifique. Ils ne seront pas en mesure de se connecter au site le plus proche en fonction de l’emplacement du client (contrairement aux clients Windows 10, 8 ou 8,1).  
  
-   La modification des stratégies en dehors de la console de gestion DirectAccess ou des applets de commande PowerShell n’est pas prise en charge.  
  
-   Une infrastructure à clé publique doit être déployée.  
  
    Pour plus d’informations, voir : Mini-module [Test Lab Guide : Infrastructure à clé publique de base pour Windows Server 2012. ](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)  
  
-   IPv6 doit être activé sur le réseau d’entreprise. Si vous utilisez le protocole ISATAP, vous devez le supprimer et utiliser le protocole IPv6 natif.  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Le scénario de déploiement multisite comprend un certain nombre d’étapes :  
  
1. [Déployez un serveur DirectAccess unique avec des paramètres avancés](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Vous devez déployer un serveur d’accès à distance unique avec des paramètres avancés avant de configurer un déploiement multisite.  
  
2. [Planifier un déploiement multisite](plan/Plan-a-Multisite-Deployment.md). Pour créer un déploiement multisite à partir d’un seul serveur, un certain nombre d’étapes de planification supplémentaires sont nécessaires, notamment la conformité à la configuration multisite requise et la planification des groupes de sécurité Active Directory, des objets de stratégie de groupe (GPO), des paramètres DNS et client.  
  
3. [Configurer un déploiement multisite](configure/Configure-a-Multisite-Deployment.md). Il s’agit d’un certain nombre d’étapes de configuration, y compris la préparation de l’infrastructure Active Directory, la configuration du serveur d’accès à distance existant et l’ajout de plusieurs serveurs d’accès à distance en tant que points d’entrée au déploiement multisite.  
  
4. [Résoudre les problèmes liés à un déploiement multisite](troubleshoot/Troubleshoot-a-Multisite-Deployment.md). Cette section de résolution des problèmes décrit plusieurs des erreurs les plus courantes qui peuvent se produire lors du déploiement de l’accès à distance dans un déploiement multisite.
  
## <a name="BKMK_APP"></a>Applications pratiques  
Un déploiement multisite fournit les éléments suivants :  
  
-   Amélioration des performances : un déploiement multisite permet aux ordinateurs clients d’accéder aux ressources internes à l’aide de l’accès à distance pour se connecter à l’aide du point d’entrée le plus proche et le plus approprié. L’accès client aux ressources internes est efficace et la vitesse des demandes Internet clientes routées via DirectAccess est améliorée. Le trafic entre les points d’entrée peut être équilibré à l’aide d’un équilibreur de charge global externe.  
  
-   Facilité de gestion-multisite permet aux administrateurs d’aligner le déploiement de l’accès à distance sur un déploiement de sites Active Directory, en fournissant une architecture simplifiée. Les paramètres partagés peuvent être facilement définis sur des serveurs de point d’entrée ou des clusters. Les paramètres d’accès à distance peuvent être gérés à partir de n’importe quel serveur dans le déploiement, ou à l’aide de Outils d’administration de serveur distant (RSAT). En outre, l’ensemble du déploiement multisite peut être analysé à partir d’une seule console de gestion de l’accès à distance.  
  
## <a name="BKMK_NEW"></a>Rôles et fonctionnalités inclus dans ce scénario  
Le tableau suivant répertorie les rôles et les fonctionnalités utilisés dans ce scénario.  
  
|Rôle/fonctionnalité|Prise en charge de ce scénario|  
|---------|-----------------|  
|Rôle Accès à distance|Ce rôle est installé et désinstallé à l’aide de la console du Gestionnaire de serveur. Il englobe à la fois DirectAccess, qui était auparavant une fonctionnalité de Windows Server 2008 R2, et le service de routage et d’accès distant (RRAS) qui était auparavant un service de rôle sous le rôle de serveur Services de stratégie et d’accès réseau. Le rôle Accès à distance est constitué de deux composants :<br /><br />-DirectAccess et les services de routage et d’accès à distance (RRAS) VPN-DirectAccess et VPN sont gérés ensemble dans la console de gestion de l’accès à distance.<br />-Routage RRAS : les fonctionnalités de routage RRAS sont gérées dans la console de routage et d’accès distant héritée.<br /><br />Les dépendances sont les suivantes :<br /><br />-Serveur Web Internet Information Services (IIS) : cette fonctionnalité est requise pour configurer le serveur emplacement réseau et la sonde Web par défaut.<br />-Base de données interne Windows : utilisée pour la comptabilité locale sur le serveur d’accès à distance.|  
|Fonctionnalité des outils de gestion de l’accès à distance|Cette fonctionnalité est installée comme suit :<br /><br />-Elle est installée par défaut sur un serveur d’accès à distance lorsque le rôle accès à distance est installé et prend en charge l’interface utilisateur de la console de gestion à distance.<br />-Il peut éventuellement être installé sur un serveur qui n’exécute pas le rôle de serveur d’accès à distance. Dans ce cas, elle est utilisée pour la gestion à distance d’un ordinateur d’accès à distance qui exécute DirectAccess et le réseau privé virtuel (VPN).<br /><br />La fonctionnalité des outils de gestion de l’accès à distance est constituée des éléments suivants :<br /><br />-Interface utilisateur graphique d’accès à distance et outils en ligne de commande<br />-Module d’accès à distance pour Windows PowerShell<br /><br />Les dépendances incluent :<br /><br />-Console de gestion des stratégies de groupe<br />-Kit d’administration du gestionnaire des connexions (CMAK) RAS<br />-Windows PowerShell 3,0<br />-Outils et infrastructure de gestion graphique|  
  
## <a name="BKMK_HARD"></a>Configuration matérielle requise  
La configuration matérielle requise pour ce scénario comprend les éléments suivants :  
  
-   Au moins deux ordinateurs d’accès à distance à collecter dans un déploiement multisite.   
  
-   Pour tester le scénario, au moins un ordinateur exécutant Windows 8 et configuré en tant que client DirectAccess est requis. Pour tester le scénario pour les clients qui exécutent Windows 7, au moins un ordinateur exécutant Windows 7 est requis.  
  
-   Pour équilibrer la charge du trafic entre les serveurs de point d’entrée, un équilibrage de charge global externe tiers est requis.  
  
## <a name="BKMK_SOFT"></a>Configuration logicielle requise  
La configuration logicielle requise pour ce scénario comprend les éléments suivants :  
  
-   Configuration logicielle requise pour un déploiement sur un seul serveur.  
  
-   Outre la configuration logicielle requise pour un serveur unique, il existe un certain nombre d’exigences spécifiques à plusieurs sites :  
  
    -   Exigences d’authentification IPsec : dans un déploiement multisite, DirectAccess doit être déployé à l’aide de l’authentification par certificat d’ordinateur IPsec. L’option permettant d’effectuer une authentification IPsec à l’aide du serveur d’accès à distance en tant que proxy Kerberos n’est pas prise en charge. Une autorité de certification interne est requise pour déployer les certificats IPsec.  
  
    -   Configuration requise pour le serveur IP-HTTPs et le serveur d’emplacement réseau : les certificats requis pour IP-HTTPs et le serveur emplacement réseau doivent être émis par une autorité de certification. L’option permettant d’utiliser des certificats automatiquement émis et auto-signés par le serveur d’accès à distance n’est pas prise en charge. Les certificats peuvent être émis par une autorité de certification interne ou une autorité de certification externe tierce.  
  
    -   Exigences de Active Directory : au moins un site Active Directory est requis. Le serveur d’accès à distance doit se trouver dans le site. Pour accélérer les mises à jour, il est recommandé que chaque site dispose d’un contrôleur de domaine accessible en écriture, bien que cela ne soit pas obligatoire.  
  
    -   Exigences des groupes de sécurité : la configuration requise est la suivante :  
  
        -   Un groupe de sécurité unique est requis pour tous les ordinateurs clients Windows 8 de tous les domaines. Il est recommandé de créer un groupe de sécurité unique de ces clients pour chaque domaine.  
  
        -   Un groupe de sécurité unique contenant des ordinateurs Windows 7 est requis pour chaque point d’entrée configuré pour prendre en charge les clients Windows 7. Il est recommandé de disposer d’un groupe de sécurité unique pour chaque point d’entrée de chaque domaine.  
  
        -   Les ordinateurs ne doivent pas être inclus dans plus d’un groupe de sécurité contenant des clients DirectAccess. Si les clients sont inclus dans plusieurs groupes, la résolution de noms des demandes client ne fonctionnera pas comme prévu.  
  
    -   Exigences relatives aux objets de stratégie de groupe : les objets de stratégie de groupe peuvent être créés manuellement avant de configurer l’accès à distance ou créés automatiquement lors du déploiement de l’accès à distance La configuration requise est la suivante :  
  
        -   Un objet de stratégie de groupe client unique est requis pour chaque domaine.  
  
        -   Un objet de stratégie de groupe de serveur est requis pour chaque point d’entrée dans le domaine dans lequel se trouve le point d’entrée. Par conséquent, si plusieurs points d’entrée se trouvent dans le même domaine, il y aura plusieurs objets de stratégie de groupe de serveur (un pour chaque point d’entrée) dans le domaine.  
  
        -   Un objet de stratégie de groupe client Windows 7 unique est requis pour chaque point d’entrée activé pour la prise en charge du client Windows 7, pour chaque domaine.  
  
## <a name="KnownIssues"></a>Problèmes connus  
Voici les problèmes connus liés à la configuration d’un scénario multisite :  
  
-   **Plusieurs points d’entrée dans le même sous-réseau IPv4**. L’ajout de plusieurs points d’entrée dans le même sous-réseau IPv4 génère un message de conflit d’adresse IP et l’adresse DNS64 du point d’entrée n’est pas configurée comme prévu. Ce problème se produit quand IPv6 n’a pas été déployé sur les interfaces internes des serveurs sur le réseau d’entreprise. Pour éviter ce problème, exécutez la commande Windows PowerShell suivante sur tous les serveurs d’accès à distance actuels et futurs :  
  
    ```  
    Set-NetIPInterface -InterfaceAlias <InternalInterfaceName> -AddressFamily IPv6 -DadTransmits 0  
    ```  
  
-   Si l’adresse publique spécifiée pour que les clients DirectAccess se connectent au serveur d’accès à distance a un suffixe inclus dans la table NRPT, DirectAccess peut ne pas fonctionner comme prévu. Assurez-vous que la table NRPT a une exemption pour le nom public. Dans un déploiement multisite, des exemptions doivent être ajoutées pour les noms publics de tous les points d’entrée. Notez que si le tunneling forcé est activé, ces exemptions sont ajoutées automatiquement. Elles sont supprimées si forcer le tunneling est désactivé.  
  
-   Quand vous utilisez l’applet de commande Windows PowerShell **Disable-DAMultiSite**, les paramètres WhatIf et Confirm n’ont aucun effet, et le multisite est désactivé et les objets de stratégie de groupe Windows 7 sont supprimés.  
  
-   Lorsque les clients Windows 7 utilisant DCA dans un déploiement multisite sont mis à niveau vers Windows 8, l’Assistant connectivité réseau ne fonctionne pas. Ce problème peut être résolu à l’avance de la mise à niveau du client en modifiant les objets de stratégie de groupe Windows 7 à l’aide des applets de commande Windows PowerShell suivantes :  
  
    ```  
    Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1  
    Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"  
    ```  
  
    Dans le cas où le client a déjà été mis à niveau, déplacez l’ordinateur client vers le groupe de sécurité Windows 8.  
  
-   Lors de la modification des paramètres du contrôleur de domaine à l’aide de l’applet de commande Windows PowerShell **Set-DAEntryPointDC**, si le paramètre ComputerName spécifié est un serveur d’accès à distance dans un point d’entrée autre que le dernier ajouté au déploiement multisite, un avertissement s’affiche pour indiquer que le serveur spécifié ne sera pas mis à jour avant la prochaine actualisation de la stratégie. Le ou les serveurs réels qui n’ont pas été mis à jour peuvent être consultés à l’aide de l' **État de configuration** dans le **tableau de bord** de la console de gestion de l' **accès à distance**. Cela ne provoque pas de problèmes fonctionnels. Toutefois, vous pouvez exécuter **gpupdate/force** sur le ou les serveurs qui n’ont pas été mis à jour pour que l’état de configuration soit mis à jour immédiatement.  
  
-   Lorsque le déploiement multisite est effectué dans un réseau d’entreprise IPv4 uniquement, la modification du préfixe IPv6 de réseau interne modifie également l’adresse DNS64, mais ne met pas à jour l’adresse sur les règles de pare-feu qui autorisent les requêtes DNS sur le service DNS64. Pour résoudre ce problème, exécutez les commandes Windows PowerShell suivantes après avoir modifié le préfixe IPv6 de réseau interne :  
  
    ```  
    $dns64Address = (Get-DAClientDnsConfiguration).NrptEntry | ?{ $_.DirectAccessDnsServers -match ':3333::1' } | Select-Object -First 1 -ExpandProperty DirectAccessDnsServers  
  
    $serverGpoName = (Get-RemoteAccess).ServerGpoName  
  
    $serverGpoDc = (Get-DAEntryPointDC).DomainControllerName  
  
    $gpoSession = Open-NetGPO -PolicyStore $serverGpoName -DomainController $serverGpoDc  
  
    Get-NetFirewallRule -GPOSession $gpoSession | ? {$_.Name -in @('0FDEEC95-1EA6-4042-8BA6-6EF5336DE91A', '24FD98AA-178E-4B01-9220-D0DADA9C8503')} |  Set-NetFirewallRule -LocalAddress $dns64Address  
  
    Save-NetGPO -GPOSession $gpoSession  
    ```  
  
-   Si DirectAccess a été déployé quand une infrastructure ISATAP existante était présente, lors de la suppression d’un point d’entrée qui était un hôte ISATAP, l’adresse IPv6 du service DNS64 sera supprimée des adresses de serveur DNS de tous les suffixes DNS dans la table NRPT.  
  
    Pour résoudre ce problème, dans l’Assistant **installation du serveur d’infrastructure** , dans la page **DNS** , supprimez les suffixes DNS qui ont été modifiés et rajoutez-les avec les adresses de serveur DNS correctes, en cliquant sur **détecter** sur les **adresses de serveur DNS.** boîte de dialogue.  
  


