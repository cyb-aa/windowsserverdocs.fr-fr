---
ms.assetid: ad61c586-ba8a-4534-8824-b45994d60c6b
title: "Vérifier qu’un serveur de fédération est opérationnel"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 2034b4c35061879a64004486395d0887c59087b2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="verify-that-a-federation-server-is-operational"></a>Vérifier qu’un serveur de fédération est opérationnel

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Vous pouvez utiliser les procédures suivantes pour vérifier qu’un serveur de fédération est opérationnel; Autrement dit, qu’un client sur le même réseau peut accéder à un serveur de fédération.  
  
L’appartenance au groupe **utilisateurs**, **opérateurs de sauvegarde**, **utilisateurs avec pouvoir**, **administrateurs** ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>La procédure 1: Pour vérifier qu’un serveur de fédération est opérationnel  
  
1.  Pour vérifier que les services Internet (IIS) \(IIS\) est correctement configuré sur le serveur de fédération, ouvrez une session sur un ordinateur client qui se trouve dans la même forêt que le serveur de fédération.  
  
2.  Ouvrez une fenêtre de navigateur, dans le type de barre d’adresse de fédération DNS du serveur nom d’hôte, puis ajouter /adfs/fs/federationserverservice.asmx pour le nouveau serveur de fédération, par exemple:  
  
    **https://FS1.fabrikam.com/adfs/fs/federationserverservice.asmx**  
  
3.  Appuyez sur entrée, puis exécutez la procédure suivante sur le serveur de fédération. Si vous voyez le message **un problème avec le certificat de sécurité de ce site Web**, cliquez sur **poursuivre sur ce site Web**.  
  
    La sortie attendue est un affichage de code XML avec le document de description du service. Si cette page apparaît, IIS sur le serveur de fédération est opérationnels et le traitement des pages avec succès.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>Procédure 2: Pour vérifier qu’un serveur de fédération est opérationnel  
  
1.  Ouvrez une session sur le nouveau serveur de fédération en tant qu’administrateur.  
  
2.  Sur le **Démarrer** , tapez **l’Observateur d’événements**, puis appuyez sur ENTRÉE.  
  
3.  Dans le volet détails, double-cliquant sur **journaux des Applications et Services**, double-cliquant sur **AD FS Eventing**, puis cliquez sur **Admin**.  
  
4.  Dans le **ID d’événement** colonne, recherchez l’événement ID 100. Si le serveur de fédération est correctement configuré, vous voyez un nouvel événement — dans le journal des applications de l’Observateur d’événements, avec l’événement ID 100. Cet événement vérifie que le serveur de fédération a été en mesure de communiquer correctement avec le Service de fédération.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification: Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  

