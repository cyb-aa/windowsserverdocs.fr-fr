---
title: Configurer des stratégies réseau
description: Cette rubrique fournit une vue d’ensemble de la configuration de la stratégie réseau pour le serveur NPS dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fe77655a-e2be-4949-92e1-aaaa215d86ea
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e8a03a6c67ddd59549cefc9742ca92e0702f92a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842000"
---
# <a name="configure-network-policies"></a>Configurer des stratégies réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour configurer des stratégies de réseau dans NPS.

## <a name="add-a-network-policy"></a>Ajouter une stratégie de réseau

Network Policy Server \(NPS\) utilise réseau des stratégies et les propriétés de numérotation des comptes d’utilisateur pour déterminer si une demande de connexion est autorisée à se connecter au réseau.

Vous pouvez utiliser cette procédure pour configurer une nouvelle stratégie de réseau dans la console NPS ou la console accès à distance.

### <a name="performing-authorization"></a>Effectue une autorisation

Lorsque NPS effectue l’autorisation d’une demande de connexion, il compare la demande à chaque stratégie réseau dans la liste ordonnée des stratégies, en commençant par la première stratégie, puis déplacement vers le bas la liste des stratégies configurées. Si NPS trouve une stratégie dont les conditions correspondent à la demande de connexion, NPS utilise la stratégie de correspondance et les propriétés d’accès à distance du compte d’utilisateur pour exécuter une autorisation. Si les propriétés d’accès à distance du compte d’utilisateur sont configurées pour accorder l’accès ou contrôle d’accès via la stratégie de réseau et la demande de connexion est autorisée, NPS applique les paramètres qui sont configurés dans la stratégie de réseau pour la connexion.

Si NPS ne trouve pas une stratégie réseau qui correspond à la demande de connexion, la demande de connexion est rejetée, sauf si les propriétés d’accès à distance sur le compte d’utilisateur sont définies pour accorder l’accès.

Si les propriétés d’accès à distance du compte d’utilisateur sont définies pour refuser l’accès, la demande de connexion est rejetée par le serveur NPS.

### <a name="key-settings"></a>Paramètres de clé

Lorsque vous utilisez l’Assistant Nouvelle stratégie de réseau pour créer une stratégie de réseau, la valeur que vous spécifiez dans **méthode de connexion réseau** est utilisé pour configurer automatiquement le **Type de stratégie** condition : 

- Si vous conservez la valeur par défaut non spécifié, la stratégie de réseau que vous créez est évaluée par le serveur NPS pour tous les types de connexion de réseau qui sont à l’aide de n’importe quel type de serveur d’accès réseau (NAS).
- Si vous spécifiez une méthode de connexion réseau, NPS évalue la stratégie réseau uniquement si la demande de connexion provient du type de serveur d’accès réseau que vous spécifiez.

Sur le **l’autorisation d’accès** page, vous devez sélectionner **accès accordé** si vous souhaitez que la stratégie pour permettre aux utilisateurs pour se connecter à votre réseau. Si vous souhaitez que la stratégie pour empêcher les utilisateurs de se connecter à votre réseau, sélectionnez **accès refusé**. 

Si vous souhaitez l’autorisation d’accès sera déterminé par l’accès à distance dans Propriétés du compte utilisateur dans Active Directory&reg; les Services de domaine \(AD DS\), vous pouvez sélectionner le **accès est déterminé par l’utilisateur aux propriétés** case à cocher.

L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-add-a-network-policy"></a>Pour ajouter une stratégie de réseau 

1. Ouvrez la console NPS, puis double-cliquez sur **stratégies**.

2. Dans l’arborescence de la console, cliquez sur **stratégies réseau**, puis cliquez sur **New**. L’Assistant Nouvelle stratégie de réseau s’ouvre.

3. Pour créer une stratégie, utilisez l’Assistant Nouvelle stratégie de réseau.

## <a name="create-network-policies-for-dial-up-or-vpn-with-a-wizard"></a>Créer des stratégies de réseau pour l’accès à distance ou VPN avec un Assistant

