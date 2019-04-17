---
ms.assetid: cb834273-828a-4141-9387-37dd8270e932
title: "Automatique Winlogon redémarrage Sign-On (ARSO)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 27d4285d34105908555458a95bd70fc04fd2901a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Automatique Winlogon redémarrage Sign-On (ARSO)

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

**Auteur**: Justin Turner, ingénieur Support résolution Senior avec le groupe de Windows

> [!NOTE]
> Ce contenu est écrit par un ingénieur du support client Microsoft et est destiné aux administrateurs expérimentés et architectes système qui recherchent des explications techniques plus approfondies des fonctionnalités et des solutions Windows Server2012R2 que proposent généralement les rubriques sur TechNet. Toutefois, il n’a pas subi les mêmes passes, afin de la partie du langage peut sembler que moins finalisée que ce qui se trouvent généralement sur TechNet.

## <a name="overview"></a>Vue d’ensemble
Windows 8 introduit des applications d’écran de verrouillage.  Ce sont les applications qui s’exécutent et affichent les notifications lorsque la session utilisateur est verrouillée (calendrier rendez-vous, courrier électronique et les messages, etc..).  Les périphériques qui sont redémarrés en raison du processus de mise à jour de Windows ne parviennent pas à afficher ces notifications d’écran de verrouillage lors du redémarrage.  Certains utilisateurs dépendent de ces applications d’écran de verrouillage.

## <a name="whats-changed"></a>Ce qui a changé?
Lorsqu’un utilisateur se connecte sur un appareil Windows 8.1, LSA enregistre les informations d’identification de l’utilisateur dans la mémoire chiffrée accessible uniquement par lsass.exe. Lors de la mise à jour Windows lance un redémarrage automatique sans la présence de l’utilisateur, ces informations d’identification seront utilisées pour configurer l’ouverture de session automatique pour l’utilisateur. Mise à jour de Windows en cours d’exécution en tant que système avec des privilèges TCB lance l’appel RPC pour effectuer cette opération.

Le redémarrage, l’utilisateur sera automatiquement être connecté via le mécanisme d’ouverture de session automatique et en outre verrouillé afin de protéger la session utilisateur. Le verrouillage sera lancée via Winlogon tandis que la gestion des informations d’identification est effectuée par l’autorité LSA.  En se connecte automatiquement et le verrouillage de l’utilisateur sur la console, les applications d’écran de verrouillage de l’utilisateur sera redémarrées et disponibles.

> [!NOTE]
> Après une mise à jour Windows induite par le redémarrage, le dernier utilisateur interactif connecté automatiquement et la session est verrouillée par conséquent, les applications d’écran de verrouillage de l’utilisateur peuvent exécuter.

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_LockScreenApp.gif)

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_LockScreen.gif)

**Vue d’ensemble**

-   Mise à jour Windows nécessite un redémarrage

-   Ordinateur capable de redémarrer (*aucune application en cours d’exécution qui ne serait de perdre des données*)?

    -   Redémarrage pour vous

    -   Se reconnecter

    -   Ordinateur de verrouillage

-   Activé ou désactivé par la stratégie de groupe

    -   Désactivé par défaut dans serveur références (SKU)

-   Pourquoi?

    -   Certaines mises à jour ne peut pas terminer jusqu'à ce que l’utilisateur se connecte à.

    -   Une meilleure expérience utilisateur: inutile d’attendre 15 minutes pour les mises à jour terminer l’installation

-   Comment? Ouverture de session automatique

    -   stocke le mot de passe, utilise ces informations d’identification pour vous connecter

    -   enregistre les informations d’identification en tant que secret LSA en mémoire paginée

    -   Ne peut être activé si BitLocker est activé

## <a name="group-policy-sign-in-last-interactive-user-automatically-after-a-system-initiated-restart"></a>Stratégie de groupe: Connectez-vous dernier utilisateur interactif automatiquement après un redémarrage initié par le système
Dans Windows 8.1 / Windows Server 2012 R2, ouverture de session automatique de l’utilisateur d’écran de verrouillage après le redémarrage de Windows Update s’abonner pour les références de serveur et désinscrire des références (SKU) du Client.

**Emplacement de la stratégie:** Configuration ordinateur > stratégies > modèles d’administration > composants Windows > Option d’ouverture de session de Windows

**Nom de la stratégie:** connexion dernier utilisateur interactif automatiquement après un redémarrage initié par le système

**Prise en charge sur:** au moins Windows Server 2012 R2, Windows 8.1 ou Windows RT 8.1

**Description/aide:**

Ce paramètre de stratégie détermine si un appareil sera automatiquement connectez-vous dernier utilisateur interactif après mise à jour Windows redémarre le système.

