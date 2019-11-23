---
title: Déployer l’accès à distance dans un cluster
description: Cette rubrique fait partie du guide déployer l’accès à distance dans un cluster dans Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5e4108eee0c62ae4d4db31560b31a6f90751c6b8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404647"
---
# <a name="deploy-remote-access-in-a-cluster"></a>Déployer l’accès à distance dans un cluster

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Windows Server 2016 et Windows Server 2012 combinent DirectAccess et le service d’accès à distance \(RAS\) VPN en un seul rôle d’accès à distance. Vous pouvez déployer l’accès à distance dans plusieurs scénarios d’entreprise. Cette vue d’ensemble fournit une introduction au scénario d’entreprise pour le déploiement de plusieurs serveurs d’accès à distance dans une charge de cluster équilibrée avec l’équilibrage de charge réseau Windows \(NLB\) ou avec un équilibreur de charge externe \(ELB\), comme F5 Big\-IP.  

## <a name="BKMK_OVER"></a>Description du scénario  
Un déploiement de cluster rassemble plusieurs serveurs d’accès à distance en une seule unité, qui agit ensuite comme un point de contact unique pour les ordinateurs clients distants qui se connectent via DirectAccess ou VPN au réseau d’entreprise interne à l’aide de l’adresse IP virtuelle \(VIP\) du cluster d’accès à distance.  Le trafic vers le cluster est équilibré en charge à l’aide de Windows NLB ou avec un équilibreur de charge externe \(comme F5 Big\-IP\).  

## <a name="prerequisites"></a>Conditions préalables  
Avant de déployer ce scénario, prenez connaissance des conditions requises suivantes qui ont leur importance :  

-   Équilibrage de charge par défaut via l’équilibrage de la charge réseau (NLB) Windows.  

-   Les équilibrages de charge externes sont pris en charge.  

-   Monodiffusion est le mode par défaut et celui recommandé pour l’équilibrage de la charge réseau (NLB).  

-   La modification des stratégies en dehors de la console de gestion DirectAccess ou des applets de commande PowerShell n’est pas prise en charge.  

-   Lorsque NLB ou un équilibreur de charge externe est utilisé, le préfixe IPHTTPS ne peut pas être remplacé par une valeur autre que \/59.  

-   Les nœuds à charge équilibrée doivent être dans le même sous-réseau IPv4.  

-   Dans les déploiements ELB, si la gestion out est nécessaire, les clients DirectAccess ne peuvent pas utiliser&nbsp;Teredo. Seul IPHTTPS peut être utilisé pour terminer la\-à\-fin de la communication.  

-   Assurez-vous que tous les correctifs ELB d'\/équilibrage de charge réseau connus sont installés.  

-   Le protocole ISATAP n'est pas pris en charge sur le réseau d'entreprise. Si vous utilisez le protocole ISATAP, vous devez le supprimer et utiliser le protocole IPv6 natif.  

## <a name="in-this-scenario"></a>Dans ce scénario  
Ce scénario de déploiement de cluster inclut plusieurs étapes :  

1.  [Déployez un serveur VPN Always on avec des options avancées](../../vpn/always-on-vpn/deploy/always-on-vpn-adv-options.md). Vous devez déployer un serveur d’accès à distance unique avec des paramètres avancés avant de configurer un déploiement de cluster.  

2.  [Planifier un déploiement de cluster d’accès à distance](plan/Plan-a-Remote-Access-Cluster-Deployment.md). Pour créer un cluster à partir d’un déploiement sur un seul serveur, vous devez effectuer un certain nombre d’étapes supplémentaires, notamment la préparation des certificats pour le déploiement de cluster.  

3.  [Configurez un cluster d’accès à distance](configure/Configure-a-Remote-Access-Cluster.md). Il s’agit d’un certain nombre d’étapes de configuration, notamment la préparation du serveur unique pour l’équilibrage de charge réseau Windows ou l’équilibrage de charge externe, la préparation de serveurs supplémentaires pour rejoindre le cluster et l’activation de l’équilibrage de charge.  

## <a name="BKMK_APP"></a>Applications pratiques  
Le regroupement de plusieurs serveurs en un cluster de serveurs fournit les avantages suivants :  

-   Extensibilité. Un serveur d’accès à distance unique offre un niveau limité de fiabilité du serveur et de performances évolutives. En regroupant les ressources de deux serveurs ou plus dans un cluster individuel, vous augmentez la capacité en termes de débit et de nombre d’utilisateurs.  

