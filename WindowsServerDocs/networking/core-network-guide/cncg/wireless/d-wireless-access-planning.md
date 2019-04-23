---
title: Planification du déploiement de l’accès sans fil
description: Cette rubrique fait partie de la « Accès sans fil authentifié 802. 1 mot de passe de déployer X » du guide de mise en réseau de Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 8c632d02-2270-4a82-8fc4-74ea3747f079
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a2571f509fbbca8384e626ad3c8c13f1c0a50400
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855460"
---
# <a name="wireless-access-deployment-planning"></a>Planification du déploiement de l’accès sans fil

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Avant de déployer l’accès sans fil, vous devez planifier les éléments suivants :

- Installation des points d’accès sans fil \(APs\) sur votre réseau

- Accès et configuration de client sans fil

Les sections suivantes fournissent des détails sur ces étapes de planification.

## <a name="planning-wireless-ap-installations"></a>Planification des installations de point d’accès sans fil
Lorsque vous concevez votre solution d’accès réseau sans fil, vous devez procédez comme suit :

1. Déterminer quelles normes vos points d’accès sans fil doivent prendre en charge.
2. Déterminer les domaines de couverture où vous souhaitez fournir des services sans fil
3. Déterminez où vous souhaitez localiser des points d’accès sans fil

En outre, vous devez planifier un modèle d’adresse IP de votre point d’accès sans fil et clients sans fil. Consultez la section **planifiez la configuration de sans fil AP dans NPS** ci-dessous pour plus d’informations.

### <a name="verify-wireless-ap-support-for-standards"></a>Vérifier la prise en charge de point d’accès sans fil des normes
Pour les besoins de cohérence et une facilité de déploiement et la gestion de l’Asie-Pacifique, il est recommandé que vous déployez des points d’accès sans fil de la même marque et le modèle.

Les points d’accès sans fil que vous déployez doivent prendre en charge les éléments suivants :

- **IEEE 802. 1 X**

- **Authentification RADIUS**

- **L’authentification sans fil et chiffrement.** Énumérés du plus informatif au moins préféré :

    1.  WPA2\-Enterprise avec AES

    2.  WPA2\-Enterprise avec TKIP

    3.  WPA\-Enterprise avec AES

    4.  WPA\-Enterprise avec TKIP

>[!NOTE]
>Pour déployer WPA2, vous devez utiliser des cartes réseau sans fil et les points d’accès sans fil qui prennent également en charge WPA2. Sinon, utiliser WPA\-Enterprise.

En outre, pour améliorer la sécurité pour le réseau, les points d’accès sans fil doivent prendre en charge les options de sécurité suivantes :

- **Filtrage de DHCP.** Le point d’accès sans fil doit filtrer sur les ports IP pour empêcher la transmission de messages de diffusion DHCP dans les cas dans lesquels le client sans fil est configuré comme un serveur DHCP. Le point d’accès sans fil doit bloquer le client d’envoyer des paquets IP à partir du port UDP 68 au réseau.

- **Filtrage de DNS.** Le point d’accès sans fil doit filtrer sur les ports IP pour empêcher un client à partir de l’exécution d’un serveur DNS. Le point d’accès sans fil doit bloquer le client d’envoyer des paquets IP à partir de TCP ou UDP port 53 au réseau.

- **Isolation du client** si votre point d’accès sans fil fournit des fonctionnalités d’isolation de client, vous devez activer la fonctionnalité éviter les possibles Address Resolution Protocol \(ARP\) l’usurpation des attaques.

### <a name="identify-areas-of-coverage-for-wireless-users"></a>Identifier les zones de couverture des utilisateurs sans fil
Utilisez des plans d’architecte de chaque étage de chaque bâtiment pour identifier les zones où vous souhaitez fournir une couverture sans fil. Par exemple, identifier les bureaux approprié, de salles de conférences, halls, cafétérias ou complexe.

Sur les dessins, indiquez tous les périphériques qui interfèrent avec les signaux sans fil, tels que les équipements médicaux, les caméras vidéo sans fil, des téléphones sans fil qui opèrent dans le 2.4 à 2,5 GHz industriel, scientifique et médical \(ISM\) plage et Bluetooth\-appareils compatibles.

