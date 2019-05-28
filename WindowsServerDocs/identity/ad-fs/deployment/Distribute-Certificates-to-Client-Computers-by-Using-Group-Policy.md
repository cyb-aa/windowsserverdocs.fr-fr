---
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: Distribuer des certificats sur les ordinateurs clients à l’aide de stratégie de groupe
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 11cdd9c75ca588ebeac9387e6512fee439621bf8
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192158"
---
# <a name="distribute-certificates-to-client-computers-by-using-group-policy"></a>Distribuer des certificats sur les ordinateurs clients à l’aide de stratégie de groupe


Vous pouvez utiliser la procédure suivante pour pousser approprié Secure Sockets Layer \(SSL\) certificats \(ou équivalent certificats liés à une racine approuvée\) pour les serveurs de fédération de compte, serveurs de fédération de ressources et les serveurs Web à chaque ordinateur client dans la forêt du partenaire de compte à l’aide de stratégie de groupe.  
  
L’appartenance au **Admins du domaine** ou **administrateurs de l’entreprise**, ou équivalent, dans les Services de domaine Active Directory \(AD DS\) est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide de comptes appropriés et les appartenances au groupe [locaux et les groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \(http :\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>Pour distribuer des certificats sur les ordinateurs clients à l’aide de stratégie de groupe  
  
1.  Sur un contrôleur de domaine dans la forêt de l’organisation partenaire de compte, démarrez le **Group Policy Management** aligner\-dans.  
  
2.  Rechercher un objet de stratégie de groupe existant \(GPO\) ou créer un nouvel objet GPO pour qu’il contienne les paramètres de certificat. Vérifiez que l’objet de stratégie de groupe est associé avec le domaine, un site ou une unité d’organisation \(unité d’organisation\) où se trouvent les comptes d’utilisateurs et d’ordinateurs appropriés.  
  
3.  Droite\-cliquez sur l’objet de stratégie de groupe, puis cliquez sur **modifier**.  
  
4.  Dans l’arborescence de la console, ouvrez **Configuration ordinateur\\stratégies\\Windows paramètres\\paramètres de sécurité\\stratégies de clé publique**, à droite\-cliquez sur **Trusted Root Certification Authorities**, puis cliquez sur **importation**.  
  
5.  Dans la page **Bienvenue** de l'Assistant Importation de certificat, cliquez sur **Suivant**.  
  
6.  Sur le **fichier à importer** , tapez le chemin d’accès aux fichiers de certificat approprié \(, par exemple, \\ \\fs1\\c$\\fs1.cer\), puis cliquez sur **Suivant**.  
  
7.  Sur le **certificat Store** , cliquez sur **placer tous les certificats dans le magasin suivant**, puis cliquez sur **suivant**.  
  
8.  Sur le **fin de l’Assistant Importation de certificat** page, vérifiez que les informations que vous avez fourni sont exactes, puis cliquez sur **Terminer**.  
  
9. Répétez les étapes 2 à 6 pour ajouter des certificats supplémentaires pour chacun des serveurs de fédération de la batterie de serveurs.  
