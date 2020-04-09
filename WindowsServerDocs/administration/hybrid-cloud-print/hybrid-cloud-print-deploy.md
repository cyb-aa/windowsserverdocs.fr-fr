---
title: Déployer Windows Server hybride Cloud Print
description: Comment configurer l’impression sur le Cloud hybride Microsoft
ms.prod: windows-server
ms.technology: windows server 2016
ms.assetid: fc239aec-e719-47ea-92fc-d82a7247c5e9
author: msjimwu
ms.author: coreyp
manager: dongill
ms.date: 3/15/2018
ms.openlocfilehash: c06aafb015b065f307eca02abc7a6adaa8ba763c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852112"
---
# <a name="deploy-windows-server-hybrid-cloud-print"></a>Déployer Windows Server hybride Cloud Print

>S’applique à Windows Server 2016

Cette rubrique, destinée aux administrateurs informatiques, décrit le déploiement de bout en bout de la solution Microsoft hybride Cloud Print (HCP). Cette solution se découche sur les serveurs Windows existants qui s’exécutent en tant que serveur d’impression et permet à Azure Active Directory (Azure AD) joints et aux appareils gérés par MDM de découvrir et d’imprimer sur les imprimantes gérées par l’organisation.

## <a name="pre-requisites"></a>Prérequis

Vous devez acquérir un certain nombre d’abonnements, de services et d’ordinateurs avant de commencer cette installation. En voici la liste :