Sur le dessin, marquer les aspects de la génération peut interférer avec les signaux sans fil ; les objets métalliques utilisés dans la construction d’un bâtiment peuvent affecter le signal sans fil. Par exemple, les objets communs suivants peuvent interférer avec la propagation du signal : Ascenseurs, le chauffage et air\-conditionnement conduits et prise en charge concrète hiloires.

Reportez-vous à votre point d’accès du fabricant pour plus d’informations sur les sources susceptibles de provoquer une atténuation RADIOFRÉQUENCE AP sans fil. La plupart des points d’accès fournissent des logiciels de test que vous pouvez utiliser pour vérifier pour la puissance du signal, taux d’erreur et le débit de données.

### <a name="determine-where-to-install-wireless-aps"></a>Déterminer où installer les points d’accès sans fil
Sur les plans, localisez vos points d’accès sans fil Fermez suffisamment ensemble pour fournir une couverture de sans fil suffisamment mais suffisamment loin les unes des autres qu’ils n’interfèrent pas entre eux.

La distance nécessaire entre les points d’accès varie selon le type de point d’accès et antenne de point d’accès, les aspects du bâtiment qui bloquent sans fil signaux et autres sources d’interférence. Vous pouvez marquer des emplacements de point d’accès sans fil afin que chaque point d’accès sans fil n’est pas plus de 300 pieds à partir de n’importe quel point d’accès sans fil adjacent. Consultez la documentation du fabricant du point d’accès sans fil pour les spécifications de point d’accès et des instructions pour la sélection élective.

Installer temporairement les points d’accès sans fil dans les emplacements spécifiés dans vos plans. Ensuite, à l’aide d’un ordinateur portable équipé d’une carte réseau sans fil 802.11 et le logiciel d’étude de site qui est généralement fourni avec les cartes sans fil, déterminez la puissance du signal dans chaque zone de couverture.

Dans les zones de couverture où la puissance du signal est faible, placez le point d’accès pour améliorer la puissance du signal pour la zone de couverture, installer des points d’accès sans fil supplémentaires pour fournir la couverture nécessaires, déplacer ou supprimer des sources d’interférence de signal.  

Mettre à jour vos plans pour indiquer la position finale de tous les points d’accès sans fil. Avoir un plan de sélection élective AP précis aidera ultérieurement pendant le dépannage des opérations ou lorsque vous souhaitez mettre à niveau ou remplacer les points d’accès.

### <a name="plan-wireless-ap-and-nps-radius-client-configuration"></a>Planifier la configuration de point d’accès et le serveur NPS RADIUS Client sans fil
Vous pouvez utiliser le serveur NPS pour configurer des points d’accès sans fil individuellement ou en groupes.

Si vous déployez un grand réseau sans fil qui inclut de nombreux points d’accès, il est beaucoup plus facile de configurer des points d’accès dans des groupes. Pour ajouter les points d’accès en tant que groupes de clients RADIUS dans NPS, vous devez configurer les points d’accès avec ces propriétés.

- Les points d’accès sans fil sont configurés avec des adresses IP à partir de la même plage d’adresses IP.

- Les points d’accès sans fil sont configurés avec le même secret partagé.

### <a name="plan-the-use-of-peap-fast-reconnect"></a>Planifiez l’utilisation de la reconnexion rapide PEAP
Dans une infrastructure X 802. 1, les points d’accès sans fil sont configurés en tant que clients RADIUS pour les serveurs RADIUS. Lorsque la reconnexion rapide PEAP est déployé, un client sans fil qui se déplace entre deux ou plusieurs points d’accès n’est pas nécessaire pour être authentifié avec chaque nouvelle association.

La reconnexion rapide PEAP réduit le temps de réponse pour l’authentification entre le client et l’authentificateur, car la demande d’authentification est transférée du nouveau point d’accès au serveur NPS qui effectuée à l’origine de l’authentification et l’autorisation pour le client demande de connexion.

Étant donné que le client PEAP et le serveur NPS utilisent tous deux précédemment mis en cache de Transport Layer Security \(TLS\) propriétés de connexion \(la collection qui porte le handle TLS\), le serveur NPS peut déterminer rapidement que le client est autorisé pour une reconnexion.

>[!IMPORTANT]
>Pour rapidement vous reconnecter pour fonctionner correctement, les points d’accès doivent être configurés en tant que clients RADIUS du serveur NPS même.

