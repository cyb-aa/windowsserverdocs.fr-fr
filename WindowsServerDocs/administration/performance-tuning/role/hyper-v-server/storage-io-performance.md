---
title: Performances d’e/s de stockage Hyper-V
description: Considérations de performances d’e/s de stockage de réglage des performances d’Hyper-V
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fedc23083914bcf97a8cde12b78c0b174143de25
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831310"
---
# <a name="hyper-v-storage-io-performance"></a>Performances d’e/s de stockage Hyper-V

Cette section décrit les différentes options et les considérations pour le réglage des performances d’e/s sur une machine virtuelle de stockage. Le chemin d’accès d’e/s de stockage s’étend de la pile de stockage invité, via la couche de virtualisation hôte, à la pile de stockage hôte, puis sur le disque physique. Voici des explications sur la façon dont les optimisations sont possibles à chacune de ces étapes.

## <a name="virtual-controllers"></a>Contrôleurs virtuels

Hyper-V propose trois types de contrôleurs virtuels : IDE, SCSI et virtuelles adaptateurs de bus hôte (HBA).

## <a name="ide"></a>IDE

Les contrôleurs IDE doivent exposent des disques IDE à la machine virtuelle. Le contrôleur IDE est émulé, et il est le seul contrôleur est disponible pour les machines virtuelles invitées exécute une version antérieure de Windows sans les Services d’intégration de Machine virtuelle. Disque d’e/s qui est effectuée à l’aide du pilote de filtre IDE qui est fourni avec les Services d’intégration de Machine virtuelle est considérablement plus performants que les performances d’e/s qui est fourni avec le contrôleur IDE émulé sur disque. Nous recommandons que les disques IDE être utilisé uniquement pour les disques de système d’exploitation, car ils ont des limitations de performances en raison de la taille maximale d’e/s qui peuvent être émises pour ces appareils.

## <a name="scsi-sas-controller"></a>SCSI (contrôleur SAS)

Contrôleurs SCSI exposent des disques SCSI à la machine virtuelle, et chaque contrôleur SCSI virtuel peut prendre en charge jusqu'à 64 périphériques. Pour des performances optimales, nous vous recommandons d’attacher plusieurs disques à un seul contrôleur SCSI virtuel et de créer des contrôleurs supplémentaires uniquement lorsqu’ils sont requis à l’échelle le nombre de disques sont connectés à la machine virtuelle. Chemin d’accès SCSI n’est pas émulée ce qui rend le contrôleur par défaut pour n’importe quel disque autre que le disque du système d’exploitation. En fait, avec les machines virtuelles de génération 2, il est le seul type de contrôleur possible. Introduite dans Windows Server 2012 R2, ce contrôleur est signalé comme SAP pour prendre en charge VHDX partagé.

## <a name="virtual-fibre-channel-hbas"></a>Adaptateurs de bus hôte Fibre Channel virtuel

Adaptateurs de bus hôte Fibre Channel virtuel peuvent être configurés pour autoriser un accès direct pour les machines virtuelles pour Fibre Channel et Fibre Channel sur Ethernet (FCoE) LUN. Les disques Fibre Channel virtuel contournent le système de fichiers NTFS dans la partition racine, ce qui réduit l’utilisation du processeur de stockage d’e/s.

Les lecteurs de données volumineux et les lecteurs qui sont partagés entre plusieurs machines virtuelles (pour le clustering invité scénarios) sont des candidats idéaux pour les disques Fibre Channel virtuels.

Les disques de Fibre Channel virtuel requièrent un ou plusieurs Fibre Channel adaptateurs de bus hôte (HBA) à être installé sur l’ordinateur hôte. Chaque hôte HBA est obligé d’utiliser un pilote HBA prenant en charge les fonctionnalités de Windows Server 2016 Virtual Fibre Channel/NPIV. L’infrastructure de réseau SAN doit prendre en charge NPIV, et l’ou les ports HBA utilisés pour le Fibre Channel virtuel doivent être configuré dans une topologie Fibre Channel qui prend en charge NPIV.

