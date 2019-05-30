---
title: Vue d’ensemble d’Azure Stack HCI
description: Azure Stack HCI est un cluster Windows Server 2019 hyperconvergé qui exécute localement des charges de travail virtualisées sur un système matériel validé, en se connectant éventuellement aux services Azure pour utiliser la sauvegarde cloud, Site Recovery, etc. Les solutions Azure Stack HCI utilisent un système matériel validé par Microsoft afin de garantir fiabilité et performances optimales. Elles prennent en charge des technologies telles que les lecteurs NVMe, la mémoire persistante et les réseaux RDMA (accès direct à la mémoire à distance).
ms.technology: storage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 05/22/2019
ms.openlocfilehash: 92d600eeb833cd70bd714702b1fd950c4fe2cd87
ms.sourcegitcommit: b190fac4bfa5599751a60d3fc3b4c4a64dd9afd7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66008977"
---
# <a name="azure-stack-hci-overview"></a>Vue d’ensemble d’Azure Stack HCI

>S’applique à : Windows Server 2019

Azure Stack HCI est un cluster Windows Server 2019 hyperconvergé qui exécute localement des charges de travail virtualisées sur un système matériel validé, en se connectant éventuellement aux services Azure pour utiliser la sauvegarde cloud, Site Recovery, etc. Les solutions Azure Stack HCI utilisent un système matériel validé par Microsoft afin de garantir fiabilité et performances optimales. Elles prennent en charge des technologies telles que les lecteurs NVMe, la mémoire persistante et les réseaux RDMA (accès direct à la mémoire à distance).

Azure Stack HCI est une solution qui combine plusieurs produits :

- Un système matériel provenant d’un partenaire OEM

- Windows Server 2019 Datacenter Edition

- Windows Admin Center

- Les services Azure (facultatifs)

![Azure Stack HCI est une solution Microsoft hyperconvergée, qui est proposée par un large éventail de fabricants de matériel partenaires.](media/AS_HCI_solution.png)

Azure Stack HCI est une solution Microsoft hyperconvergée, qui est proposée par un large éventail de fabricants de matériel partenaires. Découvrez les scénarios suivants qui impliquent une solution hyperconvergée, afin de déterminer si Azure Stack HCI est la solution qui répond le mieux à vos besoins :

- **Actualiser les systèmes matériels vieillissants.** Remplacez vos anciens serveurs et vos anciennes infrastructures de stockage dans le but d’exécuter des machines virtuelles Windows et Linux localement et à la périphérie, à l’aide d’outils et de compétences informatiques existants.

- **Regrouper les charges de travail virtualisées.** Regrouper les applications existantes sur une infrastructure efficace et hyperconvergée. Utilisez les mêmes outils d’efficacité cloud que ceux utilisés pour exécuter les centres de données hyperscale comme Microsoft Azure.

- **Se connecter à Azure pour tirer parti des services cloud hybrides.** Simplifiez l’accès aux services de sécurité et de gestion cloud dans Azure, notamment la sauvegarde hors site, Site Recovery, la supervision basée sur le cloud, etc.

## <a name="the-azure-stack-family"></a>La famille Azure Stack

Azure Stack HCI fait partie de la famille Azure et Azure Stack. Il utilise en effet les mêmes fonctionnalités de calcul, de stockage et de réseau à définition logicielle qu’Azure Stack. Voici un récapitulatif des différentes solutions :

