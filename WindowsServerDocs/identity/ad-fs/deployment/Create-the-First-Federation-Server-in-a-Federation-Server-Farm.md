---
ms.assetid: 5e334c4e-75a7-453c-83e8-5ab4243cc685
title: "Créer le premier serveur de fédération dans une batterie de serveurs de fédération"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: af0aa61f0d16d4ca567b140c95d74445d09f1cf3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="create-the-first-federation-server-in-a-federation-server-farm"></a>Créer le premier serveur de fédération dans une batterie de serveurs de fédération

 >S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Une fois que vous installez le service de rôle Service de fédération et configurez les certificats requis sur un ordinateur, vous êtes prêt à configurer l’ordinateur pour qu’il devienne un serveur de fédération. Vous pouvez utiliser la procédure suivante pour configurer l’ordinateur de devenir le premier serveur de fédération dans une nouvelle batterie de serveurs de fédération à l’aide de l’Assistant Configuration du serveur de fédération ADFS.  
  
Le fait de créer le premier serveur de fédération dans une batterie de serveurs crée un nouveau Service de fédération et rend cet ordinateur serveur de fédération principal. Cela signifie que cet ordinateur va être configuré avec une copie en lecture/écriture de la base de données de configuration ADFS. Tous les autres serveurs de fédération de cette batterie doivent répliquer les modifications apportées sur le serveur de fédération principal leurs copies en lecture seule de la base de données de configuration ADFS qu’ils stockent localement. Pour plus d’informations sur ce processus de réplication, voir [le rôle de la base de données de Configuration ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
> [!NOTE]  
> Pour la conception \(SSO\) Single\-Sign\-On de Web fédéré, vous devez disposer d’au moins un serveur de fédération dans l’organisation partenaire de compte et au moins un serveur de fédération dans l’organisation partenaire de ressource. Pour plus d’informations, voir [où placer un serveur de fédération](https://technet.microsoft.com/library/dd807127.aspx).  
  
L’appartenance à Admins du domaine ou un compte de domaine délégué qui ont été accordé accès en écriture au conteneur Program Data dans ActiveDirectory, est la condition minimale requise pour effectuer cette procédure.  
  
### <a name="to-create-the-first-federation-server-in-a-federation-server-farm"></a>Pour créer le premier serveur de fédération dans une batterie de serveurs de fédération  
  
1.  Il existe deux façons de démarrer l’Assistant Configuration du serveur de fédération ADFS. Pour démarrer l’Assistant, effectuez l’une des opérations suivantes:  
  
    -   Après l’installation du service de rôle Service de fédération est terminée, ouvrez la gestion ADFS de composants et cliquez sur le **Assistant Configuration du serveur de fédération ADFS** lier sur le **vue d’ensemble** page ou dans le **Actions** volet.  
  
    -   Une fois l’Assistant installation est terminée, ouvrez l’Explorateur Windows, accédez à la **C:\\Windows\\ADFS** dossier, puis double-cliquant sur **FsConfigWizard.exe**.  
  
2.  Sur le **Bienvenue** page, vérifiez que **créer un nouveau Service de fédération** est sélectionné, puis cliquez sur **suivant**.  
  
3.  Sur le **sélectionnez autonome autonome ou le déploiement de la batterie de serveurs**, cliquez sur **nouvelle batterie de serveurs de fédération**, puis cliquez sur **suivant**.  
  
4.  Sur le **spécifier le nom de Service de fédération** page, vérifiez que le **certificat SSL** affiché est correct. Si ce n’est pas le bon certificat, sélectionnez le certificat approprié à partir de la **certificat SSL** liste.  
  
    Ce certificat est généré à partir des paramètres \(SSL\) (Secure Sockets Layer) pour le site Web par défaut. Si le site Web par défaut n'a qu’un seul certificat SSL configuré, ce certificat est présenté et automatiquement sélectionné pour être utilisé. Si plusieurs certificats SSL sont configurés pour le site Web par défaut, tous les certificats sont répertoriés ici et vous devez sélectionner parmi. S’il n’existe pas de paramètres SSL configurés pour le site Web par défaut, la liste est générée à partir des certificats qui sont disponibles dans le magasin de certificats personnel sur l’ordinateur local.  
  
    > [!NOTE]  
    > L’Assistant vous autorisera pas à remplacer le certificat, si un certificat SSL est configuré pour IIS. Cela garantit que les destiné configuration IIS antérieures pour les certificats SSL est conservée. Pour contourner cette restriction, vous pouvez supprimer le certificat ou reconfigurer manuellement avec la Console de gestion IIS.  
  
5.  Si la base de données ADFS que vous avez sélectionné déjà existe, le **existante ADFS Configuration de base de données détectée** page s’affiche. Si cette page s’affiche, cliquez sur **base de données**, puis cliquez sur **suivant**.  
  
    > [!CAUTION]  
    > Sélectionnez cette option uniquement lorsque vous êtes sûr que les données de cette base de données ADFS ne sont pas importantes ou qu’il n’est pas utilisé dans une batterie de serveurs de fédération de production.  
  
6.  Sur le **spécifier un compte de Service**, cliquez sur **Parcourir**. Dans le **Parcourir** boîte de dialogue, recherchez le compte de domaine qui sera utilisé comme compte de service dans cette nouvelle batterie de serveurs de fédération, puis cliquez sur **OK**. Tapez le mot de passe pour ce compte, confirmez-le, puis cliquez sur **suivant**.  
  
    > [!NOTE]  
    > Voir [configurer manuellement un compte de Service pour une batterie de serveurs de fédération](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md) pour plus d’informations sur la spécification d’un compte de service pour une batterie de serveurs de fédération. Chaque serveur de fédération dans la batterie de serveurs de fédération doit spécifier le même compte de service pour la batterie soit opérationnelle. Par exemple, si le compte de service créé était contoso\\ADFS2SVC, chaque ordinateur que vous configurez pour le rôle de serveur de fédération et qui va participer à la même batterie doit spécifier contoso\\ADFS2SVC à cette étape dans l’Assistant de Configuration de serveur de fédération de la batterie soit opérationnelle.  
  
7.  Sur le **prêt à appliquer les paramètres** page, passez en revue les détails. Si les paramètres sont corrects, cliquez sur **suivant** pour commencer à configurer ADFS avec ces paramètres.  
  
8.  Sur le **résultats de la Configuration** page, passez en revue les résultats. Lorsque toutes les étapes de configuration sont terminées, cliquez sur **fermer** pour quitter l’Assistant.  
  
    > [!IMPORTANT]  
    > À des fins de sécuriser le déploiement, la détection réponse et la résolution d’artefacts sont désactivées lorsque vous utilisez l’Assistant Configuration du serveur de fédération ADFS pour configurer une batterie de serveurs de fédération. Cet Assistant configure automatiquement la base de données interne Windows pour le stockage des données de configuration du service. Vous pouvez, toutefois, par inadvertance annuler cette modification en activant le point de terminaison résolution d’artefacts à l’aide du **les points de terminaison** nœud dans la gestion ADFS enfichable ou l’applet de commande Enable-ADFSEndpoint dans Windows PowerShell. Veillez à ne pas reconfigurer le paramètre par défaut afin que ce point de terminaison reste désactivé lorsque vous utilisez conjointement une batterie de serveurs de fédération et de la base de données interne Windows.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification: Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  

