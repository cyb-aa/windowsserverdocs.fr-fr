---
ms.assetid: 2e748187-6a10-4bb0-aed5-34f886a250d2
title: fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 08/21/2018
ms.openlocfilehash: df8d25b01b67010734deb8dd7e42f3233e6011fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866810"
---
# <a name="fsutil"></a>fsutil

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Effectue des tâches qui sont liées dans le fichier FAT (allocation table) et les systèmes de fichiers NTFS, telles que la gestion d’analyse pointe, la gestion des fichiers partiellement alloués ou le démontage d’un volume. Si elle est utilisée sans paramètres, **Fsutil** affiche la liste des sous-commandes pris en charge. 

> [!Note] 
> Vous devez être connecté en tant qu’administrateur ou membre du groupe Administrateurs pour utiliser Fsutil. La commande Fsutil est un outil puissante et doit être utilisée uniquement par les utilisateurs expérimentés qui ont une connaissance approfondie des systèmes d’exploitation de Windows.
>
>Vous devez activer le sous-système de Windows pour Linux avant de pouvoir exécuter **Fsutil**. En tant qu’administrateur dans PowerShell pour activer cette fonctionnalité optionnelle, exécutez la commande suivante :
>
>```
> Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
>```
> Vous allez être invité à redémarrer votre ordinateur une fois qu’il est installé. Après le redémarrage de votre ordinateur, vous serez en mesure d’exécuter **Fsutil** en tant qu’administrateur.

## <a name="parameters"></a>Paramètres

|Sous-commande |Description|
|---|---|
|[fsutil 8dot3name](fsutil-8dot3name.md) | Interroge ou modifie les paramètres pour le comportement de nom court sur le système, par exemple, génère des noms de fichiers de longueur en caractères 8.3. Supprime les noms courts pour tous les fichiers contenus dans un répertoire. Analyse un répertoire et identifie les clés de Registre qui peuvent être impactés si les noms courts ont été supprimés à partir des fichiers dans le répertoire.|
|[comportement de fsutil](fsutil-behavior.md) |Les requêtes ou définit le comportement de volume.|
|[fsutil dirty](fsutil-dirty.md)| Interroge si le bit d’intégrité du volume est défini ou définit le bit d’intégrité d’un volume. Lorsqu’un volume's erroné bit est défini, **autochk** vérifie automatiquement le volume pour les erreurs lors du prochain redémarrage de l’ordinateur.|
|[fichier de fsutil](fsutil-file.md)|Recherche un fichier par nom d’utilisateur (si les Quotas de disque sont activés), interroge les plages allouées d’un fichier, définit un nom court, définit la longueur de données valide, données égales à zéro pour un fichier, crée un nouveau fichier d’une taille spécifiée, recherche un ID de fichier si le nom , ou s’il rencontre un nom de fichier de liaison pour un ID de fichier spécifié.|
|[fsutil fsinfo](fsutil-fsinfo.md)|Répertorie tous les lecteurs et interroge le type de lecteur, les informations de volume, des informations spécifiques de NTFS ou statistiques de système de fichiers.|
|[fsutil hardlink](fsutil-hardlink.md)|Répertorie les liens physiques pour un fichier, ou crée un lien physique (une entrée d’annuaire pour un fichier). Chaque fichier peut être considérée d’avoir au moins un lien physique. Sur les volumes NTFS, chaque fichier peut avoir plusieurs liens physiques, pour un seul fichier peut apparaître dans de nombreux répertoires (ou même dans le même répertoire, avec des noms différents). Étant donné que tous les liens de référencent le même fichier, les programmes peuvent ouvrir les liens et modifier le fichier. Un fichier est supprimé du système de fichiers uniquement une fois que tous les liens sont supprimés. Une fois que vous créez un lien physique, les programmes peuvent l’utiliser comme tout autre nom de fichier.|
|[fsutil objectid](fsutil-objectid.md)|Gère les identificateurs d’objets, qui sont utilisés par le système d’exploitation Windows pour effectuer le suivi des objets tels que les fichiers et répertoires.|
|[quota de fsutil](fsutil-quota.md)|Gère les quotas de disque sur les volumes NTFS pour fournir un contrôle plus précis du stockage basé sur le réseau. Quotas de disque sont implémentés sur par volume et activer les deux limites dur-soft-stockage et à mettre en oeuvre sur une base par utilisateur.|
|[réparation de fsutil](fsutil-repair.md)|Les requêtes ou définit l’état de réparation automatique du volume. Réparation automatique de NTFS pour tente de résoudre les altérations d’en ligne avec le système de fichiers NTFS sans nécessiter de **Chkdsk.exe** à exécuter. Inclut l’initialisation de vérification de sur le disque et en attente d’achèvement de la réparation.|
|[fsutil reparsepoint](fsutil-reparsepoint.md)|Les requêtes ou des suppressions d’analyse des points (objets NTFS qui possèdent un attribut définissable contenant des données contrôlées par l’utilisateur). D’analyse points sont utilisés pour étendre les fonctionnalités dans le sous-système d’entrée/sortie (e/s). Ils sont utilisés pour les points de jonction de répertoire et les points de montage de volume. Ils sont également utilisés par les pilotes de filtre de système de fichiers pour marquer certains fichiers propres à ce pilote.|
|[ressources de fsutil](fsutil-resource.md)|Crée un gestionnaire de ressources transactionnelles secondaire démarre ou arrête un gestionnaire de ressources transactionnelles, affiche des informations sur un gestionnaire de ressources transactionnelles ou modifie son comportement.|
|[fsutil sparse](fsutil-sparse.md)|Gère les fichiers partiellement alloués. Un fichier partiellement alloué est un fichier avec une ou plusieurs régions de données non allouées. Un programme verra ces régions non allouées comme contenant des octets avec la valeur zéro, mais aucun espace disque n’est utilisé pour représenter ces zéros non significatifs. Toutes les données significatives ou différente de zéro est alloué, tandis que toutes les données non significatives (longues chaînes de données composées de zéros) n’est pas allouée. Quand un fichier partiellement alloué est lue, données allouées sont retournées comme stockées et les données non allouées sont retournées sous forme de zéros (par défaut conformément à la spécification d’exigence de sécurité C2). Prise en charge des fichiers partiellement alloués permet aux données à désallouer dans n’importe où dans le fichier.|
|[la hiérarchisation fsutil](fsutil-tiering.md)|Permet la gestion des fonctions de niveau de stockage, comme la définition et la désactivation des indicateurs et répertorier des niveaux.|
|[transaction de fsutil](fsutil-transaction.md)|Valide une transaction spécifiée, annule une transaction spécifiée ou affiche des informations sur la transaction.|
|[fsutil usn](fsutil-usn.md)|Gère le journal des modifications nombre (USN) mise à jour de séquence, qui fournit un enregistrement cohérent de toutes les modifications apportées aux fichiers sur le volume.|
|[volume de fsutil](fsutil-volume.md)|Gère un volume. Démonte un volume, les requêtes pour voir la quantité d’espace libre est disponible sur un disque, ou trouve un fichier qui utilise un cluster spécifié.|
|[fsutil wim](fsutil-wim.md)|Fournit des fonctions pour détecter et gérer des fichiers WIM de secours.|

## <a name="see-also"></a>Voir aussi
[Clé de la syntaxe de ligne de commande](Command-Line-Syntax-Key.md)