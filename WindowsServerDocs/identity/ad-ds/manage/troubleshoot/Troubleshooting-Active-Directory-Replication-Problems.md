---
ms.assetid: b11f7a65-ec7b-4c11-8dc4-d7cabb54cd94
title: Résolution des problèmes de réplication Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ffc0933f70a2ab518c575a944efdac4c2dcd6a1a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869420"
---
# <a name="troubleshooting-active-directory-replication-problems"></a>Résolution des problèmes de réplication Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Problèmes de réplication Active Directory peuvent avoir plusieurs sources. Par exemple, système DNS (Domain Name) des problèmes, problèmes de mise en réseau ou des problèmes de sécurité tous risquez d’échouer la réplication Active Directory. 

Le reste de cette rubrique explique les outils et une méthodologie générale pour corriger les erreurs de réplication Active Directory. Les sous-rubriques suivantes couvrent les symptômes, causes et comment résoudre les erreurs de réplication spécifique :

## <a name="introduction-and-resources-for-troubleshooting-active-directory-replication"></a>Introduction et des ressources pour la résolution des problèmes de réplication Active Directory

Trafic entrant ou Échec de la réplication sortante permet aux objets Active Directory qui représentent la topologie de réplication, planification de la réplication, les contrôleurs de domaine, les utilisateurs, ordinateurs, les mots de passe, groupes de sécurité, appartenances aux groupes et d’être incohérent de la stratégie de groupe entre les contrôleurs de domaine. Échec incohérence et la réplication d’annuaire provoquer des défaillances opérationnelles ou des résultats incohérents, selon le contrôleur de domaine qui est contacté pour l’opération et peut empêcher l’application de stratégie de groupe et les autorisations de contrôle d’accès. Services de domaine Active Directory (AD DS) dépend de la connectivité réseau, résolution de noms, l’authentification et l’autorisation, la base de données d’annuaire, la topologie de réplication et le moteur de réplication. Lors de la cause racine d’un problème de réplication n’est pas immédiatement évidente, déterminer la cause parmi les causes possibles de nombreux nécessite élimination systématique des causes probables.

