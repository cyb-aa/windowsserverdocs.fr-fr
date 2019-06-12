---
title: Commandes Netsh pour Hypertext Transfer Protocol (HTTP)
description: Utiliser netsh http pour interroger et de configurer les paramètres et les paramètres de HTTP.sys.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3c5f3927abf1a2394c2dd5b8ea664c7de8d5f614
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446195"
---
# <a name="netsh-http-commands"></a>Commandes Netsh http


Utilisez **netsh http** pour interroger et de configurer les paramètres de HTTP.sys et des paramètres.  

>[!TIP]
>Si vous utilisez Windows PowerShell sur un ordinateur exécutant Windows Server 2016 ou Windows 10, tapez **netsh** et appuyez sur ENTRÉE. À l’invite netsh, tapez **http** et appuyez sur ENTRÉE pour obtenir de l’invite de commandes netsh http.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;netsh http\>

Les commandes de http netsh disponibles sont :

- [add iplisten](#add-iplisten)
- [add sslcert](#add-sslcert)
- [ajouter le délai d’attente](#add-timeout)
- [Ajouter urlacl](#add-urlacl)
- [delete cache](#delete-cache)
- [delete iplisten](#delete-iplisten)
- [supprimer sslcert](#delete-sslcert)
- [supprimer le délai d’attente](#delete-timeout)
- [supprimer les urlacl](#delete-urlacl)
- [Vider logbuffer](#flush-logbuffer)
- [Afficher cachestate](#show-cachestate)
- [Afficher iplisten](#show-iplisten)
- [Afficher servicestate](#show-servicestate)
- [Afficher sslcert](#show-sslcert)
- [afficher le délai d’attente](#show-timeout)
- [afficher les urlacl](#show-urlacl)

## <a name="add-iplisten"></a>Ajouter iplisten

Ajoute une nouvelle adresse IP à la liste d’écoutes IP, à l’exclusion du numéro de port.

**Syntaxe**

```powershell
add iplisten [ ipaddress= ] IPAddress
```

**Paramètres**

|               |                                                                                                                                                                                                                          |          |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipaddress** | Liste d’écoutes l’adresse IPv4 ou IPv6 à ajouter à l’adresse IP. La liste d’écoutes IP est utilisée pour étendre la liste d’adresses auquel le service HTTP est liée. « 0.0.0.0 » signifie que n’importe quelle adresse IPv4 et « :: » signifie que n’importe quelle adresse IPv6. | Obligatoire |

---

**Exemples**

Voici quatre exemples de la **ajouter iplisten** commande.

-   Ajouter iplisten ipaddress = fe80::1
-   Ajouter iplisten ipaddress = 1.1.1.1
-   Ajouter iplisten ipaddress = 0.0.0.0
-   Ajouter iplisten ipaddress = ::

---

## <a name="add-sslcert"></a>Ajouter sslcert

Ajoute un nouveau certificat de serveur SSL de liaison et correspondant des stratégies de certificat client pour une adresse IP et le port.

**Syntaxe**

```powershell
add sslcert [ ipport= ] IPAddress:port [ certhash= ] CertHash [ appid= ] GUID [ [ certstorename= ] CertStoreName [ verifyclientcertrevocation= ] enable | disable [verifyrevocationwithcachedclientcertonly= ] enable | disable [ usagecheck= ] enable | disable [ revocationfreshnesstime= ] U-Int [ urlretrievaltimeout= ] U-Int [sslctlidentifier= ] SSLCTIdentifier [ sslctlstorename= ] SLCtStoreName [ dsmapperusage= ] enable | disable [ clientcertnegotiation= ] enable | disable ] ]
```

**Paramètres**


|                                              |                                                                                                                                                                                          |          |
|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|                  **ipport**                  |                       Spécifie l’adresse IP et le port pour la liaison. Un signe deux-points ( :)) est utilisé comme un séparateur entre l’adresse IP et le numéro de port.                        | Obligatoire |
|                 **certhash**                 |                                     Spécifie le hachage SHA du certificat. Ce hachage est de 20 octets et est spécifié sous forme de chaîne hexadécimale.                                      | Obligatoire |
|                  **appid**                   |                                                                  Spécifie le GUID pour identifier l’application propriétaire.                                                                  | Obligatoire |
|              **certstorename**               |                                  Spécifie le nom du magasin du certificat. La valeur par défaut est MY. Certificat doit être stocké dans le contexte de l’ordinateur local.                                  | Facultatif |
|        **verifyclientcertrevocation**        |                                                      Spécifie l’Active ou désactive la vérification de révocation des certificats clients.                                                       | Facultatif |
| **verifyrevocationwithcachedclientcertonly** |                                      Spécifie si l’utilisation du certificat du client mis en cache uniquement pour la vérification de révocation est activée ou désactivée.                                       | Facultatif |
|                **usagecheck**                |                                                      Spécifie si la vérification de l’utilisation est activée ou désactivée. Valeur par défaut est activé.                                                       | Facultatif |
|         **revocationfreshnesstime**          | Spécifie l’intervalle de temps, en secondes, pour rechercher une liste de révocation de certificats mis à jour (CRL). Si cette valeur est zéro, puis la nouvelle liste de révocation est mis à jour uniquement si le précédent arrive à expiration. | Facultatif |
|           **urlretrievaltimeout**            |                            Spécifie l’intervalle de délai d’attente (en millisecondes) après la tentative d’extraction de la liste de révocation de certificat pour l’URL distante.                            | Facultatif |
|             **sslctlidentifier**             |                Spécifie la liste des émetteurs de certificats de confiance. Cette liste peut être un sous-ensemble des émetteurs de certificats approuvés par l’ordinateur.                 | Facultatif |
|             **sslctlstorename**              |                                                Spécifie le nom de magasin de certificat sous ordinateur_local où SslCtlIdentifier est stocké.                                                | Facultatif |
|              **dsmapperusage**               |                                                        Spécifie si les mappeurs DS est activé ou désactivé. Par défaut est désactivé.                                                         | Facultatif |
|          **clientcertnegotiation**           |                                              Spécifie si la négociation de certificat est activée ou désactivée. Par défaut est désactivé.                                               | Facultatif |

---

**Exemples**

Voici un exemple de la **ajouter sslcert** commande.

Ajouter sslcert ipport = 1.1.1.1:443 certhash = 0102030405060708090A0B0C0D0E0F1011121314 appid = {00112233-4455-6677-8899-AABBCCDDEEFF}

---

## <a name="add-timeout"></a>ajouter le délai d’attente

Ajoute un délai d’expiration global au service.

**Syntaxe** 

```powershell
add timeout [ timeouttype= ] IdleConnectionTimeout | HeaderWaitTimeout [ value=] U-Short
```

**Paramètres**

|                 |                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------|
| **timeouttype** |                                    Type de délai d’attente pour le paramètre.                                     |
|    **value**    | Valeur d’expiration (en secondes). Si la valeur est en notation hexadécimale, puis ajouter le préfixe 0 x. |

---

**Exemples**

Voici deux exemples de la **ajouter délai d’attente** commande.

-   add timeout timeouttype=idleconnectiontimeout value=120
-   add timeout timeouttype=headerwaittimeout value=0x40

---

## <a name="add-urlacl"></a>Ajouter urlacl

Ajoute une entrée de réservation d’URL Uniform Resource Locator (). Cette commande réserve l’URL pour les comptes et les utilisateurs non administrateurs. La liste DACL peut être spécifiée à l’aide d’un nom de compte NT avec les paramètres d’écoute et le délégué ou en utilisant une chaîne SDDL.

**Syntaxe**

```powershell
add urlacl [ url= ] URL [ [user=] User [ [ listen= ] yes | no [ delegate= ] yes | no ] | [ sddl= ] SDDL ]
```

**Paramètres**

|              |                                                                                                                                                  |          |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|   **url**    |                                          Spécifie le complet URL Uniform Resource Locator ().                                           | Obligatoire |
|   **user**   |                                                      Spécifie le nom d’utilisateur ou groupe d’utilisateurs                                                       | Obligatoire |
|  **listen**  | Spécifie une des valeurs suivantes : Oui : Autoriser l’utilisateur à inscrire l’URL. Valeur par défaut. non : Refuser l’utilisateur à partir de l’inscription d’URL. | Facultatif |
| **delegate** |  Spécifie une des valeurs suivantes : Oui : Autoriser l’utilisateur à déléguer ne l’URL : Refuser l’utilisateur à partir de la délégation de l’URL. Valeur par défaut.  | Facultatif |
|   **sddl**   |                                                Spécifie une chaîne SDDL qui décrit la liste DACL.                                                 | Facultatif |

---

**Exemples**

Voici quatre exemples de la **ajouter urlacl** commande.

- Ajouter urlacl url =https://+:80/MyUri utilisateur = DOMAIN\\utilisateur
- Ajouter urlacl url =<https://www.contoso.com:80/MyUri> utilisateur = DOMAIN\\utilisateur écoute = yes
- Ajouter urlacl url =<https://www.contoso.com:80/MyUri> utilisateur = DOMAIN\\délégué d’utilisateur = non
- Ajouter urlacl url =https://+:80/MyUri sddl =...

---

## <a name="delete-cache"></a>supprimer le cache

Supprime toutes les entrées, ou une entrée spécifiée, le noyau du service HTTP cache URI.

**Syntaxe**

```powershell
delete cache [ [ url= ] URL [ [recursive= ] yes | no ]
```

**Paramètres**

|               |                                                                                                                              |          |
|---------------|------------------------------------------------------------------------------------------------------------------------------|----------|
|    **url**    |                    Spécifie le complet URL Uniform Resource Locator () que vous souhaitez supprimer.                     | Facultatif |
| **recursive** | Spécifie si toutes les entrées dans le cache d’url sont supprimées. **Oui**: supprimer toutes les entrées **aucune**: ne pas supprimer toutes les entrées | Facultatif |

---

**Exemples**

Voici deux exemples de la **supprimer cache** commande.

- supprimer le cache url =<https://www.contoso.com:80/myresource/> récursive = yes
- supprimer le cache

---

## <a name="delete-iplisten"></a>delete iplisten

Supprime une adresse IP à partir de la liste d’écoutes IP. La liste d’écoutes IP est utilisée pour étendre la liste d’adresses auquel le service HTTP est liée.

**Syntaxe**

```powershell
delete iplisten [ ipaddress= ] IPAddress
```

**Paramètres**

|               |                                                                                                                                                                                                                                                                     |          |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipaddress** | Liste d’écoutes l’adresse IPv4 ou IPv6 doit être supprimé à partir de l’adresse IP. La liste d’écoutes IP est utilisée pour étendre la liste d’adresses auquel le service HTTP est liée. « 0.0.0.0 » signifie que n’importe quelle adresse IPv4 et « :: » signifie que n’importe quelle adresse IPv6. Cela n’inclut pas le numéro de port. | Obligatoire |

---


**Exemples**

Voici quatre exemples de la **delete iplisten** commande.

-   delete iplisten ipaddress = fe80::1
-   delete iplisten ipaddress = 1.1.1.1
-   delete iplisten ipaddress = 0.0.0.0
-   delete iplisten ipaddress = ::

---

## <a name="delete-sslcert"></a>supprimer sslcert


Supprime les liaisons de certificat de serveur SSL et des stratégies de certificat de client correspondantes pour une adresse IP et le port.

**Syntaxe**

```powershell
delete sslcert [ ipport= ] IPAddress:port
```

**Paramètres**

|            |                                                                                                                                                                                          |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | Spécifie l’adresse IPv4 ou IPv6 et le port pour laquelle obtient supprimés les liaisons de certificat SSL. Un signe deux-points ( :)) est utilisé comme un séparateur entre l’adresse IP et le numéro de port. | Obligatoire |

---


**Exemples**

Voici trois exemples de la **supprimer sslcert** commande.

- delete sslcert ipport=1.1.1.1:443
- supprimer sslcert ipport = 0.0.0.0 : 443
- delete sslcert ipport=[::]:443

---

## <a name="delete-timeout"></a>supprimer le délai d’attente

Supprime un délai d’expiration global et rend le service de rétablir les valeurs par défaut.

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

Voici deux exemples de la **supprimer le délai d’attente** commande.

-   delete timeout timeouttype=idleconnectiontimeout
-   delete timeout timeouttype=headerwaittimeout

---

## <a name="delete-urlacl"></a>supprimer les urlacl

Supprime les réservations d’URL.

**Syntaxe**

```powershell
delete urlacl [ url= ] URL
```

**Paramètres**

|         |                                                                                       |          |
|---------|---------------------------------------------------------------------------------------|----------|
| **url** | Spécifie le complet URL Uniform Resource Locator () que vous souhaitez supprimer. | Obligatoire |

---


**Exemples**

Voici deux exemples de la **supprimer urlacl** commande.

- supprimer les urlacl url =https://+:80/MyUri
- supprimer les urlacl url =<https://www.contoso.com:80/MyUri>

---

## <a name="flush-logbuffer"></a>Vider logbuffer

Vide les mémoires tampon internes pour les fichiers journaux.

**Syntaxe**

```powershell
flush logbuffer
```

---

## <a name="show-cachestate"></a>Afficher cachestate

Listes de mises en cache les ressources URI et leurs propriétés associées. Cette commande répertorie toutes les ressources et leurs propriétés associées qui sont mis en cache dans le cache de réponse HTTP ou affiche une seule ressource et ses propriétés associées.

**Syntaxe**

```powershell
show cachestate [ [url= ] URL]
```

**Paramètres**

|         |                                                                                                                                                    |          |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **url** | Spécifie l’URL qualifiée complète que vous souhaitez afficher. Si non spécifié, afficher toutes les URL. L’URL peut également être un préfixe d’URL inscrites. | Facultatif |

---


**Exemples**

Voici deux exemples de la **afficher cachestate** commande :

- afficher l’url de cachestate =<https://www.contoso.com:80/myresource>
- Afficher cachestate

---

## <a name="show-iplisten"></a>Afficher iplisten

Affiche toutes les adresses IP dans la liste d’écoutes IP. La liste d’écoutes IP est utilisée pour étendre la liste d’adresses auquel le service HTTP est liée. « 0.0.0.0 » signifie que n’importe quelle adresse IPv4 et « :: » signifie que n’importe quelle adresse IPv6.

**Syntaxe**

```powershell
show iplisten
```

---

## <a name="show-servicestate"></a>Afficher servicestate

Affiche un instantané du service HTTP.

**Syntaxe**
```powershell
show servicestate [ [ view= ] session | requestq ] [ [ verbose= ] yes | no ]
```

**Paramètres**

|             |                                                                                                                      |          |
|-------------|----------------------------------------------------------------------------------------------------------------------|----------|
|  **Affichage**   | Spécifie s’il faut afficher un instantané de l’état du service HTTP basée sur la session de serveur ou sur les files d’attente de la demande. | Facultatif |
| **Verbose** |                Spécifie s’il faut afficher des informations détaillées qui affiche également des informations de propriété.                | Facultatif |

---

**Exemples**

Voici deux exemples de la **afficher servicestate** commande.

-   afficher la vue de servicestate = « session »
-   afficher la vue de servicestate = « requestq »

---

## <a name="show-sslcert"></a>Afficher sslcert

Affiche les liaisons de certificat de serveur Secure Sockets Layer (SSL) et des stratégies de certificat de client correspondantes pour une adresse IP et le port.

**Syntaxe**

```powershell
show sslcert [ ipport= ] IPAddress:port
```

**Paramètres**

|            |                                                                                                                                                                                                                                                |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | Spécifie l’adresse IPv4 ou IPv6 et le port pour lequel le certificat SSL affichage de liaisons. Un signe deux-points ( :)) est utilisé comme un séparateur entre l’adresse IP et le numéro de port. Si vous ne spécifiez pas ipport, toutes les liaisons sont affichés. | Obligatoire |

---


**Exemples**

Voici cinq exemples illustrant la **afficher sslcert** commande.

-   show sslcert ipport=[fe80::1]:443
-   show sslcert ipport=1.1.1.1:443
-   Afficher sslcert ipport = 0.0.0.0 : 443
-   show sslcert ipport=[::]:443
-   Afficher sslcert

---

## <a name="show-timeout"></a>afficher le délai d’attente

Indique, en secondes, les valeurs de délai d’attente du service HTTP.

**Syntaxe**

```powershell
show timeout
```

---

## <a name="show-urlacl"></a>afficher les urlacl

Affiche le contrôle d’accès discrétionnaire répertorie (DACL) pour l’URL réservée spécifiée ou toutes les URL réservées.

**Syntaxe**

```powershell
show urlacl [ [url= ] URL]
```

**Paramètres**

|         |                                                                                                |          |
|---------|------------------------------------------------------------------------------------------------|----------|
| **url** | Spécifie l’URL qualifiée complète que vous souhaitez afficher. Si non spécifié, affiche toutes les URL. | Facultatif |

---


**Exemples**

Voici trois exemples de la **afficher urlacl** commande.

- Afficher urlacl url =https://+:80/MyUri
- Afficher urlacl url =<https://www.contoso.com:80/MyUri>
- afficher les urlacl

---