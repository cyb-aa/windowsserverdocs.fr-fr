---
ms.assetid: 0f21951c-b1bf-43bb-a329-bbb40c58c876
title: Erreur de réplication 1753 Le mappeur de point final n’a plus de point final disponible
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9c8efee98cc8128443d9c835ccc5cb6b7695a094
ms.sourcegitcommit: a9625758fbfb066494fe62e0da5f9570ccb738a3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68952470"
---
# <a name="replication-error-1753-there-are-no-more-endpoints-available-from-the-endpoint-mapper"></a>Erreur de réplication 1753 Le mappeur de point final n’a plus de point final disponible

>S'applique à : Windows Server

Cet article décrit les symptômes, les causes et les étapes de résolution pour les opérations de Active Directory qui échouent avec l’erreur Win32 1753: «Il n’y a plus de points de terminaison disponibles à partir du mappeur de point de terminaison».

DCDIAG signale que le test de connectivité, Active Directory test de réplication ou le test KnowsOfRoleHolders a échoué avec l’erreur 1753: «Il n’y a plus de points de terminaison disponibles à partir du mappeur de point de terminaison».

```
Testing server: <site><DC Name>
Starting test: Connectivity
* Active Directory LDAP Services Check
* Active Directory RPC Services Check
[<DC Name>] DsBindWithSpnEx() failed with error 1753,
There are no more endpoints available from the endpoint mapper..
Printing RPC Extended Error Info:
Error Record 1, ProcessID is <process ID> (DcDiag)
System Time is: <date> <time>
Generating component is 2 (RPC runtime)
Status is 1753: There are no more endpoints available from the endpoint mapper. Detection location is 500
NumberOfParameters is 4
Unicode string: ncacn_ip_tcp
Unicode string: <source DC object GUID>._msdcs.contoso.com
Long val: -481213899
Long val: 65537
Error Record 2, ProcessID is 700 (DcDiag)
System Time is: <date> <time>
Generating component is 2 (RPC runtime)
Status is 1753: There are no more endpoints available from the endpoint mapper.
NumberOfParameters is 1
Unicode string: 1025
[Replications Check,<DC Name>] A recent replication attempt failed:
From <source DC> to <destination DC>
Naming Context: <DN path of directory partition>
The replication generated an error (1753):
There are no more endpoints available from the endpoint mapper.
The failure occurred at <date> <time>.
The last success occurred at <date> <time>.
3 failures have occurred since the last success.
The directory on <DC name> is in the process.
of starting up or shutting down, and is not available.
Verify machine is not hung during boot.
```

REPADMIN. EXE signale que la tentative de réplication a échoué avec l’État 1753.
Les commandes REPADMIN qui citent communément l’État 1753 incluent, sans s’y limiter:

* REPADMIN/REPLSUM.
* REPADMIN/SHOWREPL
* REPADMIN/SHOWREPS
* REPADMIN/SYNCALL

L’exemple de sortie de «REPADMIN/SHOWREPS» illustrant la réplication entrante de CONTOSO-DC2 vers CONTOSO-DC1 échoue avec l’erreur «l’accès de réplication a été refusé» est illustré ci-dessous:

```
Default-First-Site-NameCONTOSO-DC1
DSA Options: IS_GC 
Site Options: (none)
DSA object GUID: b6dc8589-7e00-4a5d-b688-045aef63ec01
DSA invocationID: b6dc8589-7e00-4a5d-b688-045aef63ec01
==== INBOUND NEIGHBORS ======================================
DC=contoso,DC=com
Default-First-Site-NameCONTOSO-DC2 via RPC
DSA object GUID: 74fbe06c-932c-46b5-831b-af9e31f496b2
Last attempt @ <date> <time> failed, result 1753 (0x6d9):
There are no more endpoints available from the endpoint mapper.
<#> consecutive failure(s).
Last success @ <date> <time>.
```

La commande de vérification de la **topologie** de réplication dans Active Directory sites et services retourne «aucun point de terminaison n’est disponible à partir du mappeur de point de terminaison».

