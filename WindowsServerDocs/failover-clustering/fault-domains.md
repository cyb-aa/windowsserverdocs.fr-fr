---
ms.assetid: 56fc7f80-9558-467e-a6e9-a04c9abbee33
title: "Reconnaissance des domaines d’erreur"
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-failover-clustering
ms.topic: article
author: cosmosdarwin
ms.date: 09/16/2016
ms.openlocfilehash: f5c64bb8f8b7d4b8d13c76c4e94cfcf52ee32c30
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="fault-domain-awareness-in-windows-server-2016"></a>Reconnaissance des domaines d’erreur dans Windows Server2016

> S’applique à: Windows Server2016

Le clustering avec basculement permet à plusieurs serveurs de travailler ensemble pour fournir une haute disponibilité, ou d’autres termes, à tolérance de panne de nœud. Toutefois, les entreprises d’aujourd'hui exigent une disponibilité toujours plus grande de leur infrastructure. Pour obtenir la disponibilité de type cloud, même très occurrences tels que des échecs de châssis, un dysfonctionnement de rack ou catastrophes naturelles doivent être protégés contre. C’est pourquoi le Clustering de basculement dans Windows Server2016 introduit également tolérance de panne de site, les racks et châssis.

Domaines d’erreur et une tolérance de panne sont des concepts étroitement liés. Un domaine d’erreur est un ensemble de composants matériels qui partagent un point de défaillance unique. Pour obtenir une tolérance de panne à un certain niveau, vous avez besoin de plusieurs domaines d’erreur à ce niveau. Par exemple, pour être rack à tolérance de pannes, vos serveurs et vos données doivent être distribuées sur plusieurs racks.

