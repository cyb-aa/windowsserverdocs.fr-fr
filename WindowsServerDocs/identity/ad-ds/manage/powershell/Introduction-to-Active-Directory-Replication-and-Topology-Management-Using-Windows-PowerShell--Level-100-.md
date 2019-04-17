---
ms.assetid: c54b544f-cc32-4837-bb2d-a8656b22f3de
title: "Introduction à la réplication ActiveDirectory et gestion de la topologie à l’aide de Windows PowerShell (niveau 100)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 006179bb3220f7bccfc7510e1b8ef69678321074
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="introduction-to-active-directory-replication-and-topology-management-using-windows-powershell-level-100"></a>Introduction à la réplication ActiveDirectory et gestion de la topologie à l’aide de Windows PowerShell (niveau 100)

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Windows PowerShell pour ActiveDirectory inclut la possibilité de gérer la réplication, sites, domaines et forêts, les contrôleurs de domaine et les partitions. Les utilisateurs d’outils de gestion antérieurs tels que les Sites ActiveDirectory et les Services d’un composant logiciel enfichable et repadmin.exe noteront que des fonctionnalités similaires sont désormais disponibles dans le contexte Windows PowerShell pour ActiveDirectory. En outre, les applets de commande sont compatibles avec l’existant Windows PowerShell pour les applets de commande ActiveDirectory, ainsi la création d’une expérience utilisateur rationalisée et permettant aux utilisateurs de créer facilement des scripts d’automatisation.

> [!NOTE]
> Windows PowerShell pour les applets de commande de topologie et de la réplication ActiveDirectory sont disponibles dans les environnements suivants:
> 
> -    Contrôleur de domaine Windows Server2012
> -    Windows Server2012 avec les outils d’Administration de serveur distant pour les services ADDS et ADLDS installé.
> -   Windows&reg; 8 avec les outils d’Administration de serveur distant pour les services ADDS et ADLDS installés.

