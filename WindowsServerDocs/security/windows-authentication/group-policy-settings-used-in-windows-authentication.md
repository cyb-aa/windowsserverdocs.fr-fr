---
title: Paramètres de stratégie de groupe utilisés dans l’authentification Windows
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9e237f89-45b1-4a4e-9b72-11dc7d6a470b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 38df2033b57c0394b96f539f54efe6a3579500f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847930"
---
# <a name="group-policy-settings-used-in-windows-authentication"></a>Paramètres de stratégie de groupe utilisés dans l’authentification Windows

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique de référence pour les professionnels de l’informatique décrit l’utilisation et l’impact des paramètres de stratégie de groupe dans le processus d’authentification.

Vous pouvez gérer l’authentification dans les systèmes d’exploitation Windows en ajoutant des comptes d’utilisateurs, ordinateur et service aux groupes, puis en appliquant des stratégies d’authentification à ces groupes. Ces stratégies sont définies en tant que stratégies de sécurité locale et en tant que modèles d’administration, également connu sous les paramètres de stratégie de groupe. Les deux ensembles peuvent être configurés et distribuées dans votre organisation à l’aide de stratégie de groupe.

> [!NOTE]
> Fonctionnalités introduites dans Windows Server 2012 R2, vous permettent de configurer des stratégies d’authentification pour les services ciblés ou applications, communément appelé silos d’authentification, à l’aide des comptes protégés. Pour plus d’informations sur la procédure à suivre dans Active Directory, consultez [comment configurer des comptes protégés](how-to-configure-protected-accounts.md).

Par exemple, vous pouvez appliquer les stratégies suivantes aux groupes, selon leur fonction dans l’organisation :

-   Ouvrez une session localement ou à un domaine

-   Ouvrez une session sur un réseau

-   Réinitialiser les comptes

-   Créer des comptes

Le tableau suivant répertorie les groupes de stratégies pertinentes pour l’authentification et fournit des liens vers la documentation qui peuvent vous aider à configurer ces stratégies.

