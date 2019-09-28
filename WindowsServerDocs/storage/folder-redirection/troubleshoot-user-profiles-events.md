---
title: Résoudre les problèmes liés aux profils utilisateur avec des événements
description: Comment résoudre les problèmes de chargement et de déchargement des profils utilisateur à l’aide des événements et des journaux de suivi.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 9e927a77627e786015a928d798aafee13a2cc34b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394377"
---
# <a name="troubleshoot-user-profiles-with-events"></a>Résoudre les problèmes liés aux profils utilisateur avec des événements

>S’applique à : Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 et Windows Server (canal semi-annuel).

Cette rubrique explique comment résoudre les problèmes de chargement et de déchargement des profils utilisateur à l’aide d’événements et de journaux de suivi. Les sections suivantes décrivent comment utiliser les trois journaux des événements qui enregistrent les informations de profil utilisateur.

## <a name="step-1-checking-events-in-the-application-log"></a>Étape 1 : Vérification des événements dans le journal des applications

La première étape de la résolution des problèmes de chargement et de déchargement des profils utilisateur (y compris les profils utilisateur itinérants) consiste à utiliser observateur d’événements pour examiner les événements d’avertissement et d’erreur que le service de profil utilisateur enregistre dans le journal des applications.

Voici comment afficher les événements des services de profil utilisateur dans le journal des applications :

1. Démarrez observateur d’événements. Pour ce faire, ouvrez le **panneau**de configuration, sélectionnez **système et sécurité**, puis, dans la section **Outils d’administration** , sélectionnez **afficher les journaux des événements**. La fenêtre observateur d’événements s’ouvre.
2. Dans l’arborescence de la console, accédez d’abord à **journaux Windows**, puis à **application**.
3. Dans le volet Actions, sélectionnez **filtrer le journal actuel**. La boîte de dialogue Filtrer le journal actuel s’ouvre.
4. Dans la zone **sources d’événements** , activez la case à cocher **service profils utilisateur** , puis sélectionnez **OK**.
5. Passez en revue la liste des événements, en accordant une attention particulière aux événements d’erreur.
6. Lorsque vous trouvez des événements remarquables, sélectionnez le lien aide en ligne du journal des événements pour afficher des informations supplémentaires et des procédures de dépannage.
7. Pour effectuer un dépannage supplémentaire, notez la date et l’heure des événements importants, puis examinez le journal des opérations (comme décrit à l’étape 2) pour afficher des détails sur ce que le service de profil utilisateur a fait au cours de l’exécution des événements d’erreur ou d’avertissement.

>[!NOTE]
>Vous pouvez ignorer en toute sécurité l’événement de service de profil utilisateur 1530 « Windows a détecté que votre fichier de Registre est toujours utilisé par d’autres applications ou services ».

## <a name="step-2-view-the-operational-log-for-the-user-profile-service"></a>Étape 2 : Afficher le journal des opérations du service de profil utilisateur

Si vous ne parvenez pas à résoudre le problème à l’aide du journal des applications seul, utilisez la procédure suivante pour afficher les événements du service de profil utilisateur dans le journal des opérations. Ce journal montre certains des fonctionnement internes du service et peut vous aider à identifier où se produit le problème dans le processus de chargement ou de déchargement de profil.

Le journal des applications Windows et le journal des opérations du service de profil utilisateur sont activés par défaut dans toutes les installations de Windows.

Voici comment afficher le journal des opérations du service de profil utilisateur :

1. Dans l’arborescence de la console observateur d’événements, accédez à **journaux des applications et des services**, puis **Microsoft**, **Windows**, **service de profil utilisateur**, puis **opérationnel**.
2. Examinez les événements qui se sont produits au cours de l’heure des événements d’erreur ou d’avertissement que vous avez notés dans le journal des applications.

## <a name="step-3-enable-and-view-analytic-and-debug-logs"></a>Étape 3 : Activer et afficher les journaux d’analyse et de débogage

Si vous avez besoin de plus de détails que le journal des opérations, vous pouvez activer les journaux d’analyse et de débogage sur l’ordinateur concerné. Ce niveau de journalisation est bien plus détaillé et doit être désactivé sauf en cas de dépannage d’un problème.

