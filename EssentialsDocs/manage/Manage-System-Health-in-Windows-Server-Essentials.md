---
title: Gérer l'intégrité du système dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 3043f83b-389c-4f37-a1ff-85afe99314fa
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 8bca8f89e876da56dc6ede53a017e4d4331e39fc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852712"
---
# <a name="manage-system-health-in-windows-server-essentials"></a>Gérer l'intégrité du système dans Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Cette rubrique explique comment afficher et répondre à toutes les alertes de votre réseau à l'aide du Tableau de bord.  
  
> [!NOTE]
>  Dans Windows Server Essentials et Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials installé, les alertes d’intégrité pour le serveur et les ordinateurs clients du réseau ne sont plus affichées dans l’afficheur des alertes, mais elles peuvent être affichées à la place sous l’onglet **rapports d’intégrité** de la page d' **hébergement** .  
  
 Windows Server Essentials surveille activement chaque ordinateur connecté au serveur et avertit l’administrateur des problèmes liés à l’intégrité du système, notamment les mises à jour critiques, la protection contre les programmes malveillants, les définitions de virus obsolètes sur les ordinateurs clients et d’autres problèmes importants qui nécessitent une action. Ces problèmes sont affichés sous forme d’alertes dans l’afficheur des alertes, qui peut être lancé à partir du tableau de bord du serveur ou du Launchpad de l’ordinateur client dans Windows Server Essentials, ou sous l’onglet **rapports d’intégrité** dans Windows Server Essentials. Par défaut, les alertes sont actualisées toutes les 30 minutes, mais vous pouvez connaître l'état de votre réseau à tout moment en cliquant sur **Actualiser** dans l'Afficheur des alertes ou sous l'onglet **Rapports d'intégrité**.  
  
 Les rubriques suivantes vous aideront à comprendre, afficher et répondre aux alertes dans l'Afficheur des alertes et fournissent également des instructions pour configurer votre serveur de sorte de recevoir des notifications d'alerte par courrier électronique :  
  
-   [À propos du complément de rapport d’intégrité](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_AddIn)  
  
-   [Afficher les alertes à l’aide de l’afficheur des alertes](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_View)  
  
-   [Organiser les alertes dans l’afficheur des alertes](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Organize)  
  
-   [Répondre aux alertes](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Respond)  
  
-   [Configurer des notifications par courrier électronique pour les alertes](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email)  
  
-   [Alertes d’ordinateur potentielles](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Potential)  
  
##  <a name="about-the-health-report-add-in"></a><a name="BKMK_AddIn"></a>À propos du complément de rapport d’intégrité  
 Le complément de rapport d'intégrité pour Windows Server Essentials vous fournit des informations consolidées sur le réseau de Windows Server Essentials et vous permet de distribuer ces informations à d'autres personnes. Ces informations peuvent être affichées sous l'onglet **Rapports** du Tableau de bord. Avec l'onglet **Rapports**, vous pouvez effectuer les opérations suivantes :  
  
-   [Générer un rapport à la demande ou selon la planification](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Generate)  
  
-   [Personnaliser le contenu du rapport](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Customize)  
  
-   [Envoyer le rapport par courrier électronique](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_emailreport)  
  
> [!NOTE]
>  **Windows Server Essentials :** Vous pouvez télécharger le complément de rapport d’intégrité pour Windows Server Essentials à partir du [Centre de téléchargement Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=266342).  
>   
>  **Windows Server Essentials :** Par défaut, le complément de rapport d’intégrité est intégré à Windows Server Essentials ou Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials installé, et les rapports d’intégrité sont affichés sous l’onglet **rapports d’intégrité** de la page d' **espace** du tableau de bord.  
  
