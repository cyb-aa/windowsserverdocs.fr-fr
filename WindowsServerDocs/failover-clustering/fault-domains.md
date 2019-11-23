---
ms.assetid: 56fc7f80-9558-467e-a6e9-a04c9abbee33
title: Reconnaissance des domaines d'erreur
ms.prod: windows-server
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-failover-clustering
ms.topic: article
author: cosmosdarwin
ms.date: 09/16/2016
ms.openlocfilehash: 439f898b7c96ecc3d2f380509fe86d528aa737c5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361141"
---
# <a name="fault-domain-awareness"></a>Reconnaissance des domaines d'erreur

> S’applique à : Windows Server 2019 et Windows Server 2016

Le clustering de basculement permet à plusieurs serveurs de travailler ensemble pour fournir une haute disponibilité, autrement dit, pour fournir une tolérance de panne aux nœuds. Mais aujourd’hui, les entreprises exigent une disponibilité optimale de leur infrastructure. Pour obtenir un temps d’activité de type cloud, une protection contre des circonstances même très improbables comme une rupture de châssis, un dysfonctionnement de rack ou une catastrophe naturelle doit être mise en place. C’est la raison pour laquelle le clustering de basculement dans Windows Server 2016 a également introduit la tolérance de panne du châssis, du rack et du site.

## <a name="fault-domain-awareness"></a>Reconnaissance des domaines d'erreur

Les domaines d’erreur et la tolérance de panne sont des concepts étroitement liés. Un domaine d’erreur est un ensemble de composants matériels qui partagent un point de défaillance unique. Pour obtenir une tolérance de panne à un certain niveau, vous avez besoin de plusieurs domaines d’erreur à ce niveau. Par exemple, pour obtenir une tolérance de panne au niveau des racks, vos serveurs et vos données doivent être distribués sur plusieurs racks.

