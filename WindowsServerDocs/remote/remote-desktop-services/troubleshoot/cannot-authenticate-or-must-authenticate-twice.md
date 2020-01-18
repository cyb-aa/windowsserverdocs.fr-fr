---
title: L’utilisateur ne peut pas s’authentifier ou doit s’authentifier deux fois
description: 'Résolution du problème suivant : l’utilisateur ne peut pas s’authentifier ou doit s’authentifier deux fois quand il établit une connexion Bureau à distance.'
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 7dbb037e335af52dacbc56c920b1776be995e753
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265931"
---
# <a name="user-cant-authenticate-or-must-authenticate-twice"></a>L’utilisateur ne peut pas s’authentifier ou doit s’authentifier deux fois

Cet article aborde plusieurs problèmes pouvant affecter l’authentification utilisateur.

## <a name="access-denied-restricted-type-of-logon"></a>Accès refusé, type de connexion restreint

Dans cette situation, un utilisateur de Windows 10 qui tente de se connecter à des ordinateurs Windows 10 ou Windows Server2016 se voit refuser l’accès avec le message suivant :

> Connexion Bureau à distance :  
> L’administrateur système a restreint le type de connexion (réseau ou interactive) que vous pouvez utiliser. Demandez de l’aide à votre administrateur ou votre support technique.

Ce problème se produit quand l’authentification au niveau du réseau est nécessaire pour les connexions RDP, et que l’utilisateur n’est pas membre du groupe **Utilisateurs du Bureau à distance**. Il peut également se produire si le groupe **Utilisateurs du Bureau à distance** n’a pas été affecté au droit d’utilisateur **Accéder à cet ordinateur à partir du réseau**.

