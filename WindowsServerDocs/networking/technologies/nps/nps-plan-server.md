---
title: Planifier un serveur NPS en tant que serveur RADIUS
description: Cette rubrique fournit des informations sur la planification dans Windows Server 2016 du déploiement de serveur RADIUS de serveur de stratégie réseau.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2900dd2c-0f70-4f8d-9650-ed83d51d509a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5fd89ef8d95735b8cbe1334ba51ed0059595dbc3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839170"
---
# <a name="plan-nps-as-a-radius-server"></a>Planifier un serveur NPS en tant que serveur RADIUS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Lorsque vous déployez le serveur NPS \(NPS\) comme un serveur d’authentification Dial-In Service RADIUS (Remote User), NPS effectue l’authentification, autorisation et comptabilité pour les demandes de connexion pour le domaine local et pour les domaines qui approuvent le domaine local. Vous pouvez utiliser ces instructions de planification pour simplifier votre déploiement de RADIUS.

Ces instructions de planification n’incluent pas les circonstances dans lesquelles vous souhaitez déployer NPS en tant que proxy RADIUS. Lorsque vous déployez le serveur NPS en tant que proxy RADIUS, NPS transfère les demandes de connexion à un serveur exécutant NPS ou autres serveurs RADIUS dans les domaines distants, des domaines non approuvés ou les deux. 

Avant de déployer NPS comme serveur RADIUS sur votre réseau, utilisez les instructions suivantes pour planifier votre déploiement.

- Planifier la configuration de serveur NPS.

- Planifier des clients RADIUS.

- Planifiez l’utilisation des méthodes d’authentification.

- Planifier les stratégies de réseau.

- Planifier la gestion des comptes NPS.

## <a name="plan-nps-configuration"></a>Planifier la configuration du serveur NPS

Vous devez décider de domaine dans lequel le serveur NPS est membre. Pour les environnements à plusieurs domaines, un serveur NPS peut authentifier les informations d’identification pour les comptes d’utilisateur dans le domaine dont il est membre et pour tous les domaines qui approuvent le domaine local du serveur NPS. Pour autoriser le serveur NPS lire les propriétés de numérotation des comptes d’utilisateur pendant le processus d’autorisation, vous devez ajouter le compte d’ordinateur du serveur NPS au RAS et NPSs au groupe pour chaque domaine.

Après avoir déterminé l’appartenance au domaine du serveur NPS, le serveur doit être configuré pour communiquer avec les clients RADIUS, également appelés serveurs d’accès réseau, en utilisant le protocole RADIUS. En outre, configurables les types d’événements qui enregistre NPS dans le journal et vous pouvez entrer une description pour le serveur des événements.

### <a name="key-steps"></a>Étapes clés

Lors de la planification pour la configuration du serveur NPS, vous pouvez utiliser les étapes suivantes.

- Déterminer les ports RADIUS que le serveur NPS utilise pour recevoir les messages RADIUS à partir de clients RADIUS. Les ports par défaut sont les ports UDP 1812 et 1645 pour les messages d’authentification RADIUS et les ports 1813 et 1646 pour les messages de gestion des comptes RADIUS.

- Si le serveur NPS est configuré avec plusieurs cartes réseau, déterminez les adaptateurs sur laquelle vous souhaitez le trafic RADIUS pour être autorisée.

- Déterminer les types d’événements que NPS enregistre dans le journal des événements. Vous pouvez consigner les demandes d’authentification rejetés, les demandes d’authentification réussie ou les deux types de requêtes.

- Déterminer si vous déployez plusieurs NPS. Pour fournir une tolérance de panne pour l’authentification RADIUS et la comptabilité, utilisez au moins deux NPSs. Un serveur NPS est utilisé en tant que serveur RADIUS principal et l’autre est utilisée en tant que sauvegarde. Chaque client RADIUS est ensuite configuré sur les deux NPSs. Si le serveur NPS principal devient indisponible, les clients RADIUS envoient des messages de demande d’accès à l’autre NPS.

- Planifier le script utilisé pour copier une configuration de serveur NPS pour les autres NPSs pour économiser sur les coûts d’administration et pour empêcher la configuration incorrecte d’un serveur. NPS fournit les commandes Netsh qui vous permettent de copier tout ou partie d’une configuration de serveur NPS pour les importer sur un autre serveur NPS. Vous pouvez exécuter les commandes manuellement à l’invite Netsh. Toutefois, si vous enregistrez votre séquence de commandes sous forme de script, vous pouvez exécuter le script à une date ultérieure si vous décidez de modifier vos configurations de serveur.

