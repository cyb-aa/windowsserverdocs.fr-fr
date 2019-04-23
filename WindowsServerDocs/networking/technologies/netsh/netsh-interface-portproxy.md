---
title: Commandes Netsh pour l’interface portproxy
description: Utiliser les commandes netsh interface portproxy pour agir en tant que proxy entre les réseaux et applications IPv4 et IPv6.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: 194a418fe6b33e312a3f2529e82d50d76cd15f4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842480"
---
# <a name="netsh-interface-portproxy-commands"></a>Commandes Netsh interface portproxy

Utilisez le **netsh interface portproxy** commandes en tant que proxy entre les réseaux et applications IPv4 et IPv6. Vous pouvez utiliser ces commandes pour établir un service de proxy comme suit :

-   Ordinateurs configurés avec IPv4 et application les messages envoyés à d’autres applications et ordinateurs configurés avec IPv4.

-   Application et les ordinateurs configurés avec IPv4 des messages envoyés à IPv6 configurée des ordinateurs et des applications.

-   Ordinateurs envoyés au configurés avec IPv4 et les applications messages des applications et des ordinateurs configurés avec IPv6.

-   Ordinateurs configurés avec IPv6 et l’application messages envoyés à d’autres applications et ordinateurs configurés avec IPv6.

Lorsque vous écrivez des fichiers de commandes ou des scripts à l’aide de ces commandes, chaque commande doit commencer par **netsh interface portproxy**. Par exemple, lorsque vous utilisez le **supprimer v4tov6** commande pour spécifier que le serveur portproxy supprime un port IPv4 et une adresse à partir de la liste des adresses IPv4 que le serveur écoute, le fichier de commandes ou le script doit utiliser la syntaxe suivante :

```PowerShell
netsh interface portproxy delete v4tov6listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address| HostName}] [[protocol=]tcp]
```

Les commandes de portproxy interface netsh disponibles sont :

