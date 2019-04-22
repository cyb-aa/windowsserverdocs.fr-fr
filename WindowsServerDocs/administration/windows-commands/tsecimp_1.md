---
title: tsecimp
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 38582706dfa5db2b5069415b81dafc533c8a89b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822100"
---
# <a name="tsecimp"></a>tsecimp



Importe des informations sur l’attribution d’un fichier de langage XML (Extensible Markup) dans le fichier de sécurité du serveur TAPI (Tsec.ini). Vous pouvez également utiliser cette commande pour afficher la liste des fournisseurs TAPI et les appareils de lignes associé à chacun d’eux, valider la structure du fichier XML sans importer le contenu et vérifier l’appartenance au domaine.

## <a name="syntax"></a>Syntaxe

```
tsecimp /f <Filename> [{/v | /u}]
tsecimp /d
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/f \<Filename>|Obligatoire. Spécifie le nom du fichier XML qui contient les informations d’affectation que vous souhaitez importer.|
|/v|Valide la structure du fichier XML sans importer les informations dans le fichier Tsec.ini.|
|/u|Vérifie si chaque utilisateur est membre du domaine spécifié dans le fichier XML. L’ordinateur sur lequel vous utilisez ce paramètre doit être connecté au réseau. Ce paramètre peut ralentir considérablement les performances si vous traitez une grande quantité d’informations sur l’affectation utilisateur.|
|/d|Affiche une liste de fournisseurs de téléphonie installés. Pour chaque fournisseur de téléphonie, les périphériques de ligne associés sont répertoriés, ainsi que les adresses et les utilisateurs associés à chaque périphérique de ligne.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Le fichier XML à partir de laquelle vous souhaitez importer des informations sur l’attribution doit respecter la structure décrite ci-dessous.  
    -   **UserList** élément

        Le **UserList** est l’élément supérieur du fichier XML.
    -   **Utilisateur** élément

        Chaque **utilisateur** élément contient des informations sur un utilisateur qui est membre d’un domaine. Chaque utilisateur peut être affecté à un ou plusieurs périphériques de ligne.

        En outre, chaque **utilisateur** élément peut avoir un attribut nommé **NoMerge**. Lorsque cet attribut est spécifié, toutes les attributions de périphérique de ligne actuel pour l’utilisateur sont supprimées avant que d’autres sont effectuées. Vous pouvez utiliser cet attribut pour supprimer facilement des attributions d’utilisateur indésirables. Par défaut, cet attribut n’est pas défini.

        Le **utilisateur** élément doit contenir un seul **DomainUserName** élément, qui spécifie le nom de domaine et d’utilisateur de l’utilisateur. Le **utilisateur** élément peut également contenir un **FriendlyName** élément, qui spécifie un nom convivial pour l’utilisateur.

        Le **utilisateur** élément peut contenir un **LineList** élément. Si un **LineList** élément n’est pas présent, tous les périphériques de ligne pour cet utilisateur sont supprimées.
    -   **LineList** élément

        Le **LineList** élément contient des informations sur chaque ligne ou périphérique qui peut être attribué à l’utilisateur. Chaque **LineList** élément peut contenir plusieurs **ligne** élément.
    -   **Ligne** élément

        Chaque **ligne** élément spécifie un périphérique de ligne. Vous devez identifier chaque périphérique de ligne en ajoutant soit un **adresse** élément ou un **PermanentID** élément sous le **ligne** élément.

        Pour chaque **ligne** élément, vous pouvez définir le **supprimer** attribut. Si vous définissez cet attribut, l’utilisateur n’est plus affectée cet appareil de la ligne. Si cet attribut n’est pas défini, l’utilisateur accède à ce périphérique de ligne. Aucune erreur n’est générée si le périphérique de ligne n’est pas disponible à l’utilisateur.

## <a name="examples"></a>Exemples
-   Les segments de code XML exemples suivants illustrent l’utilisation correcte des éléments définis ci-dessus.  
    -   Le code suivant supprime tous les périphériques de ligne affectés à Utilisateur1.  
        ```
        <UserList>
          <User NoMerge="1">
            <DomainUser>domain1\user1</DomainUser>
          </User>
        </UserList>
        ```  
    -   Le code suivant supprime tous les périphériques de ligne affectés à Utilisateur1 avant d’affecter une ligne avec l’adresse 99999. User1 n’aura aucun autre périphérique de lignes affectées, quelle que soit si des périphériques de ligne ont été attribués précédemment.  
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
    -   Le code suivant ajoute un périphérique de ligne pour User1 sans supprimer les périphériques de la ligne précédemment affectés.  
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
    -   Le code suivant ajoute l’adresse de ligne 99999 et supprime l’adresse de ligne 88888 contre tout accès utilisateur1.  
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
    -   Le code suivant ajoute le périphérique permanent 1000 et supprime la ligne 88888 contre tout accès utilisateur1.  
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
-   L’exemple de sortie suivant s’affiche après le **/d** option de ligne de commande est spécifiée pour afficher la configuration de l’interface TAPI actuelle. Pour chaque fournisseur de téléphonie, les périphériques de ligne associés sont répertoriés, ainsi que les adresses et les utilisateurs associés à chaque périphérique de ligne.  
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

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

[Vue d’ensemble du shell de commande](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)