## <a name="installing-the-active-directory-module-for-windows-powershell"></a>L’installation du Module ActiveDirectory pour Windows PowerShell
Le Module ActiveDirectory pour Windows PowerShell est installé par défaut lorsque le rôle serveur ADDS est installé sur un serveur qui exécute Windows Server2012. Aucune des étapes supplémentaires ne sont requises autre que l’ajout du rôle de serveur. Vous pouvez également installer le Module ActiveDirectory sur un serveur qui exécute Windows Server2012 en installant les outils d’Administration de serveur distant, et vous pouvez installer le Module ActiveDirectory sur un ordinateur exécutant Windows8 en téléchargeant et en installant le [serveur d’administration (outils distant)](https://www.microsoft.com/download/details.aspx?id=28972). Voir [Instructions](https://www.microsoft.com/download/details.aspx?id=28972)pour les étapes de l’installation.

## <a name="scenarios-for-testing-windows-powershell-for-active-directory-replication-and-topology-management-cmdlets"></a>Scénarios de test de Windows PowerShell pour les cmdlets de gestion de topologie et de la réplication ActiveDirectory
Les scénarios suivants sont destinés aux administrateurs pour se familiariser avec les nouvelles applets de commande de gestion:

-   Obtenir une liste de tous les contrôleurs de domaine et leurs sites correspondants

-   Gérer la topologie de réplication

-   Afficher le statut de réplication et les informations

## <a name="lab-requirements"></a>Configuration requise du laboratoire

-   Deux contrôleurs de domaine Windows Server2012: **DC1** et **DC2** qui font partie du domaine contoso.com et se trouvent dans le site CORPORATE dans ce domaine.

## <a name="view-domain-controllers-and-their-sites"></a>Afficher les contrôleurs de domaine et leurs sites
Dans cette étape, vous allez utiliser le Module ActiveDirectory pour Windows PowerShell pour afficher la topologie de réplication pour le domaine et les contrôleurs de domaine existants.

Pour effectuer les étapes décrites dans les procédures suivantes, vous devez être membre du groupe Admins du domaine ou disposer des autorisations équivalentes.

#### <a name="to-view-all-active-directory-sites"></a>Pour afficher tous les sites ActiveDirectory

1.  Sur **DC1**, cliquez sur **Windows PowerShell** sur la barre des tâches.

2.  Tapez la commande suivante:

    `Get-ADReplicationSite -Filter *`

    Cela retourne des informations détaillées sur chaque site. Le `Filter`paramètre est utilisé dans l’ensemble des applets de commande PowerShell d’ActiveDirectory pour limiter la liste des objets renvoyés. Dans ce cas, l’astérisque (*) indique tous les objets de site.

    > [!TIP]
    > Vous pouvez utiliser la touche Tab pour compléter automatiquement les commandes dans Windows PowerShell.
    > 
    > Exemple: Tapez `Get-ADRep`et appuyez sur Tab plusieurs fois pour parcourir les commandes correspondantes jusqu'à atteindre `Get-ADReplicationSite`. Saisie semi-automatique fonctionne également pour les noms de paramètres tels que `Filter`.

    Pour mettre en forme la sortie de la `Get-ADReplicationSite`commande sous forme de tableau et limiter l’affichage des champs spécifiques, vous pouvez diriger la sortie vers le `Format-Table`commande (ou «`ft`» pour la forme courte):

    `Get-ADReplicationSite -Filter * | ft Name`

    Cela retourne une version plus courte de la liste des sites, y compris uniquement le champ de nom.

#### <a name="to-produce-a-table-of-all-domain-controllers"></a>Pour générer un tableau de tous les contrôleurs de domaine

-   Tapez la commande suivante à le **module ActiveDirectory pour Windows PowerShell** invite de commandes:

    `Get-ADDomainController -Filter * | ft Hostname,Site`

    Cette commande retourne que les contrôleurs de domaine hôte nom, ainsi que leurs associations de sites.

## <a name="manage-replication-topology"></a>Gérer la topologie de réplication
Dans l’étape précédente, après avoir exécuté la commande, `Get-ADDomainController -Filter * | ft Hostname,Site`, **DC2** était répertorié dans le cadre de la **CORPORATE** site. Dans les procédures ci-dessous, vous allez créer un nouveau site de succursale, **BRANCH1**, créer un lien de site, définir la fréquence de réplication et de coût de lien site et déplacez **DC2** à **BRANCH1**.

Pour effectuer les étapes décrites dans les procédures suivantes, vous devez être membre du groupe Admins du domaine ou disposer des autorisations équivalentes.

#### <a name="to-create-a-new-site"></a>Pour créer un nouveau site

-   Tapez la commande suivante à le **module ActiveDirectory pour Windows PowerShell** invite de commandes:

    `New-ADReplicationSite BRANCH1`

    Cette commande crée le nouveau site de succursale, branch1.

#### <a name="to-create-a-new-site-link"></a>Pour créer un lien de site

-   Tapez la commande suivante à le **module ActiveDirectory pour Windows PowerShell** invite de commandes:

    `New-ADReplicationSiteLink 'CORPORATE-BRANCH1'  -SitesIncluded CORPORATE,BRANCH1 -OtherAttributes @{'options'=1}`

    Cette commande crée le lien de sites pour **BRANCH1** et activé sur le processus de notification de modification.

    > [!TIP]
    > Onglet permet de noms de paramètres de saisie semi-automatique comme `-SitesIncluded`et `-OtherAttributes`au lieu de les taper manuellement.

#### <a name="to-set-the-site-link-cost-and-replication-frequency"></a>Pour définir la fréquence de réplication et de coût de lien site

-   Tapez la commande suivante à le **module ActiveDirectory pour Windows PowerShell** invite de commandes:

    `Set-ADReplicationSiteLink CORPORATE-BRANCH1 -Cost 100 -ReplicationFrequencyInMinutes 15`

    Cette commande définit le coût de lien de site **BRANCH1** à **100** et définir la fréquence de réplication avec le site pour **15minutes **.

#### <a name="to-move-a-domain-controller-to-a-different-site"></a>Pour déplacer un contrôleur de domaine vers un autre site

-   Tapez la commande suivante à le **module ActiveDirectory pour Windows PowerShell** invite de commandes:

    `Get-ADDomainController DC2 | Move-ADDirectoryServer -Site BRANCH1`

    Cette commande déplace le contrôleur de domaine **DC2** à la **BRANCH1** site.

### <a name="verification"></a>Vérification

##### <a name="to-verify-site-creation-new-site-link-and-cost-and-replication-frequency"></a>Pour vérifier la création de site, nouveau lien de sites et la fréquence de réplication et de coût

-   Cliquez sur **le Gestionnaire de serveur**, cliquez sur **outils** puis cliquez sur **Services et Sites ActiveDirectory** et vérifiez les éléments suivants:

    Vérifiez que le **BRANCH1** site contient toutes les valeurs correctes des commandes Windows PowerShell.

    Vérifiez le **CORPORATE-BRANCH1** lien de sites est créé et connecte le **BRANCH1** et **CORPORATE** sites.

    Vérifiez **DC2** est désormais dans le **BRANCH1** site. Vous pouvez également ouvrir le **Module ActiveDirectory pour Windows PowerShell** et tapez la commande suivante pour vérifier **DC2** est désormais dans le **BRANCH1** site:`Get-ADDomainController -Filter * | ft Hostname,Site`.

## <a name="view-replication-status-information"></a>Afficher les informations de statut de réplication
Dans les procédures suivantes, vous allez utiliser une de Windows PowerShell pour la réplication ActiveDirectory et les applets de commande de gestion, `Get-ADReplicationUpToDatenessVectorTable DC1`, pour générer un rapport de réplication simple à l’aide de la table de vecteur de mise à jour conservée par chaque contrôleur de domaine. Cette table de vecteurs de mise à jour conserve une trace de l’USN la plus élevée origine écriture à partir de chaque contrôleur de domaine dans la forêt.

Pour effectuer les étapes décrites dans les procédures suivantes, vous devez être membre du groupe Admins du domaine ou disposer des autorisations équivalentes.

#### <a name="to-view-the-up-to-dateness-vector-table-for-a-single-domain-controller"></a>Pour afficher la table de vecteur de mise à jour pour un seul contrôleur de domaine

1.  Tapez la commande suivante à le **module ActiveDirectory pour Windows PowerShell** invite de commandes:

    `Get-ADReplicationUpToDatenessVectorTable DC1`

    Cela affiche une liste des USN les plus élevés par **DC1** pour chaque contrôleur de domaine dans la forêt. Le **Server** valeur fait référence au serveur de gestion de la table, dans ce cas **DC1**. Le **partenaire** valeur fait référence au partenaire de réplication (direct ou indirect) sur lequel les modifications ont été apportées. La valeur UsnFilter est USN la plus élevée par **DC1** du partenaire. Si un contrôleur de domaine est ajouté à la forêt, il n’apparaîtra pas dans **DC1**table jusqu'à **DC1** reçoit une modification qui provient du nouveau domaine.

#### <a name="to-view-the-up-to-dateness-vector-table-for-all-domain-controllers-in-a-domain"></a>Pour afficher la table de vecteur de mise à jour pour tous les contrôleurs de domaine dans un domaine

1.  Tapez la commande suivante à l’invite de commandes Windows PowerShell du module d’ActiveDirectory:

    `Get-ADReplicationUpToDatenessVectorTable * | sort Partner,Server | ft Partner,Server,UsnFilter`

    Cette commande remplace **DC1** avec `*`, recueillant ainsi les données de table de vecteur de mise à jour à partir de tous les contrôleurs de domaine. Les données sont triées par **partenaire** et **Server** et affiche ensuite dans un tableau.

    Le tri vous permet de comparer aisément le dernier numéro USN détecté par chaque contrôleur de domaine pour un partenaire de réplication donné. Il s’agit d’un moyen rapide de vérifier que la réplication a lieu au sein de votre environnement. Si la réplication fonctionne correctement, les valeurs UsnFilter signalées pour un partenaire de réplication donné doivent être assez semblables sur tous les contrôleurs de domaine.

## <a name="see-also"></a>Voir aussi
[Gestion avancée de la réplication ActiveDirectory et topologie à l’aide de Windows PowerShell et #40; niveau 200 et #41;](Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md)


