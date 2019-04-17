---
ms.assetid: 6ecf8d85-cd61-4c87-add8-00a679a6e3ff
title: "Ajouter un serveur de fédération à une batterie de serveurs de fédération"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d67f4c252ad25a05f11b88771f12fd01d13137d4
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-federation-server-to-a-federation-server-farm"></a>Ajouter un serveur de fédération à une batterie de serveurs de fédération

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Une fois que vous installez le service de rôle Service de fédération et configurez les certificats requis sur un ordinateur, vous êtes prêt à configurer l’ordinateur pour qu’il devienne un serveur de fédération. Vous pouvez utiliser la procédure suivante pour joindre un ordinateur à une batterie de serveurs de fédération.  
  
Vous joignez un ordinateur à une batterie avec l’Assistant Configuration du serveur de fédération ADFS. Lorsque vous utilisez cet Assistant pour joindre un ordinateur à une batterie existante, l’ordinateur est configuré avec une copie en lecture seule de la base de données de configuration ADFS et elle doit recevoir les mises à jour à partir d’un serveur de fédération principal.  
  
> [!NOTE]  
> Pour la conception \(SSO\) Single\-Sign\-On de Web fédéré, vous devez disposer d’au moins un serveur de fédération dans l’organisation partenaire de compte et au moins un serveur de fédération dans l’organisation partenaire de ressource. Pour plus d’informations, voir [où placer un serveur de fédération](https://technet.microsoft.com/library/dd807127.aspx).  
  
L’appartenance au groupe **administrateurs**, ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-add-a-federation-server-to-a-federation-server-farm"></a>Pour ajouter un serveur de fédération à une batterie de serveurs de fédération  
  
1.  Il existe deux façons de démarrer l’Assistant Configuration du serveur de fédération ADFS. Pour démarrer l’Assistant, effectuez l’une des opérations suivantes:  
  
    -   Après l’installation du service de rôle Service de fédération est terminée, ouvrez la gestion ADFS de composants et cliquez sur le **Assistant Configuration du serveur de fédération ADFS** lier sur le **vue d’ensemble** page ou dans le **Actions** volet.  
  
    -   À tout moment une fois l’Assistant Installation terminé, ouvrez l’Explorateur Windows, accédez à la **C:\\Windows\\ADFS** dossier et double-cliquant sur **FsConfigWizard.exe**.  
  
2.  Sur le **Bienvenue** page, vérifiez que **ajouter un serveur de fédération à un Service de fédération** est sélectionné, puis cliquez sur **suivant**.  
  
3.  Si la base de données ADFS que vous avez sélectionné déjà existe, le **existante ADFS Configuration de base de données détectée** page s’affiche. Si tel est le cas, cliquez sur **base de données**, puis cliquez sur **suivant**.  
  
    > [!CAUTION]  
    > Sélectionnez cette option uniquement lorsque vous êtes sûr que les données de cette base de données ADFS ne sont pas importantes ou qu’il n’est pas utilisé dans une batterie de serveurs de fédération de production.  
  
4.  Sur le **spécifier le serveur de fédération principal et le compte de Service** sous **nom du serveur de fédération principal**, tapez le nom d’ordinateur du serveur de fédération principal dans la batterie de serveurs, puis cliquez sur **Parcourir**. Dans le **Parcourir** boîte de dialogue, recherchez le compte de domaine qui est utilisé comme compte de service par tous les autres serveurs de fédération dans la batterie de serveurs de fédération existante, puis cliquez sur **OK**. Tapez le mot de passe et confirmez-le, puis cliquez sur **suivant**:  
  
    > [!NOTE]  
    > Pour plus d’informations sur la spécification d’un compte de service pour une batterie de serveurs de fédération, consultez [configurer manuellement un compte de Service pour une batterie de serveurs de fédération](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md). Chaque serveur de fédération dans la batterie de serveurs de fédération doit spécifier le même compte de service pour la batterie soit opérationnelle. Par exemple, si le compte de service créé était contoso\\ADFS2SVC, chaque ordinateur que vous configurez pour le rôle de serveur de fédération et qui va participer à la même batterie doit spécifier contoso\\ADFS2SVC à cette étape dans l’Assistant de Configuration de serveur de fédération de la batterie soit opérationnelle.  
  
5.  Sur le **prêt à appliquer les paramètres** page, passez en revue les détails. Si les paramètres sont corrects, cliquez sur **suivant** pour commencer à configurer ADFS avec ces paramètres.  
  
6.  Sur le **résultats de la Configuration** page, passez en revue les résultats. Lorsque toutes les étapes de configuration sont terminées, cliquez sur **fermer** pour quitter l’Assistant.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification: Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  