###  <a name="generate-a-report-on-demand-or-on-schedule"></a><a name="BKMK_Generate"></a>Générer un rapport à la demande ou selon la planification  
 Après avoir installé le complément de rapport d'intégrité et redémarré le Tableau de bord, un nouvel onglet, **Rapports** est ajouté au Tableau de bord. Vous pouvez générer un rapport d'intégrité à la demande à tout moment en cliquant sur la tâche **Générer un rapport d'intégrité** sous l'onglet **Rapports**.  
  
 Lorsqu'un rapport d'intégrité a été généré, un nouvel élément est créé dans le volet de la liste, identifié par la date et l'heure à laquelle le rapport a été généré. Pour ouvrir un élément, vous pouvez double-cliquer dessus dans le volet de la liste, ou le sélectionner puis cliquer sur **Ouvrir le rapport d'intégrité** dans le volet des tâches. Le rapport s'affiche dans une nouvelle fenêtre au format HTML.  
  
 En plus de générer manuellement un rapport, vous pouvez souhaiter que celui-ci soit généré automatiquement selon un calendrier quotidien ou haire. Pour ce faire, dans le volet des tâches, cliquez sur **personnaliser les paramètres du rapport d’intégrité**, puis cliquez sur l’onglet **planifier et envoyer par courrier électronique** . La fonctionnalité de **planification** est désactivée par défaut et vous pouvez l’activer en cochant la case **générer un rapport d’intégrité à l’heure planifiée** .  
  
###  <a name="customize-the-content-of-the-report"></a><a name="BKMK_Customize"></a>Personnaliser le contenu du rapport  
 Le rapport d'intégrité contient les éléments suivants :  
  
- **Alertes critiques et avertissements** Conformes aux alertes critiques et avertissements qui apparaissent dans l'Afficheur des alertes du Tableau de bord. Les alertes d'informations ne sont pas incluses dans le rapport d'intégrité.  
  
- **Erreurs critiques dans les journaux des événements** Les applications et les journaux de service sont analysés et les erreurs qui ont été consignées au cours des dernières 24 heures s'afficheront dans la section **Détails** du rapport.  
  
- **Sauvegarde du serveur** Les informations sur la dernière sauvegarde du serveur sont présentées dans la section **Détails** du rapport.  
  
- **Services à démarrage automatique ne fonctionnant pas** Au moment où le rapport est généré, si un service à démarrage automatique ne fonctionne pas, les informations sur ce service apparaissent dans la section **Détails** du rapport.  
  
- **Mises à jour** Vous pouvez consulter l'état de mise à jour du serveur et de tous les ordinateurs clients dans la section **Détails**.  
  
- **Stockage** La liste des lecteurs et leur capacité est présentée dans la section **Détails** .  
  
  Dans le rapport d'intégrité, consultez d'abord le **Résumé** puis, pour les éléments signalés par une icône d'erreur rouge ou une icône d'avertissement jaune, cliquez sur le lien **Détails** sur la même ligne pour afficher les détails concernant l'élément.  
  
  Si vous n’êtes pas intéressé par certains des points de données qui sont inclus dans le rapport par défaut, vous pouvez personnaliser le contenu du rapport en cliquant sur **personnaliser les paramètres du rapport d’intégrité** dans le volet des tâches, puis sur l’onglet **contenu** . désactivez les cases à cocher du contenu que vous ne souhaitez pas voir dans le rapport. Par exemple, si vous disposez de votre propre plan de sauvegarde du serveur et que vous ne souhaitez pas voir les avertissements concernant les sauvegardes du serveur, vous pouvez exclure les sauvegardes du serveur du rapport en désactivant la case à cocher **sauvegarde** du serveur.  
  
###  <a name="email-the-report"></a><a name="BKMK_emailreport"></a>Envoyer le rapport par courrier électronique  
 Ouvrir une session de Tableau de bord pour lire des rapports n'est pas toujours pratique pour certains administrateurs, en particulier s'ils ont plusieurs serveurs à gérer. Avec la fonctionnalité de messagerie activée, dès qu'un rapport est généré, un courrier électronique est envoyé à une liste d'adresses de messagerie spécifiée avec le contenu du rapport. L'administrateur peut facilement consulter ce rapport depuis n'importe quel périphérique ou application client et vérifier que le serveur fonctionne de manière optimale.  
  
 Dans la boîte de dialogue **Personnaliser les paramètres du rapport d’intégrité**, quand vous avez activé la messagerie électronique, modifié les paramètres SMTP et spécifié une liste de destinataires, vous remarquerez qu’une nouvelle tâche, **Envoyer le rapport d’intégrité par courrier électronique**, s’affiche dans le volet des tâches. Pour plus d'informations sur les paramètres SMTP, voir [Configurer la notification des alertes par courrier électronique](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email).  
  
 Vous pouvez sélectionner un rapport existant, puis cliquer sur **Envoyer le rapport d'intégrité par courrier électronique**. Vous pouvez également générer un rapport et paramétrer son envoi automatique dans votre boîte de réception. Si vous avez configuré une planification de génération automatique du rapport, celui-ci sera déposé automatiquement dans votre boîte de réception après avoir été généré chaque jour (ou chaque heure) comme prévu.  
  
