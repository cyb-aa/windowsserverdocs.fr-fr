---
title: Planifier un serveur NPS en tant que serveur RADIUS
description: Cette rubrique fournit des informations sur la planification du déploiement du serveur de stratégie réseau RADIUS dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 2900dd2c-0f70-4f8d-9650-ed83d51d509a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bbcf3338f2cd6d8662a84faf263b486e31b140e5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405341"
---
# <a name="plan-nps-as-a-radius-server"></a>Planifier un serveur NPS en tant que serveur RADIUS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Lorsque vous déployez un serveur de stratégie réseau \(\) NPS en tant que serveur protocole RADIUS (Remote Authentication Dial-In User Service) (RADIUS), NPS effectue l’authentification, l’autorisation et la gestion des comptes pour les demandes de connexion pour le domaine local et pour les domaines qui approuvent le domaine local. Vous pouvez utiliser ces instructions de planification pour simplifier votre déploiement RADIUS.

Ces instructions de planification n’incluent pas les circonstances dans lesquelles vous souhaitez déployer NPS en tant que proxy RADIUS. Lorsque vous déployez NPS en tant que proxy RADIUS, NPS transfère les demandes de connexion à un serveur exécutant NPS ou d’autres serveurs RADIUS dans des domaines distants, des domaines non approuvés, ou les deux. 

Avant de déployer NPS en tant que serveur RADIUS sur votre réseau, suivez les instructions ci-dessous pour planifier votre déploiement.

- Planifier la configuration du serveur NPS.

- Planifiez les clients RADIUS.

- Planifiez l’utilisation des méthodes d’authentification.

- Planifiez les stratégies réseau.

- Planifier la gestion des comptes NPS.

## <a name="plan-nps-configuration"></a>Planifier la configuration de NPS

Vous devez décider dans quel domaine le serveur NPS est membre. Pour les environnements à plusieurs domaines, un serveur NPS peut authentifier les informations d’identification des comptes d’utilisateur dans le domaine dont il est membre et pour tous les domaines qui approuvent le domaine local du serveur NPS. Pour autoriser le serveur NPS à lire les propriétés de numérotation des comptes d’utilisateur pendant le processus d’autorisation, vous devez ajouter le compte d’ordinateur du serveur NPS au groupe RAS et NPSs pour chaque domaine.

Une fois que vous avez déterminé l’appartenance au domaine du serveur NPS, le serveur doit être configuré pour communiquer avec les clients RADIUS, également appelés serveurs d’accès réseau, à l’aide du protocole RADIUS. En outre, vous pouvez configurer les types d’événements que le serveur NPS enregistre dans le journal des événements et vous pouvez entrer une description pour le serveur.

### <a name="key-steps"></a>Étapes clés

Pendant la planification de la configuration de NPS, vous pouvez suivre les étapes ci-dessous.

- Déterminez les ports RADIUS que le serveur NPS utilise pour recevoir des messages RADIUS des clients RADIUS. Les ports par défaut sont les ports UDP 1812 et 1645 pour les messages d’authentification RADIUS et les ports 1813 et 1646 pour les messages de gestion des comptes RADIUS.

- Si le serveur NPS est configuré avec plusieurs cartes réseau, déterminez les adaptateurs sur lesquels vous souhaitez autoriser le trafic RADIUS.

- Déterminez les types d’événements que vous souhaitez que le serveur NPS enregistre dans le journal des événements. Vous pouvez consigner les demandes d’authentification rejetées, les demandes d’authentification réussies ou les deux types de requêtes.

- Déterminez si vous déployez plus d’un serveur NPS. Pour fournir une tolérance de panne pour l’authentification et la gestion des comptes RADIUS, utilisez au moins deux NPSs. Un serveur NPS est utilisé en tant que serveur RADIUS principal et l’autre est utilisé comme sauvegarde. Chaque client RADIUS est ensuite configuré sur les deux NPSs. Si le serveur NPS principal devient indisponible, les clients RADIUS envoient des messages de demande d’accès à l’autre serveur NPS.

- Planifiez le script utilisé pour copier une configuration NPS vers d’autres NPSs afin de réduire la charge d’administration et d’empêcher le configuration incorrect d’un serveur. NPS fournit les commandes netsh qui vous permettent de copier tout ou partie d’une configuration NPS pour l’importer sur un autre serveur NPS. Vous pouvez exécuter les commandes manuellement à l’invite netsh. Toutefois, si vous enregistrez votre séquence de commandes en tant que script, vous pouvez exécuter le script à une date ultérieure si vous décidez de modifier les configurations de votre serveur.

