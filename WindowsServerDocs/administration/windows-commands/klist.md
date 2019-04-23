---
title: klist
description: 'Rubrique de commandes de Windows pour ***- '
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
ms.openlocfilehash: 3b3d0591f9feb12782d0c77b6c786cfe17656ab2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831160"
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
|-lh|Indique la partie haute de l’identificateur unique local de l’utilisateur (LUID), exprimée au format hexadécimal. Si ni lh – ou – li sont présents, la commande par défaut est le LUID de l’utilisateur qui est actuellement connecté.|
|-li|Indique la partie basse de l’identificateur unique local de l’utilisateur (LUID), exprimée au format hexadécimal. Si ni lh – ou – li sont présents, la commande par défaut est le LUID de l’utilisateur qui est actuellement connecté.|
|tickets|Répertorie les actuellement mis en cache ticket-granting-ticket (TGT) et les tickets de service de la session d’ouverture de session spécifié. Il s’agit de l’option par défaut.|
|tgt|Affiche les TGT. Kerberos initiale|
|Purger|Vous permet de supprimer tous les tickets de l’ouverture de session spécifiée.|
|sessions|Affiche une liste des ouvertures de session sur cet ordinateur.|
|kcd_cache|Affiche le Kerberos contraint les informations de cache de délégation.|
|get|Vous permet de demander un ticket à l’ordinateur cible spécifié par le nom de principal du service (SPN).|
|add_bind|Vous permet de spécifier un contrôleur de domaine préféré pour l’authentification Kerberos.|
|query_bind|Affiche une liste mise en cache par défaut des contrôleurs de domaine pour chaque domaine Kerberos a contacté.|
|purge_bind|Supprime la mise en cache par défaut des contrôleurs de domaine pour les domaines spécifiés.|
|kdcoptions|Affiche les options de centre de Distribution de clés (KDC) spécifiées dans RFC 4120.|
|/?|Affiche l’aide de cette commande.|

## <a name="remarks"></a>Notes

L’appartenance au **Admins du domaine**, ou équivalente, est la condition minimale requise pour exécuter tous les paramètres de cette commande.

Si aucun paramètre n’est fourni, Klist récupérera tous les tickets de l’utilisateur actuellement connecté.

Les paramètres s’affichent les informations suivantes :
-   **tickets**

    Répertorie les tickets actuellement mis en cache de services que vous avez authentifié à depuis l’ouverture de session. Affiche les attributs suivants de tous les tickets mis en cache :  
    -   LogonID : Le LUID
    -   Client : La concaténation du nom de client et le nom de domaine du client
    -   Serveur : La concaténation du nom de service et le nom de domaine du service
    -   Type de chiffrement KerbTicket : Le type de chiffrement qui est utilisé pour chiffrer le ticket Kerberos
    -   Indicateurs de ticket : Les indicateurs de ticket Kerberos
    -   Heure de début : L’heure à partir de laquelle le ticket sera valide
    -   Heure de fin : L’heure que le ticket devient n’est plus valide. Lorsqu’un ticket est postérieure à cette heure, il n’est plus utilisable pour s’authentifier auprès d’un service ou être utilisée pour le renouvellement
    -   Renouveler : Le temps nécessaire une nouvelle authentification initiale
    -   Type de clé de session : L’algorithme de chiffrement qui est utilisé pour la clé de session
-   **tgt**

    Répertorie le TGT Kerberos initiale et les attributs suivants du ticket actuellement mis en cache :  
    -   LogonID : Identifiés en notation hexadécimale
    -   ServiceName: krbtgt
    -   TargetName \<SPN > : krbtgt
    -   DomainName : Nom du domaine qui émet le ticket TGT
    -   TargetDomainName: Domaine émis le ticket TGT pour
    -   AltTargetDomainName: Domaine émis le ticket TGT pour
    -   Indicateurs de ticket : Type et l’adresse et la cible des actions
    -   Clé de session : Algorithme de chiffrement et la longueur de clé
    -   Heure de début : Heure de l’ordinateur local qui le ticket a été demandé.
    -   Heure de fin : Heure que le ticket devient n’est plus valide. Lorsqu’un ticket est postérieure à cette heure, il n’est plus utilisable pour s’authentifier auprès d’un service.
    -   RenewUntil : Délai de renouvellement du ticket
    -   TimeSkew : Différence de temps avec le centre de Distribution de clés (KDC)
    -   EncodedTicket: Ticket encodé
-   **purge**

    Vous permet de supprimer un ticket spécifique. Purge des tickets détruit tous les tickets que vous avez mis en cache, utilisez cet attribut avec précaution. Il est possible que vous ne soient pas en mesure de s’authentifier auprès de ressources. Si cela se produit, vous devez vous déconnecter et se reconnecter.  
    -   LogonID : Identifiés en notation hexadécimale
