---
ms.assetid: 27e1e299-0beb-4e86-8143-1ba031dc3502
title: "Ajouter un certificat de déchiffrement de jeton"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0eba51521e7ef88542bccf93d92d2e783d800b5e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-token-decrypting-certificate"></a>Ajouter un certificat de déchiffrement de jeton

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Serveurs de fédération utilisent un certificat de déchiffrement de token\ lorsqu’un serveur de fédération de partie confiance doit déchiffrer les jetons émis avec un ancien après qu’un nouveau certificat est défini comme le certificat de déchiffrement principal. ActiveDirectory Federation Services \(ADFS\) utilise le certificat \(SSL\) (Secure Sockets Layer) pour Internet Information Services \(IIS\) en tant que le certificat de déchiffrement par défaut.  
  
> [!CAUTION]  
> Certificats utilisés pour le déchiffrement de token\ sont essentielles à la stabilité du Service de fédération. Étant donné que la perte ou suppression non planifiée d’un certificat configuré à cette fin peut perturber le service, vous devez sauvegarder tous les certificats configurés à cet effet.  
  
Vous pouvez utiliser la procédure suivante pour ajouter le certificat de déchiffrement de token\ pour la gestion ADFS enfichable à partir d’un fichier que vous avez exportés.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-add-a-token-decrypting-certificate"></a>Pour ajouter un certificat de déchiffrement de token\  
  
1.  Sur le **Démarrer**, tapez**gestion ADFS**, puis appuyez sur ENTRÉE.  
  
2.  Dans l’arborescence de la console, double-cliquant sur **Service**, puis cliquez sur **certificats**.  
  
3.  Dans le **Actions** volet, cliquez sur le **certificat de déchiffrement Token\ ajouter** lien.  
  
4.  Dans le **rechercher un fichier de certificat** boîte de dialogue zone, accédez au fichier de certificat que vous souhaitez ajouter, sélectionnez le fichier de certificat, puis cliquez sur **ouvrir**.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification: Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Certificats requis pour les serveurs de fédération](https://technet.microsoft.com/library/dd807040.aspx)  
  

