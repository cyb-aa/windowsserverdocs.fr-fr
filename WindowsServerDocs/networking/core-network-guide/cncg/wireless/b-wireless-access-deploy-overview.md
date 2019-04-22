---
title: Vue d’ensemble du déploiement de l’accès sans fil
description: Cette rubrique fait partie de la « Accès sans fil authentifié 802. 1 mot de passe de déployer X » du guide de mise en réseau de Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 29ae0f54-f045-465a-a08e-5867979345f2
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 6658c4750ba2f71b24acd4f7da02029da63179bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818920"
---
# <a name="wireless-access-deployment-overview"></a>Vue d’ensemble du déploiement de l’accès sans fil

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

L’illustration suivante montre les composants qui sont nécessaires pour déployer 802. 1 X authentifiés l’accès sans fil avec PEAP\-MS\-CHAP v2.  

![802. 1 x présentation d’Infrastructure de déploiement](../../../media/8021X-Deploy-Overview/8021X-Deploy-Overview.jpg)

## <a name="wireless-access-deployment-components"></a>Composants de déploiement d’accès sans fil
L’infrastructure suivante est nécessaire pour ce déploiement de l’accès sans fil :

### <a name="8021x-capable-wireless-access-points"></a>802. 1 X\-points d’accès sans fil
Une fois que les services d’infrastructure réseau requise prise en charge de votre réseau local sans fil sont en place, vous pouvez commencer le processus de conception pour l’emplacement des points d’accès sans fil. Le processus de conception de déploiement AP sans fil implique ces étapes :

- Identifier les zones de couverture des utilisateurs sans fil. Tandis que l’identification des zones de couverture, veillez à déterminer si vous souhaitez fournir des services sans fil en dehors de la création et si tel est le cas, déterminer plus précisément où ces zones externes sont.

- Déterminez combien de points d’accès sans fil à déployer pour garantir une couverture adéquate.

- Déterminer où placer les points d’accès sans fil.

- Sélectionnez les fréquences de canaux pour les points d’accès sans fil.

### <a name="active-directory-domain-services"></a>Services de domaine Active Directory
Les éléments suivants des services AD DS sont requis pour le déploiement de l’accès sans fil.

#### <a name="users-and-computers"></a>Utilisateurs et ordinateurs

Utilisez les utilisateurs Active Directory et alignement des ordinateurs\-in pour créer et gérer des comptes d’utilisateur et pour créer un groupe de sécurité sans fil qui inclut chaque membre du domaine auquel vous souhaitez accorder l’accès sans fil.

#### <a name="wireless-network-ieee-80211-policies"></a>Réseau sans fil \(IEEE 802.11\) stratégies

Vous pouvez utiliser le réseau sans fil \(IEEE 802.11\) l’extension des stratégies de gestion des stratégies de groupe pour configurer des stratégies qui sont appliqués aux ordinateurs sans fil lorsqu’ils tentent d’accéder au réseau.

Dans groupe de stratégie d’éditeur de gestion, lorsque vous avec le bouton droit\-cliquez sur **réseau sans fil \(IEEE 802.11\) stratégies**, vous avez deux options suivantes pour le type de stratégie sans fil que vous créez.

- **Créer une nouvelle stratégie de réseau sans fil pour Windows Vista et versions ultérieures**

- **Créer une stratégie Windows XP**

>[!TIP]
>Lorsque vous configurez une nouvelle stratégie de réseau sans fil, vous avez la possibilité de modifier le nom et la description de la stratégie. Si vous modifiez le nom de la stratégie, la modification est répercutée dans le **détails** volet de l’éditeur de la gestion de stratégie de groupe et sur la barre de titre de la boîte de dialogue de stratégie de réseau sans fil. Quelle que soit la façon dont vous renommez vos stratégies, la nouvelle stratégie sans fil XP sont toujours répertoriée dans l’éditeur d’administration de stratégie de groupe avec le **Type** affichage **XP**. Autres stratégies sont répertoriés avec le **Type** montrant **Vista et versions ultérieures**.  

La stratégie de réseau sans fil pour Windows Vista et versions ultérieures vous permet de configurer, hiérarchiser et gérer plusieurs profils sans fil. Un profil sans fil est une collection de paramètres de sécurité qui sont utilisés pour se connecter à un réseau sans fil spécifique et de connectivité. Lors de la stratégie de groupe est mise à jour sur vos ordinateurs clients sans fil, les profils que vous créez dans la stratégie de réseau sans fil sont automatiquement ajoutés à la configuration sur vos ordinateurs client sans fil à laquelle s’applique la stratégie de réseau sans fil.

