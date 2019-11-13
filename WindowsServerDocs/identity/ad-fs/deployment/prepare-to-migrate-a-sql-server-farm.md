---
title: Préparer la migration d’une batterie de serveurs AD FS SQL
description: Contient des informations sur la préparation de la migration d’une batterie de serveurs SQL AD FS Server vers Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2d8bbe5021b876712862c992b643b7828095e869
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408216"
---
# <a name="prepare-to-migrate-a-sql-server-farm"></a>Préparer la migration d'une batterie SQL Server  
 Pour préparer la migration des serveurs de fédération AD FS 2,0 appartenant à une batterie de serveurs SQL Server vers Windows Server 2012, vous devez exporter et sauvegarder les données de configuration de AD FS à partir de ces serveurs.  
  
 Pour exporter les données de configuration AD FS, effectuez les tâches suivantes :  
  
-   [Étape 1 : exporter les paramètres de service](#step-1-export-service-settings)  
  
-   [Étape 2 : sauvegarder les magasins d’attributs personnalisés](#step-2-back-up-custom-attribute-stores)  
  
-   [Étape 3 : sauvegarder les personnalisations de page Web](#step-3-back-up-webpage-customizations)  
  
## <a name="step-1-export-service-settings"></a>Étape 1 : exporter les paramètres de service  
 Pour exporter les paramètres de service, effectuez la procédure suivante :  
  
### <a name="to-export-service-settings"></a>Pour exporter les paramètres de service  
  
1.  Enregistrez le nom du sujet du certificat et la valeur de l'empreinte numérique du certificat SSL utilisé par le service de fédération. Pour rechercher le certificat SSL, ouvrez la console de gestion Internet Information Services (IIS), sélectionnez **site Web par défaut** dans le volet gauche, cliquez sur **liaisons.** . dans le volet **Actions** , recherchez et sélectionnez la liaison https, cliquez sur **Modifier**, puis sur **Afficher**.  
  
> [!NOTE]
>  Éventuellement, vous pouvez aussi exporter le certificat SSL et sa clé privée dans un fichier .pfx. Pour plus d’informations, voir [Exporter la partie clé privée d’un certificat d’authentification serveur](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md).  
>   
>  Cette étape est facultative, car ce certificat est stocké dans le magasin de certificats personnels sur l'ordinateur local et est préservé pendant la mise à niveau du système d'exploitation.  
  
2. Exportez les autres clés et les certificats de signature de jetons, de chiffrement de jetons ou de communication de service qui ne sont pas générés de manière interne par AD FS.  
  
Vous pouvez afficher tous les certificats actuellement utilisés par AD FS sur votre serveur à l'aide de Windows PowerShell. Ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande AD FS à votre session Windows PowerShell : `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Exécutez ensuite la commande suivante pour afficher tous les certificats en cours d’utilisation sur votre serveur `PSH:>Get-ADFSCertificate`. La sortie de cette commande comprend les valeurs StoreLocation et StoreName, qui spécifient l'emplacement du magasin de chaque certificat.  
  
> [!NOTE]
>  Éventuellement, vous pouvez ensuite suivre les instructions indiquées dans la rubrique [Exporter la partie clé privée d'un certificat d'authentification serveur](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md) pour exporter chaque certificat et sa clé privée dans un fichier .pfx. Cette étape est facultative, car tous les certificats externes sont préservés pendant la mise à niveau du système d'exploitation.  
  
3. Sauvegardez le fichier de configuration de l’application. Parmi d’autres paramètres, ce fichier contient la chaîne de connexion à la base de données de stratégies.  
  
Pour sauvegarder le fichier de configuration de l’application, vous devez copier manuellement le fichier `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` à un emplacement sécurisé sur un serveur de sauvegarde.  
  
> [!NOTE]
>  Enregistrez la chaîne de connexion SQL Server après « PolicyStore ConnectionString = » dans le fichier suivant : `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`. Vous avez besoin de cette chaîne lorsque vous restaurez la configuration d’AD FS d’origine sur le serveur de Fédération.  
  
4. Enregistrez l’identité du compte de service de fédération AD FS 2,0 et le mot de passe de ce compte.  
  
Pour rechercher la valeur d’identité, examinez la colonne **ouvrir une session en tant que** de **AD FS service Windows 2,0** dans la console **services** et enregistrez manuellement la valeur.  
  
## <a name="step-2-back-up-custom-attribute-stores"></a>Étape 2 : sauvegarder les magasins d’attributs personnalisés  
 Windows PowerShell vous permet d'obtenir des informations sur les magasins d'attributs personnalisés utilisés par AD FS. Ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande AD FS à votre session Windows PowerShell : `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Exécutez ensuite la commande suivante pour rechercher des informations sur les magasins d’attributs personnalisés : `PSH:>Get-ADFSAttributeStore`. Les étapes de la mise à niveau ou de la migration des magasins d’attributs personnalisés varient.  
  
## <a name="step-3-back-up-webpage-customizations"></a>Étape 3 : sauvegarder les personnalisations de page Web  
 Pour sauvegarder les personnalisations de page Web, copiez les AD FS pages Web et le fichier **Web. config** à partir du répertoire mappé au chemin d’accès virtuel **« /ADFS/LS »** dans IIS. L'emplacement par défaut est le répertoire **%systemdrive%\inetpub\adfs\ls** .  
  
## <a name="next-steps"></a>Étapes suivantes
 [Préparer la migration du serveur de fédération AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Préparer la migration du serveur proxy de fédération AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrer le serveur de fédération AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Migrer le serveur proxy de fédération AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrer les agents web AD FS 1.1](migrate-the-ad-fs-web-agent.md)