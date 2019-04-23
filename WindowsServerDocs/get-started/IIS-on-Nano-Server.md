---
title: IIS sur Nano Server
description: Détails de la configuration d’IIS sur Nano Server
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/06/2017
ms.assetid: 16984724-2d77-4d7b-9738-3dff375ed68c
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 1461f3e3266d77d2510aba37208347253a8f78e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851620"
---
# <a name="iis-on-nano-server"></a>IIS sur Nano Server

>S'applique à : Windows Server 2016

> [!IMPORTANT]
> À compter de Windows Server, version 1709, Nano Server sera uniquement disponible sous forme d’[image du système d’exploitation de base du conteneur](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Consultez [Modifications apportées à Nano Server](nano-in-semi-annual-channel.md) pour en savoir plus. 

Vous pouvez installer le rôle de serveur Internet Information Services (IIS) sur Nano Server, en utilisant le paramètre -Package avec Microsoft-NanoServer-IIS-Package. Pour plus d’informations sur la configuration de Nano Server, notamment sur l’installation des packages, consultez [Installer Nano Server](Getting-Started-with-Nano-Server.md).  

Dans cette version de Nano Server, les fonctionnalités IIS suivantes sont disponibles :  

|Fonctionnalité|Activée par défaut|  
|-----------|----------------------|  
|**Fonctionnalités HTTP communes**||  
|Document par défaut|x|  
|Exploration de répertoires|x|  
|Erreurs HTTP|x|  
|Contenu statique|x|  
|Redirection HTTP||  
|**Intégrité et Diagnostics**||  
|Journalisation HTTP|x|  
|Journalisation personnalisée||  
|Observateur de demandes||  
|Suivi||  
|**Performances**||  
|Compression de contenu statique|x|  
|Compression de contenu dynamique||  
|**Sécurité**||  
|Filtrage des demandes|x|  
|Authentification de base||  
|Authentification de mappage de certificats clients||  
|Authentification Digest||  
|Authentification de mappage de certificats clients d’IIS||  
|Restrictions d’adresse IP et de domaine||  
|Autorisation d’URL||  
|Authentification Windows||  
|**Développement d’applications**||  
|Initialisation d’applications||  
|CGI||  
|Extensions ISAPI||  
|Filtres ISAPI||  
|Inclusions côté serveur||  
|Protocole WebSocket||  
|**Outils de gestion**||  
|Module IISAdministration pour Windows PowerShell|x|  

Une série d’articles sur d’autres configurations d’IIS (par exemple, à l’aide d’ASP.NET, PHP et Java), ainsi que d’autres contenus associés au [ http://iis.net/learn ](http://iis.net/learn).  

## <a name="installing-iis-on-nano-server"></a>Installation d’IIS sur Nano Server  
Vous pouvez installer ce rôle de serveur hors connexion (avec Nano Server désactivé) ou en ligne (avec Nano Server en cours d’exécution). L’option d’installation hors connexion est recommandée.  

Pour l’installation hors connexion, ajoutez le package avec le paramètre -Packages de New-NanoServerImage, comme dans cet exemple :  

`New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath f:\ -BasePath .\Base -TargetPath .\Nano1.vhd -ComputerName Nano1 -Package Microsoft-NanoServer-IIS-Package`  

Si vous avez un fichier VHD, vous pouvez installer IIS en mode hors connexion avec DISM.exe, en montant le disque dur virtuel puis en utilisant l’option **Add-Package**.   
L’exemple suivant suppose que vous lancez l’exécution à partir du répertoire spécifié par l’option BasePath, créé après l’exécution de New-NanoServerImage.  

1.  mkdir mountdir
2.  .\Tools\dism.exe /Mount-Image /ImageFile:.\NanoServer.vhd /Index:1 /MountDir:.\mountdir
3.  .\Tools\dism.exe /Add-Package /PackagePath:.\packages\Microsoft-NanoServer-IIS-Package.cab /Image:.\mountdir
4.  .\Tools\dism.exe /Add-Package /PackagePath:.\packages\en-us\Microsoft-NanoServer-IIS-Package_en-us.cab /Image:.\mountdir
5.  .\Tools\dism.exe /Unmount-Image /MountDir:.\MountDir /Commit
 

> [!NOTE]  
> Notez que l’étape 4 ajoute le module linguistique, en l’occurrence EN-US.  

À ce stade, vous pouvez démarrer Nano Server avec IIS.  

### <a name="installing-iis-on-nano-server-online"></a>Installation d’IIS sur Nano Server en ligne  
L’installation hors connexion du rôle de serveur est recommandée, mais vous devrez peut-être effectuer une installation en ligne (avec Nano Server en cours d’exécution) dans les scénarios avec conteneurs. Pour cela, procédez comme suit:  

1.  Copiez le dossier Packages du support d’installation localement sur le Nano Server en cours d’exécution (par exemple, sur C:\packages).  

2.  Créez un fichier Unattend.xml sur un autre ordinateur et copiez-le sur Nano Server. Vous pouvez copier et coller ce contenu XML dans le fichier XML que vous avez créé:  



```  
   
    <unattend xmlns="urn:schemas-microsoft-com:unattend">  
    <servicing>  
        <package action="install">  
            <assemblyIdentity name="Microsoft-NanoServer-IIS-Package" version="10.0.14393.0" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" />  
            <source location="c:\packages\Microsoft-NanoServer-IIS-Package.cab" />  
        </package>  
        <package action="install">  
            <assemblyIdentity name="Microsoft-NanoServer-IIS-Package" version="10.0.14393.0" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="en-US" />  
            <source location="c:\packages\en-us\Microsoft-NanoServer-IIS-Package_en-us.cab" />  
        </package>  
    </servicing>  
    <cpi:offlineImage cpi:source="" xmlns:cpi="urn:schemas-microsoft-com:cpi" />  
</unattend>  
```  




3.  Dans le nouveau fichier XML que vous avez créé (ou copié), remplacez C:\packages par le répertoire dans lequel vous avez copié le contenu de Packages.  

4.  Accédez au répertoire contenant le fichier XML créé et exécutez  

    **dism /online /apply-unattend:.\unattend.xml**  


5.  Vérifiez que le package IIS et son module linguistique sont correctement installés en exécutant :  

    **dism /online /get-packages**  

    Vous devez voir « identité du Package : Microsoft-NanoServer-IIS-Package ~ 31bf3856ad364e35 ~ amd64 ~ ~ 10.0.14393.1000" répertorié deux fois, une fois pour Type de version : Module linguistique et une fois pour le Type de version : Feature Pack.  

6.  Démarrez le service W3SVC avec **net start w3svc** ou en redémarrant Nano Server.  

## <a name="starting-iis"></a>Redémarrage d’IIS  
Une fois installé et en cours d’exécution, IIS est prêt à traiter les demandes web. Vérifiez qu’IIS s’exécute en consultant sa page web par défaut à l’adresse http://\<adresse IP de Nano Server>. Sur un ordinateur physique, vous pouvez connaître cette adresseIP à l’aide de la Console de récupération. Sur une machine virtuelle, vous pouvez obtenir cette adresse IP en ouvrant une invite Windows PowerShell et en exécutant :  

`Get-VM -name <VM name> | Select -ExpandProperty networkadapters | select IPAddresses`  

Si vous ne parvenez pas à accéder à la page web par défaut d’IIS, vérifiez à nouveau l’installation d’IIS en recherchant le répertoire **c:\inetpub** sur Nano Server.  

## <a name="enabling-and-disabling-iis-features"></a>Activation et désactivation des fonctionnalités d’IIS  
Plusieurs fonctionnalités d’IIS sont activées par défaut lorsque vous installez le rôle IIS (consultez le tableau dans la section «Présentation d’IIS sur Nano Server» de cette rubrique). Vous pouvez activer (ou désactiver) des fonctionnalités supplémentaires à l’aide de DISM.exe

Chaque fonctionnalité d’IIS se présente sous la forme d’éléments de configuration. Par exemple, la fonctionnalité d’authentification Windows se compose des éléments suivants:  

|Section|Éléments de configuration|  
|-----------|--------------------------|  
|`<globalModules>`|`<add name="WindowsAuthenticationModule" image="%windir%\System32\inetsrv\authsspi.dll`|  
|`<modules>`|`<add name="WindowsAuthenticationModule" lockItem="true" \/>`|  
|`<windowsAuthentication>`|`<windowsAuthentication enabled="false" authPersistNonNTLM\="true"><providers><add value="Negotiate" /><add value="NTLM" /><br /></providers><br /></windowsAuthentication>`|  

Toutes les sous-fonctionnalités d’IIS figurent dans l’annexe 1 de cette rubrique et leurs éléments de configuration correspondants figurent dans l’annexe 2 de cette rubrique.  
 

### <a name="example-installing-windows-authentication"></a>Exemple : installation de l’authentification Windows  

1.  Ouvrez une console de session distante Windows PowerShell sur le Nano Server.  

2.  Utilisez `DISM.exe` pour installer le module d’authentification Windows :

    ```
    dism /Enable-Feature /online /featurename:IIS-WindowsAuthentication /all
    ```

    Le commutateur `/all` installe une fonctionnalité qui dépend de la fonctionnalité choisie.

### <a name="example-uninstalling-windows-authentication"></a>Exemple: désinstallation de l’authentification Windows  

1.  Ouvrez une console de session distante Windows PowerShell sur le Nano Server.  

2.  Utilisez `DISM.exe` pour désinstaller le module d’authentification Windows :

    ```
    dism /Disable-Feature /online /featurename:IIS-WindowsAuthentication
    ```

## <a name="other-common-iis-configuration-tasks"></a>Autres tâches courantes de configuration d’IIS  
**Création de sites Web**  

Utilisez cette applet de commande:  

`PS D:\> New-IISSite -Name TestSite -BindingInformation "*:80:TestSite" -PhysicalPath c:\test`  

Vous pouvez ensuite exécuter `Get-IISSite` pour vérifier l’état du site (renvoie le nom, l’ID, l’état, le chemin d’accès physique et les liaisons du site web).  

**Suppression de sites web**  

Exécutez `Remove-IISSite -Name TestSite -Confirm:$false`.  

**Création de répertoires virtuels**  

Vous pouvez créer des répertoires virtuels à l’aide de l’objet IISServerManager renvoyé par Get-IISServerManager, qui affiche l’API Microsoft.Web.Administration.ServerManager.NET. Dans cet exemple, les commandes suivantes accèdent à l’élément « Site web par défaut » de la collection Sites et à l’élément d’application racine (« / ») de la section Applications. Ensuite, elles appellent la méthode Add() de la collection VirtualDirectories pour cet élément d’application afin de créer le répertoire:  

```  
PS C:\> $sm = Get-IISServerManager  
PS C:\> $sm.Sites["Default Web Site"].Applications["/"].VirtualDirectories.Add("/DemoVirtualDir1", "c:\test\virtualDirectory1")  
PS C:\> $sm.Sites["Default Web Site"].Applications["/"].VirtualDirectories.Add("/DemoVirtualDir2", "c:\test\virtualDirectory2")  
PS C:\> $sm.CommitChanges()  
```  

**Création de pools d’applications**  

De même, vous pouvez utiliser Get-IISServerManager pour créer des pools d’applications :  

```  
PS C:\> $sm = Get-IISServerManager  
PS C:\> $sm.ApplicationPools.Add("DemoAppPool")  
```  

**Configuration HTTPS et certificats**  

Utilisez l’utilitaire Certoc.exe pour importer des certificats, comme dans cet exemple qui présente la configuration HTTPS d’un site Web sur un Nano Server:  

1.  Sur un autre ordinateur qui n’exécute pas Nano Server, créez un certificat (à l’aide de votre propre nom et mot de passe de certificat), puis exportez-le vers c:\temp\test.pfx.  

    `$newCert = New-SelfSignedCertificate -DnsName "www.foo.bar.com" -CertStoreLocation cert:\LocalMachine\my`  

    `$mypwd = ConvertTo-SecureString -String "YOUR_PFX_PASSWD" -Force -AsPlainText`  

    `Export-PfxCertificate -FilePath c:\temp\test.pfx -Cert $newCert -Password $mypwd`  

2.  Copiez le fichier test.pfx sur l’ordinateur de Nano Server.  

3.  Sur le Nano Server, importez le certificat dans le magasin «My» avec cette commande:  

    **certoc.exe -ImportPFX -p YOUR_PFX_PASSWD My c:\temp\test.pfx**  

4.  Récupérez l’empreinte numérique de ce nouveau certificat (dans cet exemple, 61E71251294B2A7BB8259C2AC5CF7BA622777E73) avec `Get-ChildItem Cert:\LocalMachine\my`.  

5.  Ajoutez la liaison HTTPS vers le site web par défaut (ou au site dont vous souhaitez ajouter la liaison) à l’aide de ces commandes Windows PowerShell :  

    ```  
    $certificate = get-item Cert:\LocalMachine\my\61E71251294B2A7BB8259C2AC5CF7BA622777E73  
    # Use your actual thumbprint instead of this example  
    $hash = $certificate.GetCertHash()  

    Import-Module IISAdministration  
    $sm = Get-IISServerManager  
    $sm.Sites["Default Web Site"].Bindings.Add("*:443:", $hash, "My", "0")    # My is the certificate store name  
    $sm.CommitChanges()  
    ```  

    Vous pouvez également utiliser l’Indication de nom de serveur (SNI) avec un nom d’hôte spécifique avec la syntaxe suivante : `$sm.Sites["Default Web Site"].Bindings.Add("*:443:www.foo.bar.com", $hash, "My", "Sni".`  

## <a name="appendix-1-list-of-iis-sub-features"></a>Annexe 1 : Liste des sous-fonctionnalités d’IIS

- IIS-WebServer
- IIS-CommonHttpFeatures
- IIS-StaticContent
- IIS-DefaultDocument
- IIS-DirectoryBrowsing
- IIS-HttpErrors
- IIS-HttpRedirect
- IIS-ApplicationDevelopment
- IIS-CGI
- IIS-ISAPIExtensions
- IIS-ISAPIFilter
- IIS-ServerSideIncludes
- IIS-WebSockets
- IIS-ApplicationInit
- IIS-Security
- IIS-BasicAuthentication
- IIS-WindowsAuthentication
- IIS-DigestAuthentication
- IIS-ClientCertificateMappingAuthentication
- IIS-IISCertificateMappingAuthentication
- IIS-URLAuthorization
- IIS-RequestFiltering
- IIS-IPSecurity
- IIS-CertProvider
- IIS-Performance
- IIS-HttpCompressionStatic
- IIS-HttpCompressionDynamic
- IIS-HealthAndDiagnostics
- IIS-HttpLogging
- IIS-LoggingLibraries
- IIS-RequestMonitor
- IIS-HttpTracing
- IIS-CustomLogging

## <a name="appendix-2-elements-of-http-features"></a>Annexe 2 : Éléments des fonctionnalités HTTP  
Chaque fonctionnalité d’IIS se présente sous la forme d’éléments de configuration. Cette annexe répertorie les éléments de configuration de toutes les fonctionnalités de cette version de Nano Server.  

### <a name="common-http-features"></a>Fonctionnalités HTTP courantes  
**Document par défaut**  

|Section|Éléments de configuration|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="DefaultDocumentModule" image="%windir%\System32\inetsrv\defdoc.dll" />`|  
|`<modules>`|`<add name="DefaultDocumentModule" lockItem="true" />`|  
|`<handlers>`|`<add name="StaticFile" path="*" verb="*" modules="DefaultDocumentModule" resourceType="EiSecther" requireAccess="Read" />`|  
|`<defaultDocument>`|`<defaultDocument enabled="true"><br /><files><br /> <add value="Default.htm" /><br />        <add value="Default.asp" /><br />        <add value="index.htm" /><br />        <add value="index.html" /><br />        <add value="iisstart.htm" /><br />    </files><br /></defaultDocument>`|  

L’entrée `StaticFile <handlers>` peut-être déjà être présente. Dans ce cas, ajoutez simplement « DefaultDocumentModule » dans l’attribut \<modules>, séparé par une virgule.  

**exploration des répertoires**  

|Section|Éléments de configuration|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name="DirectoryListingModule" image="%windir%\System32\inetsrv\dirlist.dll" />`|  
|`<modules>`|`<add name="DirectoryListingModule" lockItem="true" />`|  
|`<handlers>`|`<add name="StaticFile" path="*" verb="*" modules="DirectoryListingModule" resourceType="Either" requireAccess="Read" />`|  

L’entrée `StaticFile <handlers>` peut-être déjà être présente. Dans ce cas, ajoutez simplement « DirectoryListingModule » dans l’attribut \<modules>, séparé par une virgule.  

**Erreurs HTTP**  

|Section|Éléments de configuration|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name="CustomErrorModule" image="%windir%\System32\inetsrv\custerr.dll" />`|  
|`<modules>`|`<add name="CustomErrorModule" lockItem="true" />`|  
|`<httpErrors>`|`<httpErrors lockAttributes="allowAbsolutePathsWhenDelegated,defaultPath"><br />    <error statusCode="401"    prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="401.htm" ><br />    <error statusCode="403" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="403.htm" /><br />    <error statusCode="404" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="404.htm" /><br />    <error statusCode="405" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="405.htm" /><br />    <error statusCode="406" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="406.htm" /><br />    <error statusCode="412" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="412.htm" /><br />    <error statusCode="500" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="500.htm" /><br />    <error statusCode="501" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="501.htm" /><br />    <error statusCode="502" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="502.htm" /><br /></httpErrors>`|  

**Contenu statique**  

|Section|Éléments de configuration|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="StaticFileModule" image="%windir%\System32\inetsrv\static.dll" />`|  
|`<modules>`|`<add name="StaticFileModule" lockItem="true" />`|  
|`<handlers>`|`<add name="StaticFile" path="*" verb="*" modules="StaticFileModule" resourceType="Either" requireAccess="Read" />`|  

L’entrée `StaticFile \<handlers>` peut-être déjà être présente. Dans ce cas, ajoutez simplement « StaticFileModule » dans l’attribut \<modules>, séparé par une virgule.  

**Redirection HTTP**  

|Section|Éléments de configuration|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name="HttpRedirectionModule" image="%windir%\System32\inetsrv\redirect.dll" />`|  
|`<modules>`|`<add name="HttpRedirectionModule" lockItem="true" />`|  
|`<httpRedirect>`|`<httpRedirect enabled="false" />`|  

### <a name="health-and-diagnostics"></a>Intégrité et diagnostic  
**Journalisation HTTP**  

|Section|Éléments de configuration|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name="HttpLoggingModule" image="%windir%\System32\inetsrv\loghttp.dll" />`|  
|`<modules>`|`<add name="HttpLoggingModule" lockItem="true" />`|  
|`<httpLogging>`|`<httpLogging dontLog="false" />`|  

**Journalisation personnalisée**  

|Section|Éléments de configuration|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="CustomLoggingModule" image="%windir%\System32\inetsrv\logcust.dll" />`|  
|`<modules>`|`<add name="CustomLoggingModule" lockItem="true" />`|  

**Observateur de demandes**  

|Section|Éléments de configuration|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="RequestMonitorModule" image="%windir%\System32\inetsrv\iisreqs.dll" />`|  

**Le suivi**  

|Section|Éléments de configuration|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="TracingModule" image="%windir%\System32\inetsrv\iisetw.dll" \/><br /><add name="FailedRequestsTracingModule" image="%windir%\System32\inetsrv\iisfreb.dll" />`|  
|`<modules>`|`<add name="FailedRequestsTracingModule" lockItem="true" />`|  
|`<traceProviderDefinitions>`|`<traceProviderDefinitions><br />    <add name="WWW Server" guid\="{3a2a4e84-4c21-4981-ae10-3fda0d9b0f83}"><br />        <areas><br />            <clear /><br />            <add name="Authentication" value="2" /><br />            <add name="Security" value="4" /><br />            <add name="Filter" value="8" /><br />            <add name="StaticFile" value="16" /><br />            <add name="CGI" value="32" /><br />            <add name="Compression" value="64" /><br />            <add name="Cache" value="128" /><br />            <add name="RequestNotifications" value="256" /><br />            <add name="Module" value="512" /><br />            <add name="FastCGI" value="4096" /><br />            <add name="WebSocket" value="16384" /><br />        </areas><br />    </add><br />    <add name="ISAPI Extension" guid="{a1c2040e-8840-4c31-ba11-9871031a19ea}"><br />        <areas><br />            <clear /><br />        </areas><br />    </add><br /></traceProviderDefinitions>`|  

### <a name="performance"></a>Performances  
**Compression du contenu statique**  

|Section|Éléments de configuration|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="StaticCompressionModule" image="%windir%\System32\inetsrv\compstat.dll" />`|  
|`<modules>`|`<add name="StaticCompressionModule" lockItem="true" />`|  
|`<httpCompression>`|`<httpCompression directory="%SystemDrive%\inetpub\temp\IIS Temporary Compressed Files"><br />    <scheme name="gzip" dll="%Windir%\system32\inetsrv\gzip.dll" /><br />   <staticTypes><br />        <add mimeType="text/*" enabled="true" /><br />        <add mimeType="message/*" enabled="true" /><br />        <add mimeType="application/javascript" enabled="true" \/><br />        <add mimeType="application/atom+xml" enabled="true" /><br />        <add mimeType="application/xaml+xml" enabled="true" /><br />        <add mimeType="\*\*" enabled="false" /><br />    </staticTypes><br /></httpCompression>`|  

**Compression de contenu dynamique**  

|Section|Éléments de configuration|  
|-----------|--------------------------|  
|`<globalModules>`|`<add name="DynamicCompressionModule" image="%windir%\System32\inetsrv\compdyn.dll" />`|  
|`<modules>`|`<add name="DynamicCompressionModule" lockItem="true" />`|  
|`<httpCompression>`|`<httpCompression directory\="%SystemDrive%\inetpub\temp\IIS Temporary Compressed Files"><br />    <scheme name="gzip" dll="%Windir%\system32\inetsrv\gzip.dll" \/><br />    \<dynamicTypes><br />        <add mimeType="text/*" enabled="true" \/><br />        <add mimeType="message/*" enabled="true" /><br />        <add mimeType="application/x-javascript" enabled="true" /><br />        <add mimeType="application/javascript" enabled="true" /><br />        <add mimeType="*/*" enabled="false" /><br />    <\/dynamicTypes><br /></httpCompression>`|  

### <a name="security"></a>Sécurité  
**Filtrage des demandes**  

|Section|Éléments de configuration|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="RequestFilteringModule" image="%windir%\System32\inetsrv\modrqflt.dll" />`|  
|`<modules>`|`<add name="RequestFilteringModule" lockItem="true" />`|  
|`<requestFiltering>|`<requestFiltering><br />    <fileExtensions allowUnlisted="true" applyToWebDAV="true" /><br />    <verbs allowUnlisted="true" applyToWebDAV="true" /><br />    <hiddenSegments applyToWebDAV="true"><br />        <add segment="web.config" /><br />    </hiddenSegments><br /></requestFiltering>`|  

**Authentification de base**  

|Section|Éléments de configuration|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name="BasicAuthenticationModule" image="%windir%\System32\inetsrv\authbas.dll" />`|  
|`<modules>`|`<add name="WindowsAuthenticationModule" lockItem="true" />`|  
|`<basicAuthentication>`|`<basicAuthentication enabled="false" />`|  

**Authentification par mappage de certificat client**  

|Section|Éléments de configuration|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="CertificateMappingAuthentication" image="%windir%\System32\inetsrv\authcert.dll" />`|  
|`<modules>`|`<add name="CertificateMappingAuthenticationModule" lockItem="true" />`|  
|`<clientCertificateMappingAuthentication>`|`<clientCertificateMappingAuthentication enabled="false" />`|  

**Authentification Digest**  

|Section|Éléments de configuration|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="DigestAuthenticationModule" image="%windir%\System32\inetsrv\authmd5.dll" />`|  
|`<modules>`|`<add name="DigestAuthenticationModule" lockItem="true" />`|  
|`<other>`|`<digestAuthentication enabled="false" />`|  

**Authentification par mappage de certificat client IIS**  

|Section|Éléments de configuration|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name="CertificateMappingAuthenticationModule" image="%windir%\System32\inetsrv\authcert.dll" />`|  
|`<modules>`|`<add name="CertificateMappingAuthenticationModule" lockItem="true" `/>`|  
|`<clientCertificateMappingAuthentication>`|`<clientCertificateMappingAuthentication enabled="false" />`|  

**Restrictions IP et de domaine**  

|Section|Éléments de configuration|  
|----------------|--------------------------|  
|`<globalModules>`|```<add name="IpRestrictionModule" image="%windir%\System32\inetsrv\iprestr.dll" /><br /><add name="DynamicIpRestrictionModule" image="%windir%\System32\inetsrv\diprestr.dll" />```|  
|`<modules>`|`<add name="IpRestrictionModule" lockItem="true" \/><br /><add name="DynamicIpRestrictionModule" lockItem="true" \/>`|  
|`<ipSecurity>`|`<ipSecurity allowUnlisted="true" />`|  

**Autorisation d’URL**  

|Section|Éléments de configuration|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="UrlAuthorizationModule" image="%windir%\System32\inetsrv\urlauthz.dll" />`|  
|`<modules>`|`<add name="UrlAuthorizationModule" lockItem="true" />`|  
|`<authorization>`|`<authorization><br />    <add accessType="Allow" users="*" /><br /></authorization>`|  

**Authentification Windows**  

|Section|Éléments de configuration|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name="WindowsAuthenticationModule" image="%windir%\System32\inetsrv\authsspi.dll" />`|  
|`<modules>`|`<add name="WindowsAuthenticationModule" lockItem="true" />`|  
|`<windowsAuthentication>`|`<windowsAuthentication enabled="false" authPersistNonNTLM\="true"><br />    <providers><br />        <add value="Negotiate" /><br />        <add value="NTLM" /><br />    <\providers><br /><\windowsAuthentication><windowsAuthentication enabled="false" authPersistNonNTLM\="true"><br />    <providers><br />        <add value="Negotiate" /><br />        <add value="NTLM" /><br />    <\/providers><br /><\/windowsAuthentication>`|  

### <a name="application-development"></a>Développement d’applications  
**Initialisation de l’application**  

|Section|Éléments de configuration|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="ApplicationInitializationModule" image="%windir%\System32\inetsrv\warmup.dll" />`|  
|`<modules>`|`<add name="ApplicationInitializationModule" lockItem="true" />`|  

**CGI**  

|Section|Éléments de configuration|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="CgiModule" image="%windir%\System32\inetsrv\cgi.dll" /><br /><add name="FastCgiModule" image="%windir%\System32\inetsrv\iisfcgi.dll" />`|  
|`<modules>`|`<add name="CgiModule" lockItem="true" /><br /><add name="FastCgiModule" lockItem="true" />`|  
|`<handlers>`|`<add name="CGI-exe" path="*.exe" verb="\*" modules="CgiModule" resourceType="File" requireAccess="Execute" allowPathInfo="true" />`|  

**Extensions ISAPI**  

|Section|Éléments de configuration|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name="IsapiModule" image="%windir%\System32\inetsrv\isapi.dll" />`|  
|`<modules>`|`<add name="IsapiModule" lockItem="true" />`|  
|`<handlers>`|`<add name="ISAPI-dll" path="*.dll" verb="*" modules="IsapiModule" resourceType="File" requireAccess="Execute" allowPathInfo="true" />`|  

**Filtres ISAPI**  

|Section|Éléments de configuration|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name="IsapiFilterModule" image="%windir%\System32\inetsrv\filter.dll" />`|  
|`<modules>`|`<add name="IsapiFilterModule" lockItem="true" />`|  

**Inclusions côté serveur**  

|Section|Éléments de configuration|  
|----------------|--------------------------|  
|`<globalModules>`|<`add name="ServerSideIncludeModule" image="%windir%\System32\inetsrv\iis_ssi.dll" />`|  
|`<modules>`|`<add name="ServerSideIncludeModule" lockItem="true" />`|  
|`<handlers>`|`<add name="SSINC-stm" path="*.stm" verb="GET,HEAD,POST" modules="ServerSideIncludeModule" resourceType="File" \/><br /><add name="SSINC-shtm" path="*.shtm" verb="GET,HEAD,POST" modules="ServerSideIncludeModule" resourceType="File" /><br /><add name="SSINC-shtml" path="*.shtml" verb="GET,HEAD,POST" modules="ServerSideIncludeModule" resourceType="File" />`|  
|`<serverSideInclude>`|`<serverSideInclude ssiExecDisable="false" />`|  

**Protocole WebSocket**  

|Section|Éléments de configuration|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name="WebSocketModule" image="%windir%\System32\inetsrv\iiswsock.dll" />`|  
|`<modules>`|`<add name="WebSocketModule" lockItem="true" />`|  