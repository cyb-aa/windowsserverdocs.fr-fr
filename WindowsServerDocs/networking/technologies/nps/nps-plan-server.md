---
title: Planifier le serveur NPS comme serveur RADIUS
description: Cette rubrique fournit des informations sur le déploiement de serveur RADIUS de serveur de stratégie réseau planification dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2900dd2c-0f70-4f8d-9650-ed83d51d509a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e9bb95428c17e5d7bde588693e84288ddfe64531
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="plan-nps-as-a-radius-server"></a>Planifier le serveur NPS comme serveur RADIUS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Lorsque vous déployez \(NPS\) serveur NPS comme serveur Authentication Dial-In utilisateur RADIUS (Remote Service), NPS effectue l’authentification, l’autorisation et gestion des comptes pour les demandes de connexion pour le domaine local et pour les domaines qui approuvent le domaine local. Vous pouvez utiliser ces recommandations en matière de planification pour simplifier le déploiement de RADIUS.

Ces recommandations en matière de planification n’incluent pas les circonstances dans lesquelles vous souhaitez déployer NPS en tant que proxy RADIUS. Lorsque vous déployez NPS en tant que proxy RADIUS, NPS transfère les demandes de connexion à un serveur exécutant NPS ou autres serveurs RADIUS dans des domaines à distance, des domaines non approuvés ou les deux. 

Avant de déployer NPS comme serveur RADIUS sur votre réseau, suivez les indications ci-dessous pour planifier votre déploiement.

- Planifier la configuration du serveur NPS.

- Planifier des clients RADIUS.

- Planifier l’utilisation de méthodes d’authentification.

- Planifier les stratégies de réseau.

- Planifier la gestion des comptes NPS.

## <a name="plan-nps-server-configuration"></a>Planifier la configuration du serveur NPS

Vous devez décider de domaine dans lequel le serveur NPS est membre. Pour les environnements de plusieurs domaines, un serveur NPS peut authentifier les informations d’identification pour les comptes d’utilisateur dans le domaine dont il est membre et pour tous les domaines qui approuvent le domaine local du serveur NPS. Pour autoriser le serveur NPS lire les propriétés de numérotation des comptes d’utilisateur au cours du processus d’autorisation, vous devez ajouter le compte d’ordinateur du serveur NPS pour le groupe de serveurs RAS et NPS pour chaque domaine.

Après avoir déterminé l’appartenance au domaine du serveur NPS, le serveur doit être configuré pour communiquer avec les clients RADIUS, également appelés serveurs d’accès réseau, en utilisant le protocole RADIUS. Vous pouvez en outre, configurer aux types d’événements qui enregistre NPS dans le journal et vous pouvez entrer une description pour le serveur des événements.

### <a name="key-steps"></a>Décrit les principales étapes

Lors de la planification de la configuration du serveur NPS, vous pouvez utiliser les étapes suivantes.

- Déterminer les ports RADIUS du serveur NPS utilise pour recevoir les messages RADIUS à partir de clients RADIUS. Les ports par défaut sont les ports UDP 1812 et 1645 pour les messages d’authentification RADIUS et les ports 1813 et 1646 pour les messages de gestion de comptes RADIUS.

- Si le serveur NPS est configuré avec plusieurs cartes réseau, déterminez les cartes sur laquelle vous souhaitez que le trafic RADIUS à autoriser.

- Déterminer les types d’événements que vous souhaitez enregistrer dans le journal des événements NPS. Vous pouvez vous connecter les demandes d’authentification rejetés, les demandes d’authentification réussie ou les deux types de requêtes.

- Déterminer si vous déployez plusieurs serveurs NPS. Pour fournir une tolérance de panne pour l’authentification RADIUS et la gestion des comptes, utilisez au moins deux serveurs NPS. Un serveur NPS est utilisé en tant que serveur RADIUS principal et l’autre est utilisé en tant que sauvegarde. Chaque client RADIUS est ensuite configurée sur les deux serveurs NPS. Si le serveur NPS principal devient indisponible, les clients RADIUS envoient des messages de demande d’accès à l’autre serveur NPS.

- Planifier le script utilisé pour copier une configuration du serveur NPS à d’autres serveurs NPS pour enregistrer sur la charge administrative et éviter la cofiguration incorrecte d’un serveur. NPS fournit les commandes Netsh que vous permettent de copier tout ou partie d’une configuration de serveur NPS pour les importer sur un autre serveur NPS. Vous pouvez exécuter les commandes manuellement à l’invite de commandes Netsh. Toutefois, si vous enregistrez votre séquence de commandes en tant que script, vous pouvez exécuter le script à une date ultérieure si vous décidez de modifier vos configurations de serveur.

