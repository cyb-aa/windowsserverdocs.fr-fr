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
ms.openlocfilehash: cf89972120f3f0effa3eb1cf0fee6d29dbc8ed4e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192485"
---
# <a name="add-a-token-decrypting-certificate"></a>Ajouter un certificat de déchiffrement de jeton

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
  
[Certificats requis pour les serveurs de fédération](https://technet.microsoft.com/library/dd807040.aspx)  
  