Si le serveur NPS d’origine devient indisponible, ou si le client se déplace vers un point d’accès qui est configuré comme client RADIUS vers un autre serveur RADIUS, l’authentification complète doit se produire entre le client et l’authentificateur de nouveau.

### <a name="wireless-ap-configuration"></a>Configuration de point d’accès sans fil
La liste suivante résume les éléments couramment configurés sur 802. 1 X\-des points d’accès sans fil compatibles :

> [!NOTE]
> Les noms d’élément peuvent varier par marque et modèle et peuvent différer de celles figurant dans la liste suivante. Consultez la documentation de votre point d’accès sans fil pour la configuration\-des détails spécifiques.

- **Identificateur service set \(SSID\)**. Il s’agit du nom du réseau sans fil \(, par exemple, ExampleWlan\)et le nom qui est publié auprès des clients sans fil. Pour éviter la confusion, le SSID que vous choisissez doit être publié ne doit pas correspondre le SSID est diffusé par des réseaux sans fil qui se trouvent dans la plage de réception de votre réseau sans fil.

    Dans les cas où plusieurs points d’accès sans fil sont déployés en tant que partie du même réseau sans fil, configurez chaque point d’accès sans fil avec le même SSID. Dans les cas où plusieurs points d’accès sans fil sont déployés en tant que partie du même réseau sans fil, configurez chaque point d’accès sans fil avec le même SSID.  

    Dans les cas où vous avez besoin pour déployer les différents réseaux sans fil pour répondre aux besoins spécifiques, votre AP sans fil sur un réseau doit diffuser un SSID différent que le SSID vos autres réseaux\(s\). Par exemple, si vous avez besoin d’un réseau sans fil distinct pour vos employés et les invités, vous pouvez configurer vos points d’accès sans fil pour le réseau d’entreprise avec l’identificateur SSID de valeur pour diffuser **ExampleWLAN**. Pour votre réseau d’invité, vous pouvez ensuite définir les SSID de chaque point d’accès sans fil pour diffuser **GuestWLAN**. De cette façon vos employés et les invités peuvent se connecter au réseau voulu sans risque de confusion inutile.  

    > [!TIP]  
    > Certains points d’accès sans fil ont la possibilité de diffuser plusieurs SSID pour prendre en charge de plusieurs\-réseau des déploiements. AP sans fil qui peut diffuser de plusieurs SSID peut réduire les coûts de déploiement et d’opérations de maintenance.  

- **Authentification et chiffrement sans fil**.

    L’authentification sans fil est l’authentification de sécurité qui est utilisée lorsque le client sans fil s’associe avec un point d’accès sans fil.  

    Le chiffrement sans fil est le code de chiffrement de sécurité qui est utilisé avec l’authentification sans fil pour protéger les communications sont envoyées entre le point d’accès sans fil et le client sans fil.  

- **Sans fil d’adresse IP du point d’accès \(statique\)**. Sur chaque point d’accès sans fil, configurer une adresse IP statique unique. Si le sous-réseau est pris en charge par un serveur DHCP, assurez-vous que toutes les adresses IP de point d’accès se situent dans une plage d’exclusion de DHCP afin que le serveur DHCP n’essaie pas de problème de la même adresse IP à un autre ordinateur ou périphérique. Plages d’exclusion sont documentées dans la procédure « pour créer et activer une nouvelle étendue DHCP » dans la [Guide du réseau](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide). Si vous projetez de configurer des points d’accès en tant que clients RADIUS par groupe dans NPS, chaque point d’accès dans le groupe doit avoir une adresse IP à partir de la même plage d’adresses IP.

- **Nom DNS**. Certains points d’accès sans fil peuvent être configurés avec un nom DNS. Configurez chaque point d’accès sans fil avec un nom unique. Par exemple, si vous avez un points d’accès sans fil déployés dans un multi\-récit de génération, vous pouvez nommer les trois premiers points d’accès sans fil sont déployés au troisième étage AP3\-01, AP3\-02 et AP3\-03.

- **Masque de sous-réseau AP sans fil**. Configurer le masque pour désigner la partie de l’adresse IP adresse est l’ID de réseau et la partie de l’adresse IP est l’hôte.

- **Service de point d’accès DHCP**. Si votre point d’accès sans fil intègre un\-dans le service DHCP, désactivez-la.

