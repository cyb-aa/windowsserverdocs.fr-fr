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
ms.openlocfilehash: b9498451e8f6d7701e9ed4b3ac7d61f19d2dcdb4
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191894"
---
# <a name="verify-that-a-federation-server-is-operational"></a>Vérifier qu’un serveur de fédération est opérationnel


Vous pouvez utiliser les procédures suivantes pour vérifier qu’un serveur de fédération est opérationnel ; à savoir, qu’un client du même réseau peut atteindre un nouveau serveur de fédération.  
  
Vous devez au minimum appartenir au groupe **Utilisateurs**, **Opérateurs de sauvegarde**, **Utilisateurs avec pouvoir**, **Administrateurs** ou à un groupe équivalent sur l’ordinateur local pour pouvoir suivre cette procédure.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>Procédure 1 : Pour vérifier qu’un serveur de fédération est opérationnel  
  
1.  Pour vérifier que Internet Information Services \(IIS\) est correctement configuré sur le serveur de fédération, ouvrez une session sur un ordinateur client qui se trouve dans la même forêt que le serveur de fédération.  
  
2.  Ouvrez une fenêtre de navigateur, dans le type de barre d’adresse la fédération DNS du serveur nom d’hôte, puis s’y ajouter /adfs/fs/federationserverservice.asmx pour le nouveau serveur de fédération, par exemple :  
  
    **https://fs1.fabrikam.com/adfs/fs/federationserverservice.asmx**  
  
3.  Appuyez sur Entrée, puis exécutez la procédure suivante sur l’ordinateur du serveur de fédération. Si le message **Le certificat de sécurité de ce site Web présente un problème** apparaît, cliquez sur **Poursuivre sur ce site Web**.  
  
    La sortie attendue est l’affichage de code XML avec le document de description du service. Si cette page apparaît, IIS sur le serveur de fédération est opérationnel et le traitement des pages s’effectue avec succès.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>Procédure 2 : Pour vérifier qu’un serveur de fédération est opérationnel  
  
1.  Ouvrez une session en tant qu’administrateur le nouveau serveur de fédération.  
  
2.  Sur le **Démarrer** , tapez **Observateur d’événements**, puis appuyez sur ENTRÉE.  
  
3.  Dans le volet de détails, double-cliquez\-cliquez sur **journaux des Applications et Services**, double\-cliquez sur **événements AD FS**, puis cliquez sur **administrateur**.  
  
4.  Dans le **ID d’événement** colonne, recherchez l’évènement ID 100. Si le serveur de fédération est correctement configuré, vous voyez un nouvel événement, dans le journal des applications de l’Observateur d’événements, avec l’ID d’événement 100. Cet événement vérifie que le serveur de fédération a été en mesure de communiquer correctement avec le Service de fédération.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  

