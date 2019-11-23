---
ms.assetid: b11f7a65-ec7b-4c11-8dc4-d7cabb54cd94
title: Résolution des problèmes de réplication Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cf6b50ab3b4991bd8cab8523494261f1284945a5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71409068"
---
# <a name="troubleshooting-active-directory-replication-problems"></a>Résolution des problèmes de réplication Active Directory

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active Directory problèmes de réplication peuvent avoir plusieurs sources différentes. Par exemple, les problèmes liés au DNS (Domain Name System), aux problèmes de mise en réseau ou aux problèmes de sécurité peuvent entraîner l’échec de la réplication Active Directory. 

Le reste de cette rubrique décrit les outils et une méthodologie générale pour résoudre les erreurs de réplication Active Directory. Les sous-rubriques suivantes couvrent les symptômes, les causes et la résolution des erreurs de réplication spécifiques :

## <a name="introduction-and-resources-for-troubleshooting-active-directory-replication"></a>Présentation et ressources pour la résolution des problèmes de réplication Active Directory

L’échec de la réplication entrante ou sortante entraîne l’incohérence des objets Active Directory qui représentent la topologie de réplication, la planification de la réplication, les contrôleurs de domaine, les utilisateurs, les ordinateurs, les mots de passe, les groupes de sécurité, les appartenances aux groupes et les stratégie de groupe. entre les contrôleurs de domaine. L’incohérence de l’annuaire et l’échec de la réplication entraînent des échecs opérationnels ou des résultats incohérents, en fonction du contrôleur de domaine qui est contacté pour l’opération et peuvent empêcher l’application de stratégie de groupe et des autorisations de contrôle d’accès. Active Directory Domain Services (AD DS) dépend de la connectivité réseau, de la résolution de noms, de l’authentification et de l’autorisation, de la base de données d’annuaire, de la topologie de réplication et du moteur de réplication. Lorsque la cause racine d’un problème de réplication n’est pas immédiatement évidente, la détermination de la cause parmi les nombreuses causes possibles nécessite l’élimination systématique des causes probables.

