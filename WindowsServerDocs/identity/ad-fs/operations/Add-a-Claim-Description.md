---
ms.assetid: 7d230527-f4fe-4572-8838-0b354ee0b06b
title: Ajoutez une Description de revendication
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0e388ef656d3b690da62b077cb9f9e678a771e64
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-claim-description"></a>Ajoutez une Description de revendication

>S’applique à: Windows Server2016, Windows Server2012R2

Dans une organisation partenaire de compte, les administrateurs créent des revendications pour représenter l’appartenance d’un utilisateur dans un groupe ou un rôle ou pour représenter des données relatives à un utilisateur, par exemple, un employé numéro d’identification utilisateur.

Dans une organisation partenaire de ressource, les administrateurs créent revendications correspondantes pour représenter des groupes et utilisateurs qui peuvent être reconnues en tant qu’utilisateurs de ressource. Étant donné que sortant des revendications dans l’organigramme du partenaire de compte pour les revendications entrantes dans l’organisation partenaire de ressource, le partenaire de ressource est en mesure d’accepter les informations d’identification qui fournit le partenaire de compte. 

Vous pouvez utiliser la procédure suivante pour ajouter une revendication.

L’appartenance au groupe **administrateurs**, ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).

## <a name="to-add-a-claim-description"></a>Pour ajouter une description de revendication

1. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**. 

2.  Développez **Service** et sur le bouton droit sur **ajouter une Description de revendication**.
![Ajouter la description de revendication](media\Add-a-Claim-Description\claimdesc1.png)

3.  Sur l’ajout d’une Description de revendication la boîte de dialogue dans **nom d’affichage**, tapez un nom unique qui identifie le groupe ou le rôle de cette revendication.

4.  Ajouter un **nom abrégé**.

5.  Dans **identificateur de revendication**, tapez un URI qui est associé à ce groupe ou rôle de la revendication que vous utiliserez.

6.  Sous **Description**, tapez le texte qui décrit le mieux l’objectif de cette revendication.

7.  Selon les besoins de votre organisation, sélectionnez une des cases suivantes, selon le cas publier cette revendication dans les métadonnées de fédération:


    - Pour publier cette revendication prenant en charge que ce serveur peut accepter cette revendication partenaires, cliquez sur **publier cette revendication dans les métadonnées de fédération sous la forme d’un type de revendication que ce Service de fédération peut accepter**.
    - Pour publier cette revendication prenant en charge que ce serveur peut émettre cette revendication partenaires, cliquez sur **publier cette revendication dans les métadonnées de fédération sous la forme d’un type de revendication que ce Service de fédération peut envoyer**.

8.  Cliquez sur **OK**.

![Ajouter la description de revendication](media\Add-a-Claim-Description\claimdesc2.png)

  
## <a name="see-also"></a>Voir aussi  
[Opérations ADFS](../../ad-fs/AD-FS-2016-Operations.md) 
