---
title: Vue d’ensemble du déploiement de l’accès sans fil
description: Cette rubrique fait partie du guide de mise en réseau de Windows Server2016 «Accès sans fil authentifié 802. 1-mot de passe de déployer X»
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 29ae0f54-f045-465a-a08e-5867979345f2
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 11d87254d8819606a06acedd407bb2e09a45cb60
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="wireless-access-deployment-overview"></a>Vue d’ensemble du déploiement de l’accès sans fil

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

L’illustration suivante montre les composants qui sont nécessaires pour déployer 802. 1 X authentifiés l’accès sans fil avec PEAP\-MS\-CHAP v2.  

![802. 1 x présentation d’Infrastructure de déploiement](../../../media/8021X-Deploy-Overview/8021X-Deploy-Overview.jpg)

## <a name="wireless-access-deployment-components"></a>Composants du déploiement de l’accès sans fil
L’infrastructure suivante est requise pour ce déploiement de l’accès sans fil:

### <a name="8021x-capable-wireless-access-points"></a>802.1X\-points d’accès sans fil
Une fois que les services d’infrastructure réseau requise prise en charge de votre réseau local sans fil sont en place, vous pouvez commencer le processus de conception pour l’emplacement des points d’accès sans fil. Le processus de conception de déploiement point d’accès sans fil implique les étapes suivantes:

- Identifiez les zones de couverture des utilisateurs sans fil. Alors que l’identification des zones de couverture, veillez à déterminer si vous voulez fournir des services sans fil en dehors de la création et si tel est le cas, déterminez spécifiquement où ces zones externes sont.

- Déterminer le nombre de points d’accès sans fil à déployer pour garantir une couverture adéquate.

- Déterminer où placer les points d’accès sans fil.

- Sélectionnez les fréquences de canal pour points d’accès sans fil.

### <a name="active-directory-domain-services"></a>Services de domaine ActiveDirectory
Les éléments suivants des services ADDS sont requis pour le déploiement de l’accès sans fil.

#### <a name="users-and-computers"></a>Utilisateurs et ordinateurs

Utilisez les utilisateurs ActiveDirectory et les ordinateurs de composants pour créer et gérer des comptes d’utilisateur et pour créer un groupe de sécurité sans fil qui inclut chaque membre du domaine auquel vous souhaitez accorder l’accès sans fil.

#### <a name="wireless-network-ieee-80211-policies"></a>Réseau sans fil \ IEEE (802.11\) les stratégies

Vous pouvez utiliser le réseau sans fil \ IEEE (802.11\) l’extension des stratégies de gestion des stratégies de groupe pour configurer des stratégies qui sont appliqués aux ordinateurs sans fil lorsqu’ils tentent d’accéder au réseau.

Dans l’éditeur de gestion de stratégie de groupe, lorsque vous droit clic **réseau sans fil \ IEEE (802.11\) stratégies**, vous avez deux options suivantes pour le type de stratégie sans fil que vous créez.

- **Créer une nouvelle stratégie de réseau sans fil pour WindowsVista et versions ultérieures**

- **Créer une stratégie WindowsXP**

>[!TIP]
>Lorsque vous configurez une nouvelle stratégie de réseau sans fil, vous avez la possibilité de modifier le nom et la description de la stratégie. Si vous modifiez le nom de la stratégie, la modification est reflétée dans le **détails** volet de l’éditeur de la gestion de stratégie de groupe et sur la barre de titre de la boîte de dialogue de stratégie de réseau sans fil. Quelle que soit la façon dont vous renommez vos stratégies, la nouvelle stratégie sans fil XP toujours apparaît dans l’éditeur d’administration de stratégie de groupe avec le **Type** affichage **XP**. Autres stratégies sont répertoriés avec le **Type** montrant **Vista et versions ultérieures**.  

La stratégie de réseau sans fil pour WindowsVista et versions ultérieures vous permet de configurer, de classer par priorité et de gérer plusieurs profils sans fil. Un profil sans fil est une collection de connectivité et des paramètres de sécurité sont utilisés pour se connecter à un réseau sans fil spécifique. Lors de la stratégie de groupe est mis à jour sur vos ordinateurs clients sans fil, les profils que vous créez dans la stratégie de réseau sans fil sont automatiquement ajoutés à la configuration sur vos ordinateurs clients sans fil auquel s’applique la stratégie de réseau sans fil.

##### <a name="allowing-connections-to-multiple-wireless-networks"></a>Autoriser les connexions à plusieurs réseaux sans fil

