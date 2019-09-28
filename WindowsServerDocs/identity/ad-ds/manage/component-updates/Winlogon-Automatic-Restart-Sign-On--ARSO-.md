---
ms.assetid: cb834273-828a-4141-9387-37dd8270e932
title: Connexion par redémarrage automatique Winlogon (connexion)
description: La façon dont l’authentification de redémarrage automatique de Windows peut aider vos utilisateurs à être plus productifs.
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.reviewer: cahick
ms.date: 08/20/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 53626c4cfac17cb11402ada9ce3397c487cd0720
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389855"
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Connexion par redémarrage automatique Winlogon (connexion)

Pendant une Windows Update, des processus spécifiques à l’utilisateur doivent se produire pour que la mise à jour soit terminée. Ces processus nécessitent que l’utilisateur soit connecté à son appareil. Lors de la première connexion après qu’une mise à jour a été lancée, les utilisateurs doivent attendre que ces processus spécifiques à l’utilisateur soient terminés avant de pouvoir commencer à utiliser leur appareil.

## <a name="how-does-it-work"></a>Comment cela fonctionne-t-il ?

Lorsque Windows Update lance un redémarrage automatique, connexion extrait les informations d’identification dérivées de l’utilisateur actuellement connecté, le rend persistant sur le disque et configure l’ouverture de session automatique pour l’utilisateur. Windows Update l’exécution en tant que système avec un privilège TCB déclenchera l’appel RPC pour effectuer cette opération.

Une fois la dernière Windows Update redémarrée, l’utilisateur est automatiquement connecté via le mécanisme d’ouverture de session automatique et la session de l’utilisateur est réalimentée avec les secrets rendus persistants. En outre, l’appareil est verrouillé pour protéger la session de l’utilisateur. Le verrouillage est initié via Winlogon, tandis que la gestion des informations d’identification est effectuée par l’autorité de sécurité locale (LSA). Une fois la configuration de connexion réussie et la connexion établie, les informations d’identification enregistrées sont immédiatement supprimées du disque.

En se connectant et en verrouillant automatiquement l’utilisateur sur la console, Windows Update pouvez terminer les processus spécifiques à l’utilisateur avant que l’utilisateur ne retourne à l’appareil. De cette façon, l’utilisateur peut commencer immédiatement à utiliser son appareil.

CONNEXION traite différemment les appareils non gérés et gérés. Pour les appareils non gérés, le chiffrement de l’appareil est utilisé, mais il n’est pas obligatoire pour l’utilisateur d’obtenir connexion. Pour les appareils gérés, TPM 2,0, SecureBoot et BitLocker sont requis pour la configuration de connexion. Les administrateurs informatiques peuvent remplacer cette exigence via stratégie de groupe. CONNEXION pour les appareils gérés est actuellement disponible uniquement pour les appareils qui sont joints à Azure Active Directory.

|   | Windows Update| arrêter-g-t 0  | Redémarrages lancés par l’utilisateur | API avec indicateurs SHUTDOWN_ARSO/EWX_ARSO |
| --- | :---: | :---: | :---: | :---: |
| Appareils gérés | :heavy_check_mark:  | :heavy_check_mark: |   | :heavy_check_mark: |
| Appareils non gérés | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |

> [!NOTE]
> Après un redémarrage induit Windows Update, le dernier utilisateur interactif est automatiquement connecté et la session est verrouillée. Cela permet aux applications de l’écran de verrouillage d’un utilisateur de continuer à s’exécuter malgré le redémarrage Windows Update.

![Page Paramètres](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/gtr-adds-lockscreenapp.png)

## <a name="policy-1"></a>#1 de stratégie

### <a name="sign-in-and-lock-last-interactive-user-automatically-after-a-restart"></a>Se connecter et verrouiller le dernier utilisateur interactif automatiquement après un redémarrage

Dans Windows 10, connexion est désactivé pour les références de serveur et s’abonne aux références clientes.

**Emplacement de la stratégie de groupe :** Configuration de l’ordinateur > Modèles d’administration > les composants Windows > les options d’ouverture de session Windows

**Stratégie Intune :**

- Multi Windows 10 et versions ultérieures
- Type de profil : Modèles d’administration
- Chemin : \Windows Windows\windows Logon options

**Pris en charge sur :** Au moins Windows 10 version 1903

**Description :**

