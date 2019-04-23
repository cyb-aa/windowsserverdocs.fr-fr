---
title: Déployer l’accès à distance dans un cluster
description: Cette rubrique fait partie du guide de déploiement des accès à distance dans un Cluster dans Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ab2b0731a5673e14fb130d539324701a336f30ac
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863630"
---
# <a name="deploy-remote-access-in-a-cluster"></a>Déployer l’accès à distance dans un cluster

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Windows Server 2016 et Windows Server 2012 combinent DirectAccess et Remote Access Service \(RAS\) VPN dans un rôle d’accès à distance unique. Vous pouvez déployer l’accès à distance dans un nombre de scénarios d’entreprise. Cette présentation fournit une introduction au scénario d’entreprise pour le déploiement de plusieurs serveurs d’accès à distance dans une charge de cluster à charge équilibrée avec Windows Network Load Balancing \(NLB\) ou avec un équilibreur de charge externe \(ELB \), tel que F5 Big\-IP.  

## <a name="BKMK_OVER"></a>Description du scénario  
Un déploiement de cluster rassemble plusieurs serveurs d’accès à distance en une seule unité, qui agit alors comme un seul point de contact pour les ordinateurs clients distants qui se connectent via DirectAccess ou VPN au réseau d’entreprise interne à l’aide de l’adresse IP virtuelle externe \(VIP\) adresse du cluster d’accès à distance.  Le trafic vers le cluster est à charge équilibrée à l’aide d’équilibrage de charge réseau Windows ou l’aide d’un équilibreur de charge \(tel que F5 Big\-IP\).  

## <a name="prerequisites"></a>Prérequis  
Avant de déployer ce scénario, prenez connaissance des conditions requises suivantes qui ont leur importance :  

-   Équilibrage de charge par défaut via l’équilibrage de la charge réseau (NLB) Windows.  

-   Les équilibrages de charge externes sont pris en charge.  

-   Monodiffusion est le mode par défaut et celui recommandé pour l’équilibrage de la charge réseau (NLB).  

-   La modification des stratégies en dehors de la console de gestion DirectAccess ou des applets de commande PowerShell n’est pas prise en charge.  

-   Lors de l’équilibrage de charge réseau ou un équilibreur de charge externe est utilisé, le préfixe IPHTTPS ne sont pas modifiables sur n’importe quelle autre que \/59.  

-   Les nœuds à charge équilibrée doivent être dans le même sous-réseau IPv4.  

-   Dans les déploiements ELB, si gérer la sortie est nécessaire, les clients DirectAccess ne peut pas utiliser&nbsp;Teredo. Seul IPHTTPS peut être utilisé pour la fin\-à\-arrêter la communication.  

-   Assurez-vous tout connu NLB\/ELB correctifs sont installés.  

-   Le protocole ISATAP n'est pas pris en charge sur le réseau d'entreprise. Si vous utilisez le protocole ISATAP, vous devez le supprimer et utiliser le protocole IPv6 natif.  

## <a name="in-this-scenario"></a>Dans ce scénario  
Ce scénario de déploiement de cluster inclut plusieurs étapes :  

1.  [Déployer un toujours sur le serveur VPN avec des options avancées](../../vpn/always-on-vpn/deploy/always-on-vpn-adv-options.md). Un seul serveur d’accès à distance avec des paramètres avancés doit être déployé avant de configurer un déploiement de cluster.  

2.  [Planifier un déploiement de Cluster d’accès à distance](plan/Plan-a-Remote-Access-Cluster-Deployment.md). Pour créer un cluster à partir d’un déploiement mono-serveur un nombre d’autres étapes sont requises, y compris la préparation des certificats pour le déploiement du cluster.  

3.  [Configurer un Cluster de l’accès à distance](configure/Configure-a-Remote-Access-Cluster.md). Il s’agit d’un nombre d’étapes de configuration, y compris la préparation du serveur unique pour l’équilibrage de charge réseau Windows ou l’équilibreur de charge externe, préparation des serveurs supplémentaires à rejoindre le cluster et l’activation de l’équilibrage de charge.  

## <a name="BKMK_APP"></a>Applications pratiques  
Le regroupement de plusieurs serveurs en un cluster de serveurs fournit les avantages suivants :  

