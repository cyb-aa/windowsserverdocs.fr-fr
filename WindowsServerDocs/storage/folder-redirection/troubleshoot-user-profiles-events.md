---
title: Résoudre les problèmes de profils utilisateur avec des événements
description: Comment résoudre les problèmes de chargement et déchargement de profils utilisateur à l’aide d’événements et les journaux de suivi.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 6099dac7d77e37b761785b4f58b6106472e5ba1e
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082109"
---
# <a name="troubleshoot-user-profiles-with-events"></a>Résoudre les problèmes de profils utilisateur avec des événements

>S’applique à: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2 et Windows Server 2016.

Cette rubrique décrit comment résoudre les problèmes de chargement et déchargement profils utilisateur à l’aide d’événements et les journaux de suivi. Les sections suivantes décrivent comment utiliser les trois journaux des événements qui enregistrent les informations de profil utilisateur.

## <a name="step-1-checking-events-in-the-application-log"></a>Étape 1: Vérification des événements dans le journal des applications

La première étape dans la résolution des problèmes de charger et décharger utilisateur profils (y compris les profils utilisateur itinérants) consiste à utiliser l’Observateur d’événements pour examiner les événements d’erreur et d’avertissement qui se connectent les enregistrements dans l’Application de Service de profil utilisateur.

Voici comment procéder pour afficher les événements de Services de profil utilisateur dans le journal des applications:

1. Démarrez l’Observateur d’événements. Pour ce faire, ouvrez **Le panneau de configuration**, sélectionnez **système et sécurité**, puis, dans la section **Outils d’administration** , sélectionnez **Afficher les journaux des événements**. La fenêtre de l’Observateur d’événements s’ouvre.
2. Dans l’arborescence de la console, tout d’abord vous accédez à **Journaux Windows**, puis l' **Application**.
3. Dans le volet Actions, sélectionnez **Filtrer le journal actuel**. La boîte de dialogue Filtrer le journal actuel.
4. Dans la zone **sources d’événements** , activez la case à cocher **Service de profils utilisateur** , puis sélectionnez **OK**.
5. Passez en revue la liste des événements, en accordant une attention particulière aux événements d’erreur.
6. Lorsque vous avez trouvé les événements dignes d’intérêt, sélectionnez le lien d’aide en ligne du journal des événements pour afficher des informations supplémentaires et des procédures de dépannage.
7. Pour effectuer les procédures de dépannage supplémentaires, notez la date et l’heure d’événements dignes d’intérêt, puis examinez le journal des opérations (comme décrit à l’étape 2) pour afficher plus d’informations sur ce que le Service de profil utilisateur faisait autour de l’heure des événements d’erreur ou un avertissement.

>[!NOTE]
>Vous pouvez ignorer le Service de profil utilisateur événement 1530 «Windows a détecté que votre fichier de Registre est en cours d’utilisation par d’autres applications ou services.»

## <a name="step-2-view-the-operational-log-for-the-user-profile-service"></a>Étape 2: Afficher le journal des opérations pour le Service de profil utilisateur

Si vous ne pouvez pas résoudre le problème à l’aide de décrire le journal des applications, utilisez la procédure suivante pour afficher les événements de Service de profil utilisateur dans le journal des opérations. Ce journal montre quelques-unes des rouages du service et peut aider à identifier l’emplacement de la charge de profil ou de décharger les processus que le problème se produit.

Le journal d’Application Windows et le journal opérationnel de Service de profil utilisateur sont activées par défaut dans toutes les installations de Windows.

Voici comment afficher le journal des opérations pour le Service de profil utilisateur:

1. Dans les événements Observateur d’arborescence de la console, accédez à des **Applications et les journaux des Services**, puis **Microsoft**, puis **Windows**, puis **Le Service de profil utilisateur**et **opérationnel**.
2. Examinez les événements qui se sont produites au moment des événements d’erreur ou un avertissement que vous avez noté dans le journal des applications.

## <a name="step-3-enable-and-view-analytic-and-debug-logs"></a>Étape 3: Activer et afficher les grilles et journaux de débogage

