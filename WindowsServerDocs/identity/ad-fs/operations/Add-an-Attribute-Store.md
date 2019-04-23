---
ms.assetid: c60227a8-7b44-40f8-b807-a6532851a4a6
title: Ajouter un magasin d'attributs
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 11baba5bfdb699f120a506feb8361db21d26cff1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837860"
---
# <a name="add-an-attribute-store"></a>Ajouter un magasin d'attributs

>S'applique à : Windows Server 2016, Windows Server 2012 R2

Comptes d’utilisateurs et comptes d’ordinateurs qui requièrent l’accès à une ressource qui est protégée par Active Directory Federation Services \(AD FS\) sont stockés dans un magasin d’attributs, tels que les Services de domaine Active Directory \(AD DS \). Le moteur d’émission de revendications utilise les magasins d’attributs pour collecter des données qui sont nécessaires d’émettre des revendications. Données à partir de magasins d’attributs sont ensuite projetées en tant que revendications.  
  
Vous pouvez utiliser la procédure suivante pour ajouter un magasin d’attributs pour le Service de fédération.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-an-attribute-store"></a>Pour ajouter un magasin d’attributs  
  
1.  Ouvrez **gestion AD FS**.  
  
2.  Sous **Actions** cliquez sur **ajouter un magasin d’attributs**.  

![ajouter le magasin d’attributs](media/Add-an-Attribute-Store/addstore1.PNG)
  
3.  Dans le **ajouter un magasin d’attributs** boîte de dialogue zone, configurez les propriétés suivantes pour le magasin d’attributs que vous souhaitez ajouter :  
  
    -   Dans **nom d’affichage**, tapez le nom que vous souhaitez utiliser pour identifier le magasin d’attributs.  
  
    -   Dans **type de magasin d’attribut**, sélectionnez un type de magasin d’attribut prises en charge, à savoir **Active Directory**, **LDAP**, ou **SQL**.  
  
    -   Dans **chaîne de connexion**, si vous avez sélectionné soit Lightweight Directory Access Protocol \(LDAP\) magasin ou un langage de requête structuré \(SQL\) stocker, entrez la chaîne que vous avez utilisé pour établir une connexion au magasin d’attributs. Pour les magasins d’attributs Active Directory, aucune chaîne de connexion n’est nécessaire ; Par conséquent, ce champ est désactivé.  
  
        > [!NOTE]  
        > Par défaut, AD FS crée automatiquement un magasin d'attributs Active Directory.  
 
![ajouter le magasin d’attributs](media/Add-an-Attribute-Store/addstore2.PNG) 

4.  Cliquez sur **OK**.  
  
## <a name="additional-references"></a>Références supplémentaires  

[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md)
  
[Le rôle des magasins d’attributs](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)  
