---
ms.assetid: 27e1e299-0beb-4e86-8143-1ba031dc3502
title: Ajouter un certificat de déchiffrement de jeton
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5714a41950b9c2f818ddc154a9af7a55fdb362d8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814963"
---
# <a name="add-a-token-decrypting-certificate"></a>Ajouter un certificat de déchiffrement de jeton

Les serveurs de Fédération utilisent un certificat de déchiffrement\-lorsqu’un serveur de Fédération de partie de confiance doit déchiffrer les jetons émis avec un ancien certificat une fois qu’un nouveau certificat est défini en tant que certificat de déchiffrement principal. Services ADFS \(AD FS\) utilise le certificat protocole SSL \(SSL\) pour Internet Information Services \(IIS\) en tant que certificat de déchiffrement par défaut.  
  
> [!CAUTION]  
> Les certificats utilisés pour le déchiffrement des\-de jeton sont essentiels pour la stabilité du service FS (Federation Service). Étant donné que la perte ou la suppression non planifiée de tous les certificats configurés à cet effet peut perturber le service, vous devez sauvegarder tous les certificats configurés à cet effet.  
  
Vous pouvez utiliser la procédure suivante pour ajouter le jeton\-le déchiffrement du certificat au composant logiciel enfichable de gestion AD FS\-dans à partir d’un fichier que vous avez exporté.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs** ou d'un groupe équivalent sur l'ordinateur local.  Passez en revue les détails sur l’utilisation des comptes et des appartenances aux groupes appropriés dans les [groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \(http :\/\/Go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-token-decrypting-certificate"></a>Pour ajouter un jeton\-le déchiffrement du certificat  
  
1.  Dans l’écran d' **Accueil** , tapez**AD FS Management**, puis appuyez sur entrée.  
  
2.  Dans l’arborescence de la console, double\-cliquez sur **service**, puis sur **certificats**.  
  
3.  Dans le volet **actions** , cliquez sur le lien **ajouter un jeton\-le déchiffrement du certificat** .  
  
4.  Dans la boîte de dialogue **Rechercher un fichier de certificat** , accédez au fichier de certificat que vous souhaitez ajouter, sélectionnez le fichier de certificat, puis cliquez sur **ouvrir**.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : configuration d’un serveur de Fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Certificats requis pour les serveurs de fédération](https://technet.microsoft.com/library/dd807040.aspx)  
  

