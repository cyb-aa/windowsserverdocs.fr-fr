---
title: requête termserver
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3b89d3b4-236f-4376-90b6-939a0ec4b288
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 95a2e102a147bd7ebee24995d1e1fcd4147f8174
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371890"
---
# <a name="query-termserver"></a>requête termserver

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Affiche la liste de tous les serveurs Bureau à distance hôte de session Bureau à distance (hôte de session Bureau à distance) sur le réseau.
Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_examples).
> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.
> ## <a name="syntax"></a>Syntaxe
> ```
> query termserver [<ServerName>] [/domain:<Domain>] [/address] [/continue]
> ```
> ## <a name="parameters"></a>Paramètres
> 
> |    Paramètre     |                                                                        Description                                                                         |
> |------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |   <ServerName>   |                                               Spécifie le nom qui identifie le serveur hôte de session Bureau à distance.                                               |
> | /Domain : <Domain> | Spécifie le domaine à interroger pour les serveurs Terminal Server. Vous n’avez pas besoin de spécifier un domaine si vous interrogez le domaine dans lequel vous travaillez actuellement. |
> |     /address     |                                                  Affiche les adresses du réseau et du nœud pour chaque serveur.                                                  |
> |    /continue     |                                              Empêche la suspension après l’affichage de chaque écran d’informations.                                               |
> |        /?        |                                                            Affiche l'aide à l'invite de commandes.                                                            |
> 
> ## <a name="remarks"></a>Notes
> - **requête termserver** recherche sur le réseau tous les serveurs hôtes de session Bureau à distance attachés et retourne les informations suivantes :
>   - Nom du serveur
>   - Le réseau (et l’adresse du nœud si l’option/Address est utilisée)
>     ## <a name="BKMK_examples"></a>Illustre
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
>   #### <a name="additional-references"></a>Références supplémentaires
>   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
>   [requête](query.md)
>   [ &#40;services Bureau à distance&#41; référence de commande des services Terminal Server](remote-desktop-services-terminal-services-command-reference.md)