Si vous avez besoin de plus en détail que fournit le journal opérationnel, vous pouvez activer analytique et déboguer les journaux sur l’ordinateur affecté. Ce niveau de journalisation est beaucoup plus détaillé et doit être désactivé à l’exception lors de la tentative de résoudre un problème.

Voici comment activer et afficher les journaux de débogage:

1. Dans le volet **Actions** de l’Observateur d’événements, sélectionnez **Afficher**, puis sélectionnez **Afficher les journaux et débogage**.
2. Accédez à **journaux des Applications et Services**, puis **Microsoft**, puis **Windows**, puis **Service de profil utilisateur**et **des diagnostics**.
3. Sélectionnez **Activer le journal** , puis sélectionnez **Oui**. Cela permet le journal de Diagnostic, qui permet de démarrer la journalisation.
4. Si vous avez besoin des informations encore plus détaillées, consultez la rubrique [étape 4: création et de décodage une trace](#step-4:-creating-and-decoding-a-trace) pour plus d’informations sur la création d’un journal de suivi.
5. Lorsque vous avez terminé de résoudre le problème, accédez au journal de **Diagnostic** , sélectionnez **Désactiver le journal**, sélectionnez **Afficher** et puis désactivez la case à cocher **Afficher les journaux et débogage** pour masquer analytique et de l’enregistrement de débogage.

## <a name="step-4-creating-and-decoding-a-trace"></a>Étape 4: Création et décoder un suivi

Si vous ne pouvez pas résoudre le problème à l’aide d’événements, vous pouvez créer un journal de suivi (un fichier ETL) lors de reproduire le problème et puis décoder à l’aide de symboles publics à partir du serveur de symbole de Microsoft. Les journaux de suivi fournissent des informations très spécifiques sur ce que fait le Service de profil utilisateur et peuvent aider à identifier où l’erreur s’est produite.

La meilleure stratégie lors de l’utilisation du suivi ETL doit tout d’abord capturer le journal plus petit possible. Une fois que le journal est décodé, recherche dans le journal des échecs.

Voici comment créer et décoder un suivi pour le Service de profil utilisateur:

1. Ouvrez une session sur l’ordinateur où l’utilisateur rencontre des problèmes, en utilisant un compte qui est membre du groupe Administrateurs local.
2. À partir d’une invite de commandes avec élévation de privilèges, saisissez les commandes suivantes, où *\ < chemin_accès\nom_fichier >* est le chemin d’accès à un dossier local que vous avez créé précédemment, par exemple C:\\logs:
        
    ```PowerShell
    logman create trace -n RUP -o <Path>\RUP.etl -ets
    logman update RUP -p {eb7428f5-ab1f-4322-a4cc-1f1a9b2c5e98} 0x7FFFFFFF 0x7 -ets
    ```
3. À partir de l’écran d’accueil, sélectionnez le nom d’utilisateur, puis sélectionnez le **compte de commutateur**, en veillant à ne pas Déconnectez-vous de l’administrateur. Si vous utilisez le Bureau à distance, fermez la session administrateur pour établir la session de l’utilisateur.
4. Reproduisez le problème. La procédure à reproduire le problème est généralement à l’ouverture de session en tant que l’utilisateur concerné par le problème, connexion de l’utilisateur, ou les deux.
5. Après avoir reproduit le problème, reconnectez-vous en tant qu’administrateur local.
6. À partir d’une invite de commandes avec élévation de privilèges, exécutez la commande suivante pour enregistrer le journal dans un fichier ETL:
  
    ```PowerShell
    logman stop -n RUP -ets
    ```
7. Tapez la commande suivante pour exporter le fichier ETL dans un fichier explicite dans le répertoire actif (probablement votre dossier de base ou le dossier %WINDIR%\\System32):
    
    ```PowerShell
    Tracerpt <path>\RUP.etl
    ```
8. Ouvrez le fichier **Summary.txt** et le fichier **Dumpfile.xml** (vous pouvez les ouvrir dans Microsoft Excel pour afficher plus facilement les détails complets du journal). Recherchez les événements qui incluent **l’Échec** ou **a échoué**; Vous pouvez ignorer les lignes qui incluent le nom de l’événement **inconnu** .

## <a name="more-information"></a>Informations supplémentaires

* [Déployer les profils utilisateur itinérants](deploy-roaming-user-profiles.md)