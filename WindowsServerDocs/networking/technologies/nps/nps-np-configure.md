---
title: Configurer des stratégies de réseau
description: Cette rubrique fournit une vue d’ensemble de la configuration de la stratégie réseau pour le serveur NPS dans Windows Server2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fe77655a-e2be-4949-92e1-aaaa215d86ea
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9abcd9343f10100884f96d17d82704382e2af760
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-network-policies"></a>Configurer des stratégies de réseau

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour configurer des stratégies de réseau dans NPS.

## <a name="add-a-network-policy"></a>Ajouter une stratégie réseau

Réseau serveur NPS \(NPS\) utilise des stratégies de réseau et les propriétés de numérotation des comptes d’utilisateur pour déterminer si une demande de connexion est autorisé à se connecter au réseau.

Vous pouvez utiliser cette procédure pour configurer une nouvelle stratégie de réseau dans la console NPS ou la console accès à distance.

### <a name="performing-authorization"></a>L’autorisation

Lorsque le serveur NPS effectue l’autorisation d’une demande de connexion, il compare la demande à chaque stratégie réseau dans la liste des stratégies, à partir de la première stratégie, puis à déplacer vers le bas de la liste des stratégies configurées. Si NPS trouve une stratégie dont les conditions correspondent à la demande de connexion, NPS utilise la stratégie correspondante et les propriétés de numérotation du compte d’utilisateur pour exécuter une autorisation. Si les propriétés de numérotation du compte d’utilisateur sont configurées pour accorder l’accès ou contrôle d’accès via la stratégie de réseau et la demande de connexion est autorisée, NPS applique les paramètres qui sont configurés dans la stratégie de réseau pour la connexion.

Si NPS ne trouve pas une stratégie de réseau qui correspond à la demande de connexion, la demande de connexion est rejetée, sauf si les propriétés d’accès à distance sur le compte d’utilisateur sont définies pour accorder l’accès.

Si les propriétés de numérotation du compte d’utilisateur sont définies pour refuser l’accès, la demande de connexion est rejetée par le serveur NPS.

### <a name="key-settings"></a>Paramètres de clé

Lorsque vous utilisez l’Assistant Nouvelle stratégie de réseau pour créer une stratégie de réseau, la valeur que vous spécifiez dans **méthode de connexion réseau** est utilisé pour configurer automatiquement le **Type de stratégie** condition: 

- Si vous conservez la valeur par défaut non spécifié, la stratégie de réseau que vous créez est évaluée par NPS pour tous les types de connexion réseau qui sont à l’aide de n’importe quel type de serveur d’accès réseau (NAS).
- Si vous spécifiez une méthode de connexion réseau, NPS évalue la stratégie réseau uniquement si la demande de connexion provient du type de serveur d’accès réseau que vous spécifiez.

Sur le **l’autorisation d’accès** page, vous devez sélectionner **accès accordé** si vous souhaitez que la stratégie pour permettre aux utilisateurs de se connecter à votre réseau. Si vous souhaitez que la stratégie pour empêcher les utilisateurs de se connecter à votre réseau, sélectionnez **accès refusé**. 

Si vous souhaitez que les autorisations d’accès à être déterminé par l’utilisateur à distance dans Propriétés du compte dans ActiveDirectory&reg; \(ADDS\) Services de domaine, vous pouvez sélectionner le **accès est déterminé par l’utilisateur propriétés de numérotation** case à cocher.

L’appartenance au groupe **Admins du domaine**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-add-a-network-policy"></a>Pour ajouter une stratégie de réseau 

1. Ouvrez la console NPS, puis double-cliquez sur **stratégies**.

2. Dans l’arborescence de la console, cliquez sur **stratégies réseau**, puis cliquez sur **New**. L’Assistant de nouvelle stratégie de réseau s’ouvre.

3. Utilisez l’Assistant de nouvelle stratégie de réseau pour créer une stratégie.

## <a name="create-network-policies-for-dial-up-or-vpn-with-a-wizard"></a>Créer des stratégies de réseau pour l’accès à distance ou VPN à l’aide d’un Assistant