-   **sessions**

    Vous permet de répertorier et afficher les informations pour toutes les sessions d’ouverture de session sur cet ordinateur.  
    -   LogonID : Si spécifié, affiche l’ouverture de session uniquement par la valeur donnée. Si non spécifié, affiche toutes les sessions d’ouverture de session sur cet ordinateur.
-   **kcd_cache**

    Vous permet d’afficher les informations de cache de délégation contrainte Kerberos.  
    -   LogonID : Si spécifié, affiche les informations de cache pour l’ouverture de session par la valeur donnée. Si non spécifié, affiche les cache des informations d’ouverture de session de l’utilisateur actuel.
-   **get**

    Vous permet de demander un ticket à la cible est spécifié par le SPN.  
    -   LogonID : Si spécifié, demande un ticket à l’aide de l’ouverture de session par la valeur donnée. Si non spécifié, demande un ticket à l’aide d’ouverture de session de l’utilisateur actuel.
    -   kdcoptions : Demande un ticket avec les options de contrôleur de domaine Kerberos spécifiées.
-   **add_bind**

    Vous permet de spécifier un contrôleur de domaine préféré pour l’authentification Kerberos.
-   **query_bind**

    Vous permet d’afficher les contrôleurs de domaine de mise en cache et par défaut pour les domaines.
-   **purge_bind**

    Vous permet de supprimer des contrôleurs de domaine de mise en cache et par défaut pour les domaines.
-   **kdcoptions**

    Pour obtenir la liste des options et leurs explications, consultez [RFC 4120](http://www.ietf.org/rfc/rfc4120.txt).

**Autres considérations**
-   Klist.exe est disponible dans Windows Server 2012 et Windows 8, et il implique aucune installation spéciale.

## <a name="BKMK_Examples"></a>Exemples

1.  Lorsque vous analysez un 27 d’ID d’événement lors du traitement de demander un un service d’accord de tickets (TGS) pour le serveur cible, le compte n’avait pas de clé appropriée pour générer un ticket Kerberos. Vous pouvez utiliser Klist pour interroger le cache de ticket Kerberos pour déterminer si les tickets sont manquants, si le serveur cible ou le compte est erroné ou si le type de chiffrement n’est pas pris en charge.  
    ```
    klist 
    ```  
    ```
    klist –li 0x3e7
    ```  
2.  Lorsque vous diagnostiquez les erreurs et vous souhaitez connaître les spécificités de chaque ticket-granting-ticket qui est mis en cache sur l’ordinateur pour une ouverture de session, vous pouvez utiliser Klist pour afficher les informations de ticket TGT.  
    ```
    klist tgt
    ```  
3.  Si vous ne parvenez pas à établir une connexion et de diagnostic peut prendre trop de temps, vous pouvez vider le cache de ticket Kerberos, déconnectez-vous et puis rouvrez une session.  
    ```
    klist purge
    ```  
    ```
    klist purge –li 0x3e7
    ```  
4.  Lorsque vous souhaitez diagnostiquer une ouverture de session pour un utilisateur ou un service, vous pouvez utiliser la commande suivante pour rechercher le numéro de session qui est utilisé dans d’autres commandes Klist.  
    ```
    klist sessions
    ```  
5.  Lorsque vous souhaitez identifier le problème de délégation Kerberos contrainte, vous pouvez utiliser la commande suivante pour rechercher la dernière erreur qui s’est produite.  
    ```
    klist kcd_cache
    ```  
6.  Lorsque vous souhaitez identifier si un utilisateur ou un service peut obtenir un ticket à un serveur, vous pouvez utiliser cette commande pour demander un ticket pour un nom principal de service spécifique.  
    ```
    klist get host/%computername%
    ```  
7.  Lors du diagnostic des problèmes de réplication entre les contrôleurs de domaine, vous devez en général, l’ordinateur client pour cibler un contrôleur de domaine spécifique. Dans ce cas, vous pouvez utiliser la commande suivante pour cibler l’ordinateur client à ce contrôleur de domaine spécifique.  
    ```
    klist add_bind CONTOSO KDC.CONTOSO.COM
    
    ```  
    ```
    klist add_bind CONTOSO.COM KDC.CONTOSO.COM
    ```  
8.  Pour interroger les contrôleurs de domaine de cet ordinateur récemment contacté, vous pouvez utiliser la commande suivante.  
    ```
    klist query_bind
    ```  
9.  Lorsque vous souhaitez Kerberos pour redécouvrir les contrôleurs de domaine, vous pouvez utiliser la commande suivante. Cette commande peut également être utilisée pour vider le cache avant de créer de nouvelles liaisons de contrôleur de domaine avec klist add_bind.  
    ```
    klist purge_bind
    ```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)