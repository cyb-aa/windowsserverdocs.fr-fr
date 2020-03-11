---
title: Commandes netsh pour réseau haut débit mobile (MBN)
description: Utilisez netsh mbn pour interroger et configurer les paramètres et réglages de haut débit mobile.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: ''
ms.author: ''
author: apdutta
ms.date: 02/20/2020
ms.openlocfilehash: 7cfbe623c82951c42fc9a532e1810fb3b9eb94d9
ms.sourcegitcommit: aae1ff743df3e1f3f0a5fd10fefb9fc71c0e39d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78236230"
---
# <a name="netsh-mbn-commands"></a>Commandes netsh mbn


Utilisez **netsh mbn** pour interroger et configurer les paramètres et réglages de haut débit mobile.

> [!TIP]
> Vous pouvez obtenir de l’aide sur la commande netsh mbn en utilisant 
>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;netsh mbn /?

Les commandes netsh mbn disponibles sont les suivantes :

- [add](#add)
- [connect](#connect)
- [delete](#delete)
- [disconnect](#disconnect)
- [diagnose](#diagnose)
- [dump](#dump)
- [help](#help)
- [set](#set)
- [show](#show)

## <a name="add"></a>ajouter

Ajoute une entrée de configuration à une table.

Les commandes netsh mbn add disponibles sont les suivantes :

- [dmprofile](#dmprofile)
- [profile](#profile)

### <a name="dmprofile"></a>dmprofile

Ajoute un profil de configuration DM dans le magasin des données de profil.

**Syntaxe**

```powershell
add dmprofile [interface=]<string> [name=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |
| **name**      | Nom du fichier XML du profil. Il s’agit du nom du fichier XML contenant les données du profil.     | Obligatoire |


**Exemple**

```powershell
add dmprofile interface="Cellular" name="Profile1.xml"
```


### <a name="profile"></a>profile

Ajoute un profil réseau dans le magasin des données de profil.

**Syntaxe**

```powershell
add profile [interface=]<string> [name=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |
| **name**      | Nom du fichier XML du profil. Il s’agit du nom du fichier XML contenant les données du profil.     | Obligatoire |


**Exemple**

```powershell
add profile interface="Cellular" name="Profile1.xml"
```


## <a name="connect"></a>connect

Établit une connexion à un réseau haut débit mobile.

**Syntaxe**

```powershell
connect [interface=]<string> [connmode=]tmp|name [name=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |
| **connmode**  | Spécifie la manière dont les paramètres de connexion sont fournis. La connexion peut être demandée à l’aide d’un fichier XML de profil ou à l’aide d’un nom de profil pour le fichier XML de profil qui a été précédemment stocké dans le magasin des données de profil haut débit mobile à l’aide de la commande « netsh mbn add profile ». Dans le premier cas, le paramètre connmode doit contenir « tmp » comme valeur. En revanche, ce doit être « name » dans le dernier cas.                                       | Obligatoire |
| **name**      | Nom du fichier XML du profil. Il s’agit du nom du fichier XML contenant les données du profil.     | Obligatoire |


**Exemples**

```powershell
connect interface="Cellular" connmode=tmp name=d:\profile.xml
connect interface="Cellular" connmode=name name=MyProfileName
```


## <a name="delete"></a>supprimer

Supprime une entrée de configuration d’une table.

Les commandes netsh mbn delete disponibles sont les suivantes :

- [dmprofile](#dmprofile)
- [profile](#profile)

### <a name="dmprofile"></a>dmprofile

Supprime un profil de configuration DM du magasin des données de profil.

**Syntaxe**

```powershell
delete dmprofile [interface=]<string> [name=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |
| **name**      | Nom du fichier XML du profil. Il s’agit du nom du fichier XML contenant les données du profil.     | Obligatoire |

**Exemple**

```powershell
delete dmprofile interface="Cellular" name="myProfileName"
```

### <a name="profile"></a>profile

Supprime un profil réseau du magasin des données de profil.

**Syntaxe**

```powershell
delete profile [interface=]<string> [name=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |
| **name**      | Nom du fichier XML du profil. Il s’agit du nom du fichier XML contenant les données du profil.     | Obligatoire |


**Exemple**

```powershell
delete profile interface="Cellular" name="myProfileName"
```

## <a name="diagnose"></a>diagnose

Exécute des diagnostics pour les problèmes cellulaires de base.

**Syntaxe**

```powershell
diagnose [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |


**Exemple**

```powershell
diagnose interface="Cellular"
```


## <a name="disconnect"></a>déconnecter

Met fin à la connexion à un réseau haut débit mobile.

**Syntaxe**

```powershell
disconnect [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |


**Exemple**

```powershell
disconnect interface="Cellular"
```


## <a name="dump"></a>dump

Affiche un script de configuration. 

Crée un script qui contient la configuration actuelle.  S’il est enregistré dans un fichier, ce script peut être utilisé pour restaurer les paramètres de configuration modifiés.

**Syntaxe**

```powershell
dump
```

## <a name="help"></a>help

Affiche la liste des commandes.

**Syntaxe**

```powershell
help
```

## <a name="set"></a>set

Définit les informations de configuration.

Les commandes netsh mbn set disponibles sont les suivantes :

- [acstate](#acstate)
- [dataenablement](#dataenablement)
- [dataroamcontrol](#dataroamcontrol)
- [enterpriseapnparams](#enterpriseapnparams)
- [highestconncategory](#highestconncategory)
- [powerstate](#powerstate)
- [profileparameter](#profileparameter)
- [slotmapping](#slotmapping)
- [tracing](#tracing)

### <a name="acstate"></a>acstate

Définit l’état de connexion automatique des données haut débit mobile pour l’interface concernée.

**Syntaxe**

```powershell
set acstate [interface=]<string> [state=]autooff|autoon|manualoff|manualon
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |
| **name**      | État de connexion automatique à définir. Une des valeurs suivantes :<br>autooff : jeton de connexion automatique désactivé.<br>autoon : jeton de connexion automatique activé.<br>manualoff : jeton de connexion manuelle désactivé.<br>manualon : jeton de connexion manuelle activé. | Obligatoire |


**Exemple**

```powershell
set acstate interface="Cellular" state=autoon
```


### <a name="dataenablement"></a>dataenablement

Active ou désactive les données haut débit mobile pour le jeu de profils et l’interface concernés.

**Syntaxe**

```powershell
set dataenablement [interface=]<string> [profileset=]internet|mms|all [mode=]yes|no
```

**Parameters**

|                |                                                                                               |          |
|----------------|-----------------------------------------------------------------------------------------------|----------|
| **interface**  | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |
| **profileset** | Nom du jeu de profils.                                                                      | Obligatoire |
| **mode**       | Une des valeurs suivantes :<br>yes : active le jeu de profils cible.<br>no : désactive le jeu de profils cible.| Obligatoire |


**Exemple**

```powershell
set dataenablement interface="Cellular" profileset=mms mode=yes
```


### <a name="dataroamcontrol"></a>dataroamcontrol

Définit l’état de contrôle d’itinérance des données haut débit mobile pour le jeu de profils et l’interface concernés.

**Syntaxe**

```powershell
set dataroamcontrol [interface=]<string> [profileset=]internet|mms|all [state=]none|partner|all
```

**Parameters**

|                |                                                                                               |          |
|----------------|-----------------------------------------------------------------------------------------------|----------|
| **interface**  | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |
| **profileset** | Nom du jeu de profils.                                                                      | Obligatoire |
| **mode**       | Une des valeurs suivantes :<br>none : opérateur local uniquement.<br>partner : opérateurs locaux et partenaires uniquement.<br>all : opérateurs locaux, partenaires et d’itinérance.| Obligatoire |


**Exemple**

```powershell
set dataroamcontrol interface="Cellular" profileset=mms mode=partner
```


### <a name="enterpriseapnparams"></a>enterpriseapnparams

Définit les paramètres enterpriseAPN des données haut débit mobile pour l’interface concernée.

**Syntaxe**

```powershell
set enterpriseapnparams [interface=]<string> [allowusercontrol=]yes|no|nc [allowuserview=]yes|no|nc [profileaction=]add|delete|modify|nc
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |
| **allowusercontrol** | Une des valeurs suivantes :<br>yes : autoriser l’utilisateur final à contrôler enterpriseAPN.<br>no : ne pas autoriser l’utilisateur final à contrôler enterpriseAPN.<br>nc : aucune modification. | Obligatoire |
| **allowuserview** |Une des valeurs suivantes :<br>yes : autoriser l’utilisateur final à visualiser enterpriseAPN.<br>no : ne pas autoriser l’utilisateur final à visualiser enterpriseAPN.<br>nc : aucune modification. | Obligatoire |
| **profileaction** | Une des valeurs suivantes :<br>add : un profil enterpriseAPN est ajouté.<br>delete : un profil enterpriseAPN est supprimé.<br>modify : un profil enterpriseAPN est modifié.<br>nc : aucune modification. | Obligatoire |


**Exemple**

```powershell
set enterpriseapnparams interface="Cellular" profileset=mms mode=yes
```


### <a name="highestconncategory"></a>highestconncategory

Définit la catégorie de connexion la plus élevée des données haut débit mobile pour l’interface concernée.

**Syntaxe**

```powershell
set highestconncategory [interface=]<string> [highestcc=]admim|user|operator|device
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |
| **highestcc** | Une des valeurs suivantes :<br>admin : profils provisionnés par l’administrateur.<br>user : profils provisionnés par l’utilisateur.<br>operator : profils provisionnés par l’opérateur.<br>device : profils provisionnés par l’appareil. | Obligatoire |


**Exemple**

```powershell
set highestconncategory interface="Cellular" highestcc=operator
```


### <a name="powerstate"></a>powerstate

Active ou désactive le transmetteur radio haut débit mobile pour l’interface concernée.

**Syntaxe**

```powershell
set powerstate [interface=]<string> [state=]on|off
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |
| **state**      | Une des valeurs suivantes :<br>on : active le transmetteur radio.<br>off : désactive le transmetteur radio. | Obligatoire |


**Exemple**

```powershell
set powerstate interface="Cellular" state=on
```


### <a name="profileparameter"></a>profileparameter

Définissez les paramètres d’un profil réseau haut débit mobile.

**Syntaxe**

```powershell
set profileparameter [name=]<string> [[interface=]<string>] [[cost]=default|unrestricted|fixed|variable]
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **name**      | Nom du profil à modifier. Si l’interface est spécifiée, seul le profil sur cette interface est modifié. | Obligatoire |
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Facultatif |
| **cost**      | Coût associé au profil.                                                             | Facultatif |


**Remarques**

Au moins un paramètre doit être spécifié entre le nom de l’interface et le coût.


**Exemple**

```powershell
set profileparameter name="profile 1" cost=default
```


### <a name="slotmapping"></a>slotmapping

Définit le mappage du connecteur du modem haut débit mobile pour l’interface concernée.

**Syntaxe**

```powershell
set slotmapping [interface=]<string> [slotindex=]<integer>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |
| **slotindex** | Index du connecteur à définir.                                                                         | Obligatoire |


**Exemple**

```powershell
set slotmapping interface="Cellular" slotindex=1
```


### <a name="tracing"></a>tracing

Activez ou désactivez le traçage.

**Syntaxe**

```powershell
set tracing [mode=]yes|no
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **mode**      | Une des valeurs suivantes :<br>yes : active le traçage pour le haut débit mobile.<br>no : désactive le traçage pour le haut débit mobile.     | Obligatoire |


**Exemple**

```powershell
set tracing mode=yes
```

## <a name="show"></a>show

Affiche des informations sur le réseau haut débit mobile.

Les commandes netsh mbn set disponibles sont les suivantes :

- [acstate](#acstate)
- [capability](#capability)
- [connection](#connection)
- [dataenablement](#dataenablement)
- [dataroamcontrol](#dataroamcontrol)
- [dmprofiles](#dmprofiles)
- [enterpriseapnparams](#enterpriseapnparams)
- [highestconncategory](#highestconncategory)
- [homeprovider](#homeprovider)
- [interfaces](#interfaces)
- [netlteattachinfo](#netlteattachinfo)
- [pin](#pin)
- [pinlist](#pinlist)
- [preferredproviders](#preferredproviders)
- [profiles](#profiles)
- [profilestate](#profilestate)
- [provisionedcontexts](#provisionedcontexts)
- [purpose](#purpose)
- [radio](#radio)
- [readyinfo](#readyinfo)
- [signal](#signal)
- [slotmapping](#slotmapping)
- [slotstatus](#slotstatus)
- [smsconfig](#smsconfig)
- [tracing](#tracing)
- [visibleproviders](#visibleproviders)

### <a name="acstate"></a>acstate  

Indique l’état de connexion automatique des données haut débit mobile pour l’interface concernée.

**Syntaxe**

```powershell
show acstate [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |


**Exemple**

```powershell
show acstate interface="Cellular"
```


### <a name="capability"></a>capability

Affiche les informations de capacité d’interface pour l’interface concernée.

**Syntaxe**

```powershell
show capability [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |


**Exemple**

```powershell
show capability interface="Cellular"
```


### <a name="connection"></a>connection

Affiche les informations de connexion actuelle pour l’interface concernée.

**Syntaxe**

```powershell
show connection [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |


**Exemple**

```powershell
show connection interface="Cellular"
```


### <a name="dataenablement"></a>dataenablement

Indique l’état d’activation des données haut débit mobile pour l’interface concernée.

**Syntaxe**

```powershell
show dataenablement [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |


**Exemple**

```powershell
show dataenablement interface="Cellular"
```


### <a name="dataroamcontrol"></a>dataroamcontrol

Indique l’état de contrôle d’itinérance des données haut débit mobile pour l’interface concernée.

**Syntaxe**

```powershell
show dataroamcontrol [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |


**Exemple**

```powershell
show dataroamcontrol interface="Cellular"
```


### <a name="dmprofiles"></a>dmprofiles

Affiche la liste des profils de configuration DM configurés sur le système.

**Syntaxe**

```powershell
show dmprofiles [[name=]<string>] [[interface=]<string>]
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **name**      | Nom du profil à afficher.                                                               | Facultatif |
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Facultatif |

**Remarques**
    
Affiche les données du profil ou liste les profils sur le système.

Si le nom du profil est fourni, le contenu du profil est affiché. Sinon, les profils sont listés pour l’interface.

Si le nom de l’interface est fourni, seul le profil spécifié sur l’interface donnée est listé. Sinon, le premier profil correspondant est affiché.

**Exemple**

```powershell
show dmprofiles name="profile 1" interface="Cellular"
show dmprofiles name="profile 2"
show dmprofiles
```


### <a name="enterpriseapnparams"></a>enterpriseapnparams

Indique les paramètres enterpriseAPN des données haut débit mobile pour l’interface concernée.

**Syntaxe**

```powershell
show enterpriseapnparams [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |


**Exemple**

```powershell
show enterpriseapnparams interface="Cellular"
```


### <a name="highestconncategory"></a>highestconncategory

Indique la catégorie de connexion la plus élevée des données haut débit mobile pour l’interface concernée.

**Syntaxe**

```powershell
show highestconncategory [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |


**Exemple**

```powershell
show highestconncategory interface="Cellular"
```


### <a name="homeprovider"></a>homeprovider

Affiche les informations de fournisseur personnel pour l’interface concernée.

**Syntaxe**

```powershell
show homeprovider [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |


**Exemple**

```powershell
show homeprovider interface="Cellular"
```


### <a name="interfaces"></a>interfaces

Affiche la liste des interfaces haut débit mobile sur le système. Il n’existe aucun paramètre pour cette commande.

**Syntaxe**

```powershell
show interfaces
```


### <a name="netlteattachinfo"></a>netlteattachinfo

Affiche les informations de liaison LTE du réseau haut débit mobile pour l’interface concernée.

**Syntaxe**

```powershell
show netlteattachinfo [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |


**Exemple**

```powershell
show netlteattachinfo interface="Cellular"
```

### <a name="pin"></a>pin      

Affiche les informations de code confidentiel pour l’interface concernée.

**Syntaxe**

```powershell
show pin [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |


**Exemple**

```powershell
show pin interface="Cellular"
```


### <a name="pinlist"></a>pinlist  

Affiche les informations de liste de codes confidentiels pour l’interface concernée.

**Syntaxe**

```powershell
show pinlist [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |


**Exemple**

```powershell
show pinlist interface="Cellular"
```


### <a name="preferredproviders"></a>preferredproviders

Affiche la liste des fournisseurs préférés pour l’interface concernée.

**Syntaxe**

```powershell
show preferredproviders [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |


**Exemple**

```powershell
show preferredproviders interface="Cellular"
```


### <a name="profiles"></a>profiles 

Affiche la liste des profils configurés sur le système.

**Syntaxe**

```powershell
show profiles [[name=]<string>] [[interface=]<string>] [[purpose=]<string>]
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **name**      | Nom du profil à afficher.                                                               | Facultatif |
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Facultatif |
| **purpose**   | Objectif | Facultatif |

**Remarques**

Si le nom du profil est fourni, le contenu du profil est affiché. Sinon, les profils sont listés pour l’interface.

Si le nom de l’interface est fourni, seul le profil spécifié sur l’interface donnée est listé. Sinon, le premier profil correspondant est affiché.

Si l’objectif (purpose) est fourni, seuls les profils dotés du GUID d’objectif correspondant sont affichés.  Sinon, les profils ne sont pas filtrés par objectif.  La chaîne peut être un GUID avec des accolades ou l’une des chaînes suivantes : internet, supl, mms, ims ou allhost.
    
**Exemple**

```powershell
show profiles interface="Cellular" purpose="{00000000-0000-0000-0000-000000000000}"
show profiles name="profile 1" interface="Cellular"
show profiles name="profile 2"
show profiles
```

### <a name="profilestate"></a>profilestate

Affiche l’état d’un profil haut débit mobile pour l’interface concernée.

**Syntaxe**

```powershell
show profilestate [interface=]<string> [name=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |
| **name**      | Nom du profil. Il s’agit du nom du profil dont l’état doit être affiché.            | Obligatoire |

**Exemple**

```powershell
show profilestate interface="Cellular" name="myProfileName"
```

### <a name="provisionedcontexts"></a>provisionedcontexts

Fournit les informations de contextes provisionnés pour l’interface concernée.

**Syntaxe**

```powershell
show provisionedcontexts [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |


**Exemple**

```powershell
show provisionedcontexts interface="Cellular"
```


### <a name="purpose"></a>purpose  

Indique les GUID des groupes d’objectifs qui peuvent être utilisés pour filtrer les profils sur l’appareil. Il n’existe aucun paramètre pour cette commande.

**Syntaxe**

```powershell
show purpose
```


### <a name="radio"></a>radio    

Fournit les informations de l’état radio pour l’interface concernée.

**Syntaxe**

```powershell
show radio [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |


**Exemple**

```powershell
show radio interface="Cellular"
```


### <a name="readyinfo"></a>readyinfo

Fournit les informations prêtes pour l’interface concernée.

**Syntaxe**

```powershell
show readyinfo [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |


**Exemple**

```powershell
show readyinfo interface="Cellular"
```


### <a name="signal"></a>signal   

Fournit les informations de signal pour l’interface concernée.

**Syntaxe**

```powershell
show signal [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |


**Exemple**

```powershell
show signal interface="Cellular"
```


### <a name="slotmapping"></a>slotmapping

Indique le mappage du connecteur du modem haut débit mobile pour l’interface concernée.

**Syntaxe**

```powershell
show slotmapping [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |


**Exemple**

```powershell
show slotmapping interface="Cellular"
```


### <a name="slotstatus"></a>slotstatus

Indique le statut du connecteur du modem haut débit mobile pour l’interface concernée.

**Syntaxe**

```powershell
show slotstatus [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |


**Exemple**

```powershell
show slotstatus interface="Cellular"
```


### <a name="smsconfig"></a>smsconfig

Fournit les informations de configuration SMS pour l’interface concernée.

**Syntaxe**

```powershell
show smsconfig [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |


**Exemple**

```powershell
show smsconfig interface="Cellular"
```


### <a name="tracing"></a>tracing  

Indique si le traçage haut débit mobile est activé ou désactivé.

**Syntaxe**

```powershell
show tracing 
```


### <a name="visibleproviders"></a>visibleproviders

Fournit la liste des fournisseurs visibles pour l’interface concernée.

**Syntaxe**

```powershell
show visibleproviders [interface=]<string>
```

**Parameters**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | Nom de l’interface. Il s’agit de l’un des noms d’interface indiqués par la commande « netsh mbn show interfaces ». | Obligatoire |


**Exemple**

```powershell
show visibleproviders interface="Cellular"
```