## <a name="plan-radius-clients"></a>Planifier des clients RADIUS

Les clients RADIUS sont des serveurs d’accès réseau, tels que les points d’accès sans fil, les serveurs de réseau privé virtuel (VPN), les commutateurs 802.1 X et les serveurs d’accès à distance. Les proxys RADIUS, qui transfèrent les messages de demande de connexion aux serveurs RADIUS, sont également des clients RADIUS. NPS prend en charge tous les serveurs d’accès réseau et proxys RADIUS conformes au protocole RADIUS, comme décrit dans le document RFC 2865, « Remote Authentication Dial-in User Service (RADIUS) » et RFC 2866, « RADIUS Accounting ».

>[!IMPORTANT]
>Les clients d’accès, tels que les ordinateurs clients, ne sont pas des clients RADIUS. Seuls les serveurs d’accès réseau et les serveurs proxy qui prennent en charge le protocole RADIUS sont des clients RADIUS.

En outre, les commutateurs et les points d’accès sans fil doivent être en charge de l’authentification 802.1 X. Si vous souhaitez déployer le protocole EAP (Extensible Authentication Protocol) \(EAP\) \(ou PEAP\), les points d’accès et les commutateurs doivent prendre en charge l’utilisation du protocole EAP.

Pour tester l’interopérabilité de base pour les connexions PPP pour les points d’accès sans fil, configurez le point d’accès et le client d’accès pour utiliser le protocole PAP (Password Authentication Protocol). Utilisez des protocoles d’authentification PPP supplémentaires, tels que PEAP, jusqu’à ce que vous ayez testé ceux que vous envisagez d’utiliser pour l’accès au réseau.

### <a name="key-steps"></a>Étapes clés

Au cours de la planification des clients RADIUS, vous pouvez suivre les étapes ci-dessous.

- Documentez les attributs spécifiques au fournisseur (VSA) que vous devez configurer dans NPS. Si vos serveurs d’accès réseau requièrent VSA, consignez les informations VSA pour une utilisation ultérieure lorsque vous configurez vos stratégies réseau dans NPS. 

- Documentez les adresses IP des clients RADIUS et votre NPS pour simplifier la configuration de tous les appareils. Lorsque vous déployez vos clients RADIUS, vous devez les configurer pour qu’ils utilisent le protocole RADIUS, avec l’adresse IP du serveur NPS entrée en tant que serveur d’authentification. Et lorsque vous configurez NPS pour communiquer avec vos clients RADIUS, vous devez entrer les adresses IP du client RADIUS dans le composant logiciel enfichable NPS.

- Créer des secrets partagés pour la configuration sur les clients RADIUS et dans le composant logiciel enfichable NPS. Vous devez configurer les clients RADIUS avec un secret partagé, ou un mot de passe, que vous allez également entrer dans le composant logiciel enfichable NPS lors de la configuration des clients RADIUS dans NPS.

## <a name="plan-the-use-of-authentication-methods"></a>Planifier l’utilisation des méthodes d’authentification

NPS prend en charge les méthodes d’authentification par mot de passe et par certificat. Toutefois, tous les serveurs d’accès réseau ne prennent pas en charge les mêmes méthodes d’authentification. Dans certains cas, vous souhaiterez peut-être déployer une méthode d’authentification différente en fonction du type d’accès réseau.

Par exemple, vous souhaiterez peut-être déployer un accès sans fil et VPN pour votre organisation, mais utilisez une méthode d’authentification différente pour chaque type d’accès : EAP-TLS pour les connexions VPN, en raison de la sécurité renforcée d’EAP avec EAP-TLS (Transport Layer Security). fournit et PEAP-MS-CHAP v2 pour les connexions sans fil 802.1 X.

PEAP-MS-CHAP v2, PEAP avec Microsoft Challenge Handshake Authentication Protocol version 2 (PEAP-MS-CHAP v2) fournit une fonctionnalité appelée reconnexion rapide, conçue spécifiquement pour une utilisation avec des ordinateurs portables et d’autres périphériques sans fil. La reconnexion rapide permet aux clients sans fil de se déplacer entre des points d’accès sans fil sur le même réseau sans avoir à s’authentifier à chaque fois qu’ils sont associés à un nouveau point d’accès. Cela offre une meilleure expérience pour les utilisateurs sans fil et leur permet de se déplacer entre les points d’accès sans avoir à retaper leurs informations d’identification.
En raison de la reconnexion rapide et de la sécurité fournie par PEAP-MS-CHAP v2, PEAP-MS-CHAP v2 est un choix logique comme méthode d’authentification pour les connexions sans fil.

