---
ms.assetid: bbb84ea6-7e31-4442-85ab-a9447e7c19e8
title: Ajouter un certificat de signature de jetons
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8a94e0724d6fd2a04e2fbfc22b3054b49d87f440
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-token-signing-certificate"></a>Ajouter un certificat de signature de jetons

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Les serveurs de fédération dans ActiveDirectory Federation Services \(ADFS\) requièrent des certificats de signature de token\ pour empêcher les intrus de modifier ou de contrefaçon de jetons de sécurité dans une tentative d’accès non autorisé aux ressources fédérées. Chaque certificat de signature de token\ contient des clés privées de chiffrement et des clés publiques qui sont utilisés pour signer numériquement \ (au moyen de la clé privée) un jeton de sécurité. Plus tard, une fois ces clés sont reçues par un serveur de fédération du partenaire, ils valident l’authenticité \ (au moyen de la clé publique) du jeton de sécurité chiffré.  
  
> [!CAUTION]  
> Certificats utilisés pour la signature de token\ sont essentielles à la stabilité du Service de fédération. Étant donné que la perte ou suppression non planifiée d’un certificat configuré à cette fin peut perturber le service, vous devez sauvegarder tous les certificats configurés à cet effet.  
  
Le certificat de signature token\ doit être lié à une racine approuvée dans le Service de fédération. Vous pouvez utiliser la procédure suivante pour ajouter le certificat de signature token\ à la gestion ADFS enfichable à partir d’un fichier que vous avez exportés.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-add-a-token-signing-certificate"></a>Pour ajouter un certificat de signature token\  
  
1.  Sur le **Démarrer**, tapez**gestion ADFS**, puis appuyez sur ENTRÉE.  
  
2.  Dans l’arborescence de la console, double-cliquant sur **Service**, puis cliquez sur **certificats**.  
  
3.  Dans le **Actions** volet, cliquez sur le **certificat de signature de Token\ ajouter** lien.  
  
4.  Dans le **rechercher un fichier de certificat** boîte de dialogue zone, accédez au fichier de certificat que vous souhaitez ajouter, sélectionnez le fichier de certificat, puis cliquez sur **ouvrir**.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification: Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Certificats requis pour les serveurs de fédération](https://technet.microsoft.com/library/dd807040.aspx)  
  

