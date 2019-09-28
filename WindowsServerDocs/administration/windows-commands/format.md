---
title: Format
ms.prod: windows-server
manager: dongill
ms.author: JGerend
ms.technology: storage
ms.topic: article
ms.assetid: 51ec7423-9a01-4219-868a-25d69cdcc832
author: JasonGerend
ms.date: 10/16/2017
ms.openlocfilehash: 7db57ac8115d99327fea72c12695a2ca6d3bc05f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377039"
---
# <a name="format"></a>Format
> S'applique à : Windows 10, Windows Server 2016

Met en forme un disque pour accepter des fichiers Windows.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
format <Volume> [/fs:{FAT|FAT32|NTFS}] [/v:<Label>] [/q] [/a:<UnitSize>] [/c] [/x] [/p:<Passes>]
format <Volume> [/v:<Label>] [/q] [/f:<Size>] [/p:<Passes>]
format <Volume> [/v:<Label>] [/q] [/t:<Tracks> /n:<Sectors>] [/p:<Passes>]
format <Volume> [/v:<Label>] [/q] [/p:<Passes>]
format <Volume> [/q]
```

## <a name="parameters"></a>Paramètres

|   Paramètre    |                                                                                                                                                                                                                    Description                                                                                                                                                                                                                     |
|----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   @no__t 0Volume >    |                                                                                         Spécifie le point de montage, le nom du volume ou la lettre de lecteur (suivi du signe deux-points) du lecteur que vous souhaitez mettre en forme. Si vous ne spécifiez aucune des options de ligne de commande suivantes, **format** utilise le type de volume pour déterminer le format par défaut du disque.                                                                                         |
|    /FS : {FAT    |                                                                                                                                                                                                                       FAT32                                                                                                                                                                                                                        |
|  /v : \<Label >   |                           Spécifie le nom de volume. Si vous omettez l’option de ligne de commande **/v** ou si vous l’utilisez sans spécifier un nom de volume, **format** vous invite à entrer le nom de volume une fois la mise en forme terminée. Utilisez la syntaxe **/v:** pour éviter l’invite du nom de volume. Si vous utilisez une seule commande **format** pour formater plusieurs disques, tous les disques reçoivent le même nom de volume.                            |
| /a : \<UnitSize > | Spécifie la taille d’unité d’allocation à utiliser sur les volumes FAT, FAT32 ou NTFS. Si vous ne spécifiez pas *UnitSize*, sa valeur est choisie en fonction de la taille du volume. Les paramètres par défaut sont fortement recommandés dans le cas général. La liste suivante présente les valeurs valides *UnitSize* NTFS, FAT et FAT32 :</br>512</br>1024</br>2 048</br>4 096</br>8 192</br>16 Ko</br>32 Ko</br>64 Ko</br>FAT et FAT32 prennent également en charge des tailles de 128 Ko et 256 Ko pour une taille de secteur supérieure à 512 octets. |
|       /q       |                                                       Effectue un formatage rapide. Supprime la table de fichiers et le répertoire racine d’un volume précédemment formaté, mais n’effectue pas de recherche secteur par secteur pour les zones défectueuses. Vous devez utiliser l’option de ligne de commande **/q** pour mettre en forme uniquement les volumes précédemment formatés dont vous savez qu’ils sont en bon état. Notez que **/q** remplace **/p**.                                                       |
|   /f : \<Size >   |                                                         Spécifie la taille de la disquette à formater. Dans la mesure du possible, utilisez cette option de ligne de commande à la place des options de ligne de commande **/t** et **/n** . Windows accepte les valeurs de taille suivantes :</br>- 1440, 1440k ou 1440kb</br>- 1,44, 1,44m ou 1,44mb</br>-1,44-Mo, double-face, quadruple densité, 3,5 pouces                                                         |
|  /t : \<Tracks >  |                                                    Spécifie le nombre de pistes sur le disque. Dans la mesure du possible, utilisez l’option de ligne de commande **/f** à la place. Si vous utilisez l’option **/t**, vous devez également utiliser l’option **/n**. Ces options fournissent ensemble une autre méthode de spécification de la taille du disque qui est formaté. Cette option n’est pas valide avec l’option **/f**.                                                     |
| /n : \<Sectors >  |                                                         Spécifie le nombre de secteurs par piste. Dans la mesure du possible, utilisez l’option de ligne de commande **/f** au lieu de **/n**. Si vous utilisez **/n**, vous devez également utiliser **/t**. Ces deux options fournissent ensemble une autre méthode de spécification de la taille du disque qui est formaté. Cette option n’est pas valide avec l’option **/f**.                                                         |
|  /p : \<Passes >  |                                                                                                                                                               Met à zéro chaque secteur du volume pendant le nombre de passes spécifié. Cette option n’est pas valide avec l’option **/q**.                                                                                                                                                                |
|       /c       |                                                                                                                                                                                     NTFS uniquement. Les fichiers créés sur le nouveau volume sont compressés par défaut.                                                                                                                                                                                      |
|       /x       |                                                                                                                                                            Entraîne le démontage du volume, si nécessaire, avant son formatage. Tous les descripteurs ouverts sur le volume ne sont alors plus valides.                                                                                                                                                            |
|       /?       |                                                                                                                                                                                                        Affiche l'aide à l'invite de commandes.                                                                                                                                                                                                        |

## <a name="remarks"></a>Notes

-   Informations d’identification d’administration

    Vous devez être membre du groupe administrateurs pour formater un disque dur.
-   Utiliser le **format**

    La commande **format** crée un répertoire racine et un nouveau système de fichiers pour le disque. Il peut également rechercher les zones défectueuses sur le disque et supprimer toutes les données sur le disque. Pour pouvoir utiliser un nouveau disque, vous devez d’abord utiliser cette commande pour formater le disque.
-   Ajout d’un nom de volume

    Après la mise en forme d’une disquette, le **format** affiche le message suivant :

    `Volume label (11 characters, ENTER for none)?`

    Pour ajouter un nom de volume, tapez jusqu’à 11 caractères (espaces compris). Si vous ne souhaitez pas ajouter un nom de volume au disque, appuyez sur entrée.
-   Formatage d’un disque dur

> [!NOTE]
> Vous devez être membre du groupe administrateurs pour formater un disque dur.

Lorsque vous utilisez la commande **format** pour formater un disque dur, un message d’avertissement semblable au suivant s’affiche :
```
WARNING, ALL DATA ON NON-REMOVABLE DISK 
DRIVE x: WILL BE LOST! 
Proceed with Format (Y/N)? _ 
```
Pour formater le disque dur, appuyez sur Y ; Si vous ne souhaitez pas formater le disque, appuyez sur N.
-   Taille d’unité

    Les systèmes de fichiers FAT limitent le nombre de clusters à 65526. Les systèmes de fichiers FAT32 limitent le nombre de clusters entre 65527 et 4177917.

    La compression NTFS n’est pas prise en charge pour les tailles de l’unité d’allocation supérieures à 4 096.

> [!NOTE]
> Le **format** arrêtera immédiatement le traitement s’il détermine que les exigences précédentes ne peuvent pas être satisfaites à l’aide de la taille de cluster spécifiée.
> -   **Mettre en forme** les messages

    When formatting is complete, **format** displays messages that show the total disk space, the spaces marked as defective, and the space available for your files.
- Formatage rapide

  Vous pouvez accélérer le processus de mise en forme à l’aide de l’option de ligne de commande **/q** . Utilisez cette option uniquement s’il n’existe aucun secteur défectueux sur votre disque dur.
- Utilisation de la commande **format** avec un lecteur réaffecté ou un lecteur réseau

  Vous ne devez pas utiliser la commande **format** sur un lecteur qui a été préparé à l’aide de la commande **subst**. Vous ne pouvez pas formater des disques sur un réseau.
- Codes de sortie de la commande **format**

  Le tableau suivant répertorie chaque code de sortie et une brève description de sa signification.  

  |Code de sortie|Description|
  |---------|-----------|
  |0|L’opération de formatage a réussi.|
  |1|Des paramètres incorrects ont été fournis.|
  |4|Une erreur irrécupérable s’est produite (erreur différente de 0, 1 ou 5).|
  |5|L’utilisateur a appuyé sur N en réponse à l’invite « continuer avec le format (o/N) ? » pour arrêter le processus.|

  Vous pouvez vérifier ces codes de sortie en utilisant la variable d’environnement ERRORLEVEL avec la commande batch **if**.
- Utilisation de la commande **format** sur la console de récupération

  La commande **format**, avec des paramètres différents, est disponible à partir de la console de récupération.

## <a name="BKMK_examples"></a>Illustre

Pour formater une nouvelle disquette dans le lecteur A avec la taille par défaut, tapez :
```
format a:
```
Pour effectuer une opération de formatage rapide sur une disquette précédemment formatée dans le lecteur A, tapez :
```
format a: /q
```
Pour formater une disquette dans le lecteur A et lui affecter le nom de volume « DATA », tapez :
```
format a: /v:DATA
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](https://technet.microsoft.com/library/cc771080.aspx)