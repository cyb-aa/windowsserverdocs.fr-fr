---
ms.assetid: 551c1a0d-8d30-41b4-9c4a-35a3337dd3bc
title: Déploiement de serveurs de fédération
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 874984b469303c0f8a40a676632c144ee6079f44
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192431"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>Liste de vérification : Configuration d’AD FS pour envoyer des revendications à un Agent de Web prenant en charge les revendications AD FS 1.x

  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-claims-aware-web-agent"></a>Liste de vérification : Configurer AD FS pour envoyer les revendications en un cas de réclamation AD FS 1.x\-agent Web prenant en charge  
Cette liste comprend les tâches qui sont nécessaires à la configuration de votre Active Directory Federation Services \(AD FS\) Service de fédération dans Windows Server 2012 pour envoyer des revendications qui peuvent être reconnues par une application est hébergée par un Serveur Web exécutant les services AD FS 1. *x* revendications\-agent Web prenant en charge.  
  
> [!NOTE]  
> Effectuez dans l’ordre les tâches répertoriées dans cette liste de vérification. Quand un lien de référence vous mène à une procédure, revenez à cette rubrique après avoir terminé les étapes décrites dans cette procédure afin que vous pouvez poursuivre les tâches restantes dans cette liste de vérification.  
  
![configurer AD FS pour envoyer des revendications](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**liste de vérification : Configurer AD FS pour envoyer les revendications en un cas de réclamation AD FS 1.x\-agent Web prenant en charge**  
  
||Tâche|Référence|  
|-|--------|-------------|  
|![configurer AD FS pour envoyer les revendications](media/icon_checkboxo.gif)|Planifiez l’interopérabilité entre AD FS dans Windows Server 2012 et les versions précédentes d’AD FS et en savoir que plus sur l’ID de nom de type de revendication.|![configurer AD FS pour envoyer des revendications](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planification de l’interopérabilité avec AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![configurer AD FS pour envoyer les revendications](media/icon_checkboxo.gif)|Si vous ne le n'avez pas déjà fait, vous pouvez utiliser le lien de droite à tout d’abord créer une approbation de partie de confiance entre le Service de fédération AD FS dans Windows Server 2012 et les services AD FS 1. *x* Service de fédération.|[Liste de vérification : configuration des services AD FS pour envoyer des revendications à un service de fédération AD FS 1.x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|  
|![configurer AD FS pour envoyer les revendications](media/icon_checkboxo.gif)|Avant, vous pouvez obtenir l’interopérabilité avec une application qui est hébergée par AD FS 1. *x* revendications\-agent Web prenant en charge, vous devez d’abord créer une partie de confiance dans le Service de fédération AD FS dans Windows Server 2012 vers AD FS 1. *x* revendications\-agent Web prenant en charge. **Remarque :** Création de cette approbation dans le Service de fédération AD FS est l’équivalent de l’ajout d’une nouvelle **Application** AD FS 1.x Federation Service \( **Service de fédération\\stratégie d’approbation\\ Mon organisation\\Application**\). Cette partie de confiance est nécessaire étant donné que AD FS n’a pas d’équivalent **Application** nœud dans son propre composant logiciel enfichable\-dans. Toutefois, il doit toujours avoir un canal sécurisé à l’application.<br /><br />Lorsque vous configurez l’approbation à l’aide de la procédure dans le lien vers la droite, vous devez effectuer les opérations suivantes dans l’Assistant Ajout partie tiers confiance pour définir cette confiance pour interagir avec un AD FS 1. *x* revendications\-agent Web prenant en charge :<br /><br />1.  Sur le **sélectionner une Source de données** , sélectionnez **entrer des données sur la partie de confiance manuellement confiance**.<br />2.  Sur le **choisir le profil** page, sélectionnez **profil ADFS 1.0 et 1.1**.<br />3.  Sur le **configurer l’URL** page sous **WS\-URL Passive Federation**, type la **URL de l’Application** tel que défini dans les services AD FS 1. *x* Service de fédération du partenaire.<br />4.  Sur le **configurer les identificateurs** page sous **identifiant d’approbation de partie de partie de confiance**, type de la **URL de l’Application** tel que défini dans les services AD FS 1. *x* revendications\-agent Web prenant en charge|![configurer AD FS pour envoyer des revendications](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[créer une partie de confiance manuellement](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![configurer AD FS pour envoyer les revendications](media/icon_checkboxo.gif)|Contactez l’administrateur du serveur Web exécutant les services AD FS 1. *x* revendications\-prenant en charge Web agent et avez cet administrateur de modifier le fichier web.config qui est associé avec les revendications\-application prenant en charge \(sous le Site Web par défaut dans Internet Information Services \(IIS\) \) pour pointer l’agent Web au niveau du Service de fédération AD FS.<br /><br />Par exemple, remplacez *myresourcefederationserver* dans la balise `<fs> https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>` du fichier web.config avec un nom de serveur de fédération AD FS valid.<br /><br />Cela est nécessaire pour l’application et les services AD FS 1.x revendications\-agent Web prenant en charge pour être en mesure de consommer les revendications qui lui sont envoyées à partir du Service de fédération AD FS dans Windows Server 2012.|N\/A|  
|![configurer AD FS pour envoyer les revendications](media/icon_checkboxo.gif)|Sur la partie de confiance que vous avez créé précédemment, vous devez créer des règles de revendication qui seront utilisent des revendications entrantes qui ont été extraits à partir d’un magasin d’attributs et passer, filtrer ou les transformer en un type de revendication de nom ID qui peut être compris et consommé par le AD FS 1. *x* revendications\-agent Web prenant en charge. **Remarque :** Avant de créer cette règle, assurez-vous que l’ensemble de règles de revendication dans lequel vous créez cette règle a une règle qui le précède qui extrait d’abord un Lightweight Directory Access Protocol \(LDAP\) revendication d’attribut à partir d’un magasin d’attributs. Cette revendication sera utilisée comme entrée pour la règle que vous créez pour envoyer un AD FS 1. *x*\-revendication compatible avec. Pour plus d’informations sur la création d’une règle pour extraire un attribut LDAP, consultez [créer une règle pour envoyer les attributs LDAP en tant que revendications](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configurer AD FS pour envoyer des revendications](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[créer une règle pour envoyer un AD FS 1.x revendication compatible avec](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
  