Pour résoudre ce problème, effectuez l’une des opérations suivantes :

  - [Modifier l’attribution des droits de l’utilisateur ou l’appartenance au groupe de l’utilisateur](#modify-the-users-group-membership-or-user-rights-assignment)
  - Désactiver l’authentification au niveau du réseau (non recommandé)
  - Utilisez des clients de Bureau à distance autres que Windows 10. Les clients Windows 7, par exemple, n’ont pas ce problème.

### <a name="modify-the-users-group-membership-or-user-rights-assignment"></a>Modifier l’attribution des droits de l’utilisateur ou l’appartenance au groupe de l’utilisateur

Si ce problème concerne un seul utilisateur, la solution la plus simple consiste à ajouter l’utilisateur au groupe **Utilisateurs du Bureau à distance**.

Si l’utilisateur est déjà membre de ce groupe (ou si plusieurs membres du groupe ont le même problème), vérifiez la configuration des droits de l’utilisateur sur l’ordinateur distant Windows 10 ou Windows Server 2016.

1. Ouvrez l’Éditeur d’objets de stratégie de groupe et connectez-vous à la stratégie locale de l’ordinateur distant.
2. Accédez à **Configuration ordinateur\\Paramètres Windows\\Paramètres de sécurité\\Stratégies locales\\Attribution des droits utilisateur**, cliquez avec le bouton droit sur **Accéder à cet ordinateur à partir du réseau**, puis sélectionnez **Propriétés**.
3. Vérifiez si **Utilisateurs du Bureau à distance** (ou un groupe parent) figure dans la liste des utilisateurs et groupes.
4. Si la liste n’inclut pas **Utilisateurs du Bureau à distance** ou un groupe parent comme **Tout le monde**, vous devez l’ajouter à la liste. Si votre déploiement comprend plusieurs ordinateurs, utilisez un objet de stratégie de groupe.  
    Par exemple, l’appartenance par défaut pour **Accéder à cet ordinateur à partir du réseau** inclut **Tout le monde**. Si votre déploiement utilise un objet de stratégie de groupe pour supprimer **Tout le monde**, vous devrez peut-être restaurer l’accès en mettant à jour l’objet de stratégie de groupe de façon à ajouter **Utilisateurs du Bureau à distance**.

## <a name="access-denied-a-remote-call-to-the-sam-database-has-been-denied"></a>Accès refusé, un appel à distance à la base de données SAM a été refusé

Ce comportement est susceptible de se produire si vos contrôleurs de domaine exécutent Windows Server 2016 ou version ultérieure, et que les utilisateurs tentent de se connecter à l’aide d’une application de connexion personnalisée. Les applications qui accèdent aux informations de profil de l’utilisateur dans Active Directory, notamment, se verront refuser l’accès.

Ce comportement est dû à une modification dans Windows. Dans Windows Server 2012 R2 et les versions antérieures, quand un utilisateur se connecte à un Bureau à distance, le Gestionnaire des connexions d’accès à distance contacte le contrôleur de domaine afin d’interroger les configurations propres au Bureau à distance sur l’objet utilisateur dans Active Directory Domain Services (AD DS). Ces informations sont affichées sous l’onglet Profil des services Bureau à distance des propriétés d’objets d’un utilisateur dans le composant logiciel enfichable MMC Utilisateurs et ordinateurs Active Directory.

À compter de Windows Server 2016, le Gestionnaire des connexions d’accès à distance n’interroge plus l’objet utilisateur dans AD DS. S’il est nécessaire que le Gestionnaire des connexions d’accès à distance interroge les services AD DS car vous utilisez des attributs des Services Bureau à distance, vous devez activer la requête manuellement.

> [!IMPORTANT]  
> Suivez attentivement les étapes décrites dans cette section. De graves problèmes peuvent se produire si vous modifiez le Registre de façon incorrecte. Avant de le modifier, [sauvegardez le Registre afin de pouvoir le restaurer](https://support.microsoft.com/help/322756) en cas de problème.

Pour activer le comportement hérité du Gestionnaire des connexions d’accès à distance sur un serveur Hôte de session Bureau à distance, configurez les entrées de Registre suivantes, puis redémarrez le service **Services Bureau à distance** :  
  - **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Policies\\Microsoft\\Windows NT\\Terminal Services**
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<Winstation name\>\\**  
      - Nom : **fQueryUserConfigFromDC**
      - Tapez : **Reg\_DWORD**
      - Valeur : **1** (Décimal)

Pour activer le comportement hérité du Gestionnaire des connexions d’accès à distance sur un serveur autre qu’un serveur Hôte de session Bureau à distance, configurez ces entrées de Registre ainsi que l’entrée de Registre supplémentaire suivante (puis redémarrez le service) :
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**

Pour plus d’informations sur ce comportement, consultez l’article KB 3200967 [Modifications pour le Gestionnaire de connexion à distance dans Windows Server](https://support.microsoft.com/help/3200967/changes-to-remote-connection-manager-in-windows-server).

## <a name="user-cant-sign-in-using-a-smart-card"></a>L’utilisateur ne peut pas se connecter à l’aide d’une carte à puce

Cette section aborde trois scénarios courants dans lesquels un utilisateur ne peut pas se connecter à un Bureau à distance à l’aide d’une carte à puce.

### <a name="cant-sign-in-with-a-smart-card-in-a-branch-office-with-a-read-only-domain-controller-rodc"></a>Impossible de se connecter avec une carte à puce à une succursale avec un contrôleur de domaine en lecture seule (RODC, read-only domain controller)

Ce problème se produit dans les déploiements qui incluent un serveur RDSH sur un site de filiale qui utilise un RODC. Le serveur RDSH est hébergé dans le domaine racine. Les utilisateurs sur le site de filiale appartiennent à un domaine enfant et utilisent des cartes à puce pour l’authentification. Le RODC est configuré pour mettre en cache les mots de passe des utilisateurs (le RODC appartient au **Groupe de réplication dont le mot de passe RODC est autorisé**). Quand des utilisateurs essaient d’ouvrir des sessions sur le serveur RDSH, ils reçoivent des messages tels que « La tentative d’ouverture de session n’est pas valide. Ceci est dû soit à un nom d’utilisateur incorrect, soit à des informations d’authentification incorrectes. »

Ce problème est dû à la façon dont le contrôleur de domaine racine et le RODC gèrent le chiffrement des informations d’identification de l’utilisateur. Le contrôleur de domaine racine utilise une clé de chiffrement pour chiffrer les informations d’identification, et le RODC fournit la clé de déchiffrement au client. Quand un utilisateur reçoit l’erreur « non valide », cela signifie que les deux clés ne correspondent pas.

Pour contourner ce problème, essayez l’une des méthodes suivantes :

- Modifiez la topologie de votre contrôleur de domaine en désactivant la mise en cache du mot de passe sur le RODC, ou déployez un contrôleur de domaine accessible en écriture sur le site de la succursale.
- Déplacez le serveur RDSH vers le même domaine enfant que les utilisateurs.
- Autorisez les utilisateurs à se connecter sans carte à puce.

Notez que toutes ces solutions demandent des compromis en termes de performances ou de niveau de sécurité.

### <a name="user-cant-sign-in-to-a-windows-server-2008-sp2-computer-using-a-smart-card"></a>L’utilisateur ne peut pas se connecter à un ordinateur Windows Server 2008 SP2 à l’aide d’une carte à puce

Ce problème se produit quand des utilisateurs se connectent à un ordinateur Windows Server 2008 SP2 qui a été mis à jour avec KB4093227 (2018.4B). Quand des utilisateurs tentent de se connecter à l’aide d’une carte à puce, ils se voient refuser l’accès avec des messages tels que « Aucun certificat valide n’a été trouvé. Vérifiez que la carte est bien insérée. » En même temps, l’ordinateur Windows Server enregistre l’événement d’application « Une erreur s’est produite lors du retrait d’un certificat numérique de la carte à puce insérée. Signature non valide. »

Pour résoudre ce problème, mettez à jour l’ordinateur Windows Server avec la nouvelle publication 2018.06 B de KB 4093227, [Description de la mise à jour de sécurité pour la vulnérabilité de déni de service dans le protocole RDP (Remote Desktop Protocol) pour Windows Server 2008 datée du 10 avril 2018](https://support.microsoft.com/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008).

### <a name="cant-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs"></a>Impossible de rester connecté avec une carte à puce, et le service Services Bureau à distance se bloque

Ce problème se produit quand les utilisateurs se connectent à un ordinateur Windows ou Windows Server qui a été mis à jour avec le correctif KB 4056446. Dans un premier temps, l’utilisateur peut être en mesure de se connecter au système à l’aide d’une carte à puce, mais ensuite il reçoit un message d’erreur « SCARD\_E\_NO\_SERVICE ». L’ordinateur distant peut cesser de répondre.

Pour contourner ce problème, redémarrez l’ordinateur distant.

Pour résoudre ce problème, mettez à jour l’ordinateur distant avec le correctif approprié :

  - Windows Server 2008 SP2 : KB 4090928, [Fuites de handles dans le processus lsm.exe et les applications de carte à puce peuvent afficher des erreurs « SCARD\_E\_NO\_SERVICE »](https://support.microsoft.com/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe)
  - Windows Server 2012 R2 : KB 4103724, [17 mai 2018—KB4103724 (préversion du correctif cumulatif mensuel)](https://support.microsoft.com/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 et Windows 10, version 1607 : KB 4103720, [17 mai 2018—KB4103720 (build du système d’exploitation 14393.2273)](https://support.microsoft.com/help/4103720/windows-10-update-kb4103720)

## <a name="if-the-remote-pc-is-locked-the-user-needs-to-enter-a-password-twice"></a>Si le PC distant est verrouillé, l’utilisateur doit entrer un mot de passe à deux reprises

Ce problème peut se produire quand un utilisateur tente de se connecter à un Bureau à distance qui exécute Windows 10 version 1709 dans un déploiement dans lequel les connexions RDP ne nécessitent pas l’authentification au niveau du réseau. Dans ces conditions, si le Bureau à distance a été verrouillé, l’utilisateur doit entrer ses informations d’identification à deux reprises lors de la connexion.

Pour résoudre ce problème, mettez à jour l’ordinateur Windows 10 version 1709 avec le correctif KB 4343893, [30 août 2018—KB4343893 (build du système d’exploitation 16299.637)](https://support.microsoft.com/help/4343893/windows-10-update-kb4343893).

## <a name="user-cant-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages"></a>L’utilisateur ne peut pas se connecter et reçoit des messages « Erreur d’authentification » et « Correction d’oracle de chiffrement CredSSP »

Quand des utilisateurs essaient de se connecter à l’aide de Windows Vista SP2 (ou version ultérieure) ou de Windows Server 2008 SP2 (ou version ultérieure), l’accès leur est refusé et ils reçoivent des messages tels que les suivants :

```
An authentication error has occurred. The function requested is not supported.
...
This could be due to CredSSP encryption oracle remediation
...
```

Le terme « correction d’oracle de chiffrement CredSSP » fait référence à un ensemble de mises à jour de sécurité publiées en mars, avril et mai 2018. CredSSP est un fournisseur d’authentification qui traite les demandes d’authentification pour d’autres applications. Les mises à jour du 13 mars 2018, « 3B » et ultérieures traitent d’une faille de sécurité qui fait qu’un attaquant pourrait relayer les informations d’identification de l’utilisateur pour exécuter du code sur le système cible.

Les mises à jour initiales ont ajouté la prise en charge d’un nouvel objet de stratégie de groupe, Correction d’oracle de chiffrement, qui peut avoir les paramètres suivants :

  - Vulnérable : Les applications clientes qui utilisent CredSSP peuvent revenir aux versions non sécurisées, mais ce comportement expose les Bureaux distants à des attaques. Les services qui utilisent CredSSP acceptent les clients qui n’ont pas été mis à jour.
  - Atténué : les applications clientes qui utilisent CredSSP ne peuvent pas revenir aux versions non sécurisées, mais les services qui utilisent CredSSP acceptent les clients qui n’ont pas été mis à jour.
  - Forcer la mise à jour des clients : les applications clientes qui utilisent CredSSP ne peuvent pas revenir aux versions non sécurisées, et les services qui utilisent CredSSP n’acceptent pas les clients non corrigés. 
    > [!NOTE]
    > Ce paramètre ne doit pas être déployé tant que tous les hôtes distants ne prennent pas en charge la version la plus récente.

La mise à jour du 8 mai 2018 a changé la valeur par défaut de Correction d’oracle de chiffrement de Vulnérable à Atténué. Avec ce changement, les clients Bureau à distance disposant des mises à jour ne peuvent pas se connecter aux serveurs qui n’en disposent pas (ou aux serveurs mis à jour qui n’ont pas été redémarrés). Pour plus d’informations sur les mises à jour de CredSSP, consultez [KB 4093492](https://support.microsoft.com/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018).

Pour résoudre ce problème, mettez à jour et redémarrez tous les systèmes. Pour obtenir une liste complète des mises à jour et pour plus d’informations sur les vulnérabilités, consultez [CVE-2018-0886 | Vulnérabilité d’exécution de code à distance dans CredSSP](https://portal.msrc.microsoft.com/security-guidance/advisory/CVE-2018-0886).

Pour contourner ce problème jusqu’à ce que les mises à jour soient terminées, consultez KB 4093492 afin de connaître les types de connexions autorisés. S’il n’existe aucune alternative possible, vous pouvez envisager l’une des méthodes suivantes :

- Pour les ordinateurs clients affectés, réaffectez la valeur **Vulnérable** à la stratégie Correction d’oracle de chiffrement.
- Modifiez les stratégies suivantes dans le dossier de stratégie de groupe **Configuration ordinateur\\Modèles d’administration\\Composants Windows\\Services Bureau à distance\\Hôte de session Bureau à distance\\ Sécurité** :  
  - **Nécessite l’utilisation d’une couche de sécurité spécifique pour les connexions distantes (RDP)**  : affectez la valeur **Activé** et sélectionnez **RDP**.
  - **Requérir l’authentification utilisateur pour les connexions à distance à l’aide de l’authentification au niveau du réseau** : affectez la valeur **Désactivé**.
    > [!IMPORTANT]  
    > La modification de ces stratégies de groupe altère la sécurité du déploiement. Nous vous recommandons de ne pas les utiliser ou de ne les utiliser que temporairement.

Pour plus d’informations sur l’utilisation de la stratégie de groupe, consultez [Modification d’un objet de stratégie de groupe bloquant](rdp-error-general-troubleshooting.md#modifying-a-blocking-gpo).

## <a name="after-you-update-client-computers-some-users-need-to-sign-in-twice"></a>Après que vous avez mis à jour des ordinateurs clients, certains utilisateurs doivent se connecter à deux reprises

Quand des utilisateurs se connectent au Bureau à distance à l’aide d’un ordinateur exécutant Windows 7 ou Windows 10 version 1709, une deuxième invite de connexion apparaît immédiatement. Ce problème se produit quand l’ordinateur client dispose des mises à jour suivantes :

  - Windows 7 : KB 4103718, [8 mai 2018—KB4103718 (correctif cumulatif mensuel)](https://support.microsoft.com/help/4103718/windows-7-update-kb4103718)
  - Windows 10 1709 : KB 4103727, [8 mai 2018—KB4103727 (build du système d’exploitation 16299.431)](https://support.microsoft.com/help/4103727/windows-10-update-kb4103727)

Pour résoudre ce problème, vérifiez que les ordinateurs auxquels les utilisateurs souhaitent se connecter (ainsi que les serveurs RDSH ou RDVI) sont entièrement mis à jour jusqu’en juin 2018. Celle-ci comprend les mises à jour suivantes :

  - Windows Server 2016 : KB 4284880, [12 juin 2018—KB4284880 (build du système d’exploitation 14393.2312)](https://support.microsoft.com/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2 : KB 4284815, [12 juin 2018—KB4284815 (correctif cumulatif mensuel)](https://support.microsoft.com/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012 : KB 4284855, [12 juin 2018—KB4284855 (correctif cumulatif mensuel)](https://support.microsoft.com/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2 : KB 4284826, [12 juin 2018—KB4284826 (correctif cumulatif mensuel)](https://support.microsoft.com/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 SP2 : KB4056564, [Description de la mise à jour de sécurité pour la vulnérabilité d’exécution de code à distance dans CredSSP pour Windows Server 2008, Windows Embedded POSReady 2009 et Windows Embedded Standard 2009 datée du 13 mars 2018](https://support.microsoft.com/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

## <a name="users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers"></a>Les utilisateurs se voient refuser l’accès sur un déploiement qui utilise Credential Guard à distance avec plusieurs services Broker pour les connexions Bureau à distance

Ce problème se produit dans les déploiements à haute disponibilité qui utilisent plusieurs services Broker pour les connexions Bureau à distance, si Windows Defender Credential Guard à distance est en cours d’utilisation. Les utilisateurs ne peuvent pas se connecter aux Bureaux à distance.

Ce problème se produit car Credential Guard à distance utilise Kerberos pour l’authentification et restreint NTLM. Dans une configuration à haute disponibilité avec équilibrage de charge, les services Broker pour les connexions Bureau à distance ne peuvent pas prendre en charge les opérations Kerberos.

Si vous avez besoin d’utiliser une configuration à haute disponibilité avec des services Broker pour les connexions Bureau à distance à charge équilibrée, vous pouvez contourner ce problème en désactivant Credential Guard à distance. Pour plus d’informations sur la façon de gérer Windows Defender Credential Guard à distance, consultez [Protéger les informations d’identification du Bureau à distance avec Windows Defender Credential Guard à distance](https://docs.microsoft.com/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard).
