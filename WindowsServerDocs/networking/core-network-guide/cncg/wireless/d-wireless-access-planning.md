---
title: Accès sans fil planification du déploiement
description: Cette rubrique fait partie du guide de mise en réseau de Windows Server2016 «Accès sans fil authentifié 802. 1-mot de passe de déployer X»
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 8c632d02-2270-4a82-8fc4-74ea3747f079
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a1aa7d9fa66c480988ec7e3a97447157bd3eab9c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="wireless-access-deployment-planning"></a>Accès sans fil planification du déploiement

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Avant de déployer l’accès sans fil, vous devez planifier les éléments suivants:

- Installation de l’accès sans fil points \(APs\) sur votre réseau

- Accès et la configuration du client sans fil

Les sections suivantes fournissent plus d’informations sur ces étapes de planification.

## <a name="planning-wireless-ap-installations"></a>Planification des installations de point d’accès sans fil
Lorsque vous concevez votre solution d’accès réseau sans fil, vous devez procédez comme suit:

1. Déterminer quelles normes vos points d’accès sans fil doivent prendre en charge.
2. Déterminer les zones de couverture où vous voulez fournir des services sans fil
3. Déterminer où vous souhaitez localiser des points d’accès sans fil

En outre, vous devez planifier un modèle d’adresse IP de votre point d’accès sans fil et clients sans fil. Consultez la section **planifier la configuration de fil dans NPS** ci-dessous pour les informations relatives à.

### <a name="verify-wireless-ap-support-for-standards"></a>Vérifier la prise en charge de point d’accès sans fil des normes
Pour les besoins de la cohérence et la simplicité de déploiement et la gestion de point d’accès, il est recommandé que vous déployez des points d’accès sans fil de la même marque et le modèle.

Les points d’accès sans fil que vous déployez doivent prendre en charge les éléments suivants:

- **IEEE 802. 1 X**

- **Authentification RADIUS**

- **L’authentification sans fil et le chiffrement.** Répertoriées par ordre de plus au moins préféré:

    1.  WPA2\-entreprise avec AES

    2.  WPA2\-entreprise avec TKIP

    3.  WPA\-entreprise avec AES

    4.  WPA\-entreprise avec TKIP

>[!NOTE]
>Pour déployer le WPA2, vous devez utiliser des cartes réseau sans fil et les points d’accès sans fil qui prennent également en charge WPA2. Sinon, utilisez WPA\ à l’entreprise.

En outre, pour améliorer la sécurité pour le réseau, les points d’accès sans fil doivent prendre en charge les options de sécurité suivantes:

- **Filtrage de DHCP.** Le point d’accès sans fil doit filtrer sur les ports IP pour empêcher la transmission des messages de diffusion DHCP dans les cas dans lesquels le client sans fil est configuré comme un serveur DHCP. Le point d’accès sans fil doit empêcher le client d’envoyer des paquets IP depuis le port UDP 68 au réseau.

- **Filtrage de DNS.** Le point d’accès sans fil doit filtrer sur les ports IP pour empêcher un client d’exécuter en tant qu’un serveur DNS. Le point d’accès sans fil doit empêcher le client d’envoyer des paquets IP à partir de TCP ou UDP port 53 au réseau.

- **Isolation de client** si votre point d’accès sans fil fournit des fonctionnalités d’isolement client, vous devez activer la fonctionnalité pour empêcher possible Address Resolution Protocol \(ARP\) l’usurpation des attaques.

### <a name="identify-areas-of-coverage-for-wireless-users"></a>Identifier les zones de couverture des utilisateurs sans fil
Utiliser des plans de chaque étage pour chaque bâtiment pour identifier les zones où vous souhaitez fournir une couverture sans fil. Par exemple, identifier les bureaux approprié, salles de conférence, halls, cafétérias ou complexe.

Sur les schémas, indiquer tous les périphériques d’interférer avec les signaux sans fil, tels que des équipements médicaux, des caméras vidéo sans fil, les téléphones sans fil qui fonctionnent dans le 2.4 par le biais de plage \(ISM\) industrielle GHz, scientifique et médical 2.5 et périphériques Bluetooth\.

