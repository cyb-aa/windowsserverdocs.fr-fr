---
title: Planification de la migration VPN Always On accès à distance
description: La migration de DirectAccess vers Always On VPN nécessite une planification appropriée pour déterminer vos phases de migration, ce qui permet d’identifier les problèmes avant qu’ils n’affectent l’ensemble de l’organisation.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: lizross
author: eross-msft
ms.date: 05/29/2018
ms.openlocfilehash: 80a7a8b3ee13a9d9cc99b81ab917f6443147565f
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314942"
---
# <a name="plan-the-directaccess-to-always-on-vpn-migration"></a>Planifier la migration de DirectAccess vers VPN Toujours actif (AlwaysOn)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

&#171;[ **Précédent :** vue d’ensemble de DirectAccess pour Always on la migration VPN](da-always-on-migration-overview.md)<br>
&#187;[ **Étape suivante :** migrer vers Always on VPN et désactiver DirectAccess](da-always-on-migration-deploy.md)


La migration de DirectAccess vers Always On VPN nécessite une planification appropriée pour déterminer vos phases de migration, ce qui permet d’identifier les problèmes avant qu’ils n’affectent l’ensemble de l’organisation. Le principal objectif de la migration est de maintenir la connectivité à distance au bureau pour les utilisateurs tout au long du processus. Si vous effectuez des tâches dans le désordre, une condition de concurrence peut se produire et laisser les utilisateurs distants sans aucun moyen d’accéder aux ressources de la société. Par conséquent, Microsoft recommande d’effectuer une migration côte à côte planifiée de DirectAccess vers Always On VPN. Pour plus d’informations, consultez la section déploiement de la [migration Always on VPN](da-always-on-migration-deploy.md) .

La section décrit les avantages de la séparation des utilisateurs pour la migration, les considérations de configuration standard et les améliorations apportées aux fonctionnalités de Always On VPN. La phase de planification de la migration comprend les éléments suivants :

1.  [!INCLUDE [build-migration-rings-shortdesc-include](../includes/build-migration-rings-shortdesc-include.md)]

2.  [!INCLUDE [review-feature-mapping-shortdesc-include](../includes/review-feature-mapping-shortdesc-include.md)] 

3.  [!INCLUDE [review-the-enhancements-shortdesc-include](../includes/review-the-enhancements-shortdesc-include.md)] 

4.  [!INCLUDE [review-the-technology-overview-shortdesc-include](../includes/review-the-technology-overview-shortdesc-include.md)]

## <a name="build-migration-rings"></a>Sonneries de migration de build
Les anneaux de migration sont utilisés pour diviser l’effort de migration du client VPN Always On en plusieurs phases. Au moment où vous accédez à la dernière phase, votre processus doit être correctement testé et cohérent.

Cette section fournit une approche permettant de séparer les utilisateurs en phases de migration, puis de gérer ces phases. Quelle que soit la méthode de séparation des phases utilisateur choisie, conservez un seul groupe d’utilisateurs VPN pour faciliter la gestion lorsque la migration est terminée.

>[!NOTE] 
>La _phase_ Word n’a pas pour but d’indiquer qu’il s’agit d’un processus long. Si vous parcourez chaque phase dans quelques jours ou quelques mois, Microsoft vous recommande de tirer parti de la migration côte à côte et d’utiliser une approche progressive.

### <a name="benefits-of-dividing-the-migration-effort-into-multiple-phases"></a>Avantages de la Division de l’effort de migration en plusieurs phases

-   **Protection contre les pannes en masse.** En divisant une migration en plusieurs phases, le nombre de personnes qu’un problème généré par la migration peut affecter est bien plus petit.

-   **Amélioration du processus ou communication à partir de commentaires.** Dans l’idéal, les utilisateurs n’ont même pas remarqué que la migration s’est produite. Toutefois, si leur expérience n’était pas optimale, les commentaires de ces utilisations vous donnent la possibilité d’améliorer votre planification et d’éviter les problèmes à l’avenir.

### <a name="tips-for-building-your-migration-ring"></a>Conseils pour la création de votre anneau de migration

-   **Identifiez les utilisateurs distants.** Commencez par séparer les utilisateurs en deux compartiments : ceux qui sont souvent dans le bureau et ceux qui ne le font pas. Le processus de migration est le même pour les deux groupes, mais il est probable que les clients distants ne reçoivent pas la mise à jour de la même façon que pour ceux qui se connectent plus fréquemment. Dans l’idéal, chaque phase de migration doit inclure des membres de chaque compartiment.

-  **Hiérarchiser les utilisateurs.** La direction et les autres utilisateurs ayant un impact important sont généralement les derniers utilisateurs migrés. Toutefois, lors de la hiérarchisation des utilisateurs, pensez à leur impact sur la productivité de l’entreprise en cas d’échec de la migration de leur ordinateur client. Par exemple, si vous avez une évaluation de 1 à 3, la valeur 1 signifie que l’employé n’est pas en mesure de travailler et 3 signifie aucune interruption de travail immédiate, un analyste d’entreprise utilisant uniquement des applications métier internes, à distance, est un 1, alors qu’un commercial utilise un Cloud l’application serait 3.

-   **Migrer chaque service ou division en plusieurs phases.** Microsoft vous recommande vivement de ne pas migrer tout un service en même temps. Si un problème se pose, vous ne voulez pas qu’il entrave le travail à distance pour l’ensemble du service. Au lieu de cela, vous devez migrer chaque service ou division dans au moins deux phases.

-   **Augmentez progressivement le nombre d’utilisateurs.** La plupart des scénarios de migration classiques commencent par les membres de l’organisation informatique, puis passent à des utilisateurs professionnels suivis de la direction et d’autres utilisateurs à fort impact. Chaque phase de migration implique généralement de plus en plus de personnes. Par exemple, la première phase peut inclure dix utilisateurs et le groupe final peut inclure 5 000 utilisateurs. Pour simplifier le déploiement, créez un groupe de sécurité utilisateurs VPN unique et ajoutez des utilisateurs au fur et à mesure de leur arrivée. De cette façon, vous vous retrouvez avec un seul groupe d’utilisateurs VPN auquel vous pouvez ajouter des membres à l’avenir.

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../includes/always-on-vpn-standard-config-considerations-include.md)]


## <a name="next-step"></a>Étape suivante

|Si vous souhaitez...  |Alors consultez...  |
|---------|---------|
|Démarrer la migration vers Always On VPN     |[Migrez vers Always on VPN et désactivation de DirectAccess](da-always-on-migration-deploy.md). La migration de DirectAccess vers Always On VPN nécessite un processus spécifique pour migrer les clients, ce qui permet de réduire les conditions de concurrence qui résultent de l’exécution des étapes de migration dans le désordre.         |
|En savoir plus sur les fonctionnalités de Always On VPN et DirectAccess    |[Comparaison des fonctionnalités de Always on VPN et DirectAccess](../vpn/vpn-map-da.md). Dans les versions précédentes de l’architecture VPN Windows, les limitations de la plateforme compliquaient la fourniture des fonctionnalités nécessaires pour remplacer DirectAccess (comme les connexions automatiques initiées avant la connexion des utilisateurs). VPN Toujours actif (AlwaysOn), toutefois, a atténué la plupart de ces limitations ou étendu la fonctionnalité VPN au-delà des capacités de DirectAccess.         |



---