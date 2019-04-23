---
ms.assetid: 4b81ac66-3f34-4a39-a8bf-5411131a69c2
title: 'Liste de vérification : configuration d’AD FS pour utiliser les revendications à partir d’AD FS 1.x'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: bdfd06a28f8ccaa04014024a235cd19512adb181
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887030"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>Liste de vérification : Configuration d’AD FS pour envoyer des revendications à un Service de fédération AD FS 1.x

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-federation-service"></a>Liste de vérification : Configurer AD FS pour envoyer des revendications à un AD FS 1.x Federation Service  
Cette liste comprend les tâches qui sont nécessaires à la configuration de votre Active Directory Federation Services \(AD FS\) Service de fédération dans Windows Server 2012 pour envoyer les revendications qui peuvent être reconnues par an AD FS 1. *x* Service de fédération.  
  
> [!NOTE]  
> Effectuez dans l’ordre les tâches répertoriées dans cette liste de vérification. Quand un lien de référence vous mène à une procédure, revenez à cette rubrique après avoir terminé les étapes décrites dans cette procédure afin que vous pouvez poursuivre les tâches restantes dans cette liste de vérification.  
  
![configurer AD FS pour envoyer des revendications](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**liste de vérification : Configurer AD FS pour envoyer des revendications à un AD FS 1.x Federation Service**  
  
||Tâche|Référence|  
|-|--------|-------------|  
|![configurer AD FS pour envoyer les revendications](media/icon_checkboxo.gif)|Planifiez l’interopérabilité entre AD FS dans Windows Server 2012 et les versions précédentes d’AD FS et en savoir que plus sur l’ID de nom de type de revendication.|![configurer AD FS pour envoyer des revendications](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planification de l’interopérabilité avec AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![configurer AD FS pour envoyer les revendications](media/icon_checkboxo.gif)|Avant d’obtenir l’interopérabilité avec une version antérieure d’AD FS, vous devez d’abord créer une partie de confiance dans le Service de fédération AD FS pour les services AD FS 1. *x* Service de fédération. **Remarque :** Impossible de créer une relation d’approbation avec un AD FS 1. *x* Service de fédération à l’aide des métadonnées de fédération.<br /><br />Lorsque vous configurez l’approbation à l’aide de la procédure dans le lien vers la droite, vous devez effectuer les opérations suivantes dans l’Assistant Ajout partie tiers confiance pour définir cette confiance pour interagir avec un AD FS 1. *x* Service de fédération :<br /><br />1.  Sur le **sélectionner une Source de données** , sélectionnez **entrer des données sur la partie de confiance manuellement confiance**.<br />2.  Sur le **choisir le profil** page, sélectionnez **profil ADFS 1.0 et 1.1**.<br />3.  Sur le **configurer l’URL** page sous **WS\-URL Passive Federation**, type la **URL de point de terminaison de Service de fédération** tel que défini dans les services AD FS 1. *x* Service de fédération du partenaire.<br />4.  Sur le **configurer les identificateurs** page sous **identifiant d’approbation de partie de partie de confiance**, type de la **URI du Service de fédération** tel que défini dans les services AD FS 1. *x* Service de fédération du partenaire.|![configurer AD FS pour envoyer des revendications](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[créer une partie de confiance manuellement](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![configurer AD FS pour envoyer les revendications](media/icon_checkboxo.gif)|Sur la partie de confiance que vous avez créé précédemment, vous devez créer revendication de type qui peut être comprise et consommée par la publicité de revendication de règles qui prennent les revendications entrantes qui ont été extraits à partir d’un magasin d’attributs et passer, filtrer ou les transformer en un ID de nom FS 1. *x* Service de fédération. **Remarque :** Avant de créer cette règle, assurez-vous que l’ensemble de règles de revendication dans lequel vous créez cette règle a une règle qui le précède qui extrait d’abord un Lightweight Directory Access Protocol \(LDAP\) revendication d’attribut à partir d’un magasin d’attributs. Cette revendication sera utilisée comme entrée pour la règle que vous créez pour envoyer un AD FS 1. *x*\-revendication compatible avec. Pour plus d’informations sur la création d’une règle pour extraire un attribut LDAP, consultez [créer une règle pour envoyer les attributs LDAP en tant que revendications](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configurer AD FS pour envoyer des revendications](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[créer une règle pour envoyer un AD FS 1.x revendication compatible avec](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![configurer AD FS pour envoyer les revendications](media/icon_checkboxo.gif)|Contactez l’administrateur de l’AD FS 1. *x* Service de fédération et que l’administrateur de l’AD FS 1. *x* Service de fédération configurer une nouvelle approbation de partenaire de compte. En outre, fournir cet administrateur avec l’URI de Service de fédération \(dans les propriétés du Service de fédération\), WS\-Federation passif URL de point de terminaison \(l’URL de point de terminaison de Service de fédération\), et un jeton exporté\-fichier de certificat de signature \(avec la clé publique uniquement\). Cet administrateur devra ces éléments pour configurer l’approbation.|N\/A|  
  