Pour les connexions VPN, EAP-TLS est une méthode d’authentification basée sur les certificats qui offre une sécurité renforcée qui protège le trafic réseau, même lorsqu’il est transmis sur Internet depuis des ordinateurs personnels ou mobiles vers les serveurs VPN de votre organisation.

### <a name="certificate-based-authentication-methods"></a>Méthodes d’authentification par certificat

Les méthodes d’authentification basées sur les certificats présentent l’avantage de fournir une sécurité renforcée. et ils ont l’inconvénient d’être plus difficile à déployer que les méthodes d’authentification par mot de passe.

PEAP-MS-CHAP v2 et EAP-TLS sont des méthodes d’authentification basées sur des certificats, mais il existe de nombreuses différences entre elles et la façon dont elles sont déployées.

### <a name="eap-tls"></a>EAP-TLS

EAP-TLS utilise des certificats pour l’authentification du client et du serveur, et exige que vous déployiez une infrastructure à clé publique (PKI) dans votre organisation. Le déploiement d’une infrastructure à clé publique peut être complexe et nécessite une phase de planification indépendante de la planification de l’utilisation de NPS en tant que serveur RADIUS.

Avec EAP-TLS, le serveur NPS inscrit un certificat de serveur auprès d’une autorité de certification \(\)de l’autorité de certification, et le certificat est enregistré sur l’ordinateur local dans le magasin de certificats. Pendant le processus d’authentification, l’authentification serveur se produit lorsque le serveur NPS envoie son certificat de serveur au client d’accès pour prouver son identité au client d’accès. Le client d’accès examine diverses propriétés de certificat pour déterminer si le certificat est valide et convient à une utilisation lors de l’authentification du serveur. Si le certificat de serveur répond à la configuration minimale requise pour les certificats de serveur et s’il est émis par une autorité de certification approuvée par le client d’accès, le serveur NPS est correctement authentifié par le client.

De même, l’authentification du client se produit pendant le processus d’authentification lorsque le client envoie son certificat client au serveur NPS pour prouver son identité au serveur NPS. Le serveur NPS examine le certificat et, si le certificat client répond à la configuration minimale requise pour les certificats clients et est émis par une autorité de certification approuvée par le serveur NPS, le client d’accès est correctement authentifié par le serveur NPS.

Bien qu’il soit nécessaire que le certificat de serveur soit stocké dans le magasin de certificats sur le serveur NPS, le certificat du client ou de l’utilisateur peut être stocké dans le magasin de certificats sur le client ou sur une carte à puce.

Pour que ce processus d’authentification aboutisse, tous les ordinateurs doivent disposer du certificat d’autorité de certification de votre organisation dans le magasin de certificats des autorités de certification racines de confiance pour l’ordinateur local et l’utilisateur actuel.

### <a name="peap-ms-chap-v2"></a>PEAP-MS-CHAP v2

PEAP-MS-CHAP v2 utilise un certificat pour l’authentification du serveur et les informations d’identification basées sur le mot de passe pour l’authentification de l’utilisateur. Étant donné que les certificats sont utilisés uniquement pour l’authentification du serveur, vous n’êtes pas obligé de déployer une infrastructure à clé publique pour pouvoir utiliser PEAP-MS-CHAP v2. Lorsque vous déployez PEAP-MS-CHAP v2, vous pouvez obtenir un certificat de serveur pour le serveur NPS de l’une des deux manières suivantes :

- Vous pouvez installer les services de certificats Active Directory (AD CS), puis inscrire automatiquement les certificats sur NPSs. Si vous utilisez cette méthode, vous devez également inscrire le certificat d’autorité de certification auprès des ordinateurs clients qui se connectent à votre réseau afin qu’ils fassent confiance au certificat émis par le serveur NPS.

- Vous pouvez acheter un certificat de serveur auprès d’une autorité de certification publique, telle que VeriSign. Si vous utilisez cette méthode, assurez-vous de sélectionner une autorité de certification déjà approuvée par les ordinateurs clients. Pour déterminer si les ordinateurs clients approuvent une autorité de certification, ouvrez le composant logiciel enfichable MMC (Microsoft Management Console) certificats sur un ordinateur client, puis consultez le magasin autorités de certification racines de confiance pour l’ordinateur local et pour l’utilisateur actuel. S’il existe un certificat de l’autorité de certification dans ces magasins de certificats, l’ordinateur client approuve l’autorité de certification et approuve donc tout certificat émis par l’autorité de certification.

