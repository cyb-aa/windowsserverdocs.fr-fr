---
title: gérer-bde sur
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6a12814-df74-416c-a04a-62ea8512263e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b50cad64025e85824a8f0a27d773ffb614491fe5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841180"
---
# <a name="manage-bde-on"></a>gérer-bde : sur



Chiffre le lecteur et active BitLocker. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
manage-bde –on <Drive> {[-recoveryPassword <NumericalPassword>]|[-recoverykey <PathToExternalDirectory>]|[-startupkey <PathToExternalKeyDirectory>]|[-certificate]|
[-tpmandpin]|[-tpmandpinandstartupkey <PathToExternalKeyDirectory>]|[-tpmandstartupkey <PathToExternalKeyDirectory>]|[-password]|[-ADAccountOrGroup <Domain\Account>]}
[-UsedSpaceOnly][-encryptionmethod {aes128_diffuser|aes256_diffuser|aes128|aes256}] [-skiphardwaretest] [-discoveryvolumetype <FileSystemType>] [-ForceEncryptionType <type>] [-RemoveVolumeShadowCopies][-computername <Name>] 
[{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Drive>|Représente une lettre de lecteur suivie par un signe deux-points.|
|-recoverypassword|Ajoute un protecteur de mot de passe numérique. Vous pouvez également utiliser **- rp** comme une version abrégée de cette commande.|
|\<NumericalPassword>|Représente le mot de passe de récupération.|
|-recoverykey|Ajoute un protecteur de clé externe pour la récupération. Vous pouvez également utiliser **- rk** comme une version abrégée de cette commande.|
|\<PathToExternalDirectory>|Représente le chemin d’accès à la clé de récupération.|
|-startupkey|Ajoute un protecteur de clé externe pour le démarrage. Vous pouvez également utiliser **-sk** comme une version abrégée de cette commande.|
|\<PathToExternalKeyDirectory>|Représente le chemin d’accès à la clé de démarrage.|
|-certificat|Ajoute un protecteur de clé publique pour un lecteur de données. Vous pouvez également utiliser **-cert** comme une version abrégée de cette commande.|
|-tpmandpin|Ajoute un Module de plateforme sécurisée (TPM) et un protecteur de (PIN) numéro d’identification personnel pour le lecteur de système d’exploitation. Vous pouvez également utiliser **- tp** comme une version abrégée de cette commande.|
|-tpmandstartupkey|Ajoute un protecteur de clé TPM et de démarrage pour le lecteur de système d’exploitation. Vous pouvez également utiliser **-tâche** comme une version abrégée de cette commande.|
|-tpmandpinandstartupkey|Ajoute un module de plateforme sécurisée, un code confidentiel et un protecteur de clé de démarrage pour le lecteur de système d’exploitation. Vous pouvez également utiliser **- tpsk** comme une version abrégée de cette commande.|
|-password|Ajoute un protecteur de clé de mot de passe pour le lecteur de données. Vous pouvez également utiliser **- pw** comme une version abrégée de cette commande.|
|-ADAccountOrGroup|Ajoute un protecteur d’identité basée sur les SID pour le volume. Le volume déverrouillera automatiquement si l’utilisateur ou l’ordinateur possède les informations d’identification appropriées. Lorsque vous spécifiez un compte d’ordinateur, ajoutez un **$** à l’ordinateur nommer et spécifier **– service** pour indiquer que le déverrouillage doit se produire dans le contenu du serveur BitLocker à la place de la utilisateur. Vous pouvez également utiliser **-sid** comme une version abrégée de cette commande.|
|-UsedSpaceOnly|Définit le mode de chiffrement pour que l’espace utilisé de chiffrement. Les sections du volume contenant l’espace utilisé seront chiffrées, mais l’espace libre n’est pas. Si cette option n’est pas spécifiée, tout espace utilisé et espace libre sur le volume est chiffrée... Vous pouvez également utiliser **-utilisé** comme une version abrégée de cette commande.|
|-encryptionMethod|Configure la taille de clé et l’algorithme de chiffrement. Vous pouvez également utiliser **-em** comme une version abrégée de cette commande.|
|-skiphardwaretest|Commence le chiffrement sans un test du matériel. Vous pouvez également utiliser **-s** comme une version abrégée de cette commande.|
|-discoveryvolumetype|Spécifie le système de fichiers à utiliser pour le lecteur de données de découverte. Le lecteur de données de découverte est un lecteur masqué ajouté à un lecteur de données amovibles au format FAT, protégé par BitLocker qui contient le lecteur BitLocker To Go afin que les systèmes d’exploitation Windows Vista ou Windows XP peut être utilisés pour afficher les lecteurs protégés par BitLocker.|
|-ForceEncryptionType|BitLocker impose d’utiliser le chiffrement logiciel ou matériel. Vous pouvez spécifier **matériel** ou **logiciel** en tant que le type de chiffrement. Si le **matériel** paramètre est sélectionné, mais le lecteur ne prend pas en charge le chiffrement au niveau matériel, gérer-bde retourne une erreur. Si les paramètres de stratégie de groupe interdit le type de chiffrement spécifié, le bde gérer retourne une erreur. Vous pouvez également utiliser **l’effet de champ -** comme une version abrégée de cette commande.|
|-RemoveVolumeShadowCopies|Forcer deletikon de clichés instantanés de Volume pour le volume. Vous ne serez pas en mesure de restaurer ce volume à l’aide de points de restauration système après avoir exécuté cette commande. Vous pouvez également utiliser **- rvsc** comme une version abrégée de cette commande.|
|\<FileSystemType>|Spécifie les systèmes de fichiers peuvent être utilisés avec les lecteurs de données de découverte : FAT32, par défaut ou none.|
|-computername|Spécifie que Manage-bde est utilisée pour modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **- cn** comme une version abrégée de cette commande.|
|\<Name>|Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.|
|-? ou /?|Affiche un résumé aide à l’invite de commandes.|
|-help ou-h|Affiche une aide complète à l’invite de commandes.|

## <a name="BKMK_Examples"></a>Exemples

L’exemple suivant illustre l’utilisation de la **-sur** commande pour activer BitLocker pour le lecteur C et ajouter un mot de passe de récupération sur le lecteur.
```
manage-bde –on C: -recoverypassword
```
L’exemple suivant illustre l’utilisation de la **-sur** commande pour activer BitLocker pour le lecteur C, ajoutez un mot de passe de récupération sur le disque et enregistrer une clé de récupération sur le lecteur E.
```
manage-bde –on C: -recoverykey E:\ -recoverypassword
```
L’exemple suivant illustre l’utilisation de la **-sur** commande pour activer BitLocker pour le lecteur C à l’aide d’un protecteur de clé externe (par exemple, une clé USB) pour déverrouiller le lecteur du système d’exploitation. Cette méthode est requise si vous utilisez BitLocker avec les ordinateurs qui n’ont pas un module de plateforme sécurisée.
```
manage-bde -on C: -startupkey E:\
```
L’exemple suivant illustre l’utilisation de la **-sur** commande pour activer BitLocker pour le lecteur de données E et ajouter un protecteur de clé de mot de passe. Gérer-bde vous invitera à entrer le mot de passe une fois que cette commande a été entrée.
```
manage-bde –on E: -pw
```
L’exemple suivant illustre l’utilisation de la **-sur** commande pour activer BitLocker pour le lecteur de système d’exploitation C et utiliser le chiffrement basé sur le matériel.
```
manage-bde –on C: -fet Hardware
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)