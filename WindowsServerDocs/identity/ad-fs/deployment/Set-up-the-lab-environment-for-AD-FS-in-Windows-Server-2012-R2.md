---
ms.assetid: 6b38480e-5b1c-49f0-9d46-8cf22f70f0d2
title: Configurer l’environnement lab pour AD FS dans Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 52199ab8ca6f82443e78e72c6980746fa561363a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408304"
---
# <a name="set-up-the-lab-environment-for-ad-fs-in-windows-server-2012-r2"></a>Configurer l’environnement lab pour AD FS dans Windows Server 2012 R2


Cette rubrique indique les étapes de la configuration d'un environnement de test que vous pouvez utiliser pour effectuer les procédures des guides pas à pas suivants :

-   [Procédure pas à pas : Joindre un espace de travail avec un appareil iOS](../../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

-   [Procédure pas à pas : Joindre un espace de travail avec un appareil Windows](../../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)


-   [Guide pas à pas : Gérer les risques avec le contrôle d’accès conditionnel](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)

-   [Guide pas à pas : Gérer les risques avec des Multi-Factor Authentication supplémentaires pour les applications sensibles](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

> [!NOTE]
> Nous vous déconseillons d'installer le serveur web et le serveur de fédération sur le même ordinateur.

Pour configurer cet environnement de test, effectuez les étapes suivantes :

1.  [Étape 1 : Configurer le contrôleur de domaine (DC1)](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_1)

2.  [Étape 2 : Configurer le serveur de Fédération (ADFS1) avec Device Registration service](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)

3.  [Étape 3 : Configurer le serveur Web (WebServ1) et un exemple d’application basée sur les revendications](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_5)

4.  [Étape 4 : Configurer l’ordinateur client (CLIENT1)](../../ad-fs/deployment/../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_10)

## <a name="BKMK_1"></a>Étape 1 : configurer le contrôleur de domaine (DC1)
Dans le cadre de cet environnement de test, vous pouvez appeler votre racine Active Directory domaine **contoso.com** et <strong>pass@word1</strong> spécifier en tant que mot de passe d’administrateur.

-   Installez le service de rôle AD DS et installez Active Directory Domain Services (AD DS) pour faire de votre ordinateur un contrôleur de domaine dans Windows Server 2012 R2. Cette action met à niveau votre schéma AD DS dans le cadre de la création du contrôleur de domaine. Pour plus d’informations et pour obtenir des instructions pas à pas, consultez[https://technet.microsoft.com/library/hh472162.aspx](https://technet.microsoft.com/library/hh472162.aspx).

### <a name="BKMK_2"></a>Créer des comptes de Active Directory de test
Une fois que votre contrôleur de domaine est opérationnel, vous pouvez créer un groupe de test, tester des comptes d'utilisateur dans ce domaine et ajouter le compte d'utilisateur au compte de groupe. Vous utilisez ces comptes pour effectuer les procédures des guides pas à pas mentionnés plus haut dans cette rubrique.

Créez les comptes ci-après :

- Utilisateur : **Robert Hatley** avec les informations d'identification suivantes : Nom d'utilisateur : **RobertH** et mot de passe :<strong>P@ssword</strong>

- Groupe : **Financer**

Pour plus d’informations sur la création de comptes d’utilisateurs et de groupes dans Active Directory ( [https://technet.microsoft.com/library/cc783323%28v.aspx](https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx)AD), consultez.

Ajoutez le compte **Robert Hatley** au groupe **Finance** . Pour plus d’informations sur l’ajout d’un utilisateur à un groupe dans Active Directory [https://technet.microsoft.com/library/cc737130%28v=ws.10%29.aspx](https://technet.microsoft.com/library/cc737130%28v=ws.10%29.aspx), consultez.

### <a name="create-a-gmsa-account"></a>Créer un compte de service géré de groupe
Le compte du compte de service administré de groupe (GMSA) est requis lors de l’installation et de la configuration de Services ADFS (AD FS).

##### <a name="to-create-a-gmsa-account"></a>Pour créer un compte de service géré de groupe

1.  Ouvrez une fenêtre de commande Windows PowerShell et tapez les commandes suivantes :

    ```
    Add-KdsRootKey -EffectiveTime (Get-Date).AddHours(-10)
    New-ADServiceAccount FsGmsa -DNSHostName adfs1.contoso.com -ServicePrincipalNames http/adfs1.contoso.com

    ```

## <a name="BKMK_4"></a>Étape 2 : configurer le serveur de fédération (ADFS1) avec Device Registration Service
Pour configurer une autre machine virtuelle, installez Windows Server 2012 R2 et connectez-le au domaine **contoso.com**. Configurez l’ordinateur après l’avoir joint au domaine, puis procédez à l’installation et à la configuration du rôle AD FS.

Pour obtenir une vidéo, [consultez services ADFS série de vidéos de savoir-faire : Installation d’une batterie](https://technet.microsoft.com/video/dn469436)de serveurs AD FS.

### <a name="install-a-server-ssl-certificate"></a>Installer un certificat SSL de serveur
Vous devez installer un certificat SSL (Secure Socket Layer) de serveur sur le serveur ADFS1 dans le magasin de l'ordinateur local. Le certificat DOIT posséder les attributs suivants :

-   Nom du sujet (nom commun) : adfs1.contoso.com

-   Autre nom de l'objet (DNS) : adfs1.contoso.com

-   Autre nom de l'objet (DNS) : enterpriseregistration.contoso.com

Pour plus d’informations sur la configuration des certificats SSL, voir [Configurer SSL/TLS sur un site web dans le domaine avec une autorité de certification d’entreprise](https://social.technet.microsoft.com/wiki/contents/articles/12485.configure-ssltls-on-a-web-site-in-the-domain-with-an-enterprise-ca.aspx).

[Services ADFS série de vidéos de savoir-faire : Mise à jour](https://technet.microsoft.com/video/adfs-updating-certificates)des certificats.

### <a name="install-the-ad-fs-server-role"></a>Installer le rôle serveur AD FS

##### <a name="to-install-the-federation-service-role-service"></a>Pour installer le service de rôle de service de fédération

1. Connectez-vous au serveur à l’aide du compte administrator@contoso.comd’administrateur de domaine.

2. Démarrez le Gestionnaire de serveur. Pour démarrer le Gestionnaire de serveur, cliquez sur **Gestionnaire de serveur** dans l'écran d' **accueil** Windows, ou cliquez sur **Gestionnaire de serveur** dans la barre des tâches Windows sur le Bureau Windows. Sous l'onglet **Démarrage rapide** de la vignette **Bienvenue** sur la page **Tableau de bord** , cliquez sur **Ajouter des rôles et des fonctionnalités**. Vous pouvez également cliquer sur **Ajouter des rôles et fonctionnalités** dans le menu **Gérer** .

3. Dans la page **Avant de commencer** , cliquez sur **Suivant**.

4. Dans la page **Sélectionner le type d’installation**, cliquez sur **Installation basée sur un rôle ou une fonctionnalité**, puis sur **Suivant**.

5. Dans la page **Sélectionner le serveur de destination** , cliquez sur **Sélectionner un serveur du pool de serveurs**, vérifiez que l'ordinateur cible est sélectionné, puis cliquez sur **Suivant**.

6. Dans la page **Sélectionner des rôles de serveurs** , cliquez sur **Services AD FS (Active Directory Federation Services)** , puis cliquez sur **Suivant**.

7. Dans la page **Sélectionner les fonctionnalités** , cliquez sur **Suivant**.

8. Dans la page **Services AD FS (Active Directory Federation Services)** , cliquez sur **Suivant**.

9. Après avoir vérifié les informations de la page **Confirmer les sélections d'installation** , cochez la case **Redémarrer automatiquement le serveur de destination, si nécessaire** , puis cliquez sur **Installer**.

10. Dans la page **Progression de l'installation** , vérifiez que tout a été correctement installé, puis cliquez sur **Fermer**.

### <a name="configure-the-federation-server"></a>Configurer le serveur de fédération
L'étape suivante consiste à configurer le serveur de fédération.

##### <a name="to-configure-the-federation-server"></a>Pour configurer le serveur de fédération

1.  Dans la page **Tableau de bord** du Gestionnaire de serveur, cliquez sur le drapeau **Notifications** , puis sur **Configurer le service FS (Federation Service) sur le serveur**.

    L'**Assistant Configuration des services AD FS (Active Directory Federation Services)** s'ouvre.

2.  Dans la page **Bienvenue**, sélectionnez **Créer le premier serveur de fédération dans une batterie de serveurs de fédération**, puis cliquez sur **Suivant**.

3.  Dans la page **Connexion à AD DS** , spécifiez un compte doté de droits d’administrateur de domaine pour le domaine Active Directory **contoso.com** auquel cet ordinateur est joint, puis cliquez sur **Suivant**.

4.  Dans la page **Spécifier les propriétés de service**, effectuez la procédure suivante, puis cliquez sur **Suivant** :

    -   Importez le certificat SSL que vous avez obtenu précédemment. Ce certificat est le certificat d'authentification de service requis. Accédez à l'emplacement de votre certificat SSL.

    -   Pour attribuer un nom à votre service de fédération, tapez **adfs1.contoso.com**. Cette valeur est celle que vous avez fournie quand vous avez inscrit un certificat SSL dans les services de certificats Active Directory (AD CS).

    -   Pour attribuer un nom complet à votre service de fédération, tapez **Contoso Corporation**.

5.  Dans la page **Spécifier un compte de service** , sélectionnez **Utiliser un compte d’utilisateur de domaine ou de service administré de type groupe existant**, puis spécifiez le compte GMSA **fsgmsa** que vous avez créé en même temps que le contrôleur de domaine.

6.  Dans la page **Spécifier une base de données de configuration** , sélectionnez **Créer une base de données sur ce serveur à l'aide de la base de données interne Windows**, puis cliquez sur **Suivant**.

7.  Dans la page **Examiner les options**, vérifiez les choix que vous avez effectués pour la configuration, puis cliquez sur **Suivant**.

8.  Dans la page **Vérifications des conditions préalables**, assurez-vous que toutes les vérifications des conditions préalables ont été correctement effectuées, puis cliquez sur **Configurer**.

9. Dans la page **Résultats** , passez en revue les résultats, vérifiez si la configuration s'est déroulée correctement, puis cliquez sur **Étapes ultérieures requises pour le déploiement de votre service FS (Federation Service)** .

### <a name="configure-device-registration-service"></a>Configurer Device Registration Service
L'étape suivante consiste à configurer Device Registration Service sur le serveur ADFS1. Pour obtenir une vidéo, [consultez services ADFS série de vidéos de savoir-faire : Activation du service](https://technet.microsoft.com/video/adfs-how-to-enabling-the-device-registration-service)Device Registration.

##### <a name="to-configure-device-registration-service-for-windows-server-2012-rtm"></a>Pour configurer Device Registration Service pour Windows Server 2012 RTM

1.  > [!IMPORTANT]
    > **L’étape suivante s’applique à la version RTM de Windows Server 2012 R2.**

    Ouvrez une fenêtre de commande Windows PowerShell et tapez les commandes suivantes :

    ```
    Initialize-ADDeviceRegistration
    ```

    Quand vous êtes invité à indiquer un compte de service, tapez **contoso\fsgmsa$** .

    Exécutez maintenant l’applet de commande Windows PowerShell.

    ```
    Enable-AdfsDeviceRegistration
    ```

2.  Sur le serveur ADFS1, dans la console **Gestion AD FS** , accédez à **Stratégies d'authentification**. Sélectionnez **Modifier l'authentification principale globale**. Cochez la case **Activer l'authentification des appareils**, puis cliquez sur **OK**.

### <a name="add-host-a-and-alias-cname-resource-records-to-dns"></a>Ajouter les enregistrements de ressource d'hôte (A) et d'alias (CNAME) à DNS
Sur le contrôleur de domaine DC1, vous devez vous assurer que les enregistrements DNS (Domain Name System) suivants sont créés pour Device Registration Service.

|Entrée|Type|Address|
|---------|--------|-----------|
|adfs1|Hôte (A)|Adresse IP du serveur de AD FS|
|enterpriseregistration|Alias (CNAME)|adfs1.contoso.com|

Vous pouvez utiliser la procédure suivante pour ajouter un enregistrement de ressource d'hôte (A) aux serveurs de noms DNS d'entreprise pour le serveur de fédération et Device Registration Service.

Vous devez au minimum être membre du groupe des administrateurs ou d'un groupe équivalent pour effectuer cette procédure. Passez en revue les détails sur l’utilisation des comptes et des appartenances aux<https://go.microsoft.com/fwlink/?LinkId=83477>groupes appropriés dans le lien hypertexte «<https://go.microsoft.com/fwlink/p/?LinkId=83477>» groupes locaux et de domaine par défaut ().

##### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Pour ajouter des enregistrements de ressource d'hôte (A) et d'alias (CNAME) à DNS pour votre serveur de fédération

1.  Sur le contrôleur de domaine (DC1), à partir du Gestionnaire de serveur, dans le menu **Outils** , cliquez sur **DNS** pour ouvrir le composant logiciel enfichable DNS.

2.  Dans l'arborescence de la console, développez DC1, développez **Zones de recherche directes**, cliquez avec le bouton droit sur **contoso.com**, puis cliquez sur **Nouvel hôte (A ou AAAA)** .

3.  Dans **Nom**, tapez le nom à utiliser pour votre batterie AD FS. Pour cette procédure pas à pas, tapez **adfs1**.

4.  Dans **Adresse IP**, tapez l'adresse IP du serveur ADFS1. Cliquez sur **Ajouter un hôte**.

5.  Cliquez avec le bouton droit sur **contoso.com**, puis cliquez sur **Nouvel alias (CNAME)** .

6.  Dans la boîte de dialogue **Nouvel enregistrement de ressource**, tapez **enterpriseregistration** dans la zone **Nom de l'alias**.

7.  Dans la zone Nom de domaine pleinement qualifié (FQDN) pour l'hôte de destination, tapez **adfs1.contoso.com**, puis cliquez sur **OK**.

    > [!IMPORTANT]
    > Dans un déploiement réel, si votre entreprise possède plusieurs suffixes de nom d'utilisateur principal (UPN), vous devez créer un enregistrement CNAME par suffixe UPN dans DNS.

## <a name="BKMK_5"></a>Étape 3 : configurer le serveur web (WebServ1) et un exemple d'application basée sur les revendications
Configurez un ordinateur virtuel (WebServ1) en installant le système d’exploitation Windows Server 2012 R2 et connectez-le au domaine **contoso.com**. Après l'avoir joint au domaine, vous pouvez passer à l'installation et à la configuration du rôle de serveur web.

Pour effectuer les procédures pas à pas mentionnées plus haut dans cette rubrique, vous devez posséder un exemple d'application sécurisé par votre serveur de fédération (ADFS1).

Vous pouvez télécharger le kit de développement logiciel[https://www.microsoft.com/download/details.aspx?id=4451](https://www.microsoft.com/download/details.aspx?id=4451)(SDK) Windows Identity Foundation (, qui comprend un exemple d’application basée sur les revendications.

Vous devez effectuer les étapes suivantes pour configurer un serveur web avec cet exemple d'application basée sur des revendications.

> [!NOTE]
> Ces étapes ont été testées sur un serveur Web qui exécute le système d’exploitation Windows Server 2012 R2.

1.  [Installer le rôle de serveur Web et Windows Identity Foundation](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_15)

2.  [Installer le kit de développement logiciel Windows Identity Foundation](../../ad-fs/deployment/../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_13)

3.  [Configurer l'exemple d'application simple basée sur des revendications dans IIS](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_9)

4.  [Créer une approbation de partie de confiance sur votre serveur de Fédération](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_11)

### <a name="BKMK_15"></a>Installer le rôle de serveur Web et Windows Identity Foundation

1. > [!NOTE]
   > Vous devez avoir accès au support d’installation de Windows Server 2012 R2.

   Connectez-vous à WebServ1 à <strong>administrator@contoso.com</strong> l’aide de <strong>pass@word1</strong>et du mot de passe.

2. Depuis le Gestionnaire de serveur, sous l'onglet **Démarrage rapide** de la vignette **Bienvenue** sur la page **Tableau de bord**, cliquez sur **Ajouter des rôles et des fonctionnalités**. Vous pouvez également cliquer sur **Ajouter des rôles et fonctionnalités** dans le menu **Gérer** .

3. Dans la page **Avant de commencer** , cliquez sur **Suivant**.

4. Dans la page **Sélectionner le type d’installation**, cliquez sur **Installation basée sur un rôle ou une fonctionnalité**, puis sur **Suivant**.

5. Dans la page **Sélectionner le serveur de destination** , cliquez sur **Sélectionner un serveur du pool de serveurs**, vérifiez que l'ordinateur cible est sélectionné, puis cliquez sur **Suivant**.

6. Dans la page **Sélectionner des rôles de serveurs**, cochez la case **Serveur Web (IIS)** , cliquez sur **Ajouter des fonctionnalités**, puis cliquez sur **Suivant**.

7. Dans la page **Sélectionner des fonctionnalités** , sélectionnez **Windows Identity Foundation 3.5**, puis cliquez sur **Suivant**.

8. Dans la page **Rôle de serveur web (IIS)** , cliquez sur **Suivant**.

9. Dans la page **Sélectionner les services de rôle**, sélectionnez et développez **Développement d'applications**. Sélectionnez **ASP.NET 3.5**, cliquez sur **Ajouter des fonctionnalités**, puis cliquez sur **Suivant**.

10. Dans la page **Confirmer les sélections d'installation**, cliquez sur **Spécifier un autre chemin d'accès source**. Entrez le chemin d’accès au répertoire SxS situé dans le support d’installation de Windows Server 2012 R2. Par exemple, D:\Sources\Sxs. Cliquez sur **OK**, puis sur **Installer**.

### <a name="BKMK_13"></a>Installer le kit de développement logiciel Windows Identity Foundation

1.  Exécutez WindowsIdentityFoundation-SDK-3.5. msi pour installer le kit de développement logiciel (https://www.microsoft.com/download/details.aspx?id=4451) SDK) Windows identity Foundation 3,5 (. Choisissez toutes les options par défaut.

### <a name="BKMK_9"></a>Configuration de l’application de revendications simple dans IIS

1.  Installez un certificat SSL valide dans le magasin de certificats de l'ordinateur. Le certificat doit contenir le nom de votre serveur web, **webserv1.contoso.com**.

2.  Copiez le contenu de C:\Program Files (x86)\Windows Identity Foundation SDK\v3.5\Samples\Quick Start\Web Application\PassiveRedirectBasedClaimsAwareWebApp vers C:\Inetpub\Claimapp.

3.  Modifiez le fichier **Default.aspx.cs** de manière à éviter tout filtrage de revendications. Ainsi, l'exemple d'application affiche toutes les revendications émises par le serveur de fédération. Procédez comme suit :

    1.  Ouvrez **Default.aspx.cs** dans un éditeur de texte.

    2.  Dans le fichier, recherchez la seconde occurrence d’`ExpectedClaims`.

    3.  Commentez la totalité de l’instruction `IF` ainsi que ses accolades. Indiquez les commentaires en tapant « // » (sans les guillemets) au début d’une ligne.

    4.  L’instruction `FOREACH` doit maintenant ressembler à l’exemple de code suivant.

        ```
        Foreach (claim claim in claimsIdentity.Claims)
        {
           //Before showing the claims validate that this is an expected claim
           //If it is not in the expected claims list then don't show it
           //if (ExpectedClaims.Contains( claim.ClaimType ) )
           // {
              writeClaim( claim, table );
           //}
        }

        ```

    5.  Enregistrez et fermez le fichier **Default.aspx.cs**.

    6.  Ouvrez **web.config** dans un éditeur de texte.

    7.  Supprimez la totalité de la section `<microsoft.identityModel>` . Supprimez tout le code depuis `including <microsoft.identityModel>` jusqu’à `</microsoft.identityModel>`(compris).

    8.  Enregistrez et fermez **web.config**.

4.  **Configurer le gestionnaire des services Internet**

    1.  Ouvrez **Gestionnaire des services Internet (IIS)** .

    2.  Accédez à **Pools d'applications**, cliquez avec le bouton droit sur **DefaultAppPool**, puis sélectionnez **Paramètres avancés**. Définissez **Charger le profil utilisateur** sur **True**, puis cliquez sur **OK**.

    3.  Cliquez avec le bouton droit sur **DefaultAppPool**, puis sélectionnez **Paramètres de base**. Définissez **Version CLR .NET** sur **Version CLR .NET v2.0.50727**.

    4.  Cliquez avec le bouton droit sur **Site Web par défaut** , puis sélectionnez **Modifier les liaisons**.

    5.  Ajoutez une liaison **HTTPS** au port **443** avec le certificat SSL que vous avez installé.

    6.  Cliquez avec le bouton droit sur **Site Web par défaut**, puis sélectionnez **Ajouter une application**.

    7.  Affectez la valeur **claimapp** à l’alias et la valeur **c:\inetpub\claimapp**au chemin physique.

5.  Pour que **claimapp** fonctionne avec votre serveur de fédération, procédez comme suit :

    1.  Exécutez FedUtil.exe qui se trouve à l’emplacement suivant : **C:\Program Files (x86)\Windows Identity Foundation SDK\v3.5**.

    2.  Définissez l’emplacement de la configuration de l’application sur **C:\inetput\claimapp\web.config** et définissez l’URI de l’application sur l’URL de votre site, **https://webserv1.contoso.com /ClaimApp/** . Cliquez sur **Suivant**.

    3.  Sélectionnez **utiliser un STS existant** , puis accédez à l’URL des métadonnées de votre serveur AD FS **https://adfs1.contoso.com/federationmetadata/2007-06/federationmetadata.xml** . Cliquez sur **Suivant**.

    4.  Sélectionnez **Désactiver la validation de la chaîne de certificats**, puis cliquez sur **Suivant**.

    5.  Sélectionnez **Aucun chiffrement**, puis cliquez sur **Suivant**. Dans la page **Revendications proposées** , cliquez sur **Suivant**.

    6.  Cochez la case **Planifier une tâche pour effectuer des mises à jour quotidiennes des métadonnées WS-Federation**. Cliquez sur **Terminer**.

    7.  Votre exemple d'application est maintenant configuré. Si vous testez l’URL **https://webserv1.contoso.com/claimapp** de l’application, elle doit vous rediriger vers votre serveur de Fédération. Le serveur de fédération doit afficher une page d'erreur, car vous n'avez pas encore configuré l'approbation de partie de confiance. En d’autres termes, vous n’avez pas sécurisé cette application de test en AD FS.

Vous devez maintenant sécuriser votre exemple d’application qui s’exécute sur votre serveur Web avec AD FS. Pour ce faire, vous pouvez ajouter une approbation de partie de confiance à votre serveur de fédération (ADFS1). Pour obtenir une vidéo, [consultez services ADFS série de vidéos de savoir-faire : Ajoutez une approbation](https://technet.microsoft.com/video/adfs-how-to-add-a-relying-party-trust)de partie de confiance.

### <a name="BKMK_11"></a>Créer une approbation de partie de confiance sur votre serveur de Fédération

1.  Sur votre serveur de fédération (ADFS1), dans la **console Gestion AD FS**, accédez à **Approbations de partie de confiance**, puis cliquez sur **Ajouter une approbation de partie de confiance**.

2.  Dans la page **Sélectionner une source de données** , sélectionnez **Importer les données, publiées en ligne ou sur un réseau local, concernant la partie de confiance**, entrez l'URL des métadonnées de **claimapp**, puis cliquez sur **Suivant**. Grâce à l'outil FedUtil.exe, vous avez créé un fichier .xml de métadonnées. Il se trouve à l’emplacement **https://webserv1.contoso.com/claimapp/federationmetadata/2007-06/federationmetadata.xml** .

3.  Dans la page **Entrer le nom complet** , spécifiez le **nom complet** de votre approbation de partie de confiance ( **claimapp**), puis cliquez sur **Suivant**.

4.  Dans la page **Configurer l'authentification multifacteur maintenant ?** , sélectionnez **Ne pas configurer les paramètres d'authentification multifacteur pour cette approbation de partie de confiance**, puis cliquez sur **Suivant**.

5.  Dans la page **Choisir les règles d'autorisation d'émission**, sélectionnez **Autoriser l'accès de tous les utilisateurs à cette partie de confiance**, puis cliquez sur **Suivant**.

6.  Dans la page **Prêt à ajouter l'approbation** , cliquez sur **Suivant**.

7.  Dans la boîte de dialogue **Modifier les règles de revendication**, cliquez sur **Ajouter une règle**.

8.  Dans la page **Choisir le type de règle**, sélectionnez **Envoyer les revendications en utilisant une règle personnalisée**, puis cliquez sur **Suivant**.

9. Dans la page **Configurer la règle de revendication** , dans la zone **Nom de la règle de revendication** , tapez **All Claims**. Dans la zone **Règle personnalisée**, tapez la règle de revendication suivante.

    ```
    c:[ ]
    => issue(claim = c);

    ```

10. Cliquez sur **Terminer**, puis sur **OK**.

## <a name="BKMK_10"></a>Étape 4 : configurer l'ordinateur client (Client1)
Configurez une autre machine virtuelle et installez Windows 8.1. Cette machine virtuelle doit se trouver sur le même réseau virtuel que les autres machines. Cette machine NE DOIT PAS être jointe au domaine Contoso.

Le client doit approuver le certificat SSL utilisé pour le serveur de Fédération (ADFS1), que vous avez configuré à [l’étape 2 : Configurez le serveur de Fédération (ADFS1) avec](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)Device Registration service. Il doit aussi pouvoir valider les informations sur la révocation du certificat.

Vous devez également configurer un compte Microsoft et l'utiliser pour vous connecter à Client1.

## <a name="see-also"></a>Voir aussi


- [Services ADFS série de vidéos de savoir-faire : Installation d’une batterie de serveurs AD FS](https://technet.microsoft.com/video/dn469436)
- [Services ADFS série de vidéos de savoir-faire : Mise à jour des certificats](https://technet.microsoft.com/video/adfs-updating-certificates)
- [Services ADFS série de vidéos de savoir-faire : Ajouter une approbation de partie de confiance](https://technet.microsoft.com/video/adfs-how-to-add-a-relying-party-trust)
- [Services ADFS série de vidéos de savoir-faire : Activation du service Device Registration](https://technet.microsoft.com/video/adfs-how-to-enabling-the-device-registration-service)
- [Services ADFS série de vidéos de savoir-faire : Installation du proxy d’application Web](https://technet.microsoft.com/video/dn469438)



