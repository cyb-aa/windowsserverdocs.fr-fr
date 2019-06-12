---
title: Étape 2 configurer le serveur VPN et DirectAccess
description: Cette rubrique fait partie du guide ajouter DirectAccess à un déploiement de l’accès à distance existants (VPN, Virtual Private Network) pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe221fc9-c7d9-4508-b8a1-000d2515283c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f8a6448661861fdc9f97c66fb130bfc03d0ce72c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446975"
---
#  <a name="step-2-configure-the-directaccess-vpn-server"></a>Étape 2 configurer le serveur VPN et DirectAccess

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit comment configurer les paramètres de client et de serveur requis pour un déploiement de l'accès à distance de base à l'aide de l'Assistant Activation de DirectAccess.

Le tableau suivant fournit une vue d’ensemble des étapes que vous pouvez effectuer à l’aide de cette rubrique.

|Tâche       |Description|
|-----------|-----------|
|Configurer les clients DirectAccess|Configurez le serveur d'accès à distance avec les groupes de sécurité contenant les clients DirectAccess.|
|Configurer la topologie du réseau|Configurez les paramètres du serveur d'accès à distance.|
|Configurer la liste de recherche de suffixes DNS|Modifiez la liste de recherche de suffixes, si vous le souhaitez.|
|Configuration des objets de stratégie de groupe|Modifiez les objets de stratégie de groupe, si vous le souhaitez.|

## <a name="to-start-the-enable-directacces-wizard"></a>Pour démarrer l'Assistant Activation de DirectAccess

1. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **accès à distance**. L’Assistant Activation de DirectAccess démarre automatiquement, sauf si vous avez sélectionné **ne plus afficher cet écran**. 

2. Si l’Assistant ne démarre pas automatiquement, cliquez sur le nœud de serveur dans l’arborescence de routage et accès distant, puis cliquez sur **activer DirectAccess**.

3. Cliquez sur **Suivant**.

## <a name="configure-directaccess-clients"></a>Configurer les clients DirectAccess

Pour configurer l'utilisation de DirectAccess sur un ordinateur client, ce dernier doit appartenir au groupe de sécurité sélectionné. Une fois DirectAccess configuré, les ordinateurs clients du groupe de sécurité sont configurés pour recevoir la stratégie de groupe DirectAccess.

1. Dans la page **Sélectionner des groupes**, cliquez sur **Ajouter**.

2. Dans la boîte de dialogue **Sélectionner des groupes** , sélectionnez les groupes de sécurité contenant des ordinateurs clients DirectAccess.

3. Cochez la case **Activer DirectAccess pour les ordinateurs portables uniquement** pour autoriser uniquement les ordinateurs portables à accéder au réseau interne.

4. Cochez la case **Utiliser le tunneling forcé** pour acheminer tout le trafic client (vers le réseau interne et vers Internet) via le serveur d'accès à distance.

5. Cliquez sur **Suivant**.

## <a name="configure-the-network-topology"></a>Configurer la topologie du réseau

Pour déployer l'accès à distance, vous devez configurer le serveur d'accès à distance avec les cartes réseau appropriées, une URL publique pour le serveur d'accès à distance auquel les ordinateurs clients peuvent se connecter (l'adresse de connexion) et un certificat IP-HTTPS dont le sujet correspond à l'adresse de connexion.

1. Dans la page **Topologie du réseau**, cliquez sur la topologie de déploiement qui sera utilisée dans votre organisation. Dans **Tapez le nom public ou l'adresse IPv4 utilisé par les clients pour se connecter au serveur d'accès à distance**, entrez le nom public du déploiement (ce nom correspond au nom de sujet du certificat IP-HTTPS, par exemple : edge1.contoso.com), puis cliquez sur **Suivant**.

## <a name="configure-the-dns-suffix-search-list"></a>Configurer la liste de recherche de suffixes DNS

Pour les clients DNS, vous pouvez configurer une liste de recherche de suffixes de domaines DNS qui étend ou modifie leurs fonctionnalités de recherche DNS. En ajoutant des suffixes supplémentaires à cette liste, vous pouvez rechercher des noms d'ordinateurs courts et non qualifiés dans plusieurs domaines DNS. Ensuite, si une requête DNS échoue, le service Client DNS peut utiliser cette liste pour ajouter d’autres fins de suffixe de nom à votre nom d’origine et de répéter les requêtes DNS vers le serveur DNS pour ces autres noms de domaine complets.

1. Cochez la case **Configurer les clients DirectAccess avec la liste de recherche des suffixes de client DNS** pour spécifier des suffixes supplémentaires pour les recherches de noms de clients.

