---
title: Déployer Windows Server hybride Cloud Print-authentification directe
description: Comment configurer l’impression Cloud hybride Microsoft avec l’authentification PassThrough
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: Windows Server 2016
ms.tgt_pltfrm: na
ms.topic: ''
ms.assetid: fc239aec-e719-47ea-92fc-d82a7247c5e9
author: msjimwu
ms.author: coreyp
manager: dongill
ms.date: 3/15/2018
ms.openlocfilehash: e9d461e2e9442e9473a6d2c9b13d9ede17361348
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370398"
---
# <a name="deploy-windows-server-hybrid-cloud-print-with-passthrough-authentication"></a>Déployer Windows Server hybride Cloud Print avec l’authentification PassThrough

>S’applique à Windows Server 2016

Cette rubrique, destinée aux administrateurs informatiques, décrit le déploiement de bout en bout de la solution Microsoft hybride Cloud Print. Cette solution se découche sur les serveurs Windows existants s’exécutant en tant que serveur d’impression et permet aux appareils Azure Active Directory joints et gérés par MDM de découvrir et d’imprimer sur les imprimantes gérées par l’organisation.

## <a name="pre-requisites"></a>Prérequis

Vous devez acquérir un certain nombre d’abonnements, de services et d’ordinateurs avant de commencer cette installation. En voici la liste :