Sur le dessin, marquer les aspects de la création risquent d’interférer avec les signaux sans fil; les objets métalliques utilisés dans la construction d’un immeuble peuvent affecter le signal sans fil. Par exemple, les objets courants suivants peuvent interférer avec la propagation de signal: ascenseurs conduits chauffage et air\-conditionnement et hiloires concrets de prise en charge.

Consultez le fabricant de votre point d’accès pour plus d’informations sur les sources qui peut entraîner l’atténuation de fréquence radio de point d’accès sans fil. La plupart des points d’accès fournissent des logiciels de test que vous pouvez utiliser pour vérifier pour la force du signal, le taux d’erreur et le débit des données.

### <a name="determine-where-to-install-wireless-aps"></a>Déterminer où installer les points d’accès sans fil
Sur les plans, localisez vos points d’accès sans fil fermer suffisamment ensemble pour fournir une couverture sans fil ample mais suffisamment loin des doigts qu’ils n’interfèrent pas avec eux.

La distance entre les points d’accès nécessaire dépend du type de point d’accès et les aspects de la création de bloquer, antenne de point d’accès sans fil signaux et autres sources d’interférences. Vous pouvez marquer les emplacements de point d’accès sans fil afin que chaque point d’accès sans fil n’est pas plus de 300pieds à partir de n’importe quel point d’accès sans fil adjacente. Consultez la documentation du fabricant du point d’accès sans fil pour les spécifications du point d’accès et les recommandations en matière de sélection élective.

Installer temporairement les points d’accès sans fil dans les emplacements spécifiés sur vos plans. Ensuite, à l’aide d’un ordinateur portable équipé d’un adaptateur sans fil 802.11 et le logiciel d’étude de site qui est généralement fourni avec les cartes sans fil, déterminez la puissance du signal dans chaque zone de couverture.

Dans les zones de couverture où la puissance du signal est faible, placez le point d’accès pour améliorer la puissance du signal pour la zone de couverture, installer des points d’accès sans fil supplémentaires pour fournir la couverture nécessaire, déplacer ou supprimer des sources d’interférences.  

Mettre à jour vos plans pour indiquer la position finale de tous les points d’accès sans fil. Avoir un plan de sélection élective PA précis aidera ultérieurement au cours de la résolution des problèmes d’opérations ou lorsque vous souhaitez mettre à niveau ou remplacer des points d’accès.

### <a name="plan-wireless-ap-and-nps-radius-client-configuration"></a>Planifier la configuration de point d’accès et le serveur NPS RADIUS Client sans fil
Vous pouvez utiliser NPS pour configurer les points d’accès sans fil individuellement ou en groupes.

Si vous déployez un réseau sans fil volumineux qui inclut de nombreux points d’accès, il est beaucoup plus facile de configurer des points d’accès dans des groupes. Pour ajouter les points d’accès en tant que groupes de clients RADIUS dans NPS, vous devez configurer les points d’accès avec ces propriétés.

- Les points d’accès sans fil sont configurés avec des adresses IP à partir de la même plage d’adresses IP.

- Les points d’accès sans fil sont configurés avec le secret partagé.

### <a name="plan-the-use-of-peap-fast-reconnect"></a>Planifier l’utilisation de la reconnexion rapide PEAP
Dans une infrastructure X 802. 1, les points d’accès sans fil sont configurés en tant que clients RADIUS à des serveurs RADIUS. Lorsque la reconnexion rapide PEAP est déployé, un client qui se déplace entre deux ou plusieurs points d’accès sans fil n’est pas obligé d’être authentifié à chaque nouvelle association.

Reconnexion rapide PEAP réduit le temps de réponse pour l’authentification entre le client et l’authentificateur car la demande d’authentification est transférée du nouveau point d’accès au serveur NPS qui effectuées initialement authentification et autorisation pour la demande de connexion client.

Étant donné que le client PEAP et le serveur NPS utilisent tous deux précédemment mis en cache les propriétés de connexion de Transport Layer Security \(TLS\) \ (le regroupement qui est appelé le handle\ TLS), le serveur NPS peut déterminer rapidement que le client est autorisé pour une reconnexion.

>[!IMPORTANT]
>Pour rapide se reconnecter pour fonctionner correctement, les points d’accès doivent être configurés en tant que clients RADIUS du serveur NPS même.

Si le serveur NPS d’origine n’est pas disponible, ou si le client se déplace vers un point d’accès qui est configuré comme client RADIUS vers un autre serveur RADIUS, l’authentification complète doit avoir lieu entre le client et le nouvel authentificateur.

