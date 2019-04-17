---
ms.assetid: c50ecc6a-9504-4b4a-816f-e762dcf3a95e
title: "Installer le service de rôle Proxy du Service de fédération"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: aea1ef335604aa18f0b1a1c22ef13f6fee1601b3
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="install-the-federation-service-proxy-role-service"></a>Installer le service de rôle Proxy du Service de fédération

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Une fois que vous configurez un ordinateur avec les applications prérequises et les certificats, vous êtes prêt à installer le service de rôle Proxy du Service de fédération de \(ADFS\) ActiveDirectory Federation Services. Vous pouvez utiliser la procédure suivante pour installer le service de rôle Proxy du Service de fédération. Lorsque vous installez le service de rôle Proxy du Service de fédération sur un ordinateur, celui-ci devient un serveur proxy de fédération.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-federation-service-proxy-role-service"></a>Pour installer le service de rôle Proxy du Service de fédération  
  
1.  Sur le **Démarrer**, tapez**le Gestionnaire de serveur**, puis appuyez sur ENTRÉE.  
  
2.  Cliquez sur **gérer**, puis cliquez sur **Ajout de rôles et fonctionnalités** pour démarrer l’ajout de rôles et fonctionnalités Assistant.  
  
3.  Sur le **avant de commencer** , cliquez sur **suivant**.  
  
4.  Sur le **sélectionner le type d’installation**, cliquez sur **installation basée sur un rôle ou une fonctionnalité**, puis cliquez sur **suivant**.  
  
5.  Sur le **serveur de destination sélectionnez**, cliquez sur **sélectionner un serveur du pool de serveurs**, vérifiez que l’ordinateur cible est mis en surbrillance, puis cliquez sur **suivant**.  
  
6.  Sur le **sélectionner des rôles de serveur**, cliquez sur **ActiveDirectory Federation Services**, puis cliquez sur Suivant.  
  
    > [!NOTE]  
    > Si vous êtes invité à installer des fonctionnalités supplémentaires de .NETFramework ou le Service d’Activation des processus Windows, cliquez sur **ajouter des fonctionnalités** de les installer.  
  
7.  Sur le **sélectionner des fonctionnalités**, vérifiez que les fonctionnalités sont définies, puis cliquez sur **suivant**.  
  
8.  Sur le **ActiveDirectory Federation Services \(ADFS\)**, cliquez sur **suivant**.  
  
9. Sur le **sélectionner les services de rôle**, sélectionnez le **Proxy du Service de fédération** case à cocher, puis cliquez sur **suivant**.  
  
10. Sur le **rôle de serveur Web \(IIS\)**, cliquez sur **suivant**.  
  
11. Sur le **sélectionner les services de rôle**, cliquez sur **suivant**.  
  
12. Après avoir vérifié les informations sur la **confirmer les sélections d’installation** page, sélectionnez le **redémarrer automatiquement le serveur de destination si nécessaire** case à cocher, puis cliquez sur **installer**.  
  
13. Sur le **progression de l’Installation** page, vérifiez que tous les éléments installés correctement, puis cliquez sur **fermer**.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification: Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Liste de vérification: Configuration d’un serveur Proxy de fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