Si vous avez des clients sans fil qui sont déplacés entre des emplacements physiques dans votre organisation, par exemple entre un siège social et une filiale, vous souhaiterez peut-être ordinateurs pour se connecter à plusieurs réseaux sans fil. Dans ce cas, vous pouvez configurer un profil sans fil qui contient les paramètres de connectivité et de sécurité spécifiques pour chaque réseau.

Par exemple, supposons que votre entreprise dispose d’un réseau sans fil pour le siège social, avec un identificateur \(SSID\) WlanCorp.

Votre filiale possède également un réseau sans fil auquel vous souhaitez également vous connecter. La filiale a l’identificateur SSID comme WlanBranch.

Dans ce scénario, vous pouvez configurer un profil pour chaque réseau et les ordinateurs ou les autres périphériques qui sont utilisés à la fois du siège et succursale peut se connecter à un des réseaux sans fil lorsqu’ils se trouvent physiquement dans la plage de la zone de couverture d’un réseau.

##### <a name="mixed-mode-wireless-networks"></a>Réseaux sans fil en mode Mixed\

Sinon, supposons que votre réseau comporte un mélange d’ordinateurs sans fil et les périphériques qui prennent en charge les normes de sécurité différentes. Certains ordinateurs plus anciens ou peut-être cartes sans fil qui peuvent uniquement utiliser WPA\-entreprise, alors que les périphériques plus récents peuvent utiliser la norme WPA2\-entreprise plus puissante.

Vous pouvez créer deux profils différents qui utilisent le même SSID et les paramètres de connectivité et de sécurité quasiment identiques.

Dans un profil, vous pouvez configurer l’authentification sans fil pour WPA2\-entreprise avec AES, et dans le profil d’autres, vous pouvez spécifier à l’entreprise WPA\ avec TKIP.

Ceci est communément appelé un déploiement en mode mixed\, et il permet aux ordinateurs de différents types et sans fil pour partager le même réseau sans fil.

### <a name="network-policy-server-nps"></a>Serveur de stratégie réseau \(NPS\)
NPS vous permet de créer et appliquer des stratégies d’accès réseau pour l’authentification de demande de connexion et d’autorisation.

Lorsque vous utilisez NPS comme serveur RADIUS, vous configurez les serveurs d’accès réseau, tels que les points d’accès sans fil, en tant que clients RADIUS dans NPS. Vous configurez également les stratégies réseau que NPS utilise pour authentifier les clients d’accès et d’autoriser les demandes de connexion.  

### <a name="wireless-client-computers"></a>Ordinateurs clients sans fil
Dans le cadre de ce guide, les ordinateurs clients sans fil sont des ordinateurs et autres périphériques qui sont équipés de cartes réseau sans fil IEEE 802.11 et qui exécutent le client Windows ou des systèmes d’exploitation Windows Server.

#### <a name="server-computers-as-wireless-clients"></a>Ordinateurs de serveur en tant que clients sans fil

Par défaut, les fonctionnalités sans fil 802.11 sont désactivée sur les ordinateurs qui exécutent Windows Server.

Pour activer la connectivité sans fil sur les ordinateurs exécutant des systèmes d’exploitation serveur, vous devez installer et activer le réseau local sans fil \(WLAN\) fonctionnalité du Service à l’aide de Windows PowerShell ou l’ajout de rôles et fonctionnalités Assistant dans le Gestionnaire de serveur.

Lorsque vous installez le **Service de réseau local sans fil** de fonctionnalité, le nouveau service **configuration automatique WLAN** est installé dans **Services**. Lorsque l’installation est terminée, vous devez redémarrer le serveur.

Une fois que le serveur est redémarré, vous pouvez accéder à configuration automatique WLAN lorsque vous cliquez sur **Démarrer**, **outils d’administration Windows**, et **Services**.

Une fois l’installation et le serveur redémarré, le service est dans un état d’arrêt avec un type de démarrage de configuration automatique WLAN **automatique**. Pour démarrer le service, double-cliquez sur **configuration automatique WLAN**. Sur le **général**, cliquez sur **Démarrer**, puis cliquez sur **OK**.

Le service de configuration automatique WLAN énumère les cartes sans fil et gère les connexions sans fil et les profils sans fil qui contiennent des paramètres qui sont requises pour configurer le serveur pour se connecter à un réseau sans fil.

Pour une vue d’ensemble du déploiement de l’accès sans fil, consultez [processus de déploiement de l’accès sans fil](c-wireless-access-deploy-process.md).
