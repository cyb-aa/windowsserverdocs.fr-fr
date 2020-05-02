---
title: Configuration SC
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ad4d68a6-efe5-452b-8501-7f1f1c552a4a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/05/2018
ms.openlocfilehash: 45a94b3eea78552b61535542d85793bbaffd3df2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722216"
---
# <a name="sc-config"></a>Configuration SC



Modifie la valeur des entrées d’un service dans le registre et dans la base de données du gestionnaire de contrôle des services.



## <a name="syntax"></a>Syntaxe

```
sc [<ServerName>] config [<ServiceName>] [type= {own | share | kernel | filesys | rec | adapt | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto}] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Nom du serveur>|Spécifie le nom du serveur distant sur lequel se trouve le service. Le nom doit utiliser le format UNC (Universal Naming Convention) (par exemple, \\ \\monserveur). Pour exécuter SC. exe localement, omettez ce paramètre.|
|\<> ServiceName|Spécifie le nom du service retourné par l’opération **getkeyname** .|
|type = {\| Shared \| share \| kernel fichiers \| Rec \| adapter \| interaction type = {Own \| share}} | Spécifie le type de service.</br>**propre** -spécifie un service qui s’exécute dans son propre processus. Il ne partage pas de fichier exécutable avec d’autres services. Il s’agit de la valeur par défaut.</br>**partage** : spécifie un service qui s’exécute en tant que processus partagé. Il partage un fichier exécutable avec d’autres services.</br>**kernel** : spécifie un pilote.</br>**fichiers** -spécifie un pilote de système de fichiers.</br>**Rec** : spécifie un pilote reconnu par le système de fichiers qui identifie les systèmes de fichiers utilisés sur l’ordinateur.</br>**adapter** : spécifie un pilote de carte qui identifie les périphériques matériels tels que les claviers, les souris et les lecteurs de disque.</br>**interagir** : spécifie un service qui peut interagir avec le bureau, en recevant les entrées des utilisateurs. Les services interactifs doivent être exécutés sous le compte LocalSystem. Ce type doit être utilisé conjointement avec **type = Own** ou **type = Shared** (par exemple, **type = Interact** **type = Own**). L’utilisation de **type = interactly** génère une erreur.|
|Start = {la \| demande \| \| \| automatique du système \| de démarrage a été désactivée-auto}|Spécifie le type de démarrage du service.</br>**démarrage** : spécifie un pilote de périphérique qui est chargé par le chargeur de démarrage.</br>**système** : spécifie un pilote de périphérique qui est démarré pendant l’initialisation du noyau.</br>**auto** : spécifie un service qui démarre automatiquement chaque fois que l’ordinateur est redémarré et s’exécute même si personne ne se connecte à l’ordinateur.</br>**Demand** : spécifie un service qui doit être démarré manuellement. Il s’agit de la valeur par défaut si **Start =** n’est pas spécifié.</br>**Disabled** : spécifie un service qui ne peut pas être démarré. Pour démarrer un service désactivé, remplacez le type de démarrage par une autre valeur.</br>**retardée-auto** -spécifie un service qui démarre automatiquement une brève heure après le démarrage d’autres services automatiques.|
|erreur = { \| ignore \| critique \| critique critique}|Spécifie la gravité de l’erreur si le service ne démarre pas au moment du démarrage.</br>**normal** : spécifie que l’erreur est journalisée et qu’une boîte de message s’affiche, informant l’utilisateur qu’un service n’a pas pu démarrer. Le démarrage se poursuit. Il s'agit du paramètre par défaut.</br>**grave** : spécifie que l’erreur est consignée (si possible). L’ordinateur tente de redémarrer avec la dernière bonne configuration connue. Cela peut entraîner le redémarrage de l’ordinateur, mais le service n’est peut-être toujours pas en mesure de s’exécuter.</br>**critique** : spécifie que l’erreur est consignée (si possible). L’ordinateur tente de redémarrer avec la dernière bonne configuration connue. Si la dernière bonne configuration connue échoue, le démarrage échoue également et le processus de démarrage s’arrête avec une erreur d’arrêt.</br>**ignore** : spécifie que l’erreur est journalisée et que le démarrage se poursuit. Aucune notification n’est donnée à l’utilisateur au-delà de l’enregistrement de l’erreur dans le journal des événements.|
|BinPath = \<BinaryPathName>|Spécifie un chemin d’accès au fichier binaire du service.|
|groupe = \<LoadOrderGroup>|Spécifie le nom du groupe dont ce service est membre. La liste des groupes est stockée dans le registre, dans la sous-clé **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder** . La valeur par défaut est null.|
|tag = {oui \| non}|Spécifie s’il faut ou non obtenir un TagID à partir de l’appel de CreateService. Les balises sont utilisées uniquement pour les pilotes de démarrage système et de démarrage.|
|depend = \<dépendances>|Spécifie les noms des services ou des groupes qui doivent démarrer avant ce service. Les noms sont séparés par des barres obliques (/).|
|obj = {\<AccountName> \| \<ObjectName>}|Spécifie le nom d’un compte dans lequel un service s’exécute, ou spécifie un nom de l’objet pilote Windows dans lequel le pilote s’exécutera. La valeur par défaut est **LocalSystem**.|
|DisplayName = \<DisplayName>|Spécifie un nom descriptif pour identifier le service dans les programmes de l’interface utilisateur. Par exemple, le nom de la sous-clé d’un service particulier est **wuauserv**, avec un nom d’affichage plus convivial mises à jour automatiques.|
|password = \<mot de passe>|Spécifie un mot de passe. Cela est obligatoire si un compte autre que le compte LocalSystem est utilisé.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

-   Pour chaque option de ligne de commande (paramètre), le signe égal fait partie du nom de l’option.
-   Un espace est requis entre une option et sa valeur (par exemple, **type = Own**). Si l’espace est omis, l’opération échoue.

## <a name="examples"></a>Exemples

Pour spécifier un chemin d’accès binaire pour le service NEWSERVICE, tapez :
```
sc config NewService binpath= ntsd -d c:\windows\system32\NewServ.exe
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)