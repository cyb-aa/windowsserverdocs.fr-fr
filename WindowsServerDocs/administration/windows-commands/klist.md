---
title: klist
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4689b4a9-1740-47dd-9240-02105efca428
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a8f574b65ec8c123379e1b02ee1571cc9f21fa1
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867058"
---
# <a name="klist"></a>klist



Affiche la liste des tickets Kerberos actuellement mis en cache. Ces informations s’appliquent à Windows Server 2012. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
klist [-lh <LogonId.HighPart>] [-li <LogonId.LowPart>] tickets | tgt | purge | sessions | kcd_cache | get | add_bind | query_bind | purge_bind
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-LH|Indique la partie haute de l’identificateur unique local (LUID) de l’utilisateur, exprimée au format hexadécimal. Si aucune des options – LH ou-Li n’est présente, la commande prend par défaut le LUID de l’utilisateur actuellement connecté.|
|-Li|Indique la partie basse de l’identificateur unique local (LUID) de l’utilisateur, exprimée au format hexadécimal. Si aucune des options – LH ou-Li n’est présente, la commande prend par défaut le LUID de l’utilisateur actuellement connecté.|
|loterie|Répertorie les tickets d’accord de ticket (TGT) actuellement mis en cache et les tickets de service de la session d’ouverture de session spécifiée. Il s'agit de l'option par défaut.|
|TGT|Affiche le ticket TGT Kerberos initial.|
|Purger|Vous permet de supprimer tous les tickets de la session d’ouverture de session spécifiée.|
|Entretiens|Affiche la liste des sessions de connexion sur cet ordinateur.|
|kcd_cache|Affiche les informations du cache de délégation Kerberos avec restriction.|
|get|Vous permet de demander un ticket à l’ordinateur cible spécifié par le nom de principal du service (SPN).|
|add_bind|Vous permet de spécifier un contrôleur de domaine par défaut pour l’authentification Kerberos.|
|query_bind|Affiche la liste des contrôleurs de domaine préférés mis en cache pour chaque domaine contacté par Kerberos.|
|purge_bind|Supprime les contrôleurs de domaine préférés mis en cache pour les domaines spécifiés.|
|kdcoptions|Affiche les options de centre de distribution de clés (KDC) spécifiées dans le document RFC 4120.|
|/?|Affiche l’aide de cette commande.|

## <a name="remarks"></a>Notes

L’appartenance au **groupe Admins du domaine**, ou équivalent, est la condition minimale requise pour exécuter tous les paramètres de cette commande.

Si aucun paramètre n’est fourni, klist récupère tous les tickets pour l’utilisateur actuellement connecté.

Les paramètres affichent les informations suivantes :
-   **loterie**

    Répertorie les tickets actuellement mis en cache des services auxquels vous vous êtes authentifié depuis l’ouverture de session. Affiche les attributs suivants de tous les tickets mis en cache :  
    -   LogonID: LUID
    -   Client : La concaténation du nom du client et du nom de domaine du client
    -   Serveur : Concaténation du nom du service et du nom de domaine du service
    -   Type de chiffrement KerbTicket : Type de chiffrement utilisé pour chiffrer le ticket Kerberos
    -   Indicateurs de ticket : Indicateurs de ticket Kerberos
    -   Heure de début : Heure à partir de laquelle le ticket sera valide
    -   Heure de fin : Heure à laquelle le ticket n’est plus valide. Lorsqu’un ticket est passé à cette heure, il ne peut plus être utilisé pour s’authentifier auprès d’un service ou être utilisé pour le renouvellement
    -   Temps de renouvellement : L’heure à laquelle une nouvelle authentification initiale est requise
    -   Type de clé de session : Algorithme de chiffrement utilisé pour la clé de session
-   **TGT**

    Répertorie le ticket TGT Kerberos initial et les attributs suivants du ticket actuellement mis en cache :  
    -   LogonID: Identifié au format hexadécimal
    -   ServiceName : krbtgt
    -   > \<SPN NomCible : krbtgt
    -   NomDomaine Nom du domaine qui émet le ticket TGT
    -   TargetDomainName: Domaine auquel le TGT est émis
    -   AltTargetDomainName: Domaine auquel le TGT est émis
    -   Indicateurs de ticket : Actions et type d’adresse et de cible
    -   Clé de session : Longueur de clé et algorithme de chiffrement
    -   Heure Heure de l’ordinateur local à laquelle le ticket a été demandé
    -   EndTime Heure à laquelle le ticket n’est plus valide. Lorsqu’un ticket est passé cette fois, il ne peut plus être utilisé pour s’authentifier auprès d’un service.
    -   RenewUntil: Échéance pour le renouvellement du ticket
    -   TimeSkew: Différence de temps avec le centre de distribution de clés (KDC)
    -   EncodedTicket: Ticket encodé
