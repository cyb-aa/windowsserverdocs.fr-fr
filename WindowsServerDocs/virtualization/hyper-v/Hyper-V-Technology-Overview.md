---
title: Présentation de la technologie Hyper-V
description: Décrit ce qu’est Hyper-V, comment l’utiliser, les fonctionnalités clés et les utilisations courantes.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac069fed-7bf5-4cc3-aff5-25a2766040b8
author: KBDAzure
ms.author: kathydav
ms.date: 11/29/2016
ms.openlocfilehash: 053f92f1ef07a2e574c93412626ee792d4d982e3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366784"
---
# <a name="hyper-v-technology-overview"></a>Présentation de la technologie Hyper-V

>S’applique à : Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Hyper-V est le produit de virtualisation matérielle de Microsoft. Elle vous permet de créer et d’exécuter une version logicielle d’un ordinateur, appelée *machine virtuelle*. Chaque machine virtuelle agit comme un ordinateur complet, exécutant un système d’exploitation et des programmes. Lorsque vous avez besoin de ressources de calcul, les machines virtuelles offrent plus de flexibilité, permettent de gagner du temps et de l’argent, et constituent un moyen plus efficace d’utiliser du matériel que d’exécuter un système d’exploitation sur du matériel physique.

Hyper-V exécute chaque ordinateur virtuel dans son propre espace isolé, ce qui signifie que vous pouvez exécuter simultanément plusieurs ordinateurs virtuels sur le même matériel. Vous pouvez effectuer cette opération pour éviter des problèmes tels qu’un blocage qui affecte les autres charges de travail ou pour accorder à différents utilisateurs, groupes ou services un accès à différents systèmes.

## <a name="some-ways-hyper-v-can-help-you"></a>D’une certaine façon, Hyper-V peut vous aider

Hyper-V peut vous aider à :

- **Établissez ou développez un environnement de cloud privé.** Fournissez des services informatiques plus flexibles et à la demande en déplaçant ou en développant votre utilisation des ressources partagées et en ajustant l’utilisation à mesure que la demande change.

- **Utilisez votre matériel plus efficacement.** Consolidez les serveurs et les charges de travail sur des ordinateurs physiques moins nombreux et plus puissants pour utiliser moins d’électricité et d’espace physique.

- **Améliorer la continuité des activités.** Réduire l’impact des temps d’arrêt planifiés et non planifiés de vos charges de travail.

- **Établissez ou développez une infrastructure VDI (Virtual Desktop Infrastructure).** L’utilisation d’une stratégie de bureau centralisée avec l’infrastructure VDI peut vous aider à accroître la flexibilité et la sécurité des données, ainsi qu’à simplifier la conformité réglementaire et à gérer les systèmes d’exploitation et les applications de bureau. Déployez Hyper-V et Serveur hôte de virtualisation des services Bureau à distance (ordinateur hôte de virtualisation des services Bureau à distance) sur le même serveur pour mettre des bureaux virtuels personnels ou des pools de bureaux virtuels à la disposition de vos utilisateurs.

- **Rendez le développement et les tests plus efficaces.** Reproduire différents environnements informatiques sans avoir à acheter ou maintenir tout le matériel dont vous avez besoin si vous utilisiez uniquement des systèmes physiques.

## <a name="hyper-v-and-other-virtualization-products"></a>Hyper-V et autres produits de virtualisation

Hyper-V dans Windows et Windows Server remplace les anciens produits de virtualisation matérielle, tels que Microsoft Virtual PC, Microsoft Virtual Server et Windows Virtual PC. Hyper-V offre des fonctionnalités de mise en réseau, de performances, de stockage et de sécurité qui ne sont pas disponibles dans ces anciens produits.

