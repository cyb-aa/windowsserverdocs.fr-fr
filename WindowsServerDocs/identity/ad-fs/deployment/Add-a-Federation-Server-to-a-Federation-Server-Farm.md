---
ms.assetid: 6ecf8d85-cd61-4c87-add8-00a679a6e3ff
title: Ajouter un serveur de fédération à une batterie de serveurs de fédération
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 040caf6395b7c70313de900d522241f97699a999
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192501"
---
# <a name="add-a-federation-server-to-a-federation-server-farm"></a>Ajouter un serveur de fédération à une batterie de serveurs de fédération


Une fois que vous installez le service de rôle Service de fédération et configurez les certificats requis sur un ordinateur, vous êtes prêt à configurer l’ordinateur pour qu’il devienne un serveur de fédération. Vous pouvez utiliser la procédure suivante pour joindre un ordinateur à une nouvelle batterie de serveurs de fédération.  
  
Joindre un ordinateur à une batterie de serveurs avec l’Assistant Configuration du serveur de fédération AD FS. Lorsque vous utilisez cet Assistant pour joindre un ordinateur à une batterie de serveurs existante, l’ordinateur est configuré avec une lecture\-uniquement la copie de la base de données de configuration AD FS et il doit recevoir les mises à jour à partir d’un serveur de fédération principal.  
  
> [!NOTE]  
> Pour l’unique Web fédérée\-connexion\-sur \(SSO\) conception, vous devez disposer au moins un serveur de fédération dans l’organisation partenaire de compte et au moins un serveur de fédération dans l’organisation partenaire de ressource . Pour plus d'informations, voir [Où placer un serveur de fédération](https://technet.microsoft.com/library/dd807127.aspx).  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’aide de comptes appropriés et les appartenances au groupe [locaux et les groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \(http :\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-federation-server-to-a-federation-server-farm"></a>Pour ajouter un serveur de fédération à une batterie de serveurs de fédération  
  
1.  Il existe deux façons de démarrer l’Assistant Configuration du serveur de fédération AD FS. Pour démarrer l'Assistant, effectuez l'une des opérations suivantes :  
  
    -   Une fois l’installation de service de rôle Service de fédération est terminée, ouvrez le composant logiciel enfichable Gestion AD FS\-dans et cliquez sur le **Assistant Configuration du serveur de fédération AD FS** lien sur le **vue d’ensemble** page ou dans le **Actions** volet.  
  
    -   Une fois l’Assistant installation est terminée, ouvrez l’Explorateur Windows, accédez à la **C:\\Windows\\ADFS** dossier et double\-cliquez sur **FsConfigWizard.exe**.  
  
2.  Sur la page **Bienvenue**, vérifiez que la case **Ajouter un serveur de fédération à un service de fédération** est activée, puis cliquez sur **Suivant**.  
  
3.  Si la base de données AD FS que vous avez déjà sélectionné existe, le **AD FS Configuration Database détectée** page s’affiche. Si tel est le cas, cliquez sur **Supprimer la base de données**, puis cliquez sur **Suivant**.  
  
    > [!CAUTION]  
    > Sélectionnez cette option uniquement lorsque vous êtes sûr que les données dans cette base de données AD FS ne sont pas importantes ou qu’il n’est pas utilisé dans une batterie de serveurs de fédération de production.  
  
4.  Dans la page **Spécifiez le serveur de fédération principal et le compte de service**, sous **Nom du serveur de fédération principal**, tapez le nom d’ordinateur du serveur de fédération principal, puis cliquez sur**Parcourir**. Dans la boîte de dialogue **Parcourir**, recherchez le compte de domaine qui sera utilisé comme compte de service par tous les autres serveurs de fédération dans la nouvelle batterie de serveurs de fédération, puis cliquez sur **OK**. Tapez le mot de passe et confirmez-le, puis cliquez sur **suivant**:  
  
    > [!NOTE]  
    > Pour plus d’informations sur la spécification d’un compte de service pour une batterie de serveurs de fédération, consultez [configurer manuellement un compte de Service pour une batterie de serveurs de fédération](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md). Chaque serveur de fédération dans la batterie de serveurs de fédération doit spécifier le même compte de service pour la batterie soit opérationnelle. Par exemple, si le compte de service créé était contoso\\ADFS2SVC, chaque ordinateur que vous configurez pour le rôle de serveur de fédération et qui fera partie de la même batterie doit spécifier contoso\\ADFS2SVC à cette étape dans la Assistant serveur de fédération Configuration pour la batterie soit opérationnelle.  
  
5.  Dans la page **Prêt à appliquer les paramètres**, vérifiez les paramètres définis. Si les paramètres semblent corrects, cliquez sur **suivant** pour commencer à configurer AD FS avec ces paramètres.  
  
6.  Dans la page **Résultats de la configuration**, examinez les résultats. Lorsque toutes les étapes de configuration sont terminées, cliquez sur **fermer** pour quitter l’Assistant.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  

