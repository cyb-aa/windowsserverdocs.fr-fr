---
title: Commandes netsh pour le protocole HTTP (Hypertext Transfer Protocol)
description: Utilisez netsh http pour interroger et configurer les paramètres HTTP.sys.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7b9032eb05128532c8bf90a0db2f685b4435e6eb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401877"
---
# <a name="netsh-http-commands"></a>Commandes Netsh http


Utilisez **netsh http** pour interroger et configurer les paramètres HTTP.sys.  

>[!TIP]
>Si vous utilisez Windows PowerShell sur un ordinateur exécutant Windows Server 2016 ou Windows 10, tapez **netsh** et appuyez sur Entrée. Depuis l’invite netsh, tapez **http** et appuyez sur Entrée pour accéder à l’invite netsh http.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;netsh http\>

Les commandes netsh http disponibles sont les suivantes :

- [add iplisten](#add-iplisten)
- [add sslcert](#add-sslcert)
- [add timeout](#add-timeout)
- [add urlacl](#add-urlacl)
- [delete cache](#delete-cache)
- [delete iplisten](#delete-iplisten)
- [delete sslcert](#delete-sslcert)
- [delete timeout](#delete-timeout)
- [delete urlacl](#delete-urlacl)
- [flush logbuffer](#flush-logbuffer)
- [show cachestate](#show-cachestate)
- [show iplisten](#show-iplisten)
- [show servicestate](#show-servicestate)
- [show sslcert](#show-sslcert)
- [show timeout](#show-timeout)
- [show urlacl](#show-urlacl)

## <a name="add-iplisten"></a>add iplisten

Ajoute une nouvelle adresse IP à la liste d’écoutes IP, à l’exclusion du numéro de port.

**Syntaxe**

```powershell
add iplisten [ ipaddress= ] IPAddress
```

**Paramètres**

|               |                                                                                                                                                                                                                          |          |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipaddress** | Adresse IPv4 ou IPv6 à ajouter à la liste d’écoutes IP. La liste d’écoutes IP est utilisée pour définir l’étendue de la liste des adresses auxquelles le service HTTP est lié. « 0.0.0.0 » signifie toute adresse IPv4, tandis que « :: » signifie toute adresse IPv6. | Obligatoire |

---

**Exemples**

Voici quatre exemples de la commande **add iplisten**.

-   add iplisten ipaddress=fe80::1
-   add iplisten ipaddress=1.1.1.1
-   add iplisten ipaddress=0.0.0.0
-   add iplisten ipaddress=::

---

## <a name="add-sslcert"></a>add sslcert

Ajoute une nouvelle liaison de certificat de serveur SSL et les stratégies de certificat client correspondantes pour une adresse IP et un port.

**Syntaxe**

```powershell
add sslcert [ ipport= ] IPAddress:port [ certhash= ] CertHash [ appid= ] GUID [ [ certstorename= ] CertStoreName [ verifyclientcertrevocation= ] enable | disable [verifyrevocationwithcachedclientcertonly= ] enable | disable [ usagecheck= ] enable | disable [ revocationfreshnesstime= ] U-Int [ urlretrievaltimeout= ] U-Int [sslctlidentifier= ] SSLCTIdentifier [ sslctlstorename= ] SLCtStoreName [ dsmapperusage= ] enable | disable [ clientcertnegotiation= ] enable | disable ] ]
```

**Paramètres**


|                                              |                                                                                                                                                                                          |          |
|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|                  **ipport**                  |                       Spécifie l’adresse IP et le port de la liaison. Un caractère deux-points (:) est utilisé comme délimiteur entre l’adresse IP et le numéro de port.                        | Obligatoire |
|                 **certhash**                 |                                     Spécifie le hachage SHA du certificat. Ce hachage a une longueur de 20 octets et est spécifié sous la forme d’une chaîne hexadécimale.                                      | Obligatoire |
|                  **appid**                   |                                                                  Spécifie le GUID permettant d’identifier l’application propriétaire.                                                                  | Obligatoire |
|              **certstorename**               |                                  Spécifie le nom du stockage pour le certificat. Défini par défaut sur MY. Le certificat doit être stocké dans le contexte de l’ordinateur local.                                  | Facultatif |
|        **verifyclientcertrevocation**        |                                                      Spécifie la vérification de la révocation des certificats clients (activation/désactivation).                                                       | Facultatif |
| **verifyrevocationwithcachedclientcertonly** |                                      Spécifie si l’utilisation du certificat client mis en cache uniquement pour la vérification de la révocation est activée ou désactivée.                                       | Facultatif |
|                **usagecheck**                |                                                      Spécifie si la vérification de l’utilisation est activée ou désactivée. Par défaut, l’utilisation est activée.                                                       | Facultatif |
|         **revocationfreshnesstime**          | Spécifie l’intervalle de temps, en secondes, pour rechercher une liste de révocation de certificats (CRL) mise à jour. Si cette valeur est égale à zéro, la nouvelle liste de révocation de certificats est mise à jour uniquement si la précédente expire. | Facultatif |
|           **urlretrievaltimeout**            |                            Spécifie l’intervalle de délai d’attente (en millisecondes) après la tentative de récupération de la liste de révocation de certificats pour l’URL distante.                            | Facultatif |
|             **sslctlidentifier**             |                Spécifie la liste des émetteurs de certificat qui peuvent être approuvés. Cette liste peut être un sous-ensemble des émetteurs de certificats qui sont approuvés par l’ordinateur.                 | Facultatif |
|             **sslctlstorename**              |                                                Spécifie le nom du magasin de certificats sous LOCAL_MACHINE où SslCtlIdentifier est stocké.                                                | Facultatif |
|              **dsmapperusage**               |                                                        Spécifie si les mappeurs DS sont activés ou désactivés. Par défaut, ils sont désactivés.                                                         | Facultatif |
|          **clientcertnegotiation**           |                                              Spécifie si la négociation de certificat est activée ou désactivée. Par défaut, elle est désactivée.                                               | Facultatif |

---

**Exemples**

Voici un exemple de la commande **add sslcert**.

add sslcert ipport=1.1.1.1:443 certhash=0102030405060708090A0B0C0D0E0F1011121314 appid={00112233-4455-6677-8899- AABBCCDDEEFF}

---

## <a name="add-timeout"></a>add timeout

Ajoute un délai d’attente global au service.

**Syntaxe** 

```powershell
add timeout [ timeouttype= ] IdleConnectionTimeout | HeaderWaitTimeout [ value=] U-Short
```

**Paramètres**

|                 |                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------|
| **timeouttype** |                                    Type de délai d’attente pour le paramètre.                                     |
|    **value**    | Valeur du délai d’attente (en secondes). Si la valeur est en notation hexadécimale, ajoutez le préfixe 0x. |

---

**Exemples**

Voici deux exemples de la commande **add timeout**.

-   add timeout timeouttype=idleconnectiontimeout value=120
-   add timeout timeouttype=headerwaittimeout value=0x40

---

## <a name="add-urlacl"></a>add urlacl

Ajoute une entrée de réservation d’URL (Uniform Resource Locator). Cette commande réserve l’URL pour les comptes et les utilisateurs non-administrateurs. La liste DACL peut être spécifiée à l’aide d’un nom de compte NT avec les paramètres listen et delegate ou à l’aide d’une chaîne SDDL.

**Syntaxe**

```powershell
add urlacl [ url= ] URL [ [user=] User [ [ listen= ] yes | no [ delegate= ] yes | no ] | [ sddl= ] SDDL ]
```

**Paramètres**

|              |                                                                                                                                                  |          |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|   **url**    |                                          Spécifie l’URL (Uniform Resource Locator) complète.                                           | Obligatoire |
|   **user**   |                                                      Spécifie le nom d’utilisateur ou de groupe d’utilisateurs                                                       | Obligatoire |
|  **listen**  | Spécifie l’une des valeurs suivantes : yes : autoriser l’utilisateur à inscrire des URL. Il s'agit de la valeur par défaut. no : interdire à l’utilisateur d’inscrire des URL. | Facultatif |
| **delegate** |  Spécifie l’une des valeurs suivantes : yes : autoriser l’utilisateur à déléguer des URL. no : interdire à l’utilisateur de déléguer des URL. Il s'agit de la valeur par défaut.  | Facultatif |
|   **sddl**   |                                                Spécifie une chaîne SDDL qui décrit la liste DACL.                                                 | Facultatif |

---

**Exemples**

Voici quatre exemples de la commande **add urlacl**.

- add urlacl url=https://+:80/MyUri user=DOMAIN\\ user
- add urlacl url=<https://www.contoso.com:80/MyUri> user=DOMAIN\\user listen=yes
- add urlacl url=<https://www.contoso.com:80/MyUri> user=DOMAIN\\user delegate=no
- add urlacl url=https://+:80/MyUri sddl=...

---

## <a name="delete-cache"></a>delete cache

Supprime toutes les entrées, ou une entrée spécifiée, du cache URI du noyau du service HTTP.

**Syntaxe**

```powershell
delete cache [ [ url= ] URL [ [recursive= ] yes | no ]
```

**Paramètres**

|               |                                                                                                                              |          |
|---------------|------------------------------------------------------------------------------------------------------------------------------|----------|
|    **url**    |                    Spécifie l’URL (Uniform Resource Locator) complète à supprimer.                     | Facultatif |
| **recursive** | Spécifie si toutes les entrées sous le cache d’URL sont supprimées. **yes** : supprimer toutes les entrées. **no** : ne pas supprimer toutes les entrées | Facultatif |

---

**Exemples**

Voici deux exemples de la commande **delete cache**.

- delete cache url=<https://www.contoso.com:80/myresource/> recursive=yes
- delete cache

---

## <a name="delete-iplisten"></a>delete iplisten

Supprime une adresse IP de la liste d’écoutes IP. La liste d’écoutes IP est utilisée pour définir l’étendue de la liste des adresses auxquelles le service HTTP est lié.

**Syntaxe**

```powershell
delete iplisten [ ipaddress= ] IPAddress
```

**Paramètres**

|               |                                                                                                                                                                                                                                                                     |          |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipaddress** | Adresse IPv4 ou IPv6 à supprimer de la liste d’écoutes IP. La liste d’écoutes IP est utilisée pour définir l’étendue de la liste des adresses auxquelles le service HTTP est lié. « 0.0.0.0 » signifie toute adresse IPv4, tandis que « :: » signifie toute adresse IPv6. Le numéro de port n’est pas inclus. | Obligatoire |

---


**Exemples**

Voici quatre exemples de la commande **delete iplisten**.

-   delete iplisten ipaddress=fe80::1
-   delete iplisten ipaddress=1.1.1.1
-   delete iplisten ipaddress=0.0.0.0
-   delete iplisten ipaddress=::

---

## <a name="delete-sslcert"></a>delete sslcert


Supprime les liaisons de certificat de serveur SSL et les stratégies de certificat client correspondantes pour une adresse IP et un port.

**Syntaxe**

```powershell
delete sslcert [ ipport= ] IPAddress:port
```

**Paramètres**

|            |                                                                                                                                                                                          |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | Spécifie l’adresse IPv4 ou IPv6 et le port pour lesquels les liaisons de certificat SSL sont supprimées. Un caractère deux-points (:) est utilisé comme délimiteur entre l’adresse IP et le numéro de port. | Obligatoire |

---


**Exemples**

Voici trois exemples de la commande **delete sslcert**.

- delete sslcert ipport=1.1.1.1:443
- delete sslcert ipport=0.0.0.0:443
- delete sslcert ipport=[::]:443

---

## <a name="delete-timeout"></a>delete timeout

Supprime un délai d’attente global et rétablit les valeurs par défaut du service.

**Syntaxe**

```powershell
delete timeout [ timeouttype= ] idleconnectiontimeout | headerwaittimeout
```

**Paramètres**

|                 |                                        |          |
|-----------------|----------------------------------------|----------|
| **timeouttype** | Spécifie le type de paramètre de délai d’attente. | Obligatoire |

---


**Exemples**

Voici deux exemples de la commande **delete timeout**.

-   delete timeout timeouttype=idleconnectiontimeout
-   delete timeout timeouttype=headerwaittimeout

---

## <a name="delete-urlacl"></a>delete urlacl

Supprime les réservations d’URL.

**Syntaxe**

```powershell
delete urlacl [ url= ] URL
```

**Paramètres**

|         |                                                                                       |          |
|---------|---------------------------------------------------------------------------------------|----------|
| **url** | Spécifie l’URL (Uniform Resource Locator) complète à supprimer. | Obligatoire |

---


**Exemples**

Voici deux exemples de la commande **delete urlacl**.

- delete urlacl url=https://+:80/MyUri
- delete urlacl url=<https://www.contoso.com:80/MyUri>

---

## <a name="flush-logbuffer"></a>flush logbuffer

Vide les mémoires tampons internes pour les fichiers journaux.

**Syntaxe**

```powershell
flush logbuffer
```

---

## <a name="show-cachestate"></a>show cachestate

Liste les ressources d’URI mises en cache et leurs propriétés associées. Cette commande liste toutes les ressources et leurs propriétés associées qui sont placées dans le cache de réponse HTTP ou affiche une ressource unique et ses propriétés associées.

**Syntaxe**

```powershell
show cachestate [ [url= ] URL]
```

**Paramètres**

|         |                                                                                                                                                    |          |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **url** | Spécifie l’URL complète que vous souhaitez afficher. Si ce paramètre n’est pas spécifié, affiche toutes les URL. L’URL peut également être un préfixe des URL inscrites. | Facultatif |

---


**Exemples**

Voici deux exemples de la commande **show cachestate** :

- show cachestate url=<https://www.contoso.com:80/myresource>
- show cachestate

---

## <a name="show-iplisten"></a>show iplisten

Affiche toutes les adresses IP dans la liste d’écoutes IP. La liste d’écoutes IP est utilisée pour définir l’étendue de la liste des adresses auxquelles le service HTTP est lié. « 0.0.0.0 » signifie toute adresse IPv4, tandis que « :: » signifie toute adresse IPv6.

**Syntaxe**

```powershell
show iplisten
```

---

## <a name="show-servicestate"></a>show servicestate

Affiche un instantané du service HTTP.

**Syntaxe**
```powershell
show servicestate [ [ view= ] session | requestq ] [ [ verbose= ] yes | no ]
```

**Paramètres**

|             |                                                                                                                      |          |
|-------------|----------------------------------------------------------------------------------------------------------------------|----------|
|  **Affichage**   | Spécifie s’il faut afficher un instantané de l’état du service HTTP en fonction de la session du serveur ou des files d’attente des demandes. | Facultatif |
| **Verbose** |                Spécifie s’il faut afficher des informations détaillées qui portent également sur les propriétés.                | Facultatif |

---

**Exemples**

Voici deux exemples de la commande **show servicestate**.

-   show servicestate view="session"
-   show servicestate view="requestq"

---

## <a name="show-sslcert"></a>show sslcert

Affiche les liaisons de certificat de serveur SSL (Secure Sockets Layer) et les stratégies de certificat client correspondantes pour une adresse IP et un port.

**Syntaxe**

```powershell
show sslcert [ ipport= ] IPAddress:port
```

**Paramètres**

|            |                                                                                                                                                                                                                                                |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | Spécifie l’adresse IPv4 ou IPv6 et le port pour lesquels afficher les liaisons de certificat SSL. Un caractère deux-points (:) est utilisé comme délimiteur entre l’adresse IP et le numéro de port. Si vous ne spécifiez pas ipport, toutes les liaisons sont affichées. | Obligatoire |

---


**Exemples**

Voici cinq exemples de la commande **show sslcert**.

-   show sslcert ipport=[fe80::1]:443
-   show sslcert ipport=1.1.1.1:443
-   show sslcert ipport=0.0.0.0:443
-   show sslcert ipport=[::]:443
-   show sslcert

---

## <a name="show-timeout"></a>show timeout

Affiche, en secondes, les valeurs de délai d’attente du service HTTP.

**Syntaxe**

```powershell
show timeout
```

---

## <a name="show-urlacl"></a>show urlacl

Affiche les listes de contrôle d’accès discrétionnaire (DACL) pour l’URL réservée spécifiée ou pour toutes les URL réservées.

**Syntaxe**

```powershell
show urlacl [ [url= ] URL]
```

**Paramètres**

|         |                                                                                                |          |
|---------|------------------------------------------------------------------------------------------------|----------|
| **url** | Spécifie l’URL complète que vous souhaitez afficher. Si ce paramètre n’est pas spécifié, affiche toutes les URL. | Facultatif |

---


**Exemples**

Voici trois exemples de la commande **show urlacl**.

- show urlacl url=https://+:80/MyUri
- show urlacl url=<https://www.contoso.com:80/MyUri>
- show urlacl

---