### <a name="wireless-ap-configuration"></a>Configuration de point d’accès sans fil
La liste suivante récapitule les éléments généralement configurés sur 802.1X\-points d’accès sans fil compatibles:

> [!NOTE]
> Les noms des éléments peuvent varier selon la marque et le modèle et peuvent être différentes de celles de la liste suivante. Consultez la documentation de votre point d’accès sans fil pour plus d’informations spécifiques à configuration\.

- **Identificateur de jeu de service \(SSID\)**. C’est le nom du réseau sans fil \ (par exemple, ExampleWlan\) et le nom est publié auprès des clients sans fil. Pour réduire les risques de confusion, le SSID que vous choisissez de publier ne doit pas correspondre à l’identificateur SSID diffusées par des réseaux sans fil qui se trouvent dans la plage de réception de votre réseau sans fil.

    Dans les cas dans lesquels plusieurs points d’accès sans fil sont déployés dans le cadre du même réseau sans fil, configurez chaque point d’accès sans fil avec le même SSID. Dans les cas dans lesquels plusieurs points d’accès sans fil sont déployés dans le cadre du même réseau sans fil, configurez chaque point d’accès sans fil avec le même SSID.  

    Dans les cas où vous avez besoin pour déployer des différents réseaux sans fil pour répondre aux besoins spécifiques, votre PA sans fil sur un réseau doit diffuser un SSID différent que le SSID vos autres network\(s\). Par exemple, si vous avez besoin d’un réseau sans fil distinct pour vos employés et les invités, vous pouvez configurer vos points d’accès sans fil pour le réseau d’entreprise avec le SSID configuré pour diffuser **ExampleWLAN**. Pour le réseau invité, vous pourriez définir ensuite SSID de chaque point d’accès sans fil pour diffuser **GuestWLAN**. De cette façon vos employés et les invités peuvent se connecter au réseau voulu sans confusion inutile.  

    > [!TIP]  
    > Certains points d’accès sans fil ont la possibilité de diffuser plusieurs SSID pour prendre en charge des déploiements réseau prennent. Sans fil points d’accès que vous pouvez diffuser plusieurs identificateurs SSID peut réduire les coûts de maintenance opérationnelle et de déploiement.  

- **Sans fil de l’authentification et le chiffrement**.

    L’authentification sans fil est l’authentification de sécurité qui est utilisée lorsque le client sans fil associe à un point d’accès sans fil.  

    Le chiffrement sans fil est le chiffrement de sécurité qui est utilisé avec l’authentification sans fil pour protéger les communications sont envoyées entre le point d’accès sans fil et le client sans fil.  

- **Sans fil PA IP adresse \(static\)**. Sur chaque point d’accès sans fil, configurez une adresse IP statique unique. Si le sous-réseau est pris en charge par un serveur DHCP, assurez-vous que toutes les adresses IP du point d’accès se situent dans une plage d’exclusion DHCP afin que le serveur DHCP n’essaie pas de problème de la même adresse IP à un autre ordinateur ou périphérique. Plages d’exclusion sont documentés dans la procédure «pour créer et activer une nouvelle étendue DHCP» dans la [Guide réseau de base](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide). Si vous prévoyez de configurer des points d’accès en tant que clients RADIUS en groupe dans NPS, chaque point d’accès dans le groupe doit avoir une adresse IP à partir de la même plage d’adresses IP.

- **Nom DNS**. Certains points d’accès sans fil peuvent être configurés avec un nom DNS. Configurez chaque point d’accès sans fil avec un nom unique. Par exemple, si vous avez un points d’accès sans fil déployé dans un immeuble prennent, vous pouvez nommer les trois premiers points d’accès sans fil sont déployés sur le troisième étage AP3\-01, AP3\-02 et AP3\-03.

- **Masque de sous-réseau de point d’accès sans fil**. Configurer le masque de désigner quelle partie de l’adresse IP adresse est l’ID de réseau et la partie de l’adresse IP est l’ordinateur hôte.

- **Service DHCP PA**. Si votre point d’accès sans fil a un service DHCP intégrée, désactivez-la.