##  <a name="view-alerts-by-using-the-alert-viewer"></a><a name="BKMK_View"></a>Afficher les alertes à l’aide de l’afficheur des alertes  
 Cette section explique comment utiliser le Tableau de bord ou le Launchpad pour ouvrir l'Afficheur des alertes afin de consulter l'état d'intégrité de tous les ordinateurs du réseau du serveur.  
  
#### <a name="to-open-the-alert-viewer-by-using-the-dashboard"></a>Pour ouvrir l'Afficheur des alertes à l'aide du Tableau de bord  
  
1.  Ouvrez le Tableau de bord.  
  
2.  Dans le volet de navigation, cliquez sur une des icônes d'alerte affichées (critique, avertissement ou information). L'Afficheur des alertes s'ouvre.  
  
#### <a name="to-open-the-alert-viewer-from-the-launchpad"></a>Pour ouvrir l'Afficheur des alertes à partir du Launchpad  
  
1.  À partir de l'ordinateur connecté au serveur, ouvrez le Launchpad. Si vous y êtes invité, connectez-vous à Launchpad avec votre nom d'utilisateur et votre mot de passe.  
  
2.  Cliquez sur un des icônes d'alerte affichées (critique, avertissement ou information) en bas du Launchpad pour ouvrir l'Afficheur des alertes, puis suivez les instructions dans le volet d'informations de l'Afficheur des alertes pour résoudre l'alerte.  
  
##  <a name="organize-alerts-in-the-alert-viewer"></a><a name="BKMK_Organize"></a>Organiser les alertes dans l’afficheur des alertes  
 Vous pouvez organiser des alertes dans l'Afficheur des alertes et les afficher selon leur gravité (critique, avertissement ou information) ou selon le nom de l'ordinateur.  
  
#### <a name="to-organize-alerts-in-the-alert-viewer"></a>Pour organiser les alertes dans l'Afficheur des alertes  
  
1.  Ouvrez le Tableau de bord.  
  
2.  Dans le volet de navigation, cliquez sur une des icônes d'alerte affichées (critique, avertissement ou information). L'Afficheur des alertes s'ouvre.  
  
3.  Développez la liste déroulante **Organiser**, effectuez l'une des opérations suivantes :  
  
    1.  Sélectionnez **Filtrer par ordinateur**, puis cliquez sur le nom de l'ordinateur pour lequel vous souhaitez afficher les alertes. Cela permet de n'afficher que les alertes de l'ordinateur sélectionné dans l'Afficheur des alertes.  
  
    2.  Sélectionnez **Filtrer par type d'alerte**, puis cliquez sur le type d'alerte (critique, avertissement ou information) pour lequel vous souhaitez afficher les alertes. Cela permet de n'afficher que les alertes du type sélectionné dans l'Afficheur des alertes.  
  
##  <a name="respond-to-alerts"></a><a name="BKMK_Respond"></a>Répondre aux alertes  
 Lorsque vous rencontrez une alerte, vous pouvez choisir d'effectuer l'une des opérations suivantes :  
  
-   [Résoudre une alerte](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Resolve)  
  
-   [Ignorer une alerte](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Activer une alerte](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Supprimer une alerte](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_4)  
  
###  <a name="resolve-an-alert"></a><a name="BKMK_Resolve"></a>Résoudre une alerte  
 Suivez les instructions de résolution dans l'Afficheur des alertes pour résoudre l'alerte. Une fois qu'une alerte est résolue, elle reste affichée dans l'Afficheur des alertes jusqu'à ce qu'il soit actualisé.  
  
###  <a name="ignore-an-alert"></a><a name="BKMK_3"></a>Ignorer une alerte  
 Vous pouvez choisir d'ignorer une alerte si vous préférez y répondre ultérieurement. Lorsque vous ignorez une alerte, elle reste affichée dans l'Afficheur des alertes, mais apparaît désactivée et grisée. Une alerte ignorée n'est pas incluse dans l'évaluation d'intégrité globale de l'ordinateur. Pour répondre à une alerte ignorée, vous devez d'abord activer l'alerte.  
  
##### <a name="to-ignore-an-alert"></a>Pour ignorer une alerte  
  
1. À partir de l'ordinateur qui est connecté au serveur Windows Server Essentials, ouvrez le Launchpad.  
  
