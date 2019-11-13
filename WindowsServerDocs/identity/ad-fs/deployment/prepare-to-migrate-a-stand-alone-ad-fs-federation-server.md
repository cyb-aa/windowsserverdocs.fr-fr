---
title: Préparer la migration d’un serveur de fédération AD FS autonome
description: Contient des informations sur la préparation de la migration d’un serveur de AD FS autonome vers Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 09b8cbd9097a95cd00b1413ce9e32ff9bf2f44c3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359316"
---
#  <a name="prepare-to-migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>Préparer la migration d’un serveur de fédération AD FS autonome ou d’une batterie de AD FS à nœud unique  
 
Pour préparer la migration (même migration de serveur) d’un serveur de Fédération autonome AD FS 2,0 ou d’une batterie de AD FS à nœud unique vers Windows Server 2012, vous devez exporter et sauvegarder les données de configuration de AD FS à partir de ce serveur.  
  
Pour exporter les données de configuration AD FS, effectuez les tâches suivantes :  
  
-   [Étape 1 : exporter les paramètres de service](#step-1-export-service-settings)  
  
-   [Étape 2 : exporter des approbations de fournisseur de revendications](#step-2-export-claims-provider-trusts)  
  
-   [Étape 3 : exporter des approbations de partie de confiance](#step-3-export-relying-party-trusts)  
  
-   [Étape 4 : sauvegarder les magasins d’attributs personnalisés](#step-4-back-up-custom-attribute-stores)  
  
-   [Étape 5 : sauvegarder les personnalisations de page Web](#step-5-back-up-webpage-customizations)  
  
## <a name="step-1-export-service-settings"></a>Étape 1 : exporter les paramètres de service  
 Pour exporter les paramètres de service, effectuez la procédure suivante :  
  
### <a name="to-export-service-settings"></a>Pour exporter les paramètres de service  
  
1.  Enregistrez le nom du sujet du certificat et la valeur de l'empreinte numérique du certificat SSL utilisé par le service de fédération. Pour rechercher le certificat SSL, ouvrez la console de gestion des services Internet (IIS), sélectionnez **Site Web par défaut** dans le volet gauche, cliquez sur **Liaisons…** dans le volet **Actions** , recherchez et sélectionnez la liaison https, cliquez sur **Modifier**, puis sur **Afficher**.  
  
> [!NOTE]
>  Éventuellement, vous pouvez aussi exporter le certificat SSL utilisé par le service de fédération et sa clé privée dans un fichier .pfx. Pour plus d’informations, voir [Exporter la partie clé privée d’un certificat d’authentification serveur](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md).  
>   
>  L'exportation du certificat SSL est facultative, car ce certificat est stocké dans le magasin de certificats personnels sur l'ordinateur local et est préservé pendant la mise à niveau du système d'exploitation.  
  
2. Enregistrez la configuration des certificats de communication de service AD FS, de déchiffrement de jetons et de signature de jetons.  Pour afficher tous les certificats qui sont utilisés, ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande AD FS à votre session Windows PowerShell : `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Exécutez ensuite la commande suivante pour créer une liste de tous les certificats utilisés dans un fichier `PSH:>Get-ADFSCertificate | Out-File “.\certificates.txt”`  
  
> [!NOTE]
>  Éventuellement, vous pouvez aussi exporter les clés et les certificats de signature de jetons, de chiffrement de jetons ou de communication de service qui ne sont pas générés de manière interne, en plus de tous les certificats auto-signés. Vous pouvez afficher tous les certificats en cours d'utilisation sur votre serveur à l'aide de Windows PowerShell. Ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande AD FS à votre session Windows PowerShell : `PSH:>add-pssnapin “Microsoft.adfs.powershell`. Exécutez ensuite la commande suivante pour afficher tous les certificats en cours d’utilisation sur votre serveur `PSH:>Get-ADFSCertificate`. La sortie de cette commande comprend les valeurs StoreLocation et StoreName, qui spécifient l'emplacement du magasin de chaque certificat. Vous pouvez ensuite suivre les instructions indiquées dans la rubrique [Exporter la partie clé privée d'un certificat d'authentification serveur](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md) pour exporter chaque certificat et sa clé privée dans un fichier .pfx.  
>   
>  L'exportation de ces certificats est facultative, car tous les certificats externes sont préservés pendant la mise à niveau du système d'exploitation.  
  
3. Exportez les propriétés du service de fédération AD FS 2,0, telles que le nom du service de Fédération, le nom complet du service de Fédération et l’identificateur du serveur de Fédération dans un fichier.  
  
Pour exporter les propriétés du service de Fédération, ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande AD FS à votre session Windows PowerShell : `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Exécutez ensuite la commande suivante pour exporter les propriétés du service de Fédération : `PSH:> Get-ADFSProperties | Out-File “.\properties.txt”`.  
  
Le fichier de sortie doit contenir les valeurs de configuration importantes suivantes :  
  
    
|**Service FS (Federation Service) nom de la propriété comme indiqué par la fonction ADFSProperties**|**Nom de la propriété service FS (Federation Service) dans AD FS Management Console**|
|------|------|
|HostName|Nom du service de fédération|  
|Identificateur|Identificateur du service de fédération|  
|DisplayName|Nom complet du service de fédération|  
  
4. Sauvegardez le fichier de configuration de l’application. Parmi d’autres paramètres, ce fichier contient la chaîne de connexion à la base de données de stratégies.  
  
Pour sauvegarder le fichier de configuration de l’application, vous devez copier manuellement le fichier `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` à un emplacement sécurisé sur un serveur de sauvegarde.  
  
> [!NOTE]
>  Notez la chaîne de connexion à la base de données dans ce fichier, située immédiatement après "policystore connectionstring=". Si la chaîne de connexion spécifie une base de données SQL Server, la valeur est nécessaire lors de la restauration de la configuration AD FS d’origine sur le serveur de fédération.  
>   
>  Voici un exemple de chaîne de connexion WID : `“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`. Voici un exemple de chaîne de connexion SQL Server : `"Data Source=databasehostname;Integrated Security=True"`.  
  
5. Enregistrez l’identité du compte de service de fédération AD FS 2,0 et le mot de passe de ce compte.  
  
Pour rechercher la valeur d’identité, examinez la colonne **Ouvrir une session en tant que** de **Service Windows AD FS 2.0** dans la console des **services** et enregistrez manuellement cette valeur.  
  
> [!NOTE]
>  Pour un service de fédération autonome, le compte NETWORK SERVICE intégré est utilisé.  Dans ce cas, vous n’avez pas besoin de mot de passe.  
  
6. Exportez la liste des points de terminaison AD FS activés dans un fichier.  
  
Pour ce faire, ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande AD FS à votre session Windows PowerShell : `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Exécutez ensuite la commande suivante pour exporter la liste des points de terminaison de AD FS activés dans un fichier : `PSH:> Get-ADFSEndpoint | Out-File “.\endpoints.txt”`.  
  
7. Exportez toutes les descriptions des revendications personnalisées dans un fichier.  
  
Pour ce faire, ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande AD FS à votre session Windows PowerShell : `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Exécutez ensuite la commande suivante pour exporter les descriptions de revendication personnalisées dans un fichier : `Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`.  
  
##  <a name="step-2-export-claims-provider-trusts"></a>Étape 2 : exporter des approbations de fournisseur de revendications  
 Pour exporter les approbations de fournisseur de revendications, effectuez la procédure suivante :  
  
### <a name="to-export-claims-provider-trusts"></a>Pour exporter les approbations de fournisseur de revendications  
  
1.  Vous pouvez utiliser Windows PowerShell pour exporter toutes les approbations de fournisseur de revendications. Ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande AD FS à votre session Windows PowerShell : `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Exécutez ensuite la commande suivante pour exporter toutes les approbations de fournisseur de revendications : `PSH:>Get-ADFSClaimsProviderTrust | Out-File “.\cptrusts.txt”`.  
  
## <a name="step-3-export-relying-party-trusts"></a>Étape 3 : exporter des approbations de partie de confiance  
 Pour exporter les approbations de partie de confiance, effectuez la procédure suivante :  
  
### <a name="to-export-relying-party-trusts"></a>Pour exporter les approbations de partie de confiance  
  
1.  Pour exporter toutes les approbations de partie de confiance, ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande AD FS à votre session Windows PowerShell : `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Exécutez ensuite la commande suivante pour exporter toutes les approbations de partie de confiance :`PSH:>Get-ADFSRelyingPartyTrust | Out-File “.\rptrusts.txt”`.  
  
## <a name="step-4-back-up-custom-attribute-stores"></a>Étape 4 : sauvegarder les magasins d’attributs personnalisés  
 Windows PowerShell vous permet d'obtenir des informations sur les magasins d'attributs personnalisés utilisés par AD FS. Ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande AD FS à votre session Windows PowerShell : `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Exécutez ensuite la commande suivante pour rechercher des informations sur les magasins d’attributs personnalisés : `PSH:>Get-ADFSAttributeStore`. Les étapes de la mise à niveau ou de la migration des magasins d’attributs personnalisés varient.  
  
## <a name="step-5-back-up-webpage-customizations"></a>Étape 5 : sauvegarder les personnalisations de page Web  
 Pour sauvegarder les personnalisations de page Web, copiez les AD FS pages Web et le fichier **Web. config** à partir du répertoire mappé au chemin d’accès virtuel **« /ADFS/LS »** dans IIS. L'emplacement par défaut est le répertoire **%systemdrive%\inetpub\adfs\ls** .  

## <a name="next-steps"></a>Étapes suivantes
 [Préparer la migration du serveur de fédération AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Préparer la migration du serveur proxy de fédération AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrer le serveur de fédération AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Migrer le serveur proxy de fédération AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrer les agents web AD FS 1.1](migrate-the-ad-fs-web-agent.md)