Vous pouvez utiliser cette procédure pour créer des stratégies de demande de la connexion et stratégies nécessaires pour déployer des serveurs d’accès à distance ou réseau privé virtuel réseau \(VPN\) serveurs en tant que Remote Authentication Dial-In User Service \(RADIUS\) clients au serveur NPS RADIUS.

>[!NOTE]
>Les ordinateurs clients, tels que les ordinateurs portables et les autres ordinateurs qui exécutent les systèmes d’exploitation clients, ne sont pas des clients RADIUS. Clients RADIUS sont des serveurs d’accès réseau, tels que les points d’accès sans fil, les commutateurs de l’authentification 802. 1 X, réseau privé virtuel \(VPN\) serveurs et les serveurs d’accès à distance, car ces périphériques utilisent le protocole RADIUS pour communiquer avec les serveurs RADIUS tels que NPSs.

Cette procédure explique comment ouvrir l’Assistant nouveau accès à distance ou les connexions de réseau privé virtuel dans NPS.

Après avoir exécuté l’Assistant, les stratégies suivantes sont créées :

- Stratégie de demande une connexion
- Une stratégie réseau

Vous pouvez exécuter l’Assistant de nouveau accès à distance ou les connexions de réseau privé virtuel chaque fois que vous avez besoin créer des stratégies pour les serveurs d’accès à distance et les serveurs VPN.

L’Assistant de nouveau accès à distance ou les connexions de réseau privé virtuel en cours d’exécution n’est pas la seule étape requise pour déployer l’accès à distance ou des serveurs VPN en tant que clients RADIUS au serveur NPS. Les deux méthodes d’accès réseau nécessitent le déploiement des composants matériels et logiciels supplémentaires.

L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-create-policies-for-dial-up-or-vpn-with-a-wizard"></a>Pour créer des stratégies d’accès à distance ou VPN avec un Assistant

1. Ouvrez la console NPS. Si ce n’est pas déjà fait, cliquez sur **NPS \(Local\)**. Si vous souhaitez créer des stratégies sur un serveur NPS à distance, sélectionnez le serveur.

2. Dans **mise en route** et **Configuration Standard**, sélectionnez **serveur RADIUS pour l’accès à distance ou les connexions VPN**. Le texte et les liens sous la modification du texte pour refléter votre sélection.

3. Cliquez sur **configurer une connexion VPN ou accès à distance avec un Assistant**. L’Assistant de nouveau accès à distance ou les connexions de réseau privé virtuel s’ouvre.

4. Suivez les instructions dans l’Assistant pour terminer la création de vos nouvelles stratégies.

## <a name="create-network-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Créer des stratégies de réseau pour 802. 1 X câblé ou sans fil avec un Assistant

Vous pouvez utiliser cette procédure pour créer la stratégie de demande de connexion et de la stratégie réseau qui sont nécessaires pour déployer des commutateurs de l’authentification 802. 1 X ou points de l’accès sans fil 802. 1 X en tant que clients de l’authentification Dial-In Service RADIUS (Remote User) au serveur NPS Serveur RADIUS.

Cette procédure explique comment démarrer l’Assistant câblé sécuriser de nouveau IEEE 802. 1 X et les connexions sans fil dans NPS.

Après avoir exécuté l’Assistant, les stratégies suivantes sont créées :

- Stratégie de demande une connexion
- Une stratégie réseau

Vous pouvez exécuter l’Assistant câblé sécuriser de nouveau IEEE 802. 1 X et les connexions sans fil chaque fois que vous avez besoin créer des stratégies d’accès de 802. 1 X.

Exécution de l’Assistant Nouvelle IEEE 802. 1 X sécurisé câblé et les connexions sans fil n’est pas la seule étape requise pour déployer 802. 1 X commutateurs d’authentification et les points d’accès sans fil en tant que clients RADIUS au serveur NPS. Les deux méthodes d’accès réseau nécessitent le déploiement des composants matériels et logiciels supplémentaires.

L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-create-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Pour créer des stratégies pour 802. 1 X câblé ou sans fil avec un Assistant

1. Sur le serveur NPS, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **Network Policy Server**. La console NPS s’ouvre. 