Pour un outil basé sur l’interface utilisateur aider à surveiller la réplication et de diagnostiquer les erreurs, consultez [outil État de réplication Active Directory](https://www.microsoft.com/download/details.aspx?id=30005)

Pour un document complet qui décrit comment vous pouvez utiliser l’outil Repadmin pour résoudre les problèmes d’Active Directory, la réplication est disponible ; consultez [surveillance et dépannage de réplication Active Directory à l’aide de Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

Pour plus d’informations sur le fonctionne de la réplication Active Directory, consultez les références techniques suivantes :

- [Référence technique du modèle de réplication Active Directory](https://go.microsoft.com/fwlink/?LinkId=65958)
- [Référence technique de topologie de réplication Active Directory](https://go.microsoft.com/fwlink/?LinkId=93578)

## <a name="event-and-tool-solution-recommendations"></a>Événements et l’outil de recommandations pour la solution

Dans l’idéal, le rouge (erreur) et les événements jaunes (avertissement) dans le journal des événements Service d’annuaire suggèrent la contrainte spécifique qui provoque l’échec de la réplication sur le contrôleur de domaine source ou de destination. Si le message d’événement suggère des étapes pour une solution, essayez les étapes décrites dans l’événement. L’outil Repadmin et autres outils de diagnostics fournissent des informations qui peuvent vous aider à résoudre les échecs de réplication. 

Pour plus d’informations sur l’utilisation de Repadmin pour résoudre les problèmes de réplication, consultez [surveillance et dépannage de réplication Active Directory à l’aide de Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

## <a name="ruling-out-intentional-disruptions-or-hardware-failures"></a>Éliminait les interruptions intentionnelles ou les défaillances matérielles

Parfois, les erreurs de réplication se produisent en raison d’interruptions intentionnelles. Par exemple, lors de la résolution des problèmes de réplication Active Directory, écarter les déconnexions intentionnelles et les défaillances matérielles ou les mises à niveau tout d’abord.

### <a name="intentional-disconnections"></a>Déconnexions intentionnelles

Si des erreurs de réplication sont signalées par un contrôleur de domaine qui est une tentative de réplication avec un contrôleur de domaine qui a été créée dans un site intermédiaire et est actuellement en mode hors connexion en attente de son déploiement dans le site de production finale (un site distant, par exemple une filiale ), vous pouvez prendre en compte pour résoudre ces erreurs de réplication. Pour éviter en séparant un contrôleur de domaine à partir de la topologie de réplication pour des périodes prolongées, ce qui provoque des erreurs continues jusqu'à ce que le contrôleur de domaine est reconnecté, pensez à ajouter ces ordinateurs initialement en tant que serveurs membres et à l’aide de l’installation à partir du média ( Méthode IFM) pour installer les Services de domaine Active Directory (AD DS). Vous pouvez utiliser l’outil de ligne de commande Ntdsutil pour créer le support d’installation que vous pouvez stocker sur un support amovible (CD, DVD ou autre média) et les expédier vers le site de destination. Ensuite, vous pouvez utiliser le support d’installation pour installer les services AD DS sur les contrôleurs de domaine sur le site, sans utiliser de réplication. 

### <a name="hardware-failures-or-upgradestitle"></a>Défaillances matérielles ou des mises à niveau</title>

En cas de problèmes de réplication en raison de la défaillance matérielle (par exemple, la défaillance d’une carte mère, un sous-système de disque ou un disque dur), informez le propriétaire du serveur afin que le problème matériel peut être résolu.

Mises à niveau matérielles périodiques peuvent également entraîner des contrôleurs de domaine hors service. Assurez-vous que les propriétaires de serveurs ont un bon système de communication, tels que pannes à l’avance.

### <a name="firewall-configuration"></a>Délai d'attente du script (secondes)

Par défaut, les appels de procédure distante de la réplication Active Directory (RPC) s’exécutent dynamiquement sur un port disponible via le mappeur de point de terminaison RPC (RPCSS) sur le port 135. Assurez-vous que le pare-feu Windows avec fonctions avancées de sécurité et autres pare-feux est correctement configurés pour autoriser la réplication. Pour plus d’informations sur la spécification du port pour la réplication Active Directory et les paramètres de port, consultez [224196 dans la Base de connaissances Microsoft l’article](https://go.microsoft.com/fwlink/?LinkId=22578). 

Pour plus d’informations sur les ports utilisés par la réplication Active Directory, consultez [outils de réplication Active Directory et les paramètres](https://go.microsoft.com/fwlink/?LinkId=123774).

Pour plus d’informations sur la gestion de la réplication Active Directory sur les pare-feux, consultez [la réplication Active Directory sur les pare-feux](https://go.microsoft.com/fwlink/?LinkId=123775).

## <a name="responding-to-failure-of-an-outdated-server-running-windows-2000-server"></a>Réponse à la défaillance d’un serveur obsolète qui exécutent Windows 2000 Server

Si un contrôleur de domaine exécutant Windows 2000 Server a échoué plus longtemps que le nombre de jours de la durée de vie des objets tombstone, la solution est toujours le même : 

1. Déplacer le serveur à partir du réseau d’entreprise à un réseau privé.
2. Force supprimez Active Directory ou réinstaller le système d’exploitation.
3. Supprimer les métadonnées du serveur à partir d’Active Directory afin que l’objet serveur ne peut pas être réactivée. 

Vous pouvez utiliser un script pour nettoyer les métadonnées du serveur sur la plupart des systèmes d’exploitation de Windows. Pour plus d’informations sur l’utilisation de ce script, consultez [supprimer métadonnées contrôleur de domaine Active Directory](https://go.microsoft.com/fwlink/?LinkID=123599). 

Par défaut, les objets Paramètres NTDS qui sont supprimés sont automatiquement réactivées pour une période de 14 jours. Par conséquent, si vous ne supprimez pas les métadonnées du serveur (utilisez Ntdsutil ou le script mentionné précédemment pour effectuer le nettoyage des métadonnées), les métadonnées du serveur sont reprises dans le répertoire, ce qui vous invite à entrer la réplication tente de se produire. Dans ce cas, erreurs seront être connectés de manière permanente à la suite de l’incapacité à répliquer avec le contrôleur de domaine manquant.

## <a name="root-causes"></a>Causes principales

Si vous écarter les déconnexions intentionnelles, les défaillances matérielles et des contrôleurs de domaine Windows 2000 obsolètes, le reste des problèmes de réplication presque toujours avoir l’une des causes suivantes :

- Connectivité de réseau : La connexion réseau est peut-être indisponible ou les paramètres de réseau ne sont pas configurés correctement.
- Résolution de noms : Erreurs de configuration DNS sont une cause courante des échecs de réplication.
- Authentification et autorisation : Problèmes d’authentification et autorisation de provoquent des erreurs « Accès refusé » quand un contrôleur de domaine tente de se connecter à son partenaire de réplication.
- Base de données de l’annuaire (magasin) : La base de données de répertoire ne peut pas être en mesure de traiter des transactions assez rapidement pour faire face à des délais d’attente de réplication.
- Moteur de réplication : Si les planifications de réplication intersite sont trop courtes, les files d’attente de réplication peuvent être trop volumineux pour être traité dans le temps requis par la planification de réplication sortante. Dans ce cas, la réplication des modifications peut être bloquée indéfiniment potentiellement, suffisamment longtemps pour dépasser la durée de vie de désactivation.
- Topologie de réplication : Contrôleurs de domaine doivent avoir des liens intersites dans AD DS qui mappent aux réseau réel étendu (WAN) ou les connexions de réseau privé virtuel (VPN). Si vous créez des objets dans AD DS pour la topologie de réplication qui ne sont pas pris en charge par la topologie de site réel de votre réseau, la réplication nécessitant la topologie mal configurée échoue.

## <a name="general-approach-to-fixing-problems"></a>Approche générale de résolution des problèmes

Utilisez l’approche générale suivante à la résolution des problèmes de réplication : 

1. Surveiller l’intégrité de réplication de tous les jours, ou utilisez Repadmin.exe pour récupérer tous les jours un état de réplication.
2. Tentez de résoudre tout échec signalée en temps voulu en utilisant les méthodes qui sont décrites dans les messages d’événements et de ce guide. Si le logiciel peut être la cause, désinstallez le logiciel avant de continuer avec d’autres solutions.
3. Si le problème qui provoque l’échec des réplications ne peut pas être résolu par les méthodes connues, supprimer les services AD DS à partir du serveur, puis réinstallez les services AD DS. Pour plus d’informations sur la réinstallation des services AD DS, consultez [désaffectation d’un contrôleur de domaine](https://go.microsoft.com/fwlink/?LinkId=128290).
4. Si les services AD DS ne peut pas être supprimés normalement tandis que le serveur est connecté au réseau, utilisez une des méthodes suivantes pour résoudre le problème :

   - Forcer la suppression de AD DS dans Services Restore Mode annuaire (DSRM), des métadonnées du serveur, de nettoyage, puis réinstallez les services AD DS.
   - Réinstallez le système d’exploitation et reconstruire le contrôleur de domaine.

Pour plus d’informations sur forcer la suppression des services AD DS, consultez [suppression forcée d’un contrôleur de domaine](https://go.microsoft.com/fwlink/?LinkId=128291).

## <a name="using-repadmin-to-retrieve-replication-statustitle"></a>À l’aide de Repadmin pour récupérer l’état de réplication</title>

État de la réplication est un moyen important pour vous permettent d’évaluer l’état du service d’annuaire. Si la réplication fonctionne sans erreur, vous connaissez les contrôleurs de domaine qui sont en ligne. Vous savez également que les systèmes et services suivants fonctionnent :


- Infrastructure DNS
- Protocole d’authentification Kerberos
- Service de temps de Windows (W32time)
- Appel de procédure distante (RPC)
- Connectivité réseau

Utiliser Repadmin pour surveiller l’état de réplication quotidienne en exécutant une commande qui évalue l’état de réplication de tous les contrôleurs de domaine dans votre forêt. La procédure génère un fichier .csv que vous pouvez ouvrir dans Microsoft Excel et de filtre pour les échecs de réplication.

Vous pouvez utiliser la procédure suivante pour récupérer l’état de réplication de tous les contrôleurs de domaine dans la forêt. 

Configuration requise

L'appartenance au groupe **Administrateurs de l'entreprise**, ou équivalent, est la condition minimale requise pour effectuer cette procédure. 

Outils :

- Repadmin.exe
- Excel (Microsoft Office)

### <a name="to-generate-a-repadmin-showrepl-spreadsheet-for-domain-controllers"></a>Pour générer une feuille de calcul repadmin /showrepl pour les contrôleurs de domaine

1. Ouvrez une invite de commandes en tant qu’administrateur. Dans le menuDémarrer, cliquez avec le bouton droit surInvite de commandes, puis cliquez surExécuter en tant qu’administrateur. Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, fournissez les informations d’identification des administrateurs de l’entreprise, si nécessaire, puis cliquez sur Continuer.
2. À l’invite de commandes, tapez la commande suivante, puis appuyez sur ENTRÉE : `repadmin /showrepl * /csv > showrepl.csv`
3. Ouvrez Excel.
4. Cliquez sur le bouton Office, cliquez sur Ouvrir, accédez à showrepl.csv, puis cliquez sur Ouvrir.
5. Masquer ou supprimer la colonne A ainsi que la colonne de Type de Transport, comme suit :
6. Sélectionnez une colonne que vous souhaitez masquer ou supprimer.

   - Pour masquer la colonne, avec le bouton droit de la colonne, puis cliquez sur Masquer.
   - Pour supprimer la colonne, avec le bouton droit de la colonne sélectionnée, puis cliquez sur Supprimer.

7. Sélectionnez la ligne 1 sous la ligne d’en-tête de colonne. Sous l’onglet Affichage, cliquez sur Figer les volets, puis cliquez sur la ligne supérieure de figer.
8. Sélectionnez la feuille de calcul entière. Sous l’onglet données, cliquez sur le filtre.
9. Dans la colonne heure du dernier succès, cliquez sur la flèche vers le bas, puis cliquez sur Tri croissant.
10. Dans la colonne de contrôleur de domaine Source, cliquez sur le filtre de la flèche vers le bas, pointez sur filtres de texte, puis cliquez sur filtre personnalisé.
11. Dans la boîte de dialogue Filtre automatique personnalisé, sous Afficher les lignes où, cliquez sur ne contient-elle pas. Dans la zone de texte adjacente, tapez <userInput>del</userInput> pour éliminer, à partir de la vue, les résultats pour supprimé des contrôleurs de domaine.
12. Répétez l’étape 11 pour la colonne de l’heure du dernier échec, mais qui utilisent la valeur ne pas égaux et puis tapez la valeur 0.
13. Résoudre les échecs de réplication.

Pour chaque contrôleur de domaine dans la forêt, la feuille de calcul montre le partenaire de réplication source, le temps que la réplication dernière s’est produite et l’heure du dernier échec de réplication s’est produit pour chaque contexte de nommage (partition d’annuaire). En utilisant le filtre automatique dans Excel, vous pouvez afficher l’intégrité de réplication pour l’utilisation de contrôleurs de domaine uniquement, le basculement uniquement les contrôleurs de domaine ou des contrôleurs de domaine qui sont moins ou la plupart des cours, et vous pouvez voir les partenaires de réplication qui sont répliquées avec succès.

## <a name="replication-problems-and-resolutions"></a>Résolutions et des problèmes de réplication

Problèmes de réplication sont signalés dans les messages d’événement et divers messages d’erreur qui se produisent lorsqu’une application ou un service tente une opération. Dans l’idéal, ces messages sont collectés par votre application de surveillance ou lorsque vous récupérez l’état de réplication.

La plupart des problèmes de réplication sont identifiées dans les messages d’événements qui sont consignés dans le journal des événements Service d’annuaire. Problèmes de réplication peuvent également être identifiées dans le formulaire des messages d’erreur dans la sortie de la <system>repadmin /showrepl</system> commande.

### <a name="repadmin-showrepl-error-messages-that-indicate-replication-problems"></a>messages d’erreur repadmin /showrepl qui indiquent des problèmes de réplication

Pour identifier les problèmes de réplication Active Directory, utilisez le <system>repadmin /showrepl</system> commande, comme décrit dans la section précédente. Le tableau suivant présente les messages d’erreur qui génère de cette commande, ainsi que les causes des erreurs et des liens vers des rubriques qui fournissent des solutions pour les erreurs.

|Erreur de repadmin|Cause première|Solution|
| --- | --- | --- |
|Le temps depuis la dernière réplication de ce serveur a dépassé la durée de vie de désactivation.|Un contrôleur de domaine a échoué d’une réplication entrante avec le contrôleur de domaine source nommé long suffisant pour une suppression ont été désactivés, répliquée et garbage collector à partir d’AD DS.|ID d’événement 2042 : Trop de temps s'est écoulé depuis la dernière réplication de cet ordinateur|
|Aucun voisin entrant.|Si aucun élément n’apparaît dans la section « Voisins entrants » de la sortie générée par repadmin /showrepl, le contrôleur de domaine n’était pas en mesure d’établir des liens de réplication avec un autre contrôleur de domaine.|Résolution des problèmes de connectivité de réplication (ID d’événement 1925)| 
|L’accès est refusé.|Un lien de réplication existe entre les deux contrôleurs de domaine, mais la réplication ne peut pas être effectuée correctement à la suite d’un échec d’authentification.|Résolution des problèmes de sécurité de réplication| 
|Dernière tentative à < date - heure > a échoué avec le « nom du compte cible est incorrect. »|Ce problème peut être lié à la connectivité, de DNS ou de problèmes d’authentification. S’il s’agit d’une erreur DNS, le contrôleur de domaine local ne peut pas résoudre l’identificateur global unique (GUID)-en fonction du nom DNS de son partenaire de réplication.|Correction réplication DNS recherche des problèmes (ID d’événement 1925, 2087, 2088) résolution réplication sécurité des problèmes de résolution des problèmes de connectivité de réplication (ID d’événement 1925)| 
|Erreur LDAP 49.|Le compte ordinateur de contrôleur de domaine ne peut pas être synchronisé avec le centre de Distribution de clés (KDC).|Résolution des problèmes de sécurité de réplication| 
|Ne peut pas ouvrir de connexion LDAP au niveau hôte local|L’outil d’administration n’a pas pu contacter les services AD DS.|Résolution des problèmes de recherche DNS de réplication (ID d’événement 1925, 2087 et 2088)| 
|La réplication Active Directory a été préemptée.|La progression de la réplication entrante a été interrompue par une demande de réplication de priorité plus élevée, telle qu’une demande qui a été générée manuellement avec la commande repadmin/sync.|Attendez la réplication soit terminée. Ce message d’information indique un fonctionnement normal.| 
|Réplication validée, en attente.| Le contrôleur de domaine validé une demande de réplication et attend une réponse. La réplication est en cours de cette source.|Attendez la réplication soit terminée. Ce message d’information indique un fonctionnement normal.| 

Le tableau suivant répertorie les événements courants qui peuvent indiquer des problèmes avec Active Directory entraîne la réplication, ainsi que de la racine des problèmes et des liens vers des rubriques qui fournissent des solutions pour les problèmes. 

|ID d’événement et source|Cause racine|Solution|
| --- | --- | --- | 
|1311 NTDS KCC|Les informations de configuration de réplication dans AD DS ne reflètent pas exactement la topologie physique du réseau.|Résolution des problèmes de topologie de réplication (ID d’événement 1311)| 
|Réplication 1388 NTDS|Cohérence de réplication stricte n’est pas en vigueur, et un objet en attente ont été répliqué vers le contrôleur de domaine.|Résolution des problèmes des objets en attente de réplication (ID d’événement 1388, 1988, 2042)|
|1925 NTDS KCC|Échec de la tentative pour établir un lien de réplication pour une partition d’annuaire accessible en écriture. Cet événement peut avoir différentes causes, selon l’erreur.| Correction réplication connectivité problèmes (ID d’événement 1925) résolution DNS recherche problèmes de réplication (ID d’événement 1925, 2087, 2088)| 
|Réplication NTDS 1988|Le contrôleur de domaine local a tenté de répliquer un objet à partir d’un contrôleur de domaine source qui n’est pas présent sur le contrôleur de domaine local car peut ont été supprimé et déjà nettoyée. La réplication s’arrête pour cette partition d’annuaire avec ce partenaire jusqu'à ce que la situation est résolue.|Résolution des problèmes des objets en attente de réplication (ID d’événement 1388, 1988, 2042)|
|Réplication 2042 NTDS|La réplication n’a pas eu lieu avec ce partenaire pour une durée de vie de désactivation et la réplication ne peut pas continuer.|Résolution des problèmes des objets en attente de réplication (ID d’événement 1388, 1988, 2042)| 
|Réplication 2087 NTDS|Les services AD DS n’a pas pu résoudre le nom d’hôte DNS du contrôleur de domaine source vers une adresse IP et la réplication a échoué.|Résolution des problèmes de recherche DNS de réplication (ID d’événement 1925, 2087 et 2088)| 
|Réplication 2088 NTDS |Les services AD DS n’a pas pu résoudre le nom d’hôte DNS du contrôleur de domaine source à une adresse IP, mais la réplication a réussi.|Résolution des problèmes de recherche DNS de réplication (ID d’événement 1925, 2087 et 2088)|
|Ouverture de session réseau 5805|Un compte d’ordinateur a échoué pour l’authentification, qui est généralement dû à deux plusieurs instances du même nom d’ordinateur ou le nom d’ordinateur ne pas répliquées vers chaque contrôleur de domaine.|Résolution des problèmes de sécurité de réplication| 

Pour plus d’informations sur les concepts de réplication, consultez [Technologies de réplication Active Directory](https://go.microsoft.com/fwlink/?LinkId=41950).
  
## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations, notamment la prise en charge des articles ayant trait à des codes d’erreur consultez l’article de prise en charge : [Comment résoudre les erreurs de réplication Active Directory courantes](https://support.microsoft.com/help/3108513)