- **Secret partagé RADIUS**. Utilisez un rayon unique les secret partagé pour chaque point d’accès sans fil, sauf si vous envisagez de configurer des clients RADIUS du serveur NPS dans les groupes - dans quels cas vous devez configurer tous les points d’accès dans le groupe avec le secret partagé. Les secrets partagés doivent être une séquence aléatoire d’au moins 22caractères, avec les majuscules et minuscules, des chiffres et des signes de ponctuation. Pour garantir le caractère aléatoire, vous pouvez utiliser un programme de génération de caractères aléatoires pour créer des secrets partagés. Il est recommandé d’enregistrer le secret partagé pour chaque point d’accès sans fil et de le stocker dans un emplacement sécurisé, comme un bureau sécurisé. Lorsque vous configurez des clients RADIUS dans la console NPS, vous allez créer une version virtuelle de chaque point d’accès. Le secret partagé que vous configurez sur chaque point d’accès virtuel dans NPS doit correspondre le secret partagé sur le point d’accès réel, physique.

- **Adresse IP du serveur RADIUS**. Tapez l’adresse IP du serveur NPS que vous souhaitez utiliser pour authentifier et autoriser les demandes de connexion à ce point d’accès.

- **UDP port\(s\)**. Par défaut, NPS utilise les ports UDP1812 et 1645 pour les messages d’authentification RADIUS et les ports UDP1813 et 1646 pour les messages de gestion de comptes RADIUS. Il est recommandé de ne pas modifier les paramètres de ports UDP RADIUS par défaut.

- **Au**. Certains points d’accès sans fil nécessitent \(VSAs\) des attributs spécifiques vendor\ pour fournir une fonctionnalité de point d’accès complet sans fil.

- **Filtrage DHCP**. Configurer des points d’accès sans fil pour bloquer des clients sans fil d’envoyer des paquets IP depuis le port UDP 68 au réseau. Consultez la documentation de votre point d’accès sans fil configurer le filtrage de DHCP.

- **Filtrage DNS**. Configurer des points d’accès sans fil pour bloquer des clients sans fil d’envoyer des paquets IP à partir de port TCP ou UDP 53 au réseau. Consultez la documentation de votre point d’accès sans fil configurer le filtrage de DNS.

## <a name="planning-wireless-client-configuration-and-access"></a>Planification de l’accès et la configuration du client sans fil

Lorsque vous planifiez le déploiement de 802.1X\-authentifiés l’accès sans fil, vous devez prendre en compte plusieurs facteurs spécifiques de client:

- **Planification de la prise en charge des normes plusieurs**.

    Déterminez si vos ordinateurs sans fil utilisent tous la même version de Windows ou si elles sont un mélange d’ordinateurs exécutant différents systèmes d’exploitation. S’ils sont différents, assurez-vous de comprendre les différences dans les normes pris en charge par les systèmes d’exploitation.

    Déterminer si toutes les cartes réseau sans fil sur tous les ordinateurs clients sans fil prennent en charge les mêmes normes sans fil, ou si vous devez prendre en charge des normes. Par exemple, déterminez si certains pilotes de matériel de carte réseau prend en charge WPA2\-Enterprise et AES, tandis que d’autres prennent en charge uniquement WPA\-Enterprise et TKIP.

- **Mode d’authentification client planification**. Modes d’authentification permet de définir comment les clients Windows traitent les informations d’identification de domaine. Vous pouvez sélectionner parmi les modes d’authentification trois réseau suivants dans les stratégies de réseau sans fil.  

    1. **D’authentification utilisateur re\**. Ce mode indique que l’authentification est toujours effectuée à l’aide des informations d’identification de sécurité basées sur l’état actuel de l’ordinateur. Si aucun utilisateurs ne sont connectés à l’ordinateur, l’authentification est effectuée en utilisant les informations d’identification de l’ordinateur. Lorsqu’un utilisateur est connecté à l’ordinateur, l’authentification est toujours effectuée en utilisant les informations d’identification de l’utilisateur.  

    2. **Ordinateur uniquement**. Ordinateur uniquement en mode spécifie que l’authentification est toujours effectué en utilisant uniquement les informations d’identification de l’ordinateur.  

    3.  **L’authentification utilisateur**. Mode d’authentification utilisateur spécifie que l’authentification est effectuée uniquement lorsque l’utilisateur est connecté à l’ordinateur. Lorsqu’il n’y a pas d’utilisateurs connectés à l’ordinateur, les tentatives d’authentification ne sont pas effectuées.  

