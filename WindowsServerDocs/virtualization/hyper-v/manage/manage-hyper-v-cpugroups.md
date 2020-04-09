---
title: Contrôles de ressources de machine virtuelle
description: Utilisation de groupes d'UC de machine virtuelle
author: allenma
ms.date: 06/18/2018
ms.topic: article
ms.prod: windows-server
ms.service: windows-10-hyperv
ms.assetid: cc7bb88e-ae75-4a54-9fb4-fc7c14964d67
ms.openlocfilehash: fcf61c22a24abb6b16baf75b4846cc188dcecd49
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860792"
---
>S’applique à : Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

# <a name="virtual-machine-resource-controls"></a>Contrôles de ressources de machine virtuelle

Cet article décrit les contrôles de ressource et d’isolation Hyper-V pour les ordinateurs virtuels.  Ces fonctionnalités, que nous allons désigner comme groupes UC de machines virtuelles ou simplement « groupes UC », ont été introduites dans Windows Server 2016.  Les groupes de PROCESSEURs permettent aux administrateurs Hyper-V de mieux gérer et allouer les ressources processeur de l’ordinateur hôte sur les machines virtuelles invitées.  À l’aide de groupes de PROCESSEURs, les administrateurs Hyper-V peuvent :

* Créer des groupes de machines virtuelles, chaque groupe ayant des allocations différentes des ressources processeur totales de l’hôte de virtualisation, partagées sur l’ensemble du groupe. Cela permet à l’administrateur de l’hôte d’implémenter des classes de service pour différents types de machines virtuelles.

* Définir des limites de ressources de processeur pour des groupes spécifiques. Cette « limite de groupe » définit la limite supérieure des ressources processeur de l’ordinateur hôte que le groupe entier peut consommer, en appliquant efficacement la classe de service souhaitée pour ce groupe.

* Contraindre un groupe UC à s’exécuter uniquement sur un ensemble spécifique de processeurs du système hôte. Cela peut être utilisé pour isoler les machines virtuelles appartenant à différents groupes d’UC les unes des autres.

## <a name="managing-cpu-groups"></a>Gestion des groupes de PROCESSEURs

