---
title: Serveur de stratégie réseau (NPS)
description: Cette rubrique fournit une vue d’ensemble du serveur de stratégie réseau dans Windows Server2016 et inclut des liens vers des informations supplémentaires sur le serveur NPS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 9c7a67e0-0953-479c-8736-ccb356230bde
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8f48cbdaec82e9e60f734a4a8949082187b13166
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="network-policy-server-nps"></a>Serveur de stratégie réseau (NPS)

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour une vue d’ensemble du serveur de stratégie réseau dans Windows Server2016.

>[!NOTE]
>Outre cette rubrique, la documentation suivante sur le serveur NPS est disponible.
> - [Serveur de stratégie réseau meilleures pratiques](nps-best-practices.md)
> - [Prise en main de serveur de stratégie réseau](nps-getstart-top.md)
> - [Planifier le serveur de stratégie réseau](nps-plan-top.md)
> - [Déployer le serveur de stratégie réseau](nps-deploy.md)
> - [Gérer le serveur de stratégie réseau](nps-manage-top.md)
> - [Applets de commande de serveur (NPS) stratégie réseau](https://technet.microsoft.com/library/jj872739.aspx) pour Windows Server2016 et Windows10
> - [Applets de commande stratégie serveur (NPS) dans Windows PowerShell réseau](https://technet.microsoft.com/library/jj872739.aspx) pour Windows Server2012R2 et Windows8.1
> - [Applets de commande dans WindowsPowerShellNPS](https://technet.microsoft.com/library/jj872739.aspx) pour Windows Server2012 et Windows8

Serveur NPS (Network Policy) vous permet de créer et appliquer des stratégies d’accès réseau de l’entreprise pour l’authentification de demande de connexion et d’autorisation.

Vous pouvez également configurer NPS en tant que proxy Authentication Dial-In utilisateur RADIUS (Remote Service) pour transférer les demandes de connexion à un serveur NPS à distance ou un autre serveur RADIUS afin que vous pouvez charger équilibrer les demandes de connexion et les transférer vers le domaine approprié pour l’authentification et d’autorisation.

NPS vous permet de configurer et gérer l’authentification d’accès réseau, l’autorisation et la gestion des comptes avec les fonctionnalités suivantes de manière centralisée:

- **Serveur RADIUS**. NPS effectue une authentification centralisée, l’autorisation et gestion des comptes sans fil, l’authentification commutateur, accès à distance et des connexions de réseau privé virtuel (VPN). Lorsque vous utilisez NPS comme serveur RADIUS, vous configurez les serveurs d’accès réseau, tels que les points d’accès sans fil et les serveurs VPN, en tant que clients RADIUS dans NPS. Vous configurez également des stratégies de réseau que NPS utilise pour autoriser les demandes de connexion, et vous pouvez configurer la gestion de comptes RADIUS afin que NPS enregistre les informations de gestion des fichiers journaux sur le disque dur local ou dans une base de données MicrosoftSQLServer. Pour plus d’informations, voir [serveur RADIUS](#bkmk_server).
- **Proxy RADIUS**. Lorsque vous utilisez NPS en tant que proxy RADIUS, vous configurez des stratégies de demande de connexion qui indiquent au serveur NPS les demandes de connexion à transmettre à d’autres serveurs RADIUS et les serveurs RADIUS auxquels vous souhaitez transférer les demandes de connexion. Vous pouvez également configurer NPS pour transférer des données de gestion des comptes à enregistrer par un ou plusieurs ordinateurs dans un groupe de serveurs RADIUS distants. Pour configurer NPS comme serveur proxy RADIUS, consultez les rubriques suivantes. Pour plus d’informations, voir [proxy RADIUS](#bkmk_proxy).
    - [Configurer des stratégies de demande de connexion](nps-crp-configure.md)
- **Gestion de comptes RADIUS**. Vous pouvez configurer NPS pour consigner les événements dans un fichier journal local ou à une instance locale ou distante de MicrosoftSQLServer. Pour plus d’informations, voir [journalisation NPS](#bkmk_logging).

>[!IMPORTANT]
>Réseau la Protection d’accès \(NAP\), l’autorité HRA \(HRA\), \(HCAP\) protocole de l’autorisation d’informations d’identification ont été déconseillées dans Windows Server2012R2 et ne sont pas disponibles dans Windows Server2016. Si vous avez un déploiement de protection d’accès réseau à l’aide de systèmes d’exploitation antérieurs à Windows Server2016, vous ne pouvez pas migrer votre déploiement NAP à Windows Server2016.

Vous pouvez configurer NPS avec n’importe quelle combinaison de ces fonctionnalités. Par exemple, vous pouvez configurer un serveur NPS comme serveur RADIUS pour les connexions VPN et également en tant que proxy RADIUS pour transférer des demandes de connexion aux membres du groupe de serveurs RADIUS distants pour l’authentification et d’autorisation dans un autre domaine.

## <a name="windows-server-editions-and-nps"></a>NPS et les éditions de Windows Server

NPS offre des fonctionnalités différentes en fonction de l’édition de Windows Server que vous installez.

### <a name="windows-server-2016-datacenter-edition"></a>Windows Server2016 Datacenter Edition

Avec le serveur NPS dans Windows Server2016 Datacenter, vous pouvez configurer un nombre illimité de clients RADIUS et les groupes de serveurs RADIUS distants. En outre, vous pouvez configurer des clients RADIUS en spécifiant une plage d’adresses IP.

### <a name="windows-server-2016-standard-edition"></a>Windows Server2016 Standard Edition

Avec le serveur NPS dans Windows Server2016 Standard, vous pouvez configurer un maximum de 50clients RADIUS et un maximum de 2groupes de serveurs RADIUS distants. Vous pouvez définir un client RADIUS à l’aide d’un nom de domaine complet ou une adresse IP, mais vous ne pouvez pas définir des groupes de clients RADIUS en spécifiant une plage d’adresses IP. Si le nom de domaine complet d’un client RADIUS correspond à plusieurs adresses IP, le serveur NPS utilise la première adresse IP renvoyée dans la requête de nom de domaine (DNS).

Les sections suivantes fournissent des informations plus détaillées sur le serveur NPS comme serveur RADIUS et proxy.

## <a name="radius-server-and-proxy"></a>Proxy et serveur RADIUS

Vous pouvez utiliser NPS en tant que serveur RADIUS, proxy RADIUS ou les deux.

### <a name="bkmk_server"></a>Serveur RADIUS

NPS est l’implémentation Microsoft de la norme RADIUS spécifiée par la Internet Engineering Task Force \(IETF\) dans les RFC2865 et 2866. Comme un serveur RADIUS, NPS effectue l’authentification de connexion centralisée, l’autorisation et gestion des comptes pour de nombreux types d’accès réseau, y compris connexions routeur à routeur sans fil, l’authentification du commutateur, l’accès à distance et accès à distance \(VPN\) de réseau privé virtuel.

>[!NOTE]
>Pour plus d’informations sur le déploiement de NPS comme serveur RADIUS, consultez [déployer un serveur de stratégie réseau](nps-deploy.md).

NPS permet d’utiliser un ensemble hétérogène sans fil, commutateur, l’accès à distance ou équipement VPN. Vous pouvez utiliser le serveur NPS avec le service d’accès à distance, qui est disponible dans Windows Server2016.

NPS utilise un domaine \(ADDS\) de Services de domaine ActiveDirectory ou tente du Gestionnaire de comptes de sécurité (SAM) utilisateur comptes base de données locale pour authentifier les informations d’identification utilisateur pour la connexion. Lorsqu’un serveur exécutant NPS est membre d’un domaine ADDS, NPS utilise le service d’annuaire que sa base de données du compte utilisateur et fait partie d’une solution d’authentification unique. Le même jeu d’informations d’identification est utilisé pour le contrôle d’accès réseau \ (authentifier et autoriser l’accès à un Updatable) et ouvrir une session sur un domaine ADDS.

>[!NOTE]
>NPS utilise les propriétés de numérotation des stratégies réseau et le compte d’utilisateur pour autoriser la connexion.

Internet service fournisseurs \(ISPs\) et les organisations qui gèrent l’accès réseau ont la demande accrue de la gestion de tous les types d’accès réseau à partir d’un point unique d’administration, quel que soit le type d’équipement d’accès réseau utilisé. La norme RADIUS prend en charge cette fonctionnalité dans des environnements hétérogènes et homogènes. RADIUS est un protocole client-serveur qui permet de dispositifs d’accès réseau (utilisé en tant que clients RADIUS) pour envoyer des demandes d’authentification et gestion des comptes à un serveur RADIUS.

Un serveur RADIUS a accès aux informations de compte d’utilisateur et peut vérifier les informations d’identification de l’authentification de l’accès réseau. Si les informations d’identification de l’utilisateur sont authentifiées et la tentative de connexion est autorisée, le serveur RADIUS autorise l’accès utilisateur en fonction des conditions spécifiées, puis enregistre la connexion d’accès réseau dans un journal de gestion. L’utilisation de RADIUS permet l’authentification utilisateur d’accès réseau, d’autorisation, les données de gestion à être recueillies et conservées dans un emplacement central, plutôt que sur chaque serveur d’accès.

#### <a name="using-nps-as-a-radius-server"></a>À l’aide du serveur NPS comme serveur RADIUS

Vous pouvez utiliser le serveur NPS comme serveur RADIUS lorsque:

- Vous utilisez un domaine ADDS ou local la base de données SAM utilisateur comptes en tant que votre base de données de compte d’utilisateur pour les clients d’accès. 
- Vous utilisez l’accès à distance sur plusieurs serveurs d’accès à distance, les serveurs VPN, ou les routeurs d’accès à la demande et que vous souhaitez centraliser la configuration des stratégies de réseau et de journalisation et de gestion des comptes de connexion. 
- Vous êtes externalisation votre accès à distance, VPN ou l’accès sans fil à un fournisseur de services. Les serveurs d’accès utilisent RADIUS pour authentifier et autoriser les connexions établies par les membres de votre organisation.
- Vous souhaitez centraliser l’authentification, l’autorisation et la gestion des comptes pour un ensemble de serveurs d’accès hétérogène.

L’illustration suivante montre NPS comme serveur RADIUS pour un large éventail de clients d’accès. 

![NPS comme serveur RADIUS](../../media/Nps-Server/Nps-Server.jpg)

### <a name="bkmk_proxy"></a>Proxy RADIUS

En tant que proxy RADIUS, NPS transfère les messages d’authentification et gestion des comptes NPS et d’autres serveurs RADIUS. Vous pouvez utiliser le serveur NPS comme proxy RADIUS, afin de fournir le routage du rayon de messages entre les clients RADIUS \ (également appelé servers\ d’accès réseau) et les serveurs RADIUS qui effectuent l’authentification des utilisateurs, l’autorisation et gestion des comptes pour la tentative de connexion. 

Lorsqu’il est utilisé en tant que proxy RADIUS, NPS est un changement centrale ou un point de routage via lequel transitent les messages comptabilité et d’accès RADIUS. NPS enregistre des informations dans un journal de gestion sur les messages qui sont transférés.

#### <a name="using-nps-as-a-radius-proxy"></a>À l’aide de NPS en tant que proxy RADIUS

Vous pouvez utiliser NPS en tant que proxy RADIUS lorsque:

- Vous êtes un fournisseur de services qui propose des services d’accès réseau sans fil, VPN ou accès à distance externalisé à plusieurs clients. Votre NAS envoient des demandes de connexion au proxy RADIUS du serveur NPS. En fonction de la partie domaine du nom d’utilisateur dans la demande de connexion, le proxy RADIUS NPS transfère la demande de connexion à un serveur RADIUS qui est gérée par le client et peut authentifier et autoriser la tentative de connexion.
- Vous voulez fournir l’authentification et autorisation pour les comptes d’utilisateurs qui ne sont pas membres du domaine dans lequel le serveur NPS est un membre ou un autre domaine qui dispose d’une approbation bidirectionnelle avec le domaine dans lequel le serveur NPS est membre. Cela inclut les comptes dans des domaines non approuvés, les domaines approuvés à sens unique et d’autres forêts. Au lieu de configurer vos serveurs d’accès pour envoyer leurs demandes de connexion à un serveur NPS RADIUS, vous pouvez les configurer pour envoyer leurs demandes de connexion à un proxy RADIUS du serveur NPS. Le proxy RADIUS NPS utilise la partie du nom de domaine du nom d’utilisateur et transfère la requête à un serveur NPS dans le domaine correct ou la forêt. Tentatives de connexion pour l’utilisateur de comptes dans un domaine ou forêt peuvent être authentifiés pour NAS dans un autre domaine ou forêt.
- Vous souhaitez effectuer une authentification et autorisation à l’aide d’une base de données qui n’est pas une base de données du compte Windows. Dans ce cas, les demandes de connexion qui correspondent à un nom de domaine Kerberos spécifié sont transférées à un serveur RADIUS, qui a accès à une autre base de données de comptes d’utilisateur et les données d’autorisation. Bases de données Services NDS (Novell Directory) et le langage SQL (Structured Query) peut s’agir d’autres bases de données utilisateur.
- Vous voulez traiter un grand nombre de demandes de connexion. Dans ce cas, au lieu de configurer vos clients RADIUS pour tenter de trouver un équilibre entre leurs demandes de gestion des comptes et de connexion sur plusieurs serveurs RADIUS, vous pouvez configurer pour envoyer leurs demandes de gestion des comptes et de connexion à un proxy RADIUS du serveur NPS. Le proxy RADIUS NPS équilibre dynamiquement la charge de connexion et comptabilité demande sur plusieurs serveurs RADIUS et augmente le traitement d’un grand nombre de clients RADIUS et les authentifications par seconde.
- Vous voulez fournir l’authentification RADIUS et l’autorisation pour les fournisseurs de services externalisés et minimiser la configuration de pare-feu d’intranet. Un pare-feu intranet est entre votre réseau de périmètre (le réseau entre votre intranet et Internet) et l’intranet. En plaçant un serveur NPS sur votre réseau de périmètre, le pare-feu entre votre réseau de périmètre et votre intranet doit autoriser le trafic entre le serveur NPS et plusieurs contrôleurs de domaine. En remplaçant le serveur NPS par un proxy NPS, le pare-feu doit autoriser uniquement le trafic RADIUS entre le proxy NPS et un ou plusieurs serveurs NPS dans votre intranet.

L’illustration suivante montre NPS en tant que proxy RADIUS entre les clients RADIUS et serveurs RADIUS.

![NPS en tant que Proxy RADIUS](../../media/Nps-Proxy/Nps-Proxy.jpg)


Avec le serveur NPS, les organisations peuvent également sous-traiter infrastructure d’accès à distance à un fournisseur de services tout en conservant le contrôle sur la gestion des comptes, l’autorisation et authentification de l’utilisateur.

Configurations de serveur NPS peuvent être créées pour les scénarios suivants:

- Accès sans fil
- Accès à distance de réseau privé (VPN) à distance ou virtuel organisation
- Accès à distance externalisé ou l’accès sans fil
- Accès à Internet
- Accès authentifié aux ressources extranet pour les partenaires commerciaux

## <a name="radius-server-and-radius-proxy-configuration-examples"></a>Serveur RADIUS et des exemples de configuration de proxy RADIUS

Les exemples de configuration suivants montrent comment vous pouvez configurer NPS comme serveur RADIUS et proxy RADIUS.

**NPS comme serveur RADIUS**. Dans cet exemple, NPS est configuré comme un serveur RADIUS, la stratégie de demande de connexion par défaut est la seule stratégie configurée et toutes les demandes de connexion sont traitées par le serveur NPS local. Le serveur NPS peut authentifier et autoriser les utilisateurs dont les comptes sont dans le domaine du serveur NPS et dans les domaines approuvés.

**NPS en tant que proxy RADIUS**. Dans cet exemple, le serveur NPS est configuré en tant que proxy RADIUS qui transfère les demandes de connexion à des groupes de serveurs RADIUS distants dans deux domaines non approuvés. La stratégie de demande de connexion par défaut est supprimée, et deux nouvelles stratégies de demande de connexion sont créés pour transférer les demandes à chacune des deux domaines non approuvés. Dans cet exemple, NPS ne traite pas les demandes de connexion sur le serveur local. 

**NPS en tant que serveur RADIUS et proxy RADIUS**. En plus de la stratégie de demande de connexion par défaut, ce qui indique que les demandes de connexion sont traitées localement, une nouvelle stratégie de demande de connexion est créée qui transfère les demandes de connexion à un serveur NPS ou tout autre serveur RADIUS dans un domaine non approuvé. Cette deuxième stratégie s’appelle la stratégie de Proxy. Dans cet exemple, la stratégie de Proxy apparaît en premier dans la liste de stratégies. Si la demande de connexion correspond à la stratégie de Proxy, la demande de connexion est transférée vers le serveur RADIUS dans le groupe de serveurs RADIUS distants. Si la demande de connexion ne correspond pas à la stratégie de Proxy, mais ne correspond pas à la stratégie de demande de connexion par défaut, NPS traite la demande de connexion sur le serveur local. Si la demande de connexion ne correspond pas à une stratégie, il est supprimé.

**NPS comme serveur RADIUS avec des serveurs de gestion à distance**. Dans cet exemple, le serveur NPS local n’est pas configuré pour effectuer la gestion des comptes et la stratégie de demande de connexion par défaut est mis à jour afin que les messages de gestion de comptes RADIUS sont transférés à un serveur NPS ou tout autre serveur RADIUS dans un groupe de serveurs RADIUS distants. Bien que les messages de la gestion des comptes sont transférés, les messages d’authentification et d’autorisation ne sont pas transférés et le serveur NPS local effectue ces fonctions pour le domaine local et tous les domaines approuvés.

**NPS avec RADIUS distant vers le mappage utilisateur Windows**. Dans cet exemple, NPS, se comporte comme un serveur RADIUS et en tant que proxy RADIUS pour chaque demande de connexion en transférant la demande d’authentification à un serveur RADIUS à distance lors de l’utilisation d’un compte d’utilisateur Windows local pour l’autorisation. Cette configuration est implémentée en configurant le rayon à distance à l’attribut de mappage de l’utilisateur Windows en tant que condition de la stratégie de demande de connexion. \ (En outre, un compte d’utilisateur doit être créé localement sur le serveur RADIUS qui a le même nom que le compte d’utilisateur distant sur lequel l’authentification est effectuée par le serveur RADIUS à distance. \)

## <a name="configuration"></a>Configuration

Pour configurer NPS comme serveur RADIUS, vous pouvez utiliser configuration standard ou la configuration avancée de la console NPS ou dans le Gestionnaire de serveur. Pour configurer NPS en tant que proxy RADIUS, vous devez utiliser la configuration avancée.

### <a name="standard-configuration"></a>Configuration standard

Avec une configuration standard, les Assistants sont fournies pour vous aider à configurer NPS pour les scénarios suivants:

- Serveur RADIUS pour l’accès à distance ou les connexions VPN
- Serveur RADIUS pour 802. 1 X des connexions sans fil ou câblées

Pour configurer NPS à l’aide d’un Assistant, ouvrez la console NPS, sélectionnez un des scénarios précédents, puis cliquez sur le lien qui ouvre l’Assistant.

### <a name="advanced-configuration"></a>Configuration avancée

Lorsque vous utilisez la configuration avancée, configurer manuellement NPS en tant que serveur RADIUS ou proxy RADIUS. 

Pour configurer NPS à l’aide de configuration avancée, ouvrez la console NPS, puis cliquez sur la flèche en regard **Configuration avancée** pour développer cette section.

Les éléments de configuration avancée suivants sont fournis.

#### <a name="configure-radius-server"></a>Configurer un serveur RADIUS

Pour configurer NPS comme serveur RADIUS, vous devez configurer les clients RADIUS, la stratégie de réseau et gestion de comptes RADIUS.

Pour obtenir des instructions sur l’apport de ces configurations, consultez les rubriques suivantes.

- [Configurer des Clients RADIUS](nps-radius-clients-configure.md)
- [Configurer des stratégies de réseau](nps-np-configure.md)
- [Configurer la gestion de serveur de stratégie réseau](nps-accounting-configure.md)

#### <a name="configure-radius-proxy"></a>Configurer le proxy RADIUS

Pour configurer NPS en tant que proxy RADIUS, vous devez configurer les clients RADIUS, les groupes de serveurs RADIUS distants et les stratégies de demande de connexion.

Pour obtenir des instructions sur l’apport de ces configurations, consultez les rubriques suivantes.

- [Configurer des Clients RADIUS](nps-radius-clients-configure.md)
- [Configurer des groupes de serveurs RADIUS distants](nps-crp-rrsg-configure.md)
- [Configurer des stratégies de demande de connexion](nps-crp-configure.md)

## <a name="bkmk_logging"></a>Journalisation NPS

La journalisation NPS est également appelée gestion de comptes RADIUS. Configurer l’enregistrement de serveur NPS à vos exigences si NPS est utilisé comme un serveur RADIUS, proxy ou n’importe quelle combinaison de ces configurations.

Pour configurer la journalisation NPS, vous devez configurer les événements que vous souhaitez connecté et consultés dans l’Observateur d’événements, puis déterminerez quelles autres informations que vous souhaitez consigner. En outre, vous devez décider si vous souhaitez consigner l’authentification des utilisateurs et les informations de gestion pour les fichiers de journaux texte stockés sur l’ordinateur local ou à une base de données SQLServer sur l’ordinateur local ou sur un ordinateur distant.

Pour plus d’informations, voir [comptabilité serveur de stratégie réseau configurer](nps-accounting-configure.md).
