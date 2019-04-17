---
title: Déploiement de l’accès sans fil
description: Cette rubrique fait partie du guide de mise en réseau de Windows Server2016 «Accès sans fil authentifié 802. 1-mot de passe de déployer X»
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4b66f517-b17d-408c-828f-a3793086bc1f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f78da14b91e5c1e9a2dc22f92ea95c89153befa8
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="wireless-access-deployment"></a>Déploiement de l’accès sans fil

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Suivez ces étapes pour déployer l’accès sans fil:

- [Déployer et configurer des points d’accès sans fil](#bkmk_aps)

- [Créer un groupe de sécurité utilisateurs sans fil](#bkmk_groups)

- [Configurer le réseau sans fil \ IEEE (802.11\) les stratégies](#bkmk_policies)

- [Configurez les serveurs NPS](#bkmk_nps)

- [Joindre des nouveaux ordinateurs sans fil au domaine](#bkmk_domain)

## <a name="bkmk_aps"></a>Déployer et configurer des points d’accès sans fil

Suivez ces étapes pour déployer et configurer vos points d’accès sans fil:

- [Spécifier les fréquences de canaux de point d’accès sans fil](#bkmk_channel)

- [Configurer les points d’accès sans fil](#bkmk_wirelessaps)

>[!NOTE]
>Les procédures décrites dans ce guide n’incluent pas d’instructions pour les cas dans lesquels le **contrôle de compte d’utilisateur** boîte de dialogue s’ouvre pour demander votre autorisation pour continuer. Si cette boîte de dialogue s’ouvre pendant que vous effectuez les procédures décrites dans ce guide et si la boîte de dialogue a été ouverte en réponse à vos actions, cliquez sur **continuer**.

### <a name="bkmk_channel"></a>Spécifier les fréquences de canaux de point d’accès sans fil

Lorsque vous déployez plusieurs points d’accès sans fil sur un seul site géographique, vous devez configurer les points d’accès sans fil qui ont les fréquences de canaux unique permet de réduire les interférences entre les points d’accès sans fil des signaux qui se chevauchent.

Vous pouvez utiliser les instructions suivantes pour vous aider à choisir les fréquences de canaux qui ne sont pas en conflit avec d’autres réseaux sans fil à l’emplacement géographique de votre réseau sans fil.

- S’il existe des autres organisations qui ont des bureaux dans proximité ou dans le même bâtiment que votre organisation, identifient s’il existe des réseaux sans fil détenus par ces organisations. Découvrez le positionnement et le canal affecté les fréquences de leur point d’accès, car vous devez affecter les fréquences de canaux différents pour votre point d’accès et vous devez déterminer le meilleur emplacement pour installer votre point d’accès

- Identifiez le chevauchement de signaux sans fil sur étages située en regard de votre organisation. Une fois l’identification chevauchement de couverture à l’extérieur et au sein de votre organisation, attribuer les fréquences de canaux pour vos points d’accès sans fil, veiller à ce que les deux points d’accès sans fil avec la couverture qui se chevauchent sont affectés des fréquences autre canal.

### <a name="bkmk_wirelessaps"></a>Configurer les points d’accès sans fil

Utilisez les informations suivantes, ainsi que la documentation fournie par le fabricant de point d’accès sans fil pour configurer vos points d’accès sans fil.

Cette procédure énumère les éléments généralement configurés sur un point d’accès sans fil. Les noms des éléments peuvent varier selon la marque et le modèle et peuvent être différentes de celles de la liste suivante. Pour plus d’informations, voir la documentation de votre point d’accès sans fil.

#### <a name="to-configure-your-wireless-aps"></a>Pour configurer vos points d’accès sans fil  

- **SSID**. Spécifiez le nom du network\(s\) sans fil \ (par exemple, ExampleWLAN\). C’est le nom est publié auprès des clients sans fil.

- **Chiffrement**. Spécifiez \(preferred\) WPA2\-entreprise ou WPA\-entreprise et AES \(preferred\) ou chiffrement TKIP, en fonction des versions sont prises en charge par vos cartes réseau d’ordinateur client sans fil.

- **Sans fil PA IP adresse \(static\)**. Sur chaque point d’accès, configurez une adresse IP statique unique qui se trouve dans la plage d’exclusion de l’étendue DHCP pour le sous-réseau. À l’aide d’une adresse qui est exclue de l’attribution par DHCP empêche le serveur DHCP d’attribuer la même adresse IP à un ordinateur ou un autre périphérique.

- **Masque de sous-réseau**. Configurer pour faire correspondre les paramètres de masque de sous-réseau du réseau local auquel vous êtes connecté au point d’accès sans fil.  

- **Nom DNS**. Certains points d’accès sans fil peuvent être configurés avec un nom DNS. Le service DNS sur le réseau peut résoudre les noms DNS en une adresse IP. Sur chaque point d’accès sans fil qui prend en charge cette fonctionnalité, entrez un nom unique pour la résolution DNS.  

- **Service DHCP**. Si votre point d’accès sans fil a un service DHCP intégrée, désactivez-la.  

- **Secret partagé RADIUS**. Utiliser un secret partagé RADIUS unique pour chaque point d’accès sans fil, sauf si vous envisagez de configurer des points d’accès en tant que Clients RADIUS dans NPS par groupe. Si vous envisagez de configurer des points d’accès par groupe dans NPS, le secret partagé doit être le même pour chaque membre du groupe. En outre, chaque secret partagé que vous utilisez doit être une séquence aléatoire d’au moins 22caractères qui combine des majuscules et minuscules, des chiffres et signes de ponctuation. Pour garantir le caractère aléatoire, vous pouvez utiliser un générateur de caractères aléatoires, tels que le Générateur de caractères aléatoires trouvé dans le serveur NPS **configurer 802. 1 X** Assistant pour créer les secrets partagés.

>[!TIP]
>Enregistrez le secret partagé pour chaque point d’accès sans fil et le stocker dans un emplacement sécurisé, comme un bureau sécurisé. Lorsque vous configurez des clients RADIUS dans le serveur NPS, vous devez connaître le secret partagé pour chaque point d’accès sans fil.  

- **Adresse IP du serveur RADIUS**. Tapez l’adresse IP du serveur exécutant NPS.

- **UDP port\(s\)**. Par défaut, NPS utilise les ports UDP1812 et 1645 pour les messages d’authentification et ports UDP1813 et 1646 pour les messages de la gestion des comptes. Il est recommandé que vous utilisez ces mêmes ports UDP sur vos points d’accès, mais si vous avez une raison valable pour utiliser des ports différents, vérifiez que vous n’est pas seulement configurez les points d’accès avec les nouveaux numéros de port, mais également reconfigurez tous vos serveurs NPS pour utiliser les mêmes numéros de port en tant que les points d’accès. Si les points d’accès et les serveurs NPS ne sont pas configurés avec les mêmes ports UDP, NPS ne peut pas recevoir ou traiter les demandes de connexion à partir de points d’accès, et toutes les tentatives de connexion sans fil sur le réseau échoue.

- **Au**. Certains points d’accès sans fil nécessitent \(VSAs\) des attributs spécifiques vendor\ pour fournir une fonctionnalité de point d’accès complet sans fil. Au est ajouté dans la stratégie de réseau NPS.

- **Filtrage DHCP**. Configurer des points d’accès sans fil pour bloquer des clients sans fil d’envoyer des paquets IP depuis le port UDP 68 au réseau, comme indiqué par le fabricant de point d’accès sans fil.

- **Filtrage DNS**. Configurer des points d’accès sans fil pour bloquer des clients sans fil d’envoyer des paquets IP à partir de port TCP ou UDP 53 au réseau, comme indiqué par le fabricant de point d’accès sans fil.

## <a name="create-security-groups-for-wireless-users"></a>Créer des groupes de sécurité pour les utilisateurs sans fil

Suivez ces étapes pour créer des groupes de sécurité d’un ou plusieurs utilisateurs sans fil, puis ajouter des utilisateurs au groupe de sécurité utilisateurs sans fil appropriés:

- [Créer un groupe de sécurité utilisateurs sans fil](#bkmk_groups)

- [Ajouter des utilisateurs au groupe de sécurité sans fil](#bkmk_addusers)

### <a name="bkmk_groups"></a>Créer un groupe de sécurité utilisateurs sans fil

Vous pouvez utiliser cette procédure pour créer un groupe de sécurité sans fil dans les utilisateurs ActiveDirectory et les ordinateurs MicrosoftManagement Console \(MMC\) snap\-dans.  

L’appartenance au groupe **Admins du domaine**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

#### <a name="to-create-a-wireless-users-security-group"></a>Pour créer un groupe de sécurité utilisateurs sans fil

1. Cliquez sur **Démarrer**, cliquez sur **outils d’administration**, puis cliquez sur **ActiveDirectory Users and Computers**. L’ActiveDirectory Users and Computers enfichable s’ouvre. Si elle n’est pas déjà sélectionnée, cliquez sur le nœud de votre domaine. Par exemple, si votre domaine example.com, cliquez sur **example.com**.

2. Dans le volet d’informations, cliquez droit sur le dossier dans lequel vous souhaitez ajouter un nouveau groupe \ (par exemple, clic droit **utilisateurs**\), pointez sur **New**, puis cliquez sur **groupe**.

3. Dans **nouvel objet – groupe**, dans **nom du groupe**, tapez le nom du nouveau groupe. Par exemple, tapez **groupe sans fil**.

4. Dans **étendue du groupe**, sélectionnez une des options suivantes:

    - **Domaine local**

    - **Global**

    - **Universel**

5. Dans **type de groupe**, sélectionnez **sécurité**.

6. Cliquez sur **OK**.

Si vous avez besoin de plusieurs groupes de sécurité pour les utilisateurs sans fil, répétez ces étapes pour créer des groupes d’utilisateurs sans fil supplémentaires. Ultérieurement, vous pouvez créer des stratégies de réseau dans NPS pour appliquer des conditions différentes et contstraints à chaque groupe, en leur fournissant des autorisations d’accès différentes et des règles de connectivité.

### <a name="bkmk_addusers"></a>Ajouter des utilisateurs au groupe de sécurité utilisateurs sans fil

Vous pouvez utiliser cette procédure pour ajouter un utilisateur, ordinateur ou groupe à votre groupe de sécurité sans fil dans les utilisateurs ActiveDirectory et les ordinateurs MicrosoftManagement Console \(MMC\) snap\-dans.

L’appartenance au groupe **Admins du domaine**, ou équivalent est le minimum requis pour effectuer cette procédure.

#### <a name="to-add-users-to-the-wireless-security-group"></a>Pour ajouter des utilisateurs au groupe de sécurité sans fil

1. Cliquez sur **Démarrer**, cliquez sur **outils d’administration**, puis cliquez sur **ActiveDirectory Users and Computers**. L’ActiveDirectory MMC Utilisateurs et ordinateurs s’ouvre. Si elle n’est pas déjà sélectionnée, cliquez sur le nœud de votre domaine. Par exemple, si votre domaine example.com, cliquez sur **example.com**.

2. Dans le volet détails, double-cliquant sur le dossier qui contient votre groupe de sécurité sans fil.

3. Dans le volet d’informations, cliquez droit sur le groupe de sécurité sans fil, puis cliquez sur **propriétés**. Le **propriétés** ouvre la boîte de dialogue pour le groupe de sécurité.

4. Sur le **membres**, cliquez sur **ajouter**, puis effectuez une des procédures suivantes pour ajouter un ordinateur ou ajouter un utilisateur ou un groupe.

##### <a name="to-add-a-user-or-group"></a>Pour ajouter un utilisateur ou un groupe

1. Dans **Entrez les noms des objets à sélectionner**, tapez le nom de l’utilisateur ou le groupe que vous souhaitez ajouter, puis cliquez sur **OK**.

2. Pour attribuer l’appartenance aux groupes vers d’autres utilisateurs ou les groupes, répétez l’étape1 de cette procédure.  

##### <a name="to-add-a-computer"></a>Pour ajouter un ordinateur

1. Cliquez sur **Types d’objet**. Le **Types d’objet** boîte de dialogue s’ouvre.

2. Dans **types d’objet**, sélectionnez **ordinateurs**, puis cliquez sur **OK**.

3. Dans **Entrez les noms des objets à sélectionner**, tapez le nom de l’ordinateur que vous souhaitez ajouter, puis cliquez sur **OK**.

4. Pour attribuer l’appartenance aux groupes vers d’autres ordinateurs, répétez les étapes1\-3 de cette procédure.

## <a name="bkmk_policies"></a>Configurer le réseau sans fil \ IEEE (802.11\) les stratégies

Suivez ces étapes pour configurer un réseau sans fil \ IEEE (802.11\) extension de stratégie de groupe:

- [Ouvrir ou ajouter et ouvrir un objet de stratégie de groupe](#bkmk_opengpme)

- [Activer le réseau sans fil par défaut \ IEEE (802.11\) les stratégies](#bkmk_activate)

- [Configurer la nouvelle stratégie de réseau sans fil](#bkmk_policyconfig)

### <a name="bkmk_opengpme"></a>Ouvrir ou ajouter et ouvrir un objet de stratégie de groupe

Par défaut, la fonctionnalité Gestion de stratégie de groupe est installée sur les ordinateurs exécutant Windows Server2016 lorsque le rôle de serveur Services de domaine ActiveDirectory \(ADDS\) est installé et le serveur est configuré comme contrôleur de domaine. La procédure suivante décrit comment ouvrir la Console de gestion de stratégie de groupe \(GPMC\) sur votre contrôleur de domaine. La procédure puis décrit comment à choisir d’ouvrir une stratégie de groupe au niveau domain\ existante \(GPO\) d’objet pour le modifier, ou créer un nouveau domaine, stratégie de groupe et ouvrir pour modification.

L’appartenance au groupe **Admins du domaine**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

#### <a name="to-open-or-add-and-open-a-group-policy-object"></a>Pour ouvrir ou ajouter et ouvrir un objet de stratégie de groupe

1. Sur votre contrôleur de domaine, cliquez sur **Démarrer**, cliquez sur **outils d’administration Windows**, puis cliquez sur **gestion des stratégies de groupe**. La Console de gestion de stratégie de groupe s’ouvre.  

2. Dans le volet gauche, double-cliquant sur votre forêt. Par exemple, en double-cliquant sur **forêt: example.com**.  

3. Dans le volet gauche, double-cliquant sur **domaines**et puis double-cliqué sur le domaine pour lequel vous souhaitez gérer un objet de stratégie de groupe. Par exemple, en double-cliquant sur **example.com**.  

4. Effectuez l’une des opérations suivantes:

    -   **Pour ouvrir un objet GPO au niveau domain\ existant pour l’édition**, double-cliquez sur le domaine qui contient l’objet de stratégie de groupe que vous souhaitez gérer, cliquez droit sur la stratégie de domaine que vous souhaitez gérer, telles que la stratégie de domaine par défaut et puis cliquez sur **modifier**. **Éditeur de gestion de stratégie de groupe** s’ouvre.

    -   **Pour créer un nouvel objet de stratégie de groupe et ouvrir pour modification**, clic droit du domaine pour lequel vous souhaitez créer un nouvel objet de stratégie de groupe, puis cliquez sur **créer un objet GPO dans ce domaine et le lier ici**.

        Dans **nouvel objet GPO**, dans **nom**, tapez un nom pour le nouvel objet de stratégie de groupe, puis cliquez sur **OK**.

        Clic droit votre nouvel objet de stratégie de groupe, puis cliquez sur **modifier**. **Éditeur de gestion de stratégie de groupe** s’ouvre.

Dans la section suivante, vous allez utiliser éditeur de gestion de stratégie de groupe pour créer une stratégie sans fil.

### <a name="bkmk_activate"></a>Activer le réseau sans fil par défaut \ IEEE (802.11\) les stratégies

Cette procédure décrit comment activer la valeur par défaut du réseau sans fil \ IEEE (802.11\) les stratégies à l’aide de l’éditeur de gestion de stratégie de groupe \(GPME\).

>[!NOTE]
>Après avoir activé la **WindowsVista et versions ultérieures** version du réseau sans fil \ IEEE (802.11\) stratégies ou les **WindowsXP** version, l’option de version est automatiquement retirée de la liste des options droit clic **réseau sans fil \ IEEE (802.11\) stratégies**. Cela se produit car une fois que vous sélectionnez une version de stratégie, la stratégie est ajoutée dans le volet détails de la GPME lorsque vous sélectionnez le **réseau sans fil \ IEEE (802.11\) stratégies** nœud. Cet état reste sauf si vous supprimez la stratégie sans fil, à quel moment la version de la stratégie sans fil retourne au menu clic droit pour **réseau sans fil \ IEEE (802.11\) stratégies** dans le GPME. En outre, les stratégies sans fil uniquement répertoriés dans le volet d’informations GPME lorsque le **réseau sans fil \ IEEE (802.11\) stratégies** nœud est sélectionné.

L’appartenance au groupe **Admins du domaine**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

#### <a name="to-activate-default-wireless-network-ieee-80211-policies"></a>Pour activer par défaut de réseau sans fil \ IEEE (802.11\) les stratégies  

1. Suivez la procédure précédente, **pour ouvrir ou ajouter et ouvrir un objet de stratégie de groupe** pour ouvrir le GPME.

2. Dans la GPME, dans le volet gauche, double-cliquant sur **Configuration ordinateur**, double-cliquant sur **stratégies**, double-cliquant sur **paramètres Windows**et puis en double-cliquant sur **paramètres de sécurité**.

![Stratégie de groupe de sans fil 802.1 X](../../../media/Wireless-GP/Wireless-GP.jpg)

3. Dans **paramètres de sécurité**, clic droit **réseau sans fil \ IEEE (802.11\) stratégies**, puis cliquez sur **créer une stratégie sans fil pour WindowsVista et versions ultérieures**. 

![802. 1 x sans fil stratégie](../../../media/Wireless-Policy/Wireless-Policy.jpg)

4. Le **propriétés de nouvelle stratégie de réseau sans fil** boîte de dialogue s’ouvre. Dans **nom de la stratégie**, tapez un nouveau nom pour la stratégie ou conservez le nom par défaut. Cliquez sur **OK** pour enregistrer la stratégie. La stratégie par défaut est activée et répertoriée dans le volet détails de la GPME avec le nouveau nom que vous avez fournie ou avec le nom par défaut **nouvelle stratégie de réseau sans fil**.

![Propriétés de nouvelle stratégie réseau sans fil](../../../media/Wireless-Policy-Properties/Wireless-Policy-Properties.jpg)

5. Dans le volet détails, double-cliquant sur **nouvelle stratégie de réseau sans fil** pour l’ouvrir.

Dans la section suivante vous pouvez effectuer la configuration de la stratégie, ordre de préférence de traitement de stratégie et les autorisations de réseau.

### <a name="bkmk_policyconfig"></a>Configurer la nouvelle stratégie de réseau sans fil

Vous pouvez utiliser les procédures de cette section pour configurer le réseau sans fil \ IEEE (802.11\) stratégie. Cette stratégie vous permet de configurer les paramètres de sécurité et d’authentification, gérer les profils sans fil et spécifier des autorisations pour les réseaux sans fil qui ne sont pas configurés en tant que réseaux favoris.

- [Configurer un profil de connexion sans fil pour PEAP\-MS\-CHAP v2](#bkmk_configureprofile)  

- [Définir l’ordre de préférence des profils de connexion sans fil](#bkmk_preferenceorder)  

- [Définir les autorisations du réseau](#bkmk_permissions)  

#### <a name="bkmk_configureprofile"></a>Configurer un profil de connexion sans fil pour PEAP\-MS\-CHAP v2

Cette procédure fournit les étapes requises pour configurer un profil sans fil PEAP\-MS\-CHAP v2.  

L’appartenance au groupe **Admins du domaine**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

##### <a name="to-configure-a-wireless-connection-profile-for-peap-ms-chap-v2"></a>Pour configurer un profil de connexion sans fil pour PEAP\-MS\-CHAP v2

1. Dans GPME, dans la boîte de dialogue des propriétés de réseau sans fil pour la stratégie que vous venez de créer, dans le **général** onglet et dans **Description**, tapez une brève description de la stratégie.

2. Pour spécifier que la configuration automatique WLAN est utilisé pour configurer les paramètres de la carte réseau sans fil, assurez-vous que **service d’utilisation de configuration automatique WLAN Windows pour les clients** est sélectionné.  

3. Dans **se connecter aux réseaux disponibles selon l’ordre des profils affichés ci-dessous**, cliquez sur **ajouter**, puis sélectionnez **Infrastructure**. Le **propriétés de nouveau profil** boîte de dialogue s’ouvre.

4. Dans le**propriétés de nouveau profil** boîte de dialogue le **connexion** onglet le **nom du profil**, tapez un nouveau nom pour le profil. Par exemple, tapez **Example.com profil de réseau local sans fil pour Windows10**.

5. Dans **réseau Name\(s\) \(SSID\)**, tapez l’identificateur SSID qui correspond à l’identificateur SSID sur vos points d’accès sans fil, puis cliquez sur **ajouter**.

    Si votre déploiement utilise plusieurs identificateurs SSID et que chaque point d’accès sans fil utilise les mêmes paramètres de sécurité sans fil, répétez cette étape pour ajouter le SSID pour chaque point d’accès sans fil auquel vous souhaitez appliquer ce profil.

    Si votre déploiement utilise plusieurs identificateurs SSID et que les paramètres de sécurité pour chaque identificateur SSID ne correspondent pas, configurez un profil distinct pour chaque groupe d’identificateurs SSID utilisant les mêmes paramètres de sécurité. Par exemple, si vous disposez d’un groupe de points d’accès sans fil configurés pour utiliser WPA2\-Enterprise et AES et un autre groupe de points d’accès sans fil à utiliser WPA\-Enterprise et TKIP, configurez un profil pour chaque groupe de points d’accès sans fil.

6. Si le texte par défaut **NEWSSID** est présent, sélectionnez-le, puis cliquez sur **supprimer**.

7. Si vous avez déployé des points d’accès sans fil qui sont configurés pour supprimer la balise de diffusion, sélectionnez **se connecter même si le réseau ne diffuse pas**.

    > [!NOTE]
    > L’activation de cette option peut créer un risque de sécurité, car les clients sans fil seront détecter et tentatives de connexion à un réseau sans fil. Par défaut, ce paramètre n’est pas activé.  

8. Cliquez sur le **sécurité**, cliquez sur **avancé**, puis configurez les éléments suivants:

    1. Pour configurer les paramètres de 802. 1 X, avancés dans **IEEE 802. 1 X**, sélectionnez **appliquer avancées des paramètres de 802. 1 X**.

        Lorsque l’avancés 802. 1 X paramètres sont appliqués, les valeurs par défaut pour **Nbre max. Eapol\ start**, **période de maintien**, **période de démarrage**, et **période d’authentification** sont suffisantes pour les déploiements sans fil standard. Pour cette raison, vous n’avez pas besoin de modifier les paramètres par défaut, sauf si vous avez une raison spécifique de le faire.

    2. Pour activer l’authentification unique, sélectionnez **Activer session unique pour ce réseau **.

    3. L’autres valeurs par défaut dans **session unique** sont suffisantes pour les déploiements sans fil standard.

    4. Dans **d’itinérance rapide**, si votre point d’accès sans fil est configuré pour pre\-l’authentification, sélectionnez-le **ce réseau utilise l’authentification pre\**.

9. Pour spécifier que les communications sans fil normes FIPS 140\-2, sélectionnez **effectuer le chiffrement en mode de certifié FIPS 140\-2**.

10. Cliquez sur **OK** pour revenir à la **sécurité** onglet. Dans **sélectionner les méthodes de sécurité pour ce réseau**, dans **authentification**, sélectionnez **WPA2\-entreprise** si elle est prise en charge par votre point d’accès sans fil et les cartes réseau des clients sans fil. Dans le cas contraire, sélectionnez **WPA\-entreprise **.

11. Dans **chiffrement**, si pris en charge par votre point d’accès sans fil et les cartes réseau des clients sans fil, sélectionnez **AES-CCMP **. Si vous utilisez des points d’accès et les cartes réseau sans fil qui prennent en charge 802.11ac, sélectionnez **AES-GCMP **. Dans le cas contraire, sélectionnez **TKIP **.

    > [!NOTE]  
    > Les paramètres des options **authentification** et **chiffrement** doit correspondre les paramètres configurés sur vos points d’accès sans fil. Les paramètres par défaut **Mode d’authentification**, **max. d’échecs d’authentification**, et **mettre en Cache les informations utilisateur pour les futures connexions à ce réseau** sont suffisantes pour les déploiements sans fil standard.  

12. Dans **sélectionner une méthode d’authentification réseau**, sélectionnez **EAP protégé \(PEAP\)**, puis cliquez sur **propriétés **. Le **propriétés EAP protégées** boîte de dialogue s’ouvre.

13. Dans **propriétés EAP protégées**, vérifiez que **vérifier l’identité du serveur en validant le certificat** est sélectionné.

14. Dans **autorités de Certification racine**, sélectionnez l’autorité de certification racine de confiance \(CA\) qui a émis le serveur de certificats sur votre serveur NPS.

    > [!NOTE]  
    > Ce paramètre limite la racine auxquelles les clients font confiance aux autorités de certification sélectionnée. Si aucune autorités de certification racine de confiance n’est sélectionnée, les clients font confiance que toutes les autorités de certification dans leur magasin de certificats Autorités de Certification racines de racine.  

15. Dans le **sélectionner la méthode d’authentification**, sélectionnez **mot de passe sécurisé \ (EAP\-MS\-CHAP v2\)**.

16. Cliquez sur **configurer**. Dans le **propriétés EAP MSCHAPv2** boîte de dialogue, vérifier **utiliser automatiquement mon nom d’ouverture de session Windows et le mot de passe \ (et le domaine if any\)** est sélectionné, puis cliquez sur **OK **.

17. Pour activer la reconnexion rapide PEAP, assurez-vous que **activer la reconnexion rapide** est sélectionné.

18. Pour exiger une serveur TLV de tentatives de connexion, sélectionnez **Déconnect. Si le serveur ne présente pas TLV **.

19. Pour spécifier que l’identité de l’utilisateur est masquée dans la phase d’authentification, sélectionnez **activer la protection de la confidentialité**et dans la zone de texte, tapez un nom d’identité anonyme, ou laissez la zone de texte vide.

    >[! REMARQUES]
    >- La stratégie NPS pour 802.1 X sans fil doit être créée à l’aide de NPS **stratégie de demande de connexion **. Si la stratégie NPS est créée à l’aide de NPS **stratégie réseau**, puis de la protection de la confidentialité ne fonctionnera pas.
    >- Protection de la confidentialité EAP est fournie par certaines méthodes EAP est vide ou une identité anonyme \ (autres que l’identity\ réel) sont envoyé en réponse à la demande d’identité EAP. PEAP envoie l’identité deux fois lors de l’authentification. Dans la première phase, l’identité est envoyée en texte brut et cette identité est utilisée pour le routage, pas pour l’authentification du client. L’identité réelle: utilisé pour l’authentification, est envoyé au cours de la seconde phase de l’authentification, dans le tunnel sécurisé est établi dans la première phase. Si **activer la protection de la confidentialité** case à cocher est activée, le nom d’utilisateur est remplacée par l’entrée spécifiée dans la zone de texte. Par exemple, supposons que **activer la protection de la confidentialité** est sélectionné et l’alias de confidentialité d’identité **anonyme** est spécifié dans la zone de texte. Pour un utilisateur avec un alias de l’identité réelle **jdoe@example.com**, l’identité envoyée dans la première phase d’authentification lors de la modification **anonymous@example.com**. La partie domaine de l’identité de phase 1ern’est pas modifiée qu’il est utilisé pour le routage.  

20. Cliquez sur **OK** pour fermer la **propriétés EAP protégées** boîte de dialogue.
21. Cliquez sur **OK** pour fermer la **sécurité** onglet.
22. Si vous souhaitez créer des profils supplémentaires, cliquez sur **ajouter**, puis répétez les étapes précédentes, rendant personnaliser chaque profil pour les clients sans fil et le réseau auquel vous souhaitez que le profil appliqué de différentes manières. Lorsque vous avez terminé l’ajout de profils, cliquez sur **OK** pour fermer la boîte de dialogue Propriétés de stratégie de réseau sans fil.

Dans la section suivante, vous pouvez commander les profils de stratégie pour une sécurité optimale.

#### <a name="bkmk_preferenceorder"></a>Définir l’ordre de préférence des profils de connexion sans fil
Vous pouvez utiliser cette procédure si vous avez créé plusieurs profils sans fil dans votre stratégie de réseau sans fil et que vous souhaitez classer les profils pour l’efficacité optimale et la sécurité.

Pour vous assurer que les clients sans fil se connectent au plus haut niveau de sécurité qu’ils peuvent prendre en charge, placez vos stratégies plus restrictifs en haut de la liste.

Par exemple, si vous disposez de deux profils, un pour les clients qui prennent en charge WPA2 et un pour les clients qui prennent en charge WPA, placez le profil WPA2 plus élevé dans la liste. Cela garantit que les clients qui prennent en charge WPA2 utilise cette méthode pour la connexion au lieu de la WPA moins sécurisée.

Cette procédure fournit les étapes pour spécifier l’ordre dans lequel les profils de connexion sans fil sont utilisés pour se connecter des clients sans fil membres de domaine aux réseaux sans fil.

L’appartenance au groupe **Admins du domaine**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

##### <a name="to-set-the-preference-order-for-wireless-connection-profiles"></a>Pour définir l’ordre de préférence des profils de connexion sans fil

1. Dans GPME, dans la boîte de dialogue Propriétés du réseau sans fil pour la stratégie que vous venez de configurer, cliquez sur le **général** onglet.

2. Sur le **général** onglet **se connecter aux réseaux disponibles selon l’ordre des profils affichés ci-dessous**, sélectionnez le profil que vous souhaitez déplacer dans la liste et puis cliquez sur soit le «flèche «bouton» bas ou haut» bouton pour déplacer le profil vers l’emplacement souhaité dans la liste.

3.  Répétez l’étape2 pour chaque profil que vous souhaitez déplacer dans la liste.  

4.  Cliquez sur **OK** pour enregistrer toutes les modifications.

Dans la section suivante, vous pouvez définir des autorisations de réseau pour la stratégie sans fil.

#### <a name="bkmk_permissions"></a>Définir les autorisations du réseau
Vous pouvez configurer les paramètres sur le **autorisations réseau** onglet pour les membres du domaine pour le réseau sans fil \ IEEE (802.11\) les stratégies s’appliquent.

Vous pouvez uniquement appliquer les paramètres suivants pour les réseaux sans fil qui ne sont pas configurés sur le **général** onglet dans le **propriétés de la stratégie de réseau sans fil** page:

- Autoriser ou refuser des connexions aux réseaux sans fil spécifiques que vous spécifiez par type de réseau et de Service Set Identifier \(SSID\)

- Autoriser ou refuser des connexions aux réseaux ad hoc

- Autoriser ou refuser des connexions aux réseaux à infrastructure

- Autoriser ou refuser aux utilisateurs d’afficher les types de réseau \ (ad hoc ou infrastructure\) à laquelle ils sont interdites

- Autoriser ou refuser aux utilisateurs de créer un profil qui s’applique à tous les utilisateurs

- Les utilisateurs peuvent se connecter uniquement à des réseaux autorisés à l’aide de profils de stratégie de groupe

L’appartenance au groupe **Admins du domaine**, ou équivalente, est la condition minimale requise pour effectuer ces procédures.

##### <a name="to-allow-or-deny-connections-to-specific-wireless-networks"></a>Pour autoriser ou refuser des connexions aux réseaux sans fil spécifiques

1. Dans GPME, dans la boîte de dialogue Propriétés du réseau sans fil, cliquez sur le **autorisations réseau** onglet.

2. Sur le **autorisations réseau**, cliquez sur **ajouter **. Le **nouvelle entrée d’autorisation** boîte de dialogue s’ouvre.

3. Dans le **nouvelle entrée d’autorisation** la boîte de dialogue dans le **\(SSID\) nom de réseau** champ, entrez le SSID du réseau du réseau pour lequel vous souhaitez définir des autorisations.

4.  Dans **Type de réseau**, sélectionnez **Infrastructure** ou **Ad hoc **.

    > [!NOTE]  
    > Si vous ne savez pas si le réseau de diffusion est une infrastructure ou un réseau ad hoc, vous pouvez configurer deux entrées d’autorisation réseau, une pour chaque type de réseau.

5. Dans **autorisation**, sélectionnez **autoriser** ou **refuser **.

6. Cliquez sur **OK**, pour revenir à la **autorisations réseau** onglet.

##### <a name="to-specify-additional-network-permissions-optional"></a>Pour spécifier le réseau supplémentaires autorisations \(Optional\)

1.  Sur le **autorisations réseau** onglet, configurer tout ou partie des opérations suivantes:  

    -   Pour refuser l’accès aux réseaux ad hoc membres de votre domaine, sélectionnez **empêcher toute connexion aux réseaux ad hoc ad\**.

    -   Pour refuser l’accès aux réseaux à infrastructure membres de votre domaine, sélectionnez **empêcher toute connexion aux réseaux à infrastructure **.  

    -   Pour autoriser des membres de votre domaine afficher les types de réseau \ (ad hoc ou infrastructure\) à laquelle ils sont accès refusés, sélectionnez **autoriser les utilisateurs à afficher les réseaux refusés **.

    -   Pour permettre aux utilisateurs de créer des profils qui s’appliquent à tous les utilisateurs, sélectionnez **autoriser tout le monde à créer tous les profils utilisateur**.

    -   Pour spécifier que vos utilisateurs peuvent se connecter uniquement aux réseaux autorisés à l’aide de profils de stratégie de groupe, sélectionnez **uniquement utiliser des profils de stratégie de groupe pour les réseaux autorisés**.

## <a name="bkmk_nps"></a>Configurer vos serveurs NPS
Suivez ces étapes pour configurer les serveurs NPS pour effectuer une authentification de 802. 1 X pour l’accès sans fil:

- [Inscrire un serveur NPS dans les Services de domaine ActiveDirectory](#bkmk_npsreg)

- [Configurer un point d’accès sans fil en tant qu’un Client RADIUS de NPS](#bkmk_radiusclient)

- [Créer des stratégies de serveur NPS pour 802.1 X sans fil à l’aide d’un Assistant](#bkmk_npspolicy)

### <a name="bkmk_npsreg"></a>Inscrire un serveur NPS dans les Services de domaine ActiveDirectory
Vous pouvez utiliser cette procédure pour inscrire un serveur exécutant NPS \(NPS\) dans \(ADDS\) des Services de domaine ActiveDirectory dans le domaine où le serveur NPS est membre. Pour les serveurs NPS être autorisé à lire les dial\-propriétés des comptes d’utilisateur au cours du processus d’autorisation, chaque serveur NPS doit être enregistré dans les services ADDS. L’inscription d’un serveur NPS ajoute le serveur à le **serveurs RAS et IAS** groupe de sécurité dans ADDS.

>[!NOTE]
>Vous pouvez installer NPS sur un contrôleur de domaine ou sur un serveur dédié. Exécutez la commande Windows PowerShell suivante pour installer le serveur NPS si vous n’avez pas encore fait:
    
    Install-WindowsFeature NPAS -IncludeManagementTools
    
L’appartenance au groupe **Admins du domaine**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

#### <a name="to-register-an-nps-server-in-its-default-domain"></a>Pour inscrire un serveur NPS dans son domaine par défaut

1. Sur votre serveur NPS, dans **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **serveur NPS**. Le serveur NPS dans composants s’ouvre.

2. Clic droit **NPS \(Local\)**, puis cliquez sur **inscrire le serveur dans ActiveDirectory**. Le **serveur NPS** boîte de dialogue s’ouvre.

3. Dans **serveur NPS**, cliquez sur **OK**, puis cliquez sur **OK** à nouveau.

### <a name="bkmk_radiusclient"></a>Configurer un point d’accès sans fil en tant qu’un Client RADIUS de NPS
Vous pouvez utiliser cette procédure pour configurer un point d’accès, également appelé un *serveur d’accès réseau \(NAS\)*, comme un client distant authentification Dial\-In utilisateur Service \(RADIUS\) en utilisant le serveur NPS dans composants. 

>[!IMPORTANT]
>Les ordinateurs clients, tels que les ordinateurs portables sans fil et autres ordinateurs qui exécutent les systèmes d’exploitation clients, ne sont pas des clients RADIUS. Clients RADIUS sont des serveurs d’accès réseau, tels que les points d’accès sans fil, 802.1X\-commutateurs compatibles, les serveurs \(VPN\) de réseau privé virtuel et serveurs d’accès dial\, car ils utilisent le protocole RADIUS pour communiquer avec les serveurs RADIUS tels que les serveurs NPS.

L’appartenance au groupe **Admins du domaine**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

#### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>Pour ajouter un serveur d’accès réseau comme un client RADIUS dans NPS

1. Sur votre serveur NPS, dans **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **serveur NPS**. Le serveur NPS dans composants s’ouvre.

2. Dans le serveur NPS composant logiciel enfichable, double-cliquant sur **Clients et serveurs RADIUS**. Clic droit **Clients RADIUS**, puis cliquez sur **New**.

3. Dans **nouveau Client RADIUS**, vérifiez que le **activer ce client RADIUS** case à cocher est activée.

4. Dans **nouveau Client RADIUS**, dans **nom convivial**, tapez un nom complet pour le point d’accès sans fil.

    Par exemple, si vous souhaitez ajouter un accès sans fil point \(AP\) nommé AP\-01, tapez **AP\-01**.

5. Dans **adresse \(IP or DNS\)**, tapez l’adresse IP ou complet \(FQDN\) de nom de domaine pour le NAS.

    Si vous entrez le nom de domaine complet, pour vérifier que le nom est correct et qu’il correspond à une adresse IP valide, cliquez sur **vérifier**, puis dans **adresse vérifier**, dans le **adresse**, cliquez sur **résoudre**. Si le nom de domaine complet correspond à une adresse IP valide, l’adresse IP de ce NAS s’affiche automatiquement dans **adresse IP**. Si le nom de domaine complet ne résout pas à une adresse IP que vous recevrez un message indiquant qu’aucun hôte n’est connue. Si cela se produit, vérifiez que le nom de point d’accès correct et que le point d’accès est sous tension et connecté au réseau.  

    Cliquez sur **OK** pour fermer **vérifier une adresse**.  

6. Dans **nouveau Client RADIUS**, dans **Secret partagé**, effectuez l’une des opérations suivantes:  

    -   Pour configurer manuellement un secret partagé RADIUS, sélectionnez **manuel**, puis dans **secret partagé**, tapez le mot de passe fort qui est aussi entré sur le serveur NAS. Retapez le secret partagé dans **confirmer le secret partagé**.  

    -   Pour générer automatiquement un secret partagé, sélectionnez le **générer** case à cocher, puis cliquez sur le **générer** bouton. Enregistrer le secret partagé généré, puis utilise cette valeur pour configurer le NAS afin qu’il puisse communiquer avec le serveur NPS.  

        >[!IMPORTANT]
        >Le secret partagé RADIUS que vous entrez pour votre point d’accès virtuel dans NPS doit correspondre exactement le secret partagé RADIUS qui est configuré sur votre réelle PA sans fil Si vous utilisez l’option de serveur NPS pour générer un secret partagé RADIUS, vous devez configurer le point d’accès sans fil réelle correspondante avec le secret partagé RADIUS qui a été généré par le serveur NPS.

7. Dans **nouveau Client RADIUS**, dans le **avancé** onglet **nom du fournisseur**, spécifiez le nom du fabricant NAS. Si vous n’êtes pas sûr du nom de fabricant NAS, sélectionnez **standard RADIUS**.

8. Dans **Options supplémentaires**, si vous utilisez les méthodes d’authentification autre que EAP et PEAP et si votre NAS prend en charge l’utilisation de l’attribut de l’authentificateur de message, sélectionnez **messages de demande d’accès doivent contenir l’attribut d’authentificateur Message\**.

9. Cliquez sur **OK**. Votre NAS s’affiche dans la liste des clients RADIUS configurés sur le serveur NPS.

### <a name="bkmk_npspolicy"></a>Créer des stratégies de serveur NPS pour 802.1 X sans fil à l’aide d’un Assistant
Vous pouvez utiliser cette procédure pour créer des stratégies de demande de la connexion et stratégies nécessaires pour déployer soit 802.1X\ réseau-points d’accès sans fil compatibles en tant que clients Remote Authentication Dial\-In utilisateur Service \(RADIUS\) sur le serveur RADIUS \(NPS\) serveur de stratégie réseau en cours d’exécution.  
Après avoir exécuté l’Assistant, les stratégies suivantes sont créées:

- Stratégie de demande d’une seule connexion

- Une stratégie réseau

>[!NOTE]
>Vous pouvez exécuter l’Assistant Nouveau IEEE 802. 1 X sécurisé câblé et les connexions sans fil chaque fois que vous avez besoin créer des stratégies pour 802. 1 X un accès authentifié.

L’appartenance au groupe **Admins du domaine**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

#### <a name="create-policies-for-8021x-authenticated-wireless-by-using-a-wizard"></a>Créer des stratégies pour 802. 1 X sans fil authentifié à l’aide d’un Assistant

1. Ouvrez le serveur NPS de composants. Si elle n’est pas déjà sélectionnée, cliquez sur **NPS \(Local\)**. Si vous exécutez logiciel enfichable MMC NPS et que vous souhaitez créer des stratégies sur un serveur NPS à distance, sélectionnez le serveur.

2. Dans **prise en main**, dans **Configuration Standard**, sélectionnez **serveur RADIUS pour les connexions câblées ou sans fil X 802.1**. Le texte et les liens ci-dessous pour refléter votre sélection, la modification de texte.

3. Cliquez sur **configurer 802. 1 X**. L’Assistant Configurer 802. 1 X s’ouvre.

4.  Sur le **sélectionner le Type de connexions 802.1 X** page d’Assistant, dans **Type de connexions de 802. 1 X**, sélectionnez **sécuriser les connexions sans fil**et dans **nom**, tapez un nom pour votre stratégie ou conservez le nom par défaut **sécuriser les connexions sans fil**. Cliquez sur **suivant**.

5.  Sur le **spécifier des commutateurs de 802. 1 X** page d’Assistant, dans **clients RADIUS**, 802. 1tous les X commutateurs points d’accès sans fil que vous avez ajoutés comme Clients RADIUS dans le serveur NPS dans composants sont affichés. Effectuez l’une des opérations suivantes:

    -   Pour ajouter un réseau plus accès aux serveurs \(NASs\), tels que les points d’accès sans fil, dans **clients RADIUS**, cliquez sur **ajouter**, puis dans **client RADIUS nouveau**, entrez les informations de: **nom convivial**, **adresse \(IP or DNS\)**, et **Secret partagé**.

    -   Pour modifier les paramètres pour n’importe quel NAS, dans **clients RADIUS**, sélectionnez le point d’accès pour lequel vous souhaitez modifier les paramètres, puis cliquez sur **modifier**. Modifiez les paramètres en fonction des besoins.

    -   Pour supprimer un NAS dans la liste, dans **clients RADIUS**, sélectionnez le NAS, puis cliquez sur **supprimer**.

        >[!WARNING]
        >Suppression d’un client RADIUS à partir de la **configurer 802. 1 X** Assistant supprime le client à partir de la configuration du serveur NPS. Tous les ajouts, modifications et suppressions que vous effectuez dans le **configurer 802. 1 X** Assistant aux clients RADIUS sont répercutées dans le serveur NPS composant logiciel enfichable, dans le **Clients RADIUS** nœud sous **NPS** \/ **Clients et serveurs RADIUS**. Par exemple, si vous utilisez basculer de l’Assistant Suppression d’un 802. 1 X, le commutateur est également supprimé le serveur NPS de composants.

6. Cliquez sur **suivant**. Sur le **configurer une méthode d’authentification** page d’Assistant, dans **Type \ (basé sur la méthode d’accès et le réseau configuration\)**, sélectionnez **Microsoft: EAP protégé \(PEAP\)**, puis cliquez sur **configurer**.

    >[!TIP]
    >Si vous recevez un message d’erreur indiquant qu’Impossible de trouver un certificat pour une utilisation avec la méthode d’authentification, et que vous avez configuré les Services de certificats ActiveDirectory pour émettre automatiquement des certificats pour les serveurs RAS et IAS sur votre réseau, vérifiez d’abord que vous avez suivi les étapes pour inscrire le serveur NPS dans les Services de domaine ActiveDirectory, puis utilisez les étapes suivantes pour mettre à jour la stratégie de groupe: Cliquez sur **Démarrer**, cliquez sur **système Windows**, cliquez sur **exécuter**et dans **ouvrir**, type **gpupdate**, puis appuyez sur ENTRÉE. Lorsque la commande retourne les résultats indiquant qu’utilisateur et ordinateur stratégie de groupe ont mis à jour avec succès, sélectionnez **Microsoft: EAP protégé \(PEAP\)** à nouveau, puis cliquez sur **configurer**.
    >
    >Si après l’actualisation de stratégie de groupe que vous continuez à recevoir le message d’erreur indiquant qu’un certificat ne peut pas être trouvé pour une utilisation avec la méthode d’authentification, le certificat n’est pas affiché, car il ne respecte pas les exigences de certificat de serveur minimale comme indiqué dans le Guide d’accompagnement du réseau principal: [déployer des certificats de serveur pour 802.1 X câblé et sans fil déploiements](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments). Si cela se produit, vous devez interrompre la configuration du serveur NPS, révoquez le certificat délivré à votre server\(s\) NPS et puis suivez les instructions pour configurer un nouveau certificat en utilisant le guide de déploiement de certificats de serveur.

7.  Sur le **modifier les propriétés EAP protégées** page d’Assistant, dans **certificat émis**, assurez-vous que le certificat de serveur NPS correct est sélectionné, puis procédez comme suit:

    >[!NOTE]
    >Vérifiez que la valeur de **émetteur** est correcte pour le certificat sélectionné dans **certificat émis**. Par exemple, l’émetteur attendu pour un certificat émis par une autorité de certification \(ADCS\) des Services de certificats ActiveDirectory nommé corp\DC1, dans le domaine contoso.com, en cours d’exécution est **corp\-DC1\-CA**.

    -   Pour permettre aux utilisateurs de déplacer leurs ordinateurs sans fil entre des points d’accès sans avoir à se réauthentifier chaque fois qu’ils associer à un nouveau point d’accès, sélectionnez **activer la reconnexion rapide**.

    -   Pour spécifier que les clients sans fil qui se connectent seront termine le processus d’authentification réseau si le serveur RADIUS ne présente pas de liaison \(TLV\) Type\-Length\-valeur, sélectionnez **déconnecter les Clients sans liaison**.  

    -   Pour modifier la stratégie de paramètres pour le protocole EAP tapez en **Types de protocoles EAP**, cliquez sur **modifier**, dans **propriétés EAP MSCHAPv2**, modifiez les paramètres selon vos besoins, puis cliquez sur **OK**.  

8.  Cliquez sur **OK**. La boîte de dialogue Modifier les propriétés EAP protégées se ferme, vous ramenant à la **configurer 802. 1 X** Assistant. Cliquez sur **suivant**.

9. Dans **spécifier des groupes d’utilisateurs**, cliquez sur **ajouter**, puis tapez le nom du groupe de sécurité que vous avez configuré pour vos clients sans fil dans les utilisateurs ActiveDirectory et les ordinateurs de composants. Par exemple, si vous avez nommé votre groupe de sécurité sans fil groupe sans fil, tapez **groupe sans fil**. Cliquez sur **suivant**.

10. Cliquez sur **configurer** pour configurer les attributs standard RADIUS et les attributs vendor\ spécifiques pour les \(VLAN\) réseau local virtuel en fonction des besoins et comme spécifié par la documentation fournie par votre fournisseur de matériel de point d’accès sans fil. Cliquez sur **suivant**.

11. Passez en revue les détails du résumé configuration, puis cliquez sur **Terminer**.

Vos stratégies NPS sont désormais créés, et vous pouvez passer à joindre des ordinateurs sans fil au domaine.

## <a name="bkmk_domain"></a>Joindre des nouveaux ordinateurs sans fil au domaine
La méthode la plus simple pour joindre le domaine nouveaux ordinateurs sans fil est physiquement joindre l’ordinateur à un segment de réseau local câblé \ (un segment ne pas contrôlé par un X switch\ 802.1) avant de joindre l’ordinateur au domaine. Cela est plus simple, car les paramètres de stratégie de groupe sans fil sont appliqués immédiatement et automatiquement et, si vous avez déployé votre propre infrastructure à clé publique, l’ordinateur reçoit le certificat d’autorité de certification et le place dans le magasin de certificats Autorités de Certification racine de confiance, permettant au client sans fil d’approuver des serveurs NPS avec les certificats de serveur émis par votre autorité de certification.

De même, après qu’un nouvel ordinateur sans fil est joint au domaine, la méthode recommandée pour les utilisateurs à ouvrir une session sur le domaine est d’effectuer des journaux sur à l’aide d’une connexion filaire sur le réseau.

### <a name="other-domain-join-methods"></a>Autres méthodes de jonction de domaine
Dans les cas où il n’est pas possible de joindre des ordinateurs au domaine à l’aide d’une connexion Ethernet câblée, ou dans les cas où l’utilisateur ne peut pas se connecter au domaine pour la première fois à l’aide d’une connexion filaire, vous devez utiliser une autre méthode.

- **Configuration de l’ordinateur personnel informatique**. Un membre de l’équipe informatique joint un ordinateur sans fil au domaine et configure un session unique d’amorçage profil sans fil. Avec cette méthode, l’administrateur connecte l’ordinateur sans fil au réseau Ethernet câblé et joint l’ordinateur au domaine. L’administrateur distribue ensuite l’ordinateur à l’utilisateur. Lorsque le démarrage de l’utilisateur l’ordinateur sans utiliser une connexion filaire, les informations d’identification de domaine qu’il spécifie manuellement pour l’ouverture de session utilisateur sont utilisés à la fois à établir une connexion au réseau sans fil et ouvrir une session sur le domaine.

Pour plus d’informations, consultez la section [joindre le domaine et ouvrir une session à l’aide de la méthode de Configuration ordinateur personnel informatique](#bkmk_itstaff)

-   **Configuration du profil sans fil par les utilisateurs des données d’amorçage**. L’utilisateur configure l’ordinateur sans fil avec un profil sans fil d’amorçage manuellement et rejoint le domaine, en fonction des instructions acquises à partir d’un administrateur informatique. Le profil sans fil d’amorçage permet à l’utilisateur à établir une connexion sans fil, puis joindre le domaine. Après avoir joint l’ordinateur au domaine et le redémarrage de l’ordinateur, l’utilisateur peut se connecter le domaine à l’aide d’une connexion sans fil et leurs informations d’identification du compte de domaine.

Pour plus d’informations, consultez la section [joindre le domaine et ouvrir une session à l’aide de Configuration du profil sans fil Bootstrap par les utilisateurs](#bkmk_userbootstrap).

### <a name="bkmk_itstaff"></a>Joindre le domaine et ouvrir une session à l’aide de la méthode de Configuration ordinateur personnel informatique
Utilisateurs avec les ordinateurs appartenant à un domain\ des clients sans fil membres du domaine peuvent utiliser un profil sans fil temporaire pour se connecter à un 802.1X\-authentifié réseau sans fil sans être connecté au réseau local câblé. Ce profil sans fil temporaire est appelé un *profil sans fil des données d’amorçage*.

Un profil sans fil d’amorçage nécessite que l’utilisateur de spécifier manuellement leurs informations d’identification compte de domaine utilisateur et ne valide pas le certificat du serveur Remote Authentication Dial\-In utilisateur Service \(RADIUS\) \(NPS\) serveur de stratégie réseau en cours d’exécution.

Une fois la connexion sans fil est établie, stratégie de groupe est appliquée sur l’ordinateur client sans fil, et un nouveau profil sans fil est émis automatiquement. La nouvelle stratégie utilise les informations d’identification de compte utilisateur et d’ordinateur pour l’authentification du client. 

En outre, dans le cadre de l’authentification mutuelle PEAP\-MS\-CHAP v2 utilisant le nouveau profil au lieu du profil de démarrage, le client valide les informations d’identification du serveur RADIUS.

Après avoir joint l’ordinateur au domaine, procédez comme suit pour configurer un session unique d’amorçage profil sans fil, avant de livrer l’ordinateur sans fil à l’utilisateur domain\-member.

#### <a name="to-configure-a-single-sign-on-bootstrap-wireless-profile"></a>Pour configurer un session unique d’amorçage profil sans fil

1. Créer un profil de démarrage à l’aide de la procédure dans ce guide nommé [configurer un profil de connexion sans fil pour PEAP\-MS\-CHAP v2](#bkmk_configureprofile)et utilisez les paramètres suivants:

    - Authentification PEAP\-MS\-CHAP v2

    - Valider désactivé de certificat de serveur RADIUS

    - Authentification unique activée

2. Dans les propriétés de la stratégie de réseau sans fil au sein de laquelle vous avez créé le nouveau profil d’amorçage, sur le **général** , sélectionnez le profil de démarrage et puis cliquez sur **exporter** pour exporter le profil sur un partage réseau, lecteur flash USB, ou tout autre emplacement facilement accessible. Le profil est enregistré en tant que fichier *.xml à l’emplacement que vous spécifiez.

3. Joindre le nouvel ordinateur sans fil au domaine \ (par exemple, via une connexion Ethernet qui ne nécessite pas de IEEE 802. 1 X authentication\) et ajouter le profil sans fil de démarrage à l’ordinateur à l’aide de la **netsh wlan Ajouter profil** commande.

    >[!NOTE]
    >Pour plus d’informations, consultez commandes Netsh pour réseau Local sans fil des \(WLAN\) à [http:///\/technet.microsoft.com\/library\/dd744890.aspx](https://technet.microsoft.com/library/dd744890).

4. Distribuer le nouvel ordinateur sans fil à l’utilisateur avec la procédure «Ouvrir une session sur le domaine à l’aide des ordinateurs exécutant Windows10.»

Lorsque l’utilisateur démarre l’ordinateur, Windows invite l’utilisateur à entrer son nom de compte utilisateur de domaine et le mot de passe. Étant donné que l’authentification unique est activée, l’ordinateur utilise les informations d’identification compte de domaine utilisateur d’abord établir une connexion avec le réseau sans fil, puis ouvrez une session sur le domaine.

#### <a name="bkmk_w10"></a>Ouvrez une session sur le domaine à l’aide des ordinateurs exécutant Windows10

1. Fermez la session, ou redémarrer l’ordinateur.

2. Appuyez sur n’importe quelle touche du clavier ou cliquez sur le bureau. L’écran d’ouverture de session s’affiche avec un nom de compte d’utilisateur local affichée et un champ de saisie de mot de passe sous le nom. Ne vous connectez pas avec le compte d’utilisateur local.

3. Dans l’angle inférieur gauche de l’écran, cliquez sur **autre utilisateur**. Le journal d’un autre utilisateur sur l’écran s’affiche avec les deux champs, un pour le nom d’utilisateur et un mot de passe. Le mot de passe champ Voici le texte **vous connecter à:** , puis du nom du domaine dans lequel l’ordinateur est joint. Par exemple, si votre domaine s’appelle example.com, le texte lit **vous connecter à: exemple**.

4. Dans **nom d’utilisateur**, tapez votre nom d’utilisateur de domaine.

5. Dans **mot de passe**, tapez votre mot de passe de domaine, puis cliquez sur la flèche, ou appuyez sur ENTRÉE.

>[!NOTE]
>Si le **autre utilisateur** écran n’inclut pas le texte **vous connecter à:** et votre nom de domaine, vous devez entrer votre nom d’utilisateur au format *domain\\user*. Par exemple, pour vous connecter au domaine example.com avec un compte nommé **par l’utilisateur-01**, type **example\\User\-01**.

### <a name="bkmk_userbootstrap"></a>Joindre le domaine et ouvrir une session à l’aide de Configuration du profil sans fil Bootstrap par les utilisateurs
Avec cette méthode, vous effectuez les étapes décrites dans la section étapes générales, puis vous fournissez à vos utilisateurs domain\-member avec les instructions sur la façon de configurer manuellement un ordinateur sans fil avec un profil sans fil de démarrage. Le profil sans fil d’amorçage permet à l’utilisateur à établir une connexion sans fil, puis joindre le domaine. Une fois que l’ordinateur est joint au domaine et redémarré, l’utilisateur peut se connecter au domaine via une connexion sans fil.

#### <a name="general-steps"></a>Étapes générales

1. Configurer un compte d’administrateur local, dans **le panneau de configuration**, pour l’utilisateur.

    >[!IMPORTANT]
    >Pour joindre un ordinateur à un domaine, l’utilisateur doit être connecté à l’ordinateur avec le compte administrateur local. Sinon, l’utilisateur doit fournir les informations d’identification pour le compte administrateur local au cours du processus visant à joindre l’ordinateur au domaine. En outre, l’utilisateur doit avoir un compte d’utilisateur dans le domaine auquel l’utilisateur souhaite joindre l’ordinateur. Pendant le processus visant à joindre l’ordinateur au domaine, l’utilisateur sera invité pour des informations d’identification du compte de domaine \ (nom d’utilisateur et password\).

2. Fournir à vos utilisateurs de domaine avec les instructions pour configurer un profil sans fil de démarrage, comme indiqué dans la procédure suivante **pour configurer un profil sans fil d’amorçage **.
3. En outre, fournir aux utilisateurs avec les informations d’identification de l’ordinateur local \ (nom d’utilisateur et password\) et les informations d’identification de domaine \ (nom de compte utilisateur de domaine et password\) sous la forme *Nomdomaine\\nomutilisateur*, ainsi que les procédures permettant de «Joindre l’ordinateur au domaine,» et «Ouvrir une session sur le domaine, «comme documenté dans Windows Server2016 [Guide réseau de base ](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide).

#### <a name="to-configure-a-bootstrap-wireless-profile"></a>Pour configurer un profil sans fil d’amorçage

1. Utilisez les informations d’identification fournies par votre administrateur réseau ou un support informatique professionnels en informatique à ouvrir une session sur l’ordinateur avec un compte d’administrateur de l’ordinateur local.

2. Clic droit sur le bureau, puis cliquez sur l’icône **ouvrez Centre réseau et partage **. **Centre réseau et partage** s’ouvre. Dans **modifier vos paramètres de mise en réseau**, cliquez sur **configurer une nouvelle connexion ou un réseau **. Le **réseau ou une connexion** boîte de dialogue s’ouvre.

3. Cliquez sur **vous connecter manuellement à un réseau sans fil**, puis cliquez sur **suivant **.

4. Dans **vous connecter manuellement à un réseau sans fil**, dans **nom de réseau**, tapez le nom SSID du point d’accès.  

5. Dans **type de sécurité**, sélectionnez le paramètre fourni par votre administrateur.

6. Dans **le type de chiffrement** et **clé de sécurité**, sélectionnez ou tapez les paramètres fournis par votre administrateur.

7. Sélectionnez **lancer automatiquement cette connexion**, puis cliquez sur **suivant **.

8. Dans **ajouté avec succès *** votre réseau SSID*, cliquez sur **modifier les paramètres de connexion**.

9. Cliquez sur **modifier les paramètres de connexion **. Le *votre réseau SSID* ouvre la boîte de dialogue Propriétés de réseau sans fil.

10. Cliquez sur le **sécurité** onglet, puis dans **choisir une méthode d’authentification réseau**, sélectionnez **EAP protégé \(PEAP\)**.

11. Cliquez sur **paramètres**. Le **propriétés EAP protégées du \(PEAP\)** page s’ouvre.

12. Dans le **propriétés EAP protégées du \(PEAP\)** page, assurez-vous que **valider le certificat du serveur** est ne pas sélectionné, cliquez sur **OK** deux fois, puis cliquez sur **fermer **.

13. Windows puis tente de se connecter au réseau sans fil. Les paramètres du profil sans fil d’amorçage spécifient que vous devez fournir vos informations d’identification de domaine. Lorsque Windows vous demande un nom de compte et le mot de passe, tapez vos informations d’identification du compte de domaine comme suit: *nom Domaine\\Nom d’utilisateur de domaine*, *mot de passe de domaine *.

##### <a name="to-join-a-computer-to-the-domain"></a>Pour joindre un ordinateur au domaine

1. Ouvrez une session sur l’ordinateur avec le compte administrateur local.

2. Dans la zone de texte Rechercher, tapez **PowerShell **. Dans les résultats de recherche, cliquez sur **Windows PowerShell**, puis cliquez sur **exécuter en tant qu’administrateur **. Windows PowerShell s’ouvre avec une invite de commandes avec élévation de privilèges.

3. Dans Windows PowerShell, tapez la commande suivante et appuyez sur ENTRÉE. Veillez à remplacer le nom de domaine variable avec le nom du domaine que vous voulez joindre.
    
    Add-Computer Nom de domaine
    
4. Lorsque vous y êtes invité, tapez votre nom d’utilisateur de domaine et le mot de passe, puis cliquez sur **OK **.
5. Redémarrez l’ordinateur.
6. Suivez les instructions dans la section précédente [ouvrez une session sur le domaine à l’aide des ordinateurs exécutant Windows10](#bkmk_w10).