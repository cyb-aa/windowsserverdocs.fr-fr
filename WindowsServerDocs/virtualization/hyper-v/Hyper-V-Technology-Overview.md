---
title: Vue d’ensemble de la technologie Hyper-V
description: Décrit ce que Hyper-V, comment procéder, principales fonctionnalités et utilisations courantes.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac069fed-7bf5-4cc3-aff5-25a2766040b8
author: KBDAzure
ms.author: kathydav
ms.date: 11/29/2016
ms.openlocfilehash: 9ae3c9dce36ad7d67a19ce167c9cb875b3c91810
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864310"
---
# <a name="hyper-v-technology-overview"></a>Vue d’ensemble de la technologie Hyper-V

>S'applique à : Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Hyper-V est un produit de virtualisation de matériel de Microsoft. Il vous permet de créer et exécuter une version logicielle d’un ordinateur, appelé un *machine virtuelle*. Chaque machine virtuelle agit comme un ordinateur terminé, un système d’exploitation et les programmes en cours d’exécution. Lorsque vous avez besoin de ressources de calcul, machines virtuelles offrent plus de souplesse, permettre de gagner du temps et d’argent et sont un moyen plus efficace d’utiliser un matériel que simplement en exécutant un système d’exploitation sur du matériel physique.

Hyper-V s’exécute à chaque machine virtuelle dans son propre espace isolé, ce qui signifie que vous pouvez exécuter plusieurs machines virtuelles sur le même matériel en même temps. Vous souhaiterez peut-être le faire pour éviter les problèmes comme un incident affectant les autres charges de travail, ou pour accorder l’accès de personnes, des groupes ou des services différents pour différents systèmes.

## <a name="some-ways-hyper-v-can-help-you"></a>Quelques méthodes Hyper-V peut vous aider à

Hyper-V peut vous aider à :

- **Établir ou développer un environnement de cloud privé.** Fournir des services informatiques plus flexibles et à la demande en déplaçant vers ou à développer votre utilisation des ressources partagées et ajuster l’utilisation en tant que les modifications apportées à la demande.

- **Pour utiliser plus efficacement votre matériel.** Consolidation des serveurs et des charges de travail sur des ordinateurs physiques moins nombreuses et plus puissants à utiliser moins de puissance et un espace physique.

- **Améliorer la continuité d’activité.** Réduire l’impact des temps d’arrêt planifiés et de vos charges de travail.

- **Établir ou développer une infrastructure de bureau virtuelle (VDI).** Utilisez une stratégie de bureau centralisée avec VDI peut vous aider à augmenter la réactivité et la sécurité des données, ainsi que de simplifier la conformité aux réglementations et de gérer des systèmes d’exploitation et applications. Déployer Hyper-V et l’hôte de virtualisation Bureau à distance (hôte de virtualisation Bureau à distance) sur le même serveur de proposer des bureaux virtuels personnels ou les pools de bureaux virtuels à vos utilisateurs.

- **Développement et test plus efficace.** Reproduire différents environnements informatiques sans avoir à acheter ou mettre à jour de tout le matériel qu'est nécessaire lorsque vous avez utilisé uniquement les systèmes physiques.

## <a name="hyper-v-and-other-virtualization-products"></a>Hyper-V et autres produits de virtualisation

Hyper-V dans Windows et Windows Server remplace les anciens produits de virtualisation de matériel, telles que Microsoft Virtual PC, Microsoft Virtual Server et Windows Virtual PC. Hyper-V offre des fonctionnalités de mise en réseau, de performances, de stockage et de sécurité non disponibles dans ces produits plus anciens.

