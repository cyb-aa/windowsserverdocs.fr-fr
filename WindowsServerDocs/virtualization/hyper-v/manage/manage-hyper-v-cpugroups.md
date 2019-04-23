---
title: Contrôles de ressources de Machine virtuelle
description: Utilisation de groupes d'UC de machine virtuelle
keywords: Windows 10, Hyper-V
author: allenma
ms.date: 06/18/2018
ms.topic: article
ms.prod: windows-10-hyperv
ms.service: windows-10-hyperv
ms.assetid: cc7bb88e-ae75-4a54-9fb4-fc7c14964d67
ms.openlocfilehash: 7c4ddf3e5d2ff58eef844c50960327c27a3e0a3d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854760"
---
>S'applique à : Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

# <a name="virtual-machine-resource-controls"></a>Contrôles de ressources de Machine virtuelle

Cet article décrit les contrôles de ressources et d’isolation Hyper-V pour les machines virtuelles.  Ces fonctionnalités, nous appellerons en tant que groupes de processeurs de Machine virtuelle, ou simplement « groupes d’UC », ont été introduites dans Windows Server 2016.  Groupes d’UC permettent aux administrateurs de Hyper-V afin de mieux gérant et allouer des ressources de processeur de l’hôte sur les machines virtuelles invitées.  À l’aide de groupes d’UC, les administrateurs Hyper-V peuvent :

* Créer des groupes de machines virtuelles, chaque groupe ayant des allocations différentes de ressources processeur totales de l’hôte de virtualisation, partagés entre l’ensemble du groupe. Cela permet à l’administrateur de l’hôte implémenter des classes de service pour différents types de machines virtuelles.

* Définir des limites de ressources processeur à des groupes spécifiques. Cet embout « groupe » définit la limite supérieure pour l’hôte de ressources processeur par l’ensemble du groupe peut-être consommer, efficacement en appliquant la classe souhaitée du service pour ce groupe.

* Limiter à un groupe de processeur pour exécuter uniquement sur un ensemble spécifique de processeurs du système hôte. Cela permet d’isoler les machines virtuelles appartenant à différents groupes d’UC entre eux.

## <a name="managing-cpu-groups"></a>La gestion des groupes d’UC

