---
title: Qualité de Service (QoS) stratégie
description: Cette rubrique fournit une vue d’ensemble de la stratégie de qualité de Service (QoS), qui vous permet d’utiliser la stratégie de groupe pour hiérarchiser le trafic bande passante du réseau des applications spécifiques et des services dans Windows Server2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 16918506-102c-482e-89d3-004ad8d6aabe
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 76405c677fe7eb4d51f9c190499b909ac2fe4a0c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="quality-of-service-qos-policy"></a>Qualité de Service \(QoS\) stratégie

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser Stratégie de QoS comme un point de gestion de bande passante réseau central sur votre infrastructure Active Directory en créant des profils de qualité de service, dont les paramètres sont distribués avec la stratégie de groupe.

>[!NOTE]
>  Outre cette rubrique, la documentation suivante de la stratégie de QoS est disponible.  
>   
>  - [Prise en main de la stratégie de QoS](qos-policy-get-started.md)
>  - [Gérer la stratégie de QoS](qos-policy-manage.md)
>  - [Stratégie de QoS Forum aux Questions](qos-policy-faq.md)

Stratégies de QoS sont appliquées à une session utilisateur ou un ordinateur dans le cadre d’un objet de stratégie de groupe que vous avez lié à un conteneur ActiveDirectory, par exemple, un domaine, le site ou l’unité d’organisation \(GPO\) \(OU\).

Gestion du trafic QoS se produit sous la couche application, ce qui signifie que vos applications existantes ne souhaitez pas être modifié pour bénéficier des avantages fournis par les stratégies de QoS.

## <a name="operating-systems-that-support-qos-policy"></a>Systèmes d’exploitation qui prennent en charge de la stratégie de QoS

Vous pouvez utiliser la stratégie de QoS pour gérer la bande passante pour les ordinateurs ou utilisateurs avec les systèmes d’exploitation Microsoft suivants.

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

### <a name="location-of-qos-policy-in-group-policy"></a>Emplacement de la stratégie de QoS dans la stratégie de groupe

Dans Windows Server2016 éditeur stratégie de groupe gestion, le chemin d’accès à la stratégie de QoS pour la Configuration de l’ordinateur est la suivante.

**Stratégie de domaine par défaut | Configuration ordinateur | Stratégies | Paramètres Windows | QoS basée sur le compte**

Ce chemin d’accès est illustrée dans l’image suivante.

![Emplacement de la stratégie de QoS dans la stratégie de groupe](../../media/QoS/QoS-Gp.jpg)

Dans Windows Server2016 éditeur stratégie de groupe gestion, le chemin d’accès à la stratégie de QoS pour la Configuration de l’utilisateur est la suivante.

**Stratégie de domaine par défaut | Configuration de l’utilisateur | Stratégies | Paramètres Windows | QoS basée sur le compte**

Aucune stratégie de qualité de service n’est configurés par défaut.

## <a name="why-use-qos-policy"></a>Pourquoi utiliser une stratégie de qualité de service?
  
Comme le trafic augmente sur votre réseau, il est très important pour répartir les performances du réseau avec le coût du service - mais le trafic réseau n’est pas normalement facile de hiérarchiser et gérer.

Sur votre réseau, les applications stratégiques mission\ et latency\ sensibles doivent sont en concurrence pour la bande passante réseau contre le trafic de priorité inférieure. En même temps, certains utilisateurs et les ordinateurs avec des performances réseau spécifique exigences peuvent nécessiter différencient des niveaux de service.

Les défis de fournir des niveaux de performances réseau prévisibles économique s’affichent souvent tout d’abord sur des connexions \(WAN\) de réseau étendu ou avec les applications sensibles à la latence, comme la voix sur IP \(VoIP\) et le flux vidéo. Toutefois, l’objectif final de fournir des niveaux de service réseau prévisibles s’applique à n’importe quel environnement réseau \ (par exemple, une entreprise réseau local Updatable) et à plusieurs applications VoIP, tels que les applications de modes maximum\ métier personnalisées de votre entreprise.
  
La QoS basée sur la stratégie est l’outil de gestion de la bande passante réseau qui vous offre un contrôle de réseau - basé sur les applications, les utilisateurs et ordinateurs. 

Lorsque vous utilisez la stratégie de QoS, vos applications est inutile d’être écrite pour une application spécifique programmation interfaces \(APIs\). Cela vous donne la possibilité d’utiliser la qualité de service avec les applications existantes. En outre, la QoS basée sur la stratégie tire parti de votre infrastructure de gestion existante, car la QoS basée sur la stratégie est intégrée à la stratégie de groupe.

## <a name="define-qos-priority-through-a-differentiated-services-code-point-dscp"></a>Définir la priorité de la qualité de service via un Differentiated Services Code Point \(DSCP\)
  
Vous pouvez créer des stratégies de QoS qui définissent la priorité du trafic réseau avec la valeur \(DSCP\) différenciés Services Code Point que vous affectez aux différents types de trafic réseau. 

DSCP vous permet d’appliquer un \(0–63\) valeur dans le champ Type de Service \(TOS\) dans l’en-tête d’un paquet IPv4 et dans le champ classe de trafic IPv6. 

La valeur DSCP fournit la classification de trafic réseau au niveau du protocole Internet \(IP\), qui utilisent des routeurs pour décider du trafic queuing comportement. 

Par exemple, vous pouvez configurer des routeurs pour placer les paquets avec des valeurs DSCP spécifiques dans un des trois files d’attente: priorité élevée, meilleur effort, inférieur ou meilleur effort. 