##### <a name="allowing-connections-to-multiple-wireless-networks"></a>Autoriser les connexions à plusieurs réseaux sans fil

Si vous avez des clients sans fil qui sont déplacées entre les emplacements physiques de votre organisation, comme entre un siège social et une filiale, vous souhaiterez ordinateurs pour se connecter à plusieurs réseaux sans fil. Dans ce cas, vous pouvez configurer un profil sans fil contenant les paramètres de connectivité et de sécurité spécifiques pour chaque réseau.

Par exemple, supposons que votre entreprise possède un réseau sans fil pour le siège social d’entreprise, avec un identificateur de jeu de service \(SSID\) WlanCorp.

Votre succursale a également un réseau sans fil auquel vous souhaitez également vous connecter. La succursale a l’identificateur SSID comme WlanBranch.

Dans ce scénario, vous pouvez configurer un profil pour chaque réseau et les ordinateurs ou les autres périphériques qui sont utilisés à la fois du siège et succursale peut se connecter à un des réseaux sans fil quand ils se trouvent physiquement dans la plage de la zone de couverture d’un réseau.

##### <a name="mixed-mode-wireless-networks"></a>Mixte\-réseaux sans fil de mode

Ou bien, supposons que votre réseau comporte un mélange d’ordinateurs sans fil et les périphériques qui prennent en charge des normes de sécurité différentes. Certains ordinateurs plus anciens ou peut-être des cartes sans fil qui peuvent uniquement utiliser WPA\-entreprise, tandis que les appareils plus récente peuvent utiliser le plus fort WPA2\-Enterprise standard.

Vous pouvez créer deux profils différents qui utilisent le même SSID et les paramètres de connectivité et de sécurité presque identiques.

Dans un profil, vous pouvez définir l’authentification sans fil WPA2\-Enterprise avec AES et l’autre profil, vous pouvez spécifier WPA\-Enterprise avec TKIP.

Cela est connu comme un mixte\-déploiement en mode, qui permet aux ordinateurs de différents types et des fonctionnalités sans fil pour partager le même réseau sans fil.

### <a name="network-policy-server-nps"></a>Network Policy Server \(NPS\)
NPS vous permet de créer et appliquer des stratégies d’accès réseau pour l’authentification de demande de connexion et l’autorisation.

Lorsque vous utilisez NPS comme serveur RADIUS, vous configurez les serveurs d’accès réseau, tels que les points d’accès sans fil, en tant que clients RADIUS dans NPS. Vous configurez également les stratégies réseau que NPS utilise pour authentifier des clients d’accès et autoriser leurs demandes de connexion.  

### <a name="wireless-client-computers"></a>Ordinateurs clients sans fil
Pour les besoins de ce guide, les ordinateurs clients sans fil sont des ordinateurs et autres périphériques qui sont équipés de cartes réseau sans fil IEEE 802.11 et qui exécutent le client de Windows ou des systèmes d’exploitation Windows Server.

#### <a name="server-computers-as-wireless-clients"></a>Ordinateurs de serveur en tant que clients sans fil

Par défaut, la fonctionnalité pour 802.11 sans fil est désactivée sur les ordinateurs qui exécutent Windows Server.

Pour activer la connectivité sans fil sur les ordinateurs exécutant des systèmes d’exploitation serveur, vous devez installer et activer le réseau local sans fil \(WLAN\) fonctionnalité de Service à l’aide de Windows PowerShell ou l’ajout de rôles et fonctionnalités Assistant dans le serveur Gestionnaire.

Lorsque vous installez le **Service de réseau local sans fil** fonctionnalité, le nouveau service **configuration automatique WLAN** est installé dans **Services**. Lors de l’installation est terminée, vous devez redémarrer le serveur.

Une fois que le serveur est redémarré, vous pouvez accéder à configuration automatique WLAN lorsque vous cliquez sur **Démarrer**, **les outils d’administration Windows**, et **Services**.

Après installation et le serveur redémarré, le service est arrêté avec un type de démarrage de configuration automatique WLAN **automatique**. Pour démarrer le service, double-cliquez sur **configuration automatique WLAN**. Sur le **général** , cliquez sur **Démarrer**, puis cliquez sur **OK**.

Le service de configuration automatique WLAN énumère les adaptateurs sans fil et gère les connexions sans fil et les profils sans fil qui contiennent des paramètres qui sont requis pour configurer le serveur pour vous connecter à un réseau sans fil.

Pour une vue d’ensemble du déploiement de l’accès sans fil, consultez [processus de déploiement de l’accès sans fil](c-wireless-access-deploy-process.md).
