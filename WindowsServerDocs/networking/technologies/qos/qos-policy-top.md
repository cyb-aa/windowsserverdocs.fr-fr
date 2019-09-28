---
title: Stratégie de qualité de service (QoS, Quality of Service)
description: Cette rubrique fournit une vue d’ensemble de la stratégie de qualité de service (QoS), qui vous permet d’utiliser stratégie de groupe pour hiérarchiser la bande passante du trafic réseau d’applications et de services spécifiques dans Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 16918506-102c-482e-89d3-004ad8d6aabe
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 429c38d93c2c5c0053153d538304767c8261229c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395856"
---
# <a name="quality-of-service-qos-policy"></a>Stratégie QoS \(\) de qualité de service

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser une stratégie de qualité de service comme pilier de la gestion de bande passante réseau dans toute votre infrastructure Active Directory, en créant des profils de qualité de service dont les paramètres sont distribués à l’aide de la stratégie de groupe.

>[!NOTE]
>  En plus de cette rubrique, la documentation de la stratégie de QoS suivante est disponible.  
>   
>  - [Prise en main avec la stratégie QoS](qos-policy-get-started.md)
>  - [Gérer la stratégie QoS](qos-policy-manage.md)
>  - [Forum aux questions sur la stratégie QoS](qos-policy-faq.md)

Les stratégies de QoS sont appliquées à une session de connexion utilisateur ou à un ordinateur dans le \(cadre\) d’un objet de stratégie de groupe d’objets stratégie de groupe que vous avez lié à un conteneur Active Directory, \(tel\)qu’un domaine, un site ou une unité d’organisation.

La gestion du trafic QoS se situe au-dessous de la couche application, ce qui signifie que vos applications existantes n’ont pas besoin d’être modifiées pour tirer parti des avantages fournis par les stratégies QoS.

## <a name="operating-systems-that-support-qos-policy"></a>Systèmes d’exploitation prenant en charge la stratégie QoS

Vous pouvez utiliser la stratégie QoS pour gérer la bande passante pour les ordinateurs ou les utilisateurs avec les systèmes d’exploitation Microsoft suivants.

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

### <a name="location-of-qos-policy-in-group-policy"></a>Emplacement de la stratégie QoS dans stratégie de groupe

Dans Windows Server 2016 Éditeur de gestion des stratégies de groupe, le chemin d’accès à la stratégie QoS pour la configuration de l’ordinateur est le suivant.

**Stratégie de domaine par défaut | Configuration de l’ordinateur | Stratégies | Paramètres Windows | QoS\-basée sur la stratégie**

Ce chemin d’accès est illustré dans l’image suivante.

![Emplacement de la stratégie QoS dans stratégie de groupe](../../media/QoS/QoS-Gp.jpg)

Dans Windows Server 2016 Éditeur de gestion des stratégies de groupe, le chemin d’accès à la stratégie QoS pour la configuration utilisateur est le suivant.

**Stratégie de domaine par défaut | Configuration de l’utilisateur | Stratégies | Paramètres Windows | QoS\-basée sur la stratégie**

Par défaut, aucune stratégie de qualité de service n’est configurée.

## <a name="why-use-qos-policy"></a>Pourquoi utiliser la stratégie QoS ?
  
À mesure que le trafic augmente sur votre réseau, il est de plus en plus important d’équilibrer les performances du réseau avec le coût du service, mais le trafic réseau n’est généralement pas facile à hiérarchiser et à gérer.

Sur votre réseau,\-les applications critiques et\-sensibles à la latence doivent concurrencer la bande passante réseau par rapport au trafic de faible priorité. En même temps, certains utilisateurs et ordinateurs avec des exigences de performances réseau spécifiques peuvent nécessiter des niveaux de service différenciés.

Les défis liés à la fourniture de niveaux de performances réseau prévisibles et rentables s’affichent \(souvent\) sur des connexions WAN de réseau étendu ou avec des applications sensibles à \(la latence, comme la VoIPvoixsurIP\) et vidéo streaming. Toutefois, l’objectif final de fournir des niveaux de service réseau prévisibles s’applique à \(n’importe quel environnement réseau, par exemple,\)un réseau local de l’entreprise et plus que des applications VoIP, telles que la lignepersonnaliséedevotreentreprise.\-d'\-applications d’entreprise.
  
La QoS basée sur la stratégie est l’outil de gestion de la bande passante réseau qui vous permet de contrôler le réseau en fonction des applications, des utilisateurs et des ordinateurs. 

Lorsque vous utilisez la stratégie QoS, vos applications n’ont pas besoin d’être écrites pour des API \(\)d’interfaces de programmation d’application spécifiques. Cela vous donne la possibilité d’utiliser la qualité de service avec les applications existantes. En outre, la QoS basée sur la stratégie tire parti de votre infrastructure de gestion existante, car la qualité de service basée sur la stratégie est intégrée à stratégie de groupe.

## <a name="define-qos-priority-through-a-differentiated-services-code-point-dscp"></a>Définir la priorité QoS via un point \(de code de services différenciés DSCP\)
  
Vous pouvez créer des stratégies de QoS qui définissent la priorité du trafic réseau avec \(une\) valeur DSCP du point de code des services différenciés que vous attribuez à différents types de trafic réseau. 

Le DSCP vous permet d’appliquer une valeur \(comprise\) entre 0 et 63 dans \(le\) type de champ de service OT dans l’en-tête d’un paquet IPv4, et dans le champ de classe de trafic dans IPv6. 

La valeur DSCP fournit la classification du trafic réseau au niveau \(IP\) du protocole Internet, que les routeurs utilisent pour déterminer le comportement de la mise en file d’attente du trafic. 

