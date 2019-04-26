---
title: Stratégie de qualité de service \(QoS, Quality of Service\)
description: Cette rubrique fournit une vue d’ensemble de la stratégie de qualité de Service (QoS), qui vous permet d’utiliser la stratégie de groupe pour classer par priorité de la bande passante du trafic réseau des applications spécifiques et des services dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 16918506-102c-482e-89d3-004ad8d6aabe
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a3ef81825ef6544bc96506e37bc027e0ac2a78db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820280"
---
# <a name="quality-of-service-qos-policy"></a>Qualité de Service \(QoS\) stratégie

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser une stratégie de qualité de service comme pilier de la gestion de bande passante réseau dans toute votre infrastructure Active Directory, en créant des profils de qualité de service dont les paramètres sont distribués à l’aide de la stratégie de groupe.

>[!NOTE]
>  Outre cette rubrique, la documentation suivante de la stratégie de qualité de service est disponible.  
>   
>  - [Mise en route de la stratégie de QoS](qos-policy-get-started.md)
>  - [Gérer la stratégie de QoS](qos-policy-manage.md)
>  - [Questions fréquentes de stratégie de QoS](qos-policy-faq.md)

Stratégies QoS sont appliquées à une ouverture de session utilisateur ou un ordinateur en tant que partie d’un objet de stratégie de groupe \(GPO\) que vous avez lié à un conteneur Active Directory, tel qu’un domaine, un site ou une unité d’organisation \(unité d’organisation\).

Gestion du trafic QoS se produit sous la couche application, ce qui signifie que vos applications existantes n’avez pas besoin être modifiés pour bénéficier des avantages fournis par les stratégies de QoS.

## <a name="operating-systems-that-support-qos-policy"></a>Systèmes d’exploitation qui prennent en charge de la stratégie de QoS

Vous pouvez utiliser la stratégie de QoS pour gérer la bande passante pour les ordinateurs ou utilisateurs avec les systèmes d’exploitation Microsoft suivants.

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

### <a name="location-of-qos-policy-in-group-policy"></a>Emplacement de la stratégie de QoS dans la stratégie de groupe

Dans Windows Server 2016 éditeur stratégies de groupe gestion, le chemin d’accès à la stratégie de QoS pour la Configuration de l’ordinateur est la suivante.

**Stratégie de domaine par défaut | Configuration ordinateur | Stratégies | Les paramètres Windows | Stratégie\-en fonction de qualité de service**

Ce chemin d’accès est illustrée dans l’image suivante.

![Emplacement de la stratégie de QoS dans la stratégie de groupe](../../media/QoS/QoS-Gp.jpg)

Dans Windows Server 2016 éditeur stratégies de groupe gestion, le chemin d’accès à la stratégie de QoS pour la Configuration de l’utilisateur est la suivante.

**Stratégie de domaine par défaut | Configuration de l’utilisateur | Stratégies | Les paramètres Windows | Stratégie\-en fonction de qualité de service**

Par défaut, aucune stratégie de qualité de service n’est configurés.

## <a name="why-use-qos-policy"></a>Pourquoi utiliser la stratégie de qualité de service ?
  
À mesure que le trafic augmente sur votre réseau, il est important pour vous permet d’équilibrer les performances du réseau avec le coût du service - mais le trafic réseau n’est pas normalement facile de hiérarchiser et à gérer.

Sur votre réseau, mission\-critique et la latence\-applications sensibles doivent en concurrence pour la bande passante réseau contre le trafic de priorité inférieure. En même temps, certains utilisateurs et les ordinateurs avec exigences peuvent nécessiter des performances réseau spécifique différencient des niveaux de service.

Les défis de fournir des niveaux de performances réseau économique et prévisible, apparaissent souvent tout d’abord sur le réseau étendu \(WAN\) connexions ou avec les applications sensibles à la latence, comme la voix sur IP \(VoIP\) et le flux vidéo. Toutefois, l’objectif final de fournir des niveaux de service réseau prévisibles s’applique à n’importe quel environnement réseau \(, par exemple, une entreprise réseau\)et à plusieurs applications VoIP, telles que la ligne personnalisé de votre société\-de\-les applications métier.
  
QoS basée sur la stratégie est l’outil de gestion de la bande passante réseau qui vous offre un contrôle de réseau - basé sur les applications, les utilisateurs et ordinateurs. 

Lorsque vous utilisez la stratégie de QoS, vos applications n’avez pas besoin être écrites pour les interfaces de programmation d’application spécifique \(API\). Cela vous donne la possibilité d’utiliser la qualité de service avec les applications existantes. En outre, basée sur des stratégies de QoS tire parti de votre infrastructure de gestion existante, car la QoS basée sur des stratégies est intégrée à la stratégie de groupe.

## <a name="define-qos-priority-through-a-differentiated-services-code-point-dscp"></a>Définir la priorité de qualité de service via un Point de Code de Services différenciés \(DSCP\)
  
Vous pouvez créer des stratégies de QoS qui définissent la priorité du trafic réseau avec un Point de Code de Services différenciés \(DSCP\) valeur que vous attribuez à différents types de trafic réseau. 

DSCP vous permet d’appliquer une valeur \(0 à 63\) dans le Type de Service \(OT\) champ dans l’en-tête d’un paquet IPv4 et dans le champ de classe de trafic IPv6. 

La valeur DSCP fournit la classification de trafic réseau sur le protocole Internet \(IP\) niveau, les routeurs qui permet de déterminer le comportement de file d’attente de trafic. 

