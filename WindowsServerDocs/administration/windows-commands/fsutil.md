---
ms.assetid: 2e748187-6a10-4bb0-aed5-34f886a250d2
title: Fsutil
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 08/21/2018
ms.openlocfilehash: 717c287995be2ab56bd49f2f24d46001f77e0e68
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843902"
---
# <a name="fsutil"></a>Fsutil

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Effectue des tâches liées aux systèmes de fichiers FAT (File Allocation Table) et NTFS, telles que la gestion des points d’analyse, la gestion des fichiers partiellement alloués ou le démontage d’un volume. S’il est utilisé sans paramètres, **fsutil** affiche la liste des sous-commandes prises en charge. 

> [!Note] 
> Vous devez avoir ouvert une session en tant qu’administrateur ou membre du groupe administrateurs pour utiliser fsutil. La commande fsutil est très puissante et doit être utilisée uniquement par des utilisateurs expérimentés qui ont une connaissance approfondie des systèmes d’exploitation Windows.
>
>Vous devez activer le sous-système Windows pour Linux avant de pouvoir exécuter **fsutil**. Exécutez la commande suivante en tant qu’administrateur dans PowerShell pour activer cette fonctionnalité facultative :
>
>```
> Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
>```
> Une fois l’installation terminée, vous êtes invité à redémarrer votre ordinateur. Une fois que votre ordinateur a redémarré, vous pouvez exécuter **fsutil** en tant qu’administrateur.

### <a name="parameters"></a>Paramètres

|Sous-commande |Description|
|---|---|
|[Fsutil 8dot3name](fsutil-8dot3name.md) | Les requêtes ou modifient les paramètres pour le comportement de nom abrégé sur le système, par exemple, génère des noms de fichiers de longueur 8,3 caractères. Supprime les noms courts de tous les fichiers d’un répertoire. Analyse un répertoire et identifie les clés de Registre qui peuvent être affectées si des noms courts ont été supprimés des fichiers du répertoire.|
|[Comportement de fsutil](fsutil-behavior.md) |Interroge ou définit le comportement du volume.|
|[Fsutil Dirty](fsutil-dirty.md)| Interroge si le bit d’intégrité du volume est défini ou définit le bit d’intégrité d’un volume. Lorsque le bit d’intégrité d’un volume est défini, **Autochk** vérifie automatiquement si le volume ne comporte pas d’erreurs lors du prochain redémarrage de l’ordinateur.|
|[Fichier fsutil](fsutil-file.md)|Recherche un fichier par nom d’utilisateur (si les quotas de disque sont activés), interroge des plages allouées pour un fichier, définit le nom abrégé d’un fichier, définit la longueur de données valide d’un fichier, ne définit aucune donnée pour un fichier, crée un fichier d’une taille spécifiée, recherche un ID de fichier si le nom est donné ou recherche un nom de lien de fichier|
|[Fsutil fsinfo](fsutil-fsinfo.md)|Répertorie tous les lecteurs et interroge le type de lecteur, les informations sur le volume, les informations sur les volumes spécifiques à NTFS ou les statistiques du système de fichiers.|
|[Fsutil hardlink](fsutil-hardlink.md)|Répertorie les liens physiques d’un fichier ou crée un lien physique (une entrée d’annuaire pour un fichier). Chaque fichier peut être considéré comme ayant au moins un lien physique. Sur les volumes NTFS, chaque fichier peut avoir plusieurs liens physiques, donc un seul fichier peut apparaître dans de nombreux répertoires (ou même dans le même répertoire, avec des noms différents). Étant donné que tous les liens font référence au même fichier, les programmes peuvent ouvrir n’importe quel lien et modifier le fichier. Un fichier est supprimé du système de fichiers uniquement après la suppression de tous les liens vers celui-ci. Une fois que vous avez créé un lien physique, les programmes peuvent l’utiliser comme n’importe quel autre nom de fichier.|
|[Fsutil objectid](fsutil-objectid.md)|Gère les identificateurs d’objets, qui sont utilisés par le système d’exploitation Windows pour effectuer le suivi des objets tels que les fichiers et les répertoires.|
|[Fsutil quota](fsutil-quota.md)|Gère les quotas de disque sur les volumes NTFS pour un contrôle plus précis du stockage basé sur le réseau. Les quotas de disque sont implémentés en fonction du volume et permettent d’implémenter des limites de stockage conditionnel et logiciel pour chaque utilisateur.|
|[Fsutil Repair](fsutil-repair.md)|Interroge ou définit l’état de réparation automatique du volume. Le système de fichiers NTFS à réparation automatique tente de corriger les altérations du système de fichiers NTFS en ligne sans nécessiter l’exécution de **chkdsk. exe** . Comprend l’initiation de la vérification sur disque et l’attente de la fin de la réparation.|
|[Fsutil reparsepoint](fsutil-reparsepoint.md)|Interroge ou supprime les points d’analyse (objets du système de fichiers NTFS ayant un attribut définissable contenant des données contrôlées par l’utilisateur). Les points d’analyse sont utilisés pour étendre les fonctionnalités dans le sous-système d’entrée/sortie (e/s). Ils sont utilisés pour les points de jonction d’annuaire et les points de montage de volume. Ils sont également utilisés par les pilotes de filtre de système de fichiers pour marquer certains fichiers comme étant spéciaux pour ce pilote.|
|[Fsutil resource](fsutil-resource.md)|Crée un Gestionnaire des ressources transactionnel secondaire, démarre ou arrête un Gestionnaire des ressources transactionnel, affiche des informations sur un Gestionnaire des ressources transactionnel ou modifie son comportement.|
|[Fsutil sparse](fsutil-sparse.md)|Gère les fichiers partiellement alloués. Un fichier partiellement alloué est un fichier contenant une ou plusieurs régions de données non allouées. Un programme voit ces régions non allouées comme contenant des octets ayant la valeur zéro, mais aucun espace disque n’est utilisé pour représenter ces zéros. Toutes les données significatives ou non nulles sont allouées, alors que toutes les données non significatives (grandes chaînes de données composées de zéros) ne sont pas allouées. Lorsqu’un fichier partiellement alloué est lu, les données allouées sont retournées en tant que données stockées et non allouées sont retournées comme des zéros (par défaut conformément à la spécification de la spécification de sécurité C2). La prise en charge des fichiers partiellement alloués permet de libérer les données de n’importe où dans le fichier.|
|[Hiérarchisation de fsutil](fsutil-tiering.md)|Active la gestion des fonctions de niveau de stockage, telles que la définition et la désactivation des indicateurs et la liste des niveaux.|
|[Fsutil transaction](fsutil-transaction.md)|Valide une transaction spécifiée, restaure une transaction spécifiée ou affiche des informations sur la transaction.|
|[Fsutil USN](fsutil-usn.md)|Gère le journal des modifications du numéro de séquence de mise à jour (USN), qui fournit un journal persistant de toutes les modifications apportées aux fichiers sur le volume.|
|[Fsutil volume](fsutil-volume.md)|Gère un volume. Démonte un volume, interroge pour voir la quantité d’espace libre disponible sur un disque ou trouve un fichier qui utilise un cluster spécifié.|
|[Fsutil Wim](fsutil-wim.md)|Fournit des fonctions permettant de détecter et de gérer des fichiers WIM.|

## <a name="see-also"></a>Voir aussi
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)