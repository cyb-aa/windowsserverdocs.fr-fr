---
title: Déploiement de l’accès sans fil
description: Cette rubrique fait partie de la « Accès sans fil authentifié 802. 1 mot de passe de déployer X » du guide de mise en réseau de Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4b66f517-b17d-408c-828f-a3793086bc1f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b2e237cee6eac6be809add37a2ac29fdf1c92118
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446495"
---
# <a name="wireless-access-deployment"></a>Déploiement de l’accès sans fil

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Suivez ces étapes pour déployer l’accès sans fil :

- [Déployer et configurer des points d’accès sans fil](#bkmk_aps)

- [Créer un groupe de sécurité utilisateurs sans fil](#bkmk_groups)

- [Configurer le réseau sans fil \(IEEE 802.11\) stratégies](#bkmk_policies)

- [Configurer NPSs](#bkmk_nps)

- [Joindre des nouveaux ordinateurs sans fil au domaine](#bkmk_domain)

## <a name="bkmk_aps"></a>Déployer et configurer des points d’accès sans fil

Suivez ces étapes pour déployer et configurer vos points d’accès sans fil :

- [Spécifier les fréquences de canaux AP sans fil](#bkmk_channel)

- [Configurer des points d’accès sans fil](#bkmk_wirelessaps)

>[!NOTE]
>Les procédures décrites dans ce guide n'incluent aucune instruction dans le cas où la boîte de dialogue **Contrôle de compte d'utilisateur** s'ouvre pour demander votre autorisation de continuer. Si cette boîte de dialogue s’ouvre pendant que vous réalisez les procédures de ce guide, et si elle s’ouvre en réponse à vos actions, cliquez sur **Continuer**.

### <a name="bkmk_channel"></a>Spécifier les fréquences de canaux AP sans fil

Lorsque vous déployez plusieurs points d’accès sans fil sur un seul site géographique, vous devez configurer des points d’accès sans fil qui ont des signaux qui se chevauchent les fréquences de canal unique permet de réduire les interférences entre les points d’accès sans fil.

Vous pouvez utiliser les instructions suivantes pour vous aider à choisir les fréquences de canaux qui ne sont pas en conflit avec d’autres réseaux sans fil à l’emplacement géographique de votre réseau sans fil.

- S’il existe des autres organisations qui disposent de bureaux de proximité ou dans le même bâtiment que votre organisation, identifient s’il existe des réseaux sans fil appartenant à ces organisations. Découvrez le positionnement et le canal affecté les fréquences de leurs AP sans fil, car vous devez affecter les fréquences de différents canaux à votre point d’accès et vous devez déterminer le meilleur emplacement pour installer votre point d’accès

- Identifiez le chevauchement des signaux sans fil sur des étages contigus au sein de votre propre organisation. Après identification chevauchement des zones de couverture à l’extérieur et au sein de votre organisation, attribuer des fréquences de canaux pour vos points d’accès sans fil, s’assurer que les deux points d’accès sans fil avec une couverture qui se chevauche sont affectés des fréquences de canal différent.

### <a name="bkmk_wirelessaps"></a>Configurer des points d’accès sans fil

Utilisez les informations suivantes, ainsi que la documentation du produit fournie par le fabricant de point d’accès sans fil pour configurer vos points d’accès sans fil.

Cette procédure énumère des éléments couramment configurés sur un point d’accès sans fil. Les noms d’élément peuvent varier par marque et modèle et peuvent différer de celles figurant dans la liste suivante. Pour obtenir des détails spécifiques, consultez la documentation de votre point d’accès sans fil.

#### <a name="to-configure-your-wireless-aps"></a>Pour configurer vos points d’accès sans fil  

- **SSID**. Spécifiez le nom du réseau sans fil\(s\) \(ExampleWLAN, par exemple,\). Il s’agit du nom qui est publié auprès des clients sans fil.

- **Chiffrement**. Spécifiez WPA2\-Enterprise \(préféré\) ou WPA\-Enterprise et soit AES \(préféré\) ou code de chiffrement TKIP, selon les versions prises en charge par votre cartes réseau des ordinateurs client sans fil.

- **Sans fil d’adresse IP du point d’accès \(statique\)** . Sur chaque point d’accès, configurez une adresse IP statique unique qui se trouve dans la plage d’exclusion de l’étendue DHCP pour le sous-réseau. À l’aide d’une adresse qui est exclue d’attribution par DHCP empêche le serveur DHCP à partir de l’affectation de la même adresse IP à un ordinateur ou un autre appareil.

- **Masque de sous-réseau**. Configurez cette option pour faire correspondre les paramètres de masque de sous-réseau du réseau local auquel vous êtes connecté au point d’accès sans fil.  

- **Nom DNS**. Certains points d’accès sans fil peuvent être configurés avec un nom DNS. Le service DNS sur le réseau peut résoudre les noms DNS en une adresse IP. Sur chaque point d’accès sans fil qui prend en charge cette fonctionnalité, entrez un nom unique pour la résolution DNS.  

- **Service DHCP**. Si votre point d’accès sans fil intègre un\-dans le service DHCP, désactivez-la.  

- **Secret partagé RADIUS**. Utiliser un secret partagé RADIUS unique pour chaque point d’accès sans fil, sauf si vous projetez de configurer des points d’accès en tant que Clients RADIUS dans NPS par groupe. Si vous projetez de configurer des points d’accès par groupe dans NPS, le secret partagé doit être le même pour chaque membre du groupe. En outre, chaque secret partagé que vous utilisez doit être une séquence aléatoire d’au moins 22 caractères combinant des majuscules et minuscules, des chiffres et signes de ponctuation. Pour garantir le caractère aléatoire, vous pouvez utiliser un générateur de caractères aléatoires, tels que le Générateur de caractères aléatoires trouvé dans le serveur NPS **configurer 802. 1 X** Assistant, pour créer les secrets partagés.

>[!TIP]
>Enregistrer le secret partagé pour chaque point d’accès sans fil et le stocker dans un emplacement sécurisé, par exemple un bureau sécurisé. Lorsque vous configurez des clients RADIUS dans le serveur NPS, vous devez connaître le secret partagé pour chaque point d’accès sans fil.  

- **Adresse IP du serveur RADIUS**. Tapez l’adresse IP du serveur exécutant NPS.

- **Le port UDP\(s\)** . Par défaut, NPS utilise les ports UDP 1812 et 1645 pour les messages d’authentification et ports UDP 1813 et 1646 pour les messages de comptabilité. Il est recommandé que vous utilisez ces mêmes ports UDP sur vos points d’accès, mais si vous avez une raison valable d’utiliser des ports différents, vérifiez que vous non seulement configurez les points d’accès avec les nouveaux numéros de port mais également reconfigurez tous vos NPSs à utiliser les mêmes numéros de port comme les points d’accès. Si les points d’accès et les NPSs ne sont pas configurés avec les mêmes ports UDP, NPS ne peut pas recevoir ou traiter les demandes de connexion à partir de points d’accès, et toutes les tentatives de connexion sans fil sur le réseau échoue.

- **VSAs**. Certains points d’accès sans fil nécessitent fournisseur\-des attributs spécifiques \(VSA\) pour fournir des fonctionnalités de point d’accès complet sans fil. VSA est ajoutés dans la stratégie de réseau NPS.

- **Filtrage de DHCP**. Configurer des points d’accès sans fil pour bloquer les clients sans fil d’envoyer des paquets IP à partir du port UDP 68 au réseau, comme indiqué par le fabricant de point d’accès sans fil.

- **Filtrage DNS**. Configurer des points d’accès sans fil pour bloquer les clients sans fil d’envoyer des paquets IP à partir du port TCP ou UDP 53 au réseau, comme indiqué par le fabricant de point d’accès sans fil.

## <a name="create-security-groups-for-wireless-users"></a>Créer des groupes de sécurité pour les utilisateurs sans fil

Suivez ces étapes pour créer des groupes de sécurité d’un ou plusieurs utilisateurs sans fil, puis ajouter des utilisateurs au groupe de sécurité des utilisateurs sans fil appropriés :

- [Créer un groupe de sécurité utilisateurs sans fil](#bkmk_groups)

- [Ajouter des utilisateurs au groupe de sécurité sans fil](#bkmk_addusers)

### <a name="bkmk_groups"></a>Créer un groupe de sécurité utilisateurs sans fil

Vous pouvez utiliser cette procédure pour créer un groupe de sécurité sans fil dans les utilisateurs Active Directory et les ordinateurs Microsoft Management Console \(MMC\) aligner\-dans.  

Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs du domaine** ou à un groupe équivalent.

#### <a name="to-create-a-wireless-users-security-group"></a>Pour créer un groupe de sécurité utilisateurs sans fil

1. Cliquez sur **Démarrer**, **Outils d'administration**, puis sur **Utilisateurs et ordinateurs Active Directory**. Le composant logiciel enfichable Active Directory Users and Computers\-dans s’ouvre. Si le nœud de votre domaine n’est pas encore sélectionné, cliquez dessus. Par exemple, si votre domaine est example.com, cliquez sur **example.com**.

2. Dans le volet de détails, avec le bouton droit\-cliquez sur le dossier dans lequel vous souhaitez ajouter un nouveau groupe \(, par exemple, avec le bouton droit\-cliquez sur **utilisateurs**\), pointez sur **New**, puis cliquez sur **groupe**.

3. Dans **Nouvel objet - Groupe**, tapez le nom du nouveau groupe dans la zone **Nom du groupe**. Par exemple, tapez **groupe sans fil**.

4. Dans **Étendue du groupe**, sélectionnez l'une des options suivantes :

    - **Domaine local**

    - **global**

    - **Universel**

5. Dans **Type de groupe**, sélectionnez **Sécurité**.

6. Cliquez sur **OK**.

Si vous avez besoin de plusieurs groupes de sécurité pour les utilisateurs sans fil, répétez ces étapes pour créer des groupes d’utilisateurs sans fil supplémentaires. Plus tard, vous pouvez créer des stratégies de réseau individuels dans NPS pour appliquer différentes conditions et Constraints à chaque groupe, en leur fournissant les autorisations d’accès distinctes et les règles de connectivité.

### <a name="bkmk_addusers"></a> Ajouter des utilisateurs au groupe de sécurité utilisateurs sans fil

Vous pouvez utiliser cette procédure pour ajouter un utilisateur, un ordinateur ou un groupe à votre groupe de sécurité sans fil dans les utilisateurs Active Directory et les ordinateurs Microsoft Management Console \(MMC\) aligner\-dans.

Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs du domaine** ou à un groupe équivalent.

#### <a name="to-add-users-to-the-wireless-security-group"></a>Pour ajouter des utilisateurs au groupe de sécurité sans fil

1. Cliquez sur **Démarrer**, **Outils d'administration**, puis sur **Utilisateurs et ordinateurs Active Directory**. La console MMC Utilisateurs et ordinateurs Active Directory s’ouvre. Si le nœud de votre domaine n’est pas encore sélectionné, cliquez dessus. Par exemple, si votre domaine est example.com, cliquez sur **example.com**.

2. Dans le volet de détails, double-cliquez\-cliquez sur le dossier qui contient votre groupe de sécurité sans fil.

3. Dans le volet de détails, avec le bouton droit\-cliquez sur le groupe de sécurité sans fil, puis cliquez sur **propriétés**. Le **propriétés** ouvre la boîte de dialogue pour le groupe de sécurité.

4. Sur le **membres** , cliquez sur **ajouter**, puis effectuez une des procédures suivantes pour ajouter un ordinateur ou ajouter un utilisateur ou un groupe.

##### <a name="to-add-a-user-or-group"></a>Pour ajouter un utilisateur ou un groupe

1. Dans **Entrez les noms des objets à sélectionner**, tapez le nom de l’utilisateur ou groupe que vous souhaitez ajouter, puis cliquez sur **OK**.

2. Pour affecter l’appartenance au groupe à d’autres utilisateurs ou les groupes, répétez l’étape 1 de cette procédure.  

##### <a name="to-add-a-computer"></a>Pour ajouter un ordinateur

1. Cliquez sur **Types d'objet**. Le **Types d’objets** boîte de dialogue s’ouvre.

2. Dans **types d’objet**, sélectionnez **ordinateurs**, puis cliquez sur **OK**.

3. Dans **Entrez les noms des objets à sélectionner**, tapez le nom de l’ordinateur que vous souhaitez ajouter, puis cliquez sur **OK**.

4. Pour affecter l’appartenance aux groupes vers d’autres ordinateurs, répétez les étapes 1\-3 de cette procédure.

## <a name="bkmk_policies"></a>Configurer le réseau sans fil \(IEEE 802.11\) stratégies

Suivez ces étapes pour configurer le réseau sans fil \(IEEE 802.11\) extension de stratégie de groupe :

- [Ouvrir ou ajouter et ouvrir un objet de stratégie de groupe](#bkmk_opengpme)

- [Activer le réseau sans fil par défaut \(IEEE 802.11\) stratégies](#bkmk_activate)

- [Configurer la nouvelle stratégie de réseau sans fil](#bkmk_policyconfig)

### <a name="bkmk_opengpme"></a>Ouvrir ou ajouter et ouvrir un objet de stratégie de groupe

Par défaut, la fonctionnalité de gestion des stratégies de groupe est installée sur les ordinateurs exécutant Windows Server 2016, lorsque les Services de domaine Active Directory \(AD DS\) rôle de serveur est installé et le serveur est configuré comme un contrôleur de domaine. La procédure suivante qui explique comment ouvrir la Console de gestion de stratégie de groupe \(GPMC\) sur votre contrôleur de domaine. La procédure explique ensuite comment ouvrir un domaine existant\-au niveau objet de stratégie de groupe \(GPO\) pour la modification, ou créer un nouveau domaine, stratégie de groupe et l’ouvrir pour modification.

Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs du domaine** ou à un groupe équivalent.

#### <a name="to-open-or-add-and-open-a-group-policy-object"></a>Pour ouvrir ou ajouter et ouvrir un objet de stratégie de groupe

1. Sur votre contrôleur de domaine, cliquez sur **Démarrer**, cliquez sur **les outils d’administration Windows**, puis cliquez sur **Group Policy Management**. La Console de gestion de stratégie de groupe s’ouvre.  

2. Dans le volet gauche, double-cliquez\-cliquez sur votre forêt. Par exemple, double-cliquez\-cliquez sur **forêt : example.com**.  

3. Dans le volet gauche, double-cliquez\-cliquez sur **domaines**, puis double-cliquez\-cliquez sur le domaine pour lequel vous souhaitez gérer un objet de stratégie de groupe. Par exemple, double-cliquez\-cliquez sur **example.com**.  

4. Faites une des actions suivantes :

    -   **Pour ouvrir un domaine existant\-le niveau de stratégie de groupe pour la modification**, double-cliquez sur le domaine qui contient l’objet de stratégie de groupe que vous souhaitez gérer, droite\-cliquez sur la stratégie de domaine que vous souhaitez gérer, par exemple, la stratégie de domaine par défaut, puis cliquez sur **modifier**. **Éditeur de gestion de stratégie de groupe** s’ouvre.

    -   **Pour créer un nouvel objet de stratégie de groupe et ouvrir pour modification**, à droite\-cliquez sur le domaine pour lequel vous souhaitez créer un nouvel objet de stratégie de groupe, puis cliquez sur **créer un objet GPO dans ce domaine et le lier ici**.

        Dans **nouvel objet GPO**, dans **nom**, tapez un nom pour le nouvel objet de stratégie de groupe, puis cliquez sur **OK**.

        Droite\-cliquez sur votre nouvel objet de stratégie de groupe, puis cliquez sur **modifier**. **Éditeur de gestion de stratégie de groupe** s’ouvre.

Dans la section suivante, vous utiliserez éditeur de gestion de stratégie de groupe pour créer une stratégie sans fil.

### <a name="bkmk_activate"></a>Activer le réseau sans fil par défaut \(IEEE 802.11\) stratégies

Cette procédure décrit comment activer la valeur par défaut du réseau sans fil \(IEEE 802.11\) stratégies à l’aide de l’éditeur de gestion de stratégie de groupe \(GPME\).

>[!NOTE]
>Après avoir activé le **Windows Vista et versions ultérieures** version du réseau sans fil \(IEEE 802.11\) stratégies ou les **Windows XP** est de version, l’option de version automatiquement supprimé de la liste des options lorsque vous avec le bouton droit\-cliquez sur **réseau sans fil \(IEEE 802.11\) stratégies**. Cela se produit, car une fois que vous sélectionnez une version de stratégie, la stratégie est ajoutée dans le volet de détails de la GPME lorsque vous sélectionnez le **réseau sans fil \(IEEE 802.11\) stratégies** nœud. Cet état reste sauf si vous supprimez la stratégie sans fil, moment auquel la version de stratégie sans fil retourne à droite\-cliquez sur le menu pour **réseau sans fil \(IEEE 802.11\) stratégies** dans le GPME . En outre, les stratégies sans fil sont répertoriés uniquement dans le volet de détails GPME lorsque le **réseau sans fil \(IEEE 802.11\) stratégies** nœud est sélectionné.

Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs du domaine** ou à un groupe équivalent.

#### <a name="to-activate-default-wireless-network-ieee-80211-policies"></a>Par défaut de réseau sans fil \(IEEE 802.11\) stratégies  

1. Suivez la procédure précédente, **pour ouvrir ou ajouter et ouvrir un objet de stratégie de groupe** pour ouvrir le GPME.

2. Dans le GPME, dans le volet gauche, double-cliquez\-cliquez sur **Configuration ordinateur**, double\-cliquez sur **stratégies**, double\-cliquez sur **les paramètres Windows** , puis double-cliquez\-cliquez sur **paramètres de sécurité**.

![Stratégie de groupe de sans fil 802.1 X](../../../media/Wireless-GP/Wireless-GP.jpg)

3. Dans **paramètres de sécurité**, à droite\-cliquez sur **réseau sans fil \(IEEE 802.11\) stratégies**, puis cliquez sur **créer une nouvelle stratégie sans fil pour Windows Vista et versions ultérieures**. 

![802. 1 x sans fil stratégie](../../../media/Wireless-Policy/Wireless-Policy.jpg)

4. Le **propriétés de nouvelle stratégie de réseau sans fil** boîte de dialogue s’ouvre. Dans **nom de la stratégie**, tapez un nouveau nom pour la stratégie ou conservez le nom par défaut. Cliquez sur **OK** pour enregistrer la stratégie. La stratégie par défaut est activée et répertoriée dans le volet détails de la GPME avec le nouveau nom que vous avez fourni ou avec le nom par défaut **nouvelle stratégie de réseau sans fil**.

![Nouvelles propriétés de stratégie de réseau sans fil](../../../media/Wireless-Policy-Properties/Wireless-Policy-Properties.jpg)

5. Dans le volet de détails, double-cliquez\-cliquez sur **nouvelle stratégie de réseau sans fil** pour l’ouvrir.

Dans la section suivante, vous pouvez effectuer la configuration de stratégie, ordre de préférence de traitement de stratégie et les autorisations réseau.

### <a name="bkmk_policyconfig"></a>Configurer la nouvelle stratégie de réseau sans fil

Vous pouvez utiliser les procédures dans cette section pour configurer le réseau sans fil \(IEEE 802.11\) stratégie. Cette stratégie vous permet de configurer les paramètres de sécurité et l’authentification, gérer les profils sans fil et spécifier des autorisations pour les réseaux sans fil qui ne sont pas configurés en tant que réseaux préférés.

- [Configurer un profil de connexion sans fil pour PEAP\-MS\-CHAP v2](#bkmk_configureprofile)  

- [Définir l’ordre de préférence pour les profils de connexion sans fil](#bkmk_preferenceorder)  

- [Définir les autorisations du réseau](#bkmk_permissions)  

#### <a name="bkmk_configureprofile"></a>Configurer un profil de connexion sans fil pour PEAP\-MS\-CHAP v2

Cette procédure fournit les étapes requises pour configurer un PEAP\-MS\-profil sans fil CHAP v2.  

L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

##### <a name="to-configure-a-wireless-connection-profile-for-peap-ms-chap-v2"></a>Pour configurer un profil de connexion sans fil pour PEAP\-MS\-CHAP v2

1. Dans GPME, dans la boîte de dialogue des propriétés de réseau sans fil pour la stratégie que vous venez de créer, dans le **général** onglet et dans **Description**, tapez une brève description de la stratégie.

2. Pour spécifier que la configuration automatique WLAN est utilisé pour configurer les paramètres de la carte réseau sans fil, assurez-vous que **service d’utilisation de configuration automatique WLAN Windows pour les clients** est sélectionné.  

3. Dans **se connecter aux réseaux disponibles dans l’ordre des profils affichés ci-dessous**, cliquez sur **ajouter**, puis sélectionnez **Infrastructure**. Le **propriétés du nouveau profil** boîte de dialogue s’ouvre.

4. Dans le**propriétés du nouveau profil** boîte de dialogue le **connexion** sous l’onglet le **nom du profil** , tapez un nouveau nom pour le profil. Par exemple, tapez **profil Example.com WLAN pour Windows 10**.

5. Dans **nom réseau\(s\) \(SSID\)** , tapez le SSID qui correspond à l’identificateur SSID sur vos points d’accès sans fil, puis cliquez sur **ajouter**.

    Si votre déploiement utilise plusieurs identificateurs SSID et que chaque point d'accès sans fil utilise les mêmes paramètres de sécurité sans fil, répétez cette étape pour ajouter l'identificateur SSID pour chaque point d'accès sans fil auquel vous voulez appliquer ce profil.

    Si votre déploiement utilise plusieurs identificateurs SSID et que les paramètres de sécurité pour chaque identificateur SSID ne sont pas identiques, configurez un profil distinct pour chaque groupe d'identificateurs SSID utilisant les mêmes paramètres de sécurité. Par exemple, si vous avez un groupe de points d’accès sans fil est configuré pour utiliser WPA2\-Enterprise et AES et un autre groupe de points d’accès sans fil à utiliser WPA\-Enterprise et TKIP, configurez un profil pour chaque groupe de points d’accès sans fil.

6. Si le texte par défaut **NEWSSID** est présent, sélectionnez-le, puis cliquez sur **supprimer**.

7. Si vous avez déployé des points d'accès sans fil qui sont configurés pour supprimer la balise de diffusion, sélectionnez **Se connecter même s'il ne s'agit pas d'un réseau de diffusion**.

    > [!NOTE]
    > L’activation de cette option peut créer un risque de sécurité, car les clients sans fil va détecter les et tentatives de connexion à n’importe quel réseau sans fil. Par défaut, ce paramètre n'est pas activé.  

8. Cliquez sur l'onglet **Sécurité** , cliquez sur **Avancé**puis configurez les éléments suivants :

    1. Pour configurer les paramètres 802.1X avancés, dans **IEEE 802.1X**, sélectionnez **Appliquer les paramètres avancés IEEE 802.1X**.

        Lors de l’avancée 802. 1 X paramètres sont appliqués, les valeurs par défaut pour **Max Eapol\-empilés Démarrer**, **période de maintien**, **période de démarrage**, et  **Période d’authentification** sont suffisantes pour les déploiements sans fil standard. Pour cette raison, vous n’avez pas besoin de modifier les valeurs par défaut, sauf si vous avez une raison spécifique pour effectuer cette opération.

    2. Pour activer l'authentification unique, sélectionnez **Activer l'authentification unique pour ce réseau**.

    3. Les autres valeurs par défaut dans **Authentification unique** sont suffisantes pour les déploiements sans fil standard.

    4. Dans **itinérance rapide**, si votre point d’accès sans fil est configuré pour les versions antérieures\-l’authentification, sélectionnez **ce réseau utilise pre\-authentification**.

9. Pour spécifier que les communications sans fil répondent à FIPS 140\-2 normes, sélectionnez **effectue un chiffrement FIPS 140\-mode certifié 2**.

10. Cliquez sur **OK** pour revenir à l'onglet **Sécurité**. Dans **sélectionner les méthodes de sécurité pour ce réseau**, dans **authentification**, sélectionnez **WPA2\-entreprise** si elle est prise en charge par votre point d’accès sans fil et le sans fil cartes réseau des clients. Sinon, sélectionnez **WPA\-Enterprise**.

11. Dans **chiffrement**, si pris en charge par votre point d’accès sans fil et les cartes réseau des clients sans fil, sélectionnez **AES-CCMP**. Si vous utilisez des points d’accès et des cartes de réseau sans fil prenant en charge 802.11ac, sélectionnez **AES-GCMP**. Sinon, sélectionnez **TKIP**.

    > [!NOTE]  
    > Les paramètres des options **authentification** et **chiffrement** doit correspondre les paramètres configurés sur vos points d’accès sans fil. Les paramètres par défaut **Mode d’authentification**, **Nbre max. d’échecs d’authentification**, et **mettre en Cache les informations utilisateur pour les futures connexions à ce réseau** sont suffisant pour les déploiements sans fil standard.  

12. Dans **sélectionner une méthode d’authentification réseau**, sélectionnez **EAP protégé \(PEAP\)** , puis cliquez sur **propriétés**. Le **propriétés EAP protégées** boîte de dialogue s’ouvre.

13. Dans **propriétés EAP protégées**, vérifiez que **vérifier l’identité du serveur en validant le certificat** est sélectionné.

14. Dans **des autorités de Certification racine approuvées**, sélectionnez l’autorité de certification racine approuvée \(autorité de certification\) qui a émis le certificat de serveur pour le serveur NPS.

    > [!NOTE]  
    > Ce paramètre limite aux seules autorités de certification sélectionnées les autorités de certification racines auxquelles les clients font confiance. Si aucune autorités de certification racine de confiance n’est sélectionnée, les clients font confiance que tous les racines répertoriés dans leur magasin de certificats Autorités de Certification racine des autorités de certification.  

15. Dans le **sélectionner la méthode d’authentification** , sélectionnez **mot de passe sécurisé \(EAP\-MS\-CHAP v2\)** .

16. Cliquez sur **configurer**. Dans le **propriétés EAP MSCHAPv2** boîte de dialogue, vérifiez **utiliser automatiquement mon nom d’ouverture de session Windows et le mot de passe \(et éventuellement le domaine\)**  est sélectionnée, puis cliquez sur  **OK**.

17. Pour activer la reconnexion rapide PEAP, vérifiez que **activer la reconnexion rapide** est sélectionné.

18. Pour exiger TLV de liaison de serveur sur les tentatives de connexion, sélectionnez **déconnecter si le serveur ne présente pas TLV de liaison de**.

19. Pour spécifier que l’identité de l’utilisateur est masquée dans la première phase d’authentification, sélectionnez **activer la protection de la confidentialité**et dans la zone de texte, tapez un nom d’identité anonyme, ou laissez la zone de texte vide.

    > [!NOTES]
    > - La stratégie de serveur NPS pour 802.1 X sans fil doit être créée à l’aide de NPS **stratégie de demande de connexion**. Si la stratégie de serveur NPS est créée à l’aide de NPS **stratégie réseau**, confidentialité de l’identité ne fonctionnera pas.
    > - Confidentialité de l’identité EAP est assurée par certaines méthodes EAP, le cas vide ou une identité anonyme \(diffère de l’identité réelle\) est envoyé en réponse à la demande d’identité EAP. PEAP envoie l’identité lors de l’authentification à deux reprises. Dans la première phase, l’identité est envoyée en texte brut, et cette identité est utilisée pour le routage, pas pour l’authentification du client. L’identité réelle, utilisé pour l’authentification, est envoyé au cours de la deuxième phase de l’authentification, le tunnel sécurisé est établi dans la première phase. Si **activer la protection de la confidentialité** case à cocher est activée, le nom d’utilisateur est remplacée par l’entrée spécifiée dans la zone de texte. Par exemple, supposons que **activer la protection de la confidentialité** est sélectionné et l’alias de confidentialité identité **anonyme** est spécifié dans la zone de texte. Pour un utilisateur avec un alias de l’identité réelle <strong>jdoe@example.com</strong>, l’identité envoyée dans la première phase d’authentification sera modifiée en <strong>anonymous@example.com</strong>. La partie domaine de l’identité de phase 1er n’est pas modifiée car il est utilisé pour le routage.  

20. Cliquez sur **OK** pour fermer la **propriétés EAP protégées** boîte de dialogue.
21. Cliquez sur **OK** pour fermer la **sécurité** onglet.
22. Si vous souhaitez créer des profils supplémentaires, cliquez sur **ajouter**, puis répétez les étapes précédentes, faire des choix différents pour personnaliser chaque profil pour les clients sans fil et le réseau auquel vous souhaitez que le profil appliqué. Lorsque vous avez terminé l’ajout de profils, cliquez sur **OK** pour fermer la boîte de dialogue Propriétés de stratégie de réseau sans fil.

Dans la section suivante, vous pouvez commander les profils de stratégie pour une sécurité optimale.

#### <a name="bkmk_preferenceorder"></a>Définir l’ordre de préférence pour les profils de connexion sans fil
Vous pouvez utiliser cette procédure si vous avez créé plusieurs profils sans fil dans votre stratégie de réseau sans fil et que vous souhaitez classer les profils pour une efficacité optimale et de sécurité.

Pour vous assurer que les clients sans fil se connectent avec le plus haut niveau de sécurité qu’ils peuvent prendre en charge, placez vos stratégies plus restrictives en haut de la liste.

Par exemple, si vous avez deux profils, un pour les clients qui prennent en charge de WPA2 et un pour les clients qui prennent en charge WPA, placez le profil de WPA2 plus élevé dans la liste. Cela garantit que les clients qui prennent en charge de WPA2 utilisent cette méthode pour la connexion, plutôt que la WPA moins sécurisée.

Cette procédure fournit les étapes pour spécifier l’ordre dans lequel les profils de connexion sans fil sont utilisés pour connecter des clients sans fil membres de domaine aux réseaux sans fil.

L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

##### <a name="to-set-the-preference-order-for-wireless-connection-profiles"></a>Pour définir l’ordre de préférence pour les profils de connexion sans fil

1. Dans GPME, dans la boîte de dialogue des propriétés de réseau sans fil pour la stratégie que vous venez de configurer, cliquez sur le **général** onglet.

2. Sur le **général** sous l’onglet **se connecter aux réseaux disponibles dans l’ordre des profils affichés ci-dessous**, sélectionnez le profil que vous souhaitez déplacer dans la liste, puis cliquez sur le bouton « flèche haut » ou « flèche vers le bas » bouton pour déplacer le profil à l’emplacement souhaité dans la liste.

3.  Répétez l’étape 2 pour chaque profil que vous souhaitez déplacer dans la liste.  

4.  Cliquez sur **OK** pour enregistrer toutes les modifications.

Dans la section suivante, vous pouvez définir des autorisations de réseau pour la stratégie sans fil.

#### <a name="bkmk_permissions"></a>Définir les autorisations du réseau
Vous pouvez configurer les paramètres sur le **autorisations réseau** onglet pour les membres de domaine pour le réseau sans fil \(IEEE 802.11\) stratégies s’appliquent.

Vous pouvez uniquement appliquer les paramètres suivants pour les réseaux sans fil qui ne sont pas configurés sur le **général** onglet dans le **des propriétés de stratégie de réseau sans fil** page :

- Autoriser ou refuser des connexions aux réseaux sans fil spécifiques que vous spécifiez par type de réseau et de Service Set Identifier \(SSID\)

- Autoriser ou refuser les connexions aux réseaux ad hoc

- Autoriser ou refuser les connexions aux réseaux à infrastructure

- Autoriser ou refuser aux utilisateurs d’afficher les types de réseau \(ad hoc ou infrastructure\) auquel ils sont interdites

- Autoriser ou refuser aux utilisateurs de créer un profil qui s’applique à tous les utilisateurs

- Les utilisateurs peuvent se connecter uniquement à des réseaux autorisés à l’aide de profils de stratégie de groupe

L’appartenance au **Admins du domaine**, ou équivalente, est la condition minimale requise pour réaliser ces procédures.

##### <a name="to-allow-or-deny-connections-to-specific-wireless-networks"></a>Pour autoriser ou refuser des connexions aux réseaux sans fil spécifiques

1. Dans GPME, dans la boîte de dialogue Propriétés du réseau sans fil, cliquez sur le **autorisations réseau** onglet.

2. Sur le **autorisations réseau** , cliquez sur **ajouter**. Le **nouvelle entrée d’autorisation** boîte de dialogue s’ouvre.

3. Dans le **nouvelle entrée d’autorisation** boîte de dialogue le **nom réseau \(SSID\)**  champ, entrez le SSID du réseau du réseau pour lequel vous souhaitez définir des autorisations.

4.  Dans **réseau Type**, sélectionnez **Infrastructure** ou **Ad hoc**.

    > [!NOTE]  
    > Si vous ne savez pas si le réseau de diffusion est une infrastructure réseau ou ad hoc, vous pouvez configurer deux entrées d’autorisation réseau, une pour chaque type de réseau.

5. Dans **autorisation**, sélectionnez **autoriser** ou **Deny**.

6. Cliquez sur **OK**, pour revenir à la **autorisations réseau** onglet.

##### <a name="to-specify-additional-network-permissions-optional"></a>Pour spécifier les autorisations de réseau supplémentaire \(facultatif\)

1.  Sur le **autorisations réseau** , configurez tout ou partie des éléments suivants :  

    -   Pour refuser l’accès aux réseaux ad hoc membres de votre domaine, sélectionnez **empêcher les connexions à ad\-réseaux ad hoc**.

    -   Pour refuser l’accès aux réseaux à infrastructure membres de votre domaine, sélectionnez **empêcher des connexions aux réseaux à infrastructure**.  

    -   Pour autoriser les membres de votre domaine afficher les types de réseau \(ad hoc ou infrastructure\) auquel ils sont interdites, sélectionnez **autoriser les utilisateurs à afficher les réseaux refusés**.

    -   Pour permettre aux utilisateurs de créer des profils qui s’appliquent à tous les utilisateurs, sélectionnez **autoriser tout le monde à créer tous les profils utilisateur**.

    -   Pour spécifier que vos utilisateurs peuvent se connecter uniquement aux réseaux autorisés à l’aide de profils de stratégie de groupe, sélectionnez **utiliser uniquement des profils de stratégie de groupe pour les réseaux autorisés**.

## <a name="bkmk_nps"></a>Configurer votre NPSs
Suivez ces étapes pour configurer NPSs pour effectuer l’authentification de 802. 1 X pour l’accès sans fil :

- [Inscrire un serveur NPS dans les Services de domaine Active Directory](#bkmk_npsreg)

- [Configurer un point d’accès sans fil en tant que serveur NPS RADIUS Client](#bkmk_radiusclient)

- [Créer des stratégies du serveur NPS pour 802.1 X sans fil à l’aide d’un Assistant](#bkmk_npspolicy)

### <a name="bkmk_npsreg"></a>Inscrire un serveur NPS dans les Services de domaine Active Directory
Vous pouvez utiliser cette procédure pour inscrire un serveur exécutant le serveur NPS \(NPS\) dans les Services de domaine Active Directory \(AD DS\) dans le domaine où le serveur NPS est membre. Pour NPSs à accorder l’autorisation de lire le cadran\-dans les propriétés des comptes d’utilisateur pendant le processus d’autorisation, chaque serveur NPS doit être inscrit dans AD DS. L’inscription d’un serveur NPS ajoute le serveur à la **serveurs RAS et IAS** groupe de sécurité dans AD DS.

>[!NOTE]
>Vous pouvez installer NPS sur un contrôleur de domaine ou sur un serveur dédié. Exécutez la commande Windows PowerShell suivante pour installer NPS si vous n’avez pas encore fait :
    
    Install-WindowsFeature NPAS -IncludeManagementTools
    
L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

#### <a name="to-register-an-nps-in-its-default-domain"></a>Pour inscrire un serveur NPS dans son domaine par défaut

1. Sur le serveur NPS, dans **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **Network Policy Server**. Le serveur NPS aligner\-dans s’ouvre.

2. Droite\-cliquez sur **NPS \(Local\)** , puis cliquez sur **inscrire le serveur dans Active Directory**. La boîte de dialogue **Serveur NPS (Network Policy Server)** s’ouvre.

3. Dans **Network Policy Server**, cliquez sur **OK**, puis cliquez sur **OK** à nouveau.

### <a name="bkmk_radiusclient"></a>Configurer un point d’accès sans fil en tant que serveur NPS RADIUS Client
Vous pouvez utiliser cette procédure pour configurer un point d’accès, également appelé un *serveur d’accès réseau \(NAS\)* , comme un Remote Authentication Dial\-In User Service \(RADIUS\) client à l’aide du composant logiciel enfichable NPS\-dans. 

>[!IMPORTANT]
>Les ordinateurs clients, tels que les ordinateurs portables sans fil et autres ordinateurs exécutant des systèmes d'exploitation clients, ne sont pas des clients RADIUS. Clients RADIUS sont des serveurs d’accès réseau, tels que des points d’accès sans fil 802. 1 X\-commutateurs compatibles, réseau privé virtuel \(VPN\) serveurs et accès à distance\-jusqu'à des serveurs —, car elles utilisent le protocole RADIUS pour communiquer avec les serveurs RADIUS tels que NPSs.

L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

#### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>Pour ajouter un serveur d’accès réseau en tant que client RADIUS dans NPS

1. Sur le serveur NPS, dans **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **Network Policy Server**. Le serveur NPS aligner\-dans s’ouvre.

2. Dans le serveur NPS aligner\-, double-cliquez\-cliquez sur **Clients et serveurs RADIUS**. Droite\-cliquez sur **Clients RADIUS**, puis cliquez sur **New**.

3. Dans **nouveau Client RADIUS**, vérifiez que le **activer ce client RADIUS** case à cocher est activée.

4. Dans **nouveau Client RADIUS**, dans **nom convivial**, tapez un nom complet pour le point d’accès sans fil.

    Par exemple, si vous souhaitez ajouter un point d’accès sans fil \(AP\) nommé AP\-01, type **AP\-01**.

5. Dans **adresse \(IP ou DNS\)** , tapez l’adresse IP ou le nom de domaine complet \(FQDN\) pour le serveur NAS.

    Si vous entrez le nom de domaine complet, pour vérifier que le nom est correct et qu’il est mappé à une adresse IP valide, cliquez sur **Vérifiez**, puis dans **adresse vérifier**, dans le **adresse** , cliquez sur  **Résoudre**. Si le nom de domaine complet est mappée à une adresse IP valide, l’adresse IP de ce stockage NAS s’affiche automatiquement dans **adresse IP**. Si le nom de domaine complet n’aboutit pas à une adresse IP que vous recevrez un message indiquant qu’aucun hôte n’inconnu. Si cela se produit, vérifiez que vous avez le nom de point d’accès correct et que le point d’accès est sous tension et connecté au réseau.  

    Cliquez sur **OK** pour fermer **adresse vérifier**.  

6. Dans **nouveau Client RADIUS**, dans **Secret partagé**, effectuez l’une des opérations suivantes :  

    -   Pour configurer manuellement un secret partagé RADIUS, sélectionnez **manuel**, puis dans **secret partagé**, tapez le mot de passe fort qui est aussi entré sur le NAS. Retapez le secret partagé dans **confirmer le secret partagé**.  

    -   Pour générer automatiquement un secret partagé, sélectionnez le **générer** case à cocher, puis cliquez sur le **générer** bouton. Enregistrer le secret partagé généré et comment utiliser cette valeur pour configurer le NAS afin qu’il puisse communiquer avec le serveur NPS.  

        >[!IMPORTANT]
        >Le secret partagé RADIUS que vous entrez pour votre point d’accès virtuel dans le serveur NPS doit correspondre exactement au secret partagé RADIUS qui est configuré sur votre réelle AP sans fil Si vous utilisez l’option de serveur NPS pour générer un secret partagé RADIUS, vous devez configurer le point d’accès réel correspondant avec le secret partagé RADIUS qui a été généré par le serveur NPS.

7. Dans **nouveau Client RADIUS**, dans le **avancé** sous l’onglet **nom du fournisseur**, spécifiez le nom de fabricant NAS. Si vous n’êtes pas sûr du nom de fabricant NAS, sélectionnez **norme RADIUS**.

8. Dans **des Options supplémentaires**, si vous utilisez des méthodes d’authentification autre que EAP et PEAP et si votre NAS prend en charge l’utilisation de l’attribut de l’authentificateur de message, sélectionnez **messages de demande d’accès doivent contenir le Message\-attribut de l’authentificateur**.

9. Cliquez sur **OK**. Votre NAS s’affiche dans la liste des clients RADIUS configuré sur le serveur NPS.

### <a name="bkmk_npspolicy"></a>Créer des stratégies du serveur NPS pour 802.1 X sans fil à l’aide d’un Assistant
Vous pouvez utiliser cette procédure pour créer des stratégies de demande de la connexion et stratégies nécessaires pour déployer soit 802. 1 X réseau\-points d’accès sans fil capable comme Remote Authentication Dial\-In User Service \(RADIUS\) clients auprès du serveur RADIUS exécutant le serveur NPS \(NPS\).  
Après avoir exécuté l’Assistant, les stratégies suivantes sont créées :

- Stratégie de demande une connexion

- Une stratégie réseau

>[!NOTE]
>Vous pouvez exécuter l’Assistant câblé sécuriser de nouveau IEEE 802. 1 X et les connexions sans fil chaque fois que vous avez besoin créer des stratégies pour 802. 1 X un accès authentifié.

L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

#### <a name="create-policies-for-8021x-authenticated-wireless-by-using-a-wizard"></a>Créer des stratégies pour 802. 1 X sans fil authentifié à l’aide d’un Assistant

1. Ouvrez le serveur NPS d’alignement\-dans. Si ce n’est pas déjà fait, cliquez sur **NPS \(Local\)** . Si vous exécutez le composant logiciel enfichable MMC NPS\-dans et souhaitez créer des stratégies sur un serveur NPS à distance, sélectionnez le serveur.

2. Dans **mise en route**, dans **Configuration Standard**, sélectionnez **serveur RADIUS pour les connexions câblées ou sans fil de X 802.1**. Le texte et les liens ci-dessous pour refléter votre sélection, la modification de texte.

3. Cliquez sur **configurer 802. 1 X**. L’Assistant Configurer 802. 1 X s’ouvre.

4.  Sur le **sélectionner le Type de connexions 802.1 X** page d’Assistant, en **Type de connexions de 802. 1 X**, sélectionnez **des connexions sans fil sécurisées**et dans **nom**, tapez un nom pour votre stratégie, ou laissez le nom par défaut **des connexions sans fil sécurisées**. Cliquez sur **Suivant**.

5.  Sur le **spécifier des commutateurs de 802. 1 X** page d’Assistant, en **clients RADIUS**, une 802. 1 toutes les X commutateurs des points d’accès sans fil que vous avez ajoutés en tant que Clients RADIUS dans le composant logiciel enfichable NPS\-dans sont affichés. Effectuez l’une des actions suivantes :

    -   Pour ajouter des serveurs d’accès réseau supplémentaire \(NAS\), tels que les points d’accès sans fil, dans **clients RADIUS**, cliquez sur **ajouter**, puis dans **nouveau client RADIUS**, entrez les informations pour : **Nom convivial**, **adresse \(IP ou DNS\)** , et **Secret partagé**.

    -   Pour modifier les paramètres de n’importe quel serveur NAS, dans **clients RADIUS**, sélectionnez le point d’accès pour lequel vous souhaitez modifier les paramètres, puis cliquez sur **modifier**. Modifiez les paramètres en fonction des besoins.

    -   Pour supprimer un NAS dans la liste, **clients RADIUS**, sélectionnez le NAS, puis cliquez sur **supprimer**.

        >[!WARNING]
        >Suppression d’un client RADIUS depuis le **configurer 802. 1 X** Assistant supprime le client à partir de la configuration du serveur NPS. Tous les ajouts, modifications et suppressions que vous apportez dans le **configurer 802. 1 X** Assistant pour les clients RADIUS sont répercutées dans le serveur NPS composant logiciel enfichable\-dans, dans le **Clients RADIUS** nœud sous  **NPS** \/ **Clients et serveurs RADIUS**. Par exemple, si vous utilisez l’Assistant pour supprimer un commutateur X 802.1, le commutateur est également retiré le serveur NPS composant logiciel enfichable\-dans.

6. Cliquez sur **Suivant**. Sur le **configurer une méthode d’authentification** page d’Assistant, en **Type \(selon la méthode de configuration d’accès et réseau\)** , sélectionnez **Microsoft : Protected EAP \(PEAP\)** , puis cliquez sur **configurer**.

    >[!TIP]
    >Si vous recevez un message d’erreur indiquant qu’un certificat est introuvable pour une utilisation avec la méthode d’authentification, et que vous avez configuré les Services de certificats Active Directory afin d’émettre automatiquement les certificats sur des serveurs RAS et IAS sur votre réseau, tout d’abord Vérifiez que vous avez suivi les étapes pour inscrire le serveur NPS dans les Services de domaine Active Directory, puis procédez comme suit pour mettre à jour de la stratégie de groupe : Cliquez sur **Démarrer**, cliquez sur **Windows System**, cliquez sur **exécuter**et dans **Open**, type **gpupdate**, puis Appuyez sur ENTRÉE. Lorsque la commande renvoie les résultats indiquant que les utilisateur et stratégie de groupe ont mis à jour avec succès, sélectionnez **Microsoft : Protected EAP \(PEAP\)**  à nouveau, puis cliquez sur **configurer**.
    >
    >Si, après l’actualisation de stratégie de groupe que vous continuez à recevoir le message d’erreur indiquant qu’un certificat est introuvable pour une utilisation avec la méthode d’authentification, le certificat n’est pas affiché, car il ne respecte pas le certificat de serveur minimale configuration requise comme indiqué dans le Guide d’accompagnement du réseau : [Déployer des certificats de serveur pour 802. 1 X câblés et sans fil déploiements](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments). Si cela se produit, vous devez arrêter la configuration NPS, révoquez le certificat délivré à votre serveur NPS\(s\), puis suivez les instructions pour configurer un nouveau certificat en utilisant le guide de déploiement de certificats de serveur.

7.  Sur le **modifier les propriétés EAP protégées** page d’Assistant, en **certificat émis**, assurez-vous que le certificat de serveur NPS correct est sélectionné, puis procédez comme suit :

    >[!NOTE]
    >Vérifiez que la valeur dans **émetteur** est correct pour le certificat sélectionné dans **certificat émis**. Par exemple, l’émetteur attendu pour un certificat émis par une autorité de certification exécutant les Services de certificats Active Directory \(AD CS\) nommé corp\DC1, dans le domaine contoso.com, est **corp\-DC1\-del’autoritédecertification**.

    -   Pour autoriser les utilisateurs à se déplacent avec leurs ordinateurs sans fil entre les points d’accès sans avoir à se réauthentifier chaque fois qu’ils associer à un nouveau point d’accès, sélectionnez **activer la reconnexion rapide**.

    -   Pour spécifier que les clients sans fil de connexion se termine le processus d’authentification réseau si le serveur RADIUS ne présente pas de Type de liaison\-longueur\-valeur \(TLV\), sélectionnez **Disconnect Les clients sans liaison**.  

    -   Pour modifier la stratégie de paramètres pour le protocole EAP tapez dans **Types EAP**, cliquez sur **modifier**, dans **propriétés EAP MSCHAPv2**, modifiez les paramètres selon vos besoins, puis cliquez sur **OK**.  

8.  Cliquez sur **OK**. La boîte de dialogue Modifier les propriétés EAP protégées se ferme et vous la **configurer 802. 1 X** Assistant. Cliquez sur **Suivant**.

9. Dans **spécifier des groupes d’utilisateur**, cliquez sur **ajouter**, puis tapez le nom du groupe de sécurité que vous avez configuré pour vos clients sans fil dans Active Directory Users et alignement des ordinateurs\-dans. Par exemple, si vous avez nommé votre groupe de sécurité sans fil groupe sans fil, tapez **groupe sans fil**. Cliquez sur **Suivant**.

10. Cliquez sur **configurer** pour configurer les attributs standards RADIUS et fournisseur\-des attributs spécifiques pour le réseau local virtuel \(VLAN\) en fonction des besoins et comme spécifié par la documentation fournie par votre fournisseur de matériel de point d’accès sans fil. Cliquez sur **Suivant**.

11. Passez en revue les détails du résumé configuration, puis cliquez sur **Terminer**.

Vos stratégies NPS sont maintenant créés, et vous pouvez passer à joindre des ordinateurs sans fil au domaine.

## <a name="bkmk_domain"></a>Joindre des nouveaux ordinateurs sans fil au domaine
La méthode la plus simple pour joindre des nouveaux ordinateurs sans fil au domaine est physiquement attacher l’ordinateur à un segment de réseau local câblé \(un segment non contrôlé par un commutateur X 802.1\) avant de rejoindre l’ordinateur au domaine. Il s’agit le plus simple, car les paramètres de stratégie de groupe sans fil sont appliqués automatiquement et immédiatement et, si vous avez déployé votre propre infrastructure à clé publique, l’ordinateur reçoit le certificat d’autorité de certification et le place dans le magasin de certificats Autorités de Certification racine de confiance, Autoriser le client sans fil à approuver NPSs avec les certificats de serveur émis par votre autorité de certification.

De même, une fois un nouvel ordinateur sans fil est joint au domaine, la méthode recommandée pour les utilisateurs de se connecter au domaine est pour effectuer des journaux sur à l’aide d’une connexion câblée au réseau.

### <a name="other-domain-join-methods"></a>Autres méthodes de jonction de domaine
Dans les cas où il n’est pas pratique pour joindre des ordinateurs au domaine à l’aide d’une connexion Ethernet câblée, ou dans les cas où l’utilisateur ne peut pas se connecter au domaine pour la première fois à l’aide d’une connexion câblée, vous devez utiliser une autre méthode.

- **Configuration de l’ordinateur personnel informatique**. Un membre de l’équipe informatique joint un ordinateur sans fil au domaine et configure un authentification unique sans fil profil de démarrage. Avec cette méthode, l’administrateur informatique connecte l’ordinateur sans fil au réseau Ethernet câblé et joint l’ordinateur au domaine. L’administrateur distribue ensuite l’ordinateur à l’utilisateur. Lorsque le démarrage de l’utilisateur l’ordinateur sans utiliser une connexion filaire, les informations d’identification de domaine qu’il spécifie manuellement pour l’ouverture de session utilisateur sont utilisés à la fois à établir une connexion au réseau sans fil et à connecter au domaine.

Pour plus d’informations, consultez la section [joindre le domaine et ouvrir une session à l’aide de la méthode de Configuration ordinateur personnel informatique](#bkmk_itstaff)

-   **Démarrer la Configuration du profil sans fil par les utilisateurs**. Manuellement, l’utilisateur configure l’ordinateur sans fil avec un profil de démarrage sans fil et rejoint le domaine, en fonction des instructions acquises à partir d’un administrateur informatique. Le profil de démarrage sans fil permet à l’utilisateur établir une connexion sans fil et de rejoindre ensuite le domaine. Après la jonction de l’ordinateur au domaine et le redémarrage de l’ordinateur, l’utilisateur peut se connecter le domaine à l’aide d’une connexion sans fil et leurs informations d’identification du compte de domaine.

Pour plus d’informations, consultez la section [joindre le domaine et ouvrir une session à l’aide de Configuration du profil sans fil Bootstrap par les utilisateurs](#bkmk_userbootstrap).

### <a name="bkmk_itstaff"></a>Joindre le domaine et ouvrir une session à l’aide de la méthode de Configuration ordinateur personnel informatique
Les utilisateurs membres de domaine avec domaine\-ordinateurs clients sans fil joints peuvent utiliser un profil sans fil temporaire pour se connecter à un 802. 1 X\-authentifié de réseau sans fil sans être connecté au réseau local câblé. Ce profil sans fil temporaire est appelé un *amorcer le profil sans fil*.

Un profil de démarrage sans fil oblige l’utilisateur à spécifier manuellement leurs informations d’identification de compte domaine utilisateur et ne valide pas le certificat de Remote Authentication Dial\-In User Service \(RADIUS\) server exécutant le serveur NPS \(NPS\).

Une fois la connectivité sans fil est établie, stratégie de groupe est appliquée sur l’ordinateur client sans fil, et un nouveau profil sans fil est émis automatiquement. La nouvelle stratégie utilise les informations d’identification de compte utilisateur et d’ordinateur pour l’authentification du client. 

En outre, dans le cadre de la PEAP\-MS\-CHAP v2 l’authentification mutuelle à l’aide du nouveau profil au lieu du profil de démarrage, le client valide les informations d’identification du serveur RADIUS.

Après avoir joint l’ordinateur au domaine, utilisez cette procédure pour configurer un authentification unique sans fil profil de démarrage, avant de distribuer l’ordinateur sans fil au domaine\-utilisateur membre.

#### <a name="to-configure-a-single-sign-on-bootstrap-wireless-profile"></a>Pour configurer un authentification unique sans fil profil de démarrage

1. Créer un profil de démarrage à l’aide de la procédure dans ce guide nommé [configurer un profil de connexion sans fil pour PEAP\-MS\-CHAP v2](#bkmk_configureprofile)et utilisez les paramètres suivants :

    - PEAP\-MS\-authentification CHAP v2

    - Valider désactivé de certificat de serveur RADIUS

    - Authentification unique activée

2. Dans les propriétés de la stratégie de réseau sans fil dans lequel vous avez créé le nouveau profil de démarrage, sur le **général** onglet, sélectionnez le profil de démarrage, puis cliquez sur **exporter** pour exporter le profil à un partage réseau, lecteur flash USB, ou tout autre emplacement facilement accessible. Le profil est enregistré comme un fichier *.xml vers l’emplacement que vous spécifiez.

3. Joindre le nouvel ordinateur sans fil au domaine \(, par exemple, via une connexion Ethernet ne nécessitant pas de IEEE 802. 1 X authentification\) et ajouter le profil de démarrage sans fil à l’ordinateur à l’aide de la **netsh wlan Ajouter un profil** commande.

    >[!NOTE]
    >Pour plus d’informations, consultez les commandes Netsh pour réseau sans fil Local \(WLAN\) à [http :\/\/technet.microsoft.com\/bibliothèque\/dd744890.aspx](https://technet.microsoft.com/library/dd744890).

4. Distribuer le nouvel ordinateur sans fil à l’utilisateur avec la procédure « Ouvrir une session sur le domaine à l’aide des ordinateurs exécutant Windows 10. »

Lorsque l’utilisateur démarre l’ordinateur, Windows invite l’utilisateur à entrer son nom de compte utilisateur de domaine et le mot de passe. Étant donné que l’authentification unique est activée, l’ordinateur utilise les informations d’identification de domaine utilisateur compte pour tout d’abord établir une connexion avec le réseau sans fil, puis ouvrez une session sur le domaine.

#### <a name="bkmk_w10"></a>Ouvrez une session le domaine à l’aide des ordinateurs exécutant Windows 10

1. Fermez la session sur l’ordinateur et redémarrez-le.

2. Appuyez sur n’importe quelle touche du clavier ou cliquez sur le bureau. L’écran d’ouverture de session s’affiche avec un nom de compte d’utilisateur local affichée et un champ de saisie de mot de passe sous le nom. Ne vous connectez pas avec le compte d’utilisateur local.

3. Dans le coin inférieur gauche de l’écran, cliquez sur **autre utilisateur**. Le journal d’un autre utilisateur sur l’écran s’affiche avec deux champs, la valeur d’un nom d’utilisateur et un mot de passe. Le mot de passe champ Voici le texte **connectez-vous à :** , puis du nom du domaine dans lequel l’ordinateur est joint. Par exemple, si votre domaine s’appelle example.com, le texte est lu **connectez-vous à : EXEMPLE**.

4. Dans **nom d’utilisateur**, tapez votre nom d’utilisateur de domaine.

5. Dans **Mot de passe**, tapez votre mot de passe de domaine, puis cliquez sur la flèche ou appuyez sur ENTRÉE.

>[!NOTE]
>Si le **autre utilisateur** écran n’inclut pas le texte **connectez-vous à :** et votre nom de domaine, vous devez entrer votre nom d’utilisateur dans le format *domaine\\utilisateur*. Par exemple, pour vous connecter au domaine exemple.com avec un compte nommé **utilisateur\-01**, type **exemple\\utilisateur\-01**.

### <a name="bkmk_userbootstrap"></a>Joindre le domaine et ouvrir une session à l’aide de Configuration du profil sans fil Bootstrap par les utilisateurs
Avec cette méthode, vous effectuez les étapes décrites dans la section étapes générales, puis vous indiquez votre domaine\-utilisateurs membres avec les instructions sur la façon de configurer manuellement un ordinateur sans fil avec un profil de démarrage sans fil. Le profil de démarrage sans fil permet à l’utilisateur établir une connexion sans fil et de rejoindre ensuite le domaine. Une fois que l’ordinateur est joint au domaine et redémarré, l’utilisateur peut se connecter au domaine via une connexion sans fil.

#### <a name="general-steps"></a>Étapes générales

1. Configurer un compte d’administrateur local, dans **le panneau de configuration**, pour l’utilisateur.

    >[!IMPORTANT]
    >Pour joindre un ordinateur à un domaine, l’utilisateur doit être connecté à l’ordinateur avec le compte administrateur local. Vous pouvez également, l’utilisateur doit fournir les informations d’identification pour le compte d’administrateur local pendant le processus visant à joindre l’ordinateur au domaine. En outre, l’utilisateur doit avoir un compte d’utilisateur dans le domaine auquel l’utilisateur souhaite joindre l’ordinateur. Pendant le processus visant à joindre l’ordinateur au domaine, l’utilisateur sera invité pour les informations d’identification du compte de domaine \(nom d’utilisateur et mot de passe\).

2. Fournir à vos utilisateurs de domaine avec les instructions pour configurer un profil de démarrage sans fil, comme décrit dans la procédure suivante **pour configurer un profil de démarrage sans fil**.
3. En outre, fournir aux utilisateurs les informations d’identification ordinateur local \(nom d’utilisateur et mot de passe\)et les informations d’identification de domaine \(nom de compte utilisateur de domaine et mot de passe\) sous la forme *DomainName \\Nom d’utilisateur*, ainsi que les procédures permettant de « Joindre l’ordinateur au domaine, » et « Ouvrir une session sur le domaine, » comme documenté dans Windows Server 2016 [Guide du réseau](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide).

#### <a name="to-configure-a-bootstrap-wireless-profile"></a>Pour configurer un profil de démarrage sans fil

1. Utilisez les informations d’identification fournies par votre administrateur réseau ou de la prise en charge de l’informatique professionnelle pour vous connecter à l’ordinateur avec un compte d’administrateur de l’ordinateur local.

2. Droite\-cliquez sur l’icône de réseau sur le bureau, puis cliquez sur **ouvrez Centre réseau et partage**. La boîte de dialogue **Centre Réseau et partage** s’ouvre. Dans **modifier vos paramètres de mise en réseau**, cliquez sur **configurer une nouvelle connexion ou un réseau**. Le **réseau ou une connexion** boîte de dialogue s’ouvre.

3. Cliquez sur **connecter manuellement à un réseau sans fil**, puis cliquez sur **suivant**.

4. Dans **connecter manuellement à un réseau sans fil**, dans **nom réseau**, tapez le nom SSID de point d’accès.  

5. Dans **type de sécurité**, sélectionnez le paramètre fourni par votre administrateur.

6. Dans **type de chiffrement** et **clé de sécurité**, sélectionnez ou tapez les paramètres fournis par votre administrateur.

7. Sélectionnez **lancer automatiquement cette connexion**, puis cliquez sur **suivant**.

8. Dans **a été ajouté correctement *** votre SSID réseau*, cliquez sur **modifier les paramètres de connexion**.

9. Cliquez sur **modifier les paramètres de connexion**. Le *votre SSID réseau* ouvre la boîte de dialogue Propriétés de réseau sans fil.

10. Cliquez sur le **sécurité** onglet, puis dans **choisir une méthode d’authentification réseau**, sélectionnez **EAP protégé \(PEAP\)** .

11. Cliquez sur **Paramètres**. Le **EAP protégé \(PEAP\) propriétés** page s’ouvre.

12. Dans le **EAP protégé \(PEAP\) propriétés** page, vérifiez que **valider le certificat du serveur** est ne pas sélectionnée, cliquez sur **OK** à deux reprises, et puis cliquez sur **fermer**.

13. Windows tente ensuite de se connecter au réseau sans fil. Les paramètres du profil de démarrage sans fil spécifient que vous devez fournir vos informations d’identification de domaine. Une fois que Windows vous invite à entrer un nom de compte et le mot de passe, tapez vos informations d’identification du compte de domaine comme suit :  *Nom de domaine\\nom d’utilisateur*, *mot de passe de domaine*.

##### <a name="to-join-a-computer-to-the-domain"></a>Pour joindre un ordinateur au domaine

1. Ouvrez une session sur l’ordinateur avec le compte Administrateur local.

2. Dans la zone de texte de recherche, tapez **PowerShell**. Dans les résultats de recherche, cliquez sur **Windows PowerShell**, puis cliquez sur **exécuter en tant qu’administrateur**. Windows PowerShell s’ouvre avec une invite de commandes avec élévation de privilèges.

3. Dans Windows PowerShell, tapez la commande suivante, puis appuyez sur ENTRÉE. Veillez à remplacer le nom de domaine variable avec le nom du domaine que vous souhaitez joindre.
    
    Add-Computer DomainName
    
4. Lorsque vous y êtes invité, tapez votre nom d’utilisateur de domaine et le mot de passe, puis cliquez sur **OK**.
5. Redémarrez l’ordinateur.
6. Suivez les instructions dans la section précédente [ouvrez une session sur le domaine à l’aide des ordinateurs exécutant Windows 10](#bkmk_w10).