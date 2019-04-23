---
title: Créer de SC
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 59416460-0661-4fef-85cc-73e9d8f4beb4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7931ddc91b91d5fce01335f4b090d0305790f65c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826500"
---
# <a name="sc-create"></a>Créer de SC



Crée une sous-clé et des entrées pour un service dans le Registre et dans la base de données du Gestionnaire de contrôle de Service.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
sc [<ServerName>] create [<ServiceName>] [type= {own | share | kernel | filesys | rec | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled}] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<ServerName>|Spécifie le nom du serveur distant sur lequel se trouve le service. Le nom doit utiliser le format UNC Universal Naming Convention () (par exemple, \\ \\myserver). Pour exécuter SC.exe localement, omettez ce paramètre.|
|\<ServiceName>|Spécifie le nom de service retourné par la **getkeyname** opération.|
|type = {propre \| partager \| noyau \| filesys \| rec \| interagir type = {propre \| partager}}|Spécifie le type de service. Le paramètre par défaut est **type = propre**.</br>**propre** -Spécifie que le service s’exécute dans son propre processus. Il ne partage pas un fichier exécutable avec d’autres services. Il s’agit du paramètre par défaut.</br>**partager** -Spécifie que le service s’exécute comme un processus partagé. Il partage un fichier exécutable avec d’autres services.</br>**noyau** -spécifie un pilote.</br>**filesys** -spécifie un pilote de système de fichiers.</br>**Rec** -spécifie un pilote de reconnu de système de fichiers (identifie les systèmes de fichiers utilisés sur l’ordinateur).</br>**interagir** -Spécifie que le service peut interagir avec le bureau, en recevant des données des utilisateurs. Services interactifs doivent être exécutés sous le compte LocalSystem. Ce type doit être utilisé conjointement avec **type = propre** ou **type = partagé**. À l’aide de **type = interagir** proprement dit génère une erreur « paramètre non valide ».|
|début = {démarrage \| système \| automatique \| à la demande \| désactivé}|Spécifie le type de démarrage pour le service. Le paramètre par défaut est **démarrer = à la demande**.</br>**démarrage** -spécifie un pilote de périphérique qui est chargé par le chargeur de démarrage.</br>**système** -spécifie un pilote de périphérique est démarré pendant l’initialisation du noyau.</br>**automatique** -spécifie un service qui démarre automatiquement chaque fois que l’ordinateur est redémarré. Notez que le service s’exécute même si personne n’ouvre une session sur l’ordinateur.</br>**à la demande** -spécifie un service qui doit être démarré manuellement. Ceci est la valeur par défaut si **Démarrer =** n’est pas spécifié.</br>**désactivé** -spécifie un service qui ne peut pas être démarré. Pour démarrer un service désactivé, modifiez le type de démarrage sur une autre valeur.|
|erreur = {normal \| graves \| critique \| ignorer}|Spécifie la gravité de l’erreur si le service échoue au démarrage de l’ordinateur. Le paramètre par défaut est **erreur = normal**.</br>**normal** -Spécifie que l’erreur est enregistrée. Une boîte de message s’affiche, informant l’utilisateur qu’un service n’a pas pu démarrer. Le démarrage se poursuit. Il s’agit du paramètre par défaut.</br>**graves** -Spécifie que l’erreur est enregistrée (si possible). L’ordinateur tente de redémarrer avec la dernière bonne configuration connue. Cela pourrait entraîner l’ordinateur est capable de redémarrer, mais le service peut toujours être impossible d’exécuter.</br>**critique** -Spécifie que l’erreur est enregistrée (si possible). L’ordinateur tente de redémarrer avec la dernière bonne configuration connue. Si la dernière bonne configuration connue échoue, démarrage échoue également et le processus de démarrage s’arrête et une erreur d’arrêt.</br>**Ignorer** -Spécifie que l’erreur est enregistrée et démarrage se poursuit. Aucune notification n’est donnée à l’utilisateur au-delà de l’enregistrement de l’erreur dans le journal des événements.|
|binpath= \<BinaryPathName>|Spécifie un chemin d’accès au fichier binaire du service. Il n’existe aucune valeur par défaut pour **chemin bin =**, et cette chaîne doit être fournie.|
|group= \<LoadOrderGroup>|Spécifie le nom du groupe dont ce service est un membre. La liste des groupes est stockée dans le Registre dans le **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder** sous-clé. La valeur par défaut est null.|
|balise = {Oui \| aucune}|Spécifie si une TagID doit être obtenu à partir de l’appel de CreateService. Les balises sont utilisées uniquement pour les pilotes de démarrage et de démarrage système.|
|depend= \<dependencies>|Spécifie les noms des services ou des groupes qui doivent démarrer avant le démarrage de ce service. Les noms sont séparés par des barres obliques (/).|
|obj= {\<AccountName> \| \<ObjectName>}|Spécifie le nom d’un compte dans lequel un service s’exécutera, ou spécifie un nom de l’objet de pilote Windows dans lequel le pilote s’exécute.|
|DisplayName = \<DisplayName >|Spécifie un nom convivial qui peut être utilisé par les programmes d’interface utilisateur pour identifier le service.|
|mot de passe = \<mot de passe >|Spécifie un mot de passe. Cela est nécessaire si vous utilisez un compte autre que LocalSystem.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Pour chaque option de ligne de commande, le signe égal fait partie du nom d’option.
-   Un espace est nécessaire entre une option et sa valeur (par exemple, **type = propre**. Si l’espace est omis l’opération échoue.

## <a name="BKMK_examples"></a>Exemples

Les exemples suivants montrent comment vous pouvez utiliser la **sc créer** commande :
```
sc \\myserver create NewService binpath= c:\windows\system32\NewServ.exe
sc create NewService binpath= c:\windows\system32\NewServ.exe type= share start= auto depend= "+TDI NetBIOS"
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
