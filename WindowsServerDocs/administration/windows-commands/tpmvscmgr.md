---
title: tpmvscmgr
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8b2c8ff4-5c5d-446d-99e7-4daa1b36a163
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f6cfc15b7c089c9b6ad4d9a267b951373e63a63a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888590"
---
# <a name="tpmvscmgr"></a>tpmvscmgr



L’outil de ligne de commande Tpmvscmgr permet aux utilisateurs avec des informations d’identification d’administration créer et supprimer les cartes à puce virtuelles TPM sur un ordinateur. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
Tpmvscmgr create [/name] [/AdminKey DEFAULT | PROMPT | RANDOM] [/PIN DEFAULT | PROMPT] [/PUK DEFAULT | PROMPT] [/generate] [/machine] [/?]
```
```
Tpmvscmgr destroy [/instance <instance ID>] [/?]
```

### <a name="parameters-for-create-command"></a>Paramètres pour la commande Create

La commande Create configure de nouvelles cartes à puce virtuelles sur le système de l’utilisateur. Elle retourne l’ID d’instance de la carte nouvellement créée pour référence ultérieure si la suppression est nécessaire. L’instance ID est au format **ROOT\SMARTCARDREADER\000n** où **n** commence à 0 et est augmenté de 1 chaque fois que vous créez une nouvelle carte à puce virtuelle.

|Paramètre|Description|
|---------|-----------|
|/ nom|Obligatoire. Indique le nom de la nouvelle carte à puce virtuelle.|
|/ AdminKey|Indique la clé souhaitée d’administrateur qui peut être utilisée pour réinitialiser le code confidentiel de la carte, si l’utilisateur oublie le code PIN.</br>**Par défaut** spécifie la valeur par défaut 010203040506070801020304050607080102030405060708.</br>**INVITE** invite l’utilisateur à entrer une valeur pour la clé de l’administrateur.</br>**ALÉATOIRE** entraîne un paramètre aléatoire pour la clé de l’administrateur pour une carte qui n’est pas renvoyée à l’utilisateur. Cette opération crée une carte qui ne peut pas être gérée à l’aide des outils de gestion de carte à puce. Lors de la génération avec aléatoire, la clé de l’administrateur doit être entrée comme 48 caractères hexadécimaux.|
|/ CODE CONFIDENTIEL|Indique la valeur du code confidentiel utilisateur souhaité.</br>**Par défaut** spécifie la valeur par défaut du code confidentiel 12345678.</br>**INVITE** invite l’utilisateur à entrer un code PIN à la ligne de commande. Le code confidentiel doit être d’au moins huit caractères, et il peut contenir des chiffres, des caractères et des caractères spéciaux.|
|/PUK|Indique la valeur de code confidentiel déverrouiller clé déverrouillage souhaitée. La valeur de déverrouillage doit être d’au moins huit caractères, et il peut contenir des chiffres, des caractères et des caractères spéciaux. Si le paramètre est omis, la carte est créée sans un déverrouillage.</br>**Par défaut** spécifie la valeur par défaut déverrouillage 12345678.</br>**INVITE** vous invite à l’utilisateur à entrer un déverrouillage à la ligne de commande.|
|/ générer|Génère les fichiers dans le stockage qui sont nécessaires au fonctionnement de la carte à puce virtuelle. Si le / générer un paramètre est omis, il est équivalent à la création d’une carte sans ce système de fichiers. Une carte sans un système de fichiers peut être gérée uniquement par un système de gestion de carte à puce tels que Microsoft Configuration Manager.|
|/machine|Vous permet de spécifier le nom d’un ordinateur distant sur lequel la carte à puce virtuelle peuvent être créée. Cela peut être utilisé dans un environnement de domaine uniquement, et elle s’appuie sur DCOM. Pour la commande échoue lors de la création d’une carte à puce virtuelle sur un autre ordinateur, l’utilisateur qui exécute cette commande doit être un membre dans le groupe Administrateurs local sur l’ordinateur distant.|
|/?|Affiche l’aide de cette commande.|

### <a name="parameters-for-destroy-command"></a>Paramètres de commande de destruction

La commande Destroy supprime en toute sécurité une carte à puce virtuelle à partir de l’ordinateur de l’utilisateur.

> [!WARNING]
> Lorsqu’une carte à puce virtuelle est supprimée, il ne peut pas être récupéré.

|Paramètre|Description|
|---------|-----------|
|/instance|Spécifie l’ID d’instance de la carte à puce virtuelle à supprimer. L’ID d’instance a été généré en tant que sortie par Tpmvscmgr.exe lors de la création de la carte. Le paramètre/instance est un champ obligatoire pour la commande Destroy.|
|/?|Affiche l’aide de cette commande.|

## <a name="remarks"></a>Notes

L’appartenance à la **administrateurs** groupe (ou équivalent) sur l’ordinateur cible est la condition minimale requise pour exécuter tous les paramètres de cette commande.

Pour les entrées d’alphanumériques, le jeu de ASCII de caractères complète 127 est autorisé.

## <a name="BKMK_Examples"></a>Exemples

La commande suivante montre comment créer une carte à puce virtuelle qui peuvent être gérée ultérieurement par un outil de gestion de carte à puce lancé à partir d’un autre ordinateur.
```
tpmvscmgr.exe create /name "VirtualSmartCardForCorpAccess" /AdminKey DEFAULT /PIN PROMPT
```
Au lieu d’utiliser une clé d’administrateur par défaut, vous pouvez également créer une clé de l’administrateur en ligne de commande. La commande suivante montre comment créer une clé de l’administrateur.
```
tpmvscmgr.exe create /name "VirtualSmartCardForCorpAccess" /AdminKey PROMPT /PIN PROMPT
```
La commande suivante crée la carte à puce virtuelle non managée qui peut être utilisée pour inscrire des certificats.
```
tpmvscmgr.exe create /name "VirtualSmartCardForCorpAccess" /AdminKey RANDOM /PIN PROMPT /generate
```
La commande suivante crée une carte à puce virtuelle avec une clé de l’administrateur aléatoire. La clé est automatiquement supprimée après la cardis créé. Cela signifie que si l’utilisateur oublie le code PIN ou souhaite la modification du code confidentiel, l’utilisateur doit supprimer la carte et recréez-le. Pour supprimer la carte, l’utilisateur peut exécuter la commande suivante.
```
tpmvscmgr.exe destroy /instance <instance ID> 
```
où \<ID d’instance > est la valeur affichée sur l’écran lorsque l’utilisateur créé à la carte. Plus précisément, pour la première carte créée, l’ID d’instance est ROOT\SMARTCARDREADER\0000.

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)