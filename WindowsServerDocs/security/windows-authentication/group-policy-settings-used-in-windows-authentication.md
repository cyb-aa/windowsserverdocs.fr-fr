---
title: "Paramètres de stratégie de groupe utilisés dans l’authentification Windows"
description: "Sécurité de Windows Server"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="group-policy-settings-used-in-windows-authentication"></a>Paramètres de stratégie de groupe utilisés dans l’authentification Windows

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique de référence destinée aux professionnels de l’informatique décrit l’utilisation et l’impact des paramètres de stratégie de groupe dans le processus d’authentification.

Vous pouvez gérer l’authentification dans les systèmes d’exploitation Windows en ajoutant des comptes de service, d’ordinateur et utilisateur aux groupes, puis en appliquant des stratégies d’authentification à ces groupes. Ces stratégies sont définies en tant que les stratégies de sécurité locale et modèles d’administration, également connu sous les paramètres de stratégie de groupe. Les deux jeux peut être configurés et distribuées dans votre organisation à l’aide de stratégie de groupe.

> [!NOTE]
> Les fonctionnalités introduites dans Windows Server 2012 R2, vous permettent de configurer des stratégies d’authentification pour les services ciblés ou applications, généralement appelé silos d’authentification, à l’aide des comptes protégés. Pour plus d’informations sur la procédure à suivre dans Active Directory, consultez [comment configurer des comptes protégés](how-to-configure-protected-accounts.md).

Par exemple, vous pouvez appliquer les stratégies suivantes à des groupes, selon leur fonction dans l’organisation:

-   Ouvrez une session localement ou à un domaine

-   Ouvrez une session sur un réseau

-   Réinitialiser les comptes

-   Créer des comptes

Le tableau suivant répertorie les groupes pertinents pour l’authentification de la stratégie et fournit des liens vers la documentation qui peuvent vous aider à configurer ces stratégies.

