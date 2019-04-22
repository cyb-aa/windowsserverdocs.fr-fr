---
title: certreq
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9e48682cee40c00e187ca674bd8019b136a2c3f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818140"
---
# <a name="certreq"></a>certreq



Certreq peut être utilisé pour demander des certificats auprès d’une autorité de certification (CA), pour récupérer une réponse à une demande précédente à partir d’une autorité de certification, pour créer une nouvelle demande à partir d’un fichier .inf, accepter et installer une réponse à une demande, pour construire une certification croisée ou demande de subordination qualifiée à partir d’une requête ou un certificat d’autorité de certification existante et pour signer une demande de subordination qualifiée ou de la certification croisée.

> [!WARNING]
> - Les versions antérieures de certreq ne peuvent pas fournir toutes les options sont décrites dans ce document. Vous pouvez voir toutes les options fournies par une version spécifique de certreq en exécutant les commandes indiquées dans la section de notations de syntaxe.
> - Certreq ne prend pas en charge la création d’une nouvelle demande de certificat basée sur un modèle d’Attestation de clé dans un environnement de CEP/EC

## <a name="BKMK_Contents"></a>Contenu

Les sections principales dans cet article sont les suivantes :
1.  [Verbs](#BKMK_Verbs)
2.  [Notations de syntaxe](#BKMK_notation)
3.  [Options](#BKMK_Options)
4.  [Formats](#BKMK_Formats)
5.  [Exemples de certreq supplémentaires](#BKMK_Examples)

## <a name="BKMK_Verbs"></a>Verbes

Il le tableau suivant décrit les verbes qui peuvent être utilisés avec la commande certreq

|Basculer|Description|
|------|-----------|
|-Submit|Envoie une demande à une autorité de certification. Pour plus d’informations, consultez [Certreq-envoyer](#BKMK_Submit).|
|-récupérer *RequestID*|Récupère une réponse à une demande précédente à partir d’une autorité de certification. Pour plus d’informations, consultez [Certreq-récupérer](#BKMK_Retrieve).|
|-Nouveau|Crée une nouvelle demande à partir d’un fichier .inf. Pour plus d’informations, consultez [Certreq-nouvelle](#BKMK_New).|
|-Accepter|Accepte et installe une réponse à une demande de certificat. Pour plus d’informations, consultez [Certreq-accepter](#BKMK_accept).|
|-Policy|Définit la stratégie pour une demande. Pour plus d’informations, consultez [Certreq-stratégie](#BKMK_policy).|
|-Sign|Signe une certification croisée ou la demande de subordination qualifiée. Pour plus d’informations, consultez [Certreq-connexion](#BKMK_sign).|
|-S’inscrire|Inscrit pour ou renouvelle un certificat. Pour plus d’informations, consultez [Certreq-inscrire](#BKMK_enroll).|
|-?|Affiche une liste de syntaxe de certreq, les options et les descriptions.|
|*\<verb>* -?|Affiche l’aide pour le verbe spécifié.|
|-v -?|Affiche une liste détaillée de la syntaxe de certreq, les options et les descriptions.|

Retour à [contenu](#BKMK_Contents)

## <a name="BKMK_notation"></a>Notations de syntaxe

-   Pour la syntaxe de ligne de commande de base, exécutez `certreq -?`
-   Pour connaître la syntaxe sur l’utilisation de certutil avec un verbe spécifique, exécutez **certreq**  *\<verbe >* **- ?**
-   Pour envoyer l’ensemble de la syntaxe de certutil dans un fichier texte, exécutez les commandes suivantes :  
    -   `certreq -v -? > certreqhelp.txt`
    -   `notepad certreqhelp.txt`

Le tableau suivant décrit la notation utilisée pour indiquer la syntaxe de ligne de commande.

|Notation|Description|
|--------|-----------|
|Texte sans crochets ou accolades|Vous devez taper comme des éléments|
|\<Texte à l’intérieur de crochets pointus >|Espace réservé pour lequel vous devez fournir une valeur|
|[Texte à l’intérieur des crochets]|Éléments facultatifs|
|{Texte entre accolades}|Ensemble d’éléments requis ; Choisissez une|
|Barre verticale (&#124;)|Séparateur d’éléments qui s’excluent mutuellement ; Choisissez une|
|Points de suspension (…)|Éléments qui peuvent être répétés|

Retour à [contenu](#BKMK_Contents)

## <a name="BKMK_Submit"></a>Certreq-envoyer

Il s’agit du paramètre certreq.exe par défaut, si aucune option n’est spécifiée explicitement à l’invite de ligne de commande, certreq.exe tente d’envoyer une demande de certificat à une autorité de certification.
```
CertReq [-Submit] [Options] [RequestFileIn [CertFileOut [CertChainFileOut [FullResponseFileOut]]]]
```
Vous devez spécifier un fichier de demande de certificat lors de l’utilisation – envoyer option. Si ce paramètre est omis, une fenêtre d’ouvrir le fichier commune s’affiche, où vous pouvez sélectionner le fichier de demande de certificat approprié.

Vous pouvez utiliser ces exemples comme point de départ pour générer votre demande d’envoi de certificat :

Pour envoyer une demande de certificat simple utilisez l’exemple ci-dessous :
```
certreq –submit certRequest.req certnew.cer certnew.pfx
```
Pour demander un certificat en spécifiant l’attribut SAN, consultez les étapes détaillées dans l’article de la Base de connaissances Microsoft 931351 [comment ajouter un autre nom de l’objet à un certificat LDAP sécurisé](https://support.microsoft.com/kb/931351) dans la « comment utiliser l’utilitaire Certreq.exe pour section « créer et soumettre une demande de certificat qui inclut un réseau SAN.

Retour à [contenu](#BKMK_Contents)

## <a name="BKMK_Retrieve"></a>Certreq-récupérer

```
certreq -retrieve [Options] RequestId [CertFileOut [CertChainFileOut [FullResponseFileOut]]]
```
-   Si vous ne spécifiez pas le NomOrdinateurAutoritéCertification ou CAName dans la boîte de dialogue - config CAComputerName\CANamea apparaît et affiche une liste de toutes les autorités de certification qui sont disponibles.
-   Si vous utilisez - config - au lieu de-config CAComputerName\CAName, l’opération est traitée à l’aide de l’autorité de certification par défaut.
-   Vous pouvez utiliser certreq-récupérer *RequestID* pour récupérer le certificat une fois que l’autorité de certification a réellement émis. Le *RequestID*PKC peut être un nombre décimal ou hexadécimal avec 0 x préfixe et il peut être un numéro de série du certificat sans préfixe 0 x. Vous pouvez également l’utiliser pour récupérer n’importe quel certificat qui a déjà été émis par l’autorité de certification, y compris des certificats révoqués ou expirés, sans tenir compte si la demande de certificat a été placée dans l’état en attente.
-   Si vous envoyez une demande à l’autorité de certification, le module de stratégie de l’autorité de certification peut laisser la demande dans un état d’attente et le retour du *RequestID* à l’appelant Certreq pour affichage. Finalement, administrateur de l’autorité de certification sera émettre le certificat ou refuser la demande.

La commande suivante récupère l’id de certificat 20 et crée le fichier de certificat (.cer) :
```
certreq -retrieve 20 MyCertificate.cer 

```
Retour à [contenu](#BKMK_Contents)

## <a name="BKMK_New"></a>Certreq-nouveau

```
certreq -new [Options] [PolicyFileIn [RequestFileOut]]
```
Dans la mesure où le fichier INF permet d’un ensemble complet des paramètres et options à spécifier, il est difficile de définir un modèle par défaut que les administrateurs doivent utiliser pour tous les besoins. Par conséquent, cette section décrit toutes les options vous permettent de créer un fichier INF adapté à vos besoins spécifiques. Les mots clés suivants sont utilisés pour décrire la structure du fichier INF.
1.  Un *section* est une zone dans le fichier INF qui couvre un groupe logique de clés. Une section apparaît toujours entre parenthèses dans le fichier INF.
2.  Un *clé* est le paramètre qui est à gauche du signe égal.
3.  Un *valeur* est le paramètre qui apparaît à droite du signe égal.

Par exemple, un fichier INF minimal ressemblerait à ce qui suit :
```
[NewRequest] 
; At least one value must be set in this section 
Subject = "CN=W2K8-BO-DC.contoso2.com"
```
Voici quelques-uns des sections possibles qui peuvent être ajoutées au fichier INF :

**[NewRequest]**

Cette section est obligatoire pour un fichier INF qui agit en tant que modèle pour une nouvelle demande de certificat. Cette section requiert au moins une clé avec une valeur.

|Touche|Définition|Value|Exemple|
|---|----------|-----|-------|
|Objet|Plusieurs applications s’appuient sur les informations de sujet dans un certificat. Par conséquent, il est recommandé qu’une valeur pour cette clé est spécifiée. Si l’objet n’est pas défini ici, il est recommandé qu’un nom de sujet être inclus dans le cadre de l’extension de certificat autre nom de l’objet.|Valeurs de chaîne de nom unique relatifs|Subject = "CN=computer1.contoso.com" Subject="CN=John Smith,CN=Users,DC=Contoso,DC=com"|
|Exportable|Si cet attribut est défini sur TRUE, la clé privée peut être exportée avec le certificat. Pour garantir un haut niveau de sécurité, les clés privées ne doivent pas être exportables. Toutefois, dans certains cas, il peut être nécessaire pour rendre la clé privée exportable si plusieurs ordinateurs ou les utilisateurs doivent partager la même clé privée.|la valeur est true, false|Exportable = TRUE. Des clés CNG peuvent distinguer et exportable de texte en clair. Vous ne pouvez pas CAPI1 clés.|
|ExportableEncrypted|Spécifie si la clé privée doit être définie pour être exportable.|la valeur est true, false|ExportableEncrypted = true</br>Astuce : Pas toutes les tailles de clé publiques et les algorithmes fonctionnera avec tous les algorithmes de hachage. Tamehe spécifié que CSP doit prendre également en charge l’algorithme de hachage spécifié. Pour afficher la liste des algorithmes de hachage pris en charge, vous pouvez exécuter la commande <code>certutil -oid 1 &#124; findstr pwszCNGAlgid &#124; findstr /v CryptOIDInfo</code>|
|Élément HashAlgorithm Impossible|Algorithme de hachage à utiliser pour cette demande.|Sha256, sha384, sha512, sha1, md5, md4, md2|HashAlgorithm = sha1. Pour afficher la liste des algorithmes de hachage pris en charge, utilisez : certutil - oid 1 &#124; findstr pwszCNGAlgid &#124; findstr /v CryptOIDInfo|
|KeyAlgorithm|L’algorithme doit être utilisé par le fournisseur de services pour générer une paire de clés publique et privée.|RSA, DH, DSA, ECDH_P256, ECDH_P521, ECDSA_P256, ECDSA_P384, ECDSA_P521|KeyAlgorithm = RSA|
|KeyContainer|Il est déconseillé de définir ce paramètre pour les nouvelles requêtes où le nouveau matériel de clé est généré. Le conteneur de clé est automatiquement généré et géré par le système. Pour les demandes où le matériel de clé existant doit être utilisé, cette valeur peut être définie sur le nom de conteneur de clé de la clé existante. La commande certutil – la clé de commande pour afficher la liste des conteneurs de clés disponibles pour le contexte de l’ordinateur. La commande certutil – clé de commande de l’utilisateur pour le contexte de l’utilisateur actuel.|Valeur de chaîne aléatoire</br>Astuce : Vous devez utiliser des guillemets doubles autour de la valeur clé INF qui a des espaces ou des caractères spéciaux pour éviter d’éventuels problèmes d’analyse INF.|KeyContainer = {C347BD28-7F69-4090-AA16-BC58CF4D749C}|
|KeyLength|Définit la longueur de la clé publique et privée. La longueur de clé a un impact sur le niveau de sécurité du certificat. Longueur de clé supérieure fournit généralement un niveau de sécurité plus élevé ; Toutefois, certaines applications peuvent avoir des limitations concernant la longueur de clé.|N’importe quelle longueur de clé valide est pris en charge par le fournisseur de services de chiffrement.|KeyLength = 2048|
|KeySpec|Détermine si la clé peut être utilisée pour les signatures, pour Exchange (chiffrement) ou les deux.|AT_NONE, AT_SIGNATURE, AT_KEYEXCHANGE|KeySpec = AT_KEYEXCHANGE|
|KeyUsage|Définit la clé du certificat doit être utilisée pour.|CERT_DIGITAL_SIGNATURE_KEY_USAGE--80 (128)</br>Astuce : Les valeurs indiquées sont des valeurs (décimales) hexadécimales pour chaque définition de bits. Syntaxe plus ancienne peut également être utilisé : une valeur hexadécimale unique avec plusieurs bits ensemble, au lieu de la représentation symbolique. Par exemple, utilisation de clé = 0xa0.</br>CERT_NON_REPUDIATION_KEY_USAGE -- 40 (64)</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE -- 20 (32)</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE--10 (16)</br>CERT_KEY_AGREEMENT_KEY_USAGE -- 8</br>CERT_KEY_CERT_SIGN_KEY_USAGE -- 4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE -- 2</br>CERT_CRL_SIGN_KEY_USAGE -- 2</br>CERT_ENCIPHER_ONLY_KEY_USAGE--1</br>CERT_DECIPHER_ONLY_KEY_USAGE--8000 (32768)|KeyUsage = "CERT_DIGITAL_SIGNATURE_KEY_USAGE &#124; CERT_KEY_ENCIPHERMENT_KEY_USAGE"</br>Astuce : Plusieurs valeurs utiliseront un autre canal (&#124;) séparateur de symbole. Vérifiez que vous utilisez des guillemets doubles lors de l’utilisation de plusieurs valeurs afin d’éviter des INF problèmes d’analyse.|
|KeyUsageProperty|Récupère une valeur qui identifie l’objectif spécifique pour lequel une clé privée peut être utilisée.|NCRYPT_ALLOW_DECRYPT_FLAG -- 1</br>NCRYPT_ALLOW_SIGNING_FLAG -- 2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG -- 4</br>NCRYPT_ALLOW_ALL_USAGES--ffffff (16777215)|KeyUsageProperty = "NCRYPT_ALLOW_DECRYPT_FLAG &#124; NCRYPT_ALLOW_SIGNING_FLAG"|
|MachineKeySet|Cette clé est importante lorsque vous avez besoin créer des certificats qui sont détenus par l’ordinateur et non un utilisateur. Le matériel de clé généré est conservé dans le contexte de sécurité de l’entité de sécurité (compte utilisateur ou ordinateur) qui a créé la demande. Lorsqu’un administrateur crée une demande de certificat pour le compte d’un ordinateur, le matériel de clé doit être créé dans le contexte de sécurité de la machine et pas le contexte de sécurité administrateur. Sinon, l’ordinateur n’a pas pu accéder sa clé privée dans la mesure où il serait dans le contexte de sécurité administrateur.|la valeur est true, false|MachineKeySet = true</br>Astuce : La valeur par défaut est false.|
|NotBefore|Spécifie une date ou date et une heure avant laquelle la demande ne peut pas être émise. NotBefore peut être utilisé avec ValidityPeriod et ValidityPeriodUnits.|date ou date et heure|NotBefore = « 24/7/2012 10:31 AM »</br>Astuce : NotBefore et NotAfter concernent RequestType = cert uniquement. L’analyse de date tente d’être des paramètres régionaux. À l’aide de noms seront lever l’ambiguïté et doivent fonctionner dans tous les paramètres régionaux de mois.|
|NotAfter|Spécifie une date ou date et une heure après laquelle la demande ne peut pas être émise. NotAfter ne peut pas être utilisé avec ValidityPeriod ou ValidityPeriodUnits.|date ou date et heure|NotAfter = « 23/9/2014 10:31 AM »</br>Astuce : NotBefore et NotAfter concernent RequestType = cert uniquement. L’analyse de date tente d’être des paramètres régionaux. À l’aide de noms seront lever l’ambiguïté et doivent fonctionner dans tous les paramètres régionaux de mois.|
|PrivateKeyArchive|Le paramètre PrivateKeyArchive fonctionne uniquement si le RequestType correspondant est défini sur « CMC » autorisant uniquement les Messages de gestion de certificat sur le format de la requête CMS (CMC) pour le transfert en toute sécurité de clé privée du demandeur à l’autorité de certification pour l’archivage des clés.|la valeur est true, false|PrivateKeyArchive = True|
|EncryptionAlgorithm|L’algorithme de chiffrement à utiliser.|Les options possibles varient selon la version du système d’exploitation et l’ensemble des fournisseurs de services de chiffrement installés. Pour afficher la liste des algorithmes disponibles, exécutez la commande <code>certutil -oid 2 &#124; findstr pwszCNGAlgid</code> le CSP spécifié utilisé doit prennent également en charge l’algorithme de chiffrement symétrique spécifié et la longueur.|EncryptionAlgorithm = 3des|
|EncryptionLength|Longueur de l’algorithme de chiffrement à utiliser.|N’importe quelle longueur autorisée par le EncryptionAlgorithm spécifié.|EncryptionLength = 128|
|ProviderName|Le nom du fournisseur est le nom complet du CSP...|Si vous ne connaissez pas le nom du fournisseur du CSP que vous utilisez, exécutez certutil – csplist à partir d’une ligne de commande. La commande affiche les noms de tous les fournisseurs de services de cloud qui sont disponibles sur le système local|ProviderName = « Microsoft RSA SChannel Cryptographic Provider »|
|ProviderType|Le type de fournisseur est utilisé pour sélectionner des fournisseurs spécifiques en fonction de la fonctionnalité d’algorithme spécifiques tels que « RSA complet ».|Si vous ne connaissez pas le type de fournisseur du CSP que vous utilisez, exécutez certutil – csplist depuis une invite de ligne de commande. La commande affiche le type de fournisseur de tous les fournisseurs de services de cloud qui sont disponibles sur le système local.|ProviderType = 1|
|RenewalCert|Si vous avez besoin renouveler un certificat qui existe sur le système où la demande de certificat est générée, vous devez spécifier son hachage de certificat en tant que la valeur de cette clé.|Le hachage de certificat d’un certificat qui est disponible sur l’ordinateur sur lequel la demande de certificat est créée. Si vous ne connaissez pas le hachage de certificat, utilisez le composant logiciel enfichable MMC certificats et examinez le certificat doit être renouvelé. Ouvrez les propriétés du certificat et consultez l’attribut « Empreinte » du certificat. Renouvellement du certificat requiert un PKCS #7 ou un format de demande CMC.|RenewalCert = 4EDF274BD2919C6E9EC6A522F0F3B153E9B1582D|
|RequesterName</br>Remarque: Cela rend la demande s’inscrire au nom de la demande d’un autre utilisateur. La demande doit également être signée avec un certificat Enrollment Agent, ou l’autorité de certification rejette la demande. Utilisez - cert permet de spécifier le certificat agent d’inscription.|Le nom du demandeur peut être spécifié pour les demandes de certificat si le RequestType est défini sur PKCS #7 ou CMC. Si le RequestType est défini sur PKCS #10, cette clé sera ignorée. Le Requestername peut uniquement être défini comme partie de la demande. Vous ne pouvez pas manipuler le Requestername dans une demande en attente.|Domaine\nom d’utilisateur|RequesterName = « Contoso\BSmith »|
|RequestType|Détermine la norme qui est utilisée pour générer et envoyer la demande de certificat.|PKCS10 -- 1</br>PKCS7 -- 2</br>CMC -- 3</br>Cert -- 4</br>SCEP--fd00 (64768)</br>Astuce : Cette option indique un certificat auto-signé ou auto-émis. Il ne génère pas d’une demande, mais plutôt un nouveau certificat et puis installe le certificat. Auto-signé est la valeur par défaut. Spécifiez un certificat de signature à l’aide de – certificat permet de créer un certificat auto-émis qui n’est pas signé automatiquement.|RequestType = CMC|
|SecurityDescriptor</br>Astuce : Cela s’applique uniquement aux clés de carte à puce non - contexte ordinateur.|Contient les informations de sécurité associées aux objets sécurisables. Pour les objets sécurisables de plus, vous pouvez spécifier le descripteur de sécurité d’un objet dans l’appel de fonction qui crée l’objet.|Chaînes basées sur [langage de définition de descripteur de sécurité](https://msdn.microsoft.com/library/aa379567(v=vs.85).aspx).|SecurityDescriptor = "D:P(A;;GA;;;SY)(A;;GA;;;BA)"|
|AlternateSignatureAlgorithm|Spécifie et récupère une valeur booléenne qui indique si la signature algorithme identificateur d’objet (OID) d’une signature de demande ou de certificat PKCS #10 est discret ou combiné.|la valeur est true, false|AlternateSignatureAlgorithm = false</br>Astuce : Pour une signature RSA, false indique un v1.5 Pkcs1. True indique une signature v2.1.|
|Silencieux|Par défaut, cette option permet l’accès CSP pour les informations de demande et de bureau utilisateur interactif comme un code confidentiel de carte à puce à partir de l’utilisateur. Si cette clé est définie sur TRUE, le CSP ne doit pas interagir avec le bureau et ne peuvent pas afficher l’interface utilisateur à l’utilisateur.|la valeur est true, false|En mode silencieux = true|
|SMIME|Si ce paramètre est défini sur TRUE, une extension avec la valeur d’identificateur objet 1.2.840.113549.1.9.15 est ajoutée à la demande. Le nombre d’identificateurs d’objet dépend de la fonction la version de système d’exploitation installé et des capacités CSP, qui font référence à des algorithmes de chiffrement symétrique qui peuvent être utilisés par les applications de Secure Multipurpose Internet Mail Extensions (S/MIME), telles qu’Outlook.|la valeur est true, false|SMIME = true|
|UseExistingKeySet|Ce paramètre est utilisé pour spécifier qu’une paire de clés existante doit être utilisée dans la création d’une demande de certificat. Si cette clé est définie sur TRUE, vous devez également spécifier une valeur pour la clé RenewalCert ou le nom KeyContainer. Vous ne devez pas définir la clé Exportable, car vous ne pouvez pas modifier les propriétés d’une clé existante. Dans ce cas, aucun matériel de clé n’est générée lorsque la demande de certificat est générée.|la valeur est true, false|UseExistingKeySet = true|
|KeyProtection|Spécifie une valeur qui indique la façon dont une clé privée est protégée avant utilisation.|XCN_NCRYPT_UI_NO_PROTCTION_FLAG -- 0</br>XCN_NCRYPT_UI_PROTECT_KEY_FLAG -- 1</br>XCN_NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG -- 2|KeyProtection = NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG|
|SuppressDefaults|Spécifie une valeur booléenne qui indique si les extensions par défaut et les attributs sont inclus dans la demande. Les valeurs par défaut sont représentées par leurs identificateurs d’objet (OID).|la valeur est true, false|SuppressDefaults = true|
|FriendlyName|Un nom convivial pour le nouveau certificat.|Text|FriendlyName = « Server1 »|
|ValidityPeriodUnits</br>Remarque: Cela est utilisé uniquement lorsque la demande de type = cert.|Spécifie un nombre d’unités qui doit être utilisé avec ValidityPeriod.|Numérique|ValidityPeriodUnits = 3|
|ValidityPeriod</br>Remarque: Cela est utilisé uniquement lorsque la demande de type = cert.|VValidityPeriod doit être un anglais (États-Unis) au pluriel période de temps.|Années, mois, semaines, jours, heures, Minutes, secondes|ValidityPeriod = ans|

Retour à [contenu](#BKMK_Contents)

**[Extensions]**

Cette section est facultative.

|Extension OID|Définition|Value|Exemple|
|-------------|----------|-----|-------|
|2.5.29.17|||2.5.29.17 = « {text} »|
|_continue_|||_continuer_ = « UPN =User@Domain.com& »|
|_continue_|||_continuer_ = « E-mail =User@Domain.com& »|
|_continue_|||_continuer_ = « DNS=host.domain.com & »|
|_continue_|||_continue_ = "DirectoryName=CN=Name,DC=Domain,DC=com&"|
|_continue_|||_continue_ = "URL=http://host.domain.com/default.html&"|
|_continue_|||_continue_ = "IPAddress=10.0.0.1&"|
|_continue_|||_continue_ = "RegisteredId=1.2.3.4.5&"|
|_continue_|||_continue_ = "1.2.3.4.6.1={utf8}String&"|
|_continue_|||_continuer_ = « 1.2.3.4.6.2={octet}AAECAwQFBgc= & »|
|_continue_|||_continue_ = "1.2.3.4.6.2={octet}{hex}00 01 02 03 04 05 06 07&"|
|_continue_|||_continue_ = "1.2.3.4.6.3={asn}BAgAAQIDBAUGBw==&"|
|_continue_|||_continue_ = "1.2.3.4.6.3={hex}04 08 00 01 02 03 04 05 06 07"|
|2.5.29.37|||2.5.29.37="{text}"|
|_continue_|||_continuer_ = « 1.3.6.1.5.5.7.|
|_continue_|||_continue_ = "1.3.6.1.5.5.7.3.1"|
|2.5.29.19|||"{text}ca=0pathlength=3"|
|Critique|||Critical=2.5.29.19|
|KeySpec|||AT_NONE -- 0</br>AT_SIGNATURE -- 2</br>AT_KEYEXCHANGE--1|
|RequestType|||PKCS10 -- 1</br>PKCS7 -- 2</br>CMC -- 3</br>Cert -- 4</br>SCEP--fd00 (64768)|
|KeyUsage|||CERT_DIGITAL_SIGNATURE_KEY_USAGE--80 (128)</br>CERT_NON_REPUDIATION_KEY_USAGE -- 40 (64)</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE -- 20 (32)</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE--10 (16)</br>CERT_KEY_AGREEMENT_KEY_USAGE -- 8</br>CERT_KEY_CERT_SIGN_KEY_USAGE -- 4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE -- 2</br>CERT_CRL_SIGN_KEY_USAGE -- 2</br>CERT_ENCIPHER_ONLY_KEY_USAGE--1</br>CERT_DECIPHER_ONLY_KEY_USAGE--8000 (32768)|
|KeyUsageProperty|||NCRYPT_ALLOW_DECRYPT_FLAG -- 1</br>NCRYPT_ALLOW_SIGNING_FLAG -- 2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG -- 4</br>NCRYPT_ALLOW_ALL_USAGES--ffffff (16777215)|
|KeyProtection|||NCRYPT_UI_NO_PROTECTION_FLAG -- 0</br>NCRYPT_UI_PROTECT_KEY_FLAG -- 1</br>NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG -- 2|
|SubjectNameFlags|modèle||CT_FLAG_SUBJECT_REQUIRE_COMMON_NAME -- 40000000 (1073741824)</br>CT_FLAG_SUBJECT_REQUIRE_DIRECTORY_PATH -- 80000000 (2147483648)</br>CT_FLAG_SUBJECT_REQUIRE_DNS_AS_CN -- 10000000 (268435456)</br>CT_FLAG_SUBJECT_REQUIRE_EMAIL -- 20000000 (536870912)</br>CT_FLAG_OLD_CERT_SUPPLIES_SUBJECT_AND_ALT_NAME -- 8</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DIRECTORY_GUID -- 1000000 (16777216)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DNS -- 8000000 (134217728)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DOMAIN_DNS -- 400000 (4194304)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_EMAIL -- 4000000 (67108864)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_SPN -- 800000 (8388608)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_UPN -- 2000000 (33554432)|
|X500NameFlags|||CERT_NAME_STR_NONE -- 0</br>CERT_OID_NAME_STR -- 2</br>CERT_X500_NAME_STR -- 3</br>CERT_NAME_STR_SEMICOLON_FLAG -- 40000000 (1073741824)</br>CERT_NAME_STR_NO_PLUS_FLAG -- 20000000 (536870912)</br>CERT_NAME_STR_NO_QUOTING_FLAG -- 10000000 (268435456)</br>CERT_NAME_STR_CRLF_FLAG -- 8000000 (134217728)</br>CERT_NAME_STR_COMMA_FLAG -- 4000000 (67108864)</br>CERT_NAME_STR_REVERSE_FLAG -- 2000000 (33554432)</br>CERT_NAME_STR_FORWARD_FLAG -- 1000000 (16777216)</br>CERT_NAME_STR_DISABLE_IE4_UTF8_FLAG -- 10000 (65536)</br>CERT_NAME_STR_ENABLE_T61_UNICODE_FLAG -- 20000 (131072)</br>CERT_NAME_STR_ENABLE_UTF8_UNICODE_FLAG -- 40000 (262144)</br>CERT_NAME_STR_FORCE_UTF8_DIR_STR_FLAG -- 80000 (524288)</br>CERT_NAME_STR_DISABLE_UTF8_DIR_STR_FLAG -- 100000 (1048576)</br>CERT_NAME_STR_ENABLE_PUNYCODE_FLAG -- 200000 (2097152)|

Retour à [contenu](#BKMK_Contents)

> [!NOTE]
> SubjectNameFlags permet au fichier INF spécifier les champs d’extension de sujet et SubjectAltName doivent être rempli automatiquement par certreq en fonction de l’utilisateur actuel ou les propriétés de l’ordinateur actuel : Nom DNS, UPN et ainsi de suite. À l’aide du littéral « modèle » signifie que les indicateurs de nom de modèle sont utilisés à la place. Ainsi, un seul fichier INF à utiliser dans plusieurs contextes pour générer des requêtes avec des informations de l’objet de contexte spécifique.
>
> X500NameFlags spécifie les indicateurs à passer directement à l’API de CertStrToName quand la valeur de clés INF du sujet est convertie en un ASN.1 codé nom unique.

Pour demander un certificat à l’aide de certreq-nouveau utilisez les étapes de l’exemple ci-dessous :

> [!WARNING]
> Le contenu de cette rubrique est basé sur les paramètres par défaut pour Windows Server 2008 AD CS ; par exemple, si la longueur de clé 2048, en sélectionnant Microsoft Software Key Storage Provider en tant que le fournisseur de services cryptographiques et à l’aide de Secure Hash Algorithm 1 (SHA1). Évaluez ces sélections sur les exigences de stratégie de sécurité de votre entreprise.

Pour créer une copie du fichier de stratégie (.inf) et enregistrez l’exemple ci-dessous dans le bloc-notes et l’enregistrer en tant que RequestConfig.inf :
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
Sur l’ordinateur pour lequel vous demandez un certificat, tapez la commande suivante :
```
CertReq –New RequestConfig.inf CertRequest.req 

```
L’exemple suivant illustre l’implémentation de la syntaxe de la section [Strings] pour OID et autres difficile d’interpréter les données. Le nouvel exemple de syntaxe {text} pour l’extension EKU, qui utilise une liste séparée par des virgules des OID :
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
Retour à [contenu](#BKMK_Contents)

## <a name="BKMK_accept"></a>Certreq -accept

```
CertReq -accept [Options] [CertChainFileIn | FullResponseFileIn | CertFileIn]
```
– Accepter le paramètre lie la clé privée générée précédemment avec le certificat émis et supprime la demande de certificat en attente du système où le certificat est demandé (s’il existe une correspondance demande).

Vous pouvez utiliser cet exemple pour accepter manuellement un certificat :
```
certreq -accept certnew.cer 

```

> [!WARNING]
> -Accepter verbe, la barre d’outils - options utilisateur et – machine indiquent si le certificat en cours d’installation doit être installé dans un utilisateur ou le contexte de l’ordinateur. S’il existe une demande en attente dans un contexte qui correspond à la clé publique en cours d’installation, ces options ne sont pas nécessaires. S’il n’existe aucune demande en attente, un d’eux doit être spécifié.

Retour à [contenu](#BKMK_Contents)

## <a name="BKMK_policy"></a>Certreq -policy

```
certreq -policy [-attrib AttributeString] [-binary] [-cert CertID] [RequestFileIn [PolicyFileIn [RequestFileOut [PKCS10FileOut]]]]
```
-   Le fichier de configuration qui définit les contraintes sont appliquées à un certificat d’autorité de certification lors de la subordination qualifiée est définie est appelé Policy.inf...
-   Vous trouverez un exemple de fichier Policy.inf dans le [annexe A de planification et de Implementing Cross-Certification et de la Subordination qualifiée](https://technet.microsoft.com/library/cc738878(WS.10).aspx) livre blanc.
-   Si vous tapez le certreq-stratégie sans aucun paramètre supplémentaire il ouvrira une fenêtre de dialogue afin de pouvoir le demandé fichier (req, cmc, txt, der, cer ou crt). Une fois que vous sélectionnez le fichier demandé et cliquez sur le bouton Ouvrir, une autre fenêtre de boîte de dialogue s’ouvre afin de sélectionner le fichier INF.

Vous pouvez utiliser cet exemple pour générer une demande de certificat croisé :
```
certreq -policy Certsrv.req Policy.inf newcertsrv.req 

```
Retour à [contenu](#BKMK_Contents)

## <a name="BKMK_sign"></a>Certreq -sign

```
certreq -sign [Options] [RequestFileIn [RequestFileOut]]
```
-   Si vous tapez le certreq-connexion sans aucun paramètre supplémentaire il ouvre une fenêtre de boîte de dialogue pour vous pouvez de sélectionner le fichier demandé (req, cmc, txt, der, cer ou crt).
-   Signature de la demande de subordination qualifiée peut nécessiter des informations d’identification d’administrateur d’entreprise. Il s’agit d’une meilleure pratique pour émettre des certificats de signatures pour la subordination qualifiée.
-   Le certificat utilisé pour signer la demande de subordination qualifiée est créé à l’aide du modèle de subordination qualifiée. Administrateurs de l’entreprise aura signer la demande ou d’accorder des autorisations utilisateur pour les personnes qui signeront le certificat.
-   Lorsque vous vous connectez à la demande CMC, vous devrez peut-être avoir plusieurs personnel signer cette demande, selon le niveau d’assurance qui est associé à la subordination qualifiée.
-   Si l’autorité de certification parente de l’autorité de certification que vous installez est hors connexion, vous devez obtenir le certificat d’autorité de certification pour l’autorité de certification à partir du parent hors connexion. Si l’autorité de certification parente est en ligne, spécifiez le certificat d’autorité de certification pour l’autorité de certification lors de l’Assistant Installation de Services de certificats.

La séquence de commandes ci-dessous vous montreront comment créer une nouvelle demande de certificat, signez-le et envoyez-le :
```
certreq -new policyfile.inf MyRequest.req
certreq -sign MyRequest.req MyRequest_Sign.req
certreq -submit MyRequest_Sign.req MyRequest_cert.cer 

```
Retour à [contenu](#BKMK_Contents)

## <a name="BKMK_enroll"></a>Certreq -enroll

S’inscrire sur un certificat
```
certreq –enroll [Options] TemplateName
```
Pour renouveler un certificat existant
```
certreq –enroll –cert CertId [Options] Renew [ReuseKeys]
```
Vous pouvez uniquement renouveler les certificats qui ont une durée valide. Certificats arrivés à expiration ne peut pas être renouvelées et doivent être remplacés par un nouveau certificat.

Voici un exemple de renouveler un certificat à l’aide de son numéro de série :
```
certreq –enroll -machine –cert "61 2d 3c fe 00 00 00 00 00 05" Renew
```
Ici un exemple de l’inscription d’un modèle de certificat appelé serveur Web à l’aide de l’astérisque (*) pour sélectionner le serveur de stratégie via U/i :
```
certreq -enroll –machine –policyserver * "WebServer"
```
Retour à [contenu](#BKMK_Contents)

## <a name="BKMK_Options"></a>Options

|Options|Description|
|-------|-----------|
|-tout|Force ICertRequest::soumettre pour déterminer le type de codage.|
|-attrib \<Chaîne_attribut >|Spécifie les paires de chaîne de nom et la valeur, séparées par un signe deux-points.</br>Les paires de chaînes de nom et la valeur distinct avec \n (par exemple, Name1:Value1\nName2:Value2).|
|-binaire|Formats de fichiers de sortie binaire et non codée en base64.|
|-PolicyServer  *\<PolicyServer >*|« ldap :  *\<chemin d’accès >*»</br>Insérer l’URI ou un ID unique pour un ordinateur exécutant le Service Web Stratégie de certificat d’inscription.</br>Pour spécifier que vous souhaitez utiliser un fichier de demande en accédant, il suffit d’utiliser un signe moins (-) pour l’authentification  *\<policyserver >*.|
|-config \<ConfigString>|Traite l’opération à l’aide de l’autorité de certification spécifiée dans la chaîne de configuration, qui est CAHostName\CAName. Pour une connexion https, spécifiez l’URI du serveur d’inscription. Pour l’ordinateur local stocker l’autorité de certification, utilisez un signe moins (-) signe.|
|-Anonyme|Utilisez les informations d’identification anonymes pour les Services Web Inscription de certificats.|
|-Kerberos|Utilisez les informations d’identification Kerberos (domaine) pour les Services Web Inscription de certificats.|
|-ClientCertificate *\<ClientCertId>*|Vous pouvez remplacer le  *\<ClientCertID >* avec un empreinte numérique du certificat CN, EKU, modèle, e-mail, UPN et le nouveau nom = syntaxe de valeur.|
|-UserName  *\<nom d’utilisateur >*|Utilisé avec les Services Web Inscription de certificats. Vous pouvez remplacer  *\<nom d’utilisateur >* avec le nom SAM ou domaine\utilisateur. Cette option doit être utilisé avec l’option -p.|
|p -  *\<mot de passe >*|Utilisé avec les Services Web Inscription de certificats. Substitute  *\<mot de passe >* avec mot de passe de l’utilisateur réel. Cette option doit être utilisé avec l’option - UserName.|
|-utilisateur|Configure-contexte utilisateur pour une nouvelle demande de certificat ou spécifie le contexte pour une acceptation du certificat. Il s’agit du contexte par défaut, si aucun n’est spécifié dans le modèle ou INF.|
|-machine|Configure une nouvelle demande de certificat ou spécifie le contexte pour une une acceptation de certificat pour le contexte de l’ordinateur. Pour les nouvelles requêtes, il doit être cohérente avec la clé MachineKeyset INF et au contexte de modèle. Si cette option n’est pas spécifiée et que le modèle ne définit pas un contexte, la valeur par défaut est le contexte de l’utilisateur.|
|-crl|Inclut le certificat (listes de révocation) dans la sortie dans le fichier PKCS #7 codé en base64 spécifié par CertChainFileOut ou dans le fichier codé en base64 spécifié par RequestFileOut.|
|-rpc|Indique les Services de certificats Active Directory (AD CS) à utiliser une connexion de serveur de procédure distante (RPC) appel au lieu de Distributed de COM.|
|-AdminForceMachine|Utilisez la clé de Service ou l’emprunt d’identité pour envoyer la demande à partir du contexte du système Local. Nécessite que l’utilisateur appelant cette option soit un membre du groupe Administrateurs Local.|
|-RenewOnBehalfOf|Envoyer un renouvellement pour le compte de l’objet identifié dans le certificat de signature. Cela définit CR_IN_ROBO lors de l’appel [ICertRequest::soumettre](https://technet.microsoft.com/subscriptions/aa385040.aspx)|
|-f|Forcer les fichiers existants doivent être remplacées. Cela contourne également la mise en cache de modèles et de stratégie.|
|-q|Utiliser le mode silencieux ; supprimer toutes les invites interactives.|
|-Unicode|Écrit de sortie Unicode lors de la sortie standard est redirigée ou dirigée vers une autre commande, ce qui vous aide lorsqu’elle est appelée à partir de scripts de Windows PowerShell®).|
|-UnicodeText|Envoie la sortie Unicode lors de l’écriture de texte base64 encodée des objets BLOB dans des fichiers.|

Retour à [contenu](#BKMK_Contents)

## <a name="BKMK_Formats"></a>Formats

|Met en forme|Description|
|-------|-----------|
|RequestFileIn|Nom du fichier d’entrée binaire ou codé en Base64 : Demande de certificat PKCS #10, demande de certificat CMS, demande de renouvellement de certificat PKCS #7, certificat X.509 à être certification croisée ou une demande de certificat format KeyGen.|
|RequestFileOut|Nom de fichier de sortie codée en Base64|
|CertFileOut|Nom du fichier x-509 codé en Base64.|
|PKCS10FileOut|Pour une utilisation avec le [Certreq-stratégie](#BKMK_policy) verbe uniquement. Nom de fichier PKCS10 sortie codée en Base64.|
|CertChainFileOut|Nom du fichier PKCS #7 codé en Base64.|
|FullResponseFileOut|Nom de fichier codé en Base64 la réponse complète.|
|PolicyFileIn|Pour une utilisation avec le [Certreq-stratégie](#BKMK_policy) verbe uniquement. Fichier INF contenant une représentation textuelle des extensions utilisées pour qualifier une demande.|

## <a name="BKMK_Examples"></a>Exemples de certreq supplémentaires

Les articles suivants contiennent des exemples d’utilisation de certreq :
-   [Comment demander un certificat avec un autre nom d’objet personnalisé](https://technet.microsoft.com/library/ff625722.aspx)
-   [Guide de laboratoire de test : Déploiement d’une hiérarchie d’infrastructure à clé publique AD CS deux couches](https://technet.microsoft.com/library/hh831348.aspx)
-   [Annexe 3 : Syntaxe de certreq.exe](https://technet.microsoft.com/library/cc736326.aspx)
-   [Comment créer un certificat SSL du serveur web manuellement](http://blogs.technet.com/b/pki/archive/2009/08/05/how-to-create-a-web-server-ssl-certificate-manually.aspx)
-   [Demandez un certificat à l’aide d’une autorité de certification Windows Server 2008 de configuration AMT](https://social.technet.microsoft.com/wiki/contents/articles/request-an-amt-provisioning-certificate-using-a-windows-server-2008-ca.aspx)
-   [Inscription de certificats pour l’Agent de System Center Operations Manager](https://social.technet.microsoft.com/wiki/contents/articles/certificate-enrollment-for-system-center-operations-manager-agent.aspx)
-   [Guide détaillé de AD CS : Déploiement de hiérarchie PKI à deux niveaux](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)
-   [Comment activer le protocole LDAP sur SSL avec une autorité de certification tierce](https://support.microsoft.com/kb/321051)

Retour à [contenu](#BKMK_Contents)
