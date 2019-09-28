---
title: Commandes netsh pour l’interface portproxy
description: Utilisez les commandes netsh de l’interface portproxy pour agir en tant que proxies entre des réseaux et des applications IPv4 et IPv6.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: 6244213ea689f07230ce53288e52959972112037
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405511"
---
# <a name="netsh-interface-portproxy-commands"></a>Commandes Netsh interface portproxy

Utilisez les commandes netsh de l' **interface portproxy** pour agir en tant que proxies entre des réseaux et des applications IPv4 et IPv6. Vous pouvez utiliser ces commandes pour établir le service proxy des manières suivantes :

-   Messages d’application et d’ordinateur configurés IPv4 envoyés à d’autres ordinateurs et applications configurés avec IPv4.

-   Messages d’application et d’ordinateur configurés par IPv4 envoyés aux ordinateurs et applications configurés par IPv6.

-   Messages d’ordinateur et d’application configurés IPv6 envoyés aux ordinateurs et applications configurés par IPv4.

-   Messages d’ordinateur et d’application configurés IPv6 envoyés à d’autres ordinateurs et applications configurés par IPv6.

Lorsque vous écrivez des fichiers de commandes ou des scripts à l’aide de ces commandes, chaque commande doit commencer par l' **interface portproxy**. Par exemple, lors de l’utilisation de la commande **Delete v4tov6** pour spécifier que le serveur portproxy supprime un port et une adresse IPv4 de la liste des adresses IPv4 que le serveur écoute, le fichier de commandes ou le script doit utiliser la syntaxe suivante :

```PowerShell
netsh interface portproxy delete v4tov6listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address| HostName}] [[protocol=]tcp]
```

Les commandes de l’interface netsh portproxy disponibles sont les suivantes :

