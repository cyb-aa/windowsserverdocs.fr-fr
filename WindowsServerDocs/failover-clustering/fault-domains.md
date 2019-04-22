---
ms.assetid: 56fc7f80-9558-467e-a6e9-a04c9abbee33
title: Reconnaissance des domaines d'erreur
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-failover-clustering
ms.topic: article
author: cosmosdarwin
ms.date: 09/16/2016
ms.openlocfilehash: f5c64bb8f8b7d4b8d13c76c4e94cfcf52ee32c30
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821470"
---
# <a name="fault-domain-awareness-in-windows-server-2016"></a>Reconnaissance des domaines d’erreur dans Windows Server 2016

> S’applique à : Windows Server 2016

Le clustering de basculement permet à plusieurs serveurs de travailler ensemble pour fournir une haute disponibilité, autrement dit, pour fournir une tolérance de panne aux nœuds. Mais les entreprises d’aujourd'hui exigent une disponibilité toujours plus grande à partir de leur infrastructure. Pour obtenir un temps d’activité de type cloud, une protection contre des circonstances même très improbables comme une rupture de châssis, un dysfonctionnement de rack ou une catastrophe naturelle doit être mise en place. C’est pourquoi le Clustering de basculement dans Windows Server 2016 introduit des châssis en rack et site une tolérance de panne ainsi.

Les domaines d’erreur et la tolérance de panne sont des concepts étroitement liés. Un domaine d’erreur est un ensemble de composants matériels qui partagent un point de défaillance unique. Pour obtenir une tolérance de panne à un certain niveau, vous avez besoin de plusieurs domaines d’erreur à ce niveau. Par exemple, pour obtenir une tolérance de panne au niveau des racks, vos serveurs et vos données doivent être distribués sur plusieurs racks.

