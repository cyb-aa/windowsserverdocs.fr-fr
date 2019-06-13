---
Title: Configuration des comptes de cluster dans Active Directory
ms.date: 11/12/2012
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: c15c33e31bf0bf7261097fbea110f2a0a788dab2
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439751"
---
# <a name="configuring-cluster-accounts-in-active-directory"></a>Configuration des comptes de cluster dans Active Directory


S'applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

Dans Windows Server, lorsque vous créez un cluster de basculement et configurez des services en cluster ou des applications, les Assistants de cluster de basculement créent les comptes d’ordinateur Active Directory nécessaires (également appelés objets ordinateur) et leur accorder des autorisations spécifiques. Les Assistants créent un compte d'ordinateur pour le cluster lui-même (ce compte est également appelé objet nom de cluster) et un compte d'ordinateur pour la plupart des types de services et d'applications en cluster, l'exception étant un ordinateur virtuel Hyper-V. Les autorisations pour ces comptes sont définies automatiquement par les Assistants de cluster de basculement. Si les autorisations sont modifiées, elles devront être rechangées pour répondre aux besoins de cluster. Ce guide décrit ces comptes et autorisations Active Directory, fournit des informations générales sur leur importance et décrit les étapes de la configuration et de la gestion des comptes.
      

## <a name="overview-of-active-directory-accounts-needed-by-a-failover-cluster"></a>Vue d'ensemble des comptes Active Directory requis par un cluster de basculement

Cette section décrit les comptes d'ordinateur Active Directory (également appelés objets ordinateur Active Directory) qui sont importants pour un cluster de basculement. Voici le détail de ces comptes :

  - **Le compte d’utilisateur utilisé pour créer le cluster.** Il s'agit du compte d'utilisateur utilisé pour démarrer l'Assistant Création d'un cluster. Le compte est important parce qu'il fournit la base à partir de laquelle un compte d'ordinateur est créé pour le cluster lui-même.  
      
  - **Le compte du nom du cluster.** (le compte d’ordinateur du cluster lui-même, également appelé l’objet nom de cluster ou CNO). Ce compte est créé automatiquement par l'Assistant Création d'un cluster et a le même nom que le cluster. Le compte du nom du cluster est très important, car d'autres comptes sont créés automatiquement via ce compte lorsque vous configurez de nouveaux services et de nouvelles applications sur le cluster. Si le compte du nom du cluster est supprimé ou que des autorisations en sont retirées, il est impossible de créer d'autres comptes comme requis par le cluster, jusqu'à ce que le compte du nom du cluster soit restauré ou que les autorisations appropriées soient rétablies.  
      
    Par exemple, si vous créez un cluster appelé Cluster1 et essayez de configurer un serveur d'impression en cluster appelé PrintServer1 sur votre cluster, le compte Cluster1 dans Active Directory devra conserver les autorisations appropriées afin qu'il puisse être utilisé pour créer un compte d'ordinateur appelé PrintServer1.  
      
    Le compte du nom du cluster est créé dans le conteneur par défaut pour les comptes d'ordinateur dans Active Directory. Par défaut, il s'agit du conteneur « Ordinateurs », mais l'administrateur de domaine peut choisir de le rediriger vers un autre conteneur ou une autre unité d'organisation.  
      
  - **Le compte d’ordinateur (objet ordinateur) d’un service en cluster ou d’application.** Ces comptes sont créés automatiquement par l'Assistant Haute disponibilité dans le cadre du processus de création de la plupart des types de services ou d'applications en cluster, l'exception étant un ordinateur virtuel Hyper-V. Le compte du nom du cluster est accordé les autorisations nécessaires pour contrôler ces comptes.  
      
    Par exemple, si vous avez un cluster appelé Cluster1 et que vous créez un serveur de fichiers en cluster appelé FileServer1, l'Assistant Haute disponibilité crée un compte d'ordinateur Active Directory appelé FileServer1. L’Assistant haute disponibilité donne également le compte Cluster1 les autorisations nécessaires pour contrôler le compte FileServer1.  
      

Le tableau suivant décrit les autorisations requises pour ces comptes.


