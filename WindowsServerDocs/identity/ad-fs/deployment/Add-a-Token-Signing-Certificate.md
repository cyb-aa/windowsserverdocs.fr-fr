---
ms.assetid: bbb84ea6-7e31-4442-85ab-a9447e7c19e8
title: Ajouter un certificat de signature de jetons
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 9b737cf8c9efb89ef9b3befaa1875b273bfcadf9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814932"
---
# <a name="add-a-token-signing-certificate"></a>Ajouter un certificat de signature de jetons


Les serveurs de Fédération dans Services ADFS \(AD FS\) requièrent des certificats de signature\-pour empêcher des personnes malveillantes de modifier ou de contrefaire les jetons de sécurité en tentant d’accéder de façon non autorisée aux ressources fédérées. Chaque jeton\-certificat de signature contient des clés privées de chiffrement et des clés publiques qui sont utilisées pour signer numériquement \(au moyen de la clé privée\) un jeton de sécurité. Plus tard, une fois que ces clés sont reçues par un serveur de Fédération partenaire, elles valident l’authenticité \(au moyen de la clé publique\) du jeton de sécurité chiffré.  
  
> [!CAUTION]  
> Les certificats utilisés pour la signature du jeton\-sont essentiels pour la stabilité du service FS (Federation Service). Étant donné que la perte ou la suppression non planifiée de tous les certificats configurés à cet effet peut perturber le service, vous devez sauvegarder tous les certificats configurés à cet effet.  
  
Le jeton\-certificat de signature doit être lié à une racine approuvée dans le service FS (Federation Service). Vous pouvez utiliser la procédure suivante pour ajouter le jeton\-certificat de signature au composant logiciel enfichable de gestion AD FS\-dans à partir d’un fichier que vous avez exporté.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs** ou d'un groupe équivalent sur l'ordinateur local.  Passez en revue les détails sur l’utilisation des comptes et des appartenances aux groupes appropriés dans les [groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \(http :\/\/Go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-token-signing-certificate"></a>Pour ajouter un jeton\-certificat de signature  
  
1.  Dans l’écran d' **Accueil** , tapez**AD FS Management**, puis appuyez sur entrée.  
  
2.  Dans l’arborescence de la console, double\-cliquez sur **service**, puis sur **certificats**.  
  
3.  Dans le volet **actions** , cliquez sur le lien **ajouter un jeton\-certificat de signature** .  
  
4.  Dans la boîte de dialogue **Rechercher un fichier de certificat** , accédez au fichier de certificat que vous souhaitez ajouter, sélectionnez le fichier de certificat, puis cliquez sur **ouvrir**.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : configuration d’un serveur de Fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Certificats requis pour les serveurs de fédération](https://technet.microsoft.com/library/dd807040.aspx)  
  

