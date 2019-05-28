---
title: Résoudre les problèmes de profils utilisateur présentant des événements
description: Comment résoudre les problèmes de chargement et déchargement de profils utilisateur à l’aide des journaux de suivi et des événements.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: f30bfcd531731e3a0d14350536ddf418c50f3ea0
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475947"
---
# <a name="troubleshoot-user-profiles-with-events"></a>Résoudre les problèmes de profils utilisateur présentant des événements

>S’applique à : Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 et Windows Server (canal semi-annuel).

Cette rubrique explique comment résoudre les problèmes de chargement et déchargement des profils utilisateur à l’aide des journaux de suivi et des événements. Les sections suivantes décrivent comment utiliser les trois journaux des événements qui enregistrent les informations de profil utilisateur.

## <a name="step-1-checking-events-in-the-application-log"></a>Étape 1 : Vérification des événements dans le journal des applications

La première étape dans la résolution des problèmes avec le chargement et déchargement utilisateur profils (y compris les profils utilisateur itinérants) consiste à utiliser l’Observateur d’événements pour examiner tous les événements avertissement et d’erreur du journal des enregistrements de Service de profil utilisateur dans l’Application.

Voici comment afficher les événements des Services de profil utilisateur dans le journal des applications :

1. Démarrez l’Observateur d’événements. Pour ce faire, ouvrez **le panneau de configuration**, sélectionnez **système et sécurité**, puis, dans le **outils d’administration** section, sélectionnez **afficher les journaux des événements**. La fenêtre de l’Observateur d’événements s’ouvre.
2. Dans l’arborescence de la console, accédez tout d’abord à **les journaux Windows**, puis **Application**.
3. Dans le volet Actions, sélectionnez **filtrer le journal actuel**. La boîte de dialogue Filtrer le journal actuel s’ouvre.
4. Dans le **sources d’événements** boîte, sélectionnez le **Service de profils utilisateur** case à cocher, puis sélectionnez **OK**.
5. Passez en revue la liste des événements, en faisant particulièrement attention aux événements d’erreur.
6. Lorsque vous trouvez des événements dignes d’intérêt, sélectionnez le lien aide en ligne du journal des événements pour afficher des informations supplémentaires et des procédures de dépannage.
7. Pour effectuer la résolution des problèmes, notez la date et l’heure des événements dignes d’intérêt et examinez le journal des opérations (comme décrit à l’étape 2) pour afficher des détails sur ce que le Service de profil utilisateur a été fait au moment des événements d’erreur ou un avertissement.

>[!NOTE]
>Vous pouvez ignorer en toute sécurité de Service de profil utilisateur événement 1530 « Windows détecté que votre fichier de Registre est en cours d’utilisation par d’autres applications ou services. »

## <a name="step-2-view-the-operational-log-for-the-user-profile-service"></a>Étape 2 : Afficher le journal des opérations pour le Service de profil utilisateur

Si vous ne pouvez pas résoudre le problème avec le journal d’Application autonome, procédez comme suit pour afficher les événements de Service de profil utilisateur dans le journal des opérations. Ce journal répertorie certaines du fonctionnement interne du service et peut aider à identifier où dans la charge de profil ou décharger le processus que le problème se produit.

Le journal des applications de Windows et le journal des opérations de Service de profil utilisateur sont activées par défaut dans toutes les installations de Windows.

Voici comment afficher le journal des opérations pour le Service de profil utilisateur :

1. Dans l’arborescence de la console de l’Observateur d’événements, accédez à **journaux des Applications et Services**, puis **Microsoft**, puis **Windows**, puis **duServicedeprofilutilisateur**, puis **opérationnelle**.
2. Examinez les événements qui se sont produites au moment des événements d’erreur ou avertissement que vous avez notée dans le journal des applications.

## <a name="step-3-enable-and-view-analytic-and-debug-logs"></a>Étape 3 : Activer et afficher les analytiques et les journaux de débogage

Si vous avez besoin de plus en détail que fournit le journal des opérations, vous pouvez activer analytiques et déboguer les journaux sur l’ordinateur concerné. Ce niveau de journalisation est beaucoup plus détaillé et doit être désactivé, sauf lors de la tentative de résoudre un problème.