2. Dans le Launchpad, cliquez sur l'une des icônes d'alerte affichées (critique, avertissement et information). L'Afficheur des alertes s'ouvre.  
  
3. Dans l'Afficheur des alertes, sélectionnez l'alerte que vous voulez ignorer, puis dans la section **Tâches**, cliquez sur **Ignorer l'alerte**.  
  
   Pour répondre à une alerte désactivée, vous devez d'abord activer l'alerte.  
  
###  <a name="enable-an-alert"></a><a name="BKMK_5"></a>Activer une alerte  
 Vous pouvez activer une alerte que vous avez choisi d'ignorer précédemment. Une fois que l'alerte est activée, vous pouvez la résoudre ou la supprimer selon les besoins. Une alerte apparaît désactivée quand elle est marquée comme devant être ignorée. Lorsque vous activez une alerte précédemment désactivée, elle devient active et se réintègre à l'évaluation d'intégrité globale des ordinateurs.  
  
##### <a name="to-enable-an-alert"></a>Pour activer une alerte  
  
1.  À partir de l'ordinateur connecté au serveur, ouvrez le Launchpad.  
  
2.  Dans le Launchpad, cliquez sur l'une des icônes d'alerte affichées (critique, avertissement et information) pour ouvrir l'Afficheur des alertes.  
  
3.  Dans l'Afficheur des alertes, cliquez avec le bouton droit sur l'alerte que vous voulez activer, puis cliquez sur **Activer l'alerte**.  
  
###  <a name="delete-an-alert"></a><a name="BKMK_4"></a>Supprimer une alerte  
 Vous pouvez supprimer une alerte si vous ne souhaitez pas la résoudre ou l'ignorer. Vous pouvez utiliser l'Afficheur des alertes dans le Launchpad pour supprimer les alertes générées pour votre ordinateur. Si vous supprimez une alerte et que le serveur détecte à nouveau le problème dans le cycle d'évaluation de l'intégrité du réseau suivant, il génère une nouvelle alerte.  
  
##### <a name="to-delete-an-alert"></a>Pour supprimer une alerte  
  
1.  À partir de l'ordinateur connecté au serveur, ouvrez le Launchpad.  
  
2.  Dans le Launchpad, cliquez sur l'une des icônes d'alerte affichées (critique, avertissement et information) pour ouvrir l'Afficheur des alertes.  
  
3.  Dans l'Afficheur des alertes, cliquez avec le bouton droit sur l'alerte que vous voulez supprimer, puis cliquez sur **Supprimer l'alerte**.  
  
##  <a name="set-up-email-notifications-for-alerts"></a><a name="BKMK_Email"></a>Configurer des notifications par courrier électronique pour les alertes  
 Vous pouvez configurer votre serveur pour être averti par courrier électronique de l'occurrence des alertes. Les notifications par courrier électronique de ces alertes contiennent des informations sur les problèmes de réseau et leur résolution, qui sont identiques à celles affichées dans l'Afficheur des alertes. Certaines des évaluations d'intégrité du réseau sont effectuées par programme.  
  
 Lorsque vous configurez votre serveur pour envoyer des notifications par courrier électronique, une notification est ainsi envoyée pour les alertes détectés lors de l'évaluation de l'intégrité du réseau. Toutefois, toutes les alertes signalées dans l'Afficheur des alertes ne sont pas notifiées par courrier électronique.  
  
 Toutes les 30 minutes, la tâche d'alerte d'évaluation de messagerie s'exécute sur le serveur, qui recense les alertes actives sur le réseau. Une notification par courrier électronique est envoyée dès qu'une alerte définie pour une notification par courrier électronique se produit. Si l'alerte est toujours active lors du cycle d'évaluation suivant, un deuxième courrier électronique ne sera pas envoyé pour éviter d'encombrer votre boîte aux lettres. Toutefois, si une nouvelle alerte est détectée lors d'un cycle d'évaluation ultérieur, une notification par courrier électronique est envoyée, qui inclut à la fois les alertes anciennes et nouvelles.  
  
###  <a name="alerts-that-result-in-email-notifications"></a><a name="BKMK_list"></a>Alertes qui génèrent des notifications par courrier électronique  
 Les alertes figurant dans l'Afficheur des alertes produisent des notifications par courrier électronique lorsque vous configurez votre serveur pour notifier les alertes par courrier électronique :  
  
