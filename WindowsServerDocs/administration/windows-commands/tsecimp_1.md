---
title: tsecimp
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d7488ec6-0eff-45ff-89ee-9cbe752416bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c596d6d24a611882c0ecf234c22c83a268ec53c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363929"
---
# <a name="tsecimp"></a>tsecimp



Importe les informations d’affectation d’un fichier Extensible Markup Language (XML) dans le fichier de sécurité du serveur TAPI (sect. ini). Vous pouvez également utiliser cette commande pour afficher la liste des fournisseurs TAPI et les périphériques de ligne associés à chacun d’eux, valider la structure du fichier XML sans importer le contenu et vérifier l’appartenance au domaine.

## <a name="syntax"></a>Syntaxe

```
tsecimp /f <Filename> [{/v | /u}]
tsecimp /d
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/f \<nom de fichier >|Obligatoire. Spécifie le nom du fichier XML qui contient les informations d’affectation que vous souhaitez importer.|
|/v|Valide la structure du fichier XML sans importer les informations dans le fichier sect. ini.|
|/u.|Vérifie si chaque utilisateur est membre du domaine spécifié dans le fichier XML. L’ordinateur sur lequel vous utilisez ce paramètre doit être connecté au réseau. Ce paramètre peut ralentir considérablement les performances si vous traitez une grande quantité d’informations d’attribution de l’utilisateur.|
|/d|Affiche la liste des fournisseurs de téléphonie installés. Pour chaque fournisseur de téléphonie, les périphériques de ligne associés sont répertoriés, ainsi que les adresses et les utilisateurs associés à chaque périphérique de ligne.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Le fichier XML à partir duquel vous souhaitez importer les informations d’attribution doit suivre la structure décrite ci-dessous.  
    -   Élément **UserList**

        La **UserList** est l’élément supérieur du fichier XML.
    -   Élément **utilisateur**

        Chaque élément **utilisateur** contient des informations sur un utilisateur qui est membre d’un domaine. Un ou plusieurs périphériques de ligne peuvent être affectés à chaque utilisateur.

        En outre, chaque élément **utilisateur** peut avoir un attribut nommé **NoMerge**. Lorsque cet attribut est spécifié, toutes les attributions de périphériques de ligne en cours pour l’utilisateur sont supprimées avant que d’autres ne soient effectuées. Vous pouvez utiliser cet attribut pour supprimer facilement les affectations d’utilisateurs indésirables. Par défaut, cet attribut n’est pas défini.

        L’élément **User** doit contenir un seul élément **DomainUserName** , qui spécifie le domaine et le nom d’utilisateur de l’utilisateur. L’élément **User** peut également contenir un seul élément **FriendlyName** , qui spécifie un nom convivial pour l’utilisateur.

        L’élément **User** peut contenir un élément **LineList** . Si un élément **LineList** n’est pas présent, tous les périphériques de ligne de cet utilisateur sont supprimés.
    -   Élément **LineList**

        L’élément **LineList** contient des informations sur chaque ligne ou périphérique pouvant être attribué à l’utilisateur. Chaque élément **LineList** peut contenir plusieurs éléments de **ligne** .
    -   Élément **line**

        Chaque élément de **ligne** spécifie un périphérique de ligne. Vous devez identifier chaque périphérique de ligne en ajoutant un élément **Address** ou un élément **PermanentID** sous l’élément **line** .

        Pour chaque élément de **ligne** , vous pouvez définir l’attribut **Remove** . Si vous définissez cet attribut, l’utilisateur n’est plus affecté à ce périphérique de ligne. Si cet attribut n’est pas défini, l’utilisateur obtient l’accès à ce périphérique de ligne. Aucune erreur n’est indiquée si le périphérique de ligne n’est pas disponible pour l’utilisateur.

## <a name="examples"></a>Exemples
- Les exemples de segments de code XML suivants illustrent l’utilisation correcte des éléments définis ci-dessus.  
  - Le code suivant supprime tous les appareils de ligne affectés à User1.  
    ```
    <UserList>
      <User NoMerge="1">
        <DomainUser>domain1\user1</DomainUser>
      </User>
    </UserList>
    ```  
  - Le code suivant supprime tous les appareils de ligne affectés à User1 avant d’attribuer une ligne avec l’adresse 99999. Utilisateur1 n’aura pas d’autres lignes attribuées, que des appareils en ligne aient été attribués précédemment.  
    ```
    <UserList>
      <User NoMerge="1">
        <DomainUser>domain1\user1</DomainUser>
        <FriendlyName>User1</FriendlyName>
        <LineList>
          <Line>
            <Address>99999</Address>
          </Line>
        </LineList>
      </User>
    </UserList>
    ```  
  - Le code suivant ajoute un périphérique de ligne pour User1 sans supprimer les périphériques de ligne précédemment affectés.  
    ```
    <UserList>
      <User>
        <DomainUser>domain1\user1</DomainUser>
        <FriendlyName>User1</FriendlyName>
        <LineList>
          <Line>
            <Address>99999</Address>
          </Line>
        </LineList>
      </User>
    </UserList>
    ```  
  - Le code suivant ajoute l’adresse de ligne 99999 et supprime l’accès à l’adresse de ligne 88888 de utilisateur1.  
    ```
    <UserList>
      <User>
        <DomainUser>domain1\user1</DomainUser>
        <FriendlyName>User1</FriendlyName>
        <LineList>
          <Line>
            <Address>99999</Address>
          </Line>
          <Line Remove="1">
            <Address>88888</Address>
          </Line>
        </LineList>
      </User>
    </UserList>
    ```  
  - Le code suivant ajoute l’appareil permanent 1000 et supprime la ligne 88888 de l’accès à utilisateur1.  
    ```
    <UserList>
      <User>
        <DomainUser>domain1\user1</DomainUser>
        <FriendlyName>User1</FriendlyName>
        <LineList>
          <Line>
            <PermanentID>1000</PermanentID>
          </Line>
          <Line Remove="1">
            <Address>88888</Address>
          </Line>
        </LineList>
      </User>
    </UserList>
    ```

-   L’exemple de sortie suivant apparaît une fois que l’option de ligne de commande **/d** est spécifiée pour afficher la configuration TAPI actuelle. Pour chaque fournisseur de téléphonie, les périphériques de ligne associés sont répertoriés, ainsi que les adresses et les utilisateurs associés à chaque périphérique de ligne.  
    ```
    NDIS Proxy TAPI Service Provider
            Line: "WAN Miniport (L2TP)"
                    Permanent ID: 12345678910

    NDIS Proxy TAPI Service Provider
            Line: "LPT1DOMAIN1\User1"
                    Permanent ID: 12345678910

    Microsoft H.323 Telephony Service Provider
            Line: "H323 Line"
                    Permanent ID: 123456
                    Addresses:
                            BLDG1-TAPI32

    ```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Vue d’ensemble de l’interface de commande](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)