## <a name="plan-radius-clients"></a>Planifier des clients RADIUS

Clients RADIUS sont des serveurs d’accès réseau, tels que les points d’accès sans fil, serveurs de réseau privé virtuel (VPN), 802.1 commutateurs compatibles X et les serveurs d’accès à distance. Proxy RADIUS, ce qui permet de transférer des messages demande aux serveurs RADIUS de connexion, est également des clients RADIUS. NPS prend en charge tous les serveurs d’accès réseau et des proxys RADIUS qui respectent le rayon de protocole comme décrit dans la RFC 2865, «Remote Authentication Dial-in User Service (RADIUS),» et RFC 2866, «Gestion de comptes RADIUS».

>[!IMPORTANT]
>Les clients d’accès, tels que les ordinateurs clients, ne sont pas des clients RADIUS. Seuls les serveurs d’accès réseau et les serveurs proxy qui prennent en charge le protocole RADIUS sont des clients RADIUS.

En outre, les points d’accès sans fil et commutateurs doivent être capables d’authentification de 802. 1 X. Si vous souhaitez déployer Extensible Authentication Protocol \(EAP\) ou Protected Extensible Authentication Protocol \(PEAP\), les commutateurs et les points d’accès doivent prendre en charge l’utilisation du protocole EAP.

Pour tester une interopérabilité de base pour les connexions PPP pour les points d’accès sans fil, configurez le point d’accès et le client d’accès à utiliser le protocole PAP (Password Authentication Protocol). Utilisez des protocoles d’authentification PPP supplémentaires, telles que PEAP, jusqu'à ce que vous avez testé ceux que vous prévoyez d’utiliser pour l’accès réseau.

### <a name="key-steps"></a>Décrit les principales étapes

Lors de la planification pour les clients RADIUS, vous pouvez utiliser les étapes suivantes.

- Document spécifique au fournisseur attributs au que vous devez configurer dans NPS. Si vos serveurs d’accès réseau nécessitent au, ouvrez une session les informations VSA pour une utilisation ultérieure lorsque vous configurez vos stratégies réseau dans NPS. 

- Les adresses IP de clients RADIUS et votre serveur NPS pour simplifier la configuration de tous les périphériques de numérisation de document. Lorsque vous déployez vos clients RADIUS, vous devez les configurer pour utiliser le protocole RADIUS, avec l’adresse IP du serveur NPS entré en tant que le serveur d’authentification. Et lorsque vous configurez le serveur NPS pour communiquer avec vos clients RADIUS, vous devez entrer les adresses IP du client RADIUS dans le composant logiciel enfichable NPS.

- Créer des secrets partagés pour la configuration sur les clients RADIUS et dans le composant logiciel enfichable serveur NPS. Vous devez configurer les clients RADIUS avec un secret partagé, ou un mot de passe, vous devrez également entrer dans le composant logiciel enfichable NPS lors de la configuration de clients RADIUS dans NPS.

## <a name="plan-the-use-of-authentication-methods"></a>Planifier l’utilisation de méthodes d’authentification

NPS prend en charge les deux méthodes d’authentification par mot de passe et les certificats. Toutefois, pas tous les serveurs d’accès réseau prend en charge les mêmes méthodes d’authentification. Dans certains cas, vous souhaiterez déployer une autre méthode d’authentification en fonction du type d’accès réseau.

Par exemple, vous souhaiterez déployer sans fil et l’accès VPN pour votre organisation, mais utiliser une autre méthode d’authentification pour chaque type d’accès: EAP-TLS pour les connexions VPN, en raison de la sécurité renforcée EAP avec Transport Layer Security (EAP-TLS) fournit et PEAP-MS-CHAP v2 pour 802. 1 X sans fil.

La reconnexion spécifiquement conçu pour une utilisation avec les ordinateurs portables et autres appareils sans fil PEAP avec Microsoft Challenge Handshake Authentication Protocol version 2 (PEAP-MS-CHAP v2) fournit une fonctionnalité appelée rapide. Reconnexion rapide permet aux déplacer entre les points d’accès sans fil sur le même réseau sans être authentifiés à chaque fois qu’ils associent à un nouveau point d’accès clients sans fil. Cela offre une meilleure expérience pour les utilisateurs sans fil et leur permet de passer des points d’accès sans avoir à retaper leurs informations d’identification.
En raison de fast reconnexion et la sécurité PEAP-MS-CHAP v2, PEAP-MS-CHAP v2 est un choix logique comme méthode d’authentification pour les connexions sans fil.

