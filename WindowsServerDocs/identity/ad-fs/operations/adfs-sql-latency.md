---
title: Fine de réglage SQL et de résoudre des problèmes de latence avec AD FS
description: Ce document explique comment régler avec précision SQL avec AD FS.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 06/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb699a1f92013f5657d2fbb48b203f96a5e5a5ba
ms.sourcegitcommit: 6b6c3601fb7493ab145ccff02db26d7123df9a3d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67322861"
---
# <a name="fine-tuning-sql-and-addressing-latency-issues-with-ad-fs"></a>Fine de réglage SQL et de résoudre des problèmes de latence avec AD FS
Dans une mise à jour pour [AD FS 2016](https://support.microsoft.com/help/4503294/windows-10-update-kb4503294) nous avons introduit les améliorations suivantes pour réduire entre la latence de la base de données. Une mise à jour à venir pour AD FS 2019 inclura ces améliorations.

## <a name="in-memory-cache-update-in-background-thread"></a>Mise à jour du cache en mémoire dans le thread d’arrière-plan 
Dans Always préalable sur les déploiements de disponibilité (AoA), latence existe pour toute opération de « lecture » comme le nœud principal peut se trouver dans un centre de données distinct. L’appel entre deux différents centres de données a entraîné la latence.  

Dans la dernière mise à jour AD FS, une réduction de la latence est ciblée via l’ajout d’un thread d’arrière-plan pour actualiser le cache de configuration AD FS et un paramètre pour définir l’actualisation de la période de temps. Le temps passé pour une recherche de base de données est considérablement réduit dans le thread de demande, comme les mises à jour du cache de base de données sont déplacées vers le thread d’arrière-plan.  

Lorsque le `backgroundCacheRefreshEnabled` est définie sur true, AD FS activera le thread d’arrière-plan exécuter des mises à jour du cache. La fréquence d’extraction de données à partir du cache peut être personnalisée pour une valeur d’heure en définissant `cacheRefreshIntervalSecs`. La valeur par défaut est définie à 300 secondes lorsque `backgroundCacheRefreshEnabled` est définie sur true. Une fois que le jeu de valeur de durée, AD FS commence l’actualisation de cache et lorsque la mise à jour est en cours, les anciennes données de cache continueront à utiliser.  

>[!NOTE]
> Les données du cache sont actualisées en dehors de la `cacheRefreshIntervalSecs` valeur si ADFS reçoit une notification de SQL ce qui signifie qu’une modification s’est produite dans la base de données. Cette notification déclenchera le cache pour être actualisé. 

### <a name="recommendations-for-setting-the-cache-refresh"></a>Recommandations pour la définition de l’actualisation du cache 
La valeur par défaut pour l’actualisation du cache est **cinq minutes**. Il est recommandé de lui attribuer **1 heure** pour réduire une actualisation des données inutiles par AD FS, car les données du cache sont actualisées si des modifications SQL se produisent.  

AD FS enregistre un rappel pour les modifications SQL et cas de changement, ADFS reçoit une notification. Via cette méthode, ADFS reçoit chaque nouvelle modification à partir de SQL dès qu’il se produit. 

Valeur d’actualisation en cas d’un problème de réseau que ce qui se traduit dans AD FS manquant de la notification SQL, AD FS sera actualisée à l’intervalle spécifié par le cache. Si les problèmes de connectivité sont suspectées entre AD FS et SQL, il est recommandé de définir la valeur d’actualisation de cache inférieure à 1 heure.  

### <a name="configuration-instructions"></a>Instructions de configuration 
Le fichier de configuration prend en charge plusieurs entrées de cache. Éléments répertoriés ci-dessous peuvent tous être configurés en fonction des besoins de votre organisation. 

L’exemple suivant permet l’actualisation du cache en arrière-plan et définit la période d’actualisation du cache à 1 800 secondes, ou 30 minutes. Cette opération doit être effectuée sur chaque nœud AD FS et le service ADFS doit être redémarré par la suite. Les modifications ne pas avoir un impact sur d’autres nœuds et le premier nœud de test avant d’apporter la modification dans tous les nœuds. 

  1. Accédez au fichier de configuration AD FS et sous la section « Microsoft.IdentityServer.Service », ajoutez la sous entrée :  
  
  - `backgroundCacheRefreshEnabled`  -Spécifie si la fonctionnalité de cache d’arrière-plan est activée. valeurs « true/false ».
  - `cacheRefreshIntervalSecs` -Valeur en secondes auquel ADFS actualise le cache. AD FS actualise le cache s’il existe toute modification dans SQL. AD FS recevra une notification de SQL et actualiser le cache.  
 
 >[!NOTE]
 > Toutes les entrées dans le fichier de configuration respectent la casse.  
 &lt;cache cacheRefreshIntervalSecs="1800" backgroundCacheRefreshEnabled="true" /&gt; 
 
Valeurs prises en charge et configurables supplémentaires : 

   - **maxRelyingPartyEntries** - Maximum nombre d’entrées de tiers par partie de confiance AD FS continuera en mémoire. Cette valeur est également utilisée par le cache d’autorisation oAuth application. S’il existe davantage d’autorisations application que RPs et si toutes les seront stockées en mémoire, cette valeur doit être le nombre d’autorisations de l’application. La valeur par défaut est 1000.
   - **maxIdentityProviderEntries** - ce est le nombre maximal de revendications AD FS conserve en mémoire les entrées du fournisseur. La valeur par défaut est 200. 
   - **maxClientEntries** -Ceci est le nombre maximal d’entrées de client OAuth AD FS conserve en mémoire. La valeur par défaut est 500. 
   - **maxClaimDescriptorEntries** - Maximum nombre d’entrées de descripteur de revendication AD FS conserve en mémoire. La valeur par défaut est 500. 
   - **maxNullEntries** -il est utilisé comme cache négatif. En cas d’AD FS recherche une entrée dans la base de données et il n’est pas trouvé, AD FS ajoute dans le cache négatif. Il s’agit de la taille maximale de ce cache. Il existe un cache négatif pour chaque type d’objet, il n’est pas un cache unique pour tous les objets. La valeur par défaut est 50,0000. 

## <a name="multiple-artifact-db-support-across-datacenters"></a>Prise en charge de plusieurs artefacts de base de données entre centres de données 
Pour les configurations précédentes de plusieurs centres de données, AD FS a uniquement pris en charge une seule base de données d’artefact, à l’origine de la latence de centre de données croisées center lors des appels de récupération.  

Pour réduire la latence de centre de données croisées, un administrateur AD FS peut désormais déployer plusieurs instances de base de données d’artefact et ensuite modifier le fichier de configuration d’un nœud AD FS pour pointer vers les instances de l’artefact autre base de données. La chaîne de connexion de base de données artefact peut être fournie dans le fichier de configuration permettant d’une base de données par nœud artefact. Si la chaîne de connexion n’est pas présente dans le fichier de configuration, le nœud revient à la conception précédente à utiliser la base de données d’artefact qui n’est présent dans la base de données de configuration.  
Les environnements hybrides sont également pris en charge avec cette configuration.  

### <a name="requirements"></a>Configuration requise : 
Avant de configurer la prise en charge de plusieurs artefacts de base de données, exécutez une mise à jour sur tous les nœuds et mettre à jour les fichiers binaires dans la mesure où plusieurs nœuds appels se produisent via cette fonctionnalité. 
  1. Générer un script de déploiement pour créer la base de données d’artefact : Pour déployer plusieurs instances de base de données d’artefact, un administrateur doit générer le script de déploiement SQL pour la base de données d’artefact. Dans le cadre de cette mise à jour, existant `Export-AdfsDeploymentSQLScript`applet de commande a été mis à jour prenne éventuellement dans un paramètre spécifiant qui base de données pour générer un script de déploiement SQL pour AD FS. 
 
 Par exemple, pour générer le script de déploiement pour simplement la base de données d’artefact, spécifiez la `-DatabaseType` paramètre et passez la valeur « Artefact ». Le paramètre facultatif `-DatabaseType` paramètre spécifie le type de base de données AD FS et peut être défini sur : Tous (par défaut), artefact ou Configuration. Si aucun `-DatabaseType` paramètre est spécifié, le script configurera l’artefact et la Configuration de scripts.  

   ```PowerShell
   PS C:\> Export-AdfsDeploymentSQLScript -DestinationFolder <script folder where scripts will be created> -ServiceAccountName <domain\serviceaccount> -DatabaseType "Artifact" 
   ```
Le script généré doit être exécuté sur l’ordinateur SQL pour créer des bases de données requises et donner le compte de service AD FS, les autorisations d’administrateur système SQL pour ces bases de données.

 2. Créer la base de données artefact en utilisant le script de déploiement. Copier les scripts de déploiement qui vient d’être générés CreateDB.sql et SetPermissions.sql sur la machine SQL server et les exécuter pour créer la base de données locale de l’artefact. 
 
 3. Modifiez le fichier de Configuration pour ajouter la connexion de base de données artefact. 
 Accédez au fichier de configuration du nœud AD FS puis, sous la section « Microsoft.IdentityServer.Service », ajoutez un point d’entrée à la ArtifactDB nouvellement configuré. 

 >[!NOTE] 
 > artifactStore et connectionString sont les valeurs respectent la casse. Assurez-vous qu’ils sont configurés correctement. &lt;artifactStore connectionString="Data Source=.\SQLInstance;Integrated Security=True;Initial Catalog=AdfsArtifactStore" /&gt; 
>
>Utilisez une valeur de Source de données qui correspond à votre connexion sql.



 4. Redémarrez le service AD FS pour que les modifications entrent en vigueur. 
 
 >[!NOTE] 
 > Il n’est pas recommandé d’utiliser la réplication SQL ou la synchronisation entre les bases de données d’artefact. Il est recommandé de configurer une base de données d’artefact par centre de données. 
 
### <a name="cross-datacenter-failover-and-database-recovery"></a>Cross-récupération de basculement et la base de données du centre de données  
Il est recommandé de créer des bases de données de basculement artefact sur le même centre de données en tant que la base de données master d’artefact. Si un basculement se produit, il n’y aura aucun augmentation de latence. Bases de données artefact de basculement entre centres de données n’est pas recommandée. Ce qui suit décrit en détail comment les appels pour OAuth, SAML, ESL et le jeton relire la fonction de détection avec plusieurs bases de données artefact. 
 - **OAuth et SAML** 

   Pour les demandes d’artefact OAuth et SAML, le nœud créera l’artefact dans la base de données de l’artefact présent dans le fichier de configuration. Si le fichier de configuration ne contient pas d’une connexion de base de données d’artefact, il utilisera l’artefact commune de base de données. En cas de la requête suivante pour extraire l’artefact vers un autre nœud, l’autre nœud fera l’API rest au 1er nœud pour extraire l’artefact à partir de la base de données de l’artefact. Cela est nécessaire comme des nœuds différents peuvent avoir des bases de données différents artefacts et les nœuds ne connaissent pas à ce sujet. Si le nœud 1er est arrêté, la résolution d’artefacts échouera. En raison de cette conception, il n’est pas nécessaire de répliquer la base de données d’artefact dans différents centres de données. Si un centre de données entier est arrêté, très probablement le nœud qui a créé l’artefact est également vers le bas, ce qui signifie que cet artefact ne peut plus être résolu.  

 - **Verrouillage extranet** 

    L’artefact référencé dans le fichier de configuration va être utilisée pour les données de verrouillage Extranet. Toutefois, pour la fonctionnalité ESL, AD FS choisit un maître qui écrit les données dans la base de données de l’artefact. Tous les nœuds d’appeler une API REST pour le nœud principal pour obtenir et définir les dernières informations sur chaque utilisateur. Si plusieurs artefact DB est en cours d’utilisation, l’administrateur doit sélectionner un nœud principal pour chaque artefact DB ou le centre de données. 

    Pour sélectionner un nœud à être maître ESL, accédez au fichier de configuration du nœud AD FS puis, sous la section « Microsoft.IdentityServer.Service », ajoutez le code suivant :       
    
    Sur le maître ajoutez suivant l’entrée. Notez que toutes les trois clés respectent la casse. 

    &lt;useractivityfarmrole masterFQDN = [nom de domaine complet du principal sélectionné] isMaster = « true » /&gt;
    
    Sur les autres nœuds ajoutez suivant entrée :

   &lt;useractivityfarmrole masterFQDN = [nom de domaine complet du principal sélectionné] isMaster = « false » /&gt;
 
    >[!NOTE] 
    >Étant donné que plusieurs bases de données artefact ne pas synchroniser les données, les valeurs ESL ne seront pas synchronisés entre les artefacts des bases de données.
    Un utilisateur peut éventuellement rencontrer un autre centre de données pour une demande, ce qui rend donc le ExtranetLockoutThreshold dépend du nombre de bases de données artefact, ExtranetLockoutThreshold * nombre d’artefact des bases de données. 
 
  - **Détection de relecture de jetons** 
    
    Les données de détection de relecture de jetons sont toujours appelées à partir de la base de données centrale de l’artefact. AD FS enregistre le jeton à partir de l’approbation de fournisseur de revendications, en garantissant que le même jeton ne peut pas être relu. Si une personne malveillante tente de relire le même jeton, AD FS vérifie si le jeton existe dans la base de données d’artefact. Si le jeton est présent, la demande sera rejetée. La base de données centrale de l’artefact est utilisé pour la sécurité, dans la mesure où les données de base de données artefact ne sont pas répliquées, un intrus peut envoyer la demande à un autre centre de données et relire un jeton. Créer des copies en lecture seule supplémentaires de la ArtifactDB empêchera pas la latence entre des centres de données dans ce scénario, uniquement la base de données artefact centrale est utilisé.    
 
 