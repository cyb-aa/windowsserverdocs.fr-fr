---
ms.assetid: 5e334c4e-75a7-453c-83e8-5ab4243cc685
title: Créer le premier serveur de fédération dans une batterie de serveurs de fédération
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0fe5c3160f661357536ef3bd60762873063c8ed0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855472"
---
# <a name="create-the-first-federation-server-in-a-federation-server-farm"></a>Créer le premier serveur de fédération dans une batterie de serveurs de fédération

Après avoir installé le service de rôle service FS (Federation Service) et configuré les certificats requis sur un ordinateur, vous êtes prêt à configurer l’ordinateur pour qu’il devienne un serveur de Fédération. Vous pouvez utiliser la procédure suivante pour configurer l’ordinateur pour qu’il devienne le premier serveur de Fédération dans une nouvelle batterie de serveurs de Fédération à l’aide de l’Assistant Configuration du serveur de fédération AD FS.  
  
La création du premier serveur de fédération dans une batterie de serveurs entraîne la création d'un nouveau service de fédération, en définissant cet ordinateur en tant que serveur de fédération principal. Cela signifie que cet ordinateur sera configuré avec une copie en lecture\/en écriture de la base de données de configuration AD FS. Tous les autres serveurs de Fédération de cette batterie doivent répliquer toutes les modifications apportées sur le serveur de Fédération principal à leur lecture\-uniquement les copies de la base de données de configuration AD FS qu’elles stockent localement. Pour plus d'informations sur ce processus de réplication, voir [Rôle de la base de données de configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
> [!NOTE]  
> Pour le\-de signature de\-unique Web fédéré sur \(conception de\) SSO, vous devez disposer d’au moins un serveur de Fédération dans l’organisation partenaire de compte et au moins un serveur de Fédération dans l’organisation partenaire de ressource. Pour plus d'informations, voir [Où placer un serveur de fédération](https://technet.microsoft.com/library/dd807127.aspx).  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe Admins du domaine, ou avoir un compte de domaine délégué avec un accès en écriture au conteneur Program Data dans Active Directory.  
  
### <a name="to-create-the-first-federation-server-in-a-federation-server-farm"></a>Pour créer le premier serveur de fédération dans une batterie de serveurs de fédération  
  
1.  L'Assistant Configuration de serveur de fédération AD FS peut être démarré de deux manières différentes. Procédez selon l'une d'elles :  
  
    -   Une fois l’installation du service de rôle service FS (Federation Service) terminée, ouvrez le composant logiciel enfichable Gestion des AD FS\-dans et cliquez sur le lien de l' **Assistant Configuration du serveur de fédération AD FS** dans la page **vue d’ensemble** ou dans le volet **actions** .  
  
    -   À tout moment une fois l’exécution de l’Assistant installation terminée, ouvrez l’Explorateur Windows, accédez au dossier **C :\\windows\\ADFS** , puis double\-cliquez sur **FsConfigWizard. exe**.  
  
2.  Dans la page **Bienvenue**, vérifiez que la case **Créer un service de fédération** est cochée, puis cliquez sur **Suivant**.  
  
3.  Dans la page **Sélectionner le Stand\-seul ou déployer une batterie** de serveurs, cliquez sur **nouvelle batterie de serveurs de Fédération**, puis cliquez sur **suivant**.  
  
4.  Dans la page **Spécifier le nom du service de fédération**, vérifiez que le **Certificat SSL** affiché est correct. Si ce n'est pas le cas, sélectionnez le certificat approprié dans la liste **Certificat SSL**.  
  
    Ce certificat est généré à partir des paramètres de\) protocole SSL \(SSL pour le site Web par défaut. Si un seul certificat est configuré pour le site Web par défaut, il est présenté et automatiquement sélectionné pour être utilisé. Si plusieurs certificats SSL sont configurés pour le site Web par défaut, tous sont répertoriés et vous devez en sélectionner un dans la liste. Si aucun paramètre SSL n'est configuré pour le site Web par défaut, la liste est générée à partir des certificats disponibles dans le magasin de certificats personnel de l'ordinateur local.  
  
    > [!NOTE]  
    > L’Assistant ne vous permet pas de remplacer le certificat si un certificat SSL est configuré pour IIS. Cela garantit que toute configuration IIS antérieure des certificats SSL est préservée. Pour contourner cette restriction, vous pouvez supprimer le certificat ou le reconfigurer manuellement avec la Console de gestion IIS.  
  
5.  Si la base de données AD FS sélectionnée existe déjà, la page **Base de données de configuration AD FS existante détectée** apparaît. Le cas échéant, cliquez sur **Supprimer la base de données**, puis sur **Suivant**.  
  
    > [!CAUTION]  
    > Sélectionnez cette option uniquement lorsque vous êtes sûr que les données de cette base de données AD FS ne sont pas importantes ni utilisées dans une batterie de serveurs de fédération de production.  
  
6.  Dans la page **Spécifier un compte de service**, cliquez sur **Parcourir**. Dans la boîte de dialogue **Parcourir**, recherchez le compte de domaine qui sera utilisé comme compte de service dans la nouvelle batterie de serveurs de fédération, puis cliquez sur **OK**. Entrez le mot de passe de ce compte, confirmez-le, puis cliquez sur **Suivant**.  
  
    > [!NOTE]  
    > Pour plus d’informations sur la spécification d’un compte de service pour une batterie de serveurs de Fédération, consultez [configurer manuellement un compte de service pour une batterie](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md) de serveurs de Fédération. Chaque serveur de Fédération de la batterie de serveurs de Fédération doit spécifier le même compte de service pour que la batterie soit opérationnelle. Par exemple, si le compte de service créé était contoso\\ADFS2SVC, chaque ordinateur que vous configurez pour le rôle de serveur de Fédération et qui participera à la même batterie doit spécifier contoso\\ADFS2SVC à cette étape de l’Assistant Configuration du serveur de Fédération pour que la batterie soit opérationnelle.  
  
7.  Passez en revue les détails dans la page **Prêt à appliquer les paramètres**. Si les paramètres semblent corrects, cliquez sur **Suivant** pour commencer à configurer AD FS avec ces paramètres.  
  
8.  Dans la page **Résultats de la configuration**, passez en revue les résultats. Une fois toutes les étapes de configuration terminées, cliquez sur **Fermer** pour quitter l’Assistant.  
  
    > [!IMPORTANT]  
    > En vue de sécuriser le déploiement, la résolution des artefacts et la détection des réponses sont désactivées lorsque vous utilisez l'Assistant Configuration du serveur de fédération AD FS pour configurer une batterie de serveurs de fédération. Cet Assistant configure automatiquement la base de données interne Windows pour stocker les données de configuration du service. Toutefois, vous pouvez annuler cette modification par erreur en activant le point de terminaison de résolution d’artefact à l’aide du nœud **points de terminaison** dans le\-du composant logiciel enfichable de gestion AD FS dans ou l’applet de commande Enable\-ADFSEndpoint dans Windows PowerShell. Veillez à ne pas reconfigurer les paramètres par défaut de sorte que ce point de terminaison reste désactivé lorsque vous utilisez une batterie de serveurs de fédération en association avec la base de données interne Windows.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : configuration d’un serveur de Fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  