- **Planification des restrictions sans fil**. Déterminez si vous souhaitez fournir tous les utilisateurs sans fil avec le même niveau d’accès à votre réseau sans fil, ou si vous souhaitez restreindre l’accès pour certains de vos utilisateurs sans fil. Vous pouvez appliquer des restrictions dans NPS par rapport à certains groupes d’utilisateurs sans fil.  Par exemple, vous pouvez définir des jours et heures que certains groupes sont autorisés à accéder au réseau sans fil.  

- **Planification des méthodes pour l’ajout de nouveaux ordinateurs sans fil**. Pour les ordinateurs compatibles wireless\ qui sont joints à votre domaine avant de déployer votre réseau sans fil, si l’ordinateur est connecté à un segment de réseau câblé qui n’est pas protégé par 802. 1 X, les paramètres de configuration sans fil sont automatiquement appliquées après la configuration de réseau sans fil \ IEEE (802.11\) stratégies sur le contrôleur de domaine et une fois que la stratégie de groupe est actualisée sur le client sans fil.  

    Pour les ordinateurs qui ne sont pas déjà joint à votre domaine, toutefois, vous devez planifier une méthode pour appliquer les paramètres qui sont requis pour 802.1X\-accès authentifié. Par exemple, déterminez si vous souhaitez joindre l’ordinateur au domaine à l’aide d’une des méthodes suivantes.

    1.  Connectez l’ordinateur à un segment de réseau câblé qui n’est pas protégé par 802. 1 X, puis joindre l’ordinateur au domaine.

    2.  Fournir à vos utilisateurs sans fil avec les étapes et les paramètres dont ils ont besoin d’ajouter leur propres profil sans fil de démarrage, qui permet de joindre l’ordinateur au domaine.

    3.  Affecter le personnel informatique pour joindre les clients sans fil au domaine.

### <a name="planning-support-for-multiple-standards"></a>Planification de la prise en charge des normes plusieurs

Le réseau sans fil \ IEEE (802.11\) l’extension des stratégies dans la stratégie de groupe offre un large éventail d’options de configuration pour prendre en charge une variété d’options de déploiement.

Vous pouvez déployer des points d’accès sans fil qui sont configurés avec les normes que vous souhaitez prendre en charge et ensuite configurer plusieurs profils sans fil dans le réseau sans fil \ IEEE (802.11\), les stratégies avec chaque profil spécifiant un ensemble de normes dont vous avez besoin.

Par exemple, si votre réseau comporte des ordinateurs sans fil qui prennent en charge WPA2\-Enterprise et AES, les autres ordinateurs qui prennent en charge WPA\-Enterprise et AES et les autres ordinateurs qui prennent en charge uniquement WPA\-Enterprise et TKIP, vous devez déterminer si vous souhaitez:

- Configurer un seul profil pour prendre en charge tous les ordinateurs sans fil à l’aide de la méthode de chiffrement plus faible que tous vos ordinateurs prennent en charge - dans ce cas, WPA\-Enterprise et TKIP.  
- Configurer deux profils pour fournir la meilleure sécurité possible est pris en charge par chaque ordinateur sans fil. Dans ce cas, vous pouvez configurer un profil qui spécifie le chiffrement plus puissant \ (WPA2\-Enterprise et AES\) et un profil qui utilise le chiffrement WPA\-Enterprise et TKIP plus faible. Dans cet exemple, il est essentiel que vous placez le profil qui utilise WPA2\-Enterprise et AES plus élevé dans l’ordre de préférence. Les ordinateurs qui ne sont pas capables de recourir WPA2\-Enterprise et AES ignorer pour le profil dans l’ordre de préférence suivant et traiter le profil qui spécifie WPA\-Enterprise et TKIP automatiquement.

> [!IMPORTANT]
> Vous devez placer le profil avec les normes plus sécurisées supérieur dans la liste des profils, car la connexion des ordinateurs utilisent le premier profil qu’ils sont capables de l’utilisation.

### <a name="planning-restricted-access-to-the-wireless-network"></a>Planification de l’accès restreint au réseau sans fil