<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Compte</th>
<th>Détails sur les autorisations</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Compte utilisé pour créer le cluster</p></td>
<td><p>Requiert des autorisations administratives sur les serveurs qui deviendront des nœuds de cluster. Requiert également les autorisations <strong>Créer des objets d'ordinateur</strong> et <strong>Lire toutes les propriétés</strong> dans le conteneur utilisé pour les comptes d'ordinateur dans le domaine.</p></td>
</tr>
<tr class="even">
<td><p>Compte du nom du cluster (compte d'ordinateur du cluster lui-même)</p></td>
<td><p>Lorsque l'Assistant Création d'un cluster est exécuté, il crée le compte du nom du cluster dans le conteneur par défaut utilisé pour les comptes d'ordinateur dans le domaine. Par défaut, le compte du nom du cluster (comme d'autres comptes d'ordinateur) peut créer jusqu'à dix comptes d'ordinateur dans le domaine.</p>
<p>Si vous créez le compte du nom du cluster (objet nom de cluster) avant de créer le cluster, autrement dit, si vous prédéfinissez le compte, vous devez lui affecter les autorisations <strong>Créer des objets d'ordinateur</strong> et <strong>Lire toutes les propriétés</strong> dans le conteneur utilisé pour les comptes d'ordinateur dans le domaine. Vous devez également désactiver le compte et affecter au compte qui sera utilisé par l'administrateur qui installe le cluster un <strong>Contrôle total</strong> de ce compte. Pour plus d'informations, consultez <a href="#steps-for-prestaging-the-cluster-name-account" data-raw-source="[Steps for prestaging the cluster name account](#steps-for-prestaging-the-cluster-name-account)">Étapes de la prédéfinition du compte du nom du cluster</a>, ultérieurement dans ce guide.</p></td>
</tr>
<tr class="odd">
<td><p>Compte d'ordinateur d'un service ou d'une application en cluster</p></td>
<td><p>Lorsque l’Assistant haute disponibilité est exécution (pour créer un nouveau service en cluster ou une application), dans la plupart des cas, un compte d’ordinateur pour le service en cluster ou application est créée dans Active Directory. Le compte du nom du cluster est accordé les autorisations nécessaires pour contrôler ce compte. L’exception est un ordinateur virtuel de Hyper-V en cluster : aucun compte d’ordinateur n’est créé pour celui-ci.</p>
<p>Si vous prédéfinissez le compte d’ordinateur pour une application ou un service en cluster, vous devez le configurer avec les autorisations nécessaires. Pour plus d'informations, consultez <a href="#steps-for-prestaging-an-account-for-a-clustered-service-or-application" data-raw-source="[Steps for prestaging an account for a clustered service or application](#steps-for-prestaging-an-account-for-a-clustered-service-or-application)">Étapes de la prédéfinition d'un compte pour un service ou une application en cluster</a>, ultérieurement dans ce guide.</p></td>
</tr>
</tbody>
</table>


> [!NOTE]
> Dans les versions antérieures de Windows Server, il était un compte pour le service de Cluster. Depuis Windows Server 2008, cependant, le service de Cluster s’exécute automatiquement dans un contexte spécial qui fournit les autorisations et privilèges spécifiques nécessaires au service (similaire au contexte du système local, mais avec des privilèges réduits). Toutefois, d'autres comptes sont nécessaires, comme décrit dans ce guide. 
<br>


### <a name="how-accounts-are-created-through-wizards-in-failover-clustering"></a>Création des comptes via des Assistants dans le clustering de basculement

Le diagramme suivant illustre l'utilisation et la création des comptes d'ordinateur (objets Active Directory) décrits dans la sous-section précédente. Ces comptes entrent en jeu lorsqu'un administrateur exécute l'Assistant Création d'un cluster, puis l'Assistant Haute disponibilité (pour configurer un service ou une application en cluster).

![](media/configure-ad-accounts/Cc731002.e8a7686c-9ba8-4ddf-87b1-175b7b51f65d(WS.10).gif)

Notez que le diagramme ci-dessus montre un administrateur unique qui exécute à la fois l'Assistant Création d'un cluster et l'Assistant Haute disponibilité. Toutefois, il pourrait s'agir de deux administrateurs différents utilisant deux comptes d'utilisateurs différents, si les deux comptes avaient des autorisations suffisantes. Les autorisations sont décrites plus en détail dans les exigences liées aux clusters de basculement, les domaines Active Directory et les comptes, plus loin dans ce guide.

### <a name="how-problems-can-result-if-accounts-needed-by-the-cluster-are-changed"></a>Problèmes qui peuvent se poser si les comptes requis par le cluster sont modifiés

Le diagramme suivant illustre les problèmes qui peuvent se poser si le compte du nom du cluster (l'un des comptes requis par le cluster) est modifié après qu'il a été créé automatiquement par l'Assistant Création d'un cluster.

![](media/configure-ad-accounts/Cc731002.beecc4f7-049c-4945-8fad-2cceafd6a4a5(WS.10).gif)

Si le type de problème illustré dans le diagramme se produit, un certain événement (1193, 1194, 1206 ou 1207) est consigné dans l'Observateur d'événements. Pour plus d’informations sur ces événements, consultez [ http://go.microsoft.com/fwlink/?LinkId=118271 ](http://go.microsoft.com/fwlink/?linkid=118271).

Notez qu'un problème semblable avec la création d'un compte pour un service ou une application en cluster peut se produire si le quota à l'échelle du domaine pour la création d'objets ordinateur (par défaut, 10) a été atteint. Le cas échéant, il peut être approprié de consulter l'administrateur de domaine au sujet de l'augmentation du quota, bien qu'il s'agisse d'un paramètre à l'échelle du domaine qui ne doit être changé qu'avec précaution et uniquement après avoir vérifié que le diagramme précédent ne décrivait pas votre situation. Pour plus d'informations, consultez [Étapes de la résolution de problèmes causés par des modifications dans les comptes Active Directory associés au cluster](#steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts), ultérieurement dans ce guide.

## <a name="requirements-related-to-failover-clusters-active-directory-domains-and-accounts"></a>Conditions spéciales requises relatives aux clusters de basculement, domaines Active Directory et comptes

Comme décrit dans les trois sections précédentes, certaines conditions doivent être remplies avant que les services et applications en cluster ne puissent être configurés avec succès sur un cluster de basculement. Les conditions les plus élémentaires concernent l'emplacement des nœuds de cluster (dans un domaine unique) et le niveau des autorisations du compte de la personne qui installe le cluster. Si ces conditions sont remplies, les autres comptes requis par le cluster peuvent être créés automatiquement par les Assistants de cluster de basculement. La liste suivante fournit des détails sur ces conditions de base.

  - **nœuds :** Tous les nœuds doivent se trouver dans le même domaine Active Directory. (Le domaine ne peut pas être basé sur Windows NT 4.0, qui n'inclut pas Active Directory.)  
      
  - **Compte de la personne qui installe le cluster :** La personne qui installe le cluster doit utiliser un compte avec les caractéristiques suivantes :  
      
      - Le compte doit être un compte de domaine. Il ne doit pas être nécessairement un compte d'administrateur de domaine. Il peut s'agir d'un compte d'utilisateur de domaine s'il remplit les autres conditions dans cette liste.  
          
      - Le compte doit avoir des autorisations administratives sur les serveurs qui deviendront des nœuds de cluster. La méthode la plus simple pour cela consiste à créer un compte d'utilisateur de domaine, puis à ajouter ce compte au groupe Administrateurs local sur chacun des serveurs qui deviendront des nœuds de cluster. Pour plus d'informations, consultez [Étapes de la configuration du compte pour la personne qui installe le cluster](#steps-for-configuring-the-account-for-the-person-who-installs-the-cluster), ultérieurement dans ce guide.  
          
      - Le compte (ou le groupe duquel le compte est un membre) doit recevoir les autorisations **Créer des objets d'ordinateur** et **Lire toutes les propriétés** dans le conteneur utilisé pour les comptes d'ordinateur dans le domaine. Vous pouvez également transformer le compte en compte d'administrateur de domaine. Pour plus d'informations, consultez [Étapes de la configuration du compte pour la personne qui installe le cluster](#steps-for-configuring-the-account-for-the-person-who-installs-the-cluster), ultérieurement dans ce guide.  
          
      - Si votre organisation choisit de prédéfinir le compte du nom du cluster (compte d'ordinateur avec le même nom que le cluster), le compte du nom du cluster prédéfini doit donner l'autorisation « Contrôle total » au compte de la personne qui installe le cluster. Pour obtenir d'autres détails importants sur la façon de prédéfinir le compte du nom du cluster, consultez [Étapes de la prédéfinition du compte du nom du cluster](#steps-for-prestaging-the-cluster-name-account), ultérieurement dans ce guide.  
          

### <a name="planning-ahead-for-password-resets-and-other-account-maintenance"></a>Planification à l'avance des réinitialisations de mots de passe et d'autres tâches de maintenance des comptes

Les administrateurs de clusters de basculement devront peut-être quelquefois réinitialiser le mot de passe du compte du nom du cluster. Cette action requiert une autorisation spécifique, l'autorisation **Réinitialiser le mot de passe**. Par conséquent, il est conseillé de modifier les autorisations du compte du nom du cluster (en utilisant le composant logiciel enfichable Utilisateurs et ordinateurs Active Directory) pour accorder l'autorisation **Réinitialiser le mot de passe** aux administrateurs du cluster pour le compte du nom du cluster. Pour plus d'informations, consultez [Étapes de la résolution de problèmes de mot de passe liés au compte du nom du cluster](#steps-for-troubleshooting-password-problems-with-the-cluster-name-account), ultérieurement dans ce guide.

## <a name="steps-for-configuring-the-account-for-the-person-who-installs-the-cluster"></a>Étapes de la configuration du compte pour la personne qui installe le cluster

Le compte de la personne qui installe le cluster est important parce qu'il fournit la base à partir de laquelle un compte d'ordinateur est créé pour le cluster lui-même.

L'appartenance à un groupe minimum obligatoire pour effectuer la procédure suivante varie selon si vous créez le compte de domaine et lui attribuez les autorisations requises dans le domaine, ou si vous placez uniquement le compte (créé par quelqu'un d'autre) dans le groupe **Administrateurs** local sur les serveurs qui seront des nœuds dans le cluster de basculement. Dans le premier cas, il est nécessaire d'appartenir au minimum au groupe **Opérateurs de compte** ou **Admins du domaine**, ou à un groupe équivalent. Dans le dernier cas, il suffit d'appartenir au groupe **Administrateurs** local ou à un groupe équivalent sur les serveurs qui seront des nœuds dans le cluster de basculement. Examinez les informations relatives à l’aide de comptes appropriés et les appartenances au groupe [ http://go.microsoft.com/fwlink/?LinkId=83477 ](http://go.microsoft.com/fwlink/?linkid=83477).

#### <a name="to-configure-the-account-for-the-person-who-installs-the-cluster"></a>Pour configurer le compte pour la personne qui installe le cluster

1.  Créez ou obtenez un compte de domaine pour la personne qui installe le cluster. Ce compte peut être un compte d'utilisateur de domaine ou un compte d'administrateur de domaine (dans **Admins du domaine** ou un groupe équivalent).

2.  Si le compte qui a été créé ou obtenu à l'étape 1 est un compte d'utilisateur de domaine, ou si les comptes d'administrateur de domaine dans votre domaine ne sont pas inclus automatiquement dans le groupe **Administrateurs** local sur les ordinateurs dans le domaine, ajoutez le compte au groupe **Administrateurs** local sur les serveurs qui seront des nœuds dans le cluster de basculement :
    
    1.  Cliquez successivement sur **Démarrer**, **Outils d'administration**, puis **Gestionnaire de serveur**.  
          
    2.  Dans l'arborescence de la console, développez **Configuration**, **Utilisateurs et groupes locaux**, puis **Groupes**.  
          
    3.  Dans le volet central, cliquez avec le bouton droit sur **Administrateurs**, cliquez sur **Ajouter au groupe**, puis sur **Ajouter**.  
          
    4.  Sous **Entrez les noms des objets à sélectionner**, tapez le nom du compte d’utilisateur qui a été créé ou obtenu à l’étape 1. Si vous y êtes invité, entrez un nom de compte et le mot de passe avec des autorisations suffisantes pour cette action. Cliquez sur **OK**.  
          
    5.  Répétez ces étapes sur chaque serveur qui sera un nœud dans le cluster de basculement.  
          
    

> [!IMPORTANT]
> Ces étapes doivent être répétées sur tous les serveurs qui seront des nœuds dans le cluster. 
<br>


3. Si le compte qui a été créé ou obtenu à l'étape 1 est un compte d'administrateur de domaine, ignorez le reste de cette procédure. Sinon, accordez au compte les autorisations **Créer des objets d'ordinateur** et **Lire toutes les propriétés** dans le conteneur utilisé pour les comptes d'ordinateur dans le domaine :
    
   1.  Sur un contrôleur de domaine, cliquez sur **Démarrer**, sur **Outils d'administration**, puis sur **Utilisateurs et ordinateurs Active Directory**. Si la boîte de dialogue **Contrôle de compte d’utilisateur** apparaît, confirmez que l’action affichée est celle que vous souhaitez, puis cliquez sur **Continuer**.  
          
   2.  Dans le menu **Affichage**, assurez-vous que **Fonctionnalités avancées** est sélectionné.  
          
       Lorsque **Fonctionnalités avancées** est sélectionné, l'onglet **Sécurité** s'affiche dans les propriétés des comptes (objets) dans **Utilisateurs et ordinateurs Active Directory**.  
          
   3.  Cliquez avec le bouton droit sur le conteneur **Ordinateurs** par défaut ou le conteneur par défaut dans lequel les comptes d'ordinateur sont créés dans votre domaine, puis cliquez sur **Propriétés**. **Ordinateurs** se trouve dans <b>Active Directory Users and Computers /</b><i>nœud de domaine</i><b>/Computers.</b>.  
          
   4.  Sous l’onglet **Sécurité**, cliquez sur **Avancé**.  
          
   5.  Cliquez sur **Ajouter**, tapez le nom du compte qui a été créé ou obtenu à l'étape 1, puis cliquez sur **OK**.  
          
   6.  Dans le **entrée d’autorisation pour *** conteneur* boîte de dialogue, recherchez le **créer des objets ordinateur** et **lire toutes les propriétés** autorisations et vous assurer que le **Autoriser** case à cocher est activée pour chacun d'entre eux.  
          
       ![](media/configure-ad-accounts/Cc731002.0a863ac5-2024-4f9f-8a4d-a419aff32fa0(WS.10).gif)

## <a name="steps-for-prestaging-the-cluster-name-account"></a>Étapes de la prédéfinition du compte du nom du cluster

Il est habituellement plus simple de ne pas prédéfinir le compte du nom du cluster, mais d'autoriser à la place la création et la configuration automatique du compte lorsque vous exécutez l'Assistant Création d'un cluster. Toutefois, s'il est nécessaire de prédéfinir le compte du nom du cluster en raison de spécifications dans votre organisation, utilisez la procédure suivante.

Pour mener à bien cette procédure, il est nécessaire d'appartenir au groupe **Admins du domaine** ou à un groupe équivalent. Examinez les informations relatives à l’aide de comptes appropriés et les appartenances au groupe [ http://go.microsoft.com/fwlink/?LinkId=83477 ](http://go.microsoft.com/fwlink/?linkid=83477). Notez que vous pouvez utiliser le même compte pour cette procédure que celui utilisé lors de la création du cluster.

#### <a name="to-prestage-a-cluster-name-account"></a>Pour prédéfinir un compte du nom du cluster

1.  Assurez-vous que vous connaissez le nom que le cluster portera et celui du compte d'utilisateur qui sera utilisé par la personne qui crée le cluster. (Notez que vous pouvez utiliser ce compte pour exécuter cette procédure.)

2.  Sur un contrôleur de domaine, cliquez sur **Démarrer**, sur **Outils d'administration**, puis sur **Utilisateurs et ordinateurs Active Directory**. Si la boîte de dialogue **Contrôle de compte d’utilisateur** apparaît, confirmez que l’action affichée est celle que vous souhaitez, puis cliquez sur **Continuer**.

3.  Dans l'arborescence de la console, cliquez avec le bouton droit sur **Ordinateurs** ou le conteneur par défaut dans lequel les comptes d'ordinateur sont créés dans votre domaine. **Ordinateurs** se trouve dans <b>Active Directory Users and Computers /</b><i>nœud de domaine</i><b>/Computers.</b>.

4.  Cliquez sur **Nouveau**, puis sur **Ordinateur**.

5.  Tapez le nom qui sera utilisé pour le cluster de basculement, en d'autres termes, le nom de cluster qui sera spécifié dans l'Assistant Création d'un cluster, puis cliquez sur **OK**.

6.  Cliquez avec le bouton droit sur le compte que vous avez créé, puis cliquez sur **Désactiver le compte**. Si vous êtes invité à confirmer votre choix, cliquez sur **Oui**.
    
    Le compte doit être désactivé afin que, lorsque l'Assistant **Création d'un cluster** est exécuté, celui-ci puisse confirmer que le compte qu'il utilisera pour le cluster n'est actuellement pas utilisé par un ordinateur ou cluster existant dans le domaine.

7.  Dans le menu **Affichage**, assurez-vous que **Fonctionnalités avancées** est sélectionné.
    
    Lorsque **Fonctionnalités avancées** est sélectionné, l'onglet **Sécurité** s'affiche dans les propriétés des comptes (objets) dans **Utilisateurs et ordinateurs Active Directory**.

8.  Cliquez sur le dossier que vous avez cliqué à l’étape 3, puis cliquez sur **propriétés**.

9.  Sous l’onglet **Sécurité**, cliquez sur **Avancé**.

10. Cliquez sur **Ajouter**, sur **Types d'objets** et assurez-vous que **Ordinateurs** est sélectionné, puis cliquez sur **OK**. Ensuite, sous **Entrez le nom de l'objet à sélectionner**, tapez le nom du compte d'ordinateur que vous venez de créer, puis cliquez sur **OK**. Si un message qui indique que vous allez ajouter un objet désactivé apparaît, cliquez sur **OK**.

11. Dans la boîte de dialogue **Autorisations pour** recherchez les autorisations **Créer des objets d'ordinateur** et **Lire toutes les propriétés** et assurez-vous que la case à cocher **Autoriser** est activée pour chacune.
    
    ![](media/configure-ad-accounts/Cc731002.f5977c4d-a62e-4b17-81e3-8c19ddca2078(WS.10).gif)

12. Cliquez sur **OK** jusqu'à ce que soyez de retour dans le composant logiciel enfichable **Utilisateurs et ordinateurs Active Directory**.

13. Si vous utilisez le même compte pour exécuter cette procédure que celui utilisé pour créer le cluster, ignorez les étapes restantes. Sinon, vous devez configurer des autorisations afin que le compte d'utilisateur qui sera utilisé pour créer le cluster ait un contrôle total du compte d'ordinateur que vous venez de créer :
    
    1.  Dans le menu **Affichage**, assurez-vous que **Fonctionnalités avancées** est sélectionné.  
          
    2.  Cliquez avec le bouton droit sur le compte d'ordinateur que vous avez créé, puis cliquez sur **Propriétés**.  
          
    3.  Sous l’onglet **Sécurité**, cliquez sur **Ajouter**. Si la boîte de dialogue **Contrôle de compte d’utilisateur** apparaît, confirmez que l’action affichée est celle que vous souhaitez, puis cliquez sur **Continuer**.  
          
    4.  Utilisez la boîte de dialogue **Sélectionner les utilisateurs, les ordinateurs ou les groupes** pour spécifier le compte d'utilisateur qui sera utilisé lors de la création du cluster. Cliquez sur **OK**.  
          
    5.  Assurez-vous que le compte d'utilisateur qui vous venez d'ajouter est sélectionné puis, en regard de **Contrôle total**, activez la case à cocher **Autoriser**.  
          
        ![](media/configure-ad-accounts/Cc731002.fffaafe2-a494-498b-974c-8f9d70f7103b(WS.10).gif)

## <a name="steps-for-prestaging-an-account-for-a-clustered-service-or-application"></a>Étapes de la prédéfinition d'un compte pour un service ou une application en cluster

Il est habituellement plus simple de ne pas prédéfinir le compte d'ordinateur pour un service ou une application en cluster, mais d'autoriser à la place la création et la configuration automatique du compte lorsque vous exécutez l'Assistant Haute disponibilité. Toutefois, s'il est nécessaire de prédéfinir les comptes en raison de spécifications dans votre organisation, utilisez la procédure suivante.

Pour mener à bien cette procédure, il est nécessaire d'appartenir au minimum au groupe **Opérateurs de compte** ou à un groupe équivalent. Examinez les informations relatives à l’aide de comptes appropriés et les appartenances au groupe [ http://go.microsoft.com/fwlink/?LinkId=83477 ](http://go.microsoft.com/fwlink/?linkid=83477).

#### <a name="to-prestage-an-account-for-a-clustered-service-or-application"></a>Pour prédéfinir un compte pour un service ou une application en cluster

1.  Assurez-vous que vous connaissez le nom du cluster et celui que le service ou l'application en cluster portera.

2.  Sur un contrôleur de domaine, cliquez sur **Démarrer**, sur **Outils d'administration**, puis sur **Utilisateurs et ordinateurs Active Directory**. Si la boîte de dialogue **Contrôle de compte d’utilisateur** apparaît, confirmez que l’action affichée est celle que vous souhaitez, puis cliquez sur **Continuer**.

3.  Dans l'arborescence de la console, cliquez avec le bouton droit sur **Ordinateurs** ou le conteneur par défaut dans lequel les comptes d'ordinateur sont créés dans votre domaine. **Ordinateurs** se trouve dans <b>Active Directory Users and Computers /</b><i>nœud de domaine</i><b>/Computers.</b>.

4.  Cliquez sur **Nouveau**, puis sur **Ordinateur**.

5.  Tapez le nom que vous utiliserez pour le service ou l'application en cluster, puis cliquez sur **OK**.

6.  Dans le menu **Affichage**, assurez-vous que **Fonctionnalités avancées** est sélectionné.
    
    Lorsque **Fonctionnalités avancées** est sélectionné, l'onglet **Sécurité** s'affiche dans les propriétés des comptes (objets) dans **Utilisateurs et ordinateurs Active Directory**.

7.  Cliquez avec le bouton droit sur le compte d'ordinateur que vous avez créé, puis cliquez sur **Propriétés**.

8.  Sous l’onglet **Sécurité**, cliquez sur **Ajouter**.

9.  Cliquez sur **Types d'objets** et assurez-vous que **Ordinateurs** est sélectionné, puis cliquez sur **OK**. Ensuite, sous **Entrez le nom de l'objet à sélectionner**, tapez le compte du nom du cluster, puis cliquez sur **OK**. Si un message qui indique que vous allez ajouter un objet désactivé apparaît, cliquez sur **OK**.

10. Assurez-vous que le compte du nom du cluster est sélectionné puis, en regard de **Contrôle total**, activez la case à cocher **Autoriser**.
    
    ![](media/configure-ad-accounts/Cc731002.2e614376-87a6-453a-81ba-90ff7ebc3fa2(WS.10).gif)

## <a name="steps-for-troubleshooting-problems-related-to-accounts-used-by-the-cluster"></a>Étapes de la résolution de problèmes liés aux comptes utilisées par le cluster

Comme décrit précédemment dans ce guide, lorsque vous créez un cluster de basculement et configurez les services ou applications en cluster, les Assistants de cluster de basculement créent les comptes Active Directory nécessaires et leur donnent les autorisations appropriées. Si un compte nécessaire est supprimé, ou si des autorisations nécessaires sont modifiées, des problèmes peuvent se poser. Les sous-sections suivantes fournissent les étapes de la résolution de ces problèmes.

  - [Étapes de résolution des problèmes de mot de passe avec le compte du nom du cluster](#steps-for-troubleshooting-password-problems-with-the-cluster-name-account)
      
  - [Étapes pour résoudre les problèmes causés par des modifications dans les comptes Active Directory associés au cluster](#steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts)
      

### <a name="steps-for-troubleshooting-password-problems-with-the-cluster-name-account"></a>Étapes de la résolution de problèmes de mot de passe liés au compte du nom du cluster

Utilisez cette procédure s'il y a un message d'événement relatif aux objets ordinateur ou à l'identité de cluster qui inclut le texte suivant. Notez que ce texte figurera dans le message d'événement et non au début du message d'événement :

`Logon failure: unknown user name or bad password.`

Les messages d'événement qui correspondent à la description précédente indiquent que le mot de passe pour le compte du nom du cluster et le mot de passe correspondant stocké par le logiciel de clustering ne correspondent plus.

Pour plus d’informations sur la garantie que les administrateurs de cluster disposent des autorisations appropriées pour effectuer la procédure suivante, en fonction des besoins, consultez Planification à l’avance pour les réinitialisations de mot de passe et d’autres tâches de maintenance de compte, plus haut dans ce guide.

L'appartenance au groupe local **Administrateurs**, ou équivalent, est la condition minimale requise pour effectuer cette procédure. En outre, l'autorisation **Réinitialiser le mot de passe** doit être attribuée à votre compte pour le compte du nom du cluster (à moins que votre compte ne soit un compte **Admins du domaine** ou le propriétaire du créateur du compte du nom du cluster). Le compte qui a été utilisé par la personne qui a installé le cluster peut être utilisé pour cette procédure. Examinez les informations relatives à l’aide de comptes appropriés et les appartenances au groupe [ http://go.microsoft.com/fwlink/?LinkId=83477 ](http://go.microsoft.com/fwlink/?linkid=83477).

#### <a name="to-troubleshoot-password-problems-with-the-cluster-name-account"></a>Pour résoudre les problèmes de mot de passe liés au compte du nom du cluster

1.  Pour ouvrir le composant logiciel enfichable Gestion du cluster de basculement, cliquez sur **Démarrer**, sur **Outils d'administration**, puis sur **Gestion du cluster de basculement**. (Si le **contrôle de compte d’utilisateur** boîte de dialogue s’affiche, vérifiez que l’action affichée est ce que vous souhaitez, puis cliquez sur **continuer**.)

2.  Dans le composant logiciel enfichable Gestion du cluster de basculement, si le cluster que vous souhaitez configurer n'est pas affiché, cliquez avec le bouton droit sur **Gestion du cluster de basculement** dans l'arborescence de la console, cliquez sur **Gérer un cluster**, puis sélectionnez ou spécifiez le cluster voulu.

3.  Dans le volet central, développez **Principales ressources de cluster**.

4.  Sous **Nom du cluster**, cliquez avec le bouton droit sur l'élément **Nom**, pointez sur **Autres actions**, puis cliquez sur **Réparer l'objet Active Directory**.

### <a name="steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts"></a>Étapes de la résolution de problèmes causés par des modifications dans les comptes Active Directory associés au cluster

Si le compte du nom du cluster est supprimé ou que des autorisations en sont retirées, des problèmes se produiront lorsque vous tenterez de configurer un nouveau service ou une nouvelle application en cluster. Pour résoudre un problème avec cette cause possible, utilisez le composant logiciel enfichable Utilisateurs et ordinateurs Active Directory pour afficher ou modifier le compte du nom du cluster et d'autres comptes connexes. Pour plus d’informations sur les événements sont consignés lorsque ce type de problème produit (événement 1193, 1194, 1206 ou 1207), voir [ http://go.microsoft.com/fwlink/?LinkId=118271 ](http://go.microsoft.com/fwlink/?linkid=118271).

Pour mener à bien cette procédure, il est nécessaire d'appartenir au groupe **Admins du domaine** ou à un groupe équivalent. Examinez les informations relatives à l’aide de comptes appropriés et les appartenances au groupe [ http://go.microsoft.com/fwlink/?LinkId=83477 ](http://go.microsoft.com/fwlink/?linkid=83477).

#### <a name="to-troubleshoot-problems-caused-by-changes-in-cluster-related-active-directory-accounts"></a>Pour résoudre les problèmes causés par des modifications dans les comptes Active Directory associés au cluster

1.  Sur un contrôleur de domaine, cliquez sur **Démarrer**, sur **Outils d'administration**, puis sur **Utilisateurs et ordinateurs Active Directory**. Si la boîte de dialogue **Contrôle de compte d’utilisateur** apparaît, confirmez que l’action affichée est celle que vous souhaitez, puis cliquez sur **Continuer**.

2.  Développez le conteneur **Ordinateurs** par défaut ou le dossier dans lequel le compte du nom du cluster (compte d'ordinateur pour le cluster) se trouve. **Ordinateurs** se trouve dans <b>Active Directory Users and Computers /</b><i>nœud de domaine</i><b>/Computers.</b>.

3.  Examinez l'icône pour le compte du nom du cluster. Elle ne doit pas comporter de flèche pointant vers le bas, autrement dit, le compte ne doit pas être désactivé. S'il semble désactivé, cliquez dessus avec le bouton droit et recherchez la commande **Activer le compte**. Si vous voyez la commande, cliquez dessus.

4.  Dans le menu **Affichage**, assurez-vous que **Fonctionnalités avancées** est sélectionné.
    
    Lorsque **Fonctionnalités avancées** est sélectionné, l'onglet **Sécurité** s'affiche dans les propriétés des comptes (objets) dans **Utilisateurs et ordinateurs Active Directory**.

5.  Cliquez avec le bouton droit sur le conteneur **Ordinateurs** par défaut ou le dossier dans lequel le compte du nom du cluster se trouve.

6.  Cliquez sur **Propriétés**.

7.  Sous l’onglet **Sécurité**, cliquez sur **Avancé**.

8.  Dans la liste des comptes avec autorisations, cliquez sur le compte du nom du cluster, puis sur **Modifier**.
    

> [!NOTE]
> Si le compte du nom du cluster n'apparaît pas, cliquez sur <STRONG>Ajouter</STRONG> et ajoutez-le à la liste. 
<br>


9. Pour le compte du nom du cluster (également appelé objet nom de cluster), assurez-vous que la case à cocher **Autoriser** est activée pour les autorisations **Créer des objets d'ordinateur** et **Lire toutes les propriétés**.
    
   ![](media/configure-ad-accounts/Cc731002.f5977c4d-a62e-4b17-81e3-8c19ddca2078(WS.10).gif)

10. Cliquez sur **OK** jusqu'à ce que soyez de retour dans le composant logiciel enfichable **Utilisateurs et ordinateurs Active Directory**.

11. Examinez les stratégies de domaine (en consultant un administrateur de domaine le cas échéant) connexes à la création de comptes d'ordinateur (objets). Vérifiez que le compte du nom du cluster peut créer un compte d'ordinateur chaque fois que vous configurez un service ou une application en cluster. Par exemple, si votre administrateur de domaine a configuré des paramètres qui aboutissent à la création de tous les comptes d'ordinateur dans un conteneur spécialisé plutôt que dans le conteneur **Ordinateurs** par défaut, assurez-vous que ces paramètres permettent également au compte du nom du cluster de créer des comptes d'ordinateur dans ce conteneur.

12. Développez le conteneur **Ordinateurs** par défaut ou le conteneur dans lequel le compte d'ordinateur pour l'un des services ou applications en cluster se trouve.

13. Cliquez avec le bouton droit sur le compte d'ordinateur pour l'un des services ou applications en cluster, puis cliquez sur **Propriétés**.

14. Sous l'onglet **Sécurité**, confirmez que le compte du nom du cluster est répertorié parmi les comptes qui ont des autorisations, et sélectionnez-le. Confirmez que le compte du nom du cluster a l'autorisation **Contrôle total** (la case à cocher **Autoriser** est activée). Dans le cas contraire, ajoutez le compte du nom du cluster à la liste et donnez-lui l'autorisation **Contrôle total**.
    
   ![](media/configure-ad-accounts/Cc731002.2e614376-87a6-453a-81ba-90ff7ebc3fa2(WS.10).gif)

15. Répétez les étapes 13 et 14 pour chaque service et application en cluster configuré dans le cluster.

16. Vérifiez que le quota à l'échelle du domaine pour la création d'objets ordinateur (par défaut, 10) n'a pas été atteint (en consultant un administrateur de domaine le cas échéant). Si les éléments précédents dans cette procédure ont tous été examinés et corrigés, et si le quota a été atteint, envisagez d'augmenter le quota. Pour modifier le quota :
    
   1.  Ouvrez une invite de commandes en tant qu'administrateur et exécutez **ADSIEdit.msc**.  
          
   2.  Cliquez avec le bouton droit sur **Modification ADSI**, cliquez sur **Connexion**, puis sur **OK**. Le **Contexte d'attribution de noms par défaut** est ajouté à l'arborescence de la console.  
          
   3.  Double-cliquez sur **Contexte d'attribution de noms par défaut**, cliquez avec le bouton droit sur l'objet de domaine situé en dessous, puis cliquez sur **Propriétés**.  
          
   4.  Faites défiler jusqu'à **ms-DS-MachineAccountQuota**, sélectionnez-le, cliquez sur **Modifier**, modifiez la valeur, puis cliquez sur **OK**.

