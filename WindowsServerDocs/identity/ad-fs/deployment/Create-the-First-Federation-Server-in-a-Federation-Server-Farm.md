---
ms.assetid: 5e334c4e-75a7-453c-83e8-5ab4243cc685
title: Créer le premier serveur de fédération dans une batterie de serveurs de fédération
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: af0aa61f0d16d4ca567b140c95d74445d09f1cf3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879300"
---
# <a name="create-the-first-federation-server-in-a-federation-server-farm"></a>Créer le premier serveur de fédération dans une batterie de serveurs de fédération

 >S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Une fois que vous installez le service de rôle Service de fédération et configurez les certificats requis sur un ordinateur, vous êtes prêt à configurer l’ordinateur pour qu’il devienne un serveur de fédération. Vous pouvez utiliser la procédure suivante pour configurer l’ordinateur de devenir le premier serveur de fédération dans une nouvelle batterie de serveurs de fédération à l’aide de l’Assistant Configuration du serveur de fédération AD FS.  
  
La création du premier serveur de fédération dans une batterie de serveurs entraîne la création d'un nouveau service de fédération, en définissant cet ordinateur en tant que serveur de fédération principal. Cela signifie que cet ordinateur va être configuré avec une lecture\/écrire la copie de la base de données de configuration AD FS. Tous les autres serveurs de fédération dans cette batterie doivent répliquer les modifications apportées sur le serveur de fédération principal pour leur lecture\-uniquement des copies de la base de données de configuration AD FS qu’ils stockent localement. Pour plus d'informations sur ce processus de réplication, voir [Rôle de la base de données de configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
> [!NOTE]  
> Pour l’unique Web fédérée\-connexion\-sur \(SSO\) conception, vous devez disposer au moins un serveur de fédération dans l’organisation partenaire de compte et au moins un serveur de fédération dans l’organisation partenaire de ressource . Pour plus d'informations, voir [Où placer un serveur de fédération](https://technet.microsoft.com/library/dd807127.aspx).  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe Admins du domaine, ou avoir un compte de domaine délégué avec un accès en écriture au conteneur Program Data dans Active Directory.  
  
### <a name="to-create-the-first-federation-server-in-a-federation-server-farm"></a>Pour créer le premier serveur de fédération dans une batterie de serveurs de fédération  
  
1.  Il existe deux façons de démarrer l’Assistant Configuration du serveur de fédération AD FS. Pour démarrer l'Assistant, effectuez l'une des opérations suivantes :  
  
    -   Une fois l’installation de service de rôle Service de fédération est terminée, ouvrez le composant logiciel enfichable Gestion AD FS\-dans et cliquez sur le **Assistant Configuration du serveur de fédération AD FS** lien sur le **vue d’ensemble** page ou dans le **Actions** volet.  
  
    -   Une fois l’Assistant installation est terminée, ouvrez l’Explorateur Windows, accédez à la **C:\\Windows\\ADFS** dossier et puis double\-cliquez sur **FsConfigWizard.exe**.  
  
2.  Dans la page **Bienvenue**, vérifiez que l'option **Créer un service de fédération** est sélectionnée, puis cliquez sur **Suivant**.  
  
3.  Sur le **sélectionnez reposer\-Alone ou déploiement de batterie de serveurs** , cliquez sur **nouvelle batterie de serveurs de fédération**, puis cliquez sur **suivant**.  
  
4.  Dans la page **Spécifier le nom du service FS**, vérifiez que le **Certificat SSL** affiché est correct. Si ce n'est pas le cas, sélectionnez le certificat approprié dans la liste **Certificat SSL**.  
  
    Ce certificat est généré à partir de Secure Sockets Layer \(SSL\) paramètres du Site Web par défaut. Si un seul certificat SSL est configuré pour le site web par défaut, il est présenté et automatiquement sélectionné comme certificat à utiliser. Si plusieurs certificats SSL sont configurés pour le site web par défaut, tous les certificats disponibles vous sont présentés, et vous devez en sélectionner un. S'il n'y a pas de paramètres SSL configurés pour le site web par défaut, la liste est créée à partir des certificats qui sont disponibles dans le magasin de certificats personnel qui se trouve sur l'ordinateur local.  
  
    > [!NOTE]  
    > Si un certificat SSL est configuré pour les services SSL, l'Assistant ne vous autorisera pas à le remplacer. Cela garantit que toute configuration de services IIS antérieure pour les certificats SSL est conservée. Pour contourner cette restriction, vous pouvez supprimer le certificat ou le reconfigurer manuellement à l'aide de la console de gestion d'IIS.  
  
5.  Si la base de données AD FS que vous avez déjà sélectionné existe, le **AD FS Configuration Database détectée** page s’affiche. Si cette page s'affiche, cliquez sur **Supprimer la base de données**, puis cliquez sur **Suivant**.  
  
    > [!CAUTION]  
    > Sélectionnez cette option uniquement lorsque vous êtes sûr que les données dans cette base de données AD FS ne sont pas importantes ou qu’il n’est pas utilisé dans une batterie de serveurs de fédération de production.  
  
6.  Dans la page **Spécifier un compte de service**, cliquez sur **Parcourir**. Dans la boîte de dialogue **Parcourir**, recherchez le compte de domaine à utiliser en tant que compte de service dans cette nouvelle batterie de serveurs de fédération, puis cliquez sur **OK**. Tapez le mot de passe du compte, confirmez-le, puis cliquez sur **Suivant**.  
  
    > [!NOTE]  
    > Consultez [configurer manuellement un compte de Service pour une batterie de serveurs de fédération](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md) pour plus d’informations sur la spécification d’un compte de service pour une batterie de serveurs de fédération. Chaque serveur de fédération dans la batterie de serveurs de fédération doit spécifier le même compte de service pour la batterie soit opérationnelle. Par exemple, si le compte de service créé était contoso\\ADFS2SVC, chaque ordinateur que vous configurez pour le rôle de serveur de fédération et qui fera partie de la même batterie doit spécifier contoso\\ADFS2SVC à cette étape dans la Assistant serveur de fédération Configuration pour la batterie soit opérationnelle.  
  
7.  Dans la page **Prêt à appliquer les paramètres**, vérifiez les paramètres définis. Si les paramètres semblent corrects, cliquez sur **suivant** pour commencer à configurer AD FS avec ces paramètres.  
  
8.  Dans la page **Résultats de la configuration**, examinez les résultats. Lorsque toutes les étapes de configuration sont terminées, cliquez sur **fermer** pour quitter l’Assistant.  
  
    > [!IMPORTANT]  
    > Pour sécuriser le déploiement, la résolution d'artefacts et la détection des réponses sont désactivées lorsque vous utilisez l'Assistant Configuration du serveur de fédération AD FS pour configurer une batterie de serveurs de fédération. Cet Assistant configure automatiquement la base de données interne Windows pour le stockage des données de configuration du service. Vous pouvez, toutefois, par inadvertance annuler cette modification en activant le point de terminaison de résolution d’artefacts à l’aide du **points de terminaison** nœud dans le composant logiciel enfichable Gestion AD FS\-dans ou l’activer\-applet de commande ADFSEndpoint dans Windows PowerShell. Veillez à ne pas reconfigurer le paramètre par défaut afin que ce point de terminaison reste désactivé lorsque vous utilisez conjointement une batterie de serveurs de fédération et la base de données interne Windows.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  

