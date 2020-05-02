---
title: Manage-bde sur
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f6a12814-df74-416c-a04a-62ea8512263e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55f9dc446b17b8e61655686b9f4b6259b12dafc9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724133"
---
# <a name="manage-bde-on"></a>Manage-bde : on



Chiffre le lecteur et active BitLocker.

## <a name="syntax"></a>Syntaxe

```
manage-bde –on <Drive> {[-recoveryPassword <NumericalPassword>]|[-recoverykey <PathToExternalDirectory>]|[-startupkey <PathToExternalKeyDirectory>]|[-certificate]|
[-tpmandpin]|[-tpmandpinandstartupkey <PathToExternalKeyDirectory>]|[-tpmandstartupkey <PathToExternalKeyDirectory>]|[-password]|[-ADAccountOrGroup <Domain\Account>]}
[-UsedSpaceOnly][-encryptionmethod {aes128_diffuser|aes256_diffuser|aes128|aes256}] [-skiphardwaretest] [-discoveryvolumetype <FileSystemType>] [-ForceEncryptionType <type>] [-RemoveVolumeShadowCopies][-computername <Name>] 
[{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Lecteur>|Représente une lettre de lecteur suivie par un signe deux-points.|
|-RecoveryPassword|Ajoute un protecteur de mot de passe numérique. Vous pouvez également utiliser **-RP** comme version abrégée de cette commande.|
|\<NumericalPassword>|Représente le mot de passe de récupération.|
|-recoverykey|Ajoute un protecteur de clé externe pour la récupération. Vous pouvez également utiliser **-RK** comme version abrégée de cette commande.|
|\<PathToExternalDirectory>|Représente le chemin d’accès du répertoire à la clé de récupération.|
|-clé|Ajoute un protecteur de clé externe pour le démarrage. Vous pouvez également utiliser **-SK** comme version abrégée de cette commande.|
|\<PathToExternalKeyDirectory>|Représente le chemin d’accès au répertoire de la clé de démarrage.|
|-certificat|Ajoute un protecteur de clé publique pour un lecteur de données. Vous pouvez également utiliser **-CERT** comme version abrégée de cette commande.|
|-tpmandpin|Ajoute une Module de plateforme sécurisée (TPM) (TPM) et un protecteur de code confidentiel (PIN) pour le lecteur du système d’exploitation. Vous pouvez également utiliser **-TP** comme version abrégée de cette commande.|
|-tpmandstartupkey|Ajoute un module de plateforme sécurisée et un protecteur de clé de démarrage pour le lecteur de système d’exploitation. Vous pouvez également utiliser **-tsk** comme version abrégée de cette commande.|
|-tpmandpinandstartupkey|Ajoute un protecteur de module de plateforme sécurisée, de code confidentiel et de clé de démarrage pour le lecteur de système d’exploitation. Vous pouvez également utiliser **-tpsk** comme version abrégée de cette commande.|
|-password|Ajoute un protecteur de clé de mot de passe pour le lecteur de données. Vous pouvez également utiliser **-PW** comme version abrégée de cette commande.|
|-ADAccountOrGroup|Ajoute un protecteur d’identité basé sur SID pour le volume. Le volume est automatiquement déverrouillé si l’utilisateur ou l’ordinateur possède les informations d’identification appropriées. Lorsque vous spécifiez un compte d’ordinateur **$** , ajoutez un au nom de l’ordinateur et spécifiez **– service** pour indiquer que le déverrouillage doit se produire dans le contenu du serveur BitLocker au lieu de l’utilisateur. Vous pouvez également utiliser **-sid** comme version abrégée de cette commande.|
|-UsedSpaceOnly|Définit le mode de chiffrement sur le chiffrement de l’espace utilisé uniquement. Les sections du volume contenant l’espace utilisé seront chiffrées, mais pas l’espace libre. Si cette option n’est pas spécifiée, tout l’espace utilisé et l’espace libre sur le volume seront chiffrés. Vous pouvez également utiliser **-utilisé** comme version abrégée de cette commande.|
|-encryptionMethod|Configure l’algorithme de chiffrement et la taille de clé. Vous pouvez également utiliser **-em** comme version abrégée de cette commande.|
|-skiphardwaretest|Commence le chiffrement sans test matériel. Vous pouvez également utiliser **-s** comme version abrégée de cette commande.|
|-discoveryvolumetype|Spécifie le système de fichiers à utiliser pour le lecteur de données de détection. Le lecteur de données de détection est un lecteur masqué ajouté à un lecteur de données amovibles, protégé par BitLocker et au format FAT, qui contient le Lecteur BitLocker To Go afin que les systèmes d’exploitation Windows Vista ou Windows XP puissent être utilisés pour afficher les lecteurs protégés par BitLocker.|
|-ForceEncryptionType|Force BitLocker à utiliser un chiffrement logiciel ou matériel. Vous pouvez spécifier le type de chiffrement **matériel** ou **logiciel** . Si le paramètre **matériel** est sélectionné, mais que le lecteur ne prend pas en charge le chiffrement matériel, Manage-bde retourne une erreur. Si stratégie de groupe paramètres interdit le type de chiffrement spécifié, Manage-bde retourne une erreur. Vous pouvez également utiliser **-FET** comme version abrégée de cette commande.|
|-RemoveVolumeShadowCopies|Forcez la deletikon des clichés instantanés de volume pour le volume. Après avoir exécuté cette commande, vous ne pourrez pas restaurer ce volume à l’aide de points de restauration système précédents. Vous pouvez également utiliser **-rvsc** comme version abrégée de cette commande.|
|\<FileSystemType>|Spécifie les systèmes de fichiers qui peuvent être utilisés avec les lecteurs de données de découverte : FAT32, valeur par défaut ou aucun.|
|-ComputerName|Spécifie que Manage-bde est utilisé pour modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **-CN** comme version abrégée de cette commande.|
|\<Name>|Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Les valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.|
|-? ou /?|Affiche une brève aide à l’invite de commandes.|
|-Help ou-h|Affiche l’aide complète à l’invite de commandes.|

## <a name="examples"></a>Exemples

Pour illustrer l’utilisation de la commande **-on** pour activer BitLocker pour le lecteur C et ajouter un mot de passe de récupération au lecteur.
```
manage-bde –on C: -recoverypassword
```
Pour illustrer l’utilisation de la commande **-on** pour activer BitLocker pour le lecteur C, ajoutez un mot de passe de récupération au lecteur, puis enregistrez une clé de récupération sur le lecteur E.
```
manage-bde –on C: -recoverykey E:\ -recoverypassword
```
Pour illustrer l’utilisation de la commande **-on** pour activer BitLocker pour le lecteur C à l’aide d’un protecteur de clé externe (tel qu’une clé USB) pour déverrouiller le lecteur de système d’exploitation. Cette méthode est requise si vous utilisez BitLocker avec des ordinateurs qui n’ont pas de module de plateforme sécurisée (TPM).
```
manage-bde -on C: -startupkey E:\
```
Pour illustrer l’utilisation de la commande **-on** pour activer BitLocker pour le lecteur de données E et ajouter un protecteur de clé de mot de passe. Manage-bde vous invite à entrer le mot de passe une fois cette commande entrée.
```
manage-bde –on E: -pw
```
Pour illustrer l’utilisation de la commande **-on** pour activer BitLocker pour le lecteur C du système d’exploitation et utiliser le chiffrement basé sur le matériel.
```
manage-bde –on C: -fet Hardware
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Gérer-bde](manage-bde.md)