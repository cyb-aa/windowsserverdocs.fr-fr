---
title: Dnscmd
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e7f31cb5-a426-4e25-b714-88712b8defd5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 39478e9b7dd8e8c69ed07f5d431486a7ed96b9cb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815500"
---
# <a name="dnscmd"></a>Dnscmd

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Une interface de ligne de commande pour la gestion des serveurs DNS. Cet utilitaire est utile dans les scripts de fichiers de commandes pour vous aider à automatiser les tâches de gestion DNS routine, ou pour effectuer une installation sans assistance simple et la configuration de nouveaux serveurs DNS sur votre réseau.  
## <a name="syntax"></a>Syntaxe  
```  
dnscmd <ServerName> <command> [<command parameters>]  
```  
## <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|<ServerName>|Nom de hôte ou adresse IP d’un serveur DNS local ou distant.|  
## <a name="commands"></a>Commandes  
|Command|Description|  
|------|--------|  
|[dnscmd /ageallrecords](#BKMK_1)|Définit l’heure actuelle sur tous les horodatages dans une zone ou d’un nœud.|  
|[dnscmd /clearcache](#BKMK_2)|Efface le cache du serveur DNS.|  
|[dnscmd /config](#BKMK_3)|Réinitialise la configuration de serveur ou une zone DNS.|  
|[dnscmd /createbuiltindirectorypartitions](#BKMK_4)|crée les partitions d’annuaire DNS application intégrées.|  
|[dnscmd /createdirectorypartition](#BKMK_5)|Crée une partition de répertoire d’applications DNS.|  
|[dnscmd /deletedirectorypartition](#BKMK_6)|Supprime une partition de répertoire d’applications DNS.|  
|[dnscmd /directorypartitioninfo](#BKMK_7)|Répertorie des informations sur une partition de répertoire d’applications DNS.|  
|[dnscmd /enlistdirectorypartition](#BKMK_8)|Ajoute un serveur DNS à l’ensemble de la réplication d’une partition de répertoire d’application DNS.|  
|[dnscmd /enumdirectorypartitions](#BKMK_9)|Répertorie les partitions de répertoire d’applications DNS pour un serveur.|  
|[dnscmd /enumrecords](#BKMK_10)|Répertorie les enregistrements de ressources dans une zone.|  
|[dnscmd /enumzones](#BKMK_11)|Répertorie les zones hébergées par le serveur spécifié.|  
|[dnscmd /exportsettings](#BKMK_25a)|Écrit les informations de configuration de serveur dans un fichier texte.|  
|[dnscmd /info](#BKMK_12)|Obtient des informations sur le serveur.|  
|[dnscmd /ipvalidate](#BKMK_29a)|Valide les serveurs DNS à distance.|  
|[dnscmd /nodedelete](#BKMK_13)|Supprime tous les enregistrements pour un nœud dans une zone.|  
|[dnscmd /recordadd](#BKMK_14)|Ajoute un enregistrement de ressource à une zone.|  
|[dnscmd /recorddelete](#BKMK_15)|Supprime un enregistrement de ressource à partir d’une zone.|  
|[dnscmd /resetforwarders](#BKMK_16)|Définit les serveurs DNS pour transférer les requêtes récursives.|  
|[dnscmd /resetlistenaddresses](#BKMK_17)|Définit les adresses IP du serveur pour traiter les demandes DNS.|  
|[dnscmd /startscavenging](#BKMK_18)|Lance le nettoyage de serveur.|  
|[dnscmd /statistics](#BKMK_19)|Les requêtes ou efface les données des statistiques de serveur.|  
|[dnscmd /unenlistdirectorypartition](#BKMK_20)|Supprime un serveur DNS de l’ensemble de la réplication d’une partition d’annuaire DNS application.|  
|[dnscmd /writebackfiles](#BKMK_21)|Enregistre toutes les données de zone ou indication de racine dans un fichier.|  
|[dnscmd /zoneadd](#BKMK_22)|Crée une nouvelle zone sur le serveur DNS.|  
|[dnscmd /zonechangedirectorypartition](#BKMK_23)|Modifie la partition d’annuaire sur lequel réside une zone.|  
|[dnscmd /zonedelete](#BKMK_24)|Supprime une zone à partir du serveur DNS.|  
|[dnscmd /zoneexport](#BKMK_25)|Écrit les enregistrements de ressources d’une zone dans un fichier texte.|  
|[dnscmd /zoneinfo](#BKMK_26)|Affiche des informations de zone.|  
|[dnscmd /zonepause](#BKMK_27)|Suspend une zone.|  
|[dnscmd /zoneprint](#BKMK_28)|Affiche tous les enregistrements dans la zone.|  
|[dnscmd /zonerefresh](#BKMK_30)|force une actualisation de la zone secondaire à partir de la zone principale.|  
|[dnscmd /zonereload](#BKMK_31)|Recharge une zone à partir de sa base de données.|  
|[dnscmd /zoneresetmasters](#BKMK_32)|Modifie les serveurs maîtres qui fournissent des informations de transfert de zone à une zone secondaire.|  
|[dnscmd /zoneresetscavengeservers](#BKMK_33)|Modifie les serveurs que vous pouvant nettoyer une zone.|  
|[dnscmd /zoneresetsecondaries](#BKMK_34)|Réinitialise les informations secondaires pour une zone.|  
|[dnscmd /zoneresettype](#BKMK_29)|modifie le type de zone.|  
|[dnscmd /zoneresume](#BKMK_35)|Reprend une zone.|  
|[dnscmd /zoneupdatefromds](#BKMK_36)|Met à jour une zone intégrée à active directory avec des données à partir des Services de domaine active directory (AD DS).|  
|[dnscmd /zonewriteback](#BKMK_37)|Enregistre les données de zone dans un fichier.|  
### <a name="BKMK_1"></a>dnscmd /ageallrecords  
Définit l’heure actuelle sur un horodatage sur les enregistrements de ressource à un nœud sur un serveur DNS ou la zone spécifiée.  
#### <a name="syntax"></a>Syntaxe  
```  
dnscmd [<ServerName>] /ageallrecords <ZoneName>[<NodeName>] | [/tree]|[/f]  
```  
#### <a name="parameters"></a>Paramètres  
<ServerName>  
Spécifie le serveur DNS que l’administrateur souhaite gérer, représenté par l’adresse IP, le nom de domaine complet (FQDN) ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
<ZoneName>  
Spécifie le nom de domaine complet de la zone.  
<NodeName>  
Spécifie un nœud spécifique ou une sous-arborescence dans la zone. *NodeName* spécifie la sous-arborescence ou le nœud dans la zone en utilisant les éléments suivants :  
-   @ pour la zone racine ou le nom de domaine complet  
-   Le nom de domaine complet d’un nœud (le nom par un point (.) à la fin)  
-   Une étiquette unique pour le nom relatif à la racine de la zone  
/Tree  
Spécifie que tous les nœuds enfants reçoivent également l’horodatage.  
/f  
Exécute la commande sans demander confirmation.  
#### <a name="remarks"></a>Notes  
-   Le **ageallrecords** commande est pour la compatibilité descendante entre la version actuelle de DNS et les versions précédentes de DNS dans lequel de vieillissement et ne étaient pas pris en charge. Il ajoute un horodatage avec l’heure actuelle à des enregistrements de ressource qui n’ont pas un horodatage, et il définit l’heure actuelle sur les enregistrements de ressource qui n’ont pas un horodatage.  
-   Le nettoyage des enregistrements ne se produit pas, sauf si les enregistrements sont horodatés. Nom des enregistrements de ressource, début des enregistrements de ressource d’autorité (principale SOA), serveur de (noms NS) et les enregistrements de ressource Windows WINS Internet Name Service () ne sont pas inclus dans le processus de nettoyage, et ils ne sont pas horodatée même lorsque le **ageallrecords**commande s’exécute.  
-   Cette commande échoue, sauf si le nettoyage est activé pour le serveur DNS et de la zone. Pour plus d’informations sur l’activation du nettoyage de la zone, consultez la **vieillissement** paramètre sous la syntaxe de niveau de Zone dans le [config](#BKMK_3) commande.  
-   L’ajout d’un horodatage pour les enregistrements de ressource DNS les rend incompatibles avec les serveurs DNS qui s’exécutent sur les systèmes d’exploitation autres que Windows 2000, Windows XP ou Windows Server 2003. Un horodatage que vous ajoutez à l’aide de la **ageallrecords** commande ne peut pas être annulée.  
-   Si les paramètres facultatifs sont spécifiés, la commande retourne tous les enregistrements de ressource au niveau du nœud spécifié. Si une valeur est spécifiée pour au moins un des paramètres facultatifs, **dnscmd** énumère uniquement les enregistrements de ressources qui correspondent à la valeur ou les valeurs qui sont spécifiées dans l’ou les paramètres facultatifs.  
#### <a name="example"></a>Exemple  
Consultez [exemple 1 : Définir l’heure actuelle sur un horodatage pour les enregistrements de ressource](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_2"></a>dnscmd /clearcache  
Efface la mémoire cache DNS des enregistrements de ressources sur le serveur DNS spécifié.  
#### <a name="syntax"></a>Syntaxe  
```  
dnscmd [<ServerName>] /clearcache  
```  
#### <a name="parameters"></a>Paramètres  
<ServerName>  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
#### <a name="sample-usage"></a>Exemple d’utilisation  
`dnscmd dnssvr1.contoso.com /clearcache`  
### <a name="BKMK_3"></a>dnscmd /config  
Modifie les valeurs dans le Registre pour le serveur DNS et les zones individuelles. Accepte les paramètres au niveau du serveur et les paramètres de niveau de zone.  
> [!CAUTION]  
> Ne modifiez pas directement le Registre, sauf si vous ne disposez d’aucune alternative. L’Éditeur du Registre ignore les protections standards, autorise des paramètres qui peuvent dégrader les performances, endommager votre système ou même vous obliger à réinstaller Windows. Vous pouvez modifier en toute sécurité de la plupart des paramètres de Registre à l’aide de programmes dans le panneau de configuration ou de Microsoft Management Console (mmc). Si vous devez modifier le Registre directement, faire en premier. Lire l’aide de l’Éditeur du Registre pour plus d’informations.  
#### <a name="server-level-syntax"></a>Syntaxe de niveau serveur  
```  
dnscmd [<ServerName>] /config <Parameter>  
```  
#### <a name="dnscmd-config"></a>dnscmd /config  
Modifie la configuration du serveur spécifié.  
#### <a name="parameters"></a>Paramètres  
<ServerName>  
Spécifie le serveur DNS que vous envisagez de gérer, représenté par la syntaxe de l’ordinateur local, adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
<Parameter>  
Spécifier un paramètre et, éventuellement, une valeur. Les valeurs de paramètre, utilisez cette syntaxe : *Parameter* [*Value*]  
Les valeurs de paramètre suivantes sont décrites dans le reste de cette section :  
-   **/addressanswerlimit**  
-   **/bindsecondaries**  
-   **/bootmethod**  
-   **/defaultagingstate**  
-   **/defaultnorefreshinterval**  
-   **/defaultrefreshinterval**  
-   **/disableautoreversezones**  
-   **/disablensrecordsautocreation**  
-   **/dspollinginterval**  
-   **/dstombstoneinterval**  
-   **/ednscachetimeout**  
-   **/enablednsprobes**  
-   **/enablednssec**  
-   **/enableglobalnamessupport**  
-   **/enableglobalqueryblocklist**  
-   **/eventloglevel**  
-   **/forwarddelegations**  
-   **/forwardingtimeout**  
-   **/globalnamesqueryorder**  
-   **/globalqueryblocklist**  
-   **/isslave**  
-   **/localnetpriority**  
-   **/logfilemaxsize**  
-   **/logfilepath**  
-   **/logipfilterlist**  
-   **/loglevel**  
-   **/maxcachesize**  
-   **/maxcachettl**  
-   **/namecheckflag**  
-   **/notcp**  
-   **/norecursion**  
-   **/recursionretry**  
-   **/recursiontimeout**  
-   **/roundrobin**  
-   **/rpcprotocol**  
-   **/scavenginginterval**  
-   **/secureresponses**  
-   **/sendport**  
-   **/strictfileparsing**  
-   **/updateoptions**  
-   **/writeauthorityns**  
-   **/xfrconnecttimeout**  
**/addressanswerlimit [0|5-28]**  
Spécifie le nombre maximal d’enregistrements d’hôte un serveur DNS capable d’envoyer en réponse à une requête. La valeur peut être zéro (0), ou il peut être dans la plage de 5 à 28 enregistrements. La valeur par défaut est zéro (0).  
**/bindsecondaries[0|1]**  
modifie le format du transfert de zone afin qu’il peut atteindre l’efficacité et compression maximum. Toutefois, ce format n’est pas compatible avec les versions antérieures de nom de domaine BIND (Berkeley Internet).  
**0**  
Utilise la compression maximale. Ce format est compatible avec les versions de BIND 4.9.4 et versions ultérieures uniquement.  
**1**  
Envoie l’enregistrement de ressource qu’un seul par message vers les serveurs DNS non-Microsoft. Ce format est compatible avec les versions de liaison antérieures que 4.9.4. Il s’agit du paramètre par défaut.  
**/bootmethod[0|1|2|3]**  
Détermine la source à partir de laquelle le serveur DNS obtient ses informations de configuration.  
**0**  
Efface la source d’informations de configuration.  
**1**  
Charge à partir du fichier de liaison qui se trouve dans le répertoire DNS, qui est %systemroot%\System32\DNS par défaut.  
**2**  
Charge à partir du Registre.  
**3**  
Charges à partir des services AD DS et le Registre. Il s’agit du paramètre par défaut.  
**/defaultagingstate[0|1]**  
Détermine si la fonctionnalité de nettoyage DNS est activée par défaut sur les zones nouvellement créés.  
**0**  
Désactive le nettoyage. Il s’agit du paramètre par défaut.  
**1**  
Active le nettoyage.  
**/defaultnorefreshinterval[0x1-0xFFFFFFFF|0xA8]**  
Définit une période de temps dans laquelle aucun actualisations ne sont acceptées pour les enregistrements mis à jour dynamiquement. Zones sur le serveur héritent automatiquement de cette valeur. Pour modifier la valeur par défaut, tapez une valeur dans la plage de **0 x 1-0xFFFFFFFF**. La valeur par défaut à partir du serveur est **0xA8**.  
**/defaultrefreshinterval [0x1-0xFFFFFFFF|0xA8]**  
Définit une période de temps autorisé pour les mises à jour dynamiques des enregistrements DNS. Zones sur le serveur héritent automatiquement de cette valeur. Pour modifier la valeur par défaut, tapez une valeur dans la plage de **0 x 1-0xFFFFFFFF**. La valeur par défaut à partir du serveur est **0xA8**.  
**/disableautoreversezones [0|1]**  
Active ou désactive la création automatique de zones de recherche inversée. Zones de recherche inversée assurent la résolution d’adresses IP (Internet Protocol) aux noms de domaine DNS.  
**0**  
Permet la création automatique de zones de recherche inversée. Il s’agit du paramètre par défaut.  
**1**  
Désactive la création automatique de zones de recherche inversée.  
**/disablensrecordsautocreation {0|1}**  
Spécifie si le serveur DNS crée automatiquement le serveur de noms (NS) des enregistrements de ressource pour les zones hébergées par celui-ci.  
**0**  
Crée automatiquement les enregistrements de ressource de nom serveur de (noms NS) pour les zones qui les hôtes de serveur DNS.  
**1**  
Ne crée pas automatiquement enregistrements de ressource de nom serveur de (noms NS) pour les zones qui les hôtes de serveur DNS.  
**/dspollinginterval 0-30**  
Spécifie la fréquence à laquelle les zones intégrées dans le DNS server a interrogé AD DS pour les modifications dans active directory.  
**/dstombstoneinterval [1-30]**  
La durée en secondes pendant laquelle conserver les enregistrements supprimés dans AD DS.  
**/ednscachetimeout [<seconds>]**  
Spécifie le nombre de secondes pendant lesquelles les informations étendues DNS (EDNS) sont mises en cache. La valeur minimale est de 3600, et la valeur maximale est 15,724,800. La valeur par défaut est de 604 800 secondes (une semaine).  
**/enableednsprobes {0 | 1}**  
Active ou désactive le serveur pour une sonde d’autres serveurs pour déterminer si elles prennent en charge EDNS.  
**0**  
Désactive active la prise en charge pour les sondes EDNS.  
**1**  
Active active la prise en charge pour les sondes EDNS.  
**/enablednssec {0|1}**  
Active ou désactive la prise en charge des Extensions de sécurité DNS (DNSSEC).  
**0**  
Désactive la technologie DNSSEC.  
**1**  
Permet de DNSSEC.  
**/enableglobalnamessupport {0|1}**  
Active ou désactive la prise en charge de la zone GlobalNames. La zone GlobalNames prend en charge la résolution de noms DNS en une partie dans une forêt.  
**0**  
Désactive la prise en charge pour la zone GlobalNames. Lorsque vous définissez la valeur de cette commande vers **0**, le service serveur DNS ne résout pas les noms en une partie dans la zone GlobalNames.  
**1**  
Il prend en charge la zone GlobalNames. Lorsque vous définissez la valeur de cette commande vers **1**, le service serveur DNS résout les noms en une partie dans la zone GlobalNames.  
**/enableglobalqueryblocklist {0|1}**  
Active ou désactive la prise en charge de la liste de blocage de requête globale qui bloque la résolution des noms dans la liste. Le service serveur DNS crée et Active la liste rouge de requêtes globale par défaut, lorsque le service démarre la première fois. Pour afficher la liste rouge de requêtes globale à l’adresse actuelle, utilisez la **dnscmd /info /globalqueryblocklist** commande.  
**0**  
Désactive la prise en charge pour la liste rouge de requêtes globale. Lorsque vous définissez la valeur de cette commande vers **0**, le service serveur DNS répond aux requêtes de noms dans la liste de blocs.  
**1**  
Il prend en charge la liste rouge de requêtes globale. Lorsque vous définissez la valeur de cette commande vers **1**, le service serveur DNS ne répond pas aux requêtes de noms dans la liste de blocs.  
**/eventloglevel [0|1|2|4]**  
Détermine quels événements sont consignés dans le journal du serveur DNS dans l’Observateur d’événements.  
**0**  
N’enregistre aucun événement.  
**1**  
Enregistre uniquement les erreurs.  
**2**  
Enregistre uniquement les erreurs et avertissements.  
**4**  
Journalise les erreurs, avertissements et événements d’information. Il s’agit du paramètre par défaut.  
**/forwarddelegations [0 | 1]**  
Détermine comment le serveur DNS gère une requête pour une sous-zone déléguée. Ces requêtes peuvent être envoyés à la sous-zone est appelée dans la requête ou à la liste des redirecteurs qui est nommée pour le serveur DNS. Entrées dans le paramètre sont utilisées uniquement lorsque le transfert est activé.  
**0**  
Envoie automatiquement les requêtes qui font référence à des sous-zones déléguées à la sous-zone appropriée. Il s’agit du paramètre par défaut.  
**1**  
transfère les requêtes qui font référence à la sous-zone déléguée aux redirecteurs existants.  
**/forwardingtimeout [<seconds>]**  
Détermine le nombre de secondes (0 x 1-0xFFFFFFFF) un serveur DNS attend un redirecteur de répondre avant d’essayer de transfert sur un autre. La valeur par défaut est **0 x 5**, qui est de 5 secondes.  
**/globalneamesqueryorder** {**0|1**}  
Spécifie si le service serveur DNS recherche d’abord dans la zone GlobalNames ou zones locales lorsqu’il résout des noms.  
**0**  
Le service serveur DNS tente de résoudre les noms en interrogeant la zone GlobalNames avant elle interroge les zones pour lesquelles il fait autorité.  
**1**  
Le service serveur DNS tente de résoudre les noms en interrogeant les zones pour lesquelles il fait autorité avant qu’il interroge la zone GlobalNames.  
**/globalqueryblocklist[[<name> [<name>]...]**  
remplace la liste rouge de requêtes globale à l’adresse actuelle avec une liste de noms que vous spécifiez. Si vous ne spécifiez pas de tous les noms, cette commande efface la liste de blocs. Par défaut, la liste rouge de requêtes globale contient les éléments suivants :  
-   ISATAP  
-   WPAD  
Le service serveur DNS peut supprimer ou pour les deux de ces noms lorsqu’il démarre la première fois, si elle trouve ces noms dans une zone existante.  
**/isslave [0|1]**  
Détermine comment le serveur DNS répond lorsqu’il transfère des requêtes ne reçoivent aucune réponse.  
**0**  
Spécifie que le serveur DNS n’est pas un subordonné (également appelé un *subordonné*). Si le redirecteur ne répond pas, le serveur DNS tente de résoudre la requête elle-même. Il s’agit du paramètre par défaut.  
**1**  
Spécifie que le serveur DNS est un subordonné. Si le redirecteur ne répond pas, le serveur DNS met fin à la recherche et envoie un message d’échec au programme de résolution.  
**/localnetpriority [0 | 1]**  
Détermine l’ordre dans lequel les enregistrements d’hôte sont retournés lorsque le serveur DNS a plusieurs enregistrements d’hôte pour le même nom.  
**0**  
Renvoie les enregistrements dans l’ordre dans lequel ils sont répertoriés dans la base de données DNS.  
**1**  
Retourne les enregistrements ayant une adresse IP similaire tout d’abord les adresses réseau. Il s’agit du paramètre par défaut.  
**/logfilemaxsize [<size>]**  
Spécifie la taille maximale en octets (0 x 10000-0xFFFFFFFF) du fichier Dns.log. Lorsque le fichier atteint sa taille maximale, DNS remplace les événements les plus anciens. La taille par défaut est 0 x 400000, ce qui est de 4 mégaoctets (Mo).  
**/logfilepath [<path+LogFileName>]**  
Spécifie le chemin d’accès du fichier Dns.log. Le chemin d’accès par défaut est % systemroot%\System32\Dns\Dns.log. Vous pouvez spécifier un autre chemin d’accès en utilisant le format *chemin d’accès + nom fichier journal*.  
**/logipfilterlist <IPaddress> [,<IPaddress>...]**  
Spécifie les paquets qui sont enregistrés dans le fichier journal de débogage. Les entrées sont une liste d’adresses IP. Uniquement les paquets qui passent vers et depuis les adresses IP dans la liste sont enregistrés.  
**/loglevel [<Eventtype>]**  
Détermine quels types d’événements sont enregistrés dans le fichier Dns.log. Chaque type d’événement est représenté par un nombre hexadécimal. Si vous souhaitez que plusieurs événements dans le journal, utilisez hexadécimale addition pour ajouter les valeurs, puis entrez la somme.  
**0x0**  
Le serveur DNS ne crée pas un journal. Il s’agit de l’entrée par défaut.  
**0x10**  
Enregistre les requêtes.  
**0x10**  
Journaux des notifications.  
**0x20**  
Enregistre les mises à jour.  
**0xFE**  
Journaux de transactions de nonquery.  
**0x100**  
Question de journaux de transactions.  
**0x200**  
Enregistre des réponses.  
**0x1000**  
Journaux d’envoient des paquets.  
**0x2000**  
Journaux de recevoir des paquets.  
**0x4000**  
Enregistre les paquets du protocole UDP (User Datagram).  
**0x8000**  
Enregistre les paquets du protocole TCP (Transmission Control).  
**0xFFFF**  
Enregistre tous les paquets.  
**0x10000**  
Journaux de transactions d’écriture active directory.  
**0x20000**  
Journaux de transactions de mise à jour d’active directory.  
**0x1000000**  
Journaux de paquets complètes.  
**0x80000000**  
Les journaux à double écriture des transactions.  
**/maxcachesize**  
Spécifie la taille maximale, en kilo-octets (Ko), du cache mémoire DNS server s.  
**/maxcachettl [<seconds>]**  
Détermine le nombre de secondes (0 x 0-0xFFFFFFFF) un enregistrement dans le cache. Si le **0 x 0** est utilisé, le serveur DNS ne met pas en cache les enregistrements. Le paramètre par défaut est **0 x 15180** (86 400 secondes ou 1 jour).  
**/maxnegativecachettl [<seconds>]**  
Spécifie le nombre de secondes (0 x 1-0xFFFFFFFF) une entrée qui enregistre une réponse négative à une requête reste stockée dans le cache DNS. Le paramètre par défaut est **0x384** (900 secondes).  
**/namecheckflag [0|1|2|3]**  
Spécifie quels standard de caractère est utilisé lors de la vérification des noms DNS.  
**0**  
Utilise les caractères ANSI qui sont conformes aux Internet Engineering Task force (IETF) de demande de commentaires (RFC).  
**1**  
Utilise des caractères ANSI qui ne répondent pas nécessairement avec IETF RFC.  
**2**  
Utilise multioctets UCS Transformation format 8 caractères (UTF-8). Il s’agit du paramètre par défaut.  
**3**  
Utilise tous les caractères.  
**/norecursion [0|1]**  
Détermine si un serveur DNS effectue la résolution de noms récursive.  
**0**  
Le serveur DNS exécute la résolution de noms récursive si elles sont demandées dans une requête. Il s’agit du paramètre par défaut.  
**1**  
Le serveur DNS n’effectue pas de résolution de noms récursive.  
**/notcp**  
Ce paramètre est obsolète et il n’a aucun effet dans les versions actuelles de Windows Server.  
**/recursionretry [<seconds>]**  
Détermine le nombre de secondes (0 x 1-0xFFFFFFFF) un serveur DNS attend avant d’essayer à nouveau de contacter un serveur distant. Le paramètre par défaut est 0 x 3 (trois secondes). Cette valeur doit être augmentée lors de la récursivité se produit via une liaison de réseau (étendu WAN) lentes.  
**/recursiontimeout [<seconds>]**  
Détermine le nombre de secondes (0 x 1-0xFFFFFFFF) un serveur DNS attend avant de mettre fin tente de contacter un serveur distant. La plage de paramètres à partir de **0 x 1** via **0xFFFFFFFF**. Le paramètre par défaut est **0xF** (15 secondes). Cette valeur doit être augmentée lors de la récursivité se produit via une liaison WAN lente.  
**/roundrobin [0|1]**  
Détermine l’ordre dans lequel les enregistrements d’hôte sont renvoyés lorsqu’un serveur a plusieurs enregistrements d’hôte pour le même nom.  
0  
Le serveur DNS n’utilise pas de tourniquet (Round Robin). Au lieu de cela, elle retourne le premier enregistrement à chaque requête.  
**1**  
Le serveur DNS effectue une rotation entre les enregistrements qu’il renvoie à partir du haut vers le bas de la liste des enregistrements correspondants. Il s’agit du paramètre par défaut.  
**/RpcProtocol [0 x 0 | 0 x 1 | 0 x 2 | 0 x 4 | 0xFFFFFFFF]**  
Spécifie le protocole utilisé par l’appel de procédure distante (RPC) lorsqu’il établit une connexion à partir du serveur DNS.  
**0x0**  
Désactive le RPC de DNS.  
**0x1**  
Utilise le protocole TCP/IP.  
**0x2**  
Utilise des canaux nommés.  
**0x4**  
Utilise l’appel de procédure local (LPC).  
**0xFFFFFFFF**  
Tous les protocoles. Il s’agit du paramètre par défaut.  
**/scavenginginterval [<hours>]**  
Détermine si la fonctionnalité de nettoyage pour le serveur DNS est activée et définit le nombre d’heures (0 x 0-0xFFFFFFFF) entre les cycles de nettoyage. Le paramètre par défaut est **0 x 0**, qui désactive le nettoyage pour le serveur DNS. Un paramètre supérieur à **0 x 0** permet le nettoyage pour le serveur et définit le nombre d’heures entre les cycles de nettoyage.  
**/secureresponses [0|1]**  
Détermine si DNS filtre les enregistrements qui sont enregistrés dans un cache.  
**0**  
Enregistre toutes les réponses aux requêtes de noms dans un cache. Il s’agit du paramètre par défaut.  
**1**  
Enregistre uniquement les enregistrements qui appartiennent à la même sous-arbre DNS à un cache.  
**/sendport [<port>]**  
Spécifie le numéro de port (0 x 0-0xFFFFFFFF) DNS utilise pour envoyer des requêtes récursives à d’autres serveurs DNS. Le paramètre par défaut est **0 x 0**, ce qui signifie que le numéro de port est sélectionné au hasard.  
**/serverlevelplugindll[<Dllpath>]**  
Spécifie le chemin d’accès d’un plug-in personnalisé. Lorsque *Dllpath* Spécifie le nom de chemin d’accès qualifié complet d’un plug-in, le serveur DNS appelle des fonctions dans le plug-in pour résoudre les requêtes de noms situés en dehors de la portée de tous les localement les zones hébergées de serveur DNS valid. Si une requête de nom n’est pas le plug-in, le serveur DNS exécute la résolution de noms à l’aide de transfert ou une récursivité, tel que configuré. Si *Dllpath* n’est pas spécifié, le serveur DNS cesse d’utiliser un plug-in personnalisé si un plug-in personnalisé a été précédemment configuré.  
**/strictfileparsing [0 | 1]**  
Détermine le comportement d’un serveur DNS quand il rencontre un enregistrement erroné lors du chargement d’une zone.  
**0**  
Le serveur DNS continue à charger la zone même si le serveur rencontre un enregistrement erroné. L’erreur est enregistrée dans le journal DNS. Il s’agit du paramètre par défaut.  
**1**  
Le serveur DNS arrête le chargement de la zone, et il enregistre l’erreur dans le journal DNS.  
**/UpdateOptions <RecordValue>**  
Interdit les mises à jour dynamiques des types spécifiés d’enregistrements. Si vous souhaitez plus d’un type d’enregistrement pour être interdit dans le journal, utilisez hexadécimale addition pour ajouter les valeurs, puis entrez la somme.  
**0x0**  
Ne limite pas les types d’enregistrements.  
**0x1**  
Exclut le début des enregistrements de ressource d’autorité (principale SOA).  
**0x2**  
Exclut les enregistrements de ressource de nom du serveur de (noms NS).  
**0x4**  
Exclut la délégation des enregistrements de ressource de nom du serveur de (noms NS).  
**0x8**  
Exclut les enregistrements d’hôte de serveur.  
**0x100**  
Au cours de la mise à jour dynamique sécurisée, exclut le début des enregistrements de ressource d’autorité (principale SOA).  
**0x200**  
Au cours de la mise à jour dynamique sécurisée, exclut les enregistrements de ressource de serveur de (noms NS) de nom racine.  
**0x30F**  
Au cours de la mise à jour dynamique standard, exclut les enregistrements de ressource de serveur de (noms NS) nom, début d’enregistrements de ressource d’autorité (principale SOA) et les enregistrements d’hôte de serveur. Au cours de la mise à jour dynamique sécurisée, exclut les enregistrements de ressource de serveur de (noms NS) de nom racine et le début des enregistrements de ressource d’autorité (principale SOA). Permet des délégations et serveur héberger les mises à jour.  
**0x400**  
Au cours de la mise à jour dynamique sécurisée, exclut les enregistrements de ressource de délégation nom du serveur de (noms NS).  
**0x800**  
Au cours de la mise à jour dynamique sécurisée, exclut les enregistrements d’hôte de serveur.  
**0x1000000**  
Exclut les enregistrements de délégation DS (Delegation).  
**0x80000000**  
Désactive la mise à jour dynamique DNS.  
**/writeauthorityns [0|1]**  
Détermine quand le serveur DNS écrit les enregistrements de ressource de nom du serveur de (noms NS) dans la section de l’autorité d’une réponse.  
**0**  
Écrit les enregistrements de ressource de nom du serveur de (noms NS) dans la section de l’autorité de redirections uniquement. Ce paramètre est conforme avec Rfc 1034, les fonctionnalités d’et les concepts de noms de domaine et Rfc 2181, Clarifications pour la spécification de DNS. Il s’agit du paramètre par défaut.  
**1**  
Écrit les enregistrements de ressource de nom du serveur de (noms NS) dans la section de l’autorité de toutes les réponses faisant autorités réussites.  
**/xfrconnecttimeout [<seconds>]**  
Détermine le nombre de secondes (0 x 0-0xFFFFFFFF), qu'un serveur DNS principal attend une réponse de transfert à partir de son serveur secondaire. La valeur par défaut est **0x1E** (30 secondes). Une fois que la valeur de délai d’attente expire, la connexion est interrompue.  
#### <a name="zone-level-syntax"></a>Syntaxe de niveau de zone  
```  
dnscmd /config <Parameters>  
```  
#### <a name="dnscmd-config"></a>dnscmd /config  
Modifie la configuration de la zone spécifiée.  
#### <a name="parameters"></a>Paramètres  
**<Parameters>**  
Spécifier un paramètre, une zone Nom et, en tant qu’option, une valeur. Les valeurs de paramètre, utilisez cette syntaxe : *ZoneName Parameter* [*Value*]  
Les valeurs de paramètre suivantes sont décrites dans le reste de cette section :  
-   **/aging**  
-   **/allownsrecordsautocreation**  
-   **/allowupdate**  
-   **/forwarderslave**  
-   **/forwardertimeout**  
-   **/norefreshinterval**  
-   **/refreshinterval**  
-   **/securesecondaries**  
**/ chronologique <ZoneName>**  
Active ou désactive le nettoyage dans une zone spécifique.  
**/allownsrecordsautocreation  <ZoneName> [<Value>]**  
Remplace nom du serveur de (noms NS) ressources enregistrements création automatique du serveur DNS de paramètre. Enregistrement de ressource de serveur de (noms NS) nom qui était auparavant inscrites pour cette zone n’est pas affectés. Par conséquent, vous devez les supprimer manuellement si vous souhaitez interdire.  
**/allowupdate <ZoneName>**  
Détermine si la zone spécifiée accepte des mises à jour dynamiques.  
**/forwarderslave <ZoneName>**  
Remplace le serveur DNS **/isslave** paramètre.  
**/forwardertimeout <ZoneName>**  
Détermine le nombre de secondes une zone DNS attend un redirecteur de répondre avant d’essayer de transfert sur un autre. Cette valeur remplace la valeur est définie au niveau du serveur.  
**/NoRefreshInterval <ZoneName>**  
Définit un intervalle de temps pour une zone au cours de laquelle aucun actualisations ne peuvent mettre à jour dynamiquement les enregistrements DNS dans une zone spécifiée.  
**/RefreshInterval <ZoneName>**  
Définit un intervalle de temps pour une zone au cours de laquelle les actualisations peuvent mettre à jour dynamiquement les enregistrements DNS dans une zone spécifiée.  
**/securesecondaries <ZoneName>**  
Détermine les serveurs secondaires peuvent recevoir des mises à jour de la zone à partir du serveur maître pour cette zone.  
#### <a name="remarks"></a>Notes  
-   Le nom de la zone doit être spécifié uniquement pour les paramètres de niveau de zone.  
### <a name="BKMK_4"></a>dnscmd /createbuiltindirectorypartitions  
Crée une partition de répertoire d’applications DNS. Lorsque le service DNS est installé, le service dans une partition d’annuaire de l’application est créée au niveau forêt et domaine. Cette commande permet de créer des partitions d’annuaire d’applications DNS qui ont été supprimées ou jamais été créées. Sans paramètre, cette commande crée une partition d’annuaire DNS intégrée pour le domaine.  
#### <a name="syntax"></a>Syntaxe  
```  
dnscmd [<ServerName>] /createbuiltindirectorypartitions [/forest] [/alldomains]   
```  
#### <a name="parameters"></a>Paramètres  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**/forest**  
Crée une partition d’annuaire DNS pour la forêt.  
**/alldomains**  
crée des partitions pour tous les domaines DNS dans la forêt.  
### <a name="BKMK_5"></a>dnscmd /createdirectorypartition  
Crée une partition de répertoire d’applications DNS. Lorsque le service DNS est installé, le service dans une partition d’annuaire de l’application est créée au niveau forêt et domaine. Cette opération crée des partitions d’annuaire application DNS supplémentaires.  
#### <a name="syntax"></a>Syntaxe  
```  
dnscmd [<ServerName>] /createdirectorypartition <PartitionFQDN>  
```  
#### <a name="parameters"></a>Paramètres  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<PartitionFQDN>**  
Le nom de domaine complet de la partition de répertoire d’applications DNS qui sera créée.  
### <a name="BKMK_6"></a>dnscmd /deletedirectorypartition  
Supprime une partition de répertoire d’application DNS existante.  
#### <a name="syntax"></a>Syntaxe  
```  
dnscmd [<ServerName>] /deletedirectorypartition <PartitionFQDN>  
```  
#### <a name="parameters"></a>Paramètres  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<PartitionFQDN>**  
Le nom de domaine complet de la partition de répertoire d’applications DNS qui sera supprimée.  
### <a name="BKMK_7"></a>dnscmd /directorypartitioninfo  
Répertorie des informations sur une partition de répertoire d’application DNS spécifiée.  
#### <a name="syntax"></a>Syntaxe  
```  
dnscmd [<ServerName>] /directorypartitioninfo <PartitionFQDN> [/detail]   
```  
#### <a name="parameters"></a>Paramètres  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<PartitionFQDN>**  
Le nom de domaine complet de la partition de répertoire d’applications DNS.  
**/detail**  
Répertorie toutes les informations sur la partition d’annuaire d’application.  
### <a name="BKMK_8"></a>dnscmd /enlistdirectorypartition  
Ajoute le serveur DNS pour le jeu de réplicas de la partition de répertoire spécifié.  
#### <a name="syntax"></a>Syntaxe  
```  
dnscmd [<ServerName>] /enlistdirectorypartition <PartitionFQDN>  
```  
#### <a name="parameters"></a>Paramètres  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<PartitionFQDN>**  
Le nom de domaine complet de la partition de répertoire d’applications DNS.  
### <a name="BKMK_9"></a>dnscmd /enumdirectorypartitions  
Répertorie les partitions de répertoire d’applications DNS pour le serveur spécifié.  
#### <a name="syntax"></a>Syntaxe  
```  
dnscmd [<ServerName>] /enumdirectorypartitions [/custom]   
```  
#### <a name="parameters"></a>Paramètres  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**/custom**  
répertorie uniquement les partitions d’annuaire créés par l’utilisateur.  
### <a name="BKMK_10"></a>dnscmd /enumrecords  
Répertorie les enregistrements de ressources d’un nœud spécifié dans une zone DNS.  
#### <a name="syntax"></a>Syntaxe  
```  
dnscmd [<ServerName>] /enumrecords <ZoneName> <NodeName> [/type <RRtype> <Rrdata>] [/authority] [/glue] [/additional] [/node | /child | /startchild<ChildName>] [/continue | /detail]   
```  
#### <a name="parameters"></a>Paramètres  
**<ServerName>**  
Spécifie le serveur DNS que vous souhaitez gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**/enumrecords**  
Répertorie les enregistrements de ressource dans la zone spécifiée.  
**<ZoneName>**  
Spécifie le nom de la zone à laquelle appartiennent les enregistrements de ressources.  
**<NodeName>**  
Spécifie le nom du nœud des enregistrements de ressource.  
**/type <RRtype> <Rrdata>**  
Spécifie le type des enregistrements de ressources soit répertoriée et le type de données qui sont attendues :  
**<RRtype>**  
Spécifie le type des enregistrements de ressources à répertorier.  
**<Rrdata>**  
Spécifie le type de données qui est l’enregistrement attendu.  
**/authority**  
Inclut des données faisant autorité.  
**/glue**  
Inclut les données de type glue.  
**/additional**  
Inclut toutes les informations supplémentaires sur les enregistrements de ressources répertoriés.  
**{/ nœud | / Child | /startchild <ChildName>}**  
Filtre ou ajoute des informations à l’affichage d’enregistrement de ressource :  
**/node**  
répertorie uniquement les enregistrements de ressources du nœud spécifié.  
**/child**  
répertorie uniquement les enregistrements de ressources d’un domaine enfant spécifié.  
**/startchild <ChildName>**  
Commence la liste au niveau du domaine enfant spécifié.  
**/continue | /detail**  
Spécifie comment les données retournées sont affichées.  
**/continue**  
répertorie uniquement les enregistrements de ressources avec leur type et les données.  
**/detail**  
Répertorie toutes les informations sur les enregistrements de ressources.  
#### <a name="sample-usage"></a>Exemple d’utilisation  
`dnscmd /enumrecords test.contoso.com test /additional`  
### <a name="BKMK_11"></a>dnscmd /enumzones  
Répertorie les zones qui existent sur le serveur DNS spécifié.  
#### <a name="syntax"></a>Syntaxe  
```  
dnscmd [<ServerName>] /enumzones [/primary | /secondary | /forwarder | /stub | /cache | /auto-created] [/forward | /reverse | /ds | /file] [/domaindirectorypartition | /forestdirectorypartition | /customdirectorypartition | /legacydirectorypartition | /directorypartition <PartitionFQDN>]  
```  
#### <a name="parameters"></a>Paramètres  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**/primary | /secondary | /forwarder | /stub | /cache | /auto-created**  
Filtre les types de zones à afficher :  
**/primary**  
Répertorie toutes les zones qui sont des zones principales standard ou les zones intégrées à active directory.  
**/secondary**  
Répertorie toutes les zones secondaires standards.  
**/forwarder**  
Répertorie les zones qui transfèrent les requêtes non résolues à un autre serveur DNS.  
**/stub**  
Répertorie toutes les zones de stub.  
**/cache**  
répertorie uniquement les zones qui sont chargés dans le cache.  
**/auto-created**  
Répertorie les zones qui ont été créés automatiquement pendant l’installation du serveur DNS.  
**/ transférer | / Inverser | /DS | /file**  
Spécifie des filtres supplémentaires pour les types de zones à afficher :  
**/forward**  
listes des zones de recherche directe.  
**/reverse**  
listes des zones de recherche inversée.  
**/ds**  
Répertorie les zones intégrées à active directory.  
**/file**  
Répertorie les zones qui sont soutenus par des fichiers.  
**/domaindirectorypartition**  
Répertorie les zones qui sont stockées dans la partition d’annuaire de domaine.  
**/forestdirectorypartition**  
Répertorie les zones qui sont stockés dans la partition de forêt.  
**/customdirectorypartition**  
Répertorie toutes les zones qui sont stockés dans une partition d’annuaire d’application définie par l’utilisateur.  
**/legacydirectorypartition**  
Répertorie toutes les zones qui sont stockés dans la partition d’annuaire de domaine.  
**/directorypartition <PartitionFQDN>**  
Répertorie toutes les zones qui sont stockés dans la partition d’annuaire spécifié.  
#### <a name="remarks"></a>Notes  
-   Le **enumzones** paramètres agissent en tant que filtres dans la liste des zones. Si aucun filtre n’est spécifié, une liste complète des zones est retournée. Lorsqu’un filtre est spécifié, uniquement les zones qui répondent aux critères du filtre sont inclus dans la liste des zones renvoyée.  
#### <a name="example"></a>Exemple  
Consultez [exemple 2 : Afficher une liste complète des zones sur un serveur DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx) ou [exemple 3 : Afficher la liste des zones de créé automatiquement sur un serveur DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_25a"></a>dnscmd /exportsettings  
Crée un fichier texte qui répertorie les détails de configuration d’un serveur DNS. Le fichier texte nommé DnsSettings.txt. Il se trouve dans le répertoire %systemroot%\system32\dns du serveur.  
#### <a name="syntax"></a>Syntaxe  
```  
dnscmd [<ServerName>] /exportsettings   
```  
#### <a name="parameters"></a>Paramètres  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par la syntaxe de l’ordinateur local, adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
#### <a name="remarks"></a>Notes  
-   Vous pouvez utiliser les informations dans le fichier qui **dnscmd /exportsettings** crée pour résoudre les problèmes de configuration ou pour vous assurer que vous avez configuré plusieurs serveurs de façon identique.  
### <a name="BKMK_12"></a>dnscmd /info  
Affiche les paramètres de la section DNS du Registre du serveur spécifié : **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters**  
#### <a name="syntax"></a>Syntaxe  
```  
dnscmd [<ServerName>] /info [<Setting>]  
```  
#### <a name="parameters"></a>Paramètres  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<Setting>**  
Les paramètres que le **info** retourne de la commande peut être spécifié individuellement. Si un paramètre n’est pas spécifié, un rapport des paramètres communs est retourné.  
#### <a name="remarks"></a>Notes  
-   Cette commande affiche les paramètres de Registre situés au niveau du serveur DNS. Pour afficher les paramètres du Registre de niveau de zone, utilisez la [zoneinfo](#BKMK_26) commande. Pour obtenir une liste de paramètres qui peuvent s’afficher avec cette commande, consultez le [config](#BKMK_3) description.  
#### <a name="example"></a>Exemple  
Consultez [exemple 4 : Afficher le paramètre IsSlave à partir d’un serveur DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx) ou [exemple 5 : Afficher le paramètre Recursiontimeout à partir d’un serveur DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_29a"></a>dnscmd /ipvalidate  
Teste si une adresse IP identifie un serveur DNS opérationnel ou si le serveur DNS peut agir comme un redirecteur, un serveur d’indications de racine ou un serveur maître pour une zone spécifique.  
#### <a name="syntax"></a>Syntaxe  
```  
dnscmd [<ServerName>] /ipvalidate <Context> [<ZoneName>] [[<IPaddress>]]  
```  
#### <a name="parameters"></a>Paramètres  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par la syntaxe de l’ordinateur local, adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<Context>**  
Spécifie le type de test à effectuer. Vous pouvez spécifier un des tests suivants :  
-   **/dnsservers** vérifie que les ordinateurs avec les adresses que vous spécifiez fonctionnent serveurs DNS.  
-   **/forwarders** vérifie que les adresses que vous spécifiez identifient les serveurs DNS qui peuvent agir en tant que redirecteurs.  
-   **/roothints** vérifie que les adresses que vous spécifiez identifient les serveurs DNS qui peuvent agir en tant que serveurs de noms racines indicateur.  
-   **/zonemasters** vérifie que les adresses que vous spécifiez identifient les serveurs DNS qui sont des serveurs maîtres pour *ZoneName*.  
**<ZoneName>**  
Identifie la zone. Utilisez ce paramètre avec le **/zonemasters** paramètre.  
**<IPaddress>**  
Spécifie les adresses IP qui teste la commande.  
#### <a name="sample-usage"></a>Exemple d’utilisation  
<pre>dnscmd dnssvr1.contoso.com /ipvalidate /dnsservers 10.0.0.1 10.0.0.2  
dnscmd dnssvr1.contoso.com /ipvalidate /zonemasters corp.contoso.com 10.0.0.2</pre>  
### <a name="BKMK_13"></a>dnscmd /nodedelete  
Supprime tous les enregistrements pour un hôte spécifié. ### Syntaxe ```  
dnscmd [<ServerName>] /nodedelete <ZoneName> <NodeName> [/tree] [/f] ```  
#### Parameters  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<ZoneName>**  
Spécifie le nom de la zone.  
**<NodeName>**  
Spécifie le nom d’hôte du nœud à supprimer.  
**/tree**  
Supprime tous les enregistrements enfants.  
**/f**  
exécute la commande sans demander confirmation. ### Exemple, consultez [exemple 6 : supprimer les enregistrements d’un nœud](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_14"></a>dnscmd /recordadd  
Ajoute un enregistrement à une zone spécifiée dans un serveur DNS. ### Syntaxe ```  
dnscmd [<ServerName>] /recordadd <ZoneName> <NodeName> <RRtype> <Rrdata> ```  
#### Parameters  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par la syntaxe de l’ordinateur local, adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<ZoneName>**  
Spécifie la zone dans laquelle se trouve l’enregistrement.  
**<NodeName>**  
Spécifie un nœud spécifique dans la zone.  
**<RRtype>**  
Spécifie le type d’enregistrement doit être ajouté.  
**<Rrdata>**  
Spécifie le type de données qui sont attendus.  
> [!NOTE]  
Lorsque vous ajoutez un enregistrement, assurez-vous que vous utilisez le format de données et le type de données correct. Pour obtenir la liste des types d’enregistrements de ressources et les types de données approprié, consultez [référence d’enregistrements de ressource](https://technet.microsoft.com/library/cc758321(v=ws.10).aspx). ### Exemple d’utilisation
<pre>dnscmd dnssvr1.contoso.com /recordadd test A 10.0.0.5  
dnscmd /recordadd test.contoso.com test MX 10 mailserver.test.contoso.com</pre>  
### <a name="BKMK_15"></a>dnscmd /recorddelete  
Supprime un enregistrement de ressource d’une zone spécifiée. ### Syntaxe ```  
dnscmd <ServerName> /recorddelete <ZoneName> <NodeName> <RRtype> <Rrdata>[/f] ```  
#### Parameters  
**<ServerName>**  
> Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<ZoneName>**  
Spécifie la zone dans laquelle se trouve l’enregistrement de ressource.  
**<NodeName>**  
Spécifie le nom de l’hôte.  
**<RRtype>**  
Spécifie le type d’enregistrement de ressource à supprimer.  
**<Rrdata>**  
Spécifie le type de données qui sont attendus.  
**/f**  
exécute la commande sans demander confirmation :-étant donné que les nœuds peuvent avoir plus d’un enregistrement de ressource, cette commande vous oblige à être très spécifique sur le type d’enregistrement de ressource que vous souhaitez supprimer. -Si vous spécifiez un type de données et vous ne spécifiez pas un type de données d’enregistrement de ressource, tous les enregistrements avec ce type de données spécifique pour le nœud spécifié sont supprimés. ### Exemple d’utilisation `dnscmd /recorddelete test.contoso.com test MX 10 mailserver.test.contoso.com`  
### <a name="BKMK_16"></a>dnscmd /resetforwarders  
Sélectionne ou réinitialise les adresses IP à laquelle le serveur DNS transfère les requêtes DNS lorsqu’il ne peut pas les résoudre localement. ### Syntaxe ```  
dnscmd [<ServerName>] /resetforwarders [<IPaddress> [,<IPaddress>]...] [/ timeout <timeOut>] [/ subordonné | / noslave] ```  
#### Parameters  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<IPaddress>**  
Répertorie les adresses IP à laquelle le serveur DNS transfère les requêtes non résolues.  
**/délai d’attente <timeOut>**  
Définit le nombre de secondes pendant lequel le serveur DNS attend une réponse du redirecteur. Par défaut, cette valeur est de cinq secondes.  
**/slave|/noslave**  
Détermine si le serveur DNS exécute ses propres requêtes itératifs si le redirecteur ne parvient pas à résoudre une requête :  
**/slave**  
Empêche le serveur DNS d’effectuer ses propres requêtes itératifs si le redirecteur ne parvient pas à résoudre une requête.  
**/noslave**  
Permet au serveur DNS effectuer ses propres requêtes itératifs si le redirecteur ne parvient pas à résoudre une requête. Il s’agit du paramètre par défaut. ### Remarques - par défaut, un serveur DNS effectue des requêtes itératives quand il ne peut pas résoudre une requête. -Définition des adresses IP à l’aide de la **resetforwarders** commande oblige le serveur DNS effectuer des requêtes récursives aux serveurs DNS sur les adresses IP spécifiées. Si les redirecteurs ne résolvent pas de la requête, le serveur DNS peut effectuer ensuite ses propres requêtes itératifs. -Si le **/subordonné** paramètre est utilisé, le serveur DNS n’effectue pas de ses propres requêtes itératifs. Cela signifie que le serveur DNS transfère les requêtes non résolues uniquement vers les serveurs DNS dans la liste, et il n’essaie pas de requêtes itératives si les redirecteurs ne résolvent pas les. Il est plus efficace de définir une adresse IP comme redirecteur pour un serveur DNS. Vous pouvez utiliser la **resetforwarders** commande pour les serveurs internes dans un réseau de transférer leurs requêtes non résolues à un seul serveur DNS qui a une connexion externe. -répertoriant une adresse IP du redirecteur à deux reprises entraîne le serveur DNS tente de transférer vers ce serveur à deux reprises. ### Exemple d’utilisation
<pre>dnscmd dnssvr1.contoso.com /resetforwarders 10.0.0.1 /timeout 7 /slave  
dnscmd dnssvr1.contoso.com /resetforwarders /noslave</pre>  
### <a name="BKMK_17"></a>dnscmd /resetlistenaddresses  
Spécifie les adresses IP sur un serveur qui écoute les demandes de client DNS. #### Syntax ```  
dnscmd [<ServerName>] /resetlistenaddresses [<listenaddress>] ```  
#### Parameters  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<listenaddress>**  
Spécifie une adresse IP sur le serveur DNS qui écoute les demandes de client DNS. Si aucune adresse d’écoute n’est spécifié, toutes les adresses IP sur le serveur reçoit les demandes de client. ### Remarques - par défaut, toutes les adresses IP sur un serveur DNS écoutent les demandes des clients DNS. ### Exemple d’utilisation `dnscmd dnssvr1.contoso.com /resetlistenaddresses 10.0.0.1`  
### <a name="BKMK_18"></a>dnscmd /startscavenging  
Indique à un serveur DNS pour tenter une recherche immédiate des enregistrements de ressources obsolètes dans un serveur DNS spécifié. ### Syntaxe ```  
dnscmd [<ServerName>] /startscavenging ```  
#### Parameter  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé. ### Remarques - réussite de cette commande démarre une récupération immédiatement. -Bien que la commande pour démarrer la récupération s’affiche pour terminer avec succès, la récupération ne démarre pas, sauf si les conditions préalables suivantes sont remplies :-le nettoyage est activé pour le serveur et de la zone. -La zone est démarrée. -Les enregistrements de ressources ont un horodatage. -Pour plus d’informations sur la façon d’activer le nettoyage pour le serveur, consultez le **scavenginginterval** paramètre sous la syntaxe au niveau du serveur dans le [config](#BKMK_3) section. -Pour plus d’informations sur l’activation du nettoyage de la zone, consultez la **vieillissement** paramètre sous la syntaxe de niveau de Zone dans le [config](#BKMK_3) section. -Pour plus d’informations sur le démarrage d’une zone est en pause, consultez le [zoneresume](#BKMK_35) section. -Pour plus d’informations sur la vérification des enregistrements de ressources pour un horodatage, consultez le [ageallrecords](#BKMK_1) section. -Si la récupération échoue, aucun message d’avertissement s’affiche. ### Exemple d’utilisation `dnscmd dnssvr1.contoso.com /startscavenging`  
### <a name="BKMK_19"></a>dnscmd /statistics  
Affiche ou efface les données d’un serveur DNS spécifié. ### Syntaxe ```  
dnscmd [<ServerName>] /statistics [<StatID>] [/ effacer] ```  
#### Parameters  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<StatID>**  
Spécifie les statistiques ou une combinaison de statistiques à afficher. Un numéro d’identification est utilisé pour identifier une statistique. Si aucun numéro d’ID de statistiques n’est spécifiée, toutes les statistiques s’affiche.  
Voici une liste de nombres qui peuvent être spécifiées et les statistiques correspondant affichent :  
**00000001**  
time  
**00000002**  
requête  
**00000004**  
query2  
**00000008**  
Recurse  
**00000010**  
Maître  
**00000020**  
Secondaire  
**00000040**  
WINS  
**00000100**  
Mettre à jour/Mise à jour  
**00000200**  
SkwanSec  
**00000400**  
service d’annuaire  
**00010000**  
Mémoire  
**00100000**  
PacketMem  
**00040000**  
DBASE  
**00080000**  
Enregistrements  
**00200000**  
NbstatMem  
**/clear**  
Réinitialise le compteur des statistiques spécifié à zéro. ### Remarques - le **statistiques** commande affiche les compteurs qui commencent sur le serveur DNS lorsqu’il est démarré ou repris. ### Consultez les exemples [exemple 7 : Afficher les statistiques de temps pour un serveur DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx) ou [exemple 8 : Afficher les statistiques de NbstatMem pour un serveur DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_20"></a>dnscmd /unenlistdirectorypartition  
Supprime le serveur DNS de jeu de réplicas de la partition de répertoire spécifié. ### Syntaxe ```  
dnscmd [<ServerName>] /unenlistdirectorypartition <PartitionFQDN> ```  
#### Parameters  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<PartitionFQDN>**  
Le nom de domaine complet de la partition de répertoire d’applications DNS qui sera supprimée.  
### <a name="BKMK_21"></a>dnscmd /writebackfiles  
Vérifie la mémoire du serveur DNS pour les modifications et les écrit dans un stockage persistant. #### Syntax ```  
dnscmd [<ServerName>] /writebackfiles [<ZoneName>] ```  
#### Parameters  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<ZoneName>**  
Spécifie le nom de la zone à mettre à jour. ### Remarques - le **writebackfiles** commande met à jour toutes les zones incorrectes ou une zone spécifiée. Une zone a été modifiée lorsque des modifications sont apportées en mémoire qui n’ont pas encore été écrites dans un stockage permanent. Il s’agit d’une opération au niveau du serveur qui vérifie toutes les zones. Vous pouvez spécifier une seule zone dans cette opération, ou vous pouvez utiliser la [zonewriteback](#BKMK_37) opération. ### Exemple d’utilisation `dnscmd dnssvr1.contoso.com /writebackfiles`  
### <a name="BKMK_22"></a>dnscmd /zoneadd  
Ajoute une zone au serveur DNS. #### Syntax ```  
dnscmd [<ServerName>] /zoneadd <ZoneName> <Zonetype> [/dp <FQDN>| {/domain|/enterprise|/legacy}] ```  
#### Parameters  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<ZoneName>**  
Spécifie le nom de la zone.  
**<Zonetype>**  
Spécifie le type de zone à créer. Chaque type de zone a différents paramètres requis :  
**/dsprimary**  
Crée une zone intégrée à active directory.  
**//file principal <FileName>**  
Crée une zone principale standard et spécifie le nom du fichier qui stockera les informations de zone.  
**/secondaire <MasterIPaddress> [<MasterIPaddress>...]**  
Crée une zone secondaire standard.  
**/stub <MasterIPaddress> [<MasterIPaddress>...] / fichier <FileName>**  
Crée une zone de stub de sauvegarde de fichiers.  
**/dsstub <MasterIPaddress> [<MasterIPaddress>...]**  
Crée une zone de stub intégrée active directory.  
** / redirecteur <MasterIPaddress> [<MasterIPaddress>]... / fichier <FileName>**  
Spécifie que la zone créée transfère les requêtes non résolues à un autre serveur DNS.  
**/dsforwarder**  
Spécifie que la zone intégrée à créé active directory transfère les requêtes non résolues à un autre serveur DNS.  
**/dp <FQDN> {/ domaine | /enterprise | / hérité}**  
Spécifie la partition d’annuaire sur lequel stocker la zone.  
**<FQDN>**  
Spécifie le nom de domaine complet de la partition d’annuaire.  
**/domain**  
Enregistre la zone sur la partition d’annuaire de domaine.  
**/enterprise**  
Enregistre la zone sur la partition d’annuaire d’entreprise.  
**/legacy**  
Enregistre la zone sur une partition d’annuaire hérité. ### Remarques - spécification d’un type de zone de **/forwarder** ou **/dsforwarder** crée une zone qui assure le transfert conditionnel. ### Exemple d’utilisation
<pre>dnscmd dnssvr1.contoso.com /zoneadd test.contoso.com /dsprimary  
dnscmd dnssvr1.contoso.com /zoneadd secondtest.contoso.com /secondary 10.0.0.2</pre>  
### <a name="BKMK_23"></a>dnscmd /zonechangedirectorypartition  
Modifie la partition d’annuaire sur lequel réside la zone spécifiée. #### Syntax ```  
dnscmd [<ServerName>] /zonechangedirectorypartition <ZoneName>] {[<NewPartitionName>] | [<Zonetype>] } ```  
#### Parameters  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<ZoneName>**  
Le nom de domaine complet de la partition d’annuaire actuel sur lequel réside la zone.  
**<NewPartitionName>**  
Le nom de domaine complet de la zone sera déplacée vers la partition d’annuaire.  
**<Zonetype>**  
Spécifie le type de partition d’annuaire qui sont déplacée vers la zone.  
**/domain**  
Déplace la zone pour la partition d’annuaire de domaine intégré.  
**/forest**  
Déplace la zone pour la partition d’annuaire de forêt intégrés.  
**/legacy**  
Déplace la zone pour la partition d’annuaire est créée pour les contrôleurs de domaine active directory antérieur. Ces partitions d’annuaire ne sont pas nécessaires pour le mode natif.  
### <a name="BKMK_24"></a>dnscmd /zonedelete  
Supprime une zone spécifiée. ### Syntaxe ```  
dnscmd [<ServerName>] /zonedelete <ZoneName> [/dsdel] [/f] ```  
#### Parameters  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<ZoneName>**  
Spécifie le nom de la zone à supprimer.  
**/dsdel**  
Supprime la zone à partir d’AD DS.  
**/f**  
Exécute la commande sans demander confirmation. ### Exemple, consultez [exemple 9 : supprimer une zone à partir d’un serveur DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_25"></a>dnscmd /zoneexport  
Crée un fichier texte qui répertorie les enregistrements de ressources d’une zone spécifiée. ### Syntaxe ' dnscmd [<ServerName>] /zoneexport <ZoneName> <ZoneExportFile>' ### paramètres **<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par la syntaxe de l’ordinateur local, adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<ZoneName>**  
Spécifie le nom de la zone.  
**<ZoneExportFile>**  
Spécifie le nom du fichier à créer. ### Remarques - le **zoneexport** opération crée un fichier d’enregistrements de ressource pour une zone intégrée à active directory pour résoudre des problèmes. Par défaut, cette commande crée le fichier est placé dans le répertoire DNS, qui est, par défaut, le répertoire %systemroot%/System32/Dns. ### Exemple, consultez [exemple 10 : Exporter la liste d’enregistrements de ressource de zone dans un fichier](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_26"></a>dnscmd /zoneinfo  
Affiche les paramètres de la section du Registre de la zone spécifiée : **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters\Zones\\<ZoneName>** ### syntaxe ```  
dnscmd [<ServerName>] /zoneinfo <ZoneName> [<Setting>] ```  
#### Parameters  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<ZoneName>**  
Spécifie le nom de la zone.  
**<Setting>**  
Vous pouvez spécifier individuellement les paramètres que le **zoneinfo** commande retourne. Si vous ne spécifiez pas un paramètre, tous les paramètres sont retournés. ### Remarques - le **zoneinfo** commande affiche les paramètres de Registre qui sont à la zone DNS au niveau à **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters\Zones\\<ZoneName>**. -Pour afficher les paramètres du Registre au niveau du serveur, utilisez le [info](#BKMK_12) commande. -Pour afficher la liste de paramètres que vous pouvez afficher avec cette commande, consultez le [config](#BKMK_3) commande. ### Exemple, consultez [exemple 11 : Affiche le paramètre RefreshInterval à partir du Registre](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx) ou [exemple 12 : Affichage Chronologie de paramètre à partir du Registre](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_27"></a>dnscmd /zonepause  
Suspend la zone spécifiée, qui ignore ensuite les demandes de requête. ### Syntaxe ```  
dnscmd [<ServerName>] /zonepause <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<ZoneName>**  
Spécifie le nom de la zone de mise en pause. ### Remarques - reprendre une zone et rendre disponible une fois qu’il a été suspendu, utilisez la [zoneresume](#BKMK_35) commande. ### Exemple d’utilisation `dnscmd dnssvr1.contoso.com /zonepause test.contoso.com`  
### <a name="BKMK_28"></a>dnscmd /zoneprint  
Répertorie les enregistrements dans une zone. ### Syntaxe ```  
dnscmd [<ServerName>] /zoneprint <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par la syntaxe de l’ordinateur local, adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<ZoneName>**  
Identifie la zone à répertorier.  
### <a name="BKMK_30"></a>dnscmd /zonerefresh  
force une zone DNS secondaire pour mettre à jour à partir de la zone principale. ### Syntaxe ```  
dnscmd <ServerName> /zonerefresh <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<ZoneName>**  
Spécifie le nom de la zone à actualiser. ### Remarques - le **zonerefresh** commande force une vérification du numéro de version dans le démarrage du serveur maître s d’enregistrement de ressource d’autorité (principale SOA). Si le numéro de version sur le serveur maître est supérieur au numéro de version du serveur secondaire, qui met à jour le serveur secondaire est lancé par un transfert de zone. Si le numéro de version est le même, aucun transfert de zone se produit. -La vérification de forcé se produit par défaut toutes les 15 minutes. Pour modifier la valeur par défaut, utilisez le **dnscmd config refreshinterval** commande. ### Exemple d’utilisation `dnscmd dnssvr1.contoso.com /zonerefresh test.contoso.com`  
### <a name="BKMK_31"></a>dnscmd /zonereload  
Copies de la zone d’informations à partir de sa source. ### Syntaxe ```  
dnscmd <ServerName> /zonereload <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<ZoneName>**  
Spécifie le nom de la zone à être rechargé. ### Remarques - Si la zone est intégrée à active directory, il recharge à partir d’AD DS. -Si la zone est une zone de sauvegarde de fichiers standard, celle-ci rechargée à partir d’un fichier. ### Exemple d’utilisation `dnscmd dnssvr1.contoso.com /zonereload test.contoso.com`  
### <a name="BKMK_32"></a>dnscmd /zoneresetmasters  
Réinitialise les adresses IP du serveur maître qui fournit des informations de transfert de zone à une zone secondaire. ### Syntaxe ```  
dnscmd <ServerName> /zoneresetmasters <ZoneName> [/ local] [<IPaddress> [<IPaddress>]...] ```  
#### Parameters  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<ZoneName>**  
Spécifie le nom de la zone à être rechargé.  
**/local**  
Définit une liste principale locale. Ce paramètre est utilisé pour les zones intégrées à active directory.  
**<IPaddress>**  
Les adresses IP des serveurs maîtres de la zone secondaire. ### Remarques - cette valeur est définie à l’origine lors de la zone secondaire est créée. Utilisez le **/ZoneResetMasters** commande sur le serveur secondaire. Cette valeur n’a aucun effet si elle est définie sur le serveur DNS maître. ### Exemple d’utilisation
<pre>dnscmd dnssvr1.contoso.com /zoneresetmasters test.contoso.com 10.0.0.1  
dnscmd dnssvr1.contoso.com /zoneresetmasters test.contoso.com /local</pre>  
### <a name="BKMK_33"></a>dnscmd /zoneresetscavengeservers  
Modifie les adresses IP des serveurs que vous pouvant nettoyer la zone spécifiée. ### Syntaxe ```  
dnscmd [<ServerName>] /zoneresetscavengeservers <ZoneName> [<IPaddress> [<IPaddress>]...] ```  
#### Parameters  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par la syntaxe de l’ordinateur local, adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<ZoneName>**  
Identifie la zone à nettoyer.  
**<IPaddress>**  
Répertorie les adresses IP des serveurs que vous peuvent effectuer la récupération. Si ce paramètre est omis, tous les serveurs qui hébergent cette zone peuvent le nettoyer. ### Remarques - par défaut, tous les serveurs qui hébergent une zone peuvent nettoyer cette zone. -Si une zone est hébergée sur plusieurs serveurs DNS, vous pouvez utiliser cette commande pour réduire le nombre de fois où une zone est nettoyée. -Le nettoyage doit être activé sur le serveur DNS et de la zone qui est affectée par cette commande. ### Exemple d’utilisation `dnscmd dnssvr1.contoso.com /zoneresetscavengeservers test.contoso.com 10.0.0.1 10.0.0.2`  
### <a name="BKMK_34"></a>dnscmd /zoneresetsecondaries  
Spécifie une liste d’adresses IP des serveurs secondaires à laquelle un serveur maître répond lorsqu’il est sollicité pour un transfert de zone. ### Syntaxe ```  
dnscmd [<ServerName>] /zoneresetsecondaries <ZoneName> {/ noxfr | / non sécurisés | /securens | /securelist <SecurityIPaddresses>} {/ nonotify | / notifier | /notifylist <NotifyIPaddresses>} ```  
#### Parameters  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si l’est le paramètre est omis, le serveur local est utilisé.  
**<ZoneName>**  
Spécifie le nom de la zone qui auront à ses serveurs secondaires réinitialiser.  
**/noxfr | / non sécurisés | /securens | /SecureList <SecurityIPaddresses>**  
Spécifie si toutes ou une partie des serveurs secondaires demandant une mise à jour seulement obtenir une mise à jour.  
**/noxfr**  
Indique qu’aucun transfert de zone n’est autorisés.  
**/nonsecure**  
Spécifie que toutes les demandes de transfert de zone sont accordées.  
**/securens**  
Spécifie que seul le serveur qui est répertorié dans l’enregistrement de ressource de nom du serveur de (noms NS) pour la zone est accordé à un transfert.  
**/securelist**  
Spécifie que les transferts de zone sont accordées uniquement à la liste des serveurs. Ce paramètre doit être suivi d’une adresse IP ou les adresses qui utilise le serveur maître.  
**<SecurityIPaddresses>**  
Répertorie les adresses IP qui reçoivent des transferts de zone à partir du serveur maître. Ce paramètre est utilisé uniquement avec le **/securelist** paramètre.  
**/nonotify | / notifier | /notifylist <NotifyIPaddresses>**  
Spécifie qu’une notification de modification est envoyée uniquement à certains serveurs secondaires :  
**/nonotify**  
Spécifie qu’aucune notification de modification n’est envoyées aux serveurs secondaires.  
**/notify**  
Spécifie que les notifications de modification sont envoyées à tous les serveurs secondaires.  
**/notifylist**  
Spécifie que les notifications de modification sont envoyées à uniquement la liste des serveurs. Cette commande doit être suivie d’une adresse IP ou les adresses qui utilise le serveur maître.  
**<NotifyIPaddresses>**  
Spécifie l’adresse IP ou les adresses du serveur secondaire ou de serveurs qui changent les notifications sont envoyées. Cette liste est utilisée uniquement avec le **/notifylist** paramètre. ### Remarques - utilisez le **zoneresetsecondaries** commande sur le serveur maître pour spécifier comment il répond aux demandes de transfert de zone à partir de serveurs secondaires. ### Exemple d’utilisation
<pre>dnscmd dnssvr1.contoso.com /zoneresetsecondaries test.contoso.com /noxfr /nonotify  
dnscmd dnssvr1.contoso.com /zoneresetsecondaries test.contoso.com /securelist 11.0.0.2</pre>  
### <a name="BKMK_29"></a>dnscmd /zoneresettype  
modifie le type de la zone. ### Syntaxe ```  
dnscmd [<ServerName>] /zoneresettype <ZoneName> <Zonetype> [/ overwrite_mem | / OverWrite_Ds] ```  
#### Parameters  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par la syntaxe de l’ordinateur local, adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<ZoneName>**  
Identifie la zone sur lequel le type sera modifié.  
**<Zonetype>**  
Spécifie le type de zone à créer. Chaque type présente les différents paramètres requis :  
**/dsprimary**  
Crée une zone intégrée à active directory.  
**//file principal <FileName>**  
Crée une zone principale standard.  
**/secondaire <MasterIPaddress> [,<MasterIPaddress>...]**  
Crée une zone secondaire standard.  
**/stub <MasterIPaddress>[,<MasterIPaddress>...] / fichier <FileName>**  
Crée une zone de stub de sauvegarde de fichiers.  
**/dsstub <MasterIPaddress>[,<MasterIPaddress>...]**  
Crée une zone de stub intégrée active directory.  
**/redirecteur <MasterIPaddress[,<MasterIPaddress>]... / fichier<FileName>**  
Spécifie que la zone créée transfère les requêtes non résolues à un autre serveur DNS.  
**/dsforwarder**  
Spécifie que la zone intégrée à créé active directory transfère les requêtes non résolues à un autre serveur DNS.  
**/overwrite_mem | /overwrite_ds**  
Spécifie comment remplacer les données existantes :  
**/overwrite_mem**  
Remplace les données DNS à partir des données dans les services AD DS.  
**/overwrite_ds**  
Remplace les données existantes dans les services AD DS. ### Remarques - définition de la zone Entrez comme **/dsforwarder** crée une zone qui assure le transfert conditionnel. ### Exemple d’utilisation
<pre>dnscmd dnssvr1.contoso.com /zoneresettype test.contoso.com /primary /file test.contoso.com.dns  
dnscmd dnssvr1.contoso.com /zoneresettype second.contoso.com /secondary 10.0.0.2</pre>  
### <a name="BKMK_35"></a>dnscmd /zoneresume  
démarre une zone spécifiée qui a été précédemment interrompue. ### Syntaxe ```  
dnscmd <ServerName> /zoneresume <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<ZoneName>**  
Spécifie le nom de la zone à reprendre. ### Remarques - vous pouvez utiliser cette opération pour inverser le [zonepause](#BKMK_27) opération. ### Exemple d’utilisation `dnscmd dnssvr1.contoso.com /zoneresume test.contoso.com`  
### <a name="BKMK_36"></a>dnscmd /zoneupdatefromds  
Met à jour de la zone intégrée à l’annuaire active directory spécifié à partir d’AD DS. ### Syntaxe ```  
dnscmd <ServerName> /zoneupdatefromds <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<ZoneName>**  
Spécifie le nom de la zone à mettre à jour. ### Remarques - active directory intégrées zones effectuent cette mise à jour par défaut toutes les cinq minutes. Pour modifier ce paramètre, utilisez le **dnscmd config dspollinginterval** commande. ### Exemple d’utilisation `dnscmd dnssvr1.contoso.com /zoneupdatefromds`  
### <a name="BKMK_37"></a>dnscmd /zonewriteback  
Vérifie la mémoire du serveur DNS pour les modifications qui sont pertinentes pour une zone spécifiée et les écrit dans un stockage persistant. ### Syntaxe ```  
dnscmd <ServerName> /zonewriteback <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Spécifie le serveur DNS pour gérer, représenté par l’adresse IP, nom de domaine complet ou nom d’hôte. Si ce paramètre est omis, le serveur local est utilisé.  
**<ZoneName>**  
Spécifie le nom de la zone à mettre à jour. ### Remarques - Il s’agit d’une opération de niveau de zone. Vous pouvez mettre à jour toutes les zones sur un serveur DNS avec la [writebackfiles](#BKMK_21) opération. ### Exemple d’utilisation `dnscmd dnssvr1.contoso.com /zonewriteback test.contoso.com`  