- **Secret partagé RADIUS**. Utilisez un rayon unique les secret partagé pour chaque point d’accès sans fil, sauf si vous projetez de configurer des clients RADIUS du serveur NPS dans des groupes - dans quels cas vous devez configurer tous les points d’accès dans le groupe avec le même secret partagé. Les secrets partagés doivent être une séquence aléatoire d’au moins 22 caractères, avec des lettres majuscules et minuscules, des chiffres et signes de ponctuation. Pour garantir le caractère aléatoire, vous pouvez utiliser un programme de génération de caractère aléatoire pour créer vos secrets partagés. Il est recommandé d’enregistrer le secret partagé pour chaque point d’accès sans fil et de le stocker dans un emplacement sécurisé, par exemple un bureau sécurisé. Lorsque vous configurez des clients RADIUS dans la console NPS, vous allez créer une version virtuelle de chaque point d’accès. Le secret partagé que vous configurez sur chaque point d’accès virtuel dans NPS doit correspondre à la clé secrète partagée sur le point d’accès réel, physique.

- **Adresse IP du serveur RADIUS**. Tapez l’adresse IP du serveur NPS que vous souhaitez utiliser pour authentifier et autoriser les demandes de connexion vers ce point d’accès.

- **Le port UDP\(s\)**. Par défaut, NPS utilise les ports UDP 1812 et 1645 pour les messages d’authentification RADIUS et les ports UDP 1813 et 1646 pour les messages de gestion des comptes RADIUS. Il est recommandé de ne pas modifier les paramètres par défaut des ports UDP RADIUS.

- **VSAs**. Certains points d’accès sans fil nécessitent fournisseur\-des attributs spécifiques \(VSA\) pour fournir des fonctionnalités de point d’accès complet sans fil.

- **Filtrage de DHCP**. Configurer des points d’accès sans fil pour bloquer des clients sans fil d’envoyer des paquets IP à partir du port UDP 68 au réseau. Consultez la documentation de votre point d’accès sans fil configurer le filtrage de DHCP.

- **Filtrage DNS**. Configurer des points d’accès sans fil pour bloquer des clients sans fil d’envoyer des paquets IP à partir du port TCP ou UDP 53 pour le réseau. Consultez la documentation de votre point d’accès sans fil configurer le filtrage de DNS.

## <a name="planning-wireless-client-configuration-and-access"></a>Planification des accès et la configuration du client sans fil

Lorsque vous planifiez le déploiement de 802. 1 X\-sans fil un accès authentifié, vous devez prendre en compte plusieurs client\-facteurs spécifiques :

- **Planification de la prise en charge de plusieurs normes**.

    Déterminer si vos ordinateurs sans fil utilisent tous la même version de Windows ou si elles sont un mélange d’ordinateurs exécutant différents systèmes d’exploitation. Si elles sont différentes, assurez-vous que vous comprenez les différences dans les normes prises en charge par les systèmes d’exploitation.

    Déterminer si toutes les cartes réseau sans fil sur tous les ordinateurs clients sans fil prennent en charge les mêmes normes sans fil, ou si vous avez besoin prendre en charge diverses normes. Par exemple, déterminez si certains pilotes de matériel de carte réseau prend en charge WPA2\-Enterprise et AES, tandis que d’autres prennent en charge WPA uniquement\-Enterprise et TKIP.

- **Le mode d’authentification client planification**. Modes d’authentification définissent la manière dont les clients Windows traitent les informations d’identification de domaine. Vous pouvez sélectionner dans les modes d’authentification trois réseau suivantes dans les stratégies de réseau sans fil.  

    1. **Utilisateur re\-authentification**. Ce mode spécifie que l’authentification est toujours effectuée à l’aide des informations d’identification de sécurité basées sur l’état actuel de l’ordinateur. Lorsque aucun utilisateurs ne sont connectés à l’ordinateur, l’authentification est effectuée en utilisant les informations d’identification de l’ordinateur. Lorsqu’un utilisateur est connecté à l’ordinateur, l’authentification est toujours effectuée en utilisant les informations d’identification de l’utilisateur.  

    2. **Ordinateur uniquement** Ordinateur uniquement en mode spécifie que l’authentification est toujours effectué en utilisant uniquement les informations d’identification de l’ordinateur.  

    3.  **Authentification utilisateur** Mode d’authentification utilisateur spécifie que l’authentification est effectuée uniquement lorsque l’utilisateur est connecté à l’ordinateur. Lorsqu’il n’y a aucun utilisateur connecté à l’ordinateur, les tentatives d’authentification ne sont pas effectuées.  

