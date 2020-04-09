---
ms.assetid: 4b81ac66-3f34-4a39-a8bf-5411131a69c2
title: 'Liste de vérification : configuration de AD FS pour consommer des revendications de AD FS 1. x'
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d522dddf99d71d8921db511af6f00488e424736e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854122"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>Liste de vérification : Configuration des services AD FS pour envoyer des revendications à un service FS (Federation Service) AD FS 1.x

  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-federation-service"></a>Liste de vérification : configuration de AD FS pour envoyer des revendications à un AD FS 1. x service FS (Federation Service)  
Cette liste de vérification comprend les tâches nécessaires à la configuration de votre Services ADFS \(AD FS\) service FS (Federation Service) dans Windows Server 2012 pour envoyer des revendications qui peuvent être comprises par un AD FS 1. service FS (Federation Service) *x* .  
  
> [!NOTE]  
> Effectuez dans l’ordre les tâches répertoriées dans cette liste de vérification. Quand un lien de référence vous conduit à une procédure, revenez dans cet article, une fois les étapes de la procédure effectuées, afin de pouvoir accomplir les tâches restantes de la liste.  
  
![configurer AD FS pour envoyer des revendications](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**liste de vérification : configuration de AD FS pour envoyer des revendications à un AD FS 1. x service FS (Federation Service)**  
  
||Tâche|Référence|  
|-|--------|-------------|  
|![configurer AD FS pour envoyer des revendications](media/icon_checkboxo.gif)|Planifiez l’interopérabilité entre les AD FS dans Windows Server 2012 et les versions précédentes de AD FS et en savoir plus sur le type de revendication ID de nom.|![configurer AD FS pour envoyer](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[des revendications planification de l’interopérabilité avec AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![configurer AD FS pour envoyer des revendications](media/icon_checkboxo.gif)|Avant de pouvoir assurer l’interopérabilité avec une version précédente de AD FS, vous devez d’abord créer une approbation de partie de confiance dans le AD FS service FS (Federation Service) au AD FS 1. service FS (Federation Service) *x* . **Remarque :** Vous ne pouvez pas créer une relation d’approbation avec un AD FS 1. *x* service FS (Federation Service) à l’aide des métadonnées de Fédération.<p>Quand vous configurez l’approbation à l’aide de la procédure indiquée dans le lien situé à droite, vous devez effectuer les opérations suivantes dans l’Assistant Ajout d’approbation de partie de confiance pour configurer cette approbation afin qu’elle interagisse avec un AD FS 1. service FS (Federation Service) *x* :<p>1. dans la page **Sélectionner une source de données** , sélectionnez **entrer les données relatives à l’approbation de la partie de confiance manuellement**.<br />2. dans la page **choisir un profil** , sélectionnez le **Profil AD FS 1,0 et 1,1**.<br />3. dans la page **configurer l’URL** , sous **URL passive de Fédération WS\-** , tapez l' **URL du point de terminaison de service FS (Federation Service)** , comme défini dans la AD FS 1. *x* service FS (Federation Service) du partenaire.<br />4. dans la page **configurer les identificateurs** , sous identificateur de l’approbation de la **partie de confiance**, tapez l’URI du **service FS (Federation Service)** , tel que défini dans le AD FS 1. *x* service FS (Federation Service) du partenaire.|![configurer AD FS pour envoyer des revendications](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[créer manuellement une approbation de partie de confiance](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![configurer AD FS pour envoyer des revendications](media/icon_checkboxo.gif)|Sur l’approbation de la partie de confiance que vous avez créée précédemment, vous devez créer des règles de revendication qui vont accepter les revendications entrantes extraites d’un magasin d’attributs et les transmettre, filtrer ou transformer en un type de revendication d’ID de nom qui peut être compris et consommé par le AD FS 1. service FS (Federation Service) *x* . **Remarque :** Avant de créer cette règle, assurez-vous que le jeu de règles de revendication dans lequel vous créez cette règle comporte une règle antérieure qui extrait d’abord un protocole d’accès à Lightweight Directory Access \(LDAP\) revendication d’attribut à partir d’un magasin d’attributs. Cette revendication sera utilisée comme entrée pour la règle que vous créez pour envoyer un AD FS 1. revendication *x*\-compatible. Pour plus d’informations sur la création d’une règle pour extraire un attribut LDAP, consultez [créer une règle pour envoyer des attributs LDAP en tant que revendications](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configurer AD FS pour envoyer des revendications](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[créer une règle pour envoyer une revendication Compatible AD FS 1. x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![configurer AD FS pour envoyer des revendications](media/icon_checkboxo.gif)|Contactez l’administrateur du AD FS 1. *x* service FS (Federation Service) et que l’administrateur du AD FS 1. *x* service FS (Federation Service) configurer une nouvelle approbation de partenaire de compte. En outre, fournissez à cet administrateur l’URI service FS (Federation Service) \(dans les propriétés de la service FS (Federation Service)\), l’URL du point de terminaison passif de la Fédération WS\-\(l’URL de point de terminaison service FS (Federation Service)\)et un jeton exporté\-fichier de certificat de signature \(avec la clé publique uniquement\). Cet administrateur aura besoin de ces éléments pour configurer l’approbation.|N\/un|  
  

