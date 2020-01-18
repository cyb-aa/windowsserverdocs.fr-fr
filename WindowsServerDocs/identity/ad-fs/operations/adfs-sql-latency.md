---
title: Optimisation de SQL et résolution des problèmes de latence avec AD FS
description: Ce document explique comment ajuster SQL avec AD FS.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 06/20/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e5e90119066285ae8e04b392a13ab1a38488f5ee
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265751"
---
# <a name="fine-tuning-sql-and-addressing-latency-issues-with-ad-fs"></a>Optimisation de SQL et résolution des problèmes de latence avec AD FS
Dans une mise à jour de [AD FS 2016](https://support.microsoft.com/help/4503294/windows-10-update-kb4503294) , nous avons introduit les améliorations suivantes pour réduire la latence de la base de données croisée. Une prochaine mise à jour de AD FS 2019 comprendra ces améliorations.

## <a name="in-memory-cache-update-in-background-thread"></a>Mise à jour du cache en mémoire dans le thread d’arrière-plan 
Dans les déploiements de AoA (Always on) antérieurs, la latence existait pour toute opération de « lecture », car le nœud principal était peut-être situé dans un centre de connaissances distinct. L’appel entre deux centres de centres différents a entraîné une latence.  

Dans la dernière mise à jour de AD FS, une réduction de la latence est ciblée via l’ajout d’un thread d’arrière-plan pour actualiser le cache de configuration AD FS et un paramètre pour définir la période d’actualisation. Le temps passé pour la recherche d’une base de données est considérablement réduit dans le thread de demande, car les mises à jour du cache de la base de données sont déplacées dans le thread d’arrière-plan.  

Lorsque la `backgroundCacheRefreshEnabled` a la valeur true, AD FS permet au thread d’arrière-plan d’exécuter les mises à jour du cache. La fréquence d’extraction des données à partir du cache peut être personnalisée en valeur temporelle en définissant `cacheRefreshIntervalSecs`. La valeur par défaut est définie sur 300 secondes lorsque `backgroundCacheRefreshEnabled` a la valeur true. Après la durée définie, AD FS commence à actualiser son cache et Pendant que la mise à jour est en cours, les anciennes données du cache continuent à être utilisées.  

Lorsque AD FS reçoit une demande pour une application, AD FS récupère l’application à partir de SQL et l’ajoute au cache. À la valeur `cacheRefreshIntervalSecs`, l’application dans le cache est actualisée à l’aide du thread d’arrière-plan. Lorsqu’une entrée existe dans le cache, les demandes entrantes utilisent le cache pendant que l’actualisation en arrière-plan est en cours. Si une entrée n’est pas accessible pour 5 * `cacheRefreshIntervalSecs`, elle est supprimée du cache. L’entrée la plus ancienne peut également être supprimée du cache une fois que la valeur `maxRelyingPartyEntries` configurable est atteinte.

>[!NOTE]
> Les données du cache sont actualisées en dehors de la valeur `cacheRefreshIntervalSecs` si ADFS reçoit une notification de la part de SQL signifiant qu’une modification a été apportée dans la base de données. Cette notification déclenche l’actualisation du cache. 

### <a name="recommendations-for-setting-the-cache-refresh"></a>Recommandations pour la définition de l’actualisation du cache 
La valeur par défaut de l’actualisation du cache est de **cinq minutes**. Il est recommandé de définir la valeur sur **1 heure** pour réduire une actualisation des données inutile par AD FS, car les données du cache sont actualisées si des modifications SQL se produisent.  

AD FS inscrit un rappel pour les modifications SQL et, en cas de modification, ADFS reçoit une notification. À l’aide de cette méthode, ADFS reçoit chaque nouvelle modification de SQL dès qu’elle se produit. 

En cas de problème réseau entraînant l’absence de notification SQL dans AD FS, AD FS est actualisée à l’intervalle spécifié par la valeur d’actualisation du cache. Si des problèmes de connectivité sont suspectés entre AD FS et SQL, il est recommandé de définir la valeur d’actualisation du cache sur une valeur inférieure à 1 heure.  

### <a name="configuration-instructions"></a>Instructions de configuration 
Le fichier de configuration prend en charge plusieurs entrées de cache. La liste ci-dessous peut être configurée en fonction des besoins de votre organisation. 

L’exemple suivant active l’actualisation du cache en arrière-plan et définit la période d’actualisation du cache sur 1800 secondes, ou 30 minutes. Cette opération doit être effectuée sur chaque nœud ADFS et le service ADFS doit être redémarré par la suite. Les modifications n’affectent pas les autres nœuds et testent le premier nœud avant d’effectuer la modification dans tous les nœuds. 

  1. Accédez au fichier de configuration AD FS et, sous la section « Microsoft. IdentityServer. service », ajoutez l’entrée ci-dessous :  
  
  - `backgroundCacheRefreshEnabled` : spécifie si la fonctionnalité de cache en arrière-plan est activée. valeurs « true/false ».
  - `cacheRefreshIntervalSecs`-valeur en secondes à laquelle ADFS actualise le cache. AD FS actualise le cache en cas de modification dans SQL. AD FS recevra une notification SQL et actualisera le cache.  
 
 >[!NOTE]
 > Toutes les entrées du fichier de configuration respectent la casse.  
 &lt;cache cacheRefreshIntervalSecs = "1800" backgroundCacheRefreshEnabled = "true"/&gt; 
 
Valeurs configurables supplémentaires prises en charge : 

   - **maxRelyingPartyEntries** : nombre maximal d’entrées de partie de confiance qui AD FS conservées en mémoire. Cette valeur est également utilisée par le cache des autorisations de l’application oAuth. S’il y a plus d’autorisations d’application que RPs et si toutes sont stockées en mémoire, cette valeur doit correspondre au nombre d’autorisations d’application. La valeur par défaut est 1000.
   - **maxIdentityProviderEntries** : nombre maximal d’entrées de fournisseur de revendications AD FS conserver en mémoire. La valeur par défaut est 200. 
   - **maxClientEntries** : nombre maximal d’entrées du client OAuth AD FS conserver en mémoire. La valeur par défaut est 500. 
   - **maxClaimDescriptorEntries** : nombre maximal d’entrées du descripteur de revendication AD FS conserver en mémoire. La valeur par défaut est 500. 
   - **maxNullEntries** : utilisé comme cache négatif. Lorsque AD FS recherche une entrée dans la base de données et qu’elle est introuvable, AD FS ajoute au cache négatif. Il s’agit de la taille maximale de ce cache. Il y a un cache négatif pour chaque type d’objet, il ne s’agit pas d’un cache unique pour tous les objets. La valeur par défaut est 50, 0000. 

## <a name="multiple-artifact-db-support-across-datacenters"></a>Prise en charge de plusieurs bases de la base de BD sur plusieurs centres de 
Pour les configurations antérieures de plusieurs centres de données, AD FS n’a pris en charge qu’une seule base de données d’artefacts, provoquant la latence entre centres de données pendant les appels de récupération.  

Pour réduire la latence entre les centres de l’entreprise, un administrateur AD FS peut désormais déployer plusieurs instances de base de base de l’artefact, puis modifier le fichier de configuration d’un nœud de AD FS pour qu’il pointe vers différentes instances de base de base de l’artefact. La chaîne de connexion de la base de données des artefacts peut être fournie dans le fichier de configuration, autorisant une base de données d’artefacts par nœud. Si la chaîne de connexion n’est pas présente dans le fichier de configuration, le nœud revient à la conception précédente pour utiliser la base de données d’artefacts qui est présente dans la base de données de configuration.  
Les environnements hybrides sont également pris en charge avec cette configuration.  

### <a name="requirements"></a>Configuration requise : 
Avant de configurer la prise en charge de plusieurs bases de données d’artefacts, exécutez une mise à jour sur tous les nœuds et mettez à jour les binaires, car les appels à plusieurs nœuds se produisent par le biais de cette fonctionnalité. 
  1. Générer un script de déploiement pour créer la base de connaissances d’artefacts : pour déployer plusieurs instances de base de connaissances d’artefacts, un administrateur doit générer le script de déploiement SQL pour la base de connaissances de l’artefact. Dans le cadre de cette mise à jour, l’applet de commande `Export-AdfsDeploymentSQLScript`existante a été mise à jour pour éventuellement prendre un paramètre spécifiant la base de données AD FS pour laquelle générer un script de déploiement SQL. 
 
 Par exemple, pour générer le script de déploiement uniquement pour la base de l’artefact, spécifiez le paramètre `-DatabaseType` et transmettez la valeur « artefact ». Le paramètre `-DatabaseType` facultatif spécifie le type de base de données AD FS et peut avoir la valeur : All (valeur par défaut), artefact ou configuration. Si aucun paramètre `-DatabaseType` n’est spécifié, le script configure à la fois les scripts d’artefact et de configuration.  

   ```PowerShell
   PS C:\> Export-AdfsDeploymentSQLScript -DestinationFolder <script folder where scripts will be created> -ServiceAccountName <domain\serviceaccount> -DatabaseType "Artifact" 
   ```
Le script généré doit être exécuté sur l’ordinateur SQL pour créer les bases de données requises et octroyer au compte de service AD FS, les autorisations d’administrateur système SQL à ces bases de données.

 2. Créez la base de la base de l’artefact à l’aide du script de déploiement. Copiez les scripts de déploiement CreateDB. SQL et SetPermissions. SQL que vous venez de générer sur l’ordinateur SQL Server, puis exécutez-les pour créer la base de la base de l’artefact local. 
 
 3. Modifiez le fichier de configuration pour ajouter la connexion de base de base de l’artefact. 
 Accédez au fichier de configuration du nœud AD FS et, sous la section « Microsoft. IdentityServer. service », ajoutez un point d’entrée au ArtifactDB nouvellement configuré. 

 >[!NOTE] 
 > artifactStore et connectionString sont des valeurs sensibles à la casse. Assurez-vous qu’elles sont correctement configurées. &lt;artifactStore connectionString = "Data source = .\SQLInstance ; Integrated Security = True ; initial catalog = AdfsArtifactStore"/&gt; 
>
>Utilisez une valeur de source de données qui correspond à votre connexion SQL.



 4. Redémarrez le service AD FS pour que les modifications prennent effet. 
 
 >[!NOTE] 
 > Il n’est pas recommandé d’utiliser la réplication SQL ou la synchronisation entre les bases de données d’artefacts. La recommandation consiste à configurer une base de données d’artefacts par centre de données. 
 
### <a name="cross-datacenter-failover-and-database-recovery"></a>Basculement entre centres de données et récupération de base de données  
Il est recommandé de créer des bases de données d’artefacts de basculement sur le même centre de données que la base de données d’artefacts principale. En cas de basculement, il n’y aura aucune augmentation de la latence. Les bases de données d’artefacts de basculement dans les centres de données ne sont pas recommandées. Les informations suivantes expliquent comment appelle la fonction OAuth, SAML, ESL et la détection de relecture de jetons avec plusieurs bases de données d’artefact. 
 - **OAuth et SAML** 

   Pour les demandes d’artefacts OAuth et SAML, le nœud créera l’artefact dans la base de la base de l’artefact présent dans le fichier de configuration. Si le fichier de configuration ne contient pas de connexion à une base de données d’artefacts, il utilisera la base de données des artefacts communs. Lorsque la requête suivante pour récupérer l’artefact passe à un autre nœud, l’autre nœud fait de l’API REST le premier nœud pour extraire l’artefact de la base de la base de la base de l’artefact. Cela est nécessaire, car des nœuds différents peuvent avoir des bases de données d’artefacts différentes et les nœuds ne le savent pas. Si le premier nœud est en panne, la résolution de l’artefact échoue. En raison de cette conception, il n’est pas nécessaire de répliquer la base de la base de l’artefact dans différents centres de. Si l’un des centres de l’ensemble est défaillant, le nœud qui a créé l’artefact est également inactif, ce qui signifie que l’artefact ne peut plus être résolu.  

 - **Verrouillage extranet** 

    La base de données d’artefacts référencée dans le fichier de configuration est utilisée pour les données de verrouillage extranet. Toutefois, pour la fonctionnalité ESL, AD FS choisit un maître qui écrit les données dans la base de données des artefacts. Tous les nœuds effectuent un appel d’API REST au nœud principal pour obtenir et définir les informations les plus récentes sur chaque utilisateur. Si plusieurs bases de référence d’artefact sont en cours d’utilisation, l’administrateur doit sélectionner un nœud maître pour chaque base de référence d’artefact ou centre de référence. 

    Pour sélectionner un nœud comme maître de ESL, accédez au fichier de configuration du nœud ADFS et, sous la section « Microsoft. IdentityServer. service », ajoutez ce qui suit :       
    
    Sur le maître, ajoutez l’entrée suivante. Notez que les trois clés respectent la casse. 

    &lt;useractivityfarmrole masterFQDN = [nom de domaine complet du serveur principal sélectionné] isMaster = "true"/&gt;
    
    Sur les autres nœuds, ajoutez l’entrée suivante :

   &lt;useractivityfarmrole masterFQDN = [nom de domaine complet du serveur principal sélectionné] isMaster = « false »/&gt;
 
    >[!NOTE] 
    >Étant donné que plusieurs bases de données d’artefact ne synchronisent pas les données, les valeurs ESL ne sont pas synchronisées entre les bases de données d’artefacts.
    Un utilisateur peut potentiellement atteindre un autre centre de charge pour une demande, ce qui rend le ExtranetLockoutThreshold dépendant du nombre de bases de chaînes d’artefacts, ExtranetLockoutThreshold * du nombre de bases de chaînes d’artefacts. 
 
  - **Détection de relecture de jetons** 
    
    Les données de détection de relecture de jetons sont toujours appelées à partir de la base de données d’artefacts centrale. AD FS enregistre le jeton à partir de l’approbation de fournisseur de revendications, garantissant ainsi que le même jeton ne peut pas être relu. Si une personne malveillante tente de relire le même jeton, AD FS vérifie si le jeton existe dans la base de la base de l’artefact. Si le jeton est présent, la demande est rejetée. La base de données d’artefacts centrale est utilisée pour la sécurité, car les données de la base de données d’artefacts ne sont pas répliquées, une personne malveillante peut envoyer la demande à un autre centre de données et relire un jeton. La création de copies en lecture seule supplémentaires du ArtifactDB n’empêchera pas la latence entre les centres de données dans ce scénario, car seule la base de données d’artefacts centrale est utilisée.    
 
 
