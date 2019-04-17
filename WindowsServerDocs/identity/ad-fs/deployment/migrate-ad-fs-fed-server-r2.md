---
title: "Migrer le serveur AD FS 2.0 de fédération"
description: "Fournit des informations sur la migration d’un serveur AD FS vers Windows Server 2012 R2."
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: bef24f79cfa92dfeca1846501f14ebf6d8231f0d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>Migrer le serveur AD FS 2.0 de fédération à AD FS dans Windows Server 2012 R2

Pour migrer un serveur de fédération AD FS qui appartient à une batterie AD FS à nœud unique, une batterie WIF ou une batterie de serveurs SQL Server vers Windows Server 2012 R2, vous devez effectuer les tâches suivantes:  
  
1.  [Exporter et sauvegarder les données de configuration AD FS](migrate-ad-fs-fed-server-r2.md#export-and-backup-the-ad-fs-configuration-data)  
  
2.  [Créer une batterie de serveurs de fédération Windows Server 2012 R2](migrate-ad-fs-fed-server-r2.md#create-a-windows-server-2012-r2-federation-server-farm)  
  
3.  [Importer les données de configuration d’origine dans la batterie de serveurs AD FS de Windows Server 2012 R2](migrate-ad-fs-fed-server-r2.md#import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm)  
  
##  <a name="export-and-backup-the-ad-fs-configuration-data"></a>Exporter et sauvegarder les données de configuration AD FS  
 Pour exporter les paramètres de configuration AD FS, effectuez les procédures suivantes:  
  
###  <a name="to-export-service-settings"></a>Pour exporter les paramètres de service  
  
1.  Assurez-vous d’avoir accès aux certificats suivants et leurs clés privées dans un fichier .pfx:  
  
    -   Le certificat SSL utilisé par la batterie de serveurs de fédération que vous souhaitez migrer  
  
    -   Le certificat de communication du service (s’il est différent du certificat SSL) qui est utilisé par la batterie de serveurs de fédération que vous souhaitez migrer  
  
    -   Tous les tiers chiffrement/déchiffrement de jeton ou de signature de jetons des certificats qui sont utilisés par la batterie de serveurs de fédération que vous souhaitez migrer  
  
Pour trouver le certificat SSL, ouvrez la gestion Internet Information Services (IIS) de la console, sélectionnez **Site Web par défaut** dans le volet gauche, cliquez sur **liaisons...** dans le **Action** volet, rechercher et sélectionner la liaison https, cliquez sur **modifier**, puis cliquez sur **affichage**.  
  
Vous devez exporter le certificat SSL utilisé par le service de fédération et sa clé privée dans un fichier .pfx. Pour plus d’informations, voir [exporter la partie clé privée d’un certificat d’authentification serveur](export-the-private-key-portion-of-a-server-authentication-certificate.md).  
  
> [!NOTE]
>  Si vous prévoyez de déployer le Service d’inscription de périphérique dans le cadre de l’exécution d’ADFS dans Windows Server2012R2, vous devez obtenir un nouveau certificat SSL. Pour plus d’informations, voir [inscrire un certificat SSL pour ADFS](enroll-an-ssl-certificate-for-ad-fs.md) et [configurer un serveur de fédération avec Device Registration Service](configure-a-federation-server-with-device-registration-service.md).  
  
Pour afficher la signature de jetons, déchiffrement de jeton et les certificats de communication du service qui sont utilisés, exécutez la commande Windows PowerShell suivante pour créer une liste de tous les certificats en cours d’utilisation dans un fichier:  
  
``` powershell
Get-ADFSCertificate | Out-File “.\certificates.txt”  
 ```  
  
2.  Exporter les propriétés du service de fédération AD FS, telles que le nom service de fédération, le nom complet du service FS et l’identificateur du serveur de fédération vers un fichier.  
  
Pour exporter les propriétés du service de fédération, ouvrez Windows PowerShell et exécutez la commande suivante: 

``` powershell
Get-ADFSProperties | Out-File “.\properties.txt”`.  
``` 

Le fichier de sortie contient les valeurs de configuration importantes suivantes:  
 
|**Nom de propriété du Service de fédération signalé par Get-ADFSProperties**|**Nom de propriété du Service de fédération dans la console de gestion AD FS**|
|-----|-----|  
|Nom d’hôte|Nom de Service de fédération|  
|Identificateur|Identificateur du Service de fédération|  
|DisplayName|Nom complet du Service FS|  
  
3.  Sauvegardez le fichier de configuration d’application. Parmi les autres paramètres, ce fichier contient la chaîne de connexion de base de données de stratégie.  
  
Pour sauvegarder le fichier de configuration d’application, vous devez copier manuellement le `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` fichier à un emplacement sécurisé sur un serveur de sauvegarde.  
  
> [!NOTE]
>  Prenez note de la chaîne de connexion de base de données dans ce fichier, située immédiatement après «policystore connectionstring =». Si la chaîne de connexion spécifie une base de données SQL Server, la valeur est nécessaire lors de la restauration de la configuration AD FS d’origine sur le serveur de fédération.  
>   
>  Voici un exemple de chaîne de connexion WID: `“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`. Voici un exemple de chaîne de connexion SQL Server: `"Data Source=databasehostname;Integrated Security=True"`.  
  
4.  Enregistrez l’identité du compte de service de fédération AD FS et le mot de passe de ce compte.  
  
Pour rechercher la valeur d’identité, examinez le **journal en tant que** colonne de **AD FS 2.0 Windows Service** dans les **Services** de la console et enregistrez manuellement cette valeur.  
  
> [!NOTE]
>  Pour un service de fédération autonome, le compte de SERVICE réseau intégré est utilisé.  Dans ce cas, vous n’avez pas besoin d’avoir un mot de passe.  
  
5.  Exporter la liste des points de terminaison AD FS activés dans un fichier.  
  
Pour ce faire, ouvrez Windows PowerShell et exécutez la commande suivante: 

``` powershell
Get-ADFSEndpoint | Out-File “.\endpoints.txt”`.  
``` 

6.  Exporter toutes les descriptions des revendications personnalisées dans un fichier.  
  
Pour ce faire, ouvrez Windows PowerShell et exécutez la commande suivante: 

``` powershell
Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`.  
 ```

7.  Si vous avez des paramètres personnalisés comme useRelayStateForIdpInitiatedSignOn configurés dans le fichier web.config, veillez à sauvegarder le fichier web.config pour référence. Vous pouvez copier le fichier à partir du répertoire qui est mappé sur le chemin d’accès virtuel **«/ adfs/ls»** dans IIS. Par défaut, il est dans le **%systemdrive%\inetpub\adfs\ls** active.  
  
###  <a name="to-export-claims-provider-trusts-and-relying-party-trusts"></a>Pour exporter les approbations de fournisseur de revendications et approbations de partie de confiance  
  
1.  Pour exporter les services AD FS approbations de fournisseur de revendications et approbations de partie de confiance, vous devez vous connecter en tant qu’administrateur (Toutefois, pas en tant qu’administrateur de domaine) sur une fédération de votre serveur et exécuter Windows PowerShell suivant script qui est situé dans le **server_support/support/adfs** dossier du CD d’installation Windows Server 2012 R2: `export-federationconfiguration.ps1`.  
  
> [!IMPORTANT]
>  Le script d’exportation accepte les paramètres suivants:  
>   
>  -   Export-FederationConfiguration.ps1-chemin d’accès < string\ > [-ComputerName < string\ >] [-Credential < pscredential\ >] [-Force] [-CertificatePassword < securestring\ >]  
> -   Export-FederationConfiguration.ps1-chemin d’accès < string\ > [-ComputerName < string\ >] [-Credential < pscredential\ >] [-Force] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustIdentifier < chaîne [] >] [-ClaimsProviderTrustIdentifier < chaîne [] >]  
> -   Export-FederationConfiguration.ps1-chemin d’accès < string\ > [-ComputerName < string\ >] [-Credential < pscredential\ >] [-Force] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustName < chaîne [] >] [-ClaimsProviderTrustName < chaîne [] >]  
>   
>  **-RelyingPartyTrustIdentifier < chaîne [] >** : exporte les applets de commande uniquement dont les identificateurs sont spécifiés dans le tableau de chaînes approbations de partie de confiance. La valeur par défaut est exportée aucune des approbations de partie de partie de confiance. Si aucun des éléments RelyingPartyTrustIdentifier, ClaimsProviderTrustIdentifier, RelyingPartyTrustName et claimsprovidertrustname n’est spécifié, le script exporte toutes les approbations de partie de confiance et approbations de fournisseur de revendications.  
>   
>  **-ClaimsProviderTrustIdentifier < chaîne [] >** -l’applet de commande exporte uniquement les approbations de fournisseur de revendications dont les identificateurs sont spécifiés dans le tableau de chaînes. La valeur par défaut est exportée aucune des approbations de fournisseur de revendications.  
>   
>  **-RelyingPartyTrustName < chaîne [] >** : exporte les applets de commande uniquement dont les noms sont spécifiés dans le tableau de chaînes approbations de partie de confiance. La valeur par défaut est exportée aucune des approbations de partie de partie de confiance.  
>   
>  **-ClaimsProviderTrustName < chaîne [] >** -l’applet de commande exporte uniquement les approbations de fournisseur de revendications dont les noms sont spécifiés dans le tableau de chaînes. La valeur par défaut est exportée aucune des approbations de fournisseur de revendications.  
>   
>  **-Path < string\ >** -le chemin d’accès à un dossier qui contiendra les fichiers exportés.  
>   
>  **-ComputerName < string\ >** -Spécifie le nom d’hôte serveur STS. La valeur par défaut est l’ordinateur local. Si vous migrez AD FS 2.0 ou AD FS dans Windows Server 2012 vers AD FS dans Windows Server 2012 R2, il est le nom d’hôte du serveur AD FS hérité.  
>   
>  **-Credential < PSCredential\ >** -spécifie un compte d’utilisateur qui a l’autorisation d’effectuer cette action. La valeur par défaut est l’utilisateur actuel.  
>   
>  **-Force** : Spécifie de ne pas demander confirmation de l’utilisateur.  
>   
>  **-CertificatePassword < SecureString\ >** -spécifie un mot de passe pour l’exportation des clés privées des certificats AD FS. Si non spécifié, le script demande un mot de passe si un certificat AD FS avec une clé privée doit être exporté.  
>   
>  **Entrées**: aucune  
>   
>  **Sorties**: chaîne - cette applet de commande retourne le chemin d’accès du dossier exportation. Vous pouvez diriger l’objet retourné vers Import-FederationConfiguration.  
  
###  <a name="to-back-up-custom-attribute-stores"></a>Pour sauvegarder les magasins d’attributs personnalisés  
  
1.  Vous devez exporter manuellement tous les magasins d’attributs personnalisés que vous souhaitez conserver dans votre nouvelle batterie AD FS dans Windows Server 2012 R2.  
  
> [!NOTE]
>  Dans Windows Server 2012 R2, AD FS requiert des magasins d’attributs personnalisés qui sont basés sur .NET Framework 4.0 ou ci-dessus. Suivez les instructions de [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653) pour installer et configurer .net Framework 4.5.  
  
Vous trouverez des informations sur les magasins d’attributs personnalisés utilisés par AD FS en exécutant la commande Windows PowerShell suivante: 

``` powershell
Get-ADFSAttributeStore
```  

Les étapes pour mettre à niveau ou la migration des magasins d’attributs personnalisés varient.  
  
2.  Vous devez également exporter manuellement tous les fichiers .dll des magasins d’attributs personnalisés que vous souhaitez conserver dans votre nouvelle batterie AD FS dans Windows Server 2012 R2. Les étapes pour mettre à niveau ou la migration des fichiers .dll des magasins d’attributs personnalisés varient.  
  
##  <a name="create-a-windows-server-2012-r2-federation-server-farm"></a>Créer une batterie de serveurs de fédération Windows Server 2012 R2  
  
1.  Installer le système d’exploitation Windows Server 2012 R2 sur un ordinateur que vous souhaitez jouer à un serveur de fédération, puis ajoutez le rôle serveur AD FS. Pour plus d’informations, voir [installer le Service de rôle AD FS](install-the-ad-fs-role-service.md). Configurez ensuite votre nouveau service de fédération par le biais de l’Assistant Configuration du Service de fédération Active Directory ou via Windows PowerShell. Pour plus d’informations, voir «Configurer le premier serveur de fédération dans une batterie de serveurs de fédération» dans [configurer un serveur de fédération](configure-a-federation-server.md).  

Lors de cette étape, vous devez suivre ces instructions:  
  
-   Vous devez disposer des privilèges d’administrateur de domaine pour configurer votre service de fédération.  
  
-   Vous devez utiliser le même nom de service de fédération (nom de la batterie) que celui utilisé dans le services AD FS 2.0 ou AD FS dans Windows Server 2012. Si vous n’utilisez pas le même nom de service de fédération, les certificats que vous avez sauvegardés ne fonctionneront pas dans le service de fédération Windows Server 2012 R2 que vous essayez de configurer.  
  
-   Spécifier s’il s’agit d’une batterie de serveurs de fédération WID ou SQL Server. S’il est une batterie de serveurs SQL, spécifiez l’emplacement de base de données SQL Server et le nom de l’instance.  
  
-   Vous devez fournir un fichier pfx contenant le certificat d’authentification serveur SSL que vous avez sauvegardés dans le cadre de la préparation du processus de migration AD FS.  
  
-   Vous devez spécifier la même identité de compte de service qui a été utilisée dans AD FS 2.0 ou AD FS dans la batterie de serveurs Windows Server 2012.  
  
2.  Une fois que le nœud initial est configuré, vous pouvez ajouter des nœuds supplémentaires à votre nouvelle batterie. Pour plus d’informations, voir «Ajouter un serveur de fédération à une batterie de serveurs de fédération existante» dans [configurer un serveur de fédération](configure-a-federation-server.md).  
  
##  <a name="import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm"></a>Importer les données de configuration d’origine dans la batterie de serveurs AD FS de Windows Server 2012 R2  
 Maintenant que vous avez une batterie de serveurs de fédération AD FS en cours d’exécution dans Windows Server 2012 R2, vous pouvez importer les données de configuration AD FS d’origine dans celui-ci.  
  
1.  Importer et configurer d’autres certificats AD FS personnalisés, y compris inscrits en externe de certificats de signature de jetons et de chiffrement/déchiffrement de jeton et le certificat de communication du service s’il est différent du certificat SSL.  
  
Dans la console de gestion AD FS, sélectionnez **certificats**. Vérifier les certificats de chiffrement/déchiffrement de jeton et de signature de jetons de communications, service, par rapport aux valeurs que vous avez exportées dans le fichier certificates.txt pendant la préparation de la migration.  
  
Pour modifier les certificats de signature de jetons ou de déchiffrement de jeton à partir des valeur par défaut des certificats auto-signés pour des certificats externes, vous devez d’abord désactiver la fonctionnalité de substitution automatique de certificat qui est activée par défaut. Pour ce faire, vous pouvez utiliser la commande Windows PowerShell suivante:  
  
``` powershell 
Set-ADFSProperties –AutoCertificateRollover $false  
```  
  
2.  Configurer des paramètres de service AD FS personnalisés tels que de la durée de vie AutoCertificateRollover ou l’authentification unique à l’aide de l’applet de commande Set-AdfsProperties.  
  
3.  Pour importer les approbations de fournisseur de revendications et approbations de partie de confiance AD FS, vous devez être connecté en tant qu’administrateur (Toutefois, pas en tant qu’administrateur de domaine) sur une fédération de votre serveur et exécuter Windows PowerShell suivant script qui est situé dans le dossier \support\adfs du CD d’installation Windows Server 2012 R2:  
  
``` powershell 
import-federationconfiguration.ps1  
```  
  
> [!IMPORTANT]
>  Le script d’importation accepte les paramètres suivants:  
>   
>  -  Import-FederationConfiguration.ps1-chemin d’accès < string\ > [-ComputerName < string\ >] [-Credential < pscredential\ >] [-Force] [-LogPath < string\ >] [-CertificatePassword < securestring\ >]  
> -   Import-FederationConfiguration.ps1-chemin d’accès < string\ > [-ComputerName < string\ >] [-Credential < pscredential\ >] [-Force] [-LogPath < string\ >] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustIdentifier < chaîne [] >] [-ClaimsProviderTrustIdentifier < chaîne [] >  
> -   Import-FederationConfiguration.ps1-chemin d’accès < string\ > [-ComputerName < string\ >] [-Credential < pscredential\ >] [-Force] [-LogPath < string\ >] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustName < chaîne [] >] [-ClaimsProviderTrustName < chaîne [] >]  
>   
>  **-RelyingPartyTrustIdentifier < chaîne [] >** : importe uniquement les l’applet de commande dont les identificateurs sont spécifiés dans le tableau de chaînes approbations de partie de confiance. La valeur par défaut consiste à importer aucun des approbations de partie de confiance. Si aucun des éléments RelyingPartyTrustIdentifier, ClaimsProviderTrustIdentifier, RelyingPartyTrustName et claimsprovidertrustname n’est spécifié, le script importe toutes les approbations de partie de confiance et approbations de fournisseur de revendications.  
>   
>  **-ClaimsProviderTrustIdentifier < chaîne [] >** -l’applet de commande importe uniquement les approbations de fournisseur de revendications dont les identificateurs sont spécifiés dans le tableau de chaînes. La valeur par défaut consiste à importer aucune des approbations de fournisseur de revendications.  
>   
>  **-RelyingPartyTrustName < chaîne [] >** : importe uniquement les l’applet de commande dont les noms sont spécifiés dans le tableau de chaînes approbations de partie de confiance. La valeur par défaut consiste à importer aucun des approbations de partie de confiance.  
>   
>  **-ClaimsProviderTrustName < chaîne [] >** -l’applet de commande importe uniquement les approbations de fournisseur de revendications dont les noms sont spécifiés dans le tableau de chaînes. La valeur par défaut consiste à importer aucune des approbations de fournisseur de revendications.  
>   
>  **-Path < string\ >** -le chemin d’accès à un dossier qui contient les fichiers de configuration à importer.  
>   
>  **-LogPath < string\ >** -le chemin d’accès à un dossier qui contiendra le fichier journal d’importation. Un fichier journal nommé «import.log» sera créé dans ce dossier.  
>   
>  **-ComputerName < string\ >** -Spécifie le nom d’hôte du serveur STS. La valeur par défaut est l’ordinateur local. Si vous migrez AD FS 2.0 ou AD FS dans Windows Server 2012 vers AD FS dans Windows Server 2012 R2, ce paramètre doit être défini pour le nom d’hôte du serveur AD FS hérité.  
>   
>  **-Credential < PSCredential\ >**-spécifie un compte d’utilisateur qui a l’autorisation d’effectuer cette action. La valeur par défaut est l’utilisateur actuel.  
>   
>  **-Force** : Spécifie de ne pas demander confirmation de l’utilisateur.  
>   
>  **-CertificatePassword < SecureString\ >** -spécifie un mot de passe pour l’importation des clés privées des certificats AD FS. Si non spécifié, le script demande un mot de passe si un certificat AD FS avec une clé privée doit être importé.  
>   
>  **Entrées:** chaîne - cette commande prend le chemin d’accès du dossier importation comme entrée. Vous pouvez diriger Export-FederationConfiguration vers cette commande.  
>   
>  **Sorties:** None.  
  
Les espaces de fin dans la propriété WSFedEndpoint d’une relation de confiance peuvent provoquer le script d’importation. Dans ce cas, supprimez les espaces du fichier avant d’importer. Par exemple, les entrées suivantes provoquent des erreurs:  
  
    ```  
    <URI N="WSFedEndpoint">https://127.0.0.1:444 /</URI>  
    ```  
  
    ```  
    <URI N="WSFedEndpoint">https://myapp.cloudapp.net:83 /</URI>  
    ```  
  
     They must be edited to:  
  
    ```  
    <URI N="WSFedEndpoint">https://127.0.0.1:444/</URI>  
    ```  
  
    ```  
    <URI N="WSFedEndpoint">https://myapp.cloudapp.net:83/</URI>  
    ```  
> [!IMPORTANT]
>  Si vous avez des règles de revendication personnalisées (règles d’autres que les règles par défaut AD FS) sur l’approbation de fournisseur de revendications Active Directory dans le système source, ces ne seront pas migrés par les scripts. Il s’agit dans la mesure où Windows Server 2012 R2 a de nouvelles valeurs par défaut. Toutes les règles personnalisées doivent être fusionnées en les ajoutant manuellement à l’approbation de fournisseur de revendications Active Directory dans la nouvelle batterie de serveurs Windows Server 2012 R2.  
  
4.  Configurez tous les paramètres de point de terminaison AD FS personnalisés. Dans la console de gestion AD FS, sélectionnez **les points de terminaison**. Vérifiez les points de terminaison AD FS activés par rapport à la liste des points de terminaison AD FS activés que vous avez exporté dans un fichier pendant la préparation de la migration AD FS.  
  
     \ - Et -  
  
     Configurez toutes les descriptions des revendications personnalisées. Dans la console de gestion AD FS, sélectionnez **descriptions des revendications**. Vérifiez la liste des descriptions de revendication d’AD FS par rapport à la liste des descriptions de revendication que vous avez exporté dans un fichier pendant la préparation de la migration AD FS. Ajoutez toutes les descriptions des revendications personnalisées incluses dans votre fichier, mais ne pas inclus dans la liste par défaut dans AD FS. Notez que l’identificateur de revendication dans la console de gestion est mappé au type de revendication dans le fichier.  
  
5.  Installer et configurer personnalisés sauvegardés tous les magasins d’attributs. En tant qu’administrateur, vérifiez que les fichiers binaires des magasins attributs personnalisés sont mise à niveau vers .NET Framework 4.0 ou version ultérieure avant la mise à jour de la configuration AD FS pour pointer vers eux.  
  
6.  Configurer les propriétés du service qui correspondent aux paramètres de fichier web.config hérités.  
  
    -   Si **useRelayStateForIdpInitiatedSignOn** a été ajouté à la **web.config** de fichiers dans AD FS 2.0 ou AD FS dans la batterie de serveurs Windows Server 2012, vous devez configurer les propriétés du service suivantes dans AD FS dans la batterie de serveurs Windows Server 2012 R2:  
  
        -   Services AD FS dans Windows Server 2012 R2 incluent un **%systemroot%\ADFS\Microsoft.IdentityServer.Servicehost.exe.config** fichier. Créez un élément avec la même syntaxe que la **web.config** élément de fichier: `<useRelayStateForIdpInitiatedSignOn enabled="true" />`. Ajoutez cet élément dans le cadre de **< microsoft.identityserver.web >** section de la **Microsoft.IdentityServer.Servicehost.exe.config** fichier.  
  
    -   Si **< persistIdentityProviderInformation activée = «true & #124; false» lifetimeInDays = enablewhrPersistence «90» = «true & #124; false» / \ >** a été ajouté à la **web.config** de fichiers dans AD FS 2.0 ou AD FS dans la batterie de serveurs Windows Server 2012, vous devez configurer les propriétés du service suivantes dans AD FS dans la batterie de serveurs Windows Server 2012 R2:  
  
        1.  Dans AD FS dans Windows Server 2012 R2, exécutez la commande Windows PowerShell suivante: `Set-AdfsWebConfig –HRDCookieEnabled –HRDCookieLifetime`.  
  
    -   Si **< singleSignOn activé = «true & #124; false» / \ >** a été ajouté à la **web.config** fichier dans AD FS 2.0 ou AD FS dans la batterie Windows Server 2012, vous n’avez pas besoin de définir toutes les autres propriétés du service dans AD FS dans la batterie de serveurs Windows Server 2012 R2. L’ouverture de session unique est activée par défaut dans AD FS dans la batterie de serveurs Windows Server 2012 R2.  
  
    -   Si les paramètres localAuthenticationTypes ont été ajoutées à la **web.config** de fichiers dans AD FS 2.0 ou AD FS dans la batterie de serveurs Windows Server 2012, vous devez configurer les propriétés du service suivantes dans AD FS dans la batterie de serveurs Windows Server 2012 R2:  
  
        -   Intégré, les formulaires, le client TLS et liste de transformation de base dans AD FS équivalents dans Windows Server 2012 R2 a les paramètres de stratégie d’authentification globaux pour prendre en charge les deux types d’authentification proxy et le service de fédération. Ces paramètres peuvent être configurés dans les services AD FS dans le composant logiciel enfichable Gestion sous le **stratégies d’authentification**.  
  
 Après avoir importé les données de configuration d’origine, vous pouvez personnaliser les pages de connexion AD FS en fonction des besoins. Pour plus d’informations, voir [personnalisation des Pages AD FS Sign-in](../operations/AD-FS-Customization-in-Windows-Server-2016.md).  
  
## <a name="next-steps"></a>Étapes suivantes
 [Migrer le rôle Services de fédération Active Directory pour Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Préparation de la migration du serveur de fédération AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migration du serveur Proxy de fédération AD FS](migrate-fed-server-proxy-r2.md)   
 [Vérification de la Migration AD FS Windows Server 2012 R2](verify-ad-fs-migration.md)