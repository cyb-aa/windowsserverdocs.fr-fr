---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: Créer un serveur de fédération autonome
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 603786d8553cce20f0b559ba8a91dfc29f760488
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855482"
---
# <a name="create-a-stand-alone-federation-server"></a>Créer un serveur de fédération autonome

Après avoir installé le service de rôle service FS (Federation Service) et configuré les certificats requis sur un ordinateur, vous êtes prêt à configurer l’ordinateur pour qu’il devienne un serveur de Fédération. Vous pouvez utiliser la procédure suivante pour configurer l’ordinateur pour qu’il devienne un serveur de Fédération autonome\-. Le fait de créer un serveur de Fédération autonome\-crée également un nouveau service FS (Federation Service). Vous créez un serveur de Fédération à l’aide de l’Assistant Configuration du serveur de fédération AD FS.  
  
> [!NOTE]  
> Pour le\-de signature de\-unique Web fédéré sur \(conception de\) SSO, vous devez disposer d’au moins un serveur de Fédération dans l’organisation partenaire de compte et au moins un serveur de Fédération dans l’organisation partenaire de ressource. Pour plus d'informations, voir [Où placer un serveur de fédération](https://technet.microsoft.com/library/dd807127.aspx).  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs** ou d'un groupe équivalent sur l'ordinateur local.  Passez en revue les détails sur l’utilisation des comptes et des appartenances aux groupes appropriés dans les [groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \(http :\/\/Go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-create-a-stand-alone-federation-server"></a>Pour créer un serveur de Fédération autonome\-  
  
1.  L'Assistant Configuration de serveur de fédération AD FS peut être démarré de deux manières différentes. Procédez selon l'une d'elles :  
  
    -   Une fois l’installation du service de rôle service FS (Federation Service) terminée, ouvrez le composant logiciel enfichable Gestion des AD FS\-dans et cliquez sur le lien de l' **Assistant Configuration du serveur de fédération AD FS** dans la page **vue d’ensemble** ou dans le volet **actions** .  
  
    -   À chaque fois que l’Assistant installation est terminé, ouvrez l’Explorateur Windows, accédez au dossier **C :\\windows\\ADFS** , puis double\-cliquez sur **FsConfigWizard. exe**.  
  
2.  Dans la page **Bienvenue**, vérifiez que la case **Créer un service de fédération** est cochée, puis cliquez sur **Suivant**.  
  
3.  Dans la page **Sélectionner le stand\-seul ou déployer une batterie de serveurs** , cliquez sur **stand\-seul serveur de Fédération**, puis cliquez sur **suivant**.  
  
    > [!IMPORTANT]  
    > Lorsque vous sélectionnez l’option stand\-seul serveur de Fédération dans l’Assistant Configuration du serveur de fédération AD FS, le compte de service associé à cette service FS (Federation Service) est automatiquement affecté au compte de SERVICE réseau. L’utilisation du SERVICE réseau comme compte de service n’est recommandée que dans les situations où vous évaluez AD FS dans un environnement de laboratoire de test. Si vous envisagez d’utiliser l’option stand\-seul serveur de Fédération pour déployer un serveur de Fédération dans un environnement de production, il est important de modifier ce compte de service en un compte de service plus approprié qui peut être dédié aux demandes de ce nouveau service FS (Federation Service). La modification du compte de service en un compte autre que le SERVICE réseau permet d’atténuer les vecteurs d’attaque possibles qui pourraient rendre votre serveur de Fédération vulnérable aux attaques malveillantes.  
  
4.  Dans la page **Spécifier le nom du service de fédération**, vérifiez que le **Certificat SSL** affiché est correct. Si ce n’est pas le cas, sélectionnez le certificat approprié dans la liste **certificat SSL** .  
  
    Ce certificat est généré à partir des paramètres de\) protocole SSL \(SSL pour le site Web par défaut. Si un seul certificat est configuré pour le site Web par défaut, il est présenté et automatiquement sélectionné pour être utilisé. Si plusieurs certificats SSL sont configurés pour le site Web par défaut, tous sont répertoriés et vous devez en sélectionner un dans la liste. Si aucun paramètre SSL n'est configuré pour le site Web par défaut, la liste est générée à partir des certificats disponibles dans le magasin de certificats personnel de l'ordinateur local.  
  
    > [!NOTE]  
    > L’Assistant ne vous permet pas de remplacer le certificat si un certificat SSL est configuré pour IIS. Cela garantit que toute configuration IIS antérieure des certificats SSL est préservée. Pour contourner cette restriction, vous pouvez supprimer le certificat ou le reconfigurer manuellement à l’aide de la console de gestion IIS.  
  
5.  Si la base de données AD FS sélectionnée existe déjà, la page **Base de données de configuration AD FS existante détectée** apparaît. Le cas échéant, cliquez sur **Supprimer la base de données**, puis sur **Suivant**.  
  
    > [!CAUTION]  
    > Sélectionnez cette option uniquement lorsque vous êtes sûr que les données de cette base de données AD FS ne sont pas importantes ni utilisées dans une batterie de serveurs de fédération de production.  
  
6.  Passez en revue les détails dans la page **Prêt à appliquer les paramètres**. Si les paramètres semblent corrects, cliquez sur **Suivant** pour commencer à configurer AD FS avec ces paramètres.  
  
7.  Dans la page **Résultats de la configuration**, passez en revue les résultats. Une fois toutes les étapes de configuration terminées, cliquez sur **Fermer** pour quitter l’Assistant.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : configuration d’un serveur de Fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  

