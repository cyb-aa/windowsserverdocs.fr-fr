---
title: Connexion de redémarrage automatique Winlogon
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.service: na
ms.suite: na
ms.technology: security-auditing
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 15cddcfa-8a8e-45e4-bb76-b8e1a14ceac0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: f085cf78a01148f97a450577131213ce977a432a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402320"
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Connexion de redémarrage automatique Winlogon

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

**Auteur**: Justin Turner, ingénieur du support technique senior avec le groupe Windows  
  
> [!NOTE]  
> Ce contenu est écrit par un ingénieur du support client Microsoft et est destiné aux administrateurs expérimentés et aux architectes système qui recherchent des explications techniques plus approfondies des fonctionnalités et des solutions Windows Server 2012 R2 que n'en proposent généralement les rubriques de TechNet. Toutefois, il n'a pas subi les mêmes passes de correction. De ce fait, une partie du langage peut sembler moins finalisée que le contenu de TechNet.  
  
## <a name="overview"></a>Vue d'ensemble  
Windows 8 a introduit les applications de l’écran de verrouillage.  Il s’agit des applications qui exécutent et affichent des notifications lorsque la session de l’utilisateur est verrouillée (rendez-vous, courrier électronique et messages, etc.).  Les appareils qui sont redémarrés en raison du processus de Windows Update ne parviennent pas à afficher ces notifications d’écran de verrouillage lors du redémarrage.  Certains utilisateurs dépendent de ces applications d’écran de verrouillage.  
  
## <a name="whats-changed"></a>Nouveautés  
Lorsqu’un utilisateur se connecte sur un appareil Windows 8.1, LSA enregistre les informations d’identification de l’utilisateur dans la mémoire chiffrée accessibles uniquement par Lsass. exe. Lorsque Windows Update lance un redémarrage automatique sans présence de l’utilisateur, ces informations d’identification sont utilisées pour configurer l’ouverture de la connexion automatique pour l’utilisateur. Windows Update l’exécution en tant que système avec un privilège TCB déclenchera l’appel RPC pour effectuer cette opération.  
  
Lors du redémarrage, l’utilisateur est automatiquement connecté via le mécanisme d’ouverture de session automatique, puis verrouillé pour protéger la session de l’utilisateur. Le verrouillage est initié via Winlogon, tandis que la gestion des informations d’identification est effectuée par LSA.  En se connectant et en verrouillant automatiquement l’utilisateur sur la console, les applications de l’écran de verrouillage de l’utilisateur sont redémarrées et disponibles.  
  
> [!NOTE]  
> Après un redémarrage induit Windows Update, le dernier utilisateur interactif est automatiquement connecté et la session est verrouillée afin que les applications de l’écran de verrouillage de l’utilisateur puissent s’exécuter.  
  
![Capture d’écran montrant l’écran de verrouillage](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_LockScreenApp.gif)  
  
![Capture d’écran montrant les applications de l’écran de verrouillage](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_LockScreen.gif)  
  
**Aperçu rapide**  
  
-   Windows Update nécessite un redémarrage  
  
-   L’ordinateur est-il en mesure de redémarrer (*aucune application en cours d’exécution ne risque de perdre des données*) ?  
  
    -   Redémarrer pour vous  
  
    -   Se reconnecter  
  
    -   Verrouiller l’ordinateur  
  
-   Activé ou désactivé par stratégie de groupe  
  
    -   Désactivé par défaut dans les références serveur  
  
-   Pourquoi ?  
  
    -   Certaines mises à jour ne peuvent pas se terminer tant que l’utilisateur n’est pas connecté.  
  
    -   Amélioration de l’expérience utilisateur : il n’est pas nécessaire d’attendre 15 minutes pour terminer l’installation des mises à jour  
  
-   Utilisation? AutoLogon  
  
    -   stocke le mot de passe, utilise ces informations d’identification pour vous connecter  
  
    -   enregistre les informations d’identification en tant que secret LSA dans la mémoire paginée  
  
    -   Ne peut être activé que si BitLocker est activé  
  
## <a name="group-policy-sign-in-last-interactive-user-automatically-after-a-system-initiated-restart"></a>Stratégie de groupe: Connectez le dernier utilisateur interactif automatiquement après un redémarrage initié par le système  
Dans Windows 8.1/Windows Server 2012 R2, l’ouverture de la ligne automatique de l’utilisateur de l’écran de verrouillage après un redémarrage de la Windows Update s’abonne aux références SKU du serveur et s’abonne aux références clientes.  
  
**Emplacement de la stratégie :** Configuration de l’ordinateur > des stratégies > Modèles d’administration > composants Windows > option d’ouverture de session Windows  
  
**Nom de la stratégie :** Connectez le dernier utilisateur interactif automatiquement après un redémarrage initié par le système  
  
**Pris en charge sur :** Au minimum Windows Server 2012 R2, Windows 8.1 ou Windows RT 8,1  
  
**Description/aide :**  
  
Ce paramètre de stratégie détermine si un appareil se connecte automatiquement au dernier utilisateur interactif après Windows Update redémarre le système.  
  
