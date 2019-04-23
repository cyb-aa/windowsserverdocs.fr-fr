---
title: Sc config
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ad4d68a6-efe5-452b-8501-7f1f1c552a4a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/05/2018
ms.openlocfilehash: 361a8407aae3b5e823b58cd71b97b043159146e6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837220"
---
# <a name="sc-config"></a>Sc config



Modifie la valeur des entrées d’un service dans le Registre et dans la base de données du Gestionnaire de contrôle de Service.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
sc [<ServerName>] config [<ServiceName>] [type= {own | share | kernel | filesys | rec | adapt | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto}] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<ServerName>|Spécifie le nom du serveur distant sur lequel se trouve le service. Le nom doit utiliser le format UNC Universal Naming Convention () (par exemple, \\ \\myserver). Pour exécuter SC.exe localement, omettez ce paramètre.|
|\<ServiceName>|Spécifie le nom de service retourné par la **getkeyname** opération.|
|type = {propre\| partager \| noyau \| filesys \| rec \| adapter \| interagir type = {propre \| partager}} | Spécifie le type de service.</br>**propre** -spécifie un service qui s’exécute dans son propre processus. Il ne partage pas un fichier exécutable avec d’autres services. Valeur par défaut.</br>**partager** -spécifie un service qui s’exécute comme un processus partagé. Il partage un fichier exécutable avec d’autres services.</br>**noyau** -spécifie un pilote.</br>**filesys** -spécifie un pilote de système de fichiers.</br>**Rec** -spécifie un fichier de pilote reconnu par le système qui identifie les systèmes de fichiers utilisés sur l’ordinateur.</br>**adapter** : Spécifie un pilote de carte qui identifie les périphériques matériels tels que les claviers, souris, et les lecteurs de disque.</br>**interagir** -spécifie un service qui peut interagir avec le bureau, en recevant des données des utilisateurs. Services interactifs doivent être exécutés sous le compte LocalSystem. Ce type doit être utilisé conjointement avec **type = propre** ou **type = partagé** (par exemple, **type = interagir** **type = propre**). À l’aide de **type = interagir** proprement dit génère une erreur.|
|début = {démarrage \| système \| automatique \| à la demande \| désactivé \| automatique différé}|Spécifie le type de démarrage pour le service.</br>**démarrage** -spécifie un pilote de périphérique qui est chargé par le chargeur de démarrage.</br>**système** -spécifie un pilote de périphérique est démarré pendant l’initialisation du noyau.</br>**Auto** : Spécifie un service qu’automatiquement démarre chaque fois que l’ordinateur est redémarré et s’exécute même si personne n’ouvre une session sur l’ordinateur.</br>**à la demande** -spécifie un service qui doit être démarré manuellement. Ceci est la valeur par défaut si **Démarrer =** n’est pas spécifié.</br>**désactivé** -spécifie un service qui ne peut pas être démarré. Pour démarrer un service désactivé, modifiez le type de démarrage sur une autre valeur.</br>**automatique différé** -spécifie un service qui démarre automatiquement une courte période après le démarrent des autres services automatique.|
|erreur = {normal \| graves \| critique \| ignorer}|Spécifie la gravité de l’erreur si le service ne parvient pas à démarrer au moment du démarrage.</br>**normal** -Spécifie que l’erreur est enregistrée et une boîte de message s’affiche, informant l’utilisateur qu’un service n’a pas pu démarrer. Le démarrage se poursuit. Il s’agit du paramètre par défaut.</br>**graves** -Spécifie que l’erreur est enregistrée (si possible). L’ordinateur tente de redémarrer avec la dernière bonne configuration connue. Cela pourrait entraîner l’ordinateur est capable de redémarrer, mais le service peut toujours être impossible d’exécuter.</br>**critique** -Spécifie que l’erreur est enregistrée (si possible). L’ordinateur tente de redémarrer avec la dernière bonne configuration connue. Si la dernière bonne configuration connue échoue, démarrage échoue également et le processus de démarrage s’arrête et une erreur d’arrêt.</br>**Ignorer** -Spécifie que l’erreur est enregistrée et démarrage se poursuit. Aucune notification n’est donnée à l’utilisateur au-delà de l’enregistrement de l’erreur dans le journal des événements.|
|binpath= \<BinaryPathName>|Spécifie un chemin d’accès au fichier binaire du service.|
|group= \<LoadOrderGroup>|Spécifie le nom du groupe dont ce service est un membre. La liste des groupes est stockée dans le Registre, dans le **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder** sous-clé. La valeur par défaut est null.|
|balise = {Oui \| aucune}|Spécifie s’il faut obtenir une TagID de l’appel de CreateService. Les balises sont utilisées uniquement pour les pilotes de démarrage et de démarrage système.|
|depend= \<dependencies>|Spécifie les noms des services ou des groupes qui doivent démarrer avant ce service. Les noms sont séparés par des barres obliques (/).|
|obj= {\<AccountName> \| \<ObjectName>}|Spécifie le nom d’un compte dans lequel un service s’exécutera, ou spécifie un nom de l’objet de pilote Windows dans lequel le pilote s’exécute. Le paramètre par défaut est **LocalSystem**.|
|DisplayName = \<DisplayName >|Spécifie un nom descriptif pour identifier le service dans les programmes d’interface utilisateur. Par exemple, le nom de la sous-clé d’un service particulier est **wuauserv**, qui a un nom d’affichage plus convivial de mises à jour automatiques.|
|mot de passe = \<mot de passe >|Spécifie un mot de passe. Cela est nécessaire si un compte autre que le compte LocalSystem est utilisé.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Pour chaque option de ligne de commande (paramètre), le signe égal fait partie du nom d’option.
-   Un espace est nécessaire entre une option et sa valeur (par exemple, **type = propre**. Si l’espace est omis, l’opération échoue.

## <a name="BKMK_examples"></a>Exemples

Pour spécifier un chemin d’accès binaire pour le service du nouveau service, tapez :
```
sc config NewService binpath= "ntsd -d c:\windows\system32\NewServ.exe"
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)