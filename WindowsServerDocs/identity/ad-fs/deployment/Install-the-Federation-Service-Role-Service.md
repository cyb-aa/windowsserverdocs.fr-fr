---
ms.assetid: e33673ff-ea1c-4476-a549-3bf5899a47dd
title: Installer le service de rôle de service de fédération
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 4952fad89502f9d81faab86cfd2e0e056543defd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855372"
---
# <a name="install-the-federation-service-role-service"></a>Installer le service de rôle de service de fédération

Maintenant que vous avez correctement configuré un ordinateur avec les applications et certificats requis, vous êtes prêt à installer le service de rôle service FS (Federation Service) de Services ADFS \(AD FS\). Lorsque vous installez le service FS (Federation Service) sur un ordinateur, cet ordinateur devient un serveur de Fédération.  
  
> [!NOTE]  
> Pour le\-de signature de\-unique Web fédéré sur \(conception de\) SSO, vous devez disposer d’au moins un serveur de Fédération dans l’organisation partenaire de compte et au moins un serveur de Fédération dans l’organisation partenaire de ressource. Pour plus d'informations, voir [Où placer un serveur de fédération](https://technet.microsoft.com/library/dd807127.aspx).  
  
Vous pouvez utiliser la procédure suivante pour installer le service de rôle service FS (Federation Service) de AD FS sur un ordinateur qui deviendra le premier serveur de Fédération ou sur un ordinateur qui deviendra un serveur de Fédération pour une batterie de serveurs de fédération existante.  
  
## <a name="prerequisites"></a>Composants requis  
Vérifiez qu’un certificat SSL avec la clé privée a déjà été installé ou importé dans le magasin de certificats local \(magasin personnel\) avant de commencer cette procédure. Si vous comptez utiliser un jeton\-certificat de signature émis par une autorité de certification \(\)de l’autorité de certification, vérifiez qu’un jeton\-certificat de signature avec la clé privée a déjà été installé ou importé dans le magasin de certificats local \(magasin personnel\) avant de commencer cette procédure. En guise d’alternative, vous pouvez créer un certificat de signature d'\-de jetons auto\-signé à l’aide de l’Assistant Ajout de rôles, comme décrit dans cette procédure. Pour plus d’informations sur les certificats de signature\-de jeton, consultez [Certificate Requirements for Federation Servers](https://technet.microsoft.com/library/dd807040.aspx).  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs** ou d'un groupe équivalent sur l'ordinateur local.  Passez en revue les détails sur l’utilisation des comptes et des appartenances aux groupes appropriés dans les [groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \(http :\/\/Go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
#### <a name="to-install-the-federation-service-role-service"></a>Pour installer le service de rôle de service de fédération  
  
1.  Dans l’écran d' **Accueil** , tapez**Gestionnaire de serveur**, puis appuyez sur entrée.  
  
2.  Cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités** pour démarrer l'Assistant Ajout de rôles et de fonctionnalités.  
  
3.  Dans la page **Avant de commencer**, cliquez sur **Suivant**.  
  
4.  Dans la page **Sélectionner le type d’installation** , cliquez sur **rôle\-en fonction ou sur fonctionnalité\-installation**, puis cliquez sur **suivant**.  
  
5.  Dans la page **Sélectionner le serveur de destination**, cliquez sur **Sélectionner un serveur du pool de serveurs**, vérifiez que l'ordinateur cible est mis en surbrillance, puis cliquez sur **Suivant**.  
  
6.  Dans la page **Sélectionner des rôles de serveur**, cliquez sur **Services AD FS (Active Directory Federation Services)** , puis sur Suivant.  
  
    > [!NOTE]  
    > Si vous êtes invité à installer d'autres fonctionnalités .NET Framework ou Service d'activation des processus Windows, cliquez sur **Ajouter des fonctionnalités** pour les installer.  
  
7.  Dans la page **Sélectionner des fonctionnalités**, vérifiez que les fonctionnalités sont définies, puis cliquez sur **Suivant**.  
  
8.  Sur la page **Active Directory service FS (Federation Service) \(AD FS\)** , cliquez sur **suivant**.  
  
9. Dans la page **Sélectionner des services de rôle**, cochez la case **Service FS**, puis cliquez sur **Suivant**.  
  
10. Dans la page **rôle de serveur Web \(IIS\)** , cliquez sur **suivant**.  
  
11. Dans la page **Sélectionner des services de rôle**, cliquez sur **Suivant**.  
  
12. Après avoir vérifié les informations de la page **Confirmer les sélections d'installation**, cochez la case **Redémarrer le serveur de destination automatiquement si nécessaire**, puis cliquez sur **Installer**.  
  
13. Dans la page **Progression de l'installation**, vérifiez que tout a été correctement installé, puis cliquez sur **Fermer**.  
  

