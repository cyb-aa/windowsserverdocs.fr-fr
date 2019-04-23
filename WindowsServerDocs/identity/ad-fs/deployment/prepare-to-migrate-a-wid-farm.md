---
title: Préparer la migration d’une batterie de serveurs WID AD FS 2.0
description: Fournit des informations sur la préparation migrer une batterie de serveurs AD FS 2.0 server WID vers Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4985a8d16614bd12bce991e196d105464d37634d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845060"
---
# <a name="prepare-to-migrate-an-ad-fs-20-wid-farm"></a>Préparer la migration d’une batterie de serveurs WID AD FS 2.0  
 Pour préparer la migration des serveurs de fédération 2.0 AD FS qui appartiennent à une batterie de serveurs de base de données interne de Windows (WID) vers Windows Server 2012, vous devez exporter et sauvegarder les données de configuration AD FS à partir de ces serveurs.  
  
 Pour exporter les données de configuration AD FS, effectuez les tâches suivantes :  
  
-   [Étape 1 : - exporter les paramètres de service](#step-1-export-service-settings)  
  
-   [Étape 2 : Sauvegarder des magasins d’attributs personnalisés](#step-2-back-up-custom-attribute-stores)  
  
-   [Étape 3 : Sauvegarder les personnalisations de page Web](#step-3-back-up-webpage-customizations)  
  
## <a name="step-1-export-service-settings"></a>Étape 1 : exporter les paramètres de service  
 Pour exporter les paramètres de service, effectuez la procédure suivante :  
  
### <a name="to-export-service-settings"></a>Pour exporter les paramètres de service  
  
1.  Enregistrez le nom du sujet du certificat et la valeur de l'empreinte numérique du certificat SSL utilisé par le service de fédération. Pour trouver le certificat SSL, ouvrez la console de gestion Internet Information Services (IIS), sélectionnez **Site Web par défaut** dans le volet gauche, cliquez sur **liaisons...** dans le **Action** volet, recherchez et sélectionnez la liaison https, cliquez sur **modifier**, puis cliquez sur **vue**.  
  
> [!NOTE]
>  Éventuellement, vous pouvez aussi exporter le certificat SSL et sa clé privée dans un fichier .pfx. Pour plus d’informations, voir [Exporter la partie clé privée d’un certificat d’authentification serveur](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md).  
>   
>  Cette étape est facultative, car ce certificat est stocké dans le magasin de certificats personnels sur l'ordinateur local et est préservé pendant la mise à niveau du système d'exploitation.  
  
2.  Outre les certificats auto-signés, exportez les clés et les certificats de signature de jetons, de chiffrement de jetons ou de communication de service qui ne sont pas générés de manière interne.  
  
Vous pouvez afficher tous les certificats en cours d'utilisation sur votre serveur à l'aide de Windows PowerShell. Ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande AD FS à votre session Windows PowerShell : `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Puis exécutez la commande suivante pour afficher tous les certificats qui sont en cours d’utilisation sur votre serveur `PSH:>Get-ADFSCertificate`. La sortie de cette commande comprend les valeurs StoreLocation et StoreName, qui spécifient l'emplacement du magasin de chaque certificat.  Vous pouvez ensuite suivre les instructions indiquées dans la rubrique [Exporter la partie clé privée d'un certificat d'authentification serveur](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md) pour exporter chaque certificat et sa clé privée dans un fichier .pfx.  
  
> [!NOTE]
>  Cette étape est facultative, car tous les certificats externes sont préservés pendant la mise à niveau du système d'exploitation.  
  
3.  Enregistrez l’identité du compte de service AD FS 2.0 de fédération et le mot de passe de ce compte.  
  
Pour rechercher la valeur d’identité, examinez le **session en tant que** colonne de **AD FS 2.0 Windows Service** dans le **Services** console et enregistrez la valeur manuellement.  
  
## <a name="step-2-back-up-custom-attribute-stores"></a>Étape 2 : sauvegarder les magasins d'attributs personnalisés  
 Windows PowerShell vous permet d'obtenir des informations sur les magasins d'attributs personnalisés utilisés par AD FS. Ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande AD FS à votre session Windows PowerShell : `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Puis exécutez la commande suivante pour trouver des informations sur les magasins d’attributs personnalisés : `PSH:>Get-ADFSAttributeStore`. Les étapes de la mise à niveau ou de la migration des magasins d’attributs personnalisés varient.  
  
## <a name="step-3-back-up-webpage-customizations"></a>Étape 3 : sauvegarder les personnalisations de page web  
 Pour sauvegarder les personnalisations de page Web, copiez les pages Web AD FS et le **web.config** fichier à partir du répertoire qui est mappé au chemin virtuel **« / adfs/ls »** dans IIS. L'emplacement par défaut est le répertoire **%systemdrive%\inetpub\adfs\ls** .  

## <a name="next-steps"></a>Étapes suivantes
 [Préparer la migration du serveur AD FS 2.0 de fédération](prepare-to-migrate-ad-fs-fed-server.md)   
 [Préparer la migration du serveur Proxy pour AD FS 2.0 de fédération](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrer le serveur AD FS 2.0 de fédération](migrate-the-ad-fs-fed-server.md)   
 [Migrer le serveur Proxy AD FS 2.0 de fédération](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrer les Agents de 1.1 Web AD FS](migrate-the-ad-fs-web-agent.md)