-   Extensibilité. Un seul serveur d’accès à distance fournit un niveau limité de fiabilité serveur et des performances évolutives. En regroupant les ressources de deux serveurs ou plus dans un cluster individuel, vous augmentez la capacité en termes de débit et de nombre d’utilisateurs.  

-   Haute disponibilité. Un cluster offre une haute disponibilité pour toujours\-sur l’accès. Si un serveur du cluster échoue, les utilisateurs distants peuvent continuer à accéder au réseau d’entreprise via un autre serveur du cluster. Tous les serveurs du cluster possèdent le même ensemble de l’adresse IP virtuelle de cluster \(VIP\) adresses, tout en conservant un unique, des adresses IP pour chaque serveur dédié.  

-   Facilité\-de\-gestion. Un cluster permet la gestion de plusieurs serveurs comme une seule entité. Des paramètres partagés peuvent aisément être définis entre les serveurs du cluster. Paramètres d’accès à distance peuvent être gérés à partir de tous les serveurs du cluster ou à distance à l’aide des outils d’Administration de serveur distant \(RSAT\). De plus, le cluster entier peut être analysé à partir d’une console de gestion de l’accès à distance unique.  

## <a name="BKMK_NEW"></a>Fonctionnalités et rôles inclus dans ce scénario  
Le tableau suivant répertorie les fonctionnalités et rôles requis pour ce scénario :  

|Rôle\/fonctionnalité|Prise en charge de ce scénario|  
|---------|-----------------|  
|Rôle Accès à distance|Ce rôle est installé et désinstallé à l’aide de la console du Gestionnaire de serveur. C'est-à-dire qu’elle englobe à la fois DirectAccess, ce qui était auparavant une fonctionnalité de Windows Server 2008 R2 et de routage et de Services d’accès à distance \(RRAS\), qui étaient auparavant un service de rôle sous la stratégie de réseau et les Services d’accès \(NPAS\) rôle de serveur. Le rôle Accès à distance est constitué de deux composants :<br /><br />-Toujours sur VPN, routage et accès à distance \(RRAS\) VPN DirectAccess et VPN sont gérés ensemble dans la console de gestion de l’accès à distance.<br />-Les fonctionnalités de routage de RRAS routage-RRAS sont gérées dans la console Routage et accès distant héritée.<br /><br />Les dépendances sont les suivantes :<br /><br />-Internet Information Services \(IIS\) serveur Web - cette fonctionnalité est nécessaire pour configurer le réseau emplacement par défaut et le serveur de sonde web.<br />-Windows Database-Used interne pour la gestion des comptes locale sur le serveur d’accès à distance.|  
|Fonctionnalité des outils de gestion de l’accès à distance|Cette fonctionnalité est installée comme suit :<br /><br />-Il est installé par défaut sur un serveur d’accès à distance lorsque le rôle accès à distance est installé et prend en charge de l’interface utilisateur de console Administration à distance.<br />-Il peut éventuellement être installé sur un serveur n'exécutant pas le rôle de serveur d’accès à distance. Dans ce cas, elle est utilisée pour la gestion à distance d’un ordinateur d’accès à distance qui exécute DirectAccess et le réseau privé virtuel (VPN).<br /><br />La fonctionnalité des outils de gestion de l’accès à distance est constituée des éléments suivants :<br /><br />-Accès à distance GUI et outils de ligne de commande<br />-Module d’accès distant pour Windows PowerShell<br /><br />Les dépendances incluent :<br /><br />-Console de gestion de stratégie de groupe<br />-Kit d’Administration de Connection Manager RAS \(CMAK\)<br />-Windows PowerShell 3.0<br />-Infrastructure et des outils de gestion graphique|  
|Équilibrage de la charge réseau|Cette fonctionnalité assure l’équilibrage de charge dans un cluster utilisant l’équilibrage de charge réseau Windows.|  

## <a name="BKMK_HARD"></a>Configuration matérielle requise  
La configuration matérielle requise pour ce scénario comprend les éléments suivants :  

-   Au moins deux ordinateurs qui répondent à la configuration matérielle requise pour Windows Server 2012.  

-   Pour le scénario de l’équilibreur de charge externe, le matériel dédié est nécessaire \(par exemple, F5 BigIP\).  

-   Pour tester le scénario, vous devez disposer d’au moins un ordinateur exécutant Windows 10 est configuré comme un client VPN Always On.   