## <a name="plan-radius-clients"></a>Planifier des clients RADIUS

Les clients RADIUS sont des serveurs d’accès réseau, tels que les points d’accès sans fil, serveurs de réseau privé virtuel (VPN), 802.1 commutateurs compatibles X et les serveurs d’accès à distance. Les proxys RADIUS, qui transfèrent les messages de demande pour les serveurs RADIUS de la connexion, sont également des clients RADIUS. NPS prend en charge tous les serveurs d’accès réseau et les proxys RADIUS qui sont conformes avec le rayon de protocole comme décrit dans la RFC 2865, « Remote Authentication Dial-in User Service (RADIUS), » et RFC 2866, « Gestion des comptes RADIUS ».

>[!IMPORTANT]
>Les clients d’accès, telles que les ordinateurs clients, ne sont pas des clients RADIUS. Seuls les serveurs d’accès réseau et les serveurs proxy qui prennent en charge le protocole RADIUS sont des clients RADIUS.

En outre, les points d’accès sans fil et commutateurs doivent être capables d’authentification de 802. 1 X. Si vous souhaitez déployer Extensible Authentication Protocol \(EAP\) ou Protected Extensible Authentication Protocol \(PEAP\), points d’accès et les commutateurs doivent prendre en charge l’utilisation du protocole EAP.

Pour tester une interopérabilité de base pour les connexions PPP pour les points d’accès sans fil, configurez le point d’accès et le client d’accès à utiliser le protocole PAP (Password Authentication Protocol). Utiliser des protocoles d’authentification PPP supplémentaires, telles que PEAP, jusqu'à ce que vous avez testé ceux que vous souhaitez utiliser pour l’accès réseau.

### <a name="key-steps"></a>Étapes clés

Lors de la planification pour les clients RADIUS, vous pouvez utiliser les étapes suivantes.

- Documentez les attributs spécifiques au fournisseur (VSA) vous devez configurer dans NPS. Si vos serveurs d’accès réseau nécessitent VSA, consigner les informations de VSA pour une utilisation ultérieure lorsque vous configurez vos stratégies réseau dans NPS. 

- Documentez les adresses IP de clients RADIUS et le serveur NPS pour simplifier la configuration de tous les périphériques. Lorsque vous déployez vos clients RADIUS, vous devez les configurer pour utiliser le protocole RADIUS, avec l’adresse IP de NPS entrée en tant que le serveur d’authentification. Et lorsque vous configurez le serveur NPS pour communiquer avec vos clients RADIUS, vous devez entrer les adresses IP du client RADIUS dans le composant logiciel enfichable NPS.

- Créer des secrets partagés pour la configuration sur les clients RADIUS et dans le composant logiciel enfichable NPS. Vous devez configurer les clients RADIUS avec un secret partagé ou le mot de passe, que vous entrerez également dans le composant logiciel enfichable NPS lors de la configuration des clients RADIUS dans NPS.

## <a name="plan-the-use-of-authentication-methods"></a>Planifiez l’utilisation des méthodes d’authentification

NPS prend en charge les deux méthodes de l’authentification par mot de passe et certificat. Toutefois, pas tous les serveurs d’accès réseau prend en charge les mêmes méthodes d’authentification. Dans certains cas, vous souhaiterez déployer une autre méthode d’authentification en fonction du type d’accès réseau.

Par exemple, vous souhaiterez peut-être déployer sans fil et l’accès VPN pour votre organisation, mais utiliser une autre méthode d’authentification pour chaque type d’accès : EAP-TLS pour les connexions VPN, en raison de la sécurité renforcée par EAP avec Transport Layer Security (EAP-TLS) et PEAP-MS-CHAP v2 pour 802. 1 X des connexions sans fil.

La reconnexion spécialement conçu pour une utilisation avec les ordinateurs portables et autres périphériques sans fil PEAP avec Microsoft Challenge Handshake Authentication Protocol version 2 (PEAP-MS-CHAP v2) fournit une fonctionnalité nommée rapide. Reconnexion rapide permet aux clients sans fil pour vous déplacer entre les points d’accès sans fil sur le même réseau sans être authentifiés à chaque fois qu’ils associer à un nouveau point d’accès. Cela fournit une meilleure expérience pour les utilisateurs sans fil et leur permet de déplacer entre les points d’accès sans avoir à retaper leurs informations d’identification.
En raison de fast reconnectez-vous et la sécurité PEAP-MS-CHAP v2, PEAP-MS-CHAP v2 est un choix logique comme méthode d’authentification pour les connexions sans fil.

