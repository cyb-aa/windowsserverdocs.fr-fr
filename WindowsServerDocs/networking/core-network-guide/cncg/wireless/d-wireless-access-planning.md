---
title: Planification du déploiement de l’accès sans fil
description: Cette rubrique fait partie du Guide de mise en réseau de Windows Server 2016 « déployer l’accès sans fil authentifié 802.1 X basé sur un mot de passe »
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 8c632d02-2270-4a82-8fc4-74ea3747f079
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 44ca48ab05a7f63b1b9dd07f955e68accbcefdb7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356076"
---
# <a name="wireless-access-deployment-planning"></a>Planification du déploiement de l’accès sans fil

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Avant de déployer l’accès sans fil, vous devez planifier les éléments suivants :

- Installation des points \(\) d’accès sans fil sur votre réseau

- Configuration du client sans fil et accès

Les sections suivantes fournissent des détails sur ces étapes de planification.

## <a name="planning-wireless-ap-installations"></a>Planification des installations de AP sans fil
Lorsque vous concevez votre solution d’accès réseau sans fil, vous devez effectuer les opérations suivantes :

1. Déterminer les normes que vos points d’accès sans fil doivent prendre en charge
2. Déterminer les zones de couverture où vous souhaitez fournir un service sans fil
3. Déterminer où vous souhaitez localiser les points d’accès sans fil

En outre, vous devez planifier un schéma d’adresse IP pour les clients sans fil et les points d’accès sans fil. Consultez la section **planifier la configuration du point d’accès sans fil dans NPS** ci-dessous pour obtenir des informations connexes.

### <a name="verify-wireless-ap-support-for-standards"></a>Vérifier la prise en charge des points d’accès sans fil pour les normes
Pour des raisons de cohérence et de facilité de déploiement et de gestion des points d’accès, il est recommandé de déployer des points d’accès sans fil de la même manière et du même modèle.

Les points d’accès sans fil que vous déployez doivent prendre en charge les éléments suivants :

- **IEEE 802.1 X**

- **Authentification RADIUS**

- **Authentification et chiffrement sans fil.** Listées dans l’ordre de préférence la plus faible :

    1.  WPA2\-Enterprise avec AES

    2.  WPA2\-Enterprise avec TKIP

    3.  WPA\-Enterprise avec AES

    4.  WPA\-Enterprise avec TKIP

>[!NOTE]
>Pour déployer WPA2, vous devez utiliser des cartes réseau sans fil et des points d’accès sans fil qui prennent également en charge WPA2. Dans le cas contraire\-, utilisez WPA Enterprise.

En outre, pour renforcer la sécurité du réseau, les points d’accès sans fil doivent prendre en charge les options de sécurité suivantes :

- **Filtrage DHCP.** Le point d’accès sans fil doit filtrer sur les ports IP pour empêcher la transmission des messages de diffusion DHCP dans les cas où le client sans fil est configuré en tant que serveur DHCP. Le point d’accès sans fil doit empêcher le client d’envoyer des paquets IP du port UDP 68 au réseau.

- **Filtrage DNS.** Le point d’accès sans fil doit filtrer sur les ports IP pour empêcher un client de s’exécuter en tant que serveur DNS. Le point d’accès sans fil doit empêcher le client d’envoyer des paquets IP du port TCP ou UDP 53 au réseau.

- **Isolation du client** Si votre point d’accès sans fil fournit des fonctionnalités d’isolation du client, vous devez activer la fonctionnalité pour \(empêcher\) les attaques par usurpation d’adresse ARP du protocole de résolution d’adresses.

### <a name="identify-areas-of-coverage-for-wireless-users"></a>Identifier les zones de couverture pour les utilisateurs sans fil
Utilisez des schémas architecturaux pour chaque bâtiment pour identifier les zones où vous souhaitez fournir une couverture sans fil. Par exemple, identifiez les bureaux, salles de conférences, halls, cafétérias ou courtyardss appropriés.

Sur les dessins, indiquez tous les appareils qui interfèrent avec les signaux sans fil, tels que les appareils médicaux, les caméras vidéo sans fil, les téléphones sans fil qui fonctionnent dans le 2,4 jusqu' \(à\) 2,5 GHz, le matériel d’ISM scientifique et médical. plage et appareils Bluetooth\-.

