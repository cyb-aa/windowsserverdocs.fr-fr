---
title: Afficher et configurer les données d’événement et de service de performance
description: Gestionnaire de serveur
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 55ff19988cf502c2fdc968f08f207120956217df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383045"
---
# <a name="view-and-configure-performance-event-and-service-data"></a>Afficher et configurer les données de performances, d’événements et de services

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Cette rubrique explique comment afficher et configurer les entrées du journal des événements, les compteurs de performances et les alertes de service affichés pour les serveurs locaux et distants dans Gestionnaire de serveur.  

Les données des journaux des événements, des services et des performances sont affichées à deux emplacements dans la console Gestionnaire de serveur de Windows Server.  

-   Dans le tableau de bord, vous pouvez cliquer sur les lignes **événements**, **performances**et **services** des miniatures pour configurer les données des journaux d’événements, de performances et de services que vous souhaitez afficher pour les rôles, l’ensemble du pool de serveurs gestionnaire de serveur, les groupes créés par l’utilisateur de serveurs et le serveur local. Cliquer sur les lignes hypertexte ouvre les boîtes de dialogue **affichage des détails** qui vous permettent de spécifier les données à propos desquelles vous souhaitez recevoir des alertes dans le tableau de bord. Après avoir configuré les données des journaux d’événements, de services et de performances que vous souhaitez mettre en surbrillance dans les miniatures du tableau de bord, les entrées de journal qui correspondent aux critères que vous avez spécifiés sont répertoriées au bas des boîtes de dialogue **affichage des détails** .  

-   Les vignettes **Événements**, **Services** et **Performances** font partie des pages d’accueil de rôle et de groupe. Les commandes du menu **Tâches** de ces vignettes vous permettent de spécifier les données que vous souhaitez recueillir à partir des serveurs gérés. Les vignettes comportent des filtres et des requêtes qui, si vous le souhaitez, vous permettent de mieux restreindre les entrées de journal affichées dans chaque vignette.  

Cette rubrique contient les sections suivantes.  

