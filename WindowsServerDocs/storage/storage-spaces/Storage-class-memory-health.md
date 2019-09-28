---
ms.assetid: 2bab6bf6-90e7-46a7-b917-14a7a8f55366
title: Gestion de l’intégrité de la mémoire de classe stockage (NVDIMM-N) dans Windows
ms.prod: windows-server
ms.author: jgerend
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: JasonGerend
ms.date: 06/25/2019
ms.localizationpriority: medium
ms.openlocfilehash: 03d986832e14e0dd7b80324de3c9f14d0537dba5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402909"
---
# <a name="storage-class-memory-nvdimm-n-health-management-in-windows"></a>Gestion de l’intégrité de la mémoire de classe stockage (NVDIMM-N) dans Windows

> S'applique à : Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel), Windows 10

Cet article fournit aux administrateurs système et aux professionnels de l’informatique des informations sur la gestion des erreurs et la gestion de l’intégrité propres aux dispositifs de mémoire de classe stockage (NVDIMM-N) dans Windows, en soulignant les différences entre la mémoire de classe stockage et les dispositifs de stockage traditionnels.

Si vous ne connaissez pas la prise en charge Windows des dispositifs de mémoire de classe stockage, ces courtes vidéos vous en donnent un aperçu :
- [Utilisation de la mémoire non volatile (NVDIMM-N) comme stockage de bloc dans Windows Server 2016](https://channel9.msdn.com/Events/Build/2016/P466)
- [Utilisation de la mémoire non volatile (NVDIMM-N) en tant que stockage adressable en octets dans Windows Server 2016](https://channel9.msdn.com/Events/Build/2016/P470)
- [Accélération des performances de SQL Server 2016 avec de la mémoire persistante dans Windows Server 2016](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-Windows-Server-2016-SCM--FAST)

Consultez également [comprendre et déployer la mémoire persistante dans espaces de stockage direct](deploy-pmem.md).

Les dispositifs de mémoire de classe stockage NVDIMM-N compatibles JEDEC sont pris en charge dans Windows avec des pilotes natifs, à compter de Windows Server 2016 et Windows 10 (version 1607). Même si ces dispositifs se comportent de manière similaire aux autres disques (HDD et SSD), il existe des différences.

Toutes les conditions répertoriées ici sont supposées correspondre à des cas très rares, mais elles dépendent des conditions dans lesquelles le matériel est utilisé.

Les différents cas ci-dessous peuvent faire référence à des configurations d’espaces de stockage. La configuration la plus intéressante est celle qui comporte deux dispositifs NVDIMM-N utilisés en tant que cache en écriture différée mis en miroir dans un espace de stockage. Pour définir une telle configuration, consultez [Configuration des espaces de stockage avec un cache en écriture différée NVDIMM-N](https://msdn.microsoft.com/library/mt650885.aspx).

Dans Windows Server 2016, l’interface graphique utilisateur des espaces de stockage indique que le type de bus NVDIMM-N est INCONNU. Il ne présente aucune perte de fonctionnalités ni incapacité à créer un pool, VD de stockage. Vous pouvez vérifier le type de bus en exécutant la commande suivante :

```powershell
PS C:\>Get-PhysicalDisk | fl
```
Le paramètre BusType dans la sortie de l’applet de commande affiche correctement le type de bus en tant que « SCM »

## <a name="checking-the-health-of-storage-class-memory"></a>Vérification de l’intégrité de la mémoire de classe stockage
Pour interroger l’intégrité de la mémoire de classe stockage, utilisez les commandes suivantes dans une session Windows PowerShell.

```powershell
PS C:\> Get-PhysicalDisk | where BusType -eq "SCM" | select SerialNumber, HealthStatus, OperationalStatus, OperationalDetails
```

Vous obtenez ainsi cet exemple de sortie :

| SerialNumber | HealthStatus | OperationalStatus | OperationalDetails |
| --- | --- | --- | --- |
| 802c-01-1602-117cb5fc | Healthy | OK | |
| 802c-01-1602-117cb64f | Warning | Prévention d’erreur | {Threshold Exceeded,NVDIMM\_N Error} |

> [!NOTE]
> Pour rechercher l’emplacement physique d’un dispositif NVDIMM-N spécifié dans un événement, sur l’onglet **Détails** de l’événement dans l’observateur d’événements, accédez à **EventData** > **Emplacement**. Notez que Windows Server 2016 répertorie incorrectement l’emplacement des dispositifs NVDIMM-N, mais ce problème est résolu dans Windows Server, version 1709.

Pour mieux comprendre les différentes conditions d’intégrité, consultez les sections suivantes.

## <a name="warning-health-status"></a>État d’intégrité « Avertissement »

Cette condition s’applique quand vous vérifiez l’intégrité d’un dispositif de mémoire de classe stockage et que vous observez que son état d’intégrité est **Avertissement**, comme illustré dans cet exemple de sortie :

| SerialNumber | HealthStatus | OperationalStatus | OperationalDetails |
| --- | --- | --- | --- |
| 802c-01-1602-117cb5fc | Healthy | OK | |
| 802c-01-1602-117cb64f | Warning | Prévention d’erreur | {Threshold Exceeded,NVDIMM\_N Error} |

Le tableau suivant répertorie des informations sur cette condition.

| | Description |
| --- | --- |
| Condition probable | Violation du seuil d’avertissement NVDIMM-N |
| Cause première | Les dispositifs NVDIMM-N suivent divers seuils, comme la température, la durée de vie NVM et/ou la durée de vie de la source d’énergie. Quand l’un de ces seuils est dépassé, le système d’exploitation est notifié. |
| Comportement général | Le dispositif reste totalement opérationnel. Il s’agit d’un avertissement, pas d’une erreur. |
| Comportement des espaces de stockage | Le dispositif reste totalement opérationnel. Il s’agit d’un avertissement, pas d’une erreur. |
| Informations supplémentaires | Champ OperationalStatus de l’objet PhysicalDisk. Journal des événements – Microsoft-Windows-ScmDisk0101/Operational |
| À faire | Selon la violation du seuil d’avertissement, il peut s’avérer prudent d’envisager de remplacer l’ensemble ou certaines parties du dispositif NVDIMM-N. Par exemple, si le seuil de durée de vie NVM est dépassé, il peut s’avérer judicieux de remplacer le dispositif NVDIMM-N. |

## <a name="writes-to-an-nvdimm-n-fail"></a>Échec des écritures sur un dispositif NVDIMM-N

Cette condition s’applique quand vous vérifiez l’intégrité d’un dispositif de mémoire de classe stockage et que vous observez l’état d’intégrité **Défectueux** et l’état opérationnel **Erreur d’E/S**, comme illustré dans cet exemple de sortie :

| SerialNumber | HealthStatus | OperationalStatus | OperationalDetails |
| --- | --- | --- | --- |
| 802c-01-1602-117cb5fc | Healthy | OK | |
| 802c-01-1602-117cb64f | Unhealthy | {Métadonnées obsolètes, Erreur d’E/S, Erreur temporaire} | {Persistance de données perdue, Données perdues, NV...} |

Le tableau suivant répertorie des informations sur cette condition.

| | Description |
| --- | --- |
| Condition probable | Perte de persistance/alimentation de secours |
|Cause première|Les dispositifs NVDIMM-N s’appuient sur une source d’alimentation de secours pour leur persistance, souvent une batterie ou un supercondensateur. Si cette source d’alimentation de secours n’est pas disponible ou si le dispositif ne parvient pas à effectuer une sauvegarde pour une raison quelconque (erreur de contrôleur/Flash), les données sont exposées à un risque et Windows empêche toute nouvelle écriture sur le dispositif concerné. Les lectures sont toujours possibles pour évacuer des données.|
|Comportement général|Le volume NTFS est démonté.<br>Le champ de l’état d’intégrité de PhysicalDisk indique « Défectueux » pour tous les dispositifs NVDIMM-N concernés.|
|Comportement des espaces de stockage|L’espace de stockage reste opérationnel tant qu’un seul dispositif NVDIMM-N est concerné. Si plusieurs dispositifs sont concernés, les écritures dans l’espace de stockage échouent. <br>Le champ de l’état d’intégrité de PhysicalDisk indique « Défectueux » pour tous les dispositifs NVDIMM-N concernés.|
|Informations supplémentaires|Champ OperationalStatus de l’objet PhysicalDisk.<br>Journal des événements – Microsoft-Windows-ScmDisk0101/Operational|
|À faire|Nous vous recommandons de sauvegarder les données des dispositifs NVDIMM-N concernés. Pour obtenir un accès en lecture, vous pouvez manuellement mettre le disque en ligne (il apparaît en tant que volume NTFS en lecture seule).<br><br>Pour effacer entièrement cette condition, la cause première doit être résolue (c.-à-d. réparez le bloc d’alimentation ou remplacez le dispositif NVDIMM-N, en fonction du problème) et le volume situé sur le dispositif NVDIMM-N doit être mis hors connexion, puis remis en ligne, ou le système doit être redémarré.<br><br>Pour rendre le dispositif NVDIMM-N à nouveau utilisable dans les espaces de stockage, utilisez l’applet de commande **Reset-PhysicalDisk** qui réintègre le dispositif et démarre le processus de réparation.|

## <a name="nvdimm-n-is-shown-with-a-capacity-of-0-bytes-or-as-a-generic-physical-disk"></a>NVDIMM-N apparaît avec une capacité de 0 octet ou en tant que « Disque physique générique »

Cette condition s’applique quand un dispositif de mémoire de classe stockage apparaît avec une capacité de 0 octet et ne peut pas être initialisé, ou quand il est exposé en tant que « Disque physique générique » avec un état opérationnel **Perte de communication**, comme illustré dans cet exemple de sortie :

| SerialNumber | HealthStatus | OperationalStatus | OperationalDetails |
| --- | --- | --- | --- |
|802c-01-1602-117cb5fc|Healthy|OK||
||Warning|Perte de communication||

Le tableau suivant répertorie des informations sur cette condition.

||Description|
|---|---|
|Condition probable|Le BIOS n’a pas exposé de dispositif NVDIMM-N au système d’exploitation|
|Cause première|Les dispositifs NVDIMM-N sont de type DRAM. Quand une adresse DRAM endommagée est référencée, la plupart des processeurs lancent une vérification d’ordinateur et redémarrent le serveur. Certaines plateformes de serveur démappe le dispositif NVDIMM, ce qui empêche le système d’exploitation d’y accéder et d’entraîner éventuellement une autre vérification d’ordinateur. Ceci peut se produire si le BIOS détecte que le dispositif NVDIMM-N est défaillant et doit être remplacé.|
|Comportement général|Le dispositif NVDIMM-N apparaît comme non initialisé, avec une capacité de 0 octet et il n’est ni lisible ni inscriptible.|
|Comportement des espaces de stockage|L’espace de stockage reste opérationnel (sous réserve qu’un seul dispositif NVDIMM-N ne soit concerné).<br>L’objet PhysicalDisk NVDIMM-N apparaît avec un état d’intégrité Avertissement et en tant que « Disque physique générique »|
|Informations supplémentaires|Champ OperationalStatus de l’objet PhysicalDisk. <br>Journal des événements – Microsoft-Windows-ScmDisk0101/Operational|
|À faire|Le dispositif NVDIMM-N doit être remplacé ou assaini, pour que la plate-forme serveur l’expose à nouveau au système d’exploitation hôte. Le remplacement du dispositif est recommandé, car d’autres erreurs irrécupérables risquent de se produire. L’ajout d’un dispositif de remplacement à une configuration d’espaces de stockage peut s’effectuer à l’aide de l’applet de commande **Add-Physicaldisk**.|

## <a name="nvdimm-n-is-shown-as-a-raw-or-empty-disk-after-a-reboot"></a>Le dispositif NVDIMM-N apparaît brut ou en tant que disque vide après un redémarrage

Cette condition s’applique quand vous vérifiez l’intégrité d’un dispositif de mémoire de classe stockage et que vous observez l’état d’intégrité **Défectueux** et l’état opérationnel **Métadonnées non reconnues**, comme illustré dans cet exemple de sortie :

| SerialNumber | HealthStatus | OperationalStatus | OperationalDetails |
| --- | --- | --- | --- |
|802c-01-1602-117cb5fc|Healthy|OK|{Inconnus}|
|802c-01-1602-117cb64f|Unhealthy|{Métadonnées non reconnues, Métadonnées obsolètes}|{Inconnus}|

Le tableau suivant répertorie des informations sur cette condition.

||Description|
|---|---|
|Condition probable|Échec de sauvegarde/restauration|
|Cause première|Un échec dans la procédure de sauvegarde ou restauration a de fortes chances d’entraîner la perte de toutes les données situées sur le dispositif NVDIMM-N. Quand le système d’exploitation se charge, il apparaît comme un tout nouveau dispositif NVDIMM-N sans partition ni système de fichiers, il est brut (RAW).|
|Comportement général|Le dispositif NVDIMM-N est en mode de lecture seule. Une action explicite de l’utilisateur est nécessaire pour commencer à l’utiliser à nouveau.|
|Comportement des espaces de stockage|Les espaces de stockage restent opérationnels si un seul dispositif NVDIMM est concerné.<br>L’objet du disque physique NVDIMM-N apparaît avec l’état d’intégrité « Défectueux » et il n’est pas utilisé par les espaces de stockage.|
|Informations supplémentaires|Champ OperationalStatus de l’objet PhysicalDisk.<br>Journal des événements – Microsoft-Windows-ScmDisk0101/Operational|
|À faire|Si l’utilisateur ne veut pas remplacer le dispositif concerné, il peut utiliser l’applet de commande **Reset-PhysicalDisk** pour supprimer la condition de lecture seule sur le dispositif NVDIMM-N concerné. Dans les environnements d’espaces de stockage, celle-ci peut aussi essayer de réintégrer le dispositif NVDIMM-N dans l’espace de stockage et démarrer le processus de réparation.|

## <a name="interleaved-sets"></a>Jeux entrelacés

Des jeux entrelacés peuvent souvent être créés dans le BIOS d’une plateforme pour faire apparaître plusieurs dispositifs NVDIMM-N comme un seul dispositif pour le système d’exploitation.

Windows Server 2016 et Windows 10 Anniversary Edition ne prennent pas en charge les jeux entrelacés de dispositifs NVDIMM-N.

Au moment de la rédaction de cet article, il n’existe aucun mécanisme pour le système d’exploitation hôte lui permettant d’identifier correctement des dispositifs NVDIMM-N individuels dans un tel jeu et de communiquer clairement à l’utilisateur quel dispositif en particulier a pu entraîner une erreur ou a besoin d’être réparé.


