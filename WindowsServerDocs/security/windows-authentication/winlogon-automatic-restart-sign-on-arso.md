---
title: Connexion de redémarrage automatique Winlogon
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 172eb34fbfdb8a91adf55e35f888e90f5688d0e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849240"
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Connexion de redémarrage automatique Winlogon

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

**Auteur**: Justin Turner, ingénieur Support résolution Senior auprès du groupe Windows  
  
> [!NOTE]  
> Ce contenu est écrit par un ingénieur du support client Microsoft et est destiné aux administrateurs expérimentés et aux architectes système qui recherchent des explications techniques plus approfondies des fonctionnalités et des solutions Windows Server 2012 R2 que n'en proposent généralement les rubriques de TechNet. Toutefois, il n'a pas subi les mêmes passes de correction. De ce fait, une partie du langage peut sembler moins finalisée que le contenu de TechNet.  
  
## <a name="overview"></a>Vue d'ensemble  
Windows 8 introduit des applications d’écran de verrouillage.  Ce sont les applications qui s’exécutent et affichent les notifications pendant que la session utilisateur est verrouillée (calendrier des rendez-vous, des e-mails et des messages, etc..).  Les appareils qui sont redémarrés en raison du processus de mise à jour de Windows ne parviennent pas à afficher ces notifications de verrouillage d’écran après le redémarrage.  Certains utilisateurs dépendent de ces applications d’écran de verrouillage.  
  
## <a name="whats-changed"></a>Nouveautés  
Lorsqu’un utilisateur se connecte sur un appareil Windows 8.1, LSA enregistre les informations d’identification chiffrée mémoire accessible uniquement par lsass.exe. Lors de la mise à jour Windows lance un redémarrage automatique sans la présence de l’utilisateur, ces informations d’identification seront utilisées pour configurer l’ouverture de session automatique pour l’utilisateur. Mise à jour de Windows en cours d’exécution en tant que système avec le privilège TCB lance l’appel RPC pour ce faire.  
  
Après le redémarrage, l’utilisateur sera automatiquement être connecté via le mécanisme d’ouverture de session automatique et plus verrouillé pour protéger la session utilisateur. Le verrouillage est lancé via Winlogon tandis que la gestion d’informations d’identification est effectuée par la LSA.  En automatiquement la signature et le verrouillage de l’utilisateur sur la console, applications d’écran de verrouillage de l’utilisateur seront redémarrées et disponibles.  
  
> [!NOTE]  
> Après une mise à jour Windows induites par le redémarrage, le dernier utilisateur interactif est automatiquement connecté et la session est verrouillée par conséquent, verrou écran applications l’utilisateur peuvent s’exécuter.  
  
![Capture d’écran montrant l’écran de verrouillage](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_LockScreenApp.gif)  
  
![Capture d’écran montrant le verrou écran applications](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_LockScreen.gif)  
  
**Vue d’ensemble rapide**  
  
-   Mise à jour de Windows nécessite un redémarrage  
  
-   Est l’ordinateur capable de redémarrer (*aucune application en cours d’exécution qui ne perdrait données*) ?  
  
    -   Redémarrage pour vous  
  
    -   Reconnectez-vous  
  
    -   Machine de verrou  
  
-   Activé ou désactivé par la stratégie de groupe  
  
    -   Désactivé par défaut dans les versions serveur  
  
-   Pourquoi ?  
  
    -   Certaines mises à jour ne peut pas terminer jusqu'à ce que l’utilisateur se connecte dans.  
  
    -   Une meilleure expérience utilisateur : n’êtes pas obligé d’attendre 15 minutes pour terminer l’installation des mises à jour  
  
-   Comment ? AutoLogon  
  
    -   stocke le mot de passe, utilise ces informations d’identification pour vous connecter  
  
    -   enregistre les informations d’identification en tant que secret LSA dans la mémoire paginée  
  
    -   Peut uniquement être activée si BitLocker est activé.  
  
## <a name="group-policy-sign-in-last-interactive-user-automatically-after-a-system-initiated-restart"></a>Stratégie de groupe : Connectez-vous dernier utilisateur interactif automatiquement après un redémarrage initié par le système  
Dans Windows 8.1 / Windows Server 2012 R2, ouverture de session automatique de l’utilisateur d’écran de verrouillage après le redémarrage de la mise à jour Windows opter pour les références (SKU) de serveur et refuser pour les références (SKU) de Client.  
  
**Emplacement de la stratégie :** Configuration ordinateur > stratégies > modèles d’administration > composants de Windows > Option d’ouverture de session Windows  
  
**Nom de la stratégie :** Connectez-vous dernier utilisateur interactif automatiquement après un redémarrage initié par le système  
  
**Pris en charge :** Au minimum Windows Server 2012 R2, Windows 8.1 ou Windows RT 8.1  
  
**Description/support :**  
  
Ce paramètre de stratégie détermine si un appareil sera automatiquement connectez-vous le dernier utilisateur interactif après mise à jour Windows redémarre le système.  
  
