---
title: "Service de contrôle d’intégrité dans Windows Server 2016"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 5bc71e71-920e-454f-8195-afebd2a23725
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: 834fcfb749e89e4768dce3f229564feea550a432
ms.sourcegitcommit: 30fcae929ce7b611f5d3a5f8fee64b0299272110
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/15/2017
---
# <a name="health-service-in-windows-server-2016"></a>Service de contrôle d’intégrité dans Windows Server 2016
> S’applique à Windows Server2016

Le Service de contrôle d’intégrité est une nouvelle fonctionnalité de Windows Server 2016 qui améliore l’analyse quotidienne et expérience opérationnel pour les clusters exécutant espaces de stockage Direct.

## <a name="prerequisites"></a>Conditions préalables  

Le Service de contrôle d’intégrité est activé par défaut avec les espaces de stockage Direct. Aucune action supplémentaire n’est requis pour configurer ou le démarrer. Pour en savoir plus sur les espaces de stockage Direct, voir [espaces de stockage Direct dans Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md).  

## <a name="reports"></a>Rapports

Voir [rapports du Service de contrôle d’intégrité](health-service-reports.md).

## <a name="faults"></a>Erreurs

Voir [pannes de Service de contrôle d’intégrité](health-service-faults.md).

## <a name="actions"></a>Actions

Voir [actions du Service d’intégrité](health-service-actions.md).

## <a name="automation"></a>Automatisation  

Cette section décrit le flux de travail automatisés par le Service de contrôle d’intégrité dans le cycle de vie du disque.  

### <a name="disk-lifecycle"></a>Cycle de vie du disque   

Le Service de contrôle d’intégrité automatise la plupart des étapes du cycle de vie du disque physique. Supposons que l’état initial de votre déploiement est parfait: autrement dit, tous les disques physiques fonctionnent correctement.  

#### <a name="retirement"></a>Mise hors service  

Disques physiques sont automatiquement supprimés lorsqu’ils ne peuvent plus être utilisés, et une erreur correspondante est générée. Il existe plusieurs cas:  

-   Défaillance de média: le disque physique est définitivement a échoué ou détérioré et doit être remplacé.  

-   Perte de Communication: le disque physique a perdu la connectivité pendant plus de 15 minutes consécutives.  

-   Absence de réponse: le disque physique a présenté la latence plus 5.0 secondes trois fois ou plus en une heure.  

>[!NOTE]
> Si la connectivité est perdue au nombre de disques physiques à la fois, ou à un nœud entier ou d’un boîtier de stockage, le Service de contrôle d’intégrité *pas* hors service ces disques car ils ont peu de chances d’être l’origine du problème.  

Si le disque mis hors service a été servant de cache à de nombreux autres disques physiques, ceux-ci seront automatiquement réaffectés à un autre disque de cache s’il est disponible. Aucune action spéciale de l’utilisateur n’est requise.  

#### <a name="restoring-resiliency"></a>Restauration de la résilience  

Une fois qu’un disque physique a été mis hors service, le Service de contrôle d’intégrité commence immédiatement à copier ses données sur les disques physiques restants, pour restaurer la résilience complète. Une fois cette opération terminée, les données totalement et la tolérance de panne sûres.  

>[!NOTE]
> Cette restauration immédiate nécessite une capacité disponible suffisante entre les disques physiques restants.  

#### <a name="blinking-the-indicator-light"></a>Clignotement du témoin lumineux  

Si possible, le Service de contrôle d’intégrité commence à faire clignoter le témoin lumineux sur le disque physique mis hors service ou son emplacement. Ce clignotement se poursuit jusqu'à ce que le disque mis hors service est remplacé.  

>[!NOTE]
> Dans certains cas, le disque peut avoir échoué d’une manière qui empêche même son témoin lumineux de fonctionner - par exemple, une perte d’alimentation totale.  

#### <a name="physical-replacement"></a>Remplacement physique  

Vous devez remplacer le disque physique mis hors service quand cela est possible. Le plus souvent, il s’agit d’un chaud - autrement dit mise hors tension le boîtier de stockage ou le nœud n’est pas nécessaire. Consultez l’erreur pour utiles sur l’emplacement et les informations de l’article.  

#### <a name="verification"></a>Vérification

Lorsque le disque de remplacement est inséré, il sera vérifié par rapport au Document de composants pris en charge (voir la section suivante).

#### <a name="pooling"></a>Le regroupement  

Si autorisée, le disque de remplacement est automatiquement remplacé dans le pool de son prédécesseur pour entrer en utilisation. À ce stade, le système est rétabli à son état initial d’intégrité parfaite, puis l’erreur disparaît.  

## <a name="supported-components-document"></a>Document de composants pris en charge  