2. Tapez un nouveau nom de suffixe dans **nouveau suffixe** puis cliquez sur **ajouter**. En outre, vous pouvez modifier l’ordre de recherche et supprimer des suffixes de **Suffixes de domaine à utiliser**.

>[REMARQUE] Dans un scénario d’espace de noms dissocié \(où un ou plusieurs ordinateurs du domaine a un suffixe DNS qui ne correspond pas au domaine Active Directory auquel appartiennent les ordinateurs\), vous devez vous assurer que la liste de recherche est personnalisée pour inclure tous les suffixes requis. L'Assistant Accès à distance configurera par défaut le nom DNS Active Directory comme suffixe DNS principal sur le client. L'administrateur doit s'assurer qu'il a le suffixe DNS utilisé par les clients pour la résolution des noms.

Pour les ordinateurs et serveurs, le comportement de recherche DNS par défaut suivant est prédéterminé et utilisé lors de la résolution des noms courts et non qualifiés. Lorsque la liste de recherche de suffixe est vide ou non spécifiée, le suffixe DNS principal de l’ordinateur est ajouté aux courtes de noms non qualifié, et une requête DNS est utilisée pour résoudre le nom de domaine complet résultant. 

Si cette requête échoue, l’ordinateur peut tenter des requêtes supplémentaires pour les autres noms de domaine complets en ajoutant n’importe quel suffixe DNS spécifique à la connexion configurée pour les connexions réseau. Si aucun suffixe spécifique à la connexion est configuré ou requêtes pour ces noms de domaine complets résultants spécifique à la connexion échouent, le client peut ensuite commencer à réessayer de requêtes basées sur une réduction systématique du suffixe principal (également appelé dévolution).

Par exemple, si le suffixe principal est « example.microsoft.com », le processus de dévolution peut retenter les requêtes pour le nom court en le recherchant dans les domaines « microsoft.com » et « com ».

Lorsque le suffixe de recherche liste n’est pas vide et au moins un suffixe DNS spécifié, les tentatives de résolution des noms DNS courts est limitée à la recherche uniquement les noms de domaine complets rendues possibles par la liste de suffixes spécifiée. 

Si les requêtes de tous les noms de domaine complets résultant de l'ajout et du test de chaque suffixe de la liste ne sont pas résolues, le processus de requête échoue et indique « Impossible de trouver le nom ». 

> [!WARNING]
> Si la liste des suffixes du domaine est utilisée, les clients continuent d'envoyer d'autres requêtes supplémentaires basées sur différents noms de domaine DNS, en l'absence de réponse ou de résolution d'une requête. Lorsqu'un nom est résolu à partir d'une entrée de la liste des suffixes, les entrées de liste non utilisées ne sont pas testées. C'est la raison pour laquelle il est plus efficace de trier la liste en plaçant en premier les suffixes de domaine les plus utilisés.
> 
> Les recherches d'un suffixe de nom de domaine ne sont utilisées que lorsqu'une entrée de nom DNS n'est pas pleinement qualifiée. Pour qualifier pleinement un nom DNS, un point final (.) est entré à la suite du nom.

## <a name="gpo-configuration"></a>Configuration des objets de stratégie de groupe

Lorsque vous configurez l’accès à distance, les paramètres DirectAccess sont rassemblés dans les objets de stratégie de groupe (GPO). 

Dans **paramètres GPO**, le nom du serveur de stratégie de groupe DirectAccess et le nom de l’objet stratégie de groupe Client sont répertoriés. De plus, vous pouvez modifier les paramètres de sélection des objets de stratégie de groupe.

Deux objets de stratégie de groupe sont renseignés automatiquement avec les paramètres de DirectAccess et distribuées de cette façon :

1. **Objet de stratégie de groupe de client DirectAccess**. Cet objet de stratégie de groupe contient des paramètres client, notamment les paramètres de technologie de transition IPv6, les entrées de la table NRPT et les règles de sécurité de connexion du Pare-feu Windows avec fonctions avancées de sécurité. L'objet de stratégie de groupe est appliqué aux groupes de sécurité spécifiés pour les ordinateurs clients.

2. **Objet de stratégie de groupe de serveur DirectAccess**. Cet objet de stratégie de groupe contient les paramètres de configuration DirectAccess qui sont appliqués à tout serveur configuré comme serveur d’accès à distance dans votre déploiement. Il contient également les règles de sécurité de connexion du Pare-feu Windows avec fonctions avancées de sécurité.

## <a name="summary"></a>Récapitulatif

Une fois que la configuration de l’accès à distance est terminée le **Résumé** s’affiche. Vous pouvez modifier les paramètres configurés ou cliquer sur **Terminer** pour appliquer la configuration.
