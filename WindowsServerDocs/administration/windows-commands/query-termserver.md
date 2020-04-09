---
title: requête termserver
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3b89d3b4-236f-4376-90b6-939a0ec4b288
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d63bd158dad74203aa7ee3fd4e43dffb97c4c873
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836922"
---
# <a name="query-termserver"></a>requête termserver

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche la liste de tous les serveurs Bureau à distance hôte de session Bureau à distance (hôte de session Bureau à distance) sur le réseau.
pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_examples).
> [!NOTE]
> Sous Windows Server 2008 R2, les services Terminal Server sont appelés Services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.
> ## <a name="syntax"></a>Syntaxe
> ```
> query termserver [<ServerName>] [/domain:<Domain>] [/address] [/continue]
> ```
> ### <a name="parameters"></a>Paramètres
> 
> |    Paramètre     |                                                                        Description                                                                         |
> |------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |   <ServerName>   |                                               Spécifie le nom qui identifie le serveur hôte de session Bureau à distance.                                               |
> | /Domain :<Domain> | Spécifie le domaine à interroger pour les serveurs Terminal Server. Vous n’avez pas besoin de spécifier un domaine si vous interrogez le domaine dans lequel vous travaillez actuellement. |
> |     /address     |                                                  Affiche les adresses du réseau et du nœud pour chaque serveur.                                                  |
> |    /continue     |                                              Empêche la suspension après l’affichage de chaque écran d’informations.                                               |
> |        /?        |                                                            Affiche l'aide à l'invite de commandes.                                                            |
> 
> ## <a name="remarks"></a>Notes
> - **requête termserver** recherche sur le réseau tous les serveurs hôtes de session Bureau à distance attachés et retourne les informations suivantes :
>   - Nom du serveur
>   - Le réseau (et l’adresse du nœud si l’option/Address est utilisée)
>     ## <a name="examples"></a><a name=BKMK_examples></a>Illustre
> - Pour afficher des informations sur tous les serveurs hôtes de session Bureau à distance sur le réseau, tapez :
>   ```
>   query termserver
>   ```
> - Pour afficher des informations sur le serveur hôte de session Bureau à distance nommé Server3, tapez :
>   ```
>   query termserver Server3
>   ```
> - Pour afficher des informations sur tous les serveurs hôtes de session Bureau à distance dans le domaine CONTOSO, tapez :
>   ```
>   query termserver /domain:CONTOSO
>   ```
> - Pour afficher l’adresse du réseau et du nœud du serveur hôte de session Bureau à distance nommé Server3, tapez :
>   ```
>   query termserver Server3 /address
>   ```
>   ## <a name="additional-references"></a>Références supplémentaires
>   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
>   
>   de [requête](query.md) [services Bureau à distance (services Terminal Server) de la commande](remote-desktop-services-terminal-services-command-reference.md)
