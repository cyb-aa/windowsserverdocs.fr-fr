---
title: certreq
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7a04e51f-f395-4bff-b57a-0e9efcadf973
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e3c98fd965fc6923c6af7893261bd1e0eee7d8b
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75947590"
---
# <a name="certreq"></a>certreq



Certreq peut être utilisé pour demander des certificats auprès d’une autorité de certification, pour récupérer une réponse à une demande précédente d’une autorité de certification, pour créer une nouvelle demande à partir d’un fichier. inf, pour accepter et installer une réponse à une demande, pour construire une certification croisée ou demande de subordination qualifiée à partir d’un certificat ou d’une demande d’autorité de certification existante, et pour signer une demande de certification croisée ou de subordination qualifiée.

> [!WARNING]
> - Les versions antérieures de Certreq peuvent ne pas fournir toutes les options décrites dans ce document. Vous pouvez voir toutes les options fournies par une version spécifique de Certreq en exécutant les commandes présentées dans la section notations de syntaxe.
> - Certreq ne prend pas en charge la création d’une demande de certificat basée sur un modèle d’attestation de clé dans un environnement CEP/EC

## <a name="BKMK_Contents"></a>Sommaire

Les principales sections de cet article sont les suivantes :
1.  [Verbes](#BKMK_Verbs)
2.  [Notations de syntaxe](#BKMK_notation)
3.  [Options](#BKMK_Options)
4.  [Formats](#BKMK_Formats)
5.  [Exemples supplémentaires Certreq](#BKMK_Examples)

## <a name="BKMK_Verbs"></a>Verbes

Le tableau suivant décrit les verbes qui peuvent être utilisés avec la commande Certreq.

|Basculez|Description|
|------|-----------|
|-Envoyer|Envoie une demande à une autorité de certification. Pour plus d’informations, voir [certreq-Submit](#BKMK_Submit).|
|-récupérer *RequestId*|Récupère une réponse à une demande précédente d’une autorité de certification. Pour plus d’informations, voir [certreq-retrieve](#BKMK_Retrieve).|
|-Nouveau|Crée une nouvelle demande à partir d’un fichier. inf. Pour plus d’informations, voir [certreq-New](#BKMK_New).|
|-Accepter|Accepte et installe une réponse à une demande de certificat. Pour plus d’informations, voir [certreq-Accept](#BKMK_accept).|
|-Stratégie|Définit la stratégie pour une demande. Pour plus d’informations, voir [certreq-Policy](#BKMK_policy).|
|-Sign|Signe une demande de certification croisée ou de subordination qualifiée. Pour plus d’informations, consultez [certreq-Sign](#BKMK_sign).|
|-Inscrire|Inscrit ou renouvelle un certificat. Pour plus d’informations, voir [certreq-ENROLL](#BKMK_enroll).|
|-?|Affiche une liste de la syntaxe, des options et des descriptions Certreq.|
|*\<verbe >* - ?|Affiche l’aide pour le verbe spécifié.|
|-v- ?|Affiche une liste détaillée de la syntaxe, des options et des descriptions Certreq.|

Retourner au [contenu](#BKMK_Contents)

## <a name="BKMK_notation"></a>Notations de syntaxe

-   Pour la syntaxe de ligne de commande de base, exécutez `certreq -?`
-   Pour la syntaxe d’utilisation de Certutil avec un verbe spécifique, exécutez **certreq** *\<verbe >* **- ?**
-   Pour envoyer l’ensemble de la syntaxe certutil dans un fichier texte, exécutez les commandes suivantes :  
    -   `certreq -v -? > certreqhelp.txt`
    -   `notepad certreqhelp.txt`

Le tableau suivant décrit la notation utilisée pour indiquer la syntaxe de la ligne de commande.

|Notation|Description|
|--------|-----------|
|Texte sans crochets ou accolades|Éléments que vous devez taper comme indiqué|
|\<texte à l’intérieur des crochets pointus >|Espace réservé pour lequel vous devez fournir une valeur|
|[Texte à l’intérieur des crochets]|Éléments facultatifs|
|{Text à l’intérieur des accolades}|Ensemble d’éléments requis ; choisir un|
|Barre verticale (&#124;)|Séparateur pour les éléments s’excluant mutuellement ; choisir un|
|Points de suspension (…)|Éléments pouvant être répétés|

Retourner au [contenu](#BKMK_Contents)

## <a name="BKMK_Submit"></a>Certreq-envoyer

Il s’agit du paramètre Certreq. exe par défaut, si aucune option n’est spécifiée explicitement à l’invite de ligne de commande, Certreq. exe tente d’envoyer une demande de certificat à une autorité de certification.
```
CertReq [-Submit] [Options] [RequestFileIn [CertFileOut [CertChainFileOut [FullResponseFileOut]]]]
```
Vous devez spécifier un fichier de demande de certificat lors de l’utilisation de l’option – Submit. Si ce paramètre est omis, une fenêtre d’ouverture de fichier courante s’affiche, dans laquelle vous pouvez sélectionner le fichier de demande de certificat approprié.

Vous pouvez utiliser ces exemples comme point de départ pour générer votre demande de soumission de certificat :

Pour soumettre une demande de certificat simple, utilisez l’exemple ci-dessous :
```
certreq –submit certRequest.req certnew.cer certnew.pfx
```
Pour demander un certificat en spécifiant l’attribut SAN, consultez les étapes détaillées de l’article 931351 de la base de connaissances Microsoft [comment ajouter un autre nom d’objet à un certificat LDAP sécurisé](https://support.microsoft.com/kb/931351) dans la section « Comment utiliser l’utilitaire Certreq. exe pour créer et envoyer une demande de certificat incluant un réseau SAN ».

Retourner au [contenu](#BKMK_Contents)

## <a name="BKMK_Retrieve"></a>Certreq-récupérer

```
certreq -retrieve [Options] RequestId [CertFileOut [CertChainFileOut [FullResponseFileOut]]]
```
-   Si vous ne spécifiez pas la boîte de dialogue Nomordinateurautoritécertification ou CAName in-config CAComputerName\CANamea apparaît et affiche la liste de toutes les autorités de certification disponibles.
-   Si vous utilisez -config - au lieu de -config NomOrdinateurAutoritéCertification\NomAutoritéCertification, l'opération est traitée à l'aide de l'autorité de certification par défaut.
-   Vous pouvez utiliser certreq-retrieve *RequestId* pour récupérer le certificat une fois qu’il a été émis par l’autorité de certification. La PKC *RequestId*peut être une valeur décimale ou hexadécimale avec un préfixe 0x, et il peut s’agir d’un numéro de série de certificat sans préfixe 0x. Vous pouvez également l’utiliser pour récupérer tout certificat émis par l’autorité de certification, y compris les certificats révoqués ou expirés, sans tenir compte de l’état d’attente de la demande du certificat.
-   Si vous soumettez une demande à l’autorité de certification, le module de stratégie de l’autorité de certification peut conserver la demande dans un état d’attente et renvoyer le *RequestId* à l’appelant CertReq pour l’affichage. Finalement, l’administrateur de l’autorité de certification émettra le certificat ou refusera la demande.

La commande ci-dessous récupère l’ID de certificat 20 et crée le fichier de certificat (. cer) :
```
certreq -retrieve 20 MyCertificate.cer 
```
Retourner au [contenu](#BKMK_Contents)

## <a name="BKMK_New"></a>Certreq-nouveau

```
certreq -new [Options] [PolicyFileIn [RequestFileOut]]
```
Étant donné que le fichier INF permet de spécifier un ensemble complet de paramètres et d’options, il est difficile de définir un modèle par défaut que les administrateurs doivent utiliser pour tous les besoins. Par conséquent, cette section décrit toutes les options pour vous permettre de créer un fichier INF adapté à vos besoins spécifiques. Les mots clés suivants sont utilisés pour décrire la structure des fichiers INF.
1.  Une *section* est une zone du fichier INF qui couvre un groupe logique de clés. Une section apparaît toujours entre crochets dans le fichier INF.
2.  Une *clé* est le paramètre qui se trouve à gauche du signe égal.
3.  Une *valeur* est le paramètre qui se trouve à droite du signe égal.

Par exemple, un fichier INF minimal ressemble à ce qui suit :
```
[NewRequest] 
; At least one value must be set in this section 
Subject = "CN=W2K8-BO-DC.contoso2.com"
```
Voici quelques-unes des sections possibles qui peuvent être ajoutées au fichier INF :

**[NewRequest]**

Cette section est obligatoire pour un fichier INF qui agit comme modèle pour une nouvelle demande de certificat. Cette section nécessite au moins une clé avec une valeur.

|Clé|Définition|Value|Exemple|
|---|----------|-----|-------|
|Sujet|Plusieurs applications reposent sur les informations d’objet dans un certificat. Par conséquent, il est recommandé de spécifier une valeur pour cette clé. Si l’objet n’est pas défini ici, il est recommandé d’inclure un nom d’objet dans le cadre de l’extension de certificat autre nom de l’objet.|Valeurs de chaîne de nom unique relatif|Subject = "CN = ordinateur1. contoso. com" subject = "CN = John Smith, CN = Users, DC = contoso, DC = com"|
|Exportable|Si cet attribut est défini sur TRUE, la clé privée peut être exportée avec le certificat. Pour garantir un niveau de sécurité élevé, les clés privées ne doivent pas être exportées ; Toutefois, dans certains cas, il peut être nécessaire de rendre la clé privée exportable si plusieurs ordinateurs ou utilisateurs doivent partager la même clé privée.|true, false|Exportable = TRUE. Les clés CNG peuvent faire la distinction entre ce texte et le texte en clair pouvant être exportés. Les clés CAPI1 ne peuvent pas.|
|ExportableEncrypted|Spécifie si la clé privée doit être configurée pour être exportable.|true, false|ExportableEncrypted = true</br>Conseil : les algorithmes et les tailles de clé publique ne sont pas tous compatibles avec tous les algorithmes de hachage. Tamehe CSP spécifié doit également prendre en charge l’algorithme de hachage spécifié. Pour afficher la liste des algorithmes de hachage pris en charge, vous pouvez exécuter la commande <code>certutil -oid 1 &#124; findstr pwszCNGAlgid &#124; findstr /v CryptOIDInfo</code>|
|HashAlgorithm|Algorithme de hachage à utiliser pour cette requête.|SHA256, SHA384, SHA512, SHA1, MD5, MD4, MD2|HashAlgorithm = SHA1. Pour afficher la liste des algorithmes de hachage pris en charge, utilisez : &#124; certutil- &#124; OID 1 findstr pwszCNGAlgid findstr/v CryptOIDInfo|
|Keyalgorithme|Algorithme qui sera utilisé par le fournisseur de services pour générer une paire de clés publique et privée.|RSA, DH, DSA, ECDH_P256, ECDH_P521, ECDSA_P256, ECDSA_P384, ECDSA_P521|Keyalgorithme = RSA|
|KeyContainer|Il n’est pas recommandé de définir ce paramètre pour les nouvelles demandes où de nouveaux éléments de clé sont générés. Le conteneur de clé est généré et géré automatiquement par le système. Pour les demandes où le matériel de clé existant doit être utilisé, cette valeur peut être définie sur le nom du conteneur de clés de la clé existante. Utilisez la commande certutil – Key pour afficher la liste des conteneurs de clés disponibles pour le contexte de l’ordinateur. Utilisez la commande certutil – key – user pour le contexte de l’utilisateur actuel.|Valeur de chaîne aléatoire</br>Conseil : vous devez utiliser des guillemets doubles autour d’une valeur de clé INF qui contient des espaces ou des caractères spéciaux pour éviter les éventuels problèmes d’analyse de fichier INF.|Keycontainer = {C347BD28-7F69-4090-AA16-BC58CF4D749C}|
|KeyLength|Définit la longueur de la clé publique et privée. La longueur de la clé a un impact sur le niveau de sécurité du certificat. Une longueur de clé supérieure fournit généralement un niveau de sécurité plus élevé. Toutefois, certaines applications peuvent avoir des limitations quant à la longueur de la clé.|Toute longueur de clé valide prise en charge par le fournisseur de services de chiffrement.|KeyLength = 2048|
|KeySpec|Détermine si la clé peut être utilisée pour les signatures, pour Exchange (chiffrement), ou pour les deux.|AT_NONE, AT_SIGNATURE AT_KEYEXCHANGE|KeySpec = AT_KEYEXCHANGE|
|Utilisation|Définit la fonction pour laquelle la clé de certificat doit être utilisée.|CERT_DIGITAL_SIGNATURE_KEY_USAGE--80 (128)</br>Conseil : les valeurs affichées sont des valeurs hexadécimales (décimales) pour chaque définition de bit. Une syntaxe plus ancienne peut également être utilisée : une valeur hexadécimale unique avec plusieurs bits définis, au lieu de la représentation symbolique. Par exemple, KeyUsage = 0xa0.</br>CERT_NON_REPUDIATION_KEY_USAGE--40 (64)</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE--20 (32)</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE--10 (16)</br>CERT_KEY_AGREEMENT_KEY_USAGE--8</br>CERT_KEY_CERT_SIGN_KEY_USAGE--4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE--2</br>CERT_CRL_SIGN_KEY_USAGE--2</br>CERT_ENCIPHER_ONLY_KEY_USAGE--1</br>CERT_DECIPHER_ONLY_KEY_USAGE--8000 (32768)|KeyUsage = "CERT_DIGITAL_SIGNATURE_KEY_USAGE &#124; CERT_KEY_ENCIPHERMENT_KEY_USAGE"</br>Conseil : plusieurs valeurs utilisent un séparateur de&#124;symboles de canal (). Veillez à utiliser des guillemets doubles lors de l’utilisation de plusieurs valeurs pour éviter les problèmes d’analyse INF.|
|KeyUsageProperty|Récupère une valeur qui identifie l’objectif spécifique pour lequel une clé privée peut être utilisée.|NCRYPT_ALLOW_DECRYPT_FLAG--1</br>NCRYPT_ALLOW_SIGNING_FLAG--2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG--4</br>NCRYPT_ALLOW_ALL_USAGES--FFFFFF (16777215)|KeyUsageProperty = "NCRYPT_ALLOW_SIGNING_FLAG &#124; NCRYPT_ALLOW_DECRYPT_FLAG"|
|MachineKeySet|Cette clé est importante lorsque vous devez créer des certificats appartenant à la machine et non à un utilisateur. Le matériel de clé généré est conservé dans le contexte de sécurité du principal de sécurité (compte d’utilisateur ou d’ordinateur) qui a créé la demande. Lorsqu’un administrateur crée une demande de certificat pour le compte d’un ordinateur, le matériel de clé doit être créé dans le contexte de sécurité de l’ordinateur et non dans le contexte de sécurité de l’administrateur. Dans le cas contraire, l’ordinateur n’a pas pu accéder à sa clé privée, car il se trouve dans le contexte de sécurité de l’administrateur.|true, false|MachineKeySet = true</br>Conseil : la valeur par défaut est false.|
|NotBefore|Spécifie une date ou une date et une heure avant lesquelles la demande ne peut pas être émise. NotBefore peut être utilisé avec ValidityPeriod et ValidityPeriodUnits.|Date ou date et heure|NotBefore = "7/24/2012 10:31 AM"</br>Conseil : NotBefore et NotAfter sont pour RequestType = CERT uniquement. La date d’analyse des tentatives est sensible aux paramètres régionaux. L’utilisation de noms de mois lève une ambiguïté et doit fonctionner dans tous les paramètres régionaux.|
|NotAfter|Spécifie une date ou une date et une heure au-delà desquelles la demande ne peut pas être émise. NotAfter ne peut pas être utilisé avec ValidityPeriod ou ValidityPeriodUnits.|Date ou date et heure|NotAfter = "9/23/2014 10:31 AM"</br>Conseil : NotBefore et NotAfter sont pour RequestType = CERT uniquement. La date d’analyse des tentatives est sensible aux paramètres régionaux. L’utilisation de noms de mois lève une ambiguïté et doit fonctionner dans tous les paramètres régionaux.|
|PrivateKeyArchive|Le paramètre PrivateKeyArchive fonctionne uniquement si le RequestType correspondant est défini sur « CMC », car seuls les messages de gestion des certificats au format de demande CMS (CMC) permettent de transférer en toute sécurité la clé privée du demandeur à l’autorité de certification pour l’archivage de clé.|true, false|PrivateKeyArchive = true|
|EncryptionAlgorithm|Algorithme de chiffrement à utiliser.|Les options possibles varient en fonction de la version du système d’exploitation et de l’ensemble des fournisseurs de services de chiffrement installés. Pour afficher la liste des algorithmes disponibles, exécutez la commande <code>certutil -oid 2 &#124; findstr pwszCNGAlgid</code> le CSP spécifié doit également prendre en charge l’algorithme de chiffrement symétrique et la longueur spécifiés.|EncryptionAlgorithm = 3DES|
|EncryptionLength|Longueur de l’algorithme de chiffrement à utiliser.|Toute longueur autorisée par le EncryptionAlgorithm spécifié.|EncryptionLength = 128|
|ProviderName|Le nom du fournisseur est le nom complet du fournisseur de services de chiffrement.|Si vous ne connaissez pas le nom du fournisseur du CSP que vous utilisez, exécutez certutil – csplist à partir d’une ligne de commande. La commande affiche les noms de tous les fournisseurs de services de chiffrement disponibles sur le système local.|ProviderName = « fournisseur de services de chiffrement Microsoft RSA SChannel »|
|ProviderType|Le type de fournisseur est utilisé pour sélectionner des fournisseurs spécifiques basés sur des fonctionnalités d’algorithme spécifiques telles que « RSA Full ».|Si vous ne connaissez pas le type de fournisseur du CSP que vous utilisez, exécutez certutil – csplist à partir d’une invite de ligne de commande. La commande affiche le type de fournisseur de tous les fournisseurs de services de chiffrement disponibles sur le système local.|ProviderType = 1|
|RenewalCert|Si vous devez renouveler un certificat qui existe sur le système où la demande de certificat est générée, vous devez spécifier son hachage de certificat comme valeur pour cette clé.|Hachage de certificat de tout certificat disponible sur l’ordinateur où la demande de certificat est créée. Si vous ne connaissez pas le hachage du certificat, utilisez le composant logiciel enfichable MMC certificats et examinez le certificat qui doit être renouvelé. Ouvrez les propriétés du certificat et consultez l’attribut « empreinte » du certificat. Le renouvellement du certificat requiert un format de demande PKCS # 7 ou CMC.|RenewalCert = 4EDF274BD2919C6E9EC6A522F0F3B153E9B1582D|
|RequesterName</br>Remarque : la requête est alors inscrite pour le compte d’une autre demande d’utilisateur. La demande doit également être signée avec un certificat d’agent d’inscription, ou l’autorité de certification rejette la demande. Utilisez l’option-CERT pour spécifier le certificat de l’agent d’inscription.|Le nom du demandeur peut être spécifié pour les demandes de certificat si RequestType a la valeur PKCS # 7 ou CMC. Si RequestType a la valeur PKCS # 10, cette clé sera ignorée. Le RequesterName peut uniquement être défini dans le cadre de la requête. Vous ne pouvez pas manipuler RequesterName dans une requête en attente.|Domaine|RequesterName = "Contoso\BSmith"|
|RequestType|Détermine la norme utilisée pour générer et envoyer la demande de certificat.|PKCS10--1</br>PKCS7--2</br>CMC--3</br>CERT--4</br>SCEP--fd00 (64768)</br>Conseil : cette option indique un certificat auto-signé ou auto-émis. Elle ne génère pas de demande, mais plutôt un nouveau certificat, puis installe le certificat. La valeur par défaut est auto-signé. Spécifiez un certificat de signature à l’aide de l’option – CERT pour créer un certificat auto-émis qui n’est pas auto-signé.|RequestType = CMC|
|SecurityDescriptor</br>Conseil : cela s’applique uniquement aux clés de carte non-active de contexte machine.|Contiennent les informations de sécurité associées aux objets sécurisables. Pour la plupart des objets sécurisables, vous pouvez spécifier le descripteur de sécurité d’un objet dans l’appel de fonction qui crée l’objet.|Chaînes basées sur le [langage de définition du descripteur de sécurité](https://msdn.microsoft.com/library/aa379567(v=vs.85).aspx).|SecurityDescriptor = "D :P (A ;; GA ;;; SY) (A ;; GA ;;; BA)»|
|AlternateSignatureAlgorithm|Spécifie et récupère une valeur booléenne qui indique si l’identificateur d’objet (OID) de l’algorithme de signature pour une demande ou une signature de certificat PKCS # 10 est discret ou combiné.|true, false|AlternateSignatureAlgorithm = false</br>Conseil : pour une signature RSA, false indique un Pkcs1 v 1.5. True indique une signature v 2.1.|
|Silent|Par défaut, cette option permet au CSP d’accéder au bureau interactif de l’utilisateur et de demander des informations, telles qu’un code confidentiel de carte à puce, à l’utilisateur. Si cette clé est définie sur TRUE, le fournisseur de services de chiffrement ne doit pas interagir avec le bureau et ne pourra pas afficher l’interface utilisateur de l’utilisateur.|true, false|Silent = true|
|SMIME|Si ce paramètre est défini sur TRUE, une extension avec la valeur d’identificateur d’objet 1.2.840.113549.1.9.15 est ajoutée à la demande. Le nombre d’identificateurs d’objet dépend du sur la version du système d’exploitation installée et de la fonctionnalité CSP, qui font référence aux algorithmes de chiffrement symétrique qui peuvent être utilisés par les applications S/MIME (Secure Multipurpose Internet Mail Extensions), telles qu’Outlook.|true, false|SMIME = true|
|UseExistingKeySet|Ce paramètre est utilisé pour spécifier qu’une paire de clés existante doit être utilisée lors de la création d’une demande de certificat. Si cette clé est définie sur TRUE, vous devez également spécifier une valeur pour la clé RenewalCert ou le nom du keycontainer. Vous ne devez pas définir la clé exportable, car vous ne pouvez pas modifier les propriétés d’une clé existante. Dans ce cas, aucun matériel de clé n’est généré lors de la génération de la demande de certificat.|true, false|UseExistingKeySet = true|
|Keyprotection|Spécifie une valeur qui indique comment une clé privée est protégée avant son utilisation.|XCN_NCRYPT_UI_NO_PROTCTION_FLAG--0</br>XCN_NCRYPT_UI_PROTECT_KEY_FLAG--1</br>XCN_NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG--2|Keyprotection = NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG|
|SuppressDefaults|Spécifie une valeur booléenne qui indique si les extensions et attributs par défaut sont inclus dans la demande. Les valeurs par défaut sont représentées par leurs identificateurs d’objet (OID).|true, false|SuppressDefaults = true|
|friendlyName|Nom convivial pour le nouveau certificat.|Text|FriendlyName = "Serveur1"|
|ValidityPeriodUnits</br>Remarque : Ceci est utilisé uniquement lorsque le type de demande est CERT.|Spécifie un nombre d’unités à utiliser avec ValidityPeriod.|Numérique|ValidityPeriodUnits = 3|
|ValidityPeriod</br>Remarque : Ceci est utilisé uniquement lorsque le type de demande est CERT.|VValidityPeriod doit être une période de temps en anglais américain.|Années, mois, semaines, jours, heures, minutes, secondes|ValidityPeriod = années|

Retourner au [contenu](#BKMK_Contents)

**Extensions**

Cette section est facultative.


|  OID d’extension   | Définition | Value |                                                                                                                                                                                                                                                                                                                                                                                                                      Exemple                                                                                                                                                                                                                                                                                                                                                                                                                       |
|------------------|------------|-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    2.5.29.17     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                2.5.29.17 = "{Text}"                                                                                                                                                                                                                                                                                                                                                                                                                |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                        *continue* = "UPN =User@Domain.com&"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                       *continue* = "EMail =User@Domain.com&"                                                                                                                                                                                                                                                                                                                                                                                                        |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                        *continue* = "DNS = Host. domain. com &"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                               *continue* = "DIRECTORYNAME = CN = Name, DC = domain, DC = com &"                                                                                                                                                                                                                                                                                                                                                                                               |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                             *continue* = "URL =<http://host.domain.com/default.html&>"                                                                                                                                                                                                                                                                                                                                                                                              |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                         *continue* = "IPAddress = 10.0.0.1 &"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                       *continue* = "RegisteredId = 1.2.3.4.5 &"                                                                                                                                                                                                                                                                                                                                                                                                       |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                      *continue* = "1.2.3.4.6.1 = {UTF8} chaîne &"                                                                                                                                                                                                                                                                                                                                                                                                      |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                  *continue* = "1.2.3.4.6.2 = {octet} AAECAwQFBgc = &"                                                                                                                                                                                                                                                                                                                                                                                                   |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                          *continue* = "1.2.3.4.6.2 = {octet} {hex} 00 01 02 03 04 05 06 07 &"                                                                                                                                                                                                                                                                                                                                                                                           |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                 *continue* = "1.2.3.4.6.3 = {ASN} BAgAAQIDBAUGBw = = &"                                                                                                                                                                                                                                                                                                                                                                                                  |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                           *continue* = "1.2.3.4.6.3 = {hex} 04 08 00 01 02 03 04 05 06 07"                                                                                                                                                                                                                                                                                                                                                                                            |
|    2.5.29.37     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                 2.5.29.37 = "{Text}"                                                                                                                                                                                                                                                                                                                                                                                                                 |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                            *continue* = "1.3.6.1.5.5.7.                                                                                                                                                                                                                                                                                                                                                                                                            |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                          *continue* = "1.3.6.1.5.5.7.3.1"                                                                                                                                                                                                                                                                                                                                                                                                          |
|    2.5.29.19     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                              « {Text} ca = 0pathlength = 3 »                                                                                                                                                                                                                                                                                                                                                                                                              |
|     Critical     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                 Critique = 2.5.29.19                                                                                                                                                                                                                                                                                                                                                                                                                 |
|     KeySpec      |            |       |                                                                                                                                                                                                                                                                                                                                                                                             AT_NONE--0</br>AT_SIGNATURE--2</br>AT_KEYEXCHANGE--1                                                                                                                                                                                                                                                                                                                                                                                             |
|   RequestType    |            |       |                                                                                                                                                                                                                                                                                                                                                                                   PKCS10--1</br>PKCS7--2</br>CMC--3</br>CERT--4</br>SCEP--fd00 (64768)                                                                                                                                                                                                                                                                                                                                                                                   |
|     Utilisation     |            |       |                                                                                                                                                                                                       CERT_DIGITAL_SIGNATURE_KEY_USAGE--80 (128)</br>CERT_NON_REPUDIATION_KEY_USAGE--40 (64)</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE--20 (32)</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE--10 (16)</br>CERT_KEY_AGREEMENT_KEY_USAGE--8</br>CERT_KEY_CERT_SIGN_KEY_USAGE--4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE--2</br>CERT_CRL_SIGN_KEY_USAGE--2</br>CERT_ENCIPHER_ONLY_KEY_USAGE--1</br>CERT_DECIPHER_ONLY_KEY_USAGE--8000 (32768)                                                                                                                                                                                                       |
| KeyUsageProperty |            |       |                                                                                                                                                                                                                                                                                                                                            NCRYPT_ALLOW_DECRYPT_FLAG--1</br>NCRYPT_ALLOW_SIGNING_FLAG--2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG--4</br>NCRYPT_ALLOW_ALL_USAGES--FFFFFF (16777215)                                                                                                                                                                                                                                                                                                                                             |
|  Keyprotection   |            |       |                                                                                                                                                                                                                                                                                                                                                                NCRYPT_UI_NO_PROTECTION_FLAG--0</br>NCRYPT_UI_PROTECT_KEY_FLAG--1</br>NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG--2                                                                                                                                                                                                                                                                                                                                                                 |
| SubjectNameFlags |  modèle  |       |                                                                           CT_FLAG_SUBJECT_REQUIRE_COMMON_NAME--40 MILLIONS (1073741824)</br>CT_FLAG_SUBJECT_REQUIRE_DIRECTORY_PATH--80 MILLIONS (2147483648)</br>CT_FLAG_SUBJECT_REQUIRE_DNS_AS_CN--10 MILLIONS (268435456)</br>CT_FLAG_SUBJECT_REQUIRE_EMAIL--20 MILLIONS (536870912)</br>CT_FLAG_OLD_CERT_SUPPLIES_SUBJECT_AND_ALT_NAME--8</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DIRECTORY_GUID--1 MILLION (16777216)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DNS--8 MILLIONS (134217728)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DOMAIN_DNS--400000 (4194304)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_EMAIL--4 MILLIONS (67108864)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_SPN--800000 (8388608)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_UPN--2 MILLIONS (33554432)                                                                            |
|  L x500nameflags   |            |       | CERT_NAME_STR_NONE--0</br>CERT_OID_NAME_STR--2</br>CERT_X500_NAME_STR--3</br>CERT_NAME_STR_SEMICOLON_FLAG--40 MILLIONS (1073741824)</br>CERT_NAME_STR_NO_PLUS_FLAG--20 MILLIONS (536870912)</br>CERT_NAME_STR_NO_QUOTING_FLAG--10 MILLIONS (268435456)</br>CERT_NAME_STR_CRLF_FLAG--8 MILLIONS (134217728)</br>CERT_NAME_STR_COMMA_FLAG--4 MILLIONS (67108864)</br>CERT_NAME_STR_REVERSE_FLAG--2 MILLIONS (33554432)</br>CERT_NAME_STR_FORWARD_FLAG--1 MILLION (16777216)</br>CERT_NAME_STR_DISABLE_IE4_UTF8_FLAG--10000 (65536)</br>CERT_NAME_STR_ENABLE_T61_UNICODE_FLAG--20000 (131072)</br>CERT_NAME_STR_ENABLE_UTF8_UNICODE_FLAG--40000 (262144)</br>CERT_NAME_STR_FORCE_UTF8_DIR_STR_FLAG--80000 (524288)</br>CERT_NAME_STR_DISABLE_UTF8_DIR_STR_FLAG--100000 (1048576)</br>CERT_NAME_STR_ENABLE_PUNYCODE_FLAG--200000 (2097152) |

Retourner au [contenu](#BKMK_Contents)

> [!NOTE]
> SubjectNameFlags permet au fichier INF de spécifier les champs d’extension Subject et SubjectAltName qui doivent être remplis automatiquement par Certreq en fonction de l’utilisateur actuel ou des propriétés de l’ordinateur actuel : nom DNS, UPN, etc. L’utilisation du littéral « template » signifie que les indicateurs de nom de modèle sont utilisés à la place. Cela permet à un seul fichier INF d’être utilisé dans plusieurs contextes pour générer des demandes avec des informations de sujet spécifiques au contexte.
>
> L x500nameflags spécifie les indicateurs à transmettre directement à l’API CertStrToName lorsque la valeur des clés INF de l’objet est convertie en un nom unique encodé ASN. 1.

Pour demander un certificat basé sur Certreq-New, suivez les étapes de l’exemple ci-dessous :

> [!WARNING]
> Le contenu de cette rubrique est basé sur les paramètres par défaut pour Windows Server 2008 AD CS ; par exemple, la définition de la longueur de clé sur 2048, la sélection du fournisseur de stockage de clés logicielles Microsoft comme CSP et l’utilisation de Secure Hash Algorithm 1 (SHA1). Évaluez ces sélections par rapport aux exigences de la stratégie de sécurité de votre entreprise.

Pour créer un fichier de stratégie (. inf) copiez et enregistrez l’exemple ci-dessous dans le bloc-notes et enregistrez-le sous RequestConfig. inf :
```
[NewRequest] 
Subject = "CN=<FQDN of computer you are creating the certificate>" 
Exportable = TRUE 
KeyLength = 2048 
KeySpec = 1 
KeyUsage = 0xf0 
MachineKeySet = TRUE 
[RequestAttributes]
CertificateTemplate="WebServer"
[Extensions] 
OID = 1.3.6.1.5.5.7.3.1 
OID = 1.3.6.1.5.5.7.3.2  
```
Sur l’ordinateur pour lequel vous demandez un certificat, tapez la commande ci-dessous :
```
CertReq –New RequestConfig.inf CertRequest.req 
```
L’exemple suivant illustre l’implémentation de la syntaxe de la section [strings] pour les OID et d’autres difficultés pour interpréter les données. Nouvel exemple de syntaxe {Text} pour l’extension EKU, qui utilise une liste séparée par des virgules d’OID :
```
[Version]
Signature="$Windows NT$

[Strings]
szOID_ENHANCED_KEY_USAGE = "2.5.29.37"
szOID_PKIX_KP_SERVER_AUTH = "1.3.6.1.5.5.7.3.1"
szOID_PKIX_KP_CLIENT_AUTH = "1.3.6.1.5.5.7.3.2"

[NewRequest]
Subject = "CN=TestSelfSignedCert"
Requesttype = Cert

[Extensions]
%szOID_ENHANCED_KEY_USAGE%="{text}%szOID_PKIX_KP_SERVER_AUTH%,"
_continue_ = "%szOID_PKIX_KP_CLIENT_AUTH%"
```
Retourner au [contenu](#BKMK_Contents)

## <a name="BKMK_accept"></a>Certreq-accepter

```
CertReq -accept [Options] [CertChainFileIn | FullResponseFileIn | CertFileIn]
```
Le paramètre – Accept lie la clé privée générée précédemment au certificat émis et supprime la demande de certificat en attente du système où le certificat est demandé (s’il existe une demande correspondante).

Vous pouvez utiliser cet exemple pour accepter manuellement un certificat :
```
certreq -accept certnew.cer 
```

> [!WARNING]
> Le verbe-Accept, les options-user et – machine indiquent si le certificat en cours d’installation doit être installé dans le contexte de l’utilisateur ou de l’ordinateur. S’il existe une demande en attente dans l’un des deux contextes qui correspond à la clé publique en cours d’installation, ces options ne sont pas nécessaires. S’il n’y a aucune demande en suspens, l’une d’entre elles doit être spécifiée.

Retourner au [contenu](#BKMK_Contents)

## <a name="BKMK_policy"></a>Certreq-stratégie

```
certreq -policy [-attrib AttributeString] [-binary] [-cert CertID] [RequestFileIn [PolicyFileIn [RequestFileOut [PKCS10FileOut]]]]
```
-   Le fichier de configuration qui définit les contraintes appliquées à un certificat d’autorité de certification lorsque la subordination qualifiée est définie est appelé Policy. inf.
-   Vous trouverez un exemple de fichier de stratégie. inf dans le livre blanc [annexe A du Guide de planification et d’implémentation de la certification croisée et de la subordination qualifiée](https://technet.microsoft.com/library/cc738878(WS.10).aspx) .
-   Si vous tapez Certreq-Policy sans paramètre supplémentaire, une fenêtre de dialogue s’ouvre pour vous permettre de sélectionner le fichier demandé (Req, CMC, txt, der, CER ou CRT). Une fois que vous avez sélectionné le fichier demandé et cliqué sur le bouton Ouvrir, une autre fenêtre de dialogue s’ouvre afin de sélectionner le fichier INF.

Vous pouvez utiliser cet exemple pour générer une demande de certificat croisé :
```
certreq -policy Certsrv.req Policy.inf newcertsrv.req 
```
Retourner au [contenu](#BKMK_Contents)

## <a name="BKMK_sign"></a>Certreq-Sign

```
certreq -sign [Options] [RequestFileIn [RequestFileOut]]
```
-   Si vous tapez le signe Certreq sans paramètre supplémentaire, une fenêtre de dialogue s’ouvre pour vous permettre de sélectionner le fichier demandé (Req, CMC, txt, der, CER ou CRT).
-   La signature de la demande de subordination qualifiée peut nécessiter des informations d’identification d’administrateur d’entreprise. Il s’agit d’une meilleure pratique pour émettre des certificats de signature pour la subordination qualifiée.
-   Le certificat utilisé pour signer la demande de subordination qualifiée est créé à l’aide du modèle de subordination qualifiée. Les administrateurs de l’entreprise doivent signer la demande ou accorder des autorisations à l’utilisateur pour les personnes qui signeront le certificat.
-   Lorsque vous signez la demande CMC, vous devrez peut-être demander à plusieurs personnes de signer cette demande, en fonction du niveau d’assurance associé à la subordination qualifiée.
-   Si l’autorité de certification parente de l’autorité de certification subordonnée qualifiée que vous installez est hors connexion, vous devez obtenir le certificat d’autorité de certification pour l’autorité de certification secondaire qualifiée à partir du parent hors connexion. Si l’autorité de certification parente est en ligne, spécifiez le certificat d’autorité de certification pour l’autorité de certification subordonnée qualifiée pendant l’Assistant Installation des services de certificats.

La séquence de commandes ci-dessous montre comment créer une demande de certificat, la signer et la soumettre :
```
certreq -new policyfile.inf MyRequest.req
certreq -sign MyRequest.req MyRequest_Sign.req
certreq -submit MyRequest_Sign.req MyRequest_cert.cer 
```
Retourner au [contenu](#BKMK_Contents)

## <a name="BKMK_enroll"></a>Certreq-inscrire

Pour inscrire un certificat
```
certreq –enroll [Options] TemplateName
```
Pour renouveler un certificat existant
```
certreq –enroll –cert CertId [Options] Renew [ReuseKeys]
```
Vous pouvez uniquement renouveler les certificats qui sont valides. Les certificats arrivés à expiration ne peuvent pas être renouvelés et doivent être remplacés par un nouveau certificat.

Voici un exemple de renouvellement d’un certificat à l’aide de son numéro de série :
```
certreq –enroll -machine –cert "61 2d 3c fe 00 00 00 00 00 05" Renew
```
Voici un exemple d’inscription à un modèle de certificat appelé WebServer à l’aide de l’astérisque (*) pour sélectionner le serveur de stratégie via U/I :
```
certreq -enroll –machine –policyserver * "WebServer"
```
Retourner au [contenu](#BKMK_Contents)

## <a name="BKMK_Options"></a>Options

|Options|Description|
|-------|-----------|
|-any|Force ICertRequest :: submit pour déterminer le type d’encodage.|
|-attrib \<AttributeString >|Spécifie les paires de chaînes de nom et de valeur, séparées par un signe deux-points.</br>Séparez les paires de chaînes de noms et de valeurs par \n (par exemple, nom1 : Value1\nName2 : value2).|
|-binaire|Met en forme les fichiers de sortie au format binaire au lieu de l’encodage Base64.|
|-PolicyServer *\<PolicyServer >*|« LDAP : *chemin d’accès\<* »</br>Insérez l’URI ou l’ID unique d’un ordinateur exécutant l’service Web Stratégie d’inscription de certificats.</br>Pour spécifier que vous souhaitez utiliser un fichier de requête en navigant, utilisez simplement un signe moins (-) pour *\<> policyserver*.|
|-config \<ConfigString >|Traite l’opération à l’aide de l’autorité de certification spécifiée dans la chaîne de configuration, qui est CAHostName\CAName. Pour une connexion HTTPS, spécifiez l’URI du serveur d’inscription. Pour l’autorité de certification du magasin de l’ordinateur local, utilisez un signe moins (-).|
|-Anonymous|Utilisez des informations d’identification anonymes pour les services Web inscription de certificats.|
|-Kerberos|Utilisez les informations d’identification Kerberos (domaine) pour les services Web inscription de certificats.|
|-ClientCertificate *\<ClientCertId >*|Vous pouvez remplacer le *\<ClientCertID >* par une empreinte numérique de certificat, CN, EKU, template, E-mail, UPN et la nouvelle syntaxe name = value.|
|-UserName *\<nom d’utilisateur >*|Utilisé avec les services Web d’inscription de certificats. Vous pouvez remplacer *\<nom d’utilisateur >* par le nom Sam ou domaine\utilisateur. Cette option est destinée à être utilisée avec l’option-p.|
|-p *\<mot de passe >*|Utilisé avec les services Web d’inscription de certificats. Remplacez *\<> de mot de passe* par le mot de passe de l’utilisateur réel. Cette option est destinée à être utilisée avec l’option-UserName.|
|-utilisateur|Configure le contexte utilisateur pour une nouvelle demande de certificat ou spécifie le contexte pour l’acceptation d’un certificat. Il s’agit du contexte par défaut, si aucun n’est spécifié dans le fichier INF ou le modèle.|
|-machine|Configure une nouvelle demande de certificat ou spécifie le contexte d’une acceptation de certificat pour le contexte de l’ordinateur. Pour les nouvelles requêtes, il doit être cohérent avec la clé INF MachineKeyset et le contexte du modèle. Si cette option n’est pas spécifiée et que le modèle ne définit pas de contexte, la valeur par défaut est le contexte de l’utilisateur.|
|-CRL|Comprend les listes de révocation de certificats (CRL) dans la sortie du fichier #7 PKCS encodé en base64 spécifié par CertChainFileOut ou dans le fichier encodé en base64 spécifié par RequestFileOut.|
|-RPC|Indique à Active Directory Services de certificats (AD CS) d’utiliser une connexion de serveur d’appel de procédure distante (RPC) au lieu de COM distribué.|
|-AdminForceMachine|Utilisez le service de clé ou l’emprunt d’identité pour envoyer la demande à partir du contexte du système local. Exige que l’utilisateur qui appelle cette option soit membre du groupe Administrateurs local.|
|-RenewOnBehalfOf|Soumettez un renouvellement pour le compte du sujet identifié dans le certificat de signature. Cela définit CR_IN_ROBO lors de l’appel de [ICertRequest :: Submit](https://technet.microsoft.com/subscriptions/aa385040.aspx)|
|-f|Forcer le remplacement des fichiers existants. Cela contourne également les modèles et la stratégie de mise en cache.|
|-q|Utiliser le mode silencieux ; supprimer toutes les invites interactives.|
|-Unicode|Écrit la sortie Unicode lorsque la sortie standard est redirigée ou redirigée vers une autre commande, ce qui vous aide quand elle est appelée à partir de scripts de® Windows PowerShell).|
|-UnicodeText|Envoie une sortie Unicode lors de l’écriture d’objets BLOB de données encodées en texte Base64 dans des fichiers.|

Retourner au [contenu](#BKMK_Contents)

## <a name="BKMK_Formats"></a>Formats

|Formats|Description|
|-------|-----------|
|RequestFileIn|Nom du fichier d’entrée binaire ou encodé en base64 : demande de certificat #10 PKCS, demande de certificat CMS, demande de renouvellement de certificat PKCS #7, certificat X. 509 à certification croisée ou demande de certificat au format de balise KeyGen.|
|RequestFileOut|Nom du fichier de sortie encodé en base64|
|CertFileOut|Nom de fichier X-509 encodé en base64.|
|PKCS10FileOut|À utiliser avec le verbe [certreq-Policy](#BKMK_policy) uniquement. Nom du fichier de sortie PKCS10 encodé en base64.|
|CertChainFileOut|Nom du fichier de #7 PKCS encodé en base64.|
|FullResponseFileOut|Nom du fichier réponse complet encodé en base64.|
|PolicyFileIn|À utiliser avec le verbe [certreq-Policy](#BKMK_policy) uniquement. Fichier INF contenant une représentation textuelle des extensions utilisées pour qualifier une demande.|

## <a name="BKMK_Examples"></a>Exemples supplémentaires Certreq

Les articles suivants contiennent des exemples d’utilisation de Certreq :
-   [Comment demander un certificat avec un autre nom d’objet personnalisé](https://technet.microsoft.com/library/ff625722.aspx)
-   [Guide de laboratoire de test : déploiement d’une hiérarchie PKI à deux couches AD CS](https://technet.microsoft.com/library/hh831348.aspx)
-   [Annexe 3 : syntaxe Certreq. exe](https://technet.microsoft.com/library/cc736326.aspx)
-   [Comment créer un certificat SSL de serveur Web manuellement](https://blogs.technet.com/b/pki/archive/2009/08/05/how-to-create-a-web-server-ssl-certificate-manually.aspx)
-   [Demander un certificat de configuration AMT à l’aide d’une autorité de certification Windows Server 2008](https://social.technet.microsoft.com/wiki/contents/articles/request-an-amt-provisioning-certificate-using-a-windows-server-2008-ca.aspx)
-   [Inscription de certificats pour System Center Operations Manager agent](https://social.technet.microsoft.com/wiki/contents/articles/certificate-enrollment-for-system-center-operations-manager-agent.aspx)
-   [Guide pas à pas des services AD CS : déploiement de la hiérarchie PKI à deux niveaux](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)
-   [Comment activer LDAP sur SSL avec une autorité de certification tierce](https://support.microsoft.com/kb/321051)

Retourner au [contenu](#BKMK_Contents)
