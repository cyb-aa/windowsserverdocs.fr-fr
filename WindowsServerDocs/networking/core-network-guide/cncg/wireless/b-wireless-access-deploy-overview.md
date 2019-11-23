---
title: Vue d’ensemble du déploiement de l’accès sans fil
description: Cette rubrique fait partie du Guide de mise en réseau de Windows Server 2016 « déployer l’accès sans fil authentifié 802.1 X basé sur un mot de passe »
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 29ae0f54-f045-465a-a08e-5867979345f2
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 93fb80c550771e4e7d8bc400d647b520b0c67fdf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356089"
---
# <a name="wireless-access-deployment-overview"></a>Vue d’ensemble du déploiement de l’accès sans fil

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

L’illustration suivante montre les composants requis pour déployer l’accès sans fil authentifié 802.1 X avec PEAP\-MS\-CHAP v2.  

![Présentation de l’infrastructure de déploiement 802.1 x](../../../media/8021X-Deploy-Overview/8021X-Deploy-Overview.jpg)

## <a name="wireless-access-deployment-components"></a>Composants de déploiement de l’accès sans fil
L’infrastructure suivante est requise pour ce déploiement de l’accès sans fil :

### <a name="8021x-capable-wireless-access-points"></a>points d’accès sans fil 802.1 x\-
Une fois que les services d’infrastructure réseau requis prenant en charge votre réseau local sans fil sont en place, vous pouvez commencer le processus de conception pour l’emplacement des points d’accès sans fil. Le processus de conception du déploiement du point d’accès sans fil implique les étapes suivantes :

- Identifiez les domaines de couverture pour les utilisateurs sans fil. Tout en identifiant les zones de couverture, veillez à indiquer si vous souhaitez fournir un service sans fil en dehors du bâtiment et, si tel est le cas, déterminer spécifiquement l’emplacement des zones externes.

- Déterminez le nombre de points d’accès sans fil à déployer pour garantir une couverture adéquate.

- Déterminez où placer les points d’accès sans fil.

- Sélectionnez les fréquences de canal pour les points d’accès sans fil.

### <a name="active-directory-domain-services"></a>Services de domaine Active Directory
Les éléments suivants de AD DS sont requis pour le déploiement de l’accès sans fil.

#### <a name="users-and-computers"></a>Utilisateurs et ordinateurs

Utilisez le composant logiciel enfichable utilisateurs et ordinateurs Active Directory\-dans pour créer et gérer des comptes d’utilisateur, et pour créer un groupe de sécurité sans fil incluant chaque membre de domaine auquel vous souhaitez accorder un accès sans fil.

#### <a name="wireless-network-ieee-80211-policies"></a>Stratégies de\) \(de réseau sans fil IEEE 802,11

Vous pouvez utiliser l’extension des stratégies de réseau sans fil \(IEEE 802,11\) de gestion des stratégie de groupe pour configurer les stratégies appliquées aux ordinateurs sans fil lorsqu’ils tentent d’accéder au réseau.

Dans Éditeur de gestion des stratégies de groupe, lorsque vous cliquez avec le bouton droit sur\-sur le **réseau sans fil \(les stratégies IEEE 802,11\)** , vous disposez des deux options suivantes pour le type de stratégie sans fil que vous créez.

- **Créer une nouvelle stratégie de réseau sans fil pour Windows Vista et les versions ultérieures**

- **Créer une nouvelle stratégie Windows XP**

>[!TIP]
>Lorsque vous configurez une nouvelle stratégie de réseau sans fil, vous avez la possibilité de modifier le nom et la description de la stratégie. Si vous modifiez le nom de la stratégie, la modification est répercutée dans le volet d' **informations** de éditeur de gestion des stratégies de groupe et dans la barre de titre de la boîte de dialogue stratégie de réseau sans fil. Quelle que soit la façon dont vous renommez vos stratégies, la nouvelle stratégie sans fil XP sera toujours listée dans Éditeur de gestion des stratégies de groupe avec le **type** affichant **XP**. D’autres stratégies sont répertoriées avec le **type** contenant **Vista et les versions ultérieures**.  

La stratégie de réseau sans fil pour Windows Vista et les versions ultérieures vous permet de configurer, classer par ordre de priorité et gérer plusieurs profils sans fil. Un profil sans fil est un ensemble de paramètres de connectivité et de sécurité utilisés pour se connecter à un réseau sans fil spécifique. Lorsque stratégie de groupe est mis à jour sur vos ordinateurs clients sans fil, les profils que vous créez dans la stratégie de réseau sans fil sont automatiquement ajoutés à la configuration sur les ordinateurs clients sans fil auxquels s’applique la stratégie de réseau sans fil.