-   [Que sont les miniatures ?](#BKMK_thumb)  

-   [Afficher et configurer les événements](#BKMK_events)  

-   [Afficher et configurer les compteurs de performances](#BKMK_perf)  

-   [Gérer les services et configurer les alertes de service](#BKMK_services)  

-   [Afficher et copier les entrées d’événements ou de performances](#BKMK_copy)  

## <a name="BKMK_thumb"></a>Que sont les miniatures ?  
Les *miniatures* sont affichées dans le tableau de bord gestionnaire de serveur pour chaque rôle (la miniature d’un rôle reflète les données recueillies sur tous les serveurs du pool de gestionnaire de serveur qui exécutent le rôle), pour chaque groupe de serveurs, pour le groupe **tous les serveurs** (tous des serveurs dans le pool de Gestionnaire de serveur) et pour le serveur local. Une fois que Gestionnaire de serveur obtient les données des serveurs gérés, des miniatures sont créées automatiquement pour les rôles qui s’exécutent sur des serveurs dans le pool de serveurs.  

Si la console Gestionnaire de serveur s’exécute sur un ordinateur client dans le cadre de Outils d’administration de serveur distant, il n’y a aucune miniature de **serveur local** .  

La miniature dévoile un aperçu rapide de l’état et de la gestion des rôles, des serveurs et des groupes de serveurs. La ligne d’en-tête de miniature change de couleur (et les nombres en surbrillance sont affichés dans la marge de gauche) lorsque les événements, les compteurs de performances, les Best Practices Analyzer les résultats, les services ou les problèmes généraux de gestion répondent aux critères que vous configurez dans le **détail Affiche** les boîtes de dialogue ouvertes en cliquant sur les lignes de la miniature. Le tableau qui suit décrit les données dévoilées dans les miniatures.  

|Ligne de miniatures|Description|  
|---------|--------|  
|Facilité de gestion|La facilité de gestion d’un serveur comprend plusieurs mesures : si le serveur est en ligne ou hors connexion, s’il est accessible et signale les données à Gestionnaire de serveur, si l’utilisateur qui a ouvert une session sur l’ordinateur local dispose de droits d’utilisateur appropriés pour accéder ou gérer le serveur distant, si le serveur distant exécute tous les logiciels requis pour le gérer à distance, ou si le serveur est configuré de façon à ce qu’il soit interrogé et géré à l’aide de Gestionnaire de serveur. Les seules données de gestion que Gestionnaire de serveur pouvez collecter à partir d’un serveur qui exécute Windows Server 2003 est le fait que le serveur soit en ligne ou hors connexion. Pour obtenir des informations détaillées sur les erreurs d’état de la facilité de gestion et leur résolution, consultez le [Guide de dépannage du Gestionnaire de serveur](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx).|  
|Events|Vous pouvez configurer la ligne **Événements** d’une miniature pour afficher les alertes lorsque des événements sont consignés, qui correspondent aux niveaux de gravité, sources, périodes de temps, serveurs ou ID d’événement que vous spécifiez. Affichez des détails sur les événements et modifiez les alertes que vous souhaitez afficher en cliquant sur la ligne **événements** et en ouvrant la boîte de dialogue **affichage des détails des événements** pour le rôle ou le groupe de serveurs.|  
|Services|Vous pouvez configurer la ligne **services** pour afficher des alertes lorsque des services sont détectés dans un rôle ou un groupe de serveurs qui correspondent aux types de démarrage, à l’état du service, aux noms de service et aux serveurs que vous spécifiez dans la boîte de dialogue **affichage des détails des services** .<br /><br />Une fois qu’un serveur a été ajouté au pool de serveurs Gestionnaire de serveur, des alertes de service sur le service de détection de matériel Shell peuvent s’afficher si aucun utilisateur n’est connecté au serveur géré. Cela se produit dans la mesure où le service Détection matériel noyau ne s’exécute que si des utilisateurs sont connectés au serveur géré ou à une session Bureau à distance sur le serveur géré. Pour empêcher au service Détection matériel noyau d’informer de ce cas de figure, cliquez sur **Services** dans les miniatures des groupes de serveurs, notamment le groupe **Tous les serveurs**. Dans la boîte de dialogue **affichage des détails des services** , dans la liste déroulante **services** , désactivez la case à cocher détection matérielle de l' **interpréteur**de commandes, puis cliquez sur **OK**.|  
|Performances|Vous pouvez configurer la ligne **performances** pour afficher des alertes pour un rôle ou un groupe de serveurs lorsque des alertes de performances se produisent, qui correspondent aux types de ressources, serveurs ou périodes de temps que vous spécifiez dans la boîte de dialogue **affichage des détails des performances** .<br /><br />Par défaut, les compteurs de performances sont désactivés. Les serveurs gérés qui exécutent des systèmes d’exploitation plus récents que Windows Server 2003 et pour lesquels des compteurs de performance n’ont pas été démarrés, affichent généralement des erreurs d’état de la gestion des **compteurs de performance en ligne qui ne sont pas démarrés** sur les **serveurs.** vignette des pages de rôle ou de groupe. Pour activer les compteurs de performances pour les serveurs gérés, dans la page **tous les serveurs** , cliquez avec le bouton droit sur les entrées de la vignette **performances** qui indiquent un **État de compteur** **désactivé**, puis cliquez sur Démarrer les **compteurs de performances**. Vous pouvez également démarrer les compteurs de performances en cliquant avec le bouton droit sur les entrées pour les serveurs dans la vignette **serveurs** des pages de rôle ou de groupe, puis en cliquant sur **Démarrer les compteurs de performances**.|  
|Résultats BPA|Vous pouvez configurer la ligne **résultats BPA** pour afficher des alertes pour un rôle ou un groupe de serveurs lorsque des résultats d’analyse BPA sont trouvés qui correspondent aux niveaux de gravité, aux serveurs ou aux catégories BPA que vous spécifiez dans la boîte de dialogue **affichage des détails des résultats BPA** .|  

## <a name="BKMK_events"></a>Afficher et configurer les événements  
Dans cette section, vous allez découvrir comment configurer les données du journal des événements qui sont recueillies à partir des serveurs dans le pool de serveurs Gestionnaire de serveur et les événements que vous souhaitez mettre en surbrillance dans les miniatures.  

> [!NOTE]  
> Les événements à propos desquels vous êtes alerté dans les miniatures sont un sous-ensemble du nombre total d’événements que vous indiquez Gestionnaire de serveur à collecter à partir des serveurs gérés. Bien que la modification des critères d’événement dans la boîte de dialogue **configurer les données d’événement** des vignettes d' **événements** puisse modifier le nombre d’alertes affichées dans le tableau de bord gestionnaire de serveur, la modification des critères d’alerte d’événement dans les miniatures n’a aucun effet sur les données du journal des événements. collectées à partir des serveurs gérés.  

#### <a name="to-configure-the-events-collected-from-managed-servers"></a>Pour configurer les événements collectés sur les serveurs gérés  

1.  Dans la console Gestionnaire de serveur, ouvrez n’importe quelle page à l’exception du tableau de bord. Vous pouvez configurer les événements que vous voulez recueillir à partir des serveurs gérés dans la vignette **Événements** dans les pages de rôles, de groupes de serveurs ou de serveurs locaux.  

2.  Dans le menu **Tâches** de la vignette **Événements** , cliquez sur **Configurer les données d’événement**.  

3.  Sélectionnez les niveaux de gravité d’événement que vous souhaitez collecter à partir des serveurs dans le groupe sélectionné. Par défaut, les niveaux de gravité **Critique**, **Erreur**et **Avertissement** sont sélectionnés.  

4.  Précisez une période pendant laquelle les événements doivent survenir. La durée par défaut pour les événements est de 24 heures.  

5.  Sélectionnez les fichiers journaux d’événements à partir desquels vous souhaitez collecter les événements. Les journaux par défaut sont **Applications**, **Installation**et **Système**.  

6.  Pour enregistrer les modifications, cliquez sur **OK** pour fermer la boîte de dialogue **Configurer les données d’événement** . Les données d’événements sont actualisées automatiquement lors de l’enregistrement de vos modifications.  

#### <a name="to-configure-the-events-highlighted-in-thumbnails"></a>Pour configurer les événements mis en surbrillance dans les miniatures  

1.  Si Gestionnaire de serveur est déjà ouvert, passez à l’étape suivante. S’il n’est pas déjà ouvert, ouvrez-le en effectuant l’une des opérations suivantes.  

    -   Sur le Bureau Windows, démarrez le Gestionnaire de serveur en cliquant sur **Gestionnaire de serveur** dans la barre des tâches Windows.  

    -   Dans l'écran d'**accueil** de Windows, cliquez sur la vignette Gestionnaire de serveur.  

2.  Dans la page du tableau de bord, dans une miniature de la vignette **Rôles et groupes de serveurs**, cliquez sur la ligne **Événements**.  

3.  Dans la boîte de dialogue **affichage des détails des événements** , ajoutez un niveau de gravité aux événements que vous souhaitez afficher. Par défaut, les miniatures affichent uniquement les alertes en surbrillance pour les événements critiques. Notez que le nombre d’événements affichés dans la boîte de dialogue **affichage des détails** augmente lorsque vous ajoutez un niveau de gravité pour lequel vous souhaitez être alerté.  

4.  Dans le champ **Sources d’événements**, sélectionnez les sources d’événements concernant lesquelles vous voulez être alerté. La valeur par défaut est **All**.  

5.  Si cette miniature concerne un rôle installé sur plusieurs serveurs ou un groupe de plusieurs serveurs, vous pouvez sélectionner les serveurs pour lesquels vous souhaitez des alertes d’événement dans la liste déroulante **serveurs** .  

6.  Dans le champ **période** , spécifiez une période de temps allant jusqu’à 1440 minutes, 24 heures ou 1 jour.  

7.  Dans le champ **ID d’événement**, sélectionnez les numéros d’ID d’événements des événements spécifiques concernant lesquels vous voulez être alerté. Vous pouvez taper une plage d’ID d’événements séparés par un tiret ( **-** ) et exclure des ID d’événements de la plage en tapant le tiret avant l’ID d’événement ou la plage d’ID d’événements que vous voulez exclure. Par exemple, la valeur **1,3,5-99,-76** signifie que des alertes sont déclenchées pour les ID d’événements 1 et 3 et tous les événements dont l’ID est compris entre 5 et 99, à l’exception de l’ID d’événement 76.  

8.  Lorsque vous modifiez les critères d’affichage des alertes, le nombre d’alertes d’événements affichées dans le volet de résultats au bas de la boîte de dialogue peut changer. Sélectionnez des entrées dans la liste et cliquez sur **Masquer les alertes** pour empêcher qu’elles n’affectent le nombre d’alertes affiché dans la miniature source. Maintenez la touche **Ctrl** enfoncée à mesure que vous sélectionnez les alertes pour sélectionner plusieurs alertes simultanément. Vous pouvez réaliser cette opération pour les alertes qui sont conformes à vos critères d’alerte d’événement mais que vous n’avez pas besoin de voir.  

9. Cliquez sur **Afficher tout** pour retourner les alertes masquées dans la liste.  

10. Cliquez sur **OK** pour enregistrer vos modifications, fermer la boîte de dialogue **affichage des détails** et afficher les modifications d’alerte d’événement dans la miniature source.  

## <a name="BKMK_perf"></a>Afficher et configurer les données du journal de performances  
Dans cette section, vous allez découvrir comment configurer les données du journal des performances collectées à partir des serveurs dans le pool de serveurs Gestionnaire de serveur, ainsi que les alertes de compteur de performance que vous souhaitez mettre en évidence dans les miniatures.  

Par défaut, les compteurs de performances sont désactivés. Les serveurs gérés qui exécutent des systèmes d’exploitation plus récents que Windows Server 2003 et pour lesquels des compteurs de performance n’ont pas été démarrés, affichent généralement des erreurs d’état de la gestion des **compteurs de performance en ligne qui ne sont pas démarrés** sur les **serveurs.** vignette des pages de rôle ou de groupe. Pour activer les compteurs de performances pour les serveurs gérés, dans la page **tous les serveurs** , cliquez avec le bouton droit sur les entrées de la vignette **performances** qui indiquent un **État de compteur** **désactivé**, puis cliquez sur Démarrer les **compteurs de performances**. Vous pouvez également démarrer les compteurs de performances en cliquant avec le bouton droit sur les entrées pour les serveurs dans la vignette **serveurs** des pages de rôle ou de groupe, puis en cliquant sur **Démarrer les compteurs de performances**.  

> [!NOTE]  
> Les alertes de performances que vous affichez dans les miniatures sont un sous-ensemble des données du compteur de performances total que vous indiquez Gestionnaire de serveur à collecter à partir des serveurs gérés. Bien que la modification des critères d’alerte de performances dans la boîte de dialogue **configurer des alertes** de performances dans les vignettes de **performances** peut modifier le nombre d’alertes affichées dans le tableau de bord gestionnaire de serveur, en modifiant les critères d’alerte de performances dans les miniatures. n’a aucun effet sur les données du journal de performances qui sont collectées à partir des serveurs gérés.  
>   
> Pour cette raison, l’âge maximal des données de performance que vous pouvez afficher dans les miniatures ne peut pas être supérieur à la période d’affichage du graphique maximale configurée dans la boîte de dialogue **Configurer des alertes de performances** . Par exemple, si la valeur de la **période d’affichage du graphique** dans configurer les alertes de **performances** est **1 jour**, la valeur maximale du champ **période** d’une boîte de dialogue Affichage des **Détails des performances** que vous avez ouverte à partir du gestionnaire de serveur le tableau de bord peut être **1 jour**, **24 heures**, ou **1 440 minutes**.  

#### <a name="to-configure-the-performance-log-data-collected-from-managed-servers"></a>Pour configurer les données du journal des performances collectées sur les serveurs gérés  

1.  Dans la console Gestionnaire de serveur, ouvrez n’importe quelle page à l’exception du tableau de bord. Vous pouvez configurer les données de performances que vous voulez recueillir à partir des serveurs gérés dans la vignette **Performances** dans les pages de rôles, de groupes de serveurs ou de serveurs locaux.  

2.  Pour recueillir des données de journaux de performances à partir de serveurs gérés, vous devez activer les compteurs de performance. Si les compteurs de performance sont désactivés, cliquez avec le bouton droit sur une entrée dans la liste vignette des **performances** , puis cliquez sur **Démarrer les compteurs de performances**. La collecte de données avec les compteurs de performance peut nécessiter un certain temps en fonction du nombre de serveurs à partir desquels les données sont recueillies et de la bande passante réseau disponible. Visualisez l’état dans la colonne **État du compteur** .  

3.  Dans le menu **Tâches** de la vignette **Performances** , cliquez sur **Configurer des alertes de performances**.  

4.  pour les serveurs appartenant au groupe sélectionné ou qui exécutent le rôle sélectionné, spécifiez le pourcentage d’utilisation du processeur pour lequel vous souhaitez que les alertes de compteurs de performances soient collectées par Gestionnaire de serveur. La valeur par défaut est 85 %.  

5.  Précisez (en mégaoctets) la mémoire disponible restante dont doivent disposer les serveurs avant la collecte des alertes des compteurs de performances. La valeur par défaut est 2 Mo.  

6.  Spécifiez une période affichée par les graphiques pour les ressources **Utilisation du processeur** et **Mémoire disponible** dans la vignette **Performances** de la page sélectionnée. La période par défaut est définie à une journée. Cliquez sur **Enregistrer**.  

    Notez que le nombre d’alertes de performances affiché dans la vignette **Performances** et le mappage des alertes affichées au fil du temps par le graphique peuvent changer après un clic sur le bouton **Enregistrer**.  

    > [!NOTE]  
    > pour les machines virtuelles qui ont des [mémoire dynamique](https://technet.microsoft.com/library/ff817651.aspx) activées, l’amélioration du seuil des alertes de performances peut entraîner des fausses alertes positives.  

7.  Pour actualiser la liste d’alertes de performances recueillies à partir de vos serveurs, dans le menu **Tâches** , cliquez sur **Actualiser**.  

#### <a name="to-configure-the-performance-alerts-highlighted-in-thumbnails"></a>Pour configurer les alertes de performance mises en surbrillance dans les miniatures  

1.  Dans la page du tableau de bord, dans une miniature de la vignette **Rôles et groupes de serveurs** , cliquez sur la ligne **Performances** .  

2.  Dans la boîte de dialogue **Affichage détails des performances** , activez ou désactivez les cases à cocher pour les seuils de performances des ressources pour lesquels vous souhaitez recevoir des alertes dans le champ **type de ressource** . Notez que le nombre d’alertes de performances affichées dans la boîte de dialogue **affichage des détails** peut augmenter lorsque vous ajoutez un seuil de performances de ressource sur lequel vous souhaitez être alerté.  

3.  Si cette miniature concerne un rôle installé sur plusieurs serveurs ou un groupe de plusieurs serveurs, vous pouvez sélectionner les serveurs pour lesquels vous souhaitez des alertes de performances dans la liste déroulante **serveurs** .  

4.  Dans le champ **période** , spécifiez une période de temps allant jusqu’à 1440 minutes, 24 heures ou 1 jour.  

5.  Lorsque vous modifiez les critères d’affichage des alertes, le nombre d’alertes affichées dans le volet de résultats au bas de la boîte de dialogue peut changer. Cliquez sur **Masquer les alertes** pour masquer toutes les alertes antérieures à l’heure actuelle et les empêcher d’affecter le nombre d’alertes affiché dans la miniature source.  

6.  Cliquez sur **Afficher tout** pour retourner les alertes masquées dans la liste.  

7.  Cliquez sur **OK** pour enregistrer vos modifications, fermer la boîte de dialogue **affichage des détails** et afficher les modifications d’alerte de performances dans la miniature source.  

#### <a name="to-view-the-properties-of-performance-alerts"></a>Pour afficher les propriétés des alertes de performance  

1.  Effectuez l’une des opérations suivantes :  

    -   Dans la page du tableau de bord, dans une miniature de la vignette **Rôles et groupes de serveurs** , cliquez sur la ligne **Performances** .  

    -   Ouvrez une page d’accueil de rôle ou de groupe et recherchez la vignette **Performances** pour le rôle ou le groupe.  

2.  Double-cliquez sur une alerte de performance dans la liste pour afficher ses propriétés. En guise d’alternative, vous pouvez cliquer avec le bouton droit sur une alerte de performances, puis cliquez sur **Afficher les propriétés**.  

3.  Dans la boîte de dialogue **Propriétés d’alerte de performance** , sélectionnez des entrées de journaux pour afficher des informations sur les processus associés aux entrées dans la zone **Processus** .  

4.  Après avoir vérifié les propriétés d’alerte de performance, fermez la boîte de dialogue.  

### <a name="analyze-performance-data-and-solve-problems"></a>Analyser les données de performance et résoudre les problèmes  
Pour plus d’informations sur l’analyse des données des compteurs de performances que vous affichez dans Gestionnaire de serveur et sur la résolution des problèmes de performances sur les serveurs gérés, consultez les ressources suivantes.  

-   [Analyse des données de performances](https://go.microsoft.com/fwlink/?LinkId=239829)  

-   [Résolution des problèmes de performances](https://go.microsoft.com/fwlink/?LinkId=239831)  

Pour plus d’informations sur les outils de surveillance et d’analyse des performances avancés disponibles pour Windows Server 2012 et les versions ultérieures de Windows Server, y compris Server Performance Advisor 3,0, consultez [performances](https://msdn.microsoft.com/windows/hardware/gg463374.aspx) sur MSDN.  

## <a name="BKMK_services"></a>Gérer les services et configurer les alertes de service  
Dans cette section, vous allez découvrir comment démarrer, arrêter, redémarrer, suspendre ou reprendre des services affichés dans la vignette **services** des pages de rôles et de groupes de serveurs dans Gestionnaire de serveur. Vous pouvez également configurer les services à propos desquels vous êtes alerté dans les miniatures du tableau de bord Gestionnaire de serveur.  

> [!NOTE]  
> Vous ne pouvez pas modifier le type de démarrage pour les services, les dépendances de service, les options de récupération ou d’autres propriétés de service dans la vignette services dans Gestionnaire de serveur. Pour modifier des propriétés de services autres que l’état du service, ouvrez le composant logiciel enfichable **Services**. Un raccourci pour ouvrir le composant logiciel enfichable **services** est disponible dans le menu **Outils** de gestionnaire de serveur.  

#### <a name="to-start-stop-restart-pause-or-resume-a-service"></a>Pour démarrer, arrêter, redémarrer, suspendre ou reprendre un service  

1.  Dans la console Gestionnaire de serveur, ouvrez n’importe quelle page à l’exception du tableau de bord (en d’autres termes, toute page d’espace de rôle ou de groupe).  

2.  Dans la vignette **Services** du rôle ou du groupe, cliquez avec le bouton droit sur un service.  

3.  Dans le menu contextuel, cliquez sur l’action à laquelle vous souhaitez soumettre le service. Si vous arrêtez le service, la seule action que vous pouvez réaliser est de le démarrer. De même, si vous le suspendez, la seule action possible est de le reprendre.  

4.  Notez que lorsque vous démarrez, arrêtez, redémarrez, suspendez ou reprenez un service, la valeur de la colonne **État** change pour le service dans la vignette **Services**.  

#### <a name="to-configure-service-alerts-highlighted-in-thumbnails"></a>Pour configurer les alertes de service mises en surbrillance dans les miniatures  

1.  Dans la page du tableau de bord, dans une miniature de la vignette **Rôles et groupes de serveurs**, cliquez sur la ligne **Services**.  

2.  Dans la boîte de dialogue **Affichage détails des services** , sélectionnez les types de démarrage pour les services pour lesquels vous souhaitez être alerté. Par défaut, **automatique (début différé)** et **automatique** sont sélectionnés.  

3.  Sélectionnez les États de service pour lesquels vous souhaitez être alerté. Par défaut, **Tout** est sélectionné.  

4.  Sélectionnez les services à propos desquels vous souhaitez recevoir des alertes. Par défaut, **Tout** est sélectionné.  

5.  Sélectionnez les serveurs associés au rôle ou au groupe pour lequel vous souhaitez recevoir des alertes sur les services. Par défaut, **Tout** est sélectionné.  

6.  Lorsque vous modifiez les critères d’affichage des alertes, le nombre d’alertes affichées dans le volet de résultats au bas de la boîte de dialogue peut changer. Cliquez sur **Masquer les alertes** pour masquer toutes les alertes antérieures à l’heure actuelle et les empêcher d’affecter le nombre d’alertes affiché dans la miniature source.  

7.  Cliquez sur **Afficher tout** pour retourner les alertes masquées dans la liste.  

8.  Cliquez sur **OK** pour enregistrer vos modifications, fermer la boîte de dialogue **affichage des détails** et afficher les modifications d’alerte de service dans la miniature source.  

## <a name="BKMK_copy"></a>Afficher et copier les entrées d’événements, de services ou de performances  
Vous pouvez copier les propriétés d’une entrée d’événement, de service ou de performance dans les boîtes de dialogue **affichage des détails** et dans les vignettes **événements** et **performances** pour un rôle ou un groupe. Cliquez avec le bouton droit sur un événement ou une entrée de performance, puis cliquez sur **copier**.  

La vignette **Événements** vous permet également d’obtenir un aperçu des propriétés d’événements dans la partie inférieure de la vignette en sélectionnant un événement dans la liste. Pour copier les propriétés affichées dans l’aperçu, cliquez avec le bouton droit sur le volet de visualisation, puis cliquez sur **copier**.  

## <a name="see-also"></a>Voir aussi  
[Gestionnaire de serveur](server-manager.md)  
[Filtrer, trier et interroger des données dans les vignettes du Gestionnaire de serveur](filter-sort-and-query-data-in-server-manager-tiles.md)  