Pour les connexions VPN, EAP-TLS est une méthode d’authentification par certificat qui offre une sécurité forte qui protège le trafic réseau, même si elles sont transmises sur Internet à partir d’ordinateurs personnels ou mobiles pour les serveurs VPN de votre organisation.

### <a name="certificate-based-authentication-methods"></a>Méthodes d’authentification par certificat

Méthodes d’authentification par certificat présentent l’avantage de fournir une sécurité forte; et qu’ils ont l’inconvénient d’être plus difficile à déployer que des méthodes d’authentification par mot de passe.

PEAP-MS-CHAP v2 et EAP-TLS sont des méthodes d’authentification par certificat, mais il existe de nombreuses différences entre eux et la manière dans lequel ils sont déployés.

### <a name="eap-tls"></a>EAP-TLS

EAP-TLS utilise des certificats pour l’authentification client et serveur et nécessite que vous déployez une infrastructure à clé publique (PKI) dans votre organisation. Déploiement d’une infrastructure à clé publique peut être complexe et requiert une phase de planification est indépendante de la planification de l’utilisation du serveur NPS comme serveur RADIUS.

Avec EAP-TLS, le serveur NPS inscrit un certificat de serveur à partir d’une autorité de certification \(CA\) et le certificat est enregistré sur l’ordinateur local dans le magasin de certificats. Pendant le processus d’authentification, l’authentification du serveur se produit lorsque le serveur NPS envoie son certificat de serveur pour le client d’accès pour prouver son identité au client d’accès. Le client d’accès examine les diverses propriétés de certificat pour déterminer si le certificat est valide et qu’il est approprié à utiliser lors de l’authentification du serveur. Si le certificat de serveur répond aux exigences de certificat de serveur minimale et émis par une autorité de certification qui l’approuve le client d’accès, le serveur NPS est correctement authentifié par le client.

De même, l’authentification du client se produit pendant le processus d’authentification lorsque le client envoie son certificat de client sur le serveur NPS pour prouver son identité sur le serveur NPS. Le serveur NPS examine le certificat, et si le certificat client répond aux exigences de certificat client minimale et émis par une autorité de certification qui l’approuve le serveur NPS, le client d’accès est correctement authentifié par le serveur NPS.

Même s’il est nécessaire que le certificat de serveur est stocké dans le magasin de certificats sur le serveur NPS, le certificat client ou un utilisateur peut être stocké soit du magasin de certificats sur le client ou sur une carte à puce.

Pour ce processus d’authentification réussisse, il est nécessaire que tous les ordinateurs ont le certificat d’autorité de certification de votre organisation dans le magasin de certificats Autorités de Certification racine de confiance pour l’ordinateur Local et l’utilisateur actuel.

### <a name="peap-ms-chap-v2"></a>PEAP-MS-CHAP v2

PEAP-MS-CHAP v2 utilise un certificat pour l’authentification du serveur et les informations d’identification de mot de passe pour l’authentification utilisateur. Étant donné que les certificats sont utilisés uniquement pour l’authentification du serveur, vous ne sont pas requises pour déployer une infrastructure à clé publique pour pouvoir utiliser le protocole PEAP-MS-CHAP v2. Lorsque vous déployez PEAP-MS-CHAP v2, vous pouvez obtenir un certificat de serveur pour le serveur NPS dans un des deux manières suivantes:

- Vous pouvez installer les Services de certificats Active Directory (AD CS), puis inscrire automatiquement les certificats pour les serveurs NPS. Si vous utilisez cette méthode, vous devez également inscrire le certificat d’autorité de certification pour les ordinateurs clients se connectant à votre réseau afin qu’ils approuvent le certificat émis sur le serveur NPS.

- Vous pouvez acheter un certificat de serveur à partir d’une autorité de certification publique telle que VeriSign. Si vous utilisez cette méthode, assurez-vous que vous sélectionnez une autorité de certification qui est déjà approuvée par les ordinateurs clients. Pour déterminer si les ordinateurs clients font confiance à une autorité de certification, ouvrez le composant logiciel enfichable Microsoft Management Console (MMC) de certificats sur un ordinateur client et affichez le magasin d’autorités de Certification racine de confiance pour l’ordinateur Local et pour l’utilisateur actuel. S’il existe un certificat à partir de l’autorité de certification dans ces magasins de certificats, l’ordinateur client approuve l’autorité de certification et par conséquent font confiance à n’importe quel certificat émis par l’autorité de certification.

