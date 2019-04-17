---
ms.assetid: cea6011d-3753-4b95-aaa5-38d4e97d6e42
title: "Préparer les ordinateurs clients du partenaire de compte"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5c20b3d58c4e70fdb72a4aa54f7a05bd7bb5c347
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: fr-FR
---
# <a name="prepare-client-computers-in-the-account-partner"></a>Préparer les ordinateurs clients du partenaire de compte

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Le moyen le plus simple pour un administrateur d’une organisation partenaire de compte pour préparer les ordinateurs clients pour l’accès à ActiveDirectory Federation Services \(ADFS\) fédéré applications consiste à utiliser une stratégie de groupe. Stratégie de groupe fournit un moyen pratique pour distribuer les certificats et les paramètres qui sont requis pour la fédération sur tous les ordinateurs clients qui servira à accéder aux applications fédérées.  
  
Afin que vos ordinateurs clients peuvent accéder en toute transparence aux applications fédérées sans invite de certificat ou approuvés invite liée à un site, nous vous recommandons d’abord préparer chaque ordinateur client avant de déployer ADFS à grande échelle dans votre organisation. Utilisez la stratégie de groupe pour automatiquement:  
  
-   Configurez Internet Explorer sur chaque ordinateur client pour approuver le serveur de fédération de comptes.  
  
    Pour plus d’informations, voir [configurer des ordinateurs clients d’approuver le serveur de fédération de comptes](../../ad-fs/deployment/Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md).  
  
-   Installer le serveur de fédération de compte approprié, serveur de fédération de ressources et certificats Secure Sockets Layer \(SSL\) de serveur Web \ (ou l’équivalent les certificats liés à un root approuvée) sur chaque ordinateur client.  
  
    Pour plus d’informations, voir [distribuer des certificats pour les ordinateurs clients à l’aide de la stratégie de groupe](../../ad-fs/deployment/Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md).  
  

## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
