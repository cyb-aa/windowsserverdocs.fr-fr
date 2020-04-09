---
ms.assetid: c54b544f-cc32-4837-bb2d-a8656b22f3de
title: Gestion de la topologie et de la réplication Active Directory avec Windows PowerShell (niveau 100)
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 63ecad01ec6d4b4d72b7aaff315b74541cb0fadc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822982"
---
# <a name="introduction-to-active-directory-replication-and-topology-management-using-windows-powershell-level-100"></a>Gestion de la topologie et de la réplication Active Directory avec Windows PowerShell (niveau 100)

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Windows PowerShell pour Active Directory offre la possibilité de gérer la réplication, les sites, les domaines et forêts, les contrôleurs de domaine et les partitions. Les utilisateurs d’outils de gestion antérieurs, tels que le composant logiciel enfichable Sites et services Active Directory et repadmin.exe, noteront que des fonctions similaires sont à présent disponibles depuis le contexte Windows PowerShell pour Active Directory. En outre, les applets de commande sont compatibles avec les applets de commande Windows PowerShell pour Active Directory existantes, ce qui crée une expérience utilisateur rationalisée et permet aux clients de générer aisément des scripts d’automatisation.

> [!NOTE]
> Les applets de commande de topologie et de réplication Windows PowerShell pour Active Directory sont disponibles dans les environnements suivants :
> 
> -    Contrôleur de domaine Windows Server 2012
> -    Windows Server 2012 avec le Outils d’administration de serveur distant pour AD DS et AD LDS installés.
> -   Windows&reg; 8 avec le Outils d’administration de serveur distant pour AD DS et AD LDS installé.

