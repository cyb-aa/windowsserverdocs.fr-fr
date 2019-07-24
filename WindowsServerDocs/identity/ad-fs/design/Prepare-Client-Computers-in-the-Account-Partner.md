---
ms.assetid: cea6011d-3753-4b95-aaa5-38d4e97d6e42
title: Préparer les ordinateurs clients dans le partenaire de compte
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e3a746ec003cf312ffe0b9804f84a55c98aa8089
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190991"
---
# <a name="prepare-client-computers-in-the-account-partner"></a>Préparer les ordinateurs clients dans le partenaire de compte

Le moyen le plus simple pour un administrateur d’un compte partenaire organisation pour préparer les ordinateurs clients pour l’accès à Active Directory Federation Services \(AD FS\) applications fédérées consiste à utiliser la stratégie de groupe. La stratégie de groupe permet de distribuer les certificats et les paramètres requis pour la fédération sur tous les ordinateurs clients qui seront utilisés pour accéder aux applications fédérées.  
  
Pour que vos ordinateurs clients puissent accéder en toute transparence des applications fédérées sans invite de certificat ni invites liée à un site de confiance, nous vous recommandons de tout d’abord préparer chaque ordinateur client avant de déployer AD FS largement de votre organisation. Utilisez la stratégie de groupe pour automatiquement :  
  
-   Configurez Internet Explorer sur chaque ordinateur client à approuver le serveur de fédération de compte.  
  
    Pour plus d’informations, consultez [configurer des ordinateurs clients à approuver le serveur de fédération de compte](../../ad-fs/deployment/Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md).  
  
-   Installer le serveur de fédération de compte approprié, le serveur de fédération de ressources et le serveur Web (Secure Sockets Layer) \(SSL\) certificats \(ou équivalent certificats liés à une racine approuvée\) sur chaque ordinateur client.  
  
    Pour plus d’informations, consultez [distribuer des certificats sur les ordinateurs clients à l’aide de la stratégie de groupe](../../ad-fs/deployment/Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md).  
  

## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
