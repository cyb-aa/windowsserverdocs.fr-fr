---
title: Déploiement de l’accès sans fil
description: Cette rubrique fait partie du Guide de mise en réseau de Windows Server 2016 « déployer l’accès sans fil authentifié 802.1 X basé sur un mot de passe »
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4b66f517-b17d-408c-828f-a3793086bc1f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 64098a152d9ba485cfed80e0d0541f0e5ea72bf2
ms.sourcegitcommit: 47a9514a68e42ac236065fd6b641204b769223d1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71127678"
---
# <a name="wireless-access-deployment"></a>Déploiement de l’accès sans fil

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Pour déployer l’accès sans fil, procédez comme suit :

- [Déployer et configurer des points d’accès sans fil](#bkmk_aps)

- [Créer un groupe de sécurité utilisateurs sans fil](#bkmk_groups)

- [Configurer des stratégies \(de réseau\) sans fil IEEE 802,11](#bkmk_policies)

- [Configurer NPSs](#bkmk_nps)

- [Joindre de nouveaux ordinateurs sans fil au domaine](#bkmk_domain)

## <a name="bkmk_aps"></a>Déployer et configurer des points d’accès sans fil

Pour déployer et configurer vos points d’accès sans fil, procédez comme suit :

- [Spécifier les fréquences de canal du point d’accès sans fil](#bkmk_channel)

- [Configurer des points d’accès sans fil](#bkmk_wirelessaps)

>[!NOTE]
>Les procédures décrites dans ce guide n'incluent aucune instruction dans le cas où la boîte de dialogue **Contrôle de compte d'utilisateur** s'ouvre pour demander votre autorisation de continuer. Si cette boîte de dialogue s’ouvre pendant que vous réalisez les procédures de ce guide, et si elle s’ouvre en réponse à vos actions, cliquez sur **Continuer**.

### <a name="bkmk_channel"></a>Spécifier les fréquences de canal du point d’accès sans fil

Lorsque vous déployez plusieurs points d’accès sans fil sur un seul site géographique, vous devez configurer des points d’accès sans fil qui ont des signaux qui se chevauchent afin d’utiliser des fréquences de canal uniques pour réduire les interférences entre les points d’accès

Vous pouvez utiliser les instructions suivantes pour vous aider à choisir les fréquences de canal qui ne sont pas en conflit avec d’autres réseaux sans fil à l’emplacement géographique de votre réseau sans fil.

- Si d’autres organisations ont des bureaux à proximité ou dans le même bâtiment que votre organisation, déterminez s’il existe des réseaux sans fil appartenant à ces organisations. Déterminez à la fois le placement et les fréquences de canal attribuées de leurs points d’accès sans fil, car vous devez attribuer différentes fréquences de canal aux points d’accès et vous devez déterminer le meilleur emplacement pour installer les points d’accès.

- Identifiez les signaux sans fil qui se chevauchent sur les étages adjacents au sein de votre organisation. Après avoir identifié les zones de couverture qui se chevauchent en dehors de et au sein de votre organisation, affectez des fréquences de canaux pour vos points d’accès sans fil, en veillant à ce que les deux points d’accès sans fil avec chevauchement de la couverture

### <a name="bkmk_wirelessaps"></a>Configurer des points d’accès sans fil

Utilisez les informations suivantes, ainsi que la documentation du produit fournie par le fabricant du point d’accès sans fil pour configurer vos points d’accès sans fil.

Cette procédure énumère les éléments généralement configurés sur un point d’accès sans fil. Les noms d’éléments peuvent varier en fonction de la personnalisation et du modèle et peuvent être différents de ceux répertoriés dans la liste suivante. Pour plus d’informations, consultez la documentation de votre point d’accès sans fil.

#### <a name="to-configure-your-wireless-aps"></a>Pour configurer vos points d’accès sans fil  

- **SSID**. Spécifiez\(le nom du réseau\) \(sans fil, par exemple ExampleWLAN\). Il s’agit du nom qui est publié sur les clients sans fil.

- **Chiffrement**. Spécifiez\-WPA2 \(Enterprise\) préféré ou\-WPA Enterprise, ainsi que \(le\) chiffrement de chiffrement AES préféré ou TKIP, en fonction des versions prises en charge par votre cartes réseau des ordinateurs clients sans fil.

- Adresse IP du **point d’accès sans fil statique.\) \(** Sur chaque point d’accès, configurez une adresse IP statique unique comprise dans la plage d’exclusion de l’étendue DHCP pour le sous-réseau. L’utilisation d’une adresse qui est exclue de l’attribution par DHCP empêche le serveur DHCP d’attribuer la même adresse IP à un ordinateur ou à un autre périphérique.

- **Masque de sous-réseau**. Configurez ce paramètre pour qu’il corresponde aux paramètres de masque de sous-réseau du réseau local auquel vous avez connecté le point d’accès sans fil.  

- **Nom DNS**. Certains points d’accès sans fil peuvent être configurés avec un nom DNS. Le service DNS sur le réseau peut résoudre les noms DNS en adresses IP. Sur chaque point d’accès sans fil qui prend en charge cette fonctionnalité, entrez un nom unique pour la résolution DNS.  

- **Service DHCP**. Si votre point d’accès sans fil\-dispose d’un service DHCP intégré, désactivez-le.  

- **Secret partagé RADIUS**. Utilisez un secret partagé RADIUS unique pour chaque AP sans fil, sauf si vous envisagez de configurer des points d’accès en tant que clients RADIUS dans NPS par groupe. Si vous envisagez de configurer des points d’accès par groupe dans NPS, le secret partagé doit être le même pour tous les membres du groupe. En outre, chaque secret partagé que vous utilisez doit être une séquence aléatoire d’au moins 22 caractères qui mélange des lettres majuscules et minuscules, des chiffres et des signes de ponctuation. Pour garantir un caractère aléatoire, vous pouvez utiliser un générateur de caractères aléatoires, tel que le générateur de caractères aléatoires trouvé dans l’Assistant **configuration de 802.1 x** de NPS, pour créer les secrets partagés.

>[!TIP]
>Enregistrez le secret partagé pour chaque point d’accès sans fil et stockez-le dans un emplacement sécurisé, tel qu’un coffre-fort d’Office. Vous devez connaître le secret partagé pour chaque point d’accès sans fil lorsque vous configurez des clients RADIUS dans le serveur NPS.  

- **Adresse IP du serveur RADIUS**. Tapez l’adresse IP du serveur exécutant NPS.

- **Port\(UDPs\)** . Par défaut, NPS utilise les ports UDP 1812 et 1645 pour les messages d’authentification et les ports UDP 1813 et 1646 pour les messages de gestion des comptes. Nous vous recommandons d’utiliser ces mêmes ports UDP sur vos points d’accès, mais si vous avez une raison valable d’utiliser des ports différents, assurez-vous que vous configurez non seulement les points d’accès avec les nouveaux numéros de port, mais également reconfigurez tous vos NPSs pour utiliser les mêmes numéros de port que les points d’accès. Si les points d’accès et les NPSs ne sont pas configurés avec les mêmes ports UDP, le serveur NPS ne peut pas recevoir ou traiter les demandes de connexion des points d’accès, et toutes les tentatives de connexion sans fil sur le réseau échoueront.

- **VSA**. Certains points d’accès sans\-fil requièrent\) des attributs \(spécifiques au fournisseur VSA pour fournir une fonctionnalité de point d’accès sans fil complète. Les attributs VSA sont ajoutés dans la stratégie de réseau NPS.

- **Filtrage DHCP**. Configurez des points d’accès sans fil pour empêcher les clients sans fil d’envoyer des paquets IP du port UDP 68 au réseau, comme indiqué par le fabricant du point d’accès sans fil.

- **Filtrage DNS**. Configurez des points d’accès sans fil pour empêcher les clients sans fil d’envoyer des paquets IP du port TCP ou UDP 53 au réseau, comme indiqué par le fabricant du point d’accès sans fil.

## <a name="create-security-groups-for-wireless-users"></a>Créer des groupes de sécurité pour les utilisateurs sans fil

Procédez comme suit pour créer un ou plusieurs groupes de sécurité utilisateurs sans fil, puis ajoutez des utilisateurs au groupe de sécurité utilisateurs sans fil approprié :

- [Créer un groupe de sécurité utilisateurs sans fil](#bkmk_groups)

- [Ajouter des utilisateurs au groupe de sécurité sans fil](#bkmk_addusers)

### <a name="bkmk_groups"></a>Créer un groupe de sécurité utilisateurs sans fil

Vous pouvez utiliser cette procédure pour créer un groupe de sécurité sans fil dans Active Directory le composant logiciel enfichable \(\-MMC\) utilisateurs et ordinateurs de Microsoft Management Console.  

Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs du domaine** ou à un groupe équivalent.

#### <a name="to-create-a-wireless-users-security-group"></a>Pour créer un groupe de sécurité utilisateurs sans fil

1. Cliquez sur **Démarrer**, **Outils d'administration**, puis sur **Utilisateurs et ordinateurs Active Directory**. Le composant logiciel enfichable\-utilisateurs et ordinateurs Active Directory s’ouvre. Si le nœud de votre domaine n’est pas encore sélectionné, cliquez dessus. Par exemple, si votre domaine est example.com, cliquez sur **example.com**.

2. Dans le volet d’informations,\-cliquez avec le bouton droit sur le dossier dans lequel vous souhaitez \(ajouter un nouveau groupe\-, par exemple, cliquez avec le bouton droit sur **utilisateurs**\), pointez sur **nouveau**, puis cliquez sur **groupe**.

3. Dans **Nouvel objet - Groupe**, tapez le nom du nouveau groupe dans la zone **Nom du groupe**. Par exemple, tapez **groupe sans fil**.

4. Dans **Étendue du groupe**, sélectionnez l'une des options suivantes :

    - **Domaine local**

    - **Généralités**

    - **Universelle**

5. Dans **Type de groupe**, sélectionnez **Sécurité**.

6. Cliquez sur **OK**.

Si vous avez besoin de plusieurs groupes de sécurité pour les utilisateurs sans fil, répétez ces étapes pour créer des groupes d’utilisateurs sans fil supplémentaires. Plus tard, vous pouvez créer des stratégies réseau individuelles dans NPS pour appliquer différentes conditions et contraintes à chaque groupe, en leur fournissant des autorisations d’accès et des règles de connectivité différentes.

### <a name="bkmk_addusers"></a>Ajouter des utilisateurs au groupe de sécurité utilisateurs sans fil

Vous pouvez utiliser cette procédure pour ajouter un utilisateur, un ordinateur ou un groupe à votre groupe de sécurité sans fil dans Active Directory le composant logiciel \(enfichable\) \-MMC utilisateurs et ordinateurs de Microsoft Management Console.

Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs du domaine** ou à un groupe équivalent.

#### <a name="to-add-users-to-the-wireless-security-group"></a>Pour ajouter des utilisateurs au groupe de sécurité sans fil

1. Cliquez sur **Démarrer**, **Outils d'administration**, puis sur **Utilisateurs et ordinateurs Active Directory**. La console MMC Utilisateurs et ordinateurs Active Directory s’ouvre. Si le nœud de votre domaine n’est pas encore sélectionné, cliquez dessus. Par exemple, si votre domaine est example.com, cliquez sur **example.com**.

2. Dans le volet d’informations,\-double-cliquez sur le dossier qui contient votre groupe de sécurité sans fil.

3. Dans le volet d’informations,\-cliquez avec le bouton droit sur le groupe de sécurité sans fil, puis cliquez sur **Propriétés**. La boîte de dialogue **Propriétés** du groupe de sécurité s’ouvre.

4. Dans l’onglet **membres** , cliquez sur **Ajouter**, puis effectuez l’une des procédures suivantes pour ajouter un ordinateur ou ajouter un utilisateur ou un groupe.

##### <a name="to-add-a-user-or-group"></a>Pour ajouter un utilisateur ou un groupe

1. Dans **Entrez les noms des objets à sélectionner**, tapez le nom de l’utilisateur ou du groupe que vous souhaitez ajouter, puis cliquez sur **OK**.

2. Pour attribuer l’appartenance à un groupe à d’autres utilisateurs ou groupes, répétez l’étape 1 de cette procédure.  

##### <a name="to-add-a-computer"></a>Pour ajouter un ordinateur

1. Cliquez sur **Types d'objet**. La boîte de dialogue **types d’objets** s’ouvre.

2. Dans **types d’objets**, sélectionnez **ordinateurs**, puis cliquez sur **OK**.

3. Dans **Entrez les noms des objets à sélectionner**, tapez le nom de l’ordinateur que vous souhaitez ajouter, puis cliquez sur **OK**.

4. Pour attribuer l’appartenance à un groupe à d’autres ordinateurs\-, répétez les étapes 1 à 3 de cette procédure.

## <a name="bkmk_policies"></a>Configurer des stratégies \(de réseau\) sans fil IEEE 802,11

Pour configurer des stratégie de groupe stratégies de réseau \(sans fil\) IEEE 802,11, procédez comme suit :

- [Ouvrir ou ajouter et ouvrir un objet stratégie de groupe](#bkmk_opengpme)

- [Activer les stratégies IEEE \(802,11\) du réseau sans fil par défaut](#bkmk_activate)

- [Configurer la nouvelle stratégie de réseau sans fil](#bkmk_policyconfig)

### <a name="bkmk_opengpme"></a>Ouvrir ou ajouter et ouvrir un objet stratégie de groupe

Par défaut, la fonctionnalité de gestion des stratégie de groupe est installée sur les ordinateurs exécutant Windows Server 2016 \(lorsque\) le rôle de serveur Active Directory Domain Services AD DS est installé et que le serveur est configuré en tant que contrôleur de domaine. La procédure suivante décrit comment ouvrir la console GPMC \(\) console de gestion des stratégies de groupe sur votre contrôleur de domaine. La procédure décrit ensuite comment ouvrir un objet de stratégie de\-groupe existant stratégie de groupe \(objet\) de stratégie de groupe pour le modifier, ou créer un nouvel objet de stratégie de groupe de domaine et l’ouvrir pour le modifier.

Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs du domaine** ou à un groupe équivalent.

#### <a name="to-open-or-add-and-open-a-group-policy-object"></a>Pour ouvrir ou ajouter et ouvrir un objet stratégie de groupe

1. Sur votre contrôleur de domaine, cliquez sur **Démarrer**, sur **Outils d’administration Windows**, puis sur **gestion des stratégie de groupe**. Le Console de gestion des stratégies de groupe s’ouvre.  

2. Dans le volet gauche, double\--cliquez sur votre forêt. Par exemple, double\--cliquez sur **forêt : example.com**.  

3. Dans le volet gauche, double\--cliquez sur **domaines**, puis\-double-cliquez sur le domaine pour lequel vous souhaitez gérer un objet stratégie de groupe. Par exemple, double\--cliquez sur **example.com**.  

4. Faites une des actions suivantes :

    -   **Pour ouvrir un objet de\-stratégie de groupe de niveau domaine existant pour la modification**, double-cliquez sur le domaine qui contient l’objet\-stratégie de groupe que vous souhaitez gérer, cliquez avec le bouton droit sur la stratégie de domaine que vous souhaitez gérer, par exemple la stratégie de domaine par défaut, puis Cliquez sur **modifier**. **Éditeur de gestion des stratégies de groupe** s’ouvre.

    -   **Pour créer un objet stratégie de groupe et l’ouvrir pour modification**, cliquez\-avec le bouton droit sur le domaine pour lequel vous souhaitez créer un nouvel objet stratégie de groupe, puis cliquez sur **créer un objet de stratégie de groupe dans ce domaine, et le lier ici**.

        Dans **nouvel**objet de stratégie de groupe, dans **nom**, tapez un nom pour le nouvel objet stratégie de groupe, puis cliquez sur **OK**.

        Cliquez\-avec le bouton droit sur votre nouvel objet stratégie de groupe, puis cliquez sur **modifier**. **Éditeur de gestion des stratégies de groupe** s’ouvre.

Dans la section suivante, vous utiliserez Éditeur de gestion des stratégies de groupe pour créer une stratégie sans fil.

### <a name="bkmk_activate"></a>Activer les stratégies IEEE \(802,11\) du réseau sans fil par défaut

Cette procédure décrit comment activer les stratégies de \(réseau sans fil IEEE 802,11\) par défaut à l’aide\)de l’éditeur de gestion des stratégies de groupe \(éditeur.

>[!NOTE]
>Une fois que vous avez activé la version **Windows Vista et les versions ultérieures** des\) stratégies IEEE 802,11 du réseau \(sans fil ou **Windows XP** , l’option version est automatiquement supprimée de la liste des options lorsque vous Cliquez\-avec le bouton droit sur **stratégies IEEE 802,11 \(\) de réseau sans fil**. Cela se produit car une fois que vous avez sélectionné une version de stratégie, la stratégie est ajoutée dans le volet d’informations de éditeur lorsque vous sélectionnez le nœud **réseau \(sans fil\) IEEE 802,11 stratégies** . Cet état reste, sauf si vous supprimez la stratégie sans fil et que la version de la stratégie sans\-fil revient au menu contextuel pour les **stratégies de réseau \(sans fil IEEE 802,11\)**  dans le éditeur. En outre, les stratégies sans fil sont répertoriées dans le volet d’informations éditeur lorsque le nœud  **\(réseau\) sans fil IEEE 802,11 stratégies** est sélectionné.

Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs du domaine** ou à un groupe équivalent.

#### <a name="to-activate-default-wireless-network-ieee-80211-policies"></a>Pour activer les stratégies de \(réseau sans\) fil par défaut IEEE 802,11  

1. Suivez la procédure précédente **pour ouvrir ou ajouter et ouvrir un objet stratégie de groupe** pour ouvrir éditeur.

2. Dans le volet gauche de la éditeur, double\--cliquez sur **Configuration ordinateur**,\-double-cliquez sur\- **stratégies**, double-cliquez sur **Paramètres Windows**, puis double\--cliquez sur **sécurité. Paramètres**.

![stratégie de groupe sans fil 802.1 x](../../../media/Wireless-GP/Wireless-GP.jpg)

3. Dans **paramètres de sécurité**,\-cliquez avec le bouton droit sur **stratégies IEEE 802,11 \(\) de réseau sans fil**, puis cliquez sur **créer une nouvelle stratégie sans fil pour Windows Vista et les versions ultérieures**. 

![Stratégie sans fil 802.1 x](../../../media/Wireless-Policy/Wireless-Policy.jpg)

4. La boîte **de dialogue Propriétés de la nouvelle stratégie de réseau sans fil** s’ouvre. Dans **nom**de la stratégie, tapez un nouveau nom pour la stratégie ou conservez le nom par défaut. Cliquez sur **OK** pour enregistrer la stratégie. La stratégie par défaut est activée et listée dans le volet d’informations de la éditeur avec le nouveau nom que vous avez fourni ou avec le nom par défaut **nouvelle stratégie de réseau sans fil**.

![Nouvelles propriétés de stratégie de réseau sans fil](../../../media/Wireless-Policy-Properties/Wireless-Policy-Properties.jpg)

5. Dans le volet d’informations,\-double-cliquez sur **nouvelle stratégie de réseau sans fil** pour l’ouvrir.

Dans la section suivante, vous pouvez effectuer la configuration de la stratégie, l’ordre des préférences du traitement de la stratégie et les autorisations réseau.

### <a name="bkmk_policyconfig"></a>Configurer la nouvelle stratégie de réseau sans fil

Vous pouvez utiliser les procédures de cette section pour configurer la stratégie \(de réseau\) sans fil IEEE 802,11. Cette stratégie vous permet de configurer les paramètres de sécurité et d’authentification, de gérer les profils sans fil et de spécifier des autorisations pour les réseaux sans fil qui ne sont pas configurés en tant que réseaux préférés.

- [Configurer un profil de connexion sans fil\-pour\-PEAP MS CHAP v2](#bkmk_configureprofile)  

- [Définir l’ordre de préférence pour les profils de connexion sans fil](#bkmk_preferenceorder)  

- [Définir des autorisations réseau](#bkmk_permissions)  

#### <a name="bkmk_configureprofile"></a>Configurer un profil de connexion sans fil\-pour\-PEAP MS CHAP v2

Cette procédure fournit les étapes nécessaires à la configuration d'\-un\-profil sans fil PEAP MS CHAP v2.  

L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

##### <a name="to-configure-a-wireless-connection-profile-for-peap-ms-chap-v2"></a>Pour configurer un profil de connexion sans fil\-pour\-PEAP MS CHAP v2

1. Dans éditeur, dans la boîte de dialogue Propriétés du réseau sans fil de la stratégie que vous venez de créer, sous l’onglet **général** et dans **Description**, tapez une brève description de la stratégie.

2. Pour spécifier que la configuration automatique WLAN est utilisée pour configurer les paramètres de carte réseau sans fil, assurez-vous que l’option **utiliser le service de configuration automatique WLAN Windows pour les clients** est sélectionnée.  

3. Dans **se connecter aux réseaux disponibles selon l’ordre des profils listés ci-dessous**, cliquez sur **Ajouter**, puis sélectionnez **infrastructure**. La boîte **de dialogue Propriétés du nouveau profil** s’ouvre.

4. Dans la boîte de dialogue**Propriétés du nouveau profil** , sous l’onglet **connexion** , dans le champ **nom du profil** , tapez un nouveau nom pour le profil. Par exemple, tapez **example.com WLAN Profile pour Windows 10**.

5. Dans **nom\(du réseau\) s \(SSID,tapezleSSIDquicorrespondàl’identificateurSSIDconfigurésurvospointsd’accèssansfil,puiscliquezsurAjouter.\)**

    Si votre déploiement utilise plusieurs identificateurs SSID et que chaque point d'accès sans fil utilise les mêmes paramètres de sécurité sans fil, répétez cette étape pour ajouter l'identificateur SSID pour chaque point d'accès sans fil auquel vous voulez appliquer ce profil.

    Si votre déploiement utilise plusieurs identificateurs SSID et que les paramètres de sécurité pour chaque identificateur SSID ne sont pas identiques, configurez un profil distinct pour chaque groupe d'identificateurs SSID utilisant les mêmes paramètres de sécurité. Par exemple, si vous avez un groupe de points d’accès sans fil configurés pour utiliser WPA2\-Enterprise et AES, et un autre groupe de points d’accès sans fil pour utiliser WPA\-Enterprise et TKIP, configurez un profil pour chaque groupe de points d’accès sans fil.

6. Si le texte par défaut **NEWSSID** est présent, sélectionnez-le, puis cliquez sur **supprimer**.

7. Si vous avez déployé des points d'accès sans fil qui sont configurés pour supprimer la balise de diffusion, sélectionnez **Se connecter même s'il ne s'agit pas d'un réseau de diffusion**.

    > [!NOTE]
    > L’activation de cette option peut créer un risque de sécurité, car les clients sans fil détectent et tentent de se connecter à un réseau sans fil. Par défaut, ce paramètre n'est pas activé.  

8. Cliquez sur l'onglet **Sécurité** , cliquez sur **Avancé**puis configurez les éléments suivants :

    1. Pour configurer les paramètres 802.1X avancés, dans **IEEE 802.1X**, sélectionnez **Appliquer les paramètres avancés IEEE 802.1X**.

        Lorsque les paramètres 802.1 x avancés sont appliqués, les valeurs par défaut pour **nombre maximal de messages\-EAPOL Start**, **période de maintien**, **période de démarrage**et période d' **authentification** sont suffisantes pour les déploiements sans fil standard. Pour cette raison, vous n’avez pas besoin de modifier les valeurs par défaut, sauf si vous avez une raison particulière de le faire.

    2. Pour activer l'authentification unique, sélectionnez **Activer l'authentification unique pour ce réseau**.

    3. Les autres valeurs par défaut dans **Authentification unique** sont suffisantes pour les déploiements sans fil standard.

    4. Dans **itinérance rapide**, si votre point d’accès sans fil est configuré\-pour une authentification préalable, sélectionnez **ce\-réseau utilise la pré-authentification**.

9. Pour spécifier que les communications sans fil répondent\-aux normes FIPS 140 2, sélectionnez **effectuer le\-chiffrement en mode certifié FIPS 140 2**.

10. Cliquez sur **OK** pour revenir à l'onglet **Sécurité**. Dans **Sélectionner les méthodes de sécurité pour ce réseau**, dans **authentification**, **sélectionnez\-WPA2 Enterprise** s’il est pris en charge par votre point d’accès sans fil et par les cartes réseau des clients sans fil. Dans le cas contraire, sélectionnez **WPA\-Enterprise**.

11. Dans **chiffrement**, s’il est pris en charge par votre point d’accès sans fil et par les cartes réseau des clients sans fil, sélectionnez **AES-CCMP**. Si vous utilisez des points d’accès et des cartes réseau sans fil prenant en charge 802.11 AC, sélectionnez **AES-GCMP**. Sinon, sélectionnez **TKIP**.

    > [!NOTE]  
    > Les paramètres d' **authentification** et de **chiffrement** doivent correspondre aux paramètres configurés sur vos points d’accès sans fil. Les paramètres par défaut pour le **mode d’authentification**, les **échecs d’authentification Max**et **le cache des informations utilisateur pour les connexions suivantes à ce réseau** sont suffisants pour les déploiements sans fil standard.  

12. Dans **Sélectionner une méthode d’authentification réseau**, sélectionnez **PEAP \(\)(Protected EAP**), puis cliquez sur **Propriétés**. La boîte de dialogue **Propriétés EAP protégées** s’ouvre.

13. Dans **Propriétés EAP protégées**, vérifiez que **l’option vérifier l’identité du serveur en validant le certificat** est sélectionnée.

14. Dans **autorités de certification racines de confiance**, sélectionnez l’autorité de \(certification\) de l’autorité de certification racine de confiance qui a émis le certificat de serveur sur votre serveur NPS.

    > [!NOTE]  
    > Ce paramètre limite aux seules autorités de certification sélectionnées les autorités de certification racines auxquelles les clients font confiance. Si aucune autorité de certification racine de confiance n’est sélectionnée, les clients font confiance à toutes les autorités de certification racines répertoriées dans leur magasin de certificats autorités de certification racines de confiance.  

15. Dans la liste **Sélectionner la méthode d’authentification** , sélectionnez **mot\-de\-passe \(sécurisé\)EAP MS CHAP v2**.

16. Cliquez sur **configurer**. Dans la boîte de dialogue **Propriétés EAP MSCHAPv2** , vérifiez que l’option **utiliser automatiquement le nom \(d’ouverture de session\) Windows et le mot de passe et le domaine si un** est sélectionné, puis cliquez sur **OK**.

17. Pour activer la reconnexion rapide PEAP, assurez-vous que l’option **activer la reconnexion rapide** est sélectionnée.

18. Pour exiger le serveur TLV TLV sur les tentatives de connexion, sélectionnez **déconnecter si le serveur ne présente pas TLV TLV**.

19. Pour spécifier que l’identité de l’utilisateur est masquée dans la première phase de l’authentification, sélectionnez **activer la confidentialité**de l’identité, puis dans la zone de texte, tapez un nom d’identité anonyme ou laissez la zone de texte vide.

    > [! NOTES
    > - La stratégie de serveur NPS pour 802.1 X sans fil doit être créée à l’aide de la **stratégie de demande de connexion**NPS. Si la stratégie NPS est créée à l’aide de la **stratégie de réseau**NPS, la confidentialité de l’identité ne fonctionnera pas.
    > - La confidentialité de l’identité EAP est assurée par certaines méthodes EAP où une identité \(vide ou anonyme différente de l’identité\) réelle est envoyée en réponse à la demande d’identité EAP. PEAP envoie l’identité deux fois lors de l’authentification. Dans la première phase, l’identité est envoyée en texte brut et cette identité est utilisée à des fins de routage, et non pour l’authentification du client. L’identité réelle, utilisée pour l’authentification, est envoyée au cours de la deuxième phase de l’authentification, au sein du tunnel sécurisé qui est établi lors de la première phase. Si la case à cocher **activer la confidentialité** de l’identité est activée, le nom d’utilisateur est remplacé par l’entrée spécifiée dans la zone de texte. Par exemple, supposons que l’option **activer la confidentialité** de l’identité est sélectionnée et que l’alias de confidentialité d’identité **anonymous** est spécifié dans la zone de texte. Pour un utilisateur disposant d’un véritable <strong>jdoe@example.com</strong>alias d’identité, l’identité envoyée lors de la première phase de <strong>anonymous@example.com</strong>l’authentification est remplacée par. La partie domaine de l’identité de la première phase n’est pas modifiée, car elle est utilisée à des fins de routage.  

20. Cliquez sur **OK** pour fermer la boîte de dialogue **Propriétés EAP protégées** .
21. Cliquez sur **OK** pour fermer l’onglet **sécurité** .
22. Si vous souhaitez créer des profils supplémentaires, cliquez sur **Ajouter**, puis répétez les étapes précédentes, en effectuant différents choix pour personnaliser chaque profil pour les clients sans fil et le réseau auxquels vous souhaitez appliquer le profil. Lorsque vous avez terminé d’ajouter des profils, cliquez sur **OK** pour fermer la boîte de dialogue Propriétés de stratégie de réseau sans fil.

Dans la section suivante, vous pouvez classer les profils de stratégie pour une sécurité optimale.

#### <a name="bkmk_preferenceorder"></a>Définir l’ordre de préférence pour les profils de connexion sans fil
Vous pouvez utiliser cette procédure si vous avez créé plusieurs profils sans fil dans votre stratégie de réseau sans fil et que vous souhaitez classer les profils pour une efficacité et une sécurité optimales.

Pour vous assurer que les clients sans fil se connectent avec le niveau de sécurité le plus élevé qu’ils peuvent prendre en charge, placez vos stratégies les plus restrictives en haut de la liste.

Par exemple, si vous avez deux profils, un pour les clients qui prennent en charge WPA2 et un pour les clients qui prennent en charge WPA, placez le profil WPA2 plus haut dans la liste. Cela permet de s’assurer que les clients qui prennent en charge WPA2 utilisent cette méthode pour la connexion plutôt que le WPA moins sécurisé.

Cette procédure fournit les étapes permettant de spécifier l’ordre dans lequel les profils de connexion sans fil sont utilisés pour connecter les clients sans fil membres du domaine aux réseaux sans fil.

L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

##### <a name="to-set-the-preference-order-for-wireless-connection-profiles"></a>Pour définir l’ordre de préférence pour les profils de connexion sans fil

1. Dans éditeur, dans la boîte de dialogue Propriétés du réseau sans fil de la stratégie que vous venez de configurer, cliquez sur l’onglet **général** .

2. Sous l’onglet **général** , dans **se connecter aux réseaux disponibles selon l’ordre des profils listés ci-dessous**, sélectionnez le profil que vous souhaitez déplacer dans la liste, puis cliquez sur le bouton « flèche haut » ou « flèche bas » pour déplacer le profil vers le emplacement dans la liste.

3.  Répétez l’étape 2 pour chaque profil que vous souhaitez déplacer dans la liste.  

4.  Cliquez sur **OK** pour enregistrer toutes les modifications.

Dans la section suivante, vous pouvez définir des autorisations réseau pour la stratégie sans fil.

#### <a name="bkmk_permissions"></a>Définir des autorisations réseau
Vous pouvez configurer les paramètres de l’onglet **autorisations réseau** pour les membres du domaine auxquels s' \(appliquent\) les stratégies IEEE 802,11 du réseau sans fil.

Vous pouvez uniquement appliquer les paramètres suivants pour les réseaux sans fil qui ne sont pas configurés sous l’onglet **général** de la page Propriétés de la **stratégie de réseau sans fil** :

- Autoriser ou refuser les connexions à des réseaux sans fil spécifiques que vous spécifiez par type de réseau \(et SSID identificateur de l’ensemble de services\)

- Autoriser ou refuser les connexions aux réseaux ad hoc

- Autoriser ou refuser les connexions aux réseaux d’infrastructure

- Autoriser ou refuser les utilisateurs à afficher les \(types de réseau ad\) hoc ou l’infrastructure à laquelle ils se voient refuser l’accès

- Autoriser ou refuser les utilisateurs à créer un profil qui s’applique à tous les utilisateurs

- Les utilisateurs peuvent se connecter uniquement aux réseaux autorisés à l’aide de profils stratégie de groupe

Pour effectuer ces procédures, il est nécessaire d’appartenir au minimum au **groupe Admins du domaine**ou à un groupe équivalent.

##### <a name="to-allow-or-deny-connections-to-specific-wireless-networks"></a>Pour autoriser ou refuser des connexions à des réseaux sans fil spécifiques

1. Dans éditeur, dans la boîte de dialogue Propriétés du réseau sans fil, cliquez sur l’onglet **autorisations réseau** .

2. Sous l’onglet **autorisations réseau** , cliquez sur **Ajouter**. La boîte **de dialogue nouvelle entrée d’autorisation** s’ouvre.

3. Dans la boîte de dialogue **nouvelle entrée d’autorisation** , dans le champ **\(nom du réseau SSID\)** , tapez l’identificateur SSID du réseau pour lequel vous souhaitez définir des autorisations.

4.  Dans **type de réseau**, sélectionnez **infrastructure** ou **ad hoc**.

    > [!NOTE]  
    > Si vous n’êtes pas certain que le réseau de diffusion est une infrastructure ou un réseau ad hoc, vous pouvez configurer deux entrées d’autorisation réseau, une pour chaque type de réseau.

5. Dans **autorisation**, sélectionnez **autoriser** ou **refuser**.

6. Cliquez sur **OK**pour revenir à l’onglet **autorisations réseau** .

##### <a name="to-specify-additional-network-permissions-optional"></a>Pour spécifier des autorisations \(réseau supplémentaires facultatives\)

1.  Sous l’onglet **autorisations réseau** , configurez l’une ou l’ensemble des éléments suivants :  

    -   Pour refuser à vos membres de domaine l’accès aux réseaux ad hoc, sélectionnez **empêcher\-les connexions aux réseaux ad hoc**.

    -   Pour refuser à vos membres de domaine l’accès aux réseaux d’infrastructure, sélectionnez **empêcher les connexions aux réseaux d’infrastructure**.  

    -   Pour permettre à vos membres de domaine d’afficher \(les types de réseau\) ad hoc ou l’infrastructure à laquelle ils se voient refuser l’accès, sélectionnez **autoriser l’utilisateur à afficher les réseaux refusés**.

    -   Pour permettre aux utilisateurs de créer des profils qui s’appliquent à tous les utilisateurs, sélectionnez **autoriser tout le monde à créer tous les profils utilisateur**.

    -   Pour spécifier que vos utilisateurs peuvent se connecter uniquement aux réseaux autorisés à l’aide de profils stratégie de groupe, sélectionnez **utiliser uniquement les profils stratégie de groupe pour les réseaux autorisés**.

## <a name="bkmk_nps"></a>Configurer votre NPSs
Procédez comme suit pour configurer NPSs afin d’effectuer l’authentification 802.1 X pour l’accès sans fil :

- [Inscrire un serveur NPS dans Active Directory Domain Services](#bkmk_npsreg)

- [Configurer un point d’accès sans fil en tant que client RADIUS NPS](#bkmk_radiusclient)

- [Créer des stratégies NPS pour 802.1 X sans fil à l’aide d’un Assistant](#bkmk_npspolicy)

### <a name="bkmk_npsreg"></a>Inscrire un serveur NPS dans Active Directory Domain Services
Vous pouvez utiliser cette procédure pour inscrire \(un serveur exécutant NPS\) (Network Policy Server) dans\) Active Directory Domain Services \(AD DS dans le domaine où le serveur NPS est membre. Pour que NPSs soit autorisé à lire les propriétés d'\-accès à distance dans les comptes d’utilisateur pendant le processus d’autorisation, chaque serveur NPS doit être inscrit dans AD DS. L’inscription d’un serveur NPS ajoute le serveur au groupe de sécurité **serveurs RAS et IAS** dans AD DS.

>[!NOTE]
>Vous pouvez installer NPS sur un contrôleur de domaine ou sur un serveur dédié. Exécutez la commande Windows PowerShell suivante pour installer le serveur NPS si vous ne l’avez pas encore fait :
    
    Install-WindowsFeature NPAS -IncludeManagementTools
    
L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

#### <a name="to-register-an-nps-in-its-default-domain"></a>Pour inscrire un serveur NPS dans son domaine par défaut

1. Sur votre serveur NPS, dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur serveur NPS ( **Network Policy Server**). Le composant logiciel\-enfichable NPS s’ouvre.

2. Cliquez\-avec le bouton droit sur  **\(NPS local\)** , puis cliquez sur **inscrire le serveur dans Active Directory**. La boîte de dialogue **Serveur NPS (Network Policy Server)** s’ouvre.

3. Dans **serveur de stratégie réseau**, cliquez sur **OK**, puis de nouveau sur **OK** .

### <a name="bkmk_radiusclient"></a>Configurer un point d’accès sans fil en tant que client RADIUS NPS
Vous pouvez utiliser cette procédure pour configurer un point d’accès, également appelé *NAS\)de serveur \(d’accès réseau*, en tant qu'\-authentification à distance \(dans\) le client RADIUS du service utilisateur à l’aide du composant logiciel enfichable NPS. \-dans. 

>[!IMPORTANT]
>Les ordinateurs clients, tels que les ordinateurs portables sans fil et autres ordinateurs exécutant des systèmes d'exploitation clients, ne sont pas des clients RADIUS. Les clients RADIUS sont des serveurs d’accès réseau, tels que les points d'\-accès sans fil, les commutateurs\) 802.1 x, les\-serveurs VPN de réseau \(privé virtuel et les serveurs d’accès à distance, car ils utilisent le protocole RADIUS pour Communiquez avec les serveurs RADIUS tels que NPSs.

L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

#### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>Pour ajouter un serveur d’accès réseau en tant que client RADIUS dans NPS

1. Sur votre serveur NPS, dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur serveur NPS ( **Network Policy Server**). Le composant logiciel\-enfichable NPS s’ouvre.

2. Dans le composant logiciel\-enfichable NPS\-, double-cliquez sur **clients et serveurs RADIUS**. Cliquez\-avec le bouton droit sur **clients RADIUS**, puis cliquez sur **nouveau**.

3. Dans **nouveau client RADIUS**, vérifiez que la case à cocher **activer ce client RADIUS** est activée.

4. Dans **nouveau client RADIUS**, dans **nom convivial**, tapez un nom d’affichage pour le point d’accès sans fil.

    Par exemple, si vous souhaitez ajouter \(un point\) d’accès sans fil nommé\-AP 01, tapez **AP\-01**.

5. Dans **adresse \(IP ou DNS\)** de l’adresse, tapez l'\) adresse IP ou le \(nom de domaine complet du nom de domaine complet pour le NAS.

    Si vous entrez le nom de domaine complet, pour vérifier que le nom est correct et qu’il correspond à une adresse IP valide, cliquez sur **vérifier**, puis dans **vérifier l’adresse**, dans le champ **adresse** , cliquez sur **résoudre**. Si le nom de domaine complet est mappé à une adresse IP valide, l’adresse IP de ce NAS apparaît automatiquement dans l' **adresse IP**. Si le nom de domaine complet n’est pas résolu en une adresse IP, vous recevrez un message indiquant qu’aucun hôte de ce type n’est connu. Si cela se produit, vérifiez que vous disposez du nom d’accès approprié et que le point d’accès est sous tension et connecté au réseau.  

    Cliquez sur **OK** pour fermer **vérifier l’adresse**.  

6. Dans **nouveau client RADIUS**, dans **secret partagé**, effectuez l’une des opérations suivantes :  

    -   Pour configurer manuellement un secret partagé RADIUS, sélectionnez **Manuel**, puis dans **secret partagé**, tapez le mot de passe fort qui est également entré sur le NAS. Retapez le secret partagé dans **confirmer le secret partagé**.  

    -   Pour générer automatiquement un secret partagé, activez la case à cocher **générer** , puis cliquez sur le bouton **générer** . Enregistrez le secret partagé généré, puis utilisez cette valeur pour configurer le NAS afin qu’il puisse communiquer avec le serveur NPS.  

        >[!IMPORTANT]
        >Le secret partagé RADIUS que vous entrez pour vos points d’accès virtuels dans NPS doit correspondre exactement au secret partagé RADIUS configuré sur vos points d’accès sans fil réels. Si vous utilisez l’option NPS pour générer un secret partagé RADIUS, vous devez configurer le AP sans fil réel correspondant avec le secret partagé RADIUS qui a été généré par le serveur NPS.

7. Dans **nouveau client RADIUS**, sous l’onglet **avancé** , dans **nom du fournisseur**, spécifiez le nom du fabricant NAS. Si vous n’êtes pas sûr du nom du fabricant NAS, sélectionnez **RADIUS standard**.

8. Dans **options supplémentaires**, si vous utilisez des méthodes d’authentification autres que EAP et PEAP et si votre NAS prend en charge l’utilisation de l’attribut de l’authentificateur de message, sélectionnez **les messages de\- demande d’accès doivent contenir le message Attribut Authenticator**.

9. Cliquez sur **OK**. Votre NAS s’affiche dans la liste des clients RADIUS configurés sur le serveur NPS.

### <a name="bkmk_npspolicy"></a>Créer des stratégies NPS pour 802.1 X sans fil à l’aide d’un Assistant
Vous pouvez utiliser cette procédure pour créer les stratégies de demande de connexion et les stratégies réseau nécessaires au déploiement\-\-des points d’accès sans fil 802.1 x en tant qu' \(authentification distante dans le rayondeservicedel’utilisateur.\) clients vers le serveur RADIUS exécutant NPS\)(Network \(Policy Server).  
Après avoir exécuté l’Assistant, les stratégies suivantes sont créées :

- Une stratégie de demande de connexion

- Une stratégie réseau

>[!NOTE]
>Vous pouvez exécuter l’Assistant Nouvelle connexion câblée et sans fil sécurisée IEEE 802.1 X chaque fois que vous devez créer des stratégies pour l’accès authentifié 802.1 X.

L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

#### <a name="create-policies-for-8021x-authenticated-wireless-by-using-a-wizard"></a>Créer des stratégies pour les réseaux sans fil authentifiés 802.1 X à l’aide d’un Assistant

1. Ouvrez le composant logiciel\-enfichable NPS. S’il n’est pas déjà sélectionné, **cliquez \(sur\)NPS local**. Si vous exécutez le composant logiciel enfichable\-MMC NPS dans et que vous souhaitez créer des stratégies sur un serveur NPS distant, sélectionnez le serveur.

2. Dans **prise en main**, dans **configuration standard**, sélectionnez **serveur RADIUS pour les connexions câblées ou sans fil 802.1 x**. Le texte et les liens situés sous le texte changent pour refléter votre sélection.

3. Cliquez sur **configurer 802.1 x**. L’Assistant Configuration de 802.1 X s’ouvre.

4.  Sur la page **Sélectionner le type de connexions 802.1 x** , **dans type de connexions 802.1 x**, sélectionnez **connexions sans fil sécurisées**, et dans **nom**, tapez un nom pour votre stratégie ou laissez le nom par défaut **connexions sans fil sécurisées** . . Cliquez sur **Suivant**.

5.  Dans la page **spécifier les commutateurs 802.1 x** de l’Assistant, dans **clients RADIUS**, tous les commutateurs 802.1 x et les points d’accès sans fil que vous\-avez ajoutés en tant que clients RADIUS dans le composant logiciel enfichable NPS sont affichés. Effectuez l’une des actions suivantes :

    -   Pour ajouter des serveurs \(\)d’accès réseau supplémentaires, tels que des points d’accès sans fil, dans **clients RADIUS**, cliquez sur **Ajouter**, puis dans **nouveau client RADIUS**, entrez les informations pour : **Nom convivial**, **adresse \(IP ou DNS\)** , et **secret partagé**.

    -   Pour modifier les paramètres d’un NAS, dans **clients RADIUS**, sélectionnez le point d’accès pour lequel vous souhaitez modifier les paramètres, puis cliquez sur **modifier**. Modifiez les paramètres selon vos besoins.

    -   Pour supprimer un NAS de la liste, dans **clients RADIUS**, sélectionnez le NAS, puis cliquez sur **supprimer**.

        >[!WARNING]
        >La suppression d’un client RADIUS dans l’Assistant **configuration de 802.1 x** supprime le client de la configuration du serveur NPS. Tous les ajouts, modifications et suppressions effectués au sein de l’Assistant **configuration de 802.1 x** aux clients RADIUS sont reflétés dans\-le composant logiciel enfichable NPS dans le \/ nœud **clients RADIUS** sous **clients RADIUS NPS. et les serveurs**. Par exemple, si vous utilisez l’Assistant pour supprimer un commutateur 802.1 x, le commutateur est également supprimé du composant logiciel enfichable\-NPS.

6. Cliquez sur **Suivant**. Dans la page **configurer une méthode d’authentification** de l’Assistant, dans **type \(en fonction de la méthode\)d’accès et de la configuration réseau**, sélectionnez **Microsoft : \(PEAP PEAP\),puiscliquez sur **configurer.****

    >[!TIP]
    >Si vous recevez un message d’erreur indiquant qu’un certificat est introuvable pour une utilisation avec la méthode d’authentification et que vous avez configuré Active Directory Services de certificats pour émettre automatiquement des certificats pour les serveurs RAS et IAS sur votre réseau, commencez par Assurez-vous que vous avez suivi les étapes d’inscription de NPS dans Active Directory Domain Services, puis procédez comme suit pour mettre à jour stratégie de groupe : Cliquez **sur Démarrer**, **sur système Windows**, sur **exécuter**, puis dans **ouvrir**, tapez **gpupdate**et appuyez sur entrée. Lorsque la commande renvoie des résultats indiquant que les stratégie de groupe d’utilisateur et d’ordinateur ont été **correctement mis à jour, sélectionnez Microsoft : À nouveau \(Protected EAP** PEAP\) , puis cliquez sur **configurer**.
    >
    >Si, après actualisation stratégie de groupe, vous continuez à recevoir le message d’erreur indiquant qu’un certificat est introuvable pour une utilisation avec la méthode d’authentification, le certificat n’est pas affiché, car il ne correspond pas au certificat de serveur minimal Configuration requise décrite dans le Guide d’accompagnement du réseau de base : [Déployez des certificats de serveur pour les déploiements sans fil et câblés 802.1 x](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments). Dans ce cas, vous devez arrêter la configuration de NPS, révoquer le certificat délivré à\(votre\)serveur NPS, puis suivre les instructions pour configurer un nouveau certificat à l’aide du Guide de déploiement des certificats de serveur.

7.  Dans la page **modifier les propriétés EAP protégé** , dans **certificat émis**, vérifiez que le certificat NPS correct est sélectionné, puis procédez comme suit :

    >[!NOTE]
    >Vérifiez que la valeur dans **émetteur** est correcte pour le certificat sélectionné dans **certificat émis**. Par exemple, l’émetteur attendu pour un certificat émis par une autorité de certification exécutant Active Directory Services \(de certificats\) AD CS nommé corp\DC1 dans le domaine contoso.com, **est\-Corp\-DC1 CA**.

    -   Pour permettre aux utilisateurs de se déplacer avec leurs ordinateurs sans fil entre des points d’accès sans avoir à les réauthentifier chaque fois qu’ils sont associés à un nouveau AP, sélectionnez **activer la reconnexion rapide**.

    -   Pour spécifier que la connexion des clients sans fil met fin au processus d’authentification réseau si le serveur RADIUS ne\-présente pas \(la\)valeur de longueur\-de type TLV TLV, sélectionnez **déconnecter les clients sans TLV**.  

    -   Pour modifier les paramètres de stratégie pour le type EAP, dans **types EAP**, cliquez sur **modifier**, dans **Propriétés EAP MSCHAPv2**, modifiez les paramètres selon vos besoins, puis cliquez sur **OK**.  

8.  Cliquez sur **OK**. La boîte de dialogue Modifier les propriétés EAP protégé se ferme et vous ramène à l’Assistant **configuration d' 802.1 x** . Cliquez sur **Suivant**.

9. Dans **spécifier des groupes d’utilisateurs**, cliquez sur **Ajouter**, puis tapez le nom du groupe de sécurité que vous avez configuré pour vos clients sans fil dans le composant\-logiciel enfichable utilisateurs et ordinateurs Active Directory. Par exemple, si vous avez nommé Groupe de sécurité sans fil groupe sans fil, tapez **groupe sans fil**. Cliquez sur **Suivant**.

10. Cliquez **sur Configurer** pour configurer les attributs standard RADIUS\-et les attributs spécifiques au \(fournisseur\) pour le VLAN de réseau local virtuel en fonction des besoins, et comme indiqué dans la documentation fournie par votre fournisseur de matériel de point d’accès sans fil. Cliquez sur **Suivant**.

11. Passez en revue les détails du résumé de la configuration, puis cliquez sur **Terminer**.

Vos stratégies NPS sont maintenant créées, et vous pouvez passer à la jonction des ordinateurs sans fil au domaine.

## <a name="bkmk_domain"></a>Joindre de nouveaux ordinateurs sans fil au domaine
La méthode la plus simple pour joindre de nouveaux ordinateurs sans fil au domaine consiste à attacher physiquement l’ordinateur à un segment du \(réseau local câblé un segment non contrôlé par un\) commutateur 802.1 x avant de joindre l’ordinateur au domaine. Cela est plus facile, car les paramètres de stratégie de groupe sans fil sont appliqués automatiquement et immédiatement et, si vous avez déployé votre propre infrastructure à clé publique, l’ordinateur reçoit le certificat d’autorité de certification et le place dans le magasin de certificats autorités de certification racines de confiance, autoriser le client sans fil à approuver NPSs avec les certificats de serveur émis par votre autorité de certification.

De même, une fois qu’un nouvel ordinateur sans fil est joint au domaine, la méthode recommandée pour que les utilisateurs se connectent au domaine consiste à ouvrir une session à l’aide d’une connexion câblée au réseau.

### <a name="other-domain-join-methods"></a>Autres méthodes de jointure de domaine
Dans les cas où il n’est pas pratique de joindre des ordinateurs au domaine à l’aide d’une connexion Ethernet câblée, ou dans les cas où l’utilisateur ne peut pas se connecter au domaine pour la première fois à l’aide d’une connexion câblée, vous devez utiliser une autre méthode.

- **Configuration de l’ordinateur du personnel informatique**. Un membre du personnel informatique joint un ordinateur sans fil au domaine et configure un profil de démarrage sans fil d’authentification unique. Avec cette méthode, l’administrateur informatique connecte l’ordinateur sans fil au réseau Ethernet câblé et joint l’ordinateur au domaine. Ensuite, l’administrateur distribue l’ordinateur à l’utilisateur. Lorsque l’utilisateur démarre l’ordinateur sans utiliser de connexion câblée, les informations d’identification de domaine qu’il spécifie manuellement pour l’ouverture de session de l’utilisateur sont utilisées pour établir une connexion au réseau sans fil et pour se connecter au domaine.

Pour plus d’informations, consultez la section [joindre le domaine et ouvrir une session à l’aide de la méthode de configuration de l’ordinateur du personnel informatique](#bkmk_itstaff)

-   **Configuration du profil de démarrage sans fil par les utilisateurs**. L’utilisateur configure manuellement l’ordinateur sans fil avec un profil de démarrage sans fil et rejoint le domaine, en fonction des instructions acquises auprès d’un administrateur informatique. Le profil de démarrage sans fil permet à l’utilisateur d’établir une connexion sans fil, puis de joindre le domaine. Une fois l’ordinateur joint au domaine et le redémarrage de l’ordinateur, l’utilisateur peut ouvrir une session sur le domaine à l’aide d’une connexion sans fil et des informations d’identification de son compte de domaine.

Pour plus d’informations, consultez la section [joindre le domaine et ouvrir une session à l’aide de la configuration du profil de démarrage sans fil par les utilisateurs](#bkmk_userbootstrap).

### <a name="bkmk_itstaff"></a>Joindre le domaine et ouvrir une session à l’aide de la méthode de configuration de l’ordinateur du personnel informatique
Les utilisateurs membres d’un\-domaine avec des ordinateurs clients sans fil joints à un domaine peuvent utiliser un profil sans\-fil temporaire pour se connecter à un réseau sans fil authentifié 802.1 x sans se connecter d’abord au réseau local câblé. Ce profil sans fil temporaire est appelé *profil de démarrage sans fil*.

Un profil de démarrage sans fil oblige l’utilisateur à spécifier manuellement les informations d’identification de son compte d’utilisateur de domaine et ne valide pas le\-certificat du serveur \(d'\) accès à distance au serveur RADIUS du service utilisateur en cours d’exécution \(NPS\)(Network Policy Server).

Une fois la connectivité sans fil établie, stratégie de groupe est appliqué sur l’ordinateur client sans fil et un nouveau profil sans fil est émis automatiquement. La nouvelle stratégie utilise les informations d’identification du compte d’utilisateur et d’ordinateur pour l’authentification du client. 

En outre, dans le cadre de l'\-authentification\-mutuelle PEAP MS CHAP v2 à l’aide du nouveau profil au lieu du profil de démarrage, le client valide les informations d’identification du serveur RADIUS.

Une fois l’ordinateur joint au domaine, procédez comme suit pour configurer un profil de démarrage sans fil d’authentification unique avant de distribuer l’ordinateur sans fil à\-l’utilisateur membre du domaine.

#### <a name="to-configure-a-single-sign-on-bootstrap-wireless-profile"></a>Pour configurer un profil de démarrage sans fil d’authentification unique

1. Créez un profil de démarrage à l’aide de la procédure décrite dans ce guide intitulée [configurer un profil\-de\-connexion sans fil pour PEAP MS CHAP v2](#bkmk_configureprofile)et utilisez les paramètres suivants :

    - Authentification\-PEAP\-MS CHAP v2

    - Valider le certificat de serveur RADIUS désactivé

    - Authentification unique activée

2. Dans les propriétés de la stratégie de réseau sans fil dans laquelle vous avez créé le nouveau profil de démarrage, sous l’onglet **général** , sélectionnez le profil de démarrage, puis cliquez sur **Exporter** pour exporter le profil vers un partage réseau, un disque mémoire flash USB ou tout autre fichier facilement emplacement accessible. Le profil est enregistré sous la forme d’un fichier *. XML à l’emplacement que vous spécifiez.

3. Joindre le nouvel ordinateur sans fil au domaine \(, par exemple, via une connexion Ethernet qui ne requiert pas l’authentification\) IEEE 802.1 x et ajouter le profil de démarrage sans fil à l’ordinateur à l’aide du **profil d’ajout netsh wlan** commande.

    >[!NOTE]
    >Pour plus d’informations, consultez commandes \(netsh pour réseau local sans fil WLAN\) à l’adresse [http :\/\/technet.Microsoft.com\/Library\/dd744890. aspx](https://technet.microsoft.com/library/dd744890).

4. Distribuez le nouvel ordinateur sans fil à l’utilisateur avec la procédure « ouvrir une session sur le domaine à l’aide d’ordinateurs exécutant Windows 10 ».

Lorsque l’utilisateur démarre l’ordinateur, Windows invite l’utilisateur à entrer son nom de compte d’utilisateur de domaine et son mot de passe. Étant donné que l’authentification unique est activée, l’ordinateur utilise les informations d’identification du compte d’utilisateur de domaine pour établir d’abord une connexion avec le réseau sans fil, puis ouvrir une session sur le domaine.

#### <a name="bkmk_w10"></a>Ouvrir une session sur le domaine à l’aide d’ordinateurs exécutant Windows 10

1. Fermez la session sur l’ordinateur et redémarrez-le.

2. Appuyez sur n’importe quelle touche de votre clavier ou cliquez sur le bureau. L’écran d’ouverture de session s’affiche avec un nom de compte d’utilisateur local affiché et un champ d’entrée de mot de passe sous le nom. Ne vous connectez pas avec le compte d’utilisateur local.

3. Dans le coin inférieur gauche de l’écran, cliquez sur **autre utilisateur**. L’écran d’ouverture de session de l’autre utilisateur s’affiche avec deux champs, un pour nom d’utilisateur et un pour mot de passe. Sous le champ mot de passe se trouve le texte se **connecter à :** , puis le nom du domaine dans lequel l’ordinateur est joint. Par exemple, si votre domaine s’intitule example.com, le texte **lit se connecter à : EXEMPLE**:

4. Dans **nom d’utilisateur**, tapez votre nom d’utilisateur de domaine.

5. Dans **Mot de passe**, tapez votre mot de passe de domaine, puis cliquez sur la flèche ou appuyez sur ENTRÉE.

>[!NOTE]
>Si l' **autre écran utilisateur** n’inclut pas le texte de **connexion à :** et votre nom de domaine, vous devez entrer votre nom d’utilisateur au *format\\domaine utilisateur*. Par exemple, pour ouvrir une session sur le domaine example.com avec un compte **nommé\-User 01**, **tapez\\example\-User 01**.

### <a name="bkmk_userbootstrap"></a>Joindre le domaine et ouvrir une session à l’aide de la configuration du profil de démarrage sans fil par les utilisateurs
Avec cette méthode, vous effectuez les étapes de la section étapes générales, puis vous fournissez aux\-utilisateurs membres de domaine les instructions relatives à la configuration manuelle d’un ordinateur sans fil avec un profil de démarrage sans fil. Le profil de démarrage sans fil permet à l’utilisateur d’établir une connexion sans fil, puis de joindre le domaine. Une fois l’ordinateur joint au domaine et redémarré, l’utilisateur peut ouvrir une session sur le domaine via une connexion sans fil.

#### <a name="general-steps"></a>Étapes générales

1. Configurez un compte d’administrateur d’ordinateur local, dans le **panneau de configuration**, pour l’utilisateur.

    >[!IMPORTANT]
    >Pour joindre un ordinateur à un domaine, l’utilisateur doit avoir ouvert une session sur l’ordinateur avec le compte d’administrateur local. L’utilisateur doit également fournir les informations d’identification du compte d’administrateur local au cours du processus de jonction de l’ordinateur au domaine. En outre, l’utilisateur doit disposer d’un compte d’utilisateur dans le domaine auquel l’utilisateur souhaite joindre l’ordinateur. Pendant le processus de jonction de l’ordinateur au domaine, l’utilisateur est invité à fournir les informations d’identification du \(compte de domaine nom\)d’utilisateur et mot de passe.

2. Fournissez à vos utilisateurs de domaine les instructions de configuration d’un profil de démarrage sans fil, comme indiqué dans la procédure suivante **pour configurer un profil de démarrage sans fil**.
3. En \(outre, fournissez aux utilisateurs le nom d’utilisateur et le mot de passe\)des informations d’identification de l’ordinateur local \(, ainsi que\) le nom et le mot de passe du compte d’utilisateur de domaine et du mot de passe sous la forme *nomdomaine\\Nom d’utilisateur*, ainsi que les procédures pour « joindre l’ordinateur au domaine » et pour « ouvrir une session sur le domaine », comme indiqué dans le [Guide du réseau de base](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)de Windows Server 2016.

#### <a name="to-configure-a-bootstrap-wireless-profile"></a>Pour configurer un profil de démarrage sans fil

1. Utilisez les informations d’identification fournies par votre administrateur réseau ou votre professionnel du support technique pour ouvrir une session sur l’ordinateur avec le compte d’administrateur de l’ordinateur local.

2. Cliquez\-avec le bouton droit sur l’icône réseau sur le bureau, puis cliquez sur **ouvrir le centre réseau et partage**. La boîte de dialogue **Centre Réseau et partage** s’ouvre. Dans **modifier vos paramètres de mise en réseau**, cliquez sur **configurer une nouvelle connexion ou un nouveau réseau**. La boîte **de dialogue Configurer une connexion ou un réseau** s’ouvre.

3. Cliquez sur **se connecter manuellement à un réseau sans fil**, puis cliquez sur **suivant**.

4. Dans **connexion manuelle à un réseau sans fil**, dans **nom du réseau**, tapez le nom SSID du point d’accès.  

5. Dans **type de sécurité**, sélectionnez le paramètre fourni par votre administrateur.

6. Dans **type de chiffrement** et **clé de sécurité**, sélectionnez ou tapez les paramètres fournis par votre administrateur.

7. Sélectionnez **Démarrer automatiquement cette connexion**, puis cliquez sur **suivant**.

8. Dans **ajouté avec succès * * * votre SSID réseau*, cliquez sur **modifier les paramètres de connexion**.

9. Cliquez sur **modifier les paramètres de connexion**. La boîte *de dialogue de votre* réseau sans fil SSID réseau s’ouvre.

10. Cliquez sur l’onglet **sécurité** , puis dans **choisir une méthode d’authentification réseau**, sélectionnez **PEAP \(\)EAP protégé**.

11. Cliquez sur **Paramètres**. La page de **propriétés\)PEAPPEAP s’ouvre. \(**

12. Dans la page **propriétés \(PEAP\) PEAP** , assurez-vous que l’option **valider le certificat du serveur** n’est pas sélectionnée, cliquez deux fois sur **OK** , puis cliquez sur **Fermer**.

13. Windows tente alors de se connecter au réseau sans fil. Les paramètres du profil de démarrage sans fil spécifient que vous devez fournir vos informations d’identification de domaine. Lorsque Windows vous invite à entrer un nom de compte et un mot de passe, tapez les informations d’identification de votre compte de domaine comme suit :  *Nom de domaine nom d'utilisateur,motdepassededomaine.\\*

##### <a name="to-join-a-computer-to-the-domain"></a>Pour joindre un ordinateur au domaine

1. Ouvrez une session sur l’ordinateur avec le compte Administrateur local.

2. Dans la zone de texte Rechercher, tapez **PowerShell**. Dans les résultats de la recherche, cliquez avec le bouton droit sur **Windows PowerShell**, puis cliquez sur **exécuter en tant qu’administrateur**. Windows PowerShell s’ouvre avec une invite de commandes avec élévation de privilèges.

3. Dans Windows PowerShell, tapez la commande suivante, puis appuyez sur entrée. Veillez à remplacer la variable DomainName par le nom du domaine que vous souhaitez joindre.
    
    Add-Computer NomDomaine
    
4. Lorsque vous y êtes invité, tapez le nom d’utilisateur et le mot de passe de votre domaine, puis cliquez sur **OK**.
5. Redémarrez l’ordinateur.
6. Suivez les instructions de la section précédente [pour ouvrir une session sur le domaine à l’aide d’ordinateurs exécutant Windows 10](#bkmk_w10).
