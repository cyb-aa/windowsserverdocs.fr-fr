---
title: 'Étape 2 : configurer le serveur DirectAccess-VPN'
description: Cette rubrique fait partie du guide ajouter DirectAccess à un déploiement d’accès à distance (VPN) existant pour Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: fe221fc9-c7d9-4508-b8a1-000d2515283c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f055a51c93276474bb1b5d4162a914dfa7bbb69a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819632"
---
#  <a name="step-2-configure-the-directaccess-vpn-server"></a>Étape 2 : configurer le serveur DirectAccess-VPN

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit comment configurer les paramètres de client et de serveur requis pour un déploiement de l'accès à distance de base à l'aide de l'Assistant Activation de DirectAccess.

Le tableau suivant fournit une vue d’ensemble des étapes que vous pouvez effectuer à l’aide de cette rubrique.

|Tâche       |Description|
|-----------|-----------|
|Configurer les clients DirectAccess|Configurez le serveur d'accès à distance avec les groupes de sécurité contenant les clients DirectAccess.|
|Configurer la topologie du réseau|Configurez les paramètres du serveur d'accès à distance.|
|Configurer la liste de recherche de suffixes DNS|Modifiez la liste de recherche de suffixes, si vous le souhaitez.|
|Configuration des objets de stratégie de groupe|Modifiez les objets de stratégie de groupe, si vous le souhaitez.|

## <a name="to-start-the-enable-directacces-wizard"></a>Pour démarrer l'Assistant Activation de DirectAccess

1. Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur **accès à distance**. L’Assistant Activation de DirectAccess démarre automatiquement, sauf si vous n’avez **pas sélectionné l’option ne plus afficher cet écran**. 

2. Si l’Assistant ne démarre pas automatiquement, cliquez avec le bouton droit sur le nœud du serveur dans l’arborescence routage et accès distant, puis cliquez sur **activer DirectAccess**.

3. Cliquez sur **Suivant**.

## <a name="configure-directaccess-clients"></a>Configurer les clients DirectAccess

Pour configurer l'utilisation de DirectAccess sur un ordinateur client, ce dernier doit appartenir au groupe de sécurité sélectionné. Une fois DirectAccess configuré, les ordinateurs clients du groupe de sécurité sont configurés pour recevoir la stratégie de groupe DirectAccess.

1. Dans la page **Sélectionner des groupes**, cliquez sur **Ajouter**.

2. Dans la boîte de dialogue **Sélectionner des groupes**, sélectionnez les groupes de sécurité contenant des ordinateurs clients DirectAccess.

3. Cochez la case **Activer DirectAccess pour les ordinateurs portables uniquement** pour autoriser uniquement les ordinateurs portables à accéder au réseau interne.

4. Cochez la case **Utiliser le tunneling forcé** pour acheminer tout le trafic client (vers le réseau interne et vers Internet) via le serveur d'accès à distance.

5. Cliquez sur **Suivant**.

## <a name="configure-the-network-topology"></a>Configurer la topologie du réseau

Pour déployer l'accès à distance, vous devez configurer le serveur d'accès à distance avec les cartes réseau appropriées, une URL publique pour le serveur d'accès à distance auquel les ordinateurs clients peuvent se connecter (l'adresse de connexion) et un certificat IP-HTTPS dont le sujet correspond à l'adresse de connexion.

1. Dans la page **Topologie du réseau**, cliquez sur la topologie de déploiement qui sera utilisée dans votre organisation. Dans **Tapez le nom public ou l'adresse IPv4 utilisé par les clients pour se connecter au serveur d'accès à distance**, entrez le nom public du déploiement (ce nom correspond au nom de sujet du certificat IP-HTTPS, par exemple : edge1.contoso.com), puis cliquez sur **Suivant**.

## <a name="configure-the-dns-suffix-search-list"></a>Configurer la liste de recherche de suffixes DNS

Pour les clients DNS, vous pouvez configurer une liste de recherche de suffixes de domaines DNS qui étend ou modifie leurs fonctionnalités de recherche DNS. En ajoutant des suffixes supplémentaires à cette liste, vous pouvez rechercher des noms d'ordinateurs courts et non qualifiés dans plusieurs domaines DNS. En cas d’échec d’une requête DNS, le service client DNS peut utiliser cette liste pour ajouter d’autres terminaisons de suffixe de nom à votre nom d’origine et répéter des requêtes DNS sur le serveur DNS pour ces noms de domaine complets secondaires.

1. Cochez la case **Configurer les clients DirectAccess avec la liste de recherche des suffixes de client DNS** pour spécifier des suffixes supplémentaires pour les recherches de noms de clients.

