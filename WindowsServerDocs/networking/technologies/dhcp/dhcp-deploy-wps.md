---
title: Déployer le protocole DHCP à l’aide de Windows PowerShell
description: Vous pouvez utiliser cette rubrique pour déployer un serveur DHCP de version4Windows Server2016 IP (Internet Protocol) qui fournit des adresses IP automatique et les options DHCP aux clients DHCP IPv4 connectés à un ou plusieurs sous-réseaux de votre réseau.
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 7110ad21-a33e-48d5-bb3c-129982913bc8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2be3c02f32229c9b9ee411f5e97305776f9825d8
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-dhcp-using-windows-powershell"></a>Déployer le protocole DHCP à l’aide de Windows PowerShell

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Ce guide fournit des instructions sur l’utilisation de Windows PowerShell pour déployer un serveur \(DHCP\) Dynamic Host Configuration Protocol version4 IP (Internet Protocol) qui attribue automatiquement des adresses IP et les options DHCP aux clients DHCP IPv4 qui sont connectés à un ou plusieurs sous-réseaux de votre réseau.

>[!NOTE]
>Pour télécharger ce document au format Word dans la Galerie TechNet, consultez [déployer DHCP à l’aide de Windows PowerShell dans Windows Server2016](https://gallery.technet.microsoft.com/Deploy-DHCP-Using-Windows-246dd293).

À l’aide de serveurs DHCP pour attribuer IP adresses enregistre dans la surcharge administrative, car vous n’avez pas besoin de configurer manuellement les paramètres de v4 de TCP/IP pour chaque carte réseau sur chaque ordinateur sur votre réseau. Avec le protocole DHCP, configuration de TCP/IP v4 est effectuée automatiquement lorsqu’un ordinateur ou autres client DHCP est connecté à votre réseau.

Vous pouvez déployer votre serveur DHCP dans un groupe de travail en tant qu’un serveur autonome ou en tant que partie d’un domaine ActiveDirectory.

Ce guide contient les sections suivantes.

- [Vue d’ensemble du déploiement de DHCP](#bkmk_overview)
- [Vues d’ensemble de la technologie](#bkmk_technologies)
- [Planifier le déploiement de DHCP](#bkmk_plan)
- [À l’aide de ce Guide dans un laboratoire de Test](#bkmk_lab)
- [Déploiement de DHCP](#bkmk_deploy)
- [Vérifier les fonctionnalités de serveur](#bkmk_verify)
- [Commandes Windows PowerShell pour DHCP](#bkmk_dhcpwps)
- [Liste des commandes Windows PowerShell dans ce guide](#bkmk_list)

## <a name="bkmk_overview"></a>Vue d’ensemble du déploiement de DHCP

L’illustration suivante décrit le scénario que vous pouvez déployer à l’aide de ce guide. Le scénario inclut un serveur DHCP dans un domaine ActiveDirectory. Le serveur est configuré pour fournir des adresses IP aux clients DHCP sur deux sous-réseaux différents. Les sous-réseaux sont séparés par un routeur sur lequel le transfert de DHCP est activé.

![Vue d’ensemble de topologie de réseau DHCP](../../media/Core-Network-Guide/cng16_overview.jpg)

## <a name="bkmk_technologies"></a>Vues d’ensemble de la technologie

Les sections suivantes fournissent de brèves vues d’ensemble du protocole DHCP et TCP/IP.

### <a name="dhcp-overview"></a>Vue d’ensemble DHCP

DHCP est une norme IP permettant de simplifier la gestion de configuration IP de l’hôte. La norme permet l’utilisation de serveurs DHCP comme moyen de gérer l’allocation dynamique d’adresses IP et d’autres informations de configuration pour les clients DHCP sur votre réseau.

DHCP vous permet d’utiliser un serveur DHCP pour attribuer dynamiquement une adresse IP à un ordinateur ou un autre périphérique, tel qu’une imprimante, sur votre réseau local plutôt que de configurer manuellement chaque périphérique avec une adresse IP statique.

Chaque ordinateur sur un réseau TCP/IP doit avoir une adresse IP unique, car l’adresse IP et son masque de sous-réseau associé d’identifier l’ordinateur hôte et le sous-réseau auquel l’ordinateur est attaché. À l’aide de DHCP, vous pouvez vous assurer que tous les ordinateurs qui sont configurés en tant que clients DHCP reçoivent une adresse IP qui convient à leur emplacement réseau et le sous-réseau, et à l’aide des options DHCP, telles que la passerelle par défaut et les serveurs DNS, vous pouvez fournir automatiquement aux clients DHCP avec les informations dont ils ont besoin pour fonctionner correctement sur votre réseau.

Pour les réseaux basés sur IP, DHCP réduit la complexité et la quantité de travail administratif impliqué dans la configuration des ordinateurs.

### <a name="tcpip-overview"></a>Vue d’ensemble de TCP/IP

Par défaut, toutes les versions de systèmes d’exploitation Windows Server et Windows Client possèdent des paramètres TCP/IP pour IP version4connexions configurées pour obtenir automatiquement une adresse IP et autres informations, appelés options DHCP, à partir d’un serveur DHCP. Pour cette raison, vous n’avez pas besoin de configurer manuellement les paramètres TCP/IP, sauf si l’ordinateur est un ordinateur serveur ou un autre périphérique qui nécessite une adresse IP statique, configurée manuellement. 

Par exemple, il est recommandé de configurer manuellement l’adresse IP du serveur DHCP et les adresses IP des serveurs DNS et les contrôleurs de domaine qui exécutent les Services de domaine ActiveDirectory \(ADDS\).

TCP/IP dans Windows Server2016 est la suivante:

-   Logiciel réseau reposant sur les protocoles réseau standard.

-   Un protocole réseau d’entreprise routable qui prend en charge la connexion de votre ordinateur basé sur Windows à un réseau local (LAN) et les environnements de réseau (étendu WAN).

-   Technologies de base et des utilitaires pour connecter votre ordinateur Windows avec des systèmes hétérogènes pour le partage d’informations.

-   Une base pour pouvoir accéder aux services Internet globaux, tels que les serveurs Web et le protocole FTP (File Transfer).

-   Une infrastructure client/serveur interplateforme, évolutive et robuste.

TCP/IP propose des utilitaires TCP/IP de base qui permettent à des ordinateurs Windows à se connecter et partager des informations avec d’autres Microsoft et les systèmes non Microsoft, y compris:

- Windows Server2016

- Windows10

- Windows Server2012R2

- Windows8.1

- Windows Server2012

- Windows8

- Windows Server2008R2

- Windows7

- Windows Server2008

- WindowsVista

- Hôtes Internet

- Systèmes Apple Macintosh

- Gros systèmes IBM

- Systèmes UNIX et Linux

- Systèmes Open VMS

- Imprimantes réseau

- Tablettes et téléphones cellulaires avec les réseaux câblés Ethernet ou de la technologie sans fil 802.11 activé

## <a name="bkmk_plan"></a>Planifier le déploiement de DHCP

Voici les étapes de planification clés avant d’installer le rôle serveur DHCP.

### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>Planification des serveurs DHCP et l’acheminement DHCP

Les messages DHCP étant des messages de diffusion, ils ne sont pas acheminés entre sous-réseaux par les routeurs. Si vous avez plusieurs sous-réseaux et que vous souhaitez fournir un service DHCP pour chaque sous-réseau, vous devez effectuer l’une des opérations suivantes:

-   Installer un serveur DHCP sur chaque sous-réseau

-   Configurez les routeurs pour transférer des messages de diffusion DHCP entre les sous-réseaux et configurez plusieurs étendues sur le serveur DHCP, une étendue par sous-réseau.

Dans la plupart des cas, la configuration de routeurs pour transférer des messages de diffusion DHCP est plus efficace que le déploiement d’un serveur DHCP sur chaque segment physique du réseau.

### <a name="planning-ip-address-ranges"></a>Planification des plages d’adresses IP

Chaque sous-réseau doit avoir sa propre plage d’adresses IP unique. Ces plages sont représentées sur un serveur DHCP avec des étendues.

Une étendue est un groupement administratif des adresses IP des ordinateurs sur un sous-réseau qui utilisent le service DHCP. L’administrateur crée d’abord une étendue pour chaque sous-réseau physique, puis utilise l’étendue pour définir les paramètres utilisés par les clients.

Une étendue possède les propriétés suivantes:

-   Une plage d’adresses IP à partir duquel inclure ou exclure les adresses utilisées pour les offres de bail de service DHCP.

-   Un masque de sous-réseau, qui détermine le préfixe de sous-réseau pour une adresse IP donnée.

-   Nom d’étendue attribué lors de sa création.

-   Valeurs de durée de bail, qui sont affectées aux clients DHCP recevant des adresses IP allouées dynamiquement.

-   Des options d’étendue DHCP configurées pour être affectées aux clients DHCP, telles que l’adresse IP du serveur DNS et l’adresse IP de passerelle routeur/par défaut.

-   Réservations sont utilisées si vous le souhaitez pour vous assurer qu’un client DHCP reçoit toujours la même adresse IP.

Avant de déployer vos serveurs, liste de vos sous-réseaux et la plage d’adresses IP à utiliser pour chaque sous-réseau.

### <a name="planning-subnet-masks"></a>Planification des masques de sous-réseau

ID de réseau et ID d’hôte au sein d’une adresse IP sont distingués à l’aide d’un masque de sous-réseau. Chaque masque de sous-réseau est un nombre 32bits qui utilise des groupes de bits consécutifs de tous les zéros (1) pour identifier le réseau ID et tous les zéros (0) pour identifier les parties d’ID hôte d’une adresse IP.

Par exemple, le masque de sous-réseau normalement utilisé avec l’adresse IP 131.107.16.200 est le nombre binaire de 32bits suivant:

```
11111111 11111111 00000000 00000000
```

Ce numéro de masque de sous-réseau est 16bits de suivi de 16bits de valeur zéro, indiquant que les sections réseau les ID ID et l’hôte de cette adresse IP sont les deux une longueur de 16bits. En règle générale, ce masque de sous-réseau est affiché en notation décimale comme 255.255.0.0.

Le tableau suivant affiche les masques de sous-réseau des classes d’adresses Internet.

|Classe d’adresses|Bits pour le masque de sous-réseau|Masque de sous-réseau|
|-----------------|------------------------|---------------|
|Classe A|11111111 00000000 00000000 00000000|255.0.0.0|
|Classe B|11111111 11111111 00000000 00000000|255.255.0.0|
|Classe C|11111111 11111111 11111111 00000000|255.255.255.0|

Lorsque vous créez une étendue dans DHCP et que vous entrez la plage d’adresses IP pour l’étendue, DHCP fournit ces valeurs de masque de sous-réseau par défaut. En règle générale, les valeurs de masque de sous-réseau par défaut sont acceptables pour la plupart des réseaux avec aucune configuration particulière et où chaque segment de réseau IP correspond à un seul réseau physique.

Dans certains cas, vous pouvez utiliser des masques de sous-réseau personnalisés pour implémenter la mise en sous-réseau IP. Mise en sous-réseau IP, vous pouvez subdiviser la partie ID d’hôte par défaut d’une adresse IP pour spécifier les sous-réseaux, qui sont des sous-divisions de l’ID de réseau basée sur la classe d’origine.

En personnalisant la longueur du masque de sous-réseau, vous pouvez réduire le nombre de bits utilisés pour l’ID d’hôte réel.

Pour éviter les problèmes d’adressage et de routage, vous devez vous assurer que tous les ordinateurs TCP/IP sur un segment réseau utilisent le même masque de sous-réseau et que chaque ordinateur ou périphérique possède une adresse IP unique.

### <a name="planning-exclusion-ranges"></a>Planification des plages d’exclusion

Lorsque vous créez une étendue sur un serveur DHCP, vous spécifiez une plage d’adresses IP qui inclut toutes les adresses IP que le serveur DHCP est autorisé à louer aux clients DHCP, tels que les ordinateurs et autres périphériques. Si vous passez et configurez manuellement certains serveurs et autres périphériques avec statique des adresses IP à partir de la même plage d’adresses IP qui utilise le serveur DHCP, vous risquez de provoquer un conflit d’adresse IP, où vous et le serveur DHCP ont tous deux affectés la même adresse IP à différents appareils.

Pour résoudre ce problème, vous pouvez créer une plage d’exclusion pour l’étendue DHCP. Une plage d’exclusion est une contigu plage d’adresses IP dans la plage d’adresses IP de l’étendue que le serveur DHCP n’est pas autorisé à utiliser. Si vous créez une plage d’exclusion, le serveur DHCP n’affecte pas les adresses de cette plage, ce qui vous permet d’attribuer manuellement ces adresses sans avoir à créer un conflit d’adresse IP.

Vous pouvez exclure des adresses IP de la distribution par le serveur DHCP en créant une plage d’exclusion pour chaque étendue. Vous devez utiliser des exclusions pour tous les périphériques qui sont configurés avec une adresse IP statique. Les adresses exclues doivent figurer toutes les adresses IP que vous avez attribuées manuellement à d’autres serveurs, les clients non DHCP, stations de travail sans disque ou routage et accès à distance et clients PPP.

Il est recommandé de configurer votre plage d’exclusion avec des adresses supplémentaires pour prendre en compte la croissance future du réseau. Le tableau suivant fournit une plage d’exclusion exemple pour une étendue avec une plage d’adresses IP de 10.0.0.1 - 10.0.0.254 et un masque de sous-réseau de 255.255.255.0.

|Éléments de configuration|Exemples de valeurs|
|-----------------------|------------------|
|Adresse IP de début de la plage d’exclusion|10.0.0.1|
|Adresse IP de fin de la plage d’exclusion|10.0.0.25|

### <a name="planning-tcpip-static-configuration"></a>Planification de la configuration TCP/IP statique

Certains appareils, tels que les routeurs, les serveurs DHCP et les serveurs DNS, doivent être configurés avec une adresse IP statique. En outre, vous devrez peut-être des périphériques supplémentaires, telles que des imprimantes, que vous souhaitez vous assurer de toujours avoir la même adresse IP. Répertorier les périphériques que vous souhaitez configurer statiquement pour chaque sous-réseau, puis planifiez la plage d’exclusion que vous souhaitez utiliser sur le serveur DHCP pour vous assurer que le serveur DHCP ne loue pas l’adresse IP d’un périphérique configuré de manière statique. Une plage d’exclusion est une séquence limitée d’adresses IP dans une étendue, exclue des offres de service DHCP. Plages d’exclusion s’assurer que toutes les adresses de ces plages ne sont pas proposés par le serveur aux clients DHCP sur votre réseau.

Par exemple, si la plage d’adresses IP pour un sous-réseau est 192.168.0.1 par le biais de 192.168.0.254 et dix périphériques que vous souhaitez configurer avec une adresse IP statique, vous pouvez créer une plage d’exclusion pour le 192.168.0. *x* étendue qui inclut dix ou plusieurs adresses IP: 192.168.0.1 via 192.168.0.15.

Dans cet exemple, vous utilisez dix des adresses IP exclues pour configurer des serveurs et autres périphériques avec des adresses IP statiques et des adresses IP supplémentaires cinq restent disponibles pour la configuration statique de nouveaux périphériques que vous souhaitez ajouter à l’avenir. Avec cette plage d’exclusion, le serveur DHCP est laissé avec un pool d’adresses de 192.168.0.16 via 192.168.0.254.

Vous trouverez des exemples supplémentaires d’éléments de configuration pour les services ADDS et DNS dans le tableau suivant.

|Éléments de configuration|Exemples de valeurs|
|-----------------------|------------------|
|Liaisons de connexion réseau|Ethernet|
|Paramètres du serveur DNS|DC1.corp.contoso.com|
|Adresse IP du serveur DNS préféré|10.0.0.2|
|Valeurs d’étendue<br /><br />1. Nom de l’étendue de<br />2. Adresse IP de début<br />3. Adresse IP de fin<br />4. Masque de sous-réseau<br />5. (facultative) de la passerelle par défaut<br />6. Durée du bail|1. Sous-réseau principal<br />2.  10.0.0.1<br />3.  10.0.0.254<br />4.  255.255.255.0<br />5.  10.0.0.1<br />6. 8jours|
|Mode d’opération du serveur DHCP IPv6|Non activé|

## <a name="bkmk_lab"></a>À l’aide de ce Guide dans un laboratoire de Test

Vous pouvez utiliser ce guide pour déployer DHCP dans un laboratoire de test avant de déployer dans un environnement de production. 

>[!NOTE]
>Si vous ne souhaitez pas déployer le protocole DHCP dans un laboratoire de test, vous pouvez passer à la section [DHCP déployer](#bkmk_deploy).

La configuration requise pour votre laboratoire diffère selon que vous utilisez des serveurs physiques ou virtuels \(VMs\), et que vous à l’aide d’un domaine ActiveDirectory ou déploiement de serveur DHCP autonome. 

Vous pouvez utiliser les informations suivantes pour déterminer les ressources minimales, que vous devez tester le déploiement de DHCP à l’aide de ce guide.

### <a name="test-lab-requirements-with-vms"></a>Configuration requise du laboratoire de test avec les machines virtuelles

Pour déployer DHCP dans un laboratoire de test avec les ordinateurs virtuels, vous devez les ressources suivantes.

Pour le déploiement de domaine ou de déploiement autonome, vous devez un serveur qui est configuré comme un hôte Hyper\-V.

**Déploiement de domaine**

Ce déploiement nécessite un seul serveur physique, un seul commutateur virtuel, deux serveurs virtuels et un client virtuel:

Sur votre serveur physique, dans le Gestionnaire Hyper-V, créez les éléments suivants.

1. Un **interne** commutateur virtuel. Ne créez pas une **externe** virtuel basculer, car si votre hôte Hyper\-V sur un sous-réseau qui inclut un serveur DHCP, votre test de machines virtuelles reçoit une adresse IP de votre serveur DHCP. En outre, le serveur DHCP de test que vous déployez attribue les adresses IP à d’autres ordinateurs sur le sous-réseau sur lequel l’hôte Hyper\-V est installé.
1. Un ordinateur virtuel exécutant Windows Server2106 configuré comme contrôleur de domaine avec les Services de domaine ActiveDirectory qui est connecté au commutateur virtuel interne que vous avez créé. Pour faire correspondre ce guide, ce serveur doit disposer d’une adresse IP statique de 10.0.0.2. Pour plus d’informations sur le déploiement des services ADDS, voir la section **DC1 déploiement** dans Windows Server2016 [Guide réseau de base](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01).
1. Une machine virtuelle exécutant Windows Server2106 que vous allez configurer comme un serveur DHCP à l’aide de ce guide et qui est connecté à interne virtuel basculer vous avez créé. 
1. Un ordinateur virtuel exécutant un système d’exploitation client Windows qui est connecté à interne virtuel basculer vous avez créé et que vous allez utiliser pour vérifier que votre serveur DHCP est affecter dynamiquement des adresses IP et les options DHCP aux clients DHCP.

**Déploiement du serveur DHCP Standalone**

Ce déploiement nécessite un seul serveur physique, un seul commutateur virtuel, un serveur virtuel et un client virtuel:

Sur votre serveur physique, dans le Gestionnaire Hyper-V, créez les éléments suivants.

1. Un **interne** commutateur virtuel. Ne créez pas une **externe** virtuel basculer, car si votre hôte Hyper\-V sur un sous-réseau qui inclut un serveur DHCP, votre test de machines virtuelles reçoit une adresse IP de votre serveur DHCP. En outre, le serveur DHCP de test que vous déployez attribue les adresses IP à d’autres ordinateurs sur le sous-réseau sur lequel l’hôte Hyper\-V est installé.
1. Une machine virtuelle exécutant Windows Server2106 que vous allez configurer comme un serveur DHCP à l’aide de ce guide et qui est connecté à interne virtuel basculer vous avez créé.
1. Un ordinateur virtuel exécutant un système d’exploitation client Windows qui est connecté à interne virtuel basculer vous avez créé et que vous allez utiliser pour vérifier que votre serveur DHCP est affecter dynamiquement des adresses IP et les options DHCP aux clients DHCP.

### <a name="test-lab-requirements-with-physical-servers"></a>Configuration requise du laboratoire de test avec les serveurs physiques

Pour déployer DHCP dans un laboratoire de test avec les serveurs physiques, vous devez les ressources suivantes.

**Déploiement de domaine**

Ce déploiement requiert un hub ou commutateur, deux serveurs physiques et un client physique:

1. Un concentrateur ou commutateur Ethernet auxquels vous pouvez vous connecter les ordinateurs physiques avec des câbles Ethernet
1. Un seul ordinateur physique exécutant Windows Server2106 configuré comme contrôleur de domaine avec les Services de domaine ActiveDirectory. Pour faire correspondre ce guide, ce serveur doit disposer d’une adresse IP statique de 10.0.0.2. Pour plus d’informations sur le déploiement des services ADDS, voir la section **DC1 déploiement** dans Windows Server2016 [Guide réseau de base](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01).
1. Un seul ordinateur physique exécutant Windows Server2106 que vous allez configurer comme un serveur DHCP à l’aide de ce guide. 
1. Un seul ordinateur physique exécutant un système d’exploitation client Windows que vous allez utiliser pour vérifier que votre serveur DHCP est affecter dynamiquement des adresses IP et les options DHCP aux clients DHCP.

>[!NOTE]
>Si vous n’avez pas suffisamment machines de test pour ce déploiement, vous pouvez utiliser un ordinateur de test pour les services ADDS et de DHCP - Toutefois, cette configuration n’est pas recommandée pour un environnement de production.

**Déploiement du serveur DHCP Standalone**

Ce déploiement requiert un hub ou commutateur, un seul serveur physique et un client physique:

1. Un concentrateur ou commutateur Ethernet auxquels vous pouvez vous connecter les ordinateurs physiques avec des câbles Ethernet
2. Un seul ordinateur physique exécutant Windows Server2106 que vous allez configurer comme un serveur DHCP à l’aide de ce guide. 
3. Un seul ordinateur physique exécutant un système d’exploitation client Windows que vous allez utiliser pour vérifier que votre serveur DHCP est affecter dynamiquement des adresses IP et les options DHCP aux clients DHCP.


## <a name="bkmk_deploy"></a>Déploiement de DHCP

Cette section fournit des exemples de commandes Windows PowerShell que vous pouvez utiliser pour déployer DHCP sur un seul serveur. Avant d’exécuter ces exemples de commandes sur votre serveur, vous devez modifier les commandes pour correspondre à votre réseau et votre environnement. 

Par exemple, avant d’exécuter les commandes, vous devez remplacer les exemples de valeurs dans les commandes pour les éléments suivants:

- Noms d’ordinateur
- Plage d’adresses IP pour chaque étendue que vous souhaitez configurer (1 étendue par sous-réseau)
- Masque de sous-réseau pour chaque plage d’adresses IP à configurer
- Nom de l’étendue pour chaque étendue
- Plage d’exclusion pour chaque étendue
- Valeurs d’option DHCP, telles que la passerelle par défaut, nom de domaine et les serveurs DNS ou WINS
- Noms d’interface

>[!IMPORTANT]
>Examiner et modifier chaque commande pour votre environnement avant d’exécuter la commande.

### <a name="where-to-install-dhcp---on-a-physical-computer-or-a-vm"></a>Où installer DHCP - sur un ordinateur physique ou virtuel?

Vous pouvez installer le rôle de serveur DHCP sur un ordinateur physique ou sur un ordinateur virtuel \(VM\) est installé sur un ordinateur hôte Hyper\-V. Si vous installez DHCP sur un ordinateur virtuel et que vous souhaitez que le serveur DHCP pour fournir des attributions d’adresses IP aux ordinateurs sur le réseau physique auquel l’ordinateur hôte Hyper-V est connecté, vous devez connecter la carte réseau virtuelle de machine virtuelle à un commutateur virtuel Hyper-V qui est **externe**.

Pour plus d’informations, consultez la section **créer un commutateur virtuel avec le Gestionnaire Hyper-V** dans la rubrique [créer un réseau virtuel](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/connect-to-network).

### <a name="run-windows-powershell-as-an-administrator"></a>Exécutez Windows PowerShell en tant qu’administrateur

Vous pouvez utiliser la procédure suivante pour exécuter Windows PowerShell avec des privilèges d’administrateur.

1. Sur un ordinateur exécutant Windows Server2016, cliquez sur **Démarrer**, puis cliquez sur l’icône Windows PowerShell. Un menu s’affiche. 

2. Dans le menu, cliquez sur **plus**, puis cliquez sur **exécuter en tant qu’administrateur**. Si vous y êtes invité, tapez les informations d’identification d’un compte qui possède des privilèges d’administrateur sur l’ordinateur. Si le compte d’utilisateur avec lequel vous êtes connecté à l’ordinateur est un compte de niveau administrateur, vous recevrez pas les informations d’identification.

3. Windows PowerShell s’ouvre avec des privilèges d’administrateur.

### <a name="rename-the-dhcp-server-and-configure-a-static-ip-address"></a>Renommer le serveur DHCP et de configurer une adresse IP statique

Si vous n’avez pas déjà fait, vous pouvez utiliser les commandes Windows PowerShell suivantes pour renommer le serveur DHCP et de configurer une adresse IP statique pour le serveur.

**Configurer une adresse IP statique**

Vous pouvez utiliser les commandes suivantes pour attribuer une adresse IP statique pour le serveur DHCP et pour configurer les propriétés TCP/IP du serveur DHCP avec l’adresse IP de serveur DNS correcte. Vous devez également remplacer des noms d’interface et les adresses IP dans cet exemple avec les valeurs que vous souhaitez utiliser pour configurer votre ordinateur.

 `New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24`

 `Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2`

Pour plus d’informations sur ces commandes, consultez les rubriques suivantes.

- [New-NetIPAddress](https://technet.microsoft.com/itpro/powershell/windows/tcpip/new-netipaddress)
- [Set-DnsClientServerAddress](https://technet.microsoft.com/itpro/powershell/windows/dns-client/set-dnsclientserveraddress)

**Renommer l’ordinateur**

Vous pouvez utiliser les commandes suivantes à renommer, puis redémarrez l’ordinateur.

`Rename-Computer -Name DHCP1`

 `Restart-Computer`

Pour plus d’informations sur ces commandes, consultez les rubriques suivantes.

- [Rename-Computer](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/rename-computer)
- [Restart-Computer](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/restart-computer)

### <a name="join-the-computer-to-the-domain-optional"></a>Joindre l’ordinateur pour le domaine \(Optional\)

Si vous installez le serveur DHCP dans un environnement de domaine ActiveDirectory, vous devez joindre l’ordinateur au domaine. Ouvrez Windows PowerShell avec des privilèges d’administrateur, puis exécutez la commande suivante après avoir remplacé le nom de domaine NetBios **CORP** avec une valeur qui est appropriée pour votre environnement.

    Add-Computer CORP

Lorsque vous y êtes invité, tapez les informations d’identification pour un compte d’utilisateur de domaine qui a l’autorisation de joindre un ordinateur au domaine. 

    Restart-Computer

Pour plus d’informations sur la commande Add-Computer, consultez la rubrique suivante.

- [Add-Computer](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/add-computer)

### <a name="install-dhcp"></a>Installer DHCP

Après le redémarrage de l’ordinateur, ouvrez Windows PowerShell avec des privilèges d’administrateur, puis installer DHCP en exécutant la commande suivante.

    Install-WindowsFeature DHCP -IncludeManagementTools

Pour plus d’informations sur cette commande, consultez la rubrique suivante.

- [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature)

### <a name="create-dhcp-security-groups"></a>Créer des groupes de sécurité DHCP

Pour créer des groupes de sécurité, vous devez exécuter une commande de \(netsh\) d’interface réseau dans Windows PowerShell et puis redémarrez le service DHCP afin que les nouveaux groupes devient actifs.

Lorsque vous exécutez la commande netsh suivante sur le serveur DHCP, le **Administrateurs DHCP** et **utilisateurs DHCP** des groupes de sécurité sont créés dans **utilisateurs et groupes locaux** sur le serveur DHCP.

    netsh dhcp add securitygroups

La commande suivante redémarre le service DHCP sur l’ordinateur local.

    Restart-service dhcpserver

Pour plus d’informations sur ces commandes, consultez les rubriques suivantes.

- [Environnement réseau (Netsh)](../netsh/netsh.md)
- [Restart-Service](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/restart-service)

### <a name="authorize-the-dhcp-server-in-active-directory-optional"></a>Autoriser le serveur DHCP dans ActiveDirectory \(Optional\)

Si vous installez DHCP dans un environnement de domaine, vous devez effectuer les étapes suivantes pour autoriser le serveur DHCP pour fonctionner dans le domaine.

>[!NOTE]
>Les serveurs DHCP non autorisés qui sont installés dans les domaines ActiveDirectory ne peut pas fonctionner correctement et ne pas louer des adresses IP aux clients DHCP. La désactivation automatique de serveurs DHCP non autorisés est une fonctionnalité de sécurité qui empêche les serveurs DHCP non autorisés d’affecter des adresses IP incorrectes aux clients sur votre réseau.

Vous pouvez utiliser la commande suivante pour ajouter le serveur DHCP à la liste des serveurs DHCP autorisés dans ActiveDirectory. 

>[!NOTE]
>Si vous ne disposez pas d’un environnement de domaine, n’exécutez pas cette commande.

 `Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com -IPAddress 10.0.0.3`

Pour vérifier que le serveur DHCP est autorisé dans ActiveDirectory, vous pouvez utiliser la commande suivante.

    Get-DhcpServerInDC

Voici les résultats d’exemple qui s’affichent dans Windows PowerShell.

    
        IPAddress   DnsName
        ---------   -------
        10.0.0.3    DHCP1.corp.contoso.com
    

Pour plus d’informations sur ces commandes, consultez les rubriques suivantes.

- [Add-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverindc)
- [Get-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc)

### <a name="notify-server-manager-that-post-install-dhcp-configuration-is-complete-optional"></a>Notifier le Gestionnaire de serveur que DHCP post\-installation configuration est terminée \(Optional\)

Après avoir terminé les tâches post\-installation, telles que la création de groupes de sécurité et d’autorisation du serveur DHCP dans ActiveDirectory, le Gestionnaire de serveur peut toujours afficher une alerte dans l’interface utilisateur indiquant que les étapes de l’installation de post\ doivent être effectuées à l’aide de l’Assistant Installation après la DHCP Configuration.

Vous pouvez empêcher ce message now\ inutiles et inexacte dans le Gestionnaire de serveur en configurant la clé de Registre suivante à l’aide de cette commande Windows PowerShell.

    Set-ItemProperty –Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ServerManager\Roles\12 –Name ConfigurationState –Value 2

Pour plus d’informations sur cette commande, consultez la rubrique suivante.

- [Set-ItemProperty](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/set-itemproperty?f=255&MSPPError=-2147217396)

### <a name="set-server-level-dns-dynamic-update-configuration-settings-optional"></a>Définir la configuration paramètres \(Optional\) serveur niveau DNS mise à jour dynamique

Si vous souhaitez que le serveur DHCP pour effectuer des mises à jour dynamiques DNS pour les ordinateurs clients DHCP, vous pouvez exécuter la commande suivante pour configurer ce paramètre. Il s’agit d’un niveau server paramètre, ne pas une étendue niveau paramètre, afin qu’il affecte toutes les étendues que vous configurez sur le serveur. Cet exemple de commande configure également le serveur DHCP pour supprimer les enregistrements de ressource DNS pour les clients lorsque le client moins arrive à expiration.

    Set-DhcpServerv4DnsSetting -ComputerName "DHCP1.corp.contoso.com" -DynamicUpdates "Always" -DeleteDnsRRonLeaseExpiry $True

Vous pouvez utiliser la commande suivante pour configurer les informations d’identification que le serveur DHCP utilise pour enregistrer ou annuler l’inscription des enregistrements de clients sur un serveur DNS. Cet exemple enregistre les informations d’identification sur un serveur DHCP. La première commande utilise **Get-Credential** pour créer un **PSCredential** objet, puis stocke l’objet dans le **$Credential** variable. La commande vous invite à entrer un mot de passe et le nom d’utilisateur, assurez-vous que vous fournissiez des informations d’identification d’un compte qui a l’autorisation pour mettre à jour les enregistrements de ressources sur votre serveur DNS.

    
    $Credential = Get-Credential
    Set-DhcpServerDnsCredential -Credential $Credential -ComputerName "DHCP1.corp.contoso.com"
    

Pour plus d’informations sur ces commandes, consultez les rubriques suivantes.

- [Set-DhcpServerv4DnsSetting](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4dnssetting)
- [Set-DhcpServerDnsCredential](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverdnscredential)

### <a name="configure-the-corpnet-scope"></a>Configurer l’étendue du réseau d’entreprise

Une fois l’installation de DHCP est terminée, vous pouvez utiliser les commandes suivantes pour configurer et activer l’étendue du réseau d’entreprise, créez une plage d’exclusion pour l’étendue et configurer la passerelle par défaut d’options DHCP, une adresse IP du serveur DNS et un nom de domaine DNS.

    
    Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active`
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15`
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com`
    
    Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2
    

Pour plus d’informations sur ces commandes, consultez les rubriques suivantes.

- [DhcpServerv4Scope ajouter](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4scope)
- [DhcpServerv4ExclusionRange ajouter](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4exclusionrange)
- [Set-DhcpServerv4OptionValue](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4optionvalue)

### <a name="configure-the-corpnet2-scope-optional"></a>Configurer le Corpnet2 \(Optional\) de l’étendue

Si vous avez un deuxième sous-réseau qui est connecté au sous-réseau de premier avec un routeur sur lequel l’acheminement DHCP est activé, vous pouvez utiliser les commandes suivantes pour ajouter une seconde étendue, nommée Corpnet2 pour cet exemple. Cet exemple configure également une plage d’exclusion et l’adresse IP de la passerelle par défaut \ (le routeur adresseIP sur le subnet\) du sous-réseau Corpnet2.

 `Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active`

 `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15`

 `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com`

Si vous avez des sous-réseaux supplémentaires qui sont pris en charge par ce serveur DHCP, vous pouvez répéter ces commandes, à l’aide des valeurs différentes pour tous les paramètres de commande pour ajouter des étendues pour chaque sous-réseau.

>[!IMPORTANT]
>Assurez-vous que tous les routeurs entre votre serveur DHCP et de vos clients DHCP sont configurés pour le transfert des messages DHCP. Consultez la documentation de votre routeur pour plus d’informations sur la façon de configurer le transfert de DHCP.

## <a name="bkmk_verify"></a>Vérifier les fonctionnalités de serveur

Pour vérifier que votre serveur DHCP fournit l’allocation dynamique d’adresses IP aux clients DHCP, vous pouvez vous connecter à un autre ordinateur à un sous-réseau pris en charge. Une fois que vous vous connectez le câble Ethernet pour la carte réseau et l’ordinateur sous tension, il demande une adresse IP de votre serveur DHCP. Vous pouvez vérifier la configuration réussie à l’aide de la **ipconfig /all** commande et révision des résultats ou en effectuant des tests de connectivité, tel que d’essayer d’accéder aux ressources Web avec vos partages de navigateur ou un fichier avec l’Explorateur Windows ou d’autres applications.

Si le client ne reçoit pas une adresse IP de votre serveur DHCP, effectuez les étapes de dépannage suivantes.

1. Assurez-vous que le câble Ethernet est branché à la fois l’ordinateur et le Ethernet commutateur, hub ou le routeur.
1. Si vous connectiez à l’ordinateur client dans un segment de réseau séparé à partir du serveur DHCP par un routeur, assurez-vous que le routeur est configuré pour transférer les messages DHCP.
1. Assurez-vous que le serveur DHCP est autorisé dans ActiveDirectory en exécutant la commande suivante pour récupérer la liste des serveurs DHCP autorisés à partir d’ActiveDirectory. [Get-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc).
1. Assurez-vous que vos étendues sont activés en ouvrant la console DHCP \ (Gestionnaire de serveur, **outils**, **DHCP**\), développement de l’arborescence de serveur pour passer en revue les étendues, puis en double-cliquant sur chaque étendue. Si le menu qui apparaît inclut la sélection **Activate**, cliquez sur **Activate**. \ (Si l’étendue est déjà activé, la sélection du menu lit **Deactivate**. \) 

## <a name="bkmk_dhcpwps"></a>Commandes Windows PowerShell pour DHCP

La référence suivante fournit des descriptions de la commande et la syntaxe pour toutes les commandes Windows PowerShell de DHCP Server pour Windows Server2016. La rubrique répertorie les commandes dans l’ordre alphabétique en fonction du verbe au début des commandes, tel que **obtenir** ou **définir**.

>[!NOTE]
>Vous ne pouvez pas utiliser les commandes de Windows Server2016dans Windows Server2012R2.

- [Module DhcpServer](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/index)

La référence suivante fournit des descriptions de la commande et la syntaxe pour toutes les commandes Windows PowerShell de DHCP Server pour Windows Server2012R2. La rubrique répertorie les commandes dans l’ordre alphabétique en fonction du verbe au début des commandes, tel que **obtenir** ou **définir**.

>[!NOTE]
>Vous pouvez utiliser les commandes de Windows Server2012R2 dans Windows Server2016.

- [Les applets de commande de serveur DHCP dans Windows PowerShell](https://technet.microsoft.com/library/jj590751.aspx)

## <a name="bkmk_list"></a>Liste des commandes Windows PowerShell dans ce guide

Voici une liste simple de commandes et des exemples de valeurs qui sont utilisés dans ce guide.

    
    New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24
    Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2
    Rename-Computer -Name DHCP1
    Restart-Computer
    
    Add-Computer CORP
    Restart-Computer
    
    Install-WindowsFeature DHCP -IncludeManagementTools
    netsh dhcp add securitygroups
    Restart-service dhcpserver
    
    Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com -IPAddress 10.0.0.3
    Get-DhcpServerInDC
    
    Set-ItemProperty –Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ServerManager\Roles\12 –Name ConfigurationState –Value 2
    
    Set-DhcpServerv4DnsSetting -ComputerName "DHCP1.corp.contoso.com" -DynamicUpdates "Always" -DeleteDnsRRonLeaseExpiry $True
    
    $Credential = Get-Credential
    Set-DhcpServerDnsCredential -Credential $Credential -ComputerName "DHCP1.corp.contoso.com"
    
    rem At prompt, supply credential in form DOMAIN\user, password
    
    
    rem Configure scope Corpnet
    
    Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com
    
    Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2
    
    rem Configure scope Corpnet2
    
    Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com
    