-   **purge**

    Vous permet de supprimer un ticket spécifique. La purge des tickets détruit tous les tickets que vous avez mis en cache. Utilisez donc cet attribut avec précaution. Cela peut vous empêcher d’être en mesure de s’authentifier auprès des ressources. Dans ce cas, vous devez vous déconnecter et vous reconnecter.  
    -   LogonID: Identifié au format hexadécimal
-   **entretiens**

    Vous permet de répertorier et d’afficher les informations de toutes les sessions de connexion sur cet ordinateur.  
    -   LogonID: S’il est spécifié, affiche la session de connexion uniquement par la valeur donnée. S’il n’est pas spécifié, affiche toutes les sessions de connexion sur cet ordinateur.
-   **kcd_cache**

    Vous permet d’afficher les informations du cache de délégation Kerberos avec restriction.  
    -   LogonID: S’il est spécifié, affiche les informations de cache pour la session de connexion en fonction de la valeur donnée. S’il n’est pas spécifié, affiche les informations de cache pour la session d’ouverture de session de l’utilisateur actuel.
-   **get**

    Vous permet de demander un ticket à la cible spécifiée par le SPN.  
    -   LogonID: S’il est spécifié, demande un ticket à l’aide de la session de connexion en fonction de la valeur donnée. S’il n’est pas spécifié, demande un ticket à l’aide de la session d’ouverture de session de l’utilisateur actuel.
    -   kdcoptions: Demande un ticket avec les options KDC données
-   **add_bind**

    Vous permet de spécifier un contrôleur de domaine par défaut pour l’authentification Kerberos.
-   **query_bind**

    Vous permet d’afficher les contrôleurs de domaine préférés mis en cache pour les domaines.
-   **purge_bind**

    Vous permet de supprimer les contrôleurs de domaine préférés mis en cache pour les domaines.
-   **kdcoptions**

    Pour obtenir la liste actuelle des options et leurs explications, consultez [RFC 4120](http://www.ietf.org/rfc/rfc4120.txt).

**Autres points à considérer**
-   Klist. exe est disponible dans Windows Server 2012 et Windows 8, et ne nécessite aucune installation particulière.

## <a name="BKMK_Examples"></a>Illustre

1. Lors du diagnostic d’un ID d’événement 27 lors du traitement d’une demande TGS (Ticket-Granting Service) pour le serveur cible, le compte n’avait pas de clé appropriée pour générer un ticket Kerberos. Vous pouvez utiliser klist pour interroger le cache de tickets Kerberos afin de déterminer si des tickets sont manquants, si le serveur ou le compte cible est erroné, ou si le type de chiffrement n’est pas pris en charge.  
   ```
   klist 
   ```  
   ```
   klist –li 0x3e7
   ```  
2. Lorsque vous diagnostiquez des erreurs et que vous souhaitez connaître les spécificités de chaque ticket d’accord de tickets mis en cache sur l’ordinateur pour une ouverture de session, vous pouvez utiliser klist pour afficher les informations TGT.  
   ```
   klist tgt
   ```  
3. Si vous ne parvenez pas à établir une connexion et que le diagnostic peut prendre trop de temps, vous pouvez vider le cache du ticket Kerberos, vous déconnecter, puis vous reconnecter.  
   ```
   klist purge
   ```  
   ```
   klist purge –li 0x3e7
   ```  
4. Lorsque vous souhaitez diagnostiquer une ouverture de session pour un utilisateur ou un service, vous pouvez utiliser la commande suivante pour rechercher le LogonID utilisé dans d’autres commandes klist.  
   ```
   klist sessions
   ```  
5. Lorsque vous souhaitez diagnostiquer un échec de délégation Kerberos avec restriction, vous pouvez utiliser la commande suivante pour rechercher la dernière erreur rencontrée.  
   ```
   klist kcd_cache
   ```  
6. Lorsque vous souhaitez diagnostiquer si un utilisateur ou un service peut obtenir un ticket sur un serveur, vous pouvez utiliser cette commande pour demander un ticket pour un SPN spécifique.  
   ```
   klist get host/%computername%
   ```  
7. Lors du diagnostic des problèmes de réplication entre les contrôleurs de domaine, vous avez généralement besoin de l’ordinateur client pour cibler un contrôleur de domaine spécifique. Dans ce cas, vous pouvez utiliser la commande suivante pour cibler l’ordinateur client sur ce contrôleur de domaine spécifique.  
   ```
   klist add_bind CONTOSO KDC.CONTOSO.COM
   ```  
   ```
   klist add_bind CONTOSO.COM KDC.CONTOSO.COM
   ```  
8. Pour interroger les contrôleurs de domaine que cet ordinateur a récemment contactés, vous pouvez utiliser la commande suivante.  
   ```
   klist query_bind
   ```  
9. Lorsque vous souhaitez que Kerberos redécouvre les contrôleurs de domaine, vous pouvez utiliser la commande suivante. Cette commande peut également être utilisée pour vider le cache avant de créer de nouvelles liaisons de contrôleur de domaine avec klist add_bind.  
   ```
   klist purge_bind
   ```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)