---
title: "Préparer la migration d’un serveur de fédération ADFS autonome"
description: "Fournit des informations sur l’obtention de prêt à migrer un serveur AD FS autonome pour Windows Server 2012."
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0c1fd2bc1026d9aee25c591cf5c91a1c59f66ee0
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
#  <a name="prepare-to-migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>Préparer la migration d’un serveur de fédération ADFS autonome ou une batterie ADFS à nœud unique  
 
Pour préparer la migration (même migration serveur) serveur ADFS 2.0 de fédération autonome ou une batterie ADFS à nœud unique vers Windows Server2012, vous devez exporter et sauvegarder les données de configuration ADFS à partir de ce serveur.  
  
Pour exporter les données de configuration ADFS, effectuez les tâches suivantes:  
  
-   [Étape1: Exporter les paramètres de service](#step-1-export-service-settings)  
  
-   [Étape2: Exporter les approbations de fournisseur de revendications](#step-2-export-claims-provider-trusts)  
  
-   [Étape3: Exporter les approbations de partie de confiance](#step-3-export-relying-party-trusts)  
  
-   [Étape4: Sauvegarder les magasins d’attributs personnalisés](#step-4-back-up-custom-attribute-stores)  
  
-   [Étape5: Sauvegarder les personnalisations de page Web](#step-5-back-up-webpage-customizations)  
  
## <a name="step-1-export-service-settings"></a>Étape1: Exporter les paramètres de service  
 Pour exporter les paramètres de service, effectuez la procédure suivante:  
  
### <a name="to-export-service-settings"></a>Pour exporter les paramètres de service  
  
1.  Enregistrez le certificat nom et l’empreinte numérique valeur objet du certificat SSL utilisé par le service de fédération. Pour trouver le certificat SSL, ouvrez la gestion Internet Information Services (IIS) de la console, sélectionnez **Site Web par défaut** dans le volet gauche, cliquez sur **liaisons...** dans le **Action** volet, rechercher et sélectionner la liaison https, cliquez sur **modifier**, puis cliquez sur **affichage**.  
  
> [!NOTE]
>  Si vous le souhaitez, vous pouvez également exporter le certificat SSL utilisé par le service de fédération et sa clé privée dans un fichier .pfx. Pour plus d’informations, voir [exporter la partie clé privée d’un certificat d’authentification serveur ](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md).  
>   
>  Exportation du certificat SSL est facultative, car ce certificat est stocké dans le magasin de certificats personnel d’ordinateur local et est préservé pendant la mise à niveau du système d’exploitation.  
  
2.  Enregistrez la configuration des communications de Service ADFS, les certificats de déchiffrement de jeton et de signature de jetons.  Pour afficher tous les certificats qui sont utilisés, ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande ADFS à votre session Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Puis exécutez la commande suivante pour créer une liste de tous les certificats en cours d’utilisation dans un fichier `PSH:>Get-ADFSCertificate | Out-File “.\certificates.txt”`  
  
> [!NOTE]
>  Si vous le souhaitez, vous pouvez également exporter les clés qui ne sont pas générés de manière interne, en plus de tous les certificats auto-signés et les certificats de signature de jetons, de chiffrement de jeton ou de communication de service. Vous pouvez afficher tous les certificats qui sont en cours d’utilisation sur votre serveur à l’aide de Windows PowerShell. Ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande ADFS à votre session Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell`. Puis exécutez la commande suivante pour afficher tous les certificats qui sont en cours d’utilisation sur votre serveur `PSH:>Get-ADFSCertificate`. La sortie de cette commande inclut des valeurs StoreLocation et StoreName, qui spécifient l’emplacement du magasin de chaque certificat. Vous pouvez ensuite utiliser les recommandations de [exporter la partie clé privée d’un certificat d’authentification serveur](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md) pour exporter chaque certificat et sa clé privée dans un fichier .pfx.  
>   
>  Exportation de ces certificats est facultative, car tous les certificats externes sont préservés pendant la mise à niveau du système d’exploitation.  
  
3.  Exportation ADFS 2.0 federation service propriétés, telles que le nom du service de fédération, service de fédération nom complet et identificateur du serveur de fédération dans un fichier.  
  
Pour exporter les propriétés du service de fédération, ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande ADFS à votre session Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Puis exécutez la commande suivante pour exporter les propriétés du service de fédération:`PSH:> Get-ADFSProperties | Out-File “.\properties.txt”`.  
  
Le fichier de sortie contient les valeurs de configuration importantes suivantes:  
  
    
|**Nom de propriété du Service de fédération signalé par Get-ADFSProperties**|**Nom de propriété du Service de fédération dans la console de gestion AD FS**|
|------|------|
|Nom d’hôte|Nom de Service de fédération|  
|Identificateur|Identificateur du Service de fédération|  
|DisplayName|Nom complet du Service FS|  
  
4.  Sauvegardez le fichier de configuration d’application. Parmi les autres paramètres, ce fichier contient la chaîne de connexion de base de données de stratégie.  
  
Pour sauvegarder le fichier de configuration d’application, vous devez copier manuellement le `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` fichier à un emplacement sécurisé sur un serveur de sauvegarde.  
  
> [!NOTE]
>  Prenez note de la chaîne de connexion de base de données dans ce fichier, située immédiatement après «policystore connectionstring =»). Si la chaîne de connexion spécifie une base de données SQL Server, la valeur est nécessaire lors de la restauration de la configuration AD FS d’origine sur le serveur de fédération.  
>   
>  Voici un exemple de chaîne de connexion WID: `“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`. Voici un exemple de chaîne de connexion SQL Server: `"Data Source=databasehostname;Integrated Security=True"`.  
  
5.  Enregistrez l’identité du compte de service ADFS 2.0 de fédération et le mot de passe de ce compte.  
  
Pour rechercher la valeur d’identité, examinez le **journal en tant que** colonne de **AD FS 2.0 Windows Service** dans les **Services** de la console et enregistrez manuellement cette valeur.  
  
> [!NOTE]
>  Pour un service de fédération autonome, le compte de SERVICE réseau intégré est utilisé.  Dans ce cas, vous n’avez pas besoin d’avoir un mot de passe.  
  
6.  Exporter la liste des points de terminaison AD FS activés dans un fichier.  
  
Pour ce faire, ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande ADFS à votre session Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Puis exécutez la commande suivante pour exporter la liste des points de terminaison ADFS activés dans un fichier:`PSH:> Get-ADFSEndpoint | Out-File “.\endpoints.txt”`.  
  
7.  Exporter toutes les descriptions des revendications personnalisées dans un fichier.  
  
Pour ce faire, ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande ADFS à votre session Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Puis exécutez la commande suivante pour exporter toutes les descriptions des revendications personnalisées dans un fichier:`Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`.  
  
##  <a name="step-2-export-claims-provider-trusts"></a>Étape2: Exporter les approbations de fournisseur de revendications  
 Pour exporter les approbations de fournisseur de revendications, effectuez la procédure suivante:  
  
### <a name="to-export-claims-provider-trusts"></a>Pour exporter les approbations de fournisseur de revendications  
  
1.  Vous pouvez utiliser Windows PowerShell pour exporter toutes les approbations de fournisseur de revendications. Ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande AD FS à votre session Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Puis exécutez la commande suivante pour exporter toutes les approbations de fournisseur de revendications:`PSH:>Get-ADFSClaimsProviderTrust | Out-File “.\cptrusts.txt”`.  
  
## <a name="step-3-export-relying-party-trusts"></a>Étape3: Exporter les approbations de partie de confiance  
 Pour exporter les approbations de partie de confiance, effectuez la procédure suivante:  
  
### <a name="to-export-relying-party-trusts"></a>Pour exporter les approbations de partie de confiance  
  
1.  Pour exporter les approbations de partie de confiance tout, ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande ADFS à votre session Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Puis exécutez la commande suivante pour exporter les approbations de partie de confiance tout:`PSH:>Get-ADFSRelyingPartyTrust | Out-File “.\rptrusts.txt”`.  
  
## <a name="step-4-back-up-custom-attribute-stores"></a>Étape4: Sauvegarder les magasins d’attributs personnalisés  
 Vous trouverez des informations sur les magasins d’attributs personnalisés utilisés par ADFS à l’aide de Windows PowerShell. Ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande AD FS à votre session Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Puis exécutez la commande suivante pour trouver des informations sur les magasins d’attributs personnalisés:`PSH:>Get-ADFSAttributeStore`. Les étapes pour mettre à niveau ou la migration des magasins d’attributs personnalisés varient.  
  
## <a name="step-5-back-up-webpage-customizations"></a>Étape5: Sauvegarder les personnalisations de page Web  
 Pour sauvegarder les personnalisations de page Web, copiez les pages Web ADFS et le **web.config** fichier à partir du répertoire qui est mappé sur le chemin d’accès virtuel **«/ adfs/ls»** dans IIS. Par défaut, il est dans le **%systemdrive%\inetpub\adfs\ls** active.  

## <a name="next-steps"></a>Étapes suivantes
 [Préparer la migration du serveur AD FS 2.0 de fédération](prepare-to-migrate-ad-fs-fed-server.md)   
 [Préparer la migration AD FS 2.0 serveur Proxy de fédération](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrer le serveur AD FS 2.0 de fédération](migrate-the-ad-fs-fed-server.md)   
 [Migrer le serveur Proxy AD FS 2.0 de fédération](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrer les Agents de 1.1 Web AD FS](migrate-the-ad-fs-web-agent.md)