Dans de nombreux cas, vous souhaiterez peut-être fournir aux utilisateurs sans fil différents niveaux d’accès au réseau sans fil. Par exemple, vous souhaiterez peut-être permettre à certains utilisateurs un accès illimité, toutes les heures de la journée, tous les jours de la semaine. Pour d’autres utilisateurs, vous devrez uniquement autoriser l’accès pendant les heures de core, du lundi au vendredi et de refuser l’accès samedi et le dimanche.

Ce guide fournit des instructions pour créer un environnement d’accès qui place tous les utilisateurs sans fil dans un groupe commun accès aux ressources sans fil. Vous créez un groupe de sécurité utilisateurs sans fil dans le répertoire utilisateurs et ordinateurs Active enfichable et puis rendez chaque utilisateur pour lequel vous souhaitez accorder l’accès sans fil un membre de ce groupe.

Lorsque vous configurez des stratégies de réseau NPS, vous spécifiez le groupe de sécurité utilisateurs sans fil en tant que l’objet qui traite les NPS lors de la détermination d’autorisation.

Toutefois, si votre déploiement requiert la prise en charge pour les différents niveaux d’accès vous devez effectuer uniquement les éléments suivants:  

1. Créer plusieurs groupes de sécurité utilisateurs sans fil pour créer des groupes de sécurité sans fil supplémentaires dans ActiveDirectory Users and Computers. Par exemple, vous pouvez créer un groupe qui contient les utilisateurs qui ont un accès complet, un groupe pour ceux qui ont uniquement accès pendant les heures de travail et autres groupes correspondant aux autres critères qui correspondent à vos besoins.

2. Ajoutez des utilisateurs aux groupes de sécurité appropriés que vous avez créé.

3. Configurer des stratégies de réseau NPS supplémentaires pour chaque groupe de sécurité sans fil supplémentaires et configurer les stratégies pour appliquer des conditions et des contraintes dont vous avez besoin pour chaque groupe.

### <a name="planning-methods-for-adding-new-wireless-computers"></a>Planification des méthodes pour l’ajout de nouveaux ordinateurs sans fil

La méthode préférée pour joindre les nouveaux ordinateurs sans fil au domaine, puis ouvrez une session sur le domaine est à l’aide d’une connexion câblée à un segment de réseau local qui a accès aux contrôleurs de domaine et n’est pas protégé par un 802. 1 X commutateur Ethernet d’authentification.

Dans certains cas, toutefois, il ne peut pas être pratique d’utiliser une connexion câblée à joindre des ordinateurs au domaine, ou pour un utilisateur à utiliser une connexion câblée pour leur première ouverture de session à l’aide d’ordinateurs qui sont déjà joint au domaine.

Pour joindre un ordinateur au domaine à l’aide d’une connexion sans fil ou pour les utilisateurs à ouvrir une session sur le domaine la première fois à l’aide d’un ordinateur appartenant à un domain\ et une connexion sans fil, les clients sans fil doivent d’abord établir une connexion au réseau sans fil sur un segment qui a accès aux contrôleurs de domaine de réseau en utilisant l’une des méthodes suivantes.

1. **Un membre du personnel informatique joint un ordinateur sans fil au domaine, puis configure un session unique d’amorçage profil sans fil.** Avec cette méthode, un administrateur connecte l’ordinateur sans fil au réseau Ethernet câblé et puis joint l’ordinateur au domaine. L’administrateur distribue ensuite l’ordinateur à l’utilisateur. Lorsque l’utilisateur démarre l’ordinateur, les informations d’identification de domaine qu’ils spécifient manuellement pour le processus d’ouverture de session utilisateur servent à établir une connexion au réseau sans fil et ouvrez une session sur le domaine.  

2. **L’utilisateur configure manuellement ordinateur sans fil avec le profil sans fil d’amorçage et puis rejoint le domaine.** Avec cette méthode, les utilisateurs configurer manuellement leurs ordinateurs sans fil avec un profil sans fil d’amorçage en fonction des instructions à partir d’un administrateur informatique. Le profil sans fil d’amorçage permet aux utilisateurs d’établir une connexion sans fil et puis joindre l’ordinateur au domaine. Après avoir joint l’ordinateur au domaine et le redémarrage de l’ordinateur, l’utilisateur peut se connecter le domaine à l’aide d’une connexion sans fil et leurs informations d’identification du compte de domaine.

Pour déployer l’accès sans fil, consultez [déploiement de l’accès sans fil](e-wireless-access-deployment.md).
