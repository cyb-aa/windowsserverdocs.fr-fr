---
title: Migrer un serveur de fédération AD FS autonome ou une batterie de serveurs AD FS à nœud unique
description: Fournit des informations sur la migration d’un serveur d’ADFS 2.0 support AD seul ou à nœud unique vers Windows Server 2012
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5526afa758a142e30b9a238b4c7204cacebb1812
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444554"
---
# <a name="migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>Migrer un serveur de fédération AD FS autonome ou une batterie de serveurs AD FS à nœud unique  
Ce document fournit des informations détaillées sur la migration d’un serveur autonome de AD FS 2.0 support vers Windows Server 2012.

## <a name="migrate-a-stand-alone-ad-fs-20-server"></a>Migrer un autonome AD FS 2.0 server

Utilisez la procédure suivante pour migrer les services AD FS 2.0 server vers Windows Server 2012.
  
1.  Consultez et effectuez les procédures décrites dans [préparer la migration d’un serveur de fédération AD FS autonome ou une batterie de serveurs AD FS à nœud unique](prepare-to-migrate-a-stand-alone-ad-fs-federation-server.md).  
  
2.  Effectuer une mise à niveau sur place du système d’exploitation sur votre serveur à partir de Windows Server 2008 R2 ou Windows Server 2008 vers Windows Server 2012. Pour plus d’informations, voir [Installation de Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  En tant que le résultat de la mise à niveau du système d’exploitation, la configuration AD FS sur ce serveur est perdue et le rôle de serveur 2.0 AD FS est supprimé. Le rôle de serveur AD FS de Windows Server 2012 est installé à la place, mais il n’est pas configuré. Vous devez manuellement créer la configuration AD FS d’origine et restaurer les paramètres AD FS restants pour terminer la migration de serveur de fédération.  
  
3. Créez la configuration AD FS d’origine. Vous pouvez créer la configuration AD FS d’origine à l’aide d’une des méthodes suivantes :  
  
-   Utilisez le **Assistant Configuration du serveur de fédération AD FS** pour créer un nouveau serveur de fédération. Pour plus d'informations, consultez [Créer le premier serveur de fédération dans une batterie de serveurs de fédération](Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md).  
  
Dans l'Assistant, utilisez comme suit les informations que vous avez collectées pendant la préparation de la migration de votre serveur de fédération :  
  
 |**Option d’entrée Assistant Configuration du serveur de fédération**|**Utilisez la valeur suivante**| 
|-----|-----| 
|**Certificat SSL** dans la page **Spécifier le nom du service de fédération**|Sélectionnez le certificat SSL dont vous avez enregistré le nom du sujet et l'empreinte numérique pendant la préparation de la migration du serveur de fédération AD FS.|  
|**Compte de service** et **Mot de passe** dans la page **Spécifier un compte de service**|Entrez les informations de compte de service que vous avez enregistrées pendant la préparation de la migration du serveur de fédération AD FS. **Remarque :**  Si vous sélectionnez « serveur de fédération autonome » sur la seconde page de l'Assistant, SERVICE RÉSEAU est automatiquement utilisé comme compte de service.|  
  
> [!IMPORTANT] 
> Vous pouvez employer cette méthode uniquement si vous utilisez une base de données interne de Windows (WID) pour stocker la base de données de configuration AD FS pour votre serveur de fédération autonome ou une batterie de serveurs AD FS à nœud unique.  
>
>  Si vous utilisez SQL Server pour stocker la base de données de configuration AD FS pour votre batterie de serveurs AD FS à nœud unique, vous devez utiliser Windows PowerShell pour créer la configuration d’AD FS d’origine sur votre serveur de fédération.  
  
-   Utiliser Windows PowerShell  
  
> [!IMPORTANT]
>  Vous devez utiliser Windows PowerShell si vous utilisez SQL Server pour stocker la base de données de configuration AD FS pour votre serveur de fédération autonome ou une batterie de serveurs AD FS à nœud unique.  
  
Voici un exemple montrant comment utiliser Windows PowerShell pour créer la configuration AD FS d’origine sur un serveur de fédération dans une batterie de serveurs SQL Server à nœud unique.  Ouvrez le module Windows PowerShell et exécutez la commande suivante : `$fscredential = Get-Credential`. Entrez le nom et le mot de passe du compte de service que vous avez enregistré pendant la préparation de la migration de votre batterie de SQL Server. Puis exécutez la commande suivante : `C:\PS> Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"` où `Data Source` est la valeur de source de données dans la valeur de chaîne de connexion de magasin de stratégies dans le fichier suivant : `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`.  
  
4. Restaurez les paramètres de service AD FS restants et les relations d'approbation. Il s'agit d'une étape manuelle pendant laquelle vous pouvez utiliser les fichiers que vous avez exportés et les valeurs que vous avez collectées pendant la préparation de la migration AD FS. Pour obtenir des instructions détaillées, consultez « Restauration de la configuration restante de la batterie AD FS ».  
  
> [!NOTE]
>  Cette étape n'est nécessaire que si vous migrez un serveur de fédération autonome ou une batterie à nœud unique utilisant la base de données interne Windows.  Si le serveur de fédération utilise une base de données SQL Server comme magasin de configuration, les paramètres de service et les relations d'approbation sont conservés dans la base de données.  
  
5. Mettre à jour vos pages Web AD FS. Il s'agit d'une étape manuelle. Si vous avez sauvegardé vos pages Web AD FS personnalisées pendant la préparation de la migration, utiliser vos données de sauvegarde pour remplacer les pages Web AD FS qui ont été créés par défaut dans le **%systemdrive%\inetpub\adfs\ls** répertoire comme résultat de la configuration AD FS sur Windows Server 2012.  
  
6. Restaurez toutes les personnalisations AD FS restantes, telles que les magasins d'attributs personnalisés.  
  
## <a name="restoring-the-remaining-ad-fs-farm-configuration"></a>Restauration de la Configuration de batterie de serveurs FS AD restante  
  
-   Procédez comme suit pour restaurer les paramètres de service AD FS suivants sur une batterie à nœud unique utilisant la base de données interne Windows ou sur un serveur de fédération autonome :  
  
    -   Dans la console Gestion AD FS, sélectionnez **Service**, puis cliquez sur **Modifier le service de fédération**. Vérifiez les valeurs des paramètres du service de fédération par rapport aux valeurs que vous avez exportées dans le fichier properties.txt pendant la préparation de la migration :  
  
    
|**Nom de propriété du Service de fédération comme indiqué par Get-ADFSProperties**|**Nom de propriété du Service de fédération dans la console de gestion AD FS**|  
|-----|-----|
|DisplayName|Nom complet du service de fédération|  
|HostName|Nom du service de fédération|  
|Identificateur|Identificateur du service de fédération|  
  
-   Dans la console de gestion AD FS, sélectionnez **Certificats**. Vérifiez les certificats de communication du service, de déchiffrement de jetons et de signature de jetons par rapport aux valeurs que vous avez exportées dans le fichier certificates.txt pendant la préparation de la migration.  
  
Pour modifier les certificats de signature de jetons ou de déchiffrement de jeton en remplaçant les certificats auto-signés par défaut par des certificats externes, vous devez d’abord désactiver la fonctionnalité de substitution automatique de certificats qui est activée par défaut.  Pour ce faire, vous pouvez utiliser la commande Windows PowerShell suivante : `PSH: Set-ADFSProperties –AutoCertificateRollover $false`.  
  
-   Dans la console de gestion AD FS, sélectionnez **Points de terminaison**. Vérifiez les points de terminaison AD FS activés par rapport à la liste des points de terminaison AD FS activés que vous avez exportés dans un fichier lors de la préparation de la migration AD FS.  
  
-   Dans la console de gestion AD FS, sélectionnez **Descriptions des revendications**. Vérifiez la liste des descriptions des revendications AD FS par rapport à la liste des descriptions des revendications que vous avez exportées dans un fichier pendant la préparation de la migration AD FS. Ajoutez toutes les descriptions des revendications personnalisées incluses dans votre fichier, mais qui ne figurent pas dans la liste par défaut dans AD FS.  Notez que l’identificateur de revendication dans la console de gestion est mappé au type de revendication dans le fichier.  Pour plus d'informations sur l'ajout de descriptions de revendication, consultez [Ajouter une description de revendication](../operations/add-a-claim-description.md). Pour plus d'informations, consultez la section « Étape 1 : exporter les paramètres de service » de la rubrique « Préparer la migration du serveur de fédération AD FS 2.0 ».  
  
-   Dans la console Gestion AD FS, sélectionnez **Approbations de fournisseur de revendications**. Vous devez recréer manuellement chaque approbation de fournisseur de revendications à l'aide de l'**Assistant Ajout d'approbation de fournisseur de revendications**.  Utilisez la liste des approbations de fournisseur de revendications que vous avez exportées et enregistrées pendant la préparation de la migration AD FS. Dans le fichier, vous pouvez ignorer l'approbation de fournisseur de revendications dont l'identificateur est « AD AUTHORITY », car il s'agit de l'approbation de fournisseur de revendications « Active Directory » qui fait partie de la configuration AD FS par défaut.  Toutefois, vérifiez les règles de revendication personnalisées que vous avez éventuellement ajoutées à l'approbation Active Directory avant la migration. Pour plus d'informations sur la création d'approbations de fournisseur de revendications, consultez [Créer une approbation de fournisseur de revendications à l'aide de métadonnées de fédération](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-using-federation-metadata) ou [Créer une approbation de fournisseur de revendications manuellement](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-manually).  
  
-   Dans la console Gestion AD FS, sélectionnez **Approbations de partie de confiance**. Vous devez recréer manuellement chaque approbation de partie de confiance à l'aide de l'**Assistant Ajout d'approbation de partie de confiance**. Utilisez la liste des approbations de partie de confiance que vous avez exportées et enregistrées pendant la préparation de la migration AD FS. Pour plus d'informations sur la création d'approbations de partie de confiance, consultez [Créer une approbation de partie de confiance à l'aide de métadonnées de fédération](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-using-federation-metadata) ou [Créer une approbation de partie de confiance manuellement](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-manually). 

## <a name="next-steps"></a>Étapes suivantes
 [Préparer la migration du serveur AD FS 2.0 de fédération](prepare-to-migrate-ad-fs-fed-server.md)   
 [Préparer la migration du serveur Proxy pour AD FS 2.0 de fédération](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrer le serveur AD FS 2.0 de fédération](migrate-the-ad-fs-fed-server.md)   
 [Migrer le serveur Proxy AD FS 2.0 de fédération](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrer les agents web AD FS 1.1](migrate-the-ad-fs-web-agent.md)




-   
-    