Les groupes de PROCESSEURs sont gérés via le service de calcul hôte Hyper-V ou HCS. La description de HCS, de ses Genesis, des liens vers les API HCS et bien plus est disponible sur le blog de l’équipe de virtualisation Microsoft dans la publication [Présentation du service de calcul hôte (HCS)](https://blogs.technet.microsoft.com/virtualization/2017/01/27/introducing-the-host-compute-service-hcs/).

>[!NOTE] 
>Seul le HCS peut être utilisé pour créer et gérer des groupes de PROCESSEURs. l’applet du Gestionnaire Hyper-V, les interfaces WMI et de gestion PowerShell ne prennent pas en charge les groupes de PROCESSEURs.

Microsoft propose un utilitaire de ligne de commande, cpugroups. exe, sur le [Centre de téléchargement Microsoft](https://go.microsoft.com/fwlink/?linkid=865968) , qui utilise l’interface HCS pour gérer les groupes de processeurs.  Cet utilitaire peut également afficher la topologie de l’UC d’un ordinateur hôte.

## <a name="how-cpu-groups-work"></a>Fonctionnement des groupes de PROCESSEURs

L’allocation des ressources de calcul de l’hôte entre les groupes d’UC est appliquée par l’hyperviseur Hyper-V, à l’aide d’une limite de groupe de PROCESSEURs calculée. L’embout du groupe de PROCESSEURs représente une fraction de la capacité totale de l’UC pour un groupe de PROCESSEURs. La valeur de l’extrémité du groupe dépend de la classe de groupe ou du niveau de priorité affecté. L’extrémité du groupe calculé peut être considérée comme « un nombre de LP de temps processeur ». Ce groupe est partagé. par conséquent, si une seule machine virtuelle était active, elle pourrait utiliser l’allocation de l’UC du groupe entier pour elle-même.

L’embout du groupe de PROCESSEURs est calculé comme G = *n* x *C*, où :

    *G* is the amount of host LP we'd like to assign to the group
    *n* is the total number of logical processors (LPs) in the group
    *C* is the maximum CPU allocation — that is, the class of service desired for the group, expressed as a percentage of the system's total compute capacity

Par exemple, considérez un groupe UC configuré avec 4 processeurs logiques (de 1 à 4) et une limite de 50%.

    G = n * C
    G = 4 * 50%
    G = 2 LP's worth of CPU time for the entire group

Dans cet exemple, le groupe UC G est alloué à 2 LP de temps processeur.  

Notez que l’extrémité du groupe s’applique quel que soit le nombre de machines virtuelles ou de processeurs virtuels liés au groupe, et quel que soit l’État (par exemple, l’arrêt ou le démarrage) des machines virtuelles affectées au groupe de PROCESSEURs. Par conséquent, chaque machine virtuelle liée au même groupe de PROCESSEURs reçoit une fraction de l’allocation d’UC totale du groupe, ce qui change avec le nombre de machines virtuelles liées au groupe de PROCESSEURs. Par conséquent, comme les machines virtuelles sont des machines virtuelles liées ou indépendantes d’un groupe de PROCESSEURs, la limite globale du groupe de PROCESSEURs doit être réajustée et définie pour conserver l’extrémité de la machine virtuelle souhaitée. L’administrateur de l’hôte de machine virtuelle ou la couche logicielle de gestion de la virtualisation est responsable de la gestion des Cap de groupe selon les besoins afin d’obtenir l’allocation de ressources de processeur par machine virtuelle souhaitée.

## <a name="example-classes-of-service"></a>Exemples de classes de service

Examinons quelques exemples simples. Pour commencer, supposons que l’administrateur de l’hôte Hyper-V souhaite prendre en charge deux niveaux de service pour les machines virtuelles invitées :

1. Niveau « C » bas. Nous offrons ce niveau 10% des ressources de calcul de l’hôte entier.

1. Niveau « B » de milieu de gamme. Ce niveau est alloué à 50% de l’ensemble des ressources de calcul de l’hôte.

À ce stade de notre exemple, nous affirmerons qu’aucun autre contrôle de ressource de processeur n’est utilisé, comme des Cap, des poids et des réserves de machines virtuelles individuels.
Toutefois, les limites individuelles des machines virtuelles sont importantes, car nous verrons un peu plus tard.

Par souci de simplicité, supposons que chaque machine virtuelle a 1 VP, et que notre hôte dispose de 8 to. Nous allons commencer par un hôte vide.

Pour créer le niveau « B », l’hôte adminstartor définit la limite de groupe sur 50% :

    G = n * C
    G = 8 * 50%
    G = 4 LP's worth of CPU time for the entire group

L’administrateur de l’hôte ajoute une seule machine virtuelle de niveau « B ».
À ce stade, notre machine virtuelle de niveau « B » peut utiliser au maximum 50% de l’UC de l’hôte, ou l’équivalent de 4 unités de mémoire dans notre exemple de système.

À présent, l’administrateur ajoute une deuxième machine virtuelle « Tier B ». L’allocation du groupe de PROCESSEURs, est divisée uniformément entre toutes les machines virtuelles. Nous avons un total de 2 machines virtuelles dans le groupe B. par conséquent, chaque machine virtuelle obtient désormais la moitié du total du groupe B de 50%, 25% chacune, ou l’équivalent de 2% du temps de calcul.

## <a name="setting-cpu-caps-on-individual-vms"></a>Définition des seuils d’UC sur des machines virtuelles individuelles

En plus de l’extrémité de groupe, chaque machine virtuelle peut également avoir un « embout de machine virtuelle » individuel. Les contrôles de ressources de processeur par machine virtuelle, y compris une limite d’UC, une pondération et une réserve, font partie d’Hyper-V depuis son introduction.
Lorsqu’elle est associée à un groupe de ressources, une limite de machines virtuelles spécifie la quantité maximale d’UC que chaque VP peut obtenir, même si le groupe a des ressources processeur disponibles.

Par exemple, l’administrateur de l’hôte peut souhaiter placer une limite de 10% de machines virtuelles sur les machines virtuelles « C ».
De cette façon, même si la plupart des VPss « C » sont inactifs, chaque VP ne peut jamais obtenir plus de 10%.
Sans l’extrémité de la machine virtuelle, les machines virtuelles « C » peuvent associer façon opportuniste obtenir des performances au-delà des niveaux autorisés par leur niveau.

## <a name="isolating-vm-groups-to-specific-host-processors"></a>Isolation des groupes de machines virtuelles à des processeurs hôtes spécifiques

Les administrateurs d’ordinateurs hôtes Hyper-V peuvent également souhaiter dédier des ressources de calcul à une machine virtuelle.
Par exemple, imaginez que l’administrateur souhaitait proposer une machine virtuelle « A » Premium qui a une limite de classe de 100%.
Ces machines virtuelles Premium requièrent également la latence de planification la plus faible et l’instabilité possible. autrement dit, ils ne peuvent pas être déplanifiés par une autre machine virtuelle.
Pour atteindre cette séparation, un groupe de PROCESSEURs peut également être configuré avec un mappage d’affinités LP spécifique.

Par exemple, pour ajuster une machine virtuelle « A » sur l’hôte dans notre exemple, l’administrateur crée un nouveau groupe d’UC et définit l’affinité du processeur du groupe sur un sous-ensemble de la taille de serveur de l’hôte.
Les groupes B et C seraient affinités à la valeur de-la plus restante.
L’administrateur peut créer une machine virtuelle unique dans le groupe A, qui aurait alors un accès exclusif à tous les processeurs du groupe A, tandis que les groupes de niveaux moins élevés B et C partageraient le plus petit.

## <a name="segregating-root-vps-from-guest-vps"></a>Séparer les VPs racines des VPs invités

Par défaut, Hyper-V crée un VP racine sur chaque LP physique sous-jacente.
Ces VPs racines sont strictement mappées à 1:1 avec le système de la configuration de disque, et ne sont pas migrées, autrement dit, chaque VP racine s’exécutera toujours sur la même LP physique.
Les VPs invités peuvent être exécutés sur n’importe quel LP disponible et partager l’exécution avec la racine VPs.

Toutefois, il peut être souhaitable de séparer complètement l’activité du VP racine des VPs invités.
Prenons l’exemple ci-dessus, où nous implémentons une machine virtuelle de niveau « A » Premium.
Pour vous assurer que les VPs de la machine virtuelle « A » ont la latence la plus faible possible et le « bougé », ou la variation de planification, nous aimerions les exécuter sur un ensemble dédié de et s’assurer que la racine n’est pas exécutée sur ces deux.

Cela peut être accompli à l’aide d’une combinaison de la configuration « minroot », qui limite la partition de système d’exploitation hôte à s’exécuter sur un sous-ensemble des processeurs logiques système totaux, ainsi qu’un ou plusieurs groupes de PROCESSEURs affinité.

L’hôte de virtualisation peut être configuré pour limiter la partition de l’hôte à une unité de mesure spécifique, avec un ou plusieurs groupes de processeurs affinité à la valeur de-1 restante.
De cette manière, les partitions racine et invité peuvent s’exécuter sur des ressources processeur dédiées, et complètement isolées, sans partage de l’UC.

Pour plus d’informations sur la configuration « minroot », consultez [gestion des ressources du processeur de l’ordinateur hôte Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-minroot-2016).

## <a name="using-the-cpugroups-tool"></a>Utilisation de l’outil CpuGroups

Examinons quelques exemples d’utilisation de l’outil CpuGroups.

>[!NOTE] 
>Les paramètres de ligne de commande pour l’outil CpuGroups sont passés en utilisant uniquement des espaces comme délimiteurs. Aucun caractère « / » ou « - » ne doit continuer le commutateur de ligne de commande souhaité.

### <a name="discovering-the-cpu-topology"></a>Découverte de la topologie de l’UC

L’exécution de CpuGroups avec le GetCpuTopology retourne des informations sur le système actuel, comme indiqué ci-dessous, y compris l’index LP, le nœud NUMA auquel le LP appartient, le package et les ID de noyau, et l’index du VP Directeur racine.

L’exemple suivant montre un système avec 2 sockets d’UC et des nœuds NUMA, un total de 32 de 1 to et le multithreading activé, et configurés pour activer Minroot avec 8 VPs racine, 4 à partir de chaque nœud NUMA.
Le VPs de la valeur de la racine a un RootVpIndex > = 0 ; La valeur de 1 à 1 avec un RootVpIndex de-1 n’est pas disponible pour la partition racine, mais elle est toujours gérée par l’hyperviseur et exécutera VPs invité comme autorisé par d’autres paramètres de configuration.

```console
C:\vm\tools>CpuGroups.exe GetCpuTopology

LpIndex NodeNumber PackageId CoreId RootVpIndex
------- ---------- --------- ------ -----------
      0          0         0      0           0
      1          0         0      0           1
      2          0         0      1           2
      3          0         0      1           3
      4          0         0      2          -1
      5          0         0      2          -1
      6          0         0      3          -1
      7          0         0      3          -1
      8          0         0      4          -1
      9          0         0      4          -1
     10          0         0      5          -1
     11          0         0      5          -1
     12          0         0      6          -1
     13          0         0      6          -1
     14          0         0      7          -1
     15          0         0      7          -1
     16          1         1     16           4
     17          1         1     16           5
     18          1         1     17           6
     19          1         1     17           7
     20          1         1     18          -1
     21          1         1     18          -1
     22          1         1     19          -1
     23          1         1     19          -1
     24          1         1     20          -1
     25          1         1     20          -1
     26          1         1     21          -1
     27          1         1     21          -1
     28          1         1     22          -1
     29          1         1     22          -1
     30          1         1     23          -1
     31          1         1     23          -1
```

### <a name="example-2--print-all-cpu-groups-on-the-host"></a>Exemple 2 : imprimer tous les groupes de PROCESSEURs sur l’ordinateur hôte

Ici, nous allons dresser la liste de tous les groupes de PROCESSEURs sur l’hôte actuel, de leur GroupId, de la limite d’UC du groupe et des indices de base attribués à ce groupe.

Notez que les valeurs de l’UC valides sont comprises dans la plage [0, 65536], et ces valeurs expriment l’extrémité du groupe en pourcentage (par exemple, 32768 = 50%).

```console
C:\vm\tools>CpuGroups.exe GetGroups

CpuGroupId                          CpuCap  LpIndexes
------------------------------------ ------ --------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536  24,25,26,27,28,29,30,31
```

### <a name="example-3--print-a-single-cpu-group"></a>Exemple 3 : imprimer un seul groupe UC

Dans cet exemple, nous allons interroger un seul groupe UC en utilisant le GroupId comme filtre.

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000003
CpuGroupId                          CpuCap   LpIndexes
------------------------------------ ------ ----------
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
```

### <a name="example-4--create-a-new-cpu-group"></a>Exemple 4 : créer un groupe de PROCESSEURs

Ici, nous allons créer un nouveau groupe de processeurs, en spécifiant l’ID de groupe et l’ensemble de de jeux de ressources à affecter au groupe.

```console
C:\vm\tools>CpuGroups.exe CreateGroup /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /GroupAffinity:0,1,16,17
```

Maintenant, affichez le groupe que vous venez d’ajouter.

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 65536 0,1,16,17
36AB08CB-3A76-4B38-992E-000000000002 32768 4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536 12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-5--set-the-cpu-group-cap-to-50"></a>Exemple 5 : définir le seuil du groupe de PROCESSEURs sur 50%

Ici, nous allons définir le seuil du groupe de PROCESSEURs sur 50%.

```console
C:\vm\tools>CpuGroups.exe SetGroupProperty /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /CpuCap:32768
```

Nous allons maintenant confirmer notre paramètre en affichant le groupe que nous venons de mettre à jour.

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000001

CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 32768 0,1,16,17
```

### <a name="example-6--print-cpu-group-ids-for-all-vms-on-the-host"></a>Exemple 6 : imprimer des ID de groupe de PROCESSEURs pour toutes les machines virtuelles sur l’ordinateur hôte

```console
C:\vm\tools>CpuGroups.exe GetVmGroup

VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G2 4ABCFC2F-6C22-498C-BB38-7151CE678758 36ab08cb-3a76-4b38-992e-000000000002
    P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC 36ab08cb-3a76-4b38-992e-000000000003
    P2 A593D93A-3A5F-48AB-8862-A4350E3459E8 36ab08cb-3a76-4b38-992e-000000000004
    G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200 36ab08cb-3a76-4b38-992e-000000000002
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 36ab08cb-3a76-4b38-992e-000000000002
```

### <a name="example-7--unbind-a-vm-from-the-cpu-group"></a>Exemple 7 – dissociation d’une machine virtuelle du groupe UC

Pour supprimer une machine virtuelle d’un groupe de PROCESSEURs, affectez au CpuGroupId de la machine virtuelle la valeur du GUID NULL. Cela dissocie la machine virtuelle du groupe UC.

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:00000000-0000-0000-0000-000000000000

C:\vm\tools>CpuGroups.exe GetVmGroup
VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G2 4ABCFC2F-6C22-498C-BB38-7151CE678758 36ab08cb-3a76-4b38-992e-000000000002
    P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC 36ab08cb-3a76-4b38-992e-000000000003
    P2 A593D93A-3A5F-48AB-8862-A4350E3459E8 36ab08cb-3a76-4b38-992e-000000000004
    G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200 36ab08cb-3a76-4b38-992e-000000000002
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 00000000-0000-0000-0000-000000000000
```

### <a name="example-8--bind-a-vm-to-an-existing-cpu-group"></a>Exemple 8 : lier une machine virtuelle à un groupe de PROCESSEURs existant

Ici, nous allons ajouter une machine virtuelle à un groupe de PROCESSEURs existant.
Notez que la machine virtuelle ne doit pas être liée à un groupe d’UC existant ou que la définition de l’ID du groupe de processeurs échoue.

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:36AB08CB-3A76-4B38-992E-000000000001
```

Maintenant, vérifiez que la machine virtuelle G1 est dans le groupe de PROCESSEURs souhaité.

```console
C:\vm\tools>CpuGroups.exe GetVmGroup
VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G2 4ABCFC2F-6C22-498C-BB38-7151CE678758 36ab08cb-3a76-4b38-992e-000000000002
    P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC 36ab08cb-3a76-4b38-992e-000000000003
    P2 A593D93A-3A5F-48AB-8862-A4350E3459E8 36ab08cb-3a76-4b38-992e-000000000004
    G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200 36ab08cb-3a76-4b38-992e-000000000002
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 36AB08CB-3A76-4B38-992E-000000000001
```

### <a name="example-9--print-all-vms-grouped-by-cpu-group-id"></a>Exemple 9 : imprimer toutes les machines virtuelles regroupées par ID de groupe UC

```console
C:\vm\tools>CpuGroups.exe GetGroupVms
CpuGroupId                           VmName                                 VmId
------------------------------------ ------ ------------------------------------
36AB08CB-3A76-4B38-992E-000000000001     G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
36ab08cb-3a76-4b38-992e-000000000002     G2 4ABCFC2F-6C22-498C-BB38-7151CE678758
36ab08cb-3a76-4b38-992e-000000000002     G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200
36ab08cb-3a76-4b38-992e-000000000003     P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC
36ab08cb-3a76-4b38-992e-000000000004     P2 A593D93A-3A5F-48AB-8862-A4350E3459E8
```

### <a name="example-10--print-all-vms-for-a-single-cpu-group"></a>Exemple 10 : imprimer toutes les machines virtuelles pour un seul groupe de PROCESSEURs

```console
C:\vm\tools>CpuGroups.exe GetGroupVms /GroupId:36ab08cb-3a76-4b38-992e-000000000002

CpuGroupId                           VmName                                VmId
------------------------------------ ------ ------------------------------------
36ab08cb-3a76-4b38-992e-000000000002     G2 4ABCFC2F-6C22-498C-BB38-7151CE678758
36ab08cb-3a76-4b38-992e-000000000002     G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200
```

### <a name="example-11--attempting-to-delete-a-non-empty-cpu-group"></a>Exemple 11 : tentative de suppression d’un groupe d’UC non vide

Seuls les groupes de PROCESSEURs vides (c’est-à-dire les groupes de PROCESSEURs sans machines virtuelles associées) peuvent être supprimés.
La tentative de suppression d’un groupe d’UC non vide échouera.

```console
C:\vm\tools>CpuGroups.exe DeleteGroup /GroupId:36ab08cb-3a76-4b38-992e-000000000001
(null)
Failed with error 0xc0350070
```

### <a name="example-12--unbind-the-only-vm-from-a-cpu-group-and-delete-the-group"></a>Exemple 12 : annuler la liaison de la machine virtuelle à partir d’un groupe UC et supprimer le groupe

Dans cet exemple, nous allons utiliser plusieurs commandes pour examiner un groupe de PROCESSEURs, supprimer la machine virtuelle unique appartenant à ce groupe, puis supprimer le groupe.

Tout d’abord, nous allons énumérer les machines virtuelles de notre groupe.

```console
C:\vm\tools>CpuGroups.exe GetGroupVms /GroupId:36AB08CB-3A76-4B38-992E-000000000001
CpuGroupId                           VmName                                VmId
------------------------------------ ------ ------------------------------------
36AB08CB-3A76-4B38-992E-000000000001     G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
```

Nous voyons qu’une seule machine virtuelle, nommée G1, appartient à ce groupe.
Nous allons supprimer la machine virtuelle G1 de notre groupe en affectant à l’ID de groupe de la machine virtuelle la valeur NULL.

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:00000000-0000-0000-0000-000000000000
```

Et vérifiez notre modification...

```console
C:\vm\tools>CpuGroups.exe GetVmGroup /VmName:g1
VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 00000000-0000-0000-0000-000000000000
```

Maintenant que le groupe est vide, nous pouvons le supprimer en toute sécurité.

```console
C:\vm\tools>CpuGroups.exe DeleteGroup /GroupId:36ab08cb-3a76-4b38-992e-000000000001
```

Et confirmez que notre groupe a disparu.

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap                     LpIndexes
------------------------------------ ------ -----------------------------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-13--bind-a-vm-back-to-its-original-cpu-group"></a>Exemple 13 : reliaison d’une machine virtuelle à son groupe de PROCESSEURs d’origine

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:36AB08CB-3A76-4B38-992E-000000000002

C:\vm\tools>CpuGroups.exe GetGroupVms
CpuGroupId VmName VmId
------------------------------------ -------------------------------- ------------------------------------
36ab08cb-3a76-4b38-992e-000000000002 G2 4ABCFC2F-6C22-498C-BB38-7151CE678758
36ab08cb-3a76-4b38-992e-000000000002 G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200
36AB08CB-3A76-4B38-992E-000000000002 G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
36ab08cb-3a76-4b38-992e-000000000003 P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC
36ab08cb-3a76-4b38-992e-000000000004 P2 A593D93A-3A5F-48AB-8862-A4350E3459E8
```
