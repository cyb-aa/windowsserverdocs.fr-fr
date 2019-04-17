---
title: "Migrer un serveur de fédération AD FS autonome ou une batterie AD FS à nœud unique"
description: "Fournit des informations sur la migration d’un serveur ADFS 2.0 de support autonome ou à un seul nœud AD vers Windows Server 2012"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 10371349fe19be92fb997c9c28f19def0ecad7e9
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>Migrer un serveur de fédération AD FS autonome ou une batterie AD FS à nœud unique  
Ce document fournit des informations détaillées sur la migration d’un serveur de seul AD FS 2.0 de support pour Windows Server 2012.

## <a name="migrate-a-stand-alone-ad-fs-20-server"></a>Migrer un autonome AD FS 2.0 server

Utilisez la procédure suivante pour migrer les services AD FS 2.0 server vers Windows Server 2012.
  
1.  Consultez et effectuez les procédures de [préparer la migration d’un serveur de fédération AD FS autonome ou une batterie AD FS à nœud unique](prepare-to-migrate-a-stand-alone-ad-fs-federation-server.md).  
  
2.  Effectuez une mise à niveau sur place du système d’exploitation sur votre serveur à partir de Windows Server 2008 R2 ou Windows Server 2008 vers Windows Server 2012. Pour plus d’informations, voir [l’installation de Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Le résultat de la mise à niveau du système d’exploitation, la configuration AD FS sur ce serveur est perdue et le rôle de serveur 2.0 AD FS est supprimé. Le rôle de serveur AD FS Windows Server 2012 est installé à la place, mais il n’est pas configuré. Vous devez manuellement créer la configuration AD FS d’origine et restaurer les paramètres AD FS restants pour terminer la migration de serveur de fédération.  
  
3.  Créez la configuration AD FS d’origine. Vous pouvez créer la configuration AD FS d’origine à l’aide d’une des méthodes suivantes:  
  
-   Utilisez le **Assistant Configuration du serveur de fédération AD FS** pour créer un nouveau serveur de fédération. Pour plus d’informations, voir [créer le premier serveur de fédération dans une batterie de serveurs de fédération](Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md).  
  
Tout au long de l’Assistant, utilisez les informations que vous avez rassemblées lors de la préparation de la migration de votre serveur de fédération AD FS comme suit:  
  
 |**Option d’entrée Assistant Configuration du serveur de fédération**|**Utilisez la valeur suivante**| 
|-----|-----| 
|**Certificat SSL** sur le **spécifier le nom de Service de fédération** page|Sélectionnez le certificat SSL dont le nom de sujet et l’empreinte numérique vous avez enregistré pendant la préparation de la migration de serveur de fédération AD FS.|  
|**Compte de service** et **mot de passe** sur le **spécifier un compte de Service** page|Entrez les informations de compte de service que vous avez enregistrées pendant la préparation de la migration de serveur de fédération AD FS. **Remarque:** si vous sélectionnez le serveur de fédération autonome sur la deuxième page de l’Assistant, SERVICE réseau est automatiquement utilisé comme compte de service.|  
  
> [!IMPORTANT] 
> Vous pouvez employer cette méthode que si vous utilisez une base de données interne Windows (WID) pour stocker la base de données de configuration AD FS pour votre serveur de fédération autonome ou une batterie AD FS à nœud unique.  
>
>  Si vous utilisez SQL Server pour stocker la base de données de configuration AD FS pour votre batterie AD FS à nœud unique, vous devez utiliser Windows PowerShell pour créer la configuration AD FS d’origine sur votre serveur de fédération.  
  
-   Utiliser Windows PowerShell  
  
> [!IMPORTANT]
>  Vous devez utiliser Windows PowerShell si vous utilisez SQL Server pour stocker la base de données de configuration AD FS pour votre serveur de fédération autonome ou une batterie AD FS à nœud unique.  
  
Voici un exemple montrant comment utiliser Windows PowerShell pour créer la configuration AD FS d’origine sur un serveur de fédération dans une batterie de serveurs SQL Server à nœud unique.  Ouvrez le module Windows PowerShell et exécutez la commande suivante: `$fscredential = Get-Credential`. Entrez le nom et le mot de passe du compte de service que vous avez enregistrées pendant la préparation de votre batterie de serveurs SQL server pour la migration. Puis exécutez la commande suivante: `C:\PS> Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"` où `Data Source` est la valeur de source de données dans la valeur de chaîne de connexion de magasin de stratégie dans le fichier suivant: `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`.  
  
4.  Restaurer les paramètres de service AD FS restants et les relations d’approbation. Il s’agit d’une étape manuelle pendant laquelle vous pouvez utiliser les fichiers que vous avez exporté et les valeurs que vous avez collectées pendant la préparation de la migration AD FS. Pour obtenir des instructions détaillées, voir la restauration de la Configuration de la batterie restant AD FS.  
  
> [!NOTE]
>  Cette étape est uniquement nécessaire si vous migrez un serveur de fédération autonome ou une batterie WID à nœud unique.  Si le serveur de fédération utilise une base de données SQL Server comme magasin de configuration, les paramètres de service et les relations d’approbation sont conservées dans la base de données.  
  
5.  Mettre à jour vos pages Web AD FS. Il s’agit d’une étape manuelle. Si vous avez sauvegardé vos pages Web AD FS personnalisées pendant la préparation de la migration, utiliser vos données de sauvegarde pour remplacer les pages Web AD FS qui ont été créés par défaut dans le **%systemdrive%\inetpub\adfs\ls** répertoire suite à la configuration AD FS dans Windows Server 2012.  
  
6.  Restaurez toutes les personnalisations AD FS restantes, telles que les magasins d’attributs personnalisés.  
  
## <a name="restoring-the-remaining-ad-fs-farm-configuration"></a>Restauration de la Configuration de batterie de serveurs FS restante AD  
  
-   Restaurer les paramètres de service AD FS suivantes à une batterie à nœud unique WID ou le service de fédération autonome comme suit:  
  
    -   Dans la console de gestion AD FS, sélectionnez **Service** et cliquez sur **Service de fédération modifier... **. Vérifiez les paramètres de service de fédération par des valeurs aux valeurs que vous avez exportées dans le fichier properties.txt pendant la préparation de la migration:  
  
    
|**Nom de propriété du Service de fédération signalé par Get-ADFSProperties**|**Nom de propriété du Service de fédération dans la console de gestion AD FS**|  
|-----|-----|
|DisplayName|Nom complet du Service FS|  
|Nom d’hôte|Nom de Service de fédération|  
|Identificateur|Identificateur du Service de fédération|  
  
-   Dans la console de gestion AD FS, sélectionnez **certificats**. Vérifier la communication du service, les certificats de déchiffrement de jeton et de signature de jetons, par rapport aux valeurs que vous avez exportées dans le fichier certificates.txt pendant la préparation de la migration.  
  
Pour modifier les certificats de signature de jetons ou de déchiffrement de jeton à partir des valeur par défaut des certificats auto-signés pour des certificats externes, vous devez d’abord désactiver la fonctionnalité de substitution automatique de certificat qui est activée par défaut.  Pour ce faire, vous pouvez utiliser la commande Windows PowerShell suivante: `PSH: Set-ADFSProperties –AutoCertificateRollover $false`.  
  
-   Dans la console de gestion AD FS, sélectionnez **les points de terminaison**. Vérifiez les points de terminaison AD FS activés par rapport à la liste des points de terminaison AD FS activés que vous avez exporté dans un fichier pendant la préparation de la migration AD FS.  
  
-   Dans la console de gestion AD FS, sélectionnez **descriptions des revendications**. Vérifiez la liste des descriptions de revendication d’AD FS par rapport à la liste des descriptions de revendication que vous avez exporté dans un fichier pendant la préparation de la migration AD FS. Ajoutez toutes les descriptions des revendications personnalisées incluses dans votre fichier, mais ne pas inclus dans la liste par défaut dans AD FS.  Notez que l’identificateur de revendication dans la console de gestion est mappé au type de revendication dans le fichier.  Pour plus d’informations sur l’ajout de descriptions des revendications, voir [ajouter une Description de revendication](../operations/add-a-claim-description.md). Pour plus d’informations, voir la section «Étape 1 – exporter les paramètres du Service» préparer la migration AD FS 2.0 Federation Server.  
  
-   Dans la console de gestion AD FS, sélectionnez **approbations de fournisseur de revendications**. Vous devez recréer chaque approbation de fournisseur de revendications manuellement à l’aide du **Assistant approbation de fournisseur de revendications d’ajouter**.  Utilisez la liste des approbations de fournisseur de revendications que vous avez exportées et enregistrées pendant la préparation de la migration AD FS. Vous pouvez ignorer l’approbation de fournisseur de revendications avec l’identificateur «AD AUTHORITY» dans le fichier, car il s’agit de l’approbation de fournisseur de revendications «Active Directory» qui fait partie de la configuration AD FS par défaut.  Toutefois, vérifiez les règles de revendication personnalisées que vous avez ajouté à l’approbation Active Directory avant la migration. Pour plus d’informations sur la création d’approbations de fournisseur de revendications, voir [créer une revendications fournisseur faire confiance à l’aide de métadonnées de fédération](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-using-federation-metadata) ou [créer un revendications fournisseur approuver manuellement](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-manually).  
  
-   Dans la console de gestion AD FS, **sélectionnez approbations de partie de confiance**. Vous devez recréer chaque approbation de partie de confiance manuellement à l’aide du **Assistant Ajout de partie de confiance approbation**. Utilisez la liste des approbations de confiance que vous avez exportées et enregistrées pendant la préparation de la migration AD FS. Pour plus d’informations sur la création d’approbations de partie de confiance, voir [créer une partie de confiance partie confiance à l’aide de métadonnées de fédération](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-using-federation-metadata) ou [créer une partie de confiance manuellement](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-manually). 

## <a name="next-steps"></a>Étapes suivantes
 [Préparer la migration du serveur AD FS 2.0 de fédération](prepare-to-migrate-ad-fs-fed-server.md)   
 [Préparer la migration AD FS 2.0 serveur Proxy de fédération](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrer le serveur AD FS 2.0 de fédération](migrate-the-ad-fs-fed-server.md)   
 [Migrer le serveur Proxy AD FS 2.0 de fédération](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrer les Agents de 1.1 Web AD FS](migrate-the-ad-fs-web-agent.md)




-   
-    