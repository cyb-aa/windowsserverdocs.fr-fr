---
title: Serveur NPS (Network Policy Server)
description: Cette rubrique fournit une vue d’ensemble du serveur NPS (Network Policy Server) dans Windows Server 2016 et Windows Server 2019, et inclut des liens vers des conseils supplémentaires sur NPS.
manager: dougkim
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 9c7a67e0-0953-479c-8736-ccb356230bde
ms.author: pashort
author: shortpatti
ms.date: 06/20/2018
ms.openlocfilehash: 5685d4dae742245c131ff2fee3114e905e94e9bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396007"
---
# <a name="network-policy-server-nps"></a>Serveur NPS (Network Policy Server)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2019

Vous pouvez utiliser cette rubrique pour obtenir une vue d’ensemble du serveur NPS (Network Policy Server) dans Windows Server 2016 et Windows Server 2019. NPS est installé lorsque vous installez la fonctionnalité services de stratégie et d’accès réseau (NPAS) dans Windows Server 2016 et le serveur 2019.

> [!NOTE]
> En plus de cette rubrique, la documentation NPS suivante est disponible.
> - [Meilleures pratiques pour le serveur NPS (Network Policy Server)](nps-best-practices.md)
> - [Prise en main avec le serveur NPS (Network Policy Server)](nps-getstart-top.md)
> - [Planifier le serveur NPS (Network Policy Server)](nps-plan-top.md)
> - [Déployer un serveur NPS (Network Policy Server)](nps-deploy.md)
> - [Gérer le serveur NPS (Network Policy Server)](nps-manage-top.md)
> - [Applets de commande du serveur de stratégie réseau (NPS) dans Windows PowerShell](https://technet.microsoft.com/library/jj872739.aspx) pour windows server 2016 et Windows 10
> - [Applets de commande du serveur de stratégie réseau (NPS) dans Windows PowerShell](https://technet.microsoft.com/library/jj872739.aspx) pour windows server 2012 R2 et Windows 8.1
> - [Applets de commande NPS dans Windows PowerShell](https://technet.microsoft.com/library/jj872739.aspx) pour windows server 2012 et Windows 8

Un serveur NPS \(Network Policy Server\) vous permet de créer et d’appliquer des stratégies d’accès réseau valides pour toute l’organisation, qui régissent les demandes d’authentification et l’autorisation des demandes de connexion.

Vous pouvez également configurer NPS en tant que proxy protocole RADIUS (Remote Authentication Dial-In User Service) (RADIUS) pour transférer les demandes de connexion à un serveur NPS ou un autre serveur RADIUS distant afin de pouvoir équilibrer la charge des demandes de connexion et les transférer au domaine approprié pour l’authentification. et l’autorisation.

NPS vous permet de configurer et de gérer de manière centralisée l’authentification d’accès réseau, l’autorisation et la gestion des comptes avec les fonctionnalités suivantes :

- **Serveur RADIUS**. NPS effectue l’authentification, l’autorisation et la comptabilité centralisées pour les connexions sans fil, de commutateur d’authentification, d’accès à distance et de réseau privé virtuel (VPN). Lorsque vous utilisez NPS en tant que serveur RADIUS, vous devez configurer les serveurs d’accès réseau, tels que les point d’accès sans fil et les serveurs VPN, en tant que clients RADIUS dans NPS. Vous devez également configurer les stratégies réseau utilisées par NPS pour autoriser les demandes de connexion. Vous pouvez éventuellement configurer la gestion de comptes RADIUS de sorte que NPS enregistre les informations de gestion de comptes dans des fichiers journaux sur le disque dur local ou dans une base de données Microsoft SQL Server. Pour plus d’informations, consultez [serveur RADIUS](#radius-server).
- **Proxy RADIUS**. Lorsque vous utilisez NPS en tant que proxy RADIUS, vous configurez des stratégies de demande de connexion qui indiquent au serveur NPS les demandes de connexion à transférer vers d’autres serveurs RADIUS et à quels serveurs RADIUS vous souhaitez transférer les demandes de connexion. Vous pouvez éventuellement configurer NPS pour le transfert des données de gestion de comptes à enregistrer par un ou plusieurs ordinateurs d’un groupe de serveurs RADIUS distants. Pour configurer NPS en tant que serveur proxy RADIUS, consultez les rubriques suivantes. Pour plus d’informations, consultez [proxy RADIUS](#radius-proxy).
    - [Configurer des stratégies de demande de connexion](nps-crp-configure.md)
- **Gestion des comptes RADIUS**. Vous pouvez configurer NPS pour consigner les événements dans un fichier journal local ou sur une instance locale ou distante de Microsoft SQL Server. Pour plus d’informations, consultez [journalisation NPS](#nps-logging).

> [!IMPORTANT]
> La protection d’accès réseau \(NAP @ no__t-1, l’autorité d’inscription d’intégrité \(HRA @ no__t-3 et le protocole d’autorisation des informations d’identification de l’hôte \(HCAP @ no__t-5 ont été dépréciés dans Windows Server 2012 R2, et ne sont pas disponibles dans Windows Server 2016. Si vous avez un déploiement de protection d’accès réseau à l’aide de systèmes d’exploitation antérieurs à Windows Server 2016, vous ne pouvez pas migrer votre déploiement NAP vers Windows Server 2016.

Vous pouvez configurer NPS avec n’importe quelle combinaison de ces fonctionnalités. Par exemple, vous pouvez configurer un serveur NPS comme serveur RADIUS pour les connexions VPN et également comme proxy RADIUS pour transférer certaines demandes de connexion aux membres d’un groupe de serveurs RADIUS distants pour l’authentification et l’autorisation dans un autre domaine.

## <a name="windows-server-editions-and-nps"></a>Éditions de Windows Server et NPS

NPS offre différentes fonctionnalités en fonction de l’édition de Windows Server que vous installez.

### <a name="windows-server-2016-or-windows-server-2019-standarddatacenter-edition"></a>Windows Server 2016 ou Windows Server 2019 standard/Datacenter Edition

Avec NPS dans Windows Server 2016 standard ou Datacenter, vous pouvez configurer un nombre illimité de clients RADIUS et de groupes de serveurs RADIUS distants. En outre, vous pouvez configurer des clients RADIUS en spécifiant une plage d'adresses IP.

> [!NOTE]
> La fonctionnalité services de stratégie et d’accès réseau WIndows n’est pas disponible sur les systèmes installés avec une option d’installation minimale.

Les sections suivantes fournissent des informations plus détaillées sur NPS en tant que serveur RADIUS et proxy.

## <a name="radius-server-and-proxy"></a>Serveur RADIUS et proxy

Vous pouvez utiliser NPS en tant que serveur RADIUS, proxy RADIUS, ou les deux.

### <a name="radius-server"></a>Serveur RADIUS

NPS est l’implémentation Microsoft de la norme RADIUS spécifiée par l’IETF (Internet Engineering Task Force) @no__t 0IETF @ no__t-1 dans les RFC 2865 et 2866. En tant que serveur RADIUS, NPS effectue l’authentification, l’autorisation et la comptabilité centralisées des connexions pour de nombreux types d’accès réseau, y compris sans fil, commutateur d’authentification, accès à distance et réseau privé virtuel \(VPN @ no__t-1 accès à distance, et connexions routeur à routeur.

> [!NOTE]
> Pour plus d’informations sur le déploiement de NPS en tant que serveur RADIUS, consultez [déployer un serveur de stratégie réseau](nps-deploy.md).

NPS permet l’utilisation d’un ensemble hétérogène de périphériques sans fil, commutateur, accès à distance ou VPN. Vous pouvez utiliser NPS avec le service d’accès à distance, qui est disponible dans Windows Server 2016.

NPS utilise un domaine Active Directory Domain Services \(AD DS @ no__t-1 ou la base de données de comptes d’utilisateurs SAM (Security Accounts Manager) locale pour authentifier les informations d’identification de l’utilisateur pour les tentatives de connexion. Lorsqu’un serveur NPS est membre d’un domaine AD DS, NPS utilise le service d’annuaire en tant que base de données de compte d’utilisateur et fait partie d’une solution d’authentification unique. Le même jeu d’informations d’identification est utilisé pour le contrôle d’accès réseau @no__t 0authenticating et pour autoriser l’accès à un réseau @ no__t-1 et pour se connecter à un domaine AD DS.

> [!NOTE]
> NPS utilise les propriétés de numérotation du compte d’utilisateur et des stratégies réseau pour autoriser une connexion.

Les fournisseurs de services Internet \(ISPs @ no__t-1 et les organisations qui maintiennent l’accès au réseau ont le plus grand défi de gérer tous les types d’accès réseau à partir d’un point d’administration unique, quel que soit le type d’équipement d’accès réseau utilisé. La norme RADIUS prend en charge cette fonctionnalité dans des environnements homogènes et hétérogènes. RADIUS est un protocole client-serveur qui permet l’équipement d’accès réseau (utilisé en tant que clients RADIUS) pour envoyer des demandes d’authentification et de gestion des comptes à un serveur RADIUS.

Un serveur RADIUS a accès aux informations de compte d’utilisateur et peut vérifier les informations d’identification d’authentification d’accès réseau. Si les informations d’identification de l’utilisateur sont authentifiées et que la tentative de connexion est autorisée, le serveur RADIUS autorise l’accès des utilisateurs en fonction des conditions spécifiées, puis enregistre la connexion d’accès réseau dans un journal de comptabilité. L’utilisation de RADIUS permet de collecter et de conserver les données d’authentification, d’autorisation et de gestion des utilisateurs de l’accès réseau dans un emplacement central, plutôt que sur chaque serveur d’accès.

#### <a name="using-nps-as-a-radius-server"></a>Utilisation de NPS en tant que serveur RADIUS

Vous pouvez utiliser NPS en tant que serveur RADIUS dans les cas suivants :

- Vous utilisez un domaine AD DS ou la base de données de comptes d’utilisateurs SAM locale comme base de données de compte d’utilisateur pour les clients d’accès. 
- Vous utilisez l’accès à distance sur plusieurs serveurs d’accès à distance, serveurs VPN ou routeurs de connexion à la demande, et vous souhaitez centraliser la configuration des stratégies réseau et de la journalisation et de la gestion des connexions. 
- Vous vous contentez d’externaliser votre accès à distance, VPN ou sans fil à un fournisseur de services. Les serveurs d’accès utilisent RADIUS pour authentifier et autoriser les connexions effectuées par les membres de votre organisation.
- Vous souhaitez centraliser l’authentification, l’autorisation et la gestion des comptes pour un ensemble hétérogène de serveurs d’accès.

L’illustration suivante montre le serveur NPS en tant que serveur RADIUS pour un large éventail de clients d’accès. 

![NPS en tant que serveur RADIUS](../../media/Nps-Server/Nps-Server.jpg)

### <a name="radius-proxy"></a>Proxy RADIUS

En tant que proxy RADIUS, NPS transfère les messages d’authentification et de gestion des comptes à NPS et à d’autres serveurs RADIUS. Vous pouvez utiliser NPS en tant que proxy RADIUS pour fournir le routage des messages RADIUS entre les clients RADIUS \(also appelés serveurs d’accès réseau @ no__t-1 et les serveurs RADIUS qui effectuent l’authentification des utilisateurs, l’autorisation et la gestion des comptes pour la tentative de connexion. 

Lorsqu’il est utilisé en tant que proxy RADIUS, le serveur NPS est un point de commutation ou de routage central via lequel circulent les messages d’accès et de gestion des comptes RADIUS. Le serveur NPS enregistre les informations dans un journal de comptabilité sur les messages qui sont transférés.

#### <a name="using-nps-as-a-radius-proxy"></a>Utilisation de NPS en tant que proxy RADIUS

Vous pouvez utiliser NPS en tant que proxy RADIUS dans les cas suivants :

- Vous êtes un fournisseur de services qui offre des services d’accès à distance, de réseau privé virtuel ou d’accès réseau sans fil externalisés à plusieurs clients. Vos serveurs NAS envoient des demandes de connexion au proxy RADIUS NPS. En fonction de la partie domaine du nom d’utilisateur dans la demande de connexion, le proxy RADIUS NPS transfère la demande de connexion à un serveur RADIUS géré par le client et peut authentifier et autoriser la tentative de connexion.
- Vous souhaitez fournir une authentification et une autorisation pour les comptes d’utilisateur qui ne sont pas membres du domaine dans lequel le serveur NPS est membre ou d’un autre domaine qui a une relation d’approbation bidirectionnelle avec le domaine dont le serveur NPS est membre. Cela comprend les comptes dans les domaines non approuvés, les domaines approuvés à sens unique et les autres forêts. Au lieu de configurer vos serveurs d’accès pour qu’ils envoient leurs demandes de connexion à un serveur RADIUS NPS, vous pouvez les configurer pour qu’ils envoient leurs demandes de connexion à un proxy RADIUS NPS. Le proxy RADIUS NPS utilise la partie nom de domaine du nom d’utilisateur et transfère la demande à un serveur NPS dans le domaine ou la forêt approprié (e). Les tentatives de connexion pour les comptes d’utilisateur dans un domaine ou une forêt peuvent être authentifiées pour les serveurs de domaine dans un autre domaine ou une autre forêt.
- Vous souhaitez effectuer l’authentification et l’autorisation à l’aide d’une base de données qui n’est pas une base de données de compte Windows. Dans ce cas, les demandes de connexion qui correspondent à un nom de domaine spécifié sont transférées à un serveur RADIUS, qui a accès à une base de données de comptes d’utilisateurs et de données d’autorisation différente. Parmi les autres bases de données utilisateur, citons les bases de données Novell Directory Services (NDS) et langage SQL (SQL).
- Vous souhaitez traiter un grand nombre de demandes de connexion. Dans ce cas, au lieu de configurer vos clients RADIUS pour qu’ils tentent d’équilibrer leurs demandes de connexion et de gestion de comptes entre plusieurs serveurs RADIUS, vous pouvez les configurer pour envoyer leurs demandes de connexion et de gestion de comptes à un proxy RADIUS NPS. Le proxy RADIUS NPS équilibre de manière dynamique la charge des demandes de connexion et de gestion des comptes sur plusieurs serveurs RADIUS et augmente le traitement d’un grand nombre de clients RADIUS et d’authentifications par seconde.
- Vous souhaitez fournir une authentification et une autorisation RADIUS pour les fournisseurs de services externalisés et réduire la configuration du pare-feu intranet. Un pare-feu Intranet se trouve entre votre réseau de périmètre (le réseau entre votre intranet et Internet) et l’intranet. En plaçant un serveur NPS sur votre réseau de périmètre, le pare-feu entre votre réseau de périmètre et l’intranet doit autoriser le trafic entre le serveur NPS et plusieurs contrôleurs de domaine. En remplaçant le serveur NPS par un proxy NPS, le pare-feu doit autoriser uniquement le trafic RADIUS entre le proxy NPS et un ou plusieurs NPSs au sein de votre intranet.

L’illustration suivante montre le serveur NPS en tant que proxy RADIUS entre les clients RADIUS et les serveurs RADIUS.

![NPS en tant que proxy RADIUS](../../media/Nps-Proxy/Nps-Proxy.jpg)

Avec NPS, les organisations peuvent également externaliser l’infrastructure d’accès à distance à un fournisseur de services tout en conservant le contrôle de l’authentification, de l’autorisation et de la gestion des comptes utilisateur.

Les configurations NPS peuvent être créées pour les scénarios suivants :

- Accès sans fil
- Accès à distance à l’organisation ou au réseau privé virtuel (VPN)
- Accès à distance ou sans fil externalisé
- Accès Internet
- Accès authentifié aux ressources extranet pour les partenaires commerciaux

## <a name="radius-server-and-radius-proxy-configuration-examples"></a>Exemples de configuration de serveur RADIUS et de proxy RADIUS

Les exemples de configuration suivants montrent comment configurer NPS en tant que serveur RADIUS et proxy RADIUS.

**NPS en tant que serveur RADIUS**. Dans cet exemple, le serveur NPS est configuré en tant que serveur RADIUS, la stratégie de demande de connexion par défaut est la seule stratégie configurée et toutes les demandes de connexion sont traitées par le serveur NPS local. Le serveur NPS peut authentifier et autoriser les utilisateurs dont les comptes se trouvent dans le domaine du serveur NPS et dans des domaines approuvés.

**NPS en tant que proxy RADIUS**. Dans cet exemple, le serveur NPS est configuré en tant que proxy RADIUS qui transfère les demandes de connexion à des groupes de serveurs RADIUS distants dans deux domaines non approuvés. La stratégie de demande de connexion par défaut est supprimée et deux nouvelles stratégies de demande de connexion sont créées pour transférer les demandes à chacun des deux domaines non approuvés. Dans cet exemple, NPS ne traite pas les demandes de connexion sur le serveur local. 

**NPS en tant que serveur RADIUS et proxy RADIUS**. En plus de la stratégie de demande de connexion par défaut, qui indique que les demandes de connexion sont traitées localement, une nouvelle stratégie de demande de connexion est créée, qui transfère les demandes de connexion à un serveur NPS ou à un autre serveur RADIUS dans un domaine non approuvé. Cette deuxième stratégie est nommée Stratégie de proxy. Dans cet exemple, la stratégie de proxy apparaît en premier dans la liste ordonnée des stratégies. Si la demande de connexion correspond à la stratégie de proxy, la demande de connexion est transférée au serveur RADIUS dans le groupe de serveurs RADIUS distants. Si la demande de connexion ne correspond pas à la stratégie de proxy, mais qu’elle correspond à la stratégie de demande de connexion par défaut, NPS traite la demande de connexion sur le serveur local. Si la demande de connexion ne correspond à aucune stratégie, elle est ignorée.

**NPS en tant que serveur RADIUS avec des serveurs de comptabilité à distance**. Dans cet exemple, le serveur NPS local n’est pas configuré pour effectuer la gestion des comptes et la stratégie de demande de connexion par défaut est révisée afin que les messages de gestion des comptes RADIUS soient transférés vers un serveur NPS ou un autre serveur RADIUS dans un groupe de serveurs RADIUS distants. Bien que les messages de gestion des comptes soient transférés, les messages d’authentification et d’autorisation ne sont pas transférés, et le serveur NPS local effectue ces fonctions pour le domaine local et tous les domaines approuvés.

**Mappage de serveur NPS avec RADIUS distant vers utilisateur Windows**. Dans cet exemple, NPS agit à la fois comme serveur RADIUS et proxy RADIUS pour chaque demande de connexion en transférant la demande d’authentification à un serveur RADIUS distant lors de l’utilisation d’un compte d’utilisateur Windows local pour l’autorisation. Cette configuration est implémentée en configurant l’attribut de mappage de l’utilisateur RADIUS distant sur Windows en tant que condition de la stratégie de demande de connexion. \(In, un compte d’utilisateur doit être créé localement sur le serveur RADIUS qui porte le même nom que le compte d’utilisateur distant sur lequel l’authentification est effectuée par le serveur RADIUS distant. \)

## <a name="configuration"></a>Configuration

Pour configurer NPS en tant que serveur RADIUS, vous pouvez utiliser la configuration standard ou la configuration avancée dans la console NPS ou dans Gestionnaire de serveur. Pour configurer NPS en tant que proxy RADIUS, vous devez utiliser la configuration avancée.

### <a name="standard-configuration"></a>Configuration standard

Avec la configuration standard, des assistants sont fournis pour vous aider à configurer NPS pour les scénarios suivants :

- Serveur RADIUS pour les connexions d’accès à distance ou VPN
- Serveur RADIUS pour les connexions sans fil 802.1X ou câblées

Pour configurer NPS à l’aide d’un Assistant, ouvrez la console NPS, sélectionnez l’un des scénarios précédents, puis cliquez sur le lien qui ouvre l’Assistant.

### <a name="advanced-configuration"></a>Configuration avancée

Lorsque vous utilisez la configuration avancée, vous configurez manuellement NPS en tant que serveur RADIUS ou proxy RADIUS. 

Pour configurer NPS à l’aide de la configuration avancée, ouvrez la console NPS, puis cliquez sur la flèche en regard de **Configuration avancée** pour développer cette section.

Les éléments de configuration avancés suivants sont fournis.

#### <a name="configure-radius-server"></a>Configurer le serveur RADIUS

Pour configurer NPS en tant que serveur RADIUS, vous devez configurer les clients RADIUS, la stratégie réseau et la gestion de comptes RADIUS.

Pour obtenir des instructions sur la réalisation de ces configurations, consultez les rubriques suivantes.

- [Configurer des clients RADIUS](nps-radius-clients-configure.md)
- [Configurer des stratégies réseau](nps-np-configure.md)
- [Configurer la gestion des comptes NPS (Network Policy Server)](nps-accounting-configure.md)

#### <a name="configure-radius-proxy"></a>Configurer le proxy RADIUS

Pour configurer NPS en tant que proxy RADIUS, vous devez configurer les clients RADIUS, les groupes de serveurs RADIUS distants et les stratégies de demande de connexion.

Pour obtenir des instructions sur la réalisation de ces configurations, consultez les rubriques suivantes.

- [Configurer des clients RADIUS](nps-radius-clients-configure.md)
- [Configurer des groupes de serveurs RADIUS distants](nps-crp-rrsg-configure.md)
- [Configurer des stratégies de demande de connexion](nps-crp-configure.md)

## <a name="nps-logging"></a>Journalisation NPS

La journalisation NPS est également appelée gestion de compte RADIUS. Configurez la journalisation NPS selon vos besoins, qu’il s’agisse d’un serveur RADIUS, d’un proxy ou d’une combinaison de ces configurations.

Pour configurer la journalisation NPS, vous devez configurer les événements que vous souhaitez consigner et afficher avec observateur d’événements, puis déterminer quelles autres informations vous souhaitez consigner. En outre, vous devez décider si vous souhaitez consigner les informations d’authentification utilisateur et de gestion des comptes dans les fichiers journaux stockés sur l’ordinateur local ou dans une base de données de SQL Server sur l’ordinateur local ou sur un ordinateur distant.

Pour plus d’informations, consultez [configurer la gestion des stratégies de serveur](nps-accounting-configure.md)NPS.
