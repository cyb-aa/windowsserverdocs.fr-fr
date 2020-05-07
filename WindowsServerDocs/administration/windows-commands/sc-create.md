---
title: SC. exe créer
description: Découvrez comment inscrire de nouveaux services avec Windows Service Manager à l’aide de l’utilitaire SC. exe
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 59416460-0661-4fef-85cc-73e9d8f4beb4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9e1a581273def291502bf01e3fc9acf0c296707b
ms.sourcegitcommit: 95b60384b0b070263465eaffb27b8e3bb052a4de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82850100"
---
# <a name="scexe-create"></a>SC. exe créer

Crée une sous-clé et des entrées pour un service dans le registre et dans la base de données du gestionnaire de contrôle des services.

## <a name="syntax"></a>Syntaxe

```
sc.exe [<ServerName>] create [<ServiceName>] [type= {own | share | kernel | filesys | rec | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto }] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Nom du serveur>|Spécifie le nom du serveur distant sur lequel se trouve le service. Le nom doit utiliser le format UNC (Universal Naming Convention) (par exemple, \\ \\monserveur). Pour exécuter SC. exe localement, omettez ce paramètre.|
|\<> ServiceName|Spécifie le nom du service retourné par l’opération **getkeyname** .|
|type = \| {Shared \| share \| kernel Files \| - \| REC REC type = { \| Own share}}|Spécifie le type de service. Le paramètre par défaut est **type = Own**.</br>**Own** : spécifie que le service s’exécute dans son propre processus. Il ne partage pas de fichier exécutable avec d’autres services. Il s'agit du paramètre par défaut.</br>**partage** : spécifie que le service s’exécute en tant que processus partagé. Il partage un fichier exécutable avec d’autres services.</br>**kernel** : spécifie un pilote.</br>**fichiers** -spécifie un pilote de système de fichiers.</br>**Rec** : spécifie un pilote reconnu par le système de fichiers (identifie les systèmes de fichiers utilisés sur l’ordinateur).</br>**Interact** : spécifie que le service peut interagir avec le bureau, en recevant les entrées des utilisateurs. Les services interactifs doivent être exécutés sous le compte LocalSystem. Ce type doit être utilisé conjointement avec **type = Own** ou **type = Shared**. L’utilisation de **type = interactly** génère une erreur de paramètre non valide.|
|Start = {la \| demande \| \| \| automatique du système \| de démarrage a été désactivée-auto}|Spécifie le type de démarrage du service. Le paramètre par défaut est **Start = Demand**.</br>**démarrage** : spécifie un pilote de périphérique qui est chargé par le chargeur de démarrage.</br>**système** : spécifie un pilote de périphérique qui est démarré pendant l’initialisation du noyau.</br>**spécifie automatiquement un** service qui démarre automatiquement chaque fois que l’ordinateur est redémarré. Notez que le service s’exécute même s’il n’y a pas de connexion à l’ordinateur.</br>**Demand** : spécifie un service qui doit être démarré manuellement. Il s’agit de la valeur par défaut si **Start =** n’est pas spécifié.</br>**Disabled** : spécifie un service qui ne peut pas être démarré. Pour démarrer un service désactivé, remplacez le type de démarrage par une autre valeur.</br>**retardée-auto** -spécifie un service qui démarre automatiquement une brève heure après le démarrage d’autres services automatiques.|
|erreur = { \| ignore \| critique \| critique critique}|Spécifie la gravité de l’erreur si le service échoue au démarrage de l’ordinateur. Le paramètre par défaut est **Error = normal**.</br>**normal** : spécifie que l’erreur est consignée. Un message s’affiche, informant l’utilisateur qu’un service n’a pas pu démarrer. Le démarrage se poursuit. Il s'agit du paramètre par défaut.</br>**grave** : spécifie que l’erreur est consignée (si possible). L’ordinateur tente de redémarrer avec la dernière bonne configuration connue. Cela peut entraîner le redémarrage de l’ordinateur, mais le service n’est peut-être toujours pas en mesure de s’exécuter.</br>**critique** : spécifie que l’erreur est consignée (si possible). L’ordinateur tente de redémarrer avec la dernière bonne configuration connue. Si la dernière bonne configuration connue échoue, le démarrage échoue également et le processus de démarrage s’arrête avec une erreur d’arrêt.</br>**ignore** : spécifie que l’erreur est journalisée et que le démarrage se poursuit. Aucune notification n’est donnée à l’utilisateur au-delà de l’enregistrement de l’erreur dans le journal des événements.|
|BinPath = \<BinaryPathName>|Spécifie un chemin d’accès au fichier binaire du service. Il n’y a pas de valeur par défaut pour **BinPath =**, et cette chaîne doit être fournie.|
|groupe = \<LoadOrderGroup>|Spécifie le nom du groupe dont ce service est membre. La liste des groupes est stockée dans le registre, dans la sous-clé **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder** . La valeur par défaut est null.|
|tag = {oui \| non}|Spécifie si un TagID doit être obtenu à partir de l’appel de CreateService. Les balises sont utilisées uniquement pour les pilotes de démarrage système et de démarrage.|
|depend = \<dépendances>|Spécifie les noms des services ou des groupes qui doivent démarrer avant le démarrage de ce service. Les noms sont séparés par des barres obliques (/).|
|obj = {\<AccountName> \| \<ObjectName>}|Spécifie le nom d’un compte dans lequel un service s’exécute, ou spécifie un nom de l’objet pilote Windows dans lequel le pilote s’exécutera.|
|DisplayName = \<DisplayName>|Spécifie un nom convivial qui peut être utilisé par les programmes de l’interface utilisateur pour identifier le service.|
|password = \<mot de passe>|Spécifie un mot de passe. Cela est obligatoire si un compte autre que LocalSystem est utilisé.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

-   Pour chaque option de ligne de commande, le signe égal fait partie du nom de l’option.
-   Un espace est requis entre une option et sa valeur (par exemple, **type = Own**). Si l’espace est omis, l’opération échoue.

## <a name="examples"></a>Exemples

Les exemples suivants illustrent la façon dont vous pouvez utiliser la commande **SC. exe Create** :
```
sc.exe \\myserver create NewService binpath= c:\windows\system32\NewServ.exe
sc.exe create NewService binpath= c:\windows\system32\NewServ.exe type= share start= auto depend= +TDI NetBIOS
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
