---
ms.assetid: e7f9e518-2d5d-4a0d-9147-34e1304f42ac
title: "Liste de vérification: configuration ADFS pour utiliser les revendications à partir des services ADFS 1.x"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: fe99487d3a770547af36f69722b442d0e2cbb8b1
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="checklist-configuring-ad-fs--to-consume-claims-from-ad-fs-1x"></a>Liste de vérification: Configuration ADFS pour utiliser les revendications à partir des services ADFS 1.x

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012
  
## <a name="checklist-configuring-ad-fs-to-consume-claims-from-ad-fs-1x"></a>Liste de vérification: Configuration ADFS pour utiliser les revendications à partir des services ADFS 1.x  
Cette liste comprend les tâches qui sont nécessaires à la configuration de votre Service de fédération de \(ADFS\) ActiveDirectory Federation Services dans Windows Server2012 pour utiliser les revendications qui sont envoyées par un ADFS 1. *x* Service de fédération.  
  
> [!NOTE]  
> Effectuez les tâches de cette liste de vérification dans l’ordre. Lorsqu’un lien de référence vous redirige vers une procédure, revenez à cette rubrique après avoir effectué les étapes décrites dans cette procédure afin que vous pouvez poursuivre les tâches restantes de cette liste de vérification.  
  
![Utiliser les revendications à partir des services ADFS](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**liste de vérification: configuration ADFS pour utiliser les revendications à partir des services ADFS 1.x**  
  
||Tâche|Référence|  
|-|--------|-------------|  
|![Utiliser les revendications à partir des services ADFS](media/icon_checkboxo.gif)|Planifier l’interopérabilité entre les services ADFS dans Windows Server2012 et les versions antérieures d’ADFS et en savoir que plus sur l’ID de nom de type de revendication.|![Utiliser les revendications à partir des services ADFS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planification de l’interopérabilité avec ADFS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![Utiliser les revendications à partir des services ADFS](media/icon_checkboxo.gif)|Avant que vous pouvez interagir avec une version précédente d’ADFS, vous devez d’abord créer une approbation de fournisseur de revendications dans le Service de fédération ADFS. **Remarque:** vous ne pouvez pas créer une relation d’approbation avec un ADFS 1. *x* Service de fédération à l’aide des métadonnées de fédération.<br /><br />Lorsque vous configurez l’approbation à l’aide de la procédure dans le lien vers la droite, vous devez effectuer les opérations suivantes dans l’Assistant Ajout revendications fournisseur de confiance pour définir cette relation d’approbation pour interagir avec un ADFS 1. *x* Service de fédération:<br /><br />1. Sur le **sélectionner une Source de données**, sélectionnez **entrer des données sur la partie de confiance manuellement confiance**.<br />2. Sur le **choisir le profil** page, sélectionnez **profil ADFS 1.0 et 1.1**.<br />3. Sur le **configurer l’URL** sous **URL Passive WS-Federation**, type la **URL de point de terminaison de Service de fédération** tel que défini dans ADFS 1. *x* Service de fédération du partenaire.<br />4. Sur le **configurer les identificateurs** page, sous **identificateur d’approbation de fournisseur de revendications**, tapez le **URI du Service de fédération** tel que défini dans ADFS 1. *x* Service de fédération du partenaire.|![Utiliser les revendications à partir des services ADFS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[créer un revendications fournisseur approuver manuellement](../../ad-fs/operations/Create-a-Claims-Provider-Trust.md)|  
|![Utiliser les revendications à partir des services ADFS](media/icon_checkboxo.gif)|Sur l’approbation de fournisseur de revendications que vous avez créé précédemment, vous devez créer une règle de revendication qui prendra les revendications entrantes à partir de la ADFS 1.x Federation Service et transmettre, filtrer, ou les transformer en un type de revendication d’identificateur de nom.<br /><br />Lorsque le type de revendication d’identificateur de nom a été passé, filtrés ou transformer, il peut être utilisé en tant qu’entrée à un autre ou les règles afin qu’il peut être compris et consommée par le Service de fédération ADFS dans Windows Server2012.|![Utiliser les revendications à partir des services ADFS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[créer une règle pour envoyer un ADFS 1.x revendication compatible avec](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![Utiliser les revendications à partir des services ADFS](media/icon_checkboxo.gif)|Contactez l’administrateur d’ADFS 1. *x* Service de fédération et que l’administrateur de ADFS 1. *x* Service de fédération configurer une nouvelle approbation de partenaire de ressource. En outre, fournir cet administrateur avec l’URI du Service de fédération \ (dans le Service de fédération properties\), l’URL de point de terminaison du Service de fédération et une exporté token\ signature de fichier de certificat \ (avec only\ de clé publique). L’administrateur devra ces éléments pour définir la relation d’approbation.|N\/A|  
  

