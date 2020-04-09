---
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: Distribuer des certificats aux ordinateurs clients à l’aide de stratégie de groupe
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c0e1ab21faa42789bee20c7b8945284aba068310
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855442"
---
# <a name="distribute-certificates-to-client-computers-by-using-group-policy"></a>Distribuer des certificats aux ordinateurs clients à l’aide de stratégie de groupe


Vous pouvez utiliser la procédure suivante pour envoyer les certificats de\) protocole SSL \(SSL appropriés \(ou des certificats équivalents qui sont liés à un\) racine approuvé pour les serveurs de Fédération de comptes, les serveurs de Fédération de ressources et les serveurs Web sur chaque ordinateur client de la forêt du partenaire de compte à l’aide de stratégie de groupe.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Admins du domaine** ou administrateurs de l' **entreprise**, ou à un groupe équivalent, dans Active Directory Domain Services \(AD DS\).  Passez en revue les détails sur l’utilisation des comptes et des appartenances aux groupes appropriés dans les [groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \(http :\/\/Go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>Pour distribuer des certificats aux ordinateurs clients à l’aide de stratégie de groupe  
  
1.  Sur un contrôleur de domaine dans la forêt de l’organisation partenaire de compte, démarrez le\-du composant logiciel enfichable de **gestion de stratégie de groupe** dans.  
  
2.  Recherchez un objet stratégie de groupe existant \(objet de stratégie de groupe\) ou créez un nouvel objet de stratégie de groupe contenant les paramètres du certificat. Assurez-vous que l’objet de stratégie de groupe est associé au domaine, au site ou à l’unité d’organisation \(UO\) où se trouvent les comptes d’utilisateur et d’ordinateur appropriés.  
  
3.  Cliquez avec le bouton droit\-sur l’objet de stratégie de groupe, puis cliquez sur **modifier**.  
  
4.  Dans l’arborescence de la console, ouvrez Configuration de l' **ordinateur\\stratégies\\paramètres Windows\\paramètres de sécurité\\stratégies de clé publique**,\-cliquez avec le bouton droit sur **autorités de certification racines de confiance**, puis cliquez sur **Importer**.  
  
5.  Dans la page **Bienvenue** de l'Assistant Importation de certificat, cliquez sur **Suivant**.  
  
6.  Sur la page **fichier à importer** , tapez le chemin d’accès aux fichiers de certificat appropriés \(par exemple, \\\\FS1\\c $\\FS1. cer\), puis cliquez sur **suivant**.  
  
7.  Dans la page **magasin de certificats** , cliquez sur **Placer tous les certificats dans le magasin suivant**, puis cliquez sur **suivant**.  
  
8.  Dans la page **fin de l’Assistant importation de certificat** , vérifiez que les informations que vous avez fournies sont exactes, puis cliquez sur **Terminer**.  
  
9. Répétez les étapes 2 à 6 pour ajouter des certificats supplémentaires pour chaque serveur de Fédération de la batterie de serveurs.  
