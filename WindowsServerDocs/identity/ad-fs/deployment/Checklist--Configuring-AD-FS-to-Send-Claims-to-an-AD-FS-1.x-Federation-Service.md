---
ms.assetid: 4b81ac66-3f34-4a39-a8bf-5411131a69c2
title: 'Liste de vérification : configuration de AD FS pour consommer des revendications de AD FS 1. x'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: f91944333da9ce4c1d78bbbf7b3652f1118e1f08
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408492"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>Liste de vérification : Configuration de AD FS pour envoyer des revendications à un AD FS 1. x service FS (Federation Service)

  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-federation-service"></a>Liste de vérification : Configuration de AD FS pour envoyer des revendications à un AD FS 1. x service FS (Federation Service)  
Cette liste de vérification comprend les tâches nécessaires à la configuration de votre Services ADFS \(AD FS @ no__t-1 service FS (Federation Service) dans Windows Server 2012 pour envoyer des revendications qui peuvent être comprises par un AD FS 1. service FS (Federation Service) *x* .  
  
> [!NOTE]  
> Effectuez dans l’ordre les tâches répertoriées dans cette liste de vérification. Lorsqu’un lien de référence vous amène à une procédure, revenez à cette rubrique après avoir effectué les étapes de cette procédure afin de pouvoir effectuer les tâches restantes de cette liste de vérification.  
  
![configure AD FS pour envoyer des revendications @ no__t-1Checklist : Configuration de AD FS pour envoyer des revendications à un AD FS 1. x service FS (Federation Service) @ no__t-0  
  
||Tâche|Référence|  
|-|--------|-------------|  
|![configurer AD FS pour envoyer des revendications](media/icon_checkboxo.gif)|Planifiez l’interopérabilité entre les AD FS dans Windows Server 2012 et les versions précédentes de AD FS et en savoir plus sur le type de revendication ID de nom.|@no__t 0configure AD FS pour envoyer des revendications](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planification de l’interopérabilité avec AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![configurer AD FS pour envoyer des revendications](media/icon_checkboxo.gif)|Avant de pouvoir assurer l’interopérabilité avec une version précédente de AD FS, vous devez d’abord créer une approbation de partie de confiance dans le AD FS service FS (Federation Service) au AD FS 1. service FS (Federation Service) *x* . **Remarque :** Vous ne pouvez pas créer une relation d’approbation avec un AD FS 1. *x* service FS (Federation Service) à l’aide des métadonnées de Fédération.<br /><br />Quand vous configurez l’approbation à l’aide de la procédure indiquée dans le lien situé à droite, vous devez effectuer les opérations suivantes dans l’Assistant Ajout d’approbation de partie de confiance pour configurer cette approbation afin qu’elle interagisse avec un AD FS 1. service FS (Federation Service) *x* :<br /><br />1.  Dans la page **Sélectionner une source de données** , sélectionnez **entrer manuellement les données relatives à l’approbation de la partie de confiance**.<br />2.  Dans la page **choisir un profil** , sélectionnez le **Profil AD FS 1,0 et 1,1**.<br />3.  Dans la page **configurer l’URL** , sous **URL passive WS @ no__t-2Federation**, tapez l’URL du point de **terminaison service FS (Federation Service)** , comme défini dans le AD FS 1. *x* service FS (Federation Service) du partenaire.<br />4.  Dans la page **configurer les identificateurs** , sous identificateur de l’approbation de la **partie de confiance**, tapez l’URI du **service FS (Federation Service)** , tel que défini dans le AD FS 1. *x* service FS (Federation Service) du partenaire.|![configure AD FS pour envoyer des revendications](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[créer manuellement une approbation de partie de confiance](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![configurer AD FS pour envoyer des revendications](media/icon_checkboxo.gif)|Sur l’approbation de la partie de confiance que vous avez créée précédemment, vous devez créer des règles de revendication qui vont accepter les revendications entrantes extraites d’un magasin d’attributs et les transmettre, les filtrer ou les transformer en un type de revendication d’ID de nom qui peut être compris et consommé par la publicité. FS 1. service FS (Federation Service) *x* . **Remarque :** Avant de créer cette règle, assurez-vous que le jeu de règles de revendication dans lequel vous créez cette règle comporte une règle antérieure qui extrait d’abord une revendication d’attribut Lightweight Directory Access Protocol \(LDAP @ no__t-1 à partir d’un magasin d’attributs. Cette revendication sera utilisée comme entrée pour la règle que vous créez pour envoyer un AD FS 1. revendication *x*\-Compatible. Pour plus d’informations sur la création d’une règle pour extraire un attribut LDAP, consultez [créer une règle pour envoyer des attributs LDAP en tant que revendications](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configure AD FS pour envoyer des revendications](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[créer une règle pour envoyer une revendication Compatible AD FS 1. x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![configurer AD FS pour envoyer des revendications](media/icon_checkboxo.gif)|Contactez l’administrateur du AD FS 1. *x* service FS (Federation Service) et que l’administrateur du AD FS 1. *x* service FS (Federation Service) configurer une nouvelle approbation de partenaire de compte. Fournissez également à cet administrateur l’URI service FS (Federation Service) \(in les propriétés du service FS (Federation Service) @ no__t-1, l’URL du point de terminaison passif WS @ no__t-2Federation \(La service FS (Federation Service) URL du point de terminaison @ no__t-4 et un jeton exporté @ no__ fichier de certificat t-5signing \(with clé publique uniquement @ no__t-7. Cet administrateur aura besoin de ces éléments pour configurer l’approbation.|N\/A|  
  

