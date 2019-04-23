---
title: Déploiement de DHCP à l'aide de Windows PowerShell
description: Vous pouvez utiliser cette rubrique pour déployer un serveur DHCP Windows Server 2016 IP (Internet Protocol) version 4 fournit automatique des adresses IP et des options DHCP aux clients d’IPv4 DHCP connectés à un ou plusieurs sous-réseaux sur votre réseau.
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 7110ad21-a33e-48d5-bb3c-129982913bc8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8a53f6293067fa0f7014e7794696cf75f7179545
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849090"
---
# <a name="deploy-dhcp-using-windows-powershell"></a>Déploiement de DHCP à l'aide de Windows PowerShell

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Ce guide fournit des instructions sur l’utilisation de Windows PowerShell pour déployer une IP (Internet Protocol) version 4 Dynamic Host Configuration Protocol \(DHCP\) options de serveur qui attribue automatiquement des adresses IP et DHCP pour IPv4 DHCP clients qui sont connectés à un ou plusieurs sous-réseaux sur votre réseau.

>[!NOTE]
>Pour télécharger ce document au format Word à partir de la Galerie TechNet, consultez [déployer DHCP à l’aide de Windows PowerShell dans Windows Server 2016](https://gallery.technet.microsoft.com/Deploy-DHCP-Using-Windows-246dd293).

À l’aide de serveurs DHCP pour attribuer les IP adresses enregistre dans la surcharge administrative, car vous n’avez pas besoin de le configurer manuellement les paramètres de v4 de TCP/IP pour chaque carte réseau sur chaque ordinateur sur votre réseau. Avec DHCP, une configuration TCP/IP v4 est effectuée automatiquement lorsqu’un ordinateur ou autre client DHCP est connecté à votre réseau.

Vous pouvez déployer votre serveur DHCP dans un groupe de travail comme un serveur autonome ou en tant que partie d’un domaine Active Directory.

Ce guide contient les sections suivantes.

- [Vue d’ensemble du déploiement de DHCP](#bkmk_overview)
- [Présentations des technologies](#bkmk_technologies)
- [Planifier le déploiement de DHCP](#bkmk_plan)
- [À l’aide de ce Guide dans un laboratoire de Test](#bkmk_lab)
- [Déployer DHCP](#bkmk_deploy)
- [Vérifier la fonctionnalité de serveur](#bkmk_verify)
- [Commandes PowerShell de Windows pour DHCP](#bkmk_dhcpwps)
- [Liste des commandes de Windows PowerShell dans ce guide](#bkmk_list)

## <a name="bkmk_overview"></a>Vue d’ensemble du déploiement de DHCP

L’illustration suivante représente le scénario que vous pouvez déployer à l’aide de ce guide. Le scénario inclut un serveur DHCP dans un domaine Active Directory. Le serveur est configuré pour fournir des adresses IP aux clients DHCP sur deux sous-réseaux différents. Les sous-réseaux sont séparés par un routeur avec le transfert de DHCP est activé.

![Vue d’ensemble de topologie de réseau DHCP](../../media/Core-Network-Guide/cng16_overview.jpg)

## <a name="bkmk_technologies"></a>Présentations des technologies

Les sections suivantes fournissent de brèves vues d’ensemble des protocoles DHCP et TCP/IP.

### <a name="dhcp-overview"></a>Vue d’ensemble DHCP

DHCP est une norme IP permettant de simplifier la gestion de la configuration IP de l’hôte. Cette norme permet d’utiliser des serveurs DHCP comme moyen de gérer l’allocation dynamique d’adresses IP et d’autres informations de configuration pour les clients DHCP sur votre réseau.

DHCP vous permet d’utiliser un serveur DHCP pour attribuer dynamiquement une adresse IP à un ordinateur ou un autre appareil, tel qu’une imprimante, sur votre réseau local, plutôt que de configurer manuellement chaque appareil avec une adresse IP statique.

Chaque ordinateur d’un réseau TCP/IP doit avoir une adresse IP unique, car cette adresse et son masque de sous-réseau associé sont utilisés pour identifier l’ordinateur hôte et le sous-réseau auquel l’ordinateur est joint. Avec le protocole DHCP, vous avez l’assurance que tous les ordinateurs configurés en tant que clients DHCP reçoivent une adresse IP correspondant à leur emplacement réseau et leur sous-réseau. De plus, en utilisant les options DHCP, telles qu’une passerelle par défaut et des serveurs DNS, vous fournissez automatiquement aux clients DHCP les informations dont ils ont besoin pour fonctionner correctement sur votre réseau.

Pour les réseaux basés sur IP, DHCP réduit la complexité et la quantité de travail d’administration impliquée dans la configuration des ordinateurs.

### <a name="tcpip-overview"></a>Vue d’ensemble de TCP/IP

Par défaut, toutes les versions de systèmes d’exploitation Windows Server et Windows Client ont des paramètres TCP/IP de IP version 4 connexions configurées pour obtenir automatiquement une adresse IP et autres informations, appelés options DHCP, à partir d’un serveur DHCP. Pour cette raison, vous n’avez pas besoin de configurer manuellement les paramètres TCP/IP, sauf si l’ordinateur est un ordinateur de serveur ou un autre périphérique nécessitant une adresse IP statique configurée manuellement. 

Par exemple, il est recommandé de configurer manuellement l’adresse IP du serveur DHCP et les adresses IP des serveurs DNS et des contrôleurs de domaine qui exécutent les Services de domaine Active Directory \(AD DS\).

TCP/IP dans Windows Server 2016 est la suivante :

-   un logiciel réseau reposant sur des protocoles réseau standard ;

-   un protocole réseau d’entreprise routable qui prend en charge la connexion de votre ordinateur Windows aux environnements de réseau local (LAN, Local Area Network) et de réseau étendu (WAN, Wide Area Network) ;

-   un ensemble de technologies et d’utilitaires centraux permettant de connecter votre ordinateur Windows à des systèmes hétérogènes pour le partage d’informations ;

-   Une base permettant d’accéder à des services Internet globaux, tels que les serveurs Web et transfert de protocole FTP (File).

-   une infrastructure client/serveur interplateforme, évolutive et robuste.

TCP/IP propose des utilitaires TCP/IP de base qui permettent aux ordinateurs Windows de se connecter à d’autres systèmes d’exploitation Microsoft et non-Microsoft pour partager des informations, notamment :

- Windows Server 2016

- Windows 10

- Windows Server 2012 R2

- Windows 8.1

- Windows Server 2012

- Windows 8

- Windows Server 2008 R2

- Windows 7

- Windows Server 2008

- Windows Vista

- Hôtes Internet

- Systèmes Apple Macintosh

- Gros systèmes IBM

- Systèmes UNIX et Linux

- Systèmes Open VMS

- Imprimantes réseau

- Tablettes et téléphones cellulaires avec Ethernet câblé ou technologie 802.11 sans fil activée

## <a name="bkmk_plan"></a>Planifier le déploiement de DHCP

Voici les principales étapes de planification avant d’installer le rôle serveur DHCP.

### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>Planification des serveurs DHCP et de l’acheminement DHCP

Les messages DHCP étant des messages de diffusion, ils ne sont pas acheminés entre les sous-réseaux par les routeurs. Si vous avez plusieurs sous-réseaux et souhaitez proposer le service DHCP sur chacun d’eux, vous devez procéder comme suit :

-   Installez un serveur DHCP sur chaque sous-réseau.

-   Configurez les routeurs pour qu’ils acheminent les messages de diffusion DHCP dans les sous-réseaux et configurez plusieurs étendues sur le serveur DHCP, à raison d’une étendue par sous-réseau.

En règle générale, il est économiquement plus avantageux de configurer des routeurs pour qu’ils acheminent les messages de diffusion DHCP plutôt que de déployer un serveur DHCP sur chaque segment physique du réseau.

### <a name="planning-ip-address-ranges"></a>Planification de plages d’adresses IP

Chaque sous-réseau doit avoir sa propre plage d’adresses IP unique. Ces plages sont représentées sur un serveur DHCP avec des étendues.

Une étendue est un groupement administratif des adresses IP des ordinateurs d’un sous-réseau qui utilisent le service DHCP. L’administrateur crée d’abord une étendue pour chaque sous-réseau physique, puis utilise l’étendue pour définir les paramètres utilisés par les clients.

Une étendue possède les propriétés suivantes :

-   une plage d’adresses IP où inclure ou exclure les adresses utilisées pour les offres de bail de service DHCP ;

-   un masque de sous-réseau, qui détermine le préfixe de sous-réseau correspondant à une adresse IP donnée ;

-   un nom affecté à l’étendue lors de sa création ;

-   des valeurs de durée de bail, qui sont affectées aux clients DHCP recevant des adresses IP allouées de manière dynamique ;

-   des options d’étendue DHCP configurées pour être affectées aux clients DHCP, telles que l’adresse IP d’un serveur DNS et l’adresse IP d’un routeur/d’une passerelle par défaut ;

-   des réservations, utilisées de manière optionnelle pour s’assurer qu’un client DHCP reçoit toujours la même adresse IP.

Avant de déployer vos serveurs, dressez la liste de vos sous-réseaux et des plages d’adresses IP à utiliser pour chaque sous-réseau.

### <a name="planning-subnet-masks"></a>Planification des masques de sous-réseaux

Au sein d’une adresse IP, les ID de réseau et les ID d’hôte sont différenciés à l’aide d’un masque de sous-réseau. Chaque masque de sous-réseau est un nombre de 32 bits qui utilise des groupes de bits consécutifs composés uniquement de 1 pour identifier l’ID de réseau et composés uniquement de 0 pour identifier les parties de l’adresse IP relatives à l’ID d’hôte.

Par exemple, un masque de sous-réseau normalement utilisé avec l’adresse IP 131.107.16.200 correspond au nombre binaire de 32 bits suivant :

```
11111111 11111111 00000000 00000000
```

Ce numéro de masque de sous-réseau est 16 bits suivies de 16 bits de valeur zéro, indiquant que les sections réseau les ID ID et l’hôte de cette adresse IP sont les deux une longueur de 16 bits. Ce masque de sous-réseau est affiché en notation décimale séparée par des points sous la forme 255.255.0.0.

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

Vous pouvez éviter ce problème en créant une plage d’exclusion pour l’étendue DHCP. Une plage d’exclusion est une contiguës plage d’adresses IP au sein de la plage d’adresses IP de l’étendue que le serveur DHCP n’est pas autorisé à utiliser. Si vous créez une plage d’exclusion, le serveur DHCP ne peut attribuer aucune des adresses de cette plage. Vous pouvez alors attribuer manuellement ces adresses sans risque de générer un conflit d’adresse IP.

Vous pouvez exclure des adresses IP de la distribution par le serveur DHCP en créant une plage d’exclusion pour chaque étendue. Vous devez utiliser des exclusions pour tous les périphériques configurés avec une adresse IP statique. Dans les adresses exclues doivent figurer toutes les adresses IP que vous avez attribuées manuellement à d’autres serveurs DHCP, clients non DHCP, stations de travail sans disque ou clients de routage et d’accès à distance et clients PPP.

Il est recommandé de configurer votre plage d’exclusion avec des adresses supplémentaires afin de prendre en compte la croissance future du réseau. Le tableau suivant fournit un exemple de plage d’exclusion pour une étendue avec une plage d’adresses IP de 10.0.0.1 - 10.0.0.254 et un masque de sous-réseau 255.255.255.0.

|Éléments de configuration|Exemples de valeurs|
|-----------------------|------------------|
|Adresse IP de début de la plage d’exclusion|10.0.0.1|
|Adresse IP de fin de la plage d’exclusion|10.0.0.25|

### <a name="planning-tcpip-static-configuration"></a>Planification de la configuration TCP/IP statique

Certains périphériques, comme les routeurs, les serveurs DHCP et les serveurs DNS doivent être configurés avec une adresse IP statique. Par ailleurs, vous voudrez peut-être garantir que des périphériques supplémentaires, comme des imprimantes, disposent toujours de la même adresse IP. Dressez la liste des périphériques que vous souhaitez configurer de manière statique pour chaque sous-réseau, puis planifiez la plage d’exclusion que vous voulez utiliser sur le serveur DHCP pour faire en sorte que le serveur DHCP ne loue pas l’adresse IP d’un périphérique configuré de manière statique. Une plage d’exclusion est une séquence limitée d’adresses IP au sein d’une étendue, qui est exclue des offres du service DHCP. Les plages d’exclusion garantissent qu’aucune adresse figurant dans ces plages n’est offerte par le serveur aux clients DHCP sur votre réseau.

Par exemple, si la plage d’adresses IP d’un sous-réseau est 192.168.0.1 à 192.168.0.254 et que vous souhaitez configurer dix périphériques avec une adresse IP statique, vous pouvez créer une plage d’exclusion pour l’étendue 192.168.0.*x* qui inclut dix adresses IP ou plus : 192.168.0.1 à 192.168.0.15.

Dans cet exemple, vous utilisez dix des adresses IP exclues pour configurer des serveurs et autres périphériques avec des adresses IP statiques. Il reste cinq adresses IP pour configurer de manière statique de nouveaux périphériques que vous voudrez peut-être ajouter plus tard. Avec cette plage d’exclusion, le serveur DHCP dispose d’un pool d’adresses allant de 192.168.0.16 à 192.168.0.254.

Exemples supplémentaires d’éléments de configuration pour les services AD DS et DNS sont fournis dans le tableau suivant.

|Éléments de configuration|Exemples de valeurs|
|-----------------------|------------------|
|Liaisons de connexion réseau|Ethernet|
|Paramètres du serveur DNS|DC1.corp.contoso.com|
|Adresse IP du serveur DNS préféré|10.0.0.2|
|Valeurs d’étendue<br /><br />1.  Nom de l'étendue<br />2.  Adresse IP de début<br />3.  Adresse IP de fin<br />4.  Masque de sous-réseau<br />5.  Passerelle par défaut (facultatif)<br />6.  Durée du bail|1.  Sous-réseau principal<br />2.  10.0.0.1<br />3.  10.0.0.254<br />4.  255.255.255.0<br />5.  10.0.0.1<br />6. 8 jours|
|Mode d’opération du serveur DHCP IPv6|Non activé|

## <a name="bkmk_lab"></a>À l’aide de ce Guide dans un laboratoire de Test

Vous pouvez utiliser ce guide pour déployer DHCP dans un laboratoire de test avant de déployer dans un environnement de production. 

>[!NOTE]
>Si vous ne souhaitez pas déployer DHCP dans un laboratoire de test, vous pouvez passer à la section [DHCP déployer](#bkmk_deploy).

La configuration requise pour votre laboratoire diffère selon que vous utilisez des serveurs physiques ou virtuels \(machines virtuelles\), et que vous à l’aide d’un domaine Active Directory ou que vous déployez un serveur DHCP d’autonome. 

Vous pouvez utiliser les informations suivantes pour déterminer les ressources minimales, que vous devez tester le déploiement de DHCP à l’aide de ce guide.

### <a name="test-lab-requirements-with-vms"></a>Configuration requise du laboratoire de test avec des machines virtuelles

Pour déployer DHCP dans un laboratoire de test avec des machines virtuelles, vous devez les ressources suivantes.

Pour le déploiement dans un domaine ou de déploiement autonome, vous avez besoin d’un serveur qui est configuré comme un Hyper\-hôte V.

**Déploiement de domaine**

Ce déploiement nécessite un seul serveur physique, un seul commutateur virtuel, deux serveurs virtuels et un seul client virtuel :

Sur votre serveur physique, dans le Gestionnaire Hyper-V, créez les éléments suivants.

1. Un **interne** commutateur virtuel. Ne créez pas un **externe** commutateur virtuel, car si votre Hyper\-hôte V se trouve sur un sous-réseau qui inclut un serveur DHCP, vos machines virtuelles de test reçoit une adresse IP de votre serveur DHCP. En outre, le serveur DHCP de test que vous déployez attribue les adresses IP à d’autres ordinateurs sur le sous-réseau où Hyper\-hôte V est installé.
1. Un ordinateur virtuel exécutant Windows Server 2016 est configuré en tant que contrôleur de domaine avec les Services de domaine Active Directory qui est connectée au commutateur virtuel interne que vous avez créé. Pour faire correspondre ce guide, ce serveur doit avoir une adresse IP statique de 10.0.0.2. Pour plus d’informations sur le déploiement d’AD DS, consultez la section **DC1 déploiement** dans Windows Server 2016 [Guide du réseau](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01).
1. Une machine virtuelle exécutant Windows Server 2016, vous allez configurer comme un serveur DHCP à l’aide de ce guide et qui est connecté à l’interne virtuel commutateur que vous avez créé. 
1. Un ordinateur virtuel exécutant un système d’exploitation Windows client est connecté à l’interne virtuel commutateur que vous avez créé et que vous allez utiliser pour vérifier que votre serveur DHCP est affecter dynamiquement des adresses IP et des options DHCP aux clients DHCP.

**Déploiement de serveur Standalone DHCP**

Ce déploiement nécessite un seul serveur physique, un seul commutateur virtuel, un serveur virtuel et un seul client virtuel :

Sur votre serveur physique, dans le Gestionnaire Hyper-V, créez les éléments suivants.

1. Un **interne** commutateur virtuel. Ne créez pas un **externe** commutateur virtuel, car si votre Hyper\-hôte V se trouve sur un sous-réseau qui inclut un serveur DHCP, vos machines virtuelles de test reçoit une adresse IP de votre serveur DHCP. En outre, le serveur DHCP de test que vous déployez attribue les adresses IP à d’autres ordinateurs sur le sous-réseau où Hyper\-hôte V est installé.
1. Une machine virtuelle exécutant Windows Server 2016, vous allez configurer comme un serveur DHCP à l’aide de ce guide et qui est connecté à l’interne virtuel commutateur que vous avez créé.
1. Un ordinateur virtuel exécutant un système d’exploitation Windows client est connecté à l’interne virtuel commutateur que vous avez créé et que vous allez utiliser pour vérifier que votre serveur DHCP est affecter dynamiquement des adresses IP et des options DHCP aux clients DHCP.

### <a name="test-lab-requirements-with-physical-servers"></a>Configuration requise du laboratoire de test avec des serveurs physiques

Pour déployer DHCP dans un laboratoire de test avec des serveurs physiques, vous devez les ressources suivantes.

**Déploiement de domaine**

Ce déploiement nécessite un hub ou commutateur, deux serveurs physiques et un client physique :

1. Un concentrateur ou commutateur Ethernet auquel vous pouvez vous connecter les ordinateurs physiques avec des câbles Ethernet
1. Un ordinateur physique exécutant Windows Server 2016 est configuré comme contrôleur de domaine avec les Services de domaine Active Directory. Pour faire correspondre ce guide, ce serveur doit avoir une adresse IP statique de 10.0.0.2. Pour plus d’informations sur le déploiement d’AD DS, consultez la section **DC1 déploiement** dans Windows Server 2016 [Guide du réseau](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01).
1. Un ordinateur physique exécutant Windows Server 2016, vous allez configurer comme un serveur DHCP à l’aide de ce guide. 
1. Un ordinateur physique exécutant un système d’exploitation de client Windows que vous allez utiliser pour vérifier que votre serveur DHCP est dynamiquement l’allocation des adresses IP et des options DHCP aux clients DHCP.

>[!NOTE]
>Si vous n’avez pas suffisamment d’ordinateurs test pour ce déploiement, vous pouvez utiliser un ordinateur de test pour les services AD DS et DHCP - même si cette configuration n’est pas recommandée pour un environnement de production.

**Déploiement de serveur Standalone DHCP**

Ce déploiement nécessite un hub ou commutateur, un seul serveur physique et un client physique :

1. Un concentrateur ou commutateur Ethernet auquel vous pouvez vous connecter les ordinateurs physiques avec des câbles Ethernet
2. Un ordinateur physique exécutant Windows Server 2016, vous allez configurer comme un serveur DHCP à l’aide de ce guide. 
3. Un ordinateur physique exécutant un système d’exploitation de client Windows que vous allez utiliser pour vérifier que votre serveur DHCP est dynamiquement l’allocation des adresses IP et des options DHCP aux clients DHCP.


## <a name="bkmk_deploy"></a>Déployer DHCP

Cette section fournit des exemples de commandes Windows PowerShell que vous pouvez utiliser pour déployer DHCP sur un seul serveur. Avant d’exécuter ces exemples de commandes sur votre serveur, vous devez modifier les commandes pour correspondre à votre réseau et votre environnement. 

Par exemple, avant d’exécuter les commandes, vous devez remplacer les exemples de valeurs dans les commandes pour les éléments suivants :

- Noms d’ordinateurs
- Plage d’adresses IP pour chaque étendue que vous souhaitez configurer (1 étendue par sous-réseau)
- Masque de sous-réseau pour chaque plage d’adresses IP à configurer
- Nom de l’étendue pour chaque étendue
- Plage d’exclusion pour chaque étendue
- Valeurs d’option DHCP, comme passerelle par défaut, nom de domaine et serveurs DNS ou WINS
- Noms d’interface

>[!IMPORTANT]
>Examiner et modifier toutes les commandes pour votre environnement avant d’exécuter la commande.

### <a name="where-to-install-dhcp---on-a-physical-computer-or-a-vm"></a>Où installer DHCP - sur un ordinateur physique ou une machine virtuelle ?

Vous pouvez installer le rôle de serveur DHCP sur un ordinateur physique ou sur une machine virtuelle \(machine virtuelle\) qui est installé sur un Hyper\-hôte V. Si vous installez DHCP sur une machine virtuelle et que vous souhaitez que le serveur DHCP pour fournir des affectations d’adresses IP aux ordinateurs sur le réseau physique à laquelle l’hôte Hyper-V est connecté, vous devez vous connecter la carte réseau virtuelle de machine virtuelle à un commutateur virtuel Hyper-V qui est **Externe**.

Pour plus d’informations, consultez la section **créer un commutateur virtuel avec le Gestionnaire Hyper-V** dans la rubrique [créer un réseau virtuel](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/connect-to-network).

### <a name="run-windows-powershell-as-an-administrator"></a>Exécuter Windows PowerShell en tant qu’administrateur

Vous pouvez utiliser la procédure suivante pour exécuter Windows PowerShell avec des privilèges d’administrateur.

1. Sur un ordinateur exécutant Windows Server 2016, cliquez sur **Démarrer**, puis cliquez sur l’icône Windows PowerShell. Un menu s’affiche. 

2. Dans le menu, cliquez sur **plus**, puis cliquez sur **exécuter en tant qu’administrateur**. Si vous y êtes invité, tapez les informations d’identification pour un compte disposant des privilèges d’administrateur sur l’ordinateur. Si le compte d’utilisateur avec lequel vous êtes connecté à l’ordinateur est un compte de niveau administrateur, vous ne recevrez pas une invite d’informations d’identification.

3. Windows PowerShell s’ouvre avec des privilèges d’administrateur.

### <a name="rename-the-dhcp-server-and-configure-a-static-ip-address"></a>Renommer le serveur DHCP et de configurer une adresse IP statique

Si vous ne le n'avez pas déjà fait, vous pouvez utiliser les commandes Windows PowerShell suivantes pour renommer le serveur DHCP et de configurer une adresse IP statique pour le serveur.

**Configurer une adresse IP statique**

Vous pouvez utiliser les commandes suivantes pour affecter une adresse IP statique pour le serveur DHCP et de configurer les propriétés TCP/IP du serveur DHCP avec l’adresse IP de serveur DNS correct. Remplacez les noms d’interface et les adresses IP utilisés dans cet exemple par les valeurs de configuration souhaitées pour votre ordinateur.

 `New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24`

 `Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2`

Pour plus d’informations sur ces commandes, consultez les rubriques suivantes.

- [New-NetIPAddress](https://technet.microsoft.com/itpro/powershell/windows/tcpip/new-netipaddress)
- [Set-DnsClientServerAddress](https://technet.microsoft.com/itpro/powershell/windows/dns-client/set-dnsclientserveraddress)

**Renommer l’ordinateur**

Vous pouvez utiliser les commandes suivantes pour renommer, puis redémarrez l’ordinateur.

`Rename-Computer -Name DHCP1`

 `Restart-Computer`

Pour plus d’informations sur ces commandes, consultez les rubriques suivantes.

- [Rename-Computer](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/rename-computer)
- [Restart-Computer](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/restart-computer)

### <a name="join-the-computer-to-the-domain-optional"></a>Joindre l’ordinateur au domaine \(facultatif\)

Si vous installez votre serveur DHCP dans un environnement de domaine Active Directory, vous devez joindre l’ordinateur au domaine. Ouvrez Windows PowerShell avec des privilèges d’administrateur, puis exécutez la commande suivante après avoir remplacé le nom NetBios du domaine **CORP** avec une valeur qui est appropriée pour votre environnement.

    Add-Computer CORP

Lorsque vous y êtes invité, tapez les informations d’identification pour un compte d’utilisateur de domaine qui a l’autorisation de joindre un ordinateur au domaine. 

    Restart-Computer

Pour plus d’informations sur la commande Add-Computer, consultez la rubrique suivante.

- [Add-Computer](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/add-computer)

### <a name="install-dhcp"></a>Installer DHCP

Une fois l’ordinateur redémarré, ouvrez Windows PowerShell avec des privilèges d’administrateur et puis installer DHCP en exécutant la commande suivante.

    Install-WindowsFeature DHCP -IncludeManagementTools

Pour plus d’informations sur cette commande, consultez la rubrique suivante.

- [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature)

### <a name="create-dhcp-security-groups"></a>Créer des groupes de sécurité DHCP

Pour créer des groupes de sécurité, vous devez exécuter un interpréteur de commandes réseau \(netsh\) commande dans Windows PowerShell et redémarrez le service DHCP afin que les nouveaux groupes deviennent actifs.

Lorsque vous exécutez la commande netsh suivante sur le serveur DHCP, le **Administrateurs DHCP** et **utilisateurs DHCP** groupes de sécurité sont créés dans **utilisateurs et groupes locaux** sur le protocole DHCP serveur.

    netsh dhcp add securitygroups

La commande suivante redémarre le service DHCP sur l’ordinateur local.

    Restart-service dhcpserver

Pour plus d’informations sur ces commandes, consultez les rubriques suivantes.

- [Network Shell (Netsh)](../netsh/netsh.md)
- [Restart-Service](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/restart-service)

### <a name="authorize-the-dhcp-server-in-active-directory-optional"></a>Autoriser le serveur DHCP dans Active Directory \(facultatif\)

Si vous installez DHCP dans un environnement de domaine, vous devez effectuer les étapes suivantes pour autoriser le serveur DHCP pour fonctionner dans le domaine.

>[!NOTE]
>Les serveurs DHCP non autorisés qui sont installés dans des domaines Active Directory ne peut pas fonctionner correctement et ne pas louer des adresses IP aux clients DHCP. La désactivation automatique de serveurs DHCP non autorisés est une fonctionnalité de sécurité qui empêche les serveurs DHCP non autorisés d’affectation des adresses IP incorrectes aux clients sur votre réseau.

Vous pouvez utiliser la commande suivante pour ajouter le serveur DHCP à la liste des serveurs DHCP autorisés dans Active Directory. 

>[!NOTE]
>Si vous n’avez pas d’un environnement de domaine, n’exécutez pas cette commande.

 `Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com -IPAddress 10.0.0.3`

Pour vérifier que le serveur DHCP est autorisé dans Active Directory, vous pouvez utiliser la commande suivante.

    Get-DhcpServerInDC

Voici des exemples de résultats qui sont affichés dans Windows PowerShell.

    
        IPAddress   DnsName
        ---------   -------
        10.0.0.3    DHCP1.corp.contoso.com
    

Pour plus d’informations sur ces commandes, consultez les rubriques suivantes.

- [Add-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverindc)
- [Get-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc)

### <a name="notify-server-manager-that-post-install-dhcp-configuration-is-complete-optional"></a>Notifier le Gestionnaire de serveur à valider\-installer DHCP configuration est terminée \(facultatif\)

Une fois que vous avez terminé de post\-tâches d’installation, telles que la création de groupes de sécurité et d’autoriser un serveur DHCP dans Active Directory, le Gestionnaire de serveur peuvent toujours afficher une alerte dans l’interface utilisateur indiquant que ce post\- étapes d’installation doivent être effectuées à l’aide de l’Assistant Configuration post-installation DHCP.

Vous pouvez éviter cela maintenant\-message inutile et inexacte d’apparaître dans le Gestionnaire de serveur en configurant la clé de Registre suivante à l’aide de cette commande Windows PowerShell.

    Set-ItemProperty –Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ServerManager\Roles\12 –Name ConfigurationState –Value 2

Pour plus d’informations sur cette commande, consultez la rubrique suivante.

- [Set-ItemProperty](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/set-itemproperty?f=255&MSPPError=-2147217396)

### <a name="set-server-level-dns-dynamic-update-configuration-settings-optional"></a>Définir des paramètres de configuration de serveur au niveau DNS mise à jour dynamique \(facultatif\)

Si vous souhaitez que le serveur DHCP pour effectuer des mises à jour dynamiques DNS pour les ordinateurs clients DHCP, vous pouvez exécuter la commande suivante pour configurer ce paramètre. Il s’agit d’un niveau de serveur que paramètre, ne pas une étendue au niveau définissant, afin que cela affecte toutes les étendues que vous configurez sur le serveur. Cet exemple de commande configure également le serveur DHCP pour supprimer des enregistrements de ressource DNS pour les clients lorsque le client moins arrive à expiration.

    Set-DhcpServerv4DnsSetting -ComputerName "DHCP1.corp.contoso.com" -DynamicUpdates "Always" -DeleteDnsRRonLeaseExpiry $True

Vous pouvez utiliser la commande suivante pour configurer les informations d’identification par le serveur DHCP pour inscrire ou désinscrire des enregistrements de clients sur un serveur DNS. Cet exemple enregistre les informations d’identification sur un serveur DHCP. La première commande utilise **Get-Credential** pour créer un **PSCredential** de l’objet, puis stocke l’objet dans le **$Credential** variable. La commande vous invite pour le nom d’utilisateur et mot de passe, assurez-vous que vous fournissiez des informations d’identification d’un compte qui a l’autorisation de mettre à jour des enregistrements de ressources sur votre serveur DNS.

    
    $Credential = Get-Credential
    Set-DhcpServerDnsCredential -Credential $Credential -ComputerName "DHCP1.corp.contoso.com"
    

Pour plus d’informations sur ces commandes, consultez les rubriques suivantes.

- [Set-DhcpServerv4DnsSetting](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4dnssetting)
- [Set-DhcpServerDnsCredential](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverdnscredential)

### <a name="configure-the-corpnet-scope"></a>Configurer l’étendue du réseau d’entreprise

Une fois l’installation de DHCP est terminée, vous pouvez utiliser les commandes suivantes pour configurer et activer l’étendue du réseau d’entreprise, créer une plage d’exclusion pour l’étendue et configurer la passerelle par défaut d’options DHCP, une adresse IP du serveur DNS et un nom de domaine DNS.

    
    Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active`
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15`
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com`
    
    Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2
    

Pour plus d’informations sur ces commandes, consultez les rubriques suivantes.

- [Add-DhcpServerv4Scope](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4scope)
- [Add-DhcpServerv4ExclusionRange](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4exclusionrange)
- [Set-DhcpServerv4OptionValue](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4optionvalue)

### <a name="configure-the-corpnet2-scope-optional"></a>Configurer l’étendue Corpnet2 \(facultatif\)

Si vous avez un deuxième sous-réseau est connecté au premier sous-réseau avec un routeur où l’acheminement DHCP est activé, vous pouvez utiliser les commandes suivantes pour ajouter une seconde étendue, nommée Corpnet2 pour cet exemple. Cet exemple configure également une plage d’exclusion et l’adresse IP de la passerelle par défaut \(l’adresse IP du routeur sur le sous-réseau\) du sous-réseau Corpnet2.

 `Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active`

 `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15`

 `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com`

Si vous avez des sous-réseaux supplémentaires qui sont pris en charge par ce serveur DHCP, vous pouvez répéter ces commandes, à l’aide des valeurs différentes pour tous les paramètres de commande pour ajouter des étendues pour chaque sous-réseau.

>[!IMPORTANT]
>Vérifiez que tous les routeurs entre vos clients DHCP et de votre serveur DHCP sont configurés pour le transfert des messages DHCP. Consultez la documentation de votre routeur pour plus d’informations sur la façon de configurer le transfert de DHCP.

## <a name="bkmk_verify"></a>Vérifier la fonctionnalité de serveur

Pour vérifier que votre serveur DHCP fournit une allocation dynamique d’adresses IP aux clients DHCP, vous pouvez vous connecter à un autre ordinateur à un sous-réseau pris en charge. Une fois que vous vous connectez le câble Ethernet pour la carte réseau et la mise sous tension l’ordinateur, il demande une adresse IP de votre serveur DHCP. Vous pouvez vérifier la configuration réussie à l’aide de la **ipconfig/all** commande et examiné les résultats ou en effectuant des tests de connectivité, tel que tente d’accéder aux ressources Web avec vos partages de fichier ou navigateur avec Windows Explorateur ou autres applications.

Si le client ne reçoit pas une adresse IP de votre serveur DHCP, effectuez les étapes de dépannage suivantes.

1. Assurez-vous que le câble Ethernet est branché à la fois l’ordinateur et le Ethernet commutateur, concentrateur ou routeur.
1. Si l’ordinateur client branché un segment de réseau du serveur DHCP est séparé par un routeur, assurez-vous que le routeur est configuré pour transférer les messages DHCP.
1. Assurez-vous que le serveur DHCP est autorisé dans Active Directory en exécutant la commande suivante pour récupérer la liste des serveurs DHCP autorisés dans Active Directory. [Get-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc).
1. Assurez-vous que vos étendues sont activés en ouvrant la console DHCP \(Gestionnaire de serveur, **outils**, **DHCP**\), développez l’arborescence du serveur pour examiner les étendues, puis à droite\-en cliquant sur chaque étendue. Si le menu résultant inclut la sélection **activer**, cliquez sur **activer**. \(Si l’étendue est déjà activé, la sélection du menu lit **désactiver**.\) 

## <a name="bkmk_dhcpwps"></a>Commandes PowerShell de Windows pour DHCP

La référence suivante fournit des descriptions des commandes et la syntaxe pour toutes les commandes de DHCP Server Windows PowerShell pour Windows Server 2016. La rubrique répertorie les commandes dans l’ordre alphabétique en fonction du verbe au début des commandes, telles que **obtenir** ou **définir**.

>[!NOTE]
>Vous ne pouvez pas utiliser les commandes de Windows Server 2016 dans Windows Server 2012 R2.

- [Module DhcpServer](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/index)

La référence suivante fournit des descriptions des commandes et la syntaxe pour toutes les commandes de DHCP Server Windows PowerShell pour Windows Server 2012 R2. La rubrique répertorie les commandes dans l’ordre alphabétique en fonction du verbe au début des commandes, telles que **obtenir** ou **définir**.

>[!NOTE]
>Vous pouvez utiliser les commandes de Windows Server 2012 R2 dans Windows Server 2016.

- [Les applets de commande de serveur DHCP dans Windows PowerShell](https://technet.microsoft.com/library/jj590751.aspx)

## <a name="bkmk_list"></a>Liste des commandes de Windows PowerShell dans ce guide

Voici une liste simple des commandes et des exemples de valeurs qui sont utilisés dans ce guide.

    
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
    


