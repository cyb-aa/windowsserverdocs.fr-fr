---
title: ktpass
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47087676-311e-41f1-8414-199740d01444
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a865b94bbbf2245e8a2e07638064f66de920b90
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886670"
---
# <a name="ktpass"></a>ktpass

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configure le nom du serveur principal de l’hôte ou le service dans active directory Domain Services (AD DS) et génère un fichier .keytab qui contient la clé secrète partagée du service. Ce fichier .keytab est basé sur l’implémentation par le MIT (Massachusetts Institute of Technology) du protocole d’authentification Kerberos. L’outil de ligne de commande ktpass permet de services non Windows qui prennent en charge l’authentification Kerberos pour utiliser les fonctionnalités d’interopérabilité fournies par le service Centre de Distribution de clés Kerberos (KDC). Cette rubrique s’applique aux versions de système d’exploitation désignées dans la **s’applique à** figurant au début de la rubrique.  
  
## <a name="syntax"></a>Syntaxe  
```  
ktpass  
[/out <FileName>]   
[/princ <PrincipalName>]   
[/mapuser <UserAccount>]   
[/mapop {add|set}] [{-|+}desonly] [/in <FileName>]  
[/pass {Password|*|{-|+}rndpass}]  
[/minpass]  
[/maxpass]  
[/crypto {DES-CBC-CRC|DES-CBC-MD5|RC4-HMAC-NT|AES256-SHA1|AES128-SHA1|All}]  
[/itercount]  
[/ptype {KRB5_NT_PRINCIPAL|KRB5_NT_SRV_INST|KRB5_NT_SRV_HST}]  
[/kvno <KeyversionNum>]  
[/answer {-|+}]  
[/target]  
[/rawsalt] [{-|+}dumpsalt] [{-|+}setupn] [{-|+}setpass <Password>]  [/?|/h|/help]  
```  
## <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|/ out <FileName>|Spécifie le nom du fichier Kerberos version 5 .keytab à générer. **Remarque :** Voici le fichier .keytab qui transférer vers un ordinateur qui n’exécute pas le système d’exploitation Windows, puis remplacer ou fusionner avec votre fichier .keytab /Etc/Krb5.keytab.|  
|/princ <PrincipalName>|Spécifie le nom principal sous la forme host/computer.contoso.com@CONTOSO.COM. **Avertissement :** Ce paramètre est sensible à la casse. Consultez [remarques](#BKMK_remarks) pour plus d’informations.|  
|/mapuser <UserAccount>|Mappe le nom de l’entité de sécurité Kerberos, qui est spécifiée par le **princ** paramètre, pour le compte de domaine spécifié.|  
|/mapop {ajouter&#124;définir}|Spécifie la façon dont l’attribut de mappage est défini.<br /><br />-   **ajouter** ajoute la valeur du nom d’utilisateur local spécifié. Il s’agit de l’option par défaut.<br />-   **Définissez** définit la valeur pour norme DES (Data Encryption)-uniquement le chiffrement pour le nom d’utilisateur local spécifié.|  
|{-&#124;+}desonly|Le chiffrement DES uniquement est défini par défaut.<br /><br />-   **+** Définit un compte pour le chiffrement DES uniquement.<br />-   **-** Restriction des versions sur un compte pour le chiffrement DES uniquement. **IMPORTANT :** Depuis Windows 7 et Windows Server 2008 R2, Windows ne prend pas en charge par défaut.|  
|/in <FileName>|Spécifie le fichier .keytab pour lire à partir d’un ordinateur hôte qui n’exécute pas le système d’exploitation Windows.|  
|/ passer {mot de passe&#124;*&#124;{-&#124;+} rndpass}|Spécifie un mot de passe pour le nom d’utilisateur principal qui est spécifié par le **princ** paramètre. Utilisez « * » pour demander un mot de passe.|  
|/minpass|Définit la longueur minimale du mot de passe aléatoire et 15 caractères.|  
|/maxpass|Définit la longueur maximale du mot de passe aléatoire à 256 caractères.|  
|/crypto {DES-CBC-CRC&#124;DES-CBC-MD5&#124;RC4-HMAC-NT&#124;AES256-SHA1&#124;AES128-SHA1&#124;All}|Spécifie les clés qui sont générés dans le fichier keytab :<br /><br />-   **DES-CBC-CRC** est utilisé pour la compatibilité.<br />-   **DES-CBC-MD5** adhère plus étroitement à l’implémentation de MIT et est utilisé pour la compatibilité.<br />-   **RC4-HMAC-NT** utilise le chiffrement 128 bits.<br />-   **AES256-SHA1** utilise le chiffrement AES256-CTS-HMAC-SHA1-96.<br />-   **Aes128-SHA1** utilise le chiffrement AES128-CTS-HMAC-SHA1-96.<br />-   **Tous les** états que toutes prises en charge les types de chiffrement peuvent être utilisées. **Remarque :** Les paramètres par défaut sont basés sur les anciennes versions MIT. Par conséquent, `/crypto` doit toujours être spécifié.|  
|/itercount|Spécifie le nombre d’itérations qui est utilisé pour le chiffrement AES. La valeur par défaut est que **itercount** est ignoré pour le chiffrement de non-AES et définie à 4 096 pour le chiffrement AES.|  
|/ptype {KRB5_NT_PRINCIPAL&#124;KRB5_NT_SRV_INST&#124;KRB5_NT_SRV_HST}|Spécifie le type de principal.<br /><br />-   **KRB5_NT_PRINCIPAL** est le type d’entité de sécurité général (recommandé).<br />-   **KRB5_NT_SRV_INST** est l’instance de service utilisateur.<br />-   **KRB5_NT_SRV_HST** est l’instance du service hôte.|  
|/kvno <KeyversionNum>|Spécifie le numéro de version de la clé. La valeur par défaut est 1.|  
|/ réponses {-&#124;+}|Définit le mode de réponse en arrière-plan :<br /><br />**-** Réponses réinitialiser des invites de mot de passe automatiquement à non.<br /><br />**+** Réponses réinitialiser des invites de mot de passe automatiquement à Oui.|  
|/target|Définit le contrôleur de domaine à utiliser. La valeur par défaut est le contrôleur de domaine être détecté, en fonction du nom principal. Si le nom de contrôleur de domaine ne résout pas, une boîte de dialogue vous invite pour un contrôleur de domaine valide.|  
|/rawsalt|force ktpass à utiliser l’algorithme rawsalt lors de la génération de la clé. Ce paramètre n’est pas nécessaire.|  
|{-&#124;+}dumpsalt|La sortie de ce paramètre indique l’algorithme de salt MIT qui est utilisé pour générer la clé.|  
|{-&#124;+}setupn|Définit le nom d’utilisateur principal (UPN) en plus du nom de principal du service (SPN). La valeur par défaut consiste à définir à la fois dans le fichier .keytab.|  
|{-&#124;+} setpass <Password>|Définit le mot de passe lorsque fournis. Si rndpass est utilisé, un mot de passe aléatoire est généré à la place.|  
|/?&#124;/h&#124;/help|Affiche de ligne de commande l’aide de ktpass.|  
## <a name="BKMK_remarks"></a>Remarques  
Services qui s’exécutent sur les systèmes qui n’exécutent pas le système d’exploitation Windows peuvent être configurés avec les comptes d’instance de service dans les Services de domaine active directory. Cela permet à n’importe quel client Kerberos pour s’authentifier auprès des services qui n’exécutent pas le système d’exploitation Windows à l’aide de Windows KDC.  
Le paramètre /princ n’est pas évalué par ktpass et est utilisé comme prévu. Il n’est pas vérifié pour voir si le paramètre correspond à la casse de la **userPrincipalName** valeur d’attribut lors de la génération du fichier Keytab. Les distributions Kerberos respecte la casse à l’aide de ce fichier Keytab peuvent avoir des problèmes lorsqu’il n’existe aucune correspondance de casse exacte peut échouer lors de l’authentification préalable. Vérifier et de récupérer le bon **userPrincipalName** valeur d’attribut à partir d’un fichier d’exportation LDifDE. Exemple :  
```  
ldifde /f keytab_user.ldf /d "CN=Keytab User,OU=UserAccounts,DC=contoso,DC=corp,DC=microsoft,DC=com" /p base /l samaccountname,userprincipalname  
```  
## <a name="BKMK_examples"></a>Exemples  
L’exemple suivant illustre comment créer un fichier .keytab Kerberos, machine.keytab, dans le répertoire actif pour l’utilisateur exemple1. (Vous allez fusionner ce fichier avec le fichier Krb5.keytab sur un ordinateur hôte qui n’exécute pas le système d’exploitation Windows.) Le fichier de .keytab Kerberos doit être créé pour tous les types de chiffrement pris en charge pour le type de principal général.  
Pour générer un fichier .keytab pour un ordinateur hôte qui n’exécute pas le système d’exploitation Windows, procédez comme suit pour mapper l’entité de sécurité pour le compte et de définir le mot de passe principal hôte :  
1.  Utilisez active directory utilisateurs et ordinateurs-composant logiciel enfichable pour créer un compte d’utilisateur pour un service sur un ordinateur qui n’exécute pas le système d’exploitation Windows. Par exemple, créer un compte portant le nom exemple1.  
2.  Ktpass permet de configurer un mappage d’identité pour le compte d’utilisateur en tapant la commande suivante à une invite de commandes :  
    ```  
    ktpass /princ host/Sample1.contoso.com@CONTOSO.COM /mapuser Sample1 /pass MyPas$w0rd /out Sample1.keytab /crypto all /ptype KRB5_NT_PRINCIPAL /mapop set   
    ```  
    
    > [!NOTE]  
    > Vous ne pouvez pas mapper plusieurs instances de service pour le même compte d’utilisateur.  
    
3.  Fusionner le fichier .keytab avec le fichier /Etc/Krb5.keytab sur un ordinateur hôte qui n’exécute pas le système d’exploitation Windows. 

#### <a name="additional-references"></a>Références supplémentaires  
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