2. Tapez un nouveau nom de suffixe dans **nouveau suffixe** , puis cliquez sur **Ajouter**. En outre, vous pouvez modifier l’ordre de recherche et supprimer les suffixes des **suffixes de domaine à utiliser**.

>OBSERVE Dans un scénario d’espace de noms disjoint \(où un ou plusieurs ordinateurs de domaine ont un suffixe DNS qui ne correspond pas au domaine Active Directory auquel les ordinateurs appartiennent\), vous devez vous assurer que la liste de recherche est personnalisée pour inclure tous les suffixes requis. L'Assistant Accès à distance configurera par défaut le nom DNS Active Directory comme suffixe DNS principal sur le client. L'administrateur doit s'assurer qu'il a le suffixe DNS utilisé par les clients pour la résolution des noms.

Pour les ordinateurs et les serveurs, le comportement de recherche DNS par défaut suivant est prédéterminé et utilisé lors de l’exécution et de la résolution de noms courts et non qualifiés. Lorsque la liste de recherche de suffixes est vide ou non spécifiée, le suffixe DNS principal de l’ordinateur est ajouté aux noms non qualifiés courts, et une requête DNS est utilisée pour résoudre le nom de domaine complet (FQDN) résultant. 

Si cette requête échoue, l’ordinateur peut essayer des requêtes supplémentaires pour d’autres noms de domaine complets en ajoutant un suffixe DNS spécifique à la connexion configuré pour les connexions réseau. Si aucun suffixe spécifique à la connexion n’est configuré ou si les requêtes pour les noms de domaine complets spécifiques à la connexion résultants échouent, le client peut alors commencer à retenter les requêtes en fonction de la réduction systématique du suffixe principal (également appelé dévolution).

Par exemple, si le suffixe principal est « example.microsoft.com », le processus de dévolution peut réessayer des requêtes pour le nom abrégé en le recherchant dans les domaines « microsoft.com » et « com ».

Lorsque la liste de recherche de suffixes n’est pas vide et qu’au moins un suffixe DNS est spécifié, les tentatives de qualification et de résolution des noms DNS courts sont limitées à la recherche des noms de domaine complets rendus possibles par la liste de suffixes spécifiée. 

Si les requêtes de tous les noms de domaine complets résultant de l'ajout et du test de chaque suffixe de la liste ne sont pas résolues, le processus de requête échoue et indique « Impossible de trouver le nom ». 

> [!WARNING]
> Si la liste des suffixes du domaine est utilisée, les clients continuent d'envoyer d'autres requêtes supplémentaires basées sur différents noms de domaine DNS, en l'absence de réponse ou de résolution d'une requête. Lorsqu'un nom est résolu à partir d'une entrée de la liste des suffixes, les entrées de liste non utilisées ne sont pas testées. C'est la raison pour laquelle il est plus efficace de trier la liste en plaçant en premier les suffixes de domaine les plus utilisés.
> 
> Les recherches d'un suffixe de nom de domaine ne sont utilisées que lorsqu'une entrée de nom DNS n'est pas pleinement qualifiée. Pour qualifier pleinement un nom DNS, un point final (.) est entré à la suite du nom.

## <a name="gpo-configuration"></a>Configuration des objets de stratégie de groupe

Lorsque vous configurez l’accès à distance, les paramètres DirectAccess sont collectés dans stratégie de groupe objets (GPO). 

Dans les **paramètres GPO**, le nom de l’objet de stratégie de groupe du serveur DirectAccess et le nom de l’objet de stratégie de groupe du client De plus, vous pouvez modifier les paramètres de sélection des objets de stratégie de groupe.

Deux objets de stratégie de groupe sont automatiquement renseignés avec les paramètres DirectAccess et distribués de cette façon :

1. **Objet de stratégie de groupe de client DirectAccess**. Cet objet de stratégie de groupe contient des paramètres client, notamment les paramètres de technologie de transition IPv6, les entrées de la table NRPT et les règles de sécurité de connexion du Pare-feu Windows avec fonctions avancées de sécurité. L'objet de stratégie de groupe est appliqué aux groupes de sécurité spécifiés pour les ordinateurs clients.

2. **Objet de stratégie de groupe de serveur DirectAccess**. Cet objet de stratégie de groupe contient les paramètres de configuration DirectAccess qui sont appliqués à tout serveur configuré en tant que serveur d’accès à distance dans votre déploiement. Il contient également les règles de sécurité de connexion du Pare-feu Windows avec fonctions avancées de sécurité.

## <a name="summary"></a>Résumé

Une fois la configuration de l’accès à distance terminée, le **Résumé** s’affiche. Vous pouvez modifier les paramètres configurés ou cliquer sur **Terminer** pour appliquer la configuration.