-   Une sauvegarde d'ordinateur de client contient des erreurs.  
  
-   Le serveur a été redémarré.  
  
-   Un ou plusieurs services ne sont pas en cours d'exécution.  
  
-   Le service de stockage Windows Server ne fonctionne pas.  
  
-   Votre période d'évaluation a expiré.  
  
-   Activer maintenant.  
  
-   Arrêt du serveur source.  
  
-   Les résultats d'analyse BPA contiennent des erreurs.  
  
-   Les résultats d'analyse BPA contiennent des avertissements.  
  
-   Aucun certificat n'est disponible pour l'Accès en tout lieu.  
  
-   Le certificat d'Accès en tout lieu est arrivé à expiration.  
  
-   Le routeur n'est pas correctement configuré.  
  
-   Le serveur Web n'est pas correctement configuré.  
  
-   Les services Bureau à distance ne sont pas correctement configurés.  
  
-   Le pare-feu n'est pas correctement configuré.  
  
-   Le nom de domaine Internet a expiré.  
  
-   Impossible de mettre à jour le nom de domaine Internet.  
  
-   Erreur de licence : vérification de l’approbation de la forêt.  
  
-   Erreur de licence : vérification du contrôleur de domaine.  
  
-   Erreur de licence : vérification du rôle FSMO.  
  
-   Erreur de licence : stratégies FSMO de contrôle.  
  
-   Erreur de licence : stratégies de chargement de contrôle.  
  
-   Erreur de licence : services de domaine Active Directory  
  
-   Votre abonnement Office 365 a expiré.  
  
-   Échec de l'authentification d'Office 365.  
  
-   La stratégie de mot de passe n'est pas correcte.  
  
-   Le service de synchronisation de mot de passe ne peut pas synchroniser un mot de passe utilisateur avec Office 365.  
  
-   Modifiez votre mot de passe Windows.  
  
-   Votre mot de passe Office 365 n'est pas identique à votre mot de passe Windows.  
  
-   Impossible de se connecter à Exchange Server.  
  
-   Microsoft Exchange Server a rencontré un problème.  
  
-   Un ou plusieurs des disques durs de sauvegarde du serveur ne sont pas connectés.  
  
-   Le disque dur de sauvegarde n'a pas suffisamment d'espace libre pour la sauvegarde du serveur.  
  
-   La sauvegarde du serveur a échoué en raison de l'impossibilité d'effectuer une capture instantanée du lecteur  
  
-   Une sauvegarde planifiée ne s'est pas terminée correctement.  
  
-   Un ou plusieurs dossiers serveur prédéfinis manquent.  
  
-   Espace libre faible dans un ou plusieurs disques durs du serveur.  
  
-   L'enregistreur VSS pour le service de stockage ne fonctionne pas.  
  
-   La capacité de stockage des disques durs est faible.  
  
-   Un ou plusieurs disques durs ne fonctionnent pas et sont en mode hors-ligne.  
  
###  <a name="configuring-smtp-on-your-server-to-send-alert-notifications-by-email-in-windows-server-essentials"></a><a name="BKMK_SMTP"></a>Configuration de SMTP sur votre serveur pour envoyer des notifications d’alerte par courrier électronique dans Windows Server Essentials  
 Cette section explique comment configurer votre serveur pour notifier les alertes par courrier électronique.  
  
> [!NOTE]
>  Vous pouvez télécharger le complément de rapport d’intégrité pour Windows Server Essentials à partir du [Centre de téléchargement Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=266342).  
  
##### <a name="to-set-up-email-notification-for-alerts"></a>Pour configurer des notifications des alertes par courrier électronique  
  
1.  À partir du **Tableau de bord**, ouvrez l'**Afficheur des alertes**.  
  
2.  Dans l'**Afficheur des alertes**, cliquez sur **Configurer la notification des alertes par courrier électronique**.  
  
3.  Dans la fenêtre **Configurer les notifications par courrier électronique pour les alertes**, cliquez sur **Activer**.  
  
