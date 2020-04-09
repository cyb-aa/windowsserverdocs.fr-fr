---
ms.assetid: c60227a8-7b44-40f8-b807-a6532851a4a6
title: Ajouter un magasin d'attributs
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b197268de3c5335e30c2a24a74c5a7b01224b500
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859692"
---
# <a name="add-an-attribute-store"></a>Ajouter un magasin d'attributs


Les comptes d’utilisateur et les comptes d’ordinateur qui requièrent l’accès à une ressource protégée par Services ADFS \(AD FS\) sont stockés dans un magasin d’attributs, par exemple Active Directory Domain Services \(AD DS\). Le moteur d’émission de revendications utilise des magasins d’attributs pour collecter les données nécessaires à l’émission de revendications. Les données des magasins d’attributs sont ensuite projetées en tant que revendications.  
  
Vous pouvez utiliser la procédure suivante pour ajouter un magasin d’attributs au service FS (Federation Service).  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs** ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-an-attribute-store"></a>Pour ajouter un magasin d’attributs  
  
1.  Ouvrez la **gestion des AD FS**.  
  
2.  Sous **actions** , cliquez sur **Ajouter un magasin d’attributs**.  

![Ajouter un magasin d’attributs](media/Add-an-Attribute-Store/addstore1.PNG)
  
3. Dans la boîte de dialogue **Ajouter un magasin d’attributs** , configurez les propriétés suivantes pour le magasin d’attributs que vous souhaitez ajouter :  
  
   -   Dans **nom d’affichage**, tapez le nom que vous souhaitez utiliser pour identifier le magasin d’attributs.  
  
   -   Dans **type de magasin d’attributs**, sélectionnez un type de magasin d’attributs pris en charge, **Active Directory**, **LDAP**ou **SQL**.  
  
   -   Dans **chaîne de connexion**, si vous avez sélectionné un protocole LDAP (Lightweight Directory Access Protocol) \(le magasin LDAP\) ou un magasin langage SQL \(SQL\), entrez la chaîne que vous avez utilisée pour établir une connexion au magasin d’attributs. Pour les magasins d’attributs Active Directory, aucune chaîne de connexion n’est nécessaire ; par conséquent, ce champ est désactivé.  
  
       > [!NOTE]  
       > Par défaut, AD FS crée automatiquement un magasin d'attributs Active Directory.  
 
![Ajouter un magasin d’attributs](media/Add-an-Attribute-Store/addstore2.PNG) 

4. Cliquez sur **OK**.  
  
## <a name="additional-references"></a>Références supplémentaires  

[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md)
  
[Rôle des magasins d’attributs](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)  
