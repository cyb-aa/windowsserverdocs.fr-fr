---
ms.assetid: e33673ff-ea1c-4476-a549-3bf5899a47dd
title: Installer le service de rôle de service de fédération
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 80a6cb2bc8e6f0fdb1a777a42f5d245f98ac3dee
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192086"
---
# <a name="install-the-federation-service-role-service"></a>Installer le service de rôle de service de fédération

Maintenant que vous avez correctement configuré un ordinateur avec les applications prérequises et les certificats, vous êtes prêt à installer le service de rôle Service de fédération des Services de fédération Active Directory \(AD FS\). Lorsque vous installez le Service de fédération sur un ordinateur, celui-ci devient un serveur de fédération.  
  
> [!NOTE]  
> Pour l’unique Web fédérée\-connexion\-sur \(SSO\) conception, vous devez disposer au moins un serveur de fédération dans l’organisation partenaire de compte et au moins un serveur de fédération dans l’organisation partenaire de ressource . Pour plus d'informations, voir [Où placer un serveur de fédération](https://technet.microsoft.com/library/dd807127.aspx).  
  
Vous pouvez utiliser la procédure suivante pour installer le service de rôle Service de fédération des services AD FS sur un ordinateur qui deviendra le premier serveur de fédération ou sur un ordinateur qui va devenir un serveur de fédération pour une batterie de serveurs de fédération existante.  
  
## <a name="prerequisites"></a>Prérequis  
Vérifiez qu’un certificat SSL avec la clé privée a déjà été installé ou importé dans le magasin de certificats local \(magasin personnel\) avant de commencer cette procédure. Si vous utilisez un jeton\-certificat émis par une autorité de certification de signature \(autorité de certification\), vérifiez qu’un jeton\-certificat de signature avec la clé privée a déjà été installé ou importé dans le magasin de certificats local \(magasin personnel\) avant de commencer cette procédure. Comme alternative, vous pouvez créer un libre-service\-signé, le jeton\-signature de certificat à l’aide de l’Assistant Ajout de rôles, comme décrit dans cette procédure. Pour plus d’informations sur le jeton\-certificats de signature, consultez [Certificate Requirements for Federation Servers](https://technet.microsoft.com/library/dd807040.aspx).  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’aide de comptes appropriés et les appartenances au groupe [locaux et les groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \(http :\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
#### <a name="to-install-the-federation-service-role-service"></a>Pour installer le service de rôle de service de fédération  
  
1.  Sur le **Démarrer** , tapez**le Gestionnaire de serveur**, puis appuyez sur ENTRÉE.  
  
2.  Cliquez sur **gérer**, puis cliquez sur **Ajout de rôles et fonctionnalités** pour démarrer l’ajout de rôles et l’Assistant de fonctionnalités.  
  
3.  Dans la page **Avant de commencer** , cliquez sur **Suivant**.  
  
4.  Sur le **sélectionner type d’installation** , cliquez sur **rôle\-ou une fonctionnalité\-installation basée sur un**, puis cliquez sur **suivant**.  
  
5.  Sur le **server de sélectionner la destination** , cliquez sur **sélectionner un serveur du pool de serveurs**, vérifiez que l’ordinateur cible est mis en surbrillance, puis cliquez sur **suivant**.  
  
6.  Sur le **sélectionner des rôles de serveur** , cliquez sur **Active Directory Federation Services**, puis cliquez sur Suivant.  
  
    > [!NOTE]  
    > Si vous êtes invité à installer des fonctionnalités supplémentaires de .NET Framework ou Windows Process Activation Service, cliquez sur **ajouter des fonctionnalités** les installer.  
  
7.  Sur le **sélectionner des fonctionnalités** , vérifiez que les fonctionnalités sont définies, puis cliquez sur **suivant**.  
  
8.  Sur le **Active Directory Federation Service \(AD FS\)**  , cliquez sur **suivant**.  
  
9. Sur le **sélectionnez services de rôle** , sélectionnez le **Service de fédération** case à cocher, puis cliquez sur **suivant**.  
  
10. Sur le **rôle de serveur Web \(IIS\)**  , cliquez sur **suivant**.  
  
11. Dans la page **Sélectionner des services de rôle**, cliquez sur **Suivant**.  
  
12. Après avoir vérifié les informations de la page **Confirmer les sélections d'installation** , cochez la case **Redémarrer automatiquement le serveur de destination, si nécessaire** , puis cliquez sur **Installer**.  
  
13. Dans la page **Progression de l'installation** , vérifiez que tout a été correctement installé, puis cliquez sur **Fermer**.  
  

