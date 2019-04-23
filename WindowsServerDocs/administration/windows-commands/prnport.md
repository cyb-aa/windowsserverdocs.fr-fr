---
title: prnport
description: Découvrez comment créer, supprimer et répertorier les ports d’imprimante.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a0ec638-a21e-4a34-be5c-bd0f7ca89ffe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eb6ef7632c10217c4fdf114d4e67c8bc52be07e2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856890"
---
# <a name="prnport"></a>prnport

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée, supprime et répertorie les ports d’imprimante TCP/IP standards, en plus d’afficher et de modifier la configuration du port.

## <a name="syntax"></a>Syntaxe
```
cscript prnport {-a | -d | -l | -g | -t | -?} [-r <PortName>] 
[-s <ServerName>] [-u <UserName>] [-w <Password>] [-o {raw | lpr}] 
[-h <Hostaddress>] [-q <QueueName>] [-n <PortNumber>] -m{e | d} 
[-i <SNMPIndex>] [-y <CommunityName>] -2{e | -d}
```

## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|-a|Crée un port d’imprimante TCP/IP standard.|
|-d|Supprime un port d’imprimante TCP/IP standard.|
|-l|Répertorie tous les ports d’imprimante TCP/IP standards sur l’ordinateur spécifié avec le **-s** paramètre.|
|-g|Affiche la configuration d’un port d’imprimante TCP/IP standard.|
|-t|Configure les paramètres de port pour un port d’imprimante TCP/IP standard.|
|-r \<PortName>|Spécifie le port auquel l’imprimante est connectée.|
|s - \<nom_serveur >|Spécifie le nom de l’ordinateur distant qui héberge l’imprimante que vous souhaitez gérer. Si vous ne spécifiez pas un ordinateur, l’ordinateur local est utilisé.|
|-u \<nom d’utilisateur > -w <Password>|Spécifie un compte disposant d’autorisations pour se connecter à l’ordinateur qui héberge l’imprimante que vous souhaitez gérer. Tous les membres du groupe Administrateurs local de l’ordinateur cible disposent de ces autorisations, mais les autorisations peuvent également être accordées aux autres utilisateurs. Si vous ne spécifiez pas un compte, vous devez être connecté sous un compte disposant de ces autorisations pour la commande fonctionne.|
|-o {raw &#124; lpr}|Spécifie quel protocole le port utilise : TCP brut ou TCP lpr. Si vous utilisez TCP raw, vous pouvez éventuellement spécifier le numéro de port à l’aide de la **- n** paramètre. Le numéro de port par défaut est 9100.|
|-h \<Hostaddress >|Spécifie l’imprimante pour laquelle vous souhaitez configurer le port (par adresse IP).|
|-q \<QueueName>|Spécifie le nom de la file d’attente pour un port TCP raw.|
|-n \<numéro_port >|Spécifie le numéro de port pour un port TCP raw. Le numéro de port par défaut est 9100.|
|-m{e &#124; d}|Spécifie si SNMP est activé. Le paramètre **e** Active SNMP. Le paramètre **d** désactive SNMP.|
|i - \<SNMPIndex|Spécifie l’index SNMP, si SNMP est activé. Pour plus d’informations, consultez la Rfc 1759 sur le [site de Web Rfc editor](https://go.microsoft.com/fwlink/?LinkId=569).|
|-y \<CommunityName>|Spécifie le nom de communauté SNMP, si SNMP est activé.|
|-2{e &#124; -d}|Spécifie si les spouleurs doubles (également appelé respooling) sont activés pour les ports TCP lpr. Spouleurs doubles sont nécessaires, car TCP lpr doit inclure un nombre d’octets précis dans le fichier de contrôle qui est envoyé à l’imprimante, mais le protocole ne peut pas obtenir le nombre à partir du fournisseur d’impression local. Par conséquent, lorsqu’un fichier est mis en attente pour TCP lpr imprimer la file d’attente, il est également mis en attente dans un fichier temporaire dans le répertoire system32. TCP lpr détermine la taille du fichier temporaire et envoie la taille sur le serveur exécutant le protocole LPD. Le paramètre **e** Active les spouleurs doubles. Le paramètre **d** désactive les spouleurs doubles.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
-   Le **prnport** commande est un script Visual Basic situé dans le %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Pour utiliser cette commande, à une invite de commandes, tapez **cscript** suivie du chemin d’accès complet au fichier de prnport, ou modifiez les répertoires vers le dossier approprié. Exemple :
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnport
    ```
-   Si les informations que vous fournissez contient des espaces, utilisez des guillemets autour du texte (par exemple, `"computer Name"`).
-   Le protocole brutes TCP est un protocole de performances plus élevé sur Windows que le protocole lpr.

## <a name="BKMK_examples"></a>Exemples
Pour afficher tous les ports d’impression TCP/IP standards sur le serveur \\\Server1, tapez :
```
cscript prnport -l -s Server1
```
Pour supprimer le port d’impression TCP/IP standard sur le serveur \\\Server1 qui se connecte à une imprimante réseau à 10.2.3.4, type :
```
cscript prnport -d -s Server1 -r IP_10.2.3.4
```
Pour ajouter un port d’impression TCP/IP standard sur le serveur \\\Server1 qui se connecte à une imprimante réseau à 10.2.3.4 et utilise le protocole brutes TCP sur le port 9100, type :
```
cscript prnport -a -s Server1 -r IP_10.2.3.4 -h 10.2.3.4 -o raw -n 9100
```
Pour activer le protocole SNMP, spécifiez le nom de communauté « public » et le SNMP index défini sur 1 sur une imprimante réseau à 10.2.3.4 partagés par le serveur \\\Server1, tapez :
```
cscript prnport -t -s Server1 -r IP_10.2.3.4 -me -y public -i 1 -n 9100
```
Pour ajouter un port d’impression TCP/IP standard sur l’ordinateur local qui se connecte à une imprimante réseau à 10.2.3.4 et obtenez automatiquement les paramètres de périphérique à partir de l’imprimante, tapez :
```
cscript prnport -a -r IP_10.2.3.4 -h 10.2.3.4
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[référence des commandes d’impression](print-command-reference.md)
