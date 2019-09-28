---
title: Performances des e/s de stockage Hyper-V
description: Considérations sur les performances des e/s de stockage dans le réglage des performances d’Hyper-V
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 7c5a7b667f24ee929a80010dc51508033f991ed5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370061"
---
# <a name="hyper-v-storage-io-performance"></a>Performances des e/s de stockage Hyper-V

Cette section décrit les différentes options et considérations à prendre en compte pour le paramétrage des performances d’e/s de stockage sur une machine virtuelle. Le chemin d’accès d’e/s de stockage s’étend de la pile de stockage invité, via la couche de virtualisation de l’hôte, à la pile de stockage hôte, puis au disque physique. Vous trouverez ci-dessous des explications sur la façon dont les optimisations sont possibles à chacune de ces étapes.

## <a name="virtual-controllers"></a>Contrôleurs virtuels

Hyper-V offre trois types de contrôleurs virtuels : Les adaptateurs de bus hôte virtuel, SCSI et IDE.

## <a name="ide"></a>IDE

Les contrôleurs IDE exposent les disques IDE à la machine virtuelle. Le contrôleur IDE est émulé, et c’est le seul contrôleur disponible pour les machines virtuelles invitées exécutant une version antérieure de Windows sans la Integration Services de la machine virtuelle. Les e/s de disque effectuées à l’aide du pilote de filtre IDE fourni avec la machine virtuelle Integration Services sont nettement meilleures que les performances d’e/s disque fournies avec le contrôleur IDE émulé. Nous vous recommandons d’utiliser des disques IDE uniquement pour les disques de système d’exploitation, car ils ont des limitations de performances en raison de la taille d’e/s maximale pouvant être émise pour ces appareils.

## <a name="scsi-sas-controller"></a>SCSI (contrôleur SAS)

Les contrôleurs SCSI exposent les disques SCSI à la machine virtuelle, et chaque contrôleur SCSI virtuel peut prendre en charge jusqu’à 64 appareils. Pour des performances optimales, nous vous recommandons d’attacher plusieurs disques à un seul contrôleur SCSI virtuel et de créer des contrôleurs supplémentaires uniquement lorsqu’ils sont nécessaires pour mettre à l’échelle le nombre de disques connectés à la machine virtuelle. Le chemin d’accès SCSI n’est pas émulé, ce qui en fait le contrôleur préféré pour un disque autre que le disque du système d’exploitation. En fait, avec les machines virtuelles de génération 2, il s’agit du seul type de contrôleur possible. Introduite dans Windows Server 2012 R2, ce contrôleur est signalé comme SAS pour prendre en charge le VHDX partagé.

## <a name="virtual-fibre-channel-hbas"></a>Adaptateurs HBA Fibre Channel virtuels

Les adaptateurs de bus virtuel Fibre Channel peuvent être configurés de façon à autoriser l’accès direct des ordinateurs virtuels à Fibre Channel et aux LUN de Fibre Channel sur Ethernet (FCoE). Les disques de Fibre Channel virtuels contournent le système de fichiers NTFS dans la partition racine, ce qui réduit l’utilisation du processeur des e/s de stockage.

Les lecteurs de données et les lecteurs de grande taille qui sont partagés entre plusieurs machines virtuelles (pour les scénarios de clustering invité) sont des candidats privilégiés pour les disques Fibre Channel virtuels.

Les disques de Fibre Channel virtuels nécessitent l’installation d’un ou plusieurs adaptateurs de bus hôte (HBA) Fibre Channel sur l’ordinateur hôte. Chaque adaptateur HBA de l’ordinateur hôte est requis pour utiliser un pilote HBA qui prend en charge les fonctionnalités de Fibre Channel virtuel/NPIV de Windows Server 2016. La structure SAN doit prendre en charge la fonctionnalité NPIV, et les ports HBA utilisés pour la Fibre Channel virtuelle doivent être configurés dans une topologie Fibre Channel qui prend en charge NPIV.

