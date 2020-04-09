---
title: Paramètres de stratégie de groupe utilisés dans l’authentification Windows
description: Sécurité de Windows Server
ms.prod: windows-server
ms.technology: security-windows-auth
ms.topic: article
ms.assetid: 9e237f89-45b1-4a4e-9b72-11dc7d6a470b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 7acd7439ec2e0382f1e2c725d66363e6b3a50b40
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857572"
---
# <a name="group-policy-settings-used-in-windows-authentication"></a>Paramètres de stratégie de groupe utilisés dans l’authentification Windows

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique de référence pour les professionnels de l’informatique décrit l’utilisation et l’impact des paramètres de stratégie de groupe dans le processus d’authentification.

Vous pouvez gérer l’authentification dans les systèmes d’exploitation Windows en ajoutant des comptes d’utilisateur, d’ordinateur et de service à des groupes, puis en appliquant des stratégies d’authentification à ces groupes. Ces stratégies sont définies en tant que stratégies de sécurité locales et en tant que modèles d’administration, également appelés paramètres de stratégie de groupe. Les deux ensembles peuvent être configurés et distribués au sein de votre organisation à l’aide de stratégie de groupe.

> [!NOTE]
> Les fonctionnalités introduites dans Windows Server 2012 R2 vous permettent de configurer des stratégies d’authentification pour des services ou applications ciblés, communément appelées silos d’authentification, à l’aide de comptes protégés. Pour plus d’informations sur la procédure à suivre dans Active Directory, voir [How to configure protected Accounts](how-to-configure-protected-accounts.md).

Par exemple, vous pouvez appliquer les stratégies suivantes à des groupes, en fonction de leur fonction dans l’Organisation :

-   Ouvrir une session localement ou dans un domaine

-   Ouvrir une session sur un réseau

-   Réinitialiser les comptes

-   Créer des comptes

Le tableau suivant répertorie les groupes de stratégies relatifs à l’authentification et fournit des liens vers la documentation qui peut vous aider à configurer ces stratégies.