Ce paramètre de stratégie détermine si un appareil se connecte automatiquement et verrouille le dernier utilisateur interactif après le redémarrage du système ou après un arrêt et un démarrage à froid.

Cela se produit uniquement si le dernier utilisateur interactif ne s’est pas déconnecté avant le redémarrage ou l’arrêt.

Si l’appareil est joint à Active Directory ou Azure Active Directory, cette stratégie s’applique uniquement aux redémarrages de Windows Update. Dans le cas contraire, cette opération s’applique aux redémarrages de Windows Update et aux redémarrages et arrêts initiés par l’utilisateur.

Si vous ne configurez pas ce paramètre de stratégie, il est activé par défaut. Lorsque la stratégie est activée, l’utilisateur est automatiquement connecté et la session est automatiquement verrouillée avec toutes les applications d’écran de verrouillage configurées pour cet utilisateur après le démarrage de l’appareil.

Après avoir activé cette stratégie, vous pouvez configurer ses paramètres à l’aide de la stratégie ConfigAutomaticRestartSignOn, qui configure le mode de connexion et de verrouillage automatique du dernier utilisateur interactif après un redémarrage ou un démarrage à froid.

Si vous désactivez ce paramètre de stratégie, l’appareil ne configure pas la connexion automatique. Les applications de l’écran de verrouillage de l’utilisateur ne sont pas redémarrées après le redémarrage du système.

**Éditeur du Registre :**

| Nom de valeur | Type | Données |
| --- | --- | --- |
| DisableAutomaticRestartSignOn | DWORD | 0 (activer connexion) |
|   |   | 1 (désactiver connexion) |

**Emplacement du registre de la stratégie :** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**Type :** DWORD

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/gtr-adds-signinpolicy.png)

## <a name="policy-2"></a>#2 de stratégie

### <a name="configure-the-mode-of-automatically-signing-in-and-locking-last-interactive-user-after-a-restart-or-cold-boot"></a>Configurer le mode de connexion et de verrouillage automatique du dernier utilisateur interactif après un redémarrage ou un démarrage à froid

**Emplacement de la stratégie de groupe :** Configuration de l’ordinateur > Modèles d’administration > les composants Windows > les options d’ouverture de session Windows

**Stratégie Intune :**

- Multi Windows 10 et versions ultérieures
- Type de profil : Modèles d’administration
- Chemin : \Windows Windows\windows Logon options

**Pris en charge sur :** Au moins Windows 10 version 1903

**Description :**

Ce paramètre de stratégie contrôle la configuration selon laquelle un redémarrage et une connexion automatiques et un verrouillage se produisent après un redémarrage ou un démarrage à froid. Si vous avez choisi « désactivé » dans la stratégie « connexion et verrouillage du dernier utilisateur interactif automatiquement après un redémarrage », l’authentification automatique n’a pas lieu et cette stratégie n’a pas besoin d’être configurée.

Si vous activez ce paramètre de stratégie, vous pouvez choisir l’une des deux options suivantes :

1. « Activé si BitLocker est activé et non suspendu » indique que la connexion automatique et le verrouillage se produisent uniquement si BitLocker est actif et n’est pas suspendu au cours du redémarrage ou de l’arrêt. Vous pouvez accéder aux données personnelles sur le disque dur de l’appareil à ce moment-là si BitLocker n’est pas activé ou suspendu pendant une mise à jour. La suspension BitLocker supprime temporairement la protection des composants système et des données, mais elle peut être nécessaire dans certaines circonstances pour mettre à jour correctement les composants critiques de démarrage.
   - BitLocker est suspendu pendant les mises à jour dans les cas suivants :
      - L’appareil ne dispose pas de TPM 2,0 et PCR7, ou
      - L’appareil n’utilise pas de protecteur TPM uniquement
2. « Always Enabled » indique que la connexion automatique se produira même si BitLocker est désactivé ou suspendu lors du redémarrage ou de l’arrêt. Lorsque BitLocker n’est pas activé, les données personnelles sont accessibles sur le disque dur. Le redémarrage automatique et la connexion ne doivent être exécutés que dans ce cas, si vous êtes certain que l’appareil configuré se trouve dans un emplacement physique sécurisé.

Si vous désactivez ou ne configurez pas ce paramètre, l’authentification automatique est définie par défaut sur le comportement « activé si BitLocker est activé et non suspendu ».

**Éditeur du Registre**