2. Si ce n’est pas déjà fait, cliquez sur **NPS \(Local\)**. Si vous souhaitez créer des stratégies sur un serveur NPS à distance, sélectionnez le serveur.

3. Dans **mise en route** et **Configuration Standard**, sélectionnez **serveur RADIUS pour les connexions câblées ou sans fil de X 802.1**. Le texte et les liens sous la modification du texte pour refléter votre sélection.

4. Cliquez sur **configurer 802. 1 X à l’aide d’un Assistant**. L’Assistant Nouvelle IEEE 802. 1 X sécurisé câblé et les connexions sans fil s’ouvre.

5. Suivez les instructions dans l’Assistant pour terminer la création de vos nouvelles stratégies.

## <a name="configure-nps-to-ignore-user-account-dial-in-properties"></a>Configurer NPS afin d’ignorer les propriétés d’accès à distance du compte d’utilisateur

Cette procédure permet de configurer une stratégie de réseau NPS pour ignorer les propriétés de numérotation des comptes d’utilisateur dans Active Directory pendant le processus d’autorisation. Comptes d’utilisateur dans Active Directory Users and Computers ont des propriétés dans l’accès à NPS évalue pendant le processus d’autorisation, sauf si le **l’autorisation d’accès réseau** propriété du compte d’utilisateur est définie sur **contrôle accès via la stratégie de réseau de NPS**. 

Il existe deux cas où vous pouvez souhaiter configurer NPS pour ignorer les propriétés d’accès à distance des comptes d’utilisateur dans Active Directory :

- Lorsque vous souhaitez simplifier d’autorisation de serveur NPS à l’aide de stratégie de réseau, mais pas tous vos comptes d’utilisateur ont la **l’autorisation d’accès réseau** propriété définie sur **contrôler l’accès via la stratégie de réseau de NPS**. Par exemple, certains comptes d’utilisateur peuvent avoir le **l’autorisation d’accès réseau** la valeur de propriété du compte d’utilisateur **refuser l’accès** ou **autoriser l’accès**.

- Lorsque les autres propriétés de numérotation des comptes d’utilisateur ne sont pas applicables au type de connexion qui est configuré dans la stratégie de réseau. Par exemple, les propriétés autres que le **l’autorisation d’accès réseau** paramètre sont uniquement applicables aux connexions VPN ou d’accès à distance, mais la stratégie de réseau que vous créez est pour les connexions de commutateur sans fil d’authentification.

Vous pouvez utiliser cette procédure pour configurer NPS afin d’ignorer les propriétés d’accès à distance du compte d’utilisateur. Si une demande de connexion correspond à la stratégie de réseau où cette case est cochée, NPS n’utilise pas les propriétés d’accès à distance du compte d’utilisateur pour déterminer si l’utilisateur ou l’ordinateur est autorisé à accéder au réseau ; uniquement les paramètres dans la stratégie de réseau sont utilisés pour déterminer l’autorisation.

Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.

1. Sur le serveur NPS, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **Network Policy Server**. La console NPS s’ouvre.

2. Double-cliquez sur **stratégies**, cliquez sur **stratégies réseau**, puis, dans le volet détails, double-cliquez sur la stratégie que vous souhaitez configurer.

3. Dans la stratégie **propriétés** boîte de dialogue le **vue d’ensemble** sous l’onglet **l’autorisation d’accès**, sélectionnez le **ignorer l’accès à distance dans Propriétés du compte utilisateur**case à cocher, puis cliquez sur **OK**.

### <a name="to-configure-nps-to-ignore-user-account-dial-in-properties"></a>Pour configurer NPS afin d’ignorer les propriétés d’accès à distance du compte d’utilisateur



## <a name="configure-nps-for-vlans"></a>Configurer NPS pour les réseaux locaux virtuels

À l’aide des serveurs d’accès réseau prenant en charge les VLAN et le serveur NPS dans Windows Server 2016, vous pouvez fournir des groupes d’utilisateurs avec accès uniquement aux ressources réseau qui sont adaptées à leurs autorisations de sécurité. Par exemple, vous pouvez fournir des visiteurs avec un accès sans fil à Internet sans leur accorder l’accès réseau de votre organisation. 

