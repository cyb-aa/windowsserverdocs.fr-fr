---
title: Commandes netsh pour interface portproxy
description: Utilisez les commandes netsh interface portproxy pour mettre en place des proxys entre les réseaux et des applications IPv4 et IPv6.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 08/30/2018
ms.openlocfilehash: 645786484881af3a0f6d9503e1f3fcde32a2cdfe
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316738"
---
# <a name="netsh-interface-portproxy-commands"></a>Commandes Netsh interface portproxy

Utilisez les commandes **netsh interface portproxy** pour mettre en place des proxys entre les réseaux et des applications IPv4 et IPv6. Vous pouvez utiliser ces commandes pour établir le service proxy des manières suivantes :

-   Messages d’application et d’ordinateur configurés avec IPv4 envoyés à d’autres ordinateurs et applications configurés avec IPv4.

-   Messages d’application et d’ordinateur configurés avec IPv4 envoyés à des ordinateurs et applications configurés avec IPv6.

-   Messages d’application et d’ordinateur configurés avec IPv6 envoyés à des ordinateurs et applications configurés avec IPv4.

-   Messages d’application et d’ordinateur configurés avec IPv6 envoyés à d’autres ordinateurs et applications configurés avec IPv6.

Quand vous écrivez des fichiers de commandes ou des scripts en utilisant ces commandes, chaque commande doit commencer par **netsh interface portproxy**. Par exemple, lors de l’utilisation de la commande **delete v4tov6** pour indiquer que le serveur portproxy doit supprimer un port et une adresse IPv4 de la liste des adresses IPv4 qu’il écoute, le fichier de commandes ou le script doit utiliser la syntaxe suivante :

```PowerShell
netsh interface portproxy delete v4tov6listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address| HostName}] [[protocol=]tcp]
```

Les commandes netsh interface portproxy disponibles sont les suivantes :

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

