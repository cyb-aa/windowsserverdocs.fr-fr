---
title: "Gérer l’intégrité du système dans Windows Server Essentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3043f83b-389c-4f37-a1ff-85afe99314fa
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 91635a58c64fbf74d3b0139be7c9c36365487319
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="manage-system-health-in-windows-server-essentials"></a>Gérer l’intégrité du système dans Windows Server Essentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials
 
 Cette rubrique explique comment afficher et répondre à toutes les alertes de votre réseau à l’aide du tableau de bord.  
  
> [!NOTE]
>  Dans Windows Server Essentials et Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials installé, les alertes d’intégrité du serveur et les ordinateurs clients du réseau ne sont plus affichés dans l’Afficheur des alertes, mais au lieu de cela peut être affichés sur le **rapports d’intégrité** onglet de la **accueil** page.  
  
 Windows Server Essentials surveille activement chaque ordinateur qui est connecté au serveur et avertit l’administrateur des problèmes liés à l’intégrité système s, y compris les mises à jour critiques, protection contre les programmes malveillants, les définitions de virus obsolètes sur les ordinateurs clients et d’autres problèmes importants qui nécessitent une action manquantes. Ces problèmes sont affichés sous forme d’alertes dans l’Afficheur des alertes, qui peut être lancé depuis le serveur s tableau de bord ou de l’ordinateur client s Launchpad dans Windows Server Essentials, ou sur le **rapports d’intégrité** onglet dans Windows Server Essentials. Par défaut, les alertes sont actualisés toutes les 30 minutes, mais vous pouvez évaluer votre réseau pour les alertes à tout moment en cliquant sur **Actualiser** dans l’Afficheur des alertes ou sur le **rapports d’intégrité** onglet.  
  
 Les rubriques suivantes vous aideront à comprendre, afficher et répondre aux alertes dans l’Afficheur des alertes et fournissent également des instructions pour configurer votre serveur pour recevoir des notifications d’alerte par courrier électronique:  
  
-   [Sur l’intégrité signaler complément](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_AddIn)  
  
-   [Afficher les alertes à l’aide de l’Afficheur des alertes](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_View)  
  
-   [Organiser les alertes dans l’Afficheur des alertes](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Organize)  
  
-   [Répondre aux alertes](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Respond)  
  
-   [Configurer des notifications par courrier électronique pour les alertes](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email)  
  
-   [Alertes potentielles de l’ordinateur](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Potential)  
  
##  <a name="BKMK_AddIn"></a>Sur l’intégrité signaler complément  
 Le complément de rapport d’intégrité pour Windows Server Essentials vous fournit des informations consolidées sur le réseau de Windows Server Essentials et vous permet de distribuer ces informations à d’autres personnes. Ces informations peuvent être affichées sur la **rapports** onglet du tableau de bord. Avec la **rapports** onglet, vous pouvez procédez comme suit:  
  
-   [Générer un rapport à la demande ou planifié](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Generate)  
  
-   [Personnaliser le contenu du rapport](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Customize)  
  
-   [Le rapport de messagerie](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_emailreport)  
  