Pour les connexions VPN, EAP-TLS est une méthode d’authentification par certificat qui fournit une sécurité forte qui protège le trafic réseau, même si elles sont transmises sur Internet à partir d’ordinateurs domestique ou mobiles pour les serveurs VPN de votre organisation.

### <a name="certificate-based-authentication-methods"></a>Méthodes d’authentification par certificat

Méthodes d’authentification basée sur certificat présentent l’avantage de fournir une sécurité forte ; et ils ont l’inconvénient d’être plus difficile à déployer que les méthodes d’authentification par mot de passe.

PEAP-MS-CHAP v2 et EAP-TLS sont des méthodes d’authentification basée sur certificat, mais il existe plusieurs différences entre eux et la façon dans lequel ils sont déployés.

### <a name="eap-tls"></a>EAP-TLS

EAP-TLS utilise des certificats pour l’authentification client et serveur et nécessite que vous déployez une infrastructure à clé publique (PKI) dans votre organisation. Déploiement d’une infrastructure à clé publique peut être complexe et nécessite une phase de planification qui est indépendante de la planification de l’utilisation d’un serveur NPS comme serveur RADIUS.

Avec EAP-TLS, le serveur NPS inscrit un certificat de serveur à partir d’une autorité de certification \(autorité de certification\), et le certificat est enregistré sur l’ordinateur local dans le magasin de certificats. Pendant le processus d’authentification, l’authentification du serveur se produit lorsque le serveur NPS envoie son certificat de serveur au client d’accès pour prouver son identité au client d’accès. Le client d’accès examine les différentes propriétés du certificat pour déterminer si le certificat est valid et qu’il est approprié pour une utilisation lors de l’authentification de serveur. Si le certificat de serveur répond aux exigences de certificat de serveur minimale et émis par une autorité de certification approuvée par le client d’accès, le serveur NPS est correctement authentifié par le client.

De même, l’authentification du client se produit pendant le processus d’authentification lorsque le client envoie son certificat client au serveur NPS pour prouver son identité au serveur NPS. Le serveur NPS examine le certificat, et si le certificat client répond aux exigences de certificat minimale du client et est émis par une autorité de certification qui approuve le serveur NPS, l’accès client est correctement authentifié par le serveur NPS.

Bien qu’il soit nécessaire que le certificat de serveur est stocké dans le magasin de certificats sur le serveur NPS, le certificat de client ou un utilisateur peut être stocké dans soit le magasin de certificats sur le client ou sur une carte à puce.

Pour ce processus d’authentification réussisse, il est nécessaire que tous les ordinateurs disposent des certificats d’autorité de certification de votre organisation dans le magasin de certificats Autorités de Certification racine de confiance pour l’ordinateur Local et l’utilisateur actuel.

### <a name="peap-ms-chap-v2"></a>PEAP-MS-CHAP v2

PEAP-MS-CHAP v2 utilise un certificat pour l’authentification du serveur et mot de passe des informations d’identification pour l’authentification utilisateur. Étant donné que les certificats sont utilisés uniquement pour l’authentification de serveur, vous n’êtes pas obligé de déployer une infrastructure à clé publique pour pouvoir utiliser le protocole PEAP-MS-CHAP v2. Lorsque vous déployez PEAP-MS-CHAP v2, vous pouvez obtenir un certificat de serveur pour le serveur NPS dans un des deux manières suivantes :

- Vous pouvez installer les Services de certificats Active Directory (AD CS), puis inscrire automatiquement les certificats à NPSs. Si vous utilisez cette méthode, vous devez également inscrire le certificat d’autorité de certification pour les ordinateurs clients se connectant à votre réseau afin qu’ils approuvent le certificat émis pour le serveur NPS.

- Vous pouvez acheter un certificat de serveur à partir d’une autorité de certification publique telle que VeriSign. Si vous utilisez cette méthode, assurez-vous que vous sélectionnez une autorité de certification approuvée par les ordinateurs clients. Pour déterminer si les ordinateurs clients font confiance à une autorité de certification, ouvrez le composant logiciel enfichable certificats Microsoft Management Console (MMC) sur un ordinateur client et affichez le magasin d’autorités de Certification racine de confiance pour l’ordinateur Local et pour l’utilisateur actuel. S’il existe un certificat à partir de l’autorité de certification dans ces magasins de certificats, l’ordinateur client approuve l’autorité de certification et par conséquent approuveront n’importe quel certificat émis par l’autorité de certification.

