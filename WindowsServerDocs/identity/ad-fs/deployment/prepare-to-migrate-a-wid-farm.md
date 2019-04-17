---
title: "Préparer la migration d’une batterie ADFS 2.0 WID"
description: "Fournit des informations sur l’obtention de prêt à migrer une batterie de serveurs ADFS 2.0 server WID vers Windows Server2012."
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4985a8d16614bd12bce991e196d105464d37634d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="prepare-to-migrate-an-ad-fs-20-wid-farm"></a>Préparer la migration d’une batterie ADFS 2.0 WID  
 Pour préparer la migration des serveurs ADFS 2.0 de fédération qui appartiennent à une batterie de serveurs de base de données interne Windows (WID) vers Windows Server2012, vous devez exporter et sauvegarder les données de configuration ADFS à partir de ces serveurs.  
  
 Pour exporter les données de configuration ADFS, effectuez les tâches suivantes:  
  
-   [Étape1: - exporter les paramètres de service](#step-1-export-service-settings)  
  
-   [Étape2: Sauvegarder les magasins d’attributs personnalisés](#step-2-back-up-custom-attribute-stores)  
  
-   [Étape3: Sauvegarder les personnalisations de page Web](#step-3-back-up-webpage-customizations)  
  
## <a name="step-1-export-service-settings"></a>Étape1: Exporter les paramètres de service  
 Pour exporter les paramètres de service, effectuez la procédure suivante:  
  
### <a name="to-export-service-settings"></a>Pour exporter les paramètres de service  
  
1.  Enregistrez le certificat nom et l’empreinte numérique valeur objet du certificat SSL utilisé par le service de fédération. Pour rechercher le certificat SSL, ouvrez la console de gestion Internet Information Services (IIS), sélectionnez **site Web par défaut** dans le volet gauche, cliquez sur **liaisons...** Dans le **Action** volet, rechercher et sélectionner la liaison https, cliquez sur **modifier**, puis cliquez sur **affichage **.  
  
> [!NOTE]
>  Si vous le souhaitez, vous pouvez également exporter le certificat SSL et sa clé privée dans un fichier .pfx. Pour plus d’informations, voir [exporter la partie clé privée d’un certificat d’authentification serveur ](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md).  
>   
>  Cette étape est facultative, car ce certificat est stocké dans le magasin de certificats personnel d’ordinateur local et est préservé pendant la mise à niveau du système d’exploitation.  
  
2.  Exporter les clés qui ne sont pas générés de manière interne, en plus des certificats auto-signés et les certificats de signature de jetons, de chiffrement de jeton ou de communication de service.  
  
Vous pouvez afficher tous les certificats qui sont en cours d’utilisation sur votre serveur à l’aide de Windows PowerShell. Ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande AD FS à votre session Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Puis exécutez la commande suivante pour afficher tous les certificats qui sont en cours d’utilisation sur votre serveur `PSH:>Get-ADFSCertificate`. La sortie de cette commande inclut des valeurs StoreLocation et StoreName, qui spécifient l’emplacement du magasin de chaque certificat.  Vous pouvez ensuite utiliser les recommandations de [exporter la partie clé privée d’un certificat d’authentification serveur](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md) pour exporter chaque certificat et sa clé privée dans un fichier .pfx.  
  
> [!NOTE]
>  Cette étape est facultative, car tous les certificats externes sont préservés pendant la mise à niveau du système d’exploitation.  
  
3.  Enregistrez l’identité du compte de service ADFS 2.0 de fédération et le mot de passe de ce compte.  
  
Pour rechercher la valeur d’identité, examinez le **journal en tant que** colonne de **ADFS 2.0Windows Service** dans les **Services** de la console et enregistrez la valeur manuellement.  
  
## <a name="step-2-back-up-custom-attribute-stores"></a>Étape2: Sauvegarder les magasins d’attributs personnalisés  
 Vous trouverez des informations sur les magasins d’attributs personnalisés utilisés par ADFS à l’aide de Windows PowerShell. Ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande AD FS à votre session Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Puis exécutez la commande suivante pour trouver des informations sur les magasins d’attributs personnalisés:`PSH:>Get-ADFSAttributeStore`. Les étapes pour mettre à niveau ou la migration des magasins d’attributs personnalisés varient.  
  
## <a name="step-3-back-up-webpage-customizations"></a>Étape3: Sauvegarder les personnalisations de page Web  
 Pour sauvegarder les personnalisations de page Web, copiez les pages Web ADFS et le **web.config** fichier à partir du répertoire qui est mappé sur le chemin d’accès virtuel **«/ adfs/ls»** dans IIS. Par défaut, il est dans le **%systemdrive%\inetpub\adfs\ls** active.  

## <a name="next-steps"></a>Étapes suivantes
 [Préparer la migration du serveur AD FS 2.0 de fédération](prepare-to-migrate-ad-fs-fed-server.md)   
 [Préparer la migration AD FS 2.0 serveur Proxy de fédération](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrer le serveur AD FS 2.0 de fédération](migrate-the-ad-fs-fed-server.md)   
 [Migrer le serveur Proxy AD FS 2.0 de fédération](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrer les Agents de 1.1 Web AD FS](migrate-the-ad-fs-web-agent.md)