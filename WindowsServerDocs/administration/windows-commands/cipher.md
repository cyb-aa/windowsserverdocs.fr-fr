---
title: cipher
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 78ef795e-0f87-4acd-8d15-192c972c0f41
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d801d6e6286e97319766c879f7289f6191cc7101
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434335"
---
# <a name="cipher"></a>cipher



Affiche ou modifie le chiffrement des répertoires et des fichiers sur les volumes NTFS. Si utilisée sans paramètres, **chiffrement** affiche l’état de chiffrement de l’annuaire actuel et de tous les fichiers qu’il contient.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
cipher [/e | /d | /c] [/s:<Directory>] [/b] [/h] [PathName [...]]
cipher /k
cipher /r:<FileName> [/smartcard]
cipher /u [/n]
cipher /w:<Directory>
cipher /x[:efsfile] [FileName]
cipher /y
cipher /adduser [/certhash:<Hash> | /certfile:<FileName>] [/s:Directory] [/b] [/h] [PathName [...]]
cipher /removeuser /certhash:<Hash> [/s:<Directory>] [/b] [/h] [<PathName> [...]]
cipher /rekey [PathName [...]]
```

## <a name="parameters"></a>Paramètres

|          Paramètres           |                                                                                                                                                   Description                                                                                                                                                    |
|-------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              /b               |                                                                                                    Abandonne si une erreur s’est produite. Par défaut, **chiffrement** continue à s’exécuter même si des erreurs sont rencontrées.                                                                                                    |
|              /c               |                                                                                                                                   Affiche des informations sur les fichiers chiffrés.                                                                                                                                    |
|              /d               |                                                                                                                                   Déchiffre les fichiers ou répertoires spécifiés.                                                                                                                                   |
|              /e               |                                                                                          Chiffre les fichiers ou répertoires spécifiés. Les répertoires sont marqués afin que les fichiers ajoutés par la suite seront chiffrées.                                                                                           |
|              /h               |                                                                                                     Affiche les fichiers cachés ou les attributs du système. Par défaut, ces fichiers ne sont pas chiffrées ou déchiffrées.                                                                                                     |
|              /k               |                                                                            Crée un nouveau certificat et une clé pour une utilisation avec les fichiers de système de fichiers EFS (Encrypting File System). Si le **/k** paramètre est spécifié, tous les autres paramètres sont ignorés.                                                                            |
|  / r:\<FileName > [/smartcard]  |   Génère une clé de l’agent de récupération EFS et le certificat, puis les écrit dans un fichier .pfx (contenant le certificat et la clé privée) et un fichier .cer (contenant uniquement le certificat). Si **/smartcard** est spécifié, il écrit la clé de récupération et le certificat à une carte à puce, et aucun fichier .pfx est généré.   |
|        / s:\<répertoire >        |                                                                                                               Effectue l’opération spécifiée sur tous les sous-répertoires dans le texte spécifié *Directory*.                                                                                                               |
|            /u [/n]            |  Recherche tous les fichiers chiffrés sur les ou les disques locaux. Si utilisé avec le **/n** paramètre, aucune mise à jour n’est apportées. Si utilisée sans **/n**, **/u** compare la clé de chiffrement de fichier de l’utilisateur ou de clé de l’agent de récupération en cours et les met à jour si elles ont été modifiés. Ce paramètre fonctionne uniquement avec **/n**.  |
|        / w:\<répertoire >        | Supprime l’espace disque inutilisé disponibles sur la totalité du volume de données. Si vous utilisez le **/w** paramètre, tous les autres paramètres sont ignorés. Le répertoire spécifié peut être situé n’importe où dans un volume local. S’il s’agit de montage point ou pointe vers un répertoire dans un autre volume, les données sur que le volume est supprimé. |
|  /x[:efsfile] [\<FileName>]   |                                 Permet de sauvegarder le certificat EFS et les clés pour le nom de fichier spécifié. Si utilisé avec **: efsfile**, **/x** sauvegarde des certificats de l’utilisateur qui ont été utilisés pour chiffrer le fichier. Sinon, l’utilisateur actuel certificat EFS et les clés sont sauvegardées.                                 |
|              /y               |                                                                                                                      Affiche votre miniature de certificat EFS actuelle sur l’ordinateur local.                                                                                                                      |
|  /adduser [/ certhash :\<hachage >  |                                                                                                                                              / certfile :<FileName>]                                                                                                                                               |
|            /rekey             |                                                                                                                 Met à jour les fichiers chiffrés spécifiés pour utiliser la clé EFS actuellement configurée.                                                                                                                 |
| /RemoveUser /certhash :\<hachage > |                                                                                       Supprime un utilisateur des ou les fichiers spécifiés. Le *hachage* fourni pour **/certhash** doit être le hachage SHA1 du certificat à supprimer.                                                                                       |
|              /?               |                                                                                                                                       Affiche l'aide à l'invite de commandes.                                                                                                                                       |

## <a name="remarks"></a>Notes

-   Si le répertoire parent n’est pas chiffré, un fichier crypté peut devenir décrypté lorsque celle-ci est modifiée. Par conséquent, lorsque vous chiffrez un fichier, vous devez également chiffrer le répertoire parent.
-   Un administrateur peut ajouter le contenu d’un fichier .cer à la stratégie de récupération EFS pour créer l’agent de récupération pour les utilisateurs et puis importer le fichier .pfx pour récupérer des fichiers individuels.
-   Vous pouvez utiliser plusieurs noms de répertoires et des caractères génériques.
-   Vous devez placer des espaces entre chaque paramètre.

## <a name="BKMK_examples"></a>Exemples

Pour afficher l’état de chiffrement de chacun des fichiers et sous-répertoires dans le répertoire actif, tapez :
```
cipher
```
Les fichiers chiffrés et les répertoires sont marqués avec un **E**. Fichiers non chiffrés et les répertoires sont marqués avec un **U**. Par exemple, la sortie suivante indique que le répertoire actif et tout son contenu est actuellement non chiffré :
```
Listing C:\Users\MainUser\Documents\
New files added to this directory will not be encrypted.
U Private
U hello.doc
U hello.txt
```
Pour activer le chiffrement sur le répertoire privé utilisé dans l’exemple précédent, tapez :
```
cipher /e private
```
La sortie suivante s’affiche :
```
Encrypting files in C:\Users\MainUser\Documents\
Private             [OK]
1 file(s) [or directorie(s)] within 1 directorie(s) were encrypted.
```
Le **chiffrement** commande affiche la sortie suivante :
```
Listing C:\Users\MainUser\Documents\
New files added to this directory will not be encrypted.
E Private
U hello.doc
U hello.txt
```
Notez que le répertoire privé est marqué comme chiffré.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)