Voici comment activer et afficher les journaux d’analyse et de débogage :

1. Dans le volet **actions** de observateur d’événements, sélectionnez **affichage**, puis sélectionnez **afficher les journaux d’analyse et de débogage**.
2. Accédez à **journaux des applications et des services**, puis à **Microsoft**, puis à **Windows**, puis au **service de profil utilisateur**et enfin **diagnostic**.
3. Sélectionnez **activer le journal** , puis cliquez sur **Oui**. Cela active le journal de diagnostic, qui démarre la journalisation.
4. Si vous avez besoin d’informations encore plus détaillées, voir [Step 4 : Création et décodage d’une trace @ no__t-0 pour plus d’informations sur la création d’un journal des traces.
5. Lorsque vous avez terminé de résoudre le problème, accédez au Journal de **diagnostic** , sélectionnez **désactiver le journal**, sélectionnez **Afficher** , puis désactivez la case à cocher Afficher les **journaux d’analyse et de débogage** pour masquer l’enregistrement d’analyse et de débogage.

## <a name="step-4-creating-and-decoding-a-trace"></a>Étape 4 : Création et décodage d’une trace

Si vous ne parvenez pas à résoudre le problème à l’aide d’événements, vous pouvez créer un journal des traces (fichier ETL) tout en reproduisant le problème, puis le décoder à l’aide de symboles publics à partir du serveur de symboles Microsoft. Les journaux de suivi fournissent des informations très spécifiques sur le fonctionnement du service de profil utilisateur et peuvent aider à identifier l’endroit où la défaillance s’est produite.

La meilleure stratégie lors de l’utilisation du suivi ETL consiste à capturer d’abord le plus petit journal possible. Une fois le journal décodé, recherchez les erreurs dans le journal.

Voici comment créer et décoder une trace pour le service de profil utilisateur :

1. Connectez-vous à l’ordinateur sur lequel l’utilisateur rencontre des problèmes, à l’aide d’un compte membre du groupe Administrateurs local.
2. À partir d’une invite de commandes avec élévation de privilèges, entrez les commandes suivantes, où *\<Path @ no__t-2* est le chemin d’accès à un dossier local que vous avez créé précédemment, par exemple C : \\logs :
        
    ```PowerShell
    logman create trace -n RUP -o <Path>\RUP.etl -ets
    logman update RUP -p {eb7428f5-ab1f-4322-a4cc-1f1a9b2c5e98} 0x7FFFFFFF 0x7 -ets
    ```
3. Dans l’écran d’accueil, sélectionnez le nom d’utilisateur, puis sélectionnez **basculer le compte**et veillez à ne pas fermer la session de l’administrateur. Si vous utilisez Bureau à distance, fermez la session de l’administrateur pour établir la session utilisateur.
4. Reproduisez le problème. La procédure à suivre pour reproduire le problème consiste généralement à se connecter en tant qu’utilisateur rencontrant le problème, à déconnecter l’utilisateur ou les deux.
5. Après avoir reproduit le problème, connectez-vous à nouveau en tant qu’administrateur local.
6. À partir d’une invite de commandes avec élévation de privilèges, exécutez la commande suivante pour enregistrer le journal dans un fichier ETL :
  
    ```PowerShell
    logman stop -n RUP -ets
    ```
7. Tapez la commande suivante pour exporter le fichier ETL dans un fichier lisible dans le répertoire actif (probablement votre dossier de racine ou le dossier% WINDIR% \\System32) :
    
    ```PowerShell
    Tracerpt <path>\RUP.etl
    ```
8. Ouvrez le fichier **Summary. txt** et le fichier **dumpfile. xml** (vous pouvez les ouvrir dans Microsoft Excel pour afficher plus facilement les détails complets du journal). Recherchez les événements **qui incluent échec** ou **échec**; vous pouvez ignorer en toute sécurité les lignes qui incluent le nom d’événement **inconnu** .

## <a name="more-information"></a>Plus d’informations

* [Déployer des profils utilisateur itinérants](deploy-roaming-user-profiles.md)