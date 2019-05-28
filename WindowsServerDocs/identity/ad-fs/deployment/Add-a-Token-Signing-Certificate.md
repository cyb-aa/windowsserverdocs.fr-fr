---
ms.assetid: bbb84ea6-7e31-4442-85ab-a9447e7c19e8
title: Ajouter un certificat de signature de jetons
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ac9f9b95ad6226a8e3b7012e317899f1d48c60c9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192468"
---
# <a name="add-a-token-signing-certificate"></a>Ajouter un certificat de signature de jetons


Serveurs de fédération dans Active Directory Federation Services \(AD FS\) nécessitent le jeton\-certificats de signature pour empêcher les attaquants de modifier ou de contrefaçon des jetons de sécurité dans une tentative d’accès non autorisé ressources fédérées. Chaque jeton\-certificat de signature contient des clés privées de chiffrement et des clés publiques qui sont utilisés pour signer numériquement \(au moyen de la clé privée\) un jeton de sécurité. Plus tard, une fois que ces clés sont reçues par un serveur de fédération du partenaire, ils valident l’authenticité \(au moyen de la clé publique\) du jeton de sécurité chiffré.  
  
> [!CAUTION]  
> Certificats utilisés pour le jeton\-signature sont essentielles à la stabilité du Service de fédération. Étant donné que la perte ou suppression non planifiée de tous les certificats configurés à cet effet peut perturber le service, vous devez sauvegarder tous les certificats configurés à cet effet.  
  
Le jeton\-certificat de signature doivent être liés à une racine approuvée dans le Service de fédération. Vous pouvez utiliser la procédure suivante pour ajouter le jeton\-certificat de signature pour le composant logiciel enfichable Gestion AD FS\-dans à partir d’un fichier que vous avez exportées.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’aide de comptes appropriés et les appartenances au groupe [locaux et les groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \(http :\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-token-signing-certificate"></a>Pour ajouter un jeton\-certificat de signature  
  
1.  Sur le **Démarrer** , tapez**gestion AD FS**, puis appuyez sur ENTRÉE.  
  
2.  Dans l’arborescence de la console, double-cliquez\-cliquez sur **Service**, puis cliquez sur **certificats**.  
  
3.  Dans le **Actions** volet, cliquez sur le **ajouter un jeton\-certificat de signature** lien.  
  
4.  Dans le **rechercher le fichier de certificat** boîte de dialogue, accédez au fichier de certificat que vous souhaitez ajouter, sélectionnez le fichier de certificat, puis cliquez sur **Open**.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Certificats requis pour les serveurs de fédération](https://technet.microsoft.com/library/dd807040.aspx)  
  