-   Haute disponibilité. Un cluster offre une haute disponibilité pour toujours\-sur l’accès. Si un serveur du cluster échoue, les utilisateurs distants peuvent continuer à accéder au réseau d’entreprise via un autre serveur du cluster. Tous les serveurs du cluster ont le même ensemble d’adresses IP virtuelles \(\) d’adresses IP virtuelles de cluster, tout en conservant une adresse IP dédiée unique pour chaque serveur.  

-   Facilitez la\-de la gestion des\-. Un cluster permet de gérer plusieurs serveurs en tant qu’entité unique. Des paramètres partagés peuvent aisément être définis entre les serveurs du cluster. Les paramètres d’accès à distance peuvent être gérés à partir de n’importe quel serveur du cluster, ou à distance à l’aide de Outils d’administration de serveur distant \(\)d’administration à distance. De plus, le cluster entier peut être analysé à partir d’une console de gestion de l’accès à distance unique.  

## <a name="BKMK_NEW"></a>Rôles et fonctionnalités inclus dans ce scénario  
Le tableau suivant répertorie les fonctionnalités et rôles requis pour ce scénario :  

|Fonctionnalité de\/de rôle|Prise en charge de ce scénario|  
|---------|-----------------|  
|Rôle Accès à distance|Ce rôle est installé et désinstallé à l’aide de la console du Gestionnaire de serveur. Il englobe à la fois DirectAccess, qui était auparavant une fonctionnalité de Windows Server 2008 R2, et les services de routage et d’accès à distance \(RRAS\), qui était auparavant un service de rôle sous le rôle de serveur services de stratégie et d’accès réseau \(NPAS\) Server. Le rôle Accès à distance est constitué de deux composants :<br /><br />-Always On VPN et les services de routage et d’accès à distance \(RRAS\) VPN-DirectAccess et VPN sont gérés ensemble dans la console de gestion de l’accès à distance.<br />-Routage RRAS : les fonctionnalités de routage RRAS sont gérées dans la console de routage et d’accès distant héritée.<br /><br />Les dépendances sont les suivantes :<br /><br />-Internet Information Services \(serveur Web IIS\)-cette fonctionnalité est requise pour configurer le serveur emplacement réseau et la sonde Web par défaut.<br />-Base de données interne Windows : utilisée pour la comptabilité locale sur le serveur d’accès à distance.|  
|Fonctionnalité des outils de gestion de l’accès à distance|Cette fonctionnalité est installée comme suit :<br /><br />-Elle est installée par défaut sur un serveur d’accès à distance lorsque le rôle accès à distance est installé et prend en charge l’interface utilisateur de la console de gestion à distance.<br />-Il peut éventuellement être installé sur un serveur qui n’exécute pas le rôle de serveur d’accès à distance. Dans ce cas, elle est utilisée pour la gestion à distance d’un ordinateur d’accès à distance qui exécute DirectAccess et le réseau privé virtuel (VPN).<br /><br />La fonctionnalité des outils de gestion de l’accès à distance est constituée des éléments suivants :<br /><br />-Interface utilisateur graphique d’accès à distance et outils en ligne de commande<br />-Module d’accès à distance pour Windows PowerShell<br /><br />Les dépendances incluent :<br /><br />-Console de gestion des stratégies de groupe<br />-Le kit d’administration du gestionnaire des connexions RAS \(CMAK\)<br />-Windows PowerShell 3,0<br />-Outils et infrastructure de gestion graphique|  
|Équilibrage de la charge réseau|Cette fonctionnalité assure l’équilibrage de charge dans un cluster utilisant l’équilibrage de charge réseau Windows.|  

## <a name="BKMK_HARD"></a>Configuration matérielle requise  
La configuration matérielle requise pour ce scénario comprend les éléments suivants :  

-   Au moins deux ordinateurs qui répondent à la configuration matérielle requise pour Windows Server 2012.  

-   Pour le scénario de Load Balancer externe, un matériel dédié est nécessaire \(par exemple, F5 BigIP\).  

-   Pour tester le scénario, vous devez disposer d’au moins un ordinateur exécutant Windows 10 configuré en tant que client VPN Always On.   

