---
title: Configurer des stratégies réseau
description: Cette rubrique fournit une vue d’ensemble de la configuration de stratégie réseau pour le serveur NPS (Network Policy Server) dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: fe77655a-e2be-4949-92e1-aaaa215d86ea
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 683a2919a88a1ce399c996a6798b3f8d03c298a1
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315775"
---
# <a name="configure-network-policies"></a>Configurer des stratégies réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour configurer des stratégies réseau dans NPS.

## <a name="add-a-network-policy"></a>Ajouter une stratégie réseau

Le serveur de stratégie réseau \(\) NPS utilise des stratégies réseau et les propriétés de numérotation des comptes d’utilisateur pour déterminer si une demande de connexion est autorisée à se connecter au réseau.

Vous pouvez utiliser cette procédure pour configurer une nouvelle stratégie réseau dans la console NPS ou la console accès à distance.

### <a name="performing-authorization"></a>Exécution de l’autorisation

Lorsque NPS effectue l’autorisation d’une demande de connexion, il compare la demande à chaque stratégie réseau de la liste triée des stratégies, en commençant par la première stratégie, puis en déplaçant la liste des stratégies configurées. Si le serveur NPS trouve une stratégie dont les conditions correspondent à la demande de connexion, le serveur NPS utilise la stratégie de correspondance et les propriétés d’accès à distance du compte d’utilisateur pour effectuer l’autorisation. Si les propriétés de numérotation du compte d’utilisateur sont configurées pour accorder l’accès ou contrôler l’accès via la stratégie réseau et que la demande de connexion est autorisée, le serveur NPS applique les paramètres configurés dans la stratégie réseau à la connexion.

Si NPS ne trouve pas de stratégie réseau qui correspond à la demande de connexion, la demande de connexion est rejetée, à moins que les propriétés d’accès à distance sur le compte d’utilisateur soient définies pour accorder l’accès.

Si les propriétés d’accès à distance du compte d’utilisateur sont définies pour refuser l’accès, la demande de connexion est rejetée par le serveur NPS.

### <a name="key-settings"></a>Paramètres de clé

Lorsque vous utilisez l’Assistant Nouvelle stratégie réseau pour créer une stratégie réseau, la valeur que vous spécifiez dans la **méthode de connexion réseau** est utilisée pour configurer automatiquement la condition de **type de stratégie** : 

- Si vous conservez la valeur par défaut non spécifiée, la stratégie réseau que vous créez est évaluée par le serveur NPS pour tous les types de connexion réseau qui utilisent tout type de serveur d’accès réseau (NAS).
- Si vous spécifiez une méthode de connexion réseau, NPS évalue la stratégie réseau uniquement si la demande de connexion provient du type de serveur d’accès réseau que vous spécifiez.

Sur la page **autorisation d’accès** , vous devez sélectionner **accès accordé** si vous souhaitez que la stratégie autorise les utilisateurs à se connecter à votre réseau. Si vous souhaitez que la stratégie empêche les utilisateurs de se connecter à votre réseau, sélectionnez **accès refusé**. 

Si vous souhaitez que les autorisations d’accès soient déterminées par les propriétés d’accès à distance du compte d’utilisateur dans Active Directory Services de domaine&reg; \(AD DS\), vous pouvez activer la case à cocher l' **accès est déterminé par les propriétés d’accès à distance** de l’utilisateur.

L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-add-a-network-policy"></a>Pour ajouter une stratégie réseau 

1. Ouvrez la console NPS, puis double-cliquez sur **stratégies**.

2. Dans l’arborescence de la console, cliquez avec le bouton droit sur **stratégies réseau**, puis cliquez sur **nouveau**. L’Assistant Nouvelle stratégie réseau s’ouvre.

3. Utilisez l’Assistant Nouvelle stratégie réseau pour créer une stratégie.

## <a name="create-network-policies-for-dial-up-or-vpn-with-a-wizard"></a>Créer des stratégies réseau pour l’accès à distance ou VPN à l’aide d’un Assistant

