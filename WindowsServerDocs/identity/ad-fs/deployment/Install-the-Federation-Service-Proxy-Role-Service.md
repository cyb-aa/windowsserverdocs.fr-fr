---
ms.assetid: c50ecc6a-9504-4b4a-816f-e762dcf3a95e
title: Installer le service de rôle Proxy de service de fédération
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6e78c52f1928a3401c0532ab7c25616b012a1d8b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192103"
---
# <a name="install-the-federation-service-proxy-role-service"></a>Installer le service de rôle Proxy de service de fédération

Une fois que vous configurez un ordinateur avec les applications prérequises et les certificats, vous êtes prêt à installer le service de rôle Proxy du Service de fédération des Services de fédération Active Directory \(AD FS\). Vous pouvez utiliser la procédure suivante pour installer le service de rôle Proxy du Service de fédération. Lorsque vous installez le service de rôle Proxy du Service de fédération sur un ordinateur, celui-ci devient un serveur proxy de fédération.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-federation-service-proxy-role-service-using-the-server-manager"></a>Pour installer le service de rôle Proxy du Service de fédération à l’aide du Gestionnaire de serveur
  
1.  Sur le **Démarrer** , tapez**le Gestionnaire de serveur**, puis appuyez sur ENTRÉE.  
  
2.  Cliquez sur **gérer**, puis cliquez sur **Ajout de rôles et fonctionnalités** pour démarrer l’ajout de rôles et l’Assistant de fonctionnalités.  
  
3.  Dans la page **Avant de commencer** , cliquez sur **Suivant**.  
  
4.  Sur le **sélectionner type d’installation** , cliquez sur **rôle\-ou une fonctionnalité\-installation basée sur un**, puis cliquez sur **suivant**.  
  
5.  Sur le **server de sélectionner la destination** , cliquez sur **sélectionner un serveur du pool de serveurs**, vérifiez que l’ordinateur cible est mis en surbrillance, puis cliquez sur **suivant**.  
  
6.  Sur le **sélectionner des rôles de serveur** , cliquez sur **accès à distance**, puis cliquez sur Suivant.  
  
    > [!NOTE]  
    > Si vous êtes invité à installer des fonctionnalités supplémentaires de .NET Framework ou Windows Process Activation Service, cliquez sur **ajouter des fonctionnalités** les installer.  
  
7. Sur le **sélectionnez services de rôle** , sélectionnez le **Proxy de Federation Service** case à cocher, puis cliquez sur **suivant**.  

8. Après avoir vérifié les informations de la page **Confirmer les sélections d'installation** , cochez la case **Redémarrer automatiquement le serveur de destination, si nécessaire** , puis cliquez sur **Installer**.  
  
13. Dans la page **Progression de l'installation** , vérifiez que tout a été correctement installé, puis cliquez sur **Fermer**.  

### <a name="to-install-the-federation-service-proxy-role-service-using-powershell"></a>Pour installer le service de rôle Proxy du Service de fédération à l’aide de PowerShell

1. Ouvrez Windows PowerShell (exécuter en tant qu’administrateur)

2. Tapez la commande suivante et la presse **entrée**:

        Install-WindowsFeature Web-Application-Proxy -IncludeManagementTools



  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Liste de vérification : configuration d’un serveur de fédération proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