Le trafic réseau critiques, qui se trouve dans la file d’attente de priorité élevée, a la préférence sur le reste du trafic.

### <a name="limit-network-bandwidth-use-per-application-with-throttle-rate"></a>Limite la bande passante de réseau utilisée par l’Application avec le taux d’accélération

Vous pouvez également limiter le trafic réseau sortant de l’application en spécifiant un taux d’accélération dans la stratégie de qualité de service.

Une stratégie de QoS qui définit les limites de limitation détermine la vitesse du trafic réseau sortant. Par exemple, pour gérer les coûts de réseau étendus, un service informatique peut implémenter un contrat de niveau de service qui spécifie qu’un serveur de fichiers peut fournir jamais téléchargements au-delà d’une fréquence spécifique.  

### <a name="use-qos-policy-to-apply-dscp-values-and-throttle-rates"></a>Utiliser une stratégie QoS pour appliquer DSCP, valeurs et taux d’accélération

Vous pouvez également utiliser la stratégie de QoS pour appliquer les valeurs DSCP et limiter le taux pour le trafic réseau sortant à ce qui suit:

- Application d’envoi et de chemin d’accès de répertoire

- Les adresses IPv4 ou IPv6 source et de destination ou préfixes d’adresses

- Protocole: contrôle de la Transmission du protocole \(TCP\) et utilisateur User Datagram Protocol \(UDP\)

- Les ports source et de destination et les plages de ports \(TCP or UDP\)

- Groupes d’utilisateurs ou des ordinateurs par le biais de déploiement dans la stratégie de groupe spécifiques

À l’aide de ces contrôles, vous pouvez spécifier une stratégie de QoS avec une valeur DSCP de 46 pour une application VoIP, l’activation de routeurs placer des paquets de VoIP dans une file d’attente à faible latence, ou vous pouvez utiliser une stratégie de QoS pour limiter la bande passante un ensemble de trafic sortant des serveurs à 512kilo-octets par seconde \(KBps\) lors de l’envoi de port TCP 443.

Vous pouvez également appliquer la stratégie de QoS pour une application particulière qui a des besoins en bande passante spécial. Pour plus d’informations, voir [scénarios de stratégie de QoS](qos-policy-scenarios.md).
  
## <a name="advantages-of-qos-policy"></a>Avantages de la stratégie de QoS

Avec la stratégie de QoS, vous pouvez configurer et appliquer des stratégies de qualité de service qui ne peut pas être configurés sur les routeurs et commutateurs. Stratégie de QoS offre les avantages suivants.
  
1. **Niveau de détail:** il est difficile de créer des stratégies de QoS de niveau utilisateur sur les routeurs ou les commutateurs, en particulier si l’ordinateur est configuré à l’aide de IP dynamique l’attribution d’adresses ou si l’ordinateur n’est pas connecté à un commutateur fixe ou les ports du routeur, en tant qu’il n’est souvent le cas avec les ordinateurs portables. En revanche, stratégie de QoS rend plus facile de configurer une stratégie de QoS par l’utilisateur sur un contrôleur de domaine et de propager la stratégie de l’ordinateur de l’utilisateur.
2. **Flexibilité**. Quelle que soit l’où et comment un ordinateur se connecte au réseau, stratégie de QoS s’applique: l’ordinateur peut se connecter à l’aide de Wi-Fi ou Ethernet à partir de n’importe quel emplacement. Pour les stratégies de QoS au niveau par l’utilisateur, la stratégie de QoS est appliquée sur n’importe quel appareil compatible à n’importe quel emplacement dans lequel l’utilisateur ouvre une session.
3. **Sécurité:** si votre service informatique chiffre le trafic des utilisateurs de bout en bout à l’aide d’Internet Protocol security \(IPsec\), vous ne pouvez pas classer le trafic sur les routeurs sur des informations au-dessus de la couche IP dans le paquet \ (par exemple, un LPT1\ TCP). Toutefois, en utilisant une stratégie de QoS, vous pouvez classer les paquets au niveau du périphérique de fin pour indiquer la priorité des paquets dans l’en-tête IP avant les charges utiles IP sont chiffrés et les paquets sont envoyés.
4. **Performances:** certaines fonctions de qualité de service, telles que la limitation, sont effectuées mieux lorsqu’ils ne sont plus proches de la source. Stratégie de QoS déplace les fonctions de qualité de service le plus proche de la source.
5. **Facilité de gestion:** stratégie de QoS améliore la facilité de gestion réseau de deux manières:

    **a**. Parce qu’il est basé sur la stratégie de groupe, vous pouvez utiliser la stratégie de QoS pour configurer et gérer un ensemble de stratégies de QoS utilisateur/ordinateur chaque fois que nécessaire et sur l’ordinateur d’un contrôleur de domaine central.

    **b**. Stratégie de QoS facilite la configuration de l’utilisateur/ordinateur en fournissant un mécanisme permettant de spécifier des stratégies par \(URL\) Uniform Resource Locator au lieu de spécifier les stratégies basées sur les adresses IP de chaque serveur où les stratégies de QoS doivent être appliquées. Par exemple, supposons que votre réseau dispose d’un cluster de serveurs qui partagent une URL courantes. À l’aide de stratégie de QoS, vous pouvez créer une stratégie basée sur l’URL courantes, au lieu de créer une stratégie pour chaque serveur du cluster, avec chaque stratégie basée sur l’adresse IP de chaque serveur.

Pour la rubrique suivante dans ce guide, voir [prise en main de la stratégie de QoS](qos-policy-get-started.md).

