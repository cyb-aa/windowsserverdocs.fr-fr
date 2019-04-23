---
title: Afficher et configurer les événements de performances et des données de Service
description: Gestionnaire de serveur
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ccd59c35-4dbf-48e7-88a4-c519c00184d1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 727b81d1ba2cda32a7568e4e2f23065b1589e745
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861900"
---
# <a name="view-and-configure-performance-event-and-service-data"></a>Afficher et configurer les données de performances, d’événements et de services

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit comment afficher et configurer les entrées de journal des événements, les compteurs de performances et les alertes de service affichés pour les serveurs locaux et distants dans le Gestionnaire de serveur.  

Données de journal de performances, de service et d’événements s’affiche à deux emplacements dans la console du Gestionnaire de serveur dans Windows Server.  

-   Le tableau de bord, vous pouvez cliquer sur le **événements**, **performances**, et **Services** lignes de miniatures pour configurer les événements, les performances et les données de journal de service que vous souhaitez voir pour les rôles, l’ensemble du pool serveur gestionnaire de serveur, des groupes de serveurs créés par l’utilisateur et le serveur local. Cliquez sur l’hypertexte lignes permet d’ouvrir **affichage Détails** des boîtes de dialogue qui vous permettent de spécifient les données sur lequel vous voulez être alerté dans le tableau de bord. Après avoir configuré les données de journal de performances que vous souhaitez mettre en évidence dans les miniatures du tableau de bord, de services et d’événements, les entrées de journal qui correspondent aux critères que vous avez spécifié sont répertoriées au bas de la **affichage Détails** boîtes de dialogue.  

-   Les vignettes **Événements**, **Services** et **Performances** font partie des pages d’accueil de rôle et de groupe. Les commandes du menu **Tâches** de ces vignettes vous permettent de spécifier les données que vous souhaitez recueillir à partir des serveurs gérés. Les vignettes comportent des filtres et des requêtes qui, si vous le souhaitez, vous permettent de mieux restreindre les entrées de journal affichées dans chaque vignette.  

Cette rubrique contient les sections suivantes.  