|Stratégie de groupe|Location|Description|
|--------|------|--------|
|**Stratégie de mot de passe**|Ordinateur local local\Configuration ordinateur\Paramètres Windows\Paramètres de Sécurité\stratégies|Stratégies de mot de passe affectent les caractéristiques et le comportement des mots de passe. Stratégies de mot de passe sont utilisés pour les comptes de domaine ou des comptes d’utilisateurs locaux. Elles déterminent les paramètres pour les mots de passe, tels que la durée de vie et de l’application.<br /><br />Pour plus d’informations sur les paramètres spécifiques, consultez [stratégie de mot de passe](https://technet.microsoft.com/itpro/windows/keep-secure/password-policy).|
|**Stratégie de verrouillage de compte**|Ordinateur local local\Configuration ordinateur\Paramètres Windows\Paramètres de Sécurité\stratégies|Options de stratégie de verrouillage de compte désactivent les comptes après un nombre défini d’échecs de connexion. À l’aide de ces options peut vous aider à détecter et bloquer les tentatives de violation des mots de passe.<br /><br />Pour plus d’informations sur les options de stratégie de verrouillage de compte, consultez [stratégie de verrouillage de compte](https://technet.microsoft.com/itpro/windows/keep-secure/account-lockout-policy).|
|**Stratégie Kerberos**|Ordinateur local local\Configuration ordinateur\Paramètres Windows\Paramètres de Sécurité\stratégies|Paramètres liés à Kerberos incluent des règles de mise en œuvre et de durée de vie de ticket. Stratégie Kerberos ne s’applique pas aux bases de données de compte local, car le protocole d’authentification Kerberos n’est pas utilisé pour authentifier les comptes locaux. Par conséquent, les paramètres de stratégie Kerberos peuvent être configurés uniquement au moyen de l’objet de stratégie de groupe (GPO), où il affecte les ouvertures de session de domaine du domaine par défaut.<br /><br />Pour plus d’informations sur les options de stratégie Kerberos du contrôleur de domaine, consultez [stratégie Kerberos](https://technet.microsoft.com/itpro/windows/keep-secure/kerberos-policy).|
|**Stratégie d’audit**|Ordinateur local local\Configuration Configuration\Windows Settings\Security Settings\Local Policies\Audit Policy|Stratégie d’audit vous permet de contrôler et de comprendre l’accès aux objets, tels que les fichiers et dossiers et pour gérer les comptes d’utilisateur et de groupe et les ouvertures de session utilisateur et fermetures de session. Stratégies d’audit peuvent spécifier les catégories d’événements à auditer, définir la taille et le comportement du journal de sécurité et déterminer quels objets vous voulez surveiller l’accès et le type d’accès que vous souhaitez analyser.<br /><br />|
|**Attribution des droits utilisateur**|Ordinateur local local\Configuration Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment|Droits d’utilisateur sont généralement affectés en fonction les groupes de sécurité auquel appartient un utilisateur, tels que les administrateurs, utilisateurs avec pouvoir ou utilisateurs. Les paramètres de stratégie de cette catégorie sont généralement utilisés pour accorder ou refuser l’autorisation d’accéder à un ordinateur basé sur la méthode d’accès et sécurité des appartenances.|
|**Options de sécurité**|Ordinateur local local\Configuration ordinateur\Paramètres Windows\Paramètres de sécurité\Stratégies Locales\options|Les stratégies d’authentification sont les suivantes :<br /><br />-Appareils<br />-Contrôleur de domaine<br />: Membre de domaine<br />-Ouverture de session interactive<br />: Serveur réseau Microsoft<br />: Accès réseau<br />-Sécurité réseau<br />-Console de récupération<br />-Arrêt<br /><br />|
|**Délégation des informations d’identification**|Délégation d’informations de configuration de l’ordinateur\Modèles ordinateur|La délégation des informations d’identification est un mécanisme qui permet les informations d’identification locales à être utilisé sur d’autres systèmes, plus particulièrement les serveurs membres et les contrôleurs de domaine dans un domaine. Ces paramètres s’appliquent aux applications à l’aide de Credential Security Support Provider (SSP Cred). Connexion Bureau à distance est un exemple.|
|**KDC**|Templates\System\KDC de configuration de l’ordinateur\Modèles d’ordinateur|Ces paramètres de stratégie affectent la façon dont le centre de Distribution de clés (KDC), qui est un service sur le contrôleur de domaine, gère les demandes d’authentification Kerberos.|
|**Kerberos**|Templates\System\Kerberos de configuration de l’ordinateur\Modèles d’ordinateur|Ces paramètres de stratégie affectent la configuration de Kerberos pour gérer la prise en charge des revendications, le blindage Kerberos, l’authentification composée, serveurs proxy d’identification et d’autres configurations.|
|**Logon**|Configuration ordinateur\Modèles d’administration\Système\Ouverture de session|Ces paramètres de stratégie contrôlent comment le système présente l’expérience d’ouverture de session des utilisateurs.|
|**Ouverture de session réseau**|Ouverture de session ordinateur Configuration ordinateur\Modèles Templates\System\Net|Ces paramètres de stratégie de contrôlent la façon dont le système gère les demandes d’ouverture de session réseau, y compris comment se comporte le localisateur de contrôleur de domaine.<br /><br />Pour plus d’informations sur la façon dont le localisateur de contrôleur de domaine s’intègre dans le processus de réplication, consultez [comprendre la réplication entre des Sites](https://technet.microsoft.com/library/cc771251.aspx).|
|**Biométrie**|Ordinateur configuration de l’ordinateur\Modèles administratifs\Composants Components\Biometrics|En règle générale, ces paramètres de stratégie autorisent ou interdire l’utilisation de la biométrie comme méthode d’authentification.<br /><br />Pour plus d’informations sur l’implémentation de Windows de la biométrie, consultez Présentation de Windows Biometric Framework.|
|**Interface utilisateur d’informations d’identification**|Configuration de l’ordinateur\Modèles administratifs\Composants de Windows\interface utilisateur|Ces paramètres de stratégie de contrôlent la façon dont les informations d’identification sont gérées au point d’entrée.|
|**Synchronisation de mot de passe**|Ordinateur configuration de l’ordinateur\Modèles administratifs\Composants Components\Password synchronisation|Ces paramètres de stratégie déterminent comment le système gère la synchronisation des mots de passe entre Windows et les systèmes d’exploitation basés sur UNIX.<br /><br />Pour plus d’informations, consultez [synchronisation de mot de passe](https://technet.microsoft.com/library/cc732609.aspx).|
|**Carte à puce**|Carte d’ordinateur configuration de l’ordinateur\Modèles administratifs\Composants Components\Smart|Ces paramètres de stratégie contrôlent la façon dont le système gère les ouvertures de session de carte à puce.<br /><br />|
|**Options d’ouverture de session de Windows**|Options d’ouverture de session d’ordinateur configuration de l’ordinateur\Modèles administratifs\Composants Windows\Windows|Ces paramètres de stratégie de contrôlent quand et comment les opportunités d’ouverture de session sont disponibles.|
|**Options de Ctrl + Alt + Suppr**|Options d’ordinateur configuration de l’ordinateur\Modèles administratifs\Composants Components\Ctrl + Alt + Suppr|Ces paramètres de stratégie affectent l’apparence d’et l’accessibilité aux fonctionnalités de l’interface utilisateur (bureau sécurisé) de l’ouverture de session, tel que le Gestionnaire des tâches et le verrouillage du clavier de l’ordinateur.|
|**Logon**|Ordinateur configuration de l’ordinateur\Modèles administratifs\Composants Components\Logon|Ces paramètres de stratégie déterminent si ou les processus qui peuvent s’exécuter lorsque l’utilisateur ouvre une session.|

## <a name="see-also"></a>Voir aussi
[Présentation technique de l’authentification Windows](windows-authentication-technical-overview.md)


