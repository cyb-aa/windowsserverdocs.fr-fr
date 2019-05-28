---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: Créer un serveur de fédération autonome
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 4b70b0b048f66f9a8ba19cd7990dde57e0655ae4
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192231"
---
# <a name="create-a-stand-alone-federation-server"></a>Créer un serveur de fédération autonome

Une fois que vous installez le service de rôle Service de fédération et configurez les certificats requis sur un ordinateur, vous êtes prêt à configurer l’ordinateur pour qu’il devienne un serveur de fédération. Vous pouvez utiliser la procédure suivante pour configurer l’ordinateur de devenir un socle\-serveur de fédération autonome. L’opération de création d’un support\-serveur de fédération autonome crée également un Service de fédération. Vous créez un serveur de fédération avec l’Assistant Configuration du serveur de fédération AD FS.  
  
> [!NOTE]  
> Pour l’unique Web fédérée\-connexion\-sur \(SSO\) conception, vous devez disposer au moins un serveur de fédération dans l’organisation partenaire de compte et au moins un serveur de fédération dans l’organisation partenaire de ressource . Pour plus d'informations, voir [Où placer un serveur de fédération](https://technet.microsoft.com/library/dd807127.aspx).  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’aide de comptes appropriés et les appartenances au groupe [locaux et les groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \(http :\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-create-a-stand-alone-federation-server"></a>Pour créer un support\-serveur de fédération autonome  
  
1.  Il existe deux façons de démarrer l’Assistant Configuration du serveur de fédération AD FS. Pour démarrer l'Assistant, effectuez l'une des opérations suivantes :  
  
    -   Une fois l’installation de service de rôle Service de fédération est terminée, ouvrez le composant logiciel enfichable Gestion AD FS\-dans et cliquez sur le **Assistant Configuration du serveur de fédération AD FS** lien sur le **vue d’ensemble** page ou dans le **Actions** volet.  
  
    -   Une fois l’Assistant installation est terminée, ouvrez l’Explorateur Windows, accédez à la **C:\\Windows\\ADFS** dossier et puis double\-cliquez sur **FsConfigWizard.exe**.  
  
2.  Dans la page **Bienvenue**, vérifiez que l'option **Créer un service de fédération** est sélectionnée, puis cliquez sur **Suivant**.  
  
3.  Sur le **sélectionnez reposer\-Alone ou déploiement de batterie de serveurs** , cliquez sur **, mettez au point\-serveur de fédération autonome**, puis cliquez sur **suivant**.  
  
    > [!IMPORTANT]  
    > Lorsque vous sélectionnez le socle\-option de serveur de fédération autonome dans l’Assistant Configuration du serveur de fédération AD FS, le compte de service associé à ce Service de fédération est automatiquement affecté au compte de SERVICE de réseau. À l’aide de SERVICE réseau comme compte de service est uniquement recommandée dans les situations où vous évaluez AD FS dans un environnement de laboratoire de test. Si vous prévoyez d’utiliser le support\-option de serveur de fédération autonome pour déployer un serveur de fédération dans un environnement de production, il est important que vous modifiez ce compte de service à un compte de service plus approprié qui peut être dédié à la fourniture demandes de ce nouveau Service de fédération. Modification du compte de service à un compte autre que SERVICE réseau, se trouvent des vecteurs d’attaque qui pourraient rendre votre serveur de fédération vulnérable aux attaques malveillantes.  
  
4.  Dans la page **Spécifier le nom du service FS**, vérifiez que le **Certificat SSL** affiché est correct. Dans le cas contraire, sélectionnez le certificat approprié à partir de la **certificat SSL** liste.  
  
    Ce certificat est généré à partir de Secure Sockets Layer \(SSL\) paramètres du Site Web par défaut. Si un seul certificat SSL est configuré pour le site web par défaut, il est présenté et automatiquement sélectionné comme certificat à utiliser. Si plusieurs certificats SSL sont configurés pour le site web par défaut, tous les certificats disponibles vous sont présentés, et vous devez en sélectionner un. S'il n'y a pas de paramètres SSL configurés pour le site web par défaut, la liste est créée à partir des certificats qui sont disponibles dans le magasin de certificats personnel qui se trouve sur l'ordinateur local.  
  
    > [!NOTE]  
    > Si un certificat SSL est configuré pour les services SSL, l'Assistant ne vous autorisera pas à le remplacer. Cela garantit que toute configuration de services IIS antérieure pour les certificats SSL est conservée. Pour contourner cette restriction, vous pouvez supprimer le certificat ou le reconfigurer manuellement avec la Console de gestion IIS.  
  
5.  Si la base de données AD FS que vous avez déjà sélectionné existe, le **AD FS Configuration Database détectée** page s’affiche. Si tel est le cas, cliquez sur **Supprimer la base de données**, puis cliquez sur **Suivant**.  
  
    > [!CAUTION]  
    > Sélectionnez cette option uniquement lorsque vous êtes sûr que les données dans cette base de données AD FS ne sont pas importantes ou qu’il n’est pas utilisé dans une batterie de serveurs de fédération de production.  
  
6.  Dans la page **Prêt à appliquer les paramètres**, vérifiez les paramètres définis. Si les paramètres semblent corrects, cliquez sur **suivant** pour commencer à configurer AD FS avec ces paramètres.  
  
7.  Dans la page **Résultats de la configuration**, examinez les résultats. Lorsque toutes les étapes de configuration sont terminées, cliquez sur **fermer** pour quitter l’Assistant.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  

