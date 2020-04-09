---
ms.assetid: cea6011d-3753-4b95-aaa5-38d4e97d6e42
title: Préparer les ordinateurs clients dans le partenaire de compte
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 014524c78312c6fcd478b40ec47e212a194b0531
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858592"
---
# <a name="prepare-client-computers-in-the-account-partner"></a>Préparer les ordinateurs clients dans le partenaire de compte

Le moyen le plus simple pour un administrateur dans une organisation partenaire de compte de préparer les ordinateurs clients à l’accès à Services ADFS \(AD FS\) applications fédérées consiste à utiliser stratégie de groupe. La stratégie de groupe permet de distribuer les certificats et les paramètres requis pour la fédération sur tous les ordinateurs clients qui seront utilisés pour accéder aux applications fédérées.  
  
Afin que vos ordinateurs clients puissent accéder en toute transparence aux applications fédérées sans invite de certificat ni invites de site de confiance, nous vous recommandons de préparer tout d’abord chaque ordinateur client avant de déployer AD FS de manière large dans votre organisation. Utilisez la stratégie de groupe pour automatiquement :  
  
-   Configurez Internet Explorer sur chaque ordinateur client pour faire confiance au serveur de Fédération de comptes.  
  
    Pour plus d'informations, voir [Configure Client Computers to Trust the Account Federation Server](../../ad-fs/deployment/Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md).  
  
-   Installez le serveur de Fédération de compte approprié, le serveur de Fédération de ressources et le serveur Web protocole SSL \(SSL\) certificats \(ou des certificats équivalents qui sont liés à un\) racine approuvé sur chaque ordinateur client.  
  
    Pour plus d’informations, consultez [distribuer des certificats aux ordinateurs clients à l’aide de stratégie de groupe](../../ad-fs/deployment/Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md).  
  

## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