| Nom de valeur | Type | Données |
| --- | --- | --- |
| AutomaticRestartSignOnConfig | DWORD | 0 (activer connexion si sécurisé) |
|   |   | 1 (activer les connexion toujours) |

**Emplacement du registre de la stratégie :** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**Type :** DWORD

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/arso-policy-setting.png)

## <a name="troubleshooting"></a>Résolution des problèmes

Lorsque WinLogon se verrouille automatiquement, la trace de l’état de WinLogon est stockée dans le journal des événements WinLogon.

L’état d’une tentative de configuration d’ouverture de session automatique est journalisé

- Si elle est réussie
   - les enregistre en tant que tel
- S’il s’agit d’une défaillance :
   - enregistre l’échec
- Lorsque l’état de BitLocker change :
   - la suppression des informations d’identification sera enregistrée
   - Celles-ci seront stockées dans le journal des opérations LSA.

### <a name="reasons-why-autologon-might-fail"></a>Raisons pour lesquelles l’ouverture automatique peut échouer

Il existe plusieurs cas dans lesquels la connexion automatique d’un utilisateur ne peut pas être obtenue.  Cette section est destinée à capturer les scénarios connus dans lesquels cela peut se produire.

### <a name="user-must-change-password-at-next-login"></a>L’utilisateur doit changer de mot de passe à la prochaine connexion

La connexion de l’utilisateur peut entrer dans un État bloqué lorsque la modification du mot de passe à la prochaine connexion est requise.  Cela peut être détecté avant le redémarrage dans la plupart des cas, mais pas tous (par exemple, l’expiration du mot de passe peut être atteinte entre l’arrêt et la connexion suivante.

### <a name="user-account-disabled"></a>Compte d’utilisateur désactivé

Une session utilisateur existante peut être conservée même si elle est désactivée.  Le redémarrage d’un compte qui est désactivé peut être détecté localement dans la plupart des cas à l’avance, en fonction de la stratégie de niveau de service (ou non) pour les comptes de domaine (certains scénarios de connexion de domaine mis en cache fonctionnent même si le compte est désactivé sur le contrôleur de domaine).

### <a name="logon-hours-and-parental-controls"></a>Horaires d’accès et contrôle parental

Les heures d’ouverture de session et le contrôle parental peuvent empêcher la création d’une nouvelle session utilisateur.  Si un redémarrage se produit au cours de cette fenêtre, l’utilisateur n’est pas autorisé à se connecter.  Une stratégie supplémentaire empêche le verrouillage ou la déconnexion en tant qu’action de conformité. L’état d’une tentative de configuration de l’ouverture de session automatique est journalisé.

## <a name="security-details"></a>Détails de sécurité

### <a name="credentials-stored"></a>Informations d’identification stockées

|   | Hachage du mot de passe | Clé d’informations d’identification | Ticket d’accord de ticket | Jeton d’actualisation principal |
| --- | :---: | :---: | :---: | :---: |
| Compte local | :heavy_check_mark: | :heavy_check_mark: |   |   |
| Compte MSA | :heavy_check_mark: | :heavy_check_mark: |   |   |
| Compte joint Azure AD | :heavy_check_mark: | :heavy_check_mark: | : heavy_check_mark : (si hybride) | :heavy_check_mark: |
| Compte joint au domaine | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | : heavy_check_mark : (si hybride) |

### <a name="credential-guard-interaction"></a>Interaction Credential Guard

Si Credential Guard est activé pour un appareil, les secrets dérivés d’un utilisateur sont chiffrés avec une clé spécifique à la session de démarrage en cours. Par conséquent, connexion n’est pas actuellement pris en charge sur les appareils sur lesquels Credential Guard est activé.

## <a name="additional-resources"></a>Ressources supplémentaires

Le logo automatique est une fonctionnalité qui est présente dans Windows pour plusieurs versions. Il s’agit d’une fonctionnalité documentée de Windows qui a même des outils tels que le logo automatique pour Windows [http:/technet. Microsoft. com/Sysinternals/bb963905. aspx](https://technet.microsoft.com/sysinternals/bb963905.aspx). Il permet à un seul utilisateur de l’appareil de se connecter automatiquement sans entrer d’informations d’identification. Les informations d’identification sont configurées et stockées dans le registre en tant que secret LSA chiffré. Cela peut être problématique dans de nombreux cas enfants où le verrouillage de compte peut se produire entre le temps de sortie et la mise en éveil, en particulier si la fenêtre de maintenance est courante pendant cette période.