Hyper-V et la plupart des applications de virtualisation tierces qui requièrent les mêmes fonctionnalités de processeur ne sont pas compatibles. Cela est dû au fait que les fonctionnalités du processeur, connues sous le nom d’extensions de virtualisation matérielle, sont conçues pour ne pas être partagées. Pour plus d’informations, consultez les [applications de virtualisation ne fonctionnent pas avec Hyper-V, Device Guard et Credential Guard](https://support.microsoft.com/kb/3204980).

## <a name="what-features-does-hyper-v-have"></a>Quelles sont les fonctionnalités d’Hyper-V ?

Hyper-V offre de nombreuses fonctionnalités. Il s’agit d’une vue d’ensemble, regroupée en fonction des fonctionnalités fournies par les fonctionnalités ou de l’aide.

**Environnement informatique** : un ordinateur virtuel Hyper-V comprend les mêmes composants de base qu’un ordinateur physique, comme la mémoire, le processeur, le stockage et la mise en réseau. Toutes ces parties offrent des fonctionnalités et des options que vous pouvez configurer de manière à répondre à différents besoins. Le stockage et la mise en réseau peuvent être considérés comme des catégories propres, en raison des nombreuses façons de les configurer.

**Récupération d’urgence et sauvegarde** : pour la récupération d’urgence, le réplica Hyper-V crée des copies de machines virtuelles, destinées à être stockées dans un autre emplacement physique. vous pouvez donc restaurer l’ordinateur virtuel à partir de la copie. Pour la sauvegarde, Hyper-V offre deux types. L’une utilise les États enregistrés et les autres Service VSS (VSS) pour vous permettre de réaliser des sauvegardes cohérentes au point de l’application pour les programmes qui prennent en charge VSS.

**Optimisation** -chaque système d’exploitation invité pris en charge possède un ensemble de services et de pilotes personnalisés, appelés *services d’intégration*, qui facilitent l’utilisation du système d’exploitation dans une machine virtuelle Hyper-V.

**Portabilité** : des fonctionnalités telles que la migration dynamique, la migration du stockage et l’importation/exportation facilitent le déplacement ou la distribution d’une machine virtuelle.

**Connectivité à distance** : Hyper-V comprend connexion à un ordinateur virtuel, un outil de connexion à distance à utiliser avec Windows et Linux. Contrairement à Bureau à distance, cet outil vous donne accès à la console, ce qui vous permet de voir ce qui se passe dans l’invité, même si le système d’exploitation n’est pas encore démarré.

**Sécurité** : les machines virtuelles protégées et le démarrage sécurisé vous aident à vous protéger contre les logiciels malveillants et autres accès non autorisés à une machine virtuelle et à ses données.

Pour obtenir un résumé des fonctionnalités introduites dans cette version, consultez [Nouveautés d’Hyper-V sur Windows Server](What-s-new-in-Hyper-V-on-Windows.md). Certaines fonctionnalités ou parties ont une limite pour le nombre de composants pouvant être configurés. Pour plus d’informations, consultez [planifier l’extensibilité d’Hyper-V dans Windows Server 2016](plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md).

## <a name="how-to-get-hyper-v"></a>Comment se procurer Hyper-V

Hyper-V est disponible dans Windows Server et Windows, en tant que rôle de serveur disponible pour les versions x64 de Windows Server. Pour obtenir des instructions sur le serveur, consultez [installer le rôle Hyper-V sur Windows Server](get-started/Install-the-Hyper-V-role-on-Windows-Server.md). Sur Windows, elle est disponible en tant que [fonctionnalité](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index) dans certaines versions 64 bits de Windows. Il est également disponible sous la forme d’un produit serveur autonome, téléchargeable [Microsoft Hyper-V Server](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2019).

## <a name="supported-operating-systems"></a>Systèmes d’exploitation pris en charge

De nombreux systèmes d’exploitation s’exécutent sur des machines virtuelles. En général, un système d’exploitation qui utilise une architecture x86 s’exécute sur une machine virtuelle Hyper-V. Toutefois, tous les systèmes d’exploitation qui peuvent être exécutés ne sont pas testés et pris en charge par Microsoft. Pour obtenir la liste des éléments pris en charge, consultez :

- [Machines virtuelles Linux et FreeBSD prises en charge pour Hyper-V sur Windows](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

- [Systèmes d’exploitation invités Windows pris en charge pour Hyper-V sur Windows Server](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)

## <a name="how-hyper-v-works"></a>Fonctionnement d’Hyper-V

Hyper-V est une technologie de virtualisation basée sur un hyperviseur. Hyper-V utilise l’hyperviseur Windows, qui requiert un processeur physique avec des fonctionnalités spécifiques. Pour plus d’informations sur le matériel, consultez [Configuration système requise pour Hyper-V sur Windows Server](System-requirements-for-Hyper-V-on-Windows.md).

Dans la plupart des cas, l’hyperviseur gère les interactions entre le matériel et les ordinateurs virtuels. Cet accès contrôlé par l’hyperviseur au matériel donne aux machines virtuelles l’environnement isolé dans lequel elles s’exécutent. Dans certaines configurations, une machine virtuelle ou le système d’exploitation s’exécutant sur la machine virtuelle a un accès direct aux graphiques, à la mise en réseau ou au matériel de stockage.

## <a name="what-does-hyper-v-consist-of"></a>Qu’est-ce qu’Hyper-V ?

Hyper-V a requis des éléments qui fonctionnent ensemble pour vous permettre de créer et d’exécuter des machines virtuelles. Ensemble, ces parties sont appelées « plateforme de virtualisation ». Ils sont installés en tant que jeu quand vous installez le rôle Hyper-V. Les composants requis incluent l’hyperviseur Windows, le service de gestion d’ordinateurs virtuels Hyper-V, le fournisseur WMI de virtualisation, le bus VMbus, le fournisseur de services de virtualisation (VSP) et le pilote d’infrastructure virtuelle (VID).

Hyper-V intègre également des outils pour la gestion et la connectivité. Vous pouvez les installer sur le même ordinateur que celui sur lequel le rôle Hyper-V est installé et sur les ordinateurs sur lesquels le rôle Hyper-V n’est pas installé. Ces outils sont les suivants :

- Gestionnaire Hyper-V
- [Module Hyper-V pour Windows PowerShell](https://docs.microsoft.com/powershell/module/hyper-v/index)
- [Connexion à un ordinateur virtuel](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/hyper-v-virtual-machine-connect) \(parfois appelée vmconnect\)
- [Windows PowerShell direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)

## <a name="related-technologies"></a>Technologies associées

Voici quelques-unes des technologies de Microsoft qui sont souvent utilisées avec Hyper-V :

- [Clustering avec basculement](../../failover-clustering/whats-new-in-failover-clustering.md)
- [Services Bureau à distance](../../remote/remote-desktop-services/Host-desktops-and-apps-in-Remote-Desktop-Services.md)
- [System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/overview)

Différentes technologies de stockage : volumes partagés de cluster, SMB 3,0, espaces de stockage direct

Les conteneurs Windows offrent une autre approche de la virtualisation. Consultez la bibliothèque de [conteneurs Windows](https://docs.microsoft.com/virtualization/windowscontainers/index) sur MSDN.
