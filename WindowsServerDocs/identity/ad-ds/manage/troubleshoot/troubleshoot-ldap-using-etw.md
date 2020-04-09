---
title: Utilisation d’ETW pour résoudre les problèmes de connexion LDAP
description: Comment activer et utiliser ETW pour suivre les connexions LDAP entre les contrôleurs de domaine AD DS.
author: Teresa-Motiv
manager: dcscontentpm
ms.prod: windows-server-dev
ms.technology: active-directory-lightweight-directory-services
audience: Admin
ms.author: v-tea
ms.topic: article
ms.date: 11/22/2019
ms.openlocfilehash: f7b7df714dbd02b15555fa20c70c1e995e121a48
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822932"
---
# <a name="using-etw-to-troubleshoot-ldap-connections"></a>Utilisation d’ETW pour résoudre les problèmes de connexion LDAP

[Suivi d’v nements pour Windows (ETW)](https://docs.microsoft.com/windows/win32/etw/event-tracing-portal) peut être un outil de dépannage précieux pour Active Directory Domain Services (AD DS). Vous pouvez utiliser ETW pour suivre les communications[LDAP](https://docs.microsoft.com/previous-versions/windows/desktop/ldap/lightweight-directory-access-protocol-ldap-api)(Lightweight Directory Access Protocol) entre les clients Windows et les serveurs LDAP, y compris les contrôleurs de domaine AD DS.

## <a name="how-to-turn-on-etw-and-start-a-trace"></a>Comment activer ETW et démarrer une trace

**Pour activer ETW**

1. Ouvrez l’éditeur du Registre et créez la sous-clé de Registre suivante :

   **HKEY\_LOCAL\_MACHINE\\System\\CurrentControlSet\\Services\\ldap\\Tracing\\* ProcessName***

   Dans cette sous-clé, *ProcessName* est le nom complet du processus que vous souhaitez suivre, y compris son extension (par exemple, « svchost. exe »).

1. (**Facultatif**) Sous cette sous-clé, créez une nouvelle entrée nommée **PID**. Pour utiliser cette entrée, attribuez un ID de processus en tant que valeur DWORD.  

   Si vous spécifiez un ID de processus, ETW effectue le suivi uniquement de l’instance de l’application qui a cet ID de processus.

**Pour démarrer une session de suivi**

- Ouvrez une fenêtre d’invite de commandes, puis exécutez la commande suivante :

   ```cmd
   tracelog.exe -start <SessionName> -guid \#099614a5-5dd7-4788-8bc9-e29f43db28fc -f <FileName> -flag <TraceFlags>
   ```

   Les espaces réservés dans cette commande représentent les valeurs suivantes.

  - \<*nomsession*> est un identificateur arbitraire utilisé pour étiqueter la session de suivi.  
  > [!NOTE]  
  > Vous devrez vous référer à ce nom de session ultérieurement lorsque vous arrêterez la session de suivi.
  - \<*nom*de fichier > spécifie le fichier journal dans lequel les événements seront écrits.
  - \<*TraceFlags*> doit être une ou plusieurs des valeurs répertoriées dans la table des [indicateurs de trace](#values-for-trace-flags).

## <a name="how-to-end-a-tracing-session-and-turn-off-event-tracing"></a>Comment mettre fin à une session de suivi et désactiver le suivi des événements

**Pour arrêter le suivi**

- À l'invite de commandes, exécutez la commande suivante :

   ```cmd
   tracelog.exe -stop <SessionName>
   ```

   Dans cette commande, \<*nom_session*> est le même nom que celui que vous avez utilisé dans la commande **tracelog. exe-Start** .

**Pour désactiver ETW**

- Dans l’éditeur du Registre, supprimez le **\_LOCAL\_ordinateur\\système\\CurrentControlSet\\Services\\la sous-clé LDAP\\tracing\\* ProcessName***.

## <a name="values-for-trace-flags"></a>Valeurs pour les indicateurs de trace

Pour utiliser un indicateur, remplacez la valeur de l’indicateur de l’espace réservé <*TraceFlags*> dans les arguments de la commande **tracelog. exe-Start** .

> [!NOTE]  
> Vous pouvez spécifier plusieurs indicateurs en utilisant la somme des valeurs d’indicateur appropriées. Par exemple, pour spécifier les indicateurs **debug\_Search** (0x00000001) et **Debug\_cache** (0x00000010), la valeur \<*TraceFlags*> appropriée est **0x00000011**.

|Nom de l’indicateur |Valeur de l’indicateur |Description de l’indicateur |
| --- | --- | --- |
|**DEBUG_SEARCH** |0x00000001 |Journalise les demandes de recherche et les paramètres transmis à ces demandes. Les réponses ne sont pas enregistrées ici. Seules les demandes de recherche sont journalisées. (Utilisez **DEBUG_SPEWSEARCH** pour journaliser les réponses aux requêtes de recherche.) |
|**DEBUG_WRITE** |0x00000002 |Journalise les demandes d’écriture et les paramètres transmis à ces demandes. Les demandes d’écriture incluent les opérations d’ajout, de suppression, de modification et étendues. |
|**DEBUG_REFCNT** |0x00000004 |Journalise les données et les opérations de comptage des références pour les connexions et les demandes. |
|**DEBUG_HEAP** |0x00000008 |Journalise toutes les allocations de mémoire et les libérations de mémoire. |
|**DEBUG_CACHE** |0x00000010 |Journalise l’activité du cache. Cette activité inclut les ajouts, les suppressions, les accès, les échecs, etc. |
|**DEBUG_SSL** |0x00000020 |Journalise les informations et les erreurs SSL. |
|**DEBUG_SPEWSEARCH** |0x00000040 |Journalise toutes les réponses du serveur aux demandes de recherche. Ces réponses incluent les attributs qui ont été demandés, ainsi que toutes les données qui ont été reçues. |
|**DEBUG_SERVERDOWN** |0x00000080 |Journalise les erreurs de connexion et de serveur. |
|**DEBUG_CONNECT** |0x00000100 |Journalise les données relatives à l’établissement d’une connexion.<br />Utilisez **DEBUG_CONNECTION** pour consigner d’autres données relatives aux connexions. |
|**DEBUG_RECONNECT** |0x00000200 |Journalise l’activité de reconnexion automatique. Cette activité comprend les tentatives de reconnexion, les échecs et les erreurs associées. |
|**DEBUG_RECEIVEDATA** |0x00000400 |Journalise l’activité liée à la réception de messages à partir du serveur. Cette activité comprend des événements tels que l’attente de la réponse du serveur et la réponse reçue du serveur. |
|**DEBUG_BYTES_SENT** |0x00000800 |Journalise toutes les données envoyées par le client LDAP sur le serveur. Cette fonction est essentiellement la journalisation des paquets, mais elle enregistre toujours les données non chiffrées. (Si un paquet est envoyé via SSL, cette fonction journalise le paquet non chiffré.) Cette journalisation peut être détaillée. Cet indicateur est probablement utilisé seul ou combiné avec **DEBUG_BYTES_RECEIVED**. |
|**DEBUG_EOM** |0x00001000 |Journalise les événements liés à l’atteinte de la fin d’une liste de messages. Ces événements incluent des informations telles que « la liste des messages est désactivée », et ainsi de suite. |
|**DEBUG_BER** |0x00002000 |Journalise les opérations et les erreurs liées aux règles de codage de base (BER). Ces opérations et erreurs incluent des problèmes d’encodage, des problèmes de taille de mémoire tampon, etc. |
|**DEBUG_OUTMEMORY** |0x00004000 |Journalise les échecs pour allouer de la mémoire. Journalise également tout échec de calcul de la mémoire requise (par exemple, un dépassement de capacité qui se produit lors du calcul de la taille de mémoire tampon requise). |
|**DEBUG_CONTROLS** |0x00008000 |Journalise les données relatives aux contrôles. Ces données incluent les contrôles qui sont insérés, les problèmes qui affectent les contrôles, les contrôles obligatoires sur une connexion, etc. |
|**DEBUG_BYTES_RECEIVED** |0x00010000 |Journalise toutes les données reçues par le client LDAP. Ce comportement est essentiellement la journalisation des paquets, mais enregistre toujours les données non chiffrées. (Si un paquet est envoyé via SSL, cette option journalise le paquet non chiffré.) Ce type de journalisation peut être détaillé. Cet indicateur est probablement utilisé seul ou combiné avec **DEBUG_BYTES_SENT**. |
|**DEBUG_CLDAP** |0x00020000 |Journalise les événements spécifiques à UDP et LDAP sans connexion. |
|**DEBUG_FILTER** |0x00040000 |Journalise les événements et les erreurs rencontrés lors de la construction d’un filtre de recherche.<br/>**Remarque** Cette option consigne les événements du client uniquement lors de la construction du filtre. Il n’enregistre aucune réponse du serveur sur un filtre. |
|**DEBUG_BIND** |0x00080000 |Journalise les événements et les erreurs de liaison. Ces données incluent les informations de négociation, la réussite de la liaison, l’échec de la liaison, etc. |
|**DEBUG_NETWORK_ERRORS** |0x00100000 |Journalise les erreurs réseau générales. Ces données incluent les erreurs d’envoi et de réception.<br/>**Remarque** Si une connexion est perdue ou que le serveur est inaccessible, **DEBUG_SERVERDOWN** est la balise préférée. |
|**DEBUG_VERBOSE** |0x00200000 |Journalise les messages généraux. Utilisez cette option pour tous les messages qui ont tendance à générer une grande quantité de sortie. Par exemple, il journalise des messages tels que « fin du message atteint », « le serveur n’a pas encore répondu », et ainsi de suite. Cette option est également utile pour les messages génériques. |
|**DEBUG_PARSE** |0x00400000 |Journalise les événements de message généraux et les erreurs, ainsi que l’analyse de paquets et les erreurs et les événements de codage. |
|**DEBUG_REFERRALS** |0x00800000 |Journalise les données relatives aux références et aux références de repérage. |
|**DEBUG_REQUEST** |0x01000000 |Journalise le suivi des requêtes. |
|**DEBUG_CONNECTION** |0x02000000 |Journalise les données de connexion générales et les erreurs. |
|**DEBUG_INIT_TERM** |0x04000000 |Journalise l’initialisation et le nettoyage du module (DLL main, etc.). |
|**DEBUG_API_ERRORS** |0x08000000 |Prend en charge la journalisation de l’utilisation incorrecte de l’API. Par exemple, cette option journalise les données si l’opération de liaison est appelée deux fois sur la même connexion. |
|**DEBUG_ERRORS** |0x10000000 |Journalise les erreurs générales. La plupart de ces erreurs peuvent être classées comme des erreurs d’initialisation de module, des erreurs SSL ou des erreurs de dépassement de capacité ou de dépassement de capacité. |
|**DEBUG_PERFORMANCE** |0x20000000 |Journalise les données relatives aux statistiques d’activité LDAP du processus global après la réception d’une réponse du serveur pour une demande LDAP. |

## <a name="example"></a>Exemple

Prenons l’exemple d’une application, App1. exe, qui définit les mots de passe des comptes d’utilisateur. Supposons que App1. exe génère une erreur inattendue. Pour utiliser ETW afin de diagnostiquer ce problème, procédez comme suit :

1. Dans l’éditeur du Registre, créez l’entrée de Registre suivante :

   **HKEY\_LOCAL\_MACHINE\\System\\CurrentControlSet\\services\\LDAP\\Tracing\\App1. exe**

1. Pour démarrer une session de suivi, ouvrez une fenêtre d’invite de commandes, puis exécutez la commande suivante :

   ```cmd
   tracelog.exe -start ldaptrace -guid \#099614a5-5dd7-4788-8bc9-e29f43db28fc -f .\ldap.etl -flag 0x80000
   ```

   Après le démarrage de cette commande, **DEBUG\_bind** s’assure que le suivi des messages est écrit dans.\\LDAP. etl.

1. Démarrez App1. exe et reproduisez l’erreur inattendue.

1. Pour arrêter la session de suivi, exécutez la commande suivante à l’invite de commandes :

   ```cmd
    tracelog.exe -stop ldaptrace
   ```

1. Pour empêcher d’autres utilisateurs de procéder au suivi de l’application, supprimez la clé **HKEY\_LOCAL\_MACHINE**\\**System**\\**CurrentControlSet**\\**services**\\**LDAP**\\**Tracing**\\l’entrée de Registre **App1. exe** .

1. Pour passer en revue les informations du journal des traces, exécutez la commande suivante à l’invite de commandes :

   ```cmd
    tracerpt.exe .\ldap.etl -o -report
    ```

   > [!NOTE]  
   > Dans cette commande, **Tracerpt. exe** est un outil de [consommateur de suivi](https://go.microsoft.com/fwlink/p/?linkid=83876) .
