---
title: Ajouter des serveurs au Gestionnaire de serveur
description: Gestionnaire de serveur
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aab895f2-fe4d-4408-b66b-cdeadbd8969e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.localizationpriority: medium
ms.date: 02/01/2018
ms.openlocfilehash: a47ecbc0c7359438ed60ed34c94adf0096b14967
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435451"
---
# <a name="add-servers-to-server-manager"></a>Ajouter des serveurs au Gestionnaire de serveur

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dans Windows Server, vous pouvez gérer plusieurs serveurs distants à l’aide d’une seule console du Gestionnaire de serveur. Les serveurs que vous voulez gérer à l’aide du Gestionnaire de serveur peuvent exécuter Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008. Notez que vous ne pouvez pas gérer une version plus récente de Windows Server avec une version antérieure du Gestionnaire de serveur.

Cette rubrique explique comment ajouter des serveurs au pool de serveurs du Gestionnaire de serveur.

> [!NOTE]
> Dans nos tests, le Gestionnaire de serveur dans Windows Server 2012 et les versions ultérieures de Windows Server permet de gérer jusqu’à 100 serveurs qui sont configurés avec une charge de travail classique. Le nombre de serveurs que vous pouvez gérer en utilisant une console du Gestionnaire de serveur unique peut varier en fonction de la quantité de données que vous demandez aux serveurs gérés ainsi que des ressources matérielles et réseau disponibles pour l’ordinateur exécutant le Gestionnaire de serveur. Lorsque la quantité de données à afficher est sur le point d’atteindre la capacité des ressources de cet ordinateur, vous pouvez être confronté à des réactions lentes du Gestionnaire de serveur ainsi qu’à des retards dans la réalisation des actualisations. Pour augmenter le nombre de serveurs que vous pouvez gérer à l’aide du Gestionnaire de serveur, nous vous conseillons de limiter la quantité de données d’événement que le Gestionnaire de serveur obtient de vos serveurs gérés en utilisant les paramètres de la boîte de dialogue **Configurer les données d’événement**. Configurer les données d’événement peut être ouverte à partir du menu **Tâches** dans la vignette **Événements** . Si vous devez gérer un très grand nombre de serveurs dans votre organisation, nous vous recommandons d’évaluer des produits de la [suite Microsoft System Center](https://go.microsoft.com/fwlink/p/?LinkId=239437).
> 
> Le Gestionnaire de serveur peut recevoir uniquement des informations sur l’état en ligne ou hors connexion en provenance de serveurs exécutant Windows Server 2003. Vous pouvez utiliser le Gestionnaire de serveur pour effectuer des tâches de gestion sur les serveurs qui exécutent Windows Server 2008 R2 ou Windows Server 2008, mais vous ne pouvez pas ajouter de rôles ni de fonctionnalités aux serveurs qui exécutent Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.
> 
> Le Gestionnaire de serveur ne peut pas être utilisé pour gérer une version plus récente du système d’exploitation Windows Server. Le Gestionnaire de serveur en cours d'exécution sur Windows Server 2012 R2, Windows Server 2012, Windows 8.1 ou Windows 8 ne peut pas être utilisé pour gérer les serveurs qui exécutent Windows Server 2016.

Cette rubrique contient les sections suivantes.

-   [Ajouter des serveurs à gérer](#BKMK_add)

-   [Fournir des informations d’identification avec la commande gérer en tant que](#BKMK_creds)

## <a name="BKMK_creds"></a>Fournir des informations d’identification avec la commande gérer en tant que
Lorsque vous ajoutez des serveurs distants au Gestionnaire de serveur, certains des serveurs que vous ajoutez peuvent nécessiter des informations d’identification de compte utilisateur différentes à des fins d’accès ou de gestion. Pour spécifier des informations d’identification relatives à un serveur géré qui diffèrent de celles que vous utilisez pour vous connecter à l’ordinateur sur lequel vous exécutez le Gestionnaire de serveur, utilisez la commande **Gérer en tant que** après avoir ajouté un serveur au Gestionnaire de serveur, qui est accessible en cliquant avec le bouton droit sur l’entrée d’un serveur géré dans la vignette **Serveurs** d’une page d’accueil de rôle ou de groupe. Cliquez sur **Gérer en tant que** pour ouvrir la boîte de dialogue **Sécurité Windows** , dans laquelle vous pouvez indiquer un nom d’utilisateur disposant de droits d’accès sur le serveur géré, selon l’un des formats suivants.

-   *Nom d'utilisateur*

-   *Nom d’utilisateur*@example.domain.com

-   *Domainee*\\*Nom d’utilisateur*

La boîte de dialogue **Sécurité Windows** ouverte par la commande **Gérer en tant que** ne peut pas accepter les informations d’identification de carte à puce. L’indication d’informations d’identification de carte à puce via le Gestionnaire de serveur n’est pas prise en charge. Les informations d’identification fournies pour un serveur géré à l’aide de la commande **Gérer en tant que** sont mises en cache et persistent tant que vous gérez le serveur en utilisant le même ordinateur que celui sur lequel vous exécutez le Gestionnaire de serveur, ou tant que vous ne pas les remplacez pas par des informations d’identification différentes pour le même serveur ou aucune information d’identification. Si vous exportez les paramètres de votre Gestionnaire de serveur sur d’autres ordinateurs, ou configurez votre profil de domaine pour le rendre itinérant et permettre l’utilisation des paramètres du Gestionnaire de serveur sur d’autres ordinateurs, les informations d’identification **Gérer en tant que** des serveurs de votre pool de serveurs ne sont pas stockées dans le profil itinérant. Les utilisateurs du Gestionnaire de serveur doivent les ajouter à chaque ordinateur à partir duquel ils veulent effectuer la gestion.

Après avoir ajouté des serveurs à gérer en suivant les procédures décrites dans cette rubrique, mais avant d’utiliser la commande **Gérer en tant que** pour spécifier d’autres informations d’identification qui peuvent être nécessaires pour gérer un serveur ajouté, il est possible d’afficher les erreurs d’état de la gestion suivantes pour le serveur :

-   Erreur de résolution de cible Kerberos

-   Erreur d’authentification Kerberos

-   En ligne - Accès refusé

> [!NOTE]
> Les rôles et fonctionnalités ne prenant pas en charge la commande **Gérer en tant que** incluent les Services Bureau à distance (RDS) et le serveur de gestion des adresses IP (IPAM). Si vous ne pouvez pas gérer les Services Bureau à distance (RDS) ou le serveur de gestion des adresses IP (IPAM) à l’aide des mêmes informations d’identification utilisées sur l’ordinateur sur lequel le Gestionnaire de serveur s’exécute, essayez d’ajouter le compte que vous utilisez généralement pour gérer ces serveurs à distance au groupe Administrateurs sur l’ordinateur qui exécute le Gestionnaire de serveur. Puis, ouvrez une session sur l’ordinateur qui exécute le Gestionnaire de serveur avec le compte que vous utilisez pour gérer le serveur distant qui exécute les Services Bureau à distance ou IPAM.

## <a name="BKMK_add"></a>Ajouter des serveurs à gérer
Vous pouvez ajouter des serveurs à administrer avec le Gestionnaire de serveur à l’aide de l’une des trois méthodes proposées dans la boîte de dialogue **Ajouter des serveurs**.

-   **Active Directory Domain Services** ajoute des serveurs à gérer qu’Active Directory trouve dans le même domaine que l’ordinateur local.

-   **Entrée DNS (Domain Name System)** Recherchez les serveurs à gérer par nom d’ordinateur ou adresse IP.

-   **Importer plusieurs serveurs** Spécifiez plusieurs serveurs à importer dans un fichier qui contient les serveurs répertoriés par nom d’ordinateur ou adresse IP.

#### <a name="to-add-servers-to-the-server-pool"></a>Pour ajouter des serveurs au pool de serveurs

1.  Si le Gestionnaire de serveur est déjà ouvert, passez à l’étape suivante. S’il n’est pas déjà ouvert, ouvrez-le en effectuant l’une des opérations suivantes.

    -   Sur le Bureau Windows, démarrez le Gestionnaire de serveur en cliquant sur **Gestionnaire de serveur** dans la barre des tâches Windows.

    -   Dans l'écran d'**accueil** de Windows, cliquez sur la vignette Gestionnaire de serveur.

2.  Dans le menu **Gérer**, cliquez sur **Ajouter des serveurs**.

3.  Effectuez l’une des opérations suivantes :

    -   Sous l’onglet **Active Directory**, sélectionnez des serveurs qui se trouvent dans le domaine actif. Appuyez sur **Ctrl** pendant que vous sélectionnez plusieurs serveurs. Cliquez sur la flèche Droite pour déplacer les serveurs sélectionnés vers la liste **sélectionné**.

    -   Sous l’onglet **DNS** , tapez les premiers caractères d’un nom d’ordinateur ou d’une adresse IP, puis appuyez sur **Entrée** ou cliquez sur **Rechercher**. Choisissez les serveurs à ajouter, puis cliquez sur la flèche Droite.

    -   Sous l’onglet **importer**, naviguez jusqu’à un fichier texte qui contient les noms DNS ou adresses IP des ordinateurs que vous souhaitez afficher, un nom ou une adresse IP par ligne.

4.  Une fois que vous avez fini d’ajouter des serveurs, cliquez sur **OK**.

### <a name="add-and-manage-servers-in-workgroups"></a>Ajouter et gérer des serveurs dans des groupes de travail
Bien que l’ajout de serveurs appartenant à des groupes de travail au Gestionnaire de serveur puisse réussir, une fois ces serveurs ajoutés, la colonne **Facilité de gestion** de la vignette **Serveurs** dans une page de rôle ou de serveur qui comprend un serveur de groupe de travail peut afficher des erreurs **Informations d’identification non valide** qui se produisent lors d’une tentative de connexion ou de collecte de données à partir du serveur de groupe de travail distant.

Ces erreurs ou des erreurs similaires peuvent se produire dans les conditions suivantes.

-   Le serveur géré est dans le même groupe de travail que l’ordinateur qui exécute le Gestionnaire de serveur.

-   Le serveur géré est dans un groupe de travail différent de l’ordinateur qui exécute le Gestionnaire de serveur.

-   Un des ordinateurs est dans un groupe de travail, tandis que l’autre est dans un domaine.

-   L’ordinateur qui exécute le Gestionnaire de serveur est dans un groupe de travail et les serveurs gérés distants sont sur un sous-réseau différent.

-   Les deux ordinateurs se trouvent dans des domaines, mais il n’existe aucune relation d’approbation entre les deux domaines.

-   Les deux ordinateurs se trouvent dans des domaines, mais il n’existe qu’une relation d’approbation à sens unique entre les deux domaines.

-   Le serveur que vous souhaitez gérer a été ajouté à l’aide de son adresse IP.

##### <a name="to-add-remote-workgroup-servers-to-server-manager"></a>Pour ajouter des serveurs de groupe de travail distant au Gestionnaire de serveur

1.  Sur l’ordinateur qui exécute le Gestionnaire de serveur, ajoutez le nom du serveur de groupe de travail à la liste **TrustedHosts**. Cette opération est obligatoire dans le cadre de l’authentification NTLM. Pour ajouter un nom d’ordinateur à une liste existante d’hôtes approuvés, ajoutez le paramètre `Concatenate` à la commande suivante : Par exemple, pour ajouter l’ordinateur `Server01` à une liste existante d’hôtes approuvés, utilisez la commande suivante :

    ```
    Set-Item wsman:\localhost\Client\TrustedHosts Server01 -Concatenate -force
    ```

2.  Déterminez si le serveur de groupe de travail que vous souhaitez gérer se trouve sur le même sous-réseau que l’ordinateur sur lequel vous exécutez le Gestionnaire de serveur.

    Si les deux ordinateurs sont sur le même sous-réseau ou si le profil réseau du serveur de groupe de travail a la valeur **Privé** dans le **Centre Réseau et partage**, continuez à l’étape suivante.

    S’ils ne sont pas sur le même sous-réseau ou si le profil réseau du serveur de groupe de travail n’a pas la valeur **Privé**, sur le serveur de groupe de travail, modifiez le paramètre **Gestion à distance de Windows (HTTP-Entrée)** entrant dans le Pare-feu Windows de façon à autoriser de manière explicite les connexions en provenance des ordinateurs distants en ajoutant les noms d’ordinateurs sous l’onglet **ordinateurs** de la boîte de dialogue **Propriétés** du paramètre.

3.  > [!IMPORTANT]
    > L’exécution de l’applet de commande à cette étape a pour effet de remplacer les mesures de Contrôle de compte d’utilisateur (UAC) qui empêchent l’exécution de processus élevés sur les ordinateurs de groupe de travail, sauf si le compte Administrateur ou Système intégré exécute ces processus. L’applet de commande permet aux membres du groupe Administrateurs de gérer le serveur de groupe de travail sans se connecter via le compte Administrateur intégré. En autorisant des utilisateurs supplémentaires à gérer le serveur de groupe de travail, vous pouvez réduire la sécurité de ce dernier. Cette opération est néanmoins plus sûre que de fournir les informations d’identification d’un compte Administrateur intégré à une multitude possible de personnes chargées de gérer le serveur de groupe de travail.

    Pour outrepasser les restrictions du contrôle de compte d’utilisateur relatives à l’exécution de processus élevés sur les ordinateurs de groupe de travail, créez une entrée de Registre nommée **LocalAccountTokenFilterPolicy** sur l’ordinateur de groupe de travail en exécutant l’applet de commande suivante.

    ```
    New-ItemProperty -Name LocalAccountTokenFilterPolicy -path HKLM:\SOFTWARE\Microsoft\Windows\Currentversion\Policies\System -propertytype DWord -value 1
    ```

4.  Sur l’ordinateur sur lequel vous exécutez le Gestionnaire de serveur, ouvrez la page **Tous les serveurs**.

5.  Si l’ordinateur qui exécute le Gestionnaire de serveur et le serveur de groupe de travail cible appartiennent au même groupe de travail, passez à la dernière étape. Si les deux ordinateurs n’appartiennent pas au même groupe de travail, cliquez avec le bouton droit sur le serveur de groupe de travail cible dans la vignette **Serveurs** , puis cliquez sur **Gérer en tant que**.

6.  Connectez-vous au serveur de groupe de travail à l‘aide du compte Administrateur intégré de ce dernier.

7.  Vérifiez que le Gestionnaire de serveur peut se connecter au serveur de groupe de travail et y recueillir des données en actualisant la page **Tous les serveurs** puis en observant l’état de facilité de gestion du serveur de groupe de travail.

##### <a name="to-add-remote-servers-when-server-manager-is-running-on-a-workgroup-computer"></a>Pour ajouter des serveurs distants lorsque le Gestionnaire de serveur fonctionne sur un ordinateur de groupe de travail

1.  Sur l’ordinateur qui exécute le Gestionnaire de serveur, ajoutez des serveurs distants à la liste **TrustedHosts** de l’ordinateur local dans une session Windows PowerShell. Pour ajouter un nom d’ordinateur à une liste existante d’hôtes approuvés, ajoutez le paramètre `Concatenate` à la commande suivante : Par exemple, pour ajouter l’ordinateur `Server01` à une liste existante d’hôtes approuvés, utilisez la commande suivante :

    ```
    Set-Item wsman:\localhost\Client\TrustedHosts Server01 -Concatenate -force
    ```

2.  Déterminez si le serveur que vous souhaitez gérer se trouve sur le même sous-réseau que l’ordinateur de groupe de travail sur lequel vous exécutez le Gestionnaire de serveur.

    Si les deux ordinateurs sont sur le même sous-réseau ou si le profil réseau de l'ordinateur de groupe de travail a la valeur **Privé** dans le **Centre Réseau et partage**, continuez à l’étape suivante.

    S’ils ne sont pas sur le même sous-réseau ou si le profil réseau du serveur de groupe de travail n’a pas la valeur **Privé**, sur l’ordinateur de groupe de travail qui exécute le Gestionnaire de serveur, modifiez le paramètre **Gestion à distance de Windows (HTTP-Entrée)** entrant dans le Pare-feu Windows de façon à autoriser de manière explicite les connexions en provenance des ordinateurs distants en ajoutant les noms d’ordinateurs sous l’onglet **ordinateurs** de la boîte de dialogue **Propriétés** du paramètre.

3.  Sur l’ordinateur sur lequel vous exécutez le Gestionnaire de serveur, ouvrez la page **Tous les serveurs**.

4.  Vérifiez que le Gestionnaire de serveur peut se connecter au serveur de groupe de travail et y recueillir des données en actualisant la page **Tous les serveurs** puis en observant l’état de facilité de gestion du serveur distant. Sir la vignette **Serveurs** affiche toujours une erreur de gestion pour le serveur distant, continuez à l’étape suivante.

5.  Déconnectez-vous de l’ordinateur sur lequel vous exécutez le Gestionnaire de serveur, puis reconnectez-vous à l’aide du compte Administrateur intégré. Répétez l’étape précédente pour vérifier que le Gestionnaire de serveur peut se connecter au serveur distant et y recueillir des données.

Si vous avez suivi les procédures de cette section mais continuez à rencontrer des problèmes de gestion d’ordinateurs de groupe de travail ou d’autres ordinateurs définis par des ordinateurs de groupe de travail, voir [about_remote_Troubleshooting](https://technet.microsoft.com/library/dd347642.aspx) sur le site web de Microsoft.

### <a name="add-and-manage-servers-in-clusters"></a>Ajouter et gérer des serveurs dans des clusters
Vous pouvez utiliser le Gestionnaire de serveur pour gérer les serveurs qui se trouvent dans des clusters de basculement (également appelés clusters de serveur ou MSCS). Les serveurs se trouvant dans des clusters de basculement (qu’il s’agisse de nœuds de cluster physiques ou virtuels) ont des comportements et des contraintes de gestion uniques dans le Gestionnaire de serveur.

-   Les serveurs physiques et virtuels de clusters sont automatiquement ajoutés au Gestionnaire de serveur lorsqu’un seul serveur du cluster est ajouté au Gestionnaire de serveur. De même, lorsque vous supprimez un serveur en cluster à partir du Gestionnaire de serveur, vous êtes invité à supprimer d’autres serveurs du cluster.

-   Le Gestionnaire de serveur n’affiche pas les données des serveurs virtuels en cluster, car les données sont dynamiques et identiques à celles du serveur sur lequel le nœud en cluster virtuel est hébergé. Vous pouvez sélectionner le serveur qui héberge le serveur virtuel pour afficher ses données.

-   Si vous ajoutez un serveur au Gestionnaire de serveur en utilisant le nom d’objet de cluster virtuel du serveur, le nom d’objet virtuel s’affiche dans le Gestionnaire de serveur au lieu du nom de serveur physique (attendu).

-   Vous ne pouvez pas installer de rôles et de fonctionnalités sur un serveur virtuel en cluster.

## <a name="see-also"></a>Voir aussi
[Gestionnaire de serveur](server-manager.md)
[créer et gérer des groupes de serveurs](create-and-manage-server-groups.md)
