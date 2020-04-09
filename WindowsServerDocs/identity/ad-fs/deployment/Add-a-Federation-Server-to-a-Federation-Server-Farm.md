---
ms.assetid: 6ecf8d85-cd61-4c87-add8-00a679a6e3ff
title: Ajouter un serveur de fédération à une batterie de serveurs de fédération
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ea826f8984937c5d32a830131f042a8519528268
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815052"
---
# <a name="add-a-federation-server-to-a-federation-server-farm"></a>Ajouter un serveur de fédération à une batterie de serveurs de fédération


Après avoir installé le service de rôle service FS (Federation Service) et configuré les certificats requis sur un ordinateur, vous êtes prêt à configurer l’ordinateur pour qu’il devienne un serveur de Fédération. Vous pouvez utiliser la procédure suivante pour joindre un ordinateur à une nouvelle batterie de serveurs de fédération.  
  
Pour joindre un ordinateur à une batterie, vous utilisez l'Assistant Configuration de serveur de fédération AD FS. Lorsque vous utilisez cet Assistant pour joindre un ordinateur à une batterie de serveurs existante, l’ordinateur est configuré avec une copie en lecture\-seule de la base de données de configuration AD FS et il doit recevoir des mises à jour d’un serveur de Fédération principal.  
  
> [!NOTE]  
> Pour le\-de signature de\-unique Web fédéré sur \(conception de\) SSO, vous devez disposer d’au moins un serveur de Fédération dans l’organisation partenaire de compte et au moins un serveur de Fédération dans l’organisation partenaire de ressource. Pour plus d'informations, voir [Où placer un serveur de fédération](https://technet.microsoft.com/library/dd807127.aspx).  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs** ou d'un groupe équivalent sur l'ordinateur local.  Passez en revue les détails sur l’utilisation des comptes et des appartenances aux groupes appropriés dans les [groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \(http :\/\/Go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-federation-server-to-a-federation-server-farm"></a>Pour ajouter un serveur de Fédération à une batterie de serveurs de Fédération  
  
1.  L'Assistant Configuration de serveur de fédération AD FS peut être démarré de deux manières différentes. Procédez selon l'une d'elles :  
  
    -   Une fois l’installation du service de rôle service FS (Federation Service) terminée, ouvrez le composant logiciel enfichable Gestion des AD FS\-dans et cliquez sur le lien de l' **Assistant Configuration du serveur de fédération AD FS** dans la page **vue d’ensemble** ou dans le volet **actions** .  
  
    -   À chaque fois que l’Assistant installation est terminé, ouvrez l’Explorateur Windows, accédez au dossier **C :\\windows\\ADFS** , puis double\-cliquez sur **FsConfigWizard. exe**.  
  
2.  Dans la page **Bienvenue**, vérifiez que la case **Ajouter un serveur de fédération à un service de fédération existant** est cochée, puis cliquez sur **Suivant**.  
  
3.  Si la base de données de AD FS que vous avez sélectionnée existe déjà, la page **base de données de Configuration AD FS détectée** apparaît. Le cas échéant, cliquez sur **Supprimer la base de données**, puis sur **Suivant**.  
  
    > [!CAUTION]  
    > Sélectionnez cette option uniquement lorsque vous êtes sûr que les données de cette base de données AD FS ne sont pas importantes ni utilisées dans une batterie de serveurs de fédération de production.  
  
4.  Dans la page **Spécifier le serveur de fédération principal et le compte de service**, sous **Nom du serveur de fédération principal**, tapez le nom de l'ordinateur du serveur de fédération principal dans la batterie, puis cliquez sur **Rechercher**. Dans la boîte de dialogue **Parcourir**, recherchez le compte de domaine utilisé comme compte de service par tous les serveurs de fédération de la batterie de serveurs de fédération existante, puis cliquez sur **OK**. Confirmez le mot de passe en le tapant une deuxième fois, puis cliquez sur **Suivant** :  
  
    > [!NOTE]  
    > Pour plus d’informations sur la spécification d’un compte de service pour une batterie de serveurs de Fédération, consultez [configurer manuellement un compte de service pour une batterie de serveurs de Fédération](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md). Chaque serveur de Fédération de la batterie de serveurs de Fédération doit spécifier le même compte de service pour que la batterie soit opérationnelle. Par exemple, si le compte de service créé était contoso\\ADFS2SVC, chaque ordinateur que vous configurez pour le rôle de serveur de Fédération et qui participera à la même batterie doit spécifier contoso\\ADFS2SVC à cette étape de l’Assistant Configuration du serveur de Fédération pour que la batterie soit opérationnelle.  
  
5.  Passez en revue les détails dans la page **Prêt à appliquer les paramètres**. Si les paramètres semblent corrects, cliquez sur **Suivant** pour commencer à configurer AD FS avec ces paramètres.  
  
6.  Dans la page **Résultats de la configuration**, passez en revue les résultats. Une fois toutes les étapes de configuration terminées, cliquez sur **Fermer** pour quitter l’Assistant.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : configuration d’un serveur de Fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  