Pour maximiser le débit sur les ordinateurs hôtes qui sont installés avec plusieurs HBA, nous vous recommandons de configurer plusieurs adaptateurs de bus hôte virtuels à l’intérieur de la machine virtuelle Hyper-V (jusqu’à quatre adaptateurs HBA peuvent être configurés pour chaque ordinateur virtuel). Hyper-V fera automatiquement un meilleur effort pour équilibrer les adaptateurs HBA virtuels et héberger les HBA qui accèdent au même réseau SAN virtuel.

## <a name="virtual-disks"></a>Disques virtuels

Les disques peuvent être exposés aux machines virtuelles via les contrôleurs virtuels. Ces disques peuvent être des disques durs virtuels qui sont des abstractions de fichiers d’un disque ou d’un disque direct sur l’ordinateur hôte.

## <a name="virtual-hard-disks"></a>disques durs virtuels ;

Il existe deux formats de disque dur virtuel, VHD et VHDX. Chacun de ces formats prend en charge trois types de fichiers de disque dur virtuel.

## <a name="vhd-format"></a>Format VHD

Le format VHD était le seul format de disque dur virtuel pris en charge par Hyper-V dans les versions antérieures. Introduite dans Windows Server 2012, le format de disque dur virtuel a été modifié afin de permettre un meilleur alignement, ce qui améliore considérablement les performances sur les nouveaux disques à grands secteurs.

Tout nouveau disque dur virtuel créé sur un serveur Windows 2012 ou une version plus récente a un alignement optimal de 4 Ko. Ce format aligné est entièrement compatible avec les systèmes d’exploitation Windows Server précédents. Toutefois, la propriété d’alignement est rompue pour les nouvelles allocations des analyseurs qui ne prennent pas en charge l’alignement de 4 Ko (comme un analyseur de disque dur virtuel d’une version antérieure de Windows Server ou d’un analyseur non-Microsoft).

Tout disque dur virtuel déplacé à partir d’une version précédente n’est pas automatiquement converti en ce nouveau format VHD amélioré.

Pour convertir au nouveau format de disque dur virtuel, exécutez la commande Windows PowerShell suivante :

``` syntax
Convert-VHD –Path E:\vms\testvhd\test.vhd –DestinationPath E:\vms\testvhd\test-converted.vhd
```

Vous pouvez vérifier la propriété d’alignement pour tous les disques durs virtuels du système et la convertir en alignement optimal de 4 Ko. Vous créez un nouveau disque dur virtuel avec les données du disque dur virtuel d’origine à l’aide de l’option **créer à partir de la source** .

Pour vérifier l’alignement à l’aide de Windows PowerShell, examinez la ligne alignement, comme indiqué ci-dessous :

``` syntax
Get-VHD –Path E:\vms\testvhd\test.vhd

Path                    : E:\vms\testvhd\test.vhd
VhdFormat               : VHD
VhdType                 : Dynamic
FileSize                : 69245440
Size                    : 10737418240
MinimumSize             : 10735321088
LogicalSectorSize       : 512
PhysicalSectorSize      : 512
BlockSize               : 2097152
ParentPath              :
FragmentationPercentage : 10
Alignment               : 0
Attached                : False
DiskNumber              :
IsDeleted               : False
Number                  :
```

Pour vérifier l’alignement à l’aide de Windows PowerShell, examinez la ligne alignement, comme indiqué ci-dessous :

``` syntax
Get-VHD –Path E:\vms\testvhd\test-converted.vhd

Path                    : E:\vms\testvhd\test-converted.vhd
VhdFormat               : VHD
VhdType                 : Dynamic
FileSize                : 69369856
Size                    : 10737418240
MinimumSize             : 10735321088
LogicalSectorSize       : 512
PhysicalSectorSize      : 512
BlockSize               : 2097152
ParentPath              :
FragmentationPercentage : 0
Alignment               : 1
Attached                : False
DiskNumber              :
IsDeleted               : False
Number                  :
```

## <a name="vhdx-format"></a>Format VHDX

VHDX est un nouveau format de disque dur virtuel introduit dans Windows Server 2012, qui vous permet de créer des disques virtuels haute performance résilients jusqu’à 64 téraoctets. Les avantages de ce format sont les suivants :

