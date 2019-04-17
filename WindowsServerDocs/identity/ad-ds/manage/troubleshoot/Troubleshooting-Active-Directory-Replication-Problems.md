---
ms.assetid: b11f7a65-ec7b-4c11-8dc4-d7cabb54cd94
title: "Résolution des problèmes de réplication ActiveDirectory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: eb93ea7cab297d70aa86b71bd5466004e47bfeaf
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshooting-active-directory-replication-problems"></a>Résolution des problèmes de réplication ActiveDirectory

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012


Problèmes de réplication ActiveDirectory peuvent avoir plusieurs sources. Par exemple, des problèmes système DNS (Domain Name), des problèmes réseau ou des problèmes de sécurité peuvent entraînent toutes l’échec de la réplication ActiveDirectory. 

Le reste de cette rubrique décrit les outils et une méthodologie générale pour corriger les erreurs de réplication ActiveDirectory. Pour un atelier pratique qui montre comment résoudre les problèmes de réplication ActiveDirectory, voir [laboratoire virtuel TechNet: résolution des problèmes d’erreurs de réplication ActiveDirectory](https://go.microsoft.com/?linkid=9844718)

Les sous-rubriques suivantes couvrent les symptômes, les causes et comment résoudre les erreurs de réplication spécifique:
   
[Résolution des problèmes des objets de réplication en attente (ID d’événement 1388, 1988, 2042)](https://technet.microsoft.com/library/cc949124.aspx)
  
[Résolution des problèmes de sécurité de réplication](https://technet.microsoft.com/library/cc949132.aspx)

[Résolution DNS recherche des problèmes de réplication (ID d’événement 1925, 2087 et 2088)](https://technet.microsoft.com/library/cc949133.aspx)
  
[Résolution des problèmes de connectivité de réplication (ID d’événement 1925)](https://technet.microsoft.com/library/cc949131.aspx)
  
[Résolution des problèmes de topologie de réplication (ID d’événement 1311)](https://technet.microsoft.com/library/cc949129.aspx)
  
[Vérifier la fonctionnalité DNS pour prendre en charge la réplication d’annuaire](https://technet.microsoft.com/library/dd728017.aspx)
  
[Erreur de réplication ActiveDirectory8614 ne peut pas répliquer ce serveur car la durée écoulée depuis la dernière réplication de ce serveur a dépassé la durée de vie](https://technet.microsoft.com/library/replication-error-8614-the-active-directory-cannot-replicate-with-this-server-because-the-time-since-the-last-replication-with-this-server-has-exceeded-the-tombstone-lifetime.aspx)
     
[La réplication erreur 8524 de l’opération DSA ne peut pas continuer en raison d’une défaillance de la recherche DNS](https://technet.microsoft.com/library/replication-error-8524-the-dsa-operation-is-unable-to-proceed-because-of-a-dns-lookup-failure.aspx)
   
[Erreur de réplication 8456 ou 8457 source | serveur de destination rejette actuellement les demandes de réplication](https://technet.microsoft.com/library/replication-error-8456-the-source-server-is-currently-rejecting-replication-requests-or-replication-error-8457-the-destination-server-is-currently-rejecting-replication-requests.aspx)
   
[Accès à la réplication 8453 erreur la réplication a été refusé.](https://technet.microsoft.com/library/replication-error-8453-replication-access-was-denied.aspx)
  
[Erreur de réplication 8452 le contexte de nommage est prêt à être supprimé ou n’est pas répliqué à partir du serveur spécifié](https://technet.microsoft.com/library/replication-error-8452-the-naming-context-is-in-the-process-of-being-removed-or-is-not-replicated-from-the-specified-server.aspx)
   
[Erreur de réplication5 accès refusé](https://technet.microsoft.com/library/replication-error-5-access-is-denied.aspx)
  

      
      

  
[Erreur de réplication-2146893022 le nom du principal cible est incorrect](https://technet.microsoft.com/library/replication-error-2146893022-the-target-principal-name-is-incorrect.aspx)
  

      
      

  
[Erreur de réplication 1753 il ne sont aucun point de terminaison plus le mappeur de point de terminaison](https://technet.microsoft.com/library/replication-error-1753-there-are-no-more-endpoints-available-from-the-endpoint-mapper.aspx)
  

      
      

  
[La réplication erreur 1722 le serveur RPC n’est pas disponible](https://technet.microsoft.com/library/replication-error-1722-the-rpc-server-is-unavailable.aspx)
  

      
      

  
[Erreur de réplication Échec d’ouverture de session 1396 le nom du compte cible est incorrect](https://technet.microsoft.com/library/replication-error-1396-logon-failure-the-target-account-name-is-incorrect.aspx)
  

      
      

  
[Erreur de réplication1256 le système distant n’est pas disponible](https://technet.microsoft.com/library/replication-error-1256-the-remote-system-is-not-available.aspx)
  

      
      

  
[Erreur de réplication 1127lors de l’accès au disque dur, une opération disque a échoué malgré plusieurs essais](https://technet.microsoft.com/library/replication-error-1127-while-accessing-the-hard-disk-a-disk-operation-failed-even-after-retries.aspx)
  

      
      

  
[Erreur de réplication 8451 l’opération de réplication a rencontré une erreur de base de données](https://technet.microsoft.com/library/replication-error-8451-the-replication-operation-encountered-a-database-error.aspx)
  

      
      

  
[La réplication erreur réplication8606des attributs insuffisants ont été donnés pour créer un objet](https://technet.microsoft.com/library/replication-error-8606-insufficient-attributes-were-given-to-create-an-object.aspx)
  

## <a name="introduction-and-resources-for-troubleshooting-active-directory-replication"></a>Introduction et des ressources pour la résolution des problèmes de réplication ActiveDirectory

Trafic entrant ou des objets ActiveDirectory qui représentent la topologie de réplication, planification de réplication, contrôleurs de domaine, les utilisateurs, ordinateurs, les mots de passe, des groupes de sécurité, appartenances aux groupes et stratégie de groupe pour ne pas être cohérentes entre les contrôleurs de domaine entraîne l’échec de réplication sortant. Échec d’une incohérence et la réplication Active entraîner des échecs opérationnels ou incohérences, selon le contrôleur de domaine qui est contacté pour l’opération et peuvent empêcher l’application de stratégie de groupe et les autorisations de contrôle d’accès. Services de domaine ActiveDirectory (ADDS) dépend de la connectivité réseau, la résolution de noms, l’authentification et d’autorisation, la base de données active, la topologie de réplication et le moteur de réplication. Lorsque la cause d’un problème de réplication n’est pas immédiatement évidente, déterminer la cause parmi les causes possibles de nombreux nécessite systématique élimination des causes probables. 

Pour plus d’un outil basé sur une interface utilisateur aider à surveiller la réplication et diagnostiquer des erreurs, consultez [outil de l’état de la réplication ActiveDirectory](https://www.microsoft.com/download/details.aspx?id=30005). Il existe également un [pratiques](https://go.microsoft.com/?linkid=9844718) qui montre comment utiliser l’état de la réplication ActiveDirectory et d’autres outils pour résoudre les erreurs. 

Pour un document complet qui décrit comment vous pouvez utiliser l’outil Repadmin pour résoudre les problèmes d’ActiveDirectory, la réplication est disponible; voir [analyse et résolution des problèmes ActiveDirectory la réplication à l’aide de Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

Pour plus d’informations sur le fonctionne de la réplication ActiveDirectory, consultez les références techniques suivantes:


- [Référence technique du modèle de réplication Active Directory](https://go.microsoft.com/fwlink/?LinkId=65958)
- [Référence technique de la topologie de réplication ActiveDirectory](https://go.microsoft.com/fwlink/?LinkId=93578)

## <a name="event-and-tool-solution-recommendations"></a>Événements et l’outil de recommandations en matière de solution

Dans l’idéal, le rouge (erreur) et les événements jaunes (avertissement) dans le journal des événements Service d’annuaire suggèrent la contrainte spécifique qui provoque l’échec de la réplication sur le contrôleur de domaine source ou de destination. Si le message d’événement indique les étapes d’une solution, essayez les étapes décrites dans l’événement. L’outil Repadmin et autres outils de diagnostic fournissent également des informations qui peuvent vous aider à résoudre les échecs de réplication. 

Pour plus d’informations sur l’utilisation de Repadmin pour résoudre les problèmes de réplication, voir [analyse et résolution des problèmes ActiveDirectory la réplication à l’aide de Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

## <a name="ruling-out-intentional-disruptions-or-hardware-failures"></a>Exclure l’interruption intentionnelle ou des défaillances matérielles

Erreurs de réplication peuvent se produire en raison des interruptions intentionnelles. Par exemple, lors de la résolution des problèmes de réplication ActiveDirectory, la règle déconnexions intentionnelles et des défaillances matérielles ou des mises à niveau tout d’abord.

### <a name="intentional-disconnections"></a>Déconnexions intentionnelles

Si des erreurs de réplication sont signalées par un contrôleur de domaine qui tente de la réplication avec un contrôleur de domaine qui a été généré dans un site intermédiaire et est actuellement en mode hors connexion en attente de son déploiement dans le site de production finale (un site distant, par exemple, une filiale), vous pouvez tenir compte pour résoudre ces erreurs de réplication. Pour éviter la séparation d’un contrôleur de domaine à partir de la topologie de réplication sur de longues périodes, ce qui provoque des erreurs en continu jusqu'à ce que le contrôleur de domaine est reconnecté, envisagez initialement l’ajout de ces ordinateurs en tant que serveurs membres et à l’aide de l’installation à partir de la méthode de support (IFM) pour installer les Services de domaine ActiveDirectory (ADDS). Vous pouvez utiliser l’outil de ligne de commande Ntdsutil pour créer le support d’installation que vous pouvez stocker sur un support amovible (CD, DVD ou autre support) et l’envoyer au site de destination. Ensuite, vous pouvez utiliser le support d’installation pour installer les services ADDS sur les contrôleurs de domaine sur le site, sans avoir recours à la réplication. 

### <a name="hardware-failures-or-upgradestitle"></a>Défaillances matérielles ou les mises à niveau</title>

Si des problèmes de réplication se produisent en raison de la défaillance matérielle (par exemple, l’échec d’une carte mère, un sous-système de disque ou un disque dur), notifier le propriétaire du serveur afin que le problème matériel peut être résolu.

Mises à niveau matérielles périodique peuvent également provoquer des contrôleurs de domaine hors service. Vérifiez que les propriétaires de vos serveurs un bon système de communication à l’avance de ces interruptions.

### <a name="firewall-configuration"></a>Configuration du pare-feu

Par défaut, les appels de procédure distante de la réplication ActiveDirectory (RPC) se produisent dynamiquement via un port disponible via le mappeur de point de terminaison RPC (RPCSS) sur le port 135. Assurez-vous que le pare-feu Windows avec fonctions avancées de sécurité et autres pare-feux est correctement configurés pour autoriser la réplication. Pour plus d’informations sur la spécification du port pour la réplication ActiveDirectory et les paramètres de port, consultez [article 224196dans la Base de connaissances Microsoft](https://go.microsoft.com/fwlink/?LinkId=22578). 

Pour plus d’informations sur les ports qui utilise la réplication ActiveDirectory, consultez [paramètres et outils de réplication ActiveDirectory](https://go.microsoft.com/fwlink/?LinkId=123774). 

Pour plus d’informations sur la gestion de la réplication ActiveDirectory via des pare-feu, voir [la réplication ActiveDirectory via des pare-feu](https://go.microsoft.com/fwlink/?LinkId=123775).

## <a name="responding-to-failure-of-an-outdated-server-running-windows-2000-server"></a>Répondre à l’échec d’un serveur obsolète qui exécutent Windows2000 Server

Si un contrôleur de domaine exécutant Windows2000 Server a échoué pour dépasser le nombre de jours de la durée de vie de temporisation, la solution est toujours le même: 

1. Déplacer le serveur à partir du réseau d’entreprise à un réseau privé.
2. Soit volontairement supprimer ActiveDirectory ou réinstaller le système d’exploitation.
3. Supprimez les métadonnées du serveur ActiveDirectory afin que l’objet serveur ne peut pas être réactivée. 

Vous pouvez utiliser un script pour nettoyer les métadonnées du serveur sur la plupart des systèmes d’exploitation Windows. Pour plus d’informations sur l’utilisation de ce script, consultez [supprimer métadonnées contrôleur de domaine ActiveDirectory](https://go.microsoft.com/fwlink/?LinkID=123599). 

Par défaut, les objets Paramètres NTDS qui sont supprimés sont automatiquement réactivées pendant une période de 14jours. Par conséquent, si vous ne supprimez pas les métadonnées du serveur (utilisez Ntdsutil ou le script mentionné précédemment pour effectuer un nettoyage des métadonnées), les métadonnées du serveur ne sont pas réactivée dans le répertoire, qui demande à la réplication tente de se produire. Dans ce cas, les erreurs seront consignées durablement suite à l’incapacité à répliquer avec le contrôleur de domaine manquant.

## <a name="root-causes"></a>Causes principales.

Si vous éliminez les déconnexions intentionnelles, défaillances matérielles et les contrôleurs de domaine Windows2000obsolètes, le reste de problèmes de réplication ont presque toujours une des causes suivantes: 


- La connectivité réseau: la connexion réseau peut ne pas être disponible, ou les paramètres de réseau ne sont pas configurés correctement.
- La résolution de noms: des erreurs de configuration DNS sont une cause courante d’échecs de réplication.
- Authentification et autorisation: problèmes d’authentification et d’autorisation provoquent des erreurs «Accès refusé» lorsqu’un contrôleur de domaine tente de se connecter à son partenaire de réplication.
- Base de données active (stockage): la base de données active ne peut pas être en mesure de traiter les transactions assez rapidement pour suivre les délais d’attente de réplication.
- Moteur de réplication: si les planifications de réplication intersite sont trop courtes, les files d’attente de réplication peuvent être trop grandes pour traiter dans le temps nécessaire à la planification de réplication sortant. Dans ce cas, la réplication de certaines modifications peut être bloquée indefinitelypotentially, suffisamment long pour dépasser la durée de vie de temporisation.
- Topologie de réplication: contrôleurs de domaine doivent avoir des liens intersites dans ADDS qui correspondent aux réseau réel étendu (WAN) ou des connexions de réseau privé virtuel (VPN). Si vous créez des objets dans ADDS pour la topologie de réplication qui ne sont pas pris en charge par la topologie du site réel de votre réseau, qui nécessite la topologie mal configurée la réplication échoue.

## <a name="general-approach-to-fixing-problems"></a>Approche générale de résolution des problèmes

Utilisez l’approche général suivante pour la résolution des problèmes de réplication: 


1. Surveiller intégrité de la réplication de tous les jours, ou utilisez Repadmin.exe pour récupérer un état de réplication quotidienne.
2. Essayer de résoudre tout échec signalé en temps voulu en utilisant les méthodes décrites dans ce guide et de messages d’événement. Si le logiciel peut être à l’origine du problème, désinstallez le logiciel avant de continuer avec d’autres solutions.
3. Si le problème qui provoque l’échec de la réplication ne peut pas être résolu par les méthodes connues, supprimer les services ADDS à partir du serveur, puis réinstallez les services ADDS. Pour plus d’informations sur la réinstallation des services ADDS, voir [désaffectation d’un contrôleur de domaine](https://go.microsoft.com/fwlink/?LinkId=128290).
4. Si les services ADDS ne peut pas être supprimés normalement tandis que le serveur est connecté au réseau, utilisez une des méthodes suivantes pour résoudre le problème:
 

    - Forcer la suppression de ADDS dans les Services restaurer Mode annuaire (DSRM), les métadonnées du serveur, de nettoyage, puis réinstallez les services ADDS.
    - Réinstaller le système d’exploitation et reconstruisez le contrôleur de domaine.

Pour plus d’informations sur forcer la suppression des services ADDS, voir [forcer la suppression d’un contrôleur de domaine](https://go.microsoft.com/fwlink/?LinkId=128291).

## <a name="using-repadmin-to-retrieve-replication-statustitle"></a>À l’aide de Repadmin pour récupérer l’état de réplication</title>

État de la réplication constitue un moyen important pour vous permet d’évaluer l’état du service d’annuaire. Si la réplication fonctionne sans erreur, vous connaissez les contrôleurs de domaine qui sont en ligne. Vous savez également que les systèmes et services suivants fonctionnent:


- Infrastructure DNS
- Protocole d’authentification Kerberos
- Service de temps Windows (W32time)
- Appel de procédure distante (RPC)
- Connectivité réseau

Utiliser Repadmin pour surveiller l’état de réplication quotidienne en exécutant une commande qui évalue l’état de réplication de tous les contrôleurs de domaine dans votre forêt. La procédure génère un fichier .csv que vous pouvez ouvrir dans MicrosoftExcel et filtrer les échecs de réplication.

Vous pouvez utiliser la procédure suivante pour récupérer l’état de réplication de tous les contrôleurs de domaine dans la forêt. 
      
Configuration requise
      
L’appartenance au groupe **administrateurs de l’entreprise**, ou équivalente, est la condition minimale requise pour effectuer cette procédure. 

Outils: 


- Repadmin.exe 
- Excel (MicrosoftOffice)

### <a name="to-generate-a-repadmin-showrepl-spreadsheet-for-domain-controllers"></a>Pour générer un repadmin /showrepl feuille de calcul pour les contrôleurs de domaine



1. Ouvrez une invite de commandes en tant qu’administrateur: dans le menu Démarrer, cliquez sur invite de commandes, puis cliquez sur Exécuter en tant qu’administrateur. Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, fournissez les informations d’identification des administrateurs de l’entreprise, si nécessaire, puis cliquez sur Continuer. 
2. À l’invite de commandes, tapez la commande suivante et appuyez sur ENTRÉE:`repadmin /showrepl * /csv &gt;showrepl.csv`
3. Ouvrez Excel.
4. Cliquez sur le bouton Office, cliquez sur Ouvrir, accédez à showrepl.csv, puis cliquez sur Ouvrir.
5. Masquer ou de supprimer la colonne A ainsi que la colonne Type de Transport, comme suit:
6. Sélectionnez une colonne que vous souhaitez masquer ou supprimer.
      

    - Pour masquer la colonne, avec le bouton droit de la colonne, puis cliquez sur Masquer.
    - Pour supprimer la colonne, avec le bouton droit de la colonne sélectionnée, puis cliquez sur Supprimer.
7. Sélectionnez la ligne 1sous la ligne d’en-tête de colonne. Sous l’onglet Affichage, cliquez sur Figer les volets, puis cliquez sur haut figer ligne.
8. Sélectionnez la feuille de calcul entière. Sous l’onglet données, cliquez sur le filtre.
9. Dans la colonne de l’heure du dernier succès, cliquez sur la flèche vers le bas, puis cliquez sur Tri croissant.
10. Dans la colonne de contrôleur de domaine Source, cliquez sur le filtre flèche vers le bas, pointez sur filtres de texte, puis cliquez sur filtre personnalisé.
11. Dans la boîte de dialogue Filtre automatique personnalisé, sous Afficher les lignes où, cliquez sur ce bouton ne contient-elle pas. Dans la zone de texte adjacents, tapez <userInput>del</userInput> à éliminer les résultats pour affichage supprimé les contrôleurs de domaine.
12. Répétez l’étape11 pour la colonne de l’heure du dernier échec, mais qui utilisent la valeur sans égal et tapez la valeur 0.
13. Résoudre les échecs de réplication.

Pour chaque contrôleur de domaine dans la forêt, la feuille de calcul affiche le partenaire de réplication source, le temps que la réplication dernière s’est produite et l’heure du dernier échec de la réplication s’est produit pour chaque contexte de nommage (partition active). À l’aide de filtre automatique dans Excel, vous pouvez afficher l’intégrité de la réplication pour l’utilisation de contrôleurs de domaine seulement, échec uniquement les contrôleurs de domaine, ou sont en cours moins ou la plupart des contrôleurs de domaine, et vous pouvez voir les partenaires de réplication qui répliquent correctement.

## <a name="replication-problems-and-resolutions"></a>Résolutions et des problèmes de réplication
    
Des problèmes de réplication sont signalés dans les messages d’événement et différents messages d’erreur qui se produisent lorsqu’une application ou un service tente une opération. Dans l’idéal, ces messages sont collectées par votre application d’analyse ou lorsque vous récupérez l’état de réplication.

La plupart des problèmes de réplication sont identifiés dans les messages d’événement qui sont consignés dans le journal des événements Service d’annuaire. Des problèmes de réplication peuvent également être identifiés sous la forme de messages d’erreur dans la sortie de la <system>repadmin /showrepl</system> commande. 

### <a name="repadmin-showrepl-error-messages-that-indicate-replication-problems"></a>Repadmin /showrepl messages d’erreur qui indiquent des problèmes de réplication

Pour identifier les problèmes de réplication ActiveDirectory, utilisez le <system>repadmin /showrepl</system> de commande, comme décrit dans la section précédente. Le tableau suivant présente les messages d’erreur qui génère de cette commande, ainsi que les causes principales des erreurs et des liens vers des rubriques qui offrent des solutions pour les erreurs.


|Erreur de repadmin|Cause principale|Solution|
| --- | --- | --- |
|Le temps depuis la dernière réplication de ce serveur a dépassé la durée de vie de temporisation.|Un contrôleur de domaine a échoué à la réplication entrante avec le contrôleur de domaine source nommé temps suffisant pour une suppression ont été désactivés, répliqué et garbage collector à partir des services ADDS.|ID d’événement 2042: Il a trop de temps écoulé depuis la réplication de cet ordinateur|
|Aucun voisin entrant.|Si aucun élément n’apparaît dans la section «Inbound Neighbors» de la sortie générée par repadmin /showrepl, le contrôleur de domaine n’a pas réussi à établir des liens de réplication avec un autre contrôleur de domaine.|Résolution des problèmes de connectivité de réplication (ID d’événement 1925)| 
|L’accès est refusé.|Il existe un lien de réplication entre deux contrôleurs de domaine, mais la réplication ne peut pas être effectuée correctement à la suite d’un échec d’authentification.|Résolution des problèmes de sécurité de réplication| 
|Dernière tentative à < date - heure > a échoué avec le «nom du compte cible est incorrect».|Ce problème peut être lié à la connectivité, de DNS ou de problèmes d’authentification. S’il s’agit d’une erreur DNS, contrôleur de domaine local Impossible de résoudre l’identificateur global unique (GUID)-en fonction du nom DNS de son partenaire de réplication.|Résolution DNS recherche des problèmes (ID d’événement 1925, 2087 et 2088) résolution réplication sécurité des problèmes de réplication résolution des problèmes de connectivité de réplication (ID d’événement 1925)| 
|Erreur LDAP 49.|Le compte ordinateur de contrôleur de domaine n’est pas synchronisé avec le centre de Distribution de clés (KDC).|Résolution des problèmes de sécurité de réplication| 
|Ne peut pas établir une connexion LDAP à l’hôte local|L’outil d’administration n’a pas pu contacter les services ADDS.|Résolution DNS recherche des problèmes de réplication (ID d’événement 1925, 2087 et 2088)| 
|La réplication ActiveDirectory a été interrompue.|La progression de la réplication entrante a été interrompue par une demande de réplication de priorité plus élevée, par exemple, une demande a été générée manuellement avec la commande repadmin /sync.|Attendez la fin de la réplication. Ce message d’information indique un fonctionnement normal.| 
|La réplication validée, en attente.| Le contrôleur de domaine validé une demande de réplication et attend une réponse. La réplication est en cours de cette source.|Attendez la fin de la réplication. Ce message d’information indique un fonctionnement normal.| 

Le tableau suivant répertorie les événements courants qui peuvent indiquer des problèmes de réplication, ainsi que de racine entraîne des problèmes et des liens vers des rubriques qui fournissent des solutions pour les problèmes d’ActiveDirectory. 

|ID d’événement et source|Cause principale|Solution|
| --- | --- | --- | 
|1311NTDS KCC|Les informations de configuration de la réplication dans les services ADDS ne reflètent pas avec précision la topologie du réseau physique.|Résolution des problèmes de topologie de réplication (ID d’événement 1311)| 
|Réplication1388NTDS|Cohérence de réplication stricte n’est pas activée, et un objet en attente a été répliqué sur le contrôleur de domaine.|Résolution des problèmes des objets de réplication en attente (ID d’événement 1388, 1988, 2042)|
|1925NTDS KCC|Échec de la tentative d’établir un lien de réplication pour une partition d’annuaire accessible en écriture. Cet événement peut avoir différentes causes, en fonction de l’erreur.| Résolution connectivité problèmes (ID d’événement 1925) corriger la réplication DNS recherche des problèmes de réplication (ID d’événement 1925, 2087 et 2088)| 
|Réplication1988NTDS|Le contrôleur de domaine local a tenté de répliquer un objet à partir d’un contrôleur de domaine source qui n’est pas présent sur le contrôleur de domaine local car peut avoir été supprimé et déjà nettoyée. La réplication ne continuera pas pour cette partition d’annuaire avec ce partenaire jusqu'à ce que le problème est résolu.|Résolution des problèmes des objets de réplication en attente (ID d’événement 1388, 1988, 2042)|
|Réplication NTDS2042|La réplication n’a pas eu lieu avec ce partenaire pour une durée de vie de temporisation et la réplication ne peut pas continuer.|Résolution des problèmes des objets de réplication en attente (ID d’événement 1388, 1988, 2042)| 
|Réplication2087NTDS|Les services ADDS Impossible de résoudre le nom d’hôte DNS du contrôleur de domaine source à l’adresse IP et la réplication a échoué.|Résolution DNS recherche des problèmes de réplication (ID d’événement 1925, 2087 et 2088)| 
|Réplication2088NTDS |Les services ADDS Impossible de résoudre le nom d’hôte DNS du contrôleur de domaine source pour une adresse IP, mais la réplication a réussi.|Résolution DNS recherche des problèmes de réplication (ID d’événement 1925, 2087 et 2088)|
|Ouverture de session réseau 5805|Un compte d’ordinateur n’a pas pu s’authentifier, qui est généralement dû à un plusieurs instances du même nom d’ordinateur ou le nom d’ordinateur ne pas répliquées vers chaque contrôleur de domaine.|Résolution des problèmes de sécurité de réplication| 

Pour plus d’informations sur les concepts de réplication, voir [des Technologies de réplication ActiveDirectory](https://go.microsoft.com/fwlink/?LinkId=41950).
  
