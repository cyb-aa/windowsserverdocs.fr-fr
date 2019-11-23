---
title: Commandes netsh pour le protocole HTTP (Hypertext Transfer Protocol)
description: Utilisez netsh http pour interroger et configurer les paramètres et paramètres HTTP. sys.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7b9032eb05128532c8bf90a0db2f685b4435e6eb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401877"
---
# <a name="netsh-http-commands"></a>Commandes Netsh http


Utilisez **netsh http** pour interroger et configurer les paramètres et paramètres http. sys.  

>[!TIP]
>Si vous utilisez Windows PowerShell sur un ordinateur exécutant Windows Server 2016 ou Windows 10, tapez **netsh** , puis appuyez sur entrée. À l’invite netsh, tapez **http** , puis appuyez sur entrée pour accéder à l’invite de commandes netsh http.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;netsh http\>

Les commandes netsh http disponibles sont les suivantes :

- [Ajouter iplisten](#add-iplisten)
- [Ajouter sslcert](#add-sslcert)
- [Ajouter un délai d’attente](#add-timeout)
- [Ajouter urlacl](#add-urlacl)
- [supprimer le cache](#delete-cache)
- [supprimer iplisten](#delete-iplisten)
- [supprimer sslcert](#delete-sslcert)
- [supprimer le délai d’expiration](#delete-timeout)
- [supprimer urlacl](#delete-urlacl)
- [Vider logbuffer](#flush-logbuffer)
- [afficher le cathorax](#show-cachestate)
- [afficher iplisten](#show-iplisten)
- [afficher ServiceState](#show-servicestate)
- [afficher sslcert](#show-sslcert)
- [afficher le délai d’expiration](#show-timeout)
- [afficher urlacl](#show-urlacl)

## <a name="add-iplisten"></a>Ajouter iplisten

Ajoute une nouvelle adresse IP à la liste d’écoute IP, en excluant le numéro de port.

**Syntaxe**

```powershell
add iplisten [ ipaddress= ] IPAddress
```

**Paramètres**

|               |                                                                                                                                                                                                                          |          |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **IPAddress** | Adresse IPv4 ou IPv6 à ajouter à la liste d’écoutes IP. La liste d’écoutes IP est utilisée pour étendre la liste des adresses auxquelles le service HTTP est lié. « 0.0.0.0 » signifie toute adresse IPv4 et «  :: » signifie toute adresse IPv6. | Obligatoire |

---

**Exemples**

Voici quatre exemples de la commande **Add iplisten** .

-   Ajoutez iplisten IPAddress = FE80 :: 1
-   Ajouter iplisten IPAddress = 1.1.1.1
-   Ajouter iplisten IPAddress = 0.0.0.0
-   Ajoutez iplisten IPAddress = ::

---

## <a name="add-sslcert"></a>Ajouter sslcert

Ajoute une nouvelle liaison de certificat de serveur SSL et les stratégies de certificat client correspondantes pour une adresse IP et un port.

**Syntaxe**

```powershell
add sslcert [ ipport= ] IPAddress:port [ certhash= ] CertHash [ appid= ] GUID [ [ certstorename= ] CertStoreName [ verifyclientcertrevocation= ] enable | disable [verifyrevocationwithcachedclientcertonly= ] enable | disable [ usagecheck= ] enable | disable [ revocationfreshnesstime= ] U-Int [ urlretrievaltimeout= ] U-Int [sslctlidentifier= ] SSLCTIdentifier [ sslctlstorename= ] SLCtStoreName [ dsmapperusage= ] enable | disable [ clientcertnegotiation= ] enable | disable ] ]
```

**Paramètres**


|                                              |                                                                                                                                                                                          |          |
|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|                  **ipport**                  |                       Spécifie l’adresse IP et le port de la liaison. Un caractère deux-points ( :) est utilisé comme délimiteur entre l’adresse IP et le numéro de port.                        | Obligatoire |
|                 **certhash**                 |                                     Spécifie le hachage SHA du certificat. Ce hachage a une longueur de 20 octets et est spécifié sous la forme d’une chaîne hexadécimale.                                      | Obligatoire |
|                  **appid**                   |                                                                  Spécifie le GUID pour identifier l’application propriétaire.                                                                  | Obligatoire |
|              **certstorename**               |                                  Spécifie le nom du magasin pour le certificat. La valeur par défaut est MY. Le certificat doit être stocké dans le contexte de l’ordinateur local.                                  | Facultatif |
|        **verifyclientcertrevocation**        |                                                      Spécifie le active/désactive la vérification de la révocation des certificats clients.                                                       | Facultatif |
| **verifyrevocationwithcachedclientcertonly** |                                      Spécifie si l’utilisation du certificat client mis en cache uniquement pour la vérification de la révocation est activée ou désactivée.                                       | Facultatif |
|                **usagecheck**                |                                                      Spécifie si la vérification de l’utilisation est activée ou désactivée. La valeur par défaut est activé.                                                       | Facultatif |
|         **revocationfreshnesstime**          | Spécifie l’intervalle de temps, en secondes, pour rechercher une liste de révocation de certificats (CRL) mise à jour. Si cette valeur est égale à zéro, la nouvelle liste de révocation de certificats est mise à jour uniquement si la précédente expire. | Facultatif |
|           **urlretrievaltimeout**            |                            Spécifie l’intervalle de délai d’attente (en millisecondes) après la tentative de récupération de la liste de révocation de certificats pour l’URL distante.                            | Facultatif |
|             **sslctlidentifier**             |                Spécifie la liste des émetteurs de certificats qui peuvent être approuvés. Cette liste peut être un sous-ensemble des émetteurs de certificats qui sont approuvés par l’ordinateur.                 | Facultatif |
|             **sslctlstorename**              |                                                Spécifie le nom du magasin de certificats sous LOCAL_MACHINE où SslCtlIdentifier est stocké.                                                | Facultatif |
|              **dsmapperusage**               |                                                        Spécifie si les mappeurs DS sont activés ou désactivés. La valeur par défaut est Disabled.                                                         | Facultatif |
|          **clientcertnegotiation**           |                                              Spécifie si la négociation de certificat est activée ou désactivée. La valeur par défaut est Disabled.                                               | Facultatif |

---

**Exemples**

Voici un exemple de la commande **add sslcert** .

add sslcert ipport = 1.1.1.1:443 certhash = 0102030405060708090A0B0C0D0E0F1011121314 AppID = {00112233-4455-6677-8899-AABBCCDDEEFF}

---

## <a name="add-timeout"></a>Ajouter un délai d’attente

Ajoute un délai d’attente global au service.

**Syntaxe** 

```powershell
add timeout [ timeouttype= ] IdleConnectionTimeout | HeaderWaitTimeout [ value=] U-Short
```

**Paramètres**

|                 |                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------|
| **timeouttype** |                                    Type de délai d’expiration pour le paramètre.                                     |
|    **value**    | Valeur du délai d’attente (en secondes). Si la valeur est en notation hexadécimale, ajoutez le préfixe 0x. |

---

**Exemples**

Voici deux exemples de la commande **Add Timeout** .

-   Add Timeout timeouttype = idleconnectiontimeout, value = 120
-   Add Timeout timeouttype = HeaderWaitTimeout value = 0x40

---

## <a name="add-urlacl"></a>Ajouter urlacl

Ajoute une entrée de réservation Uniform Resource Locator (URL). Cette commande réserve l’URL pour les utilisateurs et les comptes non-administrateurs. La liste DACL peut être spécifiée à l’aide d’un nom de compte NT avec les paramètres Listen et Delegate ou à l’aide d’une chaîne SDDL.

**Syntaxe**

```powershell
add urlacl [ url= ] URL [ [user=] User [ [ listen= ] yes | no [ delegate= ] yes | no ] | [ sddl= ] SDDL ]
```

**Paramètres**

|              |                                                                                                                                                  |          |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|   **url**    |                                          Spécifie le Uniform Resource Locator complet (URL).                                           | Obligatoire |
|   **utilisateur**   |                                                      Spécifie le nom d’utilisateur ou de groupe d’utilisateurs                                                       | Obligatoire |
|  **Journal**  | Spécifie l’une des valeurs suivantes : Oui : autoriser l’utilisateur à inscrire des URL. Valeur par défaut. non : interdire à l’utilisateur d’inscrire des URL. | Facultatif |
| **disposer** |  Spécifie l’une des valeurs suivantes : Oui : autoriser l’utilisateur à déléguer des URL non : interdire à l’utilisateur de déléguer des URL. Valeur par défaut.  | Facultatif |
|   **SDDL**   |                                                Spécifie une chaîne SDDL qui décrit la liste DACL.                                                 | Facultatif |

---

**Exemples**

Voici quatre exemples de la commande **add urlacl** .

- add urlacl url =https://+:80/MyUri user = DOMAIN\\user
- Ajouter urlacl url =<https://www.contoso.com:80/MyUri> utilisateur = domaine\\utilisateur écouter = Oui
- add urlacl url =<https://www.contoso.com:80/MyUri> user = DOMAIN\\user Delegate = no
- Ajouter urlacl url =https://+:80/MyUri SDDL =...

---

## <a name="delete-cache"></a>supprimer le cache

Supprime toutes les entrées, ou une entrée spécifiée, du cache URI du noyau du service HTTP.

**Syntaxe**

```powershell
delete cache [ [ url= ] URL [ [recursive= ] yes | no ]
```

**Paramètres**

|               |                                                                                                                              |          |
|---------------|------------------------------------------------------------------------------------------------------------------------------|----------|
|    **url**    |                    Spécifie le Uniform Resource Locator complet (URL) que vous souhaitez supprimer.                     | Facultatif |
| **récursive** | Spécifie si toutes les entrées sous le cache d’URL sont supprimées. **Oui**: supprimer toutes les entrées **non**: ne pas supprimer toutes les entrées | Facultatif |

---

**Exemples**

Voici deux exemples de la commande **Delete cache** .

- supprimer l’URL du cache =<https://www.contoso.com:80/myresource/> récursive = Oui
- supprimer le cache

---

## <a name="delete-iplisten"></a>supprimer iplisten

Supprime une adresse IP de la liste d’écoutes IP. La liste d’écoutes IP est utilisée pour étendre la liste des adresses auxquelles le service HTTP est lié.

**Syntaxe**

```powershell
delete iplisten [ ipaddress= ] IPAddress
```

**Paramètres**

|               |                                                                                                                                                                                                                                                                     |          |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **IPAddress** | Adresse IPv4 ou IPv6 à supprimer de la liste d’écoutes IP. La liste d’écoutes IP est utilisée pour étendre la liste des adresses auxquelles le service HTTP est lié. « 0.0.0.0 » signifie toute adresse IPv4 et «  :: » signifie toute adresse IPv6. Cela n’inclut pas le numéro de port. | Obligatoire |

---


**Exemples**

Voici quatre exemples de la commande **Delete iplisten** .

-   Delete iplisten IPAddress = FE80 :: 1
-   supprimer iplisten IPAddress = 1.1.1.1
-   supprimer iplisten IPAddress = 0.0.0.0
-   supprimer iplisten IPAddress = ::

---

## <a name="delete-sslcert"></a>supprimer sslcert


Supprime les liaisons de certificat de serveur SSL et les stratégies de certificat client correspondantes pour une adresse IP et un port.

**Syntaxe**

```powershell
delete sslcert [ ipport= ] IPAddress:port
```

**Paramètres**

|            |                                                                                                                                                                                          |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | Spécifie l’adresse IPv4 ou IPv6 et le port pour lequel les liaisons de certificat SSL sont supprimées. Un caractère deux-points ( :) est utilisé comme délimiteur entre l’adresse IP et le numéro de port. | Obligatoire |

---


**Exemples**

Voici trois exemples de la commande **Delete sslcert** .

- supprimer sslcert ipport = 1.1.1.1:443
- supprimer sslcert ipport = 0.0.0.0:443
- Delete sslcert ipport = [ ::] : 443

---

## <a name="delete-timeout"></a>supprimer le délai d’expiration

Supprime un délai d’attente global et rend le service rétabli aux valeurs par défaut.

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

Voici deux exemples de la commande de **suppression du délai d’attente** .

-   supprimer Timeout timeouttype = idleconnectiontimeout,
-   supprimer Timeout timeouttype = HeaderWaitTimeout

---

## <a name="delete-urlacl"></a>supprimer urlacl

Supprime les réservations d’URL.

**Syntaxe**

```powershell
delete urlacl [ url= ] URL
```

**Paramètres**

|         |                                                                                       |          |
|---------|---------------------------------------------------------------------------------------|----------|
| **url** | Spécifie le Uniform Resource Locator complet (URL) que vous souhaitez supprimer. | Obligatoire |

---


**Exemples**

Voici deux exemples de la commande **Delete urlacl** .

- supprimer urlacl url =https://+:80/MyUri
- supprimer urlacl url =<https://www.contoso.com:80/MyUri>

---

## <a name="flush-logbuffer"></a>Vider logbuffer

Vide les mémoires tampons internes pour les fichiers journaux.

**Syntaxe**

```powershell
flush logbuffer
```

---

## <a name="show-cachestate"></a>afficher le cathorax

Répertorie les ressources URI mises en cache et leurs propriétés associées. Cette commande répertorie toutes les ressources et leurs propriétés associées qui sont mises en cache dans le cache de réponse HTTP ou affiche une ressource unique et ses propriétés associées.

**Syntaxe**

```powershell
show cachestate [ [url= ] URL]
```

**Paramètres**

|         |                                                                                                                                                    |          |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **url** | Spécifie l’URL complète que vous souhaitez afficher. S’il n’est pas spécifié, affiche toutes les URL. L’URL peut également être un préfixe des URL inscrites. | Facultatif |

---


**Exemples**

Voici deux exemples de la commande **Show cathoracique** :

- afficher l’URL de cathoracique =<https://www.contoso.com:80/myresource>
- afficher le cathorax

---

## <a name="show-iplisten"></a>afficher iplisten

Affiche toutes les adresses IP dans la liste d’écoutes IP. La liste d’écoutes IP est utilisée pour étendre la liste des adresses auxquelles le service HTTP est lié. « 0.0.0.0 » signifie toute adresse IPv4 et «  :: » signifie toute adresse IPv6.

**Syntaxe**

```powershell
show iplisten
```

---

## <a name="show-servicestate"></a>afficher ServiceState

Affiche un instantané du service HTTP.

**Syntaxe**
```powershell
show servicestate [ [ view= ] session | requestq ] [ [ verbose= ] yes | no ]
```

**Paramètres**

|             |                                                                                                                      |          |
|-------------|----------------------------------------------------------------------------------------------------------------------|----------|
|  **Affichage**   | Spécifie s’il faut afficher un instantané de l’état du service HTTP en fonction de la session du serveur ou des files d’attente des demandes. | Facultatif |
| **Verbose** |                Spécifie s’il faut afficher des informations détaillées qui affichent également les informations de propriété.                | Facultatif |

---

**Exemples**

Voici deux exemples de la commande **Show ServiceState** .

-   Show ServiceState View = "session"
-   Show ServiceState View = "requestq"

---

## <a name="show-sslcert"></a>afficher sslcert

Affiche les liaisons de certificat de serveur protocole SSL (SSL) et les stratégies de certificat client correspondantes pour une adresse IP et un port.

**Syntaxe**

```powershell
show sslcert [ ipport= ] IPAddress:port
```

**Paramètres**

|            |                                                                                                                                                                                                                                                |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | Spécifie l’adresse IPv4 ou IPv6 et le port que les liaisons de certificat SSL affichent. Un caractère deux-points ( :) est utilisé comme délimiteur entre l’adresse IP et le numéro de port. Si vous ne spécifiez pas ipport, toutes les liaisons sont affichées. | Obligatoire |

---


**Exemples**

Voici cinq exemples de la commande **Show sslcert** .

-   Show sslcert ipport = [fe80 :: 1] : 443
-   Show sslcert ipport = 1.1.1.1:443
-   Show sslcert ipport = 0.0.0.0:443
-   Show sslcert ipport = [ ::] : 443
-   afficher sslcert

---

## <a name="show-timeout"></a>afficher le délai d’expiration

Affiche, en secondes, les valeurs de délai d’attente du service HTTP.

**Syntaxe**

```powershell
show timeout
```

---

## <a name="show-urlacl"></a>afficher urlacl

Affiche des listes de contrôle d’accès discrétionnaire (DACL) pour l’URL réservée spécifiée ou pour toutes les URL réservées.

**Syntaxe**

```powershell
show urlacl [ [url= ] URL]
```

**Paramètres**

|         |                                                                                                |          |
|---------|------------------------------------------------------------------------------------------------|----------|
| **url** | Spécifie l’URL complète que vous souhaitez afficher. S’il n’est pas spécifié, affiche toutes les URL. | Facultatif |

---


**Exemples**

Voici trois exemples de la commande **Show urlacl** .

- afficher urlacl url =https://+:80/MyUri
- afficher urlacl url =<https://www.contoso.com:80/MyUri>
- afficher urlacl

---