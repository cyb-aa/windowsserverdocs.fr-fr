---
title: Préparer la migration d’une batterie de serveurs AD FS 2,0 WID
description: Contient des informations sur la préparation de la migration d’une batterie de serveurs AD FS 2,0 Server WID vers Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e6612856a2e00c47e9cc87c75c802ff86697b781
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359260"
---
# <a name="prepare-to-migrate-an-ad-fs-20-wid-farm"></a>Préparer la migration d’une batterie de serveurs AD FS 2,0 WID  
 Pour préparer la migration des serveurs de fédération AD FS 2,0 qui appartiennent à une batterie de serveurs de base de données interne Windows (WID) vers Windows Server 2012, vous devez exporter et sauvegarder les données de configuration AD FS à partir de ces serveurs.  
  
 Pour exporter les données de configuration AD FS, effectuez les tâches suivantes :  
  
-   [Étape 1 :-exporter les paramètres du service](#step-1-export-service-settings)  
  
-   [Étape 2 : Sauvegarder les magasins d’attributs personnalisés @ no__t-0  
  
-   [Étape 3 : Sauvegarder les personnalisations de pages Web @ no__t-0  
  
## <a name="step-1-export-service-settings"></a>Étape 1 : exporter les paramètres de service  
 Pour exporter les paramètres de service, effectuez la procédure suivante :  
  
### <a name="to-export-service-settings"></a>Pour exporter les paramètres de service  
  
1.  Enregistrez le nom du sujet du certificat et la valeur de l'empreinte numérique du certificat SSL utilisé par le service de fédération. Pour rechercher le certificat SSL, ouvrez la console de gestion Internet Information Services (IIS), sélectionnez **site Web par défaut** dans le volet gauche, cliquez sur **liaisons.** . dans le volet **actions** , recherchez et sélectionnez la liaison https, cliquez sur **modifier**, puis sur **Afficher**.  
  
> [!NOTE]
>  Éventuellement, vous pouvez aussi exporter le certificat SSL et sa clé privée dans un fichier .pfx. Pour plus d’informations, voir [Exporter la partie clé privée d’un certificat d’authentification serveur](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md).  
>   
>  Cette étape est facultative, car ce certificat est stocké dans le magasin de certificats personnels sur l'ordinateur local et est préservé pendant la mise à niveau du système d'exploitation.  
  
2. Outre les certificats auto-signés, exportez les clés et les certificats de signature de jetons, de chiffrement de jetons ou de communication de service qui ne sont pas générés de manière interne.  
  
Vous pouvez afficher tous les certificats en cours d'utilisation sur votre serveur à l'aide de Windows PowerShell. Ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande AD FS à votre session Windows PowerShell : `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Exécutez ensuite la commande suivante pour afficher tous les certificats en cours d’utilisation sur votre serveur `PSH:>Get-ADFSCertificate`. La sortie de cette commande comprend les valeurs StoreLocation et StoreName, qui spécifient l'emplacement du magasin de chaque certificat.  Vous pouvez ensuite suivre les instructions indiquées dans la rubrique [Exporter la partie clé privée d'un certificat d'authentification serveur](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md) pour exporter chaque certificat et sa clé privée dans un fichier .pfx.  
  
> [!NOTE]
>  Cette étape est facultative, car tous les certificats externes sont préservés pendant la mise à niveau du système d'exploitation.  
  
3. Enregistrez l’identité du compte de service de fédération AD FS 2,0 et le mot de passe de ce compte.  
  
Pour rechercher la valeur d’identité, examinez la colonne **ouvrir une session en tant que** de **AD FS service Windows 2,0** dans la console **services** et enregistrez manuellement la valeur.  
  
## <a name="step-2-back-up-custom-attribute-stores"></a>Étape 2 : sauvegarder les magasins d'attributs personnalisés  
 Windows PowerShell vous permet d'obtenir des informations sur les magasins d'attributs personnalisés utilisés par AD FS. Ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande AD FS à votre session Windows PowerShell : `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Exécutez ensuite la commande suivante pour rechercher des informations sur les magasins d’attributs personnalisés : `PSH:>Get-ADFSAttributeStore`. Les étapes de la mise à niveau ou de la migration des magasins d’attributs personnalisés varient.  
  
## <a name="step-3-back-up-webpage-customizations"></a>Étape 3 : sauvegarder les personnalisations de page web  
 Pour sauvegarder les personnalisations de page Web, copiez les AD FS pages Web et le fichier **Web. config** à partir du répertoire mappé au chemin d’accès virtuel **« /ADFS/LS »** dans IIS. L'emplacement par défaut est le répertoire **%systemdrive%\inetpub\adfs\ls** .  

## <a name="next-steps"></a>Étapes suivantes
 [Préparer la migration du serveur de fédération AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Préparer la migration du serveur proxy de fédération AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrer le serveur de fédération AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Migrer le serveur proxy de fédération AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrer les agents web AD FS 1.1](migrate-the-ad-fs-web-agent.md)