Si vous activez ou ne configurez pas ce paramètre de stratégie, l’appareil enregistre en toute sécurité les informations d’identification de l’utilisateur (y compris le nom d’utilisateur, le domaine et le mot de passe chiffré) pour configurer la connexion automatique après un redémarrage de Windows Update. Après le redémarrage du Windows Update, l’utilisateur est automatiquement connecté et la session est automatiquement verrouillée avec toutes les applications de l’écran de verrouillage configurées pour cet utilisateur.  
  
Si vous désactivez ce paramètre de stratégie, l’appareil ne stocke pas les informations d’identification de l’utilisateur pour la connexion automatique après un redémarrage de Windows Update. Les applications de l’écran de verrouillage de l’utilisateur ne sont pas redémarrées après le redémarrage du système.  
  
**Éditeur du Registre**  
  
|Nom de valeur|Type|Données|  
|-------|----|----|  
|DisableAutomaticRestartSignOn|DWORD|0<br /><br />**Tels**<br /><br />0 (activé)<br /><br />1 (désactivé)|  
  
**Emplacement du registre de la stratégie :** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System  
  
**Type :** DWORD  
  
**Nom du Registre :** DisableAutomaticRestartSignOn  
  
Valeur : 0 ou 1  
  
0 = activé  
  
1 = désactivé  
  
![Capture d’écran montrant le paramètre de stratégie contrôles d’interface utilisateur où vous pouvez spécifier si un appareil se connectera automatiquement au dernier utilisateur interactif après que Windows Update redémarre le système](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_SignInPolicy.gif)  
  
## <a name="troubleshooting"></a>Résolution des problèmes  
Lorsque WinLogon se verrouille automatiquement, la trace de l’état de WinLogon est stockée dans le journal des événements WinLogon.  
  
L’état d’une tentative de configuration d’ouverture de session automatique est journalisé  
  
-   Si elle est réussie  
  
    -   les enregistre en tant que tel  
  
-   S’il s’agit d’une défaillance :  
  
    -   enregistre l’échec  
  
-   Lorsque l’état de BitLocker change :  
  
    -   la suppression des informations d’identification sera enregistrée  
  
        -   Celles-ci seront stockées dans le journal des opérations LSA.  
  
### <a name="reasons-why-autologon-might-fail"></a>Raisons pour lesquelles l’ouverture automatique peut échouer  
Il existe plusieurs cas dans lesquels la connexion automatique d’un utilisateur ne peut pas être obtenue.  Cette section est destinée à capturer les scénarios connus dans lesquels cela peut se produire.  
  
### <a name="user-must-change-password-at-next-login"></a>L’utilisateur doit changer de mot de passe à la prochaine connexion  
La connexion de l’utilisateur peut entrer dans un État bloqué lorsque la modification du mot de passe à la prochaine connexion est requise.  Cela peut être détecté avant le redémarrage dans la plupart des cas, mais pas tous (par exemple, l’expiration du mot de passe peut être atteinte entre l’arrêt et la connexion suivante.  
  
### <a name="user-account-disabled"></a>Compte d’utilisateur désactivé  
Une session utilisateur existante peut être conservée même si elle est désactivée.  Le redémarrage d’un compte qui est désactivé peut être détecté localement dans la plupart des cas à l’avance, en fonction de la stratégie de niveau de service (ou non) pour les comptes de domaine (certains scénarios de connexion de domaine mis en cache fonctionnent même si le compte est désactivé sur le contrôleur de domaine).  
  
### <a name="logon-hours-and-parental-controls"></a>Horaires d’accès et contrôle parental  
Les heures d’ouverture de session et le contrôle parental peuvent empêcher la création d’une nouvelle session utilisateur.  Si un redémarrage se produit au cours de cette fenêtre, l’utilisateur n’est pas autorisé à se connecter.  Une stratégie supplémentaire empêche le verrouillage ou la déconnexion en tant qu’action de conformité.  Cela peut être problématique dans de nombreux cas enfants où le verrouillage de compte peut se produire entre le temps de sortie et la mise en éveil, en particulier si la fenêtre de maintenance est courante pendant cette période.  
  
## <a name="additional-resources"></a>Ressources complémentaires  
**Table SEQ Table \\ @ no__t-2 arabe 3 : Glossaire connexion @ no__t-0  
  
|Terme|Définition|  
|----|-------|  
|Autologon|Le logo automatique est une fonctionnalité qui est présente dans Windows pour plusieurs versions.  Il s’agit d’une fonctionnalité documentée de Windows qui a même des outils tels que l’ouverture de logos automatique pour Windows v 3.01  *[http:/technet. Microsoft. com/Sysinternals/bb963905. aspx](https://technet.microsoft.com/sysinternals/bb963905.aspx)*<br /><br />Il permet à un seul utilisateur de l’appareil de se connecter automatiquement sans entrer d’informations d’identification. Les informations d’identification sont configurées et stockées dans le registre en tant que secret LSA chiffré.|  
  

