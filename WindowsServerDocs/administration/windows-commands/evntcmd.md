---
title: Evntcmd
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c1aabb74-76e7-4304-95a6-50ad87e92fd9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 67370ee3d1ea9b17ba024372fb9ec8f8dbc2148d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725730"
---
# <a name="evntcmd"></a>Evntcmd

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configure la traduction des événements en interruptions, en destinations d’interruptions, ou les deux en fonction des informations contenues dans un fichier de configuration.   
## <a name="syntax"></a>Syntaxe  
```  
evntcmd [/s <computerName>] [/v <verbosityLevel>] [/n] <FileName>  
```  
#### <a name="parameters"></a>Paramètres  

|      Paramètre      |                                                                                                                                                            Description                                                                                                                                                             |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  commutateur<computerName>  |                                                         Spécifie, par nom, l’ordinateur sur lequel vous souhaitez configurer la traduction des événements en interruptions, en destinations d’interruptions, ou les deux. Si vous ne spécifiez pas d’ordinateur, la configuration se produit sur l’ordinateur local.                                                          |
| /f<verbosityLevel> | Spécifie les types de messages d’État qui s’affichent lorsque des interruptions et des destinations d’interruptions sont configurées. Ce paramètre doit être un entier compris entre 0 et 10. Si vous spécifiez 10, tous les types de messages s’affichent, y compris les messages de suivi et les avertissements indiquant si la configuration de l’interruption a réussi. Si vous spécifiez 0, aucun message ne s’affiche. |
|         /n          |                                                                                                           Spécifie que le service SNMP ne doit pas être redémarré si cet ordinateur reçoit des modifications de configuration d’interruption.                                                                                                            |
|     <FileName>      |                                                                                     Spécifie, par son nom, le fichier de configuration qui contient des informations sur la traduction des événements en interruptions et en destinations d’interruptions que vous souhaitez configurer.                                                                                     |
|         /?          |                                                                                                                                                Affiche l'aide à l'invite de commandes.                                                                                                                                                |

## <a name="remarks"></a>Notes   
- Si vous souhaitez configurer des interruptions mais pas des destinations d’interruption, vous pouvez créer un fichier de configuration valide en utilisant un convertisseur d’événement à interruption, qui est un utilitaire graphique. Si vous avez installé le service SNMP, vous pouvez démarrer l’événement pour le traducteur d’interruptions en tapant **evntwin** à une invite de commandes. Une fois que vous avez défini les interruptions souhaitées, cliquez sur Exporter pour créer un fichier pouvant être utilisé avec **evntcmd**. Vous pouvez utiliser un convertisseur d’événement à interruption pour créer facilement un fichier de configuration, puis utiliser le fichier de configuration avec **evntcmd** à l’invite de commandes pour configurer rapidement des interruptions sur plusieurs ordinateurs.  
- La syntaxe de configuration d’une interruption est la suivante :  
  **#pragma** <em> <EventLogFile> ajouter <EventSource> [<Count> [<Period>]] <EventID></em>  
  -   Le **#pragma** de texte doit apparaître au début de chaque entrée dans le fichier.  
  -   Le paramètre **Add** spécifie que vous souhaitez ajouter un événement à la configuration de l’interruption.  
  -   Les paramètres *EventLogFile*, *EventSource*et *eventID* sont requis. Le paramètre *EventLogFile* spécifie le fichier dans lequel l’événement est enregistré. Le paramètre *EventSource* spécifie l’application qui génère l’événement. Le paramètre *eventID* spécifie le numéro unique qui identifie chaque événement. Pour savoir quelles valeurs correspondent à des événements particuliers, démarrez le convertisseur d’événements en interruptions en tapant **evntwin** à une invite de commandes. Cliquez sur **personnalisé**, puis sur **modifier**. Sous **sources d’événements**, parcourez les dossiers jusqu’à ce que vous trouviez l’événement que vous souhaitez configurer, cliquez dessus, puis cliquez sur **Ajouter**. Les informations relatives à la source de l’événement, au fichier journal des événements et à l’ID d’événement s’affichent respectivement sous **ID spécifique**de la **source, du journal**et de l’interruption.  
  -   Le paramètre *Count* est facultatif et spécifie le nombre de fois où l’événement doit se produire avant l’envoi d’un message d’interruption. Si vous n’utilisez pas le paramètre *Count* , le message d’interruption est envoyé lorsque l’événement se produit une fois.  
  -   Le paramètre *period* est facultatif, mais vous devez utiliser le paramètre *Count* . Le paramètre *period* spécifie une durée (en secondes) au cours de laquelle l’événement doit se produire le nombre de fois spécifié avec le paramètre count avant l’envoi d’un message d’interruption. Si vous n’utilisez pas le paramètre *period* , un message d’interruption est envoyé lorsque l’événement se produit le nombre de fois spécifié avec le paramètre *Count* , quel que soit le temps écoulé entre les occurrences.  
