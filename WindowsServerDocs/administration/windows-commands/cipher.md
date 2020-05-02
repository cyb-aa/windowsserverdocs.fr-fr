---
title: cipher
description: Rubrique de référence pour la commande Cipher, qui affiche ou modifie le chiffrement des répertoires et des fichiers sur les volumes NTFS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 78ef795e-0f87-4acd-8d15-192c972c0f41
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b07a0231b6cff6be9533136bf35ef013c854c2e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82713402"
---
# <a name="cipher"></a>cipher

Affiche ou modifie le chiffrement des répertoires et des fichiers sur les volumes NTFS. En cas d’utilisation sans paramètres, l’algorithme de **chiffrement** affiche l’état de chiffrement du répertoire actif et des fichiers qu’il contient.

## <a name="syntax"></a>Syntaxe

```
cipher [/e | /d | /c] [/s:<directory>] [/b] [/h] [pathname [...]]
cipher /k
cipher /r:<filename> [/smartcard]
cipher /u [/n]
cipher /w:<directory>
cipher /x[:efsfile] [filename]
cipher /y
cipher /adduser [/certhash:<hash> | /certfile:<filename>] [/s:directory] [/b] [/h] [pathname [...]]
cipher /removeuser /certhash:<hash> [/s:<directory>] [/b] [/h] [<pathname> [...]]
cipher /rekey [pathname [...]]
```

### <a name="parameters"></a>Paramètres

| Paramètres | Description |
| ---------- | ----------- |
| /b | Abandonne si une erreur est rencontrée. Par défaut, le **chiffrement** continue à s’exécuter même si des erreurs sont rencontrées. |
| /C | Affiche des informations sur le fichier chiffré. |
| /d | Déchiffre les fichiers ou répertoires spécifiés. |
| /e | Chiffre les fichiers ou répertoires spécifiés. Les répertoires sont marqués afin que les fichiers ajoutés par la suite soient chiffrés. |
| /h | Affiche les fichiers avec des attributs système ou masqués. Par défaut, ces fichiers ne sont pas chiffrés ou déchiffrés. |
| /k | Crée un certificat et une clé à utiliser avec les fichiers système de fichiers EFS (EFS). Si le paramètre **/k** est spécifié, tous les autres paramètres sont ignorés. |
| /r :`<filename>` [/Smartcard] | Génère une clé et un certificat d’agent de récupération EFS, puis les écrit dans un fichier. pfx (contenant le certificat et la clé privée) et un fichier. cer (contenant uniquement le certificat). Si **/Smartcard** est spécifié, il écrit la clé de récupération et le certificat sur une carte à puce, et aucun fichier. pfx n’est généré. |
| commutateur`<directory>` | Exécute l’opération spécifiée sur tous les sous-répertoires du *répertoire*spécifié. |
| /u [/n] |  Recherche tous les fichiers chiffrés sur le ou les lecteurs locaux. S’il est utilisé avec le paramètre **/n** , aucune mise à jour n’est effectuée. S’il est utilisé sans **/n**, **/u** compare la clé de chiffrement de fichier de l’utilisateur ou la clé de l’agent de récupération aux valeurs actuelles, et les met à jour si elles ont été modifiées. Ce paramètre fonctionne uniquement avec **/n**. |
| /w`<directory>` | Supprime les données de l’espace disque inutilisé disponible sur l’ensemble du volume. Si vous utilisez le paramètre **/w** , tous les autres paramètres sont ignorés. Le répertoire spécifié peut se trouver n’importe où dans un volume local. S’il s’agit d’un point de montage ou pointe vers un répertoire d’un autre volume, les données de ce volume sont supprimées. |
| /x [ : efsfile] [`<FileName>`] | Sauvegarde le certificat EFS et les clés dans le nom de fichier spécifié. S’il est utilisé avec **: efsfile**, **/x** sauvegarde le ou les certificats de l’utilisateur qui ont été utilisés pour chiffrer le fichier. Dans le cas contraire, le certificat EFS actuel de l’utilisateur et les clés sont sauvegardés. |
| /y | Affiche la miniature actuelle de votre certificat EFS sur l’ordinateur local. |
| /adduser [/certhash:`<hash>` | /CertFile :`<filename>`] |
| /rekey | Met à jour le ou les fichiers chiffrés spécifiés pour utiliser la clé EFS actuellement configurée. |
| /removeuser /certhash:`<hash>` | Supprime un utilisateur du ou des fichiers spécifiés. Le *hachage* fourni pour **/certhash** doit être le hachage SHA1 du certificat à supprimer. |
| /? | Affiche l'aide à l'invite de commandes. |

### <a name="remarks"></a>Notes 

- Si le répertoire parent n’est pas chiffré, un fichier chiffré peut devenir déchiffré lorsqu’il est modifié. Par conséquent, lorsque vous chiffrez un fichier, vous devez également chiffrer le répertoire parent.

- Un administrateur peut ajouter le contenu d’un fichier. cer à la stratégie de récupération EFS pour créer l’agent de récupération pour les utilisateurs, puis importer le fichier. pfx pour récupérer des fichiers individuels.

- Vous pouvez utiliser plusieurs noms de répertoires et caractères génériques.

- Vous devez placer des espaces entre plusieurs paramètres.

## <a name="examples"></a>Exemples

Pour afficher l’état de chiffrement de chacun des fichiers et sous-répertoires du répertoire actif, tapez :

```
cipher
```

Les fichiers et les répertoires chiffrés sont marqués d’un **E**. Les fichiers et les répertoires non chiffrés sont marqués d’un **U**. Par exemple, la sortie suivante indique que le répertoire actif et tout son contenu sont actuellement non chiffrés :

```
Listing C:\Users\MainUser\Documents\
New files added to this directory will not be encrypted.
U Private
U hello.doc
U hello.txt
```

Pour activer le chiffrement sur le Répertoire privé utilisé dans l’exemple précédent, tapez :

```
cipher /e private
```

La sortie suivante s’affiche :

```
Encrypting files in C:\Users\MainUser\Documents\
Private             [OK]
1 file(s) [or directorie(s)] within 1 directorie(s) were encrypted.
```

La commande **cipher** affiche la sortie suivante :

```
Listing C:\Users\MainUser\Documents\
New files added to this directory will not be encrypted.
E Private
U hello.doc
U hello.txt
```

Où le répertoire **privé** est désormais marqué comme étant chiffré.

### <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
