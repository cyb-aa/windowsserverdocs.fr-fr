---
ms.assetid: e33673ff-ea1c-4476-a549-3bf5899a47dd
title: "Installez le Service de rôle de Service de fédération"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c520cbe22739f2bde263e133c7feb681d824d251
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="install-the-federation-service-role-service"></a>Installez le Service de rôle de Service de fédération

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Maintenant que vous avez correctement configuré un ordinateur avec les applications prérequises et les certificats, vous êtes prêt à installer le service de rôle Service de fédération de \(ADFS\) ActiveDirectory Federation Services. Lorsque vous installez le Service de fédération sur un ordinateur, celui-ci devient un serveur de fédération.  
  
> [!NOTE]  
> Pour la conception \(SSO\) Single\-Sign\-On de Web fédéré, vous devez disposer d’au moins un serveur de fédération dans l’organisation partenaire de compte et au moins un serveur de fédération dans l’organisation partenaire de ressource. Pour plus d’informations, voir [où placer un serveur de fédération](https://technet.microsoft.com/library/dd807127.aspx).  
  
Vous pouvez utiliser la procédure suivante pour installer le service de rôle Service de fédération d’ADFS sur un ordinateur qui deviendra le premier serveur de fédération ou sur un ordinateur qui deviendra serveur de fédération pour une batterie de serveurs de fédération existante.  
  
## <a name="prerequisites"></a>Conditions préalables  
Vérifiez qu’un certificat SSL avec la clé privée a déjà été installé ou importé dans le \(Personal store\) magasin de certificats local avant de commencer cette procédure. Si vous utilisez un certificat de signature token\ émis par une autorité de certification \(CA\), vérifiez qu’un certificat de signature token\ avec la clé privée a déjà été installé ou importé dans le \(Personal store\) magasin de certificats local avant de commencer cette procédure. Comme alternative, vous pouvez créer un certificat signé intégrée, la signature de token\ à l’aide de l’Assistant Ajout de rôles, comme décrit dans cette procédure. Pour plus d’informations sur les certificats de signature de token\, voir [certificats requis pour les serveurs de fédération](https://technet.microsoft.com/library/dd807040.aspx).  
  
L’appartenance au groupe **administrateurs**, ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
#### <a name="to-install-the-federation-service-role-service"></a>Pour installer le service de rôle Service de fédération  
  
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
  
9. Sur le **sélectionner les services de rôle**, sélectionnez le **Service de fédération** case à cocher, puis cliquez sur **suivant**.  
  
10. Sur le **rôle de serveur Web \(IIS\)**, cliquez sur **suivant**.  
  
11. Sur le **sélectionner les services de rôle**, cliquez sur **suivant**.  
  
12. Après avoir vérifié les informations sur la **confirmer les sélections d’installation** page, sélectionnez le **redémarrer automatiquement le serveur de destination si nécessaire** case à cocher, puis cliquez sur **installer**.  
  
13. Sur le **progression de l’Installation** page, vérifiez que tous les éléments installés correctement, puis cliquez sur **fermer**.  
  