- **Planification des restrictions sans fil**. Déterminez si vous souhaitez fournir tous vos utilisateurs sans fil avec le même niveau d’accès à votre réseau sans fil, ou si vous souhaitez restreindre l’accès pour certains de vos utilisateurs sans fil. Vous pouvez appliquer des restrictions dans NPS par rapport à des groupes spécifiques d’utilisateurs sans fil.  Par exemple, vous pouvez définir des jours et heures que certains groupes sont autorisés à accéder au réseau sans fil.  

- **Planification des méthodes pour l’ajout de nouveaux ordinateurs sans fil**. Pour le sans fil\-capables ordinateurs joints à votre domaine avant de déployer votre réseau sans fil, si l’ordinateur est connecté à un segment de réseau câblé qui n’est pas protégé par 802. 1 X, les paramètres de configuration sans fil sont appliqué automatiquement après avoir configuré le réseau sans fil \(IEEE 802.11\) stratégies sur le contrôleur de domaine et une fois la stratégie de groupe est actualisée sur le client sans fil.  

    Pour les ordinateurs qui ne sont pas déjà joint à votre domaine, toutefois, vous devez planifier une méthode pour appliquer les paramètres qui sont requis pour 802. 1 X\-un accès authentifié. Par exemple, déterminez si vous souhaitez joindre l’ordinateur au domaine en utilisant l’une des méthodes suivantes.

    1.  Connectez l’ordinateur à un segment de réseau câblé qui n’est pas protégé par 802. 1 X, puis joignez l’ordinateur au domaine.

    2.  Fournir à vos utilisateurs sans fil avec les étapes et les paramètres dont ils ont besoin d’ajouter leur propre profil de démarrage sans fil, ce qui leur permet de joindre l’ordinateur au domaine.

    3.  Affecter le service informatique pour joindre les clients sans fil au domaine.

### <a name="planning-support-for-multiple-standards"></a>Planification de la prise en charge de plusieurs normes

Le réseau sans fil \(IEEE 802.11\) l’extension des stratégies dans la stratégie de groupe fournit un large éventail d’options de configuration pour prendre en charge une variété d’options de déploiement.

Vous pouvez déployer des points d’accès sans fil qui sont configurés avec les normes que vous souhaitez prendre en charge et ensuite configurer plusieurs profils sans fil dans le réseau sans fil \(IEEE 802.11\) stratégies, chaque profil en spécifiant un jeu de normes que vous avez besoin.

Par exemple, si votre réseau comporte des ordinateurs sans fil qui prennent en charge de WPA2\-Enterprise et AES, les autres ordinateurs qui prennent en charge WPA\-Enterprise et AES et autres ordinateurs qui prennent en charge WPA uniquement\-Enterprise et TKIP, vous devez : Déterminez si vous souhaitez :

- Configurer un profil unique pour prendre en charge tous les ordinateurs sans fil à l’aide de la méthode de chiffrement plus faible que tous vos ordinateurs prennent en charge - dans ce cas, WPA\-Enterprise et TKIP.  
- Configurer deux profils pour fournir la meilleure sécurité possible qui est pris en charge par chaque ordinateur sans fil. Dans cette instance, vous configureriez un profil qui spécifie le chiffrement le plus fort \(WPA2\-Enterprise et AES\)et un profil qui utilise la plus faible WPA\-Enterprise et TKIP chiffrement. Dans cet exemple, il est essentiel que vous placiez le profil qui utilise WPA2\-Enterprise et AES le plus élevé dans l’ordre de préférence. Les ordinateurs qui ne sont pas capables d’utiliser WPA2\-Enterprise et AES est automatiquement ignorée au profil suivant dans l’ordre de préférence et traiter le profil qui spécifie WPA\-Enterprise et TKIP.

> [!IMPORTANT]
> Vous devez placer le profil avec les normes plus sécurisés plus élevées dans la liste ordonnée des profils, étant donné que les ordinateurs qui se connectent utilisent le premier profil qu’ils sont capables d’utiliser.

### <a name="planning-restricted-access-to-the-wireless-network"></a>Planification de l’accès restreint au réseau sans fil

