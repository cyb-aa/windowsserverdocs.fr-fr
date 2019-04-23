---
ms.assetid: 27e1e299-0beb-4e86-8143-1ba031dc3502
title: Ajouter un certificat de déchiffrement de jeton
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0eba51521e7ef88542bccf93d92d2e783d800b5e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842210"
---
# <a name="add-a-token-decrypting-certificate"></a>Ajouter un certificat de déchiffrement de jeton

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Serveurs de fédération utilisent un jeton\-certificat de déchiffrement lorsqu’un serveur de fédération tiers partie de confiance doit déchiffrer les jetons sont émis avec un ancien après un nouveau certificat est défini en tant que le certificat de déchiffrement principal. Active Directory Federation Services \(AD FS\) utilise Secure Sockets Layer \(SSL\) certificat pour Internet Information Services \(IIS\) en tant que le déchiffrement par défaut certificat.  
  
> [!CAUTION]  
> Certificats utilisés pour le jeton\-déchiffrement sont essentielles à la stabilité du Service de fédération. Étant donné que la perte ou suppression non planifiée de tous les certificats configurés à cet effet peut perturber le service, vous devez sauvegarder tous les certificats configurés à cet effet.  
  
Vous pouvez utiliser la procédure suivante pour ajouter le jeton\-déchiffrer le certificat pour le composant logiciel enfichable Gestion AD FS\-dans à partir d’un fichier que vous avez exportées.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’aide de comptes appropriés et les appartenances au groupe [locaux et les groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \(http :\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-token-decrypting-certificate"></a>Pour ajouter un jeton\-certificat de déchiffrement  
  
1.  Sur le **Démarrer** , tapez**gestion AD FS**, puis appuyez sur ENTRÉE.  
  
2.  Dans l’arborescence de la console, double-cliquez\-cliquez sur **Service**, puis cliquez sur **certificats**.  
  
3.  Dans le **Actions** volet, cliquez sur le **ajouter un jeton\-certificat de déchiffrement** lien.  
  
4.  Dans le **rechercher le fichier de certificat** boîte de dialogue, accédez au fichier de certificat que vous souhaitez ajouter, sélectionnez le fichier de certificat, puis cliquez sur **Open**.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Configuration requise des certificats pour les serveurs de fédération](https://technet.microsoft.com/library/dd807040.aspx)  
  

