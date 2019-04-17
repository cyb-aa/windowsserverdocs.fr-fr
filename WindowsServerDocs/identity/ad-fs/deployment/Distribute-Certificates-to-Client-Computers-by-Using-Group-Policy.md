---
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: "Distribuer des certificats sur les ordinateurs clients à l’aide de stratégie de groupe"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d3a7e05e4d16565b17b69de254e353df749bbc3a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="distribute-certificates-to-client-computers-by-using-group-policy"></a>Distribuer des certificats sur les ordinateurs clients à l’aide de stratégie de groupe

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012


Vous pouvez utiliser la procédure suivante pour pousser les certificats Secure Sockets Layer \(SSL\) appropriés \ (ou l’équivalent les certificats liés à un root approuvée) pour les serveurs de fédération de comptes, les serveurs de fédération de ressources et les serveurs Web pour chaque ordinateur client dans la forêt du partenaire de compte à l’aide de stratégie de groupe.  
  
L’appartenance au groupe **Admins du domaine** ou **administrateurs de l’entreprise**, ou équivalent, dans les Services de domaine Active Directory \(AD DS\) est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>Pour distribuer des certificats sur les ordinateurs clients à l’aide de stratégie de groupe  
  
1.  Sur un contrôleur de domaine dans la forêt de l’organisation partenaire de compte, démarrez le **gestion des stratégies de groupe** logiciel enfichable.  
  
2.  Trouver un \(GPO\) objet de stratégie de groupe existant ou créer un nouvel objet GPO pour contenir les paramètres de certificat. Assurez-vous que l’objet de stratégie de groupe est associé au domaine, site ou unité d’organisation \(OU\) où résident les comptes d’utilisateur et d’ordinateur appropriés.  
  
3.  L’objet de stratégie de groupe clic droit, puis cliquez sur **modifier**.  
  
4.  Dans l’arborescence de la console, ouvrez **stratégies de clé ordinateur Configuration\\Policies\\Windows sécurité Settings\\Public**, clic droit **Trusted Root Certification Authorities**, puis cliquez sur **Import**.  
  
5.  Sur le **Bienvenue dans l’Assistant Importation de certificat**, cliquez sur **suivant**.  
  
6.  Sur le **fichier à importer**, tapez le chemin d’accès aux fichiers de certificat approprié \ (par exemple, \\\fs1\\c$\\fs1.cer\), puis cliquez sur **suivant**.  
  
7.  Sur le **magasin de certificats**, cliquez sur **placer tous les certificats dans le magasin suivant**, puis cliquez sur **suivant**.  
  
8.  Sur le **fin de l’Assistant Importation de certificat** page, vérifiez que les informations fournies sont précises, puis cliquez sur **Terminer**.  
  
9. Répétez les étapes2 à 6 pour ajouter des certificats supplémentaires pour chacun des serveurs de fédération de la batterie de serveurs.  