Dans de nombreux cas, vous souhaiterez peut-être fournir aux utilisateurs sans fil avec différents niveaux d’accès au réseau sans fil. Par exemple, vous souhaiterez peut-être autoriser certains utilisateurs un accès illimité, toutes les heures de la journée, tous les jours de la semaine. Pour d’autres utilisateurs, vous souhaiterez uniquement autoriser l’accès pendant les heures principale, du lundi au vendredi et de refuser l’accès sur le samedi et dimanche.

Ce guide fournit des instructions pour créer un environnement d’accès qui place tous les utilisateurs sans fil dans un groupe avec accès commun aux ressources sans fil. Vous créez un groupe de sécurité sans fil aux utilisateurs dans Active Directory Users et alignement des ordinateurs\-dans, puis apportez chaque utilisateur pour lequel vous souhaitez accorder l’accès sans fil un membre de ce groupe.

Lorsque vous configurez des stratégies de réseau NPS, vous spécifiez le groupe de sécurité utilisateurs sans fil en tant que l’objet qui traite de NPS lors de la détermination de l’autorisation.

Toutefois, si votre déploiement requiert la prise en charge pour différents niveaux d’accès vous devez uniquement les opérations suivantes :  

1. Créer plusieurs groupes de sécurité utilisateurs sans fil pour créer des groupes de sécurité sans fil supplémentaires dans Active Directory Users and Computers. Par exemple, vous pouvez créer un groupe qui contient les utilisateurs qui ont un accès complet, un groupe pour ceux qui ont uniquement accès au cours des heures ouvrées normales et autres groupes qui correspondent à d’autres critères qui correspondent à vos besoins.

2. Ajouter des utilisateurs aux groupes de sécurité appropriés que vous avez créé.

3. Configurer des stratégies de réseau NPS supplémentaires pour chaque groupe de sécurité sans fil supplémentaires et configurer les stratégies pour appliquer les conditions et les contraintes dont vous avez besoin pour chaque groupe.

### <a name="planning-methods-for-adding-new-wireless-computers"></a>Planification des méthodes pour l’ajout de nouveaux ordinateurs sans fil

La méthode recommandée pour joindre des nouveaux ordinateurs sans fil au domaine, puis ouvrez une session sur le domaine consiste à l’aide d’une connexion câblée à un segment de réseau local qui a accès aux contrôleurs de domaine et n’est pas protégé par un 802. 1 X de l’authentification de commutateur Ethernet.

Dans certains cas, toutefois, il peut être pratique d’utiliser une connexion câblée pour joindre des ordinateurs au domaine, ou, pour un utilisateur d’utiliser une connexion câblée pour leur première ouverture de session à l’aide d’ordinateurs qui sont déjà joints au domaine.

Pour joindre un ordinateur au domaine à l’aide d’une connexion sans fil ou pour les utilisateurs de se connecter à l’heure de la première de domaine à l’aide d’un domaine\-joint à un ordinateur et une connexion sans fil, les clients sans fil doivent d’abord établir une connexion au réseau sans fil sur un segment qui a accès aux contrôleurs de domaine de réseau en utilisant l’une des méthodes suivantes.

1. **Un membre de l’équipe informatique joint un ordinateur sans fil au domaine et configure ensuite un authentification unique sans fil profil de démarrage.** Avec cette méthode, un administrateur connecte l’ordinateur sans fil au réseau Ethernet câblé, puis joint l’ordinateur au domaine. L’administrateur distribue ensuite l’ordinateur à l’utilisateur. Lorsque l’utilisateur démarre l’ordinateur, les informations d’identification de domaine qu’il spécifie manuellement pour l’ouverture de session servent à établir une connexion au réseau sans fil et ouvrir une session le domaine.  

2. **L’utilisateur configure manuellement ordinateur sans fil avec un profil sans fil de démarrage et puis rejoint le domaine.** Avec cette méthode, les utilisateurs configurer manuellement leurs ordinateurs sans fil avec un profil de démarrage sans fil en fonction des instructions à partir d’un administrateur informatique. Le profil de démarrage sans fil permet aux utilisateurs d’établir une connexion sans fil et puis joignez l’ordinateur au domaine. Après la jonction de l’ordinateur au domaine et le redémarrage de l’ordinateur, l’utilisateur peut se connecter le domaine à l’aide d’une connexion sans fil et leurs informations d’identification du compte de domaine.

Pour déployer l’accès sans fil, consultez [déploiement de l’accès sans fil](e-wireless-access-deployment.md).