Le fait de cliquer avec le bouton droit sur l’objet de connexion à partir d’un contrôleur de périphérique source et de sélectionner **Vérifier la topologie** de réplication échoue avec «aucun point de terminaison n’est disponible à partir du mappeur de point de terminaison». Le message d’erreur à l’écran est illustré ci-dessous:

Texte du titre de la boîte de dialogue: Vérifier le texte du message de la boîte de dialogue de la topologie de réplication: L’erreur suivante s’est produite lors de la tentative de contact du contrôleur de domaine: Il n’y a plus de points de terminaison disponibles à partir du mappeur de point de terminaison.

La commande **Replica Now** dans Active Directory sites et services retourne «aucun point de terminaison n’est disponible à partir du mappeur de point de terminaison».
Le fait de cliquer avec le bouton droit sur l’objet de connexion à partir d’un contrôleur de périphérique source et de choisir réplication échoue **maintenant** avec «aucun point de terminaison n’est disponible à partir du mappeur de point de terminaison».
Le message d’erreur à l’écran est illustré ci-dessous:

Texte du titre de la boîte de dialogue: Texte du message de la boîte de dialogue répliquer maintenant: L’erreur suivante s’est produite lors de la tentative de \<synchronisation du contexte d’appellation% nom de \<la partition d’annuaire% > \<entre le contrôleur de domaine source DC > et le contrôleur de domaine > DC de destination:

Il n’y a plus de points de terminaison disponibles à partir du mappeur de point de terminaison.
L’opération ne se poursuit pas

Les événements KCC NTDS, NTDS général ou Microsoft-Windows-ActiveDirectory_DomainService avec l’État-2146893022 sont consignés dans le journal des services d’annuaire dans observateur d’événements.

Active Directory événements qui citent couramment l’État-2146893022 incluent, mais ne sont pas limités à:

| ID d’événement | Source de l'événement | Chaîne d’événement|
| --- | --- | --- |
| 1655 | Généralités sur NTDS | Active Directory a tenté de communiquer avec le catalogue global suivant et les tentatives ont échoué. |
| 1925 | KCC NTDS | Échec de la tentative d’établissement d’un lien de réplication pour la partition d’annuaire accessible en écriture suivante. |
| 1265 | KCC NTDS | Une tentative du vérificateur de cohérence des connaissances (KCC) pour ajouter un contrat de réplication pour la partition d’annuaire et le contrôleur de domaine source suivants a échoué. |

## <a name="cause"></a>Cause

Les étapes ci-dessous illustrent le flux de travail RPC qui commence par l’inscription de l’application serveur avec le mappeur de point de terminaison RPC (EPM) à l’étape 1 à la transmission des données du client RPC vers l’application cliente à l’étape 7.

### <a name="adds-rpc-workflow"></a>Ajoute un flux de travail RPC

1. L’application serveur inscrit ses points de terminaison auprès du mappeur de point de terminaison RPC (EPM)
1. Le client effectue un appel RPC (pour le compte d’un utilisateur, d’un système d’exploitation ou d’une opération lancée par l’application)
1. RPC côté client contacte les ordinateurs cibles EPM et demandez au point de terminaison de terminer l’appel client
1. L’EPM de l’ordinateur serveur répond avec un point de terminaison
1. RPC côté client contacte l’application serveur
1. L’application serveur exécute l’appel, retourne le résultat au client RPC
1. RPC côté client retourne le résultat à l’application cliente

L’échec 1753 est généré par un échec entre les étapes #3 et #4. Plus précisément, l’erreur 1753 signifie que le client RPC (DC de destination) a pu contacter le serveur RPC (DC source) sur le port 135, mais que le serveur EPM sur le serveur RPC (DC source) n’a pas pu localiser l’application RPC d’intérêt et a renvoyé l’erreur côté serveur 1753. La présence de l’erreur 1753 indique que le client RPC (DC de destination) a reçu la réponse d’erreur côté serveur du serveur RPC (contrôleur de domaine source de réplication Active Directory) sur le réseau.