|Groupe de stratégies|Location|Description|
|--------|------|--------|
|**Stratégie de mot de passe**|Ordinateur local Local\configuration configuration \ paramètres système \ stratégies de Settings\Account|Les stratégies de mot de passe affectent les caractéristiques et le comportement des mots de passe. Les stratégies de mot de passe sont utilisées pour les comptes de domaine ou les comptes d’utilisateurs locaux. Ils déterminent les paramètres pour les mots de passe, tels que la mise en œuvre et la durée de vie.<p>Pour plus d’informations sur des paramètres spécifiques, consultez [stratégie de mot de passe](https://technet.microsoft.com/itpro/windows/keep-secure/password-policy).|
|**Stratégie de verrouillage de compte**|Ordinateur local Local\configuration configuration \ paramètres système \ stratégies de Settings\Account|Les options de stratégie de verrouillage de compte désactivent les comptes après un nombre défini de tentatives de connexion ayant échoué. L’utilisation de ces options peut vous aider à détecter et à bloquer les tentatives d’interruption des mots de passe.<p>Pour plus d’informations sur les options de stratégie de verrouillage de compte, consultez [stratégie de verrouillage de compte](https://technet.microsoft.com/itpro/windows/keep-secure/account-lockout-policy).|
|**Stratégie Kerberos**|Ordinateur local Local\configuration configuration \ paramètres système \ stratégies de Settings\Account|Les paramètres liés à Kerberos incluent la durée de vie des tickets et les règles de mise en application. La stratégie Kerberos ne s’applique pas aux bases de données de comptes locaux car le protocole d’authentification Kerberos n’est pas utilisé pour authentifier les comptes locaux. Par conséquent, les paramètres de stratégie Kerberos ne peuvent être configurés qu’au moyen de l’objet de stratégie de groupe de domaine par défaut, dans lequel il affecte les ouvertures de session de domaine.<p>Pour plus d’informations sur les options de stratégie Kerberos pour le contrôleur de domaine, consultez [stratégie Kerberos](https://technet.microsoft.com/itpro/windows/keep-secure/kerberos-policy).|
|**Stratégie d’audit**|Stratégie de \ de l’ordinateur local Local\configuration \ paramètres de configuration|La stratégie d’audit vous permet de contrôler et de comprendre l’accès aux objets, tels que les fichiers et les dossiers, et de gérer les comptes d’utilisateurs et de groupes, ainsi que les ouvertures de session et les ouvertures de session utilisateur. Les stratégies d’audit peuvent spécifier les catégories d’événements que vous voulez auditer, définir la taille et le comportement du journal de sécurité, et déterminer les objets dont vous souhaitez surveiller l’accès et le type d’accès que vous souhaitez surveiller.<p>|
|**Attribution des droits utilisateur**|Ordinateur local Local\configuration \ paramètres de configuration \ stratégies d’autorisation Locales\attribution des droits|Les droits d’utilisateur sont généralement affectés sur la base des groupes de sécurité auxquels appartient un utilisateur, par exemple administrateurs, utilisateurs avec pouvoir ou utilisateurs. Les paramètres de stratégie de cette catégorie sont généralement utilisés pour accorder ou refuser l’autorisation d’accéder à un ordinateur en fonction de la méthode d’accès et des appartenances aux groupes de sécurité.|
|**Options de sécurité**|Ordinateur local Local\configuration configuration \ paramètres de configuration \ stratégies|Les stratégies relatives à l’authentification sont les suivantes :<p>-Appareils<br />-Contrôleur de domaine<br />-Membre du domaine<br />-Ouverture de session interactive<br />-Serveur réseau Microsoft<br />-Accès réseau<br />-Sécurité réseau<br />-Console de récupération<br />-Shutdown<p>|
|**Délégation des informations d’identification**|Configuration de l’ordinateur \ délégation Templates\System\Credentials|La délégation des informations d’identification est un mécanisme qui permet d’utiliser les informations d’identification locales sur d’autres systèmes, notamment les serveurs membres et les contrôleurs de domaine au sein d’un domaine. Ces paramètres s’appliquent aux applications à l’aide du fournisseur de support de sécurité des informations d’identification (CRED SSP). Connexion Bureau à distance en est un exemple.|
|**Centre**|Configuration de l’ordinateur \ Templates\System\KDC|Ces paramètres de stratégie affectent la manière dont le centre de distribution de clés (KDC), qui est un service sur le contrôleur de domaine, gère les demandes d’authentification Kerberos.|
|**Clé**|Configuration de l’ordinateur \ Templates\System\Kerberos|Ces paramètres de stratégie affectent la manière dont Kerberos est configuré pour gérer la prise en charge des revendications, du blindage Kerberos, de l’authentification composée, de l’identification des serveurs proxy et d’autres configurations.|
|**Ouverture de session**|Configuration ordinateur\Modèles d’administration\Système\Ouverture de session|Ces paramètres de stratégie contrôlent la façon dont le système présente l’expérience d’ouverture de session pour les utilisateurs.|
|**Ouverture de session réseau**|Configuration de l’ordinateur \ connexion Templates\System\Net|Ces paramètres de stratégie contrôlent la façon dont le système gère les demandes d’ouverture de session réseau, y compris le comportement du localisateur de contrôleur de domaine.<p>Pour plus d’informations sur l’intégration du localisateur de contrôleur de domaine dans les processus de réplication, consultez Présentation de la [réplication entre sites](https://technet.microsoft.com/library/cc771251.aspx).|
|**Biométrie**|Configuration ordinateur \ modèles d’administration\Composants Components\Biometrics|Ces paramètres de stratégie autorisent généralement ou refusent l’utilisation de la biométrie comme méthode d’authentification.<p>Pour plus d’informations sur l’implémentation de la biométrie Windows, consultez Windows Biometric Framework vue d’ensemble.|
|**Interface utilisateur des informations d’identification**|Configuration ordinateur \ modèles d’administration\Composants Components\Credential de l’interface utilisateur|Ces paramètres de stratégie contrôlent la façon dont les informations d’identification sont gérées au point d’entrée.|
|**Synchronisation de mot de passe**|Configuration ordinateur \ modèles d’administration\Composants Components\Password synchronisation|Ces paramètres de stratégie déterminent la façon dont le système gère la synchronisation des mots de passe entre les systèmes d’exploitation Windows et UNIX.<p>Pour plus d’informations, consultez [synchronisation de mot de passe](https://technet.microsoft.com/library/cc732609.aspx).|
|**Carte à puce**|Configuration ordinateur \ modèles d’administration\Composants Components\Smart|Ces paramètres de stratégie contrôlent la façon dont le système gère les ouvertures de session par carte à puce.<p>|
|**Options d’ouverture de session Windows**|Configuration ordinateur \ modèles d’administration\Composants Windows\windows Logon options|Ces paramètres de stratégie contrôlent quand et comment les opportunités d’ouverture de session sont disponibles.|
|**Options CTRL + ALT + SUPPR**|Configuration ordinateur \ modèles d’administration\Composants Components\Ctrl + Alt + Suppr options|Ces paramètres de stratégie affectent l’apparence et l’accessibilité aux fonctionnalités de l’interface utilisateur d’ouverture de session (Bureau sécurisé), telles que le gestionnaire des tâches et le verrouillage du clavier de l’ordinateur.|
|**Ouverture de session**|Configuration ordinateur \ modèles d’administration\Composants Components\Logon|Ces paramètres de stratégie déterminent si ou quels processus peuvent s’exécuter lorsque l’utilisateur ouvre une session.|

## <a name="see-also"></a>Voir aussi
[Vue d’ensemble technique de l’authentification Windows](windows-authentication-technical-overview.md)


