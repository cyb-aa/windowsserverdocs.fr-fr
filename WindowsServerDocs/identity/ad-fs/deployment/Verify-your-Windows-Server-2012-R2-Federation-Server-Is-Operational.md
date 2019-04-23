---
ms.assetid: 1115d276-00f6-4c23-9278-eedcc31295d8
title: Vérifiez que votre serveur de fédération Windows Server 2012 R2 est opérationnel
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2df8a00a953196d7ca19ea0d164abbbf6eefd829
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840740"
---
# <a name="verify-your-windows-server-2012-r2-federation-server-is-operational"></a>Vérifiez que votre serveur de fédération Windows Server 2012 R2 est opérationnel

>S'applique à : Windows Server 2016, Windows Server 2012 R2

Vous pouvez utiliser les procédures suivantes pour vérifier qu’un serveur de fédération est opérationnel ; à savoir, qu’un client du même réseau peut atteindre un nouveau serveur de fédération.  
  
Vous devez au minimum appartenir au groupe **Utilisateurs**, **Opérateurs de sauvegarde**, **Utilisateurs avec pouvoir**, **Administrateurs** ou à un groupe équivalent sur l’ordinateur local pour pouvoir suivre cette procédure.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>Procédure 1 : Pour vérifier qu’un serveur de fédération est opérationnel  
  
1.  Pour vérifier que Internet Information Services \(IIS\) est correctement configuré sur le serveur de fédération, ouvrez une session sur un ordinateur client qui se trouve dans la même forêt que le serveur de fédération.  
  
2.  Ouvrez une fenêtre de navigateur, dans la barre d’adresses, tapez nom d’hôte DNS du serveur de fédération, puis ajoutez \/adfs\/fs\/federationserverservice.asmx lui pour le nouveau serveur de fédération, par exemple :  
  
    **https:\/\/fs1.fabrikam.com\/adfs\/fs\/federationserverservice.asmx**  
  
3.  Appuyez sur Entrée, puis exécutez la procédure suivante sur l’ordinateur du serveur de fédération. Si le message **Le certificat de sécurité de ce site Web présente un problème** apparaît, cliquez sur **Poursuivre sur ce site Web**.  
  
    La sortie attendue est l’affichage de code XML avec le document de description du service. Si cette page apparaît, IIS sur le serveur de fédération est opérationnel et le traitement des pages s’effectue avec succès.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>Procédure 2 : Pour vérifier qu’un serveur de fédération est opérationnel  
  
1.  Ouvrez une session en tant qu’administrateur le nouveau serveur de fédération.  
  
2.  Sur le **Démarrer** , tapez**Observateur d’événements**, puis appuyez sur ENTRÉE.  
  
3.  Dans le volet de détails, double-cliquez\-cliquez sur **journaux des Applications et Services**, double\-cliquez sur **événements AD FS**, puis cliquez sur **administrateur**.  
  
4.  Dans le **ID d’événement** colonne, recherchez l’évènement ID 100. Si le serveur de fédération est correctement configuré, vous voyez un nouvel événement, dans le journal des applications de l’Observateur d’événements, avec l’ID d’événement 100. Cet événement vérifie que le serveur de fédération a été en mesure de communiquer correctement avec le Service de fédération.  
  
## <a name="see-also"></a>Voir aussi 

[Déploiement d’AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guide de déploiement de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Déploiement d’une batterie de serveurs de fédération](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
   
  

