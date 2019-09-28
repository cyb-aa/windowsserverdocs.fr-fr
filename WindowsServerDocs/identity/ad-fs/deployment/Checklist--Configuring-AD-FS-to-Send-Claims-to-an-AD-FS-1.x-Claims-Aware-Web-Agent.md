---
ms.assetid: 551c1a0d-8d30-41b4-9c4a-35a3337dd3bc
title: Déploiement de serveurs de fédération
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 689bd33bc95c2b142dfbe6d0448a604b2971979e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359981"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>Liste de vérification : Configuration de AD FS pour envoyer des revendications à un agent Web prenant en charge les revendications AD FS 1. x

  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-claims-aware-web-agent"></a>Liste de vérification : Configuration de AD FS pour envoyer des revendications à un agent Web AD FS 1. x claims @ no__t-0aware  
Cette liste de vérification comprend les tâches nécessaires à la configuration de votre Services ADFS \(AD FS @ no__t-1 service FS (Federation Service) dans Windows Server 2012 pour envoyer des revendications qui peuvent être comprises par une application hébergée par un serveur Web. exécution du AD FS 1. *x* claims @ no__t-3aware Web agent.  
  
> [!NOTE]  
> Effectuez dans l’ordre les tâches répertoriées dans cette liste de vérification. Lorsqu’un lien de référence vous amène à une procédure, revenez à cette rubrique après avoir effectué les étapes de cette procédure afin de pouvoir effectuer les tâches restantes de cette liste de vérification.  
  
![configure AD FS pour envoyer des revendications @ no__t-1Checklist : Configuration de AD FS pour envoyer des revendications à un AD FS 1. x claims @ no__t-0aware-agent Web @ no__t-1  
  
||Tâche|Référence|  
|-|--------|-------------|  
|![configurer AD FS pour envoyer des revendications](media/icon_checkboxo.gif)|Planifiez l’interopérabilité entre les AD FS dans Windows Server 2012 et les versions précédentes de AD FS et en savoir plus sur le type de revendication ID de nom.|@no__t 0configure AD FS pour envoyer des revendications](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planification de l’interopérabilité avec AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![configurer AD FS pour envoyer des revendications](media/icon_checkboxo.gif)|Si vous ne l’avez pas encore fait, utilisez le lien sur la droite pour créer d’abord une approbation de partie de confiance entre le AD FS service FS (Federation Service) dans Windows Server 2012 et le AD FS 1. service FS (Federation Service) *x* .|[Liste de vérification : configuration des services AD FS pour envoyer des revendications à un service de fédération AD FS 1.x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|  
|![configurer AD FS pour envoyer des revendications](media/icon_checkboxo.gif)|Avant de pouvoir obtenir une interopérabilité avec une application hébergée par le AD FS 1. *x* claims @ no__t-1aware Web agent, vous devez d’abord créer une approbation de partie de confiance dans le AD FS service FS (Federation Service) de Windows Server 2012 à la AD FS 1. *x* claims @ no__t-1aware Web agent. **Remarque :** La création de cette approbation dans le service FS (Federation Service) AD FS revient à ajouter une nouvelle **application** à la AD FS 1. x service FS (Federation Service) \(**service FS (Federation Service) @ No__t-3Trust stratégie @ no__t-4My \ no__t-5Application**\). Cette approbation de partie de confiance est nécessaire, car AD FS n’a pas de nœud d' **application** équivalent dans son propre Snap @ no__t-1Dans. Toutefois, il doit toujours avoir un canal sécurisé vers l’application.<br /><br />Quand vous configurez l’approbation à l’aide de la procédure indiquée dans le lien situé à droite, vous devez effectuer les opérations suivantes dans l’Assistant Ajout d’approbation de partie de confiance pour configurer cette approbation afin qu’elle interagisse avec un AD FS 1. *x* claims @ no__t-1Aware agent Web :<br /><br />1.  Dans la page **Sélectionner une source de données** , sélectionnez **entrer manuellement les données relatives à l’approbation de la partie de confiance**.<br />2.  Dans la page **choisir un profil** , sélectionnez le **Profil AD FS 1,0 et 1,1**.<br />3.  Dans la page **configurer l’URL** , sous **WS @ No__t-2FEDERATION passive URL**, tapez l’URL de l' **application** telle que définie dans le AD FS 1. *x* service FS (Federation Service) du partenaire.<br />4.  Dans la page **configurer les identificateurs** , sous identificateur de l’approbation de la **partie de confiance**, tapez l’URL de l' **application** telle que définie dans le AD FS 1. *x* claims @ no__t-4aware Web agent|![configure AD FS pour envoyer des revendications](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[créer manuellement une approbation de partie de confiance](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![configurer AD FS pour envoyer des revendications](media/icon_checkboxo.gif)|Contactez l’administrateur du serveur Web exécutant le AD FS 1. *x* claims @ no__t-1Aware agent Web et demander à l’administrateur de modifier le fichier Web. config associé à l’application claims @ no__t-2aware \(Under le site Web par défaut dans Internet Information Services \(IIS @ no__t-5 @ no__t-6 à pointez l’agent Web sur le AD FS service FS (Federation Service).<br /><br />Par exemple, remplacez *myresourcefederationserver* dans la balise `<fs> https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>` du fichier Web. config par un nom de serveur de fédération AD FS valide.<br /><br />Cela est nécessaire pour que l’application et l’AD FS 1. x claims @ no__t-0aware agent Web puissent utiliser les revendications qui lui sont envoyées à partir de la service FS (Federation Service) AD FS dans Windows Server 2012.|N\/A|  
|![configurer AD FS pour envoyer des revendications](media/icon_checkboxo.gif)|Sur l’approbation de partie de confiance que vous avez créée précédemment, vous devez créer des règles de revendication qui acceptent les revendications entrantes extraites d’un magasin d’attributs et transmises, filtrez-les ou transformez-les en un type de revendication d’ID de nom qui peut être compris et consommé par le AD FS 1. *x* claims @ no__t-1aware Web agent. **Remarque :** Avant de créer cette règle, assurez-vous que le jeu de règles de revendication dans lequel vous créez cette règle comporte une règle antérieure qui extrait d’abord une revendication d’attribut Lightweight Directory Access Protocol \(LDAP @ no__t-1 à partir d’un magasin d’attributs. Cette revendication sera utilisée comme entrée pour la règle que vous créez pour envoyer un AD FS 1. revendication *x*\-Compatible. Pour plus d’informations sur la création d’une règle pour extraire un attribut LDAP, consultez [créer une règle pour envoyer des attributs LDAP en tant que revendications](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configure AD FS pour envoyer des revendications](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[créer une règle pour envoyer une revendication Compatible AD FS 1. x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
  

