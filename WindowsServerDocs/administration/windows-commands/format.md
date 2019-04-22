---
title: Format
ms.prod: windows-server-threshold
manager: dongill
ms.author: JGerend
ms.technology: storage
ms.topic: article
ms.assetid: 51ec7423-9a01-4219-868a-25d69cdcc832
author: JasonGerend
ms.date: 10/16/2017
ms.openlocfilehash: 10c0dd97ddd7384673573c1354105314af5d5e04
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818980"
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

|Paramètre|Description|
|---------|-----------|
|\<Volume>|Spécifie le point de montage, nom de volume ou lettre de lecteur (suivie d’un signe deux-points) du lecteur que vous souhaitez mettre en forme. Si vous ne spécifiez pas une des options de ligne de commande suivantes, **format** utilise le type de volume pour déterminer le format par défaut pour le disque.|
|/FS : {FAT|FAT32|NTFS|UDF}|Spécifie le type du système de fichiers : FAT, FAT32, NTFS ou UDF. Les disquettes peuvent uniquement utiliser le système de fichiers FAT.|
|/v:\<Label>|Spécifie le nom de volume. Si vous omettez le **/v** de ligne de commande option ou l’utiliser sans spécifier un nom de volume, **format** vous invitent à entrer le nom de volume après la mise en forme est terminée. Utilisez la syntaxe **/v:** pour éviter l’invite du nom de volume. Si vous utilisez une seule commande **format** pour formater plusieurs disques, tous les disques reçoivent le même nom de volume.|
|/ a:\<UnitSize >|Spécifie la taille d’unité d’allocation à utiliser sur les volumes FAT, FAT32 ou NTFS. Si vous ne spécifiez pas *UnitSize*, sa valeur est choisie en fonction de la taille du volume. Les paramètres par défaut sont fortement recommandés dans le cas général. La liste suivante présente les valeurs valides *UnitSize* NTFS, FAT et FAT32 :</br>512</br>1024</br>2 048</br>4 096</br>8 192</br>16 Ko</br>32 Ko</br>64 Ko</br>FAT et FAT32 prennent également en charge des tailles de 128 Ko et 256 Ko pour une taille de secteur supérieure à 512 octets.|
|/q|Effectue un formatage rapide. Supprime la table de fichiers et le répertoire racine d’un disque précédemment formaté, mais n’effectue pas une analyse secteur par secteur des zones défectueuses. Vous devez utiliser le **/q** une option de ligne de commande pour mettre en forme uniquement volumes précédemment formatés que vous savez en bon état. Notez que **/q** remplace **/p**.|
|/f :\<taille >|Spécifie la taille de la disquette à formater. Dans la mesure du possible, utilisez cette option de ligne de commande au lieu du **/t** et **/n** des options de ligne de commande. Windows accepte les valeurs de taille suivantes :</br>- 1440, 1440k ou 1440kb</br>- 1,44, 1,44m ou 1,44mb</br>-Disquette de 1,44 Mo recto-verso, quadruple densité 3,5 pouces,|
|/ t:\<pistes >|Spécifie le nombre de pistes sur le disque. Si possible, utilisez le **/f** de ligne de commande plutôt l’option. Si vous utilisez l’option **/t**, vous devez également utiliser l’option **/n**. Ces options fournissent ensemble une autre méthode de spécification de la taille du disque qui est formaté. Cette option n’est pas valide avec l’option **/f**.|
|/ n:\<secteurs >|Spécifie le nombre de secteurs par piste. Si possible, utilisez le **/f** une option de ligne de commande au lieu de **/n**. Si vous utilisez **/n**, vous devez également utiliser **/t**. Ces deux options fournissent ensemble une autre méthode de spécification de la taille du disque qui est formaté. Cette option n’est pas valide avec l’option **/f**.|
|/ p:\<passe >|Met à zéro chaque secteur du volume pendant le nombre de passes spécifié. Cette option n’est pas valide avec l’option **/q**.|
|/c|NTFS uniquement. Les fichiers créés sur le nouveau volume sont compressés par défaut.|
|/x|Entraîne le démontage du volume, si nécessaire, avant son formatage. Tous les descripteurs ouverts sur le volume ne sont alors plus valides.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Informations d’identification d’administration

    Vous devez être membre du groupe Administrateurs pour formater un disque dur.