-   Prise en charge de la capacité de stockage sur disque dur virtuel allant jusqu’à 64 téraoctets.

-   Protection contre l'altération des données lors de pannes d'alimentation en consignant les mises à jour sur les structures de métadonnées VHDX.

-   Possibilité de stocker des métadonnées personnalisées sur un fichier, qu’un utilisateur souhaite enregistrer, telles que la version du système d’exploitation ou les correctifs appliqués.

Le format VHDX offre également les avantages suivants en matière de performances :

-   Meilleur alignement du format de disque dur virtuel pour un fonctionnement correct sur les disques à grands secteurs.

-   Tailles de bloc supérieures pour les disques dynamiques et différentiels, ce qui permet à ces disques de adaptation aux besoins de la charge de travail.

-   disque virtuel de secteur logique de 4 Ko qui permet d’augmenter les performances quand il est utilisé par les applications et les charges de travail qui sont conçues pour les secteurs de 4 Ko.

-   Efficacité dans la représentation des données, ce qui entraîne une plus petite taille de fichier et permet au périphérique de stockage physique sous-jacent de récupérer l’espace inutilisé. (La suppression requiert des disques directs ou SCSI et un matériel compatible avec Trim.)

Lorsque vous effectuez une mise à niveau vers Windows Server 2016, nous vous recommandons de convertir tous les fichiers VHD au format VHDX en raison de ces avantages. Le seul scénario dans lequel il serait judicieux de conserver les fichiers au format VHD est lorsqu’un ordinateur virtuel peut être déplacé vers une version antérieure d’Hyper-V qui ne prend pas en charge le format VHDX.

## <a name="types-of-virtual-hard-disk-files"></a>Types de fichiers de disque dur virtuel

Il existe trois types de fichiers VHD. Les sections suivantes présentent les caractéristiques de performances et les compromis entre les types.

Les recommandations suivantes doivent être prises en considération en ce qui concerne la sélection d’un type de fichier VHD :

-   Lorsque vous utilisez le format VHD, nous vous recommandons d’utiliser le type fixed, car il offre de meilleures résilience et des caractéristiques de performances par rapport aux autres types de fichiers VHD.

-   Lorsque vous utilisez le format VHDX, nous vous recommandons d’utiliser le type dynamique, car il offre des garanties de résilience en plus des économies d’espace associées à l’allocation d’espace uniquement lorsque cela est nécessaire.

-   Le type fixe est également recommandé, quel que soit le format, lorsque le stockage sur le volume d’hébergement n’est pas activement surveillé pour s’assurer que l’espace disque disponible est suffisant lors du développement du fichier de disque dur virtuel au moment de l’exécution.

-   Les instantanés d’un ordinateur virtuel créent un disque dur virtuel de différenciation pour stocker les écritures sur les disques. Le fait d’avoir uniquement quelques instantanés peut élever l’utilisation du processeur des e/s de stockage, mais il peut ne pas affecter les performances de manière notable, sauf dans les charges de travail serveur nécessitant beaucoup d’e/s. Toutefois, le fait de disposer d’une longue chaîne d’instantanés peut affecter les performances, car la lecture à partir du disque dur virtuel peut nécessiter la vérification des blocs demandés dans de nombreux disques durs virtuels de différenciation. La conservation des chaînes d’instantanés est importante pour assurer une bonne performance des e/s disque.

## <a name="fixed-virtual-hard-disk-type"></a>Type de disque dur virtuel fixe

L’espace du disque dur virtuel est d’abord alloué lors de la création du fichier de disque dur virtuel. Ce type de fichier VHD est moins susceptible de fragmenter, ce qui réduit le débit d’e/s quand une seule e/s est fractionnée en plusieurs e/s. Il a la charge de processeur la plus faible des trois types de fichiers VHD, car les lectures et les écritures n’ont pas besoin de rechercher le mappage du bloc.

## <a name="dynamic-virtual-hard-disk-type"></a>Type de disque dur virtuel dynamique

