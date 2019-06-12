---
title: requête termserver
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 14270092949baf37588059d592e6f92e694a1739
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442094"
---
# <a name="query-termserver"></a>requête termserver

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche une liste de tous les serveurs hôte de Session Bureau à distance (hôte de Session Bureau à distance) sur le réseau.
Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#BKMK_examples).
> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour savoir quelles sont les nouveautés dans la version la plus récente, consultez [les nouveautés des Services Bureau à distance dans Windows Server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.
> ## <a name="syntax"></a>Syntaxe
> ```
> query termserver [<ServerName>] [/domain:<Domain>] [/address] [/continue]
> ```
> ## <a name="parameters"></a>Paramètres
> 
> |    Paramètre     |                                                                        Description                                                                         |
> |------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |   <ServerName>   |                                               Spécifie le nom qui identifie le serveur hôte de Session Bureau à distance.                                               |
> | /Domain :<Domain> | Spécifie le domaine à la requête pour les serveurs Terminal Server. Vous n’avez pas besoin de spécifier un domaine si vous interrogez le domaine dans lequel vous travaillez actuellement. |
> |     / Address     |                                                  Affiche les adresses réseau et de nœud pour chaque serveur.                                                  |
> |    / continuer     |                                              Empêche la pause après que chaque écran d’information s’affiche.                                               |
> |        /?        |                                                            Affiche l'aide à l'invite de commandes.                                                            |
> 
> ## <a name="remarks"></a>Notes
> - **interroger termserver** recherche sur le réseau pour toutes les attaché des serveurs d’hôte de Session Bureau à distance et renvoie les informations suivantes :
>   - Le nom du serveur
>   - Le réseau (et l’adresse de nœud si l’option/Address est utilisée)
>     ## <a name="BKMK_examples"></a>Exemples
> - Pour afficher des informations sur tous les serveurs d’hôte de Session Bureau à distance sur le réseau, tapez :
>   ```
>   query termserver
>   ```
> - Pour afficher des informations sur le serveur hôte de Session Bureau à distance nommée Server3, tapez :
>   ```
>   query termserver Server3
>   ```
> - Pour afficher des informations sur tous les serveurs d’hôte de Session Bureau à distance dans le domaine CONTOSO, tapez :
>   ```
>   query termserver /domain:CONTOSO
>   ```
> - Pour afficher l’adresse réseau et de nœud pour le serveur hôte de Session Bureau à distance nommé Server3, tapez :
>   ```
>   query termserver Server3 /address
>   ```
>   #### <a name="additional-references"></a>Références supplémentaires
>   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
>   [requête](query.md)
>   [Services Bureau à distance &#40;Services Terminal Server&#41; référence des commandes](remote-desktop-services-terminal-services-command-reference.md)
