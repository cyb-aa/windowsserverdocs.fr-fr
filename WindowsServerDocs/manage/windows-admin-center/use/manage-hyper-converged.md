---
title: Gérer l’Infrastructure Hyper-convergée avec Windows Admin Center
description: Gérer l’Infrastructure Hyper-convergée avec Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 02/11/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 4d849120d2daaa40cb797cc5e7d4c23c74da5bb7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874260"
---
# <a name="manage-hyper-converged-infrastructure-with-windows-admin-center"></a>Gérer l’Infrastructure Hyper-convergée avec Windows Admin Center

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

## <a name="what-is-hyper-converged-infrastructure"></a>Qu’est Hyper-Converged Infrastructure

Infrastructure Hyper-convergée consolide calcul défini par logiciel, de stockage et de mise en réseau dans un cluster à hautes performances, économique, de fournir et de virtualisation très évolutive. Cette fonctionnalité a été introduite dans Windows Server 2016 avec [espaces de stockage Direct](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview) et [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server).

> [!Tip]
> Cherchez à acquérir Hyper-Converged Infrastructure ? Microsoft recommande ces [défini par le logiciel Windows Server](https://microsoft.com/wssd) solutions proposées par nos partenaires. Elles sont conçues, assemblées et validés par rapport à notre architecture de référence pour garantir la compatibilité et la fiabilité, afin de vous être opérationnel rapidement opérationnel.

> [!IMPORTANT]
> Certaines des fonctionnalités décrites dans cet article sont uniquement disponibles en version préliminaire de Windows Admin Center. [Comment obtenir cette version ?](http://aka.ms/windowsadmincenter)

## <a name="what-is-windows-admin-center"></a>Qu'est-ce que Windows Admin Center ?

[Windows Admin Center](../understand/windows-admin-center.md) est l’outil de gestion de la nouvelle génération pour Windows Server, le successeur d’outils « dans « l’emploi traditionnels tels que le Gestionnaire de serveur. C’est gratuit et peut être installé et utilisé sans connexion Internet. Vous pouvez utiliser Windows Admin Center pour gérer et surveiller l’Infrastructure Hyper-Converged exécutant Windows Server 2016 ou une version d’évaluation de Insider de Windows Server 2019.

![Tableau de bord de cluster hyperconvergé](../media/manage-hyper-converged/hci-dashboard-v1809.png)

## <a name="key-features"></a>Principales fonctionnalités

Principales caractéristiques de Windows Admin Center pour l’Infrastructure de Hyper-Converged sont les suivantes :

- **Unifiée unique-volet-de-verre de calcul, stockage et bientôt mise en réseau.** Afficher vos machines virtuelles, les serveurs hôtes, volumes, disques, etc. au sein d’une expérience spécialisée, cohérente et interconnectée.
- **Créez et gérez des ordinateurs virtuels Hyper-V et les espaces de stockage.** Les workflows radicalement simples pour créer, ouvrir, redimensionner et supprimer des volumes ; créer, Démarrer, se connecter à et déplacer des machines virtuelles ; et bien plus encore.
- **Puissante analyse à l’échelle du cluster.** Le tableau de bord graphiques mémoire et l’utilisation du processeur, la capacité de stockage, e/s, le débit et latence en temps réel, sur chaque serveur dans le cluster, avec l’effacement des alertes lorsque quelque chose ne convient pas.
- **Prise en charge de Software Defined Networking (SDN). (Nouveautés de la version préliminaire de Windows Admin Center)**  Gérer et surveiller les réseaux virtuels, sous-réseaux, connectent des machines virtuelles aux réseaux virtuels et surveiller l’infrastructure SDN.

Windows Admin Center pour l’Infrastructure de Hyper-Converged est activement développé par Microsoft. Il reçoit des mises à jour fréquentes qui améliorent les fonctionnalités existantes et ajoutent de nouvelles fonctionnalités.

## <a name="before-you-start"></a>Avant de commencer

Pour gérer votre cluster en tant qu’Infrastructure Hyper-Converged dans Windows Admin Center, il doit exécuter Windows Server 2016 ou une version d’évaluation de Windows Server 2019, et disposer Hyper-V et espaces de stockage Direct.

> [!Tip]
> Windows Admin Center offre également une expérience de gestion générale pour n’importe quel cluster prenant en charge des charges de travail, disponible pour Windows Server 2012 et versions ultérieures. Si cela semble être un meilleur choix lorsque vous ajoutez votre cluster à Windows Admin Center, sélectionnez [ **Cluster de basculement** ](manage-failover-clusters.md) au lieu de **Hyper-Converged Cluster**.

### <a name="prepare-your-windows-server-2016-cluster-for-windows-admin-center"></a>Préparer votre cluster Windows Server 2016 pour Windows Admin Center

Windows Admin Center pour l’Infrastructure de Hyper-Converged dépend de la gestion des Qu'api ajoutées après le lancement de Windows Server 2016. Avant de pouvoir gérer votre cluster Windows Server 2016 avec Windows Admin Center, vous devez effectuer ces deux étapes :

1. Vérifiez que chaque serveur dans le cluster a installé le [2018-05 mise à jour Cumulative pour Windows Server 2016 (KB4103723)](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723) ou version ultérieure. Pour télécharger et installer cette mise à jour, accédez à **paramètres** > **mise à jour & sécurité** > **mettre à jour de Windows** et sélectionnez  **Rechercher en ligne les mises à jour à partir de Microsoft Update**.
2. Exécutez l’applet de commande PowerShell suivante en tant qu’administrateur sur le cluster :

```powershell
    Add-ClusterResourceType -Name "SDDC Management" -dll "$env:SystemRoot\Cluster\sddcres.dll" -DisplayName "SDDC Management"
```

> [!Tip]
> Vous devez uniquement exécuter une seule fois, de l’applet de commande sur n’importe quel serveur dans le cluster. Vous pouvez exécuter localement dans Windows PowerShell ou utilisez Credential Security Service Provider (CredSSP) pour l’exécuter à distance. Selon votre configuration, vous ne serez peut-être pas en mesure d’exécuter cette applet de commande dans Windows Admin Center.

> [!Important]
> Pour les déploiements dans les paramètres régionaux non anglais, il existe un problème connu dans la version 1804 de Windows Admin Center qui empêche le tableau de bord de chargement (première fois uniquement). La solution de contournement consiste à exécuter `Add-ClusterResource -Name 'SDDC Management' -Group 'Cluster Group' -ResourceType 'SDDC Management'` en remplaçant *« Groupe de clusters »* avec le nom localisé, par exemple, *« Cluster du groupe »* en Français. Ce problème sera résolu dans la prochaine mise à jour.
>
> **MISE À JOUR :** Ce problème est maintenant résolu dans Windows Admin Center préversion 1806.

### <a name="prepare-your-windows-server-2019-cluster-for-windows-admin-center"></a>Préparer votre cluster Windows Server 2019 pour Windows Admin Center

Si votre cluster exécute une version d’évaluation de Insider de Windows Server 2019, les étapes ci-dessus ne sont pas nécessaires. Ajoutez simplement le cluster vers Windows Admin Center comme décrit dans la section suivante, et vous êtes fin prêt ! [Télécharger la dernière version de la version préliminaire de Windows Server 2019](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver).

### <a name="configure-software-defined-networking-optional"></a>Configurer Software-Defined Networking (facultatif) ###

Vous pouvez configurer votre Infrastructure Hyper-Converged exécutant Windows Server 2016 ou 2019 à utiliser la mise en réseau SDN (Software Defined) en procédant comme suit :

1. Préparer le disque dur virtuel du système d’exploitation qui est le même système d’exploitation que vous avez installé sur les ordinateurs hôtes d’infrastructure Hyper-convergée. Ce disque dur virtuel sera utilisé pour toutes les machines virtuelles de contrôleur de réseau/SLB/la passerelle.
2. Télécharger tous les fichiers sous SDN Express à partir d’et dossier [ https://github.com/Microsoft/SDN/tree/master/SDNExpress ](https://github.com/Microsoft/SDN/tree/master/SDNExpress).
3. Préparer une autre machine virtuelle à l’aide de la console de déploiement. Cette machine virtuelle doit être en mesure d’accéder aux hôtes SDN. En outre, la machine virtuelle doit être avoir l’outil RSAT Hyper-V installé.
4. Copier tout ce dont vous avez téléchargé pour SDN Express à la console de déploiement machine virtuelle. Et partager ce **SDNExpress** dossier. Vérifiez que chaque hôte peut accéder à la **SDNExpress** dossier partagé, tel que défini dans la ligne de fichier de configuration 8 :
```
    \\$env:Computername\SDNExpress
```
5. Copiez le disque dur virtuel du système d’exploitation pour le **images** dossier sous le **SDNExpress** dossier sur la machine virtuelle de la console de déploiement.
6. Modifier la configuration de SDN Express avec votre configuration de l’environnement. Terminer les deux étapes suivantes après avoir modifié la configuration de SDN Express en fonction des informations de votre environnement.
7. Exécutez la commande PowerShell avec des privilèges d’administrateur pour déployer SDN :

```powershell
    .\SDNExpress.ps1 -ConfigurationDataFile .\your_fabricconfig.PSD1 -verbose 
```

Le déploiement prendra environ 30 à 45 minutes.

## <a name="get-started"></a>Prise en main

Une fois votre Infrastructure Hyper-Converged est déployé, vous pouvez gérer à l’aide de Windows Admin Center.

### <a name="install-windows-admin-center"></a>Installer Windows Admin Center

Si vous n’avez pas déjà fait, téléchargez et installez Windows Admin Center. Le plus rapide de façon à être opérationnel et en cours d’exécution consiste à installer sur votre ordinateur Windows 10 et gérer vos serveurs à distance. Cette opération prend moins de cinq minutes. [Téléchargez maintenant](https://aka.ms/windowsadmincenter) ou [en savoir plus sur les autres options d’installation](../deploy/install.md).

### <a name="add-hyper-converged-cluster"></a>Ajouter un Cluster Hyperconvergé

Pour ajouter votre cluster vers Windows Admin Center :

1. Cliquez sur **+ ajouter** sous toutes les connexions.
2. Choisissez d’ajouter un **connexion au Cluster Hyper-Converged**.
3. Tapez le nom du cluster et, si vous y êtes invité, les informations d’identification à utiliser.
4. Cliquez sur **ajouter** se termine.

Le cluster sera ajouté à votre liste de connexions. Cliquez dessus pour lancer le tableau de bord.

![Ajouter une connexion de cluster hyperconvergé](../media/manage-hyper-converged/add-hyper-converged-cluster-connection.gif)

### <a name="add-sdn-enabled-hyper-converged-cluster-windows-admin-center-preview"></a>Ajouter prenant en charge de SDN un Cluster Hyperconvergé (version préliminaire de Windows Admin Center)

La dernière version préliminaire Windows Admin Center prend en charge la gestion Sdn pour Hyper-Converged Infrastructure. En ajoutant un URI REST du contrôleur de réseau à votre connexion au cluster Hyperconvergé, vous pouvez utiliser le Gestionnaire du Cluster hyperconvergé à gérer vos ressources SDN et surveiller l’infrastructure SDN.

1. Cliquez sur **+ ajouter** sous toutes les connexions.
2. Choisissez d’ajouter un **connexion au Cluster Hyper-Converged**.
3. Tapez le nom du cluster et, si vous y êtes invité, les informations d’identification à utiliser.
4. Vérifiez **configurer le contrôleur de réseau** pour continuer.
5. Entrez le **URI du contrôleur de réseau** et cliquez sur **Validate**.
6. Cliquez sur **ajouter** se termine.

Le cluster sera ajouté à votre liste de connexions. Cliquez dessus pour lancer le tableau de bord.

![Ajouter une connexion de prenant en charge de SDN un cluster hyperconvergé](../media/manage-hyper-converged/add-snd-enabled-hci-connection.png)

> [!Important]
> Environnements SDN avec l’authentification Kerberos pour la communication Northbound ne sont actuellement pas pris en charge.

## <a name="frequently-asked-questions"></a>Forum Aux Questions

### <a name="are-there-differences-between-managing-windows-server-2016-and-windows-server-2019-insider-preview"></a>Existe-t-il des différences entre la gestion de Windows Server 2016 et Windows Server 2019 Insider Preview ?

Oui. Windows Admin Center pour Hyper-Converged Infrastructure reçoit des mises à jour fréquentes qui améliorent l’expérience de Windows Server 2016 et Windows Server 2019 Insider Preview. Toutefois, certaines nouvelles fonctionnalités sont uniquement disponibles pour Insider Preview – par exemple, le bouton bascule pour la déduplication et compression.

### <a name="can-i-use-windows-admin-center-to-manage-storage-spaces-direct-for-other-use-cases-not-hyper-converged-such-as-converged-scale-out-file-server-sofs-or-microsoft-sql-server"></a>Puis-je utiliser Windows Admin Center pour gérer les espaces de stockage Direct pour les autres cas (ne pas hyperconvergé), tels que convergé serveur de fichiers avec montée en puissance (parallèle SoFS) ou Microsoft SQL Server ?

Windows Admin Center pour l’Infrastructure de Hyper-Converged ne fournit pas de gestion ou des options de surveillance en particulier pour les autres cas d’utilisation des espaces de stockage Direct : par exemple, il ne peut pas créer des partages de fichiers. Toutefois, les fonctionnalités de tableau de bord et core, telles que la création de volumes ou de remplacement de disques, fonctionnent pour n’importe quel cluster d’espaces de stockage Direct.

### <a name="whats-the-difference-between-a-failover-cluster-and-a-hyper-converged-cluster"></a>Quelle est la différence entre un Cluster de basculement et un Cluster Hyper-Converged ?

En règle générale, le terme « hyperconvergé » fait référence à l’exécution d’Hyper-V et espaces de stockage Direct sur les mêmes serveurs en cluster pour virtualiser les ressources de calcul et de stockage. Dans le contexte de Windows Admin Center, lorsque vous cliquez sur **+ ajouter** à partir de la liste des connexions, vous pouvez choisir entre l’ajout un **connexion au Cluster de basculement** ou un **Hyper-Converged Cluster connexion**:

- Le **connexion au Cluster de basculement** est le successeur de l’application de bureau du Gestionnaire du Cluster de basculement. Il fournit une expérience de gestion familiers, à usage général pour n’importe quel cluster prenant en charge des charges de travail, y compris Microsoft SQL Server. Il est disponible pour Windows Server 2012 et versions ultérieures.

- Le **connexion Hyper-Converged Cluster** est une toute nouvelle expérience adaptée aux espaces de stockage Direct et Hyper-V. Il comprend le Tableau de bord et met en évidence les graphiques et les alertes de surveillance. Il est disponible pour Windows Server 2016 et preview builds de Windows Server 2019.

### <a name="why-do-i-need-the-latest-cumulative-update-for-windows-server-2016"></a>Pourquoi dois-je la dernière mise à jour cumulative pour Windows Server 2016 ?

Windows Admin Center pour l’Infrastructure de Hyper-Converged dépend de la gestion des Qu'api développés depuis la publication de Windows Server 2016. Ces API sont ajoutées dans le [2018-05 mise à jour Cumulative pour Windows Server 2016 (KB4103723)](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723), disponible à compter du 8 mai 2018.

### <a name="how-much-does-it-cost-to-use-windows-admin-center"></a>Combien coûte l'utilisation de Windows Admin Center ?

Windows Admin Center ne donne lieu à aucun frais supplémentaire par rapport à Windows.

Vous pouvez utiliser Windows Admin Center (disponible en téléchargement séparé) avec des licences valides de Windows Server ou Windows 10 sans coût supplémentaire, il est concédé sous licence selon un CLUF supplémentaire de Windows.

### <a name="does-windows-admin-center-require-system-center"></a>Windows Admin Center nécessite-t-il System Center ?

Non.

### <a name="does-it-require-an-internet-connection"></a>Nécessite-t-il une connexion Internet ?

Non.

Bien que Windows Admin Center offre de puissantes et intégration pratique avec le cloud Microsoft Azure, gestion principales et l’analyse de Hyper-Converged Infrastructure est complètement en local. Il peut être installé et utilisé sans connexion Internet.

## <a name="things-to-try"></a>Solutions possibles

Si vous n’êtes pas familiarisé, voici quelques didacticiels rapides pour vous aider à découvrir comment Windows Admin Center pour l’Infrastructure de Hyper-Converged est organisé et fonctionne. Veuillez exercer un bon jugement et soyez prudent avec les environnements de production. Ces vidéos ont été enregistrées avec la version de Windows Admin Center 1804 et une version d’évaluation de Insider de Windows Server 2019.

### <a name="manage-storage-spaces-direct-volumes"></a>Gérer les volumes d’espaces de stockage Direct

<ul>
               <li>(0:37) <a href="https://youtu.be/o66etKq70N8">comment créer un volume en miroir triple</a></li>
               <li>(1:17) <a href="https://youtu.be/R72QHudqWpE">comment créer un volume de parité avec accélération miroir</a></li>
               <li>(1:02) <a href="https://youtu.be/j59z7ulohs4">comment ouvrir un volume et ajouter des fichiers</a></li>
               <li>(0:51) <a href="https://youtu.be/PRibTacyKko">comment activer la déduplication et compression</a></li>
               <li>(0:47) <a href="https://youtu.be/hqyBzipBoTI">comment développer un volume</a></li>
               <li>(0:26) <a href="https://youtu.be/DbjF8r2F6Jo">comment supprimer un volume</a></li>
</ul>

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Créer un volume, en miroir triple</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/o66etKq70N8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Créer un volume, parité accélérée de miroir</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/R72QHudqWpE" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Ouvrez le volume et ajouter des fichiers</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/j59z7ulohs4" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Activer la déduplication et compression</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/PRibTacyKko" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Développez le volume</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/hqyBzipBoTI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>suppression du volume</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/DbjF8r2F6Jo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
</table>

### <a name="create-a-new-virtual-machine"></a>Créer une machine virtuelle

1. Cliquez sur le **Machines virtuelles** outil depuis le volet de navigation de gauche.
2. En haut de l’outil de Machines virtuelles, choisissez le **inventaire** onglet, puis cliquez sur **New** pour créer une machine virtuelle.
3. Entrez le nom de la machine virtuelle et le choix entre les ordinateurs virtuels de génération 1 et 2.
4. Affiche, vous pouvez ensuite choisir quel hôte pour créer la machine virtuelle sur ou utiliser l’hôte recommandé initialement.
5. Choisissez un chemin d’accès pour les fichiers de machine virtuelle. Choisissez un volume à partir de la liste déroulante ou cliquez sur **Parcourir** pour choisir un dossier à l’aide du sélecteur de dossier. Les fichiers de configuration de machine virtuelle et le fichier de disque dur virtuel seront enregistrés dans un seul dossier sous le `\Hyper-V\[virtual machine name]` chemin du volume sélectionné ou du chemin d’accès.
6. Choisissez le nombre de processeurs virtuels, si vous souhaitez que la virtualisation imbriquée est activée, configurez les paramètres de mémoire, les cartes réseau, les disques durs virtuels et choisissez si vous souhaitez installer un système d’exploitation à partir d’un fichier image .iso ou à partir du réseau.
7. Cliquez sur **Créer** pour créer l’ordinateur virtuel.
8. Une fois que la machine virtuelle est créée et apparaît dans la liste des machines virtuelles, vous pouvez démarrer l’ordinateur virtuel.
9. Une fois la machine virtuelle est démarrée, vous pouvez vous connecter à la console de la machine virtuelle par le biais de VMConnect pour installer le système d’exploitation. Sélectionnez la machine virtuelle à partir de la liste, cliquez sur **plus** > **Connect** pour télécharger le fichier .rdp. Dans l’application Connexion Bureau à distance, ouvrez le fichier .rdp. Dans la mesure où il se connecte à la console de la machine virtuelle, vous devez entrer les informations d’identification administrateur de l’hôte Hyper-V.

[En savoir plus sur la gestion d’ordinateurs virtuels avec Windows Admin Center](manage-virtual-machines.md).

### <a name="pause-and-safely-restart-a-server"></a>Suspendre et redémarrer en toute sécurité un serveur

1. À partir de la **tableau de bord**, sélectionnez **serveurs** à partir de la barre de navigation sur le côté gauche ou en cliquant sur le **afficher les serveurs >** lien sur la vignette dans le coin inférieur droit du tableau de bord .
2. En haut, passer de **Résumé** à la **inventaire** onglet.
3. Sélectionnez un serveur en cliquant sur son nom pour ouvrir la **Server** page de détails.
4. Cliquez sur **suspension d’un serveur pour la maintenance**. Si elle est fiable continuer, cela déplacera les machines virtuelles vers d’autres serveurs du cluster. Le serveur aura l’état drainage lors de cette opération. Si vous le souhaitez, vous pouvez regarder les ordinateurs virtuels déplacer le **machines virtuelles > inventaire** page, où leur serveur hôte est indiqué clairement dans la grille. Lorsque toutes les machines virtuelles ont été déplacés, l’état du serveur sera **suspendu**.
5. Cliquez sur **gérer serveur** pour accéder à tous les outils de gestion par serveur dans Windows Admin Center.
6. Cliquez sur **redémarrer**, puis **Oui**. Vous allez être renvoyé à la liste des connexions.
7. Sur le **tableau de bord**, le serveur est affiché en rouge quand il est en panne.
8. Une fois que c’est nouveau, accédez à nouveau le **Server** page et cliquez sur **server de reprise à partir de la maintenance** pour définir l’état du serveur au haut simplement. Dans le temps, les machines virtuelles va rétablir – aucune action n’est requise.

### <a name="replace-a-failed-drive"></a>Remplacer un lecteur ayant échoué

1. Quand un lecteur échoue, une alerte s’affiche dans le coin supérieur gauche **alertes** zone de la **tableau de bord**.
2. Vous pouvez également sélectionner **lecteurs** à partir de la barre de navigation sur le côté gauche ou cliquez sur le **vue lecteurs >** lien sur la vignette dans le coin inférieur droit pour parcourir les lecteurs et afficher leur état par vous-même. Dans le **inventaire** onglet, la grille prend en charge le tri, regroupement et recherche par mot clé.
3. À partir de la **tableau de bord**, cliquez sur l’alerte pour afficher les détails, comme l’emplacement physique du lecteur.
4. Pour plus d’informations, cliquez sur le **accédez à lecteur** raccourci vers le **lecteur** page de détails.
5. Si votre matériel prend en charge, vous pouvez cliquer sur **activer clair/désactiver** pour contrôler le voyant du lecteur.
6. Espaces de stockage Direct automatiquement retire et evacuates lecteurs ayant échoués. Lorsque cela s’est produit, l’état du lecteur est mis hors service et sa barre de capacité de stockage est vide.
7. Retirer le disque défectueux et insérer son remplacement.
8. Dans **lecteurs > inventaire**, le nouveau lecteur s’affiche. Dans le temps, permet d’effacer l’alerte, permet de réparer les volumes à l’état OK et rééquilibre stockage sur le nouveau disque – aucune action n’est requise.

### <a name="manage-virtual-networks-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Gérer des réseaux virtuels (clusters de HCL prenant en charge de SDN à l’aide de la version préliminaire de Windows Admin Center)

1. Sélectionnez **réseaux virtuels** à partir de la barre de navigation sur le côté gauche.
2. Cliquez sur **New** pour créer un nouveau réseau virtuel et des sous-réseaux, ou choisissez un réseau virtuel existant, puis cliquez sur **paramètres** pour modifier sa configuration.
3. Cliquez sur un réseau virtuel existant pour afficher les connexions de machine virtuelle pour les sous-réseaux du réseau virtuel et accéder aux listes de contrôle appliqués aux sous-réseaux de réseau virtuel.

![Gérer des réseaux virtuels](../media/manage-hyper-converged/manage-virtual-networks.png)

### <a name="connect-a-virtual-machine-to-a-virtual-network-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Connecter un ordinateur virtuel à un réseau virtuel (clusters de HCL prenant en charge de SDN à l’aide de la version préliminaire de Windows Admin Center)

1. Sélectionnez **Machines virtuelles** à partir de la barre de navigation sur le côté gauche.
2. Choisissez une machine virtuelle existante > cliquez sur **paramètres** > Ouvrir le **réseaux** onglet **paramètres**.
3. Configurer le **réseau virtuel** et **sous-réseau virtuel** champs à connecter la machine virtuelle à un réseau virtuel.

Vous pouvez également configurer le réseau virtuel lors de la création d’une machine virtuelle.

![Connecter un ordinateur virtuel à un réseau virtuel](../media/manage-hyper-converged/connect-vm-to-virtual-network.png)

### <a name="monitor-software-defined-networking-infrastructure-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Surveiller l’infrastructure Sdn (clusters de HCL prenant en charge de SDN à l’aide de la version préliminaire de Windows Admin Center)

1. Sélectionnez **SDN surveillance** à partir de la barre de navigation sur le côté gauche.
2. Afficher des informations détaillées sur l’intégrité de la passerelle virtuelle du contrôleur de réseau, équilibrage de charge et surveiller votre utilisation de Pool de passerelle virtuels, Public et Pool d’adresses IP privées et l’état de l’hôte SDN.

![Surveiller l’infrastructure SDN](../media/manage-hyper-converged/sdn-monitoring.png)

## <a name="feedback"></a>Commentaires

C’est une affaire de vos commentaires ! L’avantage le plus important de mises à jour fréquentes consiste à écouter ce qui fonctionne et ce qui doit être amélioré. Voici quelques méthodes pour nous dire ce que vous pensez :

- [Envoyer et votez pour les demandes de fonctionnalité sur UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [Rejoignez le forum Windows Admin Center sur Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- Pour un Tweet `@servermgmt`

### <a name="see-also"></a>Voir aussi

- [Windows Admin Center](../understand/windows-admin-center.md)
- [Espaces de stockage Direct](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
- [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)
- [Mise en réseau logicielle](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)