4.  Dans la fenêtre **Paramètres SMTP**, procédez comme suit :  
  
    1.  Pour **Adresse de l'expéditeur**, tapez l'adresse de messagerie que vous souhaitez utiliser pour envoyer le courrier électronique à partir des alertes. Cette adresse de messagerie s’affichera en tant qu’adresse de l’expéditeur dans la notification d’alerte.  
  
    2.  Pour **Nom du serveur SMTP**, dans la zone de texte **Adresse de l'expéditeur**, tapez le nom du serveur SMTP que vous avez spécifié à l'étape 4a. (Consultez le tableau 1 pour une liste de certains noms de serveur SMTP).  
  
    3.  Pour **Port SMTP**, tapez le numéro de port utilisé par le serveur SMTP pour envoyer et recevoir du courrier électronique. (Consultez le tableau 1 pour les numéros de port utilisés par certains des serveurs SMTP).  
  
    4.  Sélectionnez **Ce serveur exige une connexion sécurisée (SSL)** si le serveur SMTP utilise le protocole SSL (voir tableau 1).  
  
    5.  Sélectionnez **Ce serveur exige une authentification** si le serveur SMTP nécessite des informations de nom d'utilisateur et de mot de passe (voir tableau 1). Si vous cochez cette case, tapez le nom d'utilisateur et le mot de passe de l'adresse de messagerie que vous avez entrée dans le champ **Adresse de l'expéditeur** de l'étape 4a, puis cliquez sur **OK**.  
  
    > [!NOTE]
    >  Vous pouvez obtenir les informations sur le nom du serveur SMTP, le numéro de port et l'utilisation SSL à partir de votre fournisseur de services Internet.  
  
     **Tableau 1** Exemples de conditions requises en matière de noms de serveur SMTP, d'authentification, de chiffrement SSL et de numéros de port  
  
    |SMTP Server|SSL requis|Authentification obligatoire|Numéro de port|Nom du compte/Nom de connexion|  
    |-----------------|------------------|-----------------------------|-----------------|------------------------------|  
    |smtp.gmail.com|Oui|Oui|587|Fournir une adresse de messagerie complète avec le nom de domaine et le mot de passe pour l'authentification.|  
    |smtp.live.com|Oui|Oui|587|Fournir une adresse de messagerie complète avec le nom de domaine et le mot de passe pour l'authentification.|  
    |smtp.comcast.net|Oui|Non|587|Fournir une adresse de messagerie complète avec le nom de domaine et le mot de passe pour l'authentification.|  
    |smtp.mail.yahoo.com|Non|Oui|25|Fournir uniquement l'adresse de messagerie sans nom de domaine pour le nom d'utilisateur.|  
  
5.  Dans **Configurer les notifications pour les alertes**, pour **Destinataires du courrier électronique**, tapez les adresses de messagerie des personnes dont vous souhaitez recevoir les notifications d'alerte par courrier électronique. Séparez chaque adresse de messagerie par un point-virgule (;).  
  
6.  Pour vérifier que vous avez configuré vos paramètres correctement pour notifier les alertes par courrier électronique, cliquez sur **Appliquer et envoyer un courrier électronique**.  
  
    > [!NOTE]
    >  En cliquant sur **Appliquer et envoyer un courrier électronique**, vous recevrez généralement un exemple de notification par courrier électronique sans aucune alerte d'intégrité répertoriée. Toutefois, si une alerte d'intégrité configurée pour être notifiée par courrier électronique est identifiée au cours de ce processus de test, cette alerte sera incluse dans le courrier électronique de test.  
  
### <a name="configuring-smtp-on-your-server-to-send-health-reports-in-windows-server-essentials"></a>Configuration du protocole SMTP sur votre serveur pour envoyer des rapports d’intégrité dans Windows Server Essentials  
 Cette section explique comment configurer les paramètres SMTP pour votre serveur afin de recevoir des rapports d'intégrité par courrier électronique.  
  
> [!NOTE]
>  Par défaut, le complément de rapport d’intégrité est intégré à Windows Server Essentials ou Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials installé, et les rapports d’intégrité sont affichés sous l’onglet **rapports d’intégrité** de la page d' **espace** du tableau de bord.  
  
##### <a name="to-set-up-email-notification-for-health-reports"></a>Pour configurer la notification des alertes par courrier électronique  
  
1.  Dans le **Tableau de bord**, cliquez sur l'onglet **Rapports**.  
  
2.  Dans le volet des tâches **Tâches du rapport d'intégrité**, cliquez sur **Personnaliser les paramètres du rapport d'intégrité**.  
  
3.  Dans la fenêtre **Personnaliser les paramètres du rapport d'intégrité**, cliquez sur l'onglet **Planifier et envoyer par courrier électronique**.  
  
