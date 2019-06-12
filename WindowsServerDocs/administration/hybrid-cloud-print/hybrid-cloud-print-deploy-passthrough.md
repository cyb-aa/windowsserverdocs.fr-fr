---
title: Déployer Windows Server-impression Cloud hybride - authentification de relais
description: La configuration de l’impression de Cloud hybride de Microsoft avec l’authentification directe
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 95cced8dc06cc4ee3768addf65e17cf379e20454
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435751"
---
# <a name="deploy-windows-server-hybrid-cloud-print-with-passthrough-authentication"></a>Déployer l’impression de Cloud hybride Windows Server avec l’authentification directe

>S'applique à : Windows Server 2016

Cette rubrique, pour les administrateurs informatiques, décrit le déploiement de bout en bout de la solution d’impression de Cloud hybride de Microsoft. Couches de cette solution sur des serveurs Windows existants en cours d’exécution en tant que serveur d’impression et permet de joints à Azure Active Directory et gestion des appareils mobiles gérés, les appareils de découvrir et d’impression à organisation gérés imprimantes.

## <a name="pre-requisites"></a>Conditions préalables

Il existe un nombre d’abonnements, les services et les ordinateurs que vous devez vous procurer avant de commencer cette installation. En voici la liste :

-   Abonnement de Azure AD premium
    
    Consultez [bien démarrer avec un abonnement Azure](https://azure.microsoft.com/trial/get-started-active-directory/), pour un abonnement d’essai à Azure. 

-   Service de gestion des appareils mobiles, tel qu’Intune
    
    Consultez [Microsoft Intune](https://www.microsoft.com/en-us/cloud-platform/microsoft-intune), pour un abonnement d’essai à Intune.

-   Windows Server en cours d’exécution en tant qu’Active Directory

    Consultez [pas à pas : Configuration d’Active Directory dans Windows Server 2016](https://blogs.technet.microsoft.com/canitpro/2017/02/22/step-by-step-setting-up-active-directory-in-windows-server-2016/), de l’aide de configuration d’Active Directory.

-   Joint à un domaine Windows Server 2016 est en cours d’exécution en tant que serveur d’impression
    
    Consultez [installer des rôles, services de rôle et fonctionnalités à l’aide de l’ajouter des rôles et fonctionnalités Assistant](https://docs.microsoft.com/windows-server/administration/server-manager/install-or-uninstall-roles-role-services-or-features#BKMK_installarfw), pour plus d’informations sur la façon d’installer des rôles et des services de rôle sur Windows Server.

-   Azure AD Connect
    
    Consultez [installation personnalisée d’Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-custom), de l’aide de la configuration d’Azure AD Connect.

-   Connecteur de Proxy d’Application Azure sur un domaine distinct joint à un ordinateur Windows Server
    
    Consultez [prise en main le Proxy d’Application et installer le connecteur](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable), d’aide pour configurer le connecteur de Proxy d’Application Azure.

-   Nom de domaine accessible sur public
    
    Vous pouvez utiliser le nom de domaine créé pour vous par Azure, ou acheter votre propre nom de domaine.

## <a name="deployment-steps"></a>Étapes de déploiement

Ce guide décrit les étapes d’installation cinq (5) :

- Étape 1 : Installer Azure AD Connect pour la synchronisation entre Azure AD et AD local
- Étape 2 : Installer le package de l’impression de Cloud hybride sur le serveur d’impression
- Étape 3 : Installer le Proxy d’Application Windows Azure (AAP) avec l’authentification directe
- Étape 4 : Configurer les stratégies de gestion des appareils mobiles requis
- Étape 5 : Publier des imprimantes partagées

### <a name="step-1---install-azure-ad-connect-to-sync-between-azure-ad-and-on-premises-ad"></a>Étape 1 : installer Azure AD Connect pour la synchronisation entre Azure AD et AD local
1. Sur l’ordinateur de Windows Server Active Directory, téléchargez le logiciel d’Azure AD Connect
2. Lancez le package d’installation Azure AD Connect et sélectionnez **utiliser les paramètres express**
3. Entrez les informations demandées dans le processus d’installation, cliquez sur **installer**

### <a name="step-2---install-hybrid-cloud-print-package-on-the-print-server"></a>Étape 2 : le package d’installation hybride Cloud imprimer sur le serveur d’impression

1. Installez les modules PowerShell de Print de Cloud hybride
   - Exécutez les commandes suivantes à partir d’une invite de commandes PowerShell avec élévation de privilèges
      - `find-module -Name "PublishCloudPrinter"` pour confirmer que l’ordinateur peut atteindre la galerie PowerShell (PSGallery)
      - `install-module -Name "PublishCloudPrinter"`

     > REMARQUE : Vous pouvez voir une messagerie indiquant que « PSGallery » est un référentiel non approuvé.  Entrez « o » pour poursuivre l’installation.

2. Installer la solution d’impression de Cloud hybride
    - Dans la même invite de commande PowerShell avec élévation de privilèges, accédez au répertoire `C:\Program Files\WindowsPowerShell\Modules\PublishCloudPrinter\1.0.0.0`
    - Run <br>
        `CloudPrintDeploy.ps1 -AzureTenant <Domain name used by Azure AD Connect> -AzureTenantGuid <Azure AD Directory ID>`
3. Configurer les points de 2 terminaison IIS pour prendre en charge SSL
   -   Le certificat SSL peut être un certificat auto-signé ou un certificat émis à partir de certains autorité de certification approuvée (CA)
   -  Si vous utilisez un certificat auto-signé, assurez-vous que le certificat est importé vers les machines client
4. Installer le package de SQLite
   - Ouvrez une invite de commandes PowerShell avec élévation de privilèges
   - Exécutez la commande suivante pour télécharger les packages nuget System.Data.SQLite <br>
       `Register-PackageSource -Name nuget.org -ProviderName NuGet -Location https://www.nuget.org/api/v2/ -Trusted -Force`
   - Exécutez la commande suivante pour installer les packages<br>
   `Install-Package system.data.sqlite [-requiredversion x.x.x.x] -providername nuget`

   > REMARQUE : Il est recommandé pour télécharger et installer la dernière version en omettant le «-requiredversion « option.

5. Copiez les DLL de SQLite sur la MopriaCloudService Webapp \<bin\> dossier (**C:\\inetpub\\wwwroot\\MopriaCloudService\\bin**) : <br>
   - Les fichiers binaires SQLite doivent concerner «\\Program Files\\PackageManagement\\NuGet\\Packages »

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

   > Remarque : x.x.x.x est la version de SQLite installée précédemment.

6. Mise à jour le `c:\inetpub\wwwroot\MopriaCloudService\web.config` fichier à inclure le x.x.x.x de version de SQLite dans l’exemple suivant \<runtime\>/\<assemblyBinding\> sections :

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

7. Créer la base de données SQLite :
    -  Téléchargez et installez les fichiers binaires SQLite outils à partir de <https://www.sqlite.org/>
    -  Accédez à **c:\\inetpub\\wwwroot\\MopriaCloudService\\base de données** directory
    -  Exécutez la commande suivante pour créer la base de données dans ce répertoire :  `sqlite3.exe MopriaDeviceDb.db ".read MopriaSQLiteDb.sql"`
    -  Explorateur de fichiers, ouvrez les propriétés de fichier MopriaDeviceDb.db pour ajouter des utilisateurs/groupes qui sont autorisés à publier dans la base de données Mopria dans l’onglet sécurité
        - Vous recommandons d’ajouter uniquement le groupe d’utilisateurs administrateur requis.
8. Inscrire l’application 2 web avec Azure AD pour prendre en charge l’authentification OAuth2
   - Connectez-vous en tant que l’administrateur général pour le portail de gestion de locataire Azure AD
     1. Consultez l’onglet « Inscriptions d’application » pour ajouter le point de terminaison impression
        - Ajouter une application, sélectionnez « Nouvelle inscription d’application »
        - Spécifiez un nom approprié et sélectionnez « application Web / API »
        - URL d’authentification = «<http://MicrosoftEnterpriseCloudPrint/CloudPrint>»
     2. Répétez pour le point de terminaison de découverte
        - URL d’authentification = «<http://MopriaDiscoveryService/CloudPrint>»
     3. Répétez pour l’application cliente native
        -   Lorsque vous fournissez le nom de l’application, veillez à que sélectionner « Application cliente Native » comme « type d’Application »
        -   URI de redirection = « https://\<-machine-point de terminaison services\>/RedirectUrl »
     4. Accédez à l’application cliente Native « Paramètres »
        -   Copiez la valeur « ID d’Application » à utiliser pour les étapes de configuration ultérieures
        -   Sélectionnez « Autorisations requises »
            1.  Cliquez sur « Ajouter »
            2.  Cliquez sur « Sélectionner une API »
            3.  Recherchez le point de terminaison impression et le point de terminaison de découverte par le nom que vous avez défini lors de la création du point de terminaison d’application
            4.  Ajouter les points de 2 terminaison
            5.  Assurez-vous que l’option « Autorisations déléguées » pour chaque point de terminaison d’application est activée.
            6.  Assurez-vous que vous cliquez sur le bouton « Done » en bas
            7.  Cliquez sur « Accorder des autorisations », une fois que les deux points de terminaison ont été ajoutés.  Sélectionnez « Oui » quand vous y êtes invité à approuver la demande.
        -   Accédez à « URI de redirection » et ajouter l’URI de redirection suivante à la liste : `ms-appx-web://Microsoft.AAD.BrokerPlugin/\<NativeClientAppID\>`
            `ms-appx-web://Microsoft.AAD.BrokerPlugin/S-1-15-2-3784861210-599250757-1266852909-3189164077-45880155-1246692841-283550366`

### <a name="step-3---install-azure-application-proxy-aap-with-passthrough-authentication"></a>Étape 3 : installer le Proxy d’Application Azure (AAP) avec l’authentification directe
1. Connectez-vous à votre portail de gestion de locataire Azure AD (AAD)
    - Dans la liste de menu AAD, sélectionnez « Proxy d’Application »
    - Cliquez sur « Activer le proxy d’application » en haut de l’écran
    - Téléchargez « Application Proxy Connector » sur une machine Windows Server jointe au domaine qui agira en tant que le Proxy d’Application Web (WAP).
2. Sur l’ordinateur WAP, connectez-vous en tant qu’administrateur et installer le « connecteur Proxy d’Application »
    - Pendant l’installation, donner le connecteur de proxy d’application les informations d’identification à votre doctrine Azure que vous souhaitez activer AAP sur
    - Assurez-vous que l’ordinateur WAP est joint à Active Directory en local au domaine
3. Revenez au portail de gestion de client AAD et ajoutez les proxys d’application
   - Accédez à la **applications d’entreprise** onglet
   - Cliquez sur **nouvelle application**
   - Sélectionnez **On-premises application** et renseignez les champs
       - Nom : Nom de que votre choix
       - URL interne : Voici l’URL interne au Service Cloud découverte Mopria qui peut accéder à votre ordinateur WAP
       - URL externe : Configurez comme il convient pour votre organisation
       - Méthode de pré-authentification : Transfert direct

     >   Remarque: Si vous ne trouvez pas les paramètres ci-dessus, ajouter le proxy avec les paramètres disponibles et sélectionnez le proxy d’application que vous venez de créer, puis accédez à la **proxy d’Application** onglet et ajouter toutes les informations ci-dessus.

4. Répétez les 3, ci-dessus, pour le Service d’impression Enterprise Cloud et indiquez l’URL interne à votre Service d’impression de cloud aux entreprises

    >   Remarque: Le https://&lt;-machine-point de terminaison services &gt; /mcs URL mentionnée ci-dessous doit être l’URL externe, le programme d’installation pour votre Service de Cloud Mopria et/ou le Service impression Cloud d’entreprise.


### <a name="step-4---configure-the-required-mdm-policies"></a>Étape 4 : configurer les stratégies de gestion des appareils mobiles requis
- Connectez-vous à votre fournisseur de gestion des appareils mobiles
- Recherchez le groupe de stratégie d’impression Cloud d’entreprise et configurer les stratégies suivant les instructions ci-dessous :
  - CloudPrintOAuthAuthority = https://login.microsoftonline.com/\<Azure AD Directory ID\>
  - CloudPrintOAuthClientId = valeur « ID d’Application » de l’application Web Native que vous avez inscrite dans le portail de gestion Azure AD
  - CloudPrinterDiscoveryEndPoint = URL externe du Mopria découverte du Service Azure Proxy d’Application créé dans l’étape 3.3 (doit être exactement le même mais sans la fin /)
  - MopriaDiscoveryResourceId = « URI ID d’application » de l’application Web / API pour le point de terminaison de découverte inscrits dans l’étape 2.8.  Vous pouvez le trouver sous les paramètres -> Propriétés de l’application
  - CloudPrintResourceId = « URI ID d’application » de l’application Web / API pour le point de terminaison impression inscrits dans l’étape 2.8. Vous pouvez le trouver sous les paramètres -> Propriétés de l’application
  - DiscoveryMaxPrinterLimit = \<un entier positif\>

>   Remarque: Si vous utilisez le service Microsoft Intune, vous pouvez rechercher ces paramètres sous la catégorie « Cloud Printer ».

|Nom d’affichage d’Intune                     |Stratégie                         |
|----------------------------------------|-------------------------------|
|URL de découverte d’imprimantes                   |CloudPrinterDiscoveryEndpoint  |
|URL d’autorisation accès imprimante            |CloudPrintOAuthAuthority       |
|GUID de l’application cliente native Azure            |CloudPrintOAuthClientId        |
|URI de ressource de service d’impression              |CloudPrintResourceId           |
|Nombre maximal d’imprimantes à interroger (Mobile uniquement)  |DiscoveryMaxPrinterLimit       |
|URI de ressource du service imprimante découverte  |MopriaDiscoveryResourceId      |


>   Remarque: Si le groupe de stratégie d’impression de Cloud n’est pas disponible, mais le fournisseur de gestion des appareils mobiles prend en charge les paramètres OMA-URI, vous pouvez définir les mêmes stratégies.  Reportez-vous à ce <a href="https://docs.microsoft.com/windows/client-management/mdm/policy-csp-enterprisecloudprint#enterprisecloudprint-cloudprintoauthauthority">article</a> pour des informations supplémentaires.

- OMA-URI
    - `CloudPrintOAuthAuthority = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthAuthority`
        - Valeur = `https://login.microsoftonline.com/` \<ID de l’annuaire Azure AD\>
    - `CloudPrintOAuthClientId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthClientId`
        - Valeur = \<ID d’Application Azure AD Native de l’application\>
    - `CloudPrinterDiscoveryEndPoint = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrinterDiscoveryEndPoint`
        - Valeur = URL externe du Mopria découverte du Service Azure Proxy d’Application créé dans l’étape 3.3 (doit être exactement le même mais sans la fin /)
    - `MopriaDiscoveryResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/MopriaDiscoveryResourceId`
        - Valeur = « URI ID d’application » de l’application Web / API pour le point de terminaison de découverte inscrits dans l’étape 2.8.  Vous pouvez le trouver sous les paramètres -> Propriétés de l’application
    - `CloudPrintResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintResourceId`
        - Valeur = « URI ID d’application » de l’application Web / API pour le point de terminaison impression inscrits dans l’étape 2.8. Vous pouvez le trouver sous les paramètres -> Propriétés de l’application
    - `DiscoveryMaxPrinterLimit = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/DiscoveryMaxPrinterLimit`
        - Valeur = \<un entier positif\>
  
### <a name="step-5---publish-desired-shared-printers"></a>Étape 5 : publier des imprimantes partagées souhaités
1. Installer l’imprimante souhaitée sur le serveur d’impression
2. Partager l’imprimante via l’interface utilisateur de propriétés imprimante
3. Sélectionnez l’ensemble souhaité d’utilisateurs pour accorder l’accès
4. Enregistrer les modifications et fermer la fenêtre de propriétés d’imprimante
5. À partir d’un ordinateur Windows 10 Fall créateur mise à jour, ouvrez une invite de commandes Windows PowerShell avec élévation de privilèges
   1. Exécutez les commandes suivantes
      - `find-module -Name "PublishCloudPrinter"` pour confirmer que l’ordinateur peut atteindre la galerie PowerShell (PSGallery)
      - `install-module -Name "PublishCloudPrinter"`

        >   REMARQUE : Vous pouvez voir une messagerie indiquant que « PSGallery » est un référentiel non approuvé.  Entrez « o » pour poursuivre l’installation.

      - Publish-CloudPrinter
        - Imprimante = nom de l’imprimante partagée qui a été défini
        - Fabricant = fabricant de l’imprimante
        - Modèle = modèle d’imprimante
        - OrgLocation = string A JSON en spécifiant l’emplacement de l’imprimante, par exemple :   `{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"Microsoft", "depth":1}, {"category":"site", "vs":"Redmond, WA", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}`
        - SDDL = chaîne SDDL qui représente les autorisations sur l’imprimante. Cela peut être obtenu en modifiant l’onglet sécurité des propriétés imprimante appropriée, puis en exécutant la commande suivante dans une invite de commandes : `(Get-Printer PrinterName -full).PermissionSDDL`
            Par exemple "G:DUD:(A;OICI;FA;;;WD)"

          > REMARQUE : Vous devez ajouter **`O:BA`** comme préfixe au résultat de la commande de l’invite de commandes ci-dessus avant de définir la valeur en tant que le paramètre SDDL.  Exemple : SDDL = `O:BAG:DUD:(A;OICI;FA;;;WD)`

        - DiscoveryEndpoint = https://&lt;services-machine-endpoint&gt;/mcs
        - PrintServerEndpoint = https://&lt;services-machine-endpoint&gt;/ecp
        - AzureClientId = ID d’Application de la valeur d’application Web Native inscrite ci-dessus
        - AzureTenantGuid = ID de répertoire de votre client Azure AD
        - DiscoveryResourceId = [facultatif] ID d’Application de Service de Cloud de découverte Mopria proxy

        > REMARQUE : Vous pouvez entrer toutes les valeurs de paramètre obligatoire dans la ligne de commande également.<br>
        **CloudPrinter publier** syntaxe de commande PowerShell : <br>
        CloudPrinter publier-imprimante \<chaîne\> -fabricant \<chaîne\> -modèle \<chaîne\> - OrgLocation \<chaîne\> - Sddl \<chaîne\> - DiscoveryEndpoint \<chaîne\> - PrintServerEndpoint \<chaîne\> - AzureClientId \<chaîne\> - AzureTenantGuid \<chaîne\> [-DiscoveryResourceId \<chaîne\>] <br>
        Exemple de commande : `publish-cloudprinter -Printer EcpPrintTest -Manufacturer Microsoft -Model FilePrinterEcp -OrgLocation '{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"MyCompany", "depth":1}, {"category":"site", "vs":"MyCity, State", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}' -Sddl "O:BAG:DUD:(A;OICI;FA;;;WD)" -DiscoveryEndpoint https://<services-machine-endpoint>/mcs -PrintServerEndpoint https://<services-machine-endpoint>/ecp -AzureClientId <Native Web App ID> -AzureTenantGuid <Azure AD Directory ID> -DiscoveryResourceId <Proxied Mopria Discovery Cloud Service App ID>`


## <a name="verifing-the-deployment"></a>Vérification du déploiement
Sur un appareil rattaché à Azure AD qui a les stratégies de gestion des appareils mobiles configurées :
- Ouvrez un navigateur web et accédez à https://&lt;-machine-point de terminaison services &gt; /mcs/services
- Vous devez voir le texte JSON décrivant le jeu de fonctionnalités de ce point de terminaison
- Accédez à « paramètres de système d’exploitation -\> appareils -\> imprimantes et scanneurs »
    - Vous devez voir un lien « Rechercher des imprimantes cloud »
    - Cliquez sur le lien
    - Cliquez sur le lien « Veuillez sélectionner un emplacement de recherche »
        - Vous devez voir la hiérarchie d’emplacement de périphérique
    - Choisissez un emplacement et cliquez sur **OK** puis cliquez sur **recherche** bouton pour rechercher les imprimantes
    - Sélectionnez l’imprimante et cliquez sur **ajouter appareil** bouton
    - Après l’installation de l’imprimante réussie, impression sur l’imprimante à partir de votre application favorite

>   Remarque: Si vous utilisez l’imprimante « EcpPrintTest », vous trouverez le fichier de sortie dans l’ordinateur serveur d’impression sous « C:\\ECPTestOutput\\EcpTestPrint.xps » emplacement.