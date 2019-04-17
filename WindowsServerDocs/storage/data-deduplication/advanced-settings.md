---
ms.assetid: 01c8cece-66ce-4a83-a81e-aa6cc98e51fc
title: "Paramètres de déduplication des données avancés"
ms.prod: windows-server-threshold
ms.technology: storage-deduplication
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/15/2016
ms.openlocfilehash: 15cfc054810a2cab85aae9a04d6195c3ae6fe0b9
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="advanced-data-deduplication-settings"></a>Paramètres de déduplication des données avancés

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016

Ce document décrit la manière de modifier les paramètres de [déduplication des données](overview.md) avancés. Pour les [charges de travail recommandées](install-enable.md#enable-dedup-candidate-workloads), les paramètres par défaut peuvent suffire. La principale raison de modifier ces paramètres est d’améliorer les performances de la déduplication des données avec d’autres types de charges de travail.

## <a id="modifying-job-schedules"></a>Modification des planifications de travaux de déduplication des données
Les [planifications de travaux de déduplication des données](understand.md#job-info) sont conçues pour fonctionner parfaitement pour les charges de travail recommandées et être le moins intrusives possible (mis à part le travail d’*optimisation des priorités* qui est activé pour le type d’utilisation [**Sauvegarde**](understand.md#usage-type-backup)). Quand les charges de travail ont des besoins en ressources importants, il est possible de vérifier que les travaux s’exécutent uniquement pendant les heures d’inactivité, ou de réduire ou augmenter la quantité de ressources système qu’un travail de déduplication des données est autorisé à consommer.

### <a id="modifying-job-schedules-change-schedule"></a>Modification d’une planification de déduplication des données
Les travaux de déduplication des données sont planifiés par le biais du Planificateur de tâches de Windows. Vous pouvez les afficher et les modifier sous le chemin Microsoft\Windows\Deduplication. La déduplication des données comprend plusieurs applets de commande qui facilitent la planification.
* [`Get-DedupSchedule`](https://technet.microsoft.com/library/hh848446.aspx) présente les travaux planifiés actuels.
* [`New-DedupSchedule`](https://technet.microsoft.com/library/hh848445.aspx) crée un travail planifié.
* [`Set-DedupSchedule`](https://technet.microsoft.com/library/hh848447.aspx) modifie un travail planifié existant.
* [`Remove-DedupSchedule`](https://technet.microsoft.com/library/hh848451.aspx) supprime un travail planifié.

Le motif de modification le plus courant pendant l’exécution de travaux de déduplication des données consiste à garantir que les travaux s’exécutent pendant les heures creuses. L’exemple pas à pas suivant montre comment modifier la planification de déduplication des données pour un scénario *idéal*: un hôte Hyper-V hyperconvergé qui est inactif le week-end et après 19h00 tous les soirs de la semaine. Pour modifier la planification, exécutez les applets de commande PowerShell suivantes dans un contexte d’administrateur.

1. Désactivez les travaux d’[optimisation](understand.md#job-info-optimization) planifiés toutes les heures.  
    ```PowerShell
    Set-DedupSchedule -Name BackgroundOptimization -Enabled $false
    Set-DedupSchedule -Name PriorityOptimization -Enabled $false
    ```

2. Supprimez les travaux [Nettoyage de la mémoire](understand.md#job-info-gc) et [Nettoyage des données](understand.md#job-info-scrubbing) actuellement planifiés.
    ```PowerShell
    Get-DedupSchedule -Type GarbageCollection | ForEach-Object { Remove-DedupSchedule -InputObject $_ }
    Get-DedupSchedule -Type Scrubbing | ForEach-Object { Remove-DedupSchedule -InputObject $_ }
    ```

3. Créez un travail d’optimisation nocturne qui s’exécute à 19h00 avec une priorité élevée et tous les processeurs et la mémoire disponibles sur le système.
    ```PowerShell
    New-DedupSchedule -Name "NightlyOptimization" -Type Optimization -DurationHours 11 -Memory 100 -Cores 100 -Priority High -Days @(1,2,3,4,5) -Start (Get-Date "2016-08-08 19:00:00")
    ```

    >[!NOTE]  
    > La partie *date* du `System.Datetime` fourni à `-Start` n’est pas pertinente (dès lors qu’elle est passée), mais la partie *heure* spécifie le moment auquel le travail doit démarrer.
4. Créez un travail de nettoyage de la mémoire hebdomadaire qui s’exécute le samedi à partir de 7h00 avec une priorité élevée et tous les processeurs et la mémoire disponibles sur le système.
    ```PowerShell
    New-DedupSchedule -Name "WeeklyGarbageCollection" -Type GarbageCollection -DurationHours 23 -Memory 100 -Cores 100 -Priority High -Days @(6) -Start (Get-Date "2016-08-13 07:00:00")
    ```

5. Créez un travail de nettoyage des données hebdomadaire qui s’exécute le dimanche à partir de 7h00 avec une priorité élevée et tous les processeurs et la mémoire disponibles sur le système.
    ```PowerShell
    New-DedupSchedule -Name "WeeklyIntegrityScrubbing" -Type Scrubbing -DurationHours 23 -Memory 100 -Cores 100 -Priority High -Days @(0) -Start (Get-Date "2016-08-14 07:00:00")
    ```

### <a id="modifying-job-schedules-available-settings"></a>Paramètres disponibles à l’échelle du travail
Vous pouvez activer les paramètres suivants pour les travaux de déduplication des données nouveaux ou planifiés:

<table>
    <thead>
        <tr>
            <th style="min-width:125px">Nom du paramètre</th>
            <th>Définition</th>
            <th>Valeurs acceptées</th>
            <th>Pourquoi définir cette valeur?</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Type</td>
            <td>Type du travail à planifier</td>
            <td>
                <ul>
                    <li>Optimisation</li>
                    <li>GarbageCollection</li>
                    <li>Nettoyage</li>
                </ul>
            </td>
            <td>Cette valeur est requise parce qu’il s’agit du type de travail que vous voulez planifier. Cette valeur n’est pas modifiable une fois que la tâche a été planifiée.</td>
        </tr>
        <tr>
            <td>Priority</td>
            <td>Priorité système du travail planifié</td>
            <td>
                <ul>
                    <li>Élevé</li>
                    <li>Moyen</li>
                    <li>Faible</li>
                </ul>
            </td>
            <td>Cette valeur aide le système à déterminer comment allouer du temps processeur. *Haute* utilise plus de temps processeur, *faible* en utilise moins.</td>
        </tr>
        <tr>
            <td>Days</td>
            <td>Jours du travail planifié</td>
            <td>Une plage d’entiers compris entre 0 et 6 représente les jours de la semaine:<ul>
                <li>0 = Dimanche</li>
                <li>1 = Lundi</li>
                <li>2 = Mardi</li>
                <li>3 = Mercredi</li>
                <li>4 = Jeudi</li>
                <li>5 = Vendredi</li>
                <li>6 = Samedi</li>
            </ul></td>
            <td>Les tâches planifiées doivent s’exécuter au moins un jour.</td>
        </tr>
        <tr>
            <td>Cœurs</td>
            <td>Pourcentage de cœurs présents sur le système que doit utiliser un travail</td>
            <td>Entiers de 0 à 100 (indique un pourcentage)</td>
            <td>Pour contrôler le niveau d’impact d’un travail sur les ressources de calcul sur le système</td>
        </tr>
        <tr>
            <td>DurationHours</td>
            <td>Nombre maximal d’heures pendant lesquelles un travail doit être autorisé à s’exécuter</td>
            <td>Entiers positifs</td>
            <td>Pour empêcher un travail de s’exécuter pendant les heures d’activité d’une charge de travail</td>
        </tr>
        <tr>
            <td>Activé</td>
            <td>Indique si le travail sera exécuté</td>
            <td>Vrai/Faux</td>
            <td>Pour désactiver un travail sans le supprimer</td>
        </tr>
        <tr>
            <td>Complet</td>
            <td>Pour planifier un travail de nettoyage de la mémoire complet</td>
            <td>Commutateur (vrai/faux)</td>
            <td>Par défaut, chaque quatrième travail est un travail de nettoyage de la mémoire complet. Avec ce commutateur, vous pouvez planifier un nettoyage de la mémoire complet pour qu’il s’effectue plus souvent.</td>
        </tr>
        <tr>
            <td>InputOutputThrottle</td>
            <td>Spécifie la limite d’entrées/sorties appliquée au travail</td>
            <td>Entiers de 0 à 100 (indique un pourcentage)</td>
            <td>La limitation permet de s’assurer que les travaux n’interfèrent pas avec d’autres processus gourmands en E/S.</td>
        </tr>
        <tr>
            <td>Memory</td>
            <td>Pourcentage de mémoire sur le système que doit utiliser un travail</td>
            <td>Entiers de 0 à 100 (indique un pourcentage)</td>
            <td>Pour contrôler le niveau d’impact d’un travail sur les ressources mémoire du système</td>
        </tr>
        <tr>
            <td>Nom</td>
            <td>Nom du travail planifié</td>
            <td>String</td>
            <td>Un travail doit porter un nom identifiable de façon unique.</td>
        </tr>
        <tr>
            <td>ReadOnly</td>
            <td>Indique que le travail de nettoyage des données traite et signale les altérations qu’il trouve, mais n’exécute pas les actions de réparation.</td>
            <td>Commutateur (vrai/faux)</td>
            <td>Vous voulez restaurer manuellement les fichiers qui se trouvent sur des sections incorrectes du disque.</td>
        </tr>
        <tr>
            <td>Démarrer</td>
            <td>Spécifie l’heure de début d’un travail</td>
            <td>`System.DateTime`</td>
            <td>La partie *date* du `System.Datetime` fourni à *Démarrer* n’est pas pertinente (dès lors qu’elle est passée), mais la partie *heure* spécifie le moment auquel le travail doit démarrer.</td>
        </tr>
        <tr>
            <td>StopWhenSystemBusy</td>
            <td>Spécifie si la déduplication des données doit s’arrêter si le système est occupé</td>
            <td>Commutateur (Vrai/Faux)</td>
            <td>Ce commutateur vous permet de contrôler le comportement de la déduplication des données: ce contrôle s’avère particulièrement important si vous voulez exécuter la déduplication des données pendant que votre charge de travail n’est pas inactive.</td>
        </tr>
    </tbody>
</table>

## <a id="modifying-volume-settings"></a>Modification des paramètres de déduplication des données à l’échelle du volume
### <a id="modifying-volume-settings-how-to-toggle"></a>Activation/désactivation des paramètres de volume
Vous pouvez définir les paramètres par défaut à l’échelle du volume de la déduplication des données par le biais du [type d’utilisation](understand.md#usage-type) que vous sélectionnez quand vous activez une déduplication pour un volume. La déduplication des données comprend des applets de commande qui facilitent la modification des paramètres à l’échelle du volume:

* [`Get-DedupVolume`](https://technet.microsoft.com/library/hh848448.aspx)
* [`Set-DedupVolume`](https://technet.microsoft.com/library/hh848438.aspx)

Les principales raisons de modifier les paramètres de volume du type d’utilisation sélectionné sont d’améliorer les performances de lecture de certains fichiers spécifiques (par exemple, des fichiers multimédias ou d’autres types de fichiers déjà compressés) ou d’ajuster la déduplication des données en vue d’optimiser votre charge de travail particulière. L’exemple suivant montre comment modifier les paramètres de volume de la déduplication des données pour une charge de travail qui ressemble de près à une charge de travail de serveur de fichiers à usage général, mais qui utilise des fichiers volumineux souvent modifiés.

1. Consultez les paramètres de volume actuels du volume partagé de cluster1.
    ```PowerShell
    Get-DedupVolume -Volume C:\ClusterStorage\Volume1 | Select *
    ```

2. Activez OptimizePartialFiles sur le volume partagé de cluster1 pour que la stratégie MinimumFileAge s’applique aux sections du fichier plutôt qu’à l’intégralité du fichier. Ainsi, la majorité du fichier est optimisée même si les sections du fichier changent régulièrement.
    ```PowerShell
    Set-DedupVolume -Volume C:\ClusterStorage\Volume1 -OptimizePartialFiles
    ```

### <a id="modifying-volume-settings-available-settings"></a>Paramètres disponibles à l’échelle du volume
<table>
    <thead>
        <tr>
            <th style="min-width:125px">Nom du paramètre</th>
            <th>Définition</th>
            <th>Valeurs acceptées</th>
            <th>Pourquoi modifier cette valeur?</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ChunkRedundancyThreshold</td>
            <td>Nombre de fois qu’un bloc est référencé avant d’être dupliqué dans la section de la zone réactive du magasin de blocs. La valeur de la section de la zone réactive correspond aux blocs «à chaud» souvent référencés qui ont plusieurs chemins pour améliorer les temps d’accès.</td>
            <td>Entiers positifs</td>
            <td>La principale raison de modifier ce nombre est d’accroître le taux de réduction pour les volumes à duplication élevée. En général, la valeur par défaut (100) est le paramètre recommandé et vous ne devez pas la modifier.</td>
        </tr>
        <tr>
            <td>ExcludeFileType</td>
            <td>Types de fichiers exclus de l’optimisation</td>
            <td>Tableau des extensions de fichiers</td>
            <td>Certains types de fichiers, en particulier les fichiers multimédias ou les fichiers déjà compressés, ne tirent pas beaucoup parti d’une optimisation. Ce paramètre vous permet de configurer les types exclus.</td>
        </tr>
        <tr>
            <td>ExcludeFolder</td>
            <td>Spécifie les chemins de dossiers à ne pas considérer pour l’optimisation</td>
            <td>Tableau des chemins de dossiers</td>
            <td>Si vous voulez améliorer les performances ou empêcher le contenu de certains chemins d’être optimisé, vous pouvez exclure des chemins sur le volume pour ne pas les prendre en considération pour l’optimisation.</td>
        </tr>
        <tr>
            <td>InputOutputScale</td>
            <td>Spécifie le niveau de parallélisation des E/S (files d’attente d’E/S) que la déduplication des données doit utiliser sur un volume pendant un travail de post-traitement</td>
            <td>Entiers positifs compris entre 1 et 36</td>
            <td>La principale raison de modifier cette valeur est de réduire l’impact sur les performances d’une charge de travail à E/S élevées en limitant le nombre de files d’attente d’E/S que la déduplication des données est autorisée à utiliser sur un volume. Notez que la modification de la valeur par défaut de ce paramètre peut entraîner une exécution lente des travaux de post-traitement de la déduplication des données.</td>
        </tr>
        <tr>
            <td>MinimumFileAgeDays</td>
            <td>Nombre de jours après la création du fichier à partir duquel le fichier est considéré comme conforme à la stratégie d’optimisation.</td>
            <td>Entiers positifs (zéro compris)</td>
            <td>Les types d’utilisation **Par défaut** et **Hyper-v** définissent cette valeur à 3 pour maximiser les performances sur les fichiers à chaud ou récemment créés. Vous pouvez modifier cette valeur si vous voulez que la déduplication des données soit plus agressive ou si la latence supplémentaire associée à la déduplication ne vous dérange pas.</td>
        </tr>
        <tr>
            <td>MinimumFileSize</td>
            <td>Taille de fichier minimale qu’un fichier doit avoir pour être considéré comme conforme à la stratégie d’optimisation</td>
            <td>Entiers positifs (octets) supérieurs à 32Ko</td>
            <td>La principale raison de modifier cette valeur est d’exclure les petits fichiers dont la valeur d’optimisation est limitée pour économiser du temps de calcul.</td>
        </tr>
        <tr>
            <td>NoCompress</td>
            <td>Indique si les blocs doivent être compressées avant d’être placés dans le magasin de blocs</td>
            <td>Vrai/Faux</td>
            <td>Certains types de fichiers, notamment les fichiers multimédias et les types de fichiers déjà compressés, peuvent ne pas se compresser très bien. Ce paramètre vous permet de désactiver la compression de tous les fichiers sur le volume. Il s’avère idéal si vous optimisez un jeu de données qui comporte un grand nombre de fichiers déjà compressés.</td>
        </tr>
        <tr>
            <td>NoCompressionFileType</td>
            <td>Types de fichiers dont les blocs ne doivent pas être compressés avant d’aller dans le magasin de blocs</td>
            <td>Tableau des extensions de fichiers</td>
            <td>Certains types de fichiers, notamment les fichiers multimédias et les types de fichiers déjà compressés, peuvent ne pas se compresser très bien. Ce paramètre permet de désactiver la compression pour ces fichiers, ce qui économise des ressources processeur.</td>
        </tr>
        <tr>
            <td>OptimizeInUseFiles</td>
            <td>Quand ce paramètre est activé, les fichiers qui ont des descripteurs actifs sont considérés comme conformes à la stratégie d’optimisation.</td>
            <td>Vrai/Faux</td>
            <td>Activez ce paramètre si votre charge de travail garde des fichiers ouverts pendant de longues périodes. Si ce paramètre n’était pas activé, un fichier ne serait jamais optimisé si la charge de travail avait un descripteur ouvert, même si elle n’ajoutait qu’occasionnellement des données à la fin du fichier.</td>
        </tr>
        <tr>
            <td>OptimizePartialFiles</td>
            <td>Quand ce paramètre est activé, la valeur MinimumFileAge s’applique aux blocs d’un fichier plutôt qu’à l’intégralité du fichier.</td>
            <td>Vrai/Faux</td>
            <td>Activez ce paramètre si votre charge de travail fonctionne avec des fichiers volumineux, souvent modifiés, dont la plupart du contenu reste intacte. Si ce paramètre n’était pas activé, ces fichiers ne seraient jamais optimisés, car ils n’arrêteraient pas d’être modifiés, même si la plupart de leur contenu était prête à être optimisée.</td>
        </tr>
        <tr>
            <td>Vérifier</td>
            <td>Quand ce paramètre est activé, si le hachage d’un bloc correspond à un bloc déjà présent dans le magasin de blocs, les blocs sont comparés octet par octet pour vérifier qu’ils sont identiques.</td>
            <td>Vrai/Faux</td>
            <td>Il s’agit d’une fonctionnalité d’intégrité qui veille à ce que l’algorithme de hachage qui compare les blocs ne fasse pas d’erreur en comparant deux blocs de données qui sont en fait différents mais qui ont le même hachage. Dans la pratique, cette situation est extrêmement improbable. L’activation de la fonctionnalité de vérification surcharge considérablement le travail d’optimisation.</td>
        </tr>
    </tbody>
</table>

## <a id="modifying-dedup-system-settings"></a>Modification des paramètres de déduplication des données à l’échelle du système
La déduplication des données possède des paramètres à l’échelle du système supplémentaires que vous pouvez configurer par le biais du [Registre](https://technet.microsoft.com/library/cc755256(v=ws.11).aspx). Ces paramètres s’appliquent à tous les travaux et volumes qui s’exécutent sur le système. Une attention particulière doit être donnée à chaque modification du Registre.

Par exemple, vous voulez peut-être désactiver le nettoyage de la mémoire complet. Vous trouverez plus d’informations sur l’utilité d’une telle opération pour votre scénario dans le [Forum aux questions](#faq-why-disable-full-gc). Pour modifier le Registre avec PowerShell:

* Si la déduplication des données est en cours d’exécution dans un cluster:
    ```PowerShell
    Set-ItemProperty -Path HKLM:\System\CurrentControlSet\Services\ddpsvc\Settings -Name DeepGCInterval -Type DWord -Value 0xFFFFFFFF
    Set-ItemProperty -Path HKLM:\CLUSTER\Dedup -Name DeepGCInterval -Type DWord -Value 0xFFFFFFFF
    ```

* Si la déduplication des données n’est pas en cours d’exécution dans un cluster:
    ```PowerShell
    Set-ItemProperty -Path HKLM:\System\CurrentControlSet\Services\ddpsvc\Settings -Name DeepGCInterval -Type DWord -Value 0xFFFFFFFF
    ```

### <a id="modifying-dedup-system-settings-available-settings"></a>Paramètres disponibles à l’échelle du système
<table>
    <thead>
        <tr>
            <th style="min-width:125px">Nom du paramètre</th>
            <th>Définition</th>
            <th>Valeurs acceptées</th>
            <th>Pourquoi modifier ce paramètre?</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>WlmMemoryOverPercentThreshold</td>
            <td>Ce paramètre permet aux travaux d’utiliser plus de mémoire que la déduplication des données ne juge réellement disponible. Par exemple, un paramètre de 300 signifie que le travail doit utiliser le triple de la mémoire affectée pour être annulé.</td>
            <td>Entiers positifs (une valeur de 300 signifie 300% ou triple)</td>
            <td>Si vous avez une autre tâche qui s’arrête si la déduplication des données prend plus de mémoire</td>
        </tr>
        <tr>
            <td>DeepGCInterval</td>
            <td>Ce paramètre configure l’intervalle auquel les travaux de nettoyage de la mémoire réguliers deviennent des [travaux de nettoyage de la mémoire complet](advanced-settings.md#faq-full-v-regular-gc). Le paramètre n signifie que tous les n<sup>ièmes</sup> travaux sont un travail de nettoyage de la mémoire complet. Notez que le nettoyage de la mémoire complet est toujours désactivé (quelle que soit la valeur de Registre) pour les volumes avec le [Type d’utilisation Sauvegarde](understand.md#usage-type-backup). `Start-DedupJob -Type GarbageCollection -Full` peut être utilisée lorsqu'un nettoyage de la mémoire complet est souhaité sur un volume de sauvegarde.</td>
            <td>Entiers (-1=désactivé)</td>
            <td>Consultez cette [question du Forum aux questions](advanced-settings.md#faq-why-disable-full-gc).</td>
        </tr>
    </tbody>
</table>

## <a id="faq"></a>Forum aux questions
<a id="faq-use-responsibly"></a>**J’ai modifié un paramètre de déduplication des données et depuis, les travaux sont lents ou n’aboutissent pas, ou les performances de ma charge de travail ont diminué. Pourquoi?**  
Ces paramètres vous confèrent beaucoup de pouvoir sur l’exécution de la déduplication des données. Utilisez-les de manière responsable et [surveillez les performances](run.md#monitoring-dedup).

<a id="faq-running-dedup-jobs-manually"></a>**Je veux exécuter un travail de déduplication des données tout de suite, mais je ne veux pas créer une planification: est-ce possible?**  
Oui, [tous les travaux peuvent être exécutés manuellement](run.md#running-dedup-jobs-manually).

<a id="faq-full-v-regular-gc"></a>**Quelle est la différence entre le nettoyage de la mémoire complet et normal?**  
Il existe deux types de [nettoyage de la mémoire](understand.md#job-info-gc):

- Le *nettoyage normal* utilise un algorithme statistique pour rechercher les blocs non référencés volumineux qui remplissent certains critères (mémoire et E/S par seconde faibles). Le nettoyage de la mémoire normal compacte un conteneur de magasin de blocs uniquement si un pourcentage minimal de blocs n’est pas référencé. Ce type de nettoyage de la mémoire s’exécute beaucoup plus vite et utilise moins de ressources que le nettoyage de la mémoire complet. La planification par défaut du travail de nettoyage de la mémoire normal prévoit une exécution une fois par semaine.
- Le *nettoyage de la mémoire complet* effectue un travail beaucoup plus approfondi de recherche des blocs non référencés et de libération d’espace disque. Le nettoyage de la mémoire complet compacte chaque conteneur même si un seul bloc dans le conteneur n’est pas référencé. Le nettoyage de la mémoire complet libère également de l’espace éventuellement en cours d’utilisation, en cas d’incident ou de problème d’alimentation pendant un travail d’optimisation. Les travaux de nettoyage de la mémoire complet récupèrent 100pour cent de l’espace disponible récupérable sur un volume dédupliqué en exigeant plus de temps et de ressources système qu’un travail de nettoyage de la mémoire normal. En général, le travail de nettoyage de la mémoire complet recherche et libère jusqu’à 5pour cent de données non référencées en plus qu’un travail de nettoyage de la mémoire normal. La planification par défaut du travail de nettoyage de la mémoire complet prévoit une exécution tous les quatre nettoyages de la mémoire planifiés.

<a id="faq-why-disable-full-gc"></a>**Pourquoi désactiver le travail de nettoyage de la mémoire complet?**  
- Le travail de nettoyage de la mémoire complet peut avoir des effets négatifs sur la durée de vie des clichés instantanés du volume et la taille de la sauvegarde incrémentielle. Les charges de travail intensives ou gourmandes en E/S peuvent voir leurs performances se dégrader à cause des travaux de nettoyage de la mémoire complets.           
- Vous pouvez exécuter manuellement un travail de nettoyage de la mémoire complet depuis PowerShell pour nettoyer les fuites si vous savez que votre système a subi un incident.