4.  Dans l'onglet **Planifier et envoyer par courrier électronique**, dans la section **Courrier électronique**, procédez comme suit :  
  
    1.  Cliquez sur **Activer**, puis tapez l'adresse de messagerie que vous souhaitez utiliser pour envoyer les rapports d'intégrité. Cette adresse de messagerie s’affichera en tant qu’adresse de l’expéditeur dans les rapports d’intégrité envoyés par courrier électronique.  
  
        1.  Pour **Nom du serveur SMTP**, tapez le nom du serveur SMTP. (Consultez le tableau 1 pour une liste de certains noms de serveur SMTP).  
  
        2.  Pour **Port SMTP**, tapez le numéro de port utilisé par le serveur SMTP pour envoyer et recevoir du courrier électronique. (Consultez le tableau 1 pour les numéros de port utilisés par certains des serveurs SMTP).  
  
        3.  Sélectionnez **Ce serveur exige une connexion sécurisée (SSL)** si le serveur SMTP utilise le protocole SSL (voir tableau 1).  
  
        4.  Sélectionnez **Ce serveur exige une authentification** si le serveur SMTP nécessite des informations de nom d'utilisateur et de mot de passe (voir tableau 1). Si vous cochez cette case, tapez le nom d'utilisateur et le mot de passe de l'adresse de messagerie que vous avez entrée dans le champ **Adresse de l'expéditeur** de l'étape 4a, puis cliquez sur **OK**.  
  
            > [!NOTE]
            >  Vous pouvez obtenir les informations sur le nom du serveur SMTP, le numéro de port et l'utilisation SSL à partir de votre fournisseur de services Internet.  
  
            > [!NOTE]
            >  Microsoft vous recommande vivement d'utiliser SSL car le rapport peut contenir une information sur l'état du serveur qui pourrait être utilisée par des utilisateurs malveillants pour détecter des vulnérabilités (par exemple : une mise à jour manquante de Windows). L'activation de SSL chiffre les données en transit et atténue le risque d'exposer les vulnérabilités du serveur.  
  
5.  Après avoir activé la messagerie électronique, le lien **Modifier les paramètres SMTP** s'affiche. Par ailleurs, une nouvelle tâche, **Envoyer le rapport d'intégrité par courrier électronique**, s'affiche dans **Tâches du rapport d'intégrité**.  
  
     **Tableau 1** Exemples de conditions requises en matière de noms de serveur SMTP, d'authentification, de chiffrement SSL et de numéros de port  
  
    |SMTP Server|SSL requis|Authentification obligatoire|Numéro de port|Nom du compte/Nom de connexion|  
    |-----------------|------------------|-----------------------------|-----------------|------------------------------|  
    |smtp.gmail.com|Oui|Oui|587|Fournir une adresse de messagerie complète avec le nom de domaine et le mot de passe pour l'authentification.|  
    |smtp.live.com|Oui|Oui|587|Fournir une adresse de messagerie complète avec le nom de domaine et le mot de passe pour l'authentification.|  
    |smtp.comcast.net|Oui|Non|587|Fournir une adresse de messagerie complète avec le nom de domaine et le mot de passe pour l'authentification.|  
    |smtp.mail.yahoo.com|Non|Oui|25|Fournir uniquement l'adresse de messagerie sans nom de domaine pour le nom d'utilisateur.|  
  
6.  Dans **Personnaliser les paramètres du rapport d'intégrité**, pour **Envoyer automatiquement le rapport d'intégrité aux destinataires de courrier électronique suivants :** , tapez les adresses de messagerie des personnes dont vous souhaitez recevoir les rapports d'intégrité par courrier électronique. Séparez chaque adresse de messagerie par un point-virgule (;).  
  
7.  Pour vérifier que vous avez correctement configuré les paramètres de votre serveur SMTP de sorte d'envoyer les rapports d'intégrité par courrier électronique, dans l'onglet Rapport d'intégrité du Tableau de bord, sélectionnez un rapport, puis cliquez sur **Envoyer le rapport d'intégrité par courrier électronique** dans le volet des tâches.  
  
##  <a name="potential-computer-alerts"></a><a name="BKMK_Potential"></a>Alertes d’ordinateur potentielles  
 Cette section décrit le fonctionnement et la gestion des alertes spécifiques à votre ordinateur qui est connecté au serveur et qui apparaissent dans le Launchpad de votre ordinateur.  
  
 Le tableau suivant répertorie certaines des alertes d'ordinateur qui peuvent être générées et affichées dans l'Afficheur des alertes si elles sont applicables à votre ordinateur.  
  
