---
title: ipxroute
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3a30304f-655e-43d2-a4ac-7568abf8975c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1e011835dbdbcf7be1daca2cdfbd47c39f9355c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842062"
---
# <a name="ipxroute"></a>ipxroute

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche et modifie les informations relatives aux tables de routage utilisées par le protocole IPX. Utilisé sans paramètres, **ipxroute** affiche les paramètres par défaut pour les paquets envoyés à des adresses inconnues, de diffusion et de multidiffusion.   
## <a name="syntax"></a>Syntaxe  
```  
ipxroute servers [/type=X]  
ipxroute ripout <Network>  
ipxroute resolve {guid | name} {GUID | <AdapterName>}  
ipxroute board= N [def] [gbr] [mbr] [remove=xxxxxxxxxxxx]  
ipxroute config  
```  
#### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|serveurs [/type = X]|Affiche la table du point d’accès de service (SAP) pour le type de serveur spécifié.  **X** doit être un entier. Par exemple, **/type = 4** affiche tous les serveurs de fichiers. Si vous ne spécifiez pas **/type**, **ipxroute** Servers affiche tous les types de serveurs, en les répertoriant par nom de serveur.|  
|Réseau ripout|Détecte si le *réseau* est accessible en consultant la table de routage de la pile IPX et en envoyant une demande RIP, si nécessaire.  *Network* est le numéro de segment réseau IPX.|  
|résoudre {nom&#124; GUID} {GUID&#124; AdapterName}|Résout le nom du GUID en son nom convivial ou le nom convivial en son GUID.|  
|Board = *N*|Spécifie la carte réseau pour laquelle interroger ou définir des paramètres.|  
|def|Envoie des paquets à la diffusion tous les itinéraires. Si un paquet est transmis à une adresse MAC (Media Access Card) unique qui ne figure pas dans la table de routage source, **ipxroute** envoie le paquet à la diffusion d’itinéraires uniques par défaut.|  
|gbr|Envoie des paquets à la diffusion tous les itinéraires. Si un paquet est transmis à l’adresse de diffusion (FFFFFFFFFFFF), **ipxroute** envoie le paquet à la diffusion d’itinéraires uniques par défaut.|  
|démarrage|Envoie des paquets à la diffusion tous les itinéraires. Si un paquet est transmis à une adresse de multidiffusion (C000xxxxxxxx), **ipxroute** envoie le paquet à la diffusion d’itinéraires uniques par défaut.|  
|Remove = *xxxxxxxxxxxx*|supprime l’adresse de nœud donnée de la table de routage source.|  
|config|Affiche des informations sur toutes les liaisons pour lesquelles IPX est configuré.|  
|/?|Affiche l'aide à l'invite de commandes.|  
## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
Pour afficher les segments réseau auxquels la station de travail est attachée, l’adresse du nœud de la station de travail et le type de trame utilisé, tapez :  
```  
ipxroute config  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
