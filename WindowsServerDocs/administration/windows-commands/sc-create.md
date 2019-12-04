---
title: SC créer
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ef1480a7c1ed9fb0aa7e42077565526e5c9f7540
ms.sourcegitcommit: 4a03f263952c993dfdf339dd3491c73719854aba
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74791169"
---
# <a name="sc-create"></a>SC créer



Crée une sous-clé et des entrées pour un service dans le registre et dans la base de données du gestionnaire de contrôle des services.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
sc [<ServerName>] create [<ServiceName>] [type= {own | share | kernel | filesys | rec | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto }] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<ServerName >|Spécifie le nom du serveur distant sur lequel se trouve le service. Le nom doit utiliser le format UNC (Universal Naming Convention) (par exemple, \\\\MyServer). Pour exécuter SC. exe localement, omettez ce paramètre.|
|\<ServiceName >|Spécifie le nom du service retourné par l’opération **getkeyname** .|
|type = {Own \| partage \| kernel \| files d' \| Rec \| Interact type = {Own \| partage}}|Spécifie le type de service. Le paramètre par défaut est **type = Own**.</br>**Own** : spécifie que le service s’exécute dans son propre processus. Il ne partage pas de fichier exécutable avec d’autres services. Il s’agit du paramètre par défaut.</br>**partage** : spécifie que le service s’exécute en tant que processus partagé. Il partage un fichier exécutable avec d’autres services.</br>**kernel** : spécifie un pilote.</br>**fichiers** -spécifie un pilote de système de fichiers.</br>**Rec** : spécifie un pilote reconnu par le système de fichiers (identifie les systèmes de fichiers utilisés sur l’ordinateur).</br>**Interact** : spécifie que le service peut interagir avec le bureau, en recevant les entrées des utilisateurs. Les services interactifs doivent être exécutés sous le compte LocalSystem. Ce type doit être utilisé conjointement avec **type = Own** ou **type = Shared**. L’utilisation de **type = interactly** génère une erreur « paramètre non valide ».|
|Start = {Boot \| System \| \| de la demande automatique \| désactivé \| retardée-auto}|Spécifie le type de démarrage du service. Le paramètre par défaut est **Start = Demand**.</br>**démarrage** : spécifie un pilote de périphérique qui est chargé par le chargeur de démarrage.</br>**système** : spécifie un pilote de périphérique qui est démarré pendant l’initialisation du noyau.</br>**spécifie automatiquement un** service qui démarre automatiquement chaque fois que l’ordinateur est redémarré. Notez que le service s’exécute même s’il n’y a pas de connexion à l’ordinateur.</br>**Demand** : spécifie un service qui doit être démarré manuellement. Il s’agit de la valeur par défaut si **Start =** n’est pas spécifié.</br>**Disabled** : spécifie un service qui ne peut pas être démarré. Pour démarrer un service désactivé, remplacez le type de démarrage par une autre valeur.</br>**retardée-auto** -spécifie un service qui démarre automatiquement une brève heure après le démarrage d’autres services automatiques.|
|erreur = {normal \| critique \| critique \| ignorer}|Spécifie la gravité de l’erreur si le service échoue au démarrage de l’ordinateur. Le paramètre par défaut est **Error = normal**.</br>**normal** : spécifie que l’erreur est consignée. Un message s’affiche, informant l’utilisateur qu’un service n’a pas pu démarrer. Le démarrage se poursuit. Il s’agit du paramètre par défaut.</br>**grave** : spécifie que l’erreur est consignée (si possible). L’ordinateur tente de redémarrer avec la dernière bonne configuration connue. Cela peut entraîner le redémarrage de l’ordinateur, mais le service n’est peut-être toujours pas en mesure de s’exécuter.</br>**critique** : spécifie que l’erreur est consignée (si possible). L’ordinateur tente de redémarrer avec la dernière bonne configuration connue. Si la dernière bonne configuration connue échoue, le démarrage échoue également et le processus de démarrage s’arrête avec une erreur d’arrêt.</br>**ignore** : spécifie que l’erreur est journalisée et que le démarrage se poursuit. Aucune notification n’est donnée à l’utilisateur au-delà de l’enregistrement de l’erreur dans le journal des événements.|
|BinPath = \<BinaryPathName >|Spécifie un chemin d’accès au fichier binaire du service. Il n’y a pas de valeur par défaut pour **BinPath =** , et cette chaîne doit être fournie.|
|Groupe = \<LoadOrderGroup >|Spécifie le nom du groupe dont ce service est membre. La liste des groupes est stockée dans le registre, dans la sous-clé **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder** . La valeur par défaut est null.|
|tag = {Oui \| non}|Spécifie si un TagID doit être obtenu à partir de l’appel de CreateService. Les balises sont utilisées uniquement pour les pilotes de démarrage système et de démarrage.|
|depend = \<les dépendances >|Spécifie les noms des services ou des groupes qui doivent démarrer avant le démarrage de ce service. Les noms sont séparés par des barres obliques (/).|
|obj = {\<AccountName > \| \<ObjectName >}|Spécifie le nom d’un compte dans lequel un service s’exécute, ou spécifie un nom de l’objet pilote Windows dans lequel le pilote s’exécutera.|
|DisplayName = \<DisplayName >|Spécifie un nom convivial qui peut être utilisé par les programmes de l’interface utilisateur pour identifier le service.|
|Password = \<mot de passe >|Spécifie un mot de passe. Cela est obligatoire si un compte autre que LocalSystem est utilisé.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Pour chaque option de ligne de commande, le signe égal fait partie du nom de l’option.
-   Un espace est requis entre une option et sa valeur (par exemple, **type = Own**). Si l’espace est omis, l’opération échoue.

## <a name="BKMK_examples"></a>Illustre

Les exemples suivants illustrent la façon dont vous pouvez utiliser la commande **SC Create** :
```
sc \\myserver create NewService binpath= c:\windows\system32\NewServ.exe
sc create NewService binpath= c:\windows\system32\NewServ.exe type= share start= auto depend= "+TDI NetBIOS"
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