Par exemple, vous pouvez configurer des routeurs pour placer des paquets avec des valeurs DSCP spécifiques dans l’une des trois files d’attente : High Priority, Best effort ou Lower effort. 

Le trafic réseau stratégique, qui se trouve dans la file d’attente de priorité élevée, est prioritaire par rapport à d’autres trafics.

### <a name="limit-network-bandwidth-use-per-application-with-throttle-rate"></a>Limiter l’utilisation de la bande passante réseau par application avec taux d’accélération

Vous pouvez également limiter le trafic réseau sortant d’une application en spécifiant un taux d’accélération dans la stratégie QoS.

Une stratégie de QoS qui définit les limites de limitation détermine le taux de trafic réseau sortant. Par exemple, pour gérer les coûts du réseau étendu, un service informatique peut implémenter un contrat de niveau de service qui spécifie qu’un serveur de fichiers ne peut jamais fournir des téléchargements au-delà d’une fréquence spécifique.  

### <a name="use-qos-policy-to-apply-dscp-values-and-throttle-rates"></a>Utiliser la stratégie QoS pour appliquer des valeurs DSCP et des taux de limitation

Vous pouvez également utiliser la stratégie QoS pour appliquer des valeurs DSCP et des taux de limitation pour le trafic réseau sortant vers les éléments suivants :

- Envoi de l’application et du chemin d’accès du répertoire

- Adresses IPv4 ou IPv6 sources et de destination ou préfixes d’adresse

- Protocole \( \(TCP\) et UDP protocole UDP (User Datagram Protocol)\)

- Ports source et de destination et plages \(de ports TCP ou UDP\)

- Groupes d’utilisateurs ou d’ordinateurs spécifiques par le biais du déploiement dans stratégie de groupe

À l’aide de ces contrôles, vous pouvez spécifier une stratégie de qualité de service avec une valeur DSCP de 46 pour une application VoIP, en permettant aux routeurs de placer des paquets VoIP dans une file d’attente à faible latence, ou vous pouvez utiliser une stratégie de QoS pour limiter un ensemble de trafic sortant de serveurs à 512 kilo-octets par seconde <C1/>Kbit\) /s lors de l’envoi à partir du port TCP 443.

Vous pouvez également appliquer une stratégie de QoS à une application particulière qui a des besoins spécifiques en bande passante. Pour plus d’informations, consultez [scénarios de stratégie de QoS](qos-policy-scenarios.md).
  
## <a name="advantages-of-qos-policy"></a>Avantages de la stratégie QoS

Avec la stratégie QoS, vous pouvez configurer et appliquer des stratégies de QoS qui ne peuvent pas être configurées sur les routeurs et les commutateurs. La stratégie QoS offre les avantages suivants.
  
1. **Niveau de détail :** Il est difficile de créer des stratégies de QoS au niveau de l’utilisateur sur des routeurs ou des commutateurs, en particulier si l’ordinateur de l’utilisateur est configuré à l’aide d’une attribution d’adresse IP dynamique ou si l’ordinateur n’est pas connecté à des ports de commutateur ou de routeur fixes, comme c’est souvent le cas avec ordinateurs portables. En revanche, la stratégie QoS facilite la configuration d’une stratégie\-QoS de niveau utilisateur sur un contrôleur de domaine et la propagation de la stratégie sur l’ordinateur de l’utilisateur.
2. **Flexibilité**. Quel que soit l’emplacement ou la manière dont un ordinateur se connecte au réseau, la stratégie de qualité de service (QoS) est appliquée : l’ordinateur peut se connecter à l’aide de WiFi ou Ethernet à n’importe quel emplacement. Pour les\-stratégies QoS au niveau de l’utilisateur, la stratégie QoS est appliquée à n’importe quel appareil compatible à n’importe quel emplacement où l’utilisateur ouvre une session.
3. **Caution** Si votre service informatique chiffre le trafic des utilisateurs de bout en bout à l’aide du protocole \(IPSec\)(Internet Protocol Security) IPSec, vous ne pouvez pas classer le trafic sur les routeurs en fonction \(des informations situées au-dessus de la couche IP dans le paquet, par exemple, un Port\)TCP. Toutefois, à l’aide de la stratégie de QoS, vous pouvez classer les paquets sur l’appareil final pour indiquer la priorité des paquets dans l’en-tête IP avant le chiffrement des charges utiles IP et l’envoi des paquets.
4. **Garantie** Certaines fonctions de QoS, telles que la limitation, sont mieux exécutées lorsqu’elles sont plus proches de la source. La stratégie QoS déplace les fonctions QoS les plus proches de la source.
5. **Gestion** La stratégie QoS améliore la facilité de gestion du réseau de deux manières :

    **.** Étant donné qu’il est basé sur stratégie de groupe, vous pouvez utiliser la stratégie QoS pour configurer et gérer un ensemble de stratégies QoS utilisateur/ordinateur chaque fois que nécessaire et sur un ordinateur contrôleur de domaine central.

    **b**. La stratégie de QoS facilite la configuration des utilisateurs/ordinateurs en fournissant un mécanisme permettant de \(spécifier\) des stratégies en Uniform Resource Locator URL au lieu de spécifier des stratégies en fonction des adresses IP de chacun des serveurs dont les stratégies QoS ont besoin à appliquer. Supposons, par exemple, que votre réseau dispose d’un cluster de serveurs partageant une URL commune. À l’aide de la stratégie de QoS, vous pouvez créer une stratégie basée sur l’URL commune au lieu de créer une stratégie pour chaque serveur du cluster, chaque stratégie étant basée sur l’adresse IP de chaque serveur.

Pour la rubrique suivante de ce guide, consultez [prise en main avec la stratégie QoS](qos-policy-get-started.md).