- La syntaxe de suppression d’une interruption est la suivante :  
  **#pragma** <em> <EventLogFile> supprimer <EventSource><EventID></em>  
  -   Le **#pragma** de texte doit apparaître au début de chaque entrée dans le fichier.  
  -   Le paramètre *Delete* spécifie que vous souhaitez supprimer une configuration d’événement à interruption.  
  -   Les paramètres *EventLogFile*, *EventSource*et *eventID* sont requis. Le paramètre *EventLogFile* spécifie le journal dans lequel l’événement est enregistré. Le paramètre *EventSource* spécifie l’application qui génère l’événement. Le paramètre *eventID* spécifie le numéro unique qui identifie chaque événement.  
- La syntaxe de configuration d’une destination d’interruption est la suivante :  
  **#pragma add_TRAP_DEST** <em> <CommunityName><HostID></em>  
  -   Le **#pragma** de texte doit apparaître au début de chaque entrée dans le fichier.  
  -   Le paramètre **add_TRAP_DEST** spécifie que vous souhaitez que les messages d’interruption soient envoyés à un hôte spécifié au sein d’une communauté.  
  -   Le paramètre *CommunityName* spécifie, par nom, la Communauté dans laquelle les messages d’interruption sont envoyés.  
  -   Le paramètre *HostID* spécifie, par son nom ou son adresse IP, l’hôte auquel vous souhaitez envoyer les messages d’interruption.  
- La syntaxe de suppression d’une destination d’interruption est la suivante :  
  **#pragma delete_TRAP_DEST** <em> <CommunityName><HostID></em>  
  - Le **#pragma** de texte doit apparaître au début de chaque entrée dans le fichier.  
  - Le paramètre *delete_TRAP_DEST* spécifie que vous ne voulez pas que les messages d’interruption soient envoyés à un hôte spécifié au sein d’une communauté.  
  - Le paramètre *CommunityName* spécifie, par nom, la Communauté dans laquelle les messages d’interruption sont envoyés.  
  - Le paramètre *HostID* spécifie, par son nom ou son adresse IP, l’hôte auquel vous ne souhaitez pas envoyer de messages d’interruption.  
    ## <a name="examples"></a>Exemples  
    Les exemples suivants illustrent les entrées du fichier de configuration pour la commande **evntcmd** . Elles ne sont pas conçues pour être tapées à partir d’une invite de commandes.  
    Pour envoyer un message d’interruption si le service journal des événements est redémarré, tapez :  
    ```  
    #pragma add System Eventlog 2147489653  
    ```  
    Pour envoyer un message d’interruption si le service journal des événements est redémarré deux fois en trois minutes, tapez :  
    ```  
    #pragma add System Eventlog 2147489653 2 180  
    ```  
    Pour arrêter l’envoi d’un message d’interruption à chaque redémarrage du service journal des événements, tapez :  
    ```  
    #pragma delete System Eventlog 2147489653  
    ```  
    Pour envoyer des messages d’interruption au sein de la communauté nommée public à l’hôte avec l’adresse IP 192.168.100.100, tapez :  
    ```  
    #pragma add_TRAP_DEST public 192.168.100.100  
    ```  
    Pour envoyer des messages d’interruption au sein de la communauté nommée privé à l’ordinateur hôte nommé host1, tapez :  
    ```  
    #pragma add_TRAP_DEST private Host1  
    ```  
    Pour arrêter l’envoi de messages d’interruption au sein de la communauté nommée privé sur l’ordinateur sur lequel vous configurez les destinations des interruptions, tapez :  
    ```  
    #pragma delete_TRAP_DEST private localhost  
    ```  
    ## <a name="additional-references"></a>Références supplémentaires  
- - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