Sur le dessin, marquez les aspects du bâtiment qui peuvent interférer avec les signaux sans fil. les objets métalliques utilisés dans la construction d’un immeuble peuvent affecter le signal sans fil. Par exemple, les objets communs suivants peuvent interférer avec la propagation de signal : Les ascenseurs, les conduits de chauffage et\-de climatisation et le support concret Girders.

Reportez-vous au fabricant de votre AP pour plus d’informations sur les sources susceptibles de provoquer une atténuation de la fréquence radio du point d’accès sans fil. La plupart des APs fournissent des logiciels de test que vous pouvez utiliser pour vérifier la puissance des signaux, le taux d’erreur et le débit des données.

### <a name="determine-where-to-install-wireless-aps"></a>Déterminer où installer les points d’accès sans fil
Sur les dessins d’architecture, Localisez vos points d’accès sans fil suffisamment près pour fournir une couverture sans fil suffisamment grande, mais assez loin pour qu’ils n’interfèrent pas entre eux.

La distance nécessaire entre les points d’accès dépend du type d’antenne AP et AP, des aspects du bâtiment qui bloquent les signaux sans fil et d’autres sources d’interférence. Vous pouvez marquer des placements de points d’accès sans fil afin que chaque point d’accès sans fil ne soit pas supérieur à 300 centimètres de tout point d’accès sans fil adjacent. Consultez la documentation du fabricant du point d’accès sans fil pour connaître les spécifications et les recommandations relatives au placement.

Installez temporairement les points d’accès sans fil aux emplacements spécifiés sur vos dessins d’architecture. Ensuite, à l’aide d’un ordinateur portable équipé d’une carte sans fil 802,11 et du logiciel d’étude de site qui est couramment fourni avec les adaptateurs sans fil, déterminez la puissance du signal dans chaque zone de couverture.

Dans les zones de couverture où la puissance du signal est faible, positionnez le point d’accès pour améliorer la force du signal pour la zone de couverture, installez des points d’accès sans fil supplémentaires pour fournir la couverture nécessaire, déplacer ou supprimer des sources d’interférence de signal.  

Mettez à jour vos dessins architecturaux pour indiquer le positionnement final de tous les points d’accès sans fil. Le fait de disposer d’une carte de placement précise des points d’accès est plus tard lors des opérations de dépannage ou lorsque vous souhaitez mettre à niveau ou remplacer des points d’accès.

### <a name="plan-wireless-ap-and-nps-radius-client-configuration"></a>Planifier le point d’accès sans fil et la configuration du client RADIUS NPS
Vous pouvez utiliser NPS pour configurer des points d’accès sans fil individuellement ou dans des groupes.

Si vous déployez un grand réseau sans fil qui comprend de nombreux APs, il est beaucoup plus facile de configurer des points d’accès dans des groupes. Pour ajouter les points d’accès en tant que groupes de clients RADIUS dans NPS, vous devez configurer les points d’accès à l’aide de ces propriétés.

- Les points d’accès sans fil sont configurés avec des adresses IP de la même plage d’adresses IP.

- Les points d’accès sans fil sont tous configurés avec le même secret partagé.

### <a name="plan-the-use-of-peap-fast-reconnect"></a>Planifier l’utilisation de la reconnexion rapide PEAP
Dans une infrastructure 802.1 X, les points d’accès sans fil sont configurés en tant que clients RADIUS sur les serveurs RADIUS. Lorsque la reconnexion rapide PEAP est déployée, un client sans fil qui se déplace entre deux points d’accès ou plus n’a pas besoin d’être authentifié avec chaque nouvelle association.

La reconnexion rapide PEAP réduit le temps de réponse pour l’authentification entre le client et l’authentificateur, car la demande d’authentification est transférée du nouveau point d’accès au serveur NPS qui a effectué à l’origine l’authentification et l’autorisation pour le client. demande de connexion.

Étant donné que le client PEAP et le serveur NPS utilisent tous les deux des \(propriétés\) \(de connexion TLS de Transport Layer Security précédemment mises en cache\)dont la collection est nommée descripteur TLS, le serveur NPS peut déterminer rapidement cela le client est autorisé à se reconnecter.

>[!IMPORTANT]
>Pour que la reconnexion rapide fonctionne correctement, les points d’accès doivent être configurés en tant que clients RADIUS du même serveur NPS.

Si le serveur NPS d’origine devient indisponible, ou si le client se déplace vers un point d’accès qui est configuré en tant que client RADIUS vers un autre serveur RADIUS, l’authentification complète doit se produire entre le client et le nouvel authentificateur.