Cette brève vidéo présente une vue d’ensemble des domaines d’erreur dans Windows Server 2016 :  
[![cliquez sur cette image pour visionner une vue d’ensemble des domaines d’erreur dans Windows Server 2016](media/Fault-Domains-in-Windows-Server-2016/Part-1-Fault-Domains-Overview.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-1-Overview)

### <a name="fault-domain-awareness-in-windows-server-2019"></a>Connaissance du domaine d’erreur dans Windows Server 2019

La sensibilisation au domaine d’erreur est disponible dans Windows Server 2019, mais elle est désactivée par défaut et doit être activée via le Registre Windows.

Pour activer la reconnaissance du domaine d’erreur dans Windows Server 2019, accédez au registre Windows et définissez (obten-cluster). Clé de Registre AutoAssignNodeSite sur 1.

```Registry
    (Get-Cluster).AutoAssignNodeSite=1
```

Pour désactiver la reconnaissance du domaine d’erreur dans Windows 2019, accédez au registre Windows et définissez (obten-cluster). Clé de Registre AutoAssignNodeSite sur 0.

```Registry
    (Get-Cluster).AutoAssignNodeSite=0
```

## <a name="benefits"></a>Avantages
- **Les espaces de stockage, y compris espaces de stockage direct, utilisent des domaines d’erreur pour optimiser la sécurité des données.**  
    Du point de vue conceptuel, la résilience dans les espaces de stockage est similaire à la technologie RAID distribuée et à définition logicielle. Plusieurs copies de toutes les données restent synchronisées, et si le matériel tombe en panne et qu’une copie est perdue, les autres sont recopiées pour restaurer la résilience. Pour obtenir la meilleure résilience possible, les copies doivent être conservées dans des domaines d’erreur distincts.

- **Le [service de contrôle d’intégrité](health-service-overview.md) utilise des domaines d’erreur pour fournir des alertes plus utiles.**  
    Chaque domaine d’erreur peut être associé à des métadonnées d’emplacement, automatiquement incluses dans les alertes ultérieures. Ces descripteurs peuvent aider le personnel d’exploitation et de maintenance et réduire les erreurs en levant les ambiguïtés liées au matériel.  

- **Le clustering étendu utilise des domaines d’erreur pour l’affinité de stockage.** Le clustering étendu permet aux serveurs éloignés de rejoindre un cluster commun. Pour des performances optimales, les applications ou machines virtuelles doivent être exécutées sur des serveurs situés à proximité de ceux qui fournissent leur stockage. La sensibilisation au domaine de l’erreur active cette affinité de stockage.   

## <a name="levels-of-fault-domains"></a>Niveaux de domaines d’erreur  
Il existe quatre niveaux canoniques de domaines d’erreur : site, rack, châssis et nœud. Les nœuds sont découverts automatiquement. Chaque niveau supplémentaire est facultatif. Par exemple, si votre déploiement n’utilise pas de serveurs lames, le niveau du châssis n’a peut-être pas de sens pour vous.  

![Diagramme des différents niveaux de domaines d’erreur](media/Fault-Domains-in-Windows-Server-2016/levels-of-fault-domains.png)

## <a name="usage"></a>Utilisation  
Vous pouvez utiliser PowerShell ou le balisage XML pour spécifier des domaines d’erreur. Les deux approches sont équivalentes et fournissent des fonctionnalités complètes.

>[!IMPORTANT]
> Spécifiez les domaines d’erreur avant d’activer les espaces de stockage direct, si possible. Ainsi, la configuration automatique est activée pour préparer le pool, les niveaux et les paramètres tels que la résilience et le nombre de colonnes, pour une tolérance de panne au niveau du châssis ou rack. Une fois le pool et les volumes créés, les données ne se déplacement pas rétroactivement en réponse aux modifications apportées à la topologie des domaines d’erreur. Pour déplacer des nœuds entre des châssis ou racks après avoir activé des espaces de stockage direct, vous devez tout d’abord supprimer le nœud et ses lecteurs du pool à l’aide de `Remove-ClusterNode -CleanUpDisks`.

### <a name="defining-fault-domains-with-powershell"></a>Définition des domaines d’erreur avec PowerShell
Windows Server 2016 introduit les applets de commande suivantes pour travailler avec les domaines d’erreur :
* `Get-ClusterFaultDomain`
* `Set-ClusterFaultDomain`
* `New-ClusterFaultDomain` 
* `Remove-ClusterFaultDomain`

Cette courte vidéo montre l’utilisation de ces applets de commande.
[![cliquez sur cette image pour regarder une brève vidéo sur l’utilisation des applets de commande du domaine d’erreur du cluster](media/Fault-Domains-in-Windows-Server-2016/Part-2-Using-PowerShell.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-2-Using-PowerShell)

Utilisez `Get-ClusterFaultDomain` pour afficher la topologie du domaine d’erreur actuel. Celle-ci répertorie tous les nœuds du cluster, ainsi que tous les châssis, racks ou sites que vous avez créés. Vous pouvez les filtrer à l’aide de paramètres tels que **-Type** ou **-Name**, mais ceux-ci ne sont pas obligatoires.

```PowerShell
Get-ClusterFaultDomain
Get-ClusterFaultDomain -Type Rack
Get-ClusterFaultDomain -Name "server01.contoso.com"
```

Utilisez `New-ClusterFaultDomain` pour créer de nouveaux châssis, racks ou sites. Les paramètres `-Type` et `-Name` sont requis. Les valeurs possibles pour `-Type` sont `Chassis`, `Rack`et `Site`. La `-Name` peut être n’importe quelle chaîne. (Pour les domaines d’erreur de type `Node`, le nom doit être le nom du nœud réel, tel que défini automatiquement).

```PowerShell
New-ClusterFaultDomain -Type Chassis -Name "Chassis 007"
New-ClusterFaultDomain -Type Rack -Name "Rack A"
New-ClusterFaultDomain -Type Site -Name "Shanghai"
```

> [!IMPORTANT]  
> Windows Server ne peut pas et ne vérifie pas que les domaines d’erreur que vous créez correspondent à tout ce qui se trouve dans le monde réel. (Cela peut paraître évident, mais il est important de comprendre.) Si, dans le monde physique, vos nœuds se trouvent tous dans un seul rack, la création de deux domaines d’erreur `-Type Rack` dans le logiciel ne fournit pas de tolérance de panne de rack par magie. Il vous incombe de veiller à ce que la topologie que vous créez à l’aide de ces applets de commande corresponde à la disposition réelle de votre matériel.

Utilisez `Set-ClusterFaultDomain` pour déplacer un domaine d’erreur dans un autre. Les termes « parent » et « enfant » sont couramment utilisés pour décrire cette relation d’imbrication. Les paramètres `-Name` et `-Parent` sont requis. Dans `-Name`, indiquez le nom du domaine d’erreur qui est en cours de déplacement. dans `-Parent`, indiquez le nom de la destination. Pour déplacer plusieurs domaines d’erreur à la fois, dressez la liste de leurs noms.

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent "Rack A"
Set-ClusterFaultDomain -Name "Rack A", "Rack B", "Rack C", "Rack D" -Parent "Shanghai"
```

> [!IMPORTANT]  
> Quand des domaines d’erreur se déplacent, leurs enfants se déplacent avec eux. Dans l’exemple ci-dessus, si le rack A est le parent de server01.contoso.com, ce dernier n’a pas besoin de se déplacer séparément vers le site de Shanghai : il y est déjà puisque son parent y est, comme dans la configuration physique.

Vous pouvez voir les relations parent-enfant dans la sortie de `Get-ClusterFaultDomain`, dans les colonnes `ParentName` et `ChildrenNames`.

Vous pouvez également utiliser `Set-ClusterFaultDomain` pour modifier certaines autres propriétés des domaines d’erreur. Par exemple, vous pouvez fournir des métadonnées `-Location` ou `-Description` facultatives pour n’importe quel domaine d’erreur. Si ces informations sont fournies, elles sont incluses dans les alertes matérielles provenant du service de contrôle d’intégrité. Vous pouvez également renommer les domaines d’erreur à l’aide du paramètre `-NewName`. Ne renommez pas `Node` domaines d’erreur de type.

```PowerShell
Set-ClusterFaultDomain -Name "Rack A" -Location "Building 34, Room 4010"
Set-ClusterFaultDomain -Type Node -Description "Contoso XYZ Server"
Set-ClusterFaultDomain -Name "Shanghai" -NewName "China Region"
```

Utilisez `Remove-ClusterFaultDomain` pour supprimer le châssis, les racks ou les sites que vous avez créés. Le paramètre `-Name` est obligatoire. Vous ne pouvez pas supprimer un domaine d’erreur qui contient des enfants – tout d’abord, supprimez les enfants ou déplacez-les à l’extérieur à l’aide de `Set-ClusterFaultDomain`. Pour déplacer un domaine d’erreur en dehors de tous les autres domaines d’erreur, définissez sa `-Parent` sur la chaîne vide (""). Vous ne pouvez pas supprimer `Node` domaines d’erreur de type. Pour supprimer plusieurs domaines d’erreur à la fois, dressez la liste de leurs noms.

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent ""
Remove-ClusterFaultDomain -Name "Rack A"
```

### <a name="defining-fault-domains-with-xml-markup"></a>Définition des domaines d’erreur avec le balisage XML
Vous pouvez spécifier des domaines d’erreur en utilisant une syntaxe inspirée du langage XML. Nous vous recommandons d’utiliser votre éditeur de texte favori, tel que Visual Studio Code (disponible gratuitement *[ici](https://code.visualstudio.com/)* ) ou le Bloc-notes, pour créer un document XML que vous pouvez enregistrer et réutiliser.  

Cette courte vidéo montre l’utilisation du balisage XML pour spécifier des domaines d’erreur.

[![cliquez sur cette image pour regarder une brève vidéo sur l’utilisation de XML pour spécifier des domaines d’erreur](media/Fault-Domains-in-Windows-Server-2016/Part-3-Using-XML-Markup.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-3-Using-XML)

Dans PowerShell, exécutez l’applet de commande suivante : `Get-ClusterFaultDomainXML`. Celle-ci retourne la spécification de domaine d’erreur actuelle du cluster, au format XML. Cela reflète chaque `<Node>`découvert, inclus dans des balises d’ouverture et de fermeture `<Topology>`.  

Exécutez la commande suivante pour enregistrer cette sortie dans un fichier.  

```PowerShell
Get-ClusterFaultDomainXML | Out-File <Path>  
```

Ouvrez le fichier et ajoutez des balises `<Site>`, `<Rack>`et `<Chassis>` pour spécifier la façon dont ces nœuds sont répartis entre les sites, les racks et les châssis. Chaque balise doit être identifiée par un **nom** unique. Pour les nœuds, vous devez conserver le nom du nœud tel qu’il est renseigné par défaut.  

> [!IMPORTANT]  
> Bien que toutes les balises supplémentaires soient facultatives, elles doivent respecter la hiérarchie Site &gt; Rack &gt; Châssis &gt; transitive et être correctement fermées.  
Outre le nom, les `Location="..."` de forme libre et les descripteurs `Description="..."` peuvent être ajoutés à n’importe quelle balise.  

#### <a name="example-two-sites-one-rack-each"></a>Exemple : deux sites, un seul rack chacun  

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

Ce guide présente simplement deux exemples, mais les balises `<Site>`, `<Rack>`, `<Chassis>`et `<Node>` peuvent être mélangées et mises en correspondance de nombreuses façons supplémentaires pour refléter la topologie physique de votre déploiement, quelle que soit la valeur de. Nous espérons que ces exemples illustrent la flexibilité de ces balises et la valeur des descripteurs d’emplacement libre pour les distinguer.  

### <a name="optional-location-and-description-metadata"></a>Facultatif : métadonnées d’emplacement et de description

Vous pouvez fournir des métadonnées d' **emplacement** ou de **Description** facultatives pour tout domaine d’erreur. Si ces informations sont fournies, elles sont incluses dans les alertes matérielles provenant du service de contrôle d’intégrité. Cette brève vidéo montre la valeur de l’ajout de tels descripteurs.

[![cliquez pour voir une brève vidéo montrant la valeur de l’ajout de descripteurs d’emplacement aux domaines d’erreur](media/Fault-Domains-in-Windows-Server-2016/part-4-location-description.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-4-Location-Description)

## <a name="see-also"></a>Voir aussi  
- [Prise en main de Windows Server 2019](https://docs.microsoft.com/windows-server/get-started-19/get-started-19)  
- [Prise en main de Windows Server 2016](https://docs.microsoft.com/windows-server/get-started/server-basics)  
-   [Présentation de espaces de stockage direct](../storage/storage-spaces/storage-spaces-direct-overview.md) 
