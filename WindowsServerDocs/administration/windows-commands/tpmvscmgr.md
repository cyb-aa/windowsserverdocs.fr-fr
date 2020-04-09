---
title: tpmvscmgr
description: La rubrique commandes Windows pour tpmvscmgr, qui est un outil en ligne de commande qui permet aux utilisateurs disposant d’informations d’identification administratives de créer et de supprimer des cartes à puce virtuelles TPM sur un ordinateur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8b2c8ff4-5c5d-446d-99e7-4daa1b36a163
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4411e0ec3c75cd768b2fe32ad26b17331328e3ca
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832732"
---
# <a name="tpmvscmgr"></a>tpmvscmgr

L’outil en ligne de commande Tpmvscmgr permet aux utilisateurs disposant d’informations d’identification d’administration de créer et de supprimer des cartes à puce virtuelles TPM sur un ordinateur. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
Tpmvscmgr create [/name] [/AdminKey DEFAULT | PROMPT | RANDOM] [/PIN DEFAULT | PROMPT] [/PUK DEFAULT | PROMPT] [/generate] [/machine] [/?]
```
```
Tpmvscmgr destroy [/instance <instance ID>] [/?]
```

#### <a name="parameters-for-create-command"></a>Paramètres de la commande CREATE

La commande CREATE définit de nouvelles cartes à puce virtuelles sur le système de l’utilisateur. Elle retourne l’ID d’instance de la nouvelle carte créée pour référence ultérieure si la suppression est requise. L’ID d’instance est au format **ROOT\SMARTCARDREADER\000n** , où **n** commence à 0 et est augmenté de 1 chaque fois que vous créez une carte à puce virtuelle.

|Paramètre|Description|
|---------|-----------|
|/Name|Obligatoire. Indique le nom de la nouvelle carte à puce virtuelle.|
|/AdminKey|Indique la clé d’administrateur souhaitée qui peut être utilisée pour réinitialiser le code confidentiel de la carte si l’utilisateur oublie le code confidentiel.</br>**Par défaut** Spécifie la valeur par défaut de 010203040506070801020304050607080102030405060708.</br>**Invite** Invite l’utilisateur à entrer une valeur pour la clé d’administrateur.</br>**Aléatoire** Génère un paramètre aléatoire pour la clé d’administrateur pour une carte qui n’est pas renvoyée à l’utilisateur. Cela permet de créer une carte qui ne peut pas être gérée à l’aide des outils de gestion des cartes à puce. En cas de génération avec RANDOM, la clé d’administrateur doit être entrée sous la forme de caractères hexadécimaux 48.|
|/PIN|Indique la valeur du code confidentiel utilisateur souhaité.</br>**Par défaut** Spécifie l’axe par défaut 12345678.</br>**Invite** Invite l’utilisateur à entrer un code confidentiel sur la ligne de commande. Le code confidentiel doit comporter au minimum huit caractères, et il peut contenir des chiffres, des caractères et des caractères spéciaux.|
|/PUK|Indique la valeur de la clé de déverrouillage du code confidentiel (PUK) souhaitée. La valeur de PUK doit comporter au minimum huit caractères, et elle peut contenir des chiffres, des caractères et des caractères spéciaux. Si le paramètre est omis, la carte est créée sans PUK.</br>**Par défaut** Spécifie le PUK par défaut de 12345678.</br>**Invite** Demande à l’utilisateur d’entrer un PUK sur la ligne de commande.|
|/generate|Génère les fichiers dans le stockage qui sont nécessaires au fonctionnement de la carte à puce virtuelle. Si le paramètre/Generate est omis, cela revient à créer une carte sans ce système de fichiers. Une carte sans système de fichiers ne peut être gérée que par un système de gestion de carte à puce, tel que Microsoft Configuration Manager.|
|/machine|Vous permet de spécifier le nom d’un ordinateur distant sur lequel la carte à puce virtuelle peut être créée. Cela peut être utilisé dans un environnement de domaine uniquement et s’appuie sur DCOM. Pour que la commande aboutisse à la création d’une carte à puce virtuelle sur un autre ordinateur, l’utilisateur qui exécute cette commande doit être membre du groupe Administrateurs local sur l’ordinateur distant.|
|/?|Affiche l’aide de cette commande.|

#### <a name="parameters-for-destroy-command"></a>Paramètres de la commande destroy

La commande destroy supprime une carte à puce virtuelle de l’ordinateur de l’utilisateur en toute sécurité.

> [!WARNING]
> Lorsqu’une carte à puce virtuelle est supprimée, elle ne peut pas être récupérée.

|Paramètre|Description|
|---------|-----------|
|/instance|Spécifie l’ID d’instance de la carte à puce virtuelle à supprimer. L’instanceID a été généré comme sortie par Tpmvscmgr. exe lors de la création de la carte. Le paramètre/instance est un champ obligatoire pour la commande destroy.|
|/?|Affiche l’aide de cette commande.|

## <a name="remarks"></a>Notes

L’appartenance au groupe **administrateurs** (ou équivalent) sur l’ordinateur cible est la condition minimale requise pour exécuter tous les paramètres de cette commande.

Pour les entrées alphanumériques, le jeu ASCII complet de 127 caractères est autorisé.

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

La commande suivante montre comment créer une carte à puce virtuelle qui peut être gérée par la suite par un outil de gestion des cartes à puce lancé à partir d’un autre ordinateur.
```
tpmvscmgr.exe create /name VirtualSmartCardForCorpAccess /AdminKey DEFAULT /PIN PROMPT
```
Au lieu d’utiliser une clé d’administrateur par défaut, vous pouvez également créer une clé d’administrateur sur la ligne de commande. La commande suivante montre comment créer une clé d’administrateur.
```
tpmvscmgr.exe create /name VirtualSmartCardForCorpAccess /AdminKey PROMPT /PIN PROMPT
```
La commande suivante permet de créer la carte à puce virtuelle non gérée qui peut être utilisée pour inscrire des certificats.
```
tpmvscmgr.exe create /name VirtualSmartCardForCorpAccess /AdminKey RANDOM /PIN PROMPT /generate
```
La commande suivante crée une carte à puce virtuelle avec une clé d’administrateur aléatoire. La clé est automatiquement supprimée après la création de la carte. Cela signifie que si l’utilisateur oublie le PIN ou souhaite modifier le PIN, il doit supprimer la carte et la recréer. Pour supprimer la carte, l’utilisateur peut exécuter la commande suivante.
```
tpmvscmgr.exe destroy /instance <instance ID> 
```
où \<ID d’instance > correspond à la valeur imprimée à l’écran lorsque l’utilisateur a créé la carte. Plus précisément, pour la première carte créée, l’ID d’instance est ROOT\SMARTCARDREADER\0000.

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)