## <a name="installing-the-active-directory-module-for-windows-powershell"></a>Installation du module Active Directory pour Windows PowerShell
Le module Active Directory pour Windows PowerShell est installé par défaut lorsque le rôle serveur AD DS est installé sur un serveur qui exécute Windows Server 2012. L’ajout du rôle serveur est la seule étape supplémentaire requise. Vous pouvez également installer le module Active Directory sur un serveur qui exécute Windows Server 2012 en installant le Outils d’administration de serveur distant et vous pouvez installer le module Active Directory sur un ordinateur exécutant Windows 8 en téléchargeant et en installant les [Outils d’administration de serveur distant (RSAT)](https://www.microsoft.com/download/details.aspx?id=28972). Voir [Instructions](https://www.microsoft.com/download/details.aspx?id=28972)pour les étapes d’installation.

## <a name="scenarios-for-testing-windows-powershell-for-active-directory-replication-and-topology-management-cmdlets"></a>Scénarios de test des applets de commande de gestion de la topologie et de la réplication Windows PowerShell pour Active Directory
Les scénarios suivants sont destinés aux administrateurs pour qu’ils se familiarisent avec les nouvelles applets de commande de gestion :

-   Obtenir une liste de tous les contrôleurs de domaine et leurs sites correspondants

-   Gérer la topologie de réplication

-   Afficher les informations et l’état de réplication

## <a name="lab-requirements"></a>Configuration de laboratoire requise

-   Deux contrôleurs de domaine Windows Server 2012 : **DC1** et **DC2** qui font partie du domaine contoso.com et qui résident dans le site d’entreprise au sein de ce domaine.

## <a name="view-domain-controllers-and-their-sites"></a>Afficher les contrôleurs de domaine et leurs sites
Dans cette étape, vous allez utiliser le module Active Directory pour Windows PowerShell pour afficher les contrôleurs de domaine existants et la topologie de réplication pour le domaine.

Pour effectuer les étapes des procédures suivantes, vous devez être membre du groupe Administrateurs du domaine ou disposer des autorisations équivalentes.

#### <a name="to-view-all-active-directory-sites"></a>Pour afficher tous les sites Active Directory

1.  Sur **DC1**, cliquez sur **Windows PowerShell** dans la barre des tâches.

2.  Tapez la commande suivante :

    `Get-ADReplicationSite -Filter *`

    Des informations détaillées sur chaque site sont retournées. Le paramètre `Filter` est utilisé dans toutes les applets de commande PowerShell Active Directory afin de limiter la liste des objets renvoyés. Dans ce cas, l’astérisque (*) indique tous les objets de sites.

    > [!TIP]
    > Vous pouvez utiliser la touche Tab pour compléter automatiquement les commandes dans Windows PowerShell.
    > 
    > Exemple : tapez `Get-ADRep` et appuyez sur Tab plusieurs fois pour parcourir les commandes correspondantes jusqu’à atteindre `Get-ADReplicationSite`. La saisie semi-automatique fonctionne également pour les noms de paramètres tels que `Filter`.

    Pour mettre en forme la sortie de la commande `Get-ADReplicationSite` en tant que table et limiter l’affichage à des champs spécifiques, vous pouvez diriger la sortie vers la commande `Format-Table` (ou «`ft`» pour Short) :

    `Get-ADReplicationSite -Filter * | ft Name`

    Une version plus courte de la liste de sites est retournée, contenant uniquement le champ Name.

#### <a name="to-produce-a-table-of-all-domain-controllers"></a>Pour générer un tableau de tous les contrôleurs de domaine

-   Tapez la commande suivante à l’invite de commandes du **module Active Directory pour Windows PowerShell** :

    `Get-ADDomainController -Filter * | ft Hostname,Site`

    Cette commande retourne le nom d’hôte des contrôleurs de domaine ainsi que leurs associations de sites.

## <a name="manage-replication-topology"></a>Gérer la topologie de réplication
À l’étape précédente, après l’exécution de la commande, `Get-ADDomainController -Filter * | ft Hostname,Site`, **DC2** était répertorié comme faisant partie du site **CORPORATE**. Dans les procédures ci-dessous, vous allez créer un nouveau site de succursale, **BRANCH1**, créer un lien de site, définir le coût du lien de site et la fréquence de réplication, puis déplacer **DC2** vers **BRANCH1**.

Pour effectuer les étapes des procédures suivantes, vous devez être membre du groupe Administrateurs du domaine ou disposer des autorisations équivalentes.

#### <a name="to-create-a-new-site"></a>Pour créer un site

-   Tapez la commande suivante à l’invite de commandes du **module Active Directory pour Windows PowerShell** :

    `New-ADReplicationSite BRANCH1`

    Cette commande crée le site de la nouvelle filiale, branch1.

#### <a name="to-create-a-new-site-link"></a>Pour créer un lien de site

-   Tapez la commande suivante à l’invite de commandes du **module Active Directory pour Windows PowerShell** :

    `New-ADReplicationSiteLink 'CORPORATE-BRANCH1'  -SitesIncluded CORPORATE,BRANCH1 -OtherAttributes @{'options'=1}`

    Cette commande crée le lien de site vers **BRANCH1** et active le processus de notification de modification.

    > [!TIP]
    > Utilisez la touche Tab pour effectuer une saisie semi-automatique des noms de paramètres tels que `-SitesIncluded` et `-OtherAttributes` plutôt que les taper manuellement.

#### <a name="to-set-the-site-link-cost-and-replication-frequency"></a>Pour définir le coût du lien de site et la fréquence de réplication

-   Tapez la commande suivante à l’invite de commandes du **module Active Directory pour Windows PowerShell** :

    `Set-ADReplicationSiteLink CORPORATE-BRANCH1 -Cost 100 -ReplicationFrequencyInMinutes 15`

    Cette commande affecte la valeur **100** au coût du lien de site vers **BRANCH1** et définit la fréquence de réplication avec le site à **15 minutes**.

#### <a name="to-move-a-domain-controller-to-a-different-site"></a>Pour déplacer un contrôleur de domaine vers un autre site

-   Tapez la commande suivante à l’invite de commandes du **module Active Directory pour Windows PowerShell** :

    `Get-ADDomainController DC2 | Move-ADDirectoryServer -Site BRANCH1`

    Cette commande déplace le contrôleur de domaine, **DC2** vers le site **BRANCH1** .

### <a name="verification"></a>Vérification

##### <a name="to-verify-site-creation-new-site-link-and-cost-and-replication-frequency"></a>Pour vérifier la création du site, le lien vers le nouveau site, le coût et la fréquence de réplication

-   Cliquez sur **Gestionnaire de serveur**, sur **Outils**, puis sur **Sites et services Active Directory** et vérifiez les points suivants :

    Vérifiez que le site **BRANCH1** contient toutes les valeurs correctes des commandes Windows PowerShell.

    Vérifiez que le lien de site **CORPORATE-BRANCH1** est créé et connecte les sites **BRANCH1** et **CORPORATE** .

    Vérifiez que **DC2** figure maintenant dans le site **BRANCH1** . En guise d’alternative, vous pouvez ouvrir le **Module Active Directory pour Windows PowerShell** et taper la commande suivante pour vérifier que **DC2** figure désormais dans le site **BRANCH1** : `Get-ADDomainController -Filter * | ft Hostname,Site`.

## <a name="view-replication-status-information"></a>Afficher les informations et l’état de réplication
Dans les procédures suivantes, vous allez utiliser l’une des applets de commande Windows PowerShell pour la gestion et la réplication Active Directory, `Get-ADReplicationUpToDatenessVectorTable DC1`, pour générer un rapport de réplication simple utilisant la table de vecteurs de mise à jour conservée par chaque contrôleur de domaine. Cette table de vecteurs de mise à jour assure le suivi du numéro USN écrit source le plus élevé détecté par chaque contrôleur de domaine dans la forêt.

Pour effectuer les étapes des procédures suivantes, vous devez être membre du groupe Administrateurs du domaine ou disposer des autorisations équivalentes.

#### <a name="to-view-the-up-to-dateness-vector-table-for-a-single-domain-controller"></a>Pour afficher la table de vecteurs de mise à jour pour un seul contrôleur de domaine

1.  Tapez la commande suivante à l’invite de commandes du **module Active Directory pour Windows PowerShell** :

    `Get-ADReplicationUpToDatenessVectorTable DC1`

    Cette commande fournit une liste des numéros USN les plus élevés détectés **DC1** pour chaque contrôleur de domaine dans la forêt. La valeur **Server** fait référence au serveur qui assure la maintenance de la table, dans le cas présent **DC1**. La valeur **Partner** fait référence au partenaire de réplication (direct ou indirect) sur lequel les modifications ont été apportées. La valeur UsnFilter est le numéro USN le plus élevé détecté par **DC1** à partir du partenaire. Si un nouveau contrôleur de domaine est ajouté à la forêt, il n’apparaîtra pas dans la table de **DC1**tant que **DC1** n’aura pas reçu de modification provenant du nouveau domaine.

#### <a name="to-view-the-up-to-dateness-vector-table-for-all-domain-controllers-in-a-domain"></a>Pour afficher la table de vecteurs de mise à jour pour tous les contrôleurs d’un domaine

1.  Tapez la commande suivante à l’invite de commandes du module Active Directory pour Windows PowerShell :

    `Get-ADReplicationUpToDatenessVectorTable * | sort Partner,Server | ft Partner,Server,UsnFilter`

    Cette commande remplace **DC1** par `*`, recueillant ainsi les données de table de vecteurs de mise à jour de tous les contrôleurs de domaine. Les données sont triées par **Partner** et **Server**, puis affichées dans un tableau.

    Le tri vous permet de comparer aisément le dernier numéro USN détecté par chaque contrôleur de domaine pour un partenaire de réplication donné. Il s’agit d’un moyen rapide de vérifier que la réplication a lieu au sein de votre environnement. Si la réplication fonctionne correctement, les valeurs UsnFilter signalées pour un partenaire de réplication donné doivent être assez semblables sur tous les contrôleurs de domaine.

## <a name="see-also"></a>Voir aussi
[Gestion de la topologie et de la réplication avancée &#40;Active Directory à l’aide de Windows PowerShell niveau 200&#41;](Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md)