|Titre de l'alerte|Impact et résolution de l'alerte|  
|-----------------|---------------------------------|  
|L'état actuel du Pare-feu du réseau offre une protection réduite à cet ordinateur.|Des personnes ou des logiciels non autorisés peuvent accéder à cet ordinateur si le pare-feu Windows n'est pas activé.|  
|La protection antivirus est désactivée, non installée ou périmée|Les données sur votre ordinateur sont exposées si le paramètre de sécurité **Protection antivirus** est désactivé ou pas à jour. [To protect your computer](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Protect), suivez les étapes indiquées.|  
|La protection contre les logiciels espions et les logiciels indésirables est désactivée, non installée ou périmée.|Les données sur votre ordinateur sont exposées si le paramètre de sécurité **Protection contre les logiciels espions et indésirables** est désactivé ou périmé. [To protect your computer](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Protect), suivez les étapes indiquées.|  
|Windows Update est désactivé.|Vous ne pourrez pas bénéficier des fonctionnalités nouvelles et corrigées des mises à jour, sauf si Windows Update est activé. Pour activer Windows Update, dans l'Afficheur des alertes, cliquez sur **Ouvrir Windows Update**.<br /><br /> Si la tâche **Ouvrir Windows Update** ne s'affiche pas, vous n'êtes pas connecté à l'ordinateur sur lequel l'alerte a été déclenchée. Vous devez être connecté à l'ordinateur sur lequel l'alerte a été déclenchée pour exécuter cette tâche dans l'Afficheur des alertes.|  
|D'importantes mises à jour doivent être installées.|Vous ne pourrez pas bénéficier des fonctionnalités nouvelles et corrigées des mises à jour, sauf si Windows Update est activé. Pour activer Windows Update, dans l'Afficheur des alertes, cliquez sur **Ouvrir Windows Update**.<br /><br /> Si la tâche **Ouvrir Windows Update** ne s'affiche pas, vous n'êtes pas connecté à l'ordinateur sur lequel l'alerte a été déclenchée. Vous devez être connecté à l'ordinateur sur lequel l'alerte a été déclenchée pour exécuter cette tâche dans l'Afficheur des alertes.|  
|Redémarrez l'ordinateur pour appliquer les mises à jour.|Vous ne pourrez pas bénéficier des fonctionnalités nouvelles et corrigées des mises à jour tant qu'elles ne seront pas appliquées. Enregistrez toutes vos données et redémarrez l'ordinateur pour appliquer les mises à jour.|  
|Espace libre faible sur les disques durs.|Si de l'espace n'est pas libéré, vous ne serez pas en mesure d'enregistrer des informations supplémentaires. Pour augmenter l'espace disponible sur l'ordinateur, envisagez les options suivantes :<br /><br /> -Ajoutez un nouveau disque dur.<br /><br /> -Exécuter le **nettoyage de disque** pour supprimer les fichiers anciens et temporaires.<br /><br /> -Déplacez vos fichiers vers un dossier partagé sur un autre ordinateur.<br /><br /> -Archivez les fichiers sur un support amovible, tel qu’un CD, un DVD ou un disque dur externe.|  
|L'agent **Historique des fichiers** sur le serveur n'est pas correctement configuré pour s'exécuter sur cet ordinateur.|Impossible de créer des sauvegardes de l'historique des fichiers.|  
|Un ou plusieurs services ne sont pas en cours d'exécution.||  
|Modifiez votre mot de passe Windows.||  
|Votre mot de passe Microsoft Office 365 n'est pas identique à votre mot de passe Windows.||  
  
###  <a name="to-protect-your-computer"></a><a name="BKMK_Protect"></a>Pour protéger votre ordinateur  
  
1.  Ouvrez le Centre de sécurité.  
  
2.  Déterminez l'état de la protection antivirus installée.  
  
3.  Effectuez l'une des tâches suivantes en fonction de l'état de protection :  
  
    -   Si elle n'est pas activée, activez-la.  
  
    -   Si elle n'est pas à jour, procédez à la mise à jour des signatures.  
  
    -   Si la protection antivirus n'est pas installée, envisagez de l'installer.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Gérer Windows Server Essentials](Manage-Windows-Server-Essentials.md)