Si vous activez ou ne configurez pas ce paramètre de stratégie, le périphérique en toute sécurité enregistre les informations d’identification de l’utilisateur (y compris le nom d’utilisateur, domaine et mot de passe crypté) pour configurer connectez-vous automatique après le redémarrage d’une mise à jour de Windows. Après le redémarrage de Windows Update, l’utilisateur est connecté dans automatiquement et la session est verrouillée automatiquement avec toutes les applications d’écran de verrouillage configurées pour cet utilisateur.

Si vous désactivez ce paramètre de stratégie, l’appareil ne stocke pas les informations d’identification de l’utilisateur pour la connexion automatique après le redémarrage d’une mise à jour de Windows. Applications d’écran de verrouillage des utilisateurs ne sont pas redémarrées après le redémarrage du système.

**Éditeur du Registre**

|Nom de la valeur|Type|Données|
|--------------|--------|--------|
|DisableAutomaticRestartSignOn|DWORD|0<br /><br />**Exemple:**<br /><br />0 (activé)<br /><br />1 (désactivé)|

**Emplacement du Registre de stratégie:** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**Type:** DWORD

**Nom du Registre:** DisableAutomaticRestartSignOn

Valeur: 0 ou 1

0 = activé

1 = désactivé

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_SignInPolicy.gif)

## <a name="troubleshooting"></a>Résolution des problèmes
Lorsqu’il verrouille automatiquement WinLogon, suivi d’état de WinLogon est stocké dans le journal des événements WinLogon.

L’état d’une tentative de configuration d’ouverture de session automatique est enregistré

-   Si l’opération a réussi

    -   Il enregistre en tant que tel

-   S’il s’agit d’un échec:

    -   Quel était l’échec des enregistrements

-   Lorsque état de BitLocker modifications:

    -   la suppression des informations d’identification est enregistrée.

        -   Ils seront stockés dans le journal des opérations LSA.

### <a name="reasons-why-autologon-might-fail"></a>Raisons pourquoi l’ouverture de session automatique peut échouer
Il existe plusieurs cas dans lesquels une connexion automatique de l’utilisateur ne peut être atteint.  Cette section est conçue pour capturer les scénarios connus dans lequel cela peut se produire.

### <a name="user-must-change-password-at-next-login"></a>Utilisateur doit changer le mot de passe à la prochaine connexion
Connexion de l’utilisateur peut entrer dans un état bloqué lors de la modification du mot de passe à la prochaine ouverture de session est nécessaire.  Cela peut être détectés avant le redémarrage dans la plupart des cas, mais pas toutes les (par exemple, l’expiration du mot de passe peut être atteint entre l’arrêt et de la prochaine connexion.

### <a name="user-account-disabled"></a>Compte utilisateur désactivé
Une session utilisateur peut être conservée même si désactivé.  Redémarrage d’un compte qui est désactivé peut être détectée localement dans la plupart des cas à l’avance, en fonction de la stratégie de groupe qu'est peut-être pas pour les comptes de domaine (un domaine mises en cache travail des scénarios de connexion même si le compte est désactivé sur le contrôleur de domaine).

### <a name="logon-hours-and-parental-controls"></a>Heures d’ouverture de session et le contrôle parental
Les heures d’ouverture de session et le contrôle parental peut interdire une nouvelle session de l’utilisateur en cours de création.  Si un redémarrage doit se produire au cours de cette fenêtre, l’utilisateur ne serait pas autorisé à se connecter.  Il existe une stratégie supplémentaire qui entraîne le verrouillage ou la fermeture de session en tant qu’une action de la conformité.  Cela peut être problématique pour souvent enfant où verrouillage de compte peut se produire entre lit et veille, en particulier si la fenêtre de maintenance est couramment pendant ce temps.

## <a name="additional-resources"></a>Ressources supplémentaires
**Table de Table SEQ \\\ * ARABIC 3: ARSO glossaire**

|Terme|Définition|
|--------|--------------|
|Ouverture de session automatique|Ouverture de session automatique est une fonctionnalité qui a été présente dans Windows pour plusieurs versions.  Il s’agit d’une fonctionnalité de Windows qui a encore des outils tels que l’ouverture de session automatique pour Windows v3.01 documentée * [http:/technet.microsoft.com/sysinternals/bb963905.aspx](https://technet.microsoft.com/sysinternals/bb963905.aspx)*<br /><br />Il permet à un utilisateur unique de l’appareil pour vous connecter automatiquement sans avoir à entrer des informations d’identification. Les informations d’identification sont configurées et stockées dans le Registre en tant que secret LSA chiffré.|


