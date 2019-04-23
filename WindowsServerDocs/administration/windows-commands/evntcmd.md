---
title: Evntcmd
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c1aabb74-76e7-4304-95a6-50ad87e92fd9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a149e78170f2849512dcfc0a0a82f9eed979abe2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885980"
---
# <a name="evntcmd"></a>Evntcmd

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configure la traduction des événements en interruptions, les destinations d’interruptions ou les deux en fonction des informations dans un fichier de configuration.   
## <a name="syntax"></a>Syntaxe  
```  
evntcmd [/s <computerName>] [/v <verbosityLevel>] [/n] <FileName>  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|/s <computerName>|Spécifie le nom, l’ordinateur sur lequel vous souhaitez configurer la traduction des événements en interruptions, les destinations d’interruptions ou les deux. Si vous ne spécifiez pas un ordinateur, la configuration se produit sur l’ordinateur local.|  
|/v <verbosityLevel>|Spécifie les types d’état messages apparaissent sous la forme d’interruptions et les destinations d’interruptions sont configurés. Ce paramètre doit être un entier compris entre 0 et 10. Si vous spécifiez 10, tous les types de messages apparaissent, y compris le suivi des messages et les avertissements relatifs à la réussite de la configuration de l’interruption. Si vous spécifiez 0, aucun message n’apparaît.|  
|/n|Spécifie que le service SNMP ne doit pas être redémarré si cet ordinateur reçoit des modifications de configuration d’interruption.|  
|<FileName>|Spécifie le nom, le fichier de configuration qui contient des informations sur la traduction des événements pour les interruptions et les destinations d’interruptions que vous souhaitez configurer.|  
|/?|Affiche l'aide à l'invite de commandes.|  
## <a name="remarks"></a>Notes  
-   Si vous souhaitez configurer les interruptions, mais pas les destinations d’interruptions, vous pouvez créer un fichier de configuration valide en utilisant Event to Trap Translator, qui est un utilitaire graphique. Si vous avez le service SNMP installé, vous pouvez démarrer interruption convertisseur d’événement en tapant **evntwin** à une invite de commandes. Une fois que vous avez défini les interruptions voulues, cliquez sur Exporter pour créer un fichier pouvant être utilisé avec **evntcmd**. Événement à interruption Translator permet de facilement créer un fichier de configuration, puis utiliser le fichier de configuration avec **evntcmd** à l’invite de commande pour configurer rapidement des interruptions sur plusieurs ordinateurs.  
-   La syntaxe de configuration d’une interruption est comme suit :  
    **#pragma add***<EventLogFile> <EventSource> <EventID> [<Count> [<Period>]]*  
    -   Le texte **#pragma** doit apparaître au début de chaque entrée dans le fichier.  
    -   Le paramètre **ajouter** Spécifie que vous souhaitez ajouter un événement interruption de configuration.  
    -   Les paramètres *FichierJournalÉvénements*, *EventSource*, et *EventID* sont requis. Le paramètre *FichierJournalÉvénements* Spécifie le fichier dans lequel l’événement est enregistré. Le paramètre *EventSource* Spécifie l’application qui génère l’événement. Le *EventID* paramètre spécifie le nombre unique qui identifie chaque événement. Pour trouver les valeurs qui correspondent à des événements particuliers, démarrez interruption convertisseur d’événement en tapant **evntwin** à une invite de commandes. Cliquez sur **personnalisé**, puis cliquez sur **modifier**. Sous **Sources d’événements**, parcourir les dossiers jusqu'à ce que vous localisiez l’événement que vous souhaitez configurer, cliquez dessus, puis cliquez sur **ajouter**. Informations sur la source d’événements, le fichier de journal des événements et l’ID d’événement apparaissent sous **Source, ouvrez une session**, et **intercepter ID spécifique**, respectivement.  
    -   Le *nombre* paramètre est facultatif et spécifie combien de fois l’événement doit se produire avant l’envoi d’un message d’interruption. Si vous n’utilisez pas le *nombre* paramètre, le message d’interruption est envoyé une fois que l’événement se produit une seule fois.  
    -   Le *période* paramètre est facultatif, mais il requiert que vous utilisiez le *nombre* paramètre. Le *période* paramètre spécifie une durée (en secondes) pendant laquelle l’événement doit se produire le nombre de fois spécifié par le paramètre Count avant l’envoi d’un message d’interruption. Si vous n’utilisez pas le *période* paramètre, un message d’interruption est envoyé une fois que l’événement produit le nombre de fois spécifié par le *nombre* paramètre, quel que soit le temps écoulé entre deux occurrences.  
-   La syntaxe pour la suppression d’une interruption est comme suit :  
    **#pragma delete***<EventLogFile> <EventSource> <EventID>*  
    -   Le texte **#pragma** doit apparaître au début de chaque entrée dans le fichier.  
    -   Le paramètre *supprimer* Spécifie que vous souhaitez supprimer un événement interruption de configuration.  
    -   Les paramètres *FichierJournalÉvénements*, *EventSource*, et *EventID* sont requis. Le paramètre *FichierJournalÉvénements* Spécifie le journal dans lequel l’événement est enregistré. Le paramètre *EventSource* Spécifie l’application qui génère l’événement. Le *EventID* paramètre spécifie le nombre unique qui identifie chaque événement.  
-   La syntaxe de configuration d’une destination d’interruption est comme suit :  
    **#pragma add_TRAP_DEST***<CommunityName> <HostID>*  
    -   Le texte **#pragma** doit apparaître au début de chaque entrée dans le fichier.  
    -   Le paramètre **add_TRAP_DEST** indique que vous souhaitez des messages à envoyer à un hôte spécifique au sein d’une Communauté d’interruption.  
    -   Le paramètre *NomCommunauté* Spécifie le nom, la Communauté dans les interruptions sont envoyés.  
    -   Le paramètre *HostID* Spécifie que, par nom ou adresse IP, l’hôte auquel vous souhaitez envoyer des messages d’interruption.  
-   La syntaxe pour la suppression d’une destination d’interruption est comme suit :  
    **#pragma delete_TRAP_DEST***<CommunityName> <HostID>*  
    -   Le texte **#pragma** doit apparaître au début de chaque entrée dans le fichier.  
    -   Le paramètre *delete_TRAP_DEST* Spécifie que vous ne souhaitez pas les messages d’interruption à envoyer à un hôte spécifique au sein d’une Communauté.  
    -   Le paramètre *NomCommunauté* Spécifie le nom, la Communauté dans les interruptions sont envoyés.  
    -   Le paramètre *HostID* Spécifie que, par nom ou adresse IP, l’hôte auquel vous souhaitez envoyer des messages d’interruption.  
## <a name="BKMK_Examples"></a>Exemples  
Les exemples suivants illustrent des entrées dans le fichier de configuration pour le **evntcmd** commande. Ils ne sont pas destinées à être tapées à une invite de commandes.  
Pour envoyer un message d’interruption si le service journal des événements est redémarré, tapez :  
```  
#pragma add System "Eventlog" 2147489653  
```  
Pour envoyer un message d’interruption si le service journal des événements est redémarré deux fois dans les trois minutes, tapez :  
```  
#pragma add System "Eventlog" 2147489653 2 180  
```  
Pour arrêter l’envoi d’un message d’interruption à chaque fois que le service journal des événements est redémarré, tapez :  
```  
#pragma delete System "Eventlog" 2147489653  
```  
Pour envoyer des messages d’interruption dans la Communauté nommé Public à l’hôte et l’adresse IP 192.168.100.100, tapez :  
```  
#pragma add_TRAP_DEST public 192.168.100.100  
```  
Pour envoyer des messages d’interruption dans la Communauté Private à l’ordinateur hôte nommé Host1, tapez :  
```  
#pragma add_TRAP_DEST private Host1  
```  
Pour arrêter l’envoi des messages d’interruption dans la Communauté, nommé Private sur le même ordinateur que celui sur lequel vous configurez des destinations d’interruptions, tapez :  
```  
#pragma delete_TRAP_DEST private localhost  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
