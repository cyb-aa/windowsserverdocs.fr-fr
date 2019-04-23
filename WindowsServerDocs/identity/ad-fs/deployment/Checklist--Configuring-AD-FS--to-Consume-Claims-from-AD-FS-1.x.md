---
ms.assetid: e7f9e518-2d5d-4a0d-9147-34e1304f42ac
title: 'Liste de vérification : configuration d’AD FS pour utiliser les revendications à partir d’AD FS 1.x'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: fe99487d3a770547af36f69722b442d0e2cbb8b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828290"
---
# <a name="checklist-configuring-ad-fs--to-consume-claims-from-ad-fs-1x"></a>Liste de vérification : Configurer AD FS pour utiliser les revendications à partir d’AD FS 1.x

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="checklist-configuring-ad-fs-to-consume-claims-from-adfs1x"></a>Liste de vérification : Configurer AD FS pour utiliser les revendications à partir d’AD FS 1.x  
Cette liste comprend les tâches qui sont nécessaires à la configuration de votre Active Directory Federation Services \(AD FS\) Service de fédération dans Windows Server 2012 d’utiliser les revendications qui sont envoyées par an AD FS 1. *x* Service de fédération.  
  
> [!NOTE]  
> Effectuez dans l’ordre les tâches répertoriées dans cette liste de vérification. Quand un lien de référence vous mène à une procédure, revenez à cette rubrique après avoir terminé les étapes décrites dans cette procédure afin que vous pouvez poursuivre les tâches restantes dans cette liste de vérification.  
  
![utiliser les revendications à partir d’AD FS](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**liste de vérification : Configurer AD FS pour utiliser les revendications à partir d’AD FS 1.x**  
  
||Tâche|Référence|  
|-|--------|-------------|  
|![utiliser les revendications à partir d’AD FS](media/icon_checkboxo.gif)|Planifiez l’interopérabilité entre AD FS dans Windows Server 2012 et les versions précédentes d’AD FS et en savoir que plus sur l’ID de nom de type de revendication.|![utiliser les revendications à partir d’AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planification de l’interopérabilité avec AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![utiliser les revendications à partir d’AD FS](media/icon_checkboxo.gif)|Avant que vous pouvez interagir avec une version antérieure d’AD FS, vous devez d’abord créer une approbation de fournisseur de revendications dans le Service de fédération AD FS. **Remarque :** Impossible de créer une relation d’approbation avec un AD FS 1. *x* Service de fédération à l’aide des métadonnées de fédération.<br /><br />Lorsque vous configurez l’approbation à l’aide de la procédure dans le lien vers la droite, vous devez effectuer les opérations suivantes dans l’Assistant Ajout revendications fournisseur confiance pour définir cette confiance pour interagir avec un AD FS 1. *x* Service de fédération :<br /><br />1.  Sur le **sélectionner une Source de données** , sélectionnez **entrer des données sur la partie de confiance manuellement confiance**.<br />2.  Sur le **choisir le profil** page, sélectionnez **profil ADFS 1.0 et 1.1**.<br />3.  Sur le **configurer l’URL** page sous **WS\-URL Passive Federation**, type la **URL de point de terminaison de Service de fédération** tel que défini dans les services AD FS 1. *x* Service de fédération du partenaire.<br />4.  Sur le **configurer les identificateurs** page sous **identifiant d’approbation de fournisseur de revendications**, type de la **URI du Service de fédération** tel que défini dans les services AD FS 1. *x* Service de fédération du partenaire.|![utiliser les revendications à partir d’AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[créer un revendications fournisseur approuver manuellement](../../ad-fs/operations/Create-a-Claims-Provider-Trust.md)|  
|![utiliser les revendications à partir d’AD FS](media/icon_checkboxo.gif)|Sur l’approbation de fournisseur de revendications que vous avez créé précédemment, vous devez créer une règle de revendication qui prendra les revendications entrantes à partir d’AD FS 1.x Federation Service et passer, de filtrer ou de les transformer en un type de revendication d’identificateur de nom.<br /><br />Lorsque le type de revendication de nom ID a été passé, filtrées ou transformées, il peut être utilisé en tant qu’entrée à une autre ou les règles afin qu’il peut être compris et consommée par le Service de fédération AD FS dans Windows Server 2012.|![utiliser les revendications à partir d’AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[créer une règle pour envoyer un AD FS 1.x revendication compatible avec](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![utiliser les revendications à partir d’AD FS](media/icon_checkboxo.gif)|Contactez l’administrateur de l’AD FS 1. *x* Service de fédération et que l’administrateur de l’AD FS 1. *x* Service de fédération configurer une nouvelle approbation de partenaire de ressource. En outre, fournir cet administrateur avec l’URI de Service de fédération \(dans les propriétés du Service de fédération\), l’URL de point de terminaison de Service de fédération et un jeton exporté\-fichier de certificat de signature \(avec clé publique uniquement\). L’administrateur a besoin de ces éléments pour configurer l’approbation.|N\/A|  
  

