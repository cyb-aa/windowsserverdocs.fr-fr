---
ms.assetid: 7d230527-f4fe-4572-8838-0b354ee0b06b
title: Ajouter une description de revendication
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 454d261aa520778a6129ac9809f53894937b036a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190142"
---
# <a name="add-a-claim-description"></a>Ajouter une description de revendication


Dans une organisation partenaire de compte, les administrateurs créent des revendications pour représenter l’appartenance d’un utilisateur dans un groupe ou un rôle ou pour représenter des données relatives à un utilisateur, par exemple, un employé numéro d’identification utilisateur.

Dans une organisation partenaire de ressource, les administrateurs créent des revendications correspondantes pour représenter des groupes et utilisateurs qui peuvent être identifiés en tant qu’utilisateurs de ressources. Étant donné que sortant de revendications dans l’organigramme du partenaire de compte pour les revendications entrantes dans l’organisation partenaire de ressource, le partenaire de ressource est en mesure d’accepter les informations d’identification fournies par le partenaire de compte. 

Vous pouvez utiliser la procédure suivante pour ajouter une revendication.

Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).

## <a name="to-add-a-claim-description"></a>Pour ajouter une description de revendication

1. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**. 

2.  Développez **Service** et sur le bouton droit sur **ajouter une Description de revendication**.
![add claim description](media\Add-a-Claim-Description\claimdesc1.png)

3.  Sur l’ajout d’un Description de revendication boîte de dialogue **nom d’affichage**, tapez un nom unique qui identifie le groupe ou du rôle de cette revendication.

4.  Ajouter un **nom abrégé**.

5.  Dans **revendication d’identificateur**, tapez un URI qui est associé avec le groupe ou du rôle de la revendication que vous souhaitez utiliser.

6.  Sous **Description**, tapez le texte qui décrit le mieux l’objectif de cette revendication.

7.  Selon les besoins de votre organisation, sélectionnez une des cases à cocher suivantes, si nécessaire, pour publier cette revendication dans les métadonnées de fédération :


    - Pour publier cette revendication partenaires prenant en charge que ce serveur peut recevoir cette revendication, cliquez sur **publier cette revendication dans les métadonnées de fédération en tant que type de revendication ce Service de fédération peut accepter**.
    - Pour publier cette revendication partenaires prenant en charge que ce serveur peut émettre cette revendication, cliquez sur **publier cette revendication dans les métadonnées de fédération comme un type de revendication que ce Service de fédération peut envoyer**.

8.  Cliquez sur **OK**.

![Ajouter une description de revendication](media\Add-a-Claim-Description\claimdesc2.png)

  
## <a name="see-also"></a>Voir aussi  
[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
