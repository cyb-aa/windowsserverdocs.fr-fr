---
ms.assetid: 551c1a0d-8d30-41b4-9c4a-35a3337dd3bc
title: "Déploiement de serveurs de fédération"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 32675aa7fb8c7b928bcf80a4d1072fe5eab7cd61
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>Liste de vérification: Configuration ADFS pour envoyer des revendications pour un Agent de Web prenant en charge les revendications ADFS 1.x

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012
  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>Liste de vérification: Configuration ADFS pour envoyer des revendications pour un agent Web prenant en charge claims\ de ADFS 1.x  
Cette liste comprend les tâches qui sont nécessaires à la configuration de votre Service de fédération de \(ADFS\) ActiveDirectory Federation Services dans Windows Server2012 pour envoyer des revendications qui peuvent être comprises par une application qui est hébergée par un serveur Web exécutant les services ADFS 1. *x* l’agent Web prenant en charge claims\.  
  
> [!NOTE]  
> Effectuez les tâches de cette liste de vérification dans l’ordre. Lorsqu’un lien de référence vous redirige vers une procédure, revenez à cette rubrique après avoir effectué les étapes décrites dans cette procédure afin que vous pouvez poursuivre les tâches restantes de cette liste de vérification.  
  
![Configurer ADFS pour envoyer des revendications](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**liste de vérification: configuration ADFS pour envoyer des revendications pour un agent Web prenant en charge claims\ de ADFS 1.x**  
  
||Tâche|Référence|  
|-|--------|-------------|  
|![Configurer ADFS pour envoyer des revendications](media/icon_checkboxo.gif)|Planifier l’interopérabilité entre les services ADFS dans Windows Server2012 et les versions antérieures d’ADFS et en savoir que plus sur l’ID de nom de type de revendication.|![Configurer ADFS pour envoyer des revendications](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planification de l’interopérabilité avec ADFS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![Configurer ADFS pour envoyer des revendications](media/icon_checkboxo.gif)|Si vous n’avez pas déjà fait, utilisez le lien à droite d’abord créer une relation de confiance entre le Service de fédération ADFS dans Windows Server2012 et les services ADFS 1. *x* Service de fédération.|[Liste de vérification: Configuration ADFS pour envoyer des revendications pour un Service de fédération ADFS 1.x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|  
|![Configurer ADFS pour envoyer des revendications](media/icon_checkboxo.gif)|Avant que vous pouvez obtenir l’interopérabilité avec une application qui est hébergée par ADFS 1. *x* agent Web prenant en claims\, vous devez d’abord créer une approbation de partie de confiance dans le Service de fédération ADFS dans Windows Server2012vers ADFS 1. *x* l’agent Web prenant en charge claims\. **Remarque:** la création de cette relation d’approbation dans le Service de fédération ADFS est l’équivalent de l’ajout d’un nouveau **Application** pour les services ADFS 1.x Federation Service \ (**fédération Service\\Trust Policy\\My Organization\\Application**\). Cette partie de confiance est nécessaire car ADFS n’a pas d’équivalent **Application** nœud dans son propre composant logiciel enfichable. Toutefois, il doit toujours avoir un canal sécurisé à l’application.<br /><br />Lorsque vous configurez l’approbation à l’aide de la procédure dans le lien vers la droite, vous devez effectuer les opérations suivantes dans l’Assistant Ajout partie partie confiance pour définir cette relation d’approbation pour interagir avec un ADFS 1. *x* l’agent Web prenant en charge claims\:<br /><br />1. Sur le **sélectionner une Source de données**, sélectionnez **entrer des données sur la partie de confiance manuellement confiance**.<br />2. Sur le **choisir le profil** page, sélectionnez **profil ADFS 1.0 et 1.1**.<br />3. Sur le **configurer l’URL** sous **URL Passive WS-Federation**, type la **URL de l’Application** tel que défini dans ADFS 1. *x* Service de fédération du partenaire.<br />4. Sur le **configurer les identificateurs** sous **identificateur d’approbation de partie de confiance partie**, type la **URL de l’Application** tel que défini dans ADFS 1. *x* l’agent Web prenant en charge claims\|![Configurer ADFS pour envoyer des revendications](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[créer une partie de confiance manuellement](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![Configurer ADFS pour envoyer des revendications](media/icon_checkboxo.gif)|Contactez l’administrateur du serveur Web exécutant les services ADFS 1. *x* prenant en charge claims\ Web agent et avez cet administrateur de modifier le fichier web.config qui est associé à l’application prenant en charge claims\ \ (sous le site Web par défaut dans \(IIS\)\) Internet Information Services pour le point de l’agent Web sur le Service de fédération ADFS.<br /><br />Par exemple, remplacez *myresourcefederationserver* dans la balise `<fs>https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>`du fichier web.config avec un nom de serveur de fédération ADFS valide.<br /><br />Cela est nécessaire pour l’application et l’agent Web prenant en charge claims\ de ADFS 1.x être en mesure de consommer les revendications sont envoyées depuis le Service de fédération ADFS dans Windows Server2012.|N\/A|  
|![Configurer ADFS pour envoyer des revendications](media/icon_checkboxo.gif)|Sur l’approbation que vous avez créé précédemment, vous devez créer la revendication règles qui seront prendre les revendications entrantes qui ont été extraits à partir d’un magasin d’attributs et passent par, filtrer ou transformer en un ID de nom revendication qui peuvent être comprises et consommée par le services ADFS 1. *x* l’agent Web prenant en charge claims\. **Remarque:** avant de créer cette règle, assurez-vous que l’ensemble de règles de revendication où vous créez cette règle a une règle qui précède il qui extrait tout d’abord une revendication d’attribut Lightweight Directory Access Protocol \(LDAP\) à partir d’un magasin d’attributs. Cette revendication sera utilisée comme entrée pour la règle que vous créez pour envoyer un ADFS 1. *x*\-compatible revendication. Pour plus d’informations sur la création d’une règle pour extraire un attribut LDAP, voir [créer une règle pour envoyer les attributs LDAP en tant que revendications](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![Configurer ADFS pour envoyer des revendications](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[créer une règle pour envoyer un ADFS 1.x revendication compatible avec](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
  