##### <a name="allowing-connections-to-multiple-wireless-networks"></a>Autorisation des connexions à plusieurs réseaux sans fil

Si vous avez des clients sans fil qui sont déplacés entre des emplacements physiques de votre organisation, par exemple entre un siège social et une filiale, vous souhaiterez peut-être que les ordinateurs se connectent à plusieurs réseaux sans fil. Dans ce cas, vous pouvez configurer un profil sans fil qui contient les paramètres de sécurité et de connectivité spécifiques pour chaque réseau.

Par exemple, supposons que votre entreprise dispose d’un réseau sans fil pour le siège social, avec un identificateur de jeu de service \(SSID\) WlanCorp.

Votre succursale dispose également d’un réseau sans fil auquel vous souhaitez également vous connecter. Le SSID de la succursale est configuré en tant que WlanBranch.

Dans ce scénario, vous pouvez configurer un profil pour chaque réseau, et les ordinateurs ou autres périphériques qui sont utilisés au siège social et dans les succursales peuvent se connecter à l’un ou l’autre des réseaux sans fil lorsqu’ils sont physiquement à portée de la couverture d’un réseau.

##### <a name="mixed-mode-wireless-networks"></a>Réseaux sans fil en mode\-mixte

Vous pouvez également supposer que votre réseau a un mélange d’ordinateurs et d’appareils sans fil qui prennent en charge différentes normes de sécurité. Certains ordinateurs plus anciens disposent peut-être d’adaptateurs sans fil qui peuvent uniquement utiliser WPA\-Enterprise, tandis que les appareils plus récents peuvent utiliser la norme WPA2\-Enterprise plus puissante.

Vous pouvez créer deux profils différents qui utilisent le même SSID et des paramètres de sécurité et de connectivité quasiment identiques.

Dans un profil, vous pouvez définir l’authentification sans fil sur WPA2\-Enterprise avec AES, et dans l’autre profil, vous pouvez spécifier WPA\-Enterprise avec TKIP.

Il s’agit généralement d’un déploiement en mode\-mixte qui permet à des ordinateurs de types et de fonctionnalités sans fil différents de partager le même réseau sans fil.

### <a name="network-policy-server-nps"></a>Serveur de stratégie réseau \(\) NPS
NPS vous permet de créer et d’appliquer des stratégies d’accès réseau pour l’authentification et l’autorisation des demandes de connexion.

Lorsque vous utilisez NPS en tant que serveur RADIUS, vous configurez les serveurs d’accès réseau, tels que les points d’accès sans fil, en tant que clients RADIUS dans NPS. Vous configurez également les stratégies réseau que NPS utilise pour authentifier les clients d’accès et autoriser leurs demandes de connexion.  

### <a name="wireless-client-computers"></a>Ordinateurs clients sans fil
Dans le cadre de ce guide, les ordinateurs clients sans fil sont des ordinateurs et d’autres périphériques équipés de cartes réseau sans fil IEEE 802,11 et qui exécutent des systèmes d’exploitation clients Windows ou Windows Server.

#### <a name="server-computers-as-wireless-clients"></a>Ordinateurs serveurs en tant que clients sans fil

Par défaut, les fonctionnalités de la connexion sans fil 802,11 sont désactivées sur les ordinateurs qui exécutent Windows Server.

Pour activer la connectivité sans fil sur les ordinateurs exécutant des systèmes d’exploitation serveur, vous devez installer et activer la fonctionnalité de réseau local sans fil \(WLAN\) à l’aide de Windows PowerShell ou de l’Assistant Ajout de rôles et de fonctionnalités dans Gestionnaire de serveur.

Lorsque vous installez la fonctionnalité de **service de réseau local sans fil** , le nouveau service de **configuration automatique WLAN** est installé dans les **services**. Une fois l’installation terminée, vous devez redémarrer le serveur.

Une fois le serveur redémarré, vous pouvez accéder à la configuration automatique WLAN lorsque vous cliquez sur **Démarrer**, sur **Outils d’administration Windows**et sur **services**.

Après l’installation et le redémarrage du serveur, le service de configuration automatique WLAN est dans un état arrêté avec le type de démarrage **automatique**. Pour démarrer le service, double-cliquez sur **configuration automatique WLAN**. Sous l’onglet **général** , cliquez sur **Démarrer**, puis sur **OK**.

Le service de configuration automatique WLAN énumère les adaptateurs sans fil et gère à la fois les connexions sans fil et les profils sans fil qui contiennent les paramètres requis pour configurer le serveur afin qu’il se connecte à un réseau sans fil.

Pour une vue d’ensemble du déploiement de l’accès sans fil, consultez [processus de déploiement de l’accès sans fil](c-wireless-access-deploy-process.md).
