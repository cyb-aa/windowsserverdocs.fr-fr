---
ms.assetid: c50ecc6a-9504-4b4a-816f-e762dcf3a95e
title: Installer le service de rôle Proxy de service de fédération
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8d2ff177c821baf31bb5453b7c50e3eadca2aab7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855382"
---
# <a name="install-the-federation-service-proxy-role-service"></a>Installer le service de rôle Proxy de service de fédération

Après avoir configuré un ordinateur avec les applications et certificats requis, vous êtes prêt à installer le service de rôle proxy FSP (Federation Service Proxy) de Services ADFS \(AD FS\). Vous pouvez utiliser la procédure suivante pour installer le service de rôle proxy FSP (Federation Service Proxy). Lorsque vous installez le service de rôle proxy FSP (Federation Service Proxy) sur un ordinateur, cet ordinateur devient un serveur proxy de Fédération.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs** ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-federation-service-proxy-role-service-using-the-server-manager"></a>Pour installer le service de rôle proxy FSP (Federation Service Proxy) à l’aide de la Gestionnaire de serveur
  
1.  Dans l’écran d' **Accueil** , tapez**Gestionnaire de serveur**, puis appuyez sur entrée.  
  
2.  Cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités** pour démarrer l'Assistant Ajout de rôles et de fonctionnalités.  
  
3.  Dans la page **Avant de commencer**, cliquez sur **Suivant**.  
  
4.  Dans la page **Sélectionner le type d’installation** , cliquez sur **rôle\-en fonction ou sur fonctionnalité\-installation**, puis cliquez sur **suivant**.  
  
5.  Dans la page **Sélectionner le serveur de destination**, cliquez sur **Sélectionner un serveur du pool de serveurs**, vérifiez que l'ordinateur cible est mis en surbrillance, puis cliquez sur **Suivant**.  
  
6.  Dans la page **Sélectionner des rôles de serveurs** , cliquez sur **accès à distance**, puis sur suivant.  
  
    > [!NOTE]  
    > Si vous êtes invité à installer d'autres fonctionnalités .NET Framework ou Service d'activation des processus Windows, cliquez sur **Ajouter des fonctionnalités** pour les installer.  
  
7. Dans la page **Sélectionner des services de rôle**, cochez la case **Proxy de service FS**, puis cliquez sur **Suivant**.  

8. Après avoir vérifié les informations de la page **Confirmer les sélections d'installation**, cochez la case **Redémarrer le serveur de destination automatiquement si nécessaire**, puis cliquez sur **Installer**.  
  
13. Dans la page **Progression de l'installation**, vérifiez que tout a été correctement installé, puis cliquez sur **Fermer**.  

### <a name="to-install-the-federation-service-proxy-role-service-using-powershell"></a>Pour installer le service de rôle proxy FSP (Federation Service Proxy) à l’aide de PowerShell

1. Ouvrir Windows PowerShell (exécuter en tant qu’administrateur)

2. Tapez la commande suivante et appuyez sur **entrée**:

        Install-WindowsFeature Web-Application-Proxy -IncludeManagementTools



  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : configuration d’un serveur de Fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Liste de vérification : configuration d’un serveur proxy de Fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

