---
title: Gérer l’Infrastructure Hyperconvergée avec Windows Admin Center
description: Gérer l’Infrastructure Hyperconvergée avec Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 03/01/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 9ce4381735746ace6358aa2cb30f8b341c576054
ms.sourcegitcommit: e558dda2774345e9ad17ff04b759f68c59d88139
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2019
ms.locfileid: "9262842"
---
# Gérer l’Infrastructure Hyperconvergée avec Windows Admin Center

>S’applique à: Windows Admin Center, Windows Admin Center Preview

## Qu’est une Infrastructure hyperconvergée

Infrastructure Hyperconvergée consolide calcul défini par logiciel, le stockage et la mise en réseau dans un cluster de fournir de hautes performances, économique et la virtualisation facilement adaptable. Cette fonctionnalité a été introduite dans Windows Server 2016 avec les [Espaces de stockage Direct](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview), [Software Defined Networking](https://docs.microsoft.com/en-us/windows-server/networking/sdn/software-defined-networking) et [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server).

> [!Tip]
> Vous cherchez à acquérir une Infrastructure hyperconvergée? Microsoft recommande de ces solutions [Windows Server Software-Defined](https://microsoft.com/wssd) de nos partenaires. Ils sont conçues, assemblées et validées par rapport à notre architecture de référence pour assurer la compatibilité et la fiabilité, afin de vous préparer et rapidement opérationnels.

> [!IMPORTANT]
> Certaines des fonctionnalités décrites dans cet article sont uniquement disponibles dans Windows Admin Center Preview. [Comment obtenir cette version?](http://aka.ms/windowsadmincenter)

## Qu'est-ce que Windows Admin Center?

[Windows Admin Center](../understand/windows-admin-center.md) est l’outil de gestion de nouvelle génération pour Windows Server, le successeur des outils traditionnels «intégrés» tels que le Gestionnaire de serveur. C’est gratuit et peut être installé et utilisé sans connexion Internet. Vous pouvez utiliser Windows Admin Center pour gérer et contrôler une Infrastructure hyperconvergée exécutant Windows Server 2016 ou Windows Server 2019.

![Tableau de bord du cluster hyperconvergé](../media/manage-hyper-converged/hci-dashboard-v1809.png)

## Fonctionnalités essentielles

Points essentiels sur Windows Admin Center pour une Infrastructure hyperconvergée sont les suivantes:

- **Unifié unique volet-de-en verre pour le calcul, stockage et dès la mise en réseau.** Afficher vos machines virtuelles, serveurs hôtes, volumes, lecteurs et bien plus encore au sein d’une expérience dédiée, cohérente et interconnectée.
- **Créer et gérer des ordinateurs virtuels Hyper-V et les espaces de stockage.** Flux de travail radicalement simple pour créer, ouvrir, redimensionner et supprimer des volumes; créer, Démarrer, se connecter à et déplacer des machines virtuelles; et bien plus encore.
- **Surveillance de cluster à l’échelle du puissant.** Le tableau de bord graphiques de mémoire et d’utilisation du processeur, la capacité de stockage, / s par seconde, le débit et latence en temps réel, sur chaque serveur du cluster, avec l’effacement des alertes lorsque quelque chose n’est pas approprié.
- **Prise en charge de logiciels définies mise en réseau SDN ().** Gérer et contrôler les sous-réseaux des réseaux virtuels, connecter des machines virtuelles aux réseaux virtuels et surveiller l’infrastructure SDN.

Windows Admin Center pour une Infrastructure hyperconvergée est activement développé par Microsoft. Elle reçoit des mises à jour fréquentes qui améliorent les fonctionnalités existantes et ajouter de nouvelles fonctionnalités.

## Avant de commencer

Pour gérer votre cluster en tant qu’une Infrastructure hyperconvergée dans Windows Admin Center, il doit exécuter Windows Server 2016 ou Windows Server 2019 et ont Hyper-V et les espaces de stockage Direct activés. Si vous le souhaitez, ils peuvent également disposer de Software Defined Networking activé et gérées par le biais de Windows Admin Center.

> [!Tip]
> Windows Admin Center offre également une gestion à usage général d’expérience pour n’importe quel cluster prenant en charge les charges de travail, disponible pour Windows Server 2012 et versions ultérieures. Si cela ressemble à une meilleure solution, lorsque vous ajoutez votre cluster à Windows Admin Center, sélectionnez le [**Cluster de basculement**](manage-failover-clusters.md) au lieu de **Cluster hyperconvergé**.

### Préparer votre cluster Windows Server 2016 pour Windows Admin Center

Windows Admin Center pour une Infrastructure hyperconvergée dépend de la gestion des Qu'api ajoutées après la publication de Windows Server 2016. Avant de pouvoir gérer votre cluster Windows Server 2016 avec Windows Admin Center, vous devez effectuer ces deux étapes:

1. Vérifiez que chaque serveur du cluster a installé l' [2018-05 mise à jour Cumulative pour Windows Server 2016 (KB4103723)](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723) ou une version ultérieure. Pour télécharger et installer cette mise à jour, accédez à **paramètres** > **mise à jour de sécurité de &** > **Mise à jour Windows** et sélectionnez **vérification en ligne pour les mises à jour à partir de Microsoft Update**.
2. Exécutez l’applet de commande PowerShell suivante en tant qu’administrateur sur le cluster:

```powershell
    Add-ClusterResourceType -Name "SDDC Management" -dll "$env:SystemRoot\Cluster\sddcres.dll" -DisplayName "SDDC Management"
```

> [!Tip]
> Vous devez uniquement exécuter l’applet de commande qu’une seule fois, sur n’importe quel serveur du cluster. Vous pouvez exécuter localement dans Windows PowerShell ou utiliser le fournisseur de services de sécurité d’informations d’identification (CredSSP) pour l’exécuter à distance. En fonction de votre configuration, vous ne serez peut-être pas en mesure d’exécuter cette applet de commande à partir de dans Windows Admin Center.

### Préparer votre cluster Windows Server 2019 pour Windows Admin Center

Si votre cluster s’exécute Windows Server 2019, les étapes ci-dessus ne sont pas nécessaires. Il suffit d’ajouter le cluster au centre d’administration Windows comme décrit dans la section suivante et vous pouvez commencer!

### Configurer la définition logicielle mise en réseau (facultatif) ###

Vous pouvez configurer votre Infrastructure hyperconvergée exécutant Windows Server 2016 ou 2019 à utiliser des logiciels définis mise en réseau SDN () avec les étapes suivantes:

1. Préparer le disque dur virtuel du système d’exploitation qui est le même système d’exploitation que vous avez installé sur les hôtes d’infrastructure hyperconvergée. Ce disque dur virtuel sera utilisé pour tous les ordinateurs virtuels CN/SLB/EG.
2. Télécharger le dossier et les fichiers sous SDN Express à partir [https://github.com/Microsoft/SDN/tree/master/SDNExpress](https://github.com/Microsoft/SDN/tree/master/SDNExpress).
3. Préparer une VM différente à l’aide de la console de déploiement. Cet ordinateur virtuel doit être en mesure d’accéder aux hôtes SDN. En outre, l’ordinateur virtuel doit être avoir l’outil RSAT Hyper-V est installé.
4. Copiez tout ce dont vous avez téléchargé pour SDN Express sur la console deployment machine virtuelle. Et partager ce dossier **SDNExpress** . Assurez-vous que tous les hôtes peuvent accéder au dossier partagé **SDNExpress** , tel que défini dans la ligne de fichier de configuration 8:
```
    \\$env:Computername\SDNExpress
```
5. Copiez le disque dur virtuel du système d’exploitation sur le dossier **images** sous le dossier **SDNExpress** sur la console deployment machine virtuelle.
6. Modifier la configuration Express SDN avec votre configuration de l’environnement. Terminer les deux étapes suivantes une fois que vous modifiez la configuration Express SDN basée sur les informations de votre environnement.
7. Exécutez PowerShell avec des privilèges d’administrateur pour déployer SDN:

```powershell
    .\SDNExpress.ps1 -ConfigurationDataFile .\your_fabricconfig.PSD1 -verbose 
```

Le déploiement prendra environ 30 à 45 minutes.

## Prise en main

Une fois que votre Infrastructure hyperconvergée est déployé, vous pouvez gérer à l’aide de Windows Admin Center.

### Installer Windows Admin Center

Si vous n’avez pas déjà fait, téléchargez et installez Windows Admin Center. Le plus rapide pratique pour préparer et en cours d’exécution consiste à installer sur votre ordinateur Windows 10 et gérer vos serveurs à distance. Cela prend moins de cinq minutes. [Télécharger maintenant](https://aka.ms/windowsadmincenter) ou [en savoir plus sur les autres options d’installation](../deploy/install.md).

### Ajouter un Cluster Hyperconvergé

Pour ajouter votre cluster à Windows Admin Center:

1. Cliquez sur **+ Ajouter** sous toutes les connexions.
2. Choisir d’ajouter une **Connexion de Cluster hyperconvergé**.
3. Tapez le nom du cluster et, si vous y êtes invité, les informations d’identification à utiliser.
4. Cliquez sur **Ajouter** à la fin.

Le cluster sera ajouté à votre liste des connexions. Cliquez dessus pour lancer le tableau de bord.

![Ajouter une connexion de cluster hyperconvergé](../media/manage-hyper-converged/add-hyper-converged-cluster-connection.gif)

### Ajoutez prenant SDN de Cluster Hyperconvergé (version d’évaluation Windows Admin Center)

La plus récente de Windows Admin Center Preview prend en charge la gestion Software Defined Networking pour une Infrastructure hyperconvergée. En ajoutant un URI dans le reste du contrôleur de réseau à votre connexion de cluster Hyperconvergé, vous pouvez utiliser le Gestionnaire de Cluster hyperconvergé pour gérer vos ressources SDN et surveiller l’infrastructure SDN.

1. Cliquez sur **+ Ajouter** sous toutes les connexions.
2. Choisir d’ajouter une **Connexion de Cluster hyperconvergé**.
3. Tapez le nom du cluster et, si vous y êtes invité, les informations d’identification à utiliser.
4. Consultez **configurer le contrôleur de réseau** pour continuer.
5. Entrez l' **URI du contrôleur de réseau** et cliquez sur **Valider**.
6. Cliquez sur **Ajouter** à la fin.

Le cluster sera ajouté à votre liste des connexions. Cliquez dessus pour lancer le tableau de bord.

![Ajouter une connexion compatible SDN de cluster hyperconvergé](../media/manage-hyper-converged/add-snd-enabled-hci-connection.png)

> [!Important]
> Environnements SDN avec l’authentification Kerberos pour la communication Northbound ne sont actuellement pas pris en charge.

## Forum Aux Questions

### Existe-t-il des différences entre la gestion de Windows Server 2016 et Windows Server 2019?

Oui. Windows Admin Center pour une Infrastructure hyperconvergée reçoit des mises à jour fréquentes qui améliorent l’expérience pour Windows Server 2016 et Windows Server 2019. Toutefois, certaines nouvelles fonctionnalités sont uniquement disponibles pour Windows Server 2019: par exemple, le bouton bascule pour la déduplication et la compression.

### Puis-je utiliser Windows Admin Center pour gérer les espaces de stockage Direct pour les autres cas d’utilisation (ne pas hyperconvergé), par exemple, convergé serveur de fichiers avec montée en (en puissance) ou Microsoft SQL Server?

Windows Admin Center pour une Infrastructure hyperconvergée ne fournit pas de gestion ou les options d’analyse spécifiquement pour les autres cas d’utilisation des espaces de stockage Direct: par exemple, il ne peut pas créer des partages de fichiers. Toutefois, les fonctionnalités du tableau de bord et standard, tels que la création de volumes ou le remplacement des disques, fonctionnent pour n’importe quel cluster d’espaces de stockage Direct.

### Quelle est la différence entre un Cluster de basculement et un Cluster hyperconvergé?

En règle générale, le terme «hyperconvergé» fait référence à exécutant Hyper-V et les espaces de stockage Direct sur les mêmes serveurs en cluster pour virtualiser les ressources de calcul et de stockage. Dans le contexte de Windows Admin Center, lorsque vous cliquez sur **+ Ajouter** à partir de la liste des connexions, vous pouvez choisir entre l’ajout d’une **connexion de Cluster de basculement** ou une **connexion de Cluster hyperconvergé**:

- La **connexion de Cluster de basculement** est le successeur de l’application de bureau Gestionnaire du Cluster de basculement. Il fournit une expérience de gestion familiers, à usage général pour n’importe quel cluster prenant en charge les charges de travail, y compris Microsoft SQL Server. Il est disponible pour Windows Server 2012 et versions ultérieures.

- La **connexion de Cluster hyperconvergé** est une toute nouvelle expérience personnalisée pour les espaces de stockage Direct et Hyper-V. Il comprend le Tableau de bord et met en évidence les graphiques et les alertes de surveillance. Il est disponible pour Windows Server 2016 et Windows Server 2019.

### Pourquoi dois-je la dernière mise à jour cumulative pour Windows Server 2016?

Windows Admin Center pour une Infrastructure hyperconvergée dépend de la gestion des Qu'api développées dans la mesure où Windows Server 2016 a été publiée. Ces API est ajoutés dans la version [2018-05 mise à jour Cumulative pour Windows Server 2016 (KB4103723)](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723), disponible à partir de 8 mai 2018.

### Combien coûte l'utilisation de Windows Admin Center?

Windows Admin Center ne donne lieu à aucun frais supplémentaire par rapport à Windows.

Vous pouvez utiliser Windows Admin Center (disponible sous forme de téléchargement distinct) avec des licences valides Windows Server ou Windows 10 sans coût supplémentaire: il est fourni sous licence par un accord CLUF Windows supplémentaire.

### Windows Admin Center nécessite-t-il System Center?

Non.

### Si elle nécessite une connexion Internet?

Non.

Bien que Windows Admin Center offre de puissantes et intégration pratique avec le cloud de Microsoft Azure, la gestion de base et expérience de surveillance pour une Infrastructure hyperconvergée est entièrement sur site. Il peut être installé et utilisé sans connexion Internet.

## Éléments à essayer

Si vous êtes simplement prise en main, voici quelques didacticiels rapides pour vous aider à découvrir comment Windows Admin Center pour une Infrastructure hyperconvergée est organisé et fonctionne. Veuillez exercer une bonne appréciation et soyez prudent avec les environnements de production. Ces vidéos ont été enregistrées avec Windows Admin Center version 1804 et un build Insider Preview de Windows Server 2019.

### Gérer les volumes d’espaces de stockage Direct

<ul>
               <li>(0:37) <a href="https://youtu.be/o66etKq70N8">la création d’un volume en miroir triple</a></li>
               <li>(1:17) <a href="https://youtu.be/R72QHudqWpE">la création d’un volume de parité accélérée grâce à mise en miroir</a></li>
               <li>(1:02) <a href="https://youtu.be/j59z7ulohs4">comment ouvrir un volume et ajouter des fichiers</a></li>
               <li>(0:51) <a href="https://youtu.be/PRibTacyKko">comment activer la déduplication et compression</a></li>
               <li>(0:47) <a href="https://youtu.be/hqyBzipBoTI">comment étendre un volume</a></li>
               <li>(0:26) <a href="https://youtu.be/DbjF8r2F6Jo">comment supprimer un volume</a></li>
</ul>

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Créer le volume, la mise en miroir triple</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/o66etKq70N8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Créer le volume, la parité accélérée grâce à mise en miroir</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/R72QHudqWpE" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Ouvrir le volume et ajouter des fichiers</strong>
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
            <strong>Supprimer le volume</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/DbjF8r2F6Jo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
</table>

### Créer un ordinateur virtuel

1. Cliquez sur l’outil de **Machines virtuelles** à partir du volet de navigation gauche.
2. En haut de l’outil de Machines virtuelles, choisissez l’onglet de **l’inventaire** , puis cliquez sur **Nouveau** pour créer une machine virtuelle.
3. Entrez le nom de la machine virtuelle et choisir entre les machines virtuelles de génération 1 et 2.
4. Uou de choisir quel hôte à créer la machine virtuelle sur ou utilisez l’hôte recommandée initialement.
5. Choisissez un chemin d’accès pour les fichiers de la machine virtuelle. Choisissez un volume à partir de la liste déroulante ou cliquez sur **Parcourir** pour choisir un dossier à l’aide du sélecteur de dossiers. Les fichiers de configuration de la machine virtuelle et le fichier de disque dur virtuel seront enregistrées dans un dossier unique sous la `\Hyper-V\[virtual machine name]` chemin d’accès du volume sélectionné ou du chemin d’accès.
6. Choisissez le nombre de processeurs virtuels, si vous souhaitez que la virtualisation imbriquée activée, configurez les paramètres de la mémoire, les cartes réseau, les disques durs virtuels et choisissez si vous souhaitez installer un système d’exploitation à partir d’un fichier image .iso ou à partir du réseau.
7. Cliquez sur **créer** pour créer la machine virtuelle.
8. Une fois que la machine virtuelle est créée et s’affiche dans la liste des machines virtuelles, vous pouvez démarrer la machine virtuelle.
9. Une fois que la machine virtuelle est démarrée, vous pouvez vous connecter à la console de la machine virtuelle via VMConnect pour installer le système d’exploitation. Sélectionnez la machine virtuelle dans la liste, cliquez sur **plusieurs** > **Connect** pour télécharger le fichier .rdp. Ouvrez le fichier .rdp dans l’application Connexion Bureau à distance. Dans la mesure où il se connecte à la console de la machine virtuelle, vous devez entrer les informations d’identification d’administrateur de l’hôte Hyper-V.

[En savoir plus sur la gestion des machines virtuelles avec Windows Admin Center](manage-virtual-machines.md).

### Mettre en pause et redémarrer en toute sécurité un serveur

1. À partir du **tableau de bord**, sélectionnez les **serveurs** à partir de la navigation sur le côté gauche ou en cliquant sur le lien **Afficher les serveurs >** sur la vignette dans le coin inférieur droit du tableau de bord.
2. Dans la partie supérieure, basculer **Résumé** vers l’onglet de **l’inventaire** .
3. Sélectionnez un serveur en cliquant sur son nom pour ouvrir la page de détails du **serveur** .
4. Cliquez sur le **serveur de mise en Pause pour la maintenance**. S’il est sûr de continuer, cela se déplacent des machines virtuelles vers d’autres serveurs du cluster. Le serveur devra état drainage tandis que cela se produit. Si vous le souhaitez, vous pouvez regarder les ordinateurs virtuels se déplacent sur la page de **machines virtuelles > inventaire** , quand leur serveur hôte s’affiche clairement dans la grille. Lorsque toutes les machines virtuelles ont été déplacés, l’état du serveur sera **suspendu**.
5. Cliquez sur le **serveur de gestion** pour accéder à tous les outils de gestion par serveur dans Windows Admin Center.
6. Cliquez sur **redémarrer**, puis **Oui**. Vous allez être renvoyé à la liste des connexions.
7. Retour sur le **tableau de bord**, le serveur est en rouge alors qu’elle est vers le bas.
8. Une fois la sauvegarde, accédez à nouveau la page du **serveur** , puis cliquez sur **serveur de reprise à partir de maintenance** pour définir l’état du serveur à simplement haut. Dans le temps, machines virtuelles se déplacent précédent – aucune action utilisateur n’est nécessaire.

### Remplacer un disque défectueux

1. Lorsqu’un lecteur échoue, une alerte s’affiche dans l’angle supérieur gauche **alertes** du tableau de **bord**.
2. Vous pouvez également sélectionner les **lecteurs** à partir de la navigation sur le côté gauche ou cliquez sur le lien **d’Affichage lecteurs >** sur la vignette dans le coin inférieur droit pour rechercher des lecteurs et voir leur état par vous-même. Dans l’onglet de **l’inventaire** , la grille prend en charge le tri, le regroupement et recherche sur le mot-clé.
3. À partir du **tableau de bord**, cliquez sur l’alerte pour afficher les détails, comme l’emplacement physique du lecteur.
4. Pour plus d’informations, cliquez sur le raccourci **Atteindre le lecteur** à la page de détails du **lecteur** .
5. Si votre matériel prend en charge, vous pouvez cliquer sur **Activer/désactiver la lumière** pour contrôler le voyant du lecteur.
6. Espaces de stockage Direct automatiquement retraite et evacuates des disques défaillants. Lorsque cela s’est produit, l’état du disque est mis hors service et sa barre de capacité de stockage est vide.
7. Retirer le lecteur ayant échoué, l’insérer son remplacement.
8. Dans la zone **lecteurs > inventaire**, le nouveau lecteur s’affiche. Dans le temps, l’alerte effacera, permet de réparer les volumes de revenir à l’état OK et stockage sera rééquilibrer sur le nouveau disque – aucune action utilisateur n’est nécessaire.

### Gérer les réseaux virtuels (HCI prenant SDN clusters à l’aide de Windows Admin Center Preview)

1. Sélectionnez les **Réseaux virtuels** dans le volet de navigation sur le côté gauche.
2. Cliquez sur **Nouveau** pour créer un nouveau réseau virtuel et sous-réseaux ou choisir un réseau virtuel existant et cliquez sur les **paramètres** pour modifier sa configuration.
3. Cliquez sur un réseau virtuel existant pour afficher des connexions VM vers les sous-réseaux du réseau virtuel et accéder aux listes de contrôle appliqués à des sous-réseaux de réseau virtuel.

![Gérer les réseaux virtuels](../media/manage-hyper-converged/manage-virtual-networks.png)

### Connecter une machine virtuelle à un réseau virtuel (HCI prenant SDN clusters à l’aide de Windows Admin Center Preview)

1. Sélectionner les **Machines virtuelles** à partir de la navigation sur le côté gauche.
2. Choisir une > de machine virtuelle existante Click **paramètres** > ouvrez l’onglet **réseaux** dans les **paramètres**.
3. Configurer les champs de **Réseau virtuel** et de **Sous-réseau virtuel** pour vous connecter la machine virtuelle à un réseau virtuel.

Vous pouvez également configurer le réseau virtuel lors de la création d’une machine virtuelle.

![Connecter une machine virtuelle à un réseau virtuel](../media/manage-hyper-converged/connect-vm-to-virtual-network.png)

### Surveiller l’infrastructure Software Defined Networking (HCI prenant SDN clusters à l’aide de Windows Admin Center Preview)

1. Sélectionnez la **Surveillance SDN** dans le volet de navigation sur le côté gauche.
2. Afficher des informations détaillées sur l’intégrité de passerelle virtuelle du contrôleur de réseau, équilibrage de charge logicielle et surveiller votre utilisation virtuel Pool de passerelle, Public et privé IP Pool et l’état de l’hôte SDN.

![Surveiller l’infrastructure SDN](../media/manage-hyper-converged/sdn-monitoring.png)

## Commentaires

Il est entièrement sur vos commentaires! L’avantage le plus important de mises à jour fréquentes consiste à écouter ce qui fonctionne et ce qui doit être amélioré. Voici quelques méthodes pour nous informer de ce que vous pensez:

- [Soumettre et votez pour des demandes de fonctionnalités sur UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [Rejoindre le forum de Windows Admin Center sur la Communauté technique Microsoft](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- Tweet à `@servermgmt`

### Articles associés

- [Windows Admin Center](../understand/windows-admin-center.md)
- [Espaces de stockage direct](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
- [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)
- [SDN (Software-Defined Networking)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)