|Stratégie de groupe|Emplacement|Description|
|--------|------|--------|
|**Stratégie de mot de passe**|Ordinateur local\Configuration ordinateur\Paramètres Windows\Paramètres de Sécurité\stratégies|Les stratégies de mot de passe affectent les caractéristiques et le comportement des mots de passe. Les stratégies de mot de passe sont utilisés pour les comptes de domaine ou les comptes d’utilisateurs locaux. Elles déterminent les paramètres des mots de passe, tels que la durée de vie et de l’application.<br /><br />Pour plus d’informations sur les paramètres spécifiques, consultez [stratégie de mot de passe](https://technet.microsoft.com/itpro/windows/keep-secure/password-policy).|
|**Stratégie de verrouillage de compte**|Ordinateur local\Configuration ordinateur\Paramètres Windows\Paramètres de Sécurité\stratégies|Options de stratégie de verrouillage du compte désactivent les comptes après un certain nombre de tentatives d’ouverture de session ayant échoué. À l’aide de ces options peut vous aider à détecter et bloquer les tentatives de coupure de mots de passe.<br /><br />Pour plus d’informations sur les options de stratégie de verrouillage de compte, consultez [stratégie de verrouillage de compte](https://technet.microsoft.com/itpro/windows/keep-secure/account-lockout-policy).|
|**Stratégie Kerberos**|Ordinateur local\Configuration ordinateur\Paramètres Windows\Paramètres de Sécurité\stratégies|Paramètres liés à Kerberos incluent dans les règles de mise en œuvre et de durée de vie de ticket. Stratégie Kerberos ne s’applique pas aux bases de données de compte local, car le protocole d’authentification Kerberos n’est pas utilisé pour authentifier les comptes locaux. Par conséquent, les paramètres de stratégie Kerberos peuvent être configurés uniquement au moyen de l’objet de stratégie de groupe (GPO), où elle affecte des ouvertures de session de domaine du domaine par défaut.<br /><br />Pour plus d’informations sur les options de stratégie Kerberos du contrôleur de domaine, consultez [stratégie Kerberos](https://technet.microsoft.com/itpro/windows/keep-secure/kerberos-policy).|
|**Stratégie d’audit**|Ordinateur local\Configuration ordinateur\Paramètres Settings\Security Settings\Local Policies\Audit Policy|Stratégie d’audit vous permet de contrôler et de comprendre l’accès aux objets, tels que les fichiers et dossiers et pour gérer les comptes d’utilisateur et groupe et les ouvertures de session utilisateur et fermetures de session. Stratégies d’audit peuvent spécifier les catégories d’événements que vous souhaitez contrôler, définir la taille et le comportement du journal de sécurité et de déterminer les objets que vous souhaitez surveiller l’accès et le type d’accès que vous souhaitez analyser.<br /><br />|
|**Attribution des droits utilisateur**|Ordinateur local\Configuration ordinateur\Paramètres Windows\Paramètres sécurité\Stratégies Locales\attribution des droits|Droits d’utilisateur sont attribués en général sur la base des groupes de sécurité à laquelle appartient un utilisateur, telles que les utilisateurs, utilisateurs avec pouvoir ou administrateurs. Les paramètres de stratégie dans cette catégorie sont généralement utilisés pour accorder ou refuser des autorisations pour accéder à un ordinateur basé sur la méthode d’accès et sécurité appartenances à un groupe.|
|**Options de sécurité**|Ordinateur local local\Configuration ordinateur\Paramètres Windows\Paramètres de sécurité\Stratégies Locales\options|Les stratégies d’authentification sont les suivantes:<br /><br />-Les appareils<br />-Contrôleur de domaine<br />-Membre de domaine<br />-Ouverture de session interactive<br />: Serveur réseau Microsoft<br />: Accès réseau<br />: Sécurité réseau<br />-Console de récupération<br />-Arrêt<br /><br />|
|**Délégation des informations d’identification**|Délégation d’informations ordinateur configuration|La délégation des informations d’identification est un mécanisme qui permet aux informations d’identification locales être utilisé sur d’autres systèmes, notamment les serveurs membres et les contrôleurs de domaine au sein d’un domaine. Ces paramètres s’appliquent aux applications à l’aide de Credential Security Support Provider (SSP Cred). Connexion Bureau à distance est un exemple.|
|**KDC**|Templates\System\KDC de configuration de l’ordinateur\Modèles d’ordinateur|Ces paramètres de stratégie affectent la manière dont le centre de Distribution de clés (KDC), qui est un service sur le contrôleur de domaine, gère les demandes d’authentification Kerberos.|
|**Kerberos**|Templates\System\Kerberos de configuration de l’ordinateur\Modèles d’ordinateur|Ces paramètres de stratégie affectent comment Kerberos est configuré pour gérer la prise en charge des revendications, le blindage Kerberos, l’authentification composée, les serveurs proxy identification et autres configurations.|
|**Ouverture de session**|Remarque de configuration d’ordinateur|Ces paramètres de stratégie de contrôlent la façon dont le système présente l’expérience d’ouverture de session des utilisateurs.|
|**Ouverture de session réseau**|Ouverture de session ordinateur Configuration ordinateur\Modèles Templates\System\Net|Ces paramètres de stratégie de contrôlent la façon dont le système gère les demandes d’ouverture de session réseau, y compris comment se comporte le localisateur de contrôleur de domaine.<br /><br />Pour plus d’informations sur comment le localisateur de contrôleur de domaine s’inscrit dans le processus de réplication, voir [présentation de la réplication entre Sites](https://technet.microsoft.com/library/cc771251.aspx).|
|**Biométrie**|Ordinateur Configuration ordinateur\Modèles d’administration\Composants Components\Biometrics|En règle générale, ces paramètres de stratégie autorisent ou interdire l’utilisation de la biométrie comme méthode d’authentification.<br /><br />Pour plus d’informations sur l’implémentation de la biométrie Windows, consultez le Windows Biometric Framework Overview.|
|**Interface utilisateur d’informations d’identification**|Ordinateur Configuration ordinateur\Modèles d’administration\Composants Windows\interface utilisateur|Ces paramètres de stratégie de contrôlent la façon dont les informations d’identification sont gérées au point d’entrée.|
|**Synchronisation de mot de passe**|Ordinateur Configuration ordinateur\Modèles d’administration\Composants Components\Password synchronisation|Ces paramètres de stratégie déterminent comment le système gère la synchronisation des mots de passe entre Windows et les systèmes d’exploitation basés sur UNIX.<br /><br />Pour plus d’informations, voir [synchronisation de mot de passe](https://technet.microsoft.com/library/cc732609.aspx).|
|**Carte à puce**|Carte d’ordinateur Configuration ordinateur\Modèles d’administration\Composants Components\Smart|Ces paramètres de stratégie de contrôlent la façon dont le système gère les ouvertures de session de carte à puce.<br /><br />|
|**Options d’ouverture de session de Windows**|Options d’ouverture de session ordinateur Configuration ordinateur\Modèles d’administration\Composants Windows\Windows|Ces paramètres de stratégie de contrôlent quand et comment les opportunités d’ouverture de session sont disponibles.|
|**Options Ctrl + Alt + Suppr**|Options d’ordinateur Configuration ordinateur\Modèles d’administration\Composants Components\Ctrl + Alt + Suppr|Ces paramètres de stratégie affectent l’apparence du et l’accessibilité aux fonctionnalités dans l’interface utilisateur (bureau sécurisé) d’ouverture de session, tels que le Gestionnaire des tâches et le verrouillage du clavier de l’ordinateur.|
|**Ouverture de session**|Ordinateur Configuration ordinateur\Modèles d’administration\Composants Components\Logon|Ces paramètres de stratégie de déterminent si ou les processus qui peuvent s’exécuter lorsque l’utilisateur se connecte.|

## <a name="see-also"></a>Voir aussi
[Présentation technique de l’authentification Windows](windows-authentication-technical-overview.md)