Vous pouvez utiliser cette procédure pour créer les stratégies de demande de connexion et les stratégies réseau requises pour déployer des serveurs d’accès à distance ou un réseau privé virtuel \(des serveurs de\) VPN comme protocole RADIUS (Remote Authentication Dial-In User Service) \(RADIUS\) les clients vers le serveur RADIUS NPS.

>[!NOTE]
>Les ordinateurs clients, tels que les ordinateurs portables et les autres ordinateurs exécutant des systèmes d’exploitation clients, ne sont pas des clients RADIUS. Les clients RADIUS sont des serveurs d’accès réseau, tels que les points d’accès sans fil, les commutateurs d’authentification 802.1 X, le réseau privé virtuel \(les serveurs de\) VPN et les serveurs d’accès à distance, car ces appareils utilisent le protocole RADIUS pour communiquer avec les serveurs RADIUS tels que NPSs.

Cette procédure explique comment ouvrir l’Assistant Nouvelle connexion d’accès à distance ou de réseau privé virtuel (VPN) dans NPS.

Après avoir exécuté l’Assistant, les stratégies suivantes sont créées :

- Une stratégie de demande de connexion
- Une stratégie réseau

Vous pouvez exécuter l’Assistant Nouvelle connexion d’accès à distance ou de réseau privé virtuel chaque fois que vous devez créer de nouvelles stratégies pour les serveurs d’accès à distance et les serveurs VPN.

L’exécution de l’Assistant Nouvelle connexion d’accès à distance ou de réseau privé virtuel n’est pas la seule étape requise pour déployer des serveurs d’accès à distance ou VPN en tant que clients RADIUS sur le serveur NPS. Les deux méthodes d’accès réseau requièrent le déploiement de composants matériels et logiciels supplémentaires.

L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-create-policies-for-dial-up-or-vpn-with-a-wizard"></a>Pour créer des stratégies d’accès à distance ou VPN à l’aide d’un Assistant

1. Ouvrez la console NPS. S’il n’est pas déjà sélectionné, cliquez sur **serveur NPS \(\)local** . Si vous souhaitez créer des stratégies sur un serveur NPS distant, sélectionnez le serveur.

2. Dans **prise en main** et la **configuration standard**, sélectionnez **serveur RADIUS pour les connexions d’accès à distance ou VPN**. Le texte et les liens sous le texte changent pour refléter votre sélection.

3. Cliquez sur **configurer un VPN ou établir une connexion à distance avec un Assistant**. L’Assistant Nouvelle connexion d’accès à distance ou de réseau privé virtuel s’ouvre.

4. Suivez les instructions de l’Assistant pour terminer la création de vos nouvelles stratégies.

## <a name="create-network-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Créer des stratégies réseau pour 802.1 X câblé ou sans fil à l’aide d’un Assistant

Vous pouvez utiliser cette procédure pour créer la stratégie de demande de connexion et la stratégie réseau qui sont requises pour déployer des commutateurs d’authentification 802.1 X ou des points d’accès sans fil 802.1 X en tant que clients protocole RADIUS (Remote Authentication Dial-In User Service) RADIUS sur le serveur NPS. Serveur RADIUS.

Cette procédure explique comment démarrer l’Assistant Nouvelle connexion câblée et sans fil sécurisée IEEE 802.1 X dans NPS.

Après avoir exécuté l’Assistant, les stratégies suivantes sont créées :

- Une stratégie de demande de connexion
- Une stratégie réseau

Vous pouvez exécuter l’Assistant Nouvelle connexion câblée et sans fil sécurisée IEEE 802.1 X chaque fois que vous devez créer des stratégies pour l’accès 802.1 X.

L’exécution de l’Assistant Nouvelle connexion câblée et sans fil sécurisée IEEE 802.1 X n’est pas la seule étape requise pour déployer des commutateurs d’authentification 802.1 X et des points d’accès sans fil en tant que clients RADIUS sur le serveur NPS. Les deux méthodes d’accès réseau requièrent le déploiement de composants matériels et logiciels supplémentaires.

L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-create-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Pour créer des stratégies pour 802.1 X câblé ou sans fil à l’aide d’un Assistant

1. Sur le serveur NPS, dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur serveur NPS ( **Network Policy Server**). La console NPS s’ouvre. 