## <a name="BKMK_SOFT"></a>Configuration logicielle requise  
Plusieurs conditions sont requises pour ce scénario :  

-   Configuration logicielle requise pour un déploiement sur un seul serveur. Pour plus d’informations, consultez [déployer un serveur DirectAccess unique avec des paramètres avancés](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Un seul accès à distance).  

-   En plus de la configuration logicielle requise pour un seul serveur, il existe un nombre de cluster\-des exigences spécifiques :  

    -   Sur chaque serveur en cluster l’adresse IP\-certificat HTTPS nom du sujet doit correspondre à l’adresse ConnectTo. Un déploiement de cluster prend en charge une combinaison de caractères génériques et non\-certificats avec caractères génériques sur les serveurs du cluster.  

    -   Si le serveur Emplacement réseau est installé sur le serveur d’accès à distance, sur chaque serveur en cluster, le certificat du serveur Emplacement réseau doit avoir le même nom d’objet. En outre, le certificat du serveur Emplacement réseau ne doit pas avoir le même nom qu’un des serveurs du déploiement DirectAccess.  

    -   IP\-certificats de serveur d’emplacement réseau et de HTTPS doivent être émis en utilisant la même méthode avec laquelle le certificat vers le serveur a été émis. Par exemple, si le serveur individuel utilise une autorité de certification publique \(autorité de certification\) alors tous les serveurs du cluster doivent avoir un certificat émis par une autorité de certification publique. Ou si le serveur individuel utilise un libre-service\-certificat auto-signé pour IP\-HTTPS puis tous les serveurs du cluster doivent faire de même.  

    -   Le préfixe IPv6 attribué aux ordinateurs clients DirectAccess sur les clusters de serveurs doit comporter 59 bits. Si VPN est activé, le préfixe VPN doit comporter également 59 bits.  

## <a name="KnownIssues"></a>Problèmes connus  
Les problèmes décrits ci-après sont connus et surviennent souvent lors de la configuration d’un scénario de cluster :  

-   Après avoir configuré DirectAccess dans IPv4\-déploiement avec une seule carte réseau et après la valeur par défaut DNS64 \(l’adresse IPv6 qui contient « : 3333 :: »\) est automatiquement configuré sur la carte réseau, la tentative d’activation charge\-équilibrage via la console de gestion de l’accès à distance entraîne une invite pour l’utilisateur à fournir une adresse IPv6 DIP. Si une adresse IPv6 DIP est fournie, la configuration échoue après avoir cliqué sur **Valider** avec l’erreur : Le paramètre est incorrect  

    Pour résoudre ce problème :  

    1.  Téléchargez la sauvegarde et restaurez les scripts à partir de [Sauvegarder et restaurer la configuration de l’accès à distance](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6).  

    2.  Sauvegarder vos GPO d’accès à distance à l’aide de téléchargées script sauvegarde\-remoteaccess.ps1  

    3.  Essayez d’activer l’équilibrage de charge jusqu’à l’étape ayant provoqué l’échec. Dans la boîte de dialogue Activer l’équilibrage de charge, développez la zone de détails, droite\-cliquez dans la zone de détails, puis cliquez sur **copier le Script**.  

    4.  Ouvrez le bloc-notes et collez le contenu du Presse-papiers. Exemple :  

        ```  
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19 /255.255.255.0','fdc4:29bd:abde:3333::2/128') -InternetVirtualIPAddress @('fdc4:29bd:abde:3333::1/128', '10.244.4.21 /255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose  
        ```  

    5.  Fermez les boîtes de dialogue Accès à distance ouvertes et fermez la console de gestion de l’accès à distance.  

    6.  Modifiez le texte collé et supprimez les adresses IPv6. Exemple :  

        ```  
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19 /255.255.255.0') -InternetVirtualIPAddress @('10.244.4.21 /255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose  
        ```  

    7.  Dans une fenêtre PowerShell avec élévation de privilèges, exécutez la commande à partir de l’étape précédente.  

    8.  Si l’applet de commande échoue pendant son exécution \(pas en raison de valeurs d’entrée incorrects\), exécutez la commande Restore\-remoteaccess.ps1 et suivez les instructions pour vous assurer que l’intégrité de votre configuration d’origine est conservée. .  

    9. Vous pouvez désormais ouvrir de nouveau la console de gestion de l’accès à distance.  