L’espace du disque dur virtuel est alloué à la demande. Les blocs du disque démarrent comme des blocs non alloués et ne sont pas sauvegardés par un espace réel dans le fichier. Quand un bloc est écrit pour la première fois dans, la pile de virtualisation doit allouer de l’espace dans le fichier de disque dur virtuel pour le bloc, puis mettre à jour les métadonnées. Cela augmente le nombre d’e/s disque nécessaires pour l’écriture et augmente l’utilisation du processeur. Les lectures et écritures dans des blocs existants entraînent un accès au disque et une surcharge du processeur lors de la recherche du mappage des blocs dans les métadonnées.

## <a name="differencing-virtual-hard-disk-type"></a>Type de disque dur virtuel de différenciation

Le disque dur virtuel pointe vers un fichier de disque dur virtuel parent. Les écritures dans des blocs non écrits entraînent l’allocation d’espace dans le fichier VHD, comme avec un disque dur virtuel de taille dynamique. Les lectures sont effectuées à partir du fichier VHD si le bloc a été écrit. Dans le cas contraire, elles sont desservies à partir du fichier de disque dur virtuel parent. Dans les deux cas, les métadonnées sont lues pour déterminer le mappage du bloc. Les lectures et écritures sur ce disque dur virtuel peuvent consommer plus d’UC et entraîner plus d’e/s qu’un fichier VHD fixe.

## <a name="block-size-considerations"></a>Considérations relatives à la taille des blocs

La taille des blocs peut avoir un impact significatif sur les performances. Il est préférable de faire correspondre la taille du bloc aux modèles d’allocation de la charge de travail qui utilise le disque. Par exemple, si une application alloue des segments de 16 Mo, il serait optimal d’avoir une taille de bloc de disque dur virtuel de 16 Mo. Une taille de bloc &gt;de 2 Mo est possible uniquement sur les disques durs virtuels avec le format VHDX. Le fait d’avoir une taille de bloc supérieure à celle du modèle d’allocation pour une charge de travail d’e/s aléatoire augmente considérablement l’utilisation de l’espace sur l’hôte.

## <a name="sector-size-implications"></a>Implications de la taille des secteurs

La majeure partie de l’industrie des logiciels est tributaire des secteurs de disque de 512 octets, mais la norme passe aux secteurs de disque de 4 Ko. Pour réduire les problèmes de compatibilité qui peuvent provenir d’une modification de la taille des secteurs, les fournisseurs de disques durs introduisent une taille de transition appelée « lecteurs d’émulation 512 » (émulation).

Ces lecteurs d’émulation offrent certains des avantages offerts par les lecteurs natifs du secteur de disque de 4 Ko, par exemple l’efficacité améliorée du format et un schéma amélioré pour les codes de correction des erreurs (ECC). Ils présentent moins de problèmes de compatibilité qui se produisent en exposant une taille de secteur de 4 Ko au niveau de l’interface de disque.

## <a name="support-for-512e-disks"></a>Prise en charge des disques émulation

Un disque émulation peut effectuer une écriture uniquement en termes d’un secteur physique, autrement dit, il ne peut pas écrire directement un secteur 512byte qui lui est délivré. Le processus interne dans le disque qui rend ces écritures possibles est le suivant :

-   Le disque lit le secteur physique de 4 Ko dans son cache interne, qui contient le secteur logique de 512 octets référencé dans l’écriture.

-   Les données dans la mémoire tampon de 4 Ko sont modifiées pour inclure le secteur de 512 octets mis à jour.

-   Le disque effectue une écriture de la mémoire tampon de 4 Ko mise à jour vers son secteur physique sur le disque.

Ce processus est appelé lecture-modification-écriture (RMW). L’impact global sur les performances du processus RMW dépend des charges de travail. Le processus RMW entraîne une dégradation des performances des disques durs virtuels pour les raisons suivantes :

-   Les disques durs virtuels dynamiques et de différenciation ont un bitmap de secteur de 512 octets devant leur charge utile de données. En outre, les localisateurs de pied de page, d’en-tête et parents s’alignent sur un secteur de 512 octets. Il est courant pour le pilote de disque dur virtuel d’émettre des commandes d’écriture de 512 octets pour mettre à jour ces structures, ce qui entraîne le processus RMW décrit précédemment.