### <a name="wireless-ap-configuration"></a>Configuration du point d’accès sans fil
La liste suivante récapitule les éléments couramment configurés sur les\-points d’accès sans fil 802.1 x :

> [!NOTE]
> Les noms d’éléments peuvent varier en fonction de la personnalisation et du modèle et peuvent être différents de ceux répertoriés dans la liste suivante. Pour plus d’informations sur la configuration\-, consultez la documentation de votre point d’accès sans fil.

- SSID de l’identificateur du jeu de service.  **\(\)** Il s’agit du nom du réseau \(sans fil, par exemple ExampleWlan\), et du nom qui est publié pour les clients sans fil. Pour éviter toute confusion, le SSID que vous choisissez de publier ne doit pas correspondre à l’identificateur SSID qui est diffusé par les réseaux sans fil qui se trouvent dans la plage de réception de votre réseau sans fil.

    Dans les cas où plusieurs points d’accès sans fil sont déployés dans le cadre du même réseau sans fil, configurez chaque AP sans fil avec le même SSID. Dans les cas où plusieurs points d’accès sans fil sont déployés dans le cadre du même réseau sans fil, configurez chaque AP sans fil avec le même SSID.  

    Dans les cas où vous avez besoin de déployer différents réseaux sans fil pour répondre à des besoins spécifiques de l’entreprise, vos points d’accès sans fil sur un réseau doivent diffuser un SSID\(différent\)de celui de vos autres réseaux. Par exemple, si vous avez besoin d’un réseau sans fil distinct pour vos employés et invités, vous pouvez configurer vos points d’accès sans fil pour le réseau d’entreprise avec le SSID défini sur diffusion **ExampleWLAN**. Pour votre réseau invité, vous pouvez ensuite définir le SSID de chaque point d’accès sans fil sur Broadcast **GuestWLAN**. De cette façon, vos employés et invités peuvent se connecter au réseau prévu sans confusion inutile.  

    > [!TIP]  
    > Certains points d’accès sans fil peuvent diffuser plusieurs SSID pour s’adapter à des\-déploiements sur plusieurs réseaux. Les points d’accès sans fil qui peuvent diffuser plusieurs SSID peuvent réduire les coûts de déploiement et de maintenance opérationnelle.  

- **Authentification et chiffrement sans fil**.

    L’authentification sans fil est l’authentification de sécurité qui est utilisée lorsque le client sans fil s’associe à un point d’accès sans fil.  

    Le chiffrement sans fil est le chiffrement de chiffrement de sécurité utilisé avec l’authentification sans fil pour protéger les communications envoyées entre le point d’accès sans fil et le client sans fil.  

