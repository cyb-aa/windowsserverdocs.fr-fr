---
ms.assetid: 4b81ac66-3f34-4a39-a8bf-5411131a69c2
title: "Liste de vérification: configuration ADFS pour utiliser les revendications à partir des services ADFS 1.x"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: bdfd06a28f8ccaa04014024a235cd19512adb181
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>Liste de vérification: Configuration ADFS pour envoyer des revendications pour un Service de fédération ADFS 1.x

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012
  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>Liste de vérification: Configuration ADFS pour envoyer des revendications pour un ADFS 1.x Federation Service  
Cette liste comprend les tâches qui sont nécessaires à la configuration de votre Service de fédération de \(ADFS\) ActiveDirectory Federation Services dans Windows Server2012 pour envoyer des revendications qui peuvent être reconnues par an ADFS 1. *x* Service de fédération.  
  
> [!NOTE]  
> Effectuez les tâches de cette liste de vérification dans l’ordre. Lorsqu’un lien de référence vous redirige vers une procédure, revenez à cette rubrique après avoir effectué les étapes décrites dans cette procédure afin que vous pouvez poursuivre les tâches restantes de cette liste de vérification.  
  
![Configurer ADFS pour envoyer des revendications](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**liste de vérification: configuration ADFS pour envoyer des revendications pour un ADFS 1.x Federation Service**  
  
||Tâche|Référence|  
|-|--------|-------------|  
|![Configurer ADFS pour envoyer des revendications](media/icon_checkboxo.gif)|Planifier l’interopérabilité entre les services ADFS dans Windows Server2012 et les versions antérieures d’ADFS et en savoir que plus sur l’ID de nom de type de revendication.|![Configurer ADFS pour envoyer des revendications](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planification de l’interopérabilité avec ADFS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![Configurer ADFS pour envoyer des revendications](media/icon_checkboxo.gif)|Avant que vous pouvez obtenir l’interopérabilité avec une version précédente d’ADFS, vous devez d’abord créer une approbation de partie de confiance dans le Service de fédération ADFS pour ADFS 1. *x* Service de fédération. **Remarque:** vous ne pouvez pas créer une relation d’approbation avec un ADFS 1. *x* Service de fédération à l’aide des métadonnées de fédération.<br /><br />Lorsque vous configurez l’approbation à l’aide de la procédure dans le lien vers la droite, vous devez effectuer les opérations suivantes dans l’Assistant Ajout partie partie confiance pour définir cette relation d’approbation pour interagir avec un ADFS 1. *x* Service de fédération:<br /><br />1. Sur le **sélectionner une Source de données**, sélectionnez **entrer des données sur la partie de confiance manuellement confiance**.<br />2. Sur le **choisir le profil** page, sélectionnez **profil ADFS 1.0 et 1.1**.<br />3. Sur le **configurer l’URL** sous **URL Passive WS-Federation**, type la **URL de point de terminaison de Service de fédération** tel que défini dans ADFS 1. *x* Service de fédération du partenaire.<br />4. Sur le **configurer les identificateurs** sous **identificateur d’approbation de partie de confiance partie**, type la **URI du Service de fédération** tel que défini dans ADFS 1. *x* Service de fédération du partenaire.|![Configurer ADFS pour envoyer des revendications](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[créer une partie de confiance manuellement](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![Configurer ADFS pour envoyer des revendications](media/icon_checkboxo.gif)|Sur l’approbation que vous avez créé précédemment, vous devez créer la revendication règles qui seront prendre les revendications entrantes qui ont été extraits à partir d’un magasin d’attributs et passent par, filtrer ou transformer en un ID de nom revendication qui peuvent être comprises et consommée par le services ADFS 1. *x* Service de fédération. **Remarque:** avant de créer cette règle, assurez-vous que l’ensemble de règles de revendication où vous créez cette règle a une règle qui précède il qui extrait tout d’abord une revendication d’attribut Lightweight Directory Access Protocol \(LDAP\) à partir d’un magasin d’attributs. Cette revendication sera utilisée comme entrée pour la règle que vous créez pour envoyer un ADFS 1. *x*\-compatible revendication. Pour plus d’informations sur la création d’une règle pour extraire un attribut LDAP, voir [créer une règle pour envoyer les attributs LDAP en tant que revendications](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![Configurer ADFS pour envoyer des revendications](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[créer une règle pour envoyer un ADFS 1.x revendication compatible avec](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![Configurer ADFS pour envoyer des revendications](media/icon_checkboxo.gif)|Contactez l’administrateur d’ADFS 1. *x* Service de fédération et que l’administrateur de ADFS 1. *x* Service de fédération configurer une nouvelle approbation de partenaire de compte. En outre, fournir cet administrateur avec l’URI du Service de fédération \ (dans le Service de fédération properties\), l’URL de point de terminaison WS-Federation passif \ (le Service de fédération point de terminaison URL\), et une exporté token\-certificat de signature du fichier \ (avec only\ de clé publique). Cet administrateur devra ces éléments pour définir la relation d’approbation.|N\/A|  
  