-   Les applications émettent généralement des lectures et des écritures dans des multiples de tailles de 4 Ko (la taille de cluster par défaut de NTFS). Étant donné qu’il y a une image bitmap de secteur de 512 octets devant le bloc de charge utile de données des disques durs virtuels dynamiques et de différenciation, les blocs de 4 Ko ne sont pas alignés sur la limite physique de 4 Ko. L’illustration suivante montre un bloc VHD de 4 Ko (en surbrillance) qui n’est pas aligné avec la limite physique de 4 Ko.

![bloc VHD 4 Ko](../../media/perftune-guide-vhd-4kb-block.png)

Chaque commande d’écriture de 4 Ko émise par l’analyseur actuel pour mettre à jour les données de charge utile génère deux lectures pour deux blocs sur le disque, qui sont ensuite mis à jour et réécrits ensuite sur les deux blocs de disques. Hyper-V dans Windows Server 2016 atténue certains des effets de performances sur les disques émulation sur la pile de disque dur virtuel en préparant les structures mentionnées précédemment pour l’alignement sur des limites de 4 Ko au format VHD. Cela évite l’effet de RMW lors de l’accès aux données dans le fichier de disque dur virtuel et lors de la mise à jour des structures de métadonnées du disque dur virtuel.

Comme mentionné précédemment, les VHD copiés à partir de versions antérieures de Windows Server ne sont pas automatiquement alignés sur 4 Ko. Vous pouvez les convertir manuellement pour les aligner de manière optimale à l’aide de l’option **copier à partir du disque source** disponible dans les interfaces VHD.

Par défaut, les disques durs virtuels sont exposés avec une taille de secteur physique de 512 octets. Cela permet de s’assurer que les applications dépendantes de la taille de secteur physique ne sont pas affectées lorsque l’application et les disques durs virtuels sont déplacés à partir d’une version antérieure de Windows Server.

Par défaut, les disques au format VHDX sont créés avec la taille de secteur physique de 4 Ko pour optimiser leur profil de performances, les disques normaux et les disques de secteurs importants. Pour tirer pleinement parti des secteurs de 4 Ko, il est recommandé d’utiliser le format VHDX.

## <a name="support-for-native-4kb-disks"></a>Prise en charge des disques natifs de 4 Ko

Hyper-V dans Windows Server 2012 R2 et versions ultérieures prend en charge des disques natifs de 4 Ko. Toutefois, il est toujours possible de stocker le disque dur virtuel sur un disque natif de 4 Ko. Cela s’effectue en implémentant un algorithme RMW logiciel dans la couche de pile de stockage virtuel qui convertit les demandes d’accès et de mise à jour de 512 octets en accès et mises à jour de 4 Ko correspondants.

Étant donné que le fichier VHD peut uniquement s’exposer en tant que disques de taille de secteur logique de 512 octets, il est très probable qu’il y aura des applications qui émettent des demandes d’e/s de 512 octets. Dans ce cas, la couche RMW répond à ces demandes et entraîne une dégradation des performances. Cela est également vrai pour un disque au format VHDX qui a une taille de secteur logique de 512 octets.

Il est possible de configurer un fichier VHDX à exposer en tant que disque de taille de secteur logique de 4 Ko, ce qui constitue une configuration optimale pour les performances lorsque le disque est hébergé sur un appareil physique natif de 4 Ko. Veillez à veiller à ce que l’invité et l’application qui utilise le disque virtuel bénéficient de la taille de secteur logique de 4 Ko. La mise en forme VHDX fonctionne correctement sur un appareil de taille de secteur logique de 4 Ko.

## <a name="pass-through-disks"></a>Disques directs

Le disque dur virtuel d’une machine virtuelle peut être mappé directement sur un disque physique ou un numéro d’unité logique (LUN), plutôt que sur un fichier VHD. L’avantage est que cette configuration contourne le système de fichiers NTFS dans la partition racine, ce qui réduit l’utilisation du processeur des e/s de stockage. Le risque est que les disques physiques ou les numéros d’unités logiques peuvent être plus difficiles à déplacer entre les ordinateurs que les fichiers VHD.