-   Abonnement Azure AD Premium
    
    Pour un abonnement d’évaluation à Azure, consultez [prise en main d’un abonnement Azure](https://azure.microsoft.com/trial/get-started-active-directory/). 

-   Service MDM, tel qu’Intune
    
    Consultez [Microsoft Intune](https://www.microsoft.com/en-us/cloud-platform/microsoft-intune)pour obtenir un abonnement d’évaluation à Intune.

-   Windows Server s’exécutant en tant que Active Directory

    Pour plus d’informations sur la configuration de Active Directory, voir [procédure pas à pas : configuration de Active Directory dans Windows Server 2016](https://blogs.technet.microsoft.com/canitpro/2017/02/22/step-by-step-setting-up-active-directory-in-windows-server-2016/).

-   Joint au domaine Windows Server 2016 s’exécutant en tant que serveur d’impression
    
    Pour plus d’informations sur l’installation des rôles et des services de rôle sur Windows Server, consultez [installer des rôles, des services de rôle et des fonctionnalités à l’aide de l’Assistant Ajout de rôles et de fonctionnalités](https://docs.microsoft.com/windows-server/administration/server-manager/install-or-uninstall-roles-role-services-or-features#BKMK_installarfw).

-   Azure AD Connect
    
    Consultez [installation personnalisée de Azure ad Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-custom)pour obtenir de l’aide sur la configuration de Azure ad Connect.

-   Azure Application connecteur de proxy sur un ordinateur Windows Server joint à un domaine distinct
    
    Consultez [prise en main du proxy d’application et installation du connecteur](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable)pour obtenir de l’aide sur la configuration de Azure application connecteur proxy.

-   Nom de domaine public
    
    Vous pouvez utiliser le nom de domaine créé pour vous par Azure ou acheter votre propre nom de domaine.

## <a name="deployment-steps"></a>Étapes de déploiement

Ce guide décrit cinq (5) étapes d’installation :

- Étape 1 : installer Azure AD Connect pour effectuer une synchronisation entre des Azure AD et des services AD locaux
- Étape 2 : installer le package d’impression du Cloud hybride sur le serveur d’impression
- Étape 3 : installer Azure Application proxy (AAP) avec l’authentification directe
- Étape 4 : configurer les stratégies MDM requises
- Étape 5 : publier des imprimantes partagées

### <a name="step-1---install-azure-ad-connect-to-sync-between-azure-ad-and-on-premises-ad"></a>Étape 1 : installer Azure AD Connect pour effectuer une synchronisation entre Azure AD et un AD local
1. Sur l’ordinateur Windows Server Active Directory, téléchargez le logiciel Azure AD Connect
2. Lancez le package d’installation de Azure AD Connect et sélectionnez **utiliser les paramètres rapides**
3. Entrez les informations demandées dans le processus d’installation, puis cliquez sur **installer** .

### <a name="step-2---install-hybrid-cloud-print-package-on-the-print-server"></a>Étape 2 : installer le package d’impression hybride Cloud sur le serveur d’impression

1. Installer le Cloud hybride imprimer les modules PowerShell
   - Exécutez les commandes suivantes à partir d’une invite de commandes PowerShell avec élévation de privilèges
      - `find-module -Name "PublishCloudPrinter"` pour confirmer que l’ordinateur peut atteindre le PowerShell Gallery (PSGallery)
      - `install-module -Name "PublishCloudPrinter"`

     > Remarque : vous pouvez voir une messagerie indiquant que « PSGallery » est un référentiel non approuvé.  Entrez « y » pour poursuivre l’installation.

2. Installer la solution d’impression du Cloud hybride
    - Dans la même invite de commandes PowerShell avec élévation de privilèges, accédez au répertoire `C:\Program Files\WindowsPowerShell\Modules\PublishCloudPrinter\1.0.0.0`
    - Run <br>
        `CloudPrintDeploy.ps1 -AzureTenant <Domain name used by Azure AD Connect> -AzureTenantGuid <Azure AD Directory ID>`
3. Configurer les 2 points de terminaison IIS pour prendre en charge SSL
   -   Le certificat SSL peut être un certificat auto-signé ou un certificat émis par une autorité de certification approuvée.
   -  Si vous utilisez un certificat auto-signé, assurez-vous que le certificat est importé sur la ou les machines clientes.
4. Installer le package SQLite
   - Ouvrir une invite de commandes PowerShell avec élévation de privilèges
   - Exécutez la commande suivante pour télécharger les packages NuGet System. Data. SQLite <br>
       `Register-PackageSource -Name nuget.org -ProviderName NuGet -Location https://www.nuget.org/api/v2/ -Trusted -Force`
   - Exécutez la commande suivante pour installer les packages<br>
   `Install-Package system.data.sqlite [-requiredversion x.x.x.x] -providername nuget`

   > Remarque : il est recommandé de télécharger et d’installer la dernière version en laissant l’option « -requiredVersion ».

5. Copiez les dll SQLite dans le dossier MopriaCloudService webapp \<bin\> (**C :\\inetpub\\wwwroot\\MopriaCloudService\\bin**) : <br>
   - Les fichiers binaires SQLite doivent se trouver à l’adresse «\\Program Files\\PackageManagement\\packages de\\NuGet »

           \\System.Data.SQLite.**Core**.x.x.x.x\\lib\\net46\\System.Data.SQLite.dll
           --\> \<bin\>\\System.Data.SQLite.dll  
           \\System.Data.SQLite.**Core**.x.x.x.x\\build\\net46\\x86\\SQLite.Interop.dll
           --\> \<bin\>\\**x86**\\SQLite.Interop.dll  
           \\System.Data.SQLite.**Core**.x.x.x.x\\build\\net46\\x64\\SQLite.Interop.dll
           --\> \<bin\>\\**x64**\\SQLite.Interop.dll
           \\System.Data.SQLite.**Linq**.x.x.x.x\\lib\\net46\\System.Data.SQLite.Linq.dll
           --\> \<bin\>\\System.Data.SQLite.Linq.dll  
           \\System.Data.SQLite.**EF6**.x.x.x.x\\lib\\net46\\System.Data.SQLite.EF6.dll
           --\> \<bin\>\\System.Data.SQLite.EF6.dll

   > Remarque : x.x. x est la version SQLite installée ci-dessus.

6. Mettez à jour le fichier `c:\inetpub\wwwroot\MopriaCloudService\web.config` pour inclure SQLite version x. x. x. x dans les sections suivantes de \<Runtime\>/\<assemblyBinding\> :

       <dependentAssembly>
       assemblyIdentity name="System.Data.SQLite" culture="neutral" publicKeyToken="db937bc2d44ff139" /
       <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
       </dependentAssembly>
       <dependentAssembly>
       <assemblyIdentity name="System.Data.SQLite.Core" culture="neutral" publicKeyToken="db937bc2d44ff139" />
       <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
       </dependentAssembly>
       <dependentAssembly>
       <assemblyIdentity name="System.Data.SQLite.EF6" culture="neutral" publicKeyToken="db937bc2d44ff139" />
       <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
       </dependentAssembly>
       <dependentAssembly>
       <assemblyIdentity name="System.Data.SQLite.Linq" culture="neutral" publicKeyToken="db937bc2d44ff139" />
       <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
       </dependentAssembly>

7. Créez la base de données SQLite :
    -  Téléchargez et installez les binaires des outils SQLite à partir de <https://www.sqlite.org/>
    -  Accédez à **c :\\inetpub\\wwwroot\\MopriaCloudService\\répertoire de base de données**
    -  Exécutez la commande suivante pour créer la base de données dans ce répertoire : `sqlite3.exe MopriaDeviceDb.db ".read MopriaSQLiteDb.sql"`
    -  Dans l’Explorateur de fichiers, ouvrez les propriétés du fichier MopriaDeviceDb. DB pour ajouter des utilisateurs/groupes autorisés à publier dans la base de données Mopria sous l’onglet sécurité.
        - Vous devez uniquement ajouter le groupe d’utilisateurs administrateur requis.
8. Inscrire l’application Web 2 avec Azure AD pour prendre en charge l’authentification OAuth2
   - Connectez-vous en tant qu’administrateur général au portail de gestion des locataires Azure AD
     1. Accédez à l’onglet « inscriptions d’applications » pour ajouter un point de terminaison d’impression
        - Ajouter une application, sélectionnez « Nouvelle inscription d’application »
        - Fournissez un nom approprié et sélectionnez « application Web/API ».
        - URL de connexion = "<http://MicrosoftEnterpriseCloudPrint/CloudPrint>"
     2. Répéter pour le point de terminaison de découverte
        - URL de connexion = "<http://MopriaDiscoveryService/CloudPrint>"
     3. Répéter pour l’application cliente Native
        -   Lorsque vous fournissez le nom de l’application, veillez à sélectionner « application cliente Native » comme « type d’application ».
        -   URI de redirection = « https://\<services-machine-Endpoint\>/RedirectUrl »
     4. Accédez à la section « paramètres » de l’application cliente Native
        -   Copiez la valeur « ID d’application » à utiliser pour les étapes de configuration ultérieures.
        -   Sélectionnez « autorisations requises ».
            1.  Cliquez sur « Ajouter »
            2.  Cliquez sur « Sélectionner une API ».
            3.  Recherchez le point de terminaison d’impression et le point de terminaison de découverte par le nom que vous avez défini lors de la création du point de terminaison d’application
            4.  Ajouter les deux points de terminaison
            5.  Assurez-vous que l’option « autorisations déléguées » pour chaque point de terminaison d’application est activée
            6.  Veillez à cliquer sur le bouton « terminé » en bas
            7.  Cliquez sur « accorder des autorisations », une fois que les deux points de terminaison ont été ajoutés.  Sélectionnez « Oui » quand vous êtes invité à approuver la demande.
        -   Accédez à « URI de redirection » et ajoutez les URI de redirection suivants à la liste : `ms-appx-web://Microsoft.AAD.BrokerPlugin/\<NativeClientAppID\>`
            `ms-appx-web://Microsoft.AAD.BrokerPlugin/S-1-15-2-3784861210-599250757-1266852909-3189164077-45880155-1246692841-283550366`

### <a name="step-3---install-azure-application-proxy-aap-with-passthrough-authentication"></a>Étape 3 : installer Azure Application proxy (AAP) avec l’authentification directe
1. Connectez-vous à votre portail de gestion des locataires Azure AD (AAD)
    - Dans la liste de menus AAD, sélectionnez « proxy d’application ».
    - Cliquez sur « Activer le proxy d’application » en haut de l’écran.
    - Téléchargez le « connecteur de proxy d’application » sur un ordinateur Windows Server joint au domaine qui fera office de proxy d’application Web (WAP).
2. Sur l’ordinateur WAP, connectez-vous en tant qu’administrateur et installez le « connecteur de proxy d’application ».
    - Au cours de l’installation, donnez au connecteur du proxy d’application les informations d’identification de votre principe Azure-principe sur lequel vous souhaitez activer AAP
    - Assurez-vous que l’ordinateur WAP est joint à un domaine à votre Active Directory local
3. Revenez au portail de gestion des locataires AAD et ajoutez les proxys d’application.
   - Accéder à l’onglet **applications d’entreprise**
   - Cliquez sur **nouvelle application**
   - Sélectionnez **application locale** et renseignez les champs
       - Nom : n’importe quel nom de votre choix
       - URL interne : il s’agit de l’URL interne vers le service Cloud de découverte Mopria auquel votre ordinateur WAP peut accéder
       - URL externe : configurez le cas échéant pour votre organisation
       - Méthode de pré-authentification : passthrough

     >   Remarque : Si vous ne trouvez pas tous les paramètres ci-dessus, ajoutez le proxy avec les paramètres disponibles, puis sélectionnez le proxy d’application que vous venez de créer et accédez à l’onglet **proxy d’application** , puis ajoutez toutes les informations ci-dessus.

4. Répétez 3, ci-dessus, pour le service d’impression Cloud d’entreprise et fournissez l’URL interne de votre service d’impression Cloud d’entreprise.

    >   Remarque : le https://&lt;services-machine-Endpoint&gt;URL/MCS mentionnée ci-dessous doit être l’URL externe que vous avez configurée pour votre service Cloud Mopria et/ou service d’impression Cloud d’entreprise.


### <a name="step-4---configure-the-required-mdm-policies"></a>Étape 4 : configurer les stratégies MDM requises
- Connectez-vous à votre fournisseur MDM
- Recherchez le groupe de stratégies d’impression Cloud d’entreprise et configurez les stratégies en suivant les instructions ci-dessous :
  - CloudPrintOAuthAuthority = https://login.microsoftonline.com/\<Azure ID du répertoire AD\>
  - CloudPrintOAuthClientId = « ID d’application » de l’application Web native que vous avez inscrite dans Azure AD portail de gestion
  - CloudPrinterDiscoveryEndPoint = URL externe du proxy de Azure Application du service de découverte Mopria créé à l’étape 3,3 (doit être exactement le même mais sans la fin)
  - MopriaDiscoveryResourceId = « URI ID d’application » de l’application/API Web pour le point de terminaison de découverte inscrit à l’étape 2,8.  Vous pouvez le trouver sous les propriétés Paramètres-> de l’application.
  - CloudPrintResourceId = « URI ID d’application » de l’application/API Web pour le point de terminaison d’impression enregistré à l’étape 2,8. Vous pouvez le trouver sous les propriétés Paramètres-> de l’application.
  - DiscoveryMaxPrinterLimit = \<un entier positif\>

>   Remarque : Si vous utilisez Microsoft Intune service, vous pouvez trouver ces paramètres sous la catégorie « imprimante Cloud ».

|Nom complet Intune                     |Stratégie                         |
|----------------------------------------|-------------------------------|
|URL de détection d’imprimante                   |CloudPrinterDiscoveryEndpoint  |
|URL de l’autorité d’accès à l’imprimante            |CloudPrintOAuthAuthority       |
|GUID de l’application Azure Native Client            |CloudPrintOAuthClientId        |
|URI de ressource du service d’impression              |CloudPrintResourceId           |
|Nombre maximal d’imprimantes à interroger (mobile uniquement)  |DiscoveryMaxPrinterLimit       |
|URI de ressource du service de découverte d’imprimantes  |MopriaDiscoveryResourceId      |


>   Remarque : si le groupe de stratégies d’impression Cloud n’est pas disponible, mais que le fournisseur MDM prend en charge les paramètres OMA-URI, vous pouvez définir les mêmes stratégies.  Pour plus d’informations, reportez-vous à cet <a href="https://docs.microsoft.com/windows/client-management/mdm/policy-csp-enterprisecloudprint#enterprisecloudprint-cloudprintoauthauthority">article</a> .

- OMA-URI
    - `CloudPrintOAuthAuthority = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthAuthority`
        - Valeur = `https://login.microsoftonline.com/`\<ID du répertoire Azure AD\>
    - `CloudPrintOAuthClientId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthClientId`
        - Valeur = \<Azure AD ID d’application de l’application native\>
    - `CloudPrinterDiscoveryEndPoint = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrinterDiscoveryEndPoint`
        - Valeur = URL externe du service de découverte Mopria Azure Application le proxy créé à l’étape 3,3 (doit être exactement le même mais sans la fin)
    - `MopriaDiscoveryResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/MopriaDiscoveryResourceId`
        - Valeur = « URI ID d’application » de l’application/API Web pour le point de terminaison de découverte inscrit à l’étape 2,8.  Vous pouvez le trouver sous les propriétés Paramètres-> de l’application.
    - `CloudPrintResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintResourceId`
        - Valeur = « URI ID d’application » de l’application/API Web pour le point de terminaison d’impression enregistré à l’étape 2,8. Vous pouvez le trouver sous les propriétés Paramètres-> de l’application.
    - `DiscoveryMaxPrinterLimit = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/DiscoveryMaxPrinterLimit`
        - Valeur = \<un entier positif\>
  
### <a name="step-5---publish-desired-shared-printers"></a>Étape 5 : publication des imprimantes partagées souhaitées
1. Installer l’imprimante souhaitée sur le serveur d’impression
2. Partager l’imprimante par le biais de l’interface utilisateur des propriétés de l’imprimante
3. Sélectionnez l’ensemble d’utilisateurs souhaité pour accorder l’accès
4. Enregistrer les modifications et fermer la fenêtre Propriétés de l’imprimante
5. À partir d’un ordinateur de mise à jour Windows 10 automne Creator, ouvrez une invite de commandes Windows PowerShell avec élévation de privilèges.
   1. Exécutez les commandes suivantes
      - `find-module -Name "PublishCloudPrinter"` pour confirmer que l’ordinateur peut atteindre le PowerShell Gallery (PSGallery)
      - `install-module -Name "PublishCloudPrinter"`

        >   Remarque : vous pouvez voir une messagerie indiquant que « PSGallery » est un référentiel non approuvé.  Entrez « y » pour poursuivre l’installation.

      - Publication-CloudPrinter
        - Printer = nom de l’imprimante partagée qui a été définie
        - Fabricant = fabricant de l’imprimante
        - Modèle = modèle d’imprimante
        - OrgLocation = chaîne JSON spécifiant l’emplacement de l’imprimante, par exemple : `{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"Microsoft", "depth":1}, {"category":"site", "vs":"Redmond, WA", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}`
        - SDDL = chaîne SDDL représentant les autorisations pour l’imprimante. Pour cela, vous pouvez modifier l’onglet sécurité des propriétés de l’imprimante de manière appropriée, puis exécuter la commande suivante dans une invite de commandes : `(Get-Printer PrinterName -full).PermissionSDDL`
            par exemple, «G :DUD : (A ; OICI ; FA ;;; WD)»

          > Remarque : vous devez ajouter **`O:BA`** comme préfixe au résultat à partir de la commande d’invite de commandes ci-dessus avant de définir la valeur en tant que paramètre SDDL.  Exemple : SDDL = `O:BAG:DUD:(A;OICI;FA;;;WD)`

        - DiscoveryEndpoint = https://&lt;services-machine-Endpoint&gt;/MCS
        - PrintServerEndpoint = https://&lt;services-machine-Endpoint&gt;/ECP
        - AzureClientId = ID d’application de la valeur de l’application Web native inscrite ci-dessus
        - AzureTenantGuid = ID de répertoire de votre locataire Azure AD
        - DiscoveryResourceId = [facultatif] ID d’application du service Cloud de découverte Mopria proxy

        > Remarque : vous pouvez également entrer toutes les valeurs de paramètre requises dans la ligne de commande.<br>
        **Publication-CloudPrinter** Syntaxe de la commande PowerShell : <br>
        Publish-CloudPrinter-Printer \<chaîne\> fabricant \<chaîne\>-Model \<String\>-OrgLocation \<String\>-SDDL \<String\>-DiscoveryEndpoint \<String\>-PrintServerEndpoint \<String\>-AzureClientId \<String\>-AzureTenantGuid \<String\> [-DiscoveryResourceId \<String\>] <br>
        Exemple de commande : `publish-cloudprinter -Printer EcpPrintTest -Manufacturer Microsoft -Model FilePrinterEcp -OrgLocation '{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"MyCompany", "depth":1}, {"category":"site", "vs":"MyCity, State", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}' -Sddl "O:BAG:DUD:(A;OICI;FA;;;WD)" -DiscoveryEndpoint https://<services-machine-endpoint>/mcs -PrintServerEndpoint https://<services-machine-endpoint>/ecp -AzureClientId <Native Web App ID> -AzureTenantGuid <Azure AD Directory ID> -DiscoveryResourceId <Proxied Mopria Discovery Cloud Service App ID>`


## <a name="verifing-the-deployment"></a>Vérification le déploiement
Sur un appareil Azure AD joint avec les stratégies MDM configurées :
- Ouvrez un navigateur Web et accédez à https://&lt;s services-machine-Endpoint&gt;/MCS/services
- Vous devez voir le texte JSON décrivant l’ensemble des fonctionnalités de ce point de terminaison.
- Accédez à « paramètres du système d’exploitation-\> appareils-\> imprimantes & Scanneurs »
    - Vous devez voir un lien « Rechercher des imprimantes Cloud »
    - Cliquez sur le lien
    - Cliquez sur le lien « Veuillez sélectionner un emplacement de recherche »
        - Vous devez voir la hiérarchie de l’emplacement de l’appareil
    - Choisissez un emplacement, cliquez sur **OK** , puis cliquez sur le bouton **Rechercher** pour rechercher les imprimantes.
    - Sélectionnez imprimante, puis cliquez sur le bouton **Ajouter un périphérique** .
    - Une fois l’imprimante correctement installée, imprimez sur l’imprimante à partir de votre application favorite

>   Remarque : Si vous utilisez l’imprimante « EcpPrintTest », vous pouvez trouver le fichier de sortie sur l’ordinateur du serveur d’impression sous « C :\\ECPTestOutput\\EcpTestPrint. Xps ».