Si vous activez ou ne configurez pas ce paramètre de stratégie, l’appareil enregistre en toute sécurité les informations d’identification de l’utilisateur (y compris le nom d’utilisateur, le domaine et le mot de passe chiffré) pour configurer dans l’authentification automatique après le redémarrage d’une mise à jour de Windows. Après le redémarrage de la mise à jour de Windows, l’utilisateur est automatiquement signé dans et la session est verrouillée automatiquement avec toutes les applications d’écran de verrouillage configurées pour cet utilisateur.  
  
Si vous désactivez ce paramètre de stratégie, l’appareil ne stocke pas les informations d’identification de l’utilisateur pour la connexion automatique après le redémarrage d’une mise à jour de Windows. Applications d’écran de verrouillage utilisateurs ne sont pas redémarrées après le redémarrage du système.  
  
**Éditeur du Registre**  
  
|Nom de valeur|Type|Données|  
|-------|----|----|  
|DisableAutomaticRestartSignOn|DWORD|0<br /><br />**Exemple :**<br /><br />0 (activé)<br /><br />1 (désactivé)|  
  
**Emplacement de Registre de stratégie :** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System  
  
**Type :** DWORD  
  
**Nom du Registre :** DisableAutomaticRestartSignOn  
  
Valeur : 0 ou 1  
  
0 = activé  
  
1 = désactivé  
  
![Capture d’écran montrant le paramètre de stratégie de contrôle d’interface utilisateur où vous pouvez spécifier si un appareil sera automatiquement connectez-vous le dernier utilisateur interactif après le redémarrage du système de mise à jour de Windows](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_SignInPolicy.gif)  
  
## <a name="troubleshooting"></a>Résolution des problèmes  
Lorsque le verrouillage automatique de WinLogon, suivi d’état de WinLogon est stockée dans le journal des événements WinLogon.  
  
L’état d’une tentative de configuration d’ouverture de session automatique est enregistré  
  
-   Si l’opération a réussi  
  
    -   Il enregistre en tant que tel  
  
-   S’il s’agit d’une défaillance :  
  
    -   Quel était l’échec des enregistrements  
  
-   Lorsque état de BitLocker change :  
  
    -   la suppression des informations d’identification doivent être enregistrée  
  
        -   Celles-ci seront stockées dans le journal des opérations de LSA.  
  
### <a name="reasons-why-autologon-might-fail"></a>Raisons pourquoi l’ouverture de session automatique peut échouer  
Il existe plusieurs cas dans lequel une connexion automatique de l’utilisateur n’est pas possible.  Cette section est destinée à capturer les scénarios connus dans lequel cela peut se produire.  
  
### <a name="user-must-change-password-at-next-login"></a>Utilisateur doit changer de mot de passe à la prochaine connexion  
Connexion de l’utilisateur peut entrer dans un état bloqué lors de la modification de mot de passe à la prochaine connexion est requise.  Cela peut être détecté avant redémarrage dans la plupart des cas, mais pas tous (par exemple, expiration du mot de passe peut être atteint entre la fermeture et la prochaine connexion.  
  
### <a name="user-account-disabled"></a>Compte utilisateur désactivé  
Une session utilisateur existante peut être maintenue même si désactivé.  Redémarrage d’un compte qui est désactivé peut être détectée localement dans la plupart des cas à l’avance, en fonction de la stratégie de groupe qu'est peut-être pas pour les comptes de domaine (un domaine mis en cache travail des scénarios de connexion même si le compte est désactivé sur le contrôleur de domaine).  
  
### <a name="logon-hours-and-parental-controls"></a>Les heures d’ouverture de session et des contrôles parentaux  
Les heures d’ouverture de session et le contrôle parental peut interdire une nouvelle session utilisateur en cours de création.  Si un redémarrage se produit pendant cette fenêtre, l’utilisateur n’est pas autorisé à se connecter.  Il existe des stratégies supplémentaires qui provoque le verrouillage ou déconnexion en tant qu’une action de conformité.  Cela peut être problématique pour les nombreux cas enfant où le verrouillage de compte peut s’écouler entre lit et de veille, en particulier si la fenêtre de maintenance est couramment pendant ce temps.  
  
## <a name="additional-resources"></a>Ressources complémentaires  
**Table SEQ Table \\ \* arabe 3 : Glossaire ARSO**  
  
|Terme|Définition|  
|----|-------|  
|Autologon|Ouverture de session automatique est une fonctionnalité qui était présente dans Windows pour plusieurs versions.  C’est une fonctionnalité documentée de Windows qui a encore des outils tels que version 3.01 d’ouverture de session automatique pour Windows  *[http:/technet.microsoft.com/sysinternals/bb963905.aspx](https://technet.microsoft.com/sysinternals/bb963905.aspx)*<br /><br />Il permet à un utilisateur unique de l’appareil pour vous connecter automatiquement sans entrer les informations d’identification. Les informations d’identification sont configurées et stockées dans le Registre en tant que secret LSA chiffré.|  
  