Pendant le processus d’authentification avec PEAP-MS-CHAP v2, l’authentification serveur se produit lorsque le serveur NPS envoie son certificat de serveur à l’ordinateur client. Le client d’accès examine diverses propriétés de certificat pour déterminer si le certificat est valide et convient à une utilisation lors de l’authentification du serveur. Si le certificat de serveur répond à la configuration minimale requise pour les certificats de serveur et s’il est émis par une autorité de certification approuvée par le client d’accès, le serveur NPS est correctement authentifié par le client.

L’authentification de l’utilisateur se produit lorsqu’un utilisateur qui tente de se connecter au réseau tape des informations d’identification basées sur un mot de passe et tente de se connecter. NPS reçoit les informations d’identification et effectue l’authentification et l’autorisation. Si l’utilisateur est authentifié et autorisé correctement et si l’ordinateur client a correctement authentifié le serveur NPS, la demande de connexion est accordée.

### <a name="key-steps"></a>Étapes clés

Lors de la planification de l’utilisation des méthodes d’authentification, vous pouvez utiliser les étapes suivantes.

- Identifiez les types d’accès réseau que vous prévoyez d’offrir, tels que les connexions sans fil, VPN, les commutateurs 802.1 X et l’accès à distance.

- Déterminez la ou les méthodes d’authentification que vous souhaitez utiliser pour chaque type d’accès. Il est recommandé d’utiliser les méthodes d’authentification basées sur les certificats qui assurent une sécurité renforcée. Toutefois, il peut ne pas être pratique de déployer une infrastructure à clé publique, de sorte que d’autres méthodes d’authentification peuvent offrir un meilleur équilibre entre ce dont vous avez besoin pour votre réseau.

- Si vous déployez EAP-TLS, planifiez le déploiement de votre infrastructure à clé publique. Cela comprend la planification des modèles de certificat que vous allez utiliser pour les certificats de serveur et les certificats d’ordinateur client. Elle explique également comment inscrire des certificats pour les ordinateurs membres du domaine et non membres du domaine et déterminer si vous souhaitez utiliser des cartes à puce.

- Si vous déployez PEAP-MS-CHAP v2, déterminez si vous voulez installer les services AD CS pour émettre des certificats de serveur vers votre NPSs ou si vous souhaitez acheter des certificats de serveur auprès d’une autorité de certification publique, telle que VeriSign.

### <a name="plan-network-policies"></a>Planifier les stratégies réseau

Les stratégies réseau sont utilisées par le serveur NPS pour déterminer si les demandes de connexion reçues des clients RADIUS sont autorisées. Le serveur NPS utilise également les propriétés de numérotation du compte d’utilisateur pour effectuer une détermination d’autorisation.

Étant donné que les stratégies réseau sont traitées dans l’ordre dans lequel elles apparaissent dans le composant logiciel enfichable NPS, envisagez de placer les stratégies les plus restrictives en premier dans la liste des stratégies. Pour chaque demande de connexion, NPS tente de faire correspondre les conditions de la stratégie avec les propriétés de la demande de connexion. NPS examine chaque stratégie réseau dans l’ordre jusqu’à ce qu’elle trouve une correspondance. S’il ne trouve pas de correspondance, la demande de connexion est rejetée.

### <a name="key-steps"></a>Étapes clés

Lors de la planification des stratégies réseau, vous pouvez suivre les étapes ci-dessous.

- Déterminer l’ordre de traitement NPS préféré des stratégies réseau, du plus restrictif au moins restrictif.

- Déterminez l’état de la stratégie. L’état de la stratégie peut avoir la valeur activé ou désactivé. Si la stratégie est activée, le serveur NPS évalue la stratégie lors de l’exécution de l’autorisation. Si la stratégie n’est pas activée, elle n’est pas évaluée.

- Déterminez le type de stratégie. Vous devez déterminer si la stratégie est conçue pour accorder l’accès lorsque les conditions de la stratégie sont mises en correspondance par la demande de connexion ou si la stratégie est conçue pour refuser l’accès lorsque les conditions de la stratégie sont mises en correspondance par la demande de connexion. Par exemple, si vous souhaitez refuser explicitement l’accès sans fil aux membres d’un groupe Windows, vous pouvez créer une stratégie réseau qui spécifie le groupe, la méthode de connexion sans fil et dont le paramètre de type de stratégie est refuser l’accès.