Pendant le processus d’authentification avec PEAP-MS-CHAP v2, l’authentification du serveur se produit lorsque le serveur NPS envoie son certificat de serveur à l’ordinateur client. Le client d’accès examine les diverses propriétés de certificat pour déterminer si le certificat est valide et qu’il est approprié à utiliser lors de l’authentification du serveur. Si le certificat de serveur répond aux exigences de certificat de serveur minimale et émis par une autorité de certification qui l’approuve le client d’accès, le serveur NPS est correctement authentifié par le client.

L’authentification des utilisateurs se produit lorsqu’un utilisateur tente de se connecter le réseau types basés sur un mot de passe des informations d’identification et essaie de se connecter. NPS reçoit les informations d’identification et effectue l’authentification et l’autorisation. Si l’utilisateur est authentifié et autorisé correctement, et si l’ordinateur client s’est authentifié correctement le serveur NPS, la demande de connexion est accordée.

### <a name="key-steps"></a>Décrit les principales étapes

Lors de la planification pour l’utilisation de méthodes d’authentification, vous pouvez utiliser les étapes suivantes.

- Identifier les types d’accès réseau que vous projetez d’offrir, par exemple, sans fil, VPN, 802.1 X commutateur et l’accès à distance.

- Déterminer la méthode d’authentification ou les méthodes que vous souhaitez utiliser pour chaque type d’accès. Il est recommandé d’utiliser les méthodes d’authentification par certificat qui fournissent une sécurité renforcée; Toutefois, il peut être pas vous permet de déployer une infrastructure à clé publique, les autres méthodes d’authentification peuvent fournir un meilleur équilibre de ce que vous avez besoin pour votre réseau.

- Si vous déployez EAP-TLS, planifiez votre déploiement d’infrastructure à clé publique. Cela inclut la planification les modèles de certificat que vous vous apprêtez à utiliser pour les certificats de serveur et les certificats d’ordinateur client. Il inclut également de déterminer comment inscrire des certificats membre de domaine et les ordinateurs membres du domaine, et de déterminer si vous souhaitez utiliser des cartes à puce.

- Si vous déployez PEAP-MS-CHAP v2, déterminez si vous souhaitez installer les services AD CS pour émettre des certificats de serveur à vos serveurs NPS ou si vous souhaitez acheter des certificats de serveur à partir d’une autorité de certification publique, telle que VeriSign.

### <a name="plan-network-policies"></a>Planifier les stratégies de réseau

Stratégies de réseau sont utilisées par NPS pour déterminer si les demandes de connexion des clients RADIUS sont autorisés. NPS utilise également les propriétés de numérotation du compte d’utilisateur pour procéder à une détermination d’autorisation.

Étant donné que les stratégies de réseau sont traités dans l’ordre dans lequel elles apparaissent dans le composant logiciel enfichable NPS, envisagez de placer vos stratégies plus restrictifs en premier dans la liste des stratégies. Pour chaque demande de connexion, NPS tente de faire correspondre les conditions de la stratégie avec les propriétés de demande de connexion. NPS examine chaque stratégie réseau dans l’ordre jusqu'à ce qu’il trouve une correspondance. Si elle ne trouve pas de correspondance, la demande de connexion est rejetée.

### <a name="key-steps"></a>Décrit les principales étapes

Lors de la planification pour les stratégies réseau, vous pouvez utiliser les étapes suivantes.

- Déterminer l’ordre de traitement du serveur NPS par défaut des stratégies de réseau, de la plus restrictive à la moins restrictive.

- Déterminer l’état de la stratégie. L’état de la stratégie peut avoir la valeur d’activé ou désactivé. Si la stratégie est activée, NPS évalue la stratégie lors de l’exécution d’autorisation. Si la stratégie n’est pas activée, il n’est pas évaluée.

- Déterminer le type de stratégie. Vous devez déterminer si la stratégie est conçue pour accorder l’accès lorsque les conditions de la stratégie sont conformes à la demande de connexion ou si la stratégie est conçue pour refuser l’accès lorsque les conditions de la stratégie sont conformes à la demande de connexion. Par exemple, si vous souhaitez refuser explicitement l’accès sans fil pour les membres d’un groupe Windows, vous pouvez créer une stratégie de réseau qui spécifie le groupe, la méthode de connexion sans fil et qui a une stratégie de type de paramètre de refuser l’accès.