Pendant le processus d’authentification avec PEAP-MS-CHAP v2, l’authentification du serveur se produit lorsque le serveur NPS envoie son certificat de serveur à l’ordinateur client. Le client d’accès examine les différentes propriétés du certificat pour déterminer si le certificat est valid et qu’il est approprié pour une utilisation lors de l’authentification de serveur. Si le certificat de serveur répond aux exigences de certificat de serveur minimale et émis par une autorité de certification approuvée par le client d’accès, le serveur NPS est correctement authentifié par le client.

Authentification de l’utilisateur se produit lorsqu’un utilisateur tente de se connecter le réseau types en fonction du mot de passe des informations d’identification et essaie de se connecter. NPS reçoit les informations d’identification et effectue l’authentification et l’autorisation. Si l’utilisateur est authentifié et autorisé correctement, et si l’ordinateur client authentifié avec succès le serveur NPS, la demande de connexion est accordée.

### <a name="key-steps"></a>Étapes clés

Lors de la planification pour l’utilisation des méthodes d’authentification, vous pouvez utiliser les étapes suivantes.

- Identifier les types d’accès réseau que vous envisagez d’offrir, telles que de sans fil, VPN, 802.1 X et le commutateur compatible et l’accès à distance.

- Déterminer la méthode d’authentification ou les méthodes que vous souhaitez utiliser pour chaque type d’accès. Il est recommandé d’utiliser les méthodes d’authentification par certificat qui offrent une sécurité renforcée ; Toutefois, il peut être pratique pour vous permettent de déployer une infrastructure à clé publique, les autres méthodes d’authentification peuvent fournir un meilleur équilibre de ce dont vous avez besoin pour votre réseau.

- Si vous déployez EAP-TLS, planifiez votre déploiement de l’infrastructure à clé publique. Cela inclut la planification des modèles de certificats que vous vous apprêtez à utiliser pour les certificats de serveur et les certificats d’ordinateur client. Il inclut également déterminer comment inscrire des certificats sur le membre du domaine et les ordinateurs membre n’appartenant pas au domaine et déterminer si vous souhaitez utiliser des cartes à puce.

- Si vous déployez PEAP-MS-CHAP v2, déterminez si vous souhaitez installer les services AD CS pour émettre des certificats de serveur pour votre NPSs ou si vous souhaitez acheter des certificats de serveur à partir d’une autorité de certification publique, telle que VeriSign.

### <a name="plan-network-policies"></a>Planifier les stratégies de réseau

Stratégies de réseau sont utilisées par le serveur NPS de déterminer si les demandes de connexion des clients RADIUS sont autorisés. NPS utilise également les propriétés d’accès à distance du compte d’utilisateur pour effectuer une détermination d’autorisation.

Étant donné que les stratégies de réseau sont traités dans l’ordre dans lequel ils apparaissent dans le composant logiciel enfichable NPS, envisagez de placer vos stratégies plus restrictives en premier dans la liste des stratégies. Pour chaque demande de connexion, NPS tente de faire correspondre les conditions de la stratégie avec les propriétés de demande de connexion. NPS examine chaque stratégie de réseau dans l’ordre jusqu'à ce qu’il trouve une correspondance. Si elle ne trouve pas de correspondance, la demande de connexion est rejetée.

### <a name="key-steps"></a>Étapes clés

Lors de la planification pour les stratégies réseau, vous pouvez utiliser les étapes suivantes.

- Déterminer l’ordre de traitement de serveur NPS par défaut des stratégies de réseau, du plus restrictif au moins restrictif.

- Déterminer l’état de la stratégie. L’état de la stratégie peut avoir la valeur d’activé ou désactivé. Si la stratégie est activée, NPS évalue la stratégie lors de l’exécution d’autorisation. Si la stratégie n’est pas activée, elle n’est pas évaluée.