2. S’il n’est pas déjà sélectionné, cliquez sur **serveur NPS \(\)local** . Si vous souhaitez créer des stratégies sur un serveur NPS distant, sélectionnez le serveur.

3. Dans **prise en main** et la **configuration standard**, sélectionnez **serveur RADIUS pour les connexions câblées ou sans fil 802.1 x**. Le texte et les liens sous le texte changent pour refléter votre sélection.

4. Cliquez sur **configurer 802.1 x à l’aide d’un Assistant**. L’Assistant Nouvelle connexion câblée et sans fil sécurisée IEEE 802.1 X s’ouvre.

5. Suivez les instructions de l’Assistant pour terminer la création de vos nouvelles stratégies.

## <a name="configure-nps-to-ignore-user-account-dial-in-properties"></a>Configurer NPS pour ignorer les propriétés d’accès à distance du compte d’utilisateur

Utilisez cette procédure pour configurer une stratégie de réseau NPS afin d’ignorer les propriétés de numérotation des comptes d’utilisateur dans Active Directory pendant le processus d’autorisation. Les comptes d’utilisateur dans Active Directory utilisateurs et ordinateurs disposent de propriétés d’accès à distance que le serveur NPS évalue pendant le processus d’autorisation, sauf si la propriété **autorisation d’accès réseau** du compte d’utilisateur est définie sur **contrôler l’accès via la stratégie de réseau NPS**. 

Il existe deux cas dans lesquels vous pouvez configurer NPS pour ignorer les propriétés de numérotation des comptes d’utilisateur dans Active Directory :

- Lorsque vous souhaitez simplifier l’autorisation NPS à l’aide de la stratégie réseau, mais que tous vos comptes d’utilisateur n’ont pas la propriété **autorisation d’accès réseau** définie sur **contrôler l’accès via la stratégie de réseau NPS**. Par exemple, certains comptes d’utilisateur peuvent avoir la propriété **autorisation d’accès réseau** du compte d’utilisateur définie sur **refuser l’accès** ou **autoriser l’accès**.

- Lorsque d’autres propriétés de numérotation des comptes d’utilisateur ne sont pas applicables au type de connexion configuré dans la stratégie réseau. Par exemple, les propriétés autres que le paramètre d' **autorisation d’accès réseau** s’appliquent uniquement aux connexions d’accès à distance ou VPN, mais la stratégie réseau que vous créez est destinée aux connexions sans fil ou de commutateur d’authentification.

Vous pouvez utiliser cette procédure pour configurer NPS afin d’ignorer les propriétés d’accès à distance du compte d’utilisateur. Si une demande de connexion correspond à la stratégie réseau dans laquelle cette case à cocher est activée, NPS n’utilise pas les propriétés de connexion d’accès du compte d’utilisateur pour déterminer si l’utilisateur ou l’ordinateur est autorisé à accéder au réseau. Seuls les paramètres de la stratégie réseau sont utilisés pour déterminer l’autorisation.

Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.

1. Sur le serveur NPS, dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur serveur NPS ( **Network Policy Server**). La console NPS s’ouvre.

2. Double-cliquez sur **stratégies**, cliquez sur **stratégies réseau**, puis, dans le volet d’informations, double-cliquez sur la stratégie que vous souhaitez configurer.

3. Dans la boîte de dialogue **Propriétés** de stratégie, sous l’onglet **vue d’ensemble** , dans **autorisation d’accès**, activez la case à cocher **Ignorer les propriétés d’accès à distance du compte d’utilisateur** , puis cliquez sur **OK**.

### <a name="to-configure-nps-to-ignore-user-account-dial-in-properties"></a>Pour configurer NPS afin d’ignorer les propriétés d’accès à distance du compte d’utilisateur



## <a name="configure-nps-for-vlans"></a>Configurer NPS pour les réseaux locaux virtuels

En utilisant des serveurs d’accès réseau et NPS sensibles aux réseaux locaux virtuels dans Windows Server 2016, vous pouvez fournir aux groupes d’utilisateurs un accès uniquement aux ressources réseau appropriées pour leurs autorisations de sécurité. Par exemple, vous pouvez fournir aux visiteurs un accès sans fil à Internet sans leur permettre d’accéder au réseau de votre organisation. 