-   [add v4tov4](#add-v4tov4)

-   [add v4tov6](#add-v4tov6)

-   [add v6tov4](#add-v6tov4)

-   [add v6tov6](#add-v6tov6)

-   [delete v4tov4](#delete-v4tov4)

-   [delete v4tov6](#delete-v4tov6)

-   [delete v6tov6](#delete-v6tov6)

-   [reset](#reset)

-   [set v4tov4](#set-v4tov4)

-   [set v4tov6](#set-v4tov6)

-   [setv6tov4](#set-v6tov4)

-   [set v6tov6](#set-v6tov6)

-   [Afficher tout](#show-all)

-   [show v4tov4](#show-v4tov4)

-   [show v4tov6](#show-v4tov6)

-   [show v6tov4](#show-v6tov4)

-   [show v6tov6](#show-v6tov6)


## <a name="add-v4tov4"></a>add v4tov4

Le serveur portproxy écoute les messages envoyés à un port spécifique et adresse IPv4 et mappages d’adresses d’un port et IPv4 pour envoyer les messages reçus après l’établissement d’une connexion TCP distincte.

### <a name="syntax"></a>Syntaxe

```PowerShell
add v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName}] [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName}] [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres


| | |
|-----|--------|----------|
| **listenport**     | Spécifie le port IPv4, par numéro de port ou le service name, sur lequel écouter.                                                                                                                      | Obligatoire |
| **connectaddress** | Spécifie l’adresse IPv4 à laquelle se connecter. Les valeurs acceptables sont l’adresse IP, nom d’ordinateur NetBIOS ou nom DNS de l’ordinateur. Si une adresse n’est pas spécifiée, la valeur par défaut est l’ordinateur local. |          |
| **connectport**    | Spécifie le port IPv4, par numéro de port ou le service de nom, à laquelle se connecter. Si **connectport** n’est pas spécifié, la valeur par défaut est la valeur de **listenport** sur l’ordinateur local.              |          |
| **listenaddress**  | Spécifie l’adresse IPv4 à écouter. Les valeurs acceptables sont l’adresse IP, nom d’ordinateur NetBIOS ou nom DNS de l’ordinateur. Si une adresse n’est pas spécifiée, la valeur par défaut est l’ordinateur local. |          |
| **protocol**       | Spécifie le protocole à utiliser.                                                                                                                                                                    |          |

## <a name="add-v4tov6"></a>add v4tov6

Le serveur portproxy écoute les messages envoyés à un port spécifique et d’adresse IPv4 et de mappages d’adresses d’un port et IPv6 pour envoyer les messages reçus après l’établissement d’une connexion TCP distincte.

### <a name="syntax"></a>Syntaxe

```PowerShell
add v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|   |   |
|-----------|-------------|----------|
| **listenport**     | Spécifie le port IPv4, par numéro de port ou le service name, sur lequel écouter.       | Obligatoire |
| **connectaddress** | Spécifie l’adresse IPv6 à laquelle se connecter. Les valeurs acceptables sont l’adresse IP, nom d’ordinateur NetBIOS ou nom DNS de l’ordinateur. Si une adresse n’est pas spécifiée, la valeur par défaut est l’ordinateur local. |          |
| **connectport**    | Spécifie le port IPv6, par numéro de port ou le service de nom, à laquelle se connecter. Si **connectport** n’est pas spécifié, la valeur par défaut est la valeur de **listenport** sur l’ordinateur local.              |          |
| **listenaddress**  | Spécifie l’adresse IPv4 sur lequel écouter. Les valeurs acceptables sont l’adresse IP, nom d’ordinateur NetBIOS ou nom DNS de l’ordinateur. Si une adresse n’est pas spécifiée, la valeur par défaut est l’ordinateur local.  |          |
| **protocol**       | Spécifie le protocole à utiliser.                                                                                                                                                                    |          |

## <a name="add-v6tov4"></a>add v6tov4

Le serveur portproxy écoute les messages envoyés à un port spécifique et d’adresse IPv6 et de mappages d’un port et IPv4 d’adresses auxquelles envoyer les messages reçus après l’établissement d’une connexion TCP distincte.

### <a name="syntax"></a>Syntaxe

```PowerShell
add v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|   |   |
|------------|-------------|----------|
| **listenport**     | Spécifie le port IPv6, par numéro de port ou le service name, sur lequel écouter.              | Obligatoire |
| **connectaddress** | Spécifie l’adresse IPv4 à laquelle se connecter. Les valeurs acceptables sont l’adresse IP, nom d’ordinateur NetBIOS ou nom DNS de l’ordinateur. Si une adresse n’est pas spécifiée, la valeur par défaut est l’ordinateur local. |          |
| **connectport**    | Spécifie le port IPv4, par numéro de port ou le service de nom, à laquelle se connecter. Si **connectport** n’est pas spécifié, la valeur par défaut est la valeur de **listenport** sur l’ordinateur local.              |          |
| **listenaddress**  | Spécifie l’adresse IPv6 sur lequel écouter. Les valeurs acceptables sont l’adresse IP, nom d’ordinateur NetBIOS ou nom DNS de l’ordinateur. Si une adresse n’est pas spécifiée, la valeur par défaut est l’ordinateur local.  |          |
| **protocol**       | Spécifie le protocole à utiliser.      |          |

## <a name="add-v6tov6"></a>add v6tov6

Le serveur portproxy écoute les messages envoyés à un port spécifique et d’adresse IPv6 et de mappages d’adresses d’un port et IPv6 auquel envoyer les messages reçus après l’établissement d’une connexion TCP distincte.

### <a name="syntax"></a>Syntaxe

```PowerShell
add v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|   |   |
|-------------|------------------|----------|
| **listenport**     | Spécifie le port IPv6, par numéro de port ou le service name, sur lequel écouter.       | Obligatoire |
| **connectaddress** | Spécifie l’adresse IPv6 à laquelle se connecter. Les valeurs acceptables sont l’adresse IP, nom d’ordinateur NetBIOS ou nom DNS de l’ordinateur. Si une adresse n’est pas spécifiée, la valeur par défaut est l’ordinateur local. |          |
| **connectport**    | Spécifie le port IPv6, par numéro de port ou le service de nom, à laquelle se connecter. Si **connectport** n’est pas spécifié, la valeur par défaut est la valeur de **listenport** sur l’ordinateur local.              |          |
| **listenaddress**  | Spécifie l’adresse IPv6 sur lequel écouter. Les valeurs acceptables sont l’adresse IP, nom d’ordinateur NetBIOS ou nom DNS de l’ordinateur. Si une adresse n’est pas spécifiée, la valeur par défaut est l’ordinateur local.  |          |
| **protocol**       | Spécifie le protocole à utiliser.                                                                                                                                                                    |          |

## <a name="delete-v4tov4"></a>delete v4tov4

Le serveur portproxy supprime une adresse IPv4 dans la liste des ports et pour lequel le serveur écoute des adresses IPv4.

### <a name="syntax"></a>Syntaxe

```PowerShell
delete v4tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|   |   |
|-------------------|----------------------------------------------------------------------------------------------------------|----------|
| **listenport**    | Spécifie le port IPv4 à supprimer.                                                                       | Obligatoire |
| **listenaddress** | Spécifie l’adresse IPv4 à supprimer. Si une adresse n’est pas spécifiée, la valeur par défaut est l’ordinateur local. |          |
| **protocol**      | Spécifie le protocole à utiliser.                                                                           |          |

## <a name="delete-v4tov6"></a>delete v4tov6

Le serveur portproxy supprime la liste des adresses IPv4 que le serveur écoute un port IPv4 et une adresse.

### <a name="syntax"></a>Syntaxe

```PowerShell 
delete v4tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|   |   |
|-------------------|----------------------------------------------------------------------------------------------------------|----------|
| **listenport**    | Spécifie le port IPv4 à supprimer.                                                                       | Obligatoire |
| **listenaddress** | Spécifie l’adresse IPv4 à supprimer. Si une adresse n’est pas spécifiée, la valeur par défaut est l’ordinateur local. |          |
| **protocol**      | Spécifie le protocole à utiliser.                                                                           |          |

## <a name="delete-v6tov4"></a>delete v6tov4

Le serveur portproxy supprime un port d’IPv6 et l’adresse à partir de la liste des adresses IPv6 pour lequel le serveur écoute.

### <a name="syntax"></a>Syntaxe

```PowerShell
delete v6tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|   |   |
|-------------------|----------------------------------------------------------------------------------------------------------|----------|
| **listenport**    | Spécifie le port IPv6 à supprimer.                                                                       | Obligatoire |
| **listenaddress** | Spécifie l’adresse IPv6 à supprimer. Si une adresse n’est pas spécifiée, la valeur par défaut est l’ordinateur local. |          |
| **protocol**      | Spécifie le protocole à utiliser.                                                                           |          |

## <a name="delete-v6tov6"></a>delete v6tov6

Le serveur portproxy supprime une adresse IPv6 à partir de la liste des adresses IPv6 pour lequel le serveur écoute.

### <a name="syntax"></a>Syntaxe

```PowerShell
delete v6tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|   |   |
|-------------------|----------------------------------------------------------------------------------------------------------|----------|
| **listenport**    | Spécifie le port IPv6 à supprimer.                                                                       | Obligatoire |
| **listenaddress** | Spécifie l’adresse IPv6 à supprimer. Si une adresse n’est pas spécifiée, la valeur par défaut est l’ordinateur local. |          |
| **protocol**      | Spécifie le protocole à utiliser.                                                                           |          |

## <a name="reset"></a>Réinitialiser

Réinitialise l’état de configuration IPv6.

### <a name="syntax"></a>Syntaxe

`reset`

## <a name="set-v4tov4"></a>ensemble v4tov4

Modifie les valeurs de paramètre d’une entrée existante sur le serveur portproxy créé avec le **add v4tov4** commande, ou ajoute une nouvelle entrée à la liste que met en correspondance les paires port/adresse.

### <a name="syntax"></a>Syntaxe

```PowerShell
set v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|   |   |
|--------------------|---------------------------|----------|
| **listenport**     | Spécifie le port IPv4, par numéro de port ou le service name, sur lequel écouter.     | Obligatoire |
| **connectaddress** | Spécifie l’adresse IPv4 à laquelle se connecter. Les valeurs acceptables sont l’adresse IP, nom d’ordinateur NetBIOS ou nom DNS de l’ordinateur. Si une adresse n’est pas spécifiée, la valeur par défaut est l’ordinateur local. |          |
| **connectport**    | Spécifie le port IPv4, par numéro de port ou le service de nom, à laquelle se connecter. Si **connectport** n’est pas spécifié, la valeur par défaut est la valeur de **listenport** sur l’ordinateur local.              |          |
| **listenaddress**  | Spécifie l’adresse IPv4 à écouter. Les valeurs acceptables sont l’adresse IP, nom d’ordinateur NetBIOS ou nom DNS de l’ordinateur. Si une adresse n’est pas spécifiée, la valeur par défaut est l’ordinateur local. |          |
| **protocol**       | Spécifie le protocole à utiliser.                                                                                                                                                                    |          |

## <a name="set-v4tov6"></a>ensemble v4tov6

Modifie les valeurs de paramètre d’une entrée existante sur le serveur portproxy créé avec le **ajouter v4tov6** commande, ou ajoute une nouvelle entrée à la liste que met en correspondance les paires port/adresse.

### <a name="syntax"></a>Syntaxe

```PowerShell
set v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|   |   |
|--------------------|---------------------|----------|
| **listenport**     | Spécifie le port IPv4, par numéro de port ou le service name, sur lequel écouter.     | Obligatoire |
| **connectaddress** | Spécifie l’adresse IPv6 à laquelle se connecter. Les valeurs acceptables sont l’adresse IP, nom d’ordinateur NetBIOS ou nom DNS de l’ordinateur. Si une adresse n’est pas spécifiée, la valeur par défaut est l’ordinateur local. |          |
| **connectport**    | Spécifie le port IPv6, par numéro de port ou le service de nom, à laquelle se connecter. Si **connectport** n’est pas spécifié, la valeur par défaut est la valeur de **listenport** sur l’ordinateur local.              |          |
| **listenaddress**  | Spécifie l’adresse IPv4 sur lequel écouter. Les valeurs acceptables sont l’adresse IP, nom d’ordinateur NetBIOS ou nom DNS de l’ordinateur. Si une adresse n’est pas spécifiée, la valeur par défaut est l’ordinateur local.  |          |
| **protocol**       | Spécifie le protocole à utiliser.                                                                                                                                                                    |          |

## <a name="set-v6tov4"></a>set v6tov4

Modifie les valeurs de paramètre d’une entrée existante sur le serveur portproxy créé avec le **ajouter v6tov4** commande, ou ajoute une nouvelle entrée à la liste que met en correspondance les paires port/adresse.

### <a name="syntax"></a>Syntaxe

```PowerShell
set v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|   |   |
|--------------------|----------------------|----------|
| **listenport**     | Spécifie le port IPv6, par numéro de port ou le service name, sur lequel écouter.      | Obligatoire |
| **connectaddress** | Spécifie l’adresse IPv4 à laquelle se connecter. Les valeurs acceptables sont l’adresse IP, nom d’ordinateur NetBIOS ou nom DNS de l’ordinateur. Si une adresse n’est pas spécifiée, la valeur par défaut est l’ordinateur local. |          |
| **connectport**    | Spécifie le port IPv4, par numéro de port ou le service de nom, à laquelle se connecter. Si **connectport** n’est pas spécifié, la valeur par défaut est la valeur de **listenport** sur l’ordinateur local.              |          |
| **listenaddress**  | Spécifie l’adresse IPv6 sur lequel écouter. Les valeurs acceptables sont l’adresse IP, nom d’ordinateur NetBIOS ou nom DNS de l’ordinateur. Si une adresse n’est pas spécifiée, la valeur par défaut est l’ordinateur local.  |          |
| **protocol**       | Spécifie le protocole à utiliser.                                                                                                                                                                    |          |

## <a name="set-v6tov6"></a>ensemble v6tov6

Modifie les valeurs de paramètre d’une entrée existante sur le serveur portproxy créé avec le **add v6tov6** commande, ou ajoute une nouvelle entrée à la liste que met en correspondance les paires port/adresse.

### <a name="syntax"></a>Syntaxe

```PowerShell 
set v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|   |   |
|--------------------|-------------------------|----------|
| **listenport**     | Spécifie le port IPv6, par numéro de port ou le service name, sur lequel écouter.   | Obligatoire |
| **connectaddress** | Spécifie l’adresse IPv6 à laquelle se connecter. Les valeurs acceptables sont l’adresse IP, nom d’ordinateur NetBIOS ou nom DNS de l’ordinateur. Si une adresse n’est pas spécifiée, la valeur par défaut est l’ordinateur local.  |          |
| **connectport**    | Spécifie le port IPv6, par numéro de port ou le service de nom, à laquelle se connecter. Si **connectport** n’est pas spécifié, la valeur par défaut est la valeur de **listenport** sur l’ordinateur local.               |          |
| **listenaddress**  | Spécifie l’adresse IPv6 sur lequel écouter. Les valeurs acceptables sont l’adresse IP, nom d’ordinateur NetBIOS ou nom DNS de l’ordinateur. Si vous ne spécifiez pas une adresse, la valeur par défaut est l’ordinateur local. |          |
| **protocol**       | Spécifie le protocole à utiliser.                                                                                                                                                                     |          |

## <a name="show-all"></a>afficher tous

Affiche tous les paramètres portproxy, y compris les paires port/adresse pour v4tov4, v4tov6, v6tov4 et v6tov6.

### <a name="syntax"></a>Syntaxe

`show all`


## <a name="show-v4tov4"></a>show v4tov4

Affiche les paramètres portproxy v4tov4.

### <a name="syntax"></a>Syntaxe

`show v4tov4`

## <a name="show-v4tov6"></a>show v4tov6

Affiche les paramètres portproxy v4tov6.

### <a name="syntax"></a>Syntaxe

`show v4tov6`

## <a name="show-v6tov4"></a>show v6tov4

Affiche les paramètres portproxy v6tov4.

### <a name="syntax"></a>Syntaxe

`show v6tov4`


## <a name="show-v6tov6"></a>show v6tov6

Affiche les paramètres portproxy v6tov6.

### <a name="syntax"></a>Syntaxe

`show v6tov6`

---