Vous pouvez utiliser cette procédure pour créer les stratégies de demande de connexion et les stratégies de réseau nécessaires pour déployer des serveurs d’accès à distance ou les serveurs de réseau privé virtuel \(VPN\) en tant que clients Remote Authentication Dial-In User Service \(RADIUS\) sur le serveur NPS RADIUS.

>[!NOTE]
>Les ordinateurs clients, tels que les ordinateurs portables et les autres ordinateurs qui exécutent les systèmes d’exploitation clients, ne sont pas des clients RADIUS. Clients RADIUS sont des serveurs d’accès réseau, tels que les points d’accès sans fil, les commutateurs d’authentification 802. 1 X, les serveurs \(VPN\) de réseau privé virtuel et les serveurs d’accès à distance, car ces périphériques utilisent le protocole RADIUS pour communiquer avec les serveurs RADIUS tels que les serveurs NPS.

Cette procédure explique comment ouvrir l’Assistant de nouveau accès à distance ou les connexions de réseau privé virtuel dans NPS.

Après avoir exécuté l’Assistant, les stratégies suivantes sont créées:

- Stratégie de demande d’une seule connexion
- Une stratégie réseau

Vous pouvez exécuter l’Assistant de nouveau accès à distance ou les connexions de réseau privé virtuel chaque fois que vous avez besoin créer des stratégies pour les serveurs d’accès à distance et les serveurs VPN.

Exécutez l’Assistant de nouveau accès à distance ou les connexions de réseau privé virtuel n’est pas la seule étape requise pour déployer l’accès à distance ou des serveurs VPN en tant que clients RADIUS sur le serveur NPS. Les deux méthodes d’accès réseau nécessitent le déploiement des composants matériels et logiciels supplémentaires.

L’appartenance au groupe **Admins du domaine**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-create-policies-for-dial-up-or-vpn-with-a-wizard"></a>Pour créer des stratégies d’accès à distance ou VPN avec un Assistant

1. Ouvrez la console NPS. Si elle n’est pas déjà sélectionnée, cliquez sur **NPS \(Local\)**. Si vous souhaitez créer des stratégies sur un serveur NPS à distance, sélectionnez le serveur.

2. Dans **prise en main** et **Configuration Standard**, sélectionnez **serveur RADIUS pour l’accès à distance ou les connexions VPN**. Le texte et les liens situés sous la modification de texte pour refléter votre sélection.

3. Cliquez sur **configurer une connexion VPN ou accès à distance avec un Assistant**. L’Assistant de nouveau accès à distance ou les connexions de réseau privé virtuel s’ouvre.

4. Suivez les instructions de l’Assistant pour terminer la création de vos nouvelles stratégies.

## <a name="create-network-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Créer des stratégies de réseau pour 802. 1 X câblés ou sans fil avec un Assistant

Vous pouvez utiliser cette procédure pour créer la stratégie de demande de connexion et la stratégie réseau qui sont requises pour déployer des commutateurs d’authentification 802. 1 X ou points de l’accès sans fil 802. 1 X en tant que clients Authentication Dial-In utilisateur RADIUS (Remote Service) sur le serveur NPS RADIUS.

Cette procédure explique comment démarrer l’Assistant Nouveau IEEE 802. 1 X sécurisé câblé et les connexions sans fil dans NPS.

Après avoir exécuté l’Assistant, les stratégies suivantes sont créées:

- Stratégie de demande d’une seule connexion
- Une stratégie réseau

Vous pouvez exécuter l’Assistant Nouveau IEEE 802. 1 X sécurisé câblé et les connexions sans fil chaque fois que vous avez besoin créer des stratégies d’accès de 802. 1 X.

Exécution de l’Assistant Nouveau IEEE 802. 1 X sécurisé câblé et les connexions sans fil n’est pas la seule étape requise pour déployer 802. 1 X commutateurs d’authentification et les points d’accès sans fil en tant que clients RADIUS sur le serveur NPS. Les deux méthodes d’accès réseau nécessitent le déploiement des composants matériels et logiciels supplémentaires.