- Adresse IP du **point d’accès sans fil statique.\) \(** Sur chaque point d’accès sans fil, configurez une adresse IP statique unique. Si le sous-réseau est traité par un serveur DHCP, assurez-vous que toutes les adresses IP AP se trouvent dans une plage d’exclusion DHCP afin que le serveur DHCP n’essaie pas d’émettre la même adresse IP vers un autre ordinateur ou périphérique. Les plages d’exclusion sont documentées dans la procédure « pour créer et activer une nouvelle étendue DHCP » dans le [Guide du réseau de base](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide). Si vous envisagez de configurer des points d’accès en tant que clients RADIUS par groupe dans NPS, chaque AP du groupe doit avoir une adresse IP de la même plage d’adresses IP.

- **Nom DNS**. Certains points d’accès sans fil peuvent être configurés avec un nom DNS. Configurez chaque AP sans fil avec un nom unique. Par exemple, si vous avez déployé des points d’accès sans fil\-dans une génération à plusieurs récits, vous pouvez nommer les trois premiers points d’accès sans fil\-qui sont déployés au troisième étage\-AP3 01, AP3\-02 et AP3 03.

- **Masque de sous-réseau du point d’accès sans fil**. Configurez le masque pour désigner la partie de l’adresse IP qui correspond à l’ID réseau et la partie de l’adresse IP qui est l’hôte.

- **Service DHCP AP**. Si votre point d’accès sans fil\-dispose d’un service DHCP intégré, désactivez-le.

- **Secret partagé RADIUS**. Utilisez un secret partagé RADIUS unique pour chaque AP sans fil, sauf si vous envisagez de configurer des clients RADIUS NPS dans des groupes : dans ce cas, vous devez configurer tous les points d’accès du groupe avec le même secret partagé. Les secrets partagés doivent être une séquence aléatoire d’au moins 22 caractères, avec des lettres majuscules et minuscules, des chiffres et des signes de ponctuation. Pour garantir un caractère aléatoire, vous pouvez utiliser un programme de génération de caractères aléatoires pour créer vos secrets partagés. Nous vous recommandons d’enregistrer le secret partagé pour chaque point d’accès sans fil et de le stocker dans un emplacement sécurisé, tel qu’un coffre-fort d’Office. Lorsque vous configurez des clients RADIUS dans la console NPS, vous allez créer une version virtuelle de chaque AP. Le secret partagé que vous configurez sur chaque AP virtuel dans NPS doit correspondre au secret partagé sur le point d’accès physique réel.

- **Adresse IP du serveur RADIUS**. Tapez l’adresse IP du serveur NPS que vous souhaitez utiliser pour authentifier et autoriser les demandes de connexion à ce point d’accès.

- **Port\(UDPs\)** . Par défaut, NPS utilise les ports UDP 1812 et 1645 pour les messages d’authentification RADIUS et les ports UDP 1813 et 1646 pour les messages de gestion des comptes RADIUS. Il est recommandé de ne pas modifier les paramètres des ports UDP RADIUS par défaut.

- **VSA**. Certains points d’accès sans\-fil requièrent\) des attributs \(spécifiques au fournisseur VSA pour fournir une fonctionnalité de point d’accès sans fil complète.

- **Filtrage DHCP**. Configurez des points d’accès sans fil pour empêcher les clients sans fil d’envoyer des paquets IP du port UDP 68 vers le réseau. Consultez la documentation de votre point d’accès sans fil pour configurer le filtrage DHCP.

- **Filtrage DNS**. Configurez des points d’accès sans fil pour empêcher les clients sans fil d’envoyer des paquets IP du port TCP ou UDP 53 vers le réseau. Consultez la documentation de votre point d’accès sans fil pour configurer le filtrage DNS.

## <a name="planning-wireless-client-configuration-and-access"></a>Planification de la configuration et de l’accès des clients sans fil

Lorsque vous planifiez le déploiement d'\-un accès sans fil authentifié 802.1 x, vous devez\-prendre en compte plusieurs facteurs spécifiques au client :

- **Planification de la prise en charge de plusieurs normes**.

    Déterminez si vos ordinateurs sans fil utilisent tous la même version de Windows ou s’il s’agit d’un mélange d’ordinateurs exécutant des systèmes d’exploitation différents. S’ils sont différents, assurez-vous de bien comprendre les différences de normes prises en charge par les systèmes d’exploitation.

    Déterminez si toutes les cartes réseau sans fil sur tous les ordinateurs clients sans fil prennent en charge les mêmes normes sans fil ou si vous devez prendre en charge des normes variables. Par exemple, déterminez si certains pilotes de carte réseau prennent\-en charge WPA2 Enterprise et AES, tandis\-que d’autres ne prennent en charge que WPA Enterprise et TKIP.

- **Planification du mode d’authentification du client**. Les modes d’authentification définissent la façon dont les clients Windows traitent les informations d’identification de domaine. Vous pouvez choisir parmi les trois modes d’authentification réseau suivants dans les stratégies de réseau sans fil.  

    1. **Nouvelle\-authentification de l’utilisateur**. Ce mode spécifie que l’authentification est toujours effectuée à l’aide des informations d’identification de sécurité en fonction de l’état actuel de l’ordinateur. Quand aucun utilisateur n’est connecté à l’ordinateur, l’authentification est effectuée à l’aide des informations d’identification de l’ordinateur. Lorsqu’un utilisateur a ouvert une session sur l’ordinateur, l’authentification est toujours effectuée à l’aide des informations d’identification de l’utilisateur.  

    2. **Ordinateur uniquement** Mode ordinateur uniquement spécifie que l’authentification est toujours effectuée en utilisant uniquement les informations d’identification de l’ordinateur.  

    3.  **Authentification utilisateur** Le mode d’authentification de l’utilisateur spécifie que l’authentification est effectuée uniquement lorsque l’utilisateur a ouvert une session sur l’ordinateur. Quand aucun utilisateur n’est connecté à l’ordinateur, les tentatives d’authentification ne sont pas effectuées.  

- **Planification des restrictions sans fil**. Déterminez si vous souhaitez fournir à tous vos utilisateurs sans fil le même niveau d’accès à votre réseau sans fil, ou si vous souhaitez restreindre l’accès pour certains de vos utilisateurs sans fil. Vous pouvez appliquer des restrictions dans NPS à des groupes spécifiques d’utilisateurs sans fil.  Par exemple, vous pouvez définir des jours et heures spécifiques auxquels certains groupes sont autorisés à accéder au réseau sans fil.  

- **Méthodes de planification pour l’ajout de nouveaux ordinateurs sans fil**. Pour les\-ordinateurs dotés d’une connexion sans fil qui sont joints à votre domaine avant de déployer votre réseau sans fil, si l’ordinateur est connecté à un segment du réseau câblé qui n’est pas protégé par 802.1 x, les paramètres de configuration sans fil sont appliqué automatiquement après avoir configuré des stratégies \(de réseau\) sans fil IEEE 802,11 sur le contrôleur de domaine et une fois que stratégie de groupe est actualisé sur le client sans fil.  

    Toutefois, pour les ordinateurs qui ne sont pas encore joints à votre domaine, vous devez planifier une méthode pour appliquer les paramètres requis pour l’accès\-authentifié 802.1 x. Par exemple, déterminez si vous souhaitez joindre l’ordinateur au domaine à l’aide de l’une des méthodes suivantes.

    1.  Connectez l’ordinateur à un segment du réseau câblé qui n’est pas protégé par 802.1 X, puis joignez l’ordinateur au domaine.

    2.  Fournissez à vos utilisateurs sans fil les étapes et les paramètres nécessaires pour ajouter leur propre profil de démarrage sans fil, ce qui leur permet de joindre l’ordinateur au domaine.

    3.  Affectez au personnel informatique de joindre des clients sans fil au domaine.

### <a name="planning-support-for-multiple-standards"></a>Planification de la prise en charge de plusieurs normes

L’extension de \(stratégies de\) réseau sans fil IEEE 802,11 dans stratégie de groupe fournit un large éventail d’options de configuration pour prendre en charge diverses options de déploiement.

Vous pouvez déployer des points d’accès sans fil configurés avec les normes que vous souhaitez prendre en charge, puis configurer plusieurs profils sans \(fil dans\) des stratégies de réseau sans fil IEEE 802,11, chaque profil spécifiant un ensemble de normes. dont vous avez besoin.

Par exemple, si votre réseau comporte des ordinateurs sans fil qui\-prennent en charge WPA2 Enterprise et AES, d'\-autres ordinateurs qui prennent en charge WPA Enterprise et AES,\-et d’autres ordinateurs qui prennent en charge uniquement WPA Enterprise et TKIP, vous devez Déterminez si vous souhaitez :

- Configurez un profil unique pour prendre en charge tous les ordinateurs sans fil à l’aide de la méthode de chiffrement la plus faible que tous vos\-ordinateurs prennent en charge (dans ce cas, WPA Enterprise et TKIP).  
- Configurez deux profils pour fournir la meilleure sécurité possible prise en charge par chaque ordinateur sans fil. Dans cette instance, vous configurez un profil qui spécifie le \(chiffrement WPA2\-Enterprise\)et AES le plus élevé, et un profil qui\-utilise le chiffrement WPA Enterprise et TKIP plus faible. Dans cet exemple, il est essentiel de placer le profil qui utilise WPA2\-Enterprise et AES au plus haut dans l’ordre de préférence. Les ordinateurs qui ne peuvent pas utiliser WPA2\-Enterprise et AES passeront automatiquement au profil suivant dans l’ordre de préférence et traiteront le profil qui spécifie WPA\-Enterprise et TKIP.

> [!IMPORTANT]
> Vous devez placer le profil avec les normes les plus sécurisées plus haut dans la liste de profils triée, car la connexion des ordinateurs utilise le premier profil qu’ils sont en mesure d’utiliser.

### <a name="planning-restricted-access-to-the-wireless-network"></a>Planification de l’accès restreint au réseau sans fil

Dans de nombreux cas, vous souhaiterez peut-être fournir aux utilisateurs sans fil des niveaux différents d’accès au réseau sans fil. Par exemple, vous souhaiterez peut-être autoriser certains utilisateurs à accéder de façon illimitée, quelle que soit l’heure de la journée, tous les jours de la semaine. Pour les autres utilisateurs, vous pouvez uniquement autoriser l’accès pendant les heures de base, du lundi au vendredi et refuser l’accès le samedi et le dimanche.

Ce guide fournit des instructions pour créer un environnement d’accès qui place tous vos utilisateurs sans fil dans un groupe avec un accès courant aux ressources sans fil. Vous créez un groupe de sécurité utilisateurs sans fil dans le composant logiciel enfichable\-utilisateurs et ordinateurs Active Directory, puis attribuez à chaque utilisateur auquel vous souhaitez accorder un accès sans fil un membre de ce groupe.

Quand vous configurez des stratégies de réseau NPS, vous spécifiez le groupe de sécurité utilisateurs sans fil comme objet traité par NPS lors de la détermination de l’autorisation.

Toutefois, si votre déploiement nécessite la prise en charge de différents niveaux d’accès, vous devez uniquement effectuer les opérations suivantes :  

1. Créez plusieurs groupes de sécurité utilisateurs sans fil pour créer des groupes de sécurité sans fil supplémentaires dans Active Directory utilisateurs et ordinateurs. Par exemple, vous pouvez créer un groupe qui contient des utilisateurs disposant d’un accès complet, un groupe pour ceux qui ont uniquement accès pendant les heures de travail normales et d’autres groupes qui correspondent à d’autres critères qui correspondent à vos besoins.

2. Ajoutez des utilisateurs aux groupes de sécurité appropriés que vous avez créés.

3. Configurez des stratégies de réseau NPS supplémentaires pour chaque groupe de sécurité sans fil supplémentaire et configurez les stratégies pour appliquer les conditions et contraintes dont vous avez besoin pour chaque groupe.

### <a name="planning-methods-for-adding-new-wireless-computers"></a>Méthodes de planification pour l’ajout de nouveaux ordinateurs sans fil

La méthode recommandée pour joindre de nouveaux ordinateurs sans fil au domaine, puis ouvrir une session sur le domaine consiste à utiliser une connexion câblée à un segment du réseau local qui a accès aux contrôleurs de domaine et qui n’est pas protégé par un commutateur Ethernet d’authentification 802.1 X.

Toutefois, dans certains cas, il peut être difficile d’utiliser une connexion câblée pour joindre des ordinateurs au domaine ou, pour un utilisateur, d’utiliser une connexion câblée pour la première tentative d’ouverture de session à l’aide d’ordinateurs qui sont déjà joints au domaine.

Pour joindre un ordinateur au domaine à l’aide d’une connexion sans fil ou pour que les utilisateurs se connectent au domaine la première fois à\-l’aide d’un ordinateur joint à un domaine et d’une connexion sans fil, les clients sans fil doivent d’abord établir une connexion au réseau sans fil sur un segment qui a accès aux contrôleurs de domaine réseau à l’aide de l’une des méthodes suivantes.

1. **Un membre du personnel informatique joint un ordinateur sans fil au domaine, puis configure un profil de démarrage sans fil d’authentification unique.** Avec cette méthode, un administrateur informatique connecte l’ordinateur sans fil au réseau Ethernet câblé, puis joint l’ordinateur au domaine. Ensuite, l’administrateur distribue l’ordinateur à l’utilisateur. Lorsque l’utilisateur démarre l’ordinateur, les informations d’identification de domaine qu’il spécifie manuellement pour le processus d’ouverture de session de l’utilisateur sont utilisées pour établir une connexion au réseau sans fil et ouvrir une session sur le domaine.  

2. **L’utilisateur configure manuellement l’ordinateur sans fil avec le profil de démarrage sans fil, puis joint le domaine.** Avec cette méthode, les utilisateurs configurent manuellement leurs ordinateurs sans fil avec un profil de démarrage sans fil en fonction des instructions d’un administrateur informatique. Le profil de démarrage sans fil permet aux utilisateurs d’établir une connexion sans fil, puis de joindre l’ordinateur au domaine. Une fois l’ordinateur joint au domaine et le redémarrage de l’ordinateur, l’utilisateur peut ouvrir une session sur le domaine à l’aide d’une connexion sans fil et des informations d’identification de son compte de domaine.

Pour déployer un accès sans fil, consultez [déploiement de l’accès sans fil](e-wireless-access-deployment.md).