- Déterminez si vous souhaitez que NPS ignore les propriétés de numérotation des comptes d’utilisateur qui sont membres du groupe sur lequel la stratégie est basée. Lorsque ce paramètre n’est pas activé, les propriétés de numérotation des comptes d’utilisateurs remplacent les paramètres configurés dans les stratégies réseau. Par exemple, si une stratégie réseau autorise l’accès à un utilisateur, mais que les propriétés de connexion du compte d’utilisateur pour cet utilisateur sont définies sur refuser l’accès, l’accès est refusé à l’utilisateur. Toutefois, si vous activez le paramètre de type de stratégie ignorer les propriétés d’accès à distance du compte d’utilisateur, l’accès au réseau est accordé au même utilisateur.

- Déterminez si la stratégie utilise le paramètre source de la stratégie. Ce paramètre vous permet de spécifier facilement une source pour toutes les demandes d’accès. Les sources possibles sont une passerelle des services Terminal Server (passerelle TS), un serveur d’accès à distance (VPN ou d’accès à distance), un serveur DHCP, un point d’accès sans fil et un serveur d’autorité d’inscription d’intégrité. Vous pouvez également spécifier une source spécifique au fournisseur.

- Déterminez les conditions qui doivent être mises en correspondance pour que la stratégie réseau soit appliquée.

- Déterminez les paramètres qui sont appliqués si les conditions de la stratégie réseau sont mises en correspondance avec la demande de connexion.

- Déterminez si vous souhaitez utiliser, modifier ou supprimer les stratégies réseau par défaut.

## <a name="plan-nps-accounting"></a>Planifier la gestion des comptes NPS

NPS offre la possibilité de consigner les données de gestion des comptes RADIUS, telles que les demandes d’authentification des utilisateurs et de gestion des comptes, dans trois formats : le format IAS, le format compatible avec la base de données et la journalisation Microsoft SQL Server. 

Le format IAS et le format compatible avec la base de données créent des fichiers journaux sur le serveur NPS local au format de fichier texte. 

SQL Server la journalisation permet de se connecter à une base de données SQL Server 2000 ou SQL Server conforme à 2005, en étendant la gestion de comptes RADIUS pour tirer parti des avantages de la journalisation dans une base de données relationnelle.

### <a name="key-steps"></a>Étapes clés

Pendant la planification de la gestion des comptes NPS, vous pouvez utiliser les étapes suivantes.

- Déterminez si vous souhaitez stocker les données de gestion des comptes NPS dans des fichiers journaux ou dans une base de données SQL Server.

### <a name="nps-accounting-using-local-log-files"></a>Gestion des comptes NPS à l’aide de fichiers journaux locaux

L’enregistrement des demandes d’authentification des utilisateurs et de gestion des comptes dans les fichiers journaux est principalement utilisé pour l’analyse de la connexion et la facturation. il est également utile en tant qu’outil d’investigation de sécurité, vous offrant ainsi une méthode de suivi de l’activité d’un utilisateur malveillant après une malveillant.

### <a name="key-steps"></a>Étapes clés

Pendant la planification de la gestion des comptes NPS à l’aide des fichiers journaux locaux, vous pouvez utiliser les étapes suivantes.

- Déterminez le format de fichier texte que vous souhaitez utiliser pour vos fichiers journaux NPS.

- Choisissez le type d’informations que vous souhaitez consigner. Vous pouvez consigner les demandes de gestion des comptes, les demandes d’authentification et l’état périodique.

- Déterminez l’emplacement du disque dur où vous souhaitez stocker vos fichiers journaux.

- Concevez votre solution de sauvegarde de fichiers journaux. L’emplacement du disque dur où vous stockez vos fichiers journaux doit être un emplacement qui vous permet de sauvegarder facilement vos données. En outre, l’emplacement du disque dur doit être protégé en configurant la liste de contrôle d’accès (ACL) pour le dossier dans lequel les fichiers journaux sont stockés.

- Déterminez la fréquence à laquelle vous souhaitez créer les nouveaux fichiers journaux. Si vous souhaitez créer des fichiers journaux en fonction de la taille du fichier, déterminez la taille de fichier maximale autorisée avant la création d’un nouveau fichier journal par NPS.

