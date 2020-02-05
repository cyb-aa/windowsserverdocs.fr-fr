---
title: Déploiement de DHCP à l'aide de Windows PowerShell
description: Vous pouvez utiliser cette rubrique pour déployer un serveur DHCP Windows Server 2016 Internet Protocol (IP) version 4 qui fournit des adresses IP automatiques et des options DHCP aux clients DHCP IPv4 connectés à un ou plusieurs sous-réseaux de votre réseau.
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 7110ad21-a33e-48d5-bb3c-129982913bc8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 16900809c2c6b877d2b5c45f1c3ca26e55c6bea9
ms.sourcegitcommit: 7df2bd3a7d07a50ace86477335ed6fbfb2dac373
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77027942"
---
# <a name="deploy-dhcp-using-windows-powershell"></a>Déploiement de DHCP à l'aide de Windows PowerShell

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Ce guide fournit des instructions sur l’utilisation de Windows PowerShell pour déployer un protocole de configuration d’hôte dynamique IP (Internet Protocol) version 4 \(serveur DHCP\) qui attribue automatiquement des adresses IP et des options DHCP aux clients DHCP IPv4 qui sont connectés à un ou plusieurs sous-réseaux de votre réseau.

> [!NOTE]
> Pour télécharger ce document au format Word depuis la Galerie TechNet, voir [déployer DHCP à l’aide de Windows PowerShell dans Windows Server 2016](https://gallery.technet.microsoft.com/Deploy-DHCP-Using-Windows-246dd293).

L’utilisation de serveurs DHCP pour attribuer des adresses IP entraîne des économies en matière d’administration, car vous n’avez pas besoin de configurer manuellement les paramètres TCP/IP V4 pour chaque carte réseau de chaque ordinateur de votre réseau. Avec DHCP, la configuration TCP/IP V4 s’effectue automatiquement lorsqu’un ordinateur ou un autre client DHCP est connecté à votre réseau.

Vous pouvez déployer votre serveur DHCP dans un groupe de travail en tant que serveur autonome ou dans le cadre d’un domaine Active Directory.

Ce guide contient les sections suivantes.

- [Présentation du déploiement DHCP](#bkmk_overview)
- [Vues d’ensemble de la technologie](#bkmk_technologies)
- [Planifier le déploiement DHCP](#bkmk_plan)
- [Utilisation de ce guide dans un laboratoire de test](#bkmk_lab)
- [Déployer DHCP](#bkmk_deploy)
- [Vérifier la fonctionnalité du serveur](#bkmk_verify)
- [Commandes Windows PowerShell pour DHCP](#bkmk_dhcpwps)
- [Liste des commandes Windows PowerShell dans ce guide](#bkmk_list)

## <a name="bkmk_overview"></a>Présentation du déploiement DHCP

L’illustration suivante représente le scénario que vous pouvez déployer à l’aide de ce guide. Le scénario comprend un serveur DHCP dans un domaine Active Directory. Le serveur est configuré pour fournir des adresses IP aux clients DHCP sur deux sous-réseaux différents. Les sous-réseaux sont séparés par un routeur pour lequel le transfert DHCP est activé.

![Vue d’ensemble de la topologie du réseau DHCP](../../media/Core-Network-Guide/cng16_overview.jpg)

## <a name="bkmk_technologies"></a>Vues d’ensemble de la technologie

Les sections suivantes fournissent de brèves vues d’ensemble de DHCP et TCP/IP.

### <a name="dhcp-overview"></a>Vue d’ensemble de DHCP

DHCP est une norme IP permettant de simplifier la gestion de la configuration IP de l’hôte. Cette norme permet d’utiliser des serveurs DHCP comme moyen de gérer l’allocation dynamique d’adresses IP et d’autres informations de configuration pour les clients DHCP sur votre réseau.

DHCP vous permet d’utiliser un serveur DHCP pour attribuer dynamiquement une adresse IP à un ordinateur ou à un autre périphérique, tel qu’une imprimante, sur votre réseau local, plutôt que de configurer manuellement chaque appareil avec une adresse IP statique.

Chaque ordinateur d’un réseau TCP/IP doit avoir une adresse IP unique, car cette adresse et son masque de sous-réseau associé sont utilisés pour identifier l’ordinateur hôte et le sous-réseau auquel l’ordinateur est joint. Avec le protocole DHCP, vous avez l’assurance que tous les ordinateurs configurés en tant que clients DHCP reçoivent une adresse IP correspondant à leur emplacement réseau et leur sous-réseau. De plus, en utilisant les options DHCP, telles qu’une passerelle par défaut et des serveurs DNS, vous fournissez automatiquement aux clients DHCP les informations dont ils ont besoin pour fonctionner correctement sur votre réseau.

Pour les réseaux TCP/IP, DHCP réduit la complexité et la quantité de travail administratif nécessaires à la configuration des ordinateurs.

### <a name="tcpip-overview"></a>Vue d’ensemble de TCP/IP

Par défaut, toutes les versions des systèmes d’exploitation clients Windows Server et Windows ont des paramètres TCP/IP pour les connexions réseau IP version 4 configurées pour obtenir automatiquement une adresse IP et d’autres informations, appelées options DHCP, à partir d’un serveur DHCP. Pour cette raison, vous n’avez pas besoin de configurer les paramètres TCP/IP manuellement, sauf si l’ordinateur est un ordinateur serveur ou un autre périphérique nécessitant une adresse IP statique configurée manuellement. 

Par exemple, il est recommandé de configurer manuellement l’adresse IP du serveur DHCP, ainsi que les adresses IP des serveurs DNS et des contrôleurs de domaine qui exécutent Active Directory Domain Services \(AD DS\).

Le protocole TCP/IP dans Windows Server 2016 est le suivant :

- un logiciel réseau reposant sur des protocoles réseau standard ;

- un protocole réseau d’entreprise routable qui prend en charge la connexion de votre ordinateur Windows aux environnements de réseau local (LAN, Local Area Network) et de réseau étendu (WAN, Wide Area Network) ;

- un ensemble de technologies et d’utilitaires centraux permettant de connecter votre ordinateur Windows à des systèmes hétérogènes pour le partage d’informations ;

- Base pour accéder aux services Internet globaux, tels que les serveurs Web et protocole FTP (FTP).

- une infrastructure client/serveur interplateforme, évolutive et robuste.

TCP/IP propose des utilitaires TCP/IP de base qui permettent aux ordinateurs Windows de se connecter à d’autres systèmes d’exploitation Microsoft et non-Microsoft pour partager des informations, notamment :

- Windows Server 2016

- Windows 10

- R2 Windows Server 2012

- Windows 8.1

- Windows Server 2012

- Windows 8

- Windows Server 2008 R2

- Windows 7

- Windows Server 2008

- Windows Vista

- Hôtes Internet

- Systèmes Apple Macintosh

- Gros systèmes IBM

- Systèmes UNIX et Linux

- Systèmes Open VMS

- Imprimantes réseau prêtes à l’emploi

- Tablettes et téléphones cellulaires avec technologie Ethernet câblée ou Wireless 802,11 activée

## <a name="bkmk_plan"></a>Planifier le déploiement DHCP

Voici les principales étapes de planification avant l’installation du rôle serveur DHCP.

### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>Planification des serveurs DHCP et de l’acheminement DHCP

Les messages DHCP étant des messages de diffusion, ils ne sont pas acheminés entre les sous-réseaux par les routeurs. Si vous avez plusieurs sous-réseaux et souhaitez proposer le service DHCP sur chacun d’eux, vous devez procéder comme suit :

- Installez un serveur DHCP sur chaque sous-réseau.

- Configurez les routeurs pour qu’ils acheminent les messages de diffusion DHCP dans les sous-réseaux et configurez plusieurs étendues sur le serveur DHCP, à raison d’une étendue par sous-réseau.

En règle générale, il est économiquement plus avantageux de configurer des routeurs pour qu’ils acheminent les messages de diffusion DHCP plutôt que de déployer un serveur DHCP sur chaque segment physique du réseau.

### <a name="planning-ip-address-ranges"></a>Planification de plages d’adresses IP

Chaque sous-réseau doit avoir sa propre plage d’adresses IP unique. Ces plages sont représentées sur un serveur DHCP avec des étendues.

Une étendue est un groupement administratif des adresses IP des ordinateurs d’un sous-réseau qui utilisent le service DHCP. L’administrateur crée d’abord une étendue pour chaque sous-réseau physique, puis utilise l’étendue pour définir les paramètres utilisés par les clients.

Une étendue possède les propriétés suivantes :

- une plage d’adresses IP où inclure ou exclure les adresses utilisées pour les offres de bail de service DHCP ;

- un masque de sous-réseau, qui détermine le préfixe de sous-réseau correspondant à une adresse IP donnée ;

- un nom affecté à l’étendue lors de sa création ;

- des valeurs de durée de bail, qui sont affectées aux clients DHCP recevant des adresses IP allouées de manière dynamique ;

- des options d’étendue DHCP configurées pour être affectées aux clients DHCP, telles que l’adresse IP d’un serveur DNS et l’adresse IP d’un routeur/d’une passerelle par défaut ;

- des réservations, utilisées de manière optionnelle pour s’assurer qu’un client DHCP reçoit toujours la même adresse IP.

Avant de déployer vos serveurs, dressez la liste de vos sous-réseaux et des plages d’adresses IP à utiliser pour chaque sous-réseau.

### <a name="planning-subnet-masks"></a>Planification des masques de sous-réseaux

Au sein d’une adresse IP, les ID de réseau et les ID d’hôte sont différenciés à l’aide d’un masque de sous-réseau. Chaque masque de sous-réseau est un nombre de 32 bits qui utilise des groupes de bits consécutifs composés uniquement de 1 pour identifier l’ID de réseau et composés uniquement de 0 pour identifier les parties de l’adresse IP relatives à l’ID d’hôte.

Par exemple, un masque de sous-réseau normalement utilisé avec l’adresse IP 131.107.16.200 correspond au nombre binaire de 32 bits suivant :

```
11111111 11111111 00000000 00000000
```

Ce numéro de masque de sous-réseau est 16 1 bits suivi de 16 bits de zéro, ce qui indique que les sections ID réseau et ID d’hôte de cette adresse IP ont une longueur de 16 bits. Ce masque de sous-réseau est affiché en notation décimale séparée par des points sous la forme 255.255.0.0.

Le tableau ci-dessous indique les masques de sous-réseau des classes d’adresses Internet.

|Classe d’adresses|Bits du masque de sous-réseau|Masque de sous-réseau|
|-----------------|------------------------|---------------|
|Classe A|11111111 00000000 00000000 00000000|255.0.0.0|
|Classe B|11111111 11111111 00000000 00000000|255.255.0.0|
|Classe C|11111111 11111111 11111111 00000000|255.255.255.0|

Lorsque vous créez une étendue dans DHCP et que vous entrez la plage d’adresses IP de cette étendue, DHCP fournit ces valeurs de masque de sous-réseau par défaut. Les valeurs de masque de sous-réseau par défaut conviennent généralement pour la plupart des réseaux qui ne nécessitent aucune configuration particulière et où chaque segment de réseau IP correspond au même réseau physique.

Dans d’autres cas, vous pourrez utiliser des masques de sous-réseau personnalisés pour implémenter la mise en sous-réseau IP. Dans le cadre de la mise en sous-réseau IP, vous pouvez subdiviser la partie ID d’hôte par défaut d’une adresse IP pour spécifier des sous-réseaux, qui sont des sous-divisions de l’ID de réseau basé sur la classe.

En personnalisant la longueur du masque de sous-réseau, vous pouvez réduire le nombre de bits utilisés pour l’ID d’hôte.

Pour éviter les problèmes d’adressage et de routage, vous devez veiller à ce que tous les ordinateurs TCP/IP d’un segment réseau utilisent le même masque de sous-réseau et que chaque ordinateur ou périphérique possède une adresse IP unique.

### <a name="planning-exclusion-ranges"></a>Planification des plages d’exclusion

Lorsque vous définissez une étendue sur un serveur DHCP, vous spécifiez une plage d’adresses IP qui inclut toutes les adresses IP que le serveur DHCP est autorisé à louer aux clients DHCP, tels que les ordinateurs et autres périphériques. Si, par la suite, vous configurez manuellement certains serveurs et autres périphériques avec des adresses IP statiques de la même plage d’adresses IP que le serveur DHCP utilise, vous risquez involontairement d’attribuer à un périphérique une adresse IP qui a déjà été attribuée à un autre périphérique par le serveur DHCP, et de générer ainsi un conflit d’adresse IP.

Vous pouvez éviter ce problème en créant une plage d’exclusion pour l’étendue DHCP. Une plage d’exclusion est une plage contiguë d’adresses IP au sein de la plage d’adresses IP de l’étendue que le serveur DHCP n’est pas autorisé à utiliser. Si vous créez une plage d’exclusion, le serveur DHCP ne peut attribuer aucune des adresses de cette plage. Vous pouvez alors attribuer manuellement ces adresses sans risque de générer un conflit d’adresse IP.

Vous pouvez exclure des adresses IP de la distribution par le serveur DHCP en créant une plage d’exclusion pour chaque étendue. Vous devez utiliser des exclusions pour tous les périphériques configurés avec une adresse IP statique. Dans les adresses exclues doivent figurer toutes les adresses IP que vous avez attribuées manuellement à d’autres serveurs DHCP, clients non DHCP, stations de travail sans disque ou clients de routage et d’accès à distance et clients PPP.

Il est recommandé de configurer votre plage d’exclusion avec des adresses supplémentaires afin de prendre en compte la croissance future du réseau. Le tableau suivant fournit un exemple de plage d’exclusion pour une étendue avec une plage d’adresses IP de 10.0.0.1-10.0.0.254 et un masque de sous-réseau de 255.255.255.0.

|Éléments de configuration|Exemples de valeurs|
|-----------------------|------------------|
|Adresse IP de début de la plage d’exclusion|10.0.0.1|
|Adresse IP de fin de la plage d’exclusion|adresses 10.0.0.25|

### <a name="planning-tcpip-static-configuration"></a>Planification de la configuration TCP/IP statique

Certains périphériques, comme les routeurs, les serveurs DHCP et les serveurs DNS doivent être configurés avec une adresse IP statique. Par ailleurs, vous voudrez peut-être garantir que des périphériques supplémentaires, comme des imprimantes, disposent toujours de la même adresse IP. Dressez la liste des périphériques que vous souhaitez configurer de manière statique pour chaque sous-réseau, puis planifiez la plage d’exclusion que vous voulez utiliser sur le serveur DHCP pour faire en sorte que le serveur DHCP ne loue pas l’adresse IP d’un périphérique configuré de manière statique. Une plage d’exclusion est une séquence limitée d’adresses IP au sein d’une étendue, qui est exclue des offres du service DHCP. Les plages d’exclusion garantissent qu’aucune adresse figurant dans ces plages n’est offerte par le serveur aux clients DHCP sur votre réseau.

Par exemple, si la plage d’adresses IP d’un sous-réseau est 192.168.0.1 à 192.168.0.254 et que vous souhaitez configurer dix périphériques avec une adresse IP statique, vous pouvez créer une plage d’exclusion pour l’étendue 192.168.0.*x* qui inclut dix adresses IP ou plus : 192.168.0.1 à 192.168.0.15.

Dans cet exemple, vous utilisez dix des adresses IP exclues pour configurer des serveurs et autres périphériques avec des adresses IP statiques. Il reste cinq adresses IP pour configurer de manière statique de nouveaux périphériques que vous voudrez peut-être ajouter plus tard. Avec cette plage d’exclusion, le serveur DHCP dispose d’un pool d’adresses allant de 192.168.0.16 à 192.168.0.254.

Des exemples d’éléments de configuration supplémentaires pour AD DS et DNS sont fournis dans le tableau suivant.

|Éléments de configuration|Exemples de valeurs|
|-----------------------|------------------|
|Liaisons de connexion réseau|Ethernet|
|Paramètres du serveur DNS|DC1.corp.contoso.com|
|Adresse IP du serveur DNS préféré|10.0.0.2|
|Valeurs d’étendue<br /><br />1. nom de l’étendue<br />2. adresse IP de début<br />3. adresse IP de fin<br />4. masque de sous-réseau<br />5. passerelle par défaut (facultatif)<br />6. durée du bail|1. sous-réseau principal<br />2.10.0.0.1<br />3.10.0.0.254<br />4.255.255.255.0<br />5.10.0.0.1<br />6.8 jours|
|Mode d’opération du serveur DHCP IPv6|Non activé|

## <a name="bkmk_lab"></a>Utilisation de ce guide dans un laboratoire de test

Vous pouvez utiliser ce guide pour déployer DHCP dans un laboratoire de test avant de déployer dans un environnement de production. 

> [!NOTE]
> Si vous ne souhaitez pas déployer DHCP dans un laboratoire de test, vous pouvez passer à la section [déployer DHCP](#bkmk_deploy).

La configuration requise pour votre laboratoire varie selon que vous utilisez des serveurs physiques ou des machines virtuelles \(les machines virtuelles\), et que vous utilisez un domaine Active Directory ou que vous déployez un serveur DHCP autonome.

Vous pouvez utiliser les informations suivantes pour déterminer les ressources minimales dont vous avez besoin pour tester le déploiement DHCP à l’aide de ce guide.

### <a name="test-lab-requirements-with-vms"></a>Exigences du laboratoire de test avec les machines virtuelles

Pour déployer DHCP dans un laboratoire de test avec des machines virtuelles, vous avez besoin des ressources suivantes.

Pour un déploiement de domaine ou un déploiement autonome, vous avez besoin d’un serveur configuré en tant qu’ordinateur hôte Hyper\-V.

**Déploiement de domaine**

Ce déploiement nécessite un serveur physique, un commutateur virtuel, deux serveurs virtuels et un client virtuel :

Sur votre serveur physique, dans le Gestionnaire Hyper-V, créez les éléments suivants.

1. Un commutateur virtuel **interne** . Ne créez pas de commutateur virtuel **externe** , car si votre hôte Hyper\-V se trouve sur un sous-réseau qui comprend un serveur DHCP, vos machines virtuelles de test recevront une adresse IP de votre serveur DHCP. En outre, le serveur DHCP de test que vous déployez peut affecter des adresses IP à d’autres ordinateurs sur le sous-réseau sur lequel l’hôte Hyper\-V est installé.
1. Un ordinateur virtuel exécutant Windows Server 2016 configuré en tant que contrôleur de domaine avec Active Directory Domain Services connecté au commutateur virtuel interne que vous avez créé. Pour correspondre à ce guide, ce serveur doit avoir une adresse IP configurée statiquement 10.0.0.2. Pour plus d’informations sur le déploiement de AD DS, consultez la section **déploiement de DC1** dans le [Guide du réseau de base](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01)de Windows Server 2016.
1. Une machine virtuelle exécutant Windows Server 2016 que vous allez configurer en tant que serveur DHCP à l’aide de ce guide et qui est connectée au commutateur virtuel interne que vous avez créé. 
1. Un ordinateur virtuel exécutant un système d’exploitation client Windows connecté au commutateur virtuel interne que vous avez créé et que vous allez utiliser pour vérifier que votre serveur DHCP alloue dynamiquement des adresses IP et des options DHCP aux clients DHCP.

**Déploiement de serveur DHCP autonome**

Ce déploiement nécessite un serveur physique, un commutateur virtuel, un serveur virtuel et un client virtuel :

Sur votre serveur physique, dans le Gestionnaire Hyper-V, créez les éléments suivants.

1. Un commutateur virtuel **interne** . Ne créez pas de commutateur virtuel **externe** , car si votre hôte Hyper\-V se trouve sur un sous-réseau qui comprend un serveur DHCP, vos machines virtuelles de test recevront une adresse IP de votre serveur DHCP. En outre, le serveur DHCP de test que vous déployez peut affecter des adresses IP à d’autres ordinateurs sur le sous-réseau sur lequel l’hôte Hyper\-V est installé.
2. Une machine virtuelle exécutant Windows Server 2016 que vous allez configurer en tant que serveur DHCP à l’aide de ce guide et qui est connectée au commutateur virtuel interne que vous avez créé.
3. Un ordinateur virtuel exécutant un système d’exploitation client Windows connecté au commutateur virtuel interne que vous avez créé et que vous allez utiliser pour vérifier que votre serveur DHCP alloue dynamiquement des adresses IP et des options DHCP aux clients DHCP.

### <a name="test-lab-requirements-with-physical-servers"></a>Exigences du laboratoire de test avec les serveurs physiques

Pour déployer DHCP dans un laboratoire de test avec des serveurs physiques, vous avez besoin des ressources suivantes.

**Déploiement de domaine**

Ce déploiement nécessite un concentrateur ou un commutateur, deux serveurs physiques et un client physique :

1. Un concentrateur ou un commutateur Ethernet auquel vous pouvez connecter les ordinateurs physiques à l’aide de câbles Ethernet
2. Un ordinateur physique exécutant Windows Server 2016 configuré en tant que contrôleur de domaine avec Active Directory Domain Services. Pour correspondre à ce guide, ce serveur doit avoir une adresse IP configurée statiquement 10.0.0.2. Pour plus d’informations sur le déploiement de AD DS, consultez la section **déploiement de DC1** dans le [Guide du réseau de base](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01)de Windows Server 2016.
3. Un ordinateur physique exécutant Windows Server 2016 que vous allez configurer en tant que serveur DHCP à l’aide de ce guide.
4. Un ordinateur physique exécutant un système d’exploitation client Windows que vous utiliserez pour vérifier que votre serveur DHCP alloue dynamiquement des adresses IP et des options DHCP aux clients DHCP.

> [!NOTE]
> Si vous ne disposez pas de suffisamment d’ordinateurs de test pour ce déploiement, vous pouvez utiliser un seul ordinateur de test pour les AD DS et DHCP ; Toutefois, cette configuration n’est pas recommandée pour un environnement de production.

**Déploiement de serveur DHCP autonome**

Ce déploiement nécessite un concentrateur ou un commutateur, un serveur physique et un client physique :

1. Un concentrateur ou un commutateur Ethernet auquel vous pouvez connecter les ordinateurs physiques à l’aide de câbles Ethernet
2. Un ordinateur physique exécutant Windows Server 2016 que vous allez configurer en tant que serveur DHCP à l’aide de ce guide.
3. Un ordinateur physique exécutant un système d’exploitation client Windows que vous utiliserez pour vérifier que votre serveur DHCP alloue dynamiquement des adresses IP et des options DHCP aux clients DHCP.


## <a name="bkmk_deploy"></a>Déployer DHCP

Cette section fournit des exemples de commandes Windows PowerShell que vous pouvez utiliser pour déployer DHCP sur un serveur. Avant d’exécuter ces exemples de commandes sur votre serveur, vous devez modifier les commandes pour qu’elles correspondent à votre réseau et à votre environnement. 

Par exemple, avant d’exécuter les commandes, vous devez remplacer les exemples de valeurs dans les commandes pour les éléments suivants :

- Noms des ordinateurs
- Plage d’adresses IP pour chaque étendue que vous souhaitez configurer (1 étendue par sous-réseau)
- Masque de sous-réseau pour chaque plage d’adresses IP que vous souhaitez configurer
- Nom de l’étendue pour chaque étendue
- Plage d’exclusion pour chaque étendue
- Valeurs des options DHCP, telles que la passerelle par défaut, le nom de domaine et les serveurs DNS ou WINS
- Noms d’interface

> [!IMPORTANT]
> Examinez et modifiez chaque commande de votre environnement avant d’exécuter la commande.

### <a name="where-to-install-dhcp---on-a-physical-computer-or-a-vm"></a>Où installer DHCP-sur un ordinateur physique ou une machine virtuelle ?

Vous pouvez installer le rôle de serveur DHCP sur un ordinateur physique ou sur une machine virtuelle \(\) d’ordinateur virtuel installé sur un hôte Hyper\-V. Si vous installez DHCP sur une machine virtuelle et que vous souhaitez que le serveur DHCP fournisse des attributions d’adresses IP aux ordinateurs du réseau physique auquel l’ordinateur hôte Hyper-V est connecté, vous devez connecter la carte réseau virtuelle d’ordinateur virtuel à un commutateur virtuel Hyper-V qui est **externe**.

Pour plus d’informations, consultez la section **créer un commutateur virtuel avec le Gestionnaire Hyper-V** dans la rubrique [créer un réseau virtuel](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/connect-to-network).

### <a name="run-windows-powershell-as-an-administrator"></a>Exécuter Windows PowerShell en tant qu’administrateur

Vous pouvez utiliser la procédure suivante pour exécuter Windows PowerShell avec des privilèges d’administrateur.

1. Sur un ordinateur exécutant Windows Server 2016, cliquez sur **Démarrer**, puis cliquez avec le bouton droit sur l’icône Windows PowerShell. Un menu s’affiche.

2. Dans le menu, cliquez sur **plus**, puis sur **exécuter en tant qu’administrateur**. Si vous y êtes invité, tapez les informations d’identification d’un compte disposant de privilèges d’administrateur sur l’ordinateur. Si le compte d’utilisateur avec lequel vous êtes connecté à l’ordinateur est un compte de niveau administrateur, vous ne recevrez pas d’invite d’informations d’identification.

3. Windows PowerShell s’ouvre avec des privilèges d’administrateur.

### <a name="rename-the-dhcp-server-and-configure-a-static-ip-address"></a>Renommer le serveur DHCP et configurer une adresse IP statique

Si vous ne l’avez pas déjà fait, vous pouvez utiliser les commandes Windows PowerShell suivantes pour renommer le serveur DHCP et configurer une adresse IP statique pour le serveur.

**Configurer une adresse IP statique**

Vous pouvez utiliser les commandes suivantes pour affecter une adresse IP statique au serveur DHCP et pour configurer les propriétés TCP/IP du serveur DHCP avec l’adresse IP du serveur DNS correcte. Remplacez les noms d’interface et les adresses IP utilisés dans cet exemple par les valeurs de configuration souhaitées pour votre ordinateur.

```
New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2
```

Pour plus d’informations sur ces commandes, consultez les rubriques suivantes.

- [New-NetIPAddress](https://docs.microsoft.com/powershell/module/nettcpip/New-NetIPAddress)
- [Set-DnsClientServerAddress](https://docs.microsoft.com/powershell/module/dnsclient/Set-DnsClientServerAddress)

**Renommer l’ordinateur**

Vous pouvez utiliser les commandes suivantes pour renommer et redémarrer l’ordinateur.

```
Rename-Computer -Name DHCP1
Restart-Computer
```

Pour plus d’informations sur ces commandes, consultez les rubriques suivantes.

- [Renommer-ordinateur](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/rename-computer)
- [Restart-Computer](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/restart-computer)

### <a name="join-the-computer-to-the-domain-optional"></a>Joindre l’ordinateur au domaine \(facultatif\)

Si vous installez votre serveur DHCP dans un environnement de domaine Active Directory, vous devez joindre l’ordinateur au domaine. Ouvrez Windows PowerShell avec des privilèges d’administrateur, puis exécutez la commande suivante après avoir remplacé le nom NetBios du domaine **Corp** par une valeur appropriée pour votre environnement.

```
Add-Computer CORP
```

Lorsque vous y êtes invité, tapez les informations d’identification d’un compte d’utilisateur de domaine qui a l’autorisation de joindre un ordinateur au domaine. 

```
Restart-Computer
```

Pour plus d’informations sur la commande Add-Computer, consultez la rubrique suivante.

- [Ajouter un ordinateur](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/add-computer?view=powershell-5.1)

### <a name="install-dhcp"></a>Installer DHCP

Après le redémarrage de l’ordinateur, ouvrez Windows PowerShell avec des privilèges d’administrateur, puis installez DHCP en exécutant la commande suivante.

```
Install-WindowsFeature DHCP -IncludeManagementTools
```

Pour plus d’informations sur cette commande, consultez la rubrique suivante.

- [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/install-windowsfeature)

### <a name="create-dhcp-security-groups"></a>Créer des groupes de sécurité DHCP

Pour créer des groupes de sécurité, vous devez exécuter une commande Network Shell \(netsh\) dans Windows PowerShell, puis redémarrer le service DHCP afin que les nouveaux groupes deviennent actifs.

Lorsque vous exécutez la commande netsh suivante sur le serveur DHCP, les groupes de sécurité **Administrateurs DHCP** et **utilisateurs DHCP** sont créés dans **utilisateurs et groupes locaux** sur le serveur DHCP.

```
netsh dhcp add securitygroups
```

La commande suivante redémarre le service DHCP sur l’ordinateur local.

```
Restart-Service dhcpserver
```

Pour plus d’informations sur ces commandes, consultez les rubriques suivantes.

- [Environnement réseau (Netsh)](../netsh/netsh.md)
- [Restart-Service](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/restart-service)

### <a name="authorize-the-dhcp-server-in-active-directory-optional"></a>Autoriser le serveur DHCP dans Active Directory \(facultatif\)

Si vous installez DHCP dans un environnement de domaine, vous devez effectuer les étapes suivantes pour autoriser le serveur DHCP à fonctionner dans le domaine.

> [!NOTE]
> Les serveurs DHCP non autorisés installés dans Active Directory domaines ne peuvent pas fonctionner correctement et n’allouent pas d’adresses IP aux clients DHCP. La désactivation automatique des serveurs DHCP non autorisés est une fonctionnalité de sécurité qui empêche les serveurs DHCP non autorisés d’attribuer des adresses IP incorrectes aux clients sur votre réseau.

Vous pouvez utiliser la commande suivante pour ajouter le serveur DHCP à la liste des serveurs DHCP autorisés dans Active Directory. 

> [!NOTE]
> Si vous n’avez pas d’environnement de domaine, n’exécutez pas cette commande.

```
Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com -IPAddress 10.0.0.3
```

Pour vérifier que le serveur DHCP est autorisé dans Active Directory, vous pouvez utiliser la commande suivante.

```
Get-DhcpServerInDC
```

Voici des exemples de résultats affichés dans Windows PowerShell.

```
IPAddress   DnsName
---------   -------
10.0.0.3    DHCP1.corp.contoso.com
```

Pour plus d’informations sur ces commandes, consultez les rubriques suivantes.

- [Add-DhcpServerInDC](https://docs.microsoft.com/powershell/module/dhcpserver/add-dhcpserverindc)
- [DhcpServerInDC](https://docs.microsoft.com/powershell/module/dhcpserver/get-dhcpserverindc)

### <a name="notify-server-manager-that-post-install-dhcp-configuration-is-complete-optional"></a>Notifiez Gestionnaire de serveur que la publication\-installation de la configuration DHCP est terminée \(\) facultative

Une fois que vous avez terminé la publication\-tâches d’installation, telles que la création de groupes de sécurité et l’autorisation du serveur DHCP dans Active Directory, Gestionnaire de serveur peut toujours afficher une alerte dans l’interface utilisateur, indiquant que les étapes de publication\-doivent être effectuées à l’aide de l’Assistant Configuration de la publication DHCP.

Vous pouvez empêcher cela\-message inutile et incorrect s’affiche dans Gestionnaire de serveur en configurant la clé de Registre suivante à l’aide de cette commande Windows PowerShell.

```
Set-ItemProperty –Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ServerManager\Roles\12 –Name ConfigurationState –Value 2
```

Pour plus d’informations sur cette commande, consultez la rubrique suivante.

- [Set-ItemProperty](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/set-itemproperty)

### <a name="set-server-level-dns-dynamic-update-configuration-settings-optional"></a>Définir les paramètres de configuration de la mise à jour dynamique DNS au niveau du serveur \(facultatif\)

Si vous souhaitez que le serveur DHCP effectue des mises à jour dynamiques DNS pour les ordinateurs clients DHCP, vous pouvez exécuter la commande suivante pour configurer ce paramètre. Il s’agit d’un paramètre de niveau serveur, et non d’un paramètre de niveau d’étendue, qui affecte toutes les étendues que vous configurez sur le serveur. Cet exemple de commande configure également le serveur DHCP pour supprimer les enregistrements de ressources DNS pour les clients lorsque le client expire le moins.

```
Set-DhcpServerv4DnsSetting -ComputerName "DHCP1.corp.contoso.com" -DynamicUpdates "Always" -DeleteDnsRRonLeaseExpiry $True
```

Vous pouvez utiliser la commande suivante pour configurer les informations d’identification utilisées par le serveur DHCP pour inscrire ou désinscrire des enregistrements de clients sur un serveur DNS. Cet exemple enregistre les informations d’identification sur un serveur DHCP. La première commande utilise les **informations d’identification de récupération** pour créer un objet **PSCredential** , puis stocke l’objet dans la variable **$Credential** . La commande vous invite à entrer un nom d’utilisateur et un mot de passe. Veillez donc à fournir les informations d’identification d’un compte qui a l’autorisation de mettre à jour les enregistrements de ressource sur votre serveur DNS.
 
```
$Credential = Get-Credential
Set-DhcpServerDnsCredential -Credential $Credential -ComputerName "DHCP1.corp.contoso.com"
``` 

Pour plus d’informations sur ces commandes, consultez les rubriques suivantes.

- [Set-DhcpServerv4DnsSetting](https://docs.microsoft.com/powershell/module/dhcpserver/set-dhcpserverv4dnssetting)
- [Set-DhcpServerDnsCredential](https://docs.microsoft.com/powershell/module/dhcpserver/set-dhcpserverdnscredential)

### <a name="configure-the-corpnet-scope"></a>Configurer l’étendue du corpnet

Une fois l’installation DHCP terminée, vous pouvez utiliser les commandes suivantes pour configurer et activer l’étendue de l’corpnet, créer une plage d’exclusion pour l’étendue et configurer la passerelle par défaut des options DHCP, l’adresse IP du serveur DNS et le nom de domaine DNS.

```
Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active    
Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15
Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com
Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2
```

Pour plus d’informations sur ces commandes, consultez les rubriques suivantes.

- [Add-DhcpServerv4Scope](https://docs.microsoft.com/powershell/module/dhcpserver/Add-DhcpServerv4Scope)
- [Add-DhcpServerv4ExclusionRange](https://docs.microsoft.com/powershell/module/dhcpserver/Add-DhcpServerv4ExclusionRange)
- [Set-DhcpServerv4OptionValue](https://docs.microsoft.com/powershell/module/dhcpserver/Set-DhcpServerv4OptionValue)

### <a name="configure-the-corpnet2-scope-optional"></a>Configurez l’étendue Corpnet2 \(facultatif\)

Si vous avez un deuxième sous-réseau connecté au premier sous-réseau avec un routeur sur lequel le transfert DHCP est activé, vous pouvez utiliser les commandes suivantes pour ajouter une deuxième étendue, nommée Corpnet2 pour cet exemple. Cet exemple configure également une plage d’exclusion et l’adresse IP de la passerelle par défaut \(l’adresse IP du routeur sur le sous-réseau\) du sous-réseau Corpnet2.

```
Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active
Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15
Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com
```

Si vous disposez de sous-réseaux supplémentaires qui sont pris en service par ce serveur DHCP, vous pouvez répéter ces commandes en utilisant des valeurs différentes pour tous les paramètres de commande, afin d’ajouter des étendues pour chaque sous-réseau.

> [!IMPORTANT]
> Assurez-vous que tous les routeurs entre vos clients DHCP et votre serveur DHCP sont configurés pour le transfert de messages DHCP. Pour plus d’informations sur la configuration du transfert DHCP, consultez la documentation de votre routeur.

## <a name="bkmk_verify"></a>Vérifier la fonctionnalité du serveur

Pour vérifier que votre serveur DHCP fournit une allocation dynamique d’adresses IP aux clients DHCP, vous pouvez connecter un autre ordinateur à un sous-réseau desservi. Une fois que vous avez connecté le câble Ethernet à la carte réseau et que vous mettez l’ordinateur sous tension, il demande une adresse IP à partir de votre serveur DHCP. Vous pouvez vérifier que la configuration a réussi à l’aide de la commande **ipconfig/all** et en examinant les résultats, ou en effectuant des tests de connectivité, par exemple en tentant d’accéder à des ressources Web avec votre navigateur ou vos partages de fichiers avec l’Explorateur Windows ou d’autres applications.

Si le client ne reçoit pas d’adresse IP de votre serveur DHCP, effectuez les étapes de dépannage suivantes.

1. Assurez-vous que le câble Ethernet est branché à la fois sur l’ordinateur et sur le commutateur Ethernet, le concentrateur ou le routeur.
2. Si vous avez connecté l’ordinateur client à un segment de réseau qui est séparé du serveur DHCP par un routeur, assurez-vous que le routeur est configuré pour transférer les messages DHCP.
3. Assurez-vous que le serveur DHCP est autorisé dans Active Directory en exécutant la commande suivante pour récupérer la liste des serveurs DHCP autorisés à partir de Active Directory. [Accédez à DhcpServerInDC](https://docs.microsoft.com/powershell/module/dhcpserver/Get-DhcpServerInDC).
4. Assurez-vous que vos étendues sont activées en ouvrant la console DHCP \(Gestionnaire de serveur, **Outils**, **DHCP**\), en développant l’arborescence du serveur pour examiner les étendues, puis\-cliquez avec le bouton droit sur chaque étendue. Si le menu qui s’affiche comprend la sélection **activer**, cliquez sur **activer**. \(si l’étendue est déjà activée, la sélection de menu lit **Désactiver**.\)

## <a name="bkmk_dhcpwps"></a>Commandes Windows PowerShell pour DHCP

La référence suivante fournit des descriptions et une syntaxe de commande pour toutes les commandes Windows PowerShell de serveur DHCP pour Windows Server 2016. La rubrique répertorie les commandes dans l’ordre alphabétique en fonction du verbe situé au début des commandes, telles que l' **extraction** ou la **définition**.

> [!NOTE]
> Vous ne pouvez pas utiliser les commandes de Windows Server 2016 dans Windows Server 2012 R2.

- [Module DhcpServer](https://docs.microsoft.com/powershell/module/dhcpserver/)

La référence suivante fournit les descriptions et la syntaxe des commandes pour toutes les commandes Windows PowerShell du serveur DHCP pour Windows Server 2012 R2. La rubrique répertorie les commandes dans l’ordre alphabétique en fonction du verbe situé au début des commandes, telles que l' **extraction** ou la **définition**.

> [!NOTE]
> Vous pouvez utiliser les commandes Windows Server 2012 R2 dans Windows Server 2016.

- [Applets de commande du serveur DHCP dans Windows PowerShell](https://docs.microsoft.com/windows-server/networking/technologies/dhcp/dhcp-deploy-wps)

## <a name="bkmk_list"></a>Liste des commandes Windows PowerShell dans ce guide

Voici une liste simple de commandes et d’exemples de valeurs qui sont utilisés dans ce guide.

```
New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2
Rename-Computer -Name DHCP1
Restart-Computer

Add-Computer CORP
Restart-Computer

Install-WindowsFeature DHCP -IncludeManagementTools
netsh dhcp add securitygroups
Restart-Service dhcpserver

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
```
