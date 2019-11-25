---
ms.assetid: e7f9e518-2d5d-4a0d-9147-34e1304f42ac
title: 'Liste de vérification : configuration de AD FS pour consommer des revendications de AD FS 1. x'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1c952abc0bca5eadbfc14f3eda54d05826d50294
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408496"
---
# <a name="checklist-configuring-ad-fs--to-consume-claims-from-ad-fs-1x"></a>Liste de vérification : configuration de AD FS pour consommer des revendications de AD FS 1. x

  
## <a name="checklist-configuring-ad-fs-to-consume-claims-from-adfs1x"></a>Liste de vérification : configuration de AD FS pour consommer des revendications de AD FS 1. x  
Cette liste de vérification comprend les tâches nécessaires à la configuration de votre Services ADFS \(AD FS\) service FS (Federation Service) dans Windows Server 2012 pour consommer les revendications envoyées par un AD FS 1. service FS (Federation Service) *x* .  
  
> [!NOTE]  
> Effectuez dans l’ordre les tâches répertoriées dans cette liste de vérification. Lorsqu’un lien de référence vous amène à une procédure, revenez à cette rubrique après avoir effectué les étapes de cette procédure afin de pouvoir effectuer les tâches restantes de cette liste de vérification.  
  
![utiliser les revendications de AD FS](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**liste de vérification : configuration des AD FS pour utiliser les revendications de AD FS 1. x**  
  
||Tâche|Référence|  
|-|--------|-------------|  
|![utiliser des revendications de AD FS](media/icon_checkboxo.gif)|Planifiez l’interopérabilité entre les AD FS dans Windows Server 2012 et les versions précédentes de AD FS, et en savoir plus sur le type de revendication ID de nom.|![consomment des revendications de AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planification de l’interopérabilité avec AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![utiliser des revendications de AD FS](media/icon_checkboxo.gif)|Avant de pouvoir interagir avec une version précédente de AD FS, vous devez d’abord créer une approbation de fournisseur de revendications dans le service FS (Federation Service) AD FS. **Remarque :** Vous ne pouvez pas créer une relation d’approbation avec un AD FS 1. *x* service FS (Federation Service) à l’aide des métadonnées de Fédération.<br /><br />Quand vous configurez l’approbation à l’aide de la procédure indiquée dans le lien situé à droite, vous devez effectuer les opérations suivantes dans l’Assistant Ajout d’approbation de fournisseur de revendications pour configurer cette approbation afin qu’elle interagisse avec un AD FS 1. service FS (Federation Service) *x* :<br /><br />1. dans la page **Sélectionner une source de données** , sélectionnez **entrer les données relatives à l’approbation de la partie de confiance manuellement**.<br />2. dans la page **choisir un profil** , sélectionnez le **Profil AD FS 1,0 et 1,1**.<br />3. dans la page **configurer l’URL** , sous **URL passive de Fédération WS\-** , tapez l' **URL du point de terminaison de service FS (Federation Service)** , comme défini dans la AD FS 1. *x* service FS (Federation Service) du partenaire.<br />4. dans la page **configurer les identificateurs** , sous identificateur de l' **approbation du fournisseur de revendications**, tapez l’URI du **service FS (Federation Service)** , tel que défini dans le AD FS 1. *x* service FS (Federation Service) du partenaire.|![consommer des revendications de AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[créer manuellement une approbation de fournisseur de revendications](../../ad-fs/operations/Create-a-Claims-Provider-Trust.md)|  
|![utiliser des revendications de AD FS](media/icon_checkboxo.gif)|Sur l’approbation de fournisseur de revendications que vous avez créée précédemment, vous devez créer une règle de revendication qui prend les revendications entrantes à partir du AD FS 1. x service FS (Federation Service) et les transmet, les filtre ou les transforme en un type de revendication d’identificateur de nom.<br /><br />Lorsque le type de revendication ID de nom a été transmis, filtré ou transformé, il peut être utilisé comme entrée pour une autre règle ou des règles afin qu’il puisse être compris et consommé par le AD FS service FS (Federation Service) dans Windows Server 2012.|![utiliser des revendications de AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[créer une règle pour envoyer une revendication Compatible AD FS 1. x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![utiliser des revendications de AD FS](media/icon_checkboxo.gif)|Contactez l’administrateur du AD FS 1. *x* service FS (Federation Service) et que l’administrateur du AD FS 1. *x* service FS (Federation Service) configurer une nouvelle approbation de partenaire de ressource. En outre, fournissez à cet administrateur l’URI service FS (Federation Service) \(dans les propriétés du service FS (Federation Service)\), l’URL du point de terminaison service FS (Federation Service) et un jeton exporté\-fichier de certificat de signature \(avec la clé publique uniquement\). L’administrateur aura besoin de ces éléments pour configurer l’approbation.|N\/un|  
  