- Déterminez si vous souhaitez que NPS pour ignorer les propriétés de numérotation des comptes d’utilisateur qui sont membres du groupe sur lequel la stratégie est basée. Lorsque ce paramètre n’est pas activé, les propriétés de numérotation des comptes d’utilisateur remplacent les paramètres qui sont configurés dans les stratégies de réseau. Par exemple, si une stratégie de réseau est configurée qui accorde l’accès à un utilisateur, mais les propriétés de numérotation du compte d’utilisateur pour l’utilisateur sont définies pour refuser l’accès, l’accès est refusé. Toutefois, si vous activez les propriétés de paramètre ignorer compte à distance de l’utilisateur de type de stratégie, le même utilisateur a accès au réseau.

- Déterminer si la stratégie utilise le paramètre de source de stratégie. Ce paramètre vous permet de spécifier facilement une source de toutes les demandes d’accès. Les sources possibles sont une passerelle des Services Terminal Server (passerelle TS), un serveur d’accès à distance (accès à distance ou VPN), un serveur DHCP, un point d’accès sans fil et un serveur de l’autorité HRA. Vous pouvez également spécifier une source spécifique au fournisseur.

- Déterminer les conditions qui doivent être remplis dans l’ordre de la stratégie réseau peut être appliqué.

- Déterminez les paramètres qui sont appliqués si les conditions de la stratégie réseau sont conformes à la demande de connexion.

- Déterminez si vous souhaitez utiliser, modifier ou supprimer les stratégies de réseau par défaut.

## <a name="plan-nps-accounting"></a>Planifier la gestion des comptes NPS

NPS permet d’enregistrer les données de gestion de RADIUS, tels que l’authentification des utilisateurs et les demandes de gestion, dans trois formats: format IAS, format compatible avec la base de données et la journalisation de Microsoft SQL Server. 

Format IAS et format compatible avec la base de données de créer les fichiers journaux sur le serveur NPS local au format de fichier texte. 

La journalisation SQL Server fournit la possibilité de se connecter à un SQL Server 2000 ou une base de données SQL Server 2005 XML compatibles, l’extension de gestion de comptes RADIUS pour exploiter les avantages de la journalisation pour une base de données relationnelle.

### <a name="key-steps"></a>Décrit les principales étapes

Lors de la planification pour la gestion des comptes NPS, vous pouvez utiliser les étapes suivantes.

- Déterminez si vous souhaitez stocker les données de gestion de serveur NPS dans les fichiers journaux ou dans une base de données SQL Server.

### <a name="nps-accounting-using-local-log-files"></a>Gestion des comptes NPS à l’aide des fichiers journaux locaux

Enregistrement d’authentification et de gestion des comptes demandes des utilisateurs dans les fichiers journaux est utilisée principalement pour des raisons de facturation et d’analyse de connexion et est également utiles comme un outil d’enquête de sécurité, vous fournissant ainsi une méthode pour le suivi de l’activité d’un utilisateur malveillant après une attaque.

### <a name="key-steps"></a>Décrit les principales étapes

Lors de la planification pour la gestion des comptes NPS à l’aide des fichiers journaux locaux, vous pouvez utiliser les étapes suivantes.

- Déterminer le format de fichier texte que vous souhaitez utiliser pour vos fichiers journaux de serveur NPS.

- Choisissez le type d’informations que vous souhaitez ouvrir une session. Vous pouvez vous connecter les demandes de gestion, les demandes d’authentification et l’état périodique.

- Déterminer l’emplacement du disque dur sur lequel vous voulez stocker vos fichiers journaux.

- Concevez votre solution de sauvegarde du fichier journal. L’emplacement du disque dur où vous stockez vos fichiers journaux doit être un emplacement qui vous permet de facilement sauvegarder vos données. En outre, l’emplacement du disque dur doit être protégé en configurant la liste de contrôle d’accès (ACL) pour le dossier où sont stockés les fichiers journaux.

- Déterminer la fréquence à laquelle vous souhaitez des fichiers journaux à créer. Si vous souhaitez que les fichiers journaux pour être créé en fonction de la taille du fichier, déterminer la taille maximale autorisée avant la création d’un nouveau fichier journal par le serveur NPS.