Les disques directs doivent être évités en raison des limitations introduites dans les scénarios de migration de machines virtuelles.

## <a name="advanced-storage-features"></a>Fonctionnalités de stockage avancées

### <a name="storage-quality-of-service-qos"></a>Qualité de service de stockage

À compter de Windows Server 2012 R2, Hyper-V offre la possibilité de définir certains paramètres de qualité de service (QoS) pour le stockage sur les ordinateurs virtuels. La qualité de service de stockage fournit un isolement des performances de stockage dans un environnement multiclient et des mécanismes permettant de vous informer quand les performances d'E/S n'atteignent pas le seuil défini pour exécuter efficacement les charges de travail de vos ordinateurs virtuels.

La qualité de service de stockage permet de spécifier une valeur maximale d'opérations d'entrée/sortie par seconde pour votre disque dur virtuel. Un administrateur peut limiter les E/S de stockage pour empêcher un client de consommer trop de ressources de stockage, ce qui pourrait avoir un impact sur un autre client.

Vous pouvez également définir une valeur d’e/s par seconde minimale. et être informé quand le nombre d'opérations d'E/S par seconde pour un disque dur virtuel donné est au-dessous du seuil nécessaire pour bénéficier de performances optimales.

L'infrastructure de métriques d'ordinateur virtuel est également mise à jour avec des paramètres liés au stockage afin de permettre à l'administrateur de surveiller les performances et les paramètres liés à la rétrofacturation.

Les valeurs maximales et minimales sont spécifiées en termes d’e/s par seconde normalisées, où toutes les 8 Ko de données sont comptabilisées comme une e/s.

Voici quelques-unes des limitations suivantes :

-   Uniquement pour les disques virtuels

-   Le disque de différenciation ne peut pas avoir un disque virtuel parent sur un volume différent

-   Réplica-QoS pour le site de réplication configuré séparément du site principal

-   Le VHDX partagé n’est pas pris en charge

