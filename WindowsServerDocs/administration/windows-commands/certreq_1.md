---
title: certreq
description: Rubrique de référence pour la commande Certreq, qui demande des certificats auprès d’une autorité de certification, récupère une réponse à une demande précédente à partir d’une autorité de certification, crée une nouvelle demande à partir d’un fichier. inf, accepte et installe une réponse à une demande, construit une demande de certification croisée ou de subordination qualifiée à partir d’un certificat ou d’une demande d’autorité de certification, puis signe une demande de certification
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7a04e51f-f395-4bff-b57a-0e9efcadf973
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 14fc717ad49a676387206692af32842f212c4296
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719646"
---
# <a name="certreq"></a>certreq

La commande Certreq peut être utilisée pour demander des certificats à une autorité de certification (CA), pour récupérer une réponse à une demande précédente à partir d’une autorité de certification, pour créer une demande à partir d’un fichier. inf, pour accepter et installer une réponse à une demande, pour construire une demande de certification croisée ou de subordination qualifiée à partir d’un certificat ou d’une demande d’autorité de certification, et pour signer une demande de certification croisée ou de subordination qualifiée

> [!IMPORTANT]
> Les versions antérieures de la commande Certreq peuvent ne pas fournir toutes les options décrites ici. Pour afficher les options prises en charge en fonction de versions spécifiques de Certreq, exécutez l’option d’aide `certreq -v -?`de la ligne de commande,.
>
> La commande Certreq ne prend pas en charge la création d’une demande de certificat basée sur un modèle d’attestation de clé dans un environnement CEP/ec.

> [!WARNING]
> Le contenu de cette rubrique est basé sur les paramètres par défaut de Windows Server. par exemple, la définition de la longueur de clé sur 2048, la sélection du fournisseur de stockage de clés logicielles Microsoft comme CSP et l’utilisation de Secure Hash Algorithm 1 (SHA1). Évaluez ces sélections par rapport aux exigences de la stratégie de sécurité de votre entreprise.

## <a name="syntax"></a>Syntaxe

