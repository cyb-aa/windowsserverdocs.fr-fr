---
title: Équilibrage de charge du serveur proxy NPS
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur les fonctionnalités et fonctionnalités VPN de Windows Server 2016 et Windows 10.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 528280e6-b47e-489f-b310-b257d434aa0d
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 10a33494365f5a10923dd9ce46c3575675099b27
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315973"
---
# <a name="nps-proxy-server-load-balancing"></a>Équilibrage de charge du serveur proxy NPS

S’applique à Windows Server 2016

Les clients protocole RADIUS (Remote Authentication Dial-In User Service) (RADIUS), qui sont des serveurs d’accès réseau tels que des serveurs de réseau privé virtuel (VPN) et des points d’accès sans fil, créent des demandes de connexion et les envoient à des serveurs RADIUS tels que NPS. Dans certains cas, un serveur NPS peut recevoir un trop grand nombre de demandes de connexion à la fois, ce qui entraîne une dégradation des performances ou une surcharge. Lorsqu’un serveur NPS est surchargé, il est judicieux d’ajouter plus de NPSs à votre réseau et de configurer l’équilibrage de charge. Lorsque vous distribuez uniformément des demandes de connexion entrante entre plusieurs NPSs pour empêcher la surcharge d’un ou plusieurs NPSs, il s’agit de l’équilibrage de charge.

L’équilibrage de charge est particulièrement utile pour :

