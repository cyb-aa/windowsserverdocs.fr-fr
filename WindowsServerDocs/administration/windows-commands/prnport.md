---
title: prnport
description: Découvrez comment créer, supprimer et répertorier les ports d’imprimante.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6a0ec638-a21e-4a34-be5c-bd0f7ca89ffe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c68b703fe7feef40e765af2a7863846401398107
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722809"
---
# <a name="prnport"></a>prnport

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée, supprime et répertorie les ports d’imprimante TCP/IP standard, en plus de l’affichage et de la modification de la configuration du port.

## <a name="syntax"></a>Syntaxe
```
cscript prnport {-a | -d | -l | -g | -t | -?} [-r <PortName>] 
[-s <ServerName>] [-u <UserName>] [-w <Password>] [-o {raw | lpr}] 
[-h <Hostaddress>] [-q <QueueName>] [-n <PortNumber>] -m{e | d} 
[-i <SNMPIndex>] [-y <CommunityName>] -2{e | -d}
```

### <a name="parameters"></a>Paramètres

|          Paramètre           |                                                                                                                                                                                                                                                                                                     Description                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -a              |                                                                                                                                                                                                                                                                                       crée un port d’imprimante TCP/IP standard.                                                                                                                                                                                                                                                                                        |
|              -d              |                                                                                                                                                                                                                                                                                       supprime un port d’imprimante TCP/IP standard.                                                                                                                                                                                                                                                                                        |
|              -l              |                                                                                                                                                                                                                                                             répertorie tous les ports d’imprimante TCP/IP standard sur l’ordinateur spécifié avec le paramètre **-s** .                                                                                                                                                                                                                                                             |
|              -g              |                                                                                                                                                                                                                                                                            Affiche la configuration d’un port d’imprimante TCP/IP standard.                                                                                                                                                                                                                                                                             |
|              -T              |                                                                                                                                                                                                                                                                           Configure les paramètres de port d’un port d’imprimante TCP/IP standard.                                                                                                                                                                                                                                                                           |
|        -r \<PortName>        |                                                                                                                                                                                                                                                                                Spécifie le port auquel l’imprimante est connectée.                                                                                                                                                                                                                                                                                 |
|       -s \<nom_serveur>       |                                                                                                                                                                                                                               Spécifie le nom de l’ordinateur distant qui héberge l’imprimante que vous souhaitez gérer. Si vous ne spécifiez pas d’ordinateur, l’ordinateur local est utilisé.                                                                                                                                                                                                                                |
| -u \<nom_utilisateur>-w<Password> |                                                                                                              Spécifie un compte disposant des autorisations nécessaires pour se connecter à l’ordinateur qui héberge l’imprimante que vous souhaitez gérer. Tous les membres du groupe Administrateurs local de l’ordinateur cible disposent de ces autorisations, mais les autorisations peuvent également être accordées à d’autres utilisateurs. Si vous ne spécifiez pas de compte, vous devez avoir ouvert une session sous un compte disposant de ces autorisations pour que la commande fonctionne.                                                                                                               |
|     -o {RAW &#124; LPR}      |                                                                                                                                                                                                              Spécifie le protocole utilisé par le port : TCP RAW ou TCP LPR. Si vous utilisez TCP RAW, vous pouvez éventuellement spécifier le numéro de port à l’aide du paramètre **-n** . Le numéro de port par défaut est 9100.                                                                                                                                                                                                              |
|      -h \<hostaddress>       |                                                                                                                                                                                                                                                                   Spécifie (par adresse IP) l’imprimante pour laquelle vous souhaitez configurer le port.                                                                                                                                                                                                                                                                    |
|       -q \<QueueName>        |                                                                                                                                                                                                                                                                                     Spécifie le nom de la file d’attente d’un port TCP brut.                                                                                                                                                                                                                                                                                     |
|       -n \<numéro_port>       |                                                                                                                                                                                                                                                                    Spécifie le numéro de port pour un port TCP brut. Le numéro de port par défaut est 9100.                                                                                                                                                                                                                                                                    |
|        -m {e &#124; d}        |                                                                                                                                                                                                                                                       Spécifie si SNMP est activé. Le paramètre **e** active SNMP. Le paramètre **d** désactive SNMP.                                                                                                                                                                                                                                                        |
|        -i \<SNMPIndex        |                                                                                                                                                                                                                             Spécifie l’index SNMP, si SNMP est activé. Pour plus d’informations, consultez la RFC 1759 sur le [site Web RFC Editor](https://go.microsoft.com/fwlink/?LinkId=569).                                                                                                                                                                                                                              |
|     -y \<CommunityName>      |                                                                                                                                                                                                                                                                                Spécifie le nom de communauté SNMP, si SNMP est activé.                                                                                                                                                                                                                                                                                |
|       -2 {e &#124;-d}        | Spécifie si les spouleurs doubles (également appelés réattente) sont activés pour les ports TCP LPR. Les spouleurs doubles sont nécessaires, car TCP LPR doit inclure un nombre d’octets précis dans le fichier de contrôle qui est envoyé à l’imprimante, mais le protocole ne peut pas récupérer le nombre à partir du fournisseur d’impression local. Par conséquent, lorsqu’un fichier est mis en attente dans une file d’attente à l’impression TCP LPR, il est également mis en file d’attente en tant que fichier temporaire dans le répertoire system32. TCP LPR détermine la taille du fichier temporaire et envoie la taille au serveur exécutant LPD. Le paramètre **e** active les spouleurs doubles. Le paramètre **d** désactive les spouleurs doubles. |
|              /?              |                                                                                                                                                                                                                                                                                         Affiche l'aide à l'invite de commandes.                                                                                                                                                                                                                                                                                         |

## <a name="remarks"></a>Notes 
-   La commande **prnport** est un script Visual Basic situé dans le répertoire%windir%\system32\\\ <language> printing_Admin_Scripts. Pour utiliser cette commande, à l’invite de commandes, tapez **cscript** suivi du chemin d’accès complet au fichier prnport ou accédez au dossier approprié. Par exemple :
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnport
    ```
-   Si les informations que vous fournissez contiennent des espaces, utilisez des guillemets autour du texte ( `"computer Name"`par exemple,).
-   Le protocole TCP RAW est un protocole de performances plus élevé sur Windows que le protocole LPR.

## <a name="examples"></a><a name="BKMK_examples"></a>Illustre
Pour afficher tous les ports d’impression TCP/IP standard sur \\le serveur \Server1, tapez :
```
cscript prnport -l -s Server1
```
Pour supprimer le port d’impression TCP/IP standard sur le \\serveur \Server1 qui se connecte à une imprimante réseau sur 10.2.3.4, tapez :
```
cscript prnport -d -s Server1 -r IP_10.2.3.4
```
Pour ajouter un port d’impression TCP/IP standard sur le \\serveur \Server1 qui se connecte à une imprimante réseau sur 10.2.3.4 et qui utilise le protocole TCP RAW sur le port 9100, tapez :
```
cscript prnport -a -s Server1 -r IP_10.2.3.4 -h 10.2.3.4 -o raw -n 9100
```
Pour activer SNMP, spécifiez le nom de communauté « public » et définissez l’index SNMP sur 1 sur une imprimante réseau sur 10.2.3.4 partagé par \\le serveur \Server1, tapez :
```
cscript prnport -t -s Server1 -r IP_10.2.3.4 -me -y public -i 1 -n 9100
```
Pour ajouter un port d’impression TCP/IP standard sur l’ordinateur local qui se connecte à une imprimante réseau sur 10.2.3.4 et obtenir automatiquement les paramètres de l’appareil à partir de l’imprimante, tapez :
```
cscript prnport -a -r IP_10.2.3.4 -h 10.2.3.4
```

## <a name="additional-references"></a>Références supplémentaires
- [Référence des](command-line-syntax-key.md)
[commandes d’impression](print-command-reference.md) de la clé de syntaxe de ligne de commande
