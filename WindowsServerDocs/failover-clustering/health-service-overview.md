---
title: Service de contrôle d’intégrité dans Windows Server
ms.prod: windows-server
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 5bc71e71-920e-454f-8195-afebd2a23725
author: cosmosdarwin
ms.date: 02/09/2018
ms.openlocfilehash: 158681e2038e3d8015933771d06d3bfb24d31586
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75948472"
---
# <a name="health-service-in-windows-server"></a>Service de contrôle d’intégrité dans Windows Server

> S’applique à : Windows Server 2019, Windows Server 2016

Le Service de contrôle d’intégrité est une nouvelle fonctionnalité de Windows Server 2016 qui améliore la surveillance quotidienne et l’expérience opérationnelle pour les clusters exécutant espaces de stockage direct.

## <a name="prerequisites"></a>Prérequis  

Le service de contrôle d’intégrité est activé par défaut avec les espaces de stockage direct. Aucune action supplémentaire n’est requise pour le configurer ou le démarrer. Pour en savoir plus sur les espaces de stockage direct, consultez [espaces de stockage direct dans Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md).  

## <a name="reports"></a>Rapports

Consultez [service de contrôle d’intégrité des rapports](health-service-reports.md).

## <a name="faults"></a>Erreurs

Consultez [service de contrôle d’intégrité des erreurs](health-service-faults.md).

## <a name="actions"></a>Actions

Consultez [service de contrôle d’intégrité actions](health-service-actions.md).

## <a name="automation"></a>Automatisation  

Cette section décrit les flux de travail automatisés par le service de contrôle d’intégrité pendant le cycle de vie du disque.  

### <a name="disk-lifecycle"></a>Cycle de vie du disque   

Le service de contrôle d’intégrité automatise la plupart des étapes du cycle de vie du disque physique. Supposons que l’état initial de votre déploiement soit parfait : autrement dit, tous les disques physiques fonctionnent correctement.  

#### <a name="retirement"></a>Mise hors service  

Les disques physiques sont automatiquement mis hors service quand ils ne peuvent plus être utilisés et une erreur correspondante est générée. Plusieurs cas existent :  

-   Défaillance de média : le disque physique est définitivement défaillant ou détérioré et doit être remplacé.  

-   Perte de communication : le disque physique a perdu la connectivité pendant plus de 15 minutes consécutives.  

-   Absence de réponse : le disque physique a montré une latence de plus de 5 secondes trois fois ou plus en une heure.  

>[!NOTE]
> En cas de perte de connexion à de nombreux disques physiques en même temps ou à un nœud ou boîtier de stockage entier, le service de contrôle d’intégrité ne met *pas* hors service ces disques car ils ont peu de chances d’être à l’origine du problème.  

Si le disque mis hors service servait de cache à de nombreux autres disques physiques, ceux-ci sont automatiquement réaffectées à un autre disque de cache s’il y en a un disponible. Aucune action spéciale de l’utilisateur n’est nécessaire.  

#### <a name="restoring-resiliency"></a>Restauration de la résilience  

Une fois qu’un disque physique a été mis hors service, le service de contrôle d’intégrité commence immédiatement à copier ses données sur les disques physiques restants, pour restaurer la résilience complète. Une fois l’opération terminée, les données sont à nouveau totalement sûres et à tolérance de panne.  

>[!NOTE]
> Cette restauration immédiate nécessite une capacité disponible suffisante entre les disques physiques restants.  

#### <a name="blinking-the-indicator-light"></a>Clignotement du témoin lumineux  

Si possible, le service de contrôle d’intégrité commence à faire clignoter le témoin lumineux sur le disque physique mis hors service ou son emplacement. Ce clignotement se poursuit jusqu’à ce que le disque mis hors service soit remplacé.  

>[!NOTE]
> Dans certains cas, le disque peut avoir échoué d’une manière qui empêche même son témoin lumineux de fonctionner (par exemple, en cas de coupure totale d’alimentation).  

#### <a name="physical-replacement"></a>Remplacement physique  

Vous devez remplacer le disque physique mis hors service quand cela est possible. La plupart du temps, il s’agit d’un échange à chaud, c’est-à-dire que la mise hors tension du nœud ou du boîtier de stockage n’est pas nécessaire. Consultez l’erreur pour obtenir des informations utiles sur l’emplacement et le composant concernés.  

#### <a name="verification"></a>Vérification

Lorsque le disque de remplacement est inséré, il est vérifié par rapport au document composants pris en charge (voir la section suivante).

#### <a name="pooling"></a>Mise en pool  

