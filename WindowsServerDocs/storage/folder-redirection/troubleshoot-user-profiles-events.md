---
title: Résoudre les problèmes des profils utilisateur avec les événements
description: Guide pratique pour résoudre les problèmes de chargement et de déchargement des profils utilisateur à l’aide des événements et des journaux des traces.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 9e927a77627e786015a928d798aafee13a2cc34b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394377"
---
# <a name="troubleshoot-user-profiles-with-events"></a>Résoudre les problèmes des profils utilisateur avec les événements

>S'applique à : Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 et Windows Server (Canal semi-annuel).

Cette rubrique explique comment résoudre les problèmes de chargement et de déchargement des profils utilisateur à l’aide des événements et des journaux des traces. Les sections suivantes décrivent l’utilisation des trois journaux des événements qui enregistrent les informations relatives aux profils utilisateur.

## <a name="step-1-checking-events-in-the-application-log"></a>Étape 1 : Consultation des événements dans le journal des applications

La première étape de la résolution des problèmes de chargement et de déchargement des profils utilisateur (notamment les profils utilisateur itinérants) consiste à utiliser l’observateur d’événements pour examiner les événements Avertissement et Erreur que le service Profil utilisateur enregistre dans le journal des applications.

Voici comment voir les événements du service Profil utilisateur dans le journal des applications :

1. Démarrez l’observateur d’événements. Pour ce faire, ouvrez le **Panneau de configuration**, sélectionnez **Système et sécurité**, puis, dans la section **Outils d’administration**, sélectionnez **Afficher les journaux d’événements**. La fenêtre Observateur d’événements s’ouvre.
2. Dans l’arborescence de la console, accédez d’abord à **Journaux Windows**, puis **Application**.
3. Dans le volet Actions, sélectionnez **Filtrer le journal actuel**. La boîte de dialogue Filtrer le journal actuel s’ouvre.
4. Dans la zone **Sources d’événements**, cochez la case **Service Profil utilisateur**, puis sélectionnez **OK**.
5. Passez en revue la liste des événements, en accordant une attention particulière aux événements d’erreur.
6. Quand vous trouvez des événements pertinents, sélectionnez le lien Aide en ligne du Journal d’événements pour afficher des procédures de résolution de problèmes et des informations supplémentaires.
7. Pour approfondir la résolution des problèmes, notez la date et l’heure des événements importants, puis examinez le journal des opérations (comme décrit à l’étape 2) pour voir les détails relatifs au fonctionnement du service Profil utilisateur au moment où se sont produits les événements Erreur ou Avertissement.

>[!NOTE]
>Vous pouvez ignorer l’événement 1530 du service Profil utilisateur, qui indique « Windows a détecté que votre fichier de Registre est toujours utilisé par d’autres applications ou services. »

## <a name="step-2-view-the-operational-log-for-the-user-profile-service"></a>Étape 2 : Voir le journal des opérations du service Profil utilisateur

Si vous ne pouvez pas résoudre le problème uniquement à l’aide du journal des applications, utilisez la procédure suivante pour voir les événements du service Profil utilisateur dans le journal des opérations. Ce journal montre certains des fonctionnements internes du service. Il peut vous aider à identifier l’origine du problème dans le processus de chargement ou de déchargement du profil.

Le journal des applications Windows et le journal des opérations du service Profil utilisateur sont activés par défaut dans toutes les installations de Windows.

Voici comment voir le journal des opérations du service Profil utilisateur :

1. Dans l’arborescence de la console de l’observateur d’événements, accédez à **Journaux des applications et des services**, **Microsoft**, **Windows**, **Service Profil utilisateur**, puis **Opérationnel**.
2. Examinez les événements qui se sont produits à peu près au moment où se sont déclenchés les événements Erreur ou Avertissement que vous avez notés dans le journal des applications.

## <a name="step-3-enable-and-view-analytic-and-debug-logs"></a>Étape 3 : Activer et voir les journaux d’analyse et de débogage

Si vous avez besoin de plus de détails que ceux fournis par le journal des opérations, vous pouvez activer les journaux d’analyse et de débogage sur l’ordinateur concerné. Ce niveau de journalisation est beaucoup plus détaillé et doit être désactivé, sauf si vous essayez de résoudre un problème.

