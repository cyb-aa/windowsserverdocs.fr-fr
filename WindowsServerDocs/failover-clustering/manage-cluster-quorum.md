---
title: Configurer et gérer le quorum dans un cluster de basculement
description: Obtenir des informations détaillées sur la façon de gérer le quorum du cluster dans un cluster de basculement Windows Server.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 01/18/2019
ms.localizationpriority: medium
ms.openlocfilehash: 21f99362205e0a6aa90ebd26cef8f3a779bdc1dc
ms.sourcegitcommit: 28dc7c7f1e44fee7ab2112228af329a9ce0e02ba
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2019
ms.locfileid: "9023995"
---
# Configurer et gérer le quorum

>S’applique à: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique fournit des étapes pour configurer et gérer le quorum dans un cluster de basculement Windows Server et en arrière-plan.

## Quorum de présentation

Le quorum pour un cluster est déterminé par le nombre d’éléments votants qui doit faire partie d’appartenance au cluster actif pour ce cluster à démarrer correctement ou continuer à s’exécuter. Pour des explications plus détaillées, voir la [Présentation de document de quorum de cluster et le pool](../storage/storage-spaces/understand-quorum.md).

## Options de configuration de quorum

Le modèle de quorum dans Windows Server est flexible. Si vous avez besoin de modifier la configuration de quorum pour votre cluster, vous pouvez utiliser l’Assistant Configuration de Quorum de Cluster ou les applets de commande Windows PowerShell Clusters de basculement. Pour les étapes et les considérations pour configurer le quorum, consultez [configurer le quorum du cluster](#configure-the-cluster-quorum) plus loin dans cette rubrique.

Le tableau suivant répertorie les options de configuration de trois quorum qui sont disponibles dans l’Assistant Configuration de Quorum du Cluster.

<table>
<thead>
<tr class="header">
<th>Option</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Utilisez les paramètres par défaut</td>
<td>Le cluster attribue automatiquement un vote à chaque nœud et gère dynamiquement les voix de nœud. Si cela est approprié pour votre cluster, stockage partagé de cluster est disponible, le cluster sélectionne un témoin de disque. Cette option est recommandée dans la plupart des cas, étant donné que le logiciel du cluster choisit automatiquement une configuration de quorum et témoin qui fournit la haute disponibilité pour votre cluster.</td>
</tr>
<tr class="even">
<td>Ajouter ou modifier le témoin de quorum</td>
<td>Vous pouvez ajouter, de modifier ou supprimer une ressource témoin. Vous pouvez configurer un témoin de partage ou le disque de fichiers. Le cluster attribue automatiquement un vote à chaque nœud et gère dynamiquement les voix de nœud.</td>
</tr>
<tr class="odd">
<td>Configuration de quorum avancées et une sélection témoin</td>
<td>Vous devez sélectionner cette option uniquement lorsque vous avez des exigences spécifiques à l’application ou site pour configurer le quorum. Vous pouvez modifier le témoin de quorum, ajouter ou supprimer des voix de nœud et choisir si le cluster gère dynamiquement des voix de nœud. Par défaut, voix est attribuées à tous les nœuds et les voix de nœud sont gérées de façon dynamique.</td>
</tr>
</tbody>
</table>

En fonction de l’option de configuration de quorum que vous choisissez et vos paramètres spécifiques, le cluster est configuré dans l’un des modes de quorum suivants:

<table>
<thead>
<tr class="header">
<th>Mode</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Nœud majoritaire (aucun témoin)</td>
<td>Seuls les nœuds ont une voix. Aucun témoin de quorum n’est configuré. Le quorum du cluster est la majorité des nœuds votants dans l’appartenance au cluster active.</td>
</tr>
<tr class="even">
<td>Nœud majoritaire avec témoin (disque ou partage de fichiers)</td>
<td>Les nœuds ont une voix. En outre, un témoin de quorum a un vote. Le quorum du cluster est la majorité des nœuds votants dans l’appartenance au cluster active plus un vote témoin.<br />
<br />
Un témoin de quorum peut être un témoin de disque désigné ou un témoin de partage de fichier désigné.</td>
</tr>
<tr class="odd">
<td>Aucune majorité (témoin de disque uniquement)</td>
<td>Aucun nœud n’ont une voix. Uniquement un témoin de disque a un vote. Le quorum du cluster est déterminé par l’état de la témoin de disque.<br />
<br />
Le cluster a quorum si un nœud est disponible et communique avec un disque spécifique dans le stockage de cluster. En règle générale, ce mode n’est pas recommandé, et il ne doit pas être sélectionné, car cela crée un point unique de défaillance pour le cluster.</td>
</tr>
</tbody>
</table>

Les sections suivantes vous donne plus d’informations sur les paramètres de configuration de quorum avancées.

### Configuration de rappel

En règle générale lorsque vous configurez un quorum, les éléments votants du cluster doivent être un numéro de dimension non standard. Par conséquent, si le cluster contient un nombre pair de vote nœuds, vous devez configurer un témoin de disque ou un témoin de partage de fichiers. Le cluster sera en mesure de maintenir un nœud supplémentaire vers le bas. En outre, l’ajout d’un vote témoin permet au cluster continuer à s’exécuter si la moitié des nœuds du cluster tomber en panne simultanément ou sont déconnectés.

Un témoin de disque est généralement recommandé si tous les nœuds peuvent voir le disque. Un témoin de partage de fichiers est recommandé lorsque vous avez besoin de prendre en compte la récupération d’urgence multisite avec un stockage répliqué. Configuration d’un témoin de disque avec un stockage répliqué est possible uniquement si le fournisseur de stockage prend en charge l’accès en lecture-écriture de tous les sites dans le stockage répliqué. <strong>*Un témoin de disque n’est pas pris en charge avec les espaces de stockage Direct*</strong>.

Le tableau suivant fournit des informations supplémentaires et des considérations sur les types de témoin de quorum.

<table>
<thead>
<tr class="header">
<th>Type de témoin</th>
<th>Description</th>
<th>Configuration requise et recommandations</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Témoin de disque</td>
<td>- LUN dédié qui stocke une copie de la base de données de cluster<br />
- Particulièrement utiles pour les clusters avec un stockage partagé (pas répliqué)</td>
<td>- Taille de LUN doit être au moins 512 Mo<br />
- Doit être dédié à l’utilisation de cluster et pas affectés à un rôle en cluster<br />
- Doit être inclus dans le cluster stockage et passe de stockage des tests de validation<br />
- Ne peut pas être un disque qui est un Volume de partagé de Cluster (CSV)<br />
- Disque de base avec un volume unique<br />
- N’a pas besoin de disposer d’une lettre de lecteur<br />
- Peuvent être formatés en NTFS ou ReFS<br />
- Peut être configuré facultativement avec le matériel RAID tolérance de pannes<br />
- Doivent être exclus des sauvegardes et l’analyse antivirus<br />
- Un témoin de disque n’est pas pris en charge avec les espaces de stockage Direct</td>
</tr>
<tr class="even">
<td>Témoin de partage de fichiers</td>
<td>- Partage de fichiers SMB qui est configurée sur un serveur de fichiers exécutant Windows Server<br />
- Ne stocke pas une copie de la base de données de cluster<br />
- Conserve les informations de cluster uniquement dans un fichier witness.log<br />
- Particulièrement utiles pour les clusters multisite avec un stockage répliqué</td>
<td>- Doit avoir un minimum de 5 Mo d’espace libre<br />
- Doit être dédié au cluster unique et pas utilisé pour stocker les données utilisateur ou l’application<br />
- Doit avoir un accès en écriture activé pour l’objet ordinateur pour le nom du cluster<br />
<br />
Voici quelques autres considérations sur un serveur de fichiers qui héberge le témoin de partage de fichiers:<br />
<br />
- Un seul serveur de fichiers peut être configuré avec témoins de partage de fichiers pour plusieurs clusters.<br />
- Le serveur de fichiers doit se trouver sur un site distinct à partir de la charge de travail du cluster. Cela permet d’égalité professionnelle pour n’importe quel site de cluster résister cas de perte de communication réseau de site à site. Si le serveur de fichiers se trouve sur le même site, ce site devient le site principal, et il est le seul site pouvant atteindre le partage de fichiers.<br />
- Le serveur de fichiers peut s’exécuter sur une machine virtuelle si la machine virtuelle n’est pas hébergée sur le même cluster qui utilise le témoin de partage de fichiers.<br />
- Pour une haute disponibilité, le serveur de fichiers peut être configuré sur un cluster de basculement distinct.</td>
</tr>

<tr class-"odd">
<td>Témoin cloud</td>
<td>- Un fichier de témoin stocké dans le stockage blob Azure<br>
-Recommandé lorsque tous les serveurs du cluster ont une connexion Internet fiable.</td>
<td>Voir <a href="https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness">déployer un témoin de cloud</a>.</td>
<td>
</td>
</tr>

</tbody>
</table>

### Affectation de nœud de voix

En tant qu’une option de configuration avancée de quorum, vous pouvez choisir d’attribuer ou supprimer des voix de quorum sur une base par nœud. Par défaut, tous les nœuds sont attribués à une voix. Quel que soit l’affectation de la voix, tous les nœuds continuent à fonctionner dans le cluster, de recevoir des mises à jour de base de données de cluster et peuvent héberger des applications.

Vous souhaiterez peut-être supprimer des voix de nœuds dans certaines configurations de récupération d’urgence. Par exemple, dans un cluster multisite, vous pouvez supprimer les voix à partir des nœuds dans un site de sauvegarde afin que ces nœuds n’affectent pas les calculs de quorum. Cette configuration est recommandée uniquement pour un basculement manuel sur plusieurs sites. Pour plus d’informations, consultez [les considérations de Quorum pour les configurations de récupération d’urgence](#quorum-considerations-for-disaster-recovery-configurations) plus loin dans cette rubrique.

Le vote configuré d’un nœud peut être vérifié en fonction de la propriété commune **NodeWeight** du nœud de cluster à l’aide de l’applet de commande [Get-ClusterNode](http://technet.microsoft.com/library/hh847268.aspx)Windows PowerShell. La valeur 0 indique que le nœud n’a pas un vote quorum configuré. Une valeur égale à 1 indique que la voix de quorum du nœud est affectée, et il est géré par le cluster. Pour plus d’informations sur la gestion de nœud voix, voir [gestion de quorum dynamique](#dynamic-quorum-management) plus loin dans cette rubrique.

L’affectation votez pour tous les nœuds de cluster peut être vérifiée en utilisant le test de validation de **Valider Quorum du Cluster** .

#### Autres considérations sur nœud votent attribution d’adresses

  - Affectation de nœud de voix n’est pas recommandée de mettre en œuvre un nombre de nœuds votants dimension non standard. Au lieu de cela, vous devez configurer un témoin de disque ou un témoin de partage de fichiers. Pour plus d’informations, voir [configuration témoin](#witness-configuration) plus loin dans cette rubrique.
  - Si la gestion de quorum dynamique est activée, seuls les nœuds qui sont configurés pour que la voix de nœud affecté peuvent avoir leurs votes affectés ou supprimés de manière dynamique. Pour plus d’informations, voir [gestion de quorum dynamique](#dynamic-quorum-management) plus loin dans cette rubrique.

### Gestion de quorum dynamique

Dans Windows Server 2012, comme une option de configuration avancée de quorum, vous pouvez choisir activer la gestion de quorum dynamique par cluster. Pour plus d’informations sur le fonctionnement de quorum comment dynamique, consultez [cette explication](../storage/storage-spaces/understand-quorum.md#dynamic-quorum-behavior).

Avec la gestion de quorum dynamique, il est également possible pour un cluster à s’exécuter sur le dernier nœud de cluster restants. En ajustant dynamiquement l’exigence de majorité de quorum, le cluster peut supporter arrêts séquentielle nœud à un nœud unique.

Le vote dynamique affectée par le cluster d’un nœud peut être vérifié avec la propriété commune **DynamicWeight** du nœud de cluster à l’aide de l’applet de commande [Get-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode?view=win10-ps) Windows PowerShell. La valeur 0 indique que le nœud n’a pas un vote de quorum. Une valeur égale à 1 indique que le nœud a un vote de quorum.

L’affectation votez pour tous les nœuds de cluster peut être vérifiée en utilisant le test de validation de **Valider Quorum du Cluster** .

#### Considérations supplémentaires pour la gestion de quorum dynamique

- Gestion de quorum dynamique n’autorise pas le cluster assurer une défaillance simultanée de la majorité des membres votants. Pour continuer à s’exécuter, le cluster doit toujours avoir la majorité quorum au moment d’une défaillance ou l’arrêt du nœud.

- Si vous avez supprimé explicitement la voix d’un nœud, le cluster ne peut pas dynamiquement ajouter ou supprimer ce vote.
- Espaces de stockage Direct est activé, le cluster peut uniquement prendre en charge deux défaillances de nœud. Cela est expliqué plus dans la [section de quorum pool](../storage/storage-spaces/understand-quorum.md#poolQuorum)

## Recommandations générales pour la configuration de quorum

Le logiciel du cluster configure automatiquement le quorum pour un nouveau cluster, en fonction du nombre de nœuds configurés et la disponibilité du stockage partagé. Il s’agit généralement de la configuration de quorum la plus appropriée pour ce cluster. Toutefois, il est conseillé de passer en revue la configuration de quorum après la création du cluster, avant de passer du cluster en environnement de production. Pour afficher la configuration de quorum du cluster détaillées, vous pouvez utiliser le valider un Assistant de Configuration ou l’applet de commande [Test-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) Windows PowerShell, pour exécuter le test de **Valider la Configuration de Quorum** . Dans le Gestionnaire de Cluster de basculement, la configuration de quorum de base est affichée dans les informations concernant le cluster sélectionné, ou vous pouvez passer en revue les informations sur les ressources de quorum qui renvoie lorsque vous exécutez la commande [Get-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusterquorum?view=win10-ps) Windows PowerShell applet de commande.

À tout moment, vous pouvez exécuter le test de **Valider la Configuration de Quorum** pour valider que la configuration de quorum est optimale pour votre cluster. La sortie de test indique si une modification apportée à la configuration de quorum est recommandée et les paramètres qui sont optimales. Si une modification est recommandée, vous pouvez utiliser l’Assistant Configuration de Quorum de Cluster pour appliquer les paramètres recommandés.

Une fois le cluster en production, ne modifiez pas la configuration de quorum, sauf si vous avez déterminé que la modification est appropriée pour votre cluster. Vous souhaiterez peut-être prendre en compte la modification de la configuration de quorum dans les situations suivantes:

- Ajouter ou supprimer des nœuds
- Ajout ou la suppression de stockage
- Un échec de nœud ou un témoin à long terme
- Récupération d’un cluster dans un scénario de récupération d’urgence multisite

Pour plus d’informations sur la validation d’un cluster de basculement, voir [Valider le matériel pour un Cluster de basculement](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

## Configurer le quorum du cluster

Vous pouvez configurer les paramètres quorum de cluster à l’aide de gestionnaire du Cluster de basculement ou les applets de commande Windows PowerShell Clusters de basculement.

>[!IMPORTANT]
>Il est généralement préférable d’utiliser la configuration de quorum recommandé par l’Assistant Configuration de Quorum du Cluster. Nous vous recommandons de personnalisation de la configuration de quorum uniquement si vous avez déterminé que la modification est appropriée pour votre cluster. Pour plus d’informations, voir [les recommandations générales pour la configuration de quorum](#general-recommendations-for-quorum-configuration) dans cette rubrique.

### Configurez les paramètres quorum de cluster

L’appartenance au groupe **administrateurs** local sur chaque serveur en cluster, ou l’équivalent est les autorisations minimales requises pour effectuer cette procédure. En outre, le compte que vous utilisez doit être un compte d’utilisateur de domaine.

>[!NOTE]
>Vous pouvez modifier la configuration de quorum de cluster sans arrêter les cluster ou prise de ressources de cluster en mode hors connexion.

### Modifier la configuration de quorum dans un cluster de basculement à l’aide du Gestionnaire du Cluster de basculement

1. Dans le Gestionnaire de Cluster de basculement, sélectionnez ou spécifiez le cluster que vous souhaitez modifier.
2. Avec le cluster sélectionné, sous **Actions**, sélectionnez **d’Autres Actions**, puis sélectionnez **Configurer les paramètres Quorum du Cluster**. L’Assistant Configuration de Quorum du Cluster s’affiche. Sélectionnez **Suivant**.
3. Sur la page **Sélectionner une Option de Configuration de Quorum** , sélectionnez l’une des trois options de configuration et effectuez les étapes de cette option. Avant de configurer les paramètres quorum, vous pouvez passer en revue vos choix. Pour plus d’informations sur les options, consultez l’article [vue d’ensemble du quorum dans un cluster de basculement](#overview-of-the-quorum-in-a-failover-cluster)plus haut dans cette rubrique.

    - Pour autoriser le cluster rétablir automatiquement les paramètres quorum qui sont idéales pour votre configuration actuelle du cluster, sélectionnez **utiliser les paramètres par défaut** , puis exécutez l’Assistant.
    - Pour ajouter ou modifier le témoin de quorum, sélectionnez **Ajouter ou modifier le témoin de quorum**et puis procédez comme suit. Pour des informations et des considérations sur la configuration d’un témoin de quorum, voir [configuration témoin](#witness-configuration) précédemment dans cette rubrique.

      1. Sur la page **Sélectionner un témoin de Quorum** , sélectionnez une option pour configurer un témoin de disque ou un témoin de partage de fichiers. L’Assistant indique les options de sélection de témoin qui sont recommandées pour votre cluster.

          >[!NOTE]
          >Vous pouvez également sélectionner **ne configurez pas un témoin de quorum** , puis exécutez l’Assistant. Si vous avez un nombre pair de vote nœuds dans votre cluster, cela ne peut pas être une configuration recommandée.

      2. Si vous sélectionnez l’option pour configurer un témoin de disque, sur la page **Configurer un témoin de stockage** , sélectionnez le volume de stockage que vous souhaitez affecter en tant que le témoin de disque et puis terminez l’Assistant.
      3. Si vous sélectionnez l’option pour configurer un témoin de partage de fichiers, sur la page **Configurer témoin de partage de fichiers** , tapez ou sélectionnez dans un partage de fichiers qui sera utilisé comme la ressource témoin, puis exécutez l’Assistant.

    - Pour configurer les paramètres de gestion de quorum et pour ajouter ou modifier le témoin de quorum, sélectionnez la **configuration de quorum avancées et une sélection témoin**et puis procédez comme suit. Pour des informations et des considérations sur les paramètres de configuration de quorum avancé, voir [nœud voter affectation](#node-vote-assignment) et [gestion de quorum dynamique](#dynamic-quorum-management) précédemment dans cette rubrique.

      1. Sur la page **Sélectionner une Configuration vote** , sélectionnez une option pour affecter des voix aux nœuds. Par défaut, tous les nœuds sont attribués à un vote. Toutefois, pour certains scénarios, vous pouvez attribuer une voix uniquement à un sous-ensemble des nœuds.

          >[!NOTE]
          >Vous pouvez également sélectionner des **Nœuds non**. Cela est généralement pas recommandé, car il n’autorise pas les nœuds à participer à vote de quorum, et elle nécessite la configuration d’un témoin de disque. Ce témoin de disque devient le point unique de défaillance pour le cluster.

      2. Sur la page **Configurer la gestion de Quorum** , vous pouvez activer ou désactiver l’option **Autoriser de cluster à gérer de manière dynamique l’affectation du nœud votes** . En général, cette option augmente la disponibilité du cluster. L’option est activée par défaut, et il est fortement recommandé de ne pas désactiver cette option. Cette option permet au cluster de continuer à s’exécuter dans les scénarios de panne qui ne sont pas possibles lorsque cette option est désactivée.
      3. Sur la page **Sélectionner un témoin de Quorum** , sélectionnez une option pour configurer un témoin de disque ou un témoin de partage de fichiers. L’Assistant indique les options de sélection de témoin qui sont recommandées pour votre cluster.

          >[!NOTE]
          >Vous pouvez également sélectionner **ne configurez pas un témoin de quorum**, puis exécutez l’Assistant. Si vous avez un nombre pair de vote nœuds dans votre cluster, cela ne peut pas être une configuration recommandée.

      4. Si vous sélectionnez l’option pour configurer un témoin de disque, sur la page **Configurer un témoin de stockage** , sélectionnez le volume de stockage que vous souhaitez affecter en tant que le témoin de disque et puis terminez l’Assistant.
      5. Si vous sélectionnez l’option pour configurer un témoin de partage de fichiers, sur la page **Configurer témoin de partage de fichiers** , tapez ou sélectionnez dans un partage de fichiers qui sera utilisé comme la ressource témoin, puis exécutez l’Assistant.

4. Sélectionnez **Suivant**. Confirmez vos sélections sur la page de confirmation qui s’affiche et sélectionnez **suivant**.

Une fois que l’Assistant s’exécute et la page **Résumé** apparaît, si vous souhaitez afficher un rapport des tâches effectuées par l’Assistant, sélectionnez **Afficher le rapport**. Le dernier rapport restera dans la *systemroot *** \\Cluster\\Reports** dossier portant le nom **QuorumConfiguration.mht**.

>[!NOTE]
>Après avoir configuré le quorum du cluster, nous vous recommandons d’exécuter le test de **Valider la Configuration de Quorum** pour vérifier les paramètres quorum mis à jour.

### Commandes équivalentes de Windows PowerShell

Les exemples suivants montrent comment utiliser l’applet de commande [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum?view=win10-ps) et autres applets de commande Windows PowerShell pour configurer le quorum du cluster.

L’exemple suivant modifie la configuration de quorum de cluster *CONTOSO-FC1* à configuration majorité nœud simple avec aucun témoin de quorum.

```PowerShell
Set-ClusterQuorum –Cluster CONTOSO-FC1 -NodeMajority
```

L’exemple suivant modifie la configuration de quorum sur le cluster local à un nœud majoritaire avec la configuration de témoin. La ressource de disque nommée *2 de disque de Cluster* est configurée comme un témoin de disque.

```PowerShell
Set-ClusterQuorum -NodeAndDiskMajority "Cluster Disk 2"
```

L’exemple suivant modifie la configuration de quorum sur le cluster local à un nœud majoritaire avec la configuration de témoin. La ressource de partage de fichier nommée *\\\CONTOSO-FS\\fsw* est configurée comme un témoin de partage de fichiers.

```PowerShell
Set-ClusterQuorum -NodeAndFileShareMajority "\\fileserver\fsw"
```

L’exemple suivant supprime le vote de quorum nœud *ContosoFCNode1* sur le cluster local.

```PowerShell
(Get-ClusterNode ContosoFCNode1).NodeWeight=0
```

L’exemple suivant ajoute le vote de quorum au nœud *ContosoFCNode1* sur le cluster local.

```PowerShell
(Get-ClusterNode ContosoFCNode1).NodeWeight=1
```

L’exemple suivant permet à la propriété **DynamicQuorum** du cluster *CONTOSO-FC1* (s’il a été précédemment désactivé):

```PowerShell
(Get-Cluster CONTOSO-FC1).DynamicQuorum=1
```

## Récupérer un cluster en démarrant sans quorum

Un cluster qui n’a pas de suffisamment de votes quorum ne démarrera pas. Dans un premier temps, vous devez toujours vérifier la configuration de quorum de cluster et examiner la raison pour laquelle le cluster n’a plus quorum. Cela peut se produire si vous avez des nœuds qui a cessé de répondre, ou si le site principal n’est pas accessible dans un cluster multisite. Une fois que vous avez identifié la cause de l’échec du cluster, vous pouvez utiliser les étapes de récupération décrites dans cette section.

>[!NOTE]
> * Si le service de Cluster s’arrête, car le quorum est perdu, 1177 d’ID d’événement apparaît dans le journal système.
> * Il est toujours nécessaire d’étudier la raison pour laquelle le quorum du cluster a été perdu.
> * Il est toujours préférable à insérer un témoin de quorum ou de nœud à un état Sain (rejoindre le cluster) au lieu de démarrage du cluster sans quorum.

### Démarrage forcé de nœuds de cluster

Une fois que vous vous apercevez que vous ne pouvez pas récupérer votre cluster en mettant les nœuds ou un témoin de quorum à un état sain, forcer votre cluster à démarrer devient nécessaire. Forcer le cluster à démarrer remplace vos paramètres de configuration de quorum de cluster et démarre le cluster en mode **ForceQuorum** .

Forcer un cluster à démarrer lorsqu’elle n’a pas de quorum peut être particulièrement utile dans un cluster multisite. Imaginez un scénario de récupération d’urgence avec un cluster qui contient physiquement séparé sites principales et secondaire, *SiteA* et *SiteB*. En cas d’incident authentique à *SiteA*, elle peut prendre un certain temps pour le site revenir en ligne. Vous pouvez probablement forcer *SiteB* à venir en ligne, même si elle n’a pas de quorum.

Quand un cluster est démarré en mode **ForceQuorum** , après que elle reprend le quorum suffisant votes le cluster laisse automatiquement l’état forcée, et, elle se comporte normalement. Par conséquent, il n’est pas nécessaire de démarrer le cluster à nouveau normalement. Si le cluster perd un nœud, il perd le quorum, elle passe en mode hors connexion à nouveau, car il n’est plus dans l’état forcée. Pour le mettre en ligne lorsqu’elle n’a pas de quorum nécessite forcée démarrer sans quorum du cluster.

>[!IMPORTANT]
>* Une fois un cluster force démarré, l’administrateur est dans un contrôle total du cluster.
>* Le cluster utilise la configuration du cluster sur le nœud où le cluster est force démarré et réplique sur tous les autres nœuds qui sont disponibles.
>* Si vous forcez le démarrer sans quorum du cluster, tous les paramètres de configuration de quorum sont ignorées tandis que le cluster reste en mode **ForceQuorum** . Cela inclut les attributions de vote nœud spécifique et les paramètres de gestion de quorum dynamique.

### Empêcher le quorum sur les nœuds de cluster restants

Une fois que vous avez force en main du cluster sur un nœud, il est nécessaire démarrer les nœuds restants dans votre cluster avec un paramètre pour empêcher le quorum. Un nœud démarré avec un paramètre qui empêche le quorum indique au service de Cluster pour joindre un cluster existant en cours d’exécution au lieu de formation d’une nouvelle instance de cluster. Cela empêche les nœuds restants de former un fractionnement cluster qui contient deux instances de concurrents.

Cela devient nécessaire lorsque vous avez besoin de récupérer votre cluster dans certains scénarios de récupération d’urgence multisite une fois que vous avez force démarré le cluster sur votre site de sauvegarde, *SiteB*. Pour joindre la force démarrage cluster dans *SiteB*, les nœuds de votre site principal, *SiteA*, devra être démarrée avec le quorum a empêché.

>[!IMPORTANT]
>Une fois un cluster force démarré sur un nœud, nous vous recommandons de toujours commencer les nœuds restants avec le quorum a empêché.

Voici comment récupérer le cluster avec le Gestionnaire du Cluster de basculement:

1. Dans le Gestionnaire de Cluster de basculement, sélectionnez ou spécifiez le cluster que vous souhaitez récupérer.
2. Avec le cluster sélectionné, sous **Actions**, sélectionnez **Force le démarrage du Cluster**.

    Le Gestionnaire du Cluster de basculement force démarre le cluster sur tous les nœuds qui sont accessibles. Le cluster utilise la configuration actuelle du cluster lors du démarrage.

>[!NOTE]
>* Pour forcer le cluster à démarrer sur un nœud spécifique qui contient une configuration de cluster que vous souhaitez utiliser, vous devez utiliser les applets de commande Windows PowerShell ou des outils de ligne de commande équivalentes présenté après cette procédure. 
> * Si vous utilisez le Gestionnaire du Cluster de basculement pour se connecter à un cluster qui est force démarré, et l’action de **Démarrer le Service de Cluster** vous permet de démarrer un nœud, le nœud est automatiquement démarré avec le paramètre qui empêche le quorum.

#### Commandes équivalentes de Windows PowerShell (Start-Clusternode)

L’exemple suivant montre comment utiliser l’applet de commande **Start-ClusterNode** pour forcer l’écran de démarrage du cluster sur le nœud *ContosoFCNode1*.

```PowerShell
Start-ClusterNode –Node ContosoFCNode1 –FQ
```

Par ailleurs, vous pouvez taper la commande suivante localement sur le nœud:

```PowerShell
Net Start ClusSvc /FQ
```

L’exemple suivant montre comment utiliser l’applet de commande **Start-ClusterNode** pour démarrer le service de Cluster avec le quorum bloquée sur le nœud *ContosoFCNode1*.

```PowerShell
Start-ClusterNode –Node ContosoFCNode1 –PQ
```

Par ailleurs, vous pouvez taper la commande suivante localement sur le nœud:

```PowerShell
Net Start ClusSvc /PQ
```

## Considérations de quorum pour les configurations de récupération d’urgence

Cette section résume les caractéristiques et des configurations de quorum pour deux configurations de cluster multisite dans les déploiements de récupération d’urgence. Les instructions de configuration de quorum diffèrent selon si vous avez besoin le basculement automatique ou basculement manuel pour les charges de travail entre les sites. Votre configuration dépend généralement les contrats de niveau service (SLA) qui sont présentes dans votre organisation pour fournir et prendre en charge les charges de travail en cluster en cas d’une défaillance ou un incident sur un site.

### Basculement automatique

Dans cette configuration, le cluster se compose de deux ou plusieurs sites qui peuvent héberger les rôles en cluster. Si un échec se produit à n’importe quel site, les rôles en cluster sont censées basculent automatiquement vers les sites restants. Par conséquent, le quorum du cluster doit être configuré pour que n’importe quel site peut supporter une défaillance du site complet.

Le tableau suivant résume les considérations et recommandations pour cette configuration.

<table>
<thead>
<tr class="header">
<th>Élément</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Nombre de nœud votes par site</td>
<td>Doit être égale</td>
</tr>
<tr class="even">
<td>Affectation de nœud de voix</td>
<td>Nœud voix ne doit pas être supprimé, car tous les nœuds sont tout aussi importants</td>
</tr>
<tr class="odd">
<td>Gestion de quorum dynamique</td>
<td>Doit être activée</td>
</tr>
<tr class="even">
<td>Configuration de rappel</td>
<td>Témoin de partage de fichiers est recommandé, configurés dans un site qui se distingue les sites de cluster</td>
</tr>
<tr class="odd">
<td>Charges de travail</td>
<td>Charges de travail peuvent être configurés sur une des sites</td>
</tr>
</tbody>
</table>

#### Autres considérations sur le basculement automatique

- Configuration du témoin de partage de fichiers dans un site distinct est nécessaire pour permettre à chaque site égal pour résister. Pour plus d’informations, voir [configuration témoin](#witness-configuration) précédemment dans cette rubrique.

### Basculement manuel

Dans cette configuration, le cluster se compose d’un site principal et un site de sauvegarde (reprise), *SiteB* *SiteA*. Rôles en cluster sont hébergés sur *SiteA*. En raison de la configuration de quorum du cluster, si un échec se produit au *SiteA*, tous les nœuds du cluster cesse de fonctionner. Dans ce scénario l’administrateur doit manuellement basculer les services de cluster à *SiteB* et exécuter des tâches supplémentaires pour récupérer le cluster.

Le tableau suivant résume les considérations et recommandations pour cette configuration.

<table>
<thead>
<tr class="header">
<th>Élément</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Nombre de nœud votes par site</td>
<td>Peut être différent</td>
</tr>
<tr class="even">
<td>Affectation de nœud de voix</td>
<td>- Nœud votes ne doit pas être supprimée de nœuds sur le site principal, <em>SiteA</em><br />
- Nœud votes doit être supprimée de nœuds sur le site de sauvegarde, <em>SiteB</em><br />
- Si une panne à long terme se produit au <em>SiteA</em>, voix doit être affecté à des nœuds à <em>SiteB</em> pour activer une majorité de quorum sur ce site dans le cadre de la récupération</td>
</tr>
<tr class="odd">
<td>Gestion de quorum dynamique</td>
<td>Doit être activée</td>
</tr>
<tr class="even">
<td>Configuration de rappel</td>
<td>- Configurer un témoin s’il existe un nombre pair des nœuds à <em>SiteA</em><br />
- Si un témoin est nécessaire, configurer un témoin de partage de fichiers ou un témoin de disque qui est accessible uniquement aux nœuds de <em>SiteA</em> (parfois appelé un témoin de disque asymétrique)</td>
</tr>
<tr class="odd">
<td>Charges de travail</td>
<td>Utiliser des propriétaires favoris pour que les charges de travail en cours d’exécution sur les nœuds au <em>SiteA</em></td>
</tr>
</tbody>
</table>

#### Considérations supplémentaires pour un basculement manuel

- Seuls les nœuds à *SiteA* sont initialement configurés avec voix de quorum. Cela est nécessaire pour vous assurer que l’état des nœuds à *SiteB* n’affecte pas le quorum du cluster.
- Étapes de récupération peuvent varier en fonction de si *SiteA* élevées d’une défaillance temporaire ou à long terme.

## Informations supplémentaires

* [Clustering de basculement](failover-clustering.md)
* [Applets de commande Windows PowerShell Clusters de basculement](https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps)
* [Cluster de comprendre et de Quorum de Pool](../storage/storage-spaces/understand-quorum.md)