---
title: unlodctr
description: Rubrique relative aux commandes Windows pour unlodctr, qui supprime les noms des compteurs de performance et le texte d’explication pour un service ou un pilote de périphérique du Registre système
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fc8aa6f0-c1d9-47ea-937a-28152148e774
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe7fc3c9eafefd59a5daab625e3af06b6addd292
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832256"
---
# <a name="unlodctr"></a>unlodctr

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime du Registre système les noms des **compteurs de performances** et le texte d' **explication** pour un service ou un pilote de périphérique.   

## <a name="syntax"></a>Syntaxe  
```  
Unlodctr <DriverName>   
```  
#### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|\<DriverName >|supprime les paramètres de nom de compteur de performance et le texte d’explication pour le pilote ou le service <DriverName> à partir du registre de Windows Server 2003.|  
|/?|Affiche l'aide à l'invite de commandes.|  

## <a name="remarks"></a>Notes  
> [!WARNING]  
> Une modification incorrecte du Registre peut sérieusement endommager votre système. Avant toute modification du registre, il est conseillé de sauvegarder toutes les données importantes de votre ordinateur.  

Si les informations que vous fournissez contiennent des espaces, utilisez des guillemets autour du texte (par exemple, <DriverName>).  

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
Pour supprimer les paramètres de registre de performances actuels et le texte d’explication du compteur pour le service SMTP (Simple Mail Transfer Protocol) :  
```  
unlodctr SMTPSVC  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  
