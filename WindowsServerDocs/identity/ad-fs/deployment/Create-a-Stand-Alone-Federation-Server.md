---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: Créer un serveur de fédération autonome
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 3a948fadeb1cfa2ebe259a9bb659e93a85e06910
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359660"
---
# <a name="create-a-stand-alone-federation-server"></a>Créer un serveur de fédération autonome

Après avoir installé le service de rôle service FS (Federation Service) et configuré les certificats requis sur un ordinateur, vous êtes prêt à configurer l’ordinateur pour qu’il devienne un serveur de Fédération. Vous pouvez utiliser la procédure suivante pour configurer l’ordinateur pour qu’il devienne un serveur de Fédération stand @ no__t-0alone. Le fait de créer un serveur de Fédération stand @ no__t-0alone crée également un nouveau service FS (Federation Service). Vous créez un serveur de Fédération à l’aide de l’Assistant Configuration du serveur de fédération AD FS.  
  
> [!NOTE]  
> Pour la conception Web unique fédérée @ no__t-0Sign @ no__t-1On \(SSO @ no__t-3, vous devez disposer d’au moins un serveur de Fédération dans l’organisation partenaire de compte et au moins un serveur de Fédération dans l’organisation partenaire de ressource. Pour plus d'informations, voir [Où placer un serveur de fédération](https://technet.microsoft.com/library/dd807127.aspx).  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Passez en revue les détails sur l’utilisation des comptes et des appartenances aux groupes appropriés dans les\/ \( [groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) http :\/Go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-create-a-stand-alone-federation-server"></a>Pour créer un serveur de Fédération stand @ no__t-0alone  
  
1.  Il existe deux façons de démarrer l’Assistant Configuration du serveur de fédération AD FS. Pour démarrer l'Assistant, effectuez l'une des opérations suivantes :  
  
    -   Une fois l’installation du service de rôle service FS (Federation Service) terminée, ouvrez le AD FS Management Snap @ no__t-0in et cliquez sur le lien **AD FS Assistant Configuration du serveur de Fédération** dans la page **vue d’ensemble** ou dans le volet **actions** .  
  
    -   À chaque fois que l’Assistant installation est terminé, ouvrez l’Explorateur Windows, accédez au dossier **C : \\Windows @ no__t-2ADFS** , puis double @ no__t-3Cliquez **FsConfigWizard. exe**.  
  
2.  Dans la page **Bienvenue**, vérifiez que l'option **Créer un service de fédération** est sélectionnée, puis cliquez sur **Suivant**.  
  
3.  Dans la page **Sélectionner le stand @ no__t-1Alone ou déploiement** de la batterie de serveurs, cliquez sur **stand @ no__t-3alone Federation Server**, puis cliquez sur **suivant**.  
  
    > [!IMPORTANT]  
    > Lorsque vous sélectionnez l’option stand @ no__t-0alone Federation Server dans l’Assistant Configuration du serveur de fédération AD FS, le compte de service associé à ce service FS (Federation Service) est automatiquement affecté au compte de SERVICE réseau. L’utilisation du SERVICE réseau comme compte de service n’est recommandée que dans les situations où vous évaluez AD FS dans un environnement de laboratoire de test. Si vous envisagez d’utiliser l’option de serveur de Fédération stand @ no__t-0alone pour déployer un serveur de Fédération dans un environnement de production, il est important de modifier ce compte de service en un compte de service plus approprié qui peut être dédié à la traitement des demandes pour ce nouvel service FS (Federation Service). La modification du compte de service en un compte autre que le SERVICE réseau permet d’atténuer les vecteurs d’attaque possibles qui pourraient rendre votre serveur de Fédération vulnérable aux attaques malveillantes.  
  
4.  Dans la page **Spécifier le nom du service FS**, vérifiez que le **Certificat SSL** affiché est correct. Si ce n’est pas le cas, sélectionnez le certificat approprié dans la liste **certificat SSL** .  
  
    Ce certificat est généré à partir des paramètres protocole SSL \(SSL @ no__t-1 pour le site Web par défaut. Si un seul certificat SSL est configuré pour le site web par défaut, il est présenté et automatiquement sélectionné comme certificat à utiliser. Si plusieurs certificats SSL sont configurés pour le site web par défaut, tous les certificats disponibles vous sont présentés, et vous devez en sélectionner un. S'il n'y a pas de paramètres SSL configurés pour le site web par défaut, la liste est créée à partir des certificats qui sont disponibles dans le magasin de certificats personnel qui se trouve sur l'ordinateur local.  
  
    > [!NOTE]  
    > Si un certificat SSL est configuré pour les services SSL, l'Assistant ne vous autorisera pas à le remplacer. Cela garantit que toute configuration de services IIS antérieure pour les certificats SSL est conservée. Pour contourner cette restriction, vous pouvez supprimer le certificat ou le reconfigurer manuellement à l’aide de la console de gestion IIS.  
  
5.  Si la base de données de AD FS que vous avez sélectionnée existe déjà, la page **base de données de Configuration AD FS détectée** apparaît. Si tel est le cas, cliquez sur **Supprimer la base de données**, puis cliquez sur **Suivant**.  
  
    > [!CAUTION]  
    > Sélectionnez cette option uniquement lorsque vous êtes sûr que les données de cette AD FS base de données ne sont pas importantes ou qu’elles ne sont pas utilisées dans une batterie de serveurs de Fédération de production.  
  
6.  Dans la page **Prêt à appliquer les paramètres**, vérifiez les paramètres définis. Si les paramètres semblent corrects, cliquez sur **suivant** pour commencer à configurer AD FS avec ces paramètres.  
  
7.  Dans la page **Résultats de la configuration**, examinez les résultats. Une fois toutes les étapes de configuration terminées, cliquez sur **Fermer** pour quitter l’Assistant.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  