-   [Que sont les miniatures ?](#BKMK_thumb)  

-   [Afficher et configurer des événements](#BKMK_events)  

-   [Afficher et configurer les compteurs de performances](#BKMK_perf)  

-   [Gérer les services et configurer des alertes de service](#BKMK_services)  

-   [Afficher et copier des entrées de performances ou d’événements](#BKMK_copy)  

## <a name="BKMK_thumb"></a>Que sont les miniatures ?  
*Miniatures* sont affichés dans le tableau de bord du Gestionnaire de serveur pour chaque rôle (miniature d’un rôle reflète les données recueillies sur tous les serveurs dans le pool de gestionnaire de serveur qui exécutent le rôle), pour chaque groupe de serveurs, pour le **toutes les Serveurs** groupe (tous les serveurs dans le pool de gestionnaire de serveur) et pour le serveur local. Une fois que le Gestionnaire de serveur obtient des données à partir de serveurs gérés, les miniatures sont créées automatiquement pour les rôles qui sont en cours d’exécution sur les serveurs du pool de serveurs.  

Si la console du Gestionnaire de serveur s’exécute sur un ordinateur client dans le cadre des outils d’Administration de serveur distant, il est sans **serveur Local** miniature.  

La miniature dévoile un aperçu rapide de l’état et de la gestion des rôles, des serveurs et des groupes de serveurs. La ligne d’en-tête de miniature change de couleur (et les chiffres mis en surbrillance sont affichés dans la marge de gauche) lorsque événements, compteurs de performance, résultats Best Practices Analyzer, services ou problèmes de gestion d’ordre général répondent aux critères que vous configurez dans le **affichage Détails** boîtes de dialogue ouvertes en cliquant sur les lignes de miniatures. Le tableau qui suit décrit les données dévoilées dans les miniatures.  

|Ligne de miniatures|Description|  
|---------|--------|  
|Facilité de gestion|La facilité de gestion d’un serveur inclut plusieurs mesures : si le serveur est en ligne ou hors connexion, s’il s’agit des données accessibles et création de rapports pour le Gestionnaire de serveur, si l’utilisateur qui a ouvert une session l’ordinateur local dispose des droits d’utilisateur appropriées pour accéder à ou gérer le serveur distant, si le serveur distant est en cours d’exécution tous les logiciels requis pour le gérer à distance, ou si le serveur est configuré de manière qui lui permet d’être interrogé et géré à l’aide du Gestionnaire de serveur. Les seules données de gérabilité que le Gestionnaire de serveur peut collecter à partir d’un serveur qui exécute Windows Server 2003 sont si le serveur est en ligne ou hors connexion. Pour obtenir des informations détaillées sur les erreurs d’état de la facilité de gestion et leur résolution, consultez le [Guide de dépannage du Gestionnaire de serveur](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx).|  
|Événements|Vous pouvez configurer la ligne **Événements** d’une miniature pour afficher les alertes lorsque des événements sont consignés, qui correspondent aux niveaux de gravité, sources, périodes de temps, serveurs ou ID d’événement que vous spécifiez. Afficher les détails sur les événements et modifiez les alertes que vous souhaitez voir en cliquant sur le **événements** de ligne et en ouvrant le **événement explique en détail la vue** boîte de dialogue pour le groupe de serveurs ou de rôles.|  
|Services|Vous pouvez configurer le **Services** ligne à afficher des alertes lorsque les services sont trouvent dans un groupe de serveurs ou de rôles qui correspondent aux types de démarrage, l’état du service, les noms de service et serveurs que vous spécifiez dans le **affichage détaillent de Services**  boîte de dialogue.<br /><br />Une fois un serveur a été ajouté au pool de serveurs de gestionnaire de serveur, des alertes de service sur le service Détection matériel noyau peuvent figurer si aucun utilisateur connecté au serveur géré. Cela se produit dans la mesure où le service Détection matériel noyau ne s’exécute que si des utilisateurs sont connectés au serveur géré ou à une session Bureau à distance sur le serveur géré. Pour empêcher au service Détection matériel noyau d’informer de ce cas de figure, cliquez sur **Services** dans les miniatures des groupes de serveurs, notamment le groupe **Tous les serveurs**. Dans le **affichage détaillent de Services** boîte de dialogue le **Services** la liste déroulante, désactivez la case à cocher **Détection matériel noyau**, puis cliquez sur **OK**.|  
|Performances|Vous pouvez configurer le **performances** ligne pour afficher les alertes pour un rôle ou groupe de serveurs lorsque des alertes de performances se produisent qui correspondent aux types de ressources, serveurs ou périodes de temps que vous spécifiez dans le **détaillé de performances vue** boîte de dialogue.<br /><br />Par défaut, les compteurs de performances sont désactivés. Les serveurs gérés qui exécutent les systèmes d’exploitation plus récents que Windows Server 2003, pour les performances les compteurs n’ont pas été démarrés, affichent généralement la facilité de gestion des erreurs d’état **en ligne - compteurs de performances pas démarré** dans le **serveurs** vignette des pages de rôle ou groupe. Pour activer les compteurs de performances pour les serveurs gérés, sur le **tous les serveurs** page, cliquez sur les entrées dans le **performances** vignette qui montrent une **état du compteur** valeur  **Désactiver**, puis cliquez sur **lancer les compteurs de performances**. Vous pouvez également démarrer les compteurs de performances en double-cliquant sur les entrées pour les serveurs dans le **serveurs** vignette de rôle ou groupe pages et puis en cliquant sur **lancer les compteurs de performances**.|  
|Résultats BPA|Vous pouvez configurer le **résultats BPA** ligne pour afficher les alertes pour un groupe de serveurs ou de rôles lorsque les résultats d’analyse BPA sont trouvés qui correspondent aux niveaux de gravité, serveurs ou catégories BPA que vous spécifiez dans le **affichagededétailsdesrésultatsBPA** boîte de dialogue.|  

## <a name="BKMK_events"></a>Afficher et configurer des événements  
Dans cette section, découvrez comment configurer les données de journal des événements sont recueillies à partir de serveurs dans le pool de serveurs du Gestionnaire de serveur, et les événements que vous souhaitez mettre en évidence dans les miniatures.  

> [!NOTE]  
> Les événements sur lequel vous êtes alerté dans les miniatures sont un sous-ensemble de l’ensemble des événements que vous demandez au Gestionnaire de serveur pour collecter à partir de serveurs gérés. Bien que la modification des critères d’événement dans le **configurer les données d’événement** boîte de dialogue dans **événements** vignettes peuvent changer le nombre d’alertes visibles dans le tableau de bord du Gestionnaire de serveur, modifier les critères d’alerte événements dans miniatures n’a aucun effet sur les données de journal des événements qui sont collectées à partir des serveurs gérés.  

#### <a name="to-configure-the-events-collected-from-managed-servers"></a>Pour configurer les événements collectés sur les serveurs gérés  

1.  Dans la console Gestionnaire de serveur, ouvrez une page quelconque à l’exception du tableau de bord. Vous pouvez configurer les événements que vous voulez recueillir à partir des serveurs gérés dans la vignette **Événements** dans les pages de rôles, de groupes de serveurs ou de serveurs locaux.  

2.  Dans le menu **Tâches** de la vignette **Événements** , cliquez sur **Configurer les données d’événement**.  

3.  Sélectionnez les niveaux de gravité d’événement que vous souhaitez collecter à partir des serveurs dans le groupe sélectionné. Par défaut, les niveaux de gravité **Critique**, **Erreur**et **Avertissement** sont sélectionnés.  

4.  Précisez une période pendant laquelle les événements doivent survenir. La durée par défaut pour les événements est de 24 heures.  

5.  Sélectionnez les fichiers de journal des événements à partir de laquelle vous souhaitez que les événements à collecter. Les journaux par défaut sont **Applications**, **Installation**et **Système**.  

6.  Pour enregistrer les modifications, cliquez sur **OK** pour fermer la boîte de dialogue **Configurer les données d’événement** . Les données d’événements sont actualisées automatiquement lors de l’enregistrement de vos modifications.  

#### <a name="to-configure-the-events-highlighted-in-thumbnails"></a>Pour configurer les événements mis en surbrillance dans les miniatures  

1.  Si le Gestionnaire de serveur est déjà ouvert, passez à l’étape suivante. S’il n’est pas déjà ouvert, ouvrez-le en effectuant l’une des opérations suivantes.  

    -   Sur le Bureau Windows, démarrez le Gestionnaire de serveur en cliquant sur **Gestionnaire de serveur** dans la barre des tâches Windows.  

    -   Dans l'écran d'**accueil** de Windows, cliquez sur la vignette Gestionnaire de serveur.  

2.  Dans la page du tableau de bord, dans une miniature de la vignette **Rôles et groupes de serveurs**, cliquez sur la ligne **Événements**.  

3.  Dans le **événement explique en détail la vue** boîte de dialogue zone, ajoutez un niveau de gravité aux événements que vous souhaitez afficher. Par défaut, les miniatures affichent uniquement les alertes en surbrillance pour les événements critiques. Notez que le nombre d’événements affiché dans le **affichage Détails** boîte de dialogue augmente lorsque vous ajoutez un niveau de gravité sur lequel vous voulez être alerté.  

4.  Dans le champ **Sources d’événements**, sélectionnez les sources d’événements concernant lesquelles vous voulez être alerté. La valeur par défaut est **All**.  

5.  Si cette miniature pour un rôle qui est installé sur plusieurs serveurs ou un groupe de serveurs, vous pouvez sélectionner les serveurs pour lesquels vous souhaitez les alertes d’événement dans le **serveurs** liste déroulante.  

6.  Dans le **période** champ, spécifiez une période jusqu'à 1 440 minutes, 24 heures ou 1 jour.  

7.  Dans le champ **ID d’événement**, sélectionnez les numéros d’ID d’événements des événements spécifiques concernant lesquels vous voulez être alerté. Vous pouvez taper une plage d’ID d’événements séparés par un tiret (**-**) et exclure des ID d’événements de la plage en tapant le tiret avant l’ID d’événement ou la plage d’ID d’événements que vous voulez exclure. Par exemple, la valeur **1,3,5-99,-76** signifie que des alertes sont déclenchées pour les ID d’événements 1 et 3 et tous les événements dont l’ID est compris entre 5 et 99, à l’exception de l’ID d’événement 76.  

8.  Lorsque vous modifiez les critères d’affichage des alertes, le nombre d’alertes d’événements affichées dans le volet de résultats au bas de la boîte de dialogue peut changer. Sélectionnez des entrées dans la liste et cliquez sur **masquer les alertes** pour les empêcher d’affecter le nombre d’alertes qui s’affiche dans la miniature source. Maintenez la touche **Ctrl** enfoncée à mesure que vous sélectionnez les alertes pour sélectionner plusieurs alertes simultanément. Vous pouvez réaliser cette opération pour les alertes qui sont conformes à vos critères d’alerte d’événement mais que vous n’avez pas besoin de voir.  

9. Cliquez sur **Afficher tout** pour retourner les alertes masquées dans la liste.  

10. Cliquez sur **OK** pour enregistrer vos modifications, fermez le **affichage Détails** boîte de dialogue et afficher les événements modifications d’alertes dans la miniature source.  

## <a name="BKMK_perf"></a>Afficher et configurer les données de journal de performances  
Dans cette section, découvrez comment configurer les données de journaux de performances sont collectées à partir de serveurs dans le pool de serveurs du Gestionnaire de serveur, et les compteurs de performance que vous souhaitez mettre en évidence dans les miniatures.  

Par défaut, les compteurs de performances sont désactivés. Les serveurs gérés qui exécutent les systèmes d’exploitation plus récents que Windows Server 2003, pour les performances les compteurs n’ont pas été démarrés, affichent généralement la facilité de gestion des erreurs d’état **en ligne - compteurs de performances pas démarré** dans le **serveurs** vignette des pages de rôle ou groupe. Pour activer les compteurs de performances pour les serveurs gérés, sur le **tous les serveurs** page, cliquez sur les entrées dans le **performances** vignette qui montrent une **état du compteur** valeur  **Désactiver**, puis cliquez sur **lancer les compteurs de performances**. Vous pouvez également démarrer les compteurs de performances en double-cliquant sur les entrées pour les serveurs dans le **serveurs** vignette de rôle ou groupe pages et puis en cliquant sur **lancer les compteurs de performances**.  

> [!NOTE]  
> Les alertes de performances affichées dans les miniatures sont un sous-ensemble des données de compteur de performances totales que vous demandez au Gestionnaire de serveur pour collecter à partir de serveurs gérés. Bien que la modification des critères de performances d’alerte dans le **configurer des alertes de performances** boîte de dialogue dans **performances** vignettes peuvent changer le nombre d’alertes visibles dans le tableau de bord Gestionnaire de serveur, modification les critères d’alerte de performances dans les miniatures n’a aucun effet sur les données de journal de performances sont collectées à partir des serveurs gérés.  
>   
> Pour cette raison, l’âge maximal des données de performance que vous pouvez afficher dans les miniatures ne peut pas être supérieur à la période d’affichage du graphique maximale configurée dans la boîte de dialogue **Configurer des alertes de performances** . Par exemple, si le **période d’affichage graphique** valeur dans **configurer des alertes de performances** est **1 jour**, la valeur maximale pour le **période**champ dans un **détaillé de performances vue** boîte de dialogue que vous avez ouverte à partir du tableau de bord du Gestionnaire de serveur peut être **1 jour**, **24 heures**, ou **1 440 minutes**.  

#### <a name="to-configure-the-performance-log-data-collected-from-managed-servers"></a>Pour configurer les données du journal des performances collectées sur les serveurs gérés  

1.  Dans la console Gestionnaire de serveur, ouvrez une page quelconque à l’exception du tableau de bord. Vous pouvez configurer les données de performances que vous voulez recueillir à partir des serveurs gérés dans la vignette **Performances** dans les pages de rôles, de groupes de serveurs ou de serveurs locaux.  

2.  Pour recueillir des données de journaux de performances à partir de serveurs gérés, vous devez activer les compteurs de performance. Si les compteurs de performance sont désactivés, cliquez sur une entrée dans le **performances** liste des mosaïques, puis cliquez sur **lancer les compteurs de performances**. La collecte de données avec les compteurs de performance peut nécessiter un certain temps en fonction du nombre de serveurs à partir desquels les données sont recueillies et de la bande passante réseau disponible. Visualisez l’état dans la colonne **État du compteur** .  

3.  Dans le menu **Tâches** de la vignette **Performances** , cliquez sur **Configurer des alertes de performances**.  

4.  pour les serveurs dans le groupe sélectionné, ou qui exécutent le rôle sélectionné, spécifiez le pourcentage d’utilisation du processeur à partir des alertes de compteur de performances collectées par le Gestionnaire de serveur. La valeur par défaut est 85 %.  

5.  Précisez (en mégaoctets) la mémoire disponible restante dont doivent disposer les serveurs avant la collecte des alertes des compteurs de performances. La valeur par défaut est 2 Mo.  

6.  Spécifiez une période affichée par les graphiques pour les ressources **Utilisation du processeur** et **Mémoire disponible** dans la vignette **Performances** de la page sélectionnée. La période par défaut est définie à une journée. Cliquez sur **Enregistrer**.  

    Notez que le nombre d’alertes de performances affiché dans la vignette **Performances** et le mappage des alertes affichées au fil du temps par le graphique peuvent changer après un clic sur le bouton **Enregistrer**.  

    > [!NOTE]  
    > pour les machines virtuelles qui ont [mémoire dynamique](https://technet.microsoft.com/library/ff817651.aspx) est activée, augmenter le seuil d’alertes de performances peut entraîner des faux positifs.  

7.  Pour actualiser la liste d’alertes de performances recueillies à partir de vos serveurs, dans le menu **Tâches** , cliquez sur **Actualiser**.  

#### <a name="to-configure-the-performance-alerts-highlighted-in-thumbnails"></a>Pour configurer les alertes de performance mises en surbrillance dans les miniatures  

1.  Dans la page du tableau de bord, dans une miniature de la vignette **Rôles et groupes de serveurs** , cliquez sur la ligne **Performances** .  

2.  Dans le **détaillé de performances vue** boîte de dialogue, activez ou désactivez des cases à cocher pour les seuils de performances de ressources sur lequel vous voulez être alerté dans le **type de ressource** champ. Notez que le nombre d’alertes de performances affiché dans le **affichage Détails** boîte de dialogue peut augmenter lorsque vous ajoutez un seuil de performance de ressource sur laquelle vous voulez être alerté.  

3.  Si cette miniature pour un rôle qui est installé sur plusieurs serveurs ou un groupe de serveurs, vous pouvez sélectionner les serveurs pour lesquels vous souhaitez les alertes de performances dans le **serveurs** liste déroulante.  

4.  Dans le **période** champ, spécifiez une période jusqu'à 1 440 minutes, 24 heures ou 1 jour.  

5.  Lorsque vous modifiez les critères d’affichage des alertes, le nombre d’alertes affichées dans le volet de résultats au bas de la boîte de dialogue peut changer. Cliquez sur **Masquer les alertes** pour masquer toutes les alertes antérieures à l’heure actuelle et les empêcher d’affecter le nombre d’alertes affiché dans la miniature source.  

6.  Cliquez sur **Afficher tout** pour retourner les alertes masquées dans la liste.  

7.  Cliquez sur **OK** pour enregistrer vos modifications, fermez le **affichage Détails** boîte de dialogue et afficher les performances modifications d’alertes dans la miniature source.  

#### <a name="to-view-the-properties-of-performance-alerts"></a>Pour afficher les propriétés des alertes de performance  

1.  Effectuez l’une des opérations suivantes :  

    -   Dans la page du tableau de bord, dans une miniature de la vignette **Rôles et groupes de serveurs** , cliquez sur la ligne **Performances** .  

    -   Ouvrez une page d’accueil de rôle ou de groupe et recherchez la vignette **Performances** pour le rôle ou le groupe.  

2.  Double-cliquez sur une alerte de performance dans la liste pour afficher ses propriétés. En guise d’alternative, vous pouvez cliquer avec le bouton droit sur une alerte de performances, puis cliquez sur **Afficher les propriétés**.  

3.  Dans la boîte de dialogue **Propriétés d’alerte de performance** , sélectionnez des entrées de journaux pour afficher des informations sur les processus associés aux entrées dans la zone **Processus** .  

4.  Après avoir vérifié les propriétés d’alerte de performance, fermez la boîte de dialogue.  

### <a name="analyze-performance-data-and-solve-problems"></a>Analyser les données de performance et résoudre les problèmes  
Pour plus d’informations sur l’analyse des données de compteur de performance que vous affichez dans le Gestionnaire de serveur et la résolution des problèmes de performances sur les serveurs gérés, consultez les ressources suivantes.  

-   [Analyse des données de performances](https://go.microsoft.com/fwlink/?LinkId=239829)  

-   [Résolution des problèmes de performances](https://go.microsoft.com/fwlink/?LinkId=239831)  

Pour plus d’informations sur les performances avancées outils d’analyse et de surveillance qui sont disponibles pour Windows Server 2012 et versions ultérieures de Windows Server, notamment Server Performance Advisor 3.0, consultez [performances](https://msdn.microsoft.com/windows/hardware/gg463374.aspx) sur MSDN.  

## <a name="BKMK_services"></a>Gérer les services et configurer des alertes de service  
Dans cette section, découvrez comment démarrer, arrêter, redémarrer, suspendre ou reprendre des services qui sont affichent dans le **Services** vignette sur le rôle et les pages de groupe dans le Gestionnaire de serveur. Vous pouvez également configurer les services sur lequel vous êtes alerté dans les miniatures du tableau de bord du Gestionnaire de serveur.  

> [!NOTE]  
> Vous ne pouvez pas modifier le type de démarrage des services, des dépendances de service, des options de récupération ou d’autres propriétés de service dans la vignette de Services dans le Gestionnaire de serveur. Pour modifier des propriétés de services autres que l’état du service, ouvrez le composant logiciel enfichable **Services**. Un raccourci pour ouvrir la **Services** -composant logiciel enfichable est disponible sur le **outils** menu dans le Gestionnaire de serveur.  

#### <a name="to-start-stop-restart-pause-or-resume-a-service"></a>Pour démarrer, arrêter, redémarrer, suspendre ou reprendre un service  

1.  Dans la console Gestionnaire de serveur, ouvrez une page quelconque à l’exception du tableau de bord (en d’autres termes, n’importe quel rôle ou groupe de page d’accueil).  

2.  Dans la vignette **Services** du rôle ou du groupe, cliquez avec le bouton droit sur un service.  

3.  Dans le menu contextuel, cliquez sur l’action à laquelle vous souhaitez soumettre le service. Si vous arrêtez le service, la seule action que vous pouvez réaliser est de le démarrer. De même, si vous le suspendez, la seule action possible est de le reprendre.  

4.  Notez que lorsque vous démarrez, arrêtez, redémarrez, suspendez ou reprenez un service, la valeur de la colonne **État** change pour le service dans la vignette **Services**.  

#### <a name="to-configure-service-alerts-highlighted-in-thumbnails"></a>Pour configurer les alertes de service mises en surbrillance dans les miniatures  

1.  Dans la page du tableau de bord, dans une miniature de la vignette **Rôles et groupes de serveurs**, cliquez sur la ligne **Services**.  

2.  Dans le **affichage détaillent de Services** boîte de dialogue, sélectionnez les types de démarrage pour les services sur laquelle vous voulez être alerté. Par défaut, **automatique (début différé)** et **automatique** sont sélectionnés.  

3.  Sélectionnez les États de service sur lequel vous voulez être alerté. Par défaut, **Tout** est sélectionné.  

4.  Sélectionnez les services sur laquelle vous voulez être alerté. Par défaut, **Tout** est sélectionné.  

5.  Sélectionnez les serveurs associés avec le rôle ou un groupe pour lequel vous souhaitez recevoir des alertes sur les services. Par défaut, **Tout** est sélectionné.  

6.  Lorsque vous modifiez les critères d’affichage des alertes, le nombre d’alertes affichées dans le volet de résultats au bas de la boîte de dialogue peut changer. Cliquez sur **Masquer les alertes** pour masquer toutes les alertes antérieures à l’heure actuelle et les empêcher d’affecter le nombre d’alertes affiché dans la miniature source.  

7.  Cliquez sur **Afficher tout** pour retourner les alertes masquées dans la liste.  

8.  Cliquez sur **OK** pour enregistrer vos modifications, fermez le **affichage Détails** boîte de dialogue et afficher l’alerte de service change dans la miniature source.  

## <a name="BKMK_copy"></a>Afficher et copier des entrées de performances, de service ou d’événements  
Vous pouvez copier les propriétés d’entrées de performances, de service ou d’événements à la fois dans le **affichage Détails** boîtes de dialogue et le **événements** et **performances** mosaïques pour un rôle ou un groupe. Cliquez sur une entrée d’événement ou de performance, puis cliquez sur **copie**.  

La vignette **Événements** vous permet également d’obtenir un aperçu des propriétés d’événements dans la partie inférieure de la vignette en sélectionnant un événement dans la liste. Pour copier les propriétés affichées dans l’aperçu, cliquez avec le bouton droit sur le volet de visualisation, puis cliquez sur **copie**.  

## <a name="see-also"></a>Voir aussi  
[Gestionnaire de serveur](server-manager.md)  
[Filtrer, trier et interroger des données dans les vignettes du Gestionnaire de serveur](filter-sort-and-query-data-in-server-manager-tiles.md)  