En outre, les réseaux locaux virtuels vous permettent de regrouper logiquement les ressources réseau qui existent dans des emplacements physiques différents ou sur des sous-réseaux physiques différents. Par exemple, les membres de votre service commercial et leurs ressources réseau, tels que les ordinateurs clients, les serveurs et les imprimantes, peuvent se trouver dans différents bâtiments de votre organisation, mais vous pouvez placer toutes ces ressources sur un réseau local virtuel qui utilise la même adresse IP. plage d’adresses. Le réseau local virtuel fonctionne alors, du point de vue de l’utilisateur final, comme un sous-réseau unique.

Vous pouvez également utiliser des réseaux locaux virtuels lorsque vous souhaitez séparer un réseau entre différents groupes d’utilisateurs. Une fois que vous avez déterminé la manière dont vous souhaitez définir vos groupes, vous pouvez créer des groupes de sécurité dans le composant logiciel enfichable utilisateurs et ordinateurs Active Directory, puis ajouter des membres aux groupes.

### <a name="configure-a-network-policy-for-vlans"></a>Configurer une stratégie réseau pour les réseaux locaux virtuels

Vous pouvez utiliser cette procédure pour configurer une stratégie réseau qui attribue des utilisateurs à un réseau local virtuel. Lorsque vous utilisez du matériel réseau compatible VLAN, tel que des routeurs, des commutateurs et des contrôleurs d’accès, vous pouvez configurer la stratégie réseau pour demander aux serveurs d’accès de placer les membres de groupes de Active Directory spécifiques sur des réseaux locaux virtuels spécifiques. Cette possibilité de regrouper les ressources réseau logiquement avec les réseaux locaux virtuels offre une certaine souplesse lors de la conception et de l’implémentation de solutions réseau.

Quand vous configurez les paramètres d’une stratégie de réseau NPS pour une utilisation avec des réseaux locaux virtuels, vous devez configurer les attributs **tunnel-moyen-type**, **Tunnel-Pvt-Group-ID**, **tunnel-type**et **tunnel-tag**. 

Cette procédure est fournie à titre indicatif. la configuration de votre réseau peut nécessiter des paramètres différents de ceux décrits ci-dessous.

Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.

### <a name="to-configure-a-network-policy-for-vlans"></a>Pour configurer une stratégie réseau pour les réseaux locaux virtuels

1. Sur le serveur NPS, dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur serveur NPS ( **Network Policy Server**). La console NPS s’ouvre.

2. Double-cliquez sur **stratégies**, cliquez sur **stratégies réseau**, puis, dans le volet d’informations, double-cliquez sur la stratégie que vous souhaitez configurer.

3. Dans la boîte de dialogue **Propriétés** de stratégie, cliquez sur l’onglet **paramètres** .

4. Dans **Propriétés**de stratégie, dans **paramètres**, dans **attributs RADIUS**, vérifiez que **standard** est sélectionné.

5. Dans le volet d’informations, dans **attributs**, l’attribut de **type de service** est configuré avec la valeur par défaut **Framed**. Par défaut, pour les stratégies avec des méthodes d’accès VPN et d’accès à distance, l’attribut **Framed-Protocol** est configuré avec la valeur **PPP**. Pour spécifier d’autres attributs de connexion requis pour les réseaux locaux virtuels, cliquez sur **Ajouter**. La boîte de dialogue **Ajouter un attribut RADIUS standard** s’ouvre.

6. Dans **Ajouter un attribut RADIUS standard**, dans attributs, faites défiler la liste jusqu’à et ajoutez les attributs suivants :

    - **Tunnel-moyen-type**. Sélectionnez une valeur adaptée aux sélections précédentes que vous avez apportées à la stratégie. Par exemple, si la stratégie réseau que vous configurez est une stratégie sans fil, sélectionnez **valeur : 802 (comprend tous les supports 802 Media plus Ethernet)** .

    - **Tunnel-Pvt-Group-ID**. Entrez l’entier qui représente le nombre de réseaux locaux virtuels auxquels les membres du groupe seront affectés. 

    - **Type de tunnel**. Sélectionnez **réseaux locaux virtuels (VLAN)** .


