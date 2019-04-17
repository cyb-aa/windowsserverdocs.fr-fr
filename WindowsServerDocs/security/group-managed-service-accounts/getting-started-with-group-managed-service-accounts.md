---
title: "Prise en main de groupe des comptes de Service administrés"
description: "Sécurité de Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7130ad73-9688-4f64-aca1-46a9187a46cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: cd1e92f93701e5b430d1425fcf9a2f3590d1fe5d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="getting-started-with-group-managed-service-accounts"></a>Prise en main de groupe des comptes de Service administrés

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016


Ce guide fournit des instructions détaillées et des informations générales sur l’activation et l’utilisation de comptes de Service administrés groupe dans Windows Server 2012.

**Dans ce document**

-   [Conditions préalables](#BKMK_Prereqs)

-   [Introduction](#BKMK_Intro)

-   [Déploiement d’une batterie de serveurs](#BKMK_DeployNewFarm)

-   [Ajout d’hôtes membres à une batterie de serveurs existante](#BKMK_AddMemberHosts)

-   [Mise à jour les propriétés du compte Service administré de groupe](#BKMK_Update_gMSA)

-   [Désaffectation d’hôtes membres à partir d’une batterie de serveurs existante](#BKMK_DecommMemberHosts)


> [!NOTE]
> Cette rubrique inclut des applets de commande exemple Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, voir [applets de commande à l’aide de](https://go.microsoft.com/fwlink/p/?linkid=230693).

## <a name="BKMK_Prereqs"></a>Conditions préalables
Consultez la section dans cette rubrique sur [configuration requise pour les comptes de Service administrés groupe](#BKMK_gMSA_Req).

## <a name="BKMK_Intro"></a>Introduction
Lorsqu’un ordinateur client se connecte à un service qui est hébergé sur une batterie de serveurs à l’aide d’équilibrage de charge réseau ou une autre méthode où tous les serveurs semblent être le même service pour le client, les protocoles d’authentification prenant en charge l’authentification mutuelle comme Kerberos ne peut pas être utilisés, sauf si toutes les instances des services utilisent le même principal. Cela signifie que chaque service dispose d’utiliser les mêmes mots de passe/clés pour prouver leur identité.

> [!NOTE]
> Clusters de basculement ne prennent pas en charge les comptes Gmsa. Toutefois, les services qui s’exécutent sur le service de Cluster peuvent utiliser un compte gMSA ou un sMSA s’ils sont un service Windows, un pool d’application, une tâche planifiée ou en mode natif prend en charge des comptes gMSA ou sMSA.

Les services ont des principaux suivants sont disponibles, et chacun avec certaines limitations.

|Entités de sécurité|Étendue|Services pris en charge|Gestion de mot de passe|
|-------|-----|-----------|------------|
|Ordinateur compte du système Windows|Domaine|Limité à un domaine joint au serveur|Géré par l’ordinateur|
|Compte d’ordinateur sans système Windows|Domaine|Tout serveur joint à un domaine|None|
|Compte virtuel|Local|Limité à un serveur|Géré par l’ordinateur|
|Compte de Service administré autonome Windows 7|Domaine|Limité à un domaine joint au serveur|Géré par l’ordinateur|
|Compte d’utilisateur|Domaine|Tout serveur joint à un domaine|None|
|Compte de Service administré de groupe|Domaine|Tout serveur joint au domaine de Windows Server 2012|Le contrôleur de domaine gère et l’hôte récupère|

Un compte d’ordinateur Windows, ou Windows 7 autonome (sMSA) du compte de Service administré ou les comptes virtuels ne peuvent pas être partagés sur plusieurs systèmes. Si vous configurez un compte de services sur des batteries de serveurs à partager, vous devrez choisir un compte d’utilisateur ou un compte d’ordinateur en dehors d’un système Windows. Les deux cas, ces comptes ne disposent pas de la fonctionnalité de gestion de mot de passe unique point de contrôle. Cela crée le problème dans lequel chaque organisation a besoin pour créer une solution coûteuse pour mettre à jour de clés pour le service dans Active Directory, puis distribuer les clés à toutes les instances de ces services.

Avec Windows Server 2012, services ou les administrateurs de service n’avez pas besoin de gérer la synchronisation de mot de passe entre les instances de service à l’aide du groupe des comptes de Service administrés (gMSA). Vous configurez la fonctionnalité gMSA dans Active Directory et configurez le service qui prend en charge les comptes de Service administrés. Vous pouvez configurer un compte gMSA à l’aide du *-ADServiceAccount des applets de commande qui font partie du module Active Directory. Configuration du service identité de l’ordinateur hôte est pris en charge par:

-   Les mêmes API que sMSA, afin de produits qui prennent en charge sMSA prendront en charge de service administré de groupe

-   Services qui utilisent le Gestionnaire de contrôle de Service pour configurer l’identité d’ouverture de session

-   Services qui utilisent le Gestionnaire des services Internet pour les pools d’applications pour configurer l’identité

-   Tâches à l’aide du Planificateur de tâches.

### <a name="BKMK_gMSA_Req"></a>Configuration requise pour les comptes de Service administrés groupe
Le tableau suivant répertorie la configuration de système d’exploitation requise pour l’authentification Kerberos travailler avec les services à l’aide de service administré de groupe. La configuration requise pour Active Directory sont répertoriés après le tableau.

Une architecture 64 bits est requise pour exécuter les commandes Windows PowerShell permettant de gérer les comptes de Service administrés groupe.

**Configuration requise du système d’exploitation**

|Élément|Configuration requise|Système d'exploitation|
|------|--------|----------|
|Hôte d’Application cliente|Client Kerberos conforme de RFC|Au moins Windows XP|
|Domaine du compte d’utilisateur contrôleurs de domaine|KDC conforme aux RFC|Au minimum Windows Server 2003|
|Hôtes membres du service partagé|| Windows Server2012 |
|Domaine de l’hôte membre contrôleurs de domaine|KDC conforme aux RFC|Au minimum Windows Server 2003|
|domaine du compte de service administré de groupe contrôleurs de domaine| Windows Server 2012 contrôleurs de domaine disponible pour l’hôte puisse récupérer le mot de passe|Domaine avec Windows Server 2012, qui peut avoir des systèmes antérieurs à Windows Server 2012 |
|Hôte de service principal|Serveur d’application Kerberos conforme RFC|Au minimum Windows Server 2003|
|Domaine du compte de service principal contrôleurs de domaine|KDC conforme aux RFC|Au minimum Windows Server 2003|
|Windows PowerShell pour Active Directory|Windows PowerShell pour Active Directory installé localement sur un ordinateur prenant en charge d’une architecture 64 bits ou sur votre ordinateur de gestion à distance (par exemple, en utilisant les outils d’Administration de serveur distant)| Windows Server2012 |

**Conditions requises pour services de domaine Active Directory**

-   Le schéma Active Directory dans la forêt du domaine gMSA doit être mis à jour vers Windows Server 2012 pour créer un compte gMSA.

    Vous pouvez mettre à jour le schéma en installant un contrôleur de domaine qui exécute Windows Server 2012 ou en exécutant la version d’adprep.exe à partir d’un ordinateur exécutant Windows Server 2012. La valeur d’attribut de version de l’objet de l’objet CN = Schema, CN = Configuration, DC = Contoso, DC = Com doit être 52.

-   Nouveau compte gMSA configuré

-   Si vous gérez l’autorisation de l’hôte service à utiliser gMSA par groupe, alors groupe de sécurité nouveau ou existant

-   Si la gestion du contrôle d’accès de service par groupe, alors groupe de sécurité nouveau ou existant

-   Si la première clé racine principale pour Active Directory n’est pas déployée dans le domaine ou n’a pas été créée, puis le créer. Le résultat de sa création peut être vérifié dans le journal des opérations, ID d’événement 4004.

Pour obtenir des instructions comment créer la clé, voir [créer la clé racine KDS de Services de Distribution clé](create-the-key-distribution-services-kds-root-key.md). Clé de Service de Distribution Microsoft (kdssvc.dll) crée la clé racine pour Active Directory.

**Cycle de vie**

Le cycle de vie d’une batterie de serveurs à l’aide de la fonctionnalité gMSA généralement implique les tâches suivantes:

-   Déploiement d’une batterie de serveurs

-   Ajout d’hôtes membres à une batterie de serveurs existante

-   Désaffectation d’hôtes membres à partir d’une batterie de serveurs existante

-   Désaffectation d’une batterie de serveurs existante

-   Suppression d’un hôte membre compromis d’une batterie de serveurs si nécessaire.

## <a name="BKMK_DeployNewFarm"></a>Déploiement d’une batterie de serveurs
Lorsque vous déployez une batterie de serveurs, l’administrateur de service devra déterminer:

-   Si le service prend en charge à l’aide de comptes Gmsa

-   Si le service requiert les connexions entrantes ou sortantes authentifiées

-   Les noms de compte d’ordinateur pour les hôtes membres pour le service à l’aide de la fonctionnalité gMSA

-   Le nom NetBIOS pour le service

-   Le nom d’hôte DNS pour le service

-   Les noms de Principal du Service (SPN) pour le service

-   Le mot de passe modifier l’intervalle (valeur par défaut est 30 jours).

### <a name="BKMK_Step1"></a>Étape 1: Configuration des comptes de Service administrés groupe
Vous pouvez créer un compte gMSA uniquement si le schéma de la forêt a été mis à jour vers Windows Server 2012, la clé racine principale pour Active Directory a été déployée et il existe au moins Windows Server 2012 DC dans le domaine dans lequel le compte gMSA sera créé.

L’appartenance au groupe **Admins du domaine**, **opérateurs de compte** ou la possibilité de créer des objets msDS-GroupManagedServiceAccount, est la condition minimale requise pour effectuer les procédures suivantes.

#### <a name="BKMK_CreateGMSA"></a>Pour créer un compte gMSA à l’aide de l’applet de commande New-ADServiceAccount

1.  Sur le contrôleur de domaine Windows Server 2012, exécutez Windows PowerShell à partir de la barre des tâches.

2.  À l’invite de commande de Windows PowerShell, tapez les commandes suivantes et appuyez sur ENTRÉE. (Le module Active Directory sera chargé automatiquement).

    **Nouveau-ADServiceAccount [-nom] <string> - DNSHostName <string> [-KerberosEncryptionType <ADKerberosEncryptionType>] [-ManagedPasswordIntervalInDays < Nullable [Int32] >] [-PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >] - SamAccountName <string> - ServicePrincipalNames < chaîne [] >**

    |Paramètre|Chaîne|Exemple|
    |-------|-----|------|
    |Nom|Nom du compte|ITFarm1|
    |DNSHostName|Nom d’hôte DNS du service|ITFarm1.contoso.com|
    |KerberosEncryptionType|Les types de chiffrement pris en charge par les serveurs hôtes|RC4, AES128, AES256|
    |ManagedPasswordIntervalInDays|Intervalle de modification de mot de passe en jours (par défaut est de 30 jours si ce n’est pas fourni)|90|
    |PrincipalsAllowedToRetrieveManagedPassword|Les comptes d’ordinateur des hôtes membres ou hôtes membres appartiennent à un groupe de sécurité|ITFarmHosts|
    |SamAccountName|Nom NetBIOS pour le service si pas identique nom|ITFarm1|
    |ServicePrincipalNames|Noms de Principal du service (SPN) pour le service|http/ITFarm1.contoso.com/contoso.com, http/ITFarm1.contoso.com/contoso, http/ITFarm1/contoso.com, http/Batterieit1/contoso|

    > [!IMPORTANT]
    > L’intervalle de modification de mot de passe peut être définie uniquement lors de la création. Si vous avez besoin modifier l’intervalle, vous devez créer un nouveau compte gMSA et définir au moment de la création.

    **Exemple**

    Entrez la commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

    ```
    New-ADServiceAccount ITFarm1 -DNSHostName ITFarm1.contoso.com -PrincipalsAllowedToRetrieveManagedPassword ITFarmHosts -KerberosEncryptionType RC4, AES128, AES256 -ServicePrincipalNames http/ITFarm1.contoso.com/contoso.com, http/ITFarm1.contoso.com/contoso, http/ITFarm1/contoso.com, http/ITFarm1/contoso

    ```

L’appartenance au groupe **Admins du domaine**, **opérateurs de compte**, ou avoir la capacité de créer des objets msDS-GroupManagedServiceAccount la condition minimale requise pour effectuer cette procédure. Pour obtenir des informations détaillées sur les comptes appropriés et les appartenances de groupe, voir [locaux et groupes de domaine par défaut](https://technet.microsoft.com/library/dd728026(WS.10).aspx).

##### <a name="to-create-a-gmsa-for-outbound-authentication-only-using-the-new-adserviceaccount-cmdlet"></a>Pour créer un compte gMSA pour l’authentification sortante uniquement à l’aide de l’applet de commande New-ADServiceAccount

1.  Sur le contrôleur de domaine Windows Server 2012, exécutez Windows PowerShell à partir de la barre des tâches.

2.  À l’invite de commandes du module ActiveDirectory de Windows PowerShell, tapez les commandes suivantes et appuyez sur ENTRÉE:

    **Nouveau-ADServiceAccount [-nom] <string> - RestrictToOutboundAuthenticationOnly [-ManagedPasswordIntervalInDays < Nullable [Int32] >] [-PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >]**

    |Paramètre|Chaîne|Exemple|
    |-------|-----|------|
    |Nom|Nom du compte|ITFarm1|
    |ManagedPasswordIntervalInDays|Intervalle de modification de mot de passe en jours (par défaut est de 30 jours si ce n’est pas fourni)|75|
    |PrincipalsAllowedToRetrieveManagedPassword|Les comptes d’ordinateur des hôtes membres ou hôtes membres appartiennent à un groupe de sécurité|ITFarmHosts|

    > [!IMPORTANT]
    > L’intervalle de modification de mot de passe peut être définie uniquement lors de la création. Si vous avez besoin modifier l’intervalle, vous devez créer un nouveau compte gMSA et définir au moment de la création.

**Exemple**

```
New-ADServiceAccount ITFarm1 -RestrictToOutboundAuthenticationOnly - PrincipalsAllowedToRetrieveManagedPassword ITFarmHosts

```

### <a name="BKMK_ConfigureServiceIdentity"></a>Étape 2: Configuration du service identité de l’application service
Pour configurer les services dans Windows Server 2012, consultez la documentation des fonctionnalités suivantes:

-   Pool d’applications IIS

    Pour plus d’informations, voir [spécifier une identité pour un Pool d’applications (IIS 7)](https://technet.microsoft.com/library/cc771170(WS.10).aspx).

-   Services Windows

    Pour plus d’informations, voir [Services](https://technet.microsoft.com/library/cc772408.aspx).

-   Tâches

    Pour plus d’informations, voir la [vue d’ensemble du Planificateur de tâches](https://technet.microsoft.com/library/cc721871.aspx).

Autres services peuvent prendre en charge de service administré de groupe. Consultez la documentation du produit approprié pour plus d’informations sur la façon de configurer ces services.

## <a name="BKMK_AddMemberHosts"></a>Ajout d’hôtes membres à une batterie de serveurs existante
Si vous utilisez des groupes de sécurité pour la gestion des hôtes membres, ajoutez le compte d’ordinateur pour le nouvel hôte membre au groupe de sécurité (hôtes de membres de la fonctionnalité gMSA appartiennent à un) à l’aide d’une des méthodes suivantes.

L’appartenance au groupe **Admins du domaine**, ou la possibilité d’ajouter des membres à l’objet de groupe de sécurité, est la condition minimale requise pour effectuer ces procédures.

-   Méthode 1: Utilisateurs Active Directory et les ordinateurs

    Pour connaître les procédures comment utiliser cette méthode, voir [ajouter un compte d’ordinateur à un groupe](https://technet.microsoft.com/library/cc733097.aspx) à l’aide de l’interface Windows, et [gérer des domaines différents dans le centre d’administration Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

-   Méthode 2: dsmod

    Pour connaître les procédures comment utiliser cette méthode, voir [ajouter un compte d’ordinateur à un groupe](https://technet.microsoft.com/library/cc733097.aspx) à l’aide de la ligne de commande.

-   Méthode 3: Applet de commande Windows PowerShell Active Directory Add-ADPrincipalGroupMembership

    Pour connaître les procédures comment utiliser cette méthode, voir [Add-ADPrincipalGroupMembership](https://technet.microsoft.com/library/ee617203.aspx).

Si vous utilisez des comptes d’ordinateur, recherchez les comptes existants, puis ajoutez le nouveau compte d’ordinateur.

L’appartenance au groupe **Admins du domaine**, **opérateurs de compte**, ou la possibilité de gérer des objets msDS-GroupManagedServiceAccount, est la condition minimale requise pour effectuer cette procédure. Pour obtenir des informations détaillées sur les comptes appropriés et les appartenances de groupe, voir groupes locaux et domaine par défaut.

#### <a name="to-add-member-hosts-using-the-set-adserviceaccount-cmdlet"></a>Pour ajouter des hôtes membres à l’aide de l’applet de commande Set-ADServiceAccount

1.  Sur le contrôleur de domaine Windows Server 2012, exécutez Windows PowerShell à partir de la barre des tâches.

2.  À l’invite de commandes du module ActiveDirectory de Windows PowerShell, tapez les commandes suivantes et appuyez sur ENTRÉE:

    **Get-ADServiceAccount [-nom] <string> - PrincipalsAllowedToRetrieveManagedPassword**

3.  À l’invite de commandes du module ActiveDirectory de Windows PowerShell, tapez les commandes suivantes et appuyez sur ENTRÉE:

    **Set-ADServiceAccount [-nom] <string> - PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >**

|Paramètre|Chaîne|Exemple|
|-------|-----|------|
|Nom|Nom du compte|ITFarm1|
|PrincipalsAllowedToRetrieveManagedPassword|Les comptes d’ordinateur des hôtes membres ou hôtes membres appartiennent à un groupe de sécurité|Hôte1, Hôte2, hôte3|

**Exemple**

Par exemple, pour ajouter des membres hôtes tapez les commandes suivantes et appuyez sur ENTRÉE.

```
Get-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword

```

```
Set-ADServiceAccount [-Name] ITFarm1-PrincipalsAllowedToRetrieveManagedPassword Host1 Host2 Host3

```

## <a name="BKMK_Update_gMSA"></a>Mise à jour les propriétés de compte de Service administré de groupe
L’appartenance au groupe **Admins du domaine**, **opérateurs de compte**, ou à la possibilité d’écrire dans des objets msDS-GroupManagedServiceAccount pour réaliser ces procédures.

Ouvrez le Module Active Directory pour Windows PowerShell et définissez toute propriété à l’aide de l’applet de commande Set-ADServiceAccount.

Pour plus d’informations sur la définition de ces propriétés, voir [Set-ADServiceAccount](https://technet.microsoft.com/library/ee617252.aspx) dans la bibliothèque TechNet ou tapez **Get-Help Set-ADServiceAccount** le module Active Directory pour Windows PowerShell en commande invite de commandes et appuyez sur ENTRÉE.

## <a name="BKMK_DecommMemberHosts"></a>Désaffectation d’hôtes membres à partir d’une batterie de serveurs existante
L’appartenance au groupe **Admins du domaine**, ou la possibilité de supprimer des membres de l’objet groupe de sécurité, est la condition minimale requise pour effectuer ces procédures.

### <a name="step-1-remove-member-host-from-gmsa"></a>Étape 1: Supprimer l’hôte membre du compte gMSA
Si vous utilisez des groupes de sécurité pour la gestion des hôtes membres, supprimez le compte d’ordinateur pour l’hôte membre désaffecté du groupe de sécurité que les hôtes membres de la fonctionnalité gMSA sont un membre à l’aide d’une des méthodes suivantes.

-   Méthode 1: Utilisateurs Active Directory et les ordinateurs

    Pour connaître les procédures comment utiliser cette méthode, voir [supprimer un compte d’ordinateur](https://technet.microsoft.com/library/cc754624.aspx) à l’aide de l’interface Windows, et [gérer des domaines différents dans le centre d’administration Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

-   Méthode 2: drsm

    Pour connaître les procédures comment utiliser cette méthode, voir [supprimer un compte d’ordinateur](https://technet.microsoft.com/library/cc754624.aspx) à l’aide de la ligne de commande.

-   Méthode 3: Applet de commande Windows PowerShell Active Directory Remove-ADPrincipalGroupMembership

    Pour plus d’informations, voir [Remove-ADPrincipalGroupMembership](https://technet.microsoft.com/library/ee617243.aspx) dans la bibliothèque TechNet ou tapez **Get-Help Remove-ADPrincipalGroupMembership** le module Active Directory pour Windows PowerShell en commande invite de commandes et appuyez sur ENTRÉE.

Si la liste des comptes d’ordinateur, récupérer les comptes existants, puis ajoutez toutes sauf le compte d’ordinateur supprimé.

L’appartenance au groupe **Admins du domaine**, **opérateurs de compte**, ou la possibilité de gérer des objets msDS-GroupManagedServiceAccount, est la condition minimale requise pour effectuer cette procédure. Pour obtenir des informations détaillées sur les comptes appropriés et les appartenances de groupe, voir groupes locaux et domaine par défaut.

##### <a name="to-remove-member-hosts-using-the-set-adserviceaccount-cmdlet"></a>Pour supprimer des hôtes membres à l’aide de l’applet de commande Set-ADServiceAccount

1.  Sur le contrôleur de domaine Windows Server 2012, exécutez Windows PowerShell à partir de la barre des tâches.

2.  À l’invite de commandes du module ActiveDirectory de Windows PowerShell, tapez les commandes suivantes et appuyez sur ENTRÉE:

    **Get-ADServiceAccount [-nom] <string> - PrincipalsAllowedToRetrieveManagedPassword**

3.  À l’invite de commandes du module ActiveDirectory de Windows PowerShell, tapez les commandes suivantes et appuyez sur ENTRÉE:

    **Set-ADServiceAccount [-nom] <string> - PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >**

|Paramètre|Chaîne|Exemple|
|-------|-----|------|
|Nom|Nom du compte|ITFarm1|
|PrincipalsAllowedToRetrieveManagedPassword|Les comptes d’ordinateur des hôtes membres ou hôtes membres appartiennent à un groupe de sécurité|Hôte1, hôte3|

**Exemple**

Par exemple, pour supprimer des membres hôtes tapez les commandes suivantes et appuyez sur ENTRÉE.

```
Get-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword

```

```
Set-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword Host1 Host3

```

### <a name="BKMK_RemoveGMSA"></a>Étape 2: Suppression d’un compte de Service administré de groupe à partir du système
Supprimez les informations d’identification de compte gMSA mises en cache de l’hôte membre à l’aide de Uninstall-ADServiceAccount ou l’API NetRemoveServiceAccount sur le système hôte.

L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer ces procédures.

##### <a name="to-remove-a-gmsa-using-the-uninstall-adserviceaccount-cmdlet"></a>Pour supprimer un compte gMSA à l’aide de l’applet de commande Uninstall-ADServiceAccount

1.  Sur le contrôleur de domaine Windows Server 2012, exécutez Windows PowerShell à partir de la barre des tâches.

2.  À l’invite de commandes du module ActiveDirectory de Windows PowerShell, tapez les commandes suivantes et appuyez sur ENTRÉE:

    **Uninstall-ADServiceAccount < ADServiceAccount >**

    **Exemple**

    Par exemple, pour supprimer les informations d’identification mises en cache pour un compte gMSA nommé ITFarm1 tapez la commande suivante et appuyez sur ENTRÉE:

    ```
    Uninstall-ADServiceAccount ITFarm1
    ```

Pour plus d’informations sur l’applet de commande Uninstall-ADServiceAccount, au module Active Directory pour l’invite de commandes Windows PowerShell, tapez **Get-Help Uninstall-ADServiceAccount**, puis appuyez sur entrée ou consultez les informations dans la bibliothèque TechNet [Uninstall-ADServiceAccount](https://technet.microsoft.com/library/ee617202.aspx).



## <a name="BKMK_Links"></a>Voir aussi

-   [Vue d’ensemble des comptes de Service administrés de groupe](group-managed-service-accounts-overview.md)