Pour un outil basé sur l’interface utilisateur qui permet de surveiller la réplication et de diagnostiquer les erreurs, consultez [Active Directory Replication Status outil](https://www.microsoft.com/download/details.aspx?id=30005) .

Pour obtenir un document complet qui décrit comment vous pouvez utiliser l’outil repadmin pour résoudre les problèmes de Active Directory la réplication est disponible. consultez [surveillance et dépannage de la réplication Active Directory à l’aide de repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

Pour plus d’informations sur le fonctionnement de la réplication Active Directory, consultez les références techniques suivantes :

- [Informations techniques de référence sur le modèle de réplication Active Directory](https://go.microsoft.com/fwlink/?LinkId=65958)
- [Informations techniques de référence sur la topologie de réplication Active Directory](https://go.microsoft.com/fwlink/?LinkId=93578)

## <a name="event-and-tool-solution-recommendations"></a>Recommandations relatives à la solution Event and Tool

Dans l’idéal, les événements rouge (erreur) et jaune (avertissement) dans le journal des événements du service d’annuaire suggèrent la contrainte spécifique qui provoque l’échec de la réplication sur le contrôleur de domaine source ou de destination. Si le message d’événement suggère des étapes pour une solution, essayez les étapes décrites dans l’événement. L’outil repadmin et d’autres outils de diagnostic fournissent également des informations qui peuvent vous aider à résoudre les échecs de réplication. 

Pour plus d’informations sur l’utilisation de repadmin pour résoudre les problèmes de réplication, consultez [surveillance et dépannage de la réplication Active Directory à l’aide de repadmin](https://go.microsoft.com/fwlink/?LinkId=122830).

## <a name="ruling-out-intentional-disruptions-or-hardware-failures"></a>Arrêt intentionnel des interruptions ou défaillances matérielles

Des erreurs de réplication se produisent parfois en raison d’interruptions intentionnelles. Par exemple, lorsque vous résolvez les problèmes de réplication Active Directory, vous devez d’abord éliminer les déconnexions volontaires et les défaillances matérielles ou les mises à niveau.

### <a name="intentional-disconnections"></a>Déconnexions intentionnelles

Si des erreurs de réplication sont signalées par un contrôleur de domaine qui tente de réplication avec un contrôleur de domaine qui a été créé dans un site intermédiaire et est actuellement hors connexion en attente de son déploiement sur le site de production final (un site distant, tel qu’une filiale). ), vous pouvez tenir compte de ces erreurs de réplication. Pour éviter de séparer un contrôleur de domaine de la topologie de réplication pendant des périodes prolongées, ce qui entraîne des erreurs continues jusqu’à ce que le contrôleur de domaine soit reconnecté, envisagez d’ajouter ces ordinateurs initialement en tant que serveurs membres et en utilisant l’installation à partir du support ( La méthode IFM) pour installer Active Directory Domain Services (AD DS). Vous pouvez utiliser l’outil de ligne de commande ntdsutil pour créer un support d’installation que vous pouvez stocker sur un support amovible (CD, DVD ou tout autre support) et l’envoyer au site de destination. Ensuite, vous pouvez utiliser le support d’installation pour installer AD DS sur les contrôleurs de domaine sur le site, sans utiliser la réplication. 

### <a name="hardware-failures-or-upgradestitle"></a>Défaillances matérielles ou mises à niveau</title>

Si des problèmes de réplication se produisent suite à une défaillance matérielle (par exemple, la défaillance d’une carte mère, d’un sous-système de disque ou d’un disque dur), informez le propriétaire du serveur afin que le problème matériel puisse être résolu.

Les mises à niveau matérielles périodiques peuvent également entraîner l’arrêt du service des contrôleurs de domaine. Assurez-vous que les propriétaires de votre serveur disposent d’un bon système de communication à l’avance.

### <a name="firewall-configuration"></a>Délai d'attente du script (secondes)

Par défaut, les appels de procédure distante (RPC) de réplication Active Directory se produisent de manière dynamique sur un port disponible via le mappeur de point de terminaison RPC (RPCSS) sur le port 135. Assurez-vous que le pare-feu Windows avec fonctions avancées de sécurité et d’autres pare-feu sont configurés correctement pour permettre la réplication. Pour plus d’informations sur la spécification du port pour la réplication de Active Directory et les paramètres de port, consultez [l’article 224196 de la base de connaissances Microsoft](https://go.microsoft.com/fwlink/?LinkId=22578). 

Pour plus d’informations sur les ports utilisés par Active Directory réplication, consultez [Active Directory les outils et les paramètres de réplication](https://go.microsoft.com/fwlink/?LinkId=123774).

Pour plus d’informations sur la gestion de la réplication Active Directory sur les pare-feu, consultez [Active Directory la réplication sur les pare-feu](https://go.microsoft.com/fwlink/?LinkId=123775).

## <a name="responding-to-failure-of-an-outdated-server-running-windows-2000-server"></a>Réponse à l’échec d’un serveur obsolète exécutant Windows 2000 Server

Si un contrôleur de domaine exécutant Windows 2000 Server a échoué depuis plus longtemps que le nombre de jours dans la durée de vie de désactivation, la solution est toujours la même : 

1. Déplacez le serveur du réseau d’entreprise vers un réseau privé.
2. Supprimez Active Directory ou réinstallez le système d’exploitation.
3. Supprimez les métadonnées du serveur de Active Directory afin que l’objet serveur ne puisse pas être réactivé. 

Vous pouvez utiliser un script pour nettoyer les métadonnées du serveur sur la plupart des systèmes d’exploitation Windows. Pour plus d’informations sur l’utilisation de ce script, consultez [supprimer des métadonnées de contrôleur de domaine Active Directory](https://go.microsoft.com/fwlink/?LinkID=123599). 

Par défaut, les objets Paramètres NTDS qui sont supprimés sont automatiquement réutilisés pour une période de 14 jours. Par conséquent, si vous ne supprimez pas les métadonnées du serveur (à l’aide de Ntdsutil ou du script mentionné précédemment pour effectuer un nettoyage des métadonnées), les métadonnées du serveur sont réintégrées dans l’annuaire, ce qui invite les tentatives de réplication à se produire. Dans ce cas, les erreurs sont consignées de façon permanente suite à l’impossibilité de répliquer avec le contrôleur de domaine manquant.

## <a name="root-causes"></a>Causes racine

Si vous ignorez les déconnexions volontaires, les défaillances matérielles et les contrôleurs de domaine Windows 2000 obsolètes, le reste des problèmes de réplication a presque toujours l’une des causes suivantes :

- Connectivité réseau : la connexion réseau n’est peut-être pas disponible ou les paramètres réseau ne sont pas configurés correctement.
- Résolution de noms : les erreurs de configuration DNS sont souvent à l’origine d’échecs de réplication.
- Authentification et autorisation : les problèmes d’authentification et d’autorisation entraînent des erreurs de refus d’accès lorsqu’un contrôleur de domaine tente de se connecter à son partenaire de réplication.
- Base de données d’annuaire (Store) : la base de données d’annuaire peut ne pas être en mesure de traiter les transactions assez rapidement pour suivre les délais de réplication.
- Moteur de réplication : si les planifications de réplication intersite sont trop courtes, les files d’attente de réplication peuvent être trop volumineuses pour être traitées dans le temps nécessaire à la planification de la réplication sortante. Dans ce cas, la réplication de certaines modifications peut être bloquée indéfiniment, suffisamment longtemps pour dépasser la durée de vie de désactivation.
- Topologie de réplication : les contrôleurs de domaine doivent avoir des liaisons intersites dans AD DS qui sont mappées à des connexions de réseau étendu (WAN) ou de réseau privé virtuel (VPN). Si vous créez des objets dans AD DS pour la topologie de réplication qui ne sont pas pris en charge par la topologie de site réelle de votre réseau, la réplication qui nécessite une topologie mal configurée échoue.

## <a name="general-approach-to-fixing-problems"></a>Approche générale de la résolution des problèmes

Utilisez l’approche générale suivante pour résoudre les problèmes de réplication : 

1. Surveillez l’intégrité de la réplication quotidiennement ou utilisez Repadmin. exe pour récupérer quotidiennement l’état de la réplication.
2. Essayez de résoudre les erreurs signalées en temps utile à l’aide des méthodes décrites dans les messages d’événement et dans ce guide. Si le logiciel peut être à l’origine du problème, désinstallez-le avant de continuer avec d’autres solutions.
3. Si le problème à l’origine de l’échec de la réplication ne peut pas être résolu par des méthodes connues, supprimez AD DS du serveur, puis réinstallez AD DS. Pour plus d’informations sur la réinstallation de AD DS, consultez [désaffectation d’un contrôleur de domaine](https://go.microsoft.com/fwlink/?LinkId=128290).
4. Si AD DS ne peut pas être supprimée normalement alors que le serveur est connecté au réseau, utilisez l’une des méthodes suivantes pour résoudre le problème :

   - Forcez la suppression AD DS en mode de restauration des services d’annuaire (DSRM), nettoyez les métadonnées du serveur, puis réinstallez AD DS.
   - Réinstallez le système d’exploitation et régénérez le contrôleur de domaine.

Pour plus d’informations sur la suppression forcée de AD DS, consultez [forcer la suppression d’un contrôleur de domaine](https://go.microsoft.com/fwlink/?LinkId=128291).

## <a name="using-repadmin-to-retrieve-replication-statustitle"></a>Utilisation de repadmin pour récupérer l’état de la réplication</title>

L’état de la réplication est un moyen important d’évaluer l’état du service d’annuaire. Si la réplication fonctionne sans erreur, vous connaissez les contrôleurs de domaine qui sont en ligne. Vous savez également que les systèmes et services suivants fonctionnent :


- Infrastructure DNS
- Protocole d’authentification Kerberos
- Service de temps Windows (w32time)
- Appel de procédure distante (RPC)
- Connectivité réseau

Utilisez Repadmin pour surveiller l’état de la réplication quotidiennement en exécutant une commande qui évalue l’état de réplication de tous les contrôleurs de domaine de votre forêt. La procédure génère un fichier. csv que vous pouvez ouvrir dans Microsoft Excel et filtrer les échecs de réplication.

Vous pouvez utiliser la procédure suivante pour récupérer l’état de réplication de tous les contrôleurs de domaine dans la forêt. 

Configuration requise

L'appartenance au groupe **Administrateurs de l'entreprise**, ou équivalent, est la condition minimale requise pour effectuer cette procédure. 

Outils :

- Repadmin.exe
- Excel (Microsoft Office)

### <a name="to-generate-a-repadmin-showrepl-spreadsheet-for-domain-controllers"></a>Pour générer une feuille de calcul repadmin/showrepl pour les contrôleurs de domaine

1. Ouvrez une invite de commandes en tant qu’administrateur : dans le menu Démarrer, cliquez avec le bouton droit sur invite de commandes, puis cliquez sur Exécuter en tant qu’administrateur. Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, fournissez les informations d’identification des administrateurs d’entreprise, si nécessaire, puis cliquez sur continuer.
2. À l’invite de commandes, tapez la commande suivante, puis appuyez sur entrée : `repadmin /showrepl * /csv > showrepl.csv`
3. Ouvrez Excel.
4. Cliquez sur le bouton Office, cliquez sur Ouvrir, accédez à showrepl. csv, puis cliquez sur Ouvrir.
5. Masquez ou supprimez la colonne A, ainsi que la colonne type de transport, comme suit :
6. Sélectionnez une colonne que vous souhaitez masquer ou supprimer.

   - Pour masquer la colonne, cliquez avec le bouton droit sur la colonne, puis cliquez sur Masquer.
   - Pour supprimer la colonne, cliquez avec le bouton droit sur la colonne sélectionnée, puis cliquez sur supprimer.

7. Sélectionnez la ligne 1 sous la ligne d’en-tête de colonne. Sous l’onglet Affichage, cliquez sur figer les volets, puis sur figer la ligne supérieure.
8. Sélectionnez l’intégralité de la feuille de calcul. Sous l’onglet données, cliquez sur Filtrer.
9. Dans la colonne heure du dernier succès, cliquez sur la flèche orientée vers le bas, puis cliquez sur tri croissant.
10. Dans la colonne DC source, cliquez sur la flèche bas de filtre, pointez sur filtres de texte, puis cliquez sur filtre personnalisé.
11. Dans la boîte de dialogue Filtre automatique personnalisé, sous Afficher les lignes où, cliquez sur ne contient pas. Dans la zone de texte adjacente, tapez <userInput>del</userInput> pour supprimer de afficher les résultats pour les contrôleurs de domaine supprimés.
12. Répétez l’étape 11 pour la colonne heure du dernier échec, mais utilisez la valeur n’est pas égale à, puis tapez la valeur 0.
13. Résolvez les échecs de réplication.

Pour chaque contrôleur de domaine de la forêt, la feuille de calcul affiche le partenaire de réplication source, l’heure de la dernière réplication et l’heure de la dernière échec de réplication pour chaque contexte d’appellation (partition d’annuaire). À l’aide du filtre automatique dans Excel, vous pouvez afficher l’intégrité de la réplication pour les contrôleurs de domaine en cours d’utilisation uniquement, les contrôleurs de domaine défaillants uniquement ou les contrôleurs de domaine qui sont les moins ou les plus récents, et vous pouvez voir les partenaires de réplication qui répliquent correctement.

## <a name="replication-problems-and-resolutions"></a>Résolution des problèmes de réplication

Les problèmes de réplication sont signalés dans les messages d’événement et dans différents messages d’erreur qui se produisent lorsqu’une application ou un service tente une opération. Dans l’idéal, ces messages sont collectés par votre application de surveillance ou lorsque vous récupérez l’état de la réplication.

La plupart des problèmes de réplication sont identifiés dans les messages d’événements consignés dans le journal des événements du service d’annuaire. Les problèmes de réplication peuvent également être identifiés sous la forme de messages d’erreur dans la sortie de la commande <system>repadmin/showrepl</system> .

### <a name="repadmin-showrepl-error-messages-that-indicate-replication-problems"></a>repadmin/showrepl messages d’erreur indiquant des problèmes de réplication

Pour identifier Active Directory problèmes de réplication, utilisez la commande <system>repadmin/showrepl</system> , comme décrit dans la section précédente. Le tableau suivant indique les messages d’erreur générés par cette commande, ainsi que les causes racines des erreurs et des liens vers des rubriques qui fournissent des solutions aux erreurs.

|Erreur repadmin|Cause première|Solution|
| --- | --- | --- |
|Le temps écoulé depuis la dernière réplication avec ce serveur a dépassé la durée de vie de désactivation.|Un contrôleur de domaine a échoué une réplication entrante avec le contrôleur de domaine source nommé suffisamment longtemps pour qu’une suppression ait été désactivée, répliquée et récupérée par le garbage collector à partir d’AD DS.|ID d’événement 2042 : Trop de temps s’est écoulé depuis la réplication de cette machine|
|Aucun voisin entrant.|Si aucun élément n’apparaît dans la section « voisins entrants » de la sortie générée par repadmin/showrepl, le contrôleur de domaine n’a pas pu établir de liens de réplication avec un autre contrôleur de domaine.|Résolution des problèmes de connectivité de réplication (ID d’événement 1925)| 
|L’accès est refusé.|Un lien de réplication existe entre deux contrôleurs de domaine, mais la réplication ne peut pas être effectuée correctement suite à un échec d’authentification.|Résolution des problèmes de sécurité de réplication| 
|La dernière tentative au < de la date et de l’heure > a échoué avec le nom du compte cible incorrect.|Ce problème peut être lié à des problèmes de connectivité, DNS ou d’authentification. S’il s’agit d’une erreur DNS, le contrôleur de domaine local n’a pas pu résoudre le nom DNS basé sur l’identificateur global unique (GUID) de son partenaire de réplication.|Résolution des problèmes de recherche DNS de réplication (ID d’événement 1925, 2087, 2088) résolution des problèmes de sécurité de réplication résolution des problèmes de connectivité de réplication (ID d’événement 1925)| 
|Erreur LDAP 49.|Il se peut que le compte d’ordinateur du contrôleur de domaine ne soit pas synchronisé avec le centre de distribution de clés (KDC).|Résolution des problèmes de sécurité de réplication| 
|Impossible d’ouvrir la connexion LDAP à l’hôte local|L’outil d’administration n’a pas pu contacter AD DS.|Résolution des problèmes de recherche DNS de réplication (ID d’événement 1925, 2087 et 2088)| 
|La réplication de Active Directory a été préemptée.|La progression de la réplication entrante a été interrompue par une demande de réplication de priorité plus élevée, telle qu’une demande générée manuellement à l’aide de la commande repadmin/sync.|Attendez la fin de la réplication. Ce message d’information indique un fonctionnement normal.| 
|Réplication publiée, en attente.| Le contrôleur de domaine a publié une demande de réplication et attend une réponse. La réplication est en cours à partir de cette source.|Attendez la fin de la réplication. Ce message d’information indique un fonctionnement normal.| 

Le tableau suivant répertorie les événements courants qui peuvent indiquer des problèmes de réplication Active Directory, ainsi que les causes principales des problèmes et des liens vers des rubriques qui fournissent des solutions aux problèmes. 

|ID d’événement et source|Cause racine|Solution|
| --- | --- | --- | 
|KCC NTDS 1311|Les informations de configuration de la réplication dans AD DS ne reflètent pas avec précision la topologie physique du réseau.|Résolution des problèmes de topologie de réplication (ID d’événement 1311)| 
|réplication NTDS 1388|Une cohérence stricte de la réplication n’est pas appliquée et un objet en attente a été répliqué sur le contrôleur de domaine.|Résolution des problèmes des objets en attente de réplication (ID d’événement 1388, 1988, 2042)|
|KCC NTDS 1925|Échec de la tentative d’établissement d’un lien de réplication pour une partition d’annuaire accessible en écriture. Cet événement peut avoir différentes causes, en fonction de l’erreur.| Résolution des problèmes de connectivité de réplication (ID d’événement 1925) résolution des problèmes de recherche DNS de réplication (ID d’événement 1925, 2087, 2088)| 
|réplication NTDS 1988|Le contrôleur de domaine local a tenté de répliquer un objet à partir d’un contrôleur de domaine source qui n’est pas présent sur le contrôleur de domaine local, car il a peut-être été supprimé et a déjà été récupéré par le garbage collector. La réplication ne se poursuit pas pour cette partition d’annuaire avec ce partenaire tant que la situation n’est pas résolue.|Résolution des problèmes des objets en attente de réplication (ID d’événement 1388, 1988, 2042)|
|réplication NTDS 2042|La réplication n’a pas eu lieu avec ce partenaire pour une durée de vie de désactivation, et la réplication ne peut pas continuer.|Résolution des problèmes des objets en attente de réplication (ID d’événement 1388, 1988, 2042)| 
|réplication NTDS 2087|AD DS n’a pas pu résoudre le nom d’hôte DNS du contrôleur de domaine source en adresse IP, et la réplication a échoué.|Résolution des problèmes de recherche DNS de réplication (ID d’événement 1925, 2087 et 2088)| 
|réplication NTDS 2088 |AD DS n’a pas pu résoudre le nom d’hôte DNS du contrôleur de domaine source en adresse IP, mais la réplication a réussi.|Résolution des problèmes de recherche DNS de réplication (ID d’événement 1925, 2087 et 2088)|
|5805 ouverture de session réseau|Un compte d’ordinateur n’a pas pu s’authentifier, ce qui est généralement dû au fait que plusieurs instances du même nom d’ordinateur ou du nom d’ordinateur ne sont pas répliquées sur chaque contrôleur de domaine.|Résolution des problèmes de sécurité de réplication| 

Pour plus d’informations sur les concepts de réplication, consultez [Active Directory technologies de réplication](https://go.microsoft.com/fwlink/?LinkId=41950).
  
## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations, notamment des Articles de support spécifiques aux codes d’erreur, consultez l’article du support technique : [Comment résoudre les erreurs courantes de réplication de Active Directory](https://support.microsoft.com/help/3108513)