L’appartenance au groupe **Admins du domaine**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-create-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Pour créer des stratégies pour 802. 1 X câblés ou sans fil avec un Assistant

1. Sur le serveur NPS, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **serveur NPS**. La console NPS s’ouvre. 

2. Si elle n’est pas déjà sélectionnée, cliquez sur **NPS \(Local\)**. Si vous souhaitez créer des stratégies sur un serveur NPS à distance, sélectionnez le serveur.

3. Dans **prise en main** et **Configuration Standard**, sélectionnez **serveur RADIUS pour les connexions câblées ou sans fil X 802.1**. Le texte et les liens situés sous la modification de texte pour refléter votre sélection.

4. Cliquez sur **configurer 802. 1 X à l’aide d’un Assistant**. L’Assistant Nouveau IEEE 802. 1 X sécurisé câblé et les connexions sans fil s’ouvre.

5. Suivez les instructions de l’Assistant pour terminer la création de vos nouvelles stratégies.

## <a name="configure-nps-to-ignore-user-account-dial-in-properties"></a>Configurer NPS pour ignorer les propriétés d’accès du compte d’utilisateur

Utilisez cette procédure pour configurer une stratégie de réseau NPS pour ignorer les propriétés de numérotation des comptes d’utilisateur dans ActiveDirectory pendant le processus d’autorisation. Comptes d’utilisateur dans ActiveDirectory Users and Computers ont des propriétés à distance dans NPS évalue pendant le processus d’autorisation, sauf si la **autorisation d’accès réseau** propriété du compte d’utilisateur est définie sur **contrôler l’accès via la stratégie de réseau NPS**. 

Il existe deux cas où vous devrez configurer NPS pour ignorer les propriétés de numérotation des comptes d’utilisateur dans ActiveDirectory:

- Lorsque vous souhaitez simplifier d’autorisation du serveur NPS à l’aide de la stratégie réseau, mais n’ont pas tous vos comptes d’utilisateur le **autorisation d’accès réseau** propriété **contrôler l’accès via la stratégie de réseau NPS**. Par exemple, certains comptes d’utilisateurs peuvent avoir le **autorisation d’accès réseau** propriété du compte d’utilisateur est définie sur **refuser l’accès** ou **autoriser l’accès**.

- Lorsque les autres propriétés de numérotation des comptes d’utilisateur ne sont pas applicables au type de connexion qui est configuré dans la stratégie de réseau. Par exemple, les propriétés autres que le **autorisation d’accès réseau** paramètre s’appliquent uniquement à l’accès à distance ou les connexions VPN, mais la stratégie de réseau que vous créez est pour les connexions sans fil ou authentification commutateur.

Vous pouvez utiliser cette procédure pour configurer NPS pour ignorer les propriétés d’accès du compte d’utilisateur. Si une demande de connexion correspond à la stratégie de réseau sur lequel cette case à cocher est sélectionnée, NPS n’utilise pas les propriétés de numérotation du compte d’utilisateur pour déterminer si l’utilisateur ou l’ordinateur est autorisé à accéder au réseau; seuls les paramètres de la stratégie réseau sont utilisés pour déterminer l’autorisation.

L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

1. Sur le serveur NPS, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **serveur NPS**. La console NPS s’ouvre.

2. Double-cliquez sur **stratégies**, cliquez sur **stratégies réseau**, puis, dans le volet d’informations, double-cliquez sur la stratégie que vous souhaitez configurer.

3. Dans la stratégie **propriétés** boîte de dialogue le **vue d’ensemble** onglet **l’autorisation d’accès**, sélectionnez le **ignorer les propriétés d’accès du compte d’utilisateur** case à cocher, puis cliquez sur **OK**.

### <a name="to-configure-nps-to-ignore-user-account-dial-in-properties"></a>Pour configurer NPS pour ignorer les propriétés d’accès du compte d’utilisateur



## <a name="configure-nps-for-vlans"></a>Configurer NPS pour les réseaux locaux virtuels

À l’aide de serveurs d’accès réseau prenant en charge VLAN et le serveur NPS dans Windows Server2016, vous pouvez fournir des groupes d’utilisateurs avec accès uniquement aux ressources réseau qui sont appropriés pour leurs autorisations de sécurité. Par exemple, vous pouvez fournir des visiteurs avec accès sans fil à Internet sans leur accorder l’accès réseau de votre organisation. 