> [!NOTE]
>  **Windows Server Essentials:** que vous pouvez télécharger le complément de rapport d’intégrité pour Windows Server Essentials à partir de la [Microsoft Download Center](https://go.microsoft.com/fwlink/p/?LinkId=266342).  
>   
>  **Windows Server Essentials:** par défaut, le complément de rapport d’intégrité est intégré à Windows Server Essentials ou Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials installé, et les rapports d’intégrité sont affichés sous la **rapports d’intégrité** onglet du tableau de bord s **accueil** page.  
  
###  <a name="BKMK_Generate"></a>Générer un rapport à la demande ou planifié  
 Après avoir installé le complément de rapport d’intégrité et redémarré le tableau de bord, un nouvel onglet, **rapports** est ajouté au tableau de bord. Vous pouvez générer un rapport d’intégrité à la demande à tout moment en cliquant sur le **générer un rapport d’intégrité** de tâches sur le **rapports** onglet.  
  
 Une fois un rapport d’intégrité est généré, un nouvel élément est créé dans le volet liste, identifié par date et heure de que la génération du rapport. Pour ouvrir un élément, vous pouvez double-cliquer dessus dans le volet liste, ou vous pouvez sélectionner, puis sur **ouvrir le rapport d’intégrité** dans le volet des tâches. Le rapport s’affiche dans une nouvelle fenêtre au format HTML.  
  
 En plus de générer un rapport manuellement, vous pouvez également le rapport doit être généré automatiquement selon un calendrier quotidien ou haire. Pour ce faire, dans le volet des tâches, cliquez sur **personnaliser les paramètres du rapport d’intégrité**, puis cliquez sur le **calendrier et courrier électronique** onglet. Le **calendrier** fonctionnalité est désactivée par défaut, et vous pouvez l’activer en sélectionnant le **générer un rapport d’intégrité à l’heure planifiée** case à cocher.  
  
###  <a name="BKMK_Customize"></a>Personnaliser le contenu du rapport  
 Le rapport d’intégrité contient les éléments suivants:  
  
-   **Alertes critiques et avertissements** cela est cohérent avec les alertes critiques et avertissements qui apparaissent dans l’Afficheur des alertes sur le tableau de bord. Les alertes d’informations ne sont pas inclus dans le rapport d’intégrité.  
  
-   **Des erreurs critiques dans les journaux des événements** journaux des Applications et services sont analysés, et les erreurs sont enregistrées dans les dernières 24 heures seront afficheront dans le **détails** section du rapport.  
  
-   **Sauvegarde du serveur** les informations sur la dernière sauvegarde du serveur sont présentées dans le **détails** section du rapport.  
  
-   **Services à démarrage automatique ne s’exécute pas** à la fois le rapport est généré, si un service à démarrage automatique n’est pas en cours d’exécution, les informations sur ce service seront afficheront dans le **détails** section du rapport.  
  
-   **Mises à jour** vous pouvez voir l’état de mise à jour du serveur et tous les ordinateurs clients dans le **détails** section.  
  
-   **Stockage** la liste des lecteurs et leur capacité est présentée dans le **détails** section.  
  
 Dans le rapport d’intégrité, consultez d’abord le **Résumé**, puis pour ces éléments avec une icône d’erreur rouge ou une icône d’avertissement jaune, cliquez sur le **détails** sur la même ligne pour afficher les détails sur l’élément de lien.  
  
 Si vous n’êtes pas intéressés par certains des points de données qui sont inclus dans le rapport par défaut, vous pouvez personnaliser le contenu du rapport en cliquant sur **personnaliser les paramètres du rapport d’intégrité** dans le volet des tâches, puis en cliquant sur le **contenu**onglet désactivez les cases à cocher pour le contenu que vous ne pas afficher dans le rapport. Par exemple, si vous avez votre propre plan de sauvegarde de serveur et don t voulez voir les avertissements concernant les sauvegardes de serveur, vous pourriez exclure des sauvegardes du serveur à partir du rapport en désactivant le **sauvegarde du serveur** case à cocher.  
  
###  <a name="BKMK_emailreport"></a>Le rapport de messagerie  
 Avoir à se connecter au tableau de bord pour lire des rapports est pas toujours pratique pour certains administrateurs, en particulier si elles ont plusieurs serveurs à gérer. Avec la fonctionnalité de messagerie activée, une fois un rapport est généré, un courrier électronique est envoyé à une liste d’adresses de messagerie spécifiée avec le contenu du rapport. L’administrateur peut facilement consulter ce rapport depuis n’importe quel appareil ou de n’importe quelle application client et assurez-vous que le serveur s’exécute de manière optimale.  
  
 Dans le **personnaliser les paramètres du rapport d’intégrité** boîte de dialogue, après avoir activé la messagerie électronique, modifié les paramètres SMTP et spécifié une liste de destinataires de courrier électronique, vous remarquerez qu’une nouvelle tâche s’affiche dans le volet des tâches: **le rapport d’intégrité de la messagerie**. Pour plus d’informations sur les paramètres SMTP, voir [configurer des notifications par courrier électronique pour les alertes](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email).  
  
 Vous pouvez sélectionner un rapport existant, puis cliquez sur **le rapport d’intégrité de la messagerie**. Vous pouvez également générer un rapport et paramétrer son envoi automatique dans votre boîte de réception. Si vous avez configuré une planification pour le rapport doit être généré automatiquement, le rapport sera déposé automatiquement dans votre boîte de réception après avoir été généré chaque jour (ou toutes les heures) comme prévu.  
  
##  <a name="BKMK_View"></a>Afficher les alertes à l’aide de l’Afficheur des alertes  
 Cette section explique comment utiliser le tableau de bord ou le Launchpad pour ouvrir l’Afficheur des alertes pour afficher l’état d’intégrité de tous les ordinateurs sur le réseau du serveur.  
  
#### <a name="to-open-the-alert-viewer-by-using-the-dashboard"></a>Pour ouvrir l’Afficheur des alertes à l’aide du tableau de bord  
  
1.  Ouvrez le tableau de bord.  
  
2.  Dans le volet de navigation, cliquez sur une des icônes d’alerte affichées (critique, avertissement ou information). L’Afficheur des alertes s’ouvre.  
  
#### <a name="to-open-the-alert-viewer-from-the-launchpad"></a>Pour ouvrir l’Afficheur des alertes à partir du Launchpad  
  
1.  À partir d’un ordinateur qui est connecté au serveur, ouvrez le Launchpad. Si vous êtes invité, connectez-vous au Launchpad avec votre nom d’utilisateur et un mot de passe.  
  
2.  Cliquez sur une des icônes d’alerte affichées (critiques, avertissements et information) en bas du Launchpad pour ouvrir l’Afficheur des alertes, puis suivez les instructions dans le volet détails de l’Afficheur des alertes pour résoudre l’alerte.  
  
##  <a name="BKMK_Organize"></a>Organiser les alertes dans l’Afficheur des alertes  
 Vous pouvez organiser les alertes dans l’Afficheur des alertes et les afficher selon leur gravité (critique, avertissement ou information) ou basé sur le nom de l’ordinateur.  
  
#### <a name="to-organize-alerts-in-the-alert-viewer"></a>Pour organiser les alertes dans l’Afficheur des alertes  
  
1.  Ouvrez le tableau de bord.  
  
2.  Dans le volet de navigation, cliquez sur l’une des icônes d’alerte affichées (critique, avertissement ou information). L’Afficheur des alertes s’ouvre.  
  
3.  Développez le **organiser** liste déroulante, puis effectuez l’une des opérations suivantes:  
  
    1.  Sélectionnez **filtrer par ordinateur**, puis cliquez sur le nom d’ordinateur pour lequel vous souhaitez afficher les alertes. Cela affiche des alertes dans l’Afficheur des alertes uniquement pour l’ordinateur sélectionné.  
  
    2.  Sélectionnez **filtrer par type d’alerte**, puis cliquez sur le type d’alerte (critique, avertissement ou information) pour lequel vous souhaitez afficher les alertes. Cela affiche uniquement le type d’alerte sélectionné dans l’Afficheur des alertes.  
  
##  <a name="BKMK_Respond"></a>Répondre aux alertes  
 Lorsque vous rencontrez une alerte, vous pouvez choisir d’effectuer une des opérations suivantes:  
  
-   [Résoudre une alerte](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Resolve)  
  
-   [Ignorer une alerte](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Activer une alerte](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Supprimer une alerte](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_4)  
  
###  <a name="BKMK_Resolve"></a>Résoudre une alerte  
 Suivez les instructions de résolution de l’Afficheur des alertes pour résoudre l’alerte. Une fois une alerte est résolue, il est toujours affiché dans l’Afficheur des alertes jusqu'à ce qu’il est actualisé.  
  
###  <a name="BKMK_3"></a>Ignorer une alerte  
 Vous pouvez choisir d’ignorer une alerte si vous préférez y répondre ultérieurement. Lorsque vous ignorez une alerte, il est toujours répertorié dans l’Afficheur des alertes, et mais elle est désactivée et grisée. Une alerte ignorée n’est pas incluse dans l’évaluation d’intégrité globale de l’ordinateur. Pour résoudre une alerte ignorée, vous devez d’abord activer l’alerte.  
  
##### <a name="to-ignore-an-alert"></a>Pour ignorer une alerte  
  
1.  À partir de l’ordinateur est connecté au serveur Windows Server Essentials, ouvrez le Launchpad.  
  
2.  Sur le Launchpad, cliquez sur l’une des icônes d’alerte affichées (critiques, avertissements et information). L’Afficheur des alertes s’ouvre.  
  
3.  Dans l’Afficheur des alertes, sélectionnez l’alerte que vous voulez ignorer, puis, dans le **tâches** , cliquez sur **ignorer l’alerte**.  
  
 Pour répondre à une alerte désactivée, vous devez d’abord activer l’alerte.  
  
###  <a name="BKMK_5"></a>Activer une alerte  
 Vous pouvez activer une alerte que vous avez choisi d’ignorer précédemment. Une fois que l’alerte est activée, vous pouvez résoudre ou la supprimer selon les besoins. Une alerte apparaît désactivée quand elle est marquée comme devant être ignorée. Lorsque vous activez une alerte que vous avez précédemment désactivé, il devient actif et est à nouveau inclus dans l’évaluation d’intégrité globale des ordinateurs.  
  
##### <a name="to-enable-an-alert"></a>Pour activer une alerte  
  
1.  À partir de l’ordinateur est connecté au serveur, ouvrez le Launchpad.  
  
2.  Sur le Launchpad, cliquez sur une des icônes d’alerte affichées (critiques, avertissements et information) pour ouvrir l’Afficheur des alertes.  
  
3.  Dans l’Afficheur des alertes, l’alerte que vous souhaitez activer, puis cliquez avec le bouton droit **activer l’alerte**.  
  
###  <a name="BKMK_4"></a>Supprimer une alerte  
 Vous pouvez supprimer une alerte si vous ne souhaitez pas résoudre ou l’ignorer. Vous pouvez utiliser l’Afficheur des alertes sur le Launchpad pour supprimer les alertes générées pour votre ordinateur. Si vous supprimez une alerte et le serveur détecte le problème dans le prochain cycle d’évaluation de contrôle d’intégrité réseau, il génère une nouvelle alerte.  
  
##### <a name="to-delete-an-alert"></a>Pour supprimer une alerte  
  
1.  À partir de l’ordinateur est connecté au serveur, ouvrez le Launchpad.  
  
2.  Sur le Launchpad, cliquez sur une des icônes d’alerte affichées (critiques, avertissements et information) pour ouvrir l’Afficheur des alertes.  
  
3.  Dans l’Afficheur des alertes, l’alerte que vous souhaitez supprimer, puis cliquez avec le bouton droit **supprimer l’alerte**.  
  
##  <a name="BKMK_Email"></a>Configurer des notifications par courrier électronique pour les alertes  
 Vous pouvez configurer votre serveur pour recevoir une notification par courrier électronique de l’occurrence des alertes. Les notifications par courrier électronique pour ces alertes contiennent des informations sur les problèmes de réseau et leur résolution, qui est identique pour les informations qui sont affiche dans l’Afficheur des alertes. Certaines des évaluations d’intégrité réseau sont effectuées par programme.  
  
 Lorsque vous configurez votre serveur pour envoyer des notifications d’alerte par courrier électronique, une notification par courrier électronique est envoyée pour les alertes détectés lors de l’évaluation d’intégrité du réseau. Toutefois, pas toutes les alertes qui sont signalées dans l’Afficheur des alertes sont signalés par courrier électronique.  
  
 Toutes les 30 minutes, la tâche d’alerte par courrier électronique d’évaluation s’exécute sur le serveur, qui évalue le réseau pour les alertes. Une notification par courrier électronique est envoyée dès toute alerte définie pour une notification par courrier électronique se produit. Un deuxième courrier électronique n’est pas envoyé si l’alerte est toujours active lors du prochain cycle d’évaluation pour éviter d’encombrer votre boîte aux lettres. Toutefois, si une nouvelle alerte est détectée dans un cycle d’évaluation ultérieur, une notification par courrier électronique est envoyée, qui inclut les alertes anciennes et nouvelles.  
  
###  <a name="BKMK_list"></a>Alertes qui génèrent des notifications par courrier électronique  
 Les alertes suivantes dans l’Afficheur des alertes entraîner des notifications par courrier électronique lorsque vous configurez votre serveur pour envoyer des notifications par courrier électronique pour les alertes:  
  
-   Il existe des erreurs dans une sauvegarde de l’ordinateur client.  
  
-   Le serveur a été redémarré.  
  
-   Un ou plusieurs services ne sont pas en cours d’exécution.  
  
-   Le Service de stockage Windows Server n’est pas en cours d’exécution.  
  
-   Votre période d’évaluation a expiré.  
  
-   Activer maintenant.  
  
-   Arrêt du serveur source.  
  
-   Résultats d’analyse BPA contiennent des erreurs.  
  
-   Résultats d’analyse BPA contiennent des avertissements.  
  
-   Un certificat n’est pas disponible pour l’accès en tout lieu.  
  
-   Le certificat pour l’accès en tout lieu a expiré.  
  
-   Le routeur n’est pas configuré correctement.  
  
-   Le serveur Web n’est pas configuré correctement.  
  
-   Services Bureau à distance n’est pas configuré correctement.  
  
-   Le pare-feu n’est pas configuré correctement.  
  
-   Le nom de domaine Internet a expiré.  
  
-   Le nom de domaine Internet ne peuvent pas être mis à jour.  
  
-   Erreur de licence: Vérification de l’approbation de forêt.  
  
-   Erreur de licence: Vérification du contrôleur de domaine.  
  
-   Erreur de licence: Vérification du rôle FSMO.  
  
-   Erreur de licence: Stratégies FSMO de contrôle.  
  
-   Erreur de licence: Stratégies de chargement de l’application.  
  
-   Erreur de licence: Services de domaine Active Directory.  
  
-   Votre abonnement Office 365 a expiré.  
  
-   Échec de l’authentification d’Office 365.  
  
-   La stratégie de mot de passe n’est pas correcte.  
  
-   Le Service de synchronisation de mot de passe ne peut pas synchroniser un mot de passe utilisateur avec Office 365.  
  
-   Modifier votre mot de passe Windows.  
  
-   Votre mot de passe Office 365 n’est pas identique à votre mot de passe Windows.  
  
-   Impossible de se connecter à Exchange Server.  
  
-   Microsoft Exchange Server a un problème.  
  
-   Un ou plusieurs disques durs de sauvegarde du serveur ne sont pas connectés.  
  
-   Le disque dur de sauvegarde ne dispose pas de suffisamment d’espace libre pour la sauvegarde du serveur.  
  
-   Sauvegarde du serveur a échoué en raison d’une capture instantanée du lecteur n’a pas pu être prise.  
  
-   Une sauvegarde planifiée ne s’est pas terminée correctement.  
  
-   Un ou plusieurs dossiers serveur prédéfinis manquent.  
  
-   Espace libre est insuffisant sur un ou plusieurs disques durs du serveur.  
  
-   L’enregistreur VSS pour le Service de stockage n’est pas en cours d’exécution.  
  
-   Capacité de stockage faible sur les disques durs.  
  
-   Un ou plusieurs lecteurs ne fonctionnent pas et sont hors connexion.  
  
###  <a name="BKMK_SMTP"></a>Configuration du protocole SMTP sur votre serveur pour envoyer des notifications par courrier électronique dans Windows Server Essentials  
 Cette section explique comment configurer votre serveur pour envoyer des notifications par courrier électronique pour les alertes.  
  
> [!NOTE]
>  Vous pouvez télécharger le complément de rapport d’intégrité pour Windows Server Essentials à partir de la [Microsoft Download Center](https://go.microsoft.com/fwlink/p/?LinkId=266342).  
  
##### <a name="to-set-up-email-notification-for-alerts"></a>Pour configurer la notification par courrier électronique pour les alertes  
  
1.  À partir de la **tableau de bord**, ouvrez le **Afficheur des alertes**.  
  
2.  Dans le **Afficheur des alertes**, cliquez sur **configurer des notifications d’alerte par courrier électronique**.  
  
3.  Dans le **configurer la notification par courrier électronique pour les alertes** fenêtre, cliquez sur **activer**.  
  
4.  Dans le **paramètres SMTP** fenêtre, procédez comme suit:  
  
    1.  Pour **à partir de l’adresse de messagerie**, tapez l’adresse de messagerie que vous souhaitez utiliser pour envoyer le courrier électronique des alertes de. Cette adresse de messagerie s’affichera en tant que l’adresse d’expéditeur s dans la notification d’alerte.  
  
    2.  Pour **nom du serveur SMTP**, dans le **à partir de l’adresse de messagerie** texte, tapez le nom du serveur SMTP que vous avez spécifié dans l’étape 4 a. (Consultez le tableau 1 pour obtenir une liste de certains noms de serveur SMTP).  
  
    3.  Pour **port SMTP**, tapez le numéro de port qui est utilisé par le serveur SMTP pour envoyer et recevoir des courriers électroniques. (Consultez le tableau 1 pour les numéros de port utilisés par certains des serveurs SMTP).  
  
    4.  Sélectionnez **ce serveur exige une connexion sécurisée (SSL)** si le serveur SMTP utilise le protocole SSL (voir tableau 1).  
  
    5.  Sélectionnez **ce serveur exige une authentification** si le serveur SMTP nécessite des informations de nom et mot de passe d’utilisateur (voir tableau 1). Si vous sélectionnez cette case à cocher, tapez le nom d’utilisateur et mot de passe de l’adresse de messagerie que vous avez entré dans le **à partir de l’adresse de messagerie** champ dans l’étape 4 a, puis cliquez sur **OK**.  
  
    > [!NOTE]
    >  Vous pouvez obtenir les informations sur le nom du serveur SMTP, numéro de port et d’utilisation SSL à partir de votre fournisseur de services Internet.  
  
     **Tableau 1** les noms de serveur exemples du protocole SMTP, l’authentification et les exigences de chiffrement SSL et numéros de port  
  
    |Serveur SMTP|SSL nécessaire|Authentification requise|Numéro de port|Nom du compte/nom de connexion|  
    |-----------------|------------------|-----------------------------|-----------------|------------------------------|  
    |SMTP.Gmail.com|Oui|Oui|587|Fournir l’adresse de messagerie complète avec le nom de domaine et mot de passe pour l’authentification.|  
    |suivant|Oui|Oui|587|Fournir l’adresse de messagerie complète avec le nom de domaine et mot de passe pour l’authentification.|  
    |SMTP.Comcast.NET|Oui|N°|587|Fournir l’adresse de messagerie complète avec le nom de domaine et mot de passe pour l’authentification.|  
    |SMTP.Mail.Yahoo.com|N°|Oui|25|Fournir uniquement l’adresse de messagerie sans nom de domaine pour le nom d’utilisateur.|  
  
5.  Dans **configurer la notification des alertes**, pour **destinataires de courrier électronique**, tapez les adresses de messagerie des personnes que vous souhaitez recevoir des notifications d’alerte par courrier électronique. Séparez chaque adresse de messagerie par un point-virgule (;).  
  
6.  Pour vérifier que vous avez configuré votre serveur SMTP paramètres correctement pour envoyer des notifications par courrier électronique pour les alertes, cliquez sur **appliquer et envoyer par courrier électronique**.  
  
    > [!NOTE]
    >  Lorsque vous cliquez sur **appliquer et envoyer par courrier électronique**, en règle générale, vous recevrez une notification par courrier électronique d’exemple avec aucune liste des alertes d’intégrité. Toutefois, si une alerte d’intégrité qui est configurée pour envoyer une notification par courrier électronique est identifiée durant ce processus de test, cette alerte est incluse dans le message électronique de test.  
  
### <a name="configuring-smtp-on-your-server-to-send-health-reports-in-windows-server-essentials"></a>Configuration du protocole SMTP sur votre serveur pour envoyer des rapports d’intégrité dans Windows Server Essentials  
 Cette section explique comment configurer les paramètres SMTP pour votre serveur afin que vous pouvez recevoir des rapports d’intégrité par courrier électronique.  
  
> [!NOTE]
>  Par défaut, le complément de rapport d’intégrité est intégré à Windows Server Essentials ou Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials installé, et les rapports d’intégrité sont affichés sous la **rapports d’intégrité** onglet du tableau de bord s **accueil** page.  
  
##### <a name="to-set-up-email-notification-for-health-reports"></a>Pour configurer la notification par courrier électronique pour les rapports d’intégrité  
  
1.  À partir de la **tableau de bord**, cliquez sur le **rapports** onglet.  
  
2.  Dans le **tâches du rapport d’intégrité** volet des tâches, cliquez sur **personnaliser les paramètres du rapport d’intégrité**.  
  
3.  Dans le **personnaliser les paramètres du rapport d’intégrité** fenêtre, cliquez sur le **calendrier et courrier électronique** onglet.  
  
4.  Dans le **calendrier et courrier électronique** onglet le **messagerie** section, procédez comme suit:  
  
    1.  Cliquez sur **activer**et tapez l’adresse de messagerie que vous souhaitez utiliser pour l’envoi de l’intégrité des rapports de. Cette adresse de messagerie s’affichera en tant que l’adresse d’expéditeur s dans les rapports d’intégrité envoyés.  
  
        1.  Pour **nom du serveur SMTP**, tapez le nom du serveur SMTP. (Consultez le tableau 1 pour obtenir une liste de certains noms de serveur SMTP).  
  
        2.  Pour **port SMTP**, tapez le numéro de port qui est utilisé par le serveur SMTP pour envoyer et recevoir des courriers électroniques. (Consultez le tableau 1 pour les numéros de port utilisés par certains des serveurs SMTP).  
  
        3.  Sélectionnez **ce serveur exige une connexion sécurisée (SSL)** si le serveur SMTP utilise le protocole SSL (voir tableau 1).  
  
        4.  Sélectionnez **ce serveur exige une authentification** si le serveur SMTP nécessite des informations de nom et mot de passe d’utilisateur (voir tableau 1). Si vous sélectionnez cette case à cocher, tapez le nom d’utilisateur et mot de passe de l’adresse de messagerie que vous avez entré dans le **à partir de l’adresse de messagerie** champ dans l’étape 4 a, puis cliquez sur **OK**.  
  
            > [!NOTE]
            >  Vous pouvez obtenir les informations sur le nom du serveur SMTP, numéro de port et d’utilisation SSL à partir de votre fournisseur de services Internet.  
  
            > [!NOTE]
            >  Microsoft vous recommande vivement d’utiliser SSL, car le rapport peut contenir l’état du serveur qui peut être utilisé par des personnes malveillantes pour détecter des vulnérabilités (par exemple: mise à jour windows manquante). L’activation de SSL pour chiffrer les données en transit et réduire le risque d’exposition de vulnérabilité du serveur.  
  
5.  Après avoir activé la messagerie électronique, le **modifier les paramètres SMTP** lien s’affiche. Également une nouvelle tâche, **le rapport d’intégrité de la messagerie**, s’affiche dans **tâches du rapport d’intégrité**.  
  
     **Tableau 1** les noms de serveur exemples du protocole SMTP, l’authentification et les exigences de chiffrement SSL et numéros de port  
  
    |Serveur SMTP|SSL nécessaire|Authentification requise|Numéro de port|Nom du compte/nom de connexion|  
    |-----------------|------------------|-----------------------------|-----------------|------------------------------|  
    |SMTP.Gmail.com|Oui|Oui|587|Fournir l’adresse de messagerie complète avec le nom de domaine et mot de passe pour l’authentification.|  
    |suivant|Oui|Oui|587|Fournir l’adresse de messagerie complète avec le nom de domaine et mot de passe pour l’authentification.|  
    |SMTP.Comcast.NET|Oui|N°|587|Fournir l’adresse de messagerie complète avec le nom de domaine et mot de passe pour l’authentification.|  
    |SMTP.Mail.Yahoo.com|N°|Oui|25|Fournir uniquement l’adresse de messagerie sans nom de domaine pour le nom d’utilisateur.|  
  
6.  Dans **personnaliser les paramètres du rapport d’intégrité**, pour **automatiquement envoyer le rapport d’intégrité aux destinataires de courrier électronique suivants:**, tapez les adresses de messagerie des personnes que vous souhaitez recevoir des rapports d’intégrité par courrier électronique. Séparez chaque adresse de messagerie par un point-virgule (;).  
  
7.  Pour vérifier que vous avez configuré votre serveur SMTP paramètres correctement pour envoyer des rapports d’intégrité par courrier électronique, à partir de l’onglet rapport d’intégrité du tableau de bord, sélectionnez un rapport, puis cliquez sur **le rapport d’intégrité de la messagerie** dans le volet des tâches.  
  
##  <a name="BKMK_Potential"></a>Alertes potentielles de l’ordinateur  
 Cette section décrit le fonctionnement et la gestion des alertes qui sont spécifiques à votre ordinateur qui est connecté au serveur et qui apparaissent dans la zone de lancement de votre ordinateur.  
  
 Le tableau suivant répertorie certaines des alertes d’ordinateur qui peuvent être générées et affichées dans l’Afficheur des alertes si elles sont applicables à votre ordinateur.  
  
|Titre de l’alerte|Impact et résolution d’alerte|  
|-----------------|---------------------------------|  
|L’état actuel du pare-feu de réseau offre une protection réduite pour cet ordinateur.|Les personnes non autorisées ou des logiciels peut être en mesure d’accéder à cet ordinateur si le pare-feu Windows n’est pas activé.|  
|Protection antivirus est désactivée, non installée ou périmée.|Les données sur votre ordinateur sont exposées si le **protection antivirus** paramètre de sécurité est désactivé ou pas mis à jour. [Pour protéger votre ordinateur](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Protect), suivez les étapes indiquées.|  
|Les logiciels espions et protection des logiciels indésirables est désactivée, non installée ou périmée.|Les données sur votre ordinateur sont exposées si le **les logiciels espions et indésirables de protection logicielle** est désactivé ou pas mis à jour. [Pour protéger votre ordinateur](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Protect), suivez les étapes indiquées.|  
|Windows Update est désactivé.|Vous ne pourrez pas bénéficier des fonctionnalités nouvelles et corrigées des mises à jour, sauf si la mise à jour de Windows est activé. Pour activer Windows Update, dans l’Afficheur des alertes, cliquez sur **ouvrir Windows Update**.<br /><br /> Si le **ouvrir Windows Update** tâche n’est pas affichée, vous n’êtes pas connecté à l’ordinateur sur lequel l’alerte a été déclenchée. Vous devez être connecté à l’ordinateur sur lequel l’alerte a été déclenchée pour exécuter cette tâche dans l’Afficheur des alertes.|  
|Mises à jour importantes doivent être installés.|Vous ne pourrez pas bénéficier des fonctionnalités nouvelles et corrigées des mises à jour, sauf si la mise à jour de Windows est activé. Pour activer Windows Update, dans l’Afficheur des alertes, cliquez sur **ouvrir Windows Update**.<br /><br /> Si le **ouvrir Windows Update** tâche n’est pas affichée, vous n’êtes pas connecté à l’ordinateur sur lequel l’alerte a été déclenchée. Vous devez être connecté à l’ordinateur sur lequel l’alerte a été déclenchée pour exécuter cette tâche dans l’Afficheur des alertes.|  
|Redémarrez l’ordinateur pour appliquer les mises à jour.|Vous ne serez pas en mesure de bénéficier des fonctionnalités nouvelles et corrigées des mises à jour jusqu'à ce qu’elles sont appliquées. Enregistrez toutes vos données et redémarrez l’ordinateur pour appliquer les mises à jour.|  
|Espace libre est faible sur les disques durs.|Si l’espace n’est pas accessible, vous ne serez pas en mesure d’enregistrer des informations supplémentaires. Pour augmenter l’espace disponible sur l’ordinateur, envisagez les options suivantes:<br /><br /> -Ajouter un nouveau disque dur.<br /><br /> -Exécutez **nettoyage de disque** pour supprimer les fichiers anciens et temporaires.<br /><br /> : Permet de déplacer vos fichiers vers un dossier partagé sur un autre ordinateur.<br /><br /> -Les fichiers d’archive sur un support amovible, par exemple, un CD, DVD ou un disque dur externe.|  
|Le **l’historique des fichiers** agent sur le serveur n’est pas correctement configuré pour s’exécuter sur cet ordinateur.|Les sauvegardes de l’historique des fichiers ne peuvent pas être créés.|  
|Un ou plusieurs services ne sont pas en cours d’exécution.||  
|Modifier votre mot de passe Windows.||  
|Votre mot de passe Microsoft Office 365 n’est pas identique à votre mot de passe Windows.||  
  
###  <a name="BKMK_Protect"></a>Pour protéger votre ordinateur  
  
1.  Ouvrir le centre de sécurité.  
  
2.  Déterminer l’état de la protection antivirus installée.  
  
3.  Effectuez l’une des tâches suivantes en fonction de l’état de protection:  
  
    -   Si elle n’est pas activée, activez-la.  
  
    -   Si elle n’est pas à jour, effectuez la mise à jour les signatures.  
  
    -   Si la protection antivirus n’est pas installée, envisagez de l’installer.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Gérer WindowsServerEssentials](Manage-Windows-Server-Essentials.md)
