---
title: Planification de migration toujours sur VPN d’accès à distance
description: Migration de DirectAccess vers VPN Always On nécessite une planification appropriée déterminer vos phases de migration, qui vous aide à identifier les problèmes avant qu’ils affectent toute l’organisation.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/29/2018
ms.openlocfilehash: 494dc7916b505991c22b07bec738c2300d660ec1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835670"
---
# <a name="plan-the-directaccess-to-always-on-vpn-migration"></a>Planifier la migration de DirectAccess vers VPN Toujours actif (AlwaysOn)

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

&#171;[ **Précédente :** Vue d’ensemble de DirectAccess pour la migration VPN Always On](da-always-on-migration-overview.md)<br>
&#187;[ **Suivant :** Migrer vers VPN Always On et mise hors service de DirectAccess](da-always-on-migration-deploy.md)


Migration de DirectAccess vers VPN Always On nécessite une planification appropriée déterminer vos phases de migration, qui vous aide à identifier les problèmes avant qu’ils affectent toute l’organisation. Le principal objectif de la migration est de maintenir la connectivité à distance au bureau pour les utilisateurs tout au long du processus. Si vous effectuez des tâches dans le désordre, une condition de concurrence peut se produire et laisser les utilisateurs distants sans aucun moyen d’accéder aux ressources de la société. Par conséquent, Microsoft recommande d’effectuer une migration planifiée, côte à côte de DirectAccess pour VPN Always On. Pour plus d’informations, consultez le [migration de déploiement VPN Always On](da-always-on-migration-deploy.md) section.

La section décrit les avantages de la séparation des utilisateurs pour la migration, les considérations relatives à la configuration standard et les améliorations de la fonctionnalité VPN Always On. La phase de planification de migration inclut :

1.  [!INCLUDE [build-migration-rings-shortdesc-include](../includes/build-migration-rings-shortdesc-include.md)]

2.  [!INCLUDE [review-feature-mapping-shortdesc-include](../includes/review-feature-mapping-shortdesc-include.md)] 

3.  [!INCLUDE [review-the-enhancements-shortdesc-include](../includes/review-the-enhancements-shortdesc-include.md)] 

4.  [!INCLUDE [review-the-technology-overview-shortdesc-include](../includes/review-the-technology-overview-shortdesc-include.md)]

## <a name="build-migration-rings"></a>Générer des anneaux de migration
Anneaux de migration sont utilisés pour diviser l’effort de migration du client VPN Always On en plusieurs phases. Au moment où que vous obtenez à la dernière phase, votre processus doit être bien testée et cohérents.

Cette section fournit une approche pour la séparation des utilisateurs en phases de migration et de gestion de ces phases. Quelle que soit la méthode de séparation de phase utilisateur que vous choisissez, maintenir un seul groupe d’utilisateurs VPN pour faciliter la gestion lors de la migration est terminée.

>[!NOTE] 
>Le mot _phase_ n’est pas destinée à indiquer qu’il s’agit d’un processus long. Si vous parcourez chaque phase dans quelques jours ou quelques mois, Microsoft vous recommande de tirer parti de la migration de côte à côte et d’utiliser une approche progressive.

### <a name="benefits-of-dividing-the-migration-effort-into-multiple-phases"></a>Avantages de la division de l’effort de migration en plusieurs phases

-   **Protection de la masse de panne.** En divisant une migration en plusieurs phases, le nombre de personnes qu'un problème de migration généré peut affecter est beaucoup plus petit.

-   **Amélioration de processus ou de communication à partir des commentaires.** Dans l’idéal, les utilisateurs n’ont pas encore remarqué que la migration s’est produite. Toutefois, si leur expérience était moins optimal, des commentaires à partir de ces utilisations vous donne la possibilité d’améliorer votre planification et d’éviter les problèmes à l’avenir.

### <a name="tips-for-building-your-migration-ring"></a>Conseils pour la création de votre sonnerie de migration

-   **Identifiez les utilisateurs à distance.** Commencez par la séparation des utilisateurs en deux compartiments : ceux qui sont souvent fournies dans le bureau et ceux qui n’ont pas. Le processus de migration est le même pour les deux groupes, mais il est susceptible de prendre plus de temps pour les clients distants recevoir la mise à jour que pour ceux qui se connectent plus fréquemment. Chaque phase de migration, dans l’idéal, doit inclure les membres de chaque compartiment.

-  **Hiérarchiser les utilisateurs.** Leadership et autres utilisateurs à fort impact sont généralement entre les dernière utilisateurs migrés. Lors de la hiérarchisation des utilisateurs, toutefois, déterminer leur impact de productivité d’entreprise si la migration de leur ordinateur client tombe en panne. Par exemple, si vous aviez une évaluation de 1 à 3, avec 1 signifiant que l’employé ne serait pas en mesure de travail et 3, ce qui signifie qu’aucune interruption immédiate de travail, un analyste d’entreprise en utilisant uniquement applications internes line-of-business (LOB) à distance serait 1, tandis qu’un commercial à l’aide d’un cloud  application serait un 3.

-   **Migrer chaque département ou unité commerciale en plusieurs phases.** Microsoft recommande vivement que vous ne migrez pas un département en même temps. Si un problème doit se produire, vous ne souhaitez pas qu’il afin d’empêcher le travail à distance pour l’ensemble du service. Migrer à la place, chaque département ou unité commerciale en au moins deux phases.

-   **Augmentez progressivement le nombre d’utilisateurs.** Scénarios de migration plus classiques démarrer avec les membres de l’organisation informatique et d’accéder aux utilisateurs en entreprise suivies de leadership et d’autres utilisateurs à fort impact. Chaque phase de migration implique généralement les gens progressivement plus. Par exemple, la première phase peut inclure des dix utilisateurs, et le groupe final peut inclure de 5 000 utilisateurs. Pour simplifier le déploiement, créer un groupe de sécurité utilisateurs VPN unique et ajouter des utilisateurs en tant que leur phase arrive. De cette façon, vous vous retrouvez avec un seul groupe d’utilisateurs VPN auquel vous pouvez ajouter des membres à l’avenir.

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../includes/always-on-vpn-standard-config-considerations-include.md)]


## <a name="next-step"></a>Étape suivante

|Si vous souhaitez...  |Alors consultez...  |
|---------|---------|
|Démarrer la migration vers VPN Always On     |[Migrer vers VPN Always On et mise hors service de DirectAccess](da-always-on-migration-deploy.md). Migration de DirectAccess vers VPN Always On nécessite un processus spécifique pour migrer des clients, qui permet de réduire les conditions de concurrence surviennent d’effectuer les étapes de migration en désordre.         |
|En savoir plus sur les fonctionnalités de VPN Always On et DirectAccess    |[Comparaison des fonctionnalités Always On VPN et DirectAccess](../vpn/vpn-map-da.md). Dans les versions précédentes de l’architecture VPN Windows, les limitations de la plateforme compliquaient la fourniture des fonctionnalités nécessaires pour remplacer DirectAccess (comme les connexions automatiques initiées avant la connexion des utilisateurs). VPN Toujours actif (AlwaysOn), toutefois, a atténué la plupart de ces limitations ou étendu la fonctionnalité VPN au-delà des capacités de DirectAccess.         |



---