Hyper-V et la plupart des applications de virtualisation de fournisseurs tiers qui nécessitent les mêmes fonctionnalités de processeur ne sont pas compatibles. C’est parce que les fonctionnalités du processeur, connues en tant qu’extensions de virtualisation de matériel, sont conçues pour ne pas être partagée. Pour plus d’informations, consultez [des applications de virtualisation ne fonctionnent pas avec Hyper-V, Device Guard et Credential Guard](https://support.microsoft.com/kb/3204980).

## <a name="what-features-does-hyper-v-have"></a>Quelles fonctionnalités ont pour Hyper-V ?

Hyper-V offre de nombreuses fonctionnalités. Il s’agit d’une vue d’ensemble, regroupé par les fonctionnalités de fourniront ou vous y aider.

**Environnement informatique** -machine virtuelle A Hyper-V inclut les composants de base mêmes en tant qu’un ordinateur physique, telles que la mémoire, processeur, stockage et le réseau. Toutes ces parties ont des fonctionnalités et options que vous pouvez configurer différentes façons de répondre à des besoins différents. Mise en réseau et stockage peuvent chacune être considérée comme catégories de leurs propres, en raison des différentes manières dont vous pouvez les configurer.

**Sauvegarde et récupération d’urgence** -récupération d’urgence, réplica Hyper-V crée des copies des ordinateurs virtuels, destinés à être stockées dans un autre emplacement physique, afin de pouvoir restaurer la machine virtuelle à partir de la copie. Pour la sauvegarde, Hyper-V propose deux types. Une utilise États enregistrés et l’autre Volume Shadow Copy Service (VSS) afin de pouvoir effectuer des sauvegardes cohérentes pour les programmes qui prennent en charge VSS.

**Optimisation** -chaque système d’exploitation pris en charge a un jeu personnalisé de services et des pilotes, appelés *services d’intégration*, qui le rendent plus facile à utiliser le système d’exploitation dans un ordinateur virtuel Hyper-V.

**Portabilité** - fonctionnalités, telles que live migration, la migration du stockage et importation/exportation facilitent la déplacer ou distribuer une machine virtuelle.

**Connectivité à distance** -Hyper-V inclut la connexion de Machine virtuelle, un outil de connexion à distance pour une utilisation avec Windows et Linux. Par conséquent, contrairement au Bureau à distance, cela donne de l’outil console d’accès, vous pouvez voir ce qui se passe dans l’invité même lorsque le système d’exploitation n’est pas encore démarré.

**Sécurité** -le démarrage sécurisé et des machines virtuelles protégées à protéger contre les logiciels malveillants et autres accès non autorisé pour une machine virtuelle et ses données.

Pour obtenir un résumé des fonctionnalités introduites dans cette version, consultez [quelles sont les nouveautés dans Hyper-V sur Windows Server](What-s-new-in-Hyper-V-on-Windows.md). Certaines fonctionnalités ou les parties ont une limite au nombre peut être configuré. Pour plus d’informations, consultez [planifier extensibilité Hyper-V dans Windows Server 2016](plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md).

## <a name="how-to-get-hyper-v"></a>Comment obtenir Hyper-V

Hyper-V est disponible dans Windows Server et Windows, comme un rôle de serveur disponible pour x64 versions de Windows Server. Pour obtenir des instructions de serveur, consultez [installer le rôle Hyper-V sur Windows Server](get-started/Install-the-Hyper-V-role-on-Windows-Server.md). Sur Windows, il est disponible en tant que [fonctionnalité](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index) dans certaines versions 64 bits de Windows. Il est également disponible en tant que produit téléchargeable, serveur autonome, [Microsoft Hyper-V Server](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2019).

## <a name="supported-operating-systems"></a>Systèmes d’exploitation pris en charge

De nombreux systèmes d’exploitation s’exécute sur les machines virtuelles. En règle générale, un système d’exploitation qui utilise un x86 architecture s’exécutera sur un ordinateur virtuel Hyper-V. Pas tous les systèmes d’exploitation qui peuvent être exécutées sont testés et pris en charge par Microsoft, toutefois. Pour les listes de ce qui est pris en charge, consultez :

- [Machines virtuelles prises en charge Linux et FreeBSD pour Hyper-V sur Windows](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

- [Systèmes d’exploitation d’invités Windows pris en charge pour Hyper-V sur Windows Server](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)

## <a name="how-hyper-v-works"></a>Fonctionne de Hyper-V

Hyper-V est une technologie de virtualisation basée sur hyperviseur. Hyper-V utilise l’hyperviseur Windows, ce qui nécessite un processeur physique avec des fonctionnalités spécifiques. Pour plus d’informations de matériel, consultez [configuration système requise pour Hyper-V sur Windows Server](System-requirements-for-Hyper-V-on-Windows.md).

Dans la plupart des cas, l’hyperviseur gère les interactions entre le matériel et les machines virtuelles. Cet accès contrôlé par l’hyperviseur au matériel donne des machines virtuelles l’environnement isolé dans lequel ils sont exécutés. Dans certaines configurations, un ordinateur virtuel ou le système d’exploitation en cours d’exécution dans la machine virtuelle a un accès direct au matériel de graphiques, de mise en réseau ou de stockage.

## <a name="what-does-hyper-v-consist-of"></a>Ce que Hyper-V n’est constitué de

Hyper-V a requis des parties qui collaborent afin de pouvoir créer et exécuter des machines virtuelles. Ensemble, ces parties sont appelées de la plateforme de virtualisation. Ils sont installés en tant qu’ensemble lorsque vous installez le rôle Hyper-V. Les parties requises sont Windows hyperviseur, Service de gestion de Machine virtuelle Hyper-V, le fournisseur WMI de virtualisation, machine virtuelle bus VMbus, fournisseur de services de virtualisation (VSP) et infrastructure virtuelle (VID) de pilote.

Hyper-V a également des outils de gestion et de connectivité. Vous pouvez les installer sur le même ordinateur que rôle Hyper-V est installé sur et sur les ordinateurs sans le rôle Hyper-V installé. Ces outils sont :

- Gestionnaire Hyper-V
- [Module Hyper-V pour Windows PowerShell](https://docs.microsoft.com/powershell/module/hyper-v/index)
- [Connexion de Machine virtuelle](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/hyper-v-virtual-machine-connect) \(parfois appelé VMConnect\)
- [Windows PowerShell Direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)

## <a name="related-technologies"></a>Technologies associées

Il s’agit des technologies Microsoft qui sont souvent utilisées avec Hyper-v :

- [Clustering de basculement](../../failover-clustering/whats-new-in-failover-clustering.md)
- [Services Bureau à distance](../../remote/remote-desktop-services/Host-desktops-and-apps-in-Remote-Desktop-Services.md)
- [System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/overview)

Différentes technologies de stockage :, SMB 3.0, les volumes partagés de cluster des espaces de stockage direct

Les conteneurs Windows offrent une autre approche pour la virtualisation. Consultez le [Windows conteneurs](https://docs.microsoft.com/virtualization/windowscontainers/index) bibliothèque sur MSDN.