Voici comment activer et afficher les analytiques et les journaux de débogage :

1. Dans le **Actions** volet de l’Observateur d’événements, sélectionnez **vue**, puis sélectionnez **afficher les journaux d’analyse et de débogage**.
2. Accédez à **journaux des Applications et Services**, puis **Microsoft**, puis **Windows**, puis **Service de profil utilisateur**, puis  **Diagnostic**.
3. Sélectionnez **activer le journal** , puis sélectionnez **Oui**. Ainsi, le journal de Diagnostic, ce qui démarre la journalisation.
4. Si vous avez besoin des informations encore plus détaillées, consultez [étape 4 : Création et le décodage d’une trace](#step-4-creating-and-decoding-a-trace) pour plus d’informations sur la création d’un journal des traces.
5. Lorsque vous avez terminé de résoudre le problème, accédez à la **Diagnostic** journal, sélectionnez **désactiver le journal**, sélectionnez **vue** puis désactivez le **afficher Analyse et déboguer les journaux** case à cocher pour masquer les analyse et la journalisation du débogage.

## <a name="step-4-creating-and-decoding-a-trace"></a>Étape 4 : Création et le décodage d’une trace

Si vous ne pouvez pas résoudre le problème à l’aide d’événements, vous pouvez créer un journal des traces (un fichier ETL) lors de la reproduction du problème et décoder à l’aide de symboles publics à partir du serveur de symboles Microsoft. Journaux de suivi fournissent des informations très spécifiques sur ce que fait le Service de profil utilisateur et peut aider à identifier où la défaillance s’est produite.

La meilleure stratégie pour utiliser le suivi ETL doit tout d’abord capturer le journal plus petit possible. Une fois que le journal est décodé, rechercher le journal pour les échecs.

Voici comment créer et décoder une trace pour le Service de profil utilisateur :

1. Connectez-vous à l’ordinateur où l’utilisateur rencontre des problèmes, à l’aide d’un compte qui est membre du groupe Administrateurs local.
2. À partir d’une invite de commandes avec élévation de privilèges, entrez les commandes suivantes, où *\<chemin d’accès\>* est le chemin d’accès à un dossier local que vous avez créé précédemment, par exemple C:\\journaux :
        
    ```PowerShell
    logman create trace -n RUP -o <Path>\RUP.etl -ets
    logman update RUP -p {eb7428f5-ab1f-4322-a4cc-1f1a9b2c5e98} 0x7FFFFFFF 0x7 -ets
    ```
3. À partir de l’écran d’accueil, sélectionnez le nom d’utilisateur, puis **changer de compte**, en veillant à ne pas se déconnecter de l’administrateur. Si vous utilisez le Bureau à distance, fermez la session de l’administrateur pour établir la session utilisateur.
4. Reproduisez le problème. La procédure permettant de reproduire le problème est généralement se connectent en tant que l’utilisateur rencontre le problème, connectez l’utilisateur, ou les deux.
5. Après avoir reproduit le problème, connectez-vous en tant qu’administrateur local à nouveau.
6. À partir d’une invite de commandes avec élévation de privilèges, exécutez la commande suivante pour enregistrer le journal dans un fichier ETL :
  
    ```PowerShell
    logman stop -n RUP -ets
    ```
7. Tapez la commande suivante pour exporter le fichier ETL dans un fichier explicite dans le répertoire actif (probablement votre dossier de base ou le dossier % Windir%\\dossier System32) :
    
    ```PowerShell
    Tracerpt <path>\RUP.etl
    ```
8. Ouvrez le **Summary.txt** fichier et **Dumpfile.xml** fichier (vous pouvez les ouvrir dans Microsoft Excel pour afficher plus facilement les détails complets du journal). Recherchez les événements qui incluent **échouer** ou **échec**; vous pouvez ignorer les lignes qui incluent le **inconnu** nom de l’événement.

## <a name="more-information"></a>Informations supplémentaires

* [Déployer des profils utilisateur itinérants](deploy-roaming-user-profiles.md)