- Déterminer le type de stratégie. Vous devez déterminer si la stratégie est conçue pour accorder l’accès lorsque les conditions de la stratégie sont mis en correspondance par la demande de connexion ou si la stratégie est conçue pour refuser l’accès lorsque les conditions de la stratégie sont mises en correspondance par la demande de connexion. Par exemple, si vous souhaitez refuser explicitement l’accès sans fil pour les membres d’un groupe Windows, vous pouvez créer une stratégie de réseau qui spécifie le groupe, la méthode de connexion sans fil et qui a une stratégie de type de paramètre de refuser l’accès.

- Déterminez si vous souhaitez que NPS pour ignorer les propriétés d’accès à distance des comptes d’utilisateur qui sont membres du groupe sur lequel repose la stratégie. Lorsque ce paramètre n’est pas activé, les propriétés de numérotation des comptes d’utilisateur remplacent les paramètres qui sont configurés dans les stratégies de réseau. Par exemple, si une stratégie de réseau est configurée qui accorde l’accès à un utilisateur, mais les propriétés d’accès à distance du compte d’utilisateur pour cet utilisateur sont définies pour refuser l’accès, l’accès est refusé. Mais si vous activez les propriétés de paramètre ignorer compte accès à distance de l’utilisateur de type de stratégie, le même utilisateur a accès au réseau.

- Déterminer si la stratégie utilise le paramètre de source de stratégie. Ce paramètre vous permet de facilement spécifier une source de toutes les demandes d’accès. Les sources possibles sont une passerelle des Services Terminal Server (passerelle TS), un serveur d’accès à distance (VPN ou accès à distance), un serveur DHCP, un point d’accès sans fil et un serveur de l’autorité HRA. Vous pouvez également spécifier une source spécifiques au fournisseur.

- Déterminer les conditions qui doivent être remplies afin que la stratégie de réseau à appliquer.

- Déterminer les paramètres qui sont appliqués si les conditions de la stratégie de réseau sont mises en correspondance par la demande de connexion.

- Déterminez si vous souhaitez utiliser, modifier ou supprimer les stratégies de réseau par défaut.

## <a name="plan-nps-accounting"></a>Planifier la gestion des comptes NPS

NPS fournit la capacité d’enregistrer des données de comptabilité RADIUS, tels que l’authentification des utilisateurs et des demandes de gestion, dans trois formats : Format IAS, format compatible avec la base de données et la journalisation de Microsoft SQL Server. 

Format IAS et le format compatible avec la base de données créent les fichiers journaux sur le serveur NPS local au format de fichier texte. 

La journalisation SQL Server fournit la possibilité de se connecter à un SQL Server 2000 ou une base de données compatibles avec SQL Server 2005 XML, extension de gestion des comptes RADIUS pour exploiter les avantages de journalisation dans une base de données relationnelle.

### <a name="key-steps"></a>Étapes clés

Lors de la planification pour la gestion des comptes NPS, vous pouvez utiliser les étapes suivantes.

- Déterminez si vous souhaitez stocker les données de gestion de serveur NPS dans les fichiers journaux ou dans une base de données SQL Server.

### <a name="nps-accounting-using-local-log-files"></a>Gestion des comptes NPS à l’aide des fichiers journaux locaux

L’enregistrement de l’authentification des utilisateurs et la comptabilité des demandes dans les fichiers journaux sert principalement à des fins de facturation et d’analyse de connexion et est également utile comme outil d’enquête de sécurité, vous offrant une méthode pour le suivi de l’activité d’un utilisateur malveillant après une attaque.

### <a name="key-steps"></a>Étapes clés

Lors de la planification pour la gestion des comptes NPS à l’aide des fichiers journaux locaux, vous pouvez utiliser les étapes suivantes.

- Déterminer le format de fichier texte que vous souhaitez utiliser pour vos fichiers journaux de serveur NPS.

- Choisissez le type d’informations que vous souhaitez journaliser. Vous pouvez consigner les demandes de gestion, les demandes d’authentification et l’état périodique.

- Déterminer l’emplacement de disque dur où vous souhaitez stocker vos fichiers journaux.

- Concevoir votre solution de sauvegarde de fichier journal. L’emplacement de disque dur où vous stockez vos fichiers de journal doit être un emplacement qui vous permet de sauvegarder facilement vos données. En outre, l’emplacement de disque dur doit être protégée en configurant la liste de contrôle d’accès (ACL) pour le dossier où sont stockés les fichiers journaux.

- Déterminer la fréquence à laquelle vous souhaitez des fichiers journaux à créer. Si vous souhaitez que les fichiers journaux doit être créé en fonction de la taille du fichier, déterminer la taille de fichier maximale autorisée avant la création d’un nouveau fichier journal par le serveur NPS.