En outre, réseaux locaux virtuels vous permettent de logiquement les ressources de réseau de groupe qui existent dans différents emplacements physiques ou sur des sous-réseaux physiques différents. Par exemple, les membres de votre département et leurs ressources réseau, tels que les ordinateurs clients, serveurs et des imprimantes, peuvent se trouver dans plusieurs bâtiments différents au sein de votre organisation, mais vous pouvez placer toutes ces ressources sur un réseau local virtuel qui utilise la même plage d’adresses IP. Le réseau local virtuel puis les fonctions, à partir de la perspective de l’utilisateur final, comme un seul sous-réseau.

Vous pouvez également utiliser des réseaux locaux virtuels lorsque vous voulez isoler un réseau entre les différents groupes d’utilisateurs. Une fois que vous avez déterminé la manière dont vous souhaitez définir vos groupes, vous pouvez créer des groupes de sécurité dans les utilisateurs ActiveDirectory et les ordinateurs d’un composant logiciel enfichable et puis ajouter des membres aux groupes.

### <a name="configure-a-network-policy-for-vlans"></a>Configurer une stratégie de réseau pour les réseaux locaux virtuels

Vous pouvez utiliser cette procédure pour configurer une stratégie de réseau qui affecte les utilisateurs à un réseau local virtuel. Lorsque vous utilisez prenant en charge VLAN matériel réseau, tels que les routeurs, les commutateurs et les contrôleurs d’accès, vous pouvez configurer la stratégie de réseau pour demander les serveurs d’accès pour placer les membres de groupes ActiveDirectory spécifiques sur des réseaux locaux virtuels spécifiques. Cette possibilité de regrouper les ressources réseau logique avec VLAN flexibilité lors de la conception et l’implémentation des solutions de réseau.

Lorsque vous configurez les paramètres d’une stratégie de réseau NPS pour une utilisation avec les réseaux locaux virtuels, vous devez configurer les attributs **-moyenne-Type de Tunnel**, **Tunnel-Pvt-Group-ID**, **Type de Tunnel**, et **balise de Tunnel**. 

Cette procédure est fournie à titre indicatif; configuration de votre réseau peut nécessiter des paramètres différents de ceux décrits ci-dessous.

L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-configure-a-network-policy-for-vlans"></a>Pour configurer une stratégie de réseau pour les réseaux locaux virtuels

1. Sur le serveur NPS, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **serveur NPS**. La console NPS s’ouvre.

2. Double-cliquez sur **stratégies**, cliquez sur **stratégies réseau**, puis, dans le volet d’informations, double-cliquez sur la stratégie que vous souhaitez configurer.

3. Dans la stratégie **propriétés** boîte de dialogue, cliquez sur le **paramètres** onglet.

4. Dans la stratégie **propriétés**, dans **paramètres**, dans **attributs RADIUS**, assurez-vous que **Standard** est sélectionné.

5. Dans le volet d’informations, dans **attributs**, le **Service-Type** attribut est configuré avec la valeur par défaut **encadré**. Par défaut, pour les stratégies avec les méthodes d’accès de VPN et accès à distance, le **protocole encadré** attribut est configuré avec la valeur **PPP**. Pour spécifier les attributs de connexion supplémentaires requis pour les réseaux locaux virtuels, cliquez sur **ajouter**. Le **ajouter un attribut RADIUS Standard** boîte de dialogue s’ouvre.

6. Dans **ajouter un attribut RADIUS Standard**, dans les attributs, faites défiler vers le bas et ajoutez les attributs suivants:

    - **Type de tunnel moyenne**. Sélectionnez une valeur appropriée pour les sélections que vous avez effectués pour la stratégie. Par exemple, si vous configurez la stratégie de réseau est une stratégie sans fil, sélectionnez **valeur: 802 (format canonique Ethernet plus media inclut tous les 802)**.

    - **Tunnel-Pvt-Group-ID**. Entrez le nombre entier qui représente le nombre de réseau local virtuel auquel seront attribués les membres du groupe. 

    - **Type de tunnel**. Sélectionnez **réseaux locaux virtuels (VLAN)**.


