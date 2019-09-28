---
ms.assetid: bbb84ea6-7e31-4442-85ab-a9447e7c19e8
title: Ajouter un certificat de signature de jetons
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c8b2246842dd70c06442faed995f6b883dbaf70a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360088"
---
# <a name="add-a-token-signing-certificate"></a>Ajouter un certificat de signature de jetons


Les serveurs de Fédération dans Services ADFS \(AD FS @ no__t-1 requièrent des certificats Token @ no__t-2signing pour empêcher les attaquants de modifier ou de contrefaire les jetons de sécurité en vue d’obtenir un accès non autorisé à fédéré situées. Chaque certificat\-de signature de jetons contient des clés privées de chiffrement et des clés publiques utilisées \(pour signer numériquement par le biais\) de la clé privée un jeton de sécurité. Plus tard, une fois que ces clés sont reçues par un serveur de Fédération partenaire, elles valident le moyen \(BY de la clé publique @ no__t-1 du jeton de sécurité chiffré.  
  
> [!CAUTION]  
> Les certificats utilisés pour le jeton @ no__t-0signing sont essentiels pour la stabilité du service FS (Federation Service). Étant donné que la perte ou la suppression non planifiée de tous les certificats configurés à cet effet peut perturber le service, vous devez sauvegarder tous les certificats configurés à cet effet.  
  
Le certificat @ no__t-0signing doit être lié à une racine approuvée dans le service FS (Federation Service). Vous pouvez utiliser la procédure suivante pour ajouter le certificat @ no__t-0signing au composant logiciel enfichable de gestion de AD FS @ no__t-1Dans d’un fichier que vous avez exporté.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Passez en revue les détails sur l’utilisation des comptes et des appartenances aux groupes appropriés dans les\/ \( [groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) http :\/Go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-token-signing-certificate"></a>Pour ajouter un certificat @ no__t-0signing  
  
1.  Dans l’écran d' **Accueil** , tapez**AD FS Management**, puis appuyez sur entrée.  
  
2.  Dans l’arborescence de la console, double @ no__t-0click **service**, puis cliquez sur **certificats**.  
  
3.  Dans le volet **actions** , cliquez sur le lien **Ajouter un jeton @ No__t-2Signing certificat** .  
  
4.  Dans la boîte de dialogue **Rechercher un fichier de certificat** , accédez au fichier de certificat que vous souhaitez ajouter, sélectionnez le fichier de certificat, puis cliquez sur **ouvrir**.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Certificats requis pour les serveurs de fédération](https://technet.microsoft.com/library/dd807040.aspx)  
  