- Déterminez si vous souhaitez que NPS supprime les anciens fichiers journaux si l’espace de stockage est insuffisant sur le disque dur.

- Déterminez l’application ou les applications que vous souhaitez utiliser pour afficher les données de comptabilité et produire des rapports.

### <a name="nps-sql-server-logging"></a>Journalisation des SQL Server NPS

La journalisation des SQL Server NPS est utilisée lorsque vous avez besoin d’informations d’état de session, de création de rapports et d’analyse des données, ainsi que de centralisation et de simplification de la gestion de vos données de comptabilité.

NPS offre la possibilité d’utiliser la journalisation SQL Server pour enregistrer les demandes d’authentification des utilisateurs et de gestion des comptes reçues à partir d’un ou plusieurs serveurs d’accès réseau à une source de données sur un ordinateur exécutant le moteur d’Microsoft SQL Server Desktop \(MSDE 2000\), ou toute version de SQL Server ultérieure à SQL Server 2000.

Les données de gestion des comptes sont transmises de NPS au format XML à une procédure stockée dans la base de données, qui prend en charge le langage de requête structuré \(SQL\) et XML \(SQLXML\). L’enregistrement des demandes d’authentification des utilisateurs et de gestion des comptes dans une base de données SQL Server conforme à XML permet à plusieurs NPSs d’avoir une seule source de données.

### <a name="key-steps"></a>Étapes clés

Pendant la planification de la gestion des comptes NPS à l’aide de la journalisation SQL Server NPS, vous pouvez utiliser les étapes suivantes.

- Déterminez si vous ou un autre membre de votre organisation dispose d’SQL Server expérience de développement de base de données relationnelle 2000 ou SQL Server 2005 et que vous comprenez comment utiliser ces produits pour créer, modifier, administrer et gérer des bases de données SQL Server.

- Déterminez si SQL Server est installé sur le serveur NPS ou sur un ordinateur distant.

- Concevez la procédure stockée que vous allez utiliser dans votre base de données SQL Server pour traiter les fichiers XML entrants qui contiennent des données de gestion des comptes NPS.

- Concevoir la structure et le Flow de réplication de la base de données SQL Server.

- Déterminez l’application ou les applications que vous souhaitez utiliser pour afficher les données de comptabilité et produire des rapports.

- Prévoyez d’utiliser les serveurs d’accès réseau qui envoient l’attribut de classe dans toutes les demandes de gestion des comptes. L’attribut de classe est envoyé au client RADIUS dans un message d’acceptation d’accès et est utile pour mettre en corrélation les messages de demande de comptes avec les sessions d’authentification. Si l’attribut de classe est envoyé par le serveur d’accès réseau dans les messages de demande de comptabilité, il peut être utilisé pour faire correspondre les enregistrements de gestion des comptes et d’authentification. La combinaison des attributs unique-Serial-Number, service-reboot-Time et Server-Address doit être une identification unique pour chaque authentification acceptée par le serveur.

- Prévoyez d’utiliser les serveurs d’accès réseau qui prennent en charge la gestion de comptes intermédiaires.

- Envisagez d’utiliser des serveurs d’accès réseau qui envoient des messages de comptabilité et de gestion des comptes.

- Prévoyez d’utiliser les serveurs d’accès réseau qui prennent en charge le stockage et le transfert des données de gestion. Les serveurs d’accès réseau qui prennent en charge cette fonctionnalité peuvent stocker des données de gestion des comptes lorsque le serveur d’accès réseau ne peut pas communiquer avec le serveur NPS. Lorsque le serveur NPS est disponible, le serveur d’accès réseau transfère les enregistrements stockés au serveur NPS, ce qui améliore la fiabilité de la gestion des comptes sur les serveurs d’accès réseau qui ne fournissent pas cette fonctionnalité.

- Planifiez de toujours configurer l’attribut acct-interim-interval dans les stratégies réseau. L’attribut acct-interim-interval définit l’intervalle (en secondes) entre chaque mise à jour intermédiaire envoyée par le serveur d’accès réseau. Conformément à la norme RFC 2869, la valeur de l’attribut acct-interim-interval ne doit pas être inférieure à 60 secondes, ou une minute, et ne doit pas être inférieure à 600 secondes, ou 10 minutes. Pour plus d’informations, consultez RFC 2869, « extensions RADIUS ».

- Vérifiez que la journalisation de l’état périodique est activée sur votre NPSs.