```
certreq [-submit] [options] [requestfilein [certfileout [certchainfileout [fullresponsefileOut]]]]
certreq -retrieve [options] requestid [certfileout [certchainfileout [fullresponsefileOut]]]
certreq -new [options] [policyfilein [requestfileout]]
certreq -accept [options] [certchainfilein | fullresponsefilein | certfilein]
certreq -sign [options] [requestfilein [requestfileout]]
certreq –enroll [options] templatename
certreq –enroll –cert certId [options] renew [reusekeys]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------- | ----------- |
| -Envoyer | Envoie une demande à une autorité de certification. |
| -récupérer`<requestid>` | Récupère une réponse à une demande précédente d’une autorité de certification. |
| -nouveau | Crée une nouvelle demande à partir d’un fichier. inf. |
| -accepter | Accepte et installe une réponse à une demande de certificat. |
| -stratégie | Définit la stratégie pour une demande. |
| -Sign | Signe une demande de certification croisée ou de subordination qualifiée. |
| -inscrire | Inscrit ou renouvelle un certificat. |
| -? | Affiche une liste de la syntaxe, des options et des descriptions Certreq. |
| `<parameter>` -? | Affiche l’aide pour le paramètre spécifié. |
| -v- ? | Affiche une liste détaillée de la syntaxe, des options et des descriptions Certreq. |

## <a name="examples"></a>Exemples

### <a name="certreq--submit"></a>Certreq-envoyer

Pour soumettre une demande de certificat simple :

```
certreq –submit certrequest.req certnew.cer certnew.pfx
```

#### <a name="remarks"></a>Notes 

- Il s’agit du paramètre Certreq. exe par défaut. Si aucune option n’est spécifiée à l’invite de ligne de commande, Certreq. exe tente de soumettre une demande de certificat à une autorité de certification. Vous devez spécifier un fichier de demande de certificat lors de l’utilisation de l’option **– Submit** . Si ce paramètre est omis, une fenêtre d' **ouverture de fichier** courante s’affiche, vous permettant de sélectionner le fichier de demande de certificat approprié.

- Pour demander un certificat en spécifiant l’attribut SAN, consultez la section *utilisation de l’utilitaire Certreq. exe pour créer et soumettre une demande de certificat* de l’article 931351 de la base de connaissances Microsoft [comment ajouter un autre nom d’objet à un certificat LDAP sécurisé](https://support.microsoft.com/kb/931351).

### <a name="certreq--retrieve"></a>Certreq-récupérer

Pour récupérer l’ID de certificat 20 et créer un fichier de certificat (. cer), nommé *MyCertificate*:

```
certreq -retrieve 20 MyCertificate.cer
```

#### <a name="remarks"></a>Notes 

- Utilisez certreq-retrieve *RequestId* pour récupérer le certificat une fois que l’autorité de certification l’a émis. La PKC *RequestId* peut être une valeur décimale ou hexadécimale avec un préfixe 0x, et il peut s’agir d’un numéro de série de certificat sans préfixe 0x. Vous pouvez également l’utiliser pour récupérer tout certificat émis par l’autorité de certification, y compris les certificats révoqués ou expirés, sans tenir compte de l’état d’attente de la demande du certificat.

- Si vous soumettez une demande à l’autorité de certification, le module de stratégie de l’autorité de certification peut conserver la demande dans un état d’attente et retourner l' *ID* de requête à l’appelant CertReq pour l’affichage. Finalement, l’administrateur de l’autorité de certification émettra le certificat ou refusera la demande.

### <a name="certreq--new"></a>Certreq-nouveau

Pour créer une nouvelle demande :

```
[newrequest]
; At least one value must be set in this section
subject = CN=W2K8-BO-DC.contoso2.com
```

Voici quelques-unes des sections possibles qui peuvent être ajoutées au fichier INF :

#### <a name="newrequest"></a>[newrequest]

Cette zone du fichier INF est obligatoire pour tous les nouveaux modèles de demande de certificat et doit inclure au moins un paramètre avec une valeur.

| Clé<sup>1</sup> | Description | Valeur<sup>2</sup> |  Exemple |
| --- | ---------- | ----- | ------- |
| Objet | Plusieurs applications reposent sur les informations d’objet d’un certificat. Nous vous recommandons de spécifier une valeur pour cette clé. Si l’objet n’est pas défini ici, nous vous recommandons d’inclure un nom d’objet dans le cadre de l’extension de certificat de l’autre nom de l’objet. | Valeurs de chaîne de nom unique relatif | Subject = CN = ordinateur1. contoso. com subject = CN = John Smith, CN = Users, DC = contoso, DC = com |
| Exportable | Si la valeur est TRUE, la clé privée peut être exportée avec le certificat. Pour garantir un niveau de sécurité élevé, les clés privées ne doivent pas être exportées ; Toutefois, dans certains cas, il peut être nécessaire si plusieurs ordinateurs ou utilisateurs doivent partager la même clé privée. | `true | false` | `Exportable = TRUE`. Les clés CNG peuvent faire la distinction entre ce texte et le texte en clair pouvant être exportés. Les clés CAPI1 ne peuvent pas. |
| ExportableEncrypted | Spécifie si la clé privée doit être configurée pour être exportable. | `true | false` | `ExportableEncrypted = true`<p>**Conseil :** Tous les algorithmes et tailles de clé publique ne fonctionnent pas avec tous les algorithmes de hachage. Le fournisseur de services de chiffrement spécifié doit également prendre en charge l’algorithme de hachage spécifié. Pour afficher la liste des algorithmes de hachage pris en charge, vous pouvez exécuter la commande :`certutil -oid 1 | findstr pwszCNGAlgid | findstr /v CryptOIDInfo` |
| HashAlgorithm | Algorithme de hachage à utiliser pour cette requête. | `Sha256, sha384, sha512, sha1, md5, md4, md2` | `HashAlgorithm = sha1`. Pour afficher la liste des algorithmes de hachage pris en charge, utilisez : certutil-OID 1 | findstr pwszCNGAlgid | findstr/v CryptOIDInfo|
| KeyAlgorithm| Algorithme qui sera utilisé par le fournisseur de services pour générer une paire de clés publique et privée.| `RSA, DH, DSA, ECDH_P256, ECDH_P521, ECDSA_P256, ECDSA_P384, ECDSA_P521` | `KeyAlgorithm = RSA` |
| KeyContainer | Nous vous déconseillons de définir ce paramètre pour les nouvelles demandes où de nouveaux éléments de clé sont générés. Le conteneur de clé est généré et géré automatiquement par le système.<p>Pour les demandes où le matériel de clé existant doit être utilisé, cette valeur peut être définie sur le nom du conteneur de clés de la clé existante. Utilisez la `certutil –key` commande pour afficher la liste des conteneurs de clés disponibles pour le contexte de l’ordinateur. Utilisez la `certutil –key –user` commande pour le contexte de l’utilisateur actuel.| Valeur de chaîne aléatoire<p>**Conseil :** Utilisez des guillemets doubles autour de toute valeur de clé INF comportant des espaces ou des caractères spéciaux pour éviter les éventuels problèmes d’analyse de fichier INF. | `KeyContainer = {C347BD28-7F69-4090-AA16-BC58CF4D749C}` |
| KeyLength | Définit la longueur de la clé publique et privée. La longueur de la clé a un impact sur le niveau de sécurité du certificat. Une longueur de clé supérieure fournit généralement un niveau de sécurité plus élevé. Toutefois, certaines applications peuvent avoir des limitations quant à la longueur de la clé. | Toute longueur de clé valide prise en charge par le fournisseur de services de chiffrement. | `KeyLength = 2048` |
| KeySpec | Détermine si la clé peut être utilisée pour les signatures, pour Exchange (chiffrement), ou pour les deux. | `AT_NONE, AT_SIGNATURE, AT_KEYEXCHANGE` | `KeySpec = AT_KEYEXCHANGE` |
| Utilisation | Définit la fonction pour laquelle la clé de certificat doit être utilisée. | <ul><li>`CERT_DIGITAL_SIGNATURE_KEY_USAGE -- 80 (128)`</li><li>`CERT_NON_REPUDIATION_KEY_USAGE -- 40 (64)`</li><li>`CERT_KEY_ENCIPHERMENT_KEY_USAGE -- 20 (32)`</li><li>`CERT_DATA_ENCIPHERMENT_KEY_USAGE -- 10 (16)`</li><li>`CERT_KEY_AGREEMENT_KEY_USAGE -- 8`</li><li>`CERT_KEY_CERT_SIGN_KEY_USAGE -- 4`</li><li>`CERT_OFFLINE_CRL_SIGN_KEY_USAGE -- 2`</li><li>`CERT_CRL_SIGN_KEY_USAGE -- 2`</li><li>`CERT_ENCIPHER_ONLY_KEY_USAGE -- 1`</li><li>`CERT_DECIPHER_ONLY_KEY_USAGE -- 8000 (32768)`</li></ul> | `KeyUsage = CERT_DIGITAL_SIGNATURE_KEY_USAGE | CERT_KEY_ENCIPHERMENT_KEY_USAGE`<p>**Conseil :** Plusieurs valeurs utilisent un canal (|) séparateur de symboles. Veillez à utiliser des guillemets doubles lors de l’utilisation de plusieurs valeurs pour éviter les problèmes d’analyse INF. Les valeurs affichées sont des valeurs hexadécimales (décimales) pour chaque définition de bit. Une syntaxe plus ancienne peut également être utilisée : une valeur hexadécimale unique avec plusieurs bits définis, au lieu de la représentation symbolique. Par exemple : `KeyUsage = 0xa0`. |
| KeyUsageProperty | Récupère une valeur qui identifie l’objectif spécifique pour lequel une clé privée peut être utilisée. | <ul><li>`NCRYPT_ALLOW_DECRYPT_FLAG -- 1`</li><li>`NCRYPT_ALLOW_SIGNING_FLAG -- 2`</li><li>`NCRYPT_ALLOW_KEY_AGREEMENT_FLAG -- 4`</li><li>`NCRYPT_ALLOW_ALL_USAGES -- ffffff (16777215)`</li></ul> | `KeyUsageProperty = NCRYPT_ALLOW_DECRYPT_FLAG | NCRYPT_ALLOW_SIGNING_FLAG` |
| MachineKeySet | Cette clé est importante lorsque vous devez créer des certificats appartenant à la machine et non à un utilisateur. Le matériel de clé généré est conservé dans le contexte de sécurité du principal de sécurité (compte d’utilisateur ou d’ordinateur) qui a créé la demande. Lorsqu’un administrateur crée une demande de certificat pour le compte d’un ordinateur, le matériel de clé doit être créé dans le contexte de sécurité de l’ordinateur et non dans le contexte de sécurité de l’administrateur. Dans le cas contraire, l’ordinateur n’a pas pu accéder à sa clé privée, car il se trouve dans le contexte de sécurité de l’administrateur. | `true | false`. La valeur par défaut est false. | `MachineKeySet = true` |
| NotBefore | Spécifie une date ou une date et une heure avant lesquelles la demande ne peut pas être émise. `NotBefore`peut être utilisé avec `ValidityPeriod` et `ValidityPeriodUnits`. | Date ou date et heure | `NotBefore = 7/24/2012 10:31 AM`<p>**Conseil :** `NotBefore` et `NotAfter` sont destinés à`equestType=cert` R uniquement. La date d’analyse des tentatives est sensible aux paramètres régionaux. L’utilisation de noms de mois lève une ambiguïté et doit fonctionner dans tous les paramètres régionaux. |
| NotAfter | Spécifie une date ou une date et une heure au-delà desquelles la demande ne peut pas être émise. `NotAfter`ne peut pas être `ValidityPeriod` utilisé `ValidityPeriodUnits`avec ou. | Date ou date et heure | `NotAfter = 9/23/2014 10:31 AM`<p>**Conseil :** `NotBefore` et `NotAfter` sont destinés `RequestType=cert` uniquement à. La date d’analyse des tentatives est sensible aux paramètres régionaux. L’utilisation de noms de mois lève une ambiguïté et doit fonctionner dans tous les paramètres régionaux. |
| PrivateKeyArchive | Le paramètre PrivateKeyArchive fonctionne uniquement si le RequestType correspondant est défini sur CMC, car seuls les messages de gestion des certificats au format de demande CMS (CMC) permettent de transférer en toute sécurité la clé privée du demandeur à l’autorité de certification pour l’archivage de clé. | `true | false` | `PrivateKeyArchive = true` |
| EncryptionAlgorithm | Algorithme de chiffrement à utiliser. | Les options possibles varient en fonction de la version du système d’exploitation et de l’ensemble des fournisseurs de services de chiffrement installés. Pour afficher la liste des algorithmes disponibles, exécutez la commande suivante `certutil -oid 2 | findstr pwszCNGAlgid`:. Le CSP spécifié doit également prendre en charge l’algorithme de chiffrement symétrique et la longueur spécifiés. | `EncryptionAlgorithm = 3des` |
| EncryptionLength | Longueur de l’algorithme de chiffrement à utiliser. | Toute longueur autorisée par le EncryptionAlgorithm spécifié. | `EncryptionLength = 128` |
| ProviderName | Le nom du fournisseur est le nom d’affichage du CSP. | Si vous ne connaissez pas le nom du fournisseur du CSP que vous utilisez, `certutil –csplist` exécutez à partir d’une ligne de commande. La commande affiche les noms de tous les fournisseurs de services de chiffrement disponibles sur le système local. | `ProviderName = Microsoft RSA SChannel Cryptographic Provider` |
| ProviderType | Le type de fournisseur est utilisé pour sélectionner des fournisseurs spécifiques basés sur des fonctionnalités d’algorithme spécifiques telles que RSA Full. | Si vous ne connaissez pas le type de fournisseur du CSP que vous utilisez, exécutez `certutil –csplist` à partir d’une invite de ligne de commande. La commande affiche le type de fournisseur de tous les fournisseurs de services de chiffrement disponibles sur le système local. | `ProviderType = 1` |
| RenewalCert | Si vous devez renouveler un certificat qui existe sur le système où la demande de certificat est générée, vous devez spécifier son hachage de certificat comme valeur pour cette clé. | Hachage de certificat de tout certificat disponible sur l’ordinateur où la demande de certificat est créée. Si vous ne connaissez pas le hachage du certificat, utilisez le composant logiciel enfichable MMC certificats et examinez le certificat qui doit être renouvelé. Ouvrez les propriétés du certificat et consultez `Thumbprint` l’attribut du certificat. Le renouvellement du certificat requiert `PKCS#7` un ou `CMC` un format de demande. | `RenewalCert = 4EDF274BD2919C6E9EC6A522F0F3B153E9B1582D` |
| RequesterName | Effectue la demande d’inscription pour le compte d’une autre demande d’utilisateur. La demande doit également être signée avec un certificat d’agent d’inscription, ou l’autorité de certification rejette la demande. Utilisez l' `-cert` option pour spécifier le certificat de l’agent d’inscription. Le nom du demandeur peut être spécifié pour les demandes de certificat `RequestType` si a la `PKCS#7` valeur `CMC`ou. Si a `RequestType` la valeur `PKCS#10`, cette clé sera ignorée. Le `Requestername` ne peut être défini que dans le cadre de la requête. Vous ne pouvez pas `Requestername` manipuler la dans une demande en attente. | `Domain\User` | `Requestername = Contoso\BSmith` |
| RequestType | Détermine la norme utilisée pour générer et envoyer la demande de certificat. | <ul><li>`PKCS10 -- 1`</li><li>`PKCS7 -- 2`</li><li>`CMC -- 3`</li><li>`Cert -- 4`</li><li>`SCEP -- fd00 (64768)`</li></ul>**Conseil :** Cette option indique un certificat auto-signé ou auto-émis. Elle ne génère pas de demande, mais plutôt un nouveau certificat, puis installe le certificat. La valeur par défaut est auto-signé. Spécifiez un certificat de signature à l’aide de l’option – CERT pour créer un certificat auto-émis qui n’est pas auto-signé. | `RequestType = CMC` |
| SecurityDescriptor | Contient les informations de sécurité associées aux objets sécurisables. Pour la plupart des objets sécurisables, vous pouvez spécifier le descripteur de sécurité d’un objet dans l’appel de fonction qui crée l’objet. Chaînes basées sur le [langage de définition du descripteur de sécurité](https://msdn.microsoft.com/library/aa379567(v=vs.85).aspx).<p>**Conseil :** Cela s’applique uniquement aux clés de carte non intelligente de contexte machine. | `SecurityDescriptor = D:P(A;;GA;;;SY)(A;;GA;;;BA)` |
| AlternateSignatureAlgorithm | Spécifie et récupère une valeur booléenne qui indique si l’identificateur d’objet (OID) de l’algorithme de signature pour une demande ou une signature de certificat PKCS # 10 est discret ou combiné. | `true | false` | `AlternateSignatureAlgorithm = false`<p>Pour une signature RSA, `false` indique un `Pkcs1 v1.5`, tandis `true` que `v2.1` indique une signature. |
| Silencieux | Par défaut, cette option permet au CSP d’accéder au bureau interactif de l’utilisateur et de demander des informations, telles qu’un code confidentiel de carte à puce, à l’utilisateur. Si cette clé est définie sur TRUE, le fournisseur de services de chiffrement ne doit pas interagir avec le bureau et ne pourra pas afficher l’interface utilisateur de l’utilisateur. | `true | false` | `Silent = true` |
| SMIME | Si ce paramètre est défini sur TRUE, une extension avec la valeur d’identificateur d’objet 1.2.840.113549.1.9.15 est ajoutée à la demande. Le nombre d’identificateurs d’objet dépend du sur la version du système d’exploitation installée et de la fonctionnalité CSP, qui font référence aux algorithmes de chiffrement symétrique qui peuvent être utilisés par les applications S/MIME (Secure Multipurpose Internet Mail Extensions), telles qu’Outlook. | `true | false` | `SMIME = true` |
| UseExistingKeySet | Ce paramètre est utilisé pour spécifier qu’une paire de clés existante doit être utilisée lors de la création d’une demande de certificat. Si cette clé est définie sur TRUE, vous devez également spécifier une valeur pour la clé RenewalCert ou le nom du keycontainer. Vous ne devez pas définir la clé exportable, car vous ne pouvez pas modifier les propriétés d’une clé existante. Dans ce cas, aucun matériel de clé n’est généré lors de la génération de la demande de certificat. | `true | false` | `UseExistingKeySet = true` |
| Keyprotection | Spécifie une valeur qui indique comment une clé privée est protégée avant son utilisation. | <ul><li>`XCN_NCRYPT_UI_NO_PROTCTION_FLAG -- 0`</li><li>`XCN_NCRYPT_UI_PROTECT_KEY_FLAG -- 1`</li><li>`XCN_NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG -- 2`</li></ul> | `KeyProtection = NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG` |
| SuppressDefaults | Spécifie une valeur booléenne qui indique si les extensions et attributs par défaut sont inclus dans la demande. Les valeurs par défaut sont représentées par leurs identificateurs d’objet (OID). | `true | false` | `SuppressDefaults = true` |
| FriendlyName | Nom convivial pour le nouveau certificat. | Texte | `FriendlyName = Server1` |
| ValidityPeriodUnits | Spécifie un nombre d’unités à utiliser avec ValidityPeriod. Remarque : cela est utilisé uniquement lorsque `request type=cert`. | Numérique | `ValidityPeriodUnits = 3` |
| ValidityPeriod | ValidityPeriod doit être une période de temps en anglais américain. Remarque : Ceci est utilisé uniquement lorsque le type de demande est CERT. | `Years |  Months | Weeks | Days | Hours | Minutes | Seconds` | `ValidityPeriod = Years` |

<sup>1</sup> Paramètre à gauche du signe égal (=)

<sup>2</sup> Paramètre situé à droite du signe égal (=)

#### <a name="extensions"></a>extensions

Cette section est facultative.

| OID d’extension | Définition |  Exemple |
| ------------- | ---------- | ----- | ------- |
| 2.5.29.17 | | 2.5.29.17 = {Text} |
| *pouvoir* | | `continue = UPN=User@Domain.com&` |
| *pouvoir* | | `continue = EMail=User@Domain.com&` |
| *pouvoir* | | `continue = DNS=host.domain.com&` |
| *pouvoir* | | `continue = DirectoryName=CN=Name,DC=Domain,DC=com&` |
| *pouvoir* | | `continue = URL=<http://host.domain.com/default.html&>` |
| *pouvoir* | | `continue = IPAddress=10.0.0.1&` |
| *pouvoir* | | `continue = RegisteredId=1.2.3.4.5&` |
| *pouvoir* | | `continue = 1.2.3.4.6.1={utf8}String&` |
| *pouvoir* | | `continue = 1.2.3.4.6.2={octet}AAECAwQFBgc=&` |
| *pouvoir* | | `continue = 1.2.3.4.6.2={octet}{hex}00 01 02 03 04 05 06 07&` |
| *pouvoir* | | `continue = 1.2.3.4.6.3={asn}BAgAAQIDBAUGBw==&` |
| *pouvoir* | | `continue = 1.2.3.4.6.3={hex}04 08 00 01 02 03 04 05 06 07` |
| 2.5.29.37 | | `2.5.29.37={text}` |
| *pouvoir* | | `continue = 1.3.6.1.5.5.7` |
| *pouvoir* | | `continue = 1.3.6.1.5.5.7.3.1` |
| 2.5.29.19 | | `{text}ca=0pathlength=3` |
| Critique | | `Critical=2.5.29.19` |
| KeySpec | | <ul><li>`AT_NONE -- 0`</li><li>`AT_SIGNATURE -- 2`</li><li>`AT_KEYEXCHANGE -- 1`</ul></li> |
| RequestType | | <ul><li>`PKCS10 -- 1`</li><li>`PKCS7 -- 2`</li><li>`CMC -- 3`</li><li>`Cert -- 4`</li><li>`SCEP -- fd00 (64768)`</li></ul> |
| Utilisation | | <ul><li>`CERT_DIGITAL_SIGNATURE_KEY_USAGE -- 80 (128)`</li><li>`CERT_NON_REPUDIATION_KEY_USAGE -- 40 (64)`</li><li>`CERT_KEY_ENCIPHERMENT_KEY_USAGE -- 20 (32)`</li><li>`CERT_DATA_ENCIPHERMENT_KEY_USAGE -- 10 (16)`</li><li>`CERT_KEY_AGREEMENT_KEY_USAGE -- 8`</li><li>`CERT_KEY_CERT_SIGN_KEY_USAGE -- 4`</li><li>`CERT_OFFLINE_CRL_SIGN_KEY_USAGE -- 2`</li><li>`CERT_CRL_SIGN_KEY_USAGE -- 2`</li><li>`CERT_ENCIPHER_ONLY_KEY_USAGE -- 1`</li><li>`CERT_DECIPHER_ONLY_KEY_USAGE -- 8000 (32768)`</li></ul> |
| KeyUsageProperty | | <ul><li>`NCRYPT_ALLOW_DECRYPT_FLAG -- 1`</li><li>`NCRYPT_ALLOW_SIGNING_FLAG -- 2`</li><li>`NCRYPT_ALLOW_KEY_AGREEMENT_FLAG -- 4`</li><li>`NCRYPT_ALLOW_ALL_USAGES -- ffffff (16777215)`</li></ul> |
| Keyprotection | | <ul><li>`NCRYPT_UI_NO_PROTECTION_FLAG -- 0`</li><li>`NCRYPT_UI_PROTECT_KEY_FLAG -- 1`</li><li>`NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG -- 2`</li></ul> |
| SubjectNameFlags | template | <ul><li>`CT_FLAG_SUBJECT_REQUIRE_COMMON_NAME -- 40000000 (1073741824)`</li><li>`CT_FLAG_SUBJECT_REQUIRE_DIRECTORY_PATH -- 80000000 (2147483648)`</li><li>`CT_FLAG_SUBJECT_REQUIRE_DNS_AS_CN -- 10000000 (268435456)`</li><li>`CT_FLAG_SUBJECT_REQUIRE_EMAIL -- 20000000 (536870912)`</li><li>`CT_FLAG_OLD_CERT_SUPPLIES_SUBJECT_AND_ALT_NAME -- 8`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_DIRECTORY_GUID -- 1000000 (16777216)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_DNS -- 8000000 (134217728)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_DOMAIN_DNS -- 400000 (4194304)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_EMAIL -- 4000000 (67108864)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_SPN -- 800000 (8388608)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_UPN -- 2000000 (33554432)`</li></ul> |
| L x500nameflags | | <ul><li>`CERT_NAME_STR_NONE -- 0`</li><li>`CERT_OID_NAME_STR -- 2`</li><li>`CERT_X500_NAME_STR -- 3`</li><li>`CERT_NAME_STR_SEMICOLON_FLAG -- 40000000 (1073741824)`</li><li>`CERT_NAME_STR_NO_PLUS_FLAG -- 20000000 (536870912)`</li><li>`CERT_NAME_STR_NO_QUOTING_FLAG -- 10000000 (268435456)`</li><li>`CERT_NAME_STR_CRLF_FLAG -- 8000000 (134217728)`</li><li>`CERT_NAME_STR_COMMA_FLAG -- 4000000 (67108864)`</li><li>`CERT_NAME_STR_REVERSE_FLAG -- 2000000 (33554432)`</li><li>`CERT_NAME_STR_FORWARD_FLAG -- 1000000 (16777216)`</li><li>`CERT_NAME_STR_DISABLE_IE4_UTF8_FLAG -- 10000 (65536)`</li><li>`CERT_NAME_STR_ENABLE_T61_UNICODE_FLAG -- 20000 (131072)`</li><li>`CERT_NAME_STR_ENABLE_UTF8_UNICODE_FLAG -- 40000 (262144)`</li><li>`CERT_NAME_STR_FORCE_UTF8_DIR_STR_FLAG -- 80000 (524288)`</li><li>`CERT_NAME_STR_DISABLE_UTF8_DIR_STR_FLAG -- 100000 (1048576)`</li><li>`CERT_NAME_STR_ENABLE_PUNYCODE_FLAG -- 200000 (2097152)`</li></ul> |

> [!NOTE]
> `SubjectNameFlags`permet au fichier INF de spécifier les champs d’extension **objet** et **SubjectAltName** qui doivent être remplis automatiquement par Certreq en fonction des propriétés de l’utilisateur actuel ou de l’ordinateur actuel : nom DNS, UPN, etc. L’utilisation du modèle de littéral signifie que les indicateurs de nom de modèle sont utilisés à la place. Cela permet à un seul fichier INF d’être utilisé dans plusieurs contextes pour générer des demandes avec des informations de sujet spécifiques au contexte.
>
> `X500NameFlags`spécifie les indicateurs à transmettre directement à `CertStrToName` l’API lorsque `Subject INF keys` la valeur est convertie en un **nom**unique encodé ASN. 1.

#### <a name="example"></a> Exemple

Pour créer un fichier de stratégie (. inf) dans le bloc-notes et l’enregistrer en tant que *requestconfig. inf*:

```
[NewRequest]
Subject = CN=<FQDN of computer you are creating the certificate>
Exportable = TRUE
KeyLength = 2048
KeySpec = 1
KeyUsage = 0xf0
MachineKeySet = TRUE
[RequestAttributes]
CertificateTemplate=WebServer
[Extensions]
OID = 1.3.6.1.5.5.7.3.1
OID = 1.3.6.1.5.5.7.3.2
```

Sur l’ordinateur pour lequel vous demandez un certificat :

```
certreq –new requestconfig.inf certrequest.req
```

Pour utiliser la syntaxe de section [strings] pour les OID et d’autres difficultés pour interpréter les données. Nouvel exemple de syntaxe {Text} pour l’extension EKU, qui utilise une liste séparée par des virgules d’OID :

```
[Version]
Signature=$Windows NT$

[Strings]
szOID_ENHANCED_KEY_USAGE = 2.5.29.37
szOID_PKIX_KP_SERVER_AUTH = 1.3.6.1.5.5.7.3.1
szOID_PKIX_KP_CLIENT_AUTH = 1.3.6.1.5.5.7.3.2

[NewRequest]
Subject = CN=TestSelfSignedCert
Requesttype = Cert

[Extensions]
%szOID_ENHANCED_KEY_USAGE%={text}%szOID_PKIX_KP_SERVER_AUTH%,
_continue_ = %szOID_PKIX_KP_CLIENT_AUTH%
```

### <a name="certreq--accept"></a>Certreq-accepter

Le `–accept` paramètre lie la clé privée précédemment générée au certificat émis et supprime la demande de certificat en attente du système où le certificat est demandé (s’il existe une demande correspondante).

Pour accepter manuellement un certificat :

```
certreq -accept certnew.cer
```

> [!WARNING]
> L’utilisation `-accept` du paramètre avec `-user` les `–machine` options et indique si le certificat d’installation doit être installé dans le contexte de l' **utilisateur** ou de l' **ordinateur** . S’il existe une demande en attente dans l’un des deux contextes qui correspond à la clé publique en cours d’installation, ces options ne sont pas nécessaires. S’il n’y a aucune demande en suspens, l’une d’entre elles doit être spécifiée.

### <a name="certreq--policy"></a>Certreq-stratégie

Le fichier Policy. inf est un fichier de configuration qui définit les contraintes appliquées à une certification d’autorité de certification lorsqu’une subordination qualifiée est définie.

Pour générer une demande de certificat croisé :

```
certreq -policy certsrv.req policy.inf newcertsrv.req
```

Si `certreq -policy` vous utilisez sans paramètre supplémentaire, une fenêtre de dialogue s’ouvre, ce qui vous permet de sélectionner le fichier demandé (. req,. CMC,. txt,. der,. cer ou. CRT). Après avoir sélectionné le fichier demandé et cliqué sur **ouvrir**, une autre boîte de dialogue s’ouvre pour vous permettre de sélectionner le fichier. inf de la stratégie.

#### <a name="examples"></a>Exemples

Recherchez un exemple du fichier Policy. inf dans la [syntaxe CAPolicy. inf](https://docs.microsoft.com/windows-server/networking/core-network-guide/cncg/server-certs/prepare-the-capolicy-inf-file).

### <a name="certreq--sign"></a>Certreq-Sign

Pour créer une demande de certificat, signez-la et envoyez-la :

```
certreq -new policyfile.inf myrequest.req
certreq -sign myrequest.req myrequest.req
certreq -submit myrequest_sign.req myrequest_cert.cer
```

#### <a name="remarks"></a>Notes 

- Si `certreq -sign` vous utilisez sans paramètre supplémentaire, une fenêtre de dialogue s’ouvre pour vous permettre de sélectionner le fichier demandé (Req, CMC, txt, der, CER ou CRT).

- La signature de la demande de subordination qualifiée peut nécessiter des informations d’identification d' **administrateur d’entreprise** . Il s’agit d’une meilleure pratique pour émettre des certificats de signature pour la subordination qualifiée.

- Le certificat utilisé pour signer la demande de subordination qualifiée utilise le modèle de subordination qualifiée. Les administrateurs d’entreprise doivent signer la demande ou accorder des autorisations d’utilisateur aux personnes qui signent le certificat.

- Vous devrez peut-être demander au personnel supplémentaire de signer la demande CMC après vous. Cela dépend du niveau d’assurance associé à la subordination qualifiée.

- Si l’autorité de certification parente de l’autorité de certification subordonnée qualifiée que vous installez est hors connexion, vous devez obtenir le certificat d’autorité de certification pour l’autorité de certification secondaire qualifiée à partir du parent hors connexion. Si l’autorité de certification parente est en ligne, spécifiez le certificat d’autorité de certification pour l’autorité de certification subordonnée qualifiée pendant l’Assistant **installation des services de certificats** .

### <a name="certreq--enroll"></a>Certreq-inscrire

Vous pouvez utiliser ce commentaire pour inscrire ou renouveler vos certificats.

#### <a name="examples"></a>Exemples

Pour inscrire un certificat à l’aide du modèle *WebServer* et en sélectionnant le serveur de stratégie à l’aide de U/I :

```
certreq -enroll –machine –policyserver * WebServer
```

Pour renouveler un certificat à l’aide d’un numéro de série :

```
certreq –enroll -machine –cert 61 2d 3c fe 00 00 00 00 00 05 renew
```

Vous ne pouvez renouveler que des certificats valides. Les certificats arrivés à expiration ne peuvent pas être renouvelés et doivent être remplacés par un nouveau certificat.

## <a name="options"></a>Options

| Options | Description |
| ------- | ----------- |
| -any | `Force ICertRequest::Submit`pour déterminer le type d’encodage.|
| -attrib`<attributestring>` | Spécifie les paires de chaînes de **nom** et de **valeur** , séparées par un signe deux-points.<p>Séparez les **Value** paires de chaînes de `\n` **noms** et de valeurs à l’aide de (par exemple, nom1 : value1\nName2 : value2). |
| -binaire | Met en forme les fichiers de sortie au format binaire au lieu de l’encodage Base64. |
| -policyserver`<policyserver>` | LDAP`<path>`<br>Insérez l’URI ou l’ID unique d’un ordinateur exécutant le service Web stratégie d’inscription de certificats.<p>Pour spécifier que vous souhaitez utiliser un fichier de requête en navigant, utilisez simplement un signe moins (-) pour `<policyserver>`. |
| -config`<ConfigString>` | Traite l’opération à l’aide de l’autorité de certification spécifiée dans la chaîne de configuration, qui est **CAHostName\CAName**. Pour une connexion HTTPS\\: \ connexion, spécifiez l’URI du serveur d’inscription. Pour l’autorité de certification du magasin de l’ordinateur local, utilisez un signe moins (-). |
| -Anonyme | Utilisez des informations d’identification anonymes pour les services Web inscription de certificats. |
| -Kerberos | Utilisez les informations d’identification Kerberos (domaine) pour les services Web inscription de certificats. |
| -ClientCertificate`<ClientCertId>` | Vous pouvez remplacer le `<ClientCertId>` par une empreinte numérique de certificat, CN, EKU, template, E-mail, UPN ou la `name=value` nouvelle syntaxe. |
| -username`<username>` | Utilisé avec les services Web d’inscription de certificats. Vous pouvez remplacer `<username>` par le nom Sam ou la valeur **domaine\utilisateur** . Cette option est destinée à être utilisée `-p` avec l’option. |
| -p`<password>` | Utilisé avec les services Web d’inscription de certificats. Remplacez `<password>` par le mot de passe de l’utilisateur réel. Cette option est destinée à être utilisée `-username` avec l’option. |
| -utilisateur | Configure le `-user` contexte d’une nouvelle demande de certificat ou spécifie le contexte d’acceptation d’un certificat. Il s’agit du contexte par défaut, si aucun n’est spécifié dans le fichier INF ou le modèle. |
| -machine | Configure une nouvelle demande de certificat ou spécifie le contexte d’une acceptation de certificat pour le contexte de l’ordinateur. Pour les nouvelles requêtes, il doit être cohérent avec la clé INF MachineKeyset et le contexte du modèle. Si cette option n’est pas spécifiée et que le modèle ne définit pas de contexte, la valeur par défaut est le contexte de l’utilisateur. |
| -CRL | Comprend les listes de révocation de certificats (CRL) dans la sortie vers le fichier #7 PKCS encodé en base64 spécifié `certchainfileout` par ou vers le fichier encodé en base64 spécifié par `requestfileout`. |
| -RPC | Indique à Active Directory Services de certificats (AD CS) d’utiliser une connexion de serveur d’appel de procédure distante (RPC) au lieu de COM distribué. |
| -adminforcemachine | Utilisez le service de clé ou l’emprunt d’identité pour envoyer la demande à partir du contexte du système local. Exige que l’utilisateur qui appelle cette option soit membre du groupe Administrateurs local. |
| -renewonbehalfof | Soumettez un renouvellement pour le compte du sujet identifié dans le certificat de signature. Cela définit CR_IN_ROBO lors de l’appel de la [méthode ICertRequest :: Submit](https://docs.microsoft.com/windows/win32/api/certcli/nf-certcli-icertrequest-submit) |
| -f | Forcer le remplacement des fichiers existants. Cela contourne également les modèles et la stratégie de mise en cache. |
| -q | Utiliser le mode silencieux ; supprimer toutes les invites interactives. |
| -Unicode | Écrit la sortie Unicode lorsque la sortie standard est redirigée ou redirigée vers une autre commande, ce qui vous aide quand elle est appelée à partir de scripts Windows PowerShell. |
| -unicodetext | Envoie une sortie Unicode lors de l’écriture d’objets BLOB de données encodées en texte Base64 dans des fichiers. |

## <a name="formats"></a>Formats

| Formats | Description |
| ------- | ----------- |
| requestfilein | Nom du fichier d’entrée binaire ou encodé en base64 : demande de certificat #10 PKCS, demande de certificat CMS, demande de renouvellement de certificat PKCS #7, certificat X. 509 à certification croisée ou demande de certificat au format de balise KeyGen. |
| requestfileout | Nom du fichier de sortie encodé en base64. |
| certfileout | Nom de fichier X-509 encodé en base64. |
| PKCS10fileout | À utiliser avec le `certreq -policy` paramètre uniquement. Nom du fichier de sortie PKCS10 encodé en base64. |
| certchainfileout | Nom du fichier de #7 PKCS encodé en base64. |
| fullresponsefileout | Nom du fichier réponse complet encodé en base64. |
| policyfilein | À utiliser avec le `certreq -policy` paramètre uniquement. Fichier INF contenant une représentation textuelle des extensions utilisées pour qualifier une demande. |

## <a name="additional-resources"></a>Ressources supplémentaires

Les articles suivants contiennent des exemples d’utilisation de Certreq :

- [Comment ajouter un autre nom d’objet à un certificat LDAP sécurisé](https://support.microsoft.com/help/931351/how-to-add-a-subject-alternative-name-to-a-secure-ldap-certificate)

- [Test Lab Guide: Deploying an AD CS Two-Tier PKI Hierarchy](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831348(v=ws.11))

- [Annexe 3 : syntaxe Certreq. exe](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc736326(v=ws.10))

- [Comment créer un certificat SSL de serveur Web manuellement](https://techcommunity.microsoft.com/t5/core-infrastructure-and-security/how-to-create-a-web-server-ssl-certificate-manually/ba-p/1128529)

- [Demander un certificat de configuration AMT à l’aide d’une autorité de certification Windows Server 2008](https://social.technet.microsoft.com/wiki/contents/articles/548.request-an-amt-provisioning-certificate-using-a-windows-server-2008-ca.aspx)

- [Inscription de certificats pour System Center Operations Manager agent](https://social.technet.microsoft.com/wiki/contents/articles/2017.certificate-enrollment-for-system-center-operations-manager-agent.aspx)

- [Guide pas à pas des services AD CS : déploiement de la hiérarchie PKI à deux niveaux](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)

- [Comment activer LDAP sur SSL avec une autorité de certification tierce](https://support.microsoft.com/help/321051/how-to-enable-ldap-over-ssl-with-a-third-party-certification-authority)