Par exemple, vous pouvez configurer les routeurs pour placer des paquets avec des valeurs DSCP spécifiques dans un des trois files d’attente : priorité élevée, meilleur effort ou inférieur au meilleur effort. 

Le trafic réseau stratégiques, qui est dans la file d’attente de priorité élevée, est prioritaire sur tout autre trafic.

### <a name="limit-network-bandwidth-use-per-application-with-throttle-rate"></a>Limite la bande passante de réseau utilisée par l’Application avec le taux d’accélération

Vous pouvez également limiter le trafic réseau sortant d’une application en spécifiant un taux d’accélération dans Stratégie de QoS.

Une stratégie de QoS qui définit les limites de limitation détermine le taux de trafic réseau sortant. Par exemple, pour gérer les coûts de réseau étendus, un service informatique peut implémenter un contrat de niveau de service qui spécifie qu’un serveur de fichiers peut fournir jamais téléchargements au-delà d’une fréquence spécifique.  

### <a name="use-qos-policy-to-apply-dscp-values-and-throttle-rates"></a>Utiliser une stratégie QoS pour appliquer DSCP valeurs et taux de limitation

Vous pouvez également utiliser la stratégie de QoS pour appliquer des valeurs DSCP et la limitation des taux de trafic réseau sortant à ce qui suit :

- Application émettrice et le chemin du répertoire

- Source et destination des adresses IPv4 ou IPv6 ou les préfixes d’adresse

- Protocole - Transmission Control Protocol \(TCP\) et User Datagram Protocol \(UDP\)

- Ports source et destination et les plages de ports \(TCP ou UDP\)

- Groupes d’utilisateurs ou ordinateurs via un déploiement de stratégie de groupe spécifiques

À l’aide de ces contrôles, vous pouvez spécifier une stratégie de QoS avec une valeur DSCP de 46 pour une application VoIP, l’activation de routeurs placer des paquets de VoIP dans une file d’attente à faible latence, ou vous pouvez utiliser une stratégie de QoS pour limiter un ensemble de trafic sortant de serveurs à 512 kilo-octets par seconde <c1/>\(Kbits/s\) lors de l’envoi via le port TCP 443.

Vous pouvez également appliquer la stratégie de QoS à une application particulière qui a des besoins en bande passante spécial. Pour plus d’informations, consultez [les scénarios de stratégie de QoS](qos-policy-scenarios.md).
  
## <a name="advantages-of-qos-policy"></a>Avantages de la stratégie de QoS

Avec la stratégie de QoS, vous pouvez configurer et appliquer des stratégies de QoS qui ne peut pas être configurés sur les routeurs et commutateurs. Stratégie de QoS offre les avantages suivants.
  
1. **Niveau de détail :** Il est difficile de créer des stratégies de QoS au niveau de l’utilisateur sur les routeurs ou les commutateurs, surtout si l’ordinateur est que soit configurée à l’aide d’affectation d’adresses IP dynamiques ou si l’ordinateur n’est pas connecté au commutateur fixe ni aux ports du routeur, comme c’est souvent le cas avec ordinateurs portables. En revanche, stratégie de QoS rend plus facile à configurer un utilisateur\-le niveau de stratégie de QoS sur un contrôleur de domaine et de propager la stratégie à l’ordinateur de l’utilisateur.
2. **Flexibilité**. Quel que soit l’emplacement ou le mode un ordinateur se connecte au réseau, stratégie de QoS s’applique, l’ordinateur peut se connecter à l’aide de Wi-Fi ou Ethernet à partir de n’importe quel emplacement. Pour l’utilisateur\-le niveau de la qualité de service de stratégie est appliquée sur n’importe quel périphérique compatible à n’importe quel emplacement où l’utilisateur se connecte sur des stratégies de QoS.
3. **Sécurité :** Si votre service informatique chiffre le trafic des utilisateurs de bout en bout à l’aide d’Internet Protocol security \(IPsec\), vous ne pouvez pas classer le trafic sur les routeurs sur des informations au-dessus de la couche IP dans le paquet \(, par exemple, un Le port TCP\). Toutefois, en utilisant une stratégie de QoS, vous pouvez classer les paquets au niveau de l’appareil de fin pour indiquer la priorité des paquets dans l’en-tête IP avant les charges utiles IP sont chiffrés et les paquets sont envoyés.
4. **Performances :** Certaines fonctions de qualité de service, tels que les limitations, sont mieux exécutées lorsqu’ils sont plus proches de la source. Stratégie de QoS déplace ces fonctions de qualité de service le plus proche de la source.
5. **Facilité de gestion :** Stratégie de QoS améliore la facilité de gestion réseau de deux manières :

    **a**. Car elle est basée sur la stratégie de groupe, vous pouvez utiliser la stratégie de QoS pour configurer et gérer un ensemble de stratégies de QoS utilisateur/ordinateur chaque fois que nécessaire et sur l’ordinateur d’un contrôleur de domaine central.

    **b**. Stratégie de QoS facilite la configuration de l’ordinateur/utilisateur en fournissant un mécanisme permettant de spécifier des stratégies par Uniform Resource Locator \(URL\) au lieu de spécifier des stratégies basées sur les adresses IP de chacun des serveurs où les stratégies de QoS doivent à appliquer. Par exemple, supposons que votre réseau dispose d’un cluster de serveurs qui partagent une URL courantes. En utilisant une stratégie de QoS, vous pouvez créer une stratégie basée sur l’URL courantes, au lieu de créer une stratégie pour chaque serveur dans le cluster, avec chaque stratégie basée sur l’adresse IP de chaque serveur.

Pour la rubrique suivante de ce guide, consultez [mise en route de la stratégie de QoS](qos-policy-get-started.md).

