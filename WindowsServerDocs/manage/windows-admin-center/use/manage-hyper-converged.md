---
title: Gérer l’infrastructure hyper-convergée avec le centre d’administration Windows
description: Gérer l’infrastructure hyper-convergée avec le centre d’administration Windows (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 03/01/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 6795464bfbadd12fc220e941ad2175eb83d0f050
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322861"
---
# <a name="manage-hyper-converged-infrastructure-with-windows-admin-center"></a>Gérer l’infrastructure hyper-convergée avec le centre d’administration Windows

>S’applique à : Windows Admin Center, Windows Admin Center Preview

## <a name="what-is-hyper-converged-infrastructure"></a>Qu’est-ce que l’infrastructure hyper-convergée ?

L’infrastructure hyper-convergée consolide le calcul, le stockage et la mise en réseau définis par logiciel dans un seul cluster pour offrir une virtualisation hautement performante, rentable et facile à mettre à l’échelle. Cette fonctionnalité a été introduite dans Windows Server 2016 avec [espaces de stockage direct](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview), la [mise en réseau définie par logiciel](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking) et [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server).

> [!Tip]
> Vous cherchez à acquérir une infrastructure hyper-convergée ? Microsoft recommande ces solutions [définies par le logiciel Windows Server](https://microsoft.com/wssd) auprès de nos partenaires. Elles sont conçues, assemblées et validées par rapport à notre architecture de référence pour garantir la compatibilité et la fiabilité, ce qui vous permet d’être rapidement opérationnel.

> [!IMPORTANT]
> Certaines des fonctionnalités décrites dans cet article sont disponibles uniquement dans la version préliminaire du centre d’administration Windows. [Comment faire récupérer cette version ?](https://aka.ms/windowsadmincenter)

## <a name="what-is-windows-admin-center"></a>Qu’est-ce que Windows Admin Center ?

Le [Centre d’administration Windows](../overview.md) est l’outil de gestion de nouvelle génération pour Windows Server, le successeur des outils « intégrés » traditionnels comme gestionnaire de serveur. Il est gratuit et peut être installé et utilisé sans connexion Internet. Vous pouvez utiliser le centre d’administration Windows pour gérer et surveiller l’infrastructure hyper-convergée exécutant Windows Server 2016 ou Windows Server 2019.

![Tableau de bord du cluster hyper-convergé](../media/manage-hyper-converged/hci-dashboard-v1809.png)

## <a name="key-features"></a>Principales fonctionnalités

Voici quelques-unes des principales nouveautés du centre d’administration Windows pour l’infrastructure convergée :

- **Un volet unique unifié pour le calcul, le stockage et la mise en réseau rapide.** Affichez vos machines virtuelles, vos serveurs hôtes, vos volumes, vos lecteurs et plus encore dans une expérience unique, cohérente et intégrée.
- **Créer et gérer des espaces de stockage et des machines virtuelles Hyper-V.** Flux de travail radicalement simples pour créer, ouvrir, redimensionner et supprimer des volumes ; créer, démarrer, se connecter et déplacer des machines virtuelles ; et bien plus encore.
- **Analyse performante au niveau du cluster.** Le tableau de bord présente l’utilisation de la mémoire et de l’UC, de la capacité de stockage, des e/s par seconde, du débit et de la latence en temps réel sur chaque serveur du cluster, avec des alertes claires quand un événement n’est pas correct.
- **Prise en charge de SDN (Software Defined Networking).** Gérez et surveillez les réseaux virtuels, les sous-réseaux, connectez les machines virtuelles aux réseaux virtuels et surveillez l’infrastructure SDN.

Le centre d’administration Windows pour l’infrastructure convergée est activement développé par Microsoft. Elle reçoit des mises à jour fréquentes qui améliorent les fonctionnalités existantes et ajoutent de nouvelles fonctionnalités.

## <a name="before-you-start"></a>Avant de commencer

Pour gérer votre cluster en tant qu’infrastructure hyper-convergée dans le centre d’administration Windows, il doit exécuter Windows Server 2016 ou Windows Server 2019, avec Hyper-V et espaces de stockage direct activés. Éventuellement, la mise en réseau définie par le logiciel peut également être activée et gérée par le biais du centre d’administration Windows.

> [!Tip]
> Le centre d’administration Windows offre également une expérience de gestion à usage général pour tous les clusters prenant en charge n’importe quelle charge de travail, disponible pour Windows Server 2012 et versions ultérieures. Si cela semble mieux adapté, lorsque vous ajoutez votre cluster au centre d’administration Windows, sélectionnez [**cluster de basculement**](manage-failover-clusters.md) au lieu de **cluster hyper-convergé**.

### <a name="prepare-your-windows-server-2016-cluster-for-windows-admin-center"></a>Préparer votre cluster Windows Server 2016 pour le centre d’administration Windows

Le centre d’administration Windows pour l’infrastructure convergée hyper-convergé dépend des API de gestion ajoutées après la sortie de Windows Server 2016. Avant de pouvoir gérer votre cluster Windows Server 2016 avec le centre d’administration Windows, vous devez effectuer les deux étapes suivantes :

1. Vérifiez que chaque serveur du cluster a installé la [mise à jour Cumulative 2018-05 pour Windows server 2016 (KB4103723)](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723) ou version ultérieure. Pour télécharger et installer cette mise à jour, accédez à **paramètres** > **mettre à jour & sécurité** > **Windows Update** et sélectionnez **Rechercher les mises à jour en ligne à partir de Microsoft Update**.
2. Exécutez l’applet de commande PowerShell suivante en tant qu’administrateur sur le cluster :

```powershell
    Add-ClusterResourceType -Name "SDDC Management" -dll "$env:SystemRoot\Cluster\sddcres.dll" -DisplayName "SDDC Management"
```

> [!Tip]
> Il vous suffit d’exécuter l’applet de commande une seule fois sur n’importe quel serveur du cluster. Vous pouvez l’exécuter localement dans Windows PowerShell ou utiliser le fournisseur de services de sécurité des informations d’identification (CredSSP) pour l’exécuter à distance. Selon votre configuration, vous ne pourrez peut-être pas exécuter cette applet de commande à partir du centre d’administration Windows.

### <a name="prepare-your-windows-server-2019-cluster-for-windows-admin-center"></a>Préparer votre cluster Windows Server 2019 pour le centre d’administration Windows

Si votre cluster exécute Windows Server 2019, les étapes ci-dessus ne sont pas nécessaires. Il vous suffit d’ajouter le cluster au centre d’administration Windows, comme décrit dans la section suivante.

### <a name="configure-software-defined-networking-optional"></a>Configurer le réseau à définition logicielle (facultatif) ###

Vous pouvez configurer votre infrastructure hyper-convergée exécutant Windows Server 2016 ou 2019 pour utiliser SDN (Software Defined Networking) en procédant comme suit :

1. Préparez le disque dur virtuel du système d’exploitation qui est le même que celui que vous avez installé sur les hôtes de l’infrastructure hyper-convergée. Ce disque dur virtuel sera utilisé pour toutes les machines virtuelles NC/SLB/GW.
2. Téléchargez tous les dossiers et fichiers sous SDN Express à partir [https://github.com/Microsoft/SDN/tree/master/SDNExpress](https://github.com/Microsoft/SDN/tree/master/SDNExpress).
3. Préparez une autre machine virtuelle à l’aide de la console de déploiement. Cette machine virtuelle doit être en mesure d’accéder aux hôtes SDN. En outre, l’outil Hyper-V de la machine virtuelle doit être installé.
4. Copiez tous les éléments que vous avez téléchargés pour SDN Express sur la machine virtuelle de la console de déploiement. Et partagez ce dossier **SDNExpress** . Assurez-vous que chaque hôte peut accéder au dossier partagé **SDNExpress** , comme défini dans le fichier de configuration ligne 8 :
   ```
    \\$env:Computername\SDNExpress
   ```
5. Copiez le disque dur virtuel du système d’exploitation dans le dossier **images** sous le dossier **SDNExpress** sur la machine virtuelle de la console de déploiement.
6. Modifiez la configuration SDN Express avec la configuration de votre environnement. Terminez les deux étapes suivantes après avoir modifié la configuration SDN Express en fonction des informations de votre environnement.
7. Exécutez PowerShell avec des privilèges d’administrateur pour déployer SDN :

```powershell
    .\SDNExpress.ps1 -ConfigurationDataFile .\your_fabricconfig.PSD1 -verbose 
```

Le déploiement prendra environ 30 à 45 minutes.

## <a name="get-started"></a>Prise en main

Une fois votre infrastructure hyper-convergée déployée, vous pouvez la gérer à l’aide du centre d’administration Windows.

### <a name="install-windows-admin-center"></a>Installer Windows Admin Center

Si vous ne l’avez pas encore fait, téléchargez et installez le centre d’administration Windows. Le moyen le plus rapide d’être opérationnel consiste à l’installer sur votre ordinateur Windows 10 et à gérer vos serveurs à distance. Cela prend moins de cinq minutes. [Téléchargez](https://aka.ms/windowsadmincenter) -le maintenant ou [en savoir plus sur les autres options d’installation](../deploy/install.md).

### <a name="add-hyper-converged-cluster"></a>Ajouter un cluster hyper-convergé

Pour ajouter votre cluster au centre d’administration Windows :

1. Cliquez sur **+ Ajouter** sous toutes les connexions.
2. Choisissez d’ajouter une **connexion de cluster hyper-convergé**.
3. Tapez le nom du cluster et, si vous y êtes invité, les informations d’identification à utiliser.
4. Cliquez sur **Ajouter** pour terminer.

Le cluster est ajouté à votre liste de connexions. Cliquez dessus pour lancer le tableau de bord.

![Ajouter une connexion de cluster hyper-convergé](../media/manage-hyper-converged/add-hyper-converged-cluster-connection.gif)

### <a name="add-sdn-enabled-hyper-converged-cluster-windows-admin-center-preview"></a>Ajouter un cluster hyper-convergé compatible SDN (version préliminaire du centre d’administration Windows)

La dernière version préliminaire du centre d’administration Windows prend en charge la gestion de mise en réseau définie par logiciel pour l’infrastructure hyper-convergée. En ajoutant un URI REST de contrôleur de réseau à votre connexion de cluster hyper-convergé, vous pouvez utiliser le gestionnaire de cluster hyper-convergé pour gérer vos ressources SDN et surveiller l’infrastructure SDN.

1. Cliquez sur **+ Ajouter** sous toutes les connexions.
2. Choisissez d’ajouter une **connexion de cluster hyper-convergé**.
3. Tapez le nom du cluster et, si vous y êtes invité, les informations d’identification à utiliser.
4. Cochez **la case configurer le contrôleur de réseau** pour continuer.
5. Entrez l' **URI du contrôleur de réseau** et cliquez sur **valider**.
6. Cliquez sur **Ajouter** pour terminer.

Le cluster est ajouté à votre liste de connexions. Cliquez dessus pour lancer le tableau de bord.

![Ajouter une connexion de cluster hyper-convergé compatible avec SDN](../media/manage-hyper-converged/add-snd-enabled-hci-connection.png)

> [!Important]
> Les environnements SDN avec l’authentification Kerberos pour la communication Northbound ne sont actuellement pas pris en charge.

## <a name="frequently-asked-questions"></a>Forum aux questions

### <a name="are-there-differences-between-managing-windows-server-2016-and-windows-server-2019"></a>Existe-t-il des différences entre la gestion de Windows Server 2016 et celle de Windows Server 2019 ?

Oui. Le centre d’administration Windows pour l’infrastructure convergée hyper-converge reçoit des mises à jour fréquentes qui améliorent l’expérience pour Windows Server 2016 et Windows Server 2019. Toutefois, certaines nouvelles fonctionnalités sont uniquement disponibles pour Windows Server 2019, par exemple, le commutateur bascule pour la déduplication et la compression.

### <a name="can-i-use-windows-admin-center-to-manage-storage-spaces-direct-for-other-use-cases-not-hyper-converged-such-as-converged-scale-out-file-server-sofs-or-microsoft-sql-server"></a>Puis-je utiliser le centre d’administration Windows pour gérer les espaces de stockage direct pour d’autres cas d’usage (pas hyper-convergé), par exemple, convergé Serveur de fichiers avec montée en puissance parallèle (SoFS) ou Microsoft SQL Server ?

Le centre d’administration Windows pour l’infrastructure hyper-convergée ne fournit pas d’options de gestion ou de surveillance spécifiquement pour d’autres cas d’utilisation de espaces de stockage direct, par exemple, il ne peut pas créer de partages de fichiers. Toutefois, le tableau de bord et les fonctionnalités principales, telles que la création de volumes ou le remplacement de lecteurs, fonctionnent pour tout cluster espaces de stockage direct.

### <a name="whats-the-difference-between-a-failover-cluster-and-a-hyper-converged-cluster"></a>Quelle est la différence entre un cluster de basculement et un cluster hyper-convergé ?

En général, le terme « hyper-convergé » fait référence à l’exécution d’Hyper-V et à espaces de stockage direct sur les mêmes serveurs en cluster pour virtualiser les ressources de calcul et de stockage. Dans le contexte du centre d’administration Windows, lorsque vous cliquez sur **+ Ajouter** dans la liste connexions, vous pouvez choisir entre ajouter une **connexion de cluster de basculement** ou une **connexion de cluster hyper-convergé**:

- La **connexion de cluster de basculement** est le successeur de l’application de bureau gestionnaire du cluster de basculement. Il offre une expérience de gestion familière et à usage général pour tous les clusters prenant en charge n’importe quelle charge de travail, y compris Microsoft SQL Server. Elle est disponible pour Windows Server 2012 et versions ultérieures.

- La **connexion au cluster hyper-convergé** est une expérience entièrement nouvelle et adaptée à espaces de stockage direct et Hyper-V. Il comprend le Tableau de bord et met en évidence les graphiques et les alertes de supervision. Elle est disponible pour Windows Server 2016 et Windows Server 2019.

### <a name="why-do-i-need-the-latest-cumulative-update-for-windows-server-2016"></a>Pourquoi ai-je besoin de la dernière mise à jour cumulative pour Windows Server 2016 ?

Le centre d’administration Windows pour l’infrastructure convergée hyper-convergé dépend des API de gestion développées depuis la sortie de Windows Server 2016. Ces API sont ajoutées à la [mise à jour Cumulative 2018-05 pour Windows Server 2016 (KB4103723)](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723), disponible à partir du 8 mai 2018.

### <a name="how-much-does-it-cost-to-use-windows-admin-center"></a>Combien coûte l’utilisation de Windows Admin Center ?

Windows Admin Center n’engendre aucuns frais supplémentaires autres que Windows.

Vous pouvez utiliser Windows Admin Center (disponible en téléchargement distinct) avec des licences valides de Windows Server ou Windows 10 sans coût supplémentaire : il est fourni sous licence dans un avenant au contrat CLUF de Windows.

### <a name="does-windows-admin-center-require-system-center"></a>Windows Admin Center nécessite-t-il System Center ?

Non.

### <a name="does-it-require-an-internet-connection"></a>Nécessite-t-il une connexion Internet ?

Non.

Bien que le centre d’administration Windows offre une intégration puissante et pratique au Cloud Microsoft Azure, l’expérience de gestion et de surveillance principale de l’infrastructure convergée est entièrement locale. Il peut être installé et utilisé sans connexion Internet.

## <a name="things-to-try"></a>Éléments à essayer

Si vous êtes débutant, voici quelques didacticiels rapides pour vous aider à découvrir comment le centre d’administration Windows pour l’infrastructure convergée est organisé et fonctionne. N’hésitez pas à nous faire preuve d’un bon jugement et soyez vigilant avec les environnements de production. Ces vidéos ont été enregistrées avec le centre d’administration Windows version 1804 et une version préliminaire Insider de Windows Server 2019.

### <a name="manage-storage-spaces-direct-volumes"></a>Gérer les volumes de espaces de stockage direct

<ul>
               <li>(0:37) <a href="https://youtu.be/o66etKq70N8">comment créer un volume miroir triple</a></li>
               <li>(1:17) <a href="https://youtu.be/R72QHudqWpE">comment créer un volume de parité à accélération miroir</a></li>
               <li>(1:02) <a href="https://youtu.be/j59z7ulohs4">Comment ouvrir un volume et ajouter des fichiers</a></li>
               <li>(0:51) <a href="https://youtu.be/PRibTacyKko">Comment activer la déduplication et la compression</a></li>
               <li>(0:47) <a href="https://youtu.be/hqyBzipBoTI">comment développer un volume</a></li>
               <li>(0:26) <a href="https://youtu.be/DbjF8r2F6Jo">Comment supprimer un volume</a></li>
</ul>

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Créer un volume, miroir triple</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/o66etKq70N8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Créer un volume, parité à accélération miroir</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/R72QHudqWpE" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Ouvrir le volume et ajouter des fichiers</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/j59z7ulohs4" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Activer la déduplication et la compression</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/PRibTacyKko" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Développer</strong> le
             du volume<iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/hqyBzipBoTI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Supprimer le volume</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/DbjF8r2F6Jo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
</table>

### <a name="create-a-new-virtual-machine"></a>Créer un nouvel ordinateur virtuel

1. Cliquez sur l’outil **machines virtuelles** dans le volet de navigation de gauche.
2. En haut de l’outil machines virtuelles, choisissez l’onglet **inventaire** , puis cliquez sur **nouveau** pour créer un nouvel ordinateur virtuel.
3. Entrez le nom de l’ordinateur virtuel et choisissez entre les ordinateurs virtuels de génération 1 et 2.
4. Conviviale peut ensuite choisir l’hôte sur lequel la machine virtuelle doit être créée initialement ou utiliser l’hôte recommandé.
5. Choisissez un chemin d’accès pour les fichiers de l’ordinateur virtuel. Choisissez un volume dans la liste déroulante ou cliquez sur **Parcourir** pour choisir un dossier à l’aide du sélecteur de dossiers. Les fichiers de configuration d’ordinateur virtuel et le fichier de disque dur virtuel sont enregistrés dans un dossier unique sous le chemin d’accès `\Hyper-V\[virtual machine name]` du volume ou du chemin d’accès sélectionné.
6. Choisissez le nombre de processeurs virtuels, si vous souhaitez activer la virtualisation imbriquée, configurez les paramètres de mémoire, les cartes réseau, les disques durs virtuels et indiquez si vous souhaitez installer un système d’exploitation à partir d’un fichier image. ISO ou du réseau.
7. Cliquez sur **Créer** pour créer l’ordinateur virtuel.
8. Une fois que l’ordinateur virtuel est créé et apparaît dans la liste des machines virtuelles, vous pouvez démarrer l’ordinateur virtuel.
9. Une fois la machine virtuelle démarrée, vous pouvez vous connecter à la console de la machine virtuelle via VMConnect pour installer le système d’exploitation. Sélectionnez l’ordinateur virtuel dans la liste, cliquez sur **plus** > **se connecter** pour télécharger le fichier. RDP. Ouvrez le fichier. RDP dans l’application Connexion Bureau à distance. Dans la mesure où il se connecte à la console de la machine virtuelle, vous devrez entrer les informations d’identification d’administrateur de l’hôte Hyper-V.

[En savoir plus sur la gestion des machines virtuelles avec le centre d’administration Windows](manage-virtual-machines.md).

### <a name="pause-and-safely-restart-a-server"></a>Suspendre et redémarrer en toute sécurité un serveur

1. Dans le **tableau de bord**, sélectionnez **serveurs** dans la barre de navigation sur le côté gauche, ou cliquez sur le lien **afficher les serveurs >** sur la vignette dans le coin inférieur droit du tableau de bord.
2. En haut, passez de **Résumé** à l’onglet **inventaire** .
3. Sélectionnez un serveur en cliquant sur son nom pour ouvrir la page Détails du **serveur** .
4. Cliquez sur **suspendre le serveur pour la maintenance**. S’il est possible de continuer, les machines virtuelles seront déplacées vers d’autres serveurs du cluster. Le serveur aura un État drainage alors que cela se produit. Si vous le souhaitez, vous pouvez regarder le déplacement des machines virtuelles sur la page **machines virtuelles > inventaire** , où leur serveur hôte apparaît clairement dans la grille. Lorsque tous les ordinateurs virtuels ont été déplacés, l’état du serveur est **suspendu**.
5. Cliquez sur **gérer le serveur** pour accéder à tous les outils de gestion par serveur dans le centre d’administration Windows.
6. Cliquez sur **redémarrer**, puis sur **Oui**. Vous allez revenir à la liste des connexions.
7. De retour sur le **tableau de bord**, le serveur est coloré en rouge alors qu’il est éteint.
8. Une fois la sauvegarde effectuée, accédez de nouveau à la page **serveur** , puis cliquez sur **reprendre le serveur de la maintenance** pour rétablir l’état du serveur à tout simplement. Dans le temps, les machines virtuelles sont reculées : aucune action de l’utilisateur n’est requise.

### <a name="replace-a-failed-drive"></a>Remplacer un lecteur défaillant

1. En cas de défaillance d’un lecteur, une alerte s’affiche dans la zone **alertes** en haut à gauche du **tableau de bord**.
2. Vous pouvez également sélectionner des **lecteurs** à partir de la navigation sur le côté gauche ou cliquer sur le lien **afficher les lecteurs >** sur la vignette dans le coin inférieur droit pour parcourir les lecteurs et afficher leur état pour vous-même. Dans l’onglet **inventaire** , la grille prend en charge le tri, le regroupement et la recherche par mot clé.
3. Dans le **tableau de bord**, cliquez sur l’alerte pour afficher les détails, comme l’emplacement physique du lecteur.
4. Pour plus d’informations, cliquez sur le raccourci accéder à un **lecteur** sur la page de détails du **lecteur** .
5. Si votre matériel le prend en charge, vous pouvez cliquer sur **activer/** désactiver pour contrôler la lumière des voyants du lecteur.
6. Espaces de stockage direct répartir et évacuer automatiquement les lecteurs défaillants. Lorsque cela s’est produit, l’état du lecteur est retiré et sa barre de capacité de stockage est vide.
7. Retirez le lecteur défaillant et insérez son remplacement.
8. Dans **lecteurs > inventaire**, le nouveau lecteur s’affiche. Dans le temps, l’alerte est désactivée, l’état des volumes est rétabli sur OK et le stockage rééquilibre sur le nouveau lecteur. aucune action n’est requise de la part de l’utilisateur.

### <a name="manage-virtual-networks-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Gérer les réseaux virtuels (clusters HCI compatibles SDN à l’aide de la version préliminaire du centre d’administration Windows)

1. Sélectionnez **réseaux virtuels** dans la barre de navigation sur le côté gauche.
2. Cliquez sur **nouveau** pour créer un réseau virtuel et des sous-réseaux, ou choisissez un réseau virtuel existant, puis cliquez sur **paramètres** pour modifier sa configuration.
3. Cliquez sur un réseau virtuel existant pour afficher les connexions de machines virtuelles aux sous-réseaux du réseau virtuel et les listes de contrôle d’accès appliquées aux sous-réseaux du réseau virtuel.

![Gérer les réseaux virtuels](../media/manage-hyper-converged/manage-virtual-networks.png)

### <a name="connect-a-virtual-machine-to-a-virtual-network-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Connecter un ordinateur virtuel à un réseau virtuel (clusters HCI compatibles SDN à l’aide de la version préliminaire du centre d’administration Windows)

1. Sélectionnez **machines virtuelles** dans la barre de navigation sur le côté gauche.
2. Choisissez une machine virtuelle existante > cliquez sur **paramètres** > ouvrir l’onglet **réseaux** dans **paramètres**.
3. Configurez les champs **réseau virtuel** et **sous-réseau virtuel** pour connecter la machine virtuelle à un réseau virtuel.

Vous pouvez également configurer le réseau virtuel lors de la création d’une machine virtuelle.

![Connecter un ordinateur virtuel à un réseau virtuel](../media/manage-hyper-converged/connect-vm-to-virtual-network.png)

### <a name="monitor-software-defined-networking-infrastructure-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Surveiller l’infrastructure de mise en réseau définie par logiciel (clusters HCI compatibles SDN à l’aide de la version préliminaire du centre d’administration Windows)

1. Sélectionnez **surveillance de SDN** dans la barre de navigation sur le côté gauche.
2. Affichez des informations détaillées sur l’intégrité du contrôleur de réseau, des Load Balancer logicielles, de la passerelle virtuelle et surveillez votre pool de passerelles virtuelles, l’utilisation du pool d’adresses IP publiques et privées et l’état de l’hôte SDN.

![Surveiller l’infrastructure SDN](../media/manage-hyper-converged/sdn-monitoring.png)

## <a name="feedback"></a>Commentaires

Il s’agit de vos commentaires ! L’avantage le plus important des mises à jour fréquentes est d’apprendre ce qui fonctionne et ce qui doit être amélioré. Voici quelques façons de nous dire ce que vous pensez :

- [Envoyer et voter pour les demandes de fonctionnalités sur UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [Rejoignez le Forum du centre d’administration Windows sur la communauté Microsoft Tech](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- Tweet à `@servermgmt`

### <a name="see-also"></a>Voir aussi

- [Windows Admin Center](../overview.md)
- [Espaces de stockage direct](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
- [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)
- [Mise en réseau SDN (Software Defined Networking)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)