- [Azure](https://azure.microsoft.com) - Pour utiliser les services cloud publics
- [Azure Stack](https://azure.microsoft.com/overview/azure-stack) - Pour utiliser les services cloud localement
- [Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) - Pour exécuter localement des applications virtualisées, en se connectant éventuellement à Azure

![Azure et Azure Stack exécutent des services cloud, alors qu’Azure Stack HCI exécute des applications virtualisées localement.](media/azure_family.png)

|Azure : Utiliser les services cloud publics|Azure Stack : Utiliser les services cloud localement|Azure Stack HCI : Exécuter les applications virtualisées localement|
|-----------------|-----------------|-----------------|
|Pour les ressources de calcul libre-service à la demande permettant de moderniser les applications existantes et d’effectuer leur migration, ainsi que de générer de nouvelles applications cloud natives.|Générez et exécutez des applications cloud en périphérie lorsqu’elles sont déconnectées ou pour répondre à certaines réglementations,en utilisant des services Azure cohérents localement.| Exécutez des applications virtualisées localement, remplacez et rasssemblez votre infrastructure de serveur vieillissante et connectez-vous à Azure pour tirer parti des services cloud.|
|Plus de 100 services disponibles dans 54 régions du monde|Machines virtuelles Azure pour Windows et Linux, Azure Web Apps et Functions, Azure Key Vault, Azure Resource Manager, Place de marché Azure, Containers, Azure IoT et Event Hubs, outils administrateur (plans, offres, RBAC)|Solutions HCI validées utilisant la technologie Hyper V, les espaces de stockage direct, Windows Server 2019 et Windows Admin Center pour la gestion et l’accès intégré aux services Azure.|

Pour plus d’informations :

- Consultez le site web des solutions [Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci).
- Regardez la vidéo où les experts Microsoft Jeff Woolsey et Vijay Tewari font une [présentation des nouvelles solutions Azure Stack HCI](https://aka.ms/AzureStackOverviewVideo).

## <a name="hyperconverged-efficiencies"></a>Outils d’efficacité hyperconvergés

Les solutions Azure Stack HCI regroupent des fonctionnalités de calcul, de stockage et de réseau hautement virtualisées sur des serveurs et des composants x86 aux normes du secteur. Le regroupement des ressources dans un même cluster facilite leur déploiement, leur gestion et leur mise à l’échelle. Gérez avec l’outil d’automation en ligne de commande de votre choix ou avec Windows Admin Center.

Obtenez les meilleures performances de machine virtuelle du marché pour vos applications serveur avec Hyper-V, technologie d’hyperviseur fondatrice du cloud Microsoft, et avec la technologie Espaces de stockage direct et à sa prise en charge intégrée NVMe, sa mémoire persistante et son accès direct à la mémoire à distance (RDMA).

Sécurisez vos applications et vos données avec des machines virtuelles dotées d’une protection maximale, la microsegmentation de votre réseau, et le chiffrement natif pour les données au repos et en transit.

## <a name="hybrid-capabilities"></a>Fonctionnalités hybrides

L’utilisation d’une plateforme d’infrastructure hyperconvergée dans le cloud public vous permet de bénéficier à la fois des avantages du cloud et des systèmes locaux. Votre équipe peut développer ses compétences cloud grâce à l’intégration des services de gestion d’infrastructure Azure :

- Azure Site Recovery, pour la haute disponibilité et la reprise d’activité en tant que service (DRaaS).

- Azure Monitor, hub centralisé permettant d’effectuer le suivi des événements concernant vos applications, votre réseau et votre infrastructure à l’aide de fonctionnalités d’analytique avancées fournies par l’intelligence artificielle.

- Le témoin cloud, pour utiliser Azure en tant que tiebreaker léger pour le quorum du cluster.

- La sauvegarde Azure, pour la protection des données hors site et la protection contre les ransomware.

- Azure Update Management, pour l’évaluation et le déploiement des mises à jour sur les machines virtuelles Windows exécutées dans Azure et localement.

- La carte réseau Azure, pour connecter vos ressources locales à vos machines virtuelles dans Azure via une connexion VPN point à site.

- Synchronisez votre serveur de fichiers avec le cloud en utilisant Azure File Sync.

Pour plus d’informations, consultez [Connexion de Windows Server aux services hybrides Azure](..\manage\windows-admin-center\azure\index.md).

## <a name="management-tools-and-system-center"></a>Outils de gestion et System Center

Azure Stack HCI utilise les mêmes fonctionnalités de virtualisation, de stockage et de réseau à définition logicielle qu’Azure Stack. Avec Azure Stack HCI, vous disposez des droits d’administrateur complets sur le cluster : vous pouvez utiliser [Windows Admin Center](..\manage\windows-admin-center\overview.md), [System Center](https://www.microsoft.com/cloud-platform/system-center), ainsi que toutes les fonctionnalités d’[Hyper-V](../virtualization/hyper-v/hyper-v-on-windows-server.md), des [espaces de stockage direct](..\storage\storage-spaces\storage-spaces-direct-overview.md), de PowerShell et d’autres outils tiers, comme 5Nine Manager.

Microsoft Azure s’exécute sur le serveur Windows Server et la plateforme Hyper-V inclus dans Windows Server. Windows Server et System Center bénéficient des améliorations et bonnes pratiques issues de l’expérience qu’a acquise Microsoft grâce à sa gestion de réseaux de centre de données à l’échelle mondiale, notamment via Microsoft Azure. Vous pouvez ainsi déployer les mêmes technologies et bénéficier de leurs avantages, comme la flexibilité, l’automatisation et le contrôle lors de l’utilisation de technologies de réseau SDN (Software Defined Networking).

Déployez et gérez l’infrastructure avec System Center Virtual Machine Management (VMM) et System Center Operations Manager. Avec VMM, vous pouvez provisionner et gérer les ressources nécessaires pour créer et déployer des machines virtuelles et des services sur des clouds privés. Avec Operations Manager, vous pouvez superviser les services, les appareils et les opérations au sein de votre entreprise afin d’identifier les problèmes et les résoudre immédiatement.

## <a name="hardware-partners"></a>Partenaires matériel

Vous pouvez acheter auprès de 15 partenaires des solutions Azure Stack HCI validées qui exécutent Windows Server 2019. Le partenaire Microsoft de votre choix met tout en place pour vous, ce qui vous évite de longues phases de conception et de création. En outre, il constitue un point de contact unique pour tous les services d’implémentation et de support.

Consultez le [site web Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) pour voir les solutions Azure Stack HCI actuellement disponibles (plus de 70) auprès de ces partenaires Microsoft : ASUS, Axellio, bluechip, DataON, Dell EMC, Fujitsu, HPE, Hitachi, Huawei, Lenovo, NEC, primeLine Solutions, QCT, SecureGUARD et Supermicro.

## <a name="faq"></a>Forum Aux Questions

### <a name="what-do-azure-stack-and-azure-stack-hci-solutions-have-in-common"></a>Qu’ont en commun les solutions Azure Stack et les solutions Azure Stack HCI ?

Les solutions Azure Stack HCI contiennent les mêmes fonctionnalités de calcul, de stockage et de réseau à définition logicielle basées sur Hyper-V qu’Azure Stack. Les deux types de solutions répondent à des critères rigoureux de test et de validation qui garantissent fiabilité et compatibilité avec la plateforme matérielle sous-jacente.

### <a name="how-are-they-different"></a>En quoi sont-elles différentes ?

Avec Azure Stack, vous pouvez exécuter des services cloud localement. Vous pouvez exécuter localement des services IaaS et PaaS Azure pour générer et exécuter des applications cloud de façon constante, quel que soit l’emplacement, et les gérer localement dans le portail Azure.

Avec Azure Stack HCI, vous pouvez exécuter localement des charges de travail virtualisées, et les gérer avec Windows Admin Center et les outils Windows Server que vous avez l’habitude d’utiliser. Si vous le souhaitez, vous pouvez vous connecter à Azure pour des scénarios hybrides, comme la récupération de site ou la supervision basées sur le cloud, entre autres.

### <a name="why-is-microsoft-bringing-its-hci-offering-to-the-azure-stack-family"></a>Pourquoi Microsoft a-t-il décidé d’inclure son offre HCI dans la famille Azure Stack ?

La technologie hyperconvergée de Microsoft est déjà à la base d’Azure Stack.

De nombreux clients Microsoft disposent d’environnements informatiques complexes, et notre objectif est de leur fournir des solutions adaptées à leurs besoins. Azure Stack HCI est une évolution des solutions WSSD basées sur Windows Server 2016, qui étaient proposées auparavant par nos fabricants de matériel partenaires. Nous l’avons intégré à la famille Azure Stack, car nous proposons désormais de nouvelles options permettant de se connecter facilement à Azure pour utiliser les services de gestion d’infrastructure.

### <a name="does-azure-stack-hci-need-to-be-connected-to-azure"></a>Azure Stack HCI doit-il être connecté à Azure ?

Non, ceci est facultatif. L’intégration à Azure permet des scénarios hybrides comme la sauvegarde et la reprise d’activité hors site, ou la supervision et la gestion des mises à jour basées sur le cloud. Cependant, l’utilisation de ces fonctionnalités est facultative. Si vous avez besoin de rester déconnecté, nous le comprenons et pouvons vous aider dans cette voie.

### <a name="how-does-azure-stack-hci-relate-to-windows-server"></a>Quel est le lien entre Azure Stack HCI et Windows Server ?

Windows Server 2019 constitue le fondement de quasiment tous les produits Azure. Toutes les fonctionnalités que vous appréciez déjà sont toujours fournies et prises en charge dans Windows Server. Azure Stack HCI est recommandé pour déployer la technologie HCI localement, à l’aide d’un système matériel validé par Microsoft et disponible auprès de nos partenaires.

### <a name="will-i-be-able-to-upgrade-from-azure-stack-hci-to-azure-stack"></a>Est-il possible de procéder à une mise à niveau d’Azure Stack HCI vers Azure Stack ? 

Non, mais vous pouvez effectuer la migration de vos charges de travail Azure Stack HCI vers Azure Stack ou Azure.

### <a name="what-azure-services-can-i-connect-to-azure-stack-hci"></a>Quels sont les services Azure qui peuvent être connectés à Azure Stack HCI ?

Pour connaître la liste des services Azure que vous pouvez connecter à Azure Stack HCI, consultez [Connexion de Windows Server aux services hybrides Azure](../azure-hybrid-services/index.md).

### <a name="how-does-the-cost-of-azure-stack-hci-compare-to-azure-stack"></a>Quel est le coût d’Azure Stack HCI par rapport à Azure Stack ? 

Azure Stack est vendu sous la forme d’un système entièrement intégré incluant services et support. Vous pouvez acheter Azure Stack comme un système que vous gérez, ou comme un service entièrement managé par l’un de nos partenaires. Outre le système de base, les services Azure qui s’exécutent sur Azure Stack ou Azure sont disponibles avec paiement à l’utilisation.

Les solutions Azure Stack HCI suivent le modèle d’achat traditionnel. Vous pouvez acheter un système matériel validé auprès des partenaires Azure Stack HCI, ainsi que des logiciels (Windows Admin Center et Windows Server 2019 édition Datacenter avec fonctionnalités de centre de données à définition logicielle) via les différents canaux existants. Les services Azure qui peuvent être utilisés avec Windows Admin Center sont disponibles via un abonnement Azure.

### <a name="how-do-i-buy-azure-stack-hci-solutions"></a>Comment acheter les solutions Azure Stack HCI ?

Suivez les étapes ci-dessous :

1. Achetez un système matériel validé par Microsoft auprès du fabricant de matériel partenaire de votre choix.
1. Installez Windows Server 2019 édition Datacenter et Windows Admin Center pour bénéficier de fonctionnalités de gestion et de la possibilité de se connecter à Azure pour utiliser les services cloud.
1. Si vous le souhaitez, vous pouvez utiliser votre compte Azure pour attacher des services de gestion et de sécurité basés sur le cloud à vos charges de travail.

![Pour acheter des solutions Azure Stack HCI, choisissez un fabricant de matériel partenaire et la configuration la mieux adaptée à vos besoins.](media/howbuy_ashci.png)

## <a name="compare-azure-stack-and-azure-stack-hci"></a>Différences entre Azure Stack et Azure Stack HCI

Au fur et à mesure de votre transformation numérique, vous pouvez constater que vous allez plus vite en utilisant des services du cloud public pour créer des architectures modernes et actualiser les applications héritées. Toutefois, pour des raisons d’ordre technologique et réglementaire, de nombreuses charges de travail doivent être exécutées localement. Le tableau suivant vous permet de déterminer quelle stratégie de cloud hybride Microsoft répond le mieux à vos besoins, en apportant des innovations cloud aux charges de travail, quel que soit leur emplacement.

|Azure Stack|Azure Stack HCI|
|--------|-------|
|Nouvelles compétences, processus innovants|Mêmes compétences, processus similaires|
|Intégration des services Azure à votre centre de données|Connexion de votre centre de données aux services Azure|

### <a name="when-to-use-azure-stack"></a>Quand utiliser Azure Stack

|Azure Stack|Azure Stack HCI|
|--------|-------|
|Utilisez Azure Stack pour l’IaaS libre-service, avec une isolation renforcée, ainsi qu’un suivi précis de l’utilisation et de la rétrofacturation pour les locataires colocalisés. Idéal pour les fournisseurs de services et les clouds privés d’entreprise. Modèles de la Place de marché Azure.|Azure Stack HCI ne fournit pas de prise en charge multilocataire native.|
|Utilisez Azure Stack pour développer et exécuter des applications qui utilisent localement des services PaaS tels que Web Apps, Functions ou Event Hubs. Ces services s’exécutent sur Azure Stack exactement comme dans Azure, en fournissant un environnement de développement et d’exécution hybride cohérent.|Azure Stack HCI n’exécute pas de services PaaS localement.
|Utilisez Azure Stack pour moderniser le déploiement et l’exécution des applications à l’aide des pratiques DevOps, telles que Infrastructure as Code ou l’intégration continue et le déploiement continu (CI/CD), ainsi qu’à l’aide de fonctionnalités telles que les extensions de machine virtuelle cohérentes avec Azure. Idéal pour les équipes de développement et DevOps.|Azure Stack HCI n’inclut pas les outils DevOps nativement.

### <a name="when-to-use-azure-stack-hci"></a>Quand utiliser Azure Stack HCI

|Azure Stack|Azure Stack HCI|
|---------------|---------------|
|Azure Stack nécessite au minimum 4 nœuds et ses propres commutateurs réseau.|Utilisez Azure Stack HCI pour l’empreinte minimale des bureaux distants et des filiales (ROBO). Commencez avec seulement deux nœuds serveur et un réseau dos à dos sans commutateurs, pour une plus grande simplicité et un prix plus abordable. Les systèmes matériels démarrent à 4 disques et 64 Go de mémoire, bien en dessous de 10 000 $ par nœud.
|Azure Stack contraint la configurabilité d’Hyper-V et son ensemble de fonctionnalités en vue de maintenir la cohérence avec Azure.|Utilisez Azure Stack HCI pour une virtualisation Hyper-V simple des applications d’entreprise classiques comme Exchange, SharePoint et SQL Server, et pour virtualiser les rôles Windows Server comme Serveur de fichiers, DNS, DHCP, IIS et Active Directory. Accès illimité à toutes les fonctionnalités Hyper-V, comme les machines virtuelles dotées d’une protection maximale.|
|Azure Stack n’expose pas ces technologies d’infrastructure.|Utilisez Azure Stack HCI pour remplacer l’infrastructure à définition logicielle en place, composée de baies de stockage ou d’appliances réseau vieillissantes, sans procéder à d’importantes modifications de l’architecture. Les espaces de stockage direct et les réseaux SDN s’intègrent parfaitement aux environnements Hyper-V.|