- Déterminez si vous souhaitez que NPS pour supprimer les anciens fichiers journaux si le disque dur manque d’espace de stockage.

- Déterminer l’ou les applications que vous souhaitez utiliser pour afficher les données de gestion et produire des rapports.

### <a name="nps-sql-server-logging"></a>Journalisation NPS SQL Server

Journalisation NPS SQL Server est utilisée lorsque vous avez besoin des informations d’état de session, pour la création de rapports et des analyses de données et pour centraliser et simplifier la gestion de vos données de gestion.

NPS offre la possibilité d’utiliser SQL Server connexion à l’authentification utilisateur enregistrement et gestion des comptes de demandes reçues à partir d’un ou plusieurs serveurs d’accès réseau à une source de données sur un ordinateur exécutant le moteur de bureau Microsoft SQL Server \(MSDE 2000\) ou n’importe quelle version de SQL Server ultérieure à SQL Server 2000.

Données de gestion sont transmises à partir de NPS au format XML à une procédure stockée dans la base de données, qui prend en charge les deux langage de requêtes structurées \(SQL\) et \(SQLXML\) XML. Enregistrement de l’authentification des utilisateurs et gestion des comptes de requêtes dans une base de données SQL Server compatible XML permettent à plusieurs serveurs NPS à une source de données.

### <a name="key-steps"></a>Décrit les principales étapes

Lors de la planification pour la gestion des comptes NPS à l’aide de la journalisation NPS SQL Server, vous pouvez utiliser les étapes suivantes.

- Déterminer si vous ou un autre membre de votre organisation a SQL Server 2000 ou SQL Server 2005 développement de base de données relationnelle d’expérience et que vous comprenez comment utiliser ces produits pour créer, modifier, gérer et gérer les bases de données SQL Server.

- Déterminer si SQL Server est installé sur le serveur NPS ou sur un ordinateur distant.

- Créez la procédure stockée que vous utiliserez dans votre base de données SQL Server pour traiter les fichiers XML entrantes qui contiennent des données de gestion NPS.

- Concevez la structure de la réplication de base de données SQL Server et le flux.

- Déterminer l’ou les applications que vous souhaitez utiliser pour afficher les données de gestion et produire des rapports.

- Envisagez d’utiliser des serveurs d’accès réseau qui envoient l’attribut de classe dans toutes les demandes de comptes. L’attribut de classe est envoyé au client RADIUS dans un message d’acceptation d’accès et est utile pour la corrélation entre les messages de demande de gestion des comptes avec des sessions d’authentification. Si l’attribut de classe est envoyé par le serveur d’accès réseau dans les messages de demande de comptabilité, il peut être utilisé pour faire correspondre les enregistrements de la gestion des comptes et l’authentification. La combinaison d’attributs Unique-numéro de série, redémarrage-Service-temps et l’adresse de serveur doit être un identificateur unique pour chaque authentification acceptées par le serveur.

- Envisagez d’utiliser des serveurs d’accès réseau qui prennent en charge la gestion des comptes intermédiaires.

- Envisagez d’utiliser des serveurs d’accès réseau qui envoient des messages Accounting-on et off comptabilité.

- Envisagez d’utiliser des serveurs d’accès réseau qui prennent en charge le stockage et transfert de données de gestion. Serveurs d’accès réseau qui prennent en charge cette fonctionnalité peuvent stocker des données de gestion lorsque le serveur d’accès réseau ne peut pas communiquer avec le serveur NPS. Lorsque le serveur NPS est disponible, le serveur d’accès réseau transmet les enregistrements stockés sur le serveur NPS, en fournissant une fiabilité accrue dans Gestion des comptes sur les serveurs d’accès réseau qui ne fournissent pas cette fonctionnalité.

- Envisagez de configurer toujours l’attribut Acct-intérimaire-intervalle dans les stratégies de réseau. L’attribut Acct-intérimaire-intervalle définit l’intervalle (en secondes) entre chaque mise à jour intermédiaire qui envoie le serveur d’accès réseau. En fonction de RFC 2869, la valeur de l’attribut Acct-intérimaire-intervalle ne doit pas être inférieure à 60 secondes, ou une minute et ne doit pas être inférieure à 600 secondes, ou de 10 minutes. Pour plus d’informations, voir RFC 2869, «RADIUS Extensions».

- Assurez-vous que l’enregistrement de l’état périodique est activé sur vos serveurs NPS.