Le Service de contrôle d’intégrité fournit un mécanisme de mise en œuvre pour limiter les composants utilisés par les espaces de stockage Direct à ceux d’un Document de composants pris en charge fournie par l’administrateur ou le fournisseur de solutions. Cela peut servir à empêcher l’utilisation erronée du matériel non pris en charge par vous-même ou par d’autres personnes, ce qui peut aider à la conformité des contrats de garantie ou de la prise en charge. Cette fonctionnalité est actuellement limitée à des périphériques de disque physique, y compris les disques SSD, HDD, et les lecteurs NVMe. Le Document des composants pris en charge peut limiter les modèles, fabricants (facultatif) et versions de microprogramme (facultative).

### <a name="usage"></a>Utilisation  

Le Document de composants pris en charge utilise une syntaxe inspirée du langage XML. Nous vous recommandons d’utiliser votre éditeur de texte, tel que Visual Studio Code (disponible gratuitement [ici](http://code.visualstudio.com/)) ou le bloc-notes, pour créer un document XML que vous pouvez enregistrer et réutiliser.

#### <a name="sections"></a>Sections

Le document a deux sections indépendantes: **disques** et **Cache**.

Si le **disques** section est fournie, uniquement les lecteurs répertoriés sont autorisés à joindre des pools. Des lecteurs non listées ne peuvent pas joindre des pools, ce qui exclut effectivement leur utilisation en production. Si cette section est vide, n’importe quel lecteur sera autorisé à joindre des pools.

Si le **Cache** section est fournie, uniquement les lecteurs répertoriés seront utilisés pour la mise en cache. Si cette section est vide, les espaces de stockage Direct tente de deviner basées sur le type de média et le type de bus. Par exemple, si votre déploiement utilise des disques SSD (Solid) et les lecteurs de disque dur (HDD), le premier est automatiquement choisi pour la mise en cache; Toutefois, si votre déploiement utilise tout flash, vous devrez peut-être spécifier les appareils résistance plus élevés que vous souhaitez utiliser pour la mise en cache ici.

>[!IMPORTANT]
> Le Document des composants pris en charge ne s’applique pas rétroactivement aux lecteurs déjà mis en pool et en cours d’utilisation.  

#### <a name="example"></a>Exemple  

```XML
<Components>

  <Disks>
    <Disk>
      <Manufacturer>Contoso</Manufacturer>
      <Model>XYZ9000</Model>
      <AllowedFirmware>
        <Version>2.0</Version>
        <Version>2.1</Version>
        <Version>2.2</Version>
      </AllowedFirmware>
      <TargetFirmware>
        <Version>2.1</Version>
        <BinaryPath>\\path\to\image.bin</BinaryPath>
      </TargetFirmware>
    </Disk>
  </Disks>

  <Cache>
    <Disk>
      <Manufacturer>Fabrikam</Manufacturer>
      <Model>QRSTUV</Model>
    </Disk>
  </Cache>

</Components>

```

Pour afficher la liste plusieurs lecteurs, ajoutez simplement d’autres ** &lt;disque&gt; ** balises dans chaque section.

Pour injecter du code XML lors du déploiement d’espaces de stockage Direct, utilisez le **- XML** indicateur:

```PowerShell
Enable-ClusterS2D -XML <MyXML>
```

Pour définir ou modifier le Document de composants pris en charge une fois que les espaces de stockage Direct a été déployé (autrement dit, une fois que le Service de contrôle d’intégrité est déjà en cours d’exécution), utilisez l’applet de commande PowerShell suivante:

```PowerShell
$MyXML = Get-Content <\\path\to\file.xml> | Out-String  
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.SupportedComponents.Document" -Value $MyXML  
```

>[!NOTE]
>Les propriétés de la version du microprogramme, fabricant et le modèle doivent correspondre exactement les valeurs que vous obtenez à l’aide de la **Get-PhysicalDisk** applet de commande. Cela peut différer de vos attentes «bon sens», en fonction de l’implémentation de votre fournisseur. Par exemple, au lieu de «Contoso», le fabricant peut être «CONTOSO LTD», ou il peut être vide lorsque le modèle est «Contoso-XZY9000».  

Vous pouvez vérifier à l’aide de l’applet de commande PowerShell suivante:  

```PowerShell
Get-PhysicalDisk | Select Model, Manufacturer, FirmwareVersion  
```

## <a name="settings"></a>Paramètres

Voir [paramètres du Service de contrôle d’intégrité](health-service-settings.md).

## <a name="see-also"></a>Voir aussi

- [Rapports de Service d’intégrité](health-service-reports.md)
- [Pannes de Service de contrôle d’intégrité](health-service-faults.md)
- [Actions de Service de contrôle d’intégrité](health-service-actions.md)
- [Paramètres du Service d’intégrité](health-service-settings.md)
- [Espaces de stockage Direct dans Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md)