Cette brève vidéo présente une vue d’ensemble des domaines d’erreur dans Windows Server 2016 :  
[![Cliquez sur cette image pour regarder une vue d’ensemble des domaines d’erreur dans Windows Server 2016](media/Fault-Domains-in-Windows-Server-2016/Part-1-Fault-Domains-Overview.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-1-Overview)

## <a name="benefits"></a>Avantages
- **Espaces de stockage, y compris les espaces de stockage Direct, utilise des domaines d’erreur pour optimiser la sécurité des données.**  
    Du point de vue conceptuel, la résilience dans les espaces de stockage est similaire à la technologie RAID distribuée et à définition logicielle. Plusieurs copies de toutes les données restent synchronisées, et si le matériel tombe en panne et qu’une copie est perdue, les autres sont recopiées pour restaurer la résilience. Pour obtenir la meilleure résilience possible, les copies doivent être conservées dans des domaines d’erreur distincts.

- **Le [Service d’intégrité](health-service-overview.md) utilise des domaines pour fournir des alertes plus utiles d’erreur.**  
    Chaque domaine d’erreur peut être associé à des métadonnées d’emplacement, automatiquement incluses dans les alertes ultérieures. Ces descripteurs peuvent aider le personnel d’exploitation et de maintenance et réduire les erreurs en levant les ambiguïtés liées au matériel.  

- **Le clustering étendu utilise des domaines d’erreur pour l’affinité de stockage.** Le clustering étendu permet aux serveurs éloignés de rejoindre un cluster commun. Pour des performances optimales, les applications ou machines virtuelles doivent être exécutées sur des serveurs situés à proximité de ceux qui fournissent leur stockage. Reconnaissance des domaines d’erreur permet cette affinité de stockage.   

## <a name="levels-of-fault-domains"></a>Niveaux de domaines d’erreur  
Il existe quatre niveaux canoniques de domaines d’erreur : site, rack, châssis et nœud. Les nœuds sont découverts automatiquement. Chaque niveau supplémentaire est facultatif. Par exemple, si votre déploiement n’utilise pas de serveurs lames, le niveau du châssis n’a peut-être pas de sens pour vous.  

![Diagramme des différents niveaux de domaines d’erreur](media/Fault-Domains-in-Windows-Server-2016/levels-of-fault-domains.png)

## <a name="usage"></a>Utilisation  
Vous pouvez utiliser PowerShell ou le balisage XML pour spécifier les domaines d’erreur. Les deux approches sont équivalentes et fournissent des fonctionnalités complètes.

>[!IMPORTANT]
> Spécifiez les domaines d’erreur avant d’activer les espaces de stockage direct, si possible. Ainsi, la configuration automatique est activée pour préparer le pool, les niveaux et les paramètres tels que la résilience et le nombre de colonnes, pour une tolérance de panne au niveau du châssis ou rack. Une fois le pool et les volumes créés, les données ne se déplacement pas rétroactivement en réponse aux modifications apportées à la topologie des domaines d’erreur. Pour déplacer des nœuds entre des châssis ou racks après avoir activé des espaces de stockage direct, vous devez tout d’abord supprimer le nœud et ses lecteurs du pool à l’aide de `Remove-ClusterNode -CleanUpDisks`.

### <a name="defining-fault-domains-with-powershell"></a>Définition des domaines d’erreur avec PowerShell
Windows Server 2016 introduit des applets de commande suivantes pour travailler avec des domaines d’erreur :
* `Get-ClusterFaultDomain`
* `Set-ClusterFaultDomain`
* `New-ClusterFaultDomain` 
* `Remove-ClusterFaultDomain`

Cette courte vidéo montre l’utilisation de ces applets de commande.
[![Cliquez sur cette image pour regarder une courte vidéo sur l’utilisation des applets de commande de domaine d’erreur de Cluster](media/Fault-Domains-in-Windows-Server-2016/Part-2-Using-PowerShell.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-2-Using-PowerShell)

Utilisez `Get-ClusterFaultDomain` pour afficher la topologie des domaines d’erreur actuel. Celle-ci répertorie tous les nœuds du cluster, ainsi que tous les châssis, racks ou sites que vous avez créés. Vous pouvez les filtrer à l’aide de paramètres tels que **-Type** ou **-Name**, mais ceux-ci ne sont pas obligatoires.

```PowerShell
Get-ClusterFaultDomain
Get-ClusterFaultDomain -Type Rack
Get-ClusterFaultDomain -Name "server01.contoso.com"
```

Utilisez `New-ClusterFaultDomain` pour créer des châssis, racks ou sites. Le `-Type` et `-Name` sont des paramètres obligatoires. Les valeurs possibles pour `-Type` sont `Chassis`, `Rack`, et `Site`. Le `-Name` peut être n’importe quelle chaîne. (Pour `Node` domaines d’erreur de type, le nom doivent être le nom réel du nœud, en tant que jeu automatiquement).

```PowerShell
New-ClusterFaultDomain -Type Chassis -Name "Chassis 007"
New-ClusterFaultDomain -Type Rack -Name "Rack A"
New-ClusterFaultDomain -Type Site -Name "Shanghai"
```

> [!IMPORTANT]  
> Windows Server ne peut pas et ne vérifie pas que les domaines d’erreur que vous créez correspondent à quoi que ce soit dans le monde réel, physique. (Cela peut paraître évident, mais il est important de comprendre). Si, dans la configuration physique, vos nœuds sont tous dans un seul rack, alors la création de deux domaines d’erreur `-Type Rack` dans le logiciel ne fournit pas comme par magie une tolérance de panne au niveau du rack. Il vous incombe de veiller à ce que la topologie que vous créez à l’aide de ces applets de commande corresponde à la disposition réelle de votre matériel.

Utilisez `Set-ClusterFaultDomain` pour déplacer un domaine d’erreur dans un autre. Les termes « parent » et « enfant » sont couramment utilisés pour décrire cette relation d’imbrication. Le `-Name` et `-Parent` sont des paramètres obligatoires. Dans `-Name`, indiquez le nom du domaine d’erreur qui se déplace ; dans `-Parent`, indiquez le nom de la destination. Pour déplacer plusieurs domaines d’erreur à la fois, dressez la liste de leurs noms.

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent "Rack A"
Set-ClusterFaultDomain -Name "Rack A", "Rack B", "Rack C", "Rack D" -Parent "Shanghai"
```

> [!IMPORTANT]  
> Quand des domaines d’erreur se déplacent, leurs enfants se déplacent avec eux. Dans l’exemple ci-dessus, si le rack A est le parent de server01.contoso.com, ce dernier n’a pas besoin de se déplacer séparément vers le site de Shanghai : il y est déjà puisque son parent y est, comme dans la configuration physique.

Vous pouvez voir les relations parent-enfant dans la sortie de `Get-ClusterFaultDomain`, dans le `ParentName` et `ChildrenNames` colonnes.

Vous pouvez également utiliser `Set-ClusterFaultDomain` pour modifier certaines autres propriétés de domaines d’erreur. Par exemple, vous pouvez fournir facultatif `-Location` ou `-Description` métadonnées pour n’importe quel domaine d’erreur. Si ces informations sont fournies, elles sont incluses dans les alertes matérielles provenant du service de contrôle d’intégrité. Vous pouvez également renommer les domaines d’erreur à l’aide de le `-NewName` paramètre. Ne renommez pas `Node` les domaines d’erreur de type.

```PowerShell
Set-ClusterFaultDomain -Name "Rack A" -Location "Building 34, Room 4010"
Set-ClusterFaultDomain -Type Node -Description "Contoso XYZ Server"
Set-ClusterFaultDomain -Name "Shanghai" -NewName "China Region"
```

Utilisez `Remove-ClusterFaultDomain` pour supprimer les châssis, racks ou sites que vous avez créés. Le paramètre `-Name` est obligatoire. Vous ne peut pas supprimer un domaine d’erreur qui contient des enfants : tout d’abord, supprimez les enfants ou déplacez-les à l’extérieur à l’aide de `Set-ClusterFaultDomain`. Pour déplacer un domaine d’erreur en dehors de tous les autres domaines d’erreur, définissez son `-Parent` à la chaîne vide ( » »). Vous ne pouvez pas supprimer `Node` tapez des domaines d’erreur. Pour supprimer plusieurs domaines d’erreur à la fois, dressez la liste de leurs noms.

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent ""
Remove-ClusterFaultDomain -Name "Rack A"
```

### <a name="defining-fault-domains-with-xml-markup"></a>Définition des domaines d’erreur avec le balisage XML
Vous pouvez spécifier des domaines d’erreur en utilisant une syntaxe inspirée du langage XML. Nous vous recommandons d’utiliser votre éditeur de texte favori, tel que Visual Studio Code (disponible gratuitement *[ici](https://code.visualstudio.com/)*) ou le Bloc-notes, pour créer un document XML que vous pouvez enregistrer et réutiliser.  

Cette courte vidéo montre l’utilisation du balisage XML pour spécifier des domaines d’erreur.

[![Cliquez sur cette image pour regarder une courte vidéo sur l’utilisation de XML pour spécifier les domaines d’erreur](media/Fault-Domains-in-Windows-Server-2016/Part-3-Using-XML-Markup.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-3-Using-XML)

Dans PowerShell, exécutez l’applet de commande suivante : `Get-ClusterFaultDomainXML`. Celle-ci retourne la spécification de domaine d’erreur actuelle du cluster, au format XML. Cela reflète chaque découverts `<Node>`, encapsulé dans ouvrantes et fermantes `<Topology>` balises.  

Exécutez la commande suivante pour enregistrer cette sortie dans un fichier.  

```PowerShell
Get-ClusterFaultDomainXML | Out-File <Path>  
```

Ouvrez le fichier, puis ajoutez `<Site>`, `<Rack>`, et `<Chassis>` balises pour spécifier la façon dont ces nœuds sont répartis entre les sites, racks et châssis. Chaque balise doit être identifiée par un **nom** unique. Pour les nœuds, vous devez conserver le nom du nœud tel qu’il est renseigné par défaut.  

> [!IMPORTANT]  
> Bien que toutes les balises supplémentaires soient facultatives, elles doivent respecter la hiérarchie Site &gt; Rack &gt; Châssis &gt; transitive et être correctement fermées.  
En plus du nom, de forme libre `Location="..."` et `Description="..."` descripteurs peuvent être ajoutés à n’importe quelle balise.  

#### <a name="example-two-sites-one-rack-each"></a>Exemple : Deux sites, un seul rack chacun  

```XML
<Topology>  
  <Site Name="SEA" Location="Contoso HQ, 123 Example St, Room 4010, Seattle">  
    <Rack Name="A01" Location="Aisle A, Rack 01">  
      <Node Name="Server01" Location="Rack Unit 33" />  
      <Node Name="Server02" Location="Rack Unit 35" />  
      <Node Name="Server03" Location="Rack Unit 37" />  
    </Rack>  
  </Site>  
  <Site Name="NYC" Location="Regional Datacenter, 456 Example Ave, New York City">  
    <Rack Name="B07" Location="Aisle B, Rack 07">  
      <Node Name="Server04" Location="Rack Unit 20" />  
      <Node Name="Server05" Location="Rack Unit 22" />  
      <Node Name="Server06" Location="Rack Unit 24" />  
    </Rack>  
  </Site>  
</Topology> 
``` 

#### <a name="example-two-chassis-blade-servers"></a>Exemple : deux châssis, serveurs lames  
```XML
<Topology>  
  <Rack Name="A01" Location="Contoso HQ, Room 4010, Aisle A, Rack 01">  
    <Chassis Name="Chassis01" Location="Rack Unit 2 (Upper)" >  
      <Node Name="Server01" Location="Left" />  
      <Node Name="Server02" Location="Right" />  
    </Chassis>  
    <Chassis Name="Chassis02" Location="Rack Unit 6 (Lower)" >  
      <Node Name="Server03" Location="Left" />  
      <Node Name="Server04" Location="Right" />  
    </Chassis>  
  </Rack>  
</Topology>  
```

Pour définir la nouvelle spécification de domaine d’erreur, enregistrez votre fichier XML et exécutez la commande suivante dans PowerShell.  

```PowerShell
$xml = Get-Content <Path> | Out-String  
Set-ClusterFaultDomainXML -XML $xml
```

Ce guide présente simplement deux exemples, mais la `<Site>`, `<Rack>`, `<Chassis>`, et `<Node>` balises peuvent être mélangés et mis en correspondance de nombreuses autres manières afin de refléter la topologie physique de votre déploiement, quelle qu’elle soit. Nous espérons que ces exemples illustrent la flexibilité de ces balises et la valeur des descripteurs d’emplacement libre pour les distinguer.  

### <a name="optional-location-and-description-metadata"></a>Facultatif : Emplacement et la description des métadonnées

Vous pouvez fournir facultatif **emplacement** ou **Description** métadonnées pour n’importe quel domaine d’erreur. Si ces informations sont fournies, elles sont incluses dans les alertes matérielles provenant du service de contrôle d’intégrité. Cette courte vidéo montre la valeur de l’ajout de ces descripteurs.

[![Cliquez pour afficher une courte vidéo de démonstration de la valeur de l’ajout de descripteurs d’emplacement aux domaines d’erreur](media/Fault-Domains-in-Windows-Server-2016/part-4-location-description.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-4-Location-Description)

## <a name="see-also"></a>Voir aussi  
-   [Windows Server 2016](../get-started/windows-server-2016.md)  
-   [Espaces de stockage Direct dans Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md) 
