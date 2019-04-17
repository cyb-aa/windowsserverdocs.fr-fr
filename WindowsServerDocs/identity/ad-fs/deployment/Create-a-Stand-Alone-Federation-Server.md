---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: "Créer un serveur de fédération autonome"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: fd075c5b7d1bfce89cc27c4917a016e7e5037ce5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-stand-alone-federation-server"></a>Créer un serveur de fédération autonome

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Une fois que vous installez le service de rôle Service de fédération et configurez les certificats requis sur un ordinateur, vous êtes prêt à configurer l’ordinateur pour qu’il devienne un serveur de fédération. Vous pouvez utiliser la procédure suivante pour configurer l’ordinateur de devenir un serveur de fédération autonome. Le fait de la création d’un serveur de fédération autonome crée également un Service de fédération. Vous créez un serveur de fédération avec l’Assistant Configuration du serveur de fédération ADFS.  
  
> [!NOTE]  
> Pour la conception \(SSO\) Single\-Sign\-On de Web fédéré, vous devez disposer d’au moins un serveur de fédération dans l’organisation partenaire de compte et au moins un serveur de fédération dans l’organisation partenaire de ressource. Pour plus d’informations, voir [où placer un serveur de fédération](https://technet.microsoft.com/library/dd807127.aspx).  
  
L’appartenance au groupe **administrateurs**, ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-create-a-stand-alone-federation-server"></a>Pour créer un serveur de fédération autonome  
  
1.  Il existe deux façons de démarrer l’Assistant Configuration du serveur de fédération ADFS. Pour démarrer l’Assistant, effectuez l’une des opérations suivantes:  
  
    -   Après l’installation du service de rôle Service de fédération est terminée, ouvrez la gestion ADFS de composants et cliquez sur le **Assistant Configuration du serveur de fédération ADFS** lier sur le **vue d’ensemble** page ou dans le **Actions** volet.  
  
    -   À tout moment une fois l’Assistant Installation terminé, ouvrez l’Explorateur Windows, accédez à la **C:\\Windows\\ADFS** dossier, puis double-cliquant sur **FsConfigWizard.exe**.  
  
2.  Sur le **Bienvenue** page, vérifiez que **créer un nouveau Service de fédération** est sélectionné, puis cliquez sur **suivant**.  
  
3.  Sur le **sélectionnez autonome autonome ou le déploiement de la batterie de serveurs**, cliquez sur **serveur de fédération autonome**, puis cliquez sur **suivant**.  
  
    > [!IMPORTANT]  
    > Lorsque vous sélectionnez l’option de serveur de fédération autonome dans l’Assistant Configuration du serveur de fédération ADFS, le compte de service associé à ce Service de fédération est automatiquement affecté au compte de SERVICE réseau. À l’aide du SERVICE réseau comme compte de service est recommandée uniquement dans les situations où vous évaluez ADFS dans un environnement de laboratoire de test. Si vous prévoyez d’utiliser l’option de serveur de fédération autonome pour déployer un serveur de fédération dans un environnement de production, il est important que vous modifiez ce compte de service pour un compte de service plus approprié qui peut être dédié à servir les requêtes pour ce Service de fédération. Modification du compte de service à un compte de SERVICE réseau atténuer les vecteurs d’attaque possibles que dans le cas contraire votre serveur de fédération vulnérable aux attaques malveillantes.  
  
4.  Sur le **spécifier le nom de Service de fédération** page, vérifiez que le **certificat SSL** affiché est correct. Dans le cas contraire, sélectionnez le certificat approprié à partir de la **certificat SSL** liste.  
  
    Ce certificat est généré à partir des paramètres \(SSL\) (Secure Sockets Layer) pour le site Web par défaut. Si le site Web par défaut n'a qu’un seul certificat SSL configuré, ce certificat est présenté et automatiquement sélectionné pour être utilisé. Si plusieurs certificats SSL sont configurés pour le site Web par défaut, tous les certificats sont répertoriés ici et vous devez sélectionner parmi. S’il n’existe pas de paramètres SSL configurés pour le site Web par défaut, la liste est générée à partir des certificats qui sont disponibles dans le magasin de certificats personnel sur l’ordinateur local.  
  
    > [!NOTE]  
    > L’Assistant vous autorisera pas à remplacer le certificat, si un certificat SSL est configuré pour IIS. Cela garantit que les destiné configuration IIS antérieures pour les certificats SSL est conservée. Pour contourner cette restriction, vous pouvez supprimer le certificat ou reconfigurer manuellement avec la Console de gestion IIS.  
  
5.  Si la base de données ADFS que vous avez sélectionné déjà existe, le **existante ADFS Configuration de base de données détectée** page s’affiche. Si tel est le cas, cliquez sur **base de données**, puis cliquez sur **suivant**.  
  
    > [!CAUTION]  
    > Sélectionnez cette option uniquement lorsque vous êtes sûr que les données de cette base de données ADFS ne sont pas importantes ou qu’il n’est pas utilisé dans une batterie de serveurs de fédération de production.  
  
6.  Sur le **prêt à appliquer les paramètres** page, passez en revue les détails. Si les paramètres sont corrects, cliquez sur **suivant** pour commencer à configurer ADFS avec ces paramètres.  
  
7.  Sur le **résultats de la Configuration** page, passez en revue les résultats. Lorsque toutes les étapes de configuration sont terminées, cliquez sur **fermer** pour quitter l’Assistant.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification: Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  

