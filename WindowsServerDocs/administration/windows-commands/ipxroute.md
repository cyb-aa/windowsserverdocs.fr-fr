---
title: ipxroute
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3a30304f-655e-43d2-a4ac-7568abf8975c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d995204eea0af776a2084a82411fa95542d1d77a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889090"
---
# <a name="ipxroute"></a>ipxroute

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche et modifie les informations sur les tables de routage utilisées par le protocole IPX. Utilisée sans paramètres, **ipxroute** affiche les paramètres par défaut pour les paquets qui sont envoyés aux adresses de multidiffusion, diffusions et inconnus.   
## <a name="syntax"></a>Syntaxe  
```  
ipxroute servers [/type=X]  
ipxroute ripout <Network>  
ipxroute resolve {guid | name} {GUID | <AdapterName>}  
ipxroute board= N [def] [gbr] [mbr] [remove=xxxxxxxxxxxx]  
ipxroute config  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|servers[ /type=X]|Affiche la table de Point d’accès de Service (SAP) pour le type de serveur spécifié.  **X** doit être un entier. Par exemple, **/type = 4** affiche tous les serveurs de fichiers. Si vous ne spécifiez pas **/type**, **de routage IPX** affiche tous les types de serveurs, en les répertoriant par le nom du serveur.|  
|ripout réseau|Détecte si *réseau* est accessible en consultant la table de routage de la pile IPX et envoyer une demande rip si nécessaire.  *Réseau* est le numéro de segment de réseau IPX.|  
|résoudre {GUID&#124; nom} {GUID&#124; AdapterName}|Résout le nom du GUID à son nom convivial ou le nom convivial à son GUID.|  
|board= *N*|Spécifie la carte réseau pour lequel interroger ou définir des paramètres.|  
|def|Envoie des paquets à la diffusion de tous les itinéraires. Si un paquet est transmis à une adresse de carte MAC (Media Access) unique qui n’est pas dans la table de routage source, **ipxroute** envoie le paquet à la diffusion par défaut de ROUTES unique.|  
|gbr|Envoie des paquets à la diffusion de tous les itinéraires. Si un paquet est transmis à l’adresse de diffusion (FFFFFFFFFFFF), **ipxroute** envoie le paquet à la diffusion par défaut de ROUTES unique.|  
|mbr|Envoie des paquets à la diffusion de tous les itinéraires. Si un paquet est transmis à une adresse de multidiffusion (C000xxxxxxxx), **ipxroute** envoie le paquet à la diffusion par défaut de ROUTES unique.|  
|remove= *xxxxxxxxxxxx*|Supprime l’adresse du nœud donné de la table de routage source.|  
|config|Affiche des informations sur toutes les liaisons pour lesquelles IPX est configuré.|  
|/?|Affiche l'aide à l'invite de commandes.|  
## <a name="BKMK_Examples"></a>Exemples  
Pour afficher les segments réseau que la station de travail est attachée à l’adresse du nœud de station de travail et type de trame utilisé, tapez :  
```  
ipxroute config  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