7. Dans **Ajouter un attribut RADIUS standard**, cliquez sur **Fermer**. 

8. Si votre serveur d’accès réseau (NAS) requiert l’utilisation de l’attribut **tunnel-tag** , procédez comme suit pour ajouter l’attribut **tunnel-tag** à la stratégie réseau. Si votre documentation NAS ne mentionne pas cet attribut, ne l’ajoutez pas à la stratégie. Si nécessaire, ajoutez les attributs comme suit :

    - Dans **Propriétés**de la stratégie, dans **paramètres**, dans **attributs RADIUS**, cliquez sur **spécifique au fournisseur**. 

    - Dans le volet d’informations, cliquez sur **Ajouter**. La boîte de dialogue **Ajouter un attribut spécifique** à un fournisseur s’ouvre.

    - Dans **attributs**, faites défiler la liste jusqu’à et sélectionnez **tunnel-tag**, puis cliquez sur **Ajouter**. La boîte de dialogue **informations d’attribut** s’ouvre. 

    - Dans **valeur d’attribut**, tapez la valeur que vous avez obtenue dans la documentation de votre matériel.

## <a name="configure-the-eap-payload-size"></a>Configurer la taille de la charge utile EAP

Dans certains cas, les routeurs ou les pare-feu abandonnent les paquets, car ils sont configurés pour rejeter les paquets qui nécessitent une fragmentation.

Lorsque vous déployez NPS avec des stratégies réseau qui utilisent le protocole EAP (Extensible Authentication Protocol) \(EAP\) avec Transport Layer Security \(TLS\)ou EAP-TLS, comme méthode d’authentification, l’unité de transmission maximale par défaut \(MTU\) que le serveur NPS utilise pour les charges utiles EAP est de 1500 octets. 

Cette taille maximale pour la charge utile EAP peut créer des messages RADIUS qui nécessitent une fragmentation par un routeur ou un pare-feu entre le serveur NPS et un client RADIUS. Dans ce cas, un routeur ou un pare-feu placé entre le client RADIUS et le serveur NPS peut ignorer silencieusement certains fragments, provoquant ainsi l’échec de l’authentification et l’incapacité du client d’accès à se connecter au réseau.

Utilisez la procédure suivante pour réduire la taille maximale que NPS utilise pour les charges utiles EAP en réglant l’attribut Framed-MTU dans une stratégie réseau sur une valeur inférieure ou égale à 1344.

Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.

### <a name="to-configure-the-framed-mtu-attribute"></a>Pour configurer l’attribut Framed-MTU

1. Sur le serveur NPS, dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur serveur NPS ( **Network Policy Server**). La console NPS s’ouvre.
 
2. Double-cliquez sur **stratégies**, cliquez sur **stratégies réseau**, puis, dans le volet d’informations, double-cliquez sur la stratégie que vous souhaitez configurer.

3. Dans la boîte de dialogue **Propriétés** de stratégie, cliquez sur l’onglet **paramètres** .

4. Dans **paramètres**, dans **attributs RADIUS**, cliquez sur **standard**. Dans le volet d’informations, cliquez sur **Ajouter**. La boîte de dialogue **Ajouter un attribut RADIUS standard** s’ouvre.

5. Dans **attributs**, faites défiler la liste jusqu’à et cliquez sur **Framed-MTU**, puis cliquez sur **Ajouter**. La boîte de dialogue **informations d’attribut** s’ouvre.

6. Dans **valeur d’attribut**, tapez une valeur inférieure ou égale à **1344**. Cliquez sur **OK**, sur **Fermer**, puis sur **OK**.



Pour plus d’informations sur les stratégies réseau, consultez [stratégies réseau](nps-np-overview.md).

Pour obtenir des exemples de syntaxe de mise en correspondance de modèle pour spécifier des attributs de stratégie réseau, consultez [utiliser des expressions régulières dans NPS](nps-crp-reg-expressions.md).

Pour plus d’informations sur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).