En outre, réseaux locaux virtuels vous permettent de logiquement les ressources de réseau de groupe qui existent dans des emplacements physiques différents ou sur des sous-réseaux physiques différents. Par exemple, les membres de votre département des ventes et de leurs ressources réseau, telles que les ordinateurs clients, les serveurs et les imprimantes peuvent se trouver dans plusieurs bâtiments différents au sein de votre organisation, mais vous pouvez placer toutes ces ressources sur un réseau local virtuel qui utilise la même adresse IP plage d’adresses. Le réseau local virtuel ensuite les fonctions, à partir de la perspective de l’utilisateur final, comme un seul sous-réseau.

Vous pouvez également utiliser des réseaux locaux virtuels lorsque vous souhaitez séparer un réseau entre différents groupes d’utilisateurs. Après avoir déterminé la façon dont vous souhaitez définir vos groupes, vous pouvez créer des groupes de sécurité dans Active Directory utilisateurs et ordinateurs d’un composant logiciel enfichable et puis ajoutez des membres aux groupes.

### <a name="configure-a-network-policy-for-vlans"></a>Configurer une stratégie de réseau pour les réseaux locaux virtuels

Vous pouvez utiliser cette procédure pour configurer une stratégie de réseau qui affecte les utilisateurs à un réseau local virtuel. Lorsque vous utilisez le matériel de réseau prenant en charge les VLAN, tels que les routeurs, les commutateurs et les contrôleurs de l’accès, vous pouvez configurer la stratégie de réseau pour indiquer les serveurs d’accès pour placer les membres de groupes Active Directory spécifiques sur des réseaux locaux virtuels spécifiques. Cette possibilité de regrouper les ressources de réseau logique avec les réseaux locaux virtuels fournit la flexibilité lors de la conception et implémentation de solutions de réseau.

Lorsque vous configurez les paramètres d’une stratégie de réseau NPS pour une utilisation avec les réseaux locaux virtuels, vous devez configurer les attributs **Tunnel-Medium-Type**, **Tunnel-Pvt-Group-ID**, **duTypedeTunnel**, et **Tunnel-Tag**. 

Cette procédure est fournie à titre indicatif ; configuration de votre réseau peut nécessiter des paramètres différents de ceux décrits ci-dessous.

Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.

### <a name="to-configure-a-network-policy-for-vlans"></a>Pour configurer une stratégie de réseau pour les réseaux locaux virtuels

1. Sur le serveur NPS, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **Network Policy Server**. La console NPS s’ouvre.

2. Double-cliquez sur **stratégies**, cliquez sur **stratégies réseau**, puis, dans le volet détails, double-cliquez sur la stratégie que vous souhaitez configurer.

3. Dans la stratégie **propriétés** boîte de dialogue, cliquez sur le **paramètres** onglet.

4. Dans la stratégie **propriétés**, dans **paramètres**, dans **attributs RADIUS**, vérifiez que **Standard** est sélectionné.

5. Dans le volet de détails, dans **attributs**, le **Service-Type** attribut est configuré avec une valeur par défaut **tramé**. Par défaut, pour les stratégies avec les méthodes d’accès de VPN et accès à distance, le **tramé-protocole** attribut est configuré avec une valeur de **PPP**. Pour spécifier les attributs de connexion supplémentaires requis pour les réseaux locaux virtuels, cliquez sur **ajouter**. Le **ajouter un attribut RADIUS Standard** boîte de dialogue s’ouvre.

6. Dans **ajouter un attribut RADIUS Standard**, attributs, faites défiler jusqu'à et ajoutez les attributs suivants :

    - **Type de support de tunnel**. Sélectionnez une valeur appropriée pour les sélections précédentes, que vous avez effectué de la stratégie. Par exemple, si vous configurez la stratégie de réseau est une stratégie sans fil, sélectionnez **valeur : 802 (inclut toutes les 802 media plus format canonique Ethernet)**.

    - **Tunnel-Pvt-Group-ID**. Entrez un entier qui représente le nombre de réseau local virtuel auquel les membres du groupe seront affectées. 

    - **Type de tunnel**. Sélectionnez **des réseaux locaux virtuels (VLAN)**.


