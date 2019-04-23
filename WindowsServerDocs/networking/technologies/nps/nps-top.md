---
title: Serveur NPS (Network Policy Server)
description: Cette rubrique fournit une vue d’ensemble du serveur de stratégie réseau dans Windows Server 2016 et Windows Server 2019 et inclut des liens vers des conseils supplémentaires sur le serveur NPS.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 9c7a67e0-0953-479c-8736-ccb356230bde
ms.author: pashort
author: shortpatti
ms.date: 06/20/2018
ms.openlocfilehash: c76483031bdca184e0943738a8c921776440d1fc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829030"
---
# <a name="network-policy-server-nps"></a>Serveur NPS (Network Policy Server)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2019

Vous pouvez utiliser cette rubrique pour une vue d’ensemble du serveur de stratégie réseau dans Windows Server 2016 et Windows Server 2019. NPS est installé lorsque vous installez la stratégie de réseau et la fonctionnalité des Services d’accès (NPAS) dans Windows Server 2016 et Server 2019.

>[!NOTE]
>Outre cette rubrique, la documentation suivante sur le serveur NPS est disponible.
> - [Network Policy Server Best Practices](nps-best-practices.md)
> - [Mise en route avec le serveur NPS](nps-getstart-top.md)
> - [Planifier le serveur NPS](nps-plan-top.md)
> - [Déployer le serveur de stratégie réseau](nps-deploy.md)
> - [Gérer le serveur de stratégie réseau](nps-manage-top.md)
> - [Applets de commande stratégie Server (NPS) dans Windows PowerShell réseau](https://technet.microsoft.com/library/jj872739.aspx) pour Windows Server 2016 et Windows 10
> - [Applets de commande stratégie Server (NPS) dans Windows PowerShell réseau](https://technet.microsoft.com/library/jj872739.aspx) pour Windows Server 2012 R2 et Windows 8.1
> - [Applets de commande de serveur NPS dans Windows PowerShell](https://technet.microsoft.com/library/jj872739.aspx) pour Windows Server 2012 et Windows 8

Un serveur NPS \(Network Policy Server\) vous permet de créer et d’appliquer des stratégies d’accès réseau valides pour toute l’organisation, qui régissent les demandes d’authentification et l’autorisation des demandes de connexion.

Vous pouvez également configurer NPS en tant que l’authentification Dial-In Service RADIUS (Remote User) proxy pour transférer les demandes de connexion à un serveur NPS à distance ou un autre serveur RADIUS afin que vous puissiez équilibrer les demandes de connexion et les transférer vers le domaine approprié pour l’authentification et l’autorisation.

NPS vous permet de configurer et gérer l’authentification d’accès réseau, l’autorisation et comptabilité avec les fonctionnalités suivantes de manière centralisée :

- **Serveur RADIUS**. NPS effectue une authentification centralisée, l’autorisation et comptabilité sans fil, l’authentification de commutateur, les accès à distance et les connexions de réseau privé virtuel (VPN). Lorsque vous utilisez NPS en tant que serveur RADIUS, vous devez configurer les serveurs d’accès réseau, tels que les point d’accès sans fil et les serveurs VPN, en tant que clients RADIUS dans NPS. Vous devez également configurer les stratégies réseau utilisées par NPS pour autoriser les demandes de connexion. Vous pouvez éventuellement configurer la gestion de comptes RADIUS de sorte que NPS enregistre les informations de gestion de comptes dans des fichiers journaux sur le disque dur local ou dans une base de données Microsoft SQL Server. Pour plus d’informations, consultez [serveur RADIUS](#bkmk_server).
- **Proxy RADIUS**. Lorsque vous utilisez NPS en tant que proxy RADIUS, vous configurez des stratégies de demande de connexion qui indiquent au serveur NPS les demandes de connexion pour transférer à d’autres serveurs RADIUS et les serveurs RADIUS que vous souhaitez transférer les demandes de connexion. Vous pouvez éventuellement configurer NPS pour le transfert des données de gestion de comptes à enregistrer par un ou plusieurs ordinateurs d’un groupe de serveurs RADIUS distants. Pour configurer NPS comme serveur proxy RADIUS, consultez les rubriques suivantes. Pour plus d’informations, consultez [proxy RADIUS](#bkmk_proxy).
    - [Configurer des stratégies de demande de connexion](nps-crp-configure.md)
- **Gestion des comptes RADIUS**. Vous pouvez configurer NPS pour consigner les événements dans un fichier journal local ou à une instance locale ou distante de Microsoft SQL Server. Pour plus d’informations, consultez [journalisation NPS](#bkmk_logging).

>[!IMPORTANT]
>Protection d’accès réseau \(NAP\), Health Registration Authority \(HRA\)et Host Credential Authorization Protocol \(HCAP\) ont été dépréciées dans Windows Server 2012 R2 et ne sont pas disponibles dans Windows Server 2016. Si vous avez un déploiement de NAP à l’aide de systèmes d’exploitation antérieurs à Windows Server 2016, vous ne pouvez pas migrer votre déploiement NAP vers Windows Server 2016.

Vous pouvez configurer NPS avec n’importe quelle combinaison de ces fonctionnalités. Par exemple, vous pouvez configurer un serveur NPS comme serveur RADIUS pour les connexions VPN et également en tant que proxy RADIUS pour transférer des demandes de connexion aux membres du groupe de serveurs RADIUS distants pour l’authentification et autorisation dans un autre domaine.

## <a name="windows-server-editions-and-nps"></a>NPS et les éditions de Windows Server

NPS fournit une fonctionnalité différente selon l’édition de Windows Server que vous installez.

### <a name="windows-server-2016-or-windows-server-2019-standarddatacenter-edition"></a>Windows Server 2016 ou Windows Server 2019 Standard/Datacenter Edition

Avec le serveur NPS dans Windows Server 2016 Standard ou Datacenter, vous pouvez configurer un nombre illimité de clients RADIUS et les groupes de serveurs RADIUS distants. En outre, vous pouvez configurer des clients RADIUS en spécifiant une plage d'adresses IP.

>[!NOTE]
>La fonctionnalité de stratégie de réseau WIndows Services et d’accès n’est pas disponible sur les systèmes installés avec une option d’installation Server Core.

Les sections suivantes fournissent des informations plus détaillées sur le serveur NPS comme serveur RADIUS et proxy.

## <a name="radius-server-and-proxy"></a>Serveur RADIUS et proxy

Vous pouvez utiliser NPS en tant que serveur RADIUS, proxy RADIUS ou les deux.

### <a name="bkmk_server"></a>Serveur RADIUS

NPS est l’implémentation Microsoft de la norme RADIUS spécifiée par l’Internet Engineering Task Force \(IETF\) dans les RFC 2865 et 2866. Comme un serveur RADIUS, NPS effectue l’authentification de connexion centralisée, de d’autorisation et de comptabilité pour nombreux types d’accès réseau, notamment le commutateur sans fil, authentification, accès à distance et de réseau privé virtuel \(VPN\) à distance accès et les connexions de routeur à routeur.

>[!NOTE]
>Pour plus d’informations sur le déploiement de NPS comme serveur RADIUS, consultez [déployer un serveur de stratégie réseau](nps-deploy.md).

NPS permet d’utiliser un ensemble hétérogène de sans fil, commutateur, accès à distance ou les appareils VPN. Vous pouvez utiliser le serveur NPS avec le service d’accès à distance, qui est disponible dans Windows Server 2016.

NPS utilise un Active Directory Domain Services \(AD DS\) la base de données de domaine ou les comptes d’utilisateur de gestionnaire de comptes de sécurité (SAM) local pour authentifier les informations d’identification de l’utilisateur pour les tentatives de connexion. Lorsqu’un serveur exécutant NPS est un membre d’un domaine AD DS, NPS utilise le service d’annuaire en tant que sa base de données de compte d’utilisateur et fait partie d’une solution d’authentification unique. Le même jeu d’informations d’identification est utilisé pour le contrôle d’accès réseau \(authentifier et autoriser l’accès à un réseau\) et se connecter à un domaine AD DS.

>[!NOTE]
>NPS utilise les propriétés d’accès à distance des stratégies de réseau et de compte d’utilisateur à autoriser une connexion.

Fournisseurs de services Internet \(ISP\) et les organisations qui assurent un accès réseau ont le défi accru de la gestion de tous les types d’accès réseau à partir d’un point unique d’administration, quel que soit le type d’accès réseau matériel. La norme RADIUS prend en charge cette fonctionnalité dans les environnements homogènes ou hétérogènes. RADIUS est un protocole client-serveur qui permet de dispositifs d’accès réseau (utilisé en tant que clients RADIUS) pour envoyer les demandes à un serveur RADIUS.

Un serveur RADIUS a accès aux informations de compte d’utilisateur et peut vérifier les informations d’identification de l’authentification de l’accès réseau. Si les informations d’identification de l’utilisateur sont authentifiées et la tentative de connexion est autorisée, le serveur RADIUS autorise l’accès utilisateur en fonction des conditions spécifiées, puis enregistre la connexion d’accès réseau dans un journal de gestion. L’utilisation de RADIUS permet l’authentification utilisateur de l’accès réseau, l’autorisation et les données de comptabilité pour être collectées et conservées dans un emplacement central, plutôt que sur chaque serveur d’accès.

#### <a name="using-nps-as-a-radius-server"></a>À l’aide du serveur NPS comme serveur RADIUS

Vous pouvez utiliser le serveur NPS comme serveur RADIUS lorsque :

- Vous utilisez un domaine AD DS ou la base de données de comptes d’utilisateur SAM local en tant que votre base de données de compte d’utilisateur pour les clients d’accès. 
- Vous utilisez l’accès à distance sur plusieurs serveurs d’accès à distance, les serveurs VPN, ou les routeurs d’accès à la demande et que vous souhaitez centraliser la configuration des stratégies de réseau et de journalisation et de gestion des comptes de connexion. 
- Vous externalisez votre accès à distance, VPN ou l’accès sans fil à un fournisseur de services. Les serveurs d’accès utilisent RADIUS pour authentifier et autoriser les connexions établies par les membres de votre organisation.
- Vous souhaitez centraliser l’authentification, autorisation et comptabilité pour un ensemble hétérogène de serveurs d’accès.

L’illustration suivante montre le serveur NPS comme serveur RADIUS pour un large éventail de clients d’accès. 

![NPS comme serveur RADIUS](../../media/Nps-Server/Nps-Server.jpg)

### <a name="bkmk_proxy"></a>Proxy RADIUS

En tant que proxy RADIUS, NPS transfère les messages d’authentification et gestion des comptes NPS et les autres serveurs RADIUS. Vous pouvez utiliser le serveur NPS comme proxy RADIUS pour assurer le routage du rayon de messages échangés entre les clients RADIUS \(également appelés serveurs d’accès réseau\) et serveurs RADIUS qui effectuent l’authentification utilisateur, l’autorisation et comptabilité pour le tentative de connexion. 

Lorsqu’il est utilisé en tant que proxy RADIUS, NPS est un changement centrale ou un point de routage via le flux de messages RADIUS accès et de gestion. NPS enregistre des informations dans un journal de gestion sur les messages qui sont transférés.

#### <a name="using-nps-as-a-radius-proxy"></a>À l’aide de NPS en tant que proxy RADIUS

Vous pouvez utiliser NPS en tant que proxy RADIUS lorsque :

- Vous êtes un fournisseur de services qui offre l’accès à distance externalisé, VPN ou services d’accès réseau sans fil à plusieurs clients. Votre NAS envoient des demandes de connexion au proxy RADIUS du serveur NPS. En fonction de la partie domaine du nom d’utilisateur dans la demande de connexion, le proxy RADIUS NPS transfère la demande de connexion à un serveur RADIUS qui est maintenu par le client et peut authentifier et autoriser la tentative de connexion.
- Vous souhaitez fournir l’authentification et autorisation pour les comptes d’utilisateur qui ne sont pas membres du domaine dans lequel le serveur NPS est un membre ou un autre domaine ayant une approbation bidirectionnelle avec le domaine dans lequel le serveur NPS est membre. Cela inclut les comptes dans des domaines non approuvés, des domaines à approbation unidirectionnelle et des autres forêts. Au lieu de configurer vos serveurs d’accès pour envoyer leurs demandes de connexion à un serveur NPS RADIUS, vous pouvez les configurer pour envoyer leurs demandes de connexion à un proxy RADIUS du serveur NPS. Le proxy RADIUS NPS utilise la partie du nom de domaine du nom d’utilisateur et transfère la demande à un serveur NPS dans la forêt ou le domaine correct. Tentatives de connexion pour l’utilisateur de comptes dans un domaine ou forêt peuvent être authentifiés pour NAS dans un autre domaine ou forêt.
- Vous souhaitez effectuer l’authentification et l’autorisation à l’aide d’une base de données qui n’est pas une base de données du compte Windows. Dans ce cas, les demandes de connexion qui correspondent à un nom de domaine Kerberos spécifié sont transférés à un serveur RADIUS, qui a accès à une autre base de données de comptes d’utilisateur et les données d’autorisation. Exemples de bases de données utilisateur Services NDS (Novell Directory) et le langage SQL (Structured Query) des bases de données.
- Vous souhaitez traiter un grand nombre de demandes de connexion. Dans ce cas, au lieu de configurer vos clients RADIUS pour tenter d’équilibrer leur connexion demandes et gestion sur plusieurs serveurs RADIUS, vous pouvez les configurer pour envoyer leurs demandes de comptabilité et de connexion à un proxy RADIUS du serveur NPS. Le proxy RADIUS NPS équilibre dynamiquement la charge de connexion et de comptabilité demande sur plusieurs serveurs RADIUS et augmente le traitement d’un grand nombre de clients RADIUS et les authentifications par seconde.
- Vous souhaitez fournir l’authentification RADIUS et une autorisation pour les fournisseurs de services externalisés et minimiser la configuration de pare-feu d’intranet. Un pare-feu intranet est entre votre réseau de périmètre (le réseau entre votre intranet et Internet) et l’intranet. En plaçant un serveur NPS sur votre réseau de périmètre, le pare-feu entre votre réseau de périmètre et l’intranet doit autoriser le trafic entre le serveur NPS et plusieurs contrôleurs de domaine. En remplaçant le serveur NPS par un proxy de serveur NPS, le pare-feu doit autoriser uniquement le trafic RADIUS entre le proxy de serveur NPS et un ou plusieurs NPSs au sein de votre intranet.

L’illustration suivante montre NPS en tant que proxy RADIUS entre les clients RADIUS et serveurs RADIUS.

![NPS en tant que Proxy RADIUS](../../media/Nps-Proxy/Nps-Proxy.jpg)


Avec le serveur NPS, les organisations peuvent également sous-traiter infrastructure d’accès à distance à un fournisseur de services tout en contrôlant l’authentification utilisateur, l’autorisation et comptabilité.

Configurations de serveur NPS peuvent être créées pour les scénarios suivants :

- Accès sans fil
- Accès à distance d’organisation distance ou privé de réseau virtuel (VPN)
- Accès à distance externalisé ou accès sans fil
- Accès Internet
- Accès authentifié aux ressources extranets pour les partenaires commerciaux

## <a name="radius-server-and-radius-proxy-configuration-examples"></a>Serveur RADIUS et des exemples de configuration de proxy RADIUS

Les exemples de configuration suivants montrent comment vous pouvez configurer NPS comme serveur RADIUS et proxy RADIUS.

**NPS comme serveur RADIUS**. Dans cet exemple, NPS est configuré comme un serveur RADIUS, la stratégie de demande de connexion par défaut est la seule stratégie configurée et toutes les demandes de connexion sont traitées par le serveur NPS local. Le serveur NPS peut authentifier et autoriser les utilisateurs dont les comptes sont dans le domaine du serveur NPS et dans des domaines approuvés.

**NPS en tant que proxy RADIUS**. Dans cet exemple, le serveur NPS est configuré en tant que proxy RADIUS qui transfère les demandes de connexion à des groupes de serveurs RADIUS distants dans deux domaines non approuvés. La stratégie de demande de connexion par défaut est supprimée, et deux nouvelles stratégies de demande de connexion sont créés pour transférer les demandes à chacun des deux domaines non approuvés. Dans cet exemple, NPS ne traite pas les demandes de connexion sur le serveur local. 

**NPS en tant que serveur RADIUS et proxy RADIUS**. En plus de la stratégie de demande de connexion par défaut, ce qui indique que les demandes de connexion sont traitées localement, une nouvelle stratégie de demande de connexion est créée qui transfère les demandes de connexion à un serveur NPS ou un autre serveur RADIUS dans un domaine non approuvé. Cette deuxième stratégie est nommée de la stratégie de Proxy. Dans cet exemple, la stratégie Proxy apparaît en premier dans la liste ordonnée des stratégies. Si la demande de connexion correspond à la stratégie de Proxy, la demande de connexion est transférée au serveur RADIUS dans le groupe de serveurs RADIUS distants. Si la demande de connexion ne correspond pas à la stratégie de Proxy, mais ne correspond pas à la stratégie de demande de connexion par défaut, le serveur NPS traite la demande de connexion sur le serveur local. Si la demande de connexion ne correspond pas à une stratégie, elle est ignorée.

**NPS comme serveur RADIUS avec des serveurs distants de comptabilité**. Dans cet exemple, le serveur NPS local n’est pas configuré pour effectuer la gestion des comptes et la stratégie de demande de connexion par défaut est révisée afin que les messages de gestion des comptes RADIUS sont transférés vers un serveur NPS ou un autre serveur RADIUS dans un groupe de serveurs RADIUS distants. Bien que les messages de comptabilité sont transférés, messages d’authentification et d’autorisation ne sont pas transférés et le serveur NPS local exécute ces fonctions pour le domaine local et tous les domaines approuvés.

**NPS avec RADIUS à distance au mappage d’utilisateur Windows**. Dans cet exemple, NPS sert à la fois un serveur RADIUS et proxy RADIUS pour chaque demande de connexion en transférant la demande d’authentification à un serveur RADIUS à distance lors de l’utilisation d’un compte d’utilisateur Windows local pour l’autorisation. Cette configuration est implémentée en configurant le rayon à distance à l’attribut de mappage de l’utilisateur Windows en tant que condition de la stratégie de demande de connexion. \(En outre, un compte d’utilisateur doit être créé localement sur le serveur RADIUS qui a le même nom que le compte d’utilisateur distant sur lequel l’authentification est effectuée par le serveur RADIUS à distance.\)

## <a name="configuration"></a>Configuration

Pour configurer NPS comme serveur RADIUS, vous pouvez utiliser une configuration standard ou configuration avancée dans la console NPS ou dans le Gestionnaire de serveur. Pour configurer NPS en tant que proxy RADIUS, vous devez utiliser la configuration avancée.

### <a name="standard-configuration"></a>Configuration standard

Avec une configuration standard, les Assistants sont fournis pour vous aider à configurer NPS pour les scénarios suivants :

- Serveur RADIUS pour l’accès à distance ou les connexions VPN
- Serveur RADIUS pour les connexions sans fil 802.1X ou câblées

Pour configurer NPS à l’aide d’un Assistant, ouvrez la console NPS, sélectionnez un des scénarios précédents, puis cliquez sur le lien qui ouvre l’Assistant.

### <a name="advanced-configuration"></a>Configuration avancée

Lorsque vous utilisez la configuration avancée, vous configurez manuellement NPS en tant que serveur RADIUS ou proxy RADIUS. 

Pour configurer NPS à l’aide de configuration avancée, ouvrez la console NPS, puis cliquez sur la flèche à côté **Configuration avancée** pour développer cette section.

Éléments de configuration avancés suivants sont fournis.

#### <a name="configure-radius-server"></a>Configurer un serveur RADIUS

Pour configurer NPS comme serveur RADIUS, vous devez configurer les clients RADIUS, la stratégie de réseau et gestion des comptes RADIUS.

Pour obtenir des instructions sur la création de ces configurations, consultez les rubriques suivantes.

- [Configurer les Clients RADIUS](nps-radius-clients-configure.md)
- [Configurer des stratégies de réseau](nps-np-configure.md)
- [Configurer la gestion de serveur de stratégie réseau](nps-accounting-configure.md)

#### <a name="configure-radius-proxy"></a>Configurer le proxy RADIUS

Pour configurer NPS en tant que proxy RADIUS, vous devez configurer les clients RADIUS, groupes de serveurs RADIUS distants et les stratégies de demande de connexion.

Pour obtenir des instructions sur la création de ces configurations, consultez les rubriques suivantes.

- [Configurer les Clients RADIUS](nps-radius-clients-configure.md)
- [Configurer des groupes de serveurs RADIUS distants](nps-crp-rrsg-configure.md)
- [Configurer des stratégies de demande de connexion](nps-crp-configure.md)

## <a name="bkmk_logging"></a>Journalisation du serveur NPS

La journalisation NPS est également appelée gestion des comptes RADIUS. Configurer la journalisation de serveur NPS à vos besoins si NPS est utilisé comme un serveur RADIUS, proxy ou n’importe quelle combinaison de ces configurations.

Pour configurer la journalisation de serveur NPS, vous devez configurer les événements de votre choix enregistrés et consultés à l’Observateur d’événements, puis déterminer quelles autres informations que vous souhaitez journaliser. En outre, vous devez décider si vous souhaitez ouvrir une session d’authentification utilisateur et les informations de gestion pour les fichiers de journaux texte stockés sur l’ordinateur local ou à une base de données SQL Server sur l’ordinateur local ou sur un ordinateur distant.

Pour plus d’informations, consultez [configurer réseau stratégie serveur comptabilité](nps-accounting-configure.md).