## <a name="BKMK_SOFT"></a>Configuration logicielle requise  
Plusieurs conditions sont requises pour ce scénario :  

-   Configuration logicielle requise pour un déploiement sur un seul serveur. Pour plus d’informations, consultez [déployer un serveur DirectAccess unique avec des paramètres avancés](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Un accès à distance unique).  

-   En plus de la configuration logicielle requise pour un serveur unique, il existe un certain nombre d’exigences de cluster\-spécifiques :  

    -   Sur chaque serveur en cluster, le nom d’objet du certificat HTTPs\-IP doit correspondre à l’adresse ConnectTo. Un déploiement de cluster prend en charge une combinaison de certificats génériques et non\-sur des serveurs de cluster.  

    -   Si le serveur Emplacement réseau est installé sur le serveur d’accès à distance, sur chaque serveur en cluster, le certificat du serveur Emplacement réseau doit avoir le même nom d’objet. En outre, le certificat du serveur Emplacement réseau ne doit pas avoir le même nom qu’un des serveurs du déploiement DirectAccess.  

    -   Les certificats IP\-HTTPs et le serveur emplacement réseau doivent être émis à l’aide de la même méthode que celle avec laquelle le certificat sur le serveur unique a été émis. Par exemple, si le serveur unique utilise une autorité de certification publique \(autorité de certification\), tous les serveurs du cluster doivent avoir un certificat émis par une autorité de certification publique. Ou si le serveur unique utilise un certificat auto\-signé pour IP\-HTTPs, tous les serveurs du cluster doivent faire de même.  

    -   Le préfixe IPv6 attribué aux ordinateurs clients DirectAccess sur les clusters de serveurs doit comporter 59 bits. Si VPN est activé, le préfixe VPN doit comporter également 59 bits.  

## <a name="KnownIssues"></a>Problèmes connus  
Les problèmes décrits ci-après sont connus et surviennent souvent lors de la configuration d’un scénario de cluster :  

-   Après la configuration de DirectAccess dans un\-IPv4 uniquement avec une seule carte réseau et après que le DNS64 par défaut \(l’adresse IPv6 qui contient «  : 3333 :: »\) est automatiquement configuré sur la carte réseau, la tentative d’activation de l’équilibrage de charge\-via la console de gestion de l’accès à distance entraîne une invite invitant l’utilisateur à fournir une adresse IPv6 DIP. Si une adresse IPv6 DIP est fournie, la configuration échoue après avoir cliqué sur **Valider** avec l’erreur : Le paramètre est incorrect.  

    Pour résoudre ce problème :  

    1.  Téléchargez la sauvegarde et restaurez les scripts à partir de [Sauvegarder et restaurer la configuration de l’accès à distance](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6).  

    2.  Sauvegarder vos objets de stratégie de groupe d’accès à distance à l’aide de la sauvegarde de script téléchargé\-RemoteAccess. ps1  

    3.  Essayez d’activer l’équilibrage de charge jusqu’à l’étape ayant provoqué l’échec. Dans la boîte de dialogue Activer l’équilibrage de charge, développez la zone Détails, cliquez avec le bouton droit\-cliquez dans la zone Détails, puis cliquez sur **copier le script**.  

    4.  Ouvrez le bloc-notes et collez le contenu du Presse-papiers. Par exemple :  

        ```  
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19 /255.255.255.0','fdc4:29bd:abde:3333::2/128') -InternetVirtualIPAddress @('fdc4:29bd:abde:3333::1/128', '10.244.4.21 /255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose  
        ```  

    5.  Fermez les boîtes de dialogue Accès à distance ouvertes et fermez la console de gestion de l’accès à distance.  

    6.  Modifiez le texte collé et supprimez les adresses IPv6. Par exemple :  

        ```  
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19 /255.255.255.0') -InternetVirtualIPAddress @('10.244.4.21 /255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose  
        ```  

    7.  Dans une fenêtre PowerShell avec élévation de privilèges, exécutez la commande à partir de l’étape précédente.  

    8.  Si l’applet de commande échoue pendant son exécution \(pas en raison de valeurs d’entrée incorrectes\), exécutez la commande Restore\-RemoteAccess. ps1 et suivez les instructions pour vous assurer que l’intégrité de votre configuration d’origine est maintenue.  

    9. Vous pouvez désormais ouvrir de nouveau la console de gestion de l’accès à distance.  