7. Dans **ajouter un attribut RADIUS Standard**, cliquez sur **fermer**. 

8. Si votre serveur d’accès réseau (NAS) nécessite l’utilisation de la **Tunnel-Tag** d’attribut, procédez comme suit pour ajouter le **Tunnel-Tag** attribut à la stratégie de réseau. Si la documentation de votre NAS ne mentionne pas cet attribut, ne l’ajoutez pas à la stratégie. Si nécessaire, ajoutez les attributs comme suit :

    - Dans la stratégie **propriétés**, dans **paramètres**, dans **attributs RADIUS**, cliquez sur **fournisseur spécifique**. 

    - Dans le volet détails, cliquez sur **ajouter**. Le **ajouter un attribut spécifique fournisseur** boîte de dialogue s’ouvre.

    - Dans **attributs**, faites défiler jusqu'à et sélectionnez **Tunnel-Tag**, puis cliquez sur **ajouter**. Le **informations sur les attributs** boîte de dialogue s’ouvre. 

    - Dans **valeur d’attribut**, tapez la valeur que vous avez obtenue à partir de la documentation de votre matériel.

## <a name="configure-the-eap-payload-size"></a>Configurer la taille de charge utile EAP

Dans certains cas, les routeurs ou les pare-feux rejeter des paquets, car ils sont configurés pour les paquets qui exigent une fragmentation.

Lorsque vous déployez le serveur NPS avec les stratégies de réseau qui utilisent le protocole d’authentification Extensible \(EAP\) avec Transport Layer Security \(TLS\), ou EAP-TLS, comme méthode d’authentification, la valeur maximale par défaut unité de transmission \(MTU\) que NPS utilise pour les charges utiles EAP est 1 500 octets. 

Cette taille maximale de la charge utile EAP crée des messages RADIUS nécessitant la fragmentation par un routeur ou un pare-feu entre le serveur NPS et un client RADIUS. Si c’est le cas, un routeur ou un pare-feu placé entre le client RADIUS et le serveur NPS peut rejeter silencieusement certains fragments, ce qui entraîne l’échec d’authentification et l’incapacité du client d’accès pour se connecter au réseau.

La procédure suivante permet de réduire la taille maximale que NPS utilise pour les charges utiles EAP en ajustant l’attribut de taille MTU encadré dans une stratégie de réseau à une valeur non supérieure à 1344.

Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.

### <a name="to-configure-the-framed-mtu-attribute"></a>Pour configurer l’attribut de taille MTU tramé

1. Sur le serveur NPS, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **Network Policy Server**. La console NPS s’ouvre.
 
2. Double-cliquez sur **stratégies**, cliquez sur **stratégies réseau**, puis, dans le volet détails, double-cliquez sur la stratégie que vous souhaitez configurer.

3. Dans la stratégie **propriétés** boîte de dialogue, cliquez sur le **paramètres** onglet.

4. Dans **paramètres**, dans **attributs RADIUS**, cliquez sur **Standard**. Dans le volet détails, cliquez sur **ajouter**. Le **ajouter un attribut RADIUS Standard** boîte de dialogue s’ouvre.

5. Dans **attributs**, faites défiler jusqu'à, puis cliquez sur **tramé-MTU**, puis cliquez sur **ajouter**. Le **informations sur les attributs** boîte de dialogue s’ouvre.

6. Dans **valeur d’attribut**, tapez une valeur égale ou inférieure à **1344**. Cliquez sur **OK**, cliquez sur **fermer**, puis cliquez sur **OK**.



Pour plus d’informations sur les stratégies de réseau, consultez [stratégies réseau](nps-np-overview.md).

Pour obtenir des exemples de syntaxe des critères spéciaux pour spécifier les attributs de stratégie du réseau, consultez [utiliser des Expressions régulières dans NPS](nps-crp-reg-expressions.md).

Pour plus d’informations sur le serveur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).