Pour plus d’informations sur la qualité de service du stockage, voir [qualité de service de stockage pour Hyper-V](https://technet.microsoft.com/library/dn282281.aspx).

### <a name="numa-io"></a>E/S NUMA

Windows Server 2012 et versions ultérieures prennent en charge des machines virtuelles volumineuses, et toute configuration d’ordinateur virtuel de grande taille (par exemple, une configuration avec Microsoft SQL Server s’exécutant avec des processeurs virtuels 64) aura également besoin d’une évolutivité en termes de débit d’e/s.

Les améliorations clés suivantes introduites en premier dans la pile de stockage Windows Server 2012 et Hyper-V fournissent les besoins d’évolutivité des e/s des machines virtuelles volumineuses :

-   Augmentation du nombre de canaux de communication créés entre les appareils invités et la pile de stockage hôte.

-   Un mécanisme d’achèvement d’e/s plus efficace implique la distribution des interruptions entre les processeurs virtuels pour éviter les interruptions de l’interprocesseur.

Introduite dans Windows Server 2012, il existe quelques entrées de Registre, situées au\\niveau\\de\\HKLM System\\CurrentControlSet Enum\\VMBUS {\\ID d’appareil} {ID d’instance}\\StorChannel, qui permet d’ajuster le nombre de canaux. Ils alignent également les processeurs virtuels qui gèrent les terminaisons d’e/s sur les processeurs virtuels affectés par l’application en tant que processeurs d’e/s. Les paramètres du Registre sont configurés sur une base par adaptateur sur la clé matérielle de l’appareil.

-   **ChannelCount (DWORD)** Nombre total de canaux à utiliser, avec un maximum de 16. Par défaut, il s’agit d’un plafond, qui est le nombre de processeurs virtuels/16.

-   **ChannelMask (QWord)** Affinité du processeur pour les canaux. Si elle n’est pas définie ou si elle a la valeur 0, l’algorithme de distribution de canal existant que vous utilisez pour le stockage normal ou pour les canaux de mise en réseau est utilisé par défaut. Cela garantit que vos canaux de stockage ne sont pas en conflit avec vos canaux réseau.

### <a name="offloaded-data-transfer-integration"></a>Intégration des Transfert de données déchargées

Des tâches de maintenance cruciales pour les disques durs virtuels, telles que la fusion, le déplacement et le compactage, dépendent de la copie de grandes quantités de données. La méthode actuelle de copie des données nécessite la lecture et l'écriture des données à différents emplacements, ce qui peut prendre du temps. Il utilise également les ressources processeur et mémoire sur l’ordinateur hôte, qui pourrait avoir été utilisé pour traiter des ordinateurs virtuels.

Les fournisseurs de réseau de zone de stockage (SAN) mettent tout en œuvre pour permettre des opérations de copie quasi-instantanée de grandes quantités de données. Ce stockage est conçu pour permettre au système situé au-dessus des disques de spécifier le déplacement d’un jeu de données spécifique d’un emplacement à un autre. Cette fonctionnalité matérielle est connue sous le nom de Transfert de données déchargée.

Hyper-V dans Windows Server 2012 et versions ultérieures prend en charge les opérations de déchargement Transfert de données (ODX) afin que ces opérations puissent être passées du système d’exploitation invité au matériel hôte. Cela garantit que la charge de travail peut utiliser le stockage ODX si elle s’exécutait dans un environnement non virtualisé. La pile de stockage Hyper-V émet également des opérations ODX pendant les opérations de maintenance pour les disques durs virtuels, telles que la fusion de disques et les méta-opérations de migration de stockage où de grandes quantités de données sont déplacées.

### <a name="unmap-integration"></a>Annuler le mappage

Les fichiers de disque dur virtuel existent en tant que fichiers sur un volume de stockage et ils partagent de l’espace disponible avec d’autres fichiers. Étant donné que la taille de ces fichiers a tendance à être importante, l’espace qu’ils utilisent peut croître rapidement. La demande d’un stockage physique plus grand affecte le budget du matériel informatique. Il est important d’optimiser l’utilisation du stockage physique autant que possible.

Avant Windows Server 2012, lorsque les applications suppriment du contenu au sein d’un disque dur virtuel, ce qui a entraîné l’abandon de l’espace de stockage du contenu, la pile de stockage Windows dans le système d’exploitation invité et l’hôte Hyper-V avaient des limitations qui l’empêchaient. informations transmises au disque dur virtuel et au périphérique de stockage physique. Cela empêchait la pile de stockage Hyper-V d’optimiser l’utilisation de l’espace par les fichiers de disque virtuel basés sur VHD. Le dispositif de stockage sous-jacent empêchait également la récupération de l’espace occupé par les données supprimées.

À partir de Windows Server 2012, Hyper-V prend en charge les notifications de mappage, ce qui permet aux fichiers VHDX d’être plus efficaces pour représenter les données qu’il contient. Cela réduit la taille des fichiers et permet au périphérique de stockage physique sous-jacent de récupérer l’espace inutilisé.

Seuls les contrôleurs SCSI spécifiques à Hyper-V, les contrôleurs de l’environnement de développement intégré et les contrôleurs de Fibre Channel virtuels autorisent la commande de mappage de l’invité à atteindre la pile de stockage virtuelle de l’ordinateur hôte. Sur les disques durs virtuels, seuls les disques virtuels au format VHDX prennent en charge le mappage des commandes à partir de l’invité.

Pour ces raisons, nous vous recommandons d’utiliser des fichiers VHDX attachés à un contrôleur SCSI quand vous n’utilisez pas de disques Fibre Channel virtuels.

## <a name="see-also"></a>Voir aussi

-   [Terminologie Hyper-V](terminology.md)

-   [Architecture Hyper-V](architecture.md)

-   [Configuration du serveur Hyper-V](configuration.md)

-   [Performances du processeur Hyper-V](processor-performance.md)

-   [Performances de la mémoire Hyper-V](memory-performance.md)

-   [Performances E/S du réseau Hyper-V](network-io-performance.md)

-   [Détecter les goulots d’étranglement dans un environnement virtualisé](detecting-virtualized-environment-bottlenecks.md)

-   [Ordinateurs virtuels Linux](linux-virtual-machine-considerations.md)