7. Dans **ajouter un attribut RADIUS Standard**, cliquez sur **fermer**. 

8. Si votre serveur d’accès réseau (NAS) requiert l’utilisation de la **balise de Tunnel** d’attribut, procédez comme suit pour ajouter le **balise de Tunnel** de l’attribut à la stratégie réseau. Si votre documentation NAS ne mentionne pas cet attribut, ne l’ajoutez pas à la stratégie. Si nécessaire, ajoutez les attributs comme suit:

    - Dans la stratégie **propriétés**, dans **paramètres**, dans **attributs RADIUS**, cliquez sur **spécifique du fournisseur**. 

    - Dans le volet d’informations, cliquez sur **ajouter**. Le **ajouter un attribut spécifique fournisseur** boîte de dialogue s’ouvre.

    - Dans **attributs**, faites défiler vers le bas, puis sélectionnez **balise de Tunnel**, puis cliquez sur **ajouter**. Le **informations d’attribut** boîte de dialogue s’ouvre. 

    - Dans **valeur d’attribut**, tapez la valeur que vous avez obtenu à partir de la documentation de votre matériel.

## <a name="configure-the-eap-payload-size"></a>Configurer la taille de la charge utile EAP

Dans certains cas, les routeurs ou les pare-feu rejeter des paquets car ils sont configurés pour rejeter les paquets qui nécessitent la fragmentation.

Lorsque vous déployez un serveur NPS avec les stratégies de réseau qui utilisent le \(EAP\) Extensible Authentication Protocol Transport Layer Security \(TLS\) ou EAP-TLS, comme méthode d’authentification, l’unité de transmission maximale par défaut \(MTU\) que NPS utilise pour les charges utiles EAP est 1500octets. 

Cette taille maximale de la charge utile EAP permettre créer des messages RADIUS exigent une fragmentation par un routeur ou un pare-feu entre le serveur NPS et un client RADIUS. Si c’est le cas, un routeur ou un pare-feu placé entre le client RADIUS et le serveur NPS peut ignorer en mode silencieux certains fragments, ce qui entraîne un échec d’authentification et l’impossibilité du client d’accès pour se connecter au réseau.

La procédure suivante permet de réduire la taille maximale que NPS utilise pour les charges utiles EAP en ajustant l’attribut encadré-MTU dans une stratégie réseau sur une valeur non supérieure à 1344.

L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-configure-the-framed-mtu-attribute"></a>Pour configurer l’attribut encadré-MTU

1. Sur le serveur NPS, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **serveur NPS**. La console NPS s’ouvre.
 
2. Double-cliquez sur **stratégies**, cliquez sur **stratégies réseau**, puis, dans le volet d’informations, double-cliquez sur la stratégie que vous souhaitez configurer.

3. Dans la stratégie **propriétés** boîte de dialogue, cliquez sur le **paramètres** onglet.

4. Dans **paramètres**, dans **attributs RADIUS**, cliquez sur **Standard**. Dans le volet d’informations, cliquez sur **ajouter**. Le **ajouter un attribut RADIUS Standard** boîte de dialogue s’ouvre.

5. Dans **attributs**, faites défiler vers le bas et cliquez sur **encadré-MTU**, puis cliquez sur **ajouter**. Le **informations d’attribut** boîte de dialogue s’ouvre.

6. Dans **valeur d’attribut**, tapez une valeur égale ou inférieure à **1344**. Cliquez sur **OK**, cliquez sur **fermer**, puis cliquez sur **OK**.



Pour plus d’informations sur les stratégies de réseau, voir [stratégies réseau](nps-np-overview.md).

Pour les exemples de syntaxe des critères spéciaux pour spécifier les attributs de stratégie du réseau, consultez [utiliser des Expressions régulières dans NPS](nps-crp-reg-expressions.md).

Pour plus d’informations sur le serveur NPS, voir [serveur NPS (Network Policy)](nps-top.md).