-   À l’aide de **format**

    Le **format** commande crée une racine répertoires et les fichiers système pour le disque. Il peut également vérifier les zones défectueuses sur le disque, et il peut supprimer toutes les données sur le disque. Pour être en mesure d’utiliser un nouveau disque, vous devez tout d’abord utiliser cette commande pour formater le disque.
-   Ajout d’une étiquette de volume

    Après la mise en forme d’une disquette, **format** affiche le message suivant :

    `Volume label (11 characters, ENTER for none)?`

    Pour ajouter un nom de volume, tapez jusqu'à 11 caractères (y compris les espaces). Si vous ne souhaitez pas ajouter une étiquette de volume sur le disque, appuyez sur ENTRÉE.
-   Mise en forme d’un disque dur

> [!NOTE]
> Vous devez être membre du groupe Administrateurs pour formater un disque dur.

Lorsque vous utilisez le **format** commande pour formater un disque dur, un message d’avertissement similaire à ce qui suit affiche :
```
WARNING, ALL DATA ON NON-REMOVABLE DISK 
DRIVE x: WILL BE LOST! 
Proceed with Format (Y/N)? _ 
```
Pour formater le disque dur, appuyez sur O ; Si vous ne souhaitez pas formater le disque, appuyez sur N.
-   Taille d’unité

    Systèmes de fichiers FAT limitent le nombre de clusters à 65526 pas plus de. Les systèmes de fichiers FAT32 restreindre le nombre de clusters entre 65527 et 4177917.

    La compression NTFS n’est pas prise en charge pour les tailles de l’unité d’allocation supérieures à 4 096.

> [!NOTE]
> **Format** s’interrompt immédiatement si elle détermine que les conditions précédentes ne peuvent être satisfaites à l’aide de la taille du cluster spécifié.
-   **Format** messages

    Lors de la mise en forme est terminée, **format** affiche les messages qui indiquent l’espace disque total, les espaces signalés comme étant défectueux et l’espace disponible pour vos fichiers.
-   Formatage rapide

    Vous pouvez accélérer le processus de mise en forme à l’aide de la **/q** option de ligne de commande. Utilisez cette option uniquement s’il n’existe aucun secteur défectueux sur votre disque dur.
-   Utilisation de la commande **format** avec un lecteur réaffecté ou un lecteur réseau

    Vous ne devez pas utiliser la commande **format** sur un lecteur qui a été préparé à l’aide de la commande **subst**. Vous ne pouvez pas formater des disques sur un réseau.
-   Codes de sortie de la commande **format**

    Le tableau suivant répertorie chaque code de sortie et une brève description de sa signification.  
    |Code de sortie|Description|
    |---------|-----------|
    |0|L’opération de formatage a réussi.|
    |1|Des paramètres incorrects ont été fournis.|
    |4|Une erreur irrécupérable s’est produite (qui est une erreur autre que 0, 1 ou 5).|
    |5|L’utilisateur a appuyé sur N en réponse à l’invite « Continuer le formatage (O/N) ? » pour arrêter le processus.|

    Vous pouvez vérifier ces codes de sortie en utilisant la variable d’environnement ERRORLEVEL avec la commande batch **if**.
-   Utilisation de la commande **format** sur la console de récupération

    La commande **format**, avec des paramètres différents, est disponible à partir de la console de récupération.

## <a name="BKMK_examples"></a>Exemples

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

[Clé de la syntaxe de ligne de commande](https://technet.microsoft.com/library/cc771080.aspx)