-   [Ajouter v4tov4](#add-v4tov4)

-   [Ajouter v4tov6](#add-v4tov6)

-   [Ajouter v6tov4](#add-v6tov4)

-   [Ajouter v6tov6](#add-v6tov6)

-   [supprimer v4tov4](#delete-v4tov4)

-   [supprimer v4tov6](#delete-v4tov6)

-   [supprimer v6tov6](#delete-v6tov6)

-   [initialisation](#reset)

-   [définir v4tov4](#set-v4tov4)

-   [définir v4tov6](#set-v4tov6)

-   [setv6tov4](#set-v6tov4)

-   [définir v6tov6](#set-v6tov6)

-   [Afficher tout](#show-all)

-   [afficher v4tov4](#show-v4tov4)

-   [afficher v4tov6](#show-v4tov6)

-   [afficher v6tov4](#show-v6tov4)

-   [afficher v6tov6](#show-v6tov6)


## <a name="add-v4tov4"></a>Ajouter v4tov4

Le serveur portproxy écoute les messages envoyés à un port et une adresse IPv4 spécifiques et mappe un port et une adresse IPv4 pour envoyer les messages reçus après l’établissement d’une connexion TCP distincte.

### <a name="syntax"></a>Syntaxe

```PowerShell
add v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName}] [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName}] [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres


|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **ListenPort**   |                                                           Spécifie le port IPv4, par numéro de port ou nom de service, sur lequel écouter.                                                            |
| **connectaddress** | Spécifie l’adresse IPv4 à laquelle se connecter. Les valeurs acceptables sont adresse IP, nom NetBIOS de l’ordinateur ou nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|  **connectport**   |       Spécifie le port IPv4, par numéro de port ou nom de service, auquel se connecter. Si **ConnectPort** n’est pas spécifié, la valeur par défaut est la valeur de **ListenPort** sur l’ordinateur local.        |
| **écouter**  | Spécifie l’adresse IPv4 à écouter. Les valeurs acceptables sont adresse IP, nom NetBIOS de l’ordinateur ou nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|    **No**    |                                                                                  Spécifie le protocole à utiliser.                                                                                   |

## <a name="add-v4tov6"></a>Ajouter v4tov6

Le serveur portproxy écoute les messages envoyés à un port et une adresse IPv4 spécifiques, et mappe un port et une adresse IPv6 pour envoyer les messages reçus après l’établissement d’une connexion TCP distincte.

### <a name="syntax"></a>Syntaxe

```PowerShell
add v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **ListenPort**   |                                                           Spécifie le port IPv4, par numéro de port ou nom de service, sur lequel écouter.                                                            |
| **connectaddress** | Spécifie l’adresse IPv6 à laquelle se connecter. Les valeurs acceptables sont adresse IP, nom NetBIOS de l’ordinateur ou nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|  **connectport**   |       Spécifie le port IPv6, par numéro de port ou nom de service, auquel se connecter. Si **ConnectPort** n’est pas spécifié, la valeur par défaut est la valeur de **ListenPort** sur l’ordinateur local.        |
| **écouter**  | Spécifie l’adresse IPv4 sur laquelle écouter. Les valeurs acceptables sont adresse IP, nom NetBIOS de l’ordinateur ou nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local.  |
|    **No**    |                                                                                  Spécifie le protocole à utiliser.                                                                                   |

## <a name="add-v6tov4"></a>Ajouter v6tov4

Le serveur portproxy écoute les messages envoyés à un port et une adresse IPv6 spécifiques, et mappe un port et une adresse IPv4 auxquels envoyer les messages reçus après l’établissement d’une connexion TCP distincte.

### <a name="syntax"></a>Syntaxe

```PowerShell
add v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **ListenPort**   |                                                           Spécifie le port IPv6, par numéro de port ou nom de service, sur lequel écouter.                                                            |
| **connectaddress** | Spécifie l’adresse IPv4 à laquelle se connecter. Les valeurs acceptables sont adresse IP, nom NetBIOS de l’ordinateur ou nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|  **connectport**   |       Spécifie le port IPv4, par numéro de port ou nom de service, auquel se connecter. Si **ConnectPort** n’est pas spécifié, la valeur par défaut est la valeur de **ListenPort** sur l’ordinateur local.        |
| **écouter**  | Spécifie l’adresse IPv6 sur laquelle écouter. Les valeurs acceptables sont adresse IP, nom NetBIOS de l’ordinateur ou nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local.  |
|    **No**    |                                                                                  Spécifie le protocole à utiliser.                                                                                   |

## <a name="add-v6tov6"></a>Ajouter v6tov6

Le serveur portproxy écoute les messages envoyés à un port et une adresse IPv6 spécifiques, et mappe un port et une adresse IPv6 auxquels envoyer les messages reçus après l’établissement d’une connexion TCP distincte.

### <a name="syntax"></a>Syntaxe

```PowerShell
add v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **ListenPort**   |                                                           Spécifie le port IPv6, par numéro de port ou nom de service, sur lequel écouter.                                                            |
| **connectaddress** | Spécifie l’adresse IPv6 à laquelle se connecter. Les valeurs acceptables sont adresse IP, nom NetBIOS de l’ordinateur ou nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|  **connectport**   |       Spécifie le port IPv6, par numéro de port ou nom de service, auquel se connecter. Si **ConnectPort** n’est pas spécifié, la valeur par défaut est la valeur de **ListenPort** sur l’ordinateur local.        |
| **écouter**  | Spécifie l’adresse IPv6 sur laquelle écouter. Les valeurs acceptables sont adresse IP, nom NetBIOS de l’ordinateur ou nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local.  |
|    **No**    |                                                                                  Spécifie le protocole à utiliser.                                                                                   |

## <a name="delete-v4tov4"></a>supprimer v4tov4

Le serveur portproxy supprime une adresse IPv4 de la liste des ports et adresses IPv4 que le serveur écoute.

### <a name="syntax"></a>Syntaxe

```PowerShell
delete v4tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **ListenPort**   |                                    Spécifie le port IPv4 à supprimer.                                    |
| **écouter** | Spécifie l’adresse IPv4 à supprimer. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|   **No**    |                                      Spécifie le protocole à utiliser.                                      |

## <a name="delete-v4tov6"></a>supprimer v4tov6

Le serveur portproxy supprime un port et une adresse IPv4 de la liste des adresses IPv4 que le serveur écoute.

### <a name="syntax"></a>Syntaxe

```PowerShell 
delete v4tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **ListenPort**   |                                    Spécifie le port IPv4 à supprimer.                                    |
| **écouter** | Spécifie l’adresse IPv4 à supprimer. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|   **No**    |                                      Spécifie le protocole à utiliser.                                      |

## <a name="delete-v6tov4"></a>supprimer v6tov4

Le serveur portproxy supprime un port IPv6 et une adresse de la liste des adresses IPv6 que le serveur écoute.

### <a name="syntax"></a>Syntaxe

```PowerShell
delete v6tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **ListenPort**   |                                    Spécifie le port IPv6 à supprimer.                                    |
| **écouter** | Spécifie l’adresse IPv6 à supprimer. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|   **No**    |                                      Spécifie le protocole à utiliser.                                      |

## <a name="delete-v6tov6"></a>supprimer v6tov6

Le serveur portproxy supprime une adresse IPv6 de la liste des adresses IPv6 que le serveur écoute.

### <a name="syntax"></a>Syntaxe

```PowerShell
delete v6tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **ListenPort**   |                                    Spécifie le port IPv6 à supprimer.                                    |
| **écouter** | Spécifie l’adresse IPv6 à supprimer. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|   **No**    |                                      Spécifie le protocole à utiliser.                                      |

## <a name="reset"></a>Initialisation

Réinitialise l’état de configuration IPv6.

### <a name="syntax"></a>Syntaxe

`reset`

## <a name="set-v4tov4"></a>définir v4tov4

Modifie les valeurs des paramètres d’une entrée existante sur le serveur portproxy créé à l’aide de la commande **add v4tov4** ou ajoute une nouvelle entrée à la liste qui mappe les paires port/adresse.

### <a name="syntax"></a>Syntaxe

```PowerShell
set v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **ListenPort**   |                                                           Spécifie le port IPv4, par numéro de port ou nom de service, sur lequel écouter.                                                            |
| **connectaddress** | Spécifie l’adresse IPv4 à laquelle se connecter. Les valeurs acceptables sont adresse IP, nom NetBIOS de l’ordinateur ou nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|  **connectport**   |       Spécifie le port IPv4, par numéro de port ou nom de service, auquel se connecter. Si **ConnectPort** n’est pas spécifié, la valeur par défaut est la valeur de **ListenPort** sur l’ordinateur local.        |
| **écouter**  | Spécifie l’adresse IPv4 à écouter. Les valeurs acceptables sont adresse IP, nom NetBIOS de l’ordinateur ou nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|    **No**    |                                                                                  Spécifie le protocole à utiliser.                                                                                   |

## <a name="set-v4tov6"></a>définir v4tov6

Modifie les valeurs des paramètres d’une entrée existante sur le serveur portproxy créé à l’aide de la commande **Add v4tov6** ou ajoute une nouvelle entrée à la liste qui mappe les paires port/adresse.

### <a name="syntax"></a>Syntaxe

```PowerShell
set v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **ListenPort**   |                                                           Spécifie le port IPv4, par numéro de port ou nom de service, sur lequel écouter.                                                            |
| **connectaddress** | Spécifie l’adresse IPv6 à laquelle se connecter. Les valeurs acceptables sont adresse IP, nom NetBIOS de l’ordinateur ou nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|  **connectport**   |       Spécifie le port IPv6, par numéro de port ou nom de service, auquel se connecter. Si **ConnectPort** n’est pas spécifié, la valeur par défaut est la valeur de **ListenPort** sur l’ordinateur local.        |
| **écouter**  | Spécifie l’adresse IPv4 sur laquelle écouter. Les valeurs acceptables sont adresse IP, nom NetBIOS de l’ordinateur ou nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local.  |
|    **No**    |                                                                                  Spécifie le protocole à utiliser.                                                                                   |

## <a name="set-v6tov4"></a>définir v6tov4

Modifie les valeurs des paramètres d’une entrée existante sur le serveur portproxy créé à l’aide de la commande **Add v6tov4** ou ajoute une nouvelle entrée à la liste qui mappe les paires port/adresse.

### <a name="syntax"></a>Syntaxe

```PowerShell
set v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **ListenPort**   |                                                           Spécifie le port IPv6, par numéro de port ou nom de service, sur lequel écouter.                                                            |
| **connectaddress** | Spécifie l’adresse IPv4 à laquelle se connecter. Les valeurs acceptables sont adresse IP, nom NetBIOS de l’ordinateur ou nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|  **connectport**   |       Spécifie le port IPv4, par numéro de port ou nom de service, auquel se connecter. Si **ConnectPort** n’est pas spécifié, la valeur par défaut est la valeur de **ListenPort** sur l’ordinateur local.        |
| **écouter**  | Spécifie l’adresse IPv6 sur laquelle écouter. Les valeurs acceptables sont adresse IP, nom NetBIOS de l’ordinateur ou nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local.  |
|    **No**    |                                                                                  Spécifie le protocole à utiliser.                                                                                   |

## <a name="set-v6tov6"></a>définir v6tov6

Modifie les valeurs des paramètres d’une entrée existante sur le serveur portproxy créé à l’aide de la commande **Add v6tov6** ou ajoute une nouvelle entrée à la liste qui mappe les paires port/adresse.

### <a name="syntax"></a>Syntaxe

```PowerShell 
set v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|                    |                                                                                                                                                                                                    |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **ListenPort**   |                                                            Spécifie le port IPv6, par numéro de port ou nom de service, sur lequel écouter.                                                            |
| **connectaddress** | Spécifie l’adresse IPv6 à laquelle se connecter. Les valeurs acceptables sont adresse IP, nom NetBIOS de l’ordinateur ou nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local.  |
|  **connectport**   |        Spécifie le port IPv6, par numéro de port ou nom de service, auquel se connecter. Si **ConnectPort** n’est pas spécifié, la valeur par défaut est la valeur de **ListenPort** sur l’ordinateur local.        |
| **écouter**  | Spécifie l’adresse IPv6 sur laquelle écouter. Les valeurs acceptables sont adresse IP, nom NetBIOS de l’ordinateur ou nom DNS de l’ordinateur. Si vous ne spécifiez pas d’adresse, la valeur par défaut est l’ordinateur local. |
|    **No**    |                                                                                   Spécifie le protocole à utiliser.                                                                                   |

## <a name="show-all"></a>afficher tous

Affiche tous les paramètres portproxy, y compris les paires port/adresse pour v4tov4, v4tov6, v6tov4 et v6tov6.

### <a name="syntax"></a>Syntaxe

`show all`


## <a name="show-v4tov4"></a>afficher v4tov4

Affiche les paramètres v4tov4 portproxy.

### <a name="syntax"></a>Syntaxe

`show v4tov4`

## <a name="show-v4tov6"></a>afficher v4tov6

Affiche les paramètres v4tov6 portproxy.

### <a name="syntax"></a>Syntaxe

`show v4tov6`

## <a name="show-v6tov4"></a>afficher v6tov4

Affiche les paramètres v6tov4 portproxy.

### <a name="syntax"></a>Syntaxe

`show v6tov4`


## <a name="show-v6tov6"></a>afficher v6tov6

Affiche les paramètres v6tov6 portproxy.

### <a name="syntax"></a>Syntaxe

`show v6tov6`

---
