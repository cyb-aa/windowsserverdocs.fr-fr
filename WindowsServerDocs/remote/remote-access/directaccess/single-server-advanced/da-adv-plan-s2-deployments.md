---
title: Plan de l’étape 2 avancées des déploiements de DirectAccess
description: Cette rubrique fait partie du guide de déployer un serveur DirectAccess unique avec les paramètres avancés pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3bba28d4-23e2-449f-8319-7d2190f68d56
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 83dd3c737c616574750fecf51184996aba5703f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845300"
---
# <a name="step-2-plan-advanced-directaccess-deployments"></a>Plan de l’étape 2 avancées des déploiements de DirectAccess

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Une fois que vous avez planifié l'infrastructure DirectAccess, l'étape suivante du déploiement des fonctionnalités avancées de DirectAccess sur un serveur individuel avec IPv4 et IPv6 consiste à planifier les paramètres pour l'Assistant Configuration de l'accès à distance.  
  
|Tâche|Description|  
|----|--------|  
|[2.1 planifier le déploiement du client](#bkmk_21client)|Planifiez la façon dont les ordinateurs clients pourront se connecter à l'aide de DirectAccess. Décidez quels ordinateurs gérés seront configurés en tant que clients DirectAccess et planifiez le déploiement de l'Assistant Connectivité réseau ou de l'Assistant Connectivité DirectAccess sur les ordinateurs clients.|  
|[2.2 planifier le déploiement du serveur DirectAccess](#bkmk_22server)|Planifiez la façon de déployer le serveur DirectAccess.|  
|[2.3 planifier les serveurs d’infrastructure](#bkmk_23Infservers)|Planifiez les serveurs d'infrastructure pour votre déploiement de DirectAccess, y compris le serveur Emplacement réseau DirectAccess, les serveurs DNS (Domain Name System) et les serveurs d'administration DirectAccess.|  
|[2.4 planifier les serveurs d’applications](#bkmk_AppServers)|Planifiez des serveurs d'applications IPv4 et IPv6, et considérez éventuellement si une authentification de bout en bout entre les ordinateurs clients DirectAccess et les serveurs d'applications internes est requise.|  
|[2.5 planifier DirectAccess et les clients VPN tiers](#bkmk_DAandVPN)|Lors du déploiement de DirectAccess avec des clients VPN tiers, il peut être nécessaire de définir une valeur de Registre afin de permettre la coexistence sans heurts des deux solutions d'accès à distance.|  
  
## <a name="bkmk_21client"></a>2.1 planifier le déploiement du client  
Vous devez prendre trois décisions lors de la planification du déploiement de vos clients :  
  
1.  DirectAccess sera-t-il disponible uniquement pour les ordinateurs portables ou pour n'importe quel ordinateur ?  
  
    Lorsque vous configurez des clients DirectAccess dans l'Assistant Installation des clients DirectAccess, vous pouvez choisir d'autoriser uniquement les ordinateurs portables des groupes de sécurité spécifiés à se connecter à l'aide de DirectAccess. Si vous voulez restreindre l'accès aux ordinateurs portables, le service Accès à distance configure automatiquement un filtre WMI pour garantir que l'objet de stratégie de groupe Client DirectAccess est appliqué uniquement aux ordinateurs portables des groupes de sécurité spécifiés. Pour activer ce paramètre, l'administrateur de l'accès à distance doit disposer des autorisations Créer ou Modifier la sécurité pour créer ou modifier des filtres WMI d'objet de stratégie de groupe.  
  
2.  Quels groupes de sécurité contiendront les ordinateurs clients DirectAccess ?  
  
    Les paramètres des clients DirectAccess sont contenus dans l'objet de stratégie de groupe de client DirectAccess. L'objet de stratégie de groupe est appliqué aux ordinateurs figurant dans les groupes de sécurité que vous spécifiez dans l'Assistant Installation des clients DirectAccess. Vous pouvez spécifier que les groupes de sécurité doivent être contenus dans tout domaine pris en charge. Pour plus d’informations, consultez la section [1.7 planifier Active Directory Domain Services](da-adv-plan-s1-infrastructure.md#bkmk_16AD).  
  
    Avant de configurer DirectAccess, vous devez créer les groupes de sécurité. Vous pouvez ajouter des ordinateurs au groupe de sécurité une fois le déploiement de DirectAccess terminé. Toutefois, si vous ajoutez des ordinateurs clients qui résident dans un autre domaine que le groupe de sécurité, l'objet de stratégie de groupe de client ne sera pas appliqué à ces clients. Par exemple, si vous avez créé le groupe de sécurité GS1 dans un domaine A pour les clients DirectAccess et ajoutez ensuite des clients d'un domaine B à ce groupe, l'objet de stratégie de groupe de client ne sera pas appliqué aux clients du domaine B. Pour éviter ce problème, créez un nouveau groupe de sécurité client pour chaque domaine contenant des ordinateurs clients DirectAccess. Ou, si vous ne voulez pas créer un nouveau groupe de sécurité, exécutez l'applet de commande Windows PowerShell **Add-DAClient** avec le nom du nouvel objet de stratégie de groupe pour le nouveau domaine.  
  
3.  Quels paramètres configurerez-vous pour l'Assistant Connectivité réseau ?  
  
    L'Assistant Connectivité réseau s'exécute sur les ordinateurs clients et fournit des informations supplémentaires sur la connexion de DirectAccess aux utilisateurs finaux. Dans l'Assistant Installation des clients DirectAccess, vous pouvez configurer les éléments suivants :  
  
    -   **Vérificateurs de connectivité**  
  
        Une sonde web par défaut est créée et elle est utilisée par les clients pour valider la connectivité au réseau interne. Par défaut, son nom est :  
  
        https://directaccess-WebProbeHost.<domain_name>  
  
        Ce nom doit être enregistré manuellement dans DNS. Vous pouvez créer d'autres vérificateurs de connectivité en utilisant d'autres adresses web via HTTP ou **ping**. Pour chaque vérificateur de connectivité, une entrée DNS doit exister.  
  
    -   **Une adresse de messagerie du support technique**  
  
        Si les utilisateurs finaux rencontrent des problèmes de connectivité DirectAccess, ils peuvent envoyer un courrier électronique contenant des informations de diagnostic à l'administrateur DirectAccess pour que ce dernier résolve le problème.  
  
    -   **Un nom de connexion DirectAccess**  
  
        Spécifiez un nom de connexion DirectAccess pour aider les utilisateurs finaux à identifier la connexion DirectAccess sur leur ordinateur.  
  
    -   **Autoriser les clients DirectAccess à utiliser la résolution de noms locale**  
  
        Les clients ont besoin d'une méthode pour résoudre localement les noms. Si vous autorisez les clients DirectAccess à utiliser la résolution de noms locale, les utilisateurs finaux peuvent utiliser les serveurs DNS locaux pour résoudre les noms. Quand des utilisateurs finaux choisissent d'utiliser des serveurs DNS locaux pour la résolution de noms, DirectAccess n'envoie pas les demandes de résolution pour les noms en une partie au serveur DNS d'entreprise interne. Il utilise la résolution de noms locale à la place (en utilisant les protocoles LLMNR (Link-Local Multicast Name Resolution) et NetBIOS sur TCP/IP).  
  
## <a name="bkmk_22server"></a>2.2 planifier le déploiement du serveur DirectAccess  
Prenez en compte les décisions suivantes quand vous planifiez le déploiement de votre serveur DirectAccess :  
  
-   **Topologie de réseau**  
  
    Quelques topologies sont disponibles lorsque vous déployez un serveur DirectAccess :  
  
    -   **Deux cartes réseau**. Si vous disposez de deux cartes réseau, vous pouvez configurer DirectAccess avec une carte réseau connectée directement à Internet et l'autre connectée au réseau interne. Le serveur peut aussi être installé derrière un périphérique de périmètre tel qu'un pare-feu ou un routeur. Dans cette configuration, une carte réseau est connectée au réseau de périmètre et l'autre au réseau interne.  
  
    -   **Une seule carte réseau**. Dans cette configuration, le serveur DirectAccess est installé derrière un périphérique de périmètre tel qu'un pare-feu ou un routeur. La carte réseau est connectée au réseau interne.  
  
    Pour plus d’informations sur la sélection de la topologie pour votre déploiement, consultez [1.1 topologie de réseau de Plan et les paramètres](da-adv-plan-s1-infrastructure.md#bkmk_11Networksvrtopsettings).  
  
-   **Adresse ConnectTo**  
  
    Les ordinateurs clients utilisent l'adresse ConnectTo pour se connecter au serveur DirectAccess. L'adresse que vous choisissez doit correspondre au nom de sujet du certificat IP-HTTPS que vous déployez pour la connexion IP-HTTPS et doit être disponible dans le DNS public.  
  
-   **Cartes réseau**  
  
    L'Assistant Installation du serveur d'accès à distance détecte automatiquement les cartes réseau configurées sur le serveur DirectAccess. Vous devez vous assurer que les cartes appropriées sont sélectionnées.  
  
-   **Certificat IP-HTTPS**  
  
    L'Assistant Installation du serveur d'accès à distance détecte automatiquement un certificat adapté à la connexion IP-HTTPS. Le nom de sujet du certificat que vous sélectionnez doit correspondre à l'adresse ConnectTo. Si vous utilisez des certificats auto-signés, vous pouvez choisir d'utiliser un certificat créé automatiquement par le serveur d'accès à distance.  
  
-   **Préfixes IPv6**  
  
    Si l'Assistant Installation du serveur d'accès à distance détecte qu'IPv6 a été déployé sur les cartes réseau, il renseigne automatiquement les préfixes IPv6 pour le réseau interne, un préfixe IPv6 à affecter aux ordinateurs clients DirectAccess et un préfixe IPv6 à affecter aux ordinateurs clients VPN. Si les préfixes générés automatiquement ne sont pas corrects pour votre infrastructure IPv6 native, vous devez les modifier manuellement. Pour plus d’informations, consultez [1.1 topologie de réseau de Plan et les paramètres](da-adv-plan-s1-infrastructure.md#bkmk_11Networksvrtopsettings).  
  
-   **Authentification**  
  
    Choisissez comment les clients DirectAccess s'authentifieront auprès du serveur DirectAccess :  
  
    -   **Authentification utilisateur** Vous pouvez permettre aux utilisateurs de s'authentifier à l'aide des informations d'identification Active Directory ou d'une authentification à deux facteurs. Pour plus d’informations sur l’authentification à deux facteurs, consultez [Deploy Remote Access avec l’authentification OTP](https://technet.microsoft.com/library/hh831379.aspx).  
  
    -   **Authentification d'ordinateur**. Vous pouvez configurer l'authentification d'ordinateur pour utiliser des certificats ou pour utiliser le serveur DirectAccess en tant que proxy Kerberos de la part du client. Pour plus d’informations, consultez [1.3 Configuration requise des certificats Plan](da-adv-plan-s1-infrastructure.md#bkmk_12CAsandCerts).  
  
    -   **Les clients Windows 7**. Par défaut, les ordinateurs clients qui exécutent Windows 7 ne peut pas se connecter à un déploiement de Windows Server 2012 R2 ou Windows Server 2012 DirectAccess. Si vous avez des clients qui exécutent Windows 7 dans votre organisation, et qui nécessitent un accès à distance aux ressources internes, vous pouvez les autoriser à se connecter. Tout ordinateur client que vous voulez autoriser à accéder aux ressources internes doit être membre d'un groupe de sécurité que vous spécifiez dans l'Assistant Installation des clients DirectAccess.  
  
        > [!NOTE]  
        > Autorisant les clients qui exécutent 7 Windows pour se connecter à l’aide de DirectAccess requiert que vous utilisez l’authentification de certificat d’ordinateur.  
  
-   **Configuration de VPN**  
  
    Avant de configurer DirectAccess, choisissez si vous envisagez de fournir l'accès VPN à des clients distants non compatibles DirectAccess. Vous devez fournir l'accès VPN si vous possédez dans votre organisation des ordinateurs clients qui ne prennent pas en charge la connectivité DirectAccess (car ils sont non gérés ou exécutent un système d'exploitation pour lequel DirectAccess n'est pas pris en charge). L'Assistant Installation du serveur d'accès à distance vous permet de configurer la manière dont les adresses IP sont attribuées (en utilisant DHCP ou à partir d'un pool d'adresses statiques) et les clients VPN authentifiés (en utilisant Active Directory ou un serveur RADIUS (Remote Authentication Dial-Up Service)).  
  
## <a name="bkmk_23Infservers"></a>2.3 planifier les serveurs d’infrastructure  
DirectAccess nécessite trois types de serveurs d'infrastructure :  
  
-   **Serveurs DNS**. Pour plus d'informations, voir [1.4 Planifier la configuration DNS requise](da-adv-plan-s1-infrastructure.md#bkmk_14Dns)  
  
-   **Serveur Emplacement réseau**. Pour plus d'informations, voir [1.5 Planifier le serveur Emplacement réseau](da-adv-plan-s1-infrastructure.md#bkmk_14NLS)  
  
-   **Serveurs d'administration**. Pour plus d'informations, voir [1.6 Planifier les serveurs d'administration](da-adv-plan-s1-infrastructure.md#bkmk_15mgmtservers)  
  
## <a name="bkmk_AppServers"></a>2.4 planifier les serveurs d’applications  
Les serveurs d'applications sont les serveurs du réseau d'entreprise qui sont accessibles par les ordinateurs clients via une connexion DirectAccess. Ils sont identifiés lorsqu'ils sont ajoutés dans un groupe de sécurité. L'objet de stratégie de groupe de serveur d'applications est ensuite appliqué aux serveurs de ce groupe.  
  
> [!NOTE]  
> L'ajout des serveurs d'applications dans un groupe de sécurité est requis uniquement si vous avez besoin d'un chiffrement et d'une authentification de bout en bout.  
  
Vous pouvez éventuellement avoir besoin d'un chiffrement et d'une authentification de bout en bout entre un client DirectAccess et des serveurs d'applications internes sélectionnés. Si vous configurez l'authentification de bout en bout, les clients DirectAccess utilisent une stratégie de transport IPsec. Cette stratégie exige que l'authentification et la protection du trafic des sessions IPsec soient terminées aux serveurs d'applications spécifiés. Dans ce cas, le serveur d'accès à distance transfère les sessions IPsec authentifiées et protégées vers les serveurs d'applications.  
  
Par défaut, quand vous étendez l'authentification aux serveurs d'applications, la charge de données est chiffrée entre le client DirectAccess et les serveurs d'applications. Vous pouvez choisir de ne pas chiffrer le trafic et d'utiliser l'authentification uniquement. Toutefois, cela est moins sécurisée que l’authentification et chiffrement, et il est pris en charge uniquement pour les serveurs d’applications exécutant Windows Server 2008 R2, les systèmes d’exploitation Windows Server 2012.  
  
## <a name="bkmk_DAandVPN"></a>2.5 planifier DirectAccess et les clients VPN tiers  
Certains clients VPN tiers ne créent pas de connexions dans le dossier de connexions réseau. Cela peut amener DirectAccess à déterminer qu'il n'a aucune connectivité intranet lorsque la connexion VPN est établie et que la connectivité à l'intranet existe. Cela se produit lorsque des clients VPN tiers inscrivent leurs interfaces en les définissant comme types de point de terminaison NDIS. Vous pouvez permettre la coexistence avec ces types de clients VPN en définissant la valeur de Registre suivante sur 1 sur les clients DirectAccess :  
  
**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\NlaSvc\Parameters\ShowDomainEndpointInterfaces (REG_DWORD)**  
  
Certains clients VPN tiers utilisent une configuration de tunneling fractionné, qui permet à l'ordinateur client VPN d'accéder directement à Internet sans avoir à envoyer le trafic via la connexion VPN à l'intranet.  
  
Les configurations de tunneling fractionné laissent en général le paramètre de passerelle par défaut sur le client VPN comme non configuré ou comme configuré avec uniquement des zéros (0.0.0.0). Vous pouvez confirmer ceci en établissant une connexion VPN à votre intranet et en utilisant l'outil en ligne de commande Ipconfig.exe pour afficher la configuration obtenue.  
  
Si la connexion VPN répertorie sa passerelle par défaut comme vide ou remplie de zéros (0.0.0.0), votre client VPN est configuré de cette manière. Par défaut, le client DirectAccess n'identifie pas les configurations de tunneling fractionné. Pour configurer des clients DirectAccess afin de détecter ces types de configurations de client VPN et coexister avec eux, définissez la valeur de Registre suivante sur 1.  
  
**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\NlaSvc\Parameters\Internet\ EnableNoGatewayLocationDetection (REG_DWORD)**  
  
## <a name="BKMK_Links"></a>Étape précédente  
  
-   [Étape 1 : Planifier l’Infrastructure DirectAccess](da-adv-plan-s1-infrastructure.md)  
  


