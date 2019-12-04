---
title: Getting Started with Group Managed Service Accounts
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 45fe605228189f49d40543e5da703f9afe0d962e
ms.sourcegitcommit: 4a03f263952c993dfdf339dd3491c73719854aba
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74791219"
---
# <a name="getting-started-with-group-managed-service-accounts"></a>Getting Started with Group Managed Service Accounts

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016


Ce guide fournit des instructions pas à pas et des informations générales sur l’activation et l’utilisation de comptes de service administrés de groupe dans Windows Server 2012.

**Dans ce document**

-   [Conditions préalables](#BKMK_Prereqs)

-   [Introduction](#BKMK_Intro)

-   [Déploiement d’une nouvelle batterie de serveurs](#BKMK_DeployNewFarm)

-   [Ajout d’hôtes membres à une batterie de serveurs existante](#BKMK_AddMemberHosts)

-   [Mise à jour des propriétés du compte de service administré de groupe](#BKMK_Update_gMSA)

-   [Désaffectation d’hôtes membres d’une batterie de serveurs existante](#BKMK_DecommMemberHosts)


> [!NOTE]
> Cette rubrique inclut des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [Utilisation des applets de commande](https://go.microsoft.com/fwlink/p/?linkid=230693).

## <a name="BKMK_Prereqs"></a>Conditions préalables
Consultez la section de cette rubrique portant sur [Conditions requises pour les comptes de service administrés de groupe](#BKMK_gMSA_Req).

## <a name="BKMK_Intro"></a>Présentation
Lorsqu'un ordinateur client se connecte à un service qui est hébergé sur une batterie de serveurs à l'aide de l'équilibrage de la charge réseau ou d'une autre méthode dans laquelle tous les serveurs sont présentés au client comme étant un même service, les protocoles d'authentification prenant en charge l'authentification mutuelle comme Kerberos ne peuvent alors pas être utilisés sauf si toutes les instances des services utilisent le même principal. Cela signifie que tous les services doivent utiliser les mêmes mots de passe/clés pour prouver leur identité.

> [!NOTE]
> Les clusters de basculement ne prennent pas en charge les comptes de service administrés de groupe (gMSA, group Managed Service Account). Toutefois, les services qui s'exécutent sur le service de cluster peuvent utiliser un compte gMSA ou un compte de service administré autonome (sMSA, standalone Managed Service Account) s'il s'agit d'un service Windows, d'un pool d'applications, d'une tâche planifiée ou s'ils prennent nativement en charge les comptes gMSA ou sMSA.

Les principaux suivants sont disponibles pour les services, chacun avec certaines limitations.

|Principaux|Étendue|Services pris en charge|Gestion des mots de passe|
|-------|-----|-----------|------------|
|Compte d'ordinateur du système Windows|Domaine|Limité à un serveur joint à un domaine|Géré par l'ordinateur|
|Compte d'ordinateur sans système Windows|Domaine|Tout serveur joint à un domaine|Aucun(e)|
|Compte virtuel|Locales|Limité à un serveur|Géré par l'ordinateur|
|Compte de service administré autonome Windows 7|Domaine|Limité à un serveur joint à un domaine|Géré par l'ordinateur|
|Compte d’utilisateur|Domaine|Tout serveur joint à un domaine|Aucun(e)|
|Compte de service géré de groupe|Domaine|Tout serveur joint à un domaine Windows Server 2012|Le contrôleur de domaine gère et l'hôte récupère|

Un compte d'ordinateur Windows, un compte de service administré autonome (sMSA) Windows 7 ou des comptes virtuels ne peuvent pas être partagés sur plusieurs systèmes. Si vous configurez un compte à partager par les services de la batterie de serveurs, vous devrez choisir un compte d'utilisateur ou un compte d'ordinateur en dehors d'un système Windows. Dans tous les cas, ces comptes n'ont pas la capacité de gérer les mots de passe depuis un seul point de contrôle. Cette situation est problématique, car chaque organisation doit alors créer une solution coûteuse pour mettre à jour les clés du service dans Active Directory, puis les distribuer à toutes les instances de ces services.

Avec Windows Server 2012, les services ou les administrateurs de services n’ont pas besoin de gérer la synchronisation de mot de passe entre les instances de service lors de l’utilisation de comptes de service administrés de groupe (gMSA). Vous configurez la fonctionnalité gMSA dans Active Directory, puis configurez le service qui prend en charge les comptes de service administrés. Vous pouvez configurer un compte gMSA à l'aide des applets de commande *-ADServiceAccount qui font partie du module Active Directory. La configuration de l'identité du service sur l'hôte est prise en charge par :

-   Les mêmes API que les comptes sMSA, de sorte que les produits qui prennent en charge les comptes sMSA prendront en charge les comptes gMSA

-   Les services qui utilisent le Gestionnaire de contrôle des services pour configurer l'identité d'ouverture de session

-   Les services qui utilisent le Gestionnaire des services IIS pour les pools d'applications pour configurer l'identité

-   Les tâches qui utilisent le Planificateur de tâches

### <a name="BKMK_gMSA_Req"></a>Configuration requise pour les comptes de service administrés de groupe
Le tableau suivant répertorie la configuration requise du système d'exploitation pour que l'authentification Kerberos puisse fonctionner avec les services utilisant des comptes gMSA. Les conditions requises pour Active Directory sont répertoriées à la suite de ce tableau.

Une architecture 64 bits est requise pour exécuter les commandes Windows PowerShell utilisées pour administrer les comptes de service administrés de groupe.

**Configuration requise pour le système d’exploitation**

|Élément|Condition requise|Système d’exploitation|
|------|--------|----------|
|Hôte d'application cliente|Client Kerberos conforme aux RFC|Au minimum Windows XP|
|Contrôleurs de domaine du domaine du compte d’utilisateur|KDC conforme aux RFC|Au minimum Windows Server 2003|
|Hôtes membres du service partagé|| Windows Server 2012 |
|Contrôleurs de domaine de domaine de l’hôte membre|KDC conforme aux RFC|Au minimum Windows Server 2003|
|Contrôleurs de domaine du domaine du compte gMSA| Contrôleurs de l’ordinateur Windows Server 2012 disponibles pour l’hôte afin de récupérer le mot de passe|Domaine avec Windows Server 2012 qui peut avoir des systèmes antérieurs à Windows Server 2012 |
|Hôte de service principal|Serveur d'application Kerberos conforme aux RFC|Au minimum Windows Server 2003|
|Contrôleurs de domaine du domaine du compte de service principal|KDC conforme aux RFC|Au minimum Windows Server 2003|
|Windows PowerShell pour Active Directory|Windows PowerShell pour Active Directory installé localement sur un ordinateur prenant en charge une architecture 64 bits ou sur votre ordinateur d'administration à distance (utilisant par exemple les Outils d'administration de serveur distant)| Windows Server 2012 |

**Conditions requises pour le service domaine Active Directory**

-   Le schéma de Active Directory dans la forêt du domaine gMSA doit être mis à jour vers Windows Server 2012 pour créer un gMSA.

    Vous pouvez mettre à jour le schéma en installant un contrôleur de domaine qui exécute Windows Server 2012 ou en exécutant la version d’Adprep. exe à partir d’un ordinateur exécutant Windows Server 2012. La valeur de l'attribut object-version de l'objet CN=Schema,CN=Configuration,DC=Contoso,DC=Com doit être 52.

-   Nouveau compte gMSA configuré

-   Si vous gérez l'autorisation de l'hôte de service à utiliser gMSA par groupe, alors groupe de sécurité nouveau ou existant

-   Si vous gérez le contrôle d'accès au service par groupe, alors groupe de sécurité nouveau ou existant

-   Si la première clé racine principale pour Active Directory n'est pas déployée dans le domaine ou n'a pas été créée, alors créez-la. Le résultat de sa création peut être vérifié dans le journal des opérations du service KDS, ID d'événement 4004.

Pour obtenir des instructions sur la création de la clé, consultez [créer la clé racine KDS des services de distribution de clés](create-the-key-distribution-services-kds-root-key.md). Le service de distribution de clés Microsoft (kdssvc.dll) crée la clé racine pour Active Directory.

**Cycles**

Le cycle de vie d'une batterie de serveurs utilisant la fonctionnalité gMSA comporte généralement les tâches suivantes :

-   Déploiement d'une nouvelle batterie de serveurs

-   Ajout d'hôtes membres à une batterie de serveurs existante

-   Désaffectation d'hôtes membres d'une batterie de serveurs existante

-   Désaffectation d'une batterie de serveurs existante

-   Suppression d'un hôte membre compromis d'une batterie de serveurs, le cas échéant.

## <a name="BKMK_DeployNewFarm"></a>Déploiement d’une nouvelle batterie de serveurs
Lors du déploiement d'une nouvelle batterie de serveurs, l'administrateur du service devra déterminer les éléments suivants :

-   Si le service prend en charge l'utilisation de comptes gMSA

-   Si le service nécessite des connexions entrantes et sortantes authentifiées

-   Les noms de comptes d'ordinateur des hôtes membres du service utilisant la fonctionnalité gMSA

-   Le nom NetBIOS du service

-   Le nom d'hôte DNS du service

-   Les noms de principal du service (SPN) pour le service

-   L'intervalle de modification de mot de passe (la valeur par défaut est 30 jours).

### <a name="BKMK_Step1"></a>Étape 1 : configuration des comptes de service administrés de groupe
Vous pouvez créer un gMSA uniquement si le schéma de forêt a été mis à jour vers Windows Server 2012, si la clé racine principale de Active Directory a été déployée et qu’il existe au moins un contrôleur de domaine Windows Server 2012 dans le domaine dans lequel le gMSA sera créé.

Vous devez au minimum appartenir au groupe **Admins du domaine**ou **Opérateurs de compte** , ou avoir la capacité de créer des objets msDS-GroupManagedServiceAccount pour réaliser les procédures suivantes.

#### <a name="BKMK_CreateGMSA"></a>Pour créer un gMSA à l’aide de l’applet de commande New-ADServiceAccount

1.  Sur le contrôleur de domaine Windows Server 2012, exécutez Windows PowerShell à partir de la barre des tâches.

2.  À l'invite de commandes de Windows PowerShell, tapez les commandes suivantes et appuyez sur Entrée : (Le module Active Directory sera chargé automatiquement.)

    **New-ADServiceAccount [-name] <string>-DNSHostName <string> [-KerberosEncryptionType <ADKerberosEncryptionType>] [-ManagedPasswordIntervalInDays < Nullable [Int32] >] [-PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >]-SamAccountName <string>-ServicePrincipalNames < String [] >**

    |Paramètre|Chaîne|Exemple|
    |-------|-----|------|
    |Nom|Nom du compte|BatterieIT1|
    |DNSHostName|Nom d'hôte DNS du service|BatterieIT1.contoso.com|
    |KerberosEncryptionType|Tout type de chiffrement pris en charge par les serveurs hôtes|RC4, AES128, AES256|
    |ManagedPasswordIntervalInDays|Intervalle de modification de mot de passe exprimé en jours (la valeur par défaut est 30 jours)|90|
    |PrincipalsAllowedToRetrieveManagedPassword|Comptes d'ordinateur des hôtes membres ou groupe de sécurité auquel appartiennent les hôtes membres|HôtesBatterieIT|
    |SamAccountName|Nom NetBIOS du service s'il est différent de Name|BatterieIT1|
    |ServicePrincipalNames|Noms de principal du service (SPN) pour le service|http/BatterieIT1.contoso.com/contoso.com, http/BatterieIT1.contoso.com/contoso, httpBatterieIT1/contoso.com, http/BatterieIT1/contoso|

    > [!IMPORTANT]
    > L'intervalle de modification de mot de passe ne peut être défini qu'à la création. Si vous avez besoin de modifier l'intervalle, vous devez créer un nouveau compte gMSA et définir l'intervalle au moment de la création.

    **Exemple**

    Entrez la commande sur une seule ligne, même si elle tient ici sur plusieurs lignes du fait de contraintes de mise en forme.

    ```Powershell
    New-ADServiceAccount ITFarm1 -DNSHostName ITFarm1.contoso.com -PrincipalsAllowedToRetrieveManagedPassword ITFarmHosts$ -KerberosEncryptionType RC4, AES128, AES256 -ServicePrincipalNames http/ITFarm1.contoso.com/contoso.com, http/ITFarm1.contoso.com/contoso, http/ITFarm1/contoso.com, http/ITFarm1/contoso
    ```

Vous devez au minimum appartenir au groupe **Admins du domaine** ou **Opérateurs de compte**, ou avoir la capacité de créer des objets msDS-GroupManagedServiceAccount pour réaliser les procédures suivantes. Pour plus d’informations sur l’utilisation des comptes et appartenances à des groupes appropriés, voir [Groupes locaux et de domaine par défaut](https://technet.microsoft.com/library/dd728026(WS.10).aspx).

##### <a name="to-create-a-gmsa-for-outbound-authentication-only-using-the-new-adserviceaccount-cmdlet"></a>Pour créer un compte gMSA pour l'authentification sortante en utilisant uniquement l'applet de commande New-ADServiceAccount

1.  Sur le contrôleur de domaine Windows Server 2012, exécutez Windows PowerShell à partir de la barre des tâches.

2.  À l'invite de commandes du module Active Directory pour Windows PowerShell, tapez les commandes suivantes et appuyez sur Entrée :

    **New-ADServiceAccount [-name] <string>-RestrictToOutboundAuthenticationOnly [-ManagedPasswordIntervalInDays < Nullable [Int32] >] [-PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >]**

    |Paramètre|Chaîne|Exemple|
    |-------|-----|------|
    |Nom|Nom du compte|BatterieIT1|
    |ManagedPasswordIntervalInDays|Intervalle de modification de mot de passe exprimé en jours (la valeur par défaut est 30 jours)|75|
    |PrincipalsAllowedToRetrieveManagedPassword|Comptes d'ordinateur des hôtes membres ou groupe de sécurité auquel appartiennent les hôtes membres|HôtesBatterieIT|

    > [!IMPORTANT]
    > L'intervalle de modification de mot de passe ne peut être défini qu'à la création. Si vous avez besoin de modifier l'intervalle, vous devez créer un nouveau compte gMSA et définir l'intervalle au moment de la création.

**Exemple**

```PowerShell
New-ADServiceAccount ITFarm1 -RestrictToOutboundAuthenticationOnly - PrincipalsAllowedToRetrieveManagedPassword ITFarmHosts$
```

### <a name="BKMK_ConfigureServiceIdentity"></a>Étape 2 : configuration du service d’application d’identité de service
Pour configurer les services dans Windows Server 2012, consultez la documentation sur les fonctionnalités suivantes :

-   Pool d'applications IIS

    Pour plus d’informations, voir [Spécifier une identité pour un pool d’applications (IIS 7)](https://technet.microsoft.com/library/cc771170(WS.10).aspx).

-   Windows Services

    Pour plus d’informations, voir [Services](https://technet.microsoft.com/library/cc772408.aspx).

-   Tâches

    Pour plus d’informations, voir la [Vue d’ensemble du planificateur de tâches](https://technet.microsoft.com/library/cc721871.aspx).

D'autres services peuvent prendre en charge la fonctionnalité gMSA. Reportez-vous à la documentation produit spécifique pour plus d'informations sur la configuration de ces services.

## <a name="BKMK_AddMemberHosts"></a>Ajout d’hôtes membres à une batterie de serveurs existante
Si vous utilisez des groupes de sécurité pour la gestion des hôtes membres, ajoutez le compte d’ordinateur du nouvel hôte membre au groupe de sécurité (dont les hôtes membres de gMSA sont membres) à l’aide de l’une des méthodes suivantes.

Vous devez au minimum appartenir au groupe **Admins du domaine** ou avoir la capacité d'ajouter des membres à l'objet de groupe de sécurité pour réaliser ces procédures.

-   Méthode 1 : Utilisateurs et ordinateurs Active Directory

    Pour connaître les procédures d’utilisation de cette méthode, voir [Ajouter un compte d’ordinateur à un groupe](https://technet.microsoft.com/library/cc733097.aspx) et [Gérer des domaines différents dans le Centre d’administration Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

-   Méthode 2 : dsmod

    Pour connaître les procédures d’utilisation de cette méthode, voir [Ajouter un compte d’ordinateur à un groupe](https://technet.microsoft.com/library/cc733097.aspx) à l’aide de la ligne de commande.

-   Méthode 3 : Applet de commande Windows PowerShell Active Directory Add-ADPrincipalGroupMembership

    Pour connaître les procédures d’utilisation de cette méthode, voir [Add-ADPrincipalGroupMembership](https://technet.microsoft.com/library/ee617203.aspx).

Si vous utilisez des comptes d'ordinateur, recherchez les comptes existants et ajoutez le nouveau compte d'ordinateur.

Vous devez au minimum appartenir au groupe **Admins du domaine**ou **Opérateurs de compte**, ou avoir la capacité de gérer des objets msDS-GroupManagedServiceAccount pour réaliser les procédures suivantes. Pour plus d'informations sur l'utilisation des comptes et appartenances à des groupes appropriés, voir Groupes locaux et de domaine par défaut.

#### <a name="to-add-member-hosts-using-the-set-adserviceaccount-cmdlet"></a>Pour ajouter des hôtes membres à l'aide de l'applet de commande Set-ADServiceAccount

1.  Sur le contrôleur de domaine Windows Server 2012, exécutez Windows PowerShell à partir de la barre des tâches.

2.  À l'invite de commandes du module Active Directory pour Windows PowerShell, tapez les commandes suivantes et appuyez sur Entrée :

    **ADServiceAccount [-name] <string>-PrincipalsAllowedToRetrieveManagedPassword**

3.  À l'invite de commandes du module Active Directory pour Windows PowerShell, tapez les commandes suivantes et appuyez sur Entrée :

    **Set-ADServiceAccount [-name] <string>-PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >**

|Paramètre|Chaîne|Exemple|
|-------|-----|------|
|Nom|Nom du compte|BatterieIT1|
|PrincipalsAllowedToRetrieveManagedPassword|Comptes d'ordinateur des hôtes membres ou groupe de sécurité auquel appartiennent les hôtes membres|Hôte1, Hôte2, Hôte3|

**Exemple**

Par exemple, pour ajouter des hôtes membres, tapez les commandes suivantes et appuyez sur Entrée.

```PowerShell
Get-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword
```

```PowerShell
Set-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword Host1$,Host2$,Host3$
```

## <a name="BKMK_Update_gMSA"></a>Mise à jour des propriétés du compte de service administré de groupe
Vous devez au minimum appartenir au groupe **Admins du domaine**ou **Opérateurs de compte**, ou avoir la capacité d'écrire dans des objets msDS-GroupManagedServiceAccount pour réaliser ces procédures.

Ouvrez le module Active Directory pour Windows PowerShell et définissez toute propriété à l'aide de l'applet de commande Set-ADServiceAccount.

Pour plus d’informations sur la définition de ces propriétés, voir [Set-ADServiceAccount](https://technet.microsoft.com/library/ee617252.aspx) dans la Bibliothèque TechNet ou tapez **Get-Help Set-ADServiceAccount** à l’invite de commandes du module Active Directory pour Windows PowerShell et appuyez sur ENTRÉE.

## <a name="BKMK_DecommMemberHosts"></a>Désaffectation d’hôtes membres d’une batterie de serveurs existante
Vous devez au minimum appartenir au groupe **Admins du domaine**ou avoir la capacité de supprimer des membres de l'objet de groupe de sécurité pour réaliser ces procédures.

### <a name="step-1-remove-member-host-from-gmsa"></a>Étape 1 : Supprimer l'hôte membre du compte gMSA
Si vous utilisez des groupes de sécurité pour la gestion des hôtes membres, supprimez le compte d’ordinateur de l’hôte membre retiré du groupe de sécurité auquel les hôtes membres de gMSA sont membres à l’aide de l’une des méthodes suivantes.

-   Méthode 1 : Utilisateurs et ordinateurs Active Directory

    Pour connaître les procédures d’utilisation de cette méthode, voir [Supprimer un compte d’ordinateur](https://technet.microsoft.com/library/cc754624.aspx) à l’aide de l’interface Windows et [Gérer des domaines différents dans le Centre d’administration Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

-   Méthode 2 : drsm

    Pour connaître les procédures d’utilisation de cette méthode, voir [Supprimer un compte d’ordinateur](https://technet.microsoft.com/library/cc754624.aspx) à l’aide de la ligne de commande.

-   Méthode 3 : Applet de commande Active Directory pour Windows PowerShell Remove-ADPrincipalGroupMembership

    Pour plus d’informations, voir  [Remove-ADPrincipalGroupMembership](https://technet.microsoft.com/library/ee617243.aspx) dans la Bibliothèque TechNet ou tapez **Get-Help Remove-ADPrincipalGroupMembership** à l’invite de commandes du module Active Directory pour Windows PowerShell et appuyez sur ENTRÉE.

Si la liste des comptes d'ordinateur est affichée, récupérez les comptes existants, puis ajoutez tous les comptes sauf le compte d'ordinateur supprimé.

Vous devez au minimum appartenir au groupe **Admins du domaine**ou **Opérateurs de compte**, ou avoir la capacité de gérer des objets msDS-GroupManagedServiceAccount pour réaliser les procédures suivantes. Pour plus d'informations sur l'utilisation des comptes et appartenances à des groupes appropriés, voir Groupes locaux et de domaine par défaut.

##### <a name="to-remove-member-hosts-using-the-set-adserviceaccount-cmdlet"></a>Pour supprimer des hôtes membres à l'aide de l'applet de commande Set-ADServiceAccount

1.  Sur le contrôleur de domaine Windows Server 2012, exécutez Windows PowerShell à partir de la barre des tâches.

2.  À l'invite de commandes du module Active Directory pour Windows PowerShell, tapez les commandes suivantes et appuyez sur Entrée :

    **ADServiceAccount [-name] <string>-PrincipalsAllowedToRetrieveManagedPassword**

3.  À l'invite de commandes du module Active Directory pour Windows PowerShell, tapez les commandes suivantes et appuyez sur Entrée :

    **Set-ADServiceAccount [-name] <string>-PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [] >**

|Paramètre|Chaîne|Exemple|
|-------|-----|------|
|Nom|Nom du compte|BatterieIT1|
|PrincipalsAllowedToRetrieveManagedPassword|Comptes d'ordinateur des hôtes membres ou groupe de sécurité auquel appartiennent les hôtes membres|Hôte1, Hôte3|

**Exemple**

Par exemple, pour supprimer des hôtes membres, tapez les commandes suivantes et appuyez sur Entrée.

```PowerShell
Get-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword
```

```PowerShell
Set-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword Host1$,Host3$
```

### <a name="BKMK_RemoveGMSA"></a>Étape 2 : suppression d’un compte de service administré de groupe du système
Supprimez les informations d'identification de compte gMSA mises en cache de l'hôte membre à l'aide de Uninstall-ADServiceAccount ou de l'API NetRemoveServiceAccount sur le système hôte.

Vous devez au minimum appartenir au groupe **Administrateurs**ou à un groupe équivalent pour réaliser ces procédures.

##### <a name="to-remove-a-gmsa-using-the-uninstall-adserviceaccount-cmdlet"></a>Pour supprimer un compte gMSA à l'aide de l'applet de commande Uninstall-ADServiceAccount

1.  Sur le contrôleur de domaine Windows Server 2012, exécutez Windows PowerShell à partir de la barre des tâches.

2.  À l'invite de commandes du module Active Directory pour Windows PowerShell, tapez les commandes suivantes et appuyez sur Entrée :

    **Uninstall-ADServiceAccount < ADServiceAccount >**

    **Exemple**

    Par exemple, pour supprimer les informations d'identification mises en cache pour un gMSA nommé BatterieIT1, tapez la commande suivante et appuyez sur Entrée :

    ```PowerShell
    Uninstall-ADServiceAccount ITFarm1
    ```

Pour plus d’informations sur l’applet de commande Uninstall-ADServiceAccount, à l’invite de commandes du module Active Directory pour Windows PowerShell, tapez **Get-Help Uninstall-ADServiceAccount**et appuyez sur ENTRÉE ou consultez [Uninstall-ADServiceAccount](https://technet.microsoft.com/library/ee617202.aspx)dans la Bibliothèque TechNet.



## <a name="BKMK_Links"></a>Voir aussi

-   [Présentation des comptes de service administrés de groupe](group-managed-service-accounts-overview.md)