- Les organisations qui utilisent le protocole EAP-TLS (Extensible Authentication Protocol-Transport Layer Security \(EAP-TLS\) ou Protected Extensible Authentication Protocol \(PEAP\)-TLS pour l’authentification. Étant donné que ces méthodes d’authentification utilisent des certificats pour l’authentification du serveur et pour l’authentification de l’utilisateur ou de l’ordinateur client, la charge sur les proxys et les serveurs RADIUS est plus lourde que lors de l’utilisation des méthodes d’authentification par mot de passe.
- Les organisations qui doivent maintenir une disponibilité continue des services.
- Les fournisseurs de services Internet \(\) ISP qui sous-traitent l’accès VPN pour d’autres organisations. Les services VPN externalisés peuvent générer un volume important de trafic d’authentification.

Vous pouvez utiliser deux méthodes pour équilibrer la charge des demandes de connexion envoyées à votre NPSs :

- Configurez vos serveurs d’accès réseau pour envoyer des demandes de connexion à plusieurs serveurs RADIUS. Par exemple, si vous disposez de 20 points d’accès sans fil et de deux serveurs RADIUS, configurez chaque point d’accès pour envoyer des demandes de connexion aux deux serveurs RADIUS. Vous pouvez équilibrer la charge et fournir un basculement sur chaque serveur d’accès réseau en configurant le serveur d’accès pour envoyer des demandes de connexion à plusieurs serveurs RADIUS dans un ordre de priorité spécifié. Cette méthode d’équilibrage de charge est généralement idéale pour les petites organisations qui ne déploient pas un grand nombre de clients RADIUS.
- Utilisez NPS configuré en tant que proxy RADIUS pour équilibrer la charge des demandes de connexion entre plusieurs NPSs ou d’autres serveurs RADIUS. Par exemple, si vous avez 100 points d’accès sans fil, un proxy NPS et trois serveurs RADIUS, vous pouvez configurer les points d’accès pour envoyer tout le trafic vers le proxy NPS. Sur le proxy NPS, configurez l’équilibrage de charge afin que le proxy distribue uniformément les demandes de connexion entre les trois serveurs RADIUS. Cette méthode d’équilibrage de charge est idéale pour les entreprises de taille moyenne ou grande qui possèdent de nombreux clients et serveurs RADIUS.

Dans de nombreux cas, la meilleure approche de l’équilibrage de charge consiste à configurer les clients RADIUS pour envoyer des demandes de connexion à deux serveurs proxy NPS, puis à configurer les proxys NPS pour équilibrer la charge entre les serveurs RADIUS. Cette approche offre à la fois le basculement et l’équilibrage de charge pour les proxies et les serveurs RADIUS NPS.

## <a name="radius-server-priority-and-weight"></a>Priorité et poids des serveurs RADIUS

Pendant le processus de configuration du proxy NPS, vous pouvez créer des groupes de serveurs RADIUS distants, puis ajouter des serveurs RADIUS à chaque groupe. Pour configurer l’équilibrage de charge, vous devez disposer de plusieurs serveurs RADIUS par groupe de serveurs RADIUS distants. Lors de l’ajout de membres de groupe, ou après la création d’un serveur RADIUS en tant que membre d’un groupe, vous pouvez accéder à la boîte de dialogue Ajouter un serveur RADIUS pour configurer les éléments suivants sous l’onglet équilibrage de charge :

- **Priorité**. Priorité spécifie l’ordre d’importance du serveur RADIUS pour le serveur proxy NPS. Un niveau de priorité doit être affecté à une valeur qui est un entier, par exemple 1, 2 ou 3. Plus le nombre est faible, plus la priorité du proxy NPS est élevée pour le serveur RADIUS. Par exemple, si le serveur RADIUS reçoit la priorité la plus élevée de 1, le proxy NPS envoie d’abord les demandes de connexion au serveur RADIUS ; Si les serveurs avec la priorité 1 ne sont pas disponibles, NPS envoie ensuite des demandes de connexion aux serveurs RADIUS avec la priorité 2, et ainsi de suite. Vous pouvez affecter la même priorité à plusieurs serveurs RADIUS, puis utiliser le paramètre de pondération pour équilibrer la charge entre eux.

- **Poids**. NPS utilise ce paramètre de pondération pour déterminer le nombre de demandes de connexion à envoyer à chaque membre du groupe lorsque les membres du groupe ont le même niveau de priorité. Une valeur comprise entre 1 et 100 doit être affectée au paramètre Weight et la valeur représente un pourcentage de 100%. Par exemple, si le groupe de serveurs RADIUS distants contient deux membres qui ont tous deux un niveau de priorité de 1 et une évaluation de poids de 50, le proxy NPS transfère 50% des demandes de connexion à chaque serveur RADIUS.

- **Paramètres avancés**. Ces paramètres de basculement permettent à NPS de déterminer si le serveur RADIUS distant n’est pas disponible. Si le serveur NPS détermine qu’un serveur RADIUS n’est pas disponible, il peut commencer à envoyer des demandes de connexion à d’autres membres du groupe. Avec ces paramètres, vous pouvez configurer le nombre de secondes pendant lesquelles le proxy NPS attend une réponse du serveur RADIUS avant de considérer que la demande a été supprimée. nombre maximal de demandes abandonnées avant que le proxy NPS n’identifie le serveur RADIUS comme non disponible ; et le nombre de secondes qui peuvent s’écouler entre les demandes avant que le proxy NPS n’identifie le serveur RADIUS comme non disponible.

## <a name="configure-nps-proxy-load-balancing"></a>Configurer l’équilibrage de charge du proxy NPS

Avant de configurer l’équilibrage de charge, créez un plan de déploiement qui comprend le nombre de groupes de serveurs RADIUS distants dont vous avez besoin, les serveurs membres de chaque groupe particulier et le paramètre priorité et poids pour chaque serveur.

>[!NOTE]
>Les étapes suivantes supposent que vous avez déjà déployé et configuré des serveurs RADIUS.

Pour configurer NPS pour agir en tant que serveur proxy et transférer les demandes de connexion des clients RADIUS vers les serveurs RADIUS distants, vous devez effectuer les actions suivantes :

1. Déployez vos clients RADIUS \(les serveurs VPN, les serveurs d’accès à distance, les serveurs de passerelle des services Terminal Server, les commutateurs d’authentification 802.1 X et les points d’accès sans fil 802.1 X\) et configurez-les pour envoyer des demandes de connexion à vos serveurs proxy NPS.

2. Sur le proxy NPS, configurez les serveurs d’accès réseau en tant que clients RADIUS. Pour plus d’informations, consultez [configurer des clients RADIUS](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-radius-clients-configure).

3. Sur le proxy NPS, créez un ou plusieurs groupes de serveurs RADIUS distants. Au cours de ce processus, ajoutez des serveurs RADIUS aux groupes de serveurs RADIUS distants. Pour plus d’informations, consultez [configurer des groupes de serveurs RADIUS distants](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-rrsg-configure).

4. Sur le proxy NPS, pour chaque serveur RADIUS que vous ajoutez à un groupe de serveurs RADIUS distants, cliquez sur l’onglet **équilibrage de charge** du serveur RADIUS, puis configurez les paramètres **priorité**, **poids**et **avancé**.

5. Sur le proxy NPS, configurez les stratégies de demande de connexion pour transférer les demandes d’authentification et de gestion des comptes aux groupes de serveurs RADIUS distants. Vous devez créer une stratégie de demande de connexion par groupe de serveurs RADIUS distants. Pour plus d’informations, consultez [configurer des stratégies de demande de connexion](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-configure).