Voici comment activer et voir les journaux d’analyse et de débogage :

1. Dans le volet **Actions** de l’observateur d’événements, sélectionnez **Affichage**, puis **Afficher les journaux d’analyse et de débogage**.
2. Accédez à **Journaux des applications et des services**, **Microsoft**, **Windows**, **Service Profil utilisateur**, puis **Diagnostic**.
3. Sélectionnez **Activer le journal**, puis **Oui**. Cela permet d’activer le journal de diagnostic, ce qui entraîne le démarrage de la journalisation.
4. Si vous avez besoin d’informations encore plus détaillées, consultez [Étape 4 : Création et décodage d’une trace](#step-4-creating-and-decoding-a-trace) pour plus d’informations sur la création d’un journal des traces.
5. Une fois que vous avez fini de résoudre le problème, accédez au **journal de ​diagnostic**, sélectionnez **Désactiver le journal**, sélectionnez **Afficher**, puis décochez la case **Afficher les journaux d’analyse et de débogage** pour masquer la journalisation de l’analyse et du débogage.

## <a name="step-4-creating-and-decoding-a-trace"></a>Étape 4 : Création et décodage d’une trace

Si vous ne pouvez pas résoudre le problème à l’aide des événements, vous pouvez créer un journal des traces (fichier ETL) tout en reproduisant le problème. Décodez ensuite ce journal à l’aide des symboles publics du serveur de symboles Microsoft. Les journaux des traces fournissent des informations très spécifiques sur le fonctionnement du service Profil utilisateur. Ils peuvent vous aider à identifier l’origine de la défaillance.

La meilleure stratégie d’utilisation du traçage ETL consiste à capturer d’abord le plus petit journal possible. Une fois le journal décodé, cherchez les erreurs qu’il contient.

Voici comment créer et décoder une trace pour le service Profil utilisateur :

1. Connectez-vous à l’ordinateur sur lequel l’utilisateur rencontre des problèmes, à l’aide d’un compte membre du groupe Administrateurs local.
2. À partir d’une invite de commandes avec élévation de privilèges, entrez les commandes suivantes, où *\<Path\>* représente le chemin d’un dossier local que vous avez créé, par exemple C:\\logs:
        
    ```PowerShell
    logman create trace -n RUP -o <Path>\RUP.etl -ets
    logman update RUP -p {eb7428f5-ab1f-4322-a4cc-1f1a9b2c5e98} 0x7FFFFFFF 0x7 -ets
    ```
3. Dans l’écran d’accueil, sélectionnez le nom d’utilisateur, puis sélectionnez **Changer de compte**, en veillant à ne pas déconnecter l’administrateur. Si vous utilisez le Bureau à distance, fermez la session administrateur pour établir la session utilisateur.
4. Reproduisez le problème. La procédure à suivre pour reproduire le problème consiste généralement à se connecter en tant qu’utilisateur confronté au problème, à déconnecter l’utilisateur, ou à effectuer ces deux actions.
5. Après avoir reproduit le problème, reconnectez-vous en tant qu’administrateur local.
6. À partir d’une invite de commandes avec élévation de privilèges, exécutez la commande suivante pour enregistrer le journal dans un fichier ETL :
  
    ```PowerShell
    logman stop -n RUP -ets
    ```
7. Tapez la commande suivante pour exporter le fichier ETL vers un fichier plus facile à lire pour un humain dans le répertoire actif (probablement votre dossier de base ou le dossier %WINDIR%\\System32) :
    
    ```PowerShell
    Tracerpt <path>\RUP.etl
    ```
8. Ouvrez le fichier **Summary.txt** et le fichier **Dumpfile.xml** (vous pouvez les ouvrir dans Microsoft Excel pour voir plus facilement les détails complets du journal). Recherchez les événements qui incluent **échec** ou **erreur**. Vous pouvez ignorer les lignes qui contiennent le nom d’événement **Inconnu**.

## <a name="more-information"></a>Autres informations

* [Déployer des profils utilisateur itinérants](deploy-roaming-user-profiles.md)