Cette courte vidéo présente une vue d’ensemble des domaines d’erreur dans Windows Server2016:  
[![Cliquez sur cette image pour regarder une vue d’ensemble des domaines d’erreur dans Windows Server2016](media/Fault-Domains-in-Windows-Server-2016/Part-1-Fault-Domains-Overview.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-1-Overview)

## <a name="benefits"></a>Avantages
- **Espaces de stockage, y compris les espaces de stockage Direct, utilise des domaines d’erreur pour optimiser la sécurité des données.**  
    Résilience dans les espaces de stockage est similaire à RAID distribuée, définie par logiciel. Plusieurs copies de toutes les données restent synchronisées, et si le matériel tombe en panne et une copie est perdue, d’autres sont recopiées pour restaurer la résilience. Pour obtenir la meilleure résilience possible, les copies doivent être conservées dans des domaines d’erreur distincts.

- **Le [Service d’intégrité](health-service-overview.md) utilise pour fournir d’autres alertes utiles des domaines d’erreur.**  
    Chaque domaine d’erreur peut être associé à des métadonnées d’emplacement, automatiquement incluses dans les alertes ultérieures. Ces descripteurs peuvent aider le personnel de maintenance et réduire les erreurs à lever les ambiguïtés entre matériel.  

- **Le clustering étendu utilise des domaines d’erreur pour l’affinité de stockage.** Le clustering étendu permet aux serveurs éloignés de rejoindre un cluster commun. Pour des performances optimales, les applications ou les ordinateurs virtuels doit être exécutés sur les serveurs situés à proximité de ceux qui fournissent leur stockage. Reconnaissance des domaines d’erreur permet cette affinité de stockage.   

## <a name="levels-of-fault-domains"></a>Niveaux de domaines d’erreur  
Il existe quatre niveaux canoniques de domaines d’erreur - site, rack, châssis et nœud. Les nœuds sont découverts automatiquement; chaque niveau supplémentaire est facultatif. Par exemple, si votre déploiement n’utilise pas de serveurs lames, le niveau de châssis peut dénué de sens pour vous.  

![Diagramme des différents niveaux de domaines d’erreur](media/Fault-Domains-in-Windows-Server-2016/levels-of-fault-domains.png)

## <a name="usage"></a>Utilisation  
Vous pouvez utiliser PowerShell ou le balisage XML pour spécifier des domaines d’erreur. Les deux approches sont équivalentes et fournissent des fonctionnalités complètes.

>[!IMPORTANT]
> Spécifiez les domaines d’erreur avant d’activer les espaces de stockage Direct, si possible. Cela permet la configuration automatique préparer le pool, niveaux et paramètres de résilience et le nombre de colonnes, à tolérance de pannes des châssis ou rack. Une fois que le pool et les volumes ont été créées, les données seront déplacement pas rétroactivement en réponse aux modifications à la topologie de domaine d’erreur. Pour déplacer des nœuds entre des châssis ou racks après avoir activé des espaces de stockage Direct, vous devez tout d’abord supprimer le nœud et ses lecteurs du pool à l’aide `Remove-ClusterNode -CleanUpDisks`.

### <a name="defining-fault-domains-with-powershell"></a>Définition des domaines d’erreur avec PowerShell
Windows Server2016 introduit les applets de commande suivantes pour travailler avec des domaines d’erreur:
* `Get-ClusterFaultDomain`
* `Set-ClusterFaultDomain`
* `New-ClusterFaultDomain` 
* `Remove-ClusterFaultDomain`

Cette courte vidéo montre l’utilisation de ces applets de commande.
[![Cliquez sur cette image pour visionner une vidéo sur l’utilisation des applets de commande domaine d’erreur Cluster courte](media/Fault-Domains-in-Windows-Server-2016/Part-2-Using-PowerShell.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-2-Using-PowerShell)

Utilisez `Get-ClusterFaultDomain`pour afficher la topologie de domaine d’erreur en cours. Celle-ci répertorie tous les nœuds du cluster, ainsi que les châssis, racks ou sites que vous avez créés. Vous pouvez filtrer à l’aide de paramètres tels que **-Type** ou **-nom**, mais ils ne sont pas requis.

```PowerShell
Get-ClusterFaultDomain
Get-ClusterFaultDomain -Type Rack
Get-ClusterFaultDomain -Name "server01.contoso.com"
```

Utilisez `New-ClusterFaultDomain`pour créer le nouveau châssis, racks ou sites. Le `-Type`et `-Name`sont des paramètres obligatoires. Les valeurs possibles pour `-Type`sont `Chassis`, `Rack`, et `Site`. Le `-Name`peut être n’importe quelle chaîne. (Pour `Node`type des domaines d’erreur, le nom doivent être le nom réel du nœud, défini automatiquement).

```PowerShell
New-ClusterFaultDomain -Type Chassis -Name "Chassis 007"
New-ClusterFaultDomain -Type Rack -Name "Rack A"
New-ClusterFaultDomain -Type Site -Name "Shanghai"
```

> [!IMPORTANT]  
> Windows Server ne peut pas et ne vérifie pas que les domaines d’erreur que vous créez correspondent à n’importe où dans le monde réel, physique. (Cela peut paraître évident, mais il est important de comprendre.) Si, dans le monde physique, vos nœuds sont tous dans un seul rack, puis en créant deux `-Type Rack`domaines d’erreur dans le logiciel ne fournit pas comme par magie une tolérance de panne en rack. Vous êtes responsable de la topologie que vous créez à l’aide de ces applets de commande correspond à la disposition réelle de votre matériel.

Utilisez `Set-ClusterFaultDomain`pour déplacer un domaine d’erreur dans un autre. Les termes «parent» et «enfant» sont couramment utilisés pour décrire cette relation d’imbrication. Le `-Name`et `-Parent`sont des paramètres obligatoires. Dans `-Name`, indiquez le nom de domaine d’erreur qui se déplace; dans `-Parent`, indiquez le nom de la destination. Pour déplacer plusieurs domaines d’erreur à la fois, dressez la liste leurs noms.

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent "Rack A"
Set-ClusterFaultDomain -Name "Rack A", "Rack B", "Rack C", "Rack D" -Parent "Shanghai"
```

> [!IMPORTANT]  
> Lorsque vous déplacement des domaines d’erreur, leurs enfants se déplacent avec eux. Dans l’exemple ci-dessus, si le Rack A est le parent de server01.contoso.com, ce dernier pas séparément doit-elle être déplacés vers le site de Shanghai: il est déjà puisque son parent y, comme dans le monde physique.

Vous pouvez voir les relations parent-enfant dans la sortie de `Get-ClusterFaultDomain`, dans le `ParentName`et `ChildrenNames`colonnes.

Vous pouvez également utiliser `Set-ClusterFaultDomain`pour modifier certaines autres propriétés de domaines d’erreur. Par exemple, vous pouvez fournir facultatif `-Location`ou `-Description`métadonnées pour n’importe quel domaine d’erreur. Si fourni, ces informations sont incluses dans les alertes matérielles à partir du Service de contrôle d’intégrité. Vous pouvez également renommer les domaines d’erreur à l’aide de la `-NewName`paramètre. Ne renommez pas `Node`tapez des domaines d’erreur.

```PowerShell
Set-ClusterFaultDomain -Name "Rack A" -Location "Building 34, Room 4010"
Set-ClusterFaultDomain -Type Node -Description "Contoso XYZ Server"
Set-ClusterFaultDomain -Name "Shanghai" -NewName "China Region"
```

Utilisez `Remove-ClusterFaultDomain`pour supprimer les châssis, racks ou sites que vous avez créés. Le `-Name`paramètre est obligatoire. Vous ne peut pas supprimer un domaine d’erreur qui contient des enfants: tout d’abord, supprimez les enfants ou déplacez-les à l’extérieur à l’aide de `Set-ClusterFaultDomain`. Pour déplacer un domaine d’erreur en dehors de tous les autres domaines d’erreur, définissez son `-Parent`à une chaîne vide («»). Vous ne pouvez pas supprimer `Node`tapez des domaines d’erreur. Pour supprimer plusieurs domaines d’erreur à la fois, dressez la liste leurs noms.

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent ""
Remove-ClusterFaultDomain -Name "Rack A"
```

### <a name="defining-fault-domains-with-xml-markup"></a>Définition des domaines d’erreur avec le balisage XML
Domaines d’erreur peuvent être spécifiés à l’aide d’une syntaxe inspirée du langage XML. Nous vous recommandons d’utiliser votre éditeur de texte, tel que Visual Studio Code (disponible gratuitement *[ici](https://code.visualstudio.com/)*) ou le bloc-notes, pour créer un document XML que vous pouvez enregistrer et réutiliser.  

Cette courte vidéo montre l’utilisation du balisage XML pour spécifier des domaines d’erreur.

[![CCliquez sur cette image pour visionner une vidéo sur l’utilisation du XML pour spécifier des domaines d’erreur courte](media/Fault-Domains-in-Windows-Server-2016/Part-3-Using-XML-Markup.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-3-Using-XML)

Dans PowerShell, exécutez l’applet de commande suivant:`Get-ClusterFaultDomainXML`. Cette propriété renvoie la spécification du domaine actuel pannes pour le cluster, au format XML. Cette spécification reflète tous découverts `<Node>`, encapsulé dans l’ouverture et fermeture `<Topology>`balises.  

Exécutez la commande suivante pour enregistrer cette sortie dans un fichier.  

```PowerShell
Get-ClusterFaultDomainXML | Out-File <Path>  
```

Ouvrez le fichier, puis ajoutez `<Site>`, `<Rack>`, et `<Chassis>`balises pour indiquer comment ces nœuds sont distribués entre les sites, racks et châssis. Chaque balise doit être identifié par un unique **nom **. Pour les nœuds, vous devez conserver le nom du nœud comme défini par défaut.  

> [!IMPORTANT]  
> Alors que toutes les balises supplémentaires soient facultatives, elles doivent respecter le Site transitif &gt;Rack &gt;châssis &gt;hiérarchie et doit être fermé correctement.  
En plus du nom, vous `Location="..."`et `Description="..."`descripteurs peuvent être ajoutées à n’importe quelle balise.  

#### <a name="example-two-sites-one-rack-each"></a>Exemple: Deux sites, un seul rack chacun  

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

#### <a name="example-two-chassis-blade-servers"></a>Exemple: deux châssis, serveurs lames  
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

Ce guide présente simplement deux exemples, mais la `<Site>`, `<Rack>`, `<Chassis>`, et `<Node>`balises peuvent être mélangées et associées de nombreuses autres manières afin de refléter la topologie physique de votre déploiement, quelle qu’elle soit. Nous espérons que ces exemples illustrent la flexibilité de ces balises et la valeur de descripteurs d’emplacement libre pour les distinguer.  

### <a name="optional-location-and-description-metadata"></a>Facultatif: Métadonnées d’emplacement et la description

Vous pouvez fournir facultatif **emplacement** ou **Description** métadonnées pour n’importe quel domaine d’erreur. Si fourni, ces informations sont incluses dans les alertes matérielles à partir du Service de contrôle d’intégrité. Cette courte vidéo montre la valeur de l’ajout de ces descripteurs.

[![CCliquez sur une courte vidéo montrant la valeur de l’ajout de descripteurs d’emplacement aux domaines d’erreur](media/Fault-Domains-in-Windows-Server-2016/part-4-location-description.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-4-Location-Description)

## <a name="see-also"></a>Voir aussi  
-   [Windows Server2016](../get-started/windows-server-2016.md)  
-   [Espaces de stockage Direct dans Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md) 
