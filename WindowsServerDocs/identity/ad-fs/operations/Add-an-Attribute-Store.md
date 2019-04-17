---
ms.assetid: c60227a8-7b44-40f8-b807-a6532851a4a6
title: "Ajouter un magasin d’attributs"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 11baba5bfdb699f120a506feb8361db21d26cff1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="add-an-attribute-store"></a>Ajouter un magasin d’attributs

>S’applique à: Windows Server2016, Windows Server2012R2

Comptes d’utilisateur et les comptes d’ordinateurs qui nécessitent un accès à une ressource qui est protégé par ActiveDirectory Federation Services \(ADFS\) sont stockés dans un magasin d’attributs, tels que les Services de domaine ActiveDirectory \(ADDS\). Le moteur d’émission des revendications utilise les magasins d’attributs à collecter des données qui sont nécessaires pour émettre des revendications. Les données à partir de magasins d’attributs sont ensuite projetées en tant que revendications.  
  
Vous pouvez utiliser la procédure suivante pour ajouter un magasin d’attributs au Service de fédération.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-an-attribute-store"></a>Pour ajouter un magasin d’attributs  
  
1.  Ouvrez **gestion ADFS**.  
  
2.  Sous **Actions** cliquez sur **ajouter un magasin d’attributs**.  

![Ajouter le magasin d’attributs](media/Add-an-Attribute-Store/addstore1.PNG)
  
3.  Dans le **ajouter un magasin d’attributs** boîte de dialogue zone, configurer les propriétés suivantes pour le magasin d’attributs que vous souhaitez ajouter:  
  
    -   Dans **nom d’affichage**, tapez le nom que vous souhaitez utiliser pour identifier le magasin d’attributs.  
  
    -   Dans **type d’attribut store**, sélectionnez un type de magasin d’attributs pris en charge, soit **ActiveDirectory**, **LDAP**, ou **SQL**.  
  
    -   Dans **chaîne de connexion**, si vous avez sélectionné un magasin \(LDAP\) Lightweight Directory Access Protocol ou un magasin \(SQL\) Structured Query Language, entrez la chaîne que vous avez utilisé pour établir une connexion vers le magasin d’attributs. Pour les magasins d’attributs ActiveDirectory, aucune chaîne de connexion n’est nécessaire; par conséquent, ce champ est désactivé.  
  
        > [!NOTE]  
        > ADFS crée automatiquement un magasin d’attributs ActiveDirectory, par défaut.  
 
![Ajouter le magasin d’attributs](media/Add-an-Attribute-Store/addstore2.PNG) 

4.  Cliquez sur **OK**.  
  
## <a name="additional-references"></a>Références supplémentaires  

[Opérations ADFS](../../ad-fs/AD-FS-2016-Operations.md)
  
[Le rôle des magasins d’attributs](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)  
