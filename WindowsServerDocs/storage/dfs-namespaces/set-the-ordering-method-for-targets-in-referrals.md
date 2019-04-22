---
title: Définir la méthode de classement des cibles contenues dans les références
description: Cet article décrit comment définir la méthode de classement des cibles contenues dans les références.
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 06e7aa1309b453da649537d5ae9b22acce830530
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816860"
---
# <a name="set-the-ordering-method-for-targets-in-referrals"></a>Définir la méthode de classement des cibles contenues dans les références

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Une référence constitue une liste de cibles, organisée selon un ordre particulier, qu’un ordinateur client reçoit à partir d’un contrôleur de domaine ou d’un serveur d’espace de noms lorsque l’utilisateur accède à une racine d’espace de noms ou à un dossier avec cibles. Une fois en possession de la référence, le client tente d’accéder à la première cible de la liste. Si cette cible n’est pas disponible, le client tente d’accéder à la suivante.
Les cibles sur le site du client sont toujours répertoriées en premier dans une référence. Les cibles en dehors du site du client sont répertoriées en fonction de la méthode de classement.

Utilisez les sections suivantes pour spécifier l’ordre dans lequel les cibles doivent être référencées pour les clients et pour comprendre les différentes méthodes de classement des références de cibles :

## <a name="to-set-the-ordering-method-for-targets-in-namespace-root-referrals"></a>Pour définir la méthode de classement des cibles contenues dans les références de racines d’espaces de noms

Utilisez la procédure suivante pour définir la méthode de classement sur la racine d’espace de noms :

1.  Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestion du système de fichiers distribués DFS**.

2.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, cliquez avec le bouton droit sur un espace de noms, puis cliquez sur **Propriétés**.

3.  Sous l’onglet **Références**, sélectionnez une méthode de classement.

> [!NOTE]
> Pour utiliser Windows PowerShell afin de définir la méthode de classement des cibles dans les références de racines d’espaces de noms, utilisez l’applet de commande [Set-DfsnRoot](https://technet.microsoft.com/library/jj884281.aspx) avec l’un des paramètres suivants :
   -   **EnableSiteCosting** Spécifie la méthode de classement **Moindre coût**
   -   **EnableInsiteReferrals** Spécifie la méthode de classement **Exclure les cibles en dehors du site du client**
   -   Si vous omettez l’un de ces paramètres, la méthode de classement des références **Ordre aléatoire** est spécifiée. 

Le module DFSN Windows PowerShell a été introduit dans Windows Server 2012.
   
## <a name="to-set-the-ordering-method-for-targets-in-folder-referrals"></a>Pour définir la méthode de classement des cibles contenues dans les références de dossiers

Les dossiers avec cibles héritent, de la racine de l’espace de noms, de la méthode de classement. Vous pouvez remplacer la méthode de classement en procédant comme suit :

1.  Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestion du système de fichiers distribués DFS**.

2.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, cliquez avec le bouton droit sur un dossier avec cibles, puis cliquez sur **Propriétés**.

3.  Sous l’onglet **Références**, cochez la case **Exclure les cibles en dehors du site du client**.

> [!NOTE]
> Pour utiliser Windows PowerShell afin d’exclure les cibles de dossier en dehors du site du client, utilisez l’applet de commande [Set-DfsnFolder – EnableInsiteReferrals](https://technet.microsoft.com/library/jj884283.aspx).

## <a name="target-referral-ordering-methods"></a>Méthodes de classement des références de cibles

Les trois méthodes de classement sont :

-   Ordre aléatoire
-   Moindre coût
-   Exclure les cibles en dehors du site du client

## <a name="random-order"></a>Ordre aléatoire

Dans cette méthode, les cibles sont classées comme suit :

1.  Cibles dans le même site Active Directory Directory Services (AD DS) que le client sont répertoriées dans un ordre aléatoire en haut de la référence.
2.  Les cibles situées en dehors du site du client sont répertoriées dans un ordre aléatoire.

Si aucun serveur cible de même site n’est disponible, l’ordinateur client est référencé sur un serveur cible aléatoire, quel que soit le coût de la connexion ou la distance de la cible.

## <a name="lowest-cost"></a>Moindre coût

Dans cette méthode, les cibles sont classées comme suit :

1.  Les cibles situées dans le même site que le client sont répertoriées dans un ordre aléatoire en haut de la référence.
2.  Les cibles situées en dehors du site du client sont répertoriées du coût le plus bas au coût le plus élevé. Les références de même coût sont regroupées et les cibles sont répertoriées dans un ordre aléatoire au sein de chaque groupe.

> [!NOTE]
> Les coûts de lien de site ne sont pas affichés dans le composant logiciel enfichable Gestion du système de fichiers distribués DFS. Pour les afficher, utilisez le composant logiciel enfichable Sites et services Active Directory.

## <a name="exclude-targets-outside-of-the-clients-site"></a>Exclure les cibles en dehors du site du client

Dans cette méthode, les références contiennent uniquement les cibles situées dans le même site que le client. Ces cibles de même site sont répertoriées dans un ordre aléatoire. S’il n’existe aucune cible de même site, le client ne reçoit pas de référence et ne peut pas accéder à cette partie de l’espace de noms.

> [!NOTE]
> Les cibles dont la priorité est définie sur « Première de toutes les cibles » ou « Dernière de toutes les cibles » sont répertoriées dans la référence, même si la méthode de classement est définie sur **Exclure les cibles en dehors du site du client**.

## <a name="see-also"></a>Voir aussi 

-   [Réglage des espaces de noms DFS](tuning-dfs-namespaces.md)
-   [Déléguer des autorisations de gestion pour les espaces de noms DFS](delegate-management-permissions-for-dfs-namespaces.md)