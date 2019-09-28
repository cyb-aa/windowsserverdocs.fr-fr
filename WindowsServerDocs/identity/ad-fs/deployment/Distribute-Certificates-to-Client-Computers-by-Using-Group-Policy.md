---
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: Distribuer des certificats aux ordinateurs clients à l’aide de stratégie de groupe
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1d9692bc099174f15b77e792087f4c7055bf85d1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359623"
---
# <a name="distribute-certificates-to-client-computers-by-using-group-policy"></a>Distribuer des certificats aux ordinateurs clients à l’aide de stratégie de groupe


Vous pouvez utiliser la procédure suivante pour pousser les certificats équivalents de l’protocole SSL \(SSL @ no__t-1 \(or qui s’enchaînent à une racine approuvée @ no__t-3 pour les serveurs de Fédération de comptes, les serveurs de Fédération de ressources et Serveurs Web sur chaque ordinateur client de la forêt du partenaire de compte à l’aide de stratégie de groupe.  
  
Pour effectuer cette procédure, vous devez au minimum appartenir au groupe **Admins du domaine** ou administrateurs de l' **entreprise**ou à un groupe équivalent dans Active Directory Domain Services \(AD DS @ no__t-3.  Passez en revue les détails sur l’utilisation des comptes et des appartenances aux groupes appropriés dans les\/ \( [groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) http :\/Go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>Pour distribuer des certificats aux ordinateurs clients à l’aide de stratégie de groupe  
  
1.  Sur un contrôleur de domaine dans la forêt de l’organisation partenaire de compte, démarrez le composant logiciel enfichable de **gestion de stratégie de groupe** @ no__t-1Dans.  
  
2.  Recherchez un objet stratégie de groupe existant \(GPO @ no__t-1 ou créez un nouvel objet de stratégie de groupe contenant les paramètres du certificat. Assurez-vous que l’objet de stratégie de groupe est associé au domaine, au site ou à l’unité d’organisation @no__t 0OU @ no__t-1 où se trouvent les comptes d’utilisateur et d’ordinateur appropriés.  
  
3.  Right @ no__t-0click l’objet de stratégie de groupe, puis cliquez sur **modifier**.  
  
4.  Dans l’arborescence de la console, ouvrez **Configuration ordinateur @ no__t-1Policies @ no__t-2Windows paramètres @ no__t-3Security paramètres @ no__t-4Public stratégies de clé**, à droite @ no__t-5Click **autorités de certification racines de confiance**, puis cliquez sur **Importer.** .  
  
5.  Dans la page **Bienvenue** de l'Assistant Importation de certificat, cliquez sur **Suivant**.  
  
6.  Sur la page **fichier à importer** , tapez le chemin d’accès aux fichiers de certificat appropriés \(Pour exemple, \\ @ no__t-3FS1 @ no__t-4c $ @no__t -5fs1. cer @ no__t-6, puis cliquez sur **suivant**.  
  
7.  Dans la page **magasin de certificats** , cliquez sur **Placer tous les certificats dans le magasin suivant**, puis cliquez sur **suivant**.  
  
8.  Dans la page **fin de l’Assistant importation de certificat** , vérifiez que les informations que vous avez fournies sont exactes, puis cliquez sur **Terminer**.  
  
9. Répétez les étapes 2 à 6 pour ajouter des certificats supplémentaires pour chaque serveur de Fédération de la batterie de serveurs.  
