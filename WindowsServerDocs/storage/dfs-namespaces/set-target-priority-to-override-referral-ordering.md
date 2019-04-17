---
title: "Définir la priorité de cible pour remplacer le classement des références"
description: "Cet article décrit comment définir la priorité de cible pour remplacer le classement des références"
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 10f5e8979ae2f6390da76276dfa193226019e5d3
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="set-target-priority-to-override-referral-ordering"></a>Définir la priorité de cible pour remplacer le classement des références

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2, WindowsServer2008

Une référence constitue une liste de cibles, organisée selon un ordre particulier, qu’un ordinateur client reçoit à partir d’un contrôleur de domaine ou d’un serveur d’espace de noms lorsque l’utilisateur accède à une racine d’espace de noms ou à un dossier avec cibles dans l’espace de noms. Dans une référence, chaque cible est classée en fonction de la méthode de classement définie pour la racine d’espace de noms ou le dossier. 

Si vous voulez préciser la façon dont les cibles doivent être ordonnées, définissez la priorité sur des cibles individuelles. Vous pouvez ainsi indiquer la préséance ou non d’une cible par rapport à toutes les autres, mais aussi sa position la plus élevée (ou la plus basse) par rapport aux autres cibles affichant un coût identique.

## <a name="to-set-target-priority-on-a-root-target-for-a-domain-based-namespace"></a>Pour définir la priorité sur une cible racine d’un espace de noms de domaine

Pour définir la priorité donnée à une cible racine d’un espace de noms de domaine, suivez la procédure décrite ci-après:

1.  Cliquez sur **Démarrer**, pointez sur **Outils d’administration**, puis cliquez sur **GestionDFS**.

2.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, cliquez sur l’espace de noms de domaine dont la priorité des cibles racines doit être définie.

3.  Dans le volet **Détails**, sous l’onglet **Serveurs d’espaces de noms**, cliquez avec le bouton droit sur la cible racine dont vous voulez modifier la priorité, puis cliquez sur **Propriétés**.

4.  Sous l’onglet **Avancé**, cliquez sur **Remplacer l’ordre des références**, puis cliquez sur la priorité de votre choix.

    -   **Première de toutes les cibles**  Spécifie que les utilisateurs doivent toujours être référencés par cette cible si la cible est disponible.
    -   **Dernière de toutes les cibles** Spécifie que les utilisateurs ne doivent jamais être référencés par cette cible à moins que toutes les autres cibles soient indisponibles.
    -   **Première parmi les cibles de même coût**  Spécifie que les utilisateurs doivent être référencés par cette cible avant les autres cibles de même coût (qui correspondent habituellement aux autres cibles du même site).
    -   **Dernière parmi les cibles de même coût**  Spécifie que les utilisateurs ne doivent jamais être référencés par cette cible si d’autres cibles de même coût sont disponibles (qui correspondent habituellement à d’autres cibles du même site).

## <a name="to-set-target-priority-on-a-folder-target"></a>Pour définir la priorité d’une cible de dossier

Pour définir la priorité d’une cible de dossier, suivez la procédure décrite ci-après:

1.  Cliquez sur **Démarrer**, pointez sur **Outils d’administration**, puis cliquez sur **GestionDFS**.

2.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, cliquez sur le dossier des cibles dont la priorité doit être définie.

3.  Dans le volet **Détails**, sous l’onglet **Cibles de dossier**, cliquez avec le bouton droit sur la cible de dossier dont vous voulez modifier la priorité, puis cliquez sur **Propriétés**.

4.  Sous l’onglet **Avancé**, cliquez sur **Remplacer l’ordre des références**, puis cliquez sur la priorité de votre choix.

> [!NOTE]
> Pour définir les priorités à l’aide de WindowsPowerShell, utilisez les applets de commande [Set-DfsnRootTarget](https://technet.microsoft.com/library/jj884266.aspx) et [Set-DfsnFolderTarget](https://technet.microsoft.com/library/jj884264.aspx) avec les paramètres **ReferralPriorityClass** et **ReferralPriorityRank**. Ces applets de commande ont été introduites dans WindowsServer2012.

## <a name="see-also"></a>Articles associés

-   [Réglage des espaces de nomsDFS](tuning-dfs-namespaces.md)
-   [Déléguer les autorisations de gestion pour les espaces de nomsDFS](delegate-management-permissions-for-dfs-namespaces.md)