- Abonnement Azure AD Premium.

  Consultez [prise en main d’un abonnement Azure](https://azure.microsoft.com/trial/get-started-active-directory/) pour un abonnement d’évaluation à Azure.

- Service MDM, tel qu’Intune.

  Consultez [Microsoft Intune](https://www.microsoft.com/cloud-platform/microsoft-intune) pour obtenir un abonnement d’évaluation à Intune.

- Ordinateur Windows Server 2016 ou version ultérieure exécutant Active Directory.

  Pour plus d’informations sur la configuration de Active Directory [, voir procédure pas à pas : configuration de Active Directory dans Windows Server 2016](https://blogs.technet.microsoft.com/canitpro/2017/02/22/step-by-step-setting-up-active-directory-in-windows-server-2016/) .

- Un ordinateur Windows Server 2016 ou version ultérieure dédié, qui s’exécute en tant que serveur d’impression.

- Un ordinateur Windows Server 2016 ou ultérieur dédié à un domaine, qui s’exécute en tant que serveur connecteur.

  Pour plus d’informations, consultez [comprendre les connecteurs de proxy d’Application Azure ad](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-connectors) .

- Une mise à jour du créateur de automne Windows 10 ou une machine ultérieure pour la publication d’imprimantes.

- Nom de domaine public.

  Vous pouvez utiliser le nom de domaine créé pour vous par Azure (*nomdomaine*. onmicrosoft.com) ou acheter votre propre nom de domaine. Consultez [ajouter votre nom de domaine personnalisé à l’aide du portail Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/add-custom-domain).

## <a name="deployment-steps"></a>Étapes de déploiement

Les étapes ci-dessous concernent un déploiement d’impression de Cloud hybride classique.

### <a name="step-1---install-azure-ad-connect"></a>Étape 1 : installer Azure AD Connect

1. Azure AD Connect synchronise Azure AD à l’annuaire Active Directory local. Sur l’ordinateur Windows Server avec Active Directory, téléchargez et installez le logiciel Azure AD Connect avec les paramètres Express. Consultez [prise en main de Azure ad Connect à l’aide des paramètres Express](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-express).

### <a name="step-2---install-application-proxy"></a>Étape 2 : installer le proxy d’application

1. Le proxy d’application permet aux utilisateurs de votre organisation d’accéder à des applications locales à partir du Cloud. Installez le proxy d’application sur le serveur du connecteur.
    - Pour obtenir des instructions d’installation, consultez [Didacticiel : ajouter une application locale pour l’accès à distance via le proxy d’application dans Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application).
    - Un groupe de connecteurs dédié est recommandé si l’organisation dispose d’une topologie de réseau complexe. Consultez [publier des applications sur des réseaux et emplacements distincts à l’aide de groupes de connecteurs](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-connector-groups).

### <a name="step-3---register-and-configure-applications"></a>Étape 3 : inscrire et configurer des applications

Pour activer la communication authentifiée avec les services HCP, nous devons créer 3 applications : 2 applications Web pour représenter les deux services HCP et 1 application native pour communiquer avec ces services.

1. Connectez-vous à Portail Azure pour inscrire des applications Web.
    - Sous Azure Active Directory, accédez à **inscriptions d’applications** > **nouvelle inscription**.

    ![Inscription d’application AAD 1](../media/hybrid-cloud-print/AAD-AppRegistration.png)

    - Entrez un nom d’application pour le service de découverte Mopria. Cliquez sur **s’inscrire** pour terminer.

    ![Inscription d’application AAD 2](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria.png)

    - Répétez l’opération pour le service d’impression Cloud d’entreprise.
    - Répétez cette opération pour l’application native.
    - Les trois applications doivent être affichées sous **inscriptions d’applications**.

    ![Inscription d’application AAD 3](../media/hybrid-cloud-print/AAD-AppRegistration-AllApps.png)

2. Exposez l’API pour les deux applications Web.
    - Toujours dans le panneau **inscriptions d’applications** , cliquez sur l’application Mopria Discovery Service, sélectionnez **exposer une API**, puis cliquez sur **définir** en regard de Uri ID d’application.

    ![API d’exposition AAD 1](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-ExposeAPI.png)

    - Cliquez sur **Enregistrer** sans modifier la valeur par défaut de l’URI ID d’application. Cette valeur doit être définie maintenant et sera modifiée ultérieurement.

    ![API d’exposition AAD 2](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-ExposeAPI-Save.png)

    - Cliquez sur **Ajouter une étendue**.

    ![API d’exposition AAD 3](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-ExposeAPI-AddScope.png)

    - Spécifiez un nom d’étendue, autorisez les administrateurs et les utilisateurs à donner leur consentement, entrez la description de l’accord, puis cliquez sur **Ajouter une étendue** pour terminer.

    ![API d’exposition AAD 4](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-ExposeAPI-ScopeName.png)

    - Répétez l’opération pour le service d’impression Cloud d’entreprise. Utilisez un autre nom de portée et une autre description de consentement.

    ![API d’exposition AAD 5](../media/hybrid-cloud-print/AAD-AppRegistration-ECP-ExposeAPI-ScopeName.png)

3. Ajouter des autorisations d’API
    - Revenez au panneau inscriptions d’applications. Cliquez sur l’application native et sélectionnez autorisations de l’API. Cliquez sur **Ajouter une autorisation**.

    ![Autorisation d’API AAD 1](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission.png)

    - Basculez vers les **API utilisées par mon organisation**, puis utilisez la zone de recherche pour rechercher le service de découverte Mopria ajouté précédemment. Cliquez sur le service dans le résultat de la recherche.

    ![Autorisation d’API AAD 2](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-Mopria.png)

    - Sélectionnez **autorisations déléguées**. Cochez la case en regard de l’étendue de l’API. Cliquez sur **Ajouter des autorisations**.

    ![Autorisation d’API AAD 3](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-Mopria-Add.png)

    - Répétez cette opération pour ajouter des autorisations au service d’impression Cloud d’entreprise.

    ![Autorisation d’API AAD 4](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-ECP-Add.png)

    - Une fois renvoyé au panneau autorisations d’API, patientez 10 secondes avant de cliquer sur consentement de l' **administrateur général...** .

    ![Autorisation de l’API AAD 5](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-GrantConsent.png)

    - Cliquez sur **Oui** lorsque vous y êtes invité.

    ![Autorisation de l’API AAD 6](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-GrantConsent-Yes.png)

    - Vérifiez que la colonne État de l’autorisation de l’API est affichée avec des coches vertes.

    ![Autorisation de l’API AAD 7](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-Verify.png)

4. Configurer le proxy d’application pour les applications Web
    - Accédez à **Azure Active Directory** > **applications d’entreprise** > **toutes les applications**. Recherchez le service de découverte Mopria et cliquez dessus.

    ![Proxy d’application AAD 1](../media/hybrid-cloud-print/AAD-EnterpriseApp-AllApps.png)

    - Cliquez sur **proxy d’application**. Entrez l’URL interne en utilisant le format `https://<fully qualified domain name of the Print Server>/mcs/`. Cliquez sur **Enregistrer** pour terminer.

    ![Proxy d’application AAD 2](../media/hybrid-cloud-print/AAD-EnterpriseApp-Mopria-AppProxy.png)

    - Répétez l’opération pour le service d’impression Cloud d’entreprise. Notez que l’URL interne est `https://<fully qualified domain name of the Print Server>/ecp/`.

    ![Proxy d’application AAD 3](../media/hybrid-cloud-print/AAD-EnterpriseApp-ECP-AppProxy.png)

    - Accédez à **Azure Active Directory** > **inscriptions d’applications**. Cliquez sur le service de découverte Mopria. Sous **vue d’ensemble**, Notez que l’URI de l’ID d’application a été remplacé par la valeur par défaut URL externe sous **proxy d’application**. L’URI sera utilisé lors de l’installation du serveur d’impression, dans la stratégie MDM du client et pour la publication de l’imprimante.

    ![Proxy d’application AAD 4](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-Overview.png)

5. Affecter des utilisateurs à des applications
    - Accédez à **Azure Active Directory** > **applications d’entreprise** > **toutes les applications**. Recherchez le service de découverte Mopria et cliquez dessus.
    - Cliquez sur **utilisateurs et groupes** et affectez des utilisateurs, ou cliquez **sur Propriétés** et modifiez **affectation d’utilisateur obligatoire ?** en **non** .
    - Répétez l’opération pour le service d’impression Cloud d’entreprise.

6. Configurer l’URI de redirection dans l’application native
    - Accédez à **Azure Active Directory** > **inscriptions d’applications**. Cliquez sur l’application native. Accédez à **vue d’ensemble** et copiez l’ID de l' **application (client)** .

    ![URI de redirection AAD 1](../media/hybrid-cloud-print/AAD-AppRegistration-Native-Overview.png)

    - Accédez à **authentification**. Modifiez la zone de liste déroulante **type** pour `Public...`, puis entrez deux URI de redirection selon le format ci-dessous, où `<NativeClientAppID>` est l’étape précédente :

        `ms-appx-web://Microsoft.AAD.BrokerPlugin/<NativeClientAppID>`

        `ms-appx-web://Microsoft.AAD.BrokerPlugin/S-1-15-2-3784861210-599250757-1266852909-3189164077-45880155-1246692841-283550366`

    ![URI de redirection AAD 2](../media/hybrid-cloud-print/AAD-AppRegistration-Native-Authentication.png)

    - Cliquez sur **Enregistrer** pour terminer.

### <a name="step-4---setup-the-print-server"></a>Étape 4 : configurer le serveur d’impression

1. Assurez-vous que tous les Windows Update disponibles sont installés sur le serveur d’impression. Remarque : le serveur 2019 doit être corrigé pour générer 17763,165 ou version ultérieure.
    - Installez les rôles de serveur suivants :
        - Rôle serveur d’impression
        - Internet Information Services (IIS)
    - Pour plus d’informations sur l’installation des rôles de serveur, consultez [installer des rôles, des services de rôle et des fonctionnalités à l’aide de l’Assistant Ajout de rôles et de fonctionnalités](https://docs.microsoft.com/windows-server/administration/server-manager/install-or-uninstall-roles-role-services-or-features#BKMK_installarfw) .

    ![Rôles de serveur d’impression](../media/hybrid-cloud-print/PrintServer-Roles.png)

2. Installez les modules PowerShell d’impression du Cloud hybride.
    - Exécutez les commandes suivantes à partir d’une invite de commandes PowerShell avec élévation de privilèges :

        `find-module -Name PublishCloudPrinter` pour confirmer que l’ordinateur peut atteindre le PowerShell Gallery (PSGallery)

        `install-module -Name PublishCloudPrinter`

    > Remarque : vous pouvez voir une messagerie indiquant que « PSGallery » est un référentiel non approuvé.  Entrez « y » pour poursuivre l’installation.

    ![Publication sur le serveur d’impression imprimante Cloud](../media/hybrid-cloud-print/PrintServer-PublishCloudPrinter.png)

3. Installez la solution d’impression de Cloud hybride.
    - Dans la même invite de commandes PowerShell avec élévation de privilèges, remplacez le répertoire par celui ci-dessous (guillemets requis) :

        `C:\Program Files\WindowsPowerShell\Modules\PublishCloudPrinter\1.0.0.0`

    - Run

        `.\CloudPrintDeploy.ps1 -AzureTenant <Azure Active Directory domain name> -AzureTenantGuid <Azure Active Directory ID>`

    - Reportez-vous à la capture d’écran ci-dessous pour trouver le nom de domaine Azure Active Directory.

    ![Serveur d’impression procédure d’obtention du nom de domaine AAD](../media/hybrid-cloud-print/PrintServer-GetAADDomainName.png)

    - Reportez-vous à la capture d’écran ci-dessous pour trouver l’ID de Azure Active Directory.

    ![Serveur d’impression-imprimer le déploiement sur le Cloud](../media/hybrid-cloud-print/PrintServer-GetAADId.png)

    - La sortie du script CloudPrintDeploy ressemble à ceci :

    ![Serveur d’impression-imprimer le déploiement sur le Cloud](../media/hybrid-cloud-print/PrintServer-CloudPrintDeploy.png)

    - Consultez le fichier journal pour voir s’il y a une erreur : `C:\Program Files\WindowsPowerShell\Modules\PublishCloudPrinter\1.0.0.0\CloudPrintDeploy.log`

4. Exécutez **RegitEdit** dans une invite de commandes avec élévation de privilèges. Accéder à l’ordinateur \ HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows\CurrentVersion\CloudPrint\EnterpriseCloudPrintService.
    - Assurez-vous que AzureAudience est défini sur l’URI d’ID d’application de l’application d’impression Cloud d’entreprise.
    - Assurez-vous que AzureTenant est défini sur le nom de domaine Azure AD.

    ![Clés de Registre ECP du serveur d’impression](../media/hybrid-cloud-print/PrintServer-RegEdit-ECP.png)

5. Accéder à l’ordinateur \ HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows\CurrentVersion\CloudPrint\MopriaDiscoveryService.
    - Vérifiez que AzureAudience est l’URI d’ID d’application de l’application de service de découverte Mopria.
    - Assurez-vous que AzureTenant est le nom de domaine Azure AD.
    - Assurez-vous que URL est l’URI d’ID d’application de l’application de service de découverte Mopria.

    ![Clés de Registre Mopria du serveur d’impression](../media/hybrid-cloud-print/PrintServer-RegEdit-Mopria.png)

6. Exécutez **IISReset** dans une invite de commandes PowerShell d’élévation. Cela permet de s’assurer que toute modification du Registre effectuée à l’étape précédente prend effet.

7. Configurez les points de terminaison IIS pour prendre en charge SSL.
    - Le certificat SSL peut être un certificat auto-signé ou un certificat émis par une autorité de certification approuvée.
    - Si vous utilisez un certificat auto-signé, assurez-vous **que le certificat est importé sur la ou les machines clientes**.
    - Si vous inscrivez votre domaine auprès d’un fournisseur tiers, vous devrez configurer les points de terminaison IIS avec un certificat SSL. Pour plus d’informations, consultez ce [Guide](https://www.sslsupportdesk.com/microsoft-server-2016-iis-10-10-5-ssl-installation/) .

8. Installez le package SQLite.
   - Ouvrez une invite de commandes PowerShell avec élévation de privilèges.
   - Exécutez la commande suivante pour télécharger les packages NuGet System. Data. SQLite.

        `Register-PackageSource -Name nuget.org -ProviderName NuGet -Location https://www.nuget.org/api/v2/ -Trusted -Force`

   - Exécutez la commande suivante pour installer les packages.

        `Install-Package system.data.sqlite [-requiredversion x.x.x.x] -providername nuget`

   > Remarque : il est recommandé de télécharger et d’installer la dernière version en laissant l’option-requiredVersion.

    ![Clés de Registre Mopria du serveur d’impression](../media/hybrid-cloud-print/PrintServer-InstallSQLite.png)

9. Copiez les dll SQLite dans le dossier MopriaCloudService webapp bin (C:\inetpub\wwwroot\MopriaCloudService\bin).
    - Créez un fichier. ps1 contenant le script PowerShell ci-dessous.
    - Remplacez la variable $version par la version SQLite installée à l’étape précédente.
    - Exécutez le fichier. ps1 dans une invite de commandes PowerShell avec élévation de privilèges.

    ```powershell
    $source = \Program Files\PackageManagement\NuGet\Packages
    $core = System.Data.SQLite.Core
    $linq = System.Data.SQLite.Linq
    $ef6 = System.Data.SQLite.EF6
    $version = x.x.x.x
    $target = C:\inetpub\wwwroot\MopriaCloudService\bin

    xcopy /y $source\$core.$version\lib\net46\System.Data.SQLite.dll $target\
    xcopy /y $source\$core.$version\build\net46\x86\SQLite.Interop.dll $target\x86\
    xcopy /y $source\$core.$version\build\net46\x64\SQLite.Interop.dll $target\x64\
    xcopy /y $source\$linq.$version\lib\net46\System.Data.SQLite.Linq.dll $target\
    xcopy /y $source\$ef6.$version\lib\net46\System.Data.SQLite.EF6.dll $target\
    ```

10. Mettez à jour le fichier c:\inetpub\wwwroot\MopriaCloudService\web.config pour inclure la version SQLite x. x. x dans les sections `<runtime>/<assemblyBinding>` suivantes. Il s’agit de la même version que celle utilisée à l’étape précédente.

    ```xml
    ...
    <dependentAssembly>
    assemblyIdentity name=System.Data.SQLite culture=neutral publicKeyToken=db937bc2d44ff139 /
    <bindingRedirect oldVersion=0.0.0.0-x.x.x.x newVersion=x.x.x.x />
    </dependentAssembly>
    <dependentAssembly>
    <assemblyIdentity name=System.Data.SQLite.Core culture=neutral publicKeyToken=db937bc2d44ff139 />
    <bindingRedirect oldVersion=0.0.0.0-x.x.x.x newVersion=x.x.x.x />
    </dependentAssembly>
    <dependentAssembly>
    <assemblyIdentity name=System.Data.SQLite.EF6 culture=neutral publicKeyToken=db937bc2d44ff139 />
    <bindingRedirect oldVersion=0.0.0.0-x.x.x.x newVersion=x.x.x.x />
    </dependentAssembly>
    <dependentAssembly>
    <assemblyIdentity name=System.Data.SQLite.Linq culture=neutral publicKeyToken=db937bc2d44ff139 />
    <bindingRedirect oldVersion=0.0.0.0-x.x.x.x newVersion=x.x.x.x />
    </dependentAssembly>
    ...
    ```

11. Créez la base de données SQLite.
    - Téléchargez et installez les fichiers binaires des outils SQLite à partir de `https://www.sqlite.org/`.
    - Accédez à `c:\inetpub\wwwroot\MopriaCloudService\Database` Directory.
    - Exécutez la commande suivante pour créer la base de données dans ce répertoire :

        `sqlite3.exe MopriaDeviceDb.db .read MopriaSQLiteDb.sql`

    - Dans l’Explorateur de fichiers, ouvrez les propriétés du fichier MopriaDeviceDb. DB pour ajouter des utilisateurs ou des groupes qui sont autorisés à publier dans la base de données Mopria sous l’onglet sécurité. Les utilisateurs ou les groupes doivent exister dans les Active Directory locaux et synchronisés avec Azure AD.
    - Si la solution est déployée sur un domaine non routable (par exemple, *MyDomain*. local), le domaine Azure ad (par exemple, *nomdomaine*. onmicrosoft.com ou un fournisseur tiers) doit être ajouté comme suffixe UPN à la Active Directory locale. Ainsi, vous pouvez ajouter exactement le même utilisateur qui publiera des imprimantes (par exemple, admin@*nomdomaine*. onmicrosoft.com) dans le paramètre de sécurité du fichier de base de données. Consultez [préparer un domaine non routable pour la synchronisation d’annuaires](https://docs.microsoft.com/office365/enterprise/prepare-a-non-routable-domain-for-directory-synchronization).

    ![Clés de Registre Mopria du serveur d’impression](../media/hybrid-cloud-print/PrintServer-SQLiteDB.png)

### <a name="step-5-optional---configure-pre-authentication-with-azure-ad"></a>Étape 5 \[\] facultative-configurer la pré-authentification avec Azure AD

1. Passez en revue le document [délégation Kerberos confrontée pour l’authentification unique à vos applications avec le proxy d’application](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-with-kcd).

2. Configurez des Active Directory locaux.
    - Sur l’ordinateur Active Directory, ouvrez Gestionnaire de serveur et accédez à **outils** > **Active Directory utilisateurs et ordinateurs**.
    - Accédez au nœud **ordinateurs** et sélectionnez le serveur du connecteur.
    - Cliquez avec le bouton droit et sélectionnez **propriétés** -> onglet de **délégation** .
    - Sélectionnez **n’approuver cet ordinateur que pour la délégation aux services spécifiés**.
    - Sélectionnez **utiliser n’importe quel protocole d’authentification**.
    - Sous **services auxquels ce compte peut présenter des informations d’identification déléguées**.
        - Ajoutez le nom de principal du service (SPN) de l’ordinateur du serveur d’impression.
        - Sélectionnez hôte comme type de service.
    ![Active Directory la délégation](../media/hybrid-cloud-print/AD-Delegation.png)

3. Vérifiez que l’authentification Windows est activée dans IIS.
    - Sur le serveur d’impression, ouvrez Gestionnaire de serveur > Outils > Gestionnaire Internet Information Services (IIS).
    - Accédez au site.
    - Double-cliquez sur **authentification**.
    - Cliquez sur **authentification Windows** , puis sur **activer** sous **actions**.
    ![l’authentification IIS du serveur d’impression](../media/hybrid-cloud-print/PrintServer-IIS-Authentication.png)

4. Configurez l’authentification unique.
    - Sur Portail Azure, accédez à **Azure Active Directory** > **applications d’entreprise** > **toutes les applications**.
    - Sélectionnez application MopriaDiscoveryService.
    - Accédez au **proxy d’application**. Remplacez la méthode de pré-authentification par **Azure Active Directory**.
    - Accédez à **authentification unique**. Sélectionnez authentification Windows intégrée comme méthode d’authentification unique.
    - Définissez **SPN d’application interne** sur le nom de principal du service de l’ordinateur du serveur d’impression.
    - Définit l' **identité de connexion déléguée** sur le nom d’utilisateur principal.
    - Répétez l’opération pour l’application EntperiseCloudPrint.
    ![l’authentification unique AAD IWA](../media/hybrid-cloud-print/AAD-SingleSignOn-IWA.png)

### <a name="step-6---configure-the-required-mdm-policies"></a>Étape 6 : configurer les stratégies MDM requises

1. Connectez-vous à votre fournisseur MDM.
2. Recherchez le groupe de stratégies d’impression Cloud d’entreprise et configurez les stratégies en suivant les instructions ci-dessous :
    - CloudPrintOAuthAuthority = `https://login.microsoftonline.com/<Azure AD Directory ID>`. L’ID de répertoire se trouve sous Azure Active Directory propriétés >.
    - CloudPrintOAuthClientId = valeur de l’ID\) client de l’application \(de l’application native. Vous pouvez le trouver sous Azure Active Directory > inscriptions d’applications > sélectionnez la vue d’ensemble de l’application native >.
    - CloudPrinterDiscoveryEndPoint = URL externe de l’application de service de découverte Mopria. Vous pouvez le trouver sous Azure Active Directory > applications d’entreprise > sélectionnez l’application Mopria Discovery Service > proxy d’application. **Elle doit être exactement la même, mais sans la fin/** .
    - MopriaDiscoveryResourceId = URI de l’ID d’application de l’application de service de découverte Mopria. Vous pouvez le trouver sous Azure Active Directory > inscriptions d’applications > sélectionnez l’application Mopria Discovery Service vue d’ensemble >. **Elle doit être exactement la même avec le/** .
    - CloudPrintResourceId = URI de l’ID d’application de l’application d’impression Cloud d’entreprise. Vous pouvez le trouver sous Azure Active Directory > inscriptions d’applications > sélectionnez la vue d’ensemble de l’application d’impression Cloud d’entreprise >. **Elle doit être exactement la même avec le/** .
    - DiscoveryMaxPrinterLimit = \<un entier positif\>.

> Remarque : Si vous utilisez Microsoft Intune service, vous pouvez trouver ces paramètres sous la catégorie imprimante Cloud.

|Nom complet Intune                     |Stratégie                         |
|----------------------------------------|-------------------------------|
|URL de détection d’imprimante                   |CloudPrinterDiscoveryEndpoint  |
|URL de l’autorité d’accès à l’imprimante            |CloudPrintOAuthAuthority       |
|GUID de l’application Azure Native Client            |CloudPrintOAuthClientId        |
|URI de ressource du service d’impression              |CloudPrintResourceId           |
|Nombre maximal d’imprimantes à interroger (mobile uniquement)  |DiscoveryMaxPrinterLimit       |
|URI de ressource du service de découverte d’imprimantes  |MopriaDiscoveryResourceId      |

> Remarque : si le groupe de stratégies d’impression Cloud n’est pas disponible, mais que le fournisseur MDM prend en charge les paramètres OMA-URI, vous pouvez définir les mêmes stratégies.  Pour plus d’informations, reportez-vous à [cette](https://docs.microsoft.com/windows/client-management/mdm/policy-csp-enterprisecloudprint#enterprisecloudprint-cloudprintoauthauthority) rubrique.

    - Valeurs pour OMA-URI
        - CloudPrintOAuthAuthority =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthAuthority
            - Valeur = https://login.microsoftonline.com/<Azure AD Directory ID>
        - CloudPrintOAuthClientId =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthClientId
            - Valeur = < Azure AD ID d’application de l’application native >
        - CloudPrinterDiscoveryEndPoint =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrinterDiscoveryEndPoint
            - Valeur = URL externe de l’application de service de découverte Mopria (doit être exactement la même, mais sans la fin/)
        - MopriaDiscoveryResourceId =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/MopriaDiscoveryResourceId
            - Valeur = l’URI d’ID d’application de l’application de service de découverte Mopria
        - CloudPrintResourceId =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintResourceId
            - Valeur = l’URI d’ID d’application de l’application d’impression Cloud d’entreprise
        - DiscoveryMaxPrinterLimit =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/DiscoveryMaxPrinterLimit
            - Valeur = un entier positif
    
### <a name="step-7---publish-the-shared-printer"></a>Étape 7-publier l’imprimante partagée

1. Installez l’imprimante souhaitée sur le serveur d’impression.
2. Partagez l’imprimante par le biais de l’interface utilisateur des propriétés de l’imprimante.
3. Sélectionnez l’ensemble d’utilisateurs souhaité pour accorder l’accès.
4. Enregistrez les modifications et fermez la fenêtre Propriétés de l’imprimante.
5. Préparez une mise à jour du créateur de automne Windows 10 ou une machine ultérieure. Joignez l’ordinateur à Azure AD et connectez-vous en tant qu’utilisateur qui est synchronisé avec Active Directory local et qu’il a reçu l’autorisation appropriée pour le fichier MopriaDeviceDb. db.
6. À partir de l’ordinateur Windows 10, ouvrez une invite de commandes Windows PowerShell avec élévation de privilèges.
    - Exécutez les commandes suivantes.
        - `find-module -Name PublishCloudPrinter` pour confirmer que l’ordinateur peut atteindre le PowerShell Gallery (PSGallery)
        - `install-module -Name PublishCloudPrinter`

            > Remarque : vous pouvez voir une messagerie indiquant que « PSGallery » est un référentiel non approuvé.  Entrez « y » pour poursuivre l’installation.

        - `Publish-CloudPrinter`
        - Printer = nom de l’imprimante partagée. Ce nom doit correspondre exactement au nom de partage indiqué sous l’onglet **partage** des propriétés de l’imprimante, ouvert sur le serveur d’impression.

        ![Propriétés de l’imprimante du serveur d’impression](../media/hybrid-cloud-print/PrintServer-PrinterProperties.png)

        - Fabricant = fabricant de l’imprimante.
        - Modèle = modèle d’imprimante.
        - OrgLocation = chaîne JSON spécifiant l’emplacement de l’imprimante, par exemple

            `{attrs: [{category:country, vs:USA, depth:0}, {category:organization, vs:Microsoft, depth:1}, {category:site, vs:Redmond, WA, depth:2}, {category:building, vs:Building 1, depth:3}, {category:floor_number, vs:1, depth:4}, {category:room_name, vs:1111, depth:5}]}`

        - SDDL = chaîne SDDL représentant les autorisations pour l’imprimante.
            - Connectez-vous au serveur d’impression en tant qu’administrateur, puis exécutez la commande PowerShell suivante sur l’imprimante que vous souhaitez publier : `(Get-Printer PrinterName -full).PermissionSDDL`.
            - Ajoutez **O :BA** comme préfixe au résultat à partir de la commande ci-dessus. Exemple Si la chaîne retournée par la commande précédente est G :DUD : (A ; OICI ; FA ;;; WD), puis SDDL = O :BAG : DUD : (A ; OICI ; FA ;;; WD).
        - DiscoveryEndpoint = Connectez-vous à Portail Azure, puis récupérez la chaîne à partir des applications d’entreprise > application de service de découverte Mopria > proxy d’application > URL externe. Omettez la fin/.
        - PrintServerEndpoint = Connectez-vous à Portail Azure, puis récupérez la chaîne à partir des applications d’entreprise > application d’impression Cloud d’entreprise > proxy d’application > URL externe. Omettez la fin/.
        - AzureClientId = ID d’application de l’application native inscrite.
        - AzureTenantGuid = ID de répertoire de votre locataire Azure AD.
        - DiscoveryResourceId = URI ID d’application de l’application de service de découverte Mopria.

    - Vous pouvez également entrer toutes les valeurs de paramètre requises dans la ligne de commande. La syntaxe est la suivante :

        `Publish-CloudPrinter -Printer <string> -Manufacturer <string> -Model <string> -OrgLocation <string> -Sddl <string> -DiscoveryEndpoint <string> -PrintServerEndpoint <string> -AzureClientId <string> -AzureTenantGuid <string> -DiscoveryResourceId <string>`

        Exemple de commande :

        `Publish-CloudPrinter -Printer HcpTestPrinter -Manufacturer Manufacturer1 -Model Model1 -OrgLocation '{attrs: [{category:country, vs:USA, depth:0}, {category:organization, vs:MyCompany, depth:1}, {category:site, vs:MyCity, State, depth:2}, {category:building, vs:Building 1, depth:3}, {category:floor_name, vs:1, depth:4}, {category:room_name, vs:1111, depth:5}]}' -Sddl O:BAG:DUD:(A;OICI;FA;;;WD) -DiscoveryEndpoint https://mopriadiscoveryservice-contoso.msappproxy.net/mcs -PrintServerEndpoint https://enterprisecloudprint-contoso.msappproxy.net/ecp -AzureClientId dbe4feeb-cb69-40fc-91aa-73272f6d8fe1 -AzureTenantGuid 8de6a14a-5a23-4c1c-9ae4-1481ce356034 -DiscoveryResourceId https://mopriadiscoveryservice-contoso.msappproxy.net/mcs/`

    - Utilisez la commande suivante pour vérifier que l’imprimante est publiée.

        `Publish-CloudPrinter -Query -DiscoveryEndpoint <string> -AzureClientId <string> -AzureTenantGuid <string> -DiscoveryResourceId <string>`

        Exemple de commande :

        `Publish-CloudPrinter -Query -DiscoveryEndpoint https://mopriadiscoveryservice-contoso.msappproxy.net/mcs -AzureClientId dbe4feeb-cb69-40fc-91aa-73272f6d8fe1 -AzureTenantGuid 8de6a14a-5a23-4c1c-9ae4-1481ce356034 -DiscoveryResourceId https://mopriadiscoveryservice-contoso.msappproxy.net/mcs/`

## <a name="verify-the-deployment"></a>Vérifier le déploiement

Sur un appareil Azure AD joint avec les stratégies MDM configurées :
- Ouvrez un navigateur Web et accédez à https://mopriadiscoveryservice-*nom-client*. msappproxy.net/MCS/services.
- Vous devez voir le texte JSON qui décrit l’ensemble des fonctionnalités de ce point de terminaison.
- Accédez à **paramètres** > **appareils** > **imprimantes & Scanneurs**.
    - Cliquez sur **Ajouter une imprimante ou un scanneur**.
    - Vous devez voir un lien Rechercher des imprimantes Cloud (ou Rechercher des imprimantes dans mon organisation sur un ordinateur Windows 10 plus récent).
    - Cliquez sur le lien.
    - Cliquez sur le lien Sélectionnez un emplacement de recherche.
        - La hiérarchie de l’emplacement de l’appareil doit s’afficher.
    - Choisissez un emplacement et cliquez sur **OK** , puis cliquez sur le bouton **Rechercher** pour rechercher les imprimantes.
    - Sélectionnez imprimante, puis cliquez sur le bouton **Ajouter un périphérique** .
    - Une fois l’imprimante correctement installée, imprimez sur l’imprimante à partir de votre application favorite.

> Remarque : Si vous utilisez l’imprimante EcpPrintTest, vous pouvez trouver le fichier de sortie sur l’ordinateur du serveur d’impression sous C :\\ECPTestOutput\\EcpTestPrint. Xps.

## <a name="troubleshooting"></a>Résolution des problèmes

Voici les problèmes courants lors du déploiement de HCP

|Error |Étapes recommandées |
|------|------|
|Échec du script PowerShell CloudPrintDeploy | <ul><li>Assurez-vous que Windows Server dispose de la dernière mise à jour.</li><li>Si Windows Server Update Services (WSUS) est utilisé, consultez [Comment rendre les fonctionnalités à la demande et les modules linguistiques disponibles lorsque vous utilisez WSUS/SCCM](https://docs.microsoft.com/windows/deployment/update/fod-and-lang-packs).</li></ul> |
|Échec de l’installation de SQLite avec le message : boucle de dépendance détectée pour le package’System. Data. SQLite' | Install-Package System. Data. sqlite. Core-ProviderName NuGet-SkipDependencies<br>Install-Package System. Data. sqlite. EF6-ProviderName NuGet-SkipDependencies<br>Install-Package System. Data. sqlite. Linq-ProviderName NuGet-SkipDependencies<br><br>Une fois les packages téléchargés, assurez-vous qu’ils ont la même version. Si ce n’est pas le cas, ajoutez le paramètre-requiredVersion aux commandes ci-dessus et affectez-lui la même version. |
|Échec de la publication de l’imprimante | <ul><li>Pour la pré-authentification directe, assurez-vous que l’utilisateur qui publie l’imprimante dispose des autorisations adéquates pour la base de données de publication.</li><li>Pour Azure AD la pré-authentification, assurez-vous que l’authentification Windows est activée dans IIS. Consultez l’étape 5,3. En outre, essayez d’abord la pré-authentification PassThrough. Si la pré-authentification directe fonctionne, le problème est probablement lié au proxy d’application. Consultez [résoudre les problèmes liés au proxy d’application et aux messages d’erreur](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-troubleshoot). Notez que le basculement vers passthrough réinitialise le paramètre d’authentification unique ; reportez-vous à l’étape 5 pour configurer Azure AD à nouveau l’authentification.</li></ul> |
|Les travaux d’impression sont conservés dans l’état de l’imprimante | <ul><li>Vérifiez que TLS 1,2 est activé sur le serveur du connecteur. Consultez l’article lié à l’étape 2,1.</li><li>Vérifiez que HTTP2 est désactivé sur le serveur du connecteur. Consultez l’article lié à l’étape 2,1.</li></ul> |

Voici les emplacements des journaux qui peuvent aider à résoudre les problèmes

|Component |Emplacement du journal |
|------|------|
|Client Windows 10 | <ul><li>Utilisez observateur d’événements pour voir le journal des opérations de Azure AD. Cliquez sur **Démarrer** , puis tapez observateur d’événements. Accédez à journaux des applications et des services > Microsoft > Windows > AAD > opération.</li><li>Utilisez le hub de commentaires pour collecter les journaux. Consultez [Envoyer des commentaires à Microsoft avec l’application Hub de commentaires](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)</li></ul> |
|Serveur de connecteur | Utilisez observateur d’événements pour afficher le journal du proxy d’application. Cliquez sur **Démarrer** , puis tapez observateur d’événements. Accédez à journaux des applications et des services > le connecteur Microsoft > AadApplicationProxy > > administrateur. |
|Serveur d’impression | Les journaux de l’application de service de découverte Mopria et de l’application d’impression Cloud d’entreprise se trouvent dans C:\inetpub\logs\LogFiles\W3SVC1. |