-   [show all](#show-all)

-   [show v4tov4](#show-v4tov4)

-   [show v4tov6](#show-v4tov6)

-   [show v6tov4](#show-v6tov4)

-   [show v6tov6](#show-v6tov6)


## <a name="add-v4tov4"></a>add v4tov4

Le serveur portproxy écoute les messages envoyés à un port et une adresse IPv4 spécifiques et mappe un port et une adresse IPv4 pour envoyer les messages reçus après l’établissement d’une connexion TCP distincte.

### <a name="syntax"></a>Syntaxe

```PowerShell
add v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName}] [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName}] [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres


|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Spécifie le port IPv4, par numéro de port ou nom de service, sur lequel écouter.                                                            |
| **connectaddress** | Spécifie l’adresse IPv4 à laquelle se connecter. Les valeurs acceptables sont l’adresse IP, le nom NetBIOS de l’ordinateur ou le nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|  **connectport**   |       Spécifie le port IPv4, par numéro de port ou nom de service, auquel se connecter. Si **connectport** n’est pas spécifié, la valeur par défaut est la valeur de **listenport** sur l’ordinateur local.        |
| **listenaddress**  | Spécifie l’adresse IPv4 à écouter. Les valeurs acceptables sont l’adresse IP, le nom NetBIOS de l’ordinateur ou le nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|    **protocol**    |                                                                                  Spécifie le protocole à utiliser.                                                                                   |

## <a name="add-v4tov6"></a>add v4tov6

Le serveur portproxy écoute les messages envoyés à un port et une adresse IPv4 spécifiques et mappe un port et une adresse IPv6 pour envoyer les messages reçus après l’établissement d’une connexion TCP distincte.

### <a name="syntax"></a>Syntaxe

```PowerShell
add v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Spécifie le port IPv4, par numéro de port ou nom de service, sur lequel écouter.                                                            |
| **connectaddress** | Spécifie l’adresse IPv6 à laquelle se connecter. Les valeurs acceptables sont l’adresse IP, le nom NetBIOS de l’ordinateur ou le nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|  **connectport**   |       Spécifie le port IPv6, par numéro de port ou nom de service, auquel se connecter. Si **connectport** n’est pas spécifié, la valeur par défaut est la valeur de **listenport** sur l’ordinateur local.        |
| **listenaddress**  | Spécifie l’adresse IPv4 sur laquelle écouter. Les valeurs acceptables sont l’adresse IP, le nom NetBIOS de l’ordinateur ou le nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local.  |
|    **protocol**    |                                                                                  Spécifie le protocole à utiliser.                                                                                   |

## <a name="add-v6tov4"></a>add v6tov4

Le serveur portproxy écoute les messages envoyés à un port et une adresse IPv6 spécifiques et mappe un port et une adresse IPv4 auxquels envoyer les messages reçus après l’établissement d’une connexion TCP distincte.

### <a name="syntax"></a>Syntaxe

```PowerShell
add v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Spécifie le port IPv6, par numéro de port ou nom de service, sur lequel écouter.                                                            |
| **connectaddress** | Spécifie l’adresse IPv4 à laquelle se connecter. Les valeurs acceptables sont l’adresse IP, le nom NetBIOS de l’ordinateur ou le nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|  **connectport**   |       Spécifie le port IPv4, par numéro de port ou nom de service, auquel se connecter. Si **connectport** n’est pas spécifié, la valeur par défaut est la valeur de **listenport** sur l’ordinateur local.        |
| **listenaddress**  | Spécifie l’adresse IPv6 sur laquelle écouter. Les valeurs acceptables sont l’adresse IP, le nom NetBIOS de l’ordinateur ou le nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local.  |
|    **protocol**    |                                                                                  Spécifie le protocole à utiliser.                                                                                   |

## <a name="add-v6tov6"></a>add v6tov6

Le serveur portproxy écoute les messages envoyés à un port et une adresse IPv6 spécifiques et mappe un port et une adresse IPv6 auxquels envoyer les messages reçus après l’établissement d’une connexion TCP distincte.

### <a name="syntax"></a>Syntaxe

```PowerShell
add v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Spécifie le port IPv6, par numéro de port ou nom de service, sur lequel écouter.                                                            |
| **connectaddress** | Spécifie l’adresse IPv6 à laquelle se connecter. Les valeurs acceptables sont l’adresse IP, le nom NetBIOS de l’ordinateur ou le nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|  **connectport**   |       Spécifie le port IPv6, par numéro de port ou nom de service, auquel se connecter. Si **connectport** n’est pas spécifié, la valeur par défaut est la valeur de **listenport** sur l’ordinateur local.        |
| **listenaddress**  | Spécifie l’adresse IPv6 sur laquelle écouter. Les valeurs acceptables sont l’adresse IP, le nom NetBIOS de l’ordinateur ou le nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local.  |
|    **protocol**    |                                                                                  Spécifie le protocole à utiliser.                                                                                   |

## <a name="delete-v4tov4"></a>delete v4tov4

Le serveur portproxy supprime une adresse IPv4 de la liste des ports et adresses IPv4 qu’il écoute.

### <a name="syntax"></a>Syntaxe

```PowerShell
delete v4tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Spécifie le port IPv4 à supprimer.                                    |
| **listenaddress** | Spécifie l’adresse IPv4 à supprimer. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|   **protocol**    |                                      Spécifie le protocole à utiliser.                                      |

## <a name="delete-v4tov6"></a>delete v4tov6

Le serveur portproxy supprime un port et une adresse IPv4 de la liste des adresses IPv4 qu’il écoute.

### <a name="syntax"></a>Syntaxe

```PowerShell 
delete v4tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Spécifie le port IPv4 à supprimer.                                    |
| **listenaddress** | Spécifie l’adresse IPv4 à supprimer. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|   **protocol**    |                                      Spécifie le protocole à utiliser.                                      |

## <a name="delete-v6tov4"></a>delete v6tov4

Le serveur portproxy supprime un port et une adresse IPv6 de la liste des adresses IPv6 qu’il écoute.

### <a name="syntax"></a>Syntaxe

```PowerShell
delete v6tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Spécifie le port IPv6 à supprimer.                                    |
| **listenaddress** | Spécifie l’adresse IPv6 à supprimer. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|   **protocol**    |                                      Spécifie le protocole à utiliser.                                      |

## <a name="delete-v6tov6"></a>delete v6tov6

Le serveur portproxy supprime une adresse IPv6 de la liste des adresses IPv6 qu’il écoute.

### <a name="syntax"></a>Syntaxe

```PowerShell
delete v6tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Spécifie le port IPv6 à supprimer.                                    |
| **listenaddress** | Spécifie l’adresse IPv6 à supprimer. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|   **protocol**    |                                      Spécifie le protocole à utiliser.                                      |

## <a name="reset"></a>reset

Réinitialise l’état de configuration IPv6.

### <a name="syntax"></a>Syntaxe

`reset`

## <a name="set-v4tov4"></a>set v4tov4

Modifie les valeurs des paramètres d’une entrée existante sur le serveur portproxy créée à l’aide de la commande **add v4tov4** ou ajoute une nouvelle entrée à la liste qui mappe les paires port/adresse.

### <a name="syntax"></a>Syntaxe

```PowerShell
set v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Spécifie le port IPv4, par numéro de port ou nom de service, sur lequel écouter.                                                            |
| **connectaddress** | Spécifie l’adresse IPv4 à laquelle se connecter. Les valeurs acceptables sont l’adresse IP, le nom NetBIOS de l’ordinateur ou le nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|  **connectport**   |       Spécifie le port IPv4, par numéro de port ou nom de service, auquel se connecter. Si **connectport** n’est pas spécifié, la valeur par défaut est la valeur de **listenport** sur l’ordinateur local.        |
| **listenaddress**  | Spécifie l’adresse IPv4 à écouter. Les valeurs acceptables sont l’adresse IP, le nom NetBIOS de l’ordinateur ou le nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|    **protocol**    |                                                                                  Spécifie le protocole à utiliser.                                                                                   |

## <a name="set-v4tov6"></a>set v4tov6

Modifie les valeurs des paramètres d’une entrée existante sur le serveur portproxy créée à l’aide de la commande **add v4tov6** ou ajoute une nouvelle entrée à la liste qui mappe les paires port/adresse.

### <a name="syntax"></a>Syntaxe

```PowerShell
set v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Spécifie le port IPv4, par numéro de port ou nom de service, sur lequel écouter.                                                            |
| **connectaddress** | Spécifie l’adresse IPv6 à laquelle se connecter. Les valeurs acceptables sont l’adresse IP, le nom NetBIOS de l’ordinateur ou le nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|  **connectport**   |       Spécifie le port IPv6, par numéro de port ou nom de service, auquel se connecter. Si **connectport** n’est pas spécifié, la valeur par défaut est la valeur de **listenport** sur l’ordinateur local.        |
| **listenaddress**  | Spécifie l’adresse IPv4 sur laquelle écouter. Les valeurs acceptables sont l’adresse IP, le nom NetBIOS de l’ordinateur ou le nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local.  |
|    **protocol**    |                                                                                  Spécifie le protocole à utiliser.                                                                                   |

## <a name="set-v6tov4"></a>set v6tov4

Modifie les valeurs des paramètres d’une entrée existante sur le serveur portproxy créée à l’aide de la commande **add v6tov4** ou ajoute une nouvelle entrée à la liste qui mappe les paires port/adresse.

### <a name="syntax"></a>Syntaxe

```PowerShell
set v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Spécifie le port IPv6, par numéro de port ou nom de service, sur lequel écouter.                                                            |
| **connectaddress** | Spécifie l’adresse IPv4 à laquelle se connecter. Les valeurs acceptables sont l’adresse IP, le nom NetBIOS de l’ordinateur ou le nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local. |
|  **connectport**   |       Spécifie le port IPv4, par numéro de port ou nom de service, auquel se connecter. Si **connectport** n’est pas spécifié, la valeur par défaut est la valeur de **listenport** sur l’ordinateur local.        |
| **listenaddress**  | Spécifie l’adresse IPv6 sur laquelle écouter. Les valeurs acceptables sont l’adresse IP, le nom NetBIOS de l’ordinateur ou le nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local.  |
|    **protocol**    |                                                                                  Spécifie le protocole à utiliser.                                                                                   |

## <a name="set-v6tov6"></a>set v6tov6

Modifie les valeurs des paramètres d’une entrée existante sur le serveur portproxy créée à l’aide de la commande **add v6tov6** ou ajoute une nouvelle entrée à la liste qui mappe les paires port/adresse.

### <a name="syntax"></a>Syntaxe

```PowerShell 
set v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Paramètres

|                    |                                                                                                                                                                                                    |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                            Spécifie le port IPv6, par numéro de port ou nom de service, sur lequel écouter.                                                            |
| **connectaddress** | Spécifie l’adresse IPv6 à laquelle se connecter. Les valeurs acceptables sont l’adresse IP, le nom NetBIOS de l’ordinateur ou le nom DNS de l’ordinateur. Si aucune adresse n’est spécifiée, la valeur par défaut est l’ordinateur local.  |
|  **connectport**   |        Spécifie le port IPv6, par numéro de port ou nom de service, auquel se connecter. Si **connectport** n’est pas spécifié, la valeur par défaut est la valeur de **listenport** sur l’ordinateur local.        |
| **listenaddress**  | Spécifie l’adresse IPv6 sur laquelle écouter. Les valeurs acceptables sont l’adresse IP, le nom NetBIOS de l’ordinateur ou le nom DNS de l’ordinateur. Si vous ne spécifiez aucune adresse, la valeur par défaut est l’ordinateur local. |
|    **protocol**    |                                                                                   Spécifie le protocole à utiliser.                                                                                   |

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
