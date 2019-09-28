---
ms.assetid: 7d230527-f4fe-4572-8838-0b354ee0b06b
title: Ajouter une description de revendication
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ff50ac8d41a5bbde282b1d5b93c85610f841b5ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407789"
---
# <a name="add-a-claim-description"></a>Ajouter une description de revendication


Dans une organisation partenaire de compte, les administrateurs créent des revendications pour représenter l’appartenance d’un utilisateur à un groupe ou à un rôle, ou pour représenter des données relatives à un utilisateur, par exemple, le numéro d’identification d’un utilisateur.

Dans une organisation partenaire de ressource, les administrateurs créent des revendications correspondantes pour représenter les groupes et les utilisateurs qui peuvent être reconnus en tant qu’utilisateurs de ressources. Étant donné que les revendications sortantes dans l’organisation partenaire de compte sont mappées aux revendications entrantes dans l’organisation partenaire de ressource, le partenaire de ressource est en mesure d’accepter les informations d’identification fournies par le partenaire de compte. 

Vous pouvez utiliser la procédure suivante pour ajouter une revendication.

Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).

## <a name="to-add-a-claim-description"></a>Pour ajouter une description de revendication

1. Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**. 

2. Développez **service** , puis cliquez avec le bouton droit sur **Ajouter une description de revendication**.
   ![Ajouter une description de revendication](media/Add-a-Claim-Description/claimdesc1.png)

3. Dans la boîte de dialogue Ajouter une description de revendication, dans **nom complet**, tapez un nom unique qui identifie le groupe ou le rôle de cette revendication.

4. Ajoutez un **nom abrégé**.

5. Dans **identificateur de revendication**, tapez un URI associé au groupe ou au rôle de la revendication que vous allez utiliser.

6. Sous **Description**, tapez le texte qui décrit le mieux l’objectif de cette revendication.

7. En fonction des besoins de votre organisation, activez l’une des cases à cocher suivantes, le cas échéant, pour publier cette revendication dans les métadonnées de Fédération :


~~~
- To publish this claim to make partners aware that this server can accept this claim, click **Publish this claim in federation metadata as a claim type that this Federation Service can accept**.
- To publish this claim to make partners aware that this server can issue this claim, click **Publish this claim in federation metadata as a claim type that this Federation Service can send**.
~~~

8. Cliquez sur **OK**.

![Ajouter une description de revendication](media/Add-a-Claim-Description/claimdesc2.png)


## <a name="see-also"></a>Voir aussi  
[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