Groupes d’UC sont gérées via le Service de calcul hôte Hyper-V, ou HCS. Une excellente description de la HCS, son genesis, des liens vers les API HCS et bien plus encore sont disponible sur le blog de l’équipe Microsoft Virtualization dans la validation [présentation de l’hôte de Service de calcul (HCS)](https://blogs.technet.microsoft.com/virtualization/2017/01/27/introducing-the-host-compute-service-hcs/).

>[!NOTE] 
>Uniquement la HCS peut servir à créer et gérer des groupes d’UC ; l’applet Gestionnaire Hyper-V, les interfaces de gestion WMI et PowerShell ne prennent pas en charge les groupes d’UC.

Microsoft fournit une ligne de commande utilitaire, cpugroups.exe, sur le [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=865968) qui utilise l’interface HCS pour gérer des groupes d’UC.  Cet utilitaire peut également afficher la topologie de l’UC d’un ordinateur hôte.

## <a name="how-cpu-groups-work"></a>Fonctionnement des groupes de processeur

Allocation des ressources de calcul d’hôte entre les groupes d’UC est appliquée par l’hyperviseur Hyper-V, à l’aide d’une limite de groupe du processeur calculée. La limite de groupe du processeur est une fraction de la capacité totale de l’UC pour un groupe de processeur. La valeur de l’extrémité du groupe dépend de la classe de groupe, ou niveau de priorité. La limite de groupe calculée peut être considérée « en tant que nombre de LP's important du temps processeur ». Ce budget de groupe est partagé, si seulement une seule machine virtuelle était active, il pouvait ainsi utiliser d’allocation de l’ensemble du groupe processeur pour lui-même.

La limite de groupe du processeur est calculée comme G = *n* x *C*, où :

    *G* is the amount of host LP we'd like to assign to the group
    *n* is the total number of logical processors (LPs) in the group
    *C* is the maximum CPU allocation — that is, the class of service desired for the group, expressed as a percentage of the system’s total compute capacity

Par exemple, considérez un groupe de processeur configuré avec 4 processeurs logiques (LPs) et une limite supérieure de 50 %.

    G = n * C
    G = 4 * 50%
    G = 2 LP's worth of CPU time for the entire group

Dans cet exemple, le groupe de processeur G est alloué 2 LP's important du temps processeur.  

Notez que la limite de groupe s’applique quel que soit le nombre de machines virtuelles ou liées au groupe de processeurs virtuels et quel que soit l’état (par exemple, arrêt ou de démarrage) des machines virtuelles affectées au groupe du processeur. Par conséquent, chaque machine virtuelle liée au même groupe de processeur reçoit une fraction de l’allocation d’UC totale du groupe, et cela modifiera avec le nombre de machines virtuelles liée au groupe de processeur. Par conséquent, comme les machines virtuelles sont liés ou indépendant des machines virtuelles à partir d’un groupe de processeur, la limite de groupe globale du processeur doit être réajustée et définie pour maintenir le cap par machine virtuelle qui en résulte souhaité. La gestion des hôtes machine virtuelle administrateur ou la virtualisation de couche logicielle est chargé de gérer les plafonds de groupe que nécessaire pour atteindre l’allocation des ressources du processeur souhaitée par machine virtuelle.

## <a name="example-classes-of-service"></a>Exemples de Classes de Service

Examinons quelques exemples simples. Pour commencer, supposons que l’administrateur de l’hôte Hyper-V aimeriez prendre en charge deux niveaux de service pour les machines virtuelles invitées :

1. Un niveau bas de gamme de « C ». Nous vous offrons ce niveau 10 % de l’hôte ensemble ressources de calcul.

1. Un niveau de milieu de gamme « B ». Ce niveau est alloué 50 % de l’hôte ensemble ressources de calcul.

À ce stade dans notre exemple, nous allons déclarer qu’aucune autres les contrôles de ressources processeur sont en cours d’utilisation, telles que des embouts de machine virtuelle individuelles, poids et réserve.
Toutefois, les plafonds de machine virtuelle individuelles sont importantes, comme nous allons un peu plus loin.

Par souci de simplicité, supposons que chaque machine virtuelle 1 VP, et que notre hôte a 8 LPs. Nous allons commencer par un hôte vide.

Pour créer le niveau « B », l’hôte adminstartor définit la limite de groupe à 50 % :

    G = n * C
    G = 8 * 50%
    G = 4 LP's worth of CPU time for the entire group

L’administrateur de l’hôte ajoute un niveau « B » unique machine virtuelle.
À ce stade, notre machine virtuelle de niveau « B » permettre utiliser au maximum 50 % intéressant du processeur de l’ordinateur hôte, ou l’équivalent de 4 LPs dans notre exemple de système.

À présent, l’administrateur ajoute une deuxième « couche B « machine virtuelle. L’allocation du groupe du processeur — est réparti uniformément entre toutes les machines virtuelles. Nous avons un total de 2 machines virtuelles dans le groupe B, pour chaque machine virtuelle obtient désormais la moitié du nombre total du groupe B de 50 %, 25 % ou l’équivalent de 2 LPs intéressant de temps de calcul.

## <a name="setting-cpu-caps-on-individual-vms"></a>Définir des plafonds de l’UC sur des machines virtuelles individuelles

En plus de la limite de groupe, chaque machine virtuelle peut également avoir un embout de machine virtuelle « individuel ». Les contrôles de ressources du processeur par machine virtuelle, y compris une extrémité de l’UC, le poids et la réserve, ont été une partie d’Hyper-V depuis son introduction.
Lorsqu’elles sont combinées avec une limite supérieure de groupe, une extrémité de la machine virtuelle spécifie la quantité maximale d’UC que chaque VP peut obtenir, même si le groupe dispose des ressources processeur disponibles.

Par exemple, l’administrateur de l’hôte souhaiterez peut-être placer une extrémité de machine virtuelle de 10 % sur les machines virtuelles « C ».
De cette façon, même si la plupart des VPs « C » sont inactives, chaque VP deviendrait jamais plus de 10 %.
Sans une extrémité de la machine virtuelle, machines virtuelles « C » pourraient façon opportuniste optimisent les performances au-delà des niveaux autorisée par leur niveau.

## <a name="isolating-vm-groups-to-specific-host-processors"></a>Isoler les groupes de machines virtuelles avec des processeurs de l’ordinateur hôte spécifique

Administrateurs de l’ordinateur hôte Hyper-V peuvent également la possibilité de dédier les ressources de calcul à une machine virtuelle.
Par exemple, imaginez l’administrateur souhaitait offrir une prime de machine virtuelle « A » qui a une limite de classe de 100 %.
Ces machines virtuelles premium également requièrent une latence plus faible de planification et d’instabilité possibles ; Autrement dit, ils ne sont pas retirer programmées par toute autre machine virtuelle.
Pour atteindre cette séparation, un processeur groupe peut également être configuré avec un mappage d’affinité LP spécifique.

Par exemple, pour s’ajuster à une machine virtuelle « A » sur l’ordinateur hôte dans notre exemple, l’administrateur voulez-vous créer un nouveau groupe de processeur et définir l’affinité de processeur du groupe à un sous-ensemble de LPs l’hôte.
Groupes B et C suivants sont associés aux LPs restantes.
L’administrateur peut créer une seule machine virtuelle dans le groupe A, dont un accès exclusif à toutes les LPs dans un groupe, tout en les groupes de niveau inférieurs vraisemblablement B et C doivent partager les LPs restantes.

## <a name="segregating-root-vps-from-guest-vps"></a>Séparation des VPs racine à partir de l’invité VPs

Par défaut, Hyper-V crée une racine VP sur chaque LP physique sous-jacent.
Ces VPs racine sont strictement mappé 1:1 avec le système LPs et ne migrent pas, autrement dit, chaque racine VP s’exécute toujours sur le même LP physique.
Invité VPs peuvent s’exécuter sur n’importe quel LP disponible et partagent l’exécution avec racine VPs.

Toutefois, il peut être souhaitable à la racine totalement distincte activité de vice-président à partir de l’invité VPs.
Prenons notre exemple ci-dessus dans lequel nous implémentons un niveau de « A » premium machine virtuelle.
Pour assurer VPs notre « un » de la machine virtuelle la latence plus faible possible et « instabilité » ou planification variation, nous souhaitons les exécuter sur un ensemble dédié de LPs et garantir que la racine n’est pas exécuté sur ces LPs.

Cela peut être accompli en utilisant une combinaison de la configuration « minroot », ce qui limite l’hôte de partition de système d’exploitation en cours d’exécution sur un sous-ensemble des processeurs logiques totale du système, ainsi qu’un ou plusieurs groupes d’UC des affinités avec.

L’hôte de virtualisation permettre être configurée pour limiter la partition hôte aux LPs spécifiques, avec un ou plusieurs groupes d’UC avec affinité pour les LPs restantes.
De cette manière, les partitions racines et les invités peuvent s’exécuter sur des ressources de processeur dédiés et complètement isolant, sans partage d’UC.

Pour plus d’informations sur la configuration de « minroot », consultez [gestion des ressources de processeur hôte Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-minroot-2016).

## <a name="using-the-cpugroups-tool"></a>À l’aide de l’outil CpuGroups

Examinons quelques exemples montrant comment utiliser l’outil CpuGroups.

>[!NOTE] 
>Paramètres de ligne de commande pour l’outil CpuGroups sont transmises en utilisant uniquement des espaces comme délimiteurs. Ne '/' ou '-' caractères doivent continuer le commutateur de ligne de commande de votre choix.

### <a name="discovering-the-cpu-topology"></a>Découverte de la topologie de l’UC

L’exécution de CpuGroups avec la GetCpuTopology retourne des informations sur le système actuel, comme indiqué ci-dessous, y compris l’Index LP, le nœud NUMA auquel le LP appartient, le Package et ID de base et l’index VP de la racine.

L’exemple suivant montre un système avec 2 sockets d’UC et les nœuds NUMA, un total de 32 LPs et multithreading activé et configuré pour autoriser Minroot avec 8 racine VPs, 4 à partir de chaque nœud NUMA.
LPs ayant racine VPs ont un RootVpIndex > = 0 ; LPs avec un RootVpIndex de -1 ne sont pas disponibles pour la partition racine, mais sont toujours gérés par l’hyperviseur et exécuteront invité VPs comme autorisé par d’autres paramètres de configuration.

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

### <a name="example-2--print-all-cpu-groups-on-the-host"></a>Exemple 2 : imprimer tous les groupes d’UC sur l’ordinateur hôte

Ici, nous allons répertorier tous les groupes d’UC sur l’hôte actuel, leur GroupId, limite de processeur du groupe et les indices de LPs affectés à ce groupe.

Notez que les valeurs de limite de processeur valides sont dans la plage [0, 65536], et ces valeurs express la limite de groupe en pourcentage (par exemple, 32768 = 50 %).

```console
C:\vm\tools>CpuGroups.exe GetGroups

CpuGroupId                          CpuCap  LpIndexes
------------------------------------ ------ --------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536  24,25,26,27,28,29,30,31
```

### <a name="example-3--print-a-single-cpu-group"></a>Exemple 3 : imprimer un seul groupe de processeur

Dans cet exemple, nous allons interroger un seul groupe de processeur à l’aide de GroupId en tant que filtre.

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000003
CpuGroupId                          CpuCap   LpIndexes
------------------------------------ ------ ----------
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
```

### <a name="example-4--create-a-new-cpu-group"></a>Exemple 4 : créer un nouveau groupe de processeur

Ici, nous allons créer un nouveau groupe de processeur, en spécifiant l’ID de groupe et le jeu de LPs à affecter au groupe.

```console
C:\vm\tools>CpuGroups.exe CreateGroup /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /GroupAffinity:0,1,16,17
```

Afficher maintenant notre groupe récemment créé.

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 65536 0,1,16,17
36AB08CB-3A76-4B38-992E-000000000002 32768 4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536 12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-5--set-the-cpu-group-cap-to-50"></a>Exemple 5 – Définissez la limite de groupe de processeur à 50 %

Ici, nous allons définir la limite de groupe du processeur à 50 %.

```console
C:\vm\tools>CpuGroups.exe SetGroupProperty /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /CpuCap:32768
```

Maintenant nous allons confirmer notre paramètre en affichant le groupe juste mis à jour.

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000001

CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 32768 0,1,16,17
```

### <a name="example-6--print-cpu-group-ids-for-all-vms-on-the-host"></a>Exemple 6 : ID de groupe du processeur d’impression pour toutes les machines virtuelles sur l’ordinateur hôte

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

### <a name="example-7--unbind-a-vm-from-the-cpu-group"></a>Exemple 7 – séparer une machine virtuelle à partir du groupe de processeur

Pour supprimer une machine virtuelle à partir d’un groupe de processeur, la valeur CpuGroupId de la machine virtuelle vers le GUID de valeur NULL. Cette opération annule la liaison la machine virtuelle à partir du groupe de processeur.

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

### <a name="example-8--bind-a-vm-to-an-existing-cpu-group"></a>Exemple 8 : lier une machine virtuelle à un groupe existant de processeur

Ici, nous allons ajouter une machine virtuelle à un groupe d’UC existant.
Notez que la machine virtuelle ne doit pas être liée à n’importe quel groupe d’UC existant, ou id de groupe du processeur de paramètre échoue.

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:36AB08CB-3A76-4B38-992E-000000000001
```

Maintenant, vérifiez que le G1 de machine virtuelle est dans le groupe de processeur souhaité.

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

### <a name="example-9--print-all-vms-grouped-by-cpu-group-id"></a>Exemple 9 – toutes les machines virtuelles regroupées par id de groupe du processeur d’impression

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

### <a name="example-10--print-all-vms-for-a-single-cpu-group"></a>Exemple 10 – imprimer toutes les machines virtuelles pour un seul groupe de processeur

```console
C:\vm\tools>CpuGroups.exe GetGroupVms /GroupId:36ab08cb-3a76-4b38-992e-000000000002

CpuGroupId                           VmName                                VmId
------------------------------------ ------ ------------------------------------
36ab08cb-3a76-4b38-992e-000000000002     G2 4ABCFC2F-6C22-498C-BB38-7151CE678758
36ab08cb-3a76-4b38-992e-000000000002     G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200
```

### <a name="example-11--attempting-to-delete-a-non-empty-cpu-group"></a>Exemple 11 : tentative de suppression d’un groupe d’UC non vide

Vide uniquement les groupes d’UC, autrement dit, les groupes d’UC non lié machines virtuelles, peuvent être supprimés.
Pour supprimer un groupe d’UC non vides échoue.

```console
C:\vm\tools>CpuGroups.exe DeleteGroup /GroupId:36ab08cb-3a76-4b38-992e-000000000001
(null)
Failed with error 0xc0350070
```

### <a name="example-12--unbind-the-only-vm-from-a-cpu-group-and-delete-the-group"></a>Exemple 12 – annuler la liaison de la seule machine virtuelle à partir d’un groupe de processeur et de supprimer le groupe

Dans cet exemple, nous allons utiliser plusieurs commandes pour examiner un groupe de processeur, de supprimer la machine virtuelle unique appartenant à ce groupe, puis supprimez le groupe.

Tout d’abord, nous allons énumérer les machines virtuelles dans notre groupe.

```console
C:\vm\tools>CpuGroups.exe GetGroupVms /GroupId:36AB08CB-3A76-4B38-992E-000000000001
CpuGroupId                           VmName                                VmId
------------------------------------ ------ ------------------------------------
36AB08CB-3A76-4B38-992E-000000000001     G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
```

Nous voyons qu’uniquement une seule machine virtuelle, nommée G1, appartient à ce groupe.
Nous allons supprimer la machine virtuelle G1 notre groupe en définissant l’ID de groupe de la machine virtuelle avec la valeur NULL.

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:00000000-0000-0000-0000-000000000000
```

Et vérifier notre modification...

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

Et confirmer que notre groupe a disparu.

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap                     LpIndexes
------------------------------------ ------ -----------------------------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-13--bind-a-vm-back-to-its-original-cpu-group"></a>Exemple de 13 : lier une machine virtuelle à son groupe d’UC d’origine

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