Les causes racines spécifiques de l’erreur 1753 sont les suivantes:

* L’application serveur n’a jamais démarré (à savoir, l’étape #1 dans le diagramme «plus d’informations» situé ci-dessus n’a jamais été tentée).
* L’application serveur a démarré, mais une erreur s’est produite lors de l’initialisation qui l’a empêchée de s’inscrire auprès du mappeur de point de terminaison RPC (c’est-à-dire l’étape #1 dans le diagramme «plus d’informations» ci-dessus a été tentée, mais a échoué).
* L’application serveur a démarré, mais elle est suspendue. (par exemple, l’étape #1 dans le diagramme «plus d’informations» ci-dessus a été effectuée avec succès, mais a été annulée ultérieurement, car le serveur est mort).
* L’application serveur a supprimé manuellement ses points de terminaison (comme 3 mais intentionnellement). Non probable, mais inclus à des fins d’exhaustivité.)
* Le client RPC (contrôleur de domaine de destination) a contacté un autre serveur RPC que celui prévu en raison d’une erreur de mappage de nom à IP dans DNS, WINS ou fichier hôte/Lmhosts.

L’erreur 1753 n’est pas provoquée par:

* Absence de connectivité réseau entre le client RPC (DC de destination) et le serveur RPC (DC source) sur le port 135
* Absence de connectivité réseau entre le serveur RPC (contrôleur de réseau source) utilisant le port 135 et le client RPC (DC de destination) sur le port éphémère.
* Une incompatibilité de mot de passe ou l’incapacité du contrôleur de du code source à déchiffrer un paquet chiffré Kerberos

## <a name="resolutions"></a>Résolutions

Vérifier que le service qui inscrit son service auprès du mappeur de point de terminaison a démarré

* Pour les contrôleurs de Windows 2000 et Windows Server 2003: Assurez-vous que le contrôleur de périphérique source est démarré en mode normal.
* Pour Windows Server 2008 ou Windows Server 2008 R2: À partir de la console du DC source, démarrez le gestionnaire de services (services. msc) et vérifiez que le service Active Directory Domain Services est en cours d’exécution.

Vérifier que le client RPC (DC de destination) s’est connecté au serveur RPC prévu (DC source)

Tous les contrôleurs de domaine d’une forêt Active Directory commune inscrivent un enregistrement CNAMe de contrôleur de domaine dans la _ msdcs. \<domaine racine de la forêt > zone DNS quel que soit le domaine dans lequel ils résident dans la forêt. L’enregistrement CNAMe DC est dérivé de l’attribut objectGUID de l’objet Paramètres NTDS pour chaque contrôleur de domaine.

Lors de l’exécution d’opérations basées sur la réplication, un contrôleur de domaine de destination interroge DNS pour l’enregistrement CNAMe des contrôleurs de domaine sources. L’enregistrement CNAMe contient le nom d’ordinateur complet du contrôleur de domaine source utilisé pour dériver l’adresse IP des contrôleurs de domaine sources via la recherche dans le cache du client DNS, la recherche de fichiers hôte/LMHost, l’hôte A/AAAA enregistrement dans DNS ou WINS.

Les objets de paramètres NTDS obsolètes et les mappages de nom à adresse IP incorrects dans les fichiers DNS, WINS, Host et LMHOST peuvent entraîner la connexion du client RPC (contrôleur de domaine de destination) au mauvais serveur RPC (contrôleur de domaine source). En outre, le mappage nom-adresse IP incorrect peut entraîner la connexion du client RPC (DC de destination) à un ordinateur qui n’a même pas l’application serveur RPC d’intérêt (le rôle Active Directory dans ce cas). (Exemple: un enregistrement hôte périmé pour DC2 contient l’adresse IP de DC3 ou un ordinateur membre).

Vérifiez que l’objectGUID du contrôleur de périphérique source qui existe dans la copie des contrôleurs de source de destination de Active Directory correspond à l’attribut objectGUID DC source stocké dans la copie source DCs de Active Directory. En cas de divergence, utilisez Repadmin/showobjmeta sur l’objet Paramètres NTDS pour déterminer lequel correspond à la dernière promotion du contrôleur de source (hint: comparer les horodatages de la date de création de l’objet Paramètres NTDS de/showobjmeta à la date de la dernière promotion dans le fichier Dcpromo. log du contrôleur de source de contrôle d’accès. Vous devrez peut-être utiliser la dernière date de modification/création de DCPROMO. Fichier journal lui-même). Si les GUID d’objet ne sont pas identiques, il est probable que le DC de destination dispose d’un objet de paramètres NTDS obsolètes pour le contrôleur de périphérique source dont l’enregistrement CNAMe fait référence à un enregistrement d’hôte avec un nom incorrect pour le mappage IP.

Sur le contrôleur de domaine de destination, exécutez IPCONFIG/ALL pour déterminer les serveurs DNS que le contrôleur de domaine de destination utilise pour la résolution de noms:

```
c:>ipconfig /all
```

Sur le contrôleur de l’emplacement de destination, exécutez NSLOOKUP sur l’enregistrement CNAMe complet du contrôleur de source de contrôle d’alimentation:

```
c:>nslookup -type=cname <fully qualified cname of source DC> <destination DCs primary DNS Server IP >
c:>nslookup -type=cname <fully qualified cname of source DC> <destination DCs secondary DNS Server IP>
```

Vérifiez que l’adresse IP renvoyée par NSLOOKUP «possède» le nom d’hôte/l’identité de sécurité du contrôleur de périphérique source:

```
C:>NBTSTAT -A <IP address returned by NSLOOKUP in the step above>
```

Connectez-vous à la console du DC source, exécutez «IPCONFIG» à partir de l’invite CMD et vérifiez que le contrôleur de source de source possède l’adresse IP retournée par la commande NSLOOKUP ci-dessus.

Rechercher les mappages de l’hôte obsolète/dupliqué sur IP dans DNS

```
NSLOOKUP -type=hostname <single label hostname of source DC> <primary DNS Server IP on destination DC>
NSLOOKUP -type=hostname <single label hostname of source DC> <secondary DNS Server IP on destination DC>

NSLOOKUP -type=hostname <fully qualified computer name of source DC> <primary DNS Server IP on destination DC>
NSLOOKUP -type=hostname <fully qualified computer name of source DC> <secondary DNS Server IP on dest. DC>
```

Si des adresses IP non valides existent dans les enregistrements de l’hôte, vérifiez si le nettoyage DNS est activé et correctement configuré.

Si les tests ci-dessus ou un suivi réseau n’affichent pas une requête de nom qui retourne une adresse IP non valide, envisagez d’utiliser des entrées obsolètes dans les fichiers hôtes, les fichiers LMHOSTS et les serveurs WINS. Notez que les serveurs DNS peuvent également être configurés pour effectuer la résolution de noms de secours WINS.

* Vérifier que l’application serveur (Active Directory et al) s’est inscrite auprès du mappeur de point de terminaison sur le serveur RPC (DC source)
* Active Directory utilise une combinaison de ports bien connus et inscrits de manière dynamique. Ce tableau répertorie les ports et protocoles bien connus utilisés par les contrôleurs de domaine Active Directory.

| Application serveur RPC | Port | TCP | UDP |
| --- | --- | --- | --- |
| Serveur DNS | 53 | X | X |
| Kerberos | 88 | X | X |
| Serveur LDAP | 389 | X | X |
| Microsoft-DS | 445 | X | X |
| SSL LDAP | 636 | X | X |
| Serveur du catalogue global | 3268 | X |   |
| Serveur du catalogue global | 3269 | X |   |

Les ports connus ne sont pas enregistrés avec le mappeur de point de terminaison.

Active Directory et d’autres applications inscrivent également des services qui reçoivent des ports affectés dynamiquement dans la plage de ports éphémères RPC. Ces applications serveur RPC sont attribuées de façon dynamique entre 1024 et 5000 sur les ordinateurs Windows 2000 et Windows Server 2003, et les ports entre 49152 et 65535 sont compris entre les ordinateurs Windows Server 2008 et Windows Server 2008 R2. Le port RPC utilisé par la réplication peut être codé en dur dans le registre à l’aide de la procédure décrite dans [l’article 224196](https://support.microsoft.com/kb/224196)de la base de connaissances. Active Directory continue à s’inscrire auprès de EPM lorsqu’il est configuré pour utiliser un port codé en dur.

Vérifiez que l’application serveur RPC concernée s’est inscrite auprès du mappeur de point de terminaison RPC sur le serveur RPC (le contrôleur de domaine source dans le cas de la réplication Active Directory).

Il existe plusieurs façons d’accomplir cette tâche, mais l’une est d’installer et d’exécuter PORTQRY à partir d’une invite CMD privilégiée par l’administrateur sur la console du contrôleur de domaines source à l’aide de la syntaxe suivante:

```
portquery -n <source DC> -e 135 > file.txt
```

Dans la sortie de PortQry, notez les numéros de port inscrits dynamiquement par l’interface «MS NT Directory DRS interface» (UUID = 351...) pour le protocole ncacn_ip_tcp. L’extrait de code ci-dessous montre un exemple de sortie portquery à partir d’un contrôleur de Windows Server 2008 R2 DC:

```
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\pipe\lsass] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\PIPE\protected_storage] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_ip_tcp:CONTOSO-DC01[49156] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[49157] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[6004]
```

Autres méthodes possibles pour résoudre cette erreur:

* Vérifiez que le contrôleur de périphérique source est démarré en mode normal et que le système d’exploitation et le rôle de contrôleur de périphérique sur le contrôleur de périphérique source ont entièrement démarré.
* Vérifiez que le service domaine Active Directory est en cours d’exécution. Si le service est actuellement arrêté ou n’a pas été configuré avec des valeurs de démarrage par défaut, réinitialisez les valeurs de démarrage par défaut, redémarrez le contrôleur de périphérique modifié, puis recommencez l’opération.
* Vérifiez que la valeur de démarrage et l’état du service RPC et le localisateur RPC sont corrects pour la version du système d’exploitation du client RPC (DC de destination) et le serveur RPC (contrôleur de source). Si le service est actuellement arrêté ou n’a pas été configuré avec des valeurs de démarrage par défaut, réinitialisez les valeurs de démarrage par défaut, redémarrez le contrôleur de périphérique modifié, puis recommencez l’opération.
   * En outre, assurez-vous que le contexte de service correspond aux paramètres par défaut indiqués dans le tableau suivant.

      | de diffusion en continu | État par défaut (type de démarrage) dans Windows Server 2003 et versions ultérieures | État par défaut (type de démarrage) dans Windows Server 2000 |
      | --- | --- | --- |
      | Appel de procédure distante | Démarré (automatique) | Démarré (automatique) |
      | Localisateur d’appels de procédure distante | Null ou arrêté (manuel) | Démarré (automatique) |

* Vérifiez que la taille de la plage de ports dynamique n’a pas été restreinte. La syntaxe NETSH de Windows Server 2008 et Windows Server 2008 R2 pour énumérer la plage de ports RPC est indiquée ci-dessous:

   ```
   netsh int ipv4 show dynamicport tcp
   netsh int ipv4 show dynamicport udp
   netsh int ipv6 show dynamicport tcp
   netsh int ipv6 show dynamicport udp
   ```

* Vérifiez que les définitions de port codées en dur définies dans l’article KB 224196 se situent dans la plage de ports dynamiques pour la version du système d’exploitation DCs source. Passez en revue l' [article 224196](https://support.microsoft.com/kb/224196) de la base de connaissances et vérifiez que le port codé en dur se situe dans la plage de ports éphémères pour la version du système d’exploitation du contrôleur de réseau source.

* Vérifiez que la clé ClientProtocols existe sous HKLM\Software\Microsoft\Rpc et contient les 5 valeurs par défaut suivantes:

   ```
   ncacn_http REG_SZ rpcrt4.dll
   ncacn_ip_tcp REG_SZ rpcrt4.dll
   ncacn_nb_tcp REG_SZ rpcrt4.dll
   ncacn_np REG_SZ rpcrt4.dll
   ncacn_ip_udp REG_SZ rpcrt4.dll
   ```

## <a name="more-information"></a>Plus d’informations

Exemple de nom incorrect pour le mappage IP provoquant l’erreur RPC 1753 et-2146893022: le nom du principal cible est incorrect

Le domaine contoso.com est constitué de DC1 et de DC2 avec les adresses IP x. x. 1.1 et x. x. 1.2. Les enregistrements «A»/«AAAA» de l’hôte pour DC2 sont correctement inscrits sur tous les serveurs DNS configurés pour DC1. En outre, le fichier HOSTs sur DC1 contient une entrée mappage d’un nom d’hôte complet DC2s à l’adresse IP x. x. 1.2. Plus tard, l’adresse IP de DC2's passe de X. X. 1.2 à X. X. 1.3 et un nouvel ordinateur membre est joint au domaine avec l’adresse IP x. x. 1.2. Les tentatives de réplication Active Directory déclenchées par la commande **répliquer maintenant** dans Active Directory composant logiciel enfichable sites et services échouent avec l’erreur 1753, comme indiqué dans la trace ci-dessous:

```
F# SRC    DEST    Operation
1 x.x.1.1 x.x.1.2 ARP:Request, x.x.1.1 asks for x.x.1.2
2 x.x.1.2 x.x.1.1 ARP:Response, x.x.1.2 at 00-13-72-28-C8-5E
3 x.x.1.1 x.x.1.2 TCP:Flags=......S., SrcPort=50206, DstPort=DCE endpoint resolution(135)
4 x.x.1.2 x.x.1.1 ARP:Request, x.x.1.2 asks for x.x.1.1
5 x.x.1.1 x.x.1.2 ARP:Response, x.x.1.1 at 00-15-5D-42-2E-00
6 x.x.1.2 x.x.1.1 TCP:Flags=...A..S., SrcPort=DCE endpoint resolution(135)
7 x.x.1.1 x.x.1.2 TCP:Flags=...A...., SrcPort=50206, DstPort=DCE endpoint resolution(135)
8 x.x.1.1 x.x.1.2 MSRPC:c/o Bind: UUID{E1AF8308-5D1F-11C9-91A4-08002B14A0FA} EPT(EPMP)
9 x.x.1.2 x.x.1.1 MSRPC:c/o Bind Ack: Call=0x2 Assoc Grp=0x5E68 Xmit=0x16D0 Recv=0x16D0
10 x.x.1.1 x.x.1.2 EPM:Request: ept_map: NDR, DRSR(DRSR) {E3514235-4B06-11D1-AB04-00C04FC2DCD2} [DCE endpoint resolution(135)]
11 x.x.1.2 x.x.1.1 EPM:Response: ept_map: 0x16C9A0D6 - EP_S_NOT_REGISTERED
```

Au niveau de l’image **10**, le DC de destination interroge le mappeur de point de terminaison des contrôleurs de clés source sur le port 135 pour le Active Directory UUID de la classe de service de réplication...

Dans le frame **11**, le DC source, dans ce cas un ordinateur membre qui n’héberge pas encore le rôle DC et n’a donc pas inscrit le E351... L’UUID du service de réplication avec son compte EPM local répond avec une erreur symbolique EP_S_NOT_REGISTERED qui correspond à l’erreur décimale 1753, à l’erreur hexadécimale 0x6d9 et à l’erreur conviviale «aucun point de terminaison n’est disponible à partir du mappeur de point de terminaison».

Plus tard, l’ordinateur membre avec l’adresse IP x. x. 1.2 est promu en tant que réplica «MayberryDC» dans le domaine contoso.com. Là encore, la commande **Replica Now** est utilisée pour déclencher la réplication, mais cette fois-ci échoue avec l’erreur à l’écran «le nom du principal cible est incorrect.» L’ordinateur dont la carte réseau est affectée à l’adresse IP x. x. 1.2 est un contrôleur de domaine, est actuellement démarré en mode normal et a inscrit le E351... UUID du service de réplication avec son EPM local, mais il ne possède pas le nom ou l’identité de sécurité DC2 et ne peut pas déchiffrer la requête Kerberos à partir de DC1. la demande échoue alors avec l’erreur «le nom du principal cible est incorrect.» L’erreur est mappée à l’erreur décimale-2146893022/hex 0x80090322.

Des mappages d’hôte à IP non valides peuvent être provoqués par des entrées obsolètes dans les fichiers Host/Lmhost, pour héberger les inscriptions A/AAAA dans DNS ou WINS.

Résumé : Cet exemple a échoué, car un mappage hôte à IP non valide (dans le fichier hôte dans ce cas) a provoqué la résolution du contrôleur de domaine de destination en un contrôleur de domaine «source» qui n’avait pas le service Active Directory Domain Services en cours d’exécution (ou même installé) pour que la réplication Le SPN n’était pas encore inscrit et le contrôleur de service source a retourné l’erreur 1753. Dans le deuxième cas, un mappage hôte à IP non valide (encore une fois dans le fichier hôte) a entraîné la connexion du contrôleur de périphérique de destination à un contrôleur de périphérique qui avait enregistré le E351... nom de principal du service de réplication, mais cette source avait un nom d’hôte et une identité de sécurité différents de ceux du contrôleur de source prévu afin que les tentatives échouent avec l’erreur 2146893022: Le nom du principal cible est incorrect.

## <a name="related-topics"></a>Rubriques connexes

* [Dépannage des opérations de Active Directory qui échouent avec l’erreur 1753: Il n’y a plus de points de terminaison disponibles à partir du mappeur de point de terminaison.](https://support.microsoft.com/kb/2089874)
* [Article 839880 de la base de connaissances résolution des erreurs du mappeur de point de terminaison RPC à l’aide des outils de support Windows Server 2003 du CD du produit](https://support.microsoft.com/kb/839880)
* [Article 832017 de la base de connaissances présentation du service et exigences du port réseau pour le système Windows Server](https://support.microsoft.com/kb/832017/)
* [Article 224196 de la base de connaissances restriction du trafic de réplication Active Directory et du trafic RPC client vers un port spécifique](https://support.microsoft.com/kb/224196/)
* [Article 154596 de la base de connaissances comment configurer l’allocation de port dynamique RPC pour fonctionner avec des pare-feu](https://support.microsoft.com/kb/154596)
* [Fonctionnement de RPC](https://msdn.microsoft.com/library/aa373935(VS.85).aspx)
* [Préparation d’une connexion par le serveur](https://msdn.microsoft.com/library/aa373938(VS.85).aspx)
* [Comment le client établit une connexion](https://msdn.microsoft.com/library/aa373937(VS.85).aspx)
* [Inscription de l’interface](https://msdn.microsoft.com/library/aa375357(VS.85).aspx)
* [Mise à disposition du serveur sur le réseau](https://msdn.microsoft.com/library/aa373974(VS.85).aspx)
* [Inscription des points de terminaison](https://msdn.microsoft.com/library/aa375255(VS.85).aspx)
* [Écoute des appels clients](https://msdn.microsoft.com/library/aa373966(VS.85).aspx)
* [Comment le client établit une connexion](https://msdn.microsoft.com/library/aa373937(VS.85).aspx)
* [Restriction du trafic de réplication Active Directory et du trafic RPC client vers un port spécifique](https://support.microsoft.com/kb/224196)
* [Nom principal de service pour un contrôleur de service cible dans AD DS](https://msdn.microsoft.com/library/dd207688(PROT.13).aspx)