Sur autorisation, le disque de remplacement est automatiquement remplacé dans le pool de son prédécesseur pour entrer en utilisation. À ce stade, le système est rétabli à son état initial d’intégrité parfaite, puis l’erreur disparaît.  

## <a name="supported-components-document"></a>Document sur les composants pris en charge  

Le Service de contrôle d’intégrité fournit un mécanisme de mise en œuvre pour limiter les composants utilisés par espaces de stockage direct à ceux d’un document de composants pris en charge fourni par l’administrateur ou le fournisseur de la solution. Ce mécanisme permet de vous empêcher, ainsi que d’autres, d’utiliser par erreur du matériel non pris en charge, facilitant ainsi la conformité aux contrats de garantie ou de support. Cette fonctionnalité est actuellement limitée aux périphériques de disque physique, y compris les disques SSD, les disques durs et les lecteurs NVMe. Le document composants pris en charge peut restreindre sur le modèle, le fabricant (facultatif) et la version du microprogramme (facultatif).

### <a name="usage"></a>Utilisation  

Le document sur les composants pris en charge utilise une syntaxe XML inspirée. Nous vous recommandons d’utiliser votre éditeur de texte préféré, tel que le [Visual Studio code](https://code.visualstudio.com/) ou le bloc-notes gratuit, pour créer un document XML que vous pouvez enregistrer et réutiliser.

#### <a name="sections"></a>Sections

Le document comporte deux sections indépendantes : `Disks` et `Cache`.

Si la section `Disks` est fournie, seuls les lecteurs répertoriés (comme `Disk`) sont autorisés à joindre des pools. Les lecteurs qui ne sont pas répertoriés ne sont pas autorisés à joindre des pools, ce qui exclut efficacement leur utilisation en production. Si cette section est laissée vide, tout lecteur est autorisé à joindre des pools.

Si la section `Cache` est fournie, seuls les lecteurs répertoriés (comme `CacheDisk`) sont utilisés pour la mise en cache. Si cette section est laissée vide, espaces de stockage direct tente de [deviner en fonction du type de média et du type de bus](../storage/storage-spaces/understand-the-cache.md#cache-drives-are-selected-automatically). Les lecteurs répertoriés ici doivent également être répertoriés dans `Disks`.

>[!IMPORTANT]
> Le document composants pris en charge ne s’applique pas rétroactivement aux lecteurs déjà regroupés et en cours d’utilisation.  

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
        <BinaryPath>C:\ClusterStorage\path\to\image.bin</BinaryPath>
      </TargetFirmware>
    </Disk>
    <Disk>
      <Manufacturer>Fabrikam</Manufacturer>
      <Model>QRSTUV</Model>
    </Disk>
  </Disks>

  <Cache>
    <CacheDisk>
      <Manufacturer>Fabrikam</Manufacturer>
      <Model>QRSTUV</Model>
    </CacheDisk>
  </Cache>

</Components>

```

Pour répertorier plusieurs lecteurs, ajoutez simplement des balises `<Disk>` ou `<CacheDisk>` supplémentaires.

Pour injecter ce code XML lors du déploiement de espaces de stockage direct, utilisez le paramètre `-XML` :

```PowerShell
$MyXML = Get-Content <Filepath> | Out-String  
Enable-ClusterS2D -XML $MyXML
```

Pour définir ou modifier le document composants pris en charge une fois espaces de stockage direct a été déployé :

```PowerShell
$MyXML = Get-Content <Filepath> | Out-String  
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.SupportedComponents.Document" -Value $MyXML  
```

>[!NOTE]
>Les propriétés du modèle, du fabricant et de la version du microprogramme doivent correspondre exactement aux valeurs que vous obtenez à l’aide de l’applet de commande **Get-PhysicalDisk**. Celles-ci ne tombent pas toujours sous le sens, mais dépendent de l’implémentation de votre fournisseur. Par exemple, au lieu de « Contoso », le fabricant peut être « CONTOSO LTD » ou il peut ne pas être renseigné quand le modèle est « Contoso-XZY9000 ».  

Vous pouvez procéder à une vérification à l’aide de l’applet de commande PowerShell suivante :  

```PowerShell
Get-PhysicalDisk | Select Model, Manufacturer, FirmwareVersion  
```

## <a name="settings"></a>Paramètres

Consultez [paramètres de service de contrôle d’intégrité](health-service-settings.md).

## <a name="see-also"></a>Articles associés

- [Rapports de Service de contrôle d’intégrité](health-service-reports.md)
- [Erreurs de Service de contrôle d’intégrité](health-service-faults.md)
- [Actions Service de contrôle d’intégrité](health-service-actions.md)
- [Paramètres de Service de contrôle d’intégrité](health-service-settings.md)
- [Storage Spaces Direct dans Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md)
