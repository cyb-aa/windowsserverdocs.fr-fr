---
ms.assetid: ad61c586-ba8a-4534-8824-b45994d60c6b
title: Vérifier qu’un serveur de fédération est opérationnel
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: bdaa43fcb501405529415de950ecdf40a91ed088
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868048"
---
# <a name="verify-that-a-federation-server-is-operational"></a>Vérifier qu’un serveur de fédération est opérationnel


Vous pouvez utiliser les procédures suivantes pour vérifier qu’un serveur de fédération est opérationnel ; à savoir, qu’un client du même réseau peut atteindre un nouveau serveur de fédération.  
  
Vous devez au minimum appartenir au groupe **Utilisateurs**, **Opérateurs de sauvegarde**, **Utilisateurs avec pouvoir**, **Administrateurs** ou à un groupe équivalent sur l’ordinateur local pour pouvoir suivre cette procédure.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>Procédure 1 : Pour vérifier qu’un serveur de fédération est opérationnel  
  
1.  Pour vérifier que Internet Information Services \(IIS\) est configuré correctement sur le serveur de Fédération, connectez-vous à un ordinateur client situé dans la même forêt que le serveur de Fédération.  
  
2.  Ouvrez une fenêtre de navigateur, dans la barre d’adresse, tapez le nom d’hôte DNS du serveur de Fédération, puis ajoutez/adfs/fs/federationserverservice.asmx à ce dernier pour le nouveau serveur de Fédération, par exemple :  
  
    **https://fs1.fabrikam.com/adfs/fs/federationserverservice.asmx**  
  
3.  Appuyez sur Entrée, puis exécutez la procédure suivante sur l’ordinateur du serveur de fédération. Si vous voyez le message **il y a un problème avec le certificat de sécurité de ce site Web**, cliquez sur **Continuer sur ce site Web**.  
  
    La sortie attendue est l’affichage de code XML avec le document de description du service. Si cette page apparaît, IIS sur le serveur de fédération est opérationnel et le traitement des pages s’effectue avec succès.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>Procédure 2 : Pour vérifier qu’un serveur de fédération est opérationnel  
  
1.  Connectez-vous au nouveau serveur de Fédération en tant qu’administrateur.  
  
2.  Dans l’écran d' **Accueil** , tapez **Observateur d’événements**, puis appuyez sur entrée.  
  
3.  Dans le volet d’informations,\-double-cliquez sur **journaux des applications et des services**, double\--cliquez sur **AD FS Eventing**, puis cliquez sur **admin**.  
  
4.  Dans la colonne **ID d’événement** , recherchez l’ID d’événement 100. Si le serveur de Fédération est correctement configuré, vous voyez un nouvel événement, dans le journal des applications de observateur d’événements, avec l’ID d’événement 100. Cet événement vérifie que le serveur de Fédération a pu communiquer avec le service FS (Federation Service).  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  