Pour maximiser le débit sur les ordinateurs hôtes qui sont installés avec plusieurs adaptateurs HBA, nous vous recommandons de configurer plusieurs HBA virtuel à l’intérieur de la machine virtuelle Hyper-V (jusqu'à quatre adaptateurs de bus hôte peut être configuré pour chaque machine virtuelle). Hyper-V apporte automatiquement un meilleur effort pour équilibrer les HBA virtuels pour les adaptateurs HBA hôtes qui accèdent à la même réseau SAN virtuel.

## <a name="virtual-disks"></a>Disques virtuels

Les disques peuvent être exposées aux ordinateurs virtuels via les contrôleurs virtuels. Ces disques pourraient être des disques durs virtuels qui sont des abstractions de fichier d’un disque ou un disque direct sur l’ordinateur hôte.

## <a name="virtual-hard-disks"></a>disques durs virtuels ;

Il existe deux formats de disque dur virtuel, VHD et VHDX. Chacun de ces formats prend en charge trois types de fichiers de disque dur virtuel.

## <a name="vhd-format"></a>Format de disque dur virtuel

Le format de disque dur virtuel a été le format de disque dur virtuel uniquement pris en charge par Hyper-V dans les versions précédentes. Introduite dans Windows Server 2012, le format de disque dur virtuel a été modifié pour permettre une meilleure adéquation, entraînant ainsi améliorer considérablement les performances sur de nouveaux disques à grands secteurs.

N’importe quel nouveau disque dur virtuel est créé sur un ordinateur Windows Server 2012 ou version ultérieure a l’alignement de 4 Ko optimal. Ce format aligné est entièrement compatible avec les systèmes d’exploitation Windows Server précédents. Toutefois, la propriété d’alignement sera rompue pour les nouvelles allocations à partir d’analyseurs qui ne sont pas de 4 Ko alignement prenant en charge telles que (un analyseur de disque dur virtuel à partir d’une version précédente de Windows Server) ou un analyseur non Microsoft.

Un disque dur virtuel est déplacé d’une version précédente ne pas automatiquement converti en ce nouveau format de disque dur virtuel amélioré.

Pour convertir au nouveau format de disque dur virtuel, exécutez la commande Windows PowerShell suivante :

``` syntax
Convert-VHD –Path E:\vms\testvhd\test.vhd –DestinationPath E:\vms\testvhd\test-converted.vhd
```

Vous pouvez vérifier la propriété d’alignement pour tous les disques durs virtuels sur le système, et elle doit être convertie en l’alignement de 4 Ko optimal. Vous créez un nouveau disque dur virtuel avec les données à partir du disque dur virtuel d’origine à l’aide de la **créer à partir de sources** option.

Pour vérifier l’alignement à l’aide de Windows Powershell, examinez la ligne d’alignement, comme indiqué ci-dessous :

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

Pour vérifier l’alignement à l’aide de Windows PowerShell, examinez la ligne d’alignement, comme indiqué ci-dessous :

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

VHDX est un nouveau format de disque dur virtuel introduit dans Windows Server 2012, qui permet de créer des disques virtuels de résilientes hautes performances pouvant atteindre 64 téraoctets. Avantages de ce format :

-   Prise en charge de la capacité de stockage de disque dur virtuel de jusqu'à 64 téraoctets.

-   Protection contre l'altération des données lors de pannes d'alimentation en consignant les mises à jour sur les structures de métadonnées VHDX.

-   Possibilité de stocker des métadonnées personnalisées sur un fichier, un utilisateur souhaite enregistrer, telles que la version du système d’exploitation ou les correctifs appliqués.

Le format VHDX fournit également les avantages de performances suivants :

-   Meilleur alignement du format de disque dur virtuel pour un fonctionnement correct sur les disques à grands secteurs.

-   Tailles de bloc supérieures pour les disques dynamiques et différentielles, ce qui permet de ces disques à l’adaptation aux besoins de la charge de travail.

-   Disque virtuel de 4 Ko secteur logique qui permet de meilleures performances lorsqu’il est utilisé par les applications et charges de travail qui sont conçus pour les secteurs de 4 Ko.

-   Efficacité dans la représentation des données, ce qui entraîne la taille de fichier et permettant à l’appareil de stockage physique sous-jacent récupérer l’espace inutilisé. (Le découpage nécessite directe ou de disques SCSI et de matériel compatible avec trim.)

Lorsque vous mettez à niveau vers Windows Server 2016, nous vous conseillons de convertir tous les fichiers de disque dur virtuel au format VHDX en raison de ces avantages. Le seul scénario dans lequel il serait judicieux de conserver les fichiers dans le format de disque dur virtuel est lorsqu’une machine virtuelle est susceptible d’être déplacé vers une version antérieure d’Hyper-V qui ne prend pas en charge le format VHDX.

## <a name="types-of-virtual-hard-disk-files"></a>Types de fichiers de disque dur virtuel

Il existe trois types de fichiers de disque dur virtuel. Les sections suivantes sont les caractéristiques de performances et les compromis entre les types.

Les recommandations suivantes doivent être prises en considération en ce qui concerne la sélection d’un type de fichier de disque dur virtuel :

-   Lorsque vous utilisez le format de disque dur virtuel, nous vous recommandons d’utiliser le type fixe, car il a une meilleure résilience et les caractéristiques de performances par rapport aux autres types de fichiers de disque dur virtuel.

-   Lorsque vous utilisez le format VHDX, nous vous recommandons d’utiliser le type dynamique, car il offre des garanties de résilience en plus des gains d’espace associés à l’allocation d’espace uniquement lorsqu’il est nécessaire pour ce faire.

-   Le type fixe est également recommandé, quel que soit le format, lorsque le stockage sur le volume hôte n’est pas activement surveillé pour vous assurer que suffisamment d’espace disque est présent lors du développement du fichier de disque dur virtuel en cours d’exécution.

-   Créent des instantanés d’une machine virtuelle une différenciation VHD pour stocker les écritures sur les disques. Avoir uniquement quelques instantanés peuvent élever l’utilisation du processeur de stockage e/s, mais il peut pas considérablement affecter les performances, sauf dans les charges de travail hautement e/S intensives server. Toutefois, une longue chaîne d’instantanés risquent d’affecter les performances, car la lecture à partir du disque dur virtuel peut exiger la vérification des blocs demandés dans nombreux disques durs virtuels de différenciation. Conservation des chaînes d’instantané courts est important pour maintenir les performances d’e/s de disque de bonne.

## <a name="fixed-virtual-hard-disk-type"></a>Type de disque dur virtuel fixe

Espace du disque dur virtuel est d’abord alloué lorsque le fichier de disque dur virtuel est créé. Ce type de fichier de disque dur virtuel est moins susceptible de fragment, ce qui réduit le débit d’e/s lorsqu’une seule e/s est divisée en plusieurs e/s. Il a la surcharge du processeur la plus faible des trois types de fichier de disque dur virtuel, car les lectures et écritures est inutile de rechercher le mappage du bloc.

## <a name="dynamic-virtual-hard-disk-type"></a>Type de disque dur virtuel dynamique

Espace du disque dur virtuel est alloué à la demande. Les blocs dans le disque de démarrer en tant que blocs non alloués et ne sont pas soutenues par n’importe quel espace réel dans le fichier. Lorsqu’un bloc est d’abord écrites dans, la pile de virtualisation doit allouer de l’espace dans le fichier de disque dur virtuel pour le bloc et puis mettez à jour les métadonnées. Cela augmente le nombre d’e/s de disque nécessaire pour l’écriture et augmente l’utilisation du processeur. Lit et écrit pour les blocs existants entraîne des accès au disque et la surcharge du processeur lors de la recherche de mappage des blocs dans les métadonnées.

## <a name="differencing-virtual-hard-disk-type"></a>Type de disque dur virtuel de différenciation

Le disque dur virtuel pointe vers un fichier de disque dur virtuel parent. Toutes les écritures dans les blocs ne pas écrites dans le résultat dans l’espace est alloué dans le fichier de disque dur virtuel, qu’avec un disque dur virtuel de taille dynamique. Lectures sont traitées à partir du fichier de disque dur virtuel si le bloc a été écrit dans. Sinon, ils sont gérés à partir du fichier de disque dur virtuel parent. Dans les deux cas, les métadonnées sont lu pour déterminer le mappage du bloc. Lectures et écritures sur ce disque dur virtuel peuvent consommer davantage de ressources processeur et entraîne des e/s plus qu’un fichier de disque dur virtuel fixe.

## <a name="block-size-considerations"></a>Considérations de taille de bloc

Taille de bloc peut affecter considérablement les performances. Il est souhaitable de correspond à la taille de bloc pour les modèles d’allocation de la charge de travail qui utilise le disque. Par exemple, si une application alloue en blocs de 16 Mo, il serait préférable d’avoir une taille de bloc de disque dur virtuel de 16 Mo. Une taille de bloc de &gt;2 Mo est possible uniquement sur les disques durs virtuels avec le format VHDX. Avoir une plus grande taille de bloc que le modèle d’allocation pour une charge de travail d’e/s aléatoire vise à augmenter considérablement l’utilisation de l’espace sur l’ordinateur hôte.

## <a name="sector-size-implications"></a>Implications en matière de taille de secteur

La majeure partie de l’industrie des logiciels a dépendu de secteurs de disque de 512 octets, mais le déplacement de la norme pour les secteurs de disque de 4 Ko. Pour réduire les problèmes de compatibilité qui peuvent se produire à partir d’un changement de taille de secteur, les fournisseurs de disques durs sont présentation une taille transitoire appelée lecteurs d’émulation 512 (émulation de 512 octets).

Ces lecteurs d’émulation offrent certains des avantages offerts par 4 Ko secteur natif lecteurs de disque, telles que l’efficacité du format renforcée et un schéma amélioré pour les codes de correction d’erreur (ECC). Ils sont fournis avec moins de problèmes de compatibilité qui seraient produisent en exposant une taille de secteur de 4 Ko au niveau de l’interface de disque.

## <a name="support-for-512e-disks"></a>Prise en charge des disques d’émulation de 512 octets

Un disque de l’émulation de 512 octets permettre effectuer une écriture uniquement en termes d’un secteur physique, autrement dit, il ne peut pas écrire directement d’un secteur de 512 octets est émis. Le processus interne dans le disque qui rend ces écritures possible procédez comme suit :

-   Le disque lit le secteur physique de 4 Ko à son cache interne, qui contient le secteur logique de 512 octets fait référence dans l’écriture.

-   Les données dans la mémoire tampon de 4 Ko sont modifiées pour inclure le secteur de 512 octets mis à jour.

-   Le disque effectue une écriture de la mémoire tampon de 4 Ko mise à jour vers son secteur physique sur le disque.

Ce processus est appelé en lecture-modification-écriture (RMW). L’impact sur les performances globales du processus RMW varie selon les charges de travail. Le processus RMW entraîne une dégradation des performances de disques durs virtuels pour les raisons suivantes :

-   Les disques durs virtuels dynamiques et de différenciation ont un bitmap de secteur de 512 octets devant leur charge utile de données. En outre, les localisateurs de parent, en-tête et pied de page alignent dans un secteur de 512 octets. Il est courant pour le pilote de disque dur virtuel d’émettre des commandes de 512 octets écriture pour mettre à jour de ces structures, ce qui entraîne le processus RMW décrit précédemment.

-   Applications couramment problème lit et en multiples de tailles de 4 Ko (la taille de cluster par défaut de NTFS). Car il existe un bitmap de secteur de 512 octets devant le bloc de charge utile de données dynamique et les disques durs virtuels de différenciation, les blocs de 4 Ko ne sont pas alignées sur la limite physique de 4 Ko. La figure suivante montre un disque dur virtuel 4 Ko bloc (mis en surbrillance) qui est ne pas aligné avec la limite physique de 4 Ko.

![bloc de 4 Ko de disque dur virtuel](../../media/perftune-guide-vhd-4kb-block.png)

Chaque commande d’écriture de 4 Ko qui est émis par l’analyseur actuel pour mettre à jour les données de charge utile génère deux lectures pour deux blocs sur le disque, qui sont ensuite mis à jour et par la suite mis à jour pour les deux blocs de disques. Hyper-V dans Windows Server 2016 permet d’atténuer certains effets de performances sur les disques d’émulation de 512 octets sur la pile de disque dur virtuel en préparant les structures mentionnées précédemment pour l’alignement des limites de 4 Ko dans le format de disque dur virtuel. Cela évite l’effet RMW lors de l’accès aux données dans le fichier de disque dur virtuel et la mise à jour les structures de métadonnées de disque dur virtuel.

Comme mentionné précédemment, les disques durs virtuels qui sont copiés à partir de versions précédentes de Windows Server ne seront pas automatiquement être alignées à 4 Ko. Vous pouvez les convertir manuellement en à aligner de façon optimale à l’aide du **copie à partir de la Source** option est disponible dans les interfaces de disque dur virtuel de disque.

Par défaut, les disques durs virtuels sont affichés avec une taille de secteur physique de 512 octets. Cela est fait pour vous assurer que les applications dépendantes de taille de secteur physique ne sont pas affectées lorsque l’application et les disques durs virtuels sont déplacés à partir d’une version précédente de Windows Server.

Par défaut, les disques avec le format VHDX sont créés avec la taille de secteur physique de 4 Ko pour optimiser leurs disques régulière de profil de performances et les disques à grands secteurs. Pour tirer pleinement parti des secteurs de 4 Ko, il est recommandé d’utiliser le format VHDX.

## <a name="support-for-native-4kb-disks"></a>Prise en charge des disques de 4 Ko native

Hyper-V dans Windows Server 2012 R2 et au-delà de 4 Ko prend en charge des disques natifs. Mais il est toujours possible de stocker le disque dur virtuel sur le disque natif de 4 Ko. Cela est accompli en implémentant un logiciel RMW algorithme dans la couche de pile de stockage virtuel qui convertit les demandes de mise à jour et des accès de 512 octets 4 Ko correspondants accède et met à jour.

Étant donné que le fichier de disque dur virtuel peut s’exposent uniquement en tant que disques de taille de secteur logique de 512 octets, il est très probable qu’il y aura des applications qui émettent des demandes d’e/s de 512 octets. Dans ce cas, la couche RMW sera satisfaire à ces demandes et entraîner une dégradation des performances. Cela vaut également pour un disque au format VHDX qui a une taille de secteur logique de 512 octets.

Il est possible de configurer un fichier VHDX doit être exposée comme un disque de taille de secteur logique de 4 Ko, et cela serait une configuration optimale pour les performances lorsque le disque est hébergé sur un appareil physique de 4 Ko native. Être vigilant pour vous assurer que l’invité et l’application qui utilise le disque virtuel sont soutenus par la taille de secteur logique de 4 Ko. La mise en forme de VHDX ne fonctionne pas correctement sur un appareil de taille de secteur logique de 4 Ko.

## <a name="pass-through-disks"></a>Disques directs

Le disque dur virtuel sur un ordinateur virtuel peut être mappé directement à un disque physique ou un numéro d’unité logique (LUN), au lieu d’un fichier de disque dur virtuel. L’avantage est que cette configuration contourne le système de fichiers NTFS dans la partition racine, ce qui réduit l’utilisation du processeur de stockage d’e/s. Le risque est que les disques physiques ou des numéros d’unités logiques peuvent être plus difficiles à déplacer entre des ordinateurs que les fichiers de disque dur virtuel.

Les disques directs doivent être évitées en raison des limitations introduites avec les scénarios de migration de machine virtuelle.

## <a name="advanced-storage-features"></a>Fonctionnalités de stockage avancées

### <a name="storage-quality-of-service-qos"></a>Qualité de service de stockage

À compter de Windows Server 2012 R2, Hyper-V inclut la possibilité de définir certains paramètres (QoS) de qualité de service pour le stockage sur les machines virtuelles. La qualité de service de stockage fournit un isolement des performances de stockage dans un environnement multiclient et des mécanismes permettant de vous informer quand les performances d'E/S n'atteignent pas le seuil défini pour exécuter efficacement les charges de travail de vos ordinateurs virtuels.

La qualité de service de stockage permet de spécifier une valeur maximale d'opérations d'entrée/sortie par seconde pour votre disque dur virtuel. Un administrateur peut limiter les E/S de stockage pour empêcher un client de consommer trop de ressources de stockage, ce qui pourrait avoir un impact sur un autre client.

Vous pouvez également définir une valeur minimale d’e/s. et être informé quand le nombre d'opérations d'E/S par seconde pour un disque dur virtuel donné est au-dessous du seuil nécessaire pour bénéficier de performances optimales.

L'infrastructure de métriques d'ordinateur virtuel est également mise à jour avec des paramètres liés au stockage afin de permettre à l'administrateur de surveiller les performances et les paramètres liés à la rétrofacturation.

Valeurs maximales et minimales sont spécifiées en termes d’e/s normalisées, où chaque 8 Ko de données est comptabilisée comme une e/s.

Certaines de ces restrictions sont les suivantes :

-   Uniquement pour les disques virtuels

-   Disque de différenciation ne peut pas avoir de disque virtuel parent sur un autre volume

-   Réplica - qualité de service pour le site de réplication configuré séparément à partir du site principal

-   VHDX partagé n’est pas pris en charge.

Pour plus d’informations sur la qualité de Service de stockage, consultez [qualité de Service de stockage pour Hyper-V](https://technet.microsoft.com/library/dn282281.aspx).

### <a name="numa-io"></a>E/S DE NUMA

Windows Server 2012 et au-delà des machines virtuelles volumineuses prend en charge et toute configuration de grands groupes de machines virtuelles (par exemple, il s’agit d’une configuration avec Microsoft SQL Server est en cours d’exécution avec 64 processeurs virtuels) doivent également l’évolutivité en termes de débit d’e/s.

Les améliorations clées suivantes introduites dans la pile de stockage de Windows Server 2012 et Hyper-V fournissent les besoins en évolutivité d’e/s de grandes machines virtuelles :

-   Une augmentation du nombre de canaux de communication créés entre les appareils de l’invité et de la pile de stockage hôte.

-   Un mécanisme plus efficace d’e/s achèvement impliquant la distribution d’interruption entre les processeurs virtuels pour éviter les interruptions INTERPROCESSEUR coûteuses.

Introduite dans Windows Server 2012, il existe quelques entrées de Registre, situées dans HKLM\\système\\CurrentControlSet\\Enum\\VMBUS\\{id d’appareil}\\{id d’instance}\\StorChannel, qui permettent le nombre de canaux à ajuster. Ils s’alignent également les processeurs virtuels qui gèrent les saisies semi-automatiques d’e/s pour les processeurs virtuels qui sont affectés par l’application soit les processeurs d’e/s. Les paramètres du Registre sont configurés sur une base par carte de clé matérielle de l’appareil.

-   **ChannelCount (DWORD)** le nombre total de canaux à utiliser, avec un maximum de 16. Un plafond, qui correspond au nombre de processeurs virtuels/16 par défaut.

-   **ChannelMask (QWORD)** l’affinité du processeur pour les canaux. S’il n’est pas définie ou a la valeur 0, la valeur par défaut est l’algorithme de distribution de canal existant que vous utilisez pour le stockage normal ou pour les canaux de mise en réseau. Cela garantit que vos canaux de stockage ne sont pas en conflit avec les canaux de votre réseau.

### <a name="offloaded-data-transfer-integration"></a>Intégration de transfert de données déchargée

Tâches de maintenance essentielles pour les disques durs virtuels, telles que la fusion, déplacent et compact, dépendent de copie de grandes quantités de données. La méthode actuelle de copie des données nécessite la lecture et l'écriture des données à différents emplacements, ce qui peut prendre du temps. Il utilise également des ressources processeur et mémoire sur l’hôte, ce qui pourrait avoir été utilisé pour les machines virtuelles de service.

Les fournisseurs de réseau de zone de stockage (SAN) mettent tout en œuvre pour permettre des opérations de copie quasi-instantanée de grandes quantités de données. Ce stockage est conçu pour permettre au système au-dessus des disques de spécifier le déplacement d’un ensemble spécifique de données à partir d’un emplacement vers un autre. Cette fonctionnalité matérielle est appelée un transfert de données déchargées.

Hyper-V dans Windows Server 2012 et versions ultérieures prend en charge les opérations de transfert de données par déchargement (ODX) afin que ces opérations peuvent être passées du système d’exploitation invité au matériel hôte. Cela garantit que la charge de travail permettre utiliser stockage compatible ODX comme il le ferait s’il était exécuté dans un environnement non virtualisé. La pile de stockage Hyper-V émet également les opérations ODX pendant les opérations de maintenance pour les disques durs virtuels comme la fusion de disques et migration meta-opérations de stockage où les grandes quantités de données sont déplacées.

### <a name="unmap-integration"></a>Annuler le mappage d’intégration

Les fichiers de disque dur virtuel existent en tant que fichiers sur un volume de stockage, et elles partagent l’espace disponible avec d’autres fichiers. Étant donné que la taille de ces fichiers ont tendance à être volumineux, il se peut que l’espace qu’ils occupent peut augmenter rapidement. À la demande pour le stockage physique affecte le budget de matériel informatique. Il est important optimiser l’utilisation du stockage physique autant que possible.

Avant Windows Server 2012, lorsque les applications suppriment du contenu au sein d’un disque dur virtuel, lequel efficacement abandonnée espace de stockage du contenu, la pile de stockage Windows dans le système d’exploitation invité et l’hôte Hyper-V présente les limitations qui a empêché cette informations d’en cours de communication pour le disque dur virtuel et le périphérique de stockage physique. Cela empêchait la pile de stockage Hyper-V à partir de l’optimisation de l’espace utilisé par les fichiers de disque virtuel basé sur le disque dur virtuel. Il empêche également le dispositif de stockage sous-jacent ne peut pas récupérer l’espace précédemment occupé par les données supprimées.

Prend en charge Hyper-V à partir de Windows Server 2012, annuler le mappage des notifications, qui permettent de fichiers VHDX d’être plus efficaces dans la représentation des données qu’il contient. Il en résulte plus petite taille de fichiers, et il permet à l’appareil de stockage physique sous-jacent récupérer l’espace inutilisé.

Compatible avec uniquement spécifiques à Hyper-V SCSI, IDE et Fibre Channel virtuel contrôleurs autoriser la commande Annuler le mappage de l’invité pour atteindre la pile de stockage virtuel d’hôte. Sur les disques durs virtuels, uniquement les disques virtuels VHDX prennent en charge le format annuler le mappage des commandes à partir de l’invité.

Pour ces raisons, nous vous recommandons d’utiliser les fichiers VHDX attachés à un contrôleur SCSI lorsque vous N'utilisez pas les disques Fibre Channel virtuel.

## <a name="see-also"></a>Voir aussi

-   [Terminologie Hyper-V](terminology.md)

-   [Architecture Hyper-V](architecture.md)

-   [Configuration de serveur Hyper-V](configuration.md)

-   [Performances du processeur de Hyper-V](processor-performance.md)

-   [Performances de la mémoire Hyper-V](memory-performance.md)

-   [Réseau de Hyper-V les performances d’e/s](network-io-performance.md)

-   [Détecter les goulots d’étranglement dans un environnement virtualisé](detecting-virtualized-environment-bottlenecks.md)

-   [Machines virtuelles Linux](linux-virtual-machine-considerations.md)