- Déterminez si vous souhaitez que le serveur NPS pour supprimer les anciens fichiers journaux si le disque dur manque d’espace de stockage.

- Déterminer l’ou les applications que vous souhaitez utiliser pour afficher les données de gestion et de générer des rapports.

### <a name="nps-sql-server-logging"></a>Journalisation du serveur NPS SQL Server

Journalisation de NPS SQL Server est utilisée lorsque vous avez besoin des informations d’état de session pour la création de rapports et des analyses de données et à centraliser et simplifier la gestion de vos données de comptabilité.

NPS fournit la possibilité d’utiliser SQL Server, la journalisation dans l’authentification utilisateur des enregistrements et de comptabilité des demandes reçues à partir d’un ou plusieurs serveurs d’accès réseau à une source de données sur un ordinateur exécutant Microsoft SQL Server Desktop Engine \(MSDE 2000\), ou n’importe quelle version de SQL Server SQL Server 2000 au plus tard.

Données de gestion sont passées à partir du serveur NPS au format XML à une procédure stockée dans la base de données, qui prend en charge les deux langages de requête structurée \(SQL\) et XML \(SQLXML\). L’enregistrement de l’authentification des utilisateurs et la comptabilité des demandes dans une base de données SQL Server compatible XML permettent à plusieurs NPSs avoir une source de données.

### <a name="key-steps"></a>Étapes clés

Lors de la planification pour la gestion des comptes NPS à l’aide de la journalisation NPS SQL Server, vous pouvez utiliser les étapes suivantes.

- Déterminer si vous ou un autre membre de votre organisation a développement de base de données relationnelle SQL Server 2000 ou SQL Server 2005 d’expérience et que vous savez comment utiliser ces produits pour créer, modifier, administrer et gérer des bases de données SQL Server.

- Déterminer si SQL Server est installé sur le serveur NPS ou sur un ordinateur distant.

- Créez la procédure stockée que vous utiliserez dans votre base de données SQL Server pour traiter les fichiers XML entrants qui contiennent des données de gestion NPS.

- Concevoir la structure de réplication de base de données SQL Server et le flux.

- Déterminer l’ou les applications que vous souhaitez utiliser pour afficher les données de gestion et de générer des rapports.

- Envisagez d’utiliser des serveurs d’accès réseau qui envoient l’attribut de classe dans toutes les demandes de comptes. L’attribut de classe est envoyé au client RADIUS dans un message d’acceptation d’accès et est utile pour la corrélation des messages de demande de compte avec les sessions d’authentification. Si l’attribut de classe est envoyé par le serveur d’accès réseau dans les messages de demande de comptabilité, il peut être utilisé pour faire correspondre les enregistrements de comptabilité et de l’authentification. La combinaison d’attributs Unique-numéro de série, heure de redémarrage de Service et l’adresse de serveur doit être une identification unique pour chaque authentification que le serveur accepte.

- Envisagez d’utiliser des serveurs d’accès réseau qui prennent en charge la gestion de compte provisoire.

- Envisagez d’utiliser des serveurs d’accès réseau qui envoient des messages de gestion des comptes actifs et inactifs.

- Envisagez d’utiliser des serveurs d’accès réseau qui prennent en charge le stockage et transfert de données de gestion. Serveurs d’accès réseau qui prennent en charge cette fonctionnalité peuvent stocker des données de gestion lorsque le serveur d’accès réseau ne peut pas communiquer avec le serveur NPS. Lorsque le serveur NPS est disponible, le serveur d’accès réseau transfère les enregistrements stockés au serveur NPS, en fournissant une fiabilité accrue dans Gestion des comptes sur les serveurs d’accès réseau qui ne fournissent pas cette fonctionnalité.

- Envisagez de toujours configurer l’attribut Acct-Interim-Interval dans les stratégies de réseau. L’attribut Acct-Interim-Interval définit l’intervalle (en secondes) entre chaque mise à jour intermédiaire qui envoie le serveur d’accès réseau. En fonction de la RFC 2869, la valeur de l’attribut Acct-Interim-Interval ne doit pas être inférieure à 60 secondes ou d’une minute et ne doit pas être inférieure à 600 secondes ou de 10 minutes. Pour plus d’informations, consultez RFC 2869, « RADIUS Extensions ».

- Vérifiez que la journalisation de l’état périodique est activée sur votre NPSs.

