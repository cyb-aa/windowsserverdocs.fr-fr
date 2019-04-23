---
title: unlodctr
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fc8aa6f0-c1d9-47ea-937a-28152148e774
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e1a662da10acc65b4ad2fd0d055cf9d46de603be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886650"
---
# <a name="unlodctr"></a>unlodctr

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime les noms de compteur de performances et le texte d’explication pour un pilote de service ou un périphérique à partir du Registre système.   

## <a name="syntax"></a>Syntaxe  
```  
Unlodctr <DriverName>   
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|\<DriverName>|Supprime les performances des paramètres de nom de compteur et le texte pour le pilote ou le service explicatif <DriverName> à partir du Registre de Windows Server 2003.|  
|/?|Affiche l'aide à l'invite de commandes.|  

## <a name="remarks"></a>Notes  
> [!WARNING]  
> Une modification incorrecte du Registre peut endommager gravement votre système. Avant toute modification du registre, il est conseillé de sauvegarder toutes les données importantes de votre ordinateur.  

Si les informations que vous fournissez contient des espaces, utilisez des guillemets autour du texte (par exemple, «<DriverName>»).  

## <a name="BKMK_Examples"></a>Exemples  
Pour supprimer les paramètres de Registre de performances actuels et compteur de texte d’explication pour le service de transfert de protocole SMTP (Simple Mail) :  
```  
unlodctr SMTPSVC  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
  
