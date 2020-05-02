---
title: certutil
description: Rubrique de référence pour la commande certutil, qui est un programme de ligne de commande qui vide et affiche les informations de configuration de l’autorité de certification, configure les services de certificats, sauvegarde et restaure les composants de l’autorité de certification et vérifie les certificats, les paires de clés et les chaînes de certificats.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c264ccf0-ba1e-412b-9dd3-d77dd9345ad9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3848c5493247e7e2d5e5b57be6d5d6e4015708b4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716242"
---
# <a name="certutil"></a>certutil

Certutil. exe est un programme de ligne de commande installé dans le cadre des services de certificats. Vous pouvez utiliser certutil. exe pour vider et afficher les informations de configuration de l’autorité de certification, configurer les services de certificats, sauvegarder et restaurer les composants de l’autorité de certification et vérifier les certificats, les paires de clés et les chaînes de certificats.

Si la commande certutil est exécutée sur une autorité de certification sans paramètres supplémentaires, elle affiche la configuration de l’autorité de certification actuelle. Si certutil est exécuté sur une autorité de certification qui n’est pas une autorité de certification, `certutil [-dump]` la commande utilise par défaut la commande.

> [!IMPORTANT]
> Les versions antérieures de Certutil peuvent ne pas fournir toutes les options décrites dans ce document. Vous pouvez voir toutes les options fournies par une version spécifique de Certutil en exécutant `certutil -?` ou `certutil <parameter> -?`.

## <a name="parameters"></a>Paramètres

### <a name="-dump"></a>-dump

Videz des fichiers ou des informations de configuration.

```
certutil [options] [-dump]
certutil [options] [-dump] file
```

```
[-f] [-silent] [-split] [-p password] [-t timeout]
```

### <a name="-asn"></a>-ASN

Analysez le fichier ASN. 1.

```
certutil [options] -asn file [type]
```

`[type]`: Numeric CRYPT_STRING_ * type de décodage

### <a name="-decodehex"></a>-decodehex

Décodez un fichier encodé en hexadécimal.

```
certutil [options] -decodehex infile outfile [type]
```

`[type]`: Numeric CRYPT_STRING_ * type d’encodage

```
[-f]
```

### <a name="-decode"></a>-Decode

Décodez un fichier encodé en base64.

```
certutil [options] -decode infile outfile
```

```
[-f]
```

### <a name="-encode"></a>-encode

Encoder un fichier en base64.

```
certutil [options] -encode infile outfile
```

```
[-f] [-unicodetext]
```

### <a name="-deny"></a>-refuser

Refuser une demande en attente.

```
certutil [options] -deny requestID
```

```
[-config Machine\CAName]
```

### <a name="-resubmit"></a>-renvoyer

Soumettez à nouveau une demande en attente.

```
certutil [options] -resubmit requestId
```

```
[-config Machine\CAName]
```

### <a name="-setattributes"></a>-SetAttributes

Définir les attributs d’une demande de certificat en attente.

```
certutil [options] -setattributes RequestID attributestring
```

Où :

- **RequestId** est l’ID de demande numérique pour la demande en attente.

- **AttributeString** est le nom de l’attribut de requête et les paires de valeurs.

```
[-config Machine\CAName]
```

#### <a name="remarks"></a>Notes 

- Les noms et les valeurs doivent être séparés par des points-virgules, tandis que les paires nom-valeur doivent être séparées par des sauts de ligne. Par exemple : `CertificateTemplate:User\nEMail:User@Domain.com` où la `\n` séquence est convertie en séparateur de saut de ligne.

### <a name="-setextension"></a>-setextension

Définissez une extension pour une demande de certificat en attente.

```
certutil [options] -setextension requestID extensionname flags {long | date | string | \@infile}
```

Où :

- **RequestId** est l’ID de demande numérique pour la demande en attente.

- **ExtensionName** est la chaîne ObjectID pour l’extension.

- **Flags** définit la priorité de l’extension. `0`est recommandé, tandis que `1` définit l’extension sur `2` critique, désactive l’extension et `3` les deux.

```
[-config Machine\CAName]
```

#### <a name="remarks"></a>Notes 

- Si le dernier paramètre est numérique, il est pris comme **long**.

- Si le dernier paramètre peut être analysé en tant que date, il est pris comme **Date**.

- Si le dernier paramètre commence par `\@`, le reste du jeton est pris comme nom de fichier avec des données binaires ou un vidage hexadécimal de texte ASCII.

- Si le dernier paramètre est autre chose, il est pris comme une chaîne.

### <a name="-revoke"></a>-Revoke

Révoquer un certificat.

```
certutil [options] -revoke serialnumber [reason]
```

Où :

- **SerialNumber** est une liste séparée par des virgules de numéros de série de certificats à révoquer.

- **reason** est la représentation numérique ou symbolique du motif de révocation, y compris :

  - **0. CRL_REASON_UNSPECIFIED** -non spécifié (par défaut)

  - **1. CRL_REASON_KEY_COMPROMISE** -clé compromise

  - **2. CRL_REASON_CA_COMPROMISE** -la compromission de l’autorité de certification

  - **3. CRL_REASON_AFFILIATION_CHANGED** -affiliation modifiée

  - **4. CRL_REASON_SUPERSEDED** -remplacé

  - **5.** abandon de l’opération CRL_REASON_CESSATION_OF_OPERATION

  - **6. CRL_REASON_CERTIFICATE_HOLD** certificat de conservation

  - **8. CRL_REASON_REMOVE_FROM_CRL** -supprimer de la liste de révocation de certificats

  - **1. annulez la révocation** -annuler la révocation

```
[-config Machine\CAName]
```

### <a name="-isvalid"></a>-IsValid

Affiche la disposition du certificat actuel.

```
certutil [options] -isvalid serialnumber | certhash
```

```
[-config Machine\CAName]
```

### <a name="-getconfig"></a>-GetConfig

Obtient la chaîne de configuration par défaut.

```
certutil [options] -getconfig
```

```
[-config Machine\CAName]
```

### <a name="-ping"></a>-ping

Essayez de contacter l’interface de demande Active Directory Certificate Services.

```
certutil [options] -ping [maxsecondstowait | camachinelist]
```

Où :

- **camachinelist** est une liste séparée par des virgules des noms d’ordinateur de l’autorité de certification. Pour un seul ordinateur, utilisez une virgule de fin. Cette option affiche également le coût du site pour chaque ordinateur de l’autorité de certification.

```
[-config Machine\CAName]
```

### <a name="-cainfo"></a>-infos

Affichez des informations sur l’autorité de certification.

```
certutil [options] -cainfo [infoname [index | errorcode]]
```

Où :

- **InfoName** indique la propriété de l’autorité de certification à afficher, en fonction de la syntaxe d’argument InfoName suivante :

  - version du **fichier de fichier**
  
  - **Product** -version du produit
  
  - **exitcount** -quitter le nombre de modules
  
  - **sortie `[index]` ** : description du module de sortie
  
  - Description du **module de stratégie de stratégie**
  
  - **nom** -nom de l’autorité de certification
  
  - **sanitizedname** -nom de l’autorité de certification nettoyée
  
  - **dsname** -nom abrégé de l’autorité de certification nettoyé (nom du service de domaine)
  
  - **SharedFolder** -dossier partagé
  
  - **Error1 ErrorCode** -texte du message d’erreur
  
  - **Error2 ErrorCode** -texte du message d’erreur et code d’erreur
  
  - **type-ca** type
  
  - **info-informations** sur l’autorité de certification
  
  - autorité de certification **parent** -parent
  
  - nombre de certificats **certcount** -ca
  
  - nombre de certificats **xchgcount** -ca Exchange
  
  - nombre de certificats **kracount** -kra
  
  - nombre d’utilisations du certificat **kraused** -kra
  
  - **propidmax** -ID d’autorité de certification maximal
  
  - **certificat `[index]` certstate** -ca
  
  - version du certificat **certversion `[index]` ** -ca
  
  - État de vérification du certificat **certstatuscode `[index]` ** -ca
  
  - **crlstate `[index]` ** -CRL
  
  - **krastate `[index]` ** -certificat KRA
  
  - **crossstate + `[index]` ** -transférer le certificat croisé
  
  - **crossstate- `[index]` ** -Cross Cross CERT
  
  - CERT-certificat d’autorité de certification ** `[index]` **
  
  - chaîne de certificats **certchain `[index]` ** -ca
  
  - **certcrlchain `[index]` ** -chaîne de certificats ca avec listes de révocation de certificats
  
  - certificat d’échange **Xchg `[index]` ** -ca
  
  - chaîne de certificats Exchange **xchgchain `[index]` ** -ca

  - **xchgcrlchain `[index]` ** -ca-chaîne de certificats Exchange avec listes de révocation de certificats
  
  - **certificat `[index]` kra** -kra
  
  - **Cross + `[index]` ** -Forward Cross CERT
  
  - Cross **- `[index]` ** -Backward Cross-CERT
  
  - **Liste `[index]` ** de révocation de certificats de base
  
  - **DeltaCRL `[index]` ** -liste de révocation de certificats delta
  
  - État de publication de la liste de révocation de certificats **crlstatus `[index]` **
  
  - **deltacrlstatus `[index]` ** -état de publication de la liste de révocation de certificats delta
  
  - Nom **DNS** -DNS
  
  - Séparation **des rôles**
  
  - **ADS** -serveur avancé
  
  - **modèles** -modèles
  
  - **CSP `[index]` ** -URL OCSP
  
  - **URL `[index]` AIA** -AIA
  
  - **CDP `[index]` ** -URL CDP
  
  - **localename** -nom des paramètres régionaux de l’autorité de certification
  
  - **subjecttemplateoids** -OID de modèle subject
  
  - **&#42;** : affiche toutes les propriétés

- **index** est l’index de propriété de base zéro facultatif.

- **ErrorCode** est le code d’erreur numérique.

```
[-f] [-split] [-config Machine\CAName]
```

### <a name="-cacert"></a>-ca. cert

Récupérez le certificat de l’autorité de certification.

```
certutil [options] -ca.cert outcacertfile [index]
```

Où :

- **outcacertfile** est le fichier de sortie.

- **index** est l’index de renouvellement du certificat d’autorité de certification (par défaut le plus récent).

```
[-f] [-split] [-config Machine\CAName]
```

### <a name="-cachain"></a>-ca. Chain

Récupérez la chaîne de certificats pour l’autorité de certification.

```
certutil [options] -ca.chain outcacertchainfile [index]
```

Où :

- **OutCACertChainFile** est le fichier de sortie.

- **index** est l’index de renouvellement du certificat d’autorité de certification (par défaut le plus récent).

```
[-f] [-split] [-config Machine\CAName]
```

### <a name="-getcrl"></a>-getcrl

Obtient une liste de révocation de certificats (CRL).

certutil [options]-getcrl out [index] [Delta]

Où :

- **index est l'** index de la liste de révocation de certificats ou de la clé (la liste de révocation par défaut de la clé la plus récente).

- **Delta** est la liste de révocation de certificats delta (la valeur par défaut est la liste CRL de base).

```
[-f] [-split] [-config Machine\CAName]
```

### <a name="-crl"></a>-CRL

Publier de nouvelles listes de révocation de certificats (CRL) ou des listes de révocation de certificats delta.

```
certutil [options] -crl [dd:hh | republish] [delta]
```

Où :

- **JJ : HH** est la nouvelle période de validité de la liste de révocation de certificats en jours et heures.

- **republier** republie les listes de révocation de certificats les plus récentes.

- **Delta** publie uniquement les listes de révocation de certificats delta (par défaut, les listes de révocation de certificats de base et Delta).

```
[-split] [-config Machine\CAName]
```

### <a name="-shutdown"></a>-Shutdown

Arrête les services de certificats Active Directory.

```
certutil [options] -shutdown
```

```
[-config Machine\CAName]
```

### <a name="-installcert"></a>-installcert

Installe un certificat d’autorité de certification.

```
certutil [options] -installcert [cacertfile]
```

```
[-f] [-silent] [-config Machine\CAName]
```

### <a name="-renewcert"></a>-renewcert

Renouvelle un certificat d’autorité de certification.

```
certutil [options] -renewcert [reusekeys] [Machine\ParentCAName]
```

- Utilisez `-f` pour ignorer une demande de renouvellement en suspens et pour générer une nouvelle requête.

```
[-f] [-silent] [-config Machine\CAName]
```

### <a name="-schema"></a>-schéma

Vide le schéma pour le certificat.

```
certutil [options] -schema [ext | attrib | cRL]
```
Où :

- La commande est définie par défaut sur la table de demande et de certificat.

- **ext** est la table d’extension.
  
- l' **attribut** est la table d’attributs.

- la **liste de révocation de certificats** est la table CRL.

```
[-split] [-config Machine\CAName]
```

### <a name="-view"></a>-afficher

Vide la vue du certificat.

```
certutil [options] -view [queue | log | logfail | revoked | ext | attrib | crl] [csv]
```

Où :

- la **file d’attente** fait un dump d’une file d’attente de demandes spécifique.

- le **Journal** vide les certificats émis ou révoqués, ainsi que toutes les demandes ayant échoué.

- **logfail** fait un dump des demandes ayant échoué.

- **révoqué** fait un dump des certificats révoqués.

- **ext** vide la table d’extension.
  
- l' **attribut** fait un dump de la table d’attributs.

- la **liste de révocation de certificats** fait un dump de la table CRL.

- **CSV** fournit la sortie à l’aide de valeurs séparées par des virgules.

```
[-silent] [-split] [-config Machine\CAName] [-restrict RestrictionList] [-out ColumnList]
```

#### <a name="remarks"></a>Notes 

- Pour afficher la colonne **statusCode** pour toutes les entrées, tapez`-out StatusCode`

- Pour afficher toutes les colonnes de la dernière entrée, tapez :`-restrict RequestId==$`

- Pour afficher le **RequestId** et la **disposition** de trois requêtes, tapez :`-restrict requestID>37,requestID<40 -out requestID,disposition`

- Pour afficher**les ID de ligne et les** **numéros de liste de révocation de certificats** pour toutes les listes CRL de base, tapez :`-restrict crlminbase=0 -out crlrowID,crlnumber crl`

- Pour afficher, tapez :`-v -restrict crlminbase=0,crlnumber=3 -out crlrawcrl crl`

- Pour afficher l’intégralité de la table de liste de révocation de certificats, tapez :`CRL`

- Utilisez `Date[+|-dd:hh]` pour les restrictions de date.

- Utilisez `now+dd:hh` pour une date relative à l’heure actuelle.

### <a name="-db"></a>-db

Vide la base de données brute.

```
certutil [options] -db
```

```
[-config Machine\CAName] [-restrict RestrictionList] [-out ColumnList]
```

### <a name="-deleterow"></a>-deleteRow

Supprime une ligne de la base de données du serveur.

```
certutil [options] -deleterow rowID | date [request | cert | ext | attrib | crl]
```

Où :

- la **demande** supprime les demandes en échec et en attente, en fonction de la date d’envoi.

- le **certificat** supprime les certificats expirés et révoqués, en fonction de la date d’expiration.

- **ext** supprime la table d’extension.
  
- l' **attribut** supprime la table d’attributs.

- la **liste de révocation de certificats** supprime la table CRL.

```
[-f] [-config Machine\CAName]
```

#### <a name="examples"></a>Exemples

- Pour supprimer les demandes en échec et en attente soumises par le 22 janvier 2001, tapez :`1/22/2001 request`

- Pour supprimer tous les certificats expirés avant le 22 janvier 2001, tapez :`1/22/2001 cert`

- Pour supprimer la ligne, les attributs et les extensions du certificat pour RequestID 37, tapez :`37`

- Pour supprimer les listes de révocation de certificats expirées avant le 22 janvier 2001, tapez :`1/22/2001 crl`

### <a name="-backup"></a>-backup

Sauvegarde les services de certificats Active Directory.

```
certutil [options] -backup backupdirectory [incremental] [keeplog]
```

Où :

- **BackupDirectory** est le répertoire dans lequel stocker les données sauvegardées.

- **incrémentielle** effectue une sauvegarde incrémentielle uniquement (sauvegarde complète par défaut).

- **keeplog** conserve les fichiers journaux de la base de données (par défaut, tronque les fichiers journaux).

```
[-f] [-config Machine\CAName] [-p Password]
```

### <a name="-backupdb"></a>-backupdb

Sauvegarde la base de données Active Directory Certificate Services.

```
certutil [options] -backupdb backupdirectory [incremental] [keeplog]
```

Où :

- **BackupDirectory** est le répertoire dans lequel stocker les fichiers de la base de données sauvegardée.

- **incrémentielle** effectue une sauvegarde incrémentielle uniquement (sauvegarde complète par défaut).

- **keeplog** conserve les fichiers journaux de la base de données (par défaut, tronque les fichiers journaux).

```
[-f] [-config Machine\CAName]
```

### <a name="-backupkey"></a>-backupkey

Sauvegarde le certificat et la clé privée Active Directory Certificate Services.

```
certutil [options] -backupkey backupdirectory
```

Où :

- **BackupDirectory** est le répertoire dans lequel stocker le fichier PFX sauvegardé.

```
[-f] [-config Machine\CAName] [-p password] [-t timeout]
```

### <a name="-restore"></a>-restore

Restaure les services de certificats Active Directory.

```
certutil [options] -restore backupdirectory
```

Où :

- **BackupDirectory** est le répertoire contenant les données à restaurer.

```
[-f] [-config Machine\CAName] [-p password]
```

### <a name="-restoredb"></a>-RestoreDB

Restaure la base de données des services de certificats Active Directory.

```
certutil [options] -restoredb backupdirectory
```

Où :

- **BackupDirectory** est le répertoire contenant les fichiers de base de données à restaurer.

```
[-f] [-config Machine\CAName]
```

### <a name="-restorekey"></a>-restorekey

Restaure la clé privée et le certificat des services de certificats Active Directory.

```
certutil [options] -restorekey backupdirectory | pfxfile
```

Où :

- **BackupDirectory** est le répertoire contenant le fichier PFX à restaurer.

```
[-f] [-config Machine\CAName] [-p password]
```

### <a name="-importpfx"></a>-importpfx

Importez le certificat et la clé privée. Pour plus d’informations, consultez `-store` le paramètre dans cet article.

```
certutil [options] -importpfx [certificatestorename] pfxfile [modifiers]
```

Où :

- **CertificateStoreName** est le nom du magasin de certificats.

- les **modificateurs** sont des listes séparées par des virgules, qui peuvent inclure un ou plusieurs des éléments suivants :

  1. **AT_SIGNATURE** -remplace le KeySpec par la signature
  
  2. **AT_KEYEXCHANGE** -remplace le KeySpec par l’échange de clés
  
  3. **Noexport** -rend la clé privée non exportable
  
  4. **NoCert** -n’importe pas le certificat
  
  5. **Nochain** -n’importe pas la chaîne de certificats
  
  6. **Noroot** -n’importe pas le certificat racine
  
  7. **Protéger** -protège les clés à l’aide d’un mot de passe
  
  8. **Noprotect** -ne protège pas les clés par mot de passe à l’aide d’un mot de passe

```
[-f] [-user] [-p password] [-csp provider]
```

#### <a name="remarks"></a>Notes 

- La valeur par défaut est Personal machine Store.

### <a name="-dynamicfilelist"></a>-dynamicfilelist

Affiche une liste de fichiers dynamiques.

```
certutil [options] -dynamicfilelist
```

```
[-config Machine\CAName]
```

### <a name="-databaselocations"></a>-databaselocations

Affiche les emplacements des bases de données.

```
certutil [options] -databaselocations
```

```
[-config Machine\CAName]
```

### <a name="-hashfile"></a>-hashfile

Génère et affiche un hachage de chiffrement sur un fichier.

```
certutil [options] -hashfile infile [hashalgorithm]
```

### <a name="-store"></a>-Store

Vide le magasin de certificats.

```
certutil [options] -store [certificatestorename [certID [outputfile]]]
```

Où :

- **CertificateStoreName** est le nom du magasin de certificats. Par exemple :

  - `My, CA (default), Root,`
  
  - `ldap:///CN=Certification Authorities,CN=Public Key Services,CN=Services,CN=Configuration,DC=cpandl,DC=com?cACertificate?one?objectClass=certificationAuthority (View Root Certificates)`
  
  - `ldap:///CN=CAName,CN=Certification Authorities,CN=Public Key Services,CN=Services,CN=Configuration,DC=cpandl,DC=com?cACertificate?base?objectClass=certificationAuthority (Modify Root Certificates)`
  
  - `ldap:///CN=CAName,CN=MachineName,CN=CDP,CN=Public Key Services,CN=Services,CN=Configuration,DC=cpandl,DC=com?certificateRevocationList?base?objectClass=cRLDistributionPoint (View CRLs)`
  
  - `ldap:///CN=NTAuthCertificates,CN=Public Key Services,CN=Services,CN=Configuration,DC=cpandl,DC=com?cACertificate?base?objectClass=certificationAuthority (Enterprise CA Certificates)`
  
  - `ldap: (AD computer object certificates)`
  
  - `-user ldap: (AD user object certificates)`

- **certID** est le jeton de correspondance du certificat ou de la liste de révocation de certificats. Il peut s’agir d’un numéro de série, d’un certificat SHA-1, d’une liste de révocation de certificats ou d’un hachage de clé publique, d’un index de certificat numérique (0, 1, etc.), d’un index de liste de révocation de certificats numérique (. 0, 1, etc.), d’un index CTL numérique (.. 0,.. 1, et ainsi de suite), une clé publique, une signature ou une extension ObjectId, un nom commun d’objet de certificat, une adresse de messagerie, un UPN ou un nom DNS, un nom de conteneur de clé ou un nom CSP, un nom de modèle ou un ID d’objet, un ID d’application améliorée ou des stratégies d’application ou un nom commun d’émetteur de liste La plupart d’entre eux peuvent entraîner des correspondances multiples.

- **FichierSortie** est le fichier utilisé pour enregistrer les certificats correspondants.

```
[-f] [-user] [-enterprise] [-service] [-grouppolicy] [-silent] [-split] [-dc DCName]
```

#### <a name="options"></a>Options

- L' `-user` option permet d’accéder à un magasin de l’utilisateur au lieu d’un magasin de l’ordinateur.

- L' `-enterprise` option permet d’accéder à un ordinateur de l’entreprise.

- L' `-service` option permet d’accéder à un magasin de services de l’ordinateur.

- L' `-grouppolicy` option accède au magasin de stratégies de groupe de l’ordinateur.

Par exemple :

- `-enterprise NTAuth`

- `-enterprise Root 37`

- `-user My 26e0aaaf000000000004`

- `CA .11`

### <a name="-addstore"></a>-AddStore

Ajoute un certificat au magasin. Pour plus d’informations, consultez `-store` le paramètre dans cet article.

```
certutil [options] -addstore certificatestorename infile
```

Où :

- **CertificateStoreName** est le nom du magasin de certificats.

- **infile** est le fichier de certificat ou de liste de révocation de certificats que vous souhaitez ajouter au magasin.

```
[-f] [-user] [-enterprise] [-grouppolicy] [-dc DCName]
```

### <a name="-delstore"></a>-delstore

Supprime un certificat du magasin. Pour plus d’informations, consultez `-store` le paramètre dans cet article.

```
certutil [options] -delstore certificatestorename certID
```

Où :

- **CertificateStoreName** est le nom du magasin de certificats.

- **certID** est le jeton de correspondance du certificat ou de la liste de révocation de certificats.

```
[-enterprise] [-user] [-grouppolicy] [-dc DCName]
```

### <a name="-verifystore"></a>-verifystore

Vérifie un certificat dans le magasin. Pour plus d’informations, consultez `-store` le paramètre dans cet article.

```
certutil [options] -verifystore certificatestorename [certID]
```

Où :

- **CertificateStoreName** est le nom du magasin de certificats.

- **certID** est le jeton de correspondance du certificat ou de la liste de révocation de certificats.

```
[-enterprise] [-user] [-grouppolicy] [-silent] [-split] [-dc DCName] [-t timeout]
```

### <a name="-repairstore"></a>-repairstore

Répare une association de clé ou met à jour des propriétés de certificat ou le descripteur de sécurité de clé. Pour plus d’informations, consultez `-store` le paramètre dans cet article.

```
certutil [options] -repairstore certificatestorename certIDlist [propertyinffile | SDDLsecuritydescriptor]
```

Où :

- **CertificateStoreName** est le nom du magasin de certificats.

- **certIDlist** est la liste séparée par des virgules des jetons de correspondance de certificat ou de liste de révocation de certificats. Pour plus d’informations, consultez `-store certID` la description dans cet article.

- **propertyinffile** est le fichier INF contenant les propriétés externes, notamment :

  ```
  [Properties]
      19 = Empty ; Add archived property, OR:
      19 =       ; Remove archived property

      11 = {text}Friendly Name ; Add friendly name property

      127 = {hex} ; Add custom hexadecimal property
          _continue_ = 00 01 02 03 04 05 06 07 08 09 0a 0b 0c 0d 0e 0f
          _continue_ = 10 11 12 13 14 15 16 17 18 19 1a 1b 1c 1d 1e 1f

      2 = {text} ; Add Key Provider Information property
        _continue_ = Container=Container Name&
        _continue_ = Provider=Microsoft Strong Cryptographic Provider&
        _continue_ = ProviderType=1&
        _continue_ = Flags=0&
        _continue_ = KeySpec=2

      9 = {text} ; Add Enhanced Key Usage property
        _continue_ = 1.3.6.1.5.5.7.3.2,
        _continue_ = 1.3.6.1.5.5.7.3.1,
  ```

```
[-f] [-enterprise] [-user] [-grouppolicy] [-silent] [-split] [-csp provider]
```

### <a name="-viewstore"></a>-viewstore

Vide le magasin de certificats. Pour plus d’informations, consultez `-store` le paramètre dans cet article.

```
certutil [options] -viewstore [certificatestorename [certID [outputfile]]]
```

Où :

- **CertificateStoreName** est le nom du magasin de certificats.

- **certID** est le jeton de correspondance du certificat ou de la liste de révocation de certificats.

- **FichierSortie** est le fichier utilisé pour enregistrer les certificats correspondants.

```
[-f] [-user] [-enterprise] [-service] [-grouppolicy] [-dc DCName]
```

#### <a name="options"></a>Options

- L' `-user` option permet d’accéder à un magasin de l’utilisateur au lieu d’un magasin de l’ordinateur.

- L' `-enterprise` option permet d’accéder à un ordinateur de l’entreprise.

- L' `-service` option permet d’accéder à un magasin de services de l’ordinateur.

- L' `-grouppolicy` option accède au magasin de stratégies de groupe de l’ordinateur.

Par exemple :

- `-enterprise NTAuth`

- `-enterprise Root 37`

- `-user My 26e0aaaf000000000004`

- `CA .11`

### <a name="-viewdelstore"></a>-viewdelstore

Supprime un certificat du magasin.

```
certutil [options] -viewdelstore [certificatestorename [certID [outputfile]]]
```

Où :

- **CertificateStoreName** est le nom du magasin de certificats.

- **certID** est le jeton de correspondance du certificat ou de la liste de révocation de certificats.

- **FichierSortie** est le fichier utilisé pour enregistrer les certificats correspondants.

```
[-f] [-user] [-enterprise] [-service] [-grouppolicy] [-dc DCName]
```

#### <a name="options"></a>Options

- L' `-user` option permet d’accéder à un magasin de l’utilisateur au lieu d’un magasin de l’ordinateur.

- L' `-enterprise` option permet d’accéder à un ordinateur de l’entreprise.

- L' `-service` option permet d’accéder à un magasin de services de l’ordinateur.

- L' `-grouppolicy` option accède au magasin de stratégies de groupe de l’ordinateur.

Par exemple :

- `-enterprise NTAuth`

- `-enterprise Root 37`

- `-user My 26e0aaaf000000000004`

- `CA .11`

### <a name="-dspublish"></a>-dspublish

Publie un certificat ou une liste de révocation de certificats (CRL) pour Active Directory.

```
certutil [options] -dspublish certfile [NTAuthCA | RootCA | SubCA | CrossCA | KRA | User | Machine]
```

```
certutil [options] -dspublish CRLfile [DSCDPContainer [DSCDPCN]]
```

Où :

- **CertFile** est le nom du fichier de certificat à publier.

- **NTAuthCA** publie le certificat dans le magasin DS Enterprise.

- **RootCA** publie le certificat dans le magasin racine de confiance DS.

- **SubCA** publie le certificat de l’autorité de certification sur l’objet de l’autorité de certification DS.

- **CrossCA** publie le certificat croisé vers l’objet de l’autorité de certification DS.

- L’agent de récupération de **clé publie le** certificat sur l’objet agent de récupération de clé DS.

- L' **utilisateur** publie le certificat sur l’objet DS de l’utilisateur.

- L' **ordinateur** publie le certificat sur l’objet de la machine DS.

- **FichierCRL** est le nom du fichier de liste de révocation de certificats à publier.

- **DSCDPContainer** est le nom commun du conteneur DS CDP, généralement le nom de l’ordinateur de l’autorité de certification.

- **DSCDPCN** est le nom commun de l’objet CDP DS, généralement basé sur le nom abrégé et l’index de clé de l’autorité de certification.

- Utilisez `-f` pour créer un nouvel objet de service d’annuaire.

```
[-f] [-user] [-dc DCName]
```

### <a name="-adtemplate"></a>-adtemplate

Affiche Active Directory modèles.

```
certutil [options] -adtemplate [template]
```

```
[-f] [-user] [-ut] [-mt] [-dc DCName]
```

### <a name="-template"></a>-modèle

Affiche les modèles de certificats.

```
certutil [options] -template [template]
```

```
[-f] [-user] [-silent] [-policyserver URLorID] [-anonymous] [-kerberos] [-clientcertificate clientcertID] [-username username] [-p password]
```

### <a name="-templatecas"></a>-templatecas

Affiche les autorités de certification (ca) d’un modèle de certificat.

```
certutil [options] -templatecas template
```

```
[-f] [-user] [-dc DCName]
```

### <a name="-catemplates"></a>-camodèles

Affiche les modèles de l’autorité de certification.

```
certutil [options] -catemplates [template]
```

```
[-f] [-user] [-ut] [-mt] [-config Machine\CAName] [-dc DCName]
```

### <a name="-setcasites"></a>-setcasites

Gère les noms de site, y compris la définition, la vérification et la suppression des noms de site d’autorité de certification

```
certutil [options] -setcasites [set] [sitename]
certutil [options] -setcasites verify [sitename]
certutil [options] -setcasites delete
```

Où :

- **SiteName** est autorisé uniquement lors du ciblage d’une autorité de certification unique.

```
[-f] [-config Machine\CAName] [-dc DCName]
```

#### <a name="remarks"></a>Notes 

- L' `-config` option cible une seule autorité de certification (par défaut, toutes les autorités de certification).

- L' `-f` option peut être utilisée pour remplacer les erreurs de validation pour le **SiteName** spécifié ou pour supprimer tous les sitenames d’autorité de certification.

> [!NOTE]
> Pour plus d’informations sur la configuration des autorités de certification pour la reconnaissance des sites Active Directory Domain Services (AD DS), consultez [AD DS la reconnaissance des sites pour les clients AD CS et PKI](https://social.technet.microsoft.com/wiki/contents/articles/14106.ad-ds-site-awareness-for-ad-cs-and-pki-clients.aspx).

### <a name="-enrollmentserverurl"></a>-enrollmentserverURL

Affiche, ajoute ou supprime les URL du serveur d’inscription associées à une autorité de certification.

```
certutil [options] -enrollmentServerURL [URL authenticationtype [priority] [modifiers]]
certutil [options] -enrollmentserverURL URL delete
```

Où :

- **AuthenticationType** spécifie l’une des méthodes d’authentification client suivantes, lors de l’ajout d’une URL :

  1. **Kerberos** : utilisez les informations d’identification Kerberos SSL.

  2. **nom d’utilisateur** : utilisez un compte nommé pour les informations d’identification SSL.

  3. **ClientCertificate**:-utilisez les informations d’identification SSL du certificat X. 509.

  4. **anonyme** : utilisez des informations d’identification SSL anonymes.

- **supprimer** supprime l’URL spécifiée associée à l’autorité de certification.

- la **priorité** est définie `1` par défaut sur si elle n’est pas spécifiée lors de l’ajout d’une URL.

- **modificateurs** est une liste séparée par des virgules, qui comprend un ou plusieurs des éléments suivants :

1. les demandes de renouvellement **allowrenewalsonly** uniquement peuvent être envoyées à cette autorité de certification par le biais de cette URL.

2. **allowkeybasedrenewal** : autorise l’utilisation d’un certificat qui n’a pas de compte associé dans la publicité. Cela s’applique uniquement avec le mode ClientCertificate et le mode allowrenewalsonly

```
[-config Machine\CAName] [-dc DCName]
```

### <a name="-adca"></a>-adca

Affiche Active Directory autorités de certification.

```
certutil [options] -adca [CAName]
```

```
[-f] [-split] [-dc DCName]
```

### <a name="-ca"></a>-ca

Affiche les autorités de certification de la stratégie d’inscription.

```
certutil [options] -CA [CAName | templatename]
```

```
[-f] [-user] [-silent] [-split] [-policyserver URLorID] [-anonymous] [-kerberos] [-clientcertificate clientcertID] [-username username] [-p password]
```

### <a name="-policy"></a>-stratégie

Affiche la stratégie d’inscription.

```
[-f] [-user] [-silent] [-split] [-policyserver URLorID] [-anonymous] [-kerberos] [-clientcertificate clientcertID] [-username username] [-p password]
```

### <a name="-policycache"></a>-policycache

Affiche ou supprime les entrées du cache de stratégie d’inscription.

```
certutil [options] -policycache [delete]
```

Où :

- **supprimer** supprime les entrées du cache du serveur de stratégie.

- **-f** supprime toutes les entrées de cache

```
[-f] [-user] [-policyserver URLorID]
```

### <a name="-credstore"></a>-credstore

Affiche, ajoute ou supprime des entrées de la Banque d’informations d’identification.

```
certutil [options] -credstore [URL]
certutil [options] -credstore URL add
certutil [options] -credstore URL delete
```

Où :

- **URL** est l’URL cible. Vous pouvez également utiliser `*` pour faire correspondre toutes les `https://machine*` entrées ou pour correspondre à un préfixe d’URL.

- **Ajouter** ajoute une entrée de magasin d’informations d’identification. L’utilisation de cette option requiert également l’utilisation d’informations d’identification SSL.

- **Delete** supprime les entrées de la Banque d’informations d’identification.

- **-f** remplace une seule entrée ou supprime plusieurs entrées.

```
[-f] [-user] [-silent] [-anonymous] [-kerberos] [-clientcertificate clientcertID] [-username username] [-p password]
```

### <a name="-installdefaulttemplates"></a>-installdefaulttemplates

Installe les modèles de certificats par défaut.

```
certutil [options] -installdefaulttemplates
```

```
[-dc DCName]
```

### <a name="-urlcache"></a>-URLcache

Affiche ou supprime les entrées du cache d’URL.

```
certutil [options] -URLcache [URL | CRL | * [delete]]
```

Où :

- **URL** est l’URL mise en cache.

- La **liste de révocation de certificats** s’exécute uniquement sur toutes les URL CRL mises en cache.

- **&#42;** fonctionne sur toutes les URL mises en cache.

- **supprimer** supprime les URL pertinentes du cache local de l’utilisateur actuel.

- **-f** force l’extraction d’une URL spécifique et la mise à jour du cache.

```
[-f] [-split]
```

### <a name="-pulse"></a>-Pulse

Pulse les événements d’inscription automatique.

```
certutil [options] -pulse
```

```
[-user]
```

### <a name="-machineinfo"></a>-machineinfo

Affiche des informations sur l’objet machine Active Directory.

```
certutil [options] -machineinfo domainname\machinename$
```

### <a name="-dcinfo"></a>-DCInfo

Affiche des informations sur le contrôleur de domaine. La valeur par défaut affiche les certificats DC sans vérification.

```
certutil [options] -DCInfo [domain] [verify | deletebad | deleteall]
```

```
[-f] [-user] [-urlfetch] [-dc DCName] [-t timeout]
```

> [!TIP]
> La possibilité de spécifier un domaine Active Directory Domain Services (AD DS) **[domaine]** et de spécifier un contrôleur de domaine (**-DC**) a été ajoutée dans Windows Server 2012. Pour exécuter la commande, vous devez utiliser un compte membre du **groupe Admins du domaine** ou administrateurs de l' **entreprise**. Les modifications de comportement de cette commande sont les suivantes :<ol><li>1. Si aucun domaine n’est spécifié et qu’aucun contrôleur de domaine n’est spécifié, cette option renvoie une liste des contrôleurs de domaine à traiter à partir du contrôleur de domaine par défaut.</li><li>2. Si aucun domaine n’est spécifié, mais qu’un contrôleur de domaine est spécifié, un rapport des certificats sur le contrôleur de domaine spécifié est généré.</li><li>3. Si un domaine est spécifié, mais qu’aucun contrôleur de domaine n’est spécifié, une liste de contrôleurs de domaine est générée, ainsi que des rapports sur les certificats pour chaque contrôleur de domaine de la liste.</li><li>4. Si le domaine et le contrôleur de domaine sont spécifiés, une liste de contrôleurs de domaine est générée à partir du contrôleur de domaine ciblé. Un rapport des certificats de chaque contrôleur de domaine dans la liste est également généré.</li></ol>
>
>Par exemple, supposons qu’il existe un domaine nommé CPANDL avec un contrôleur de domaine nommé CPANDL-DC1. Vous pouvez exécuter la commande suivante pour récupérer une liste de contrôleurs de domaine et leurs certificats provenant de CPANDL-DC1 :`certutil -dc cpandl-dc1 -DCInfo cpandl`

### <a name="-entinfo"></a>-entinfo

Affiche des informations sur une autorité de certification d’entreprise.

```
certutil [options] -entinfo domainname\machinename$
```

```
[-f] [-user]
```

### <a name="-tcainfo"></a>-tcainfo

Affiche des informations sur l’autorité de certification.

```
certutil [options] -tcainfo [domainDN | -]
```

```
[-f] [-enterprise] [-user] [-urlfetch] [-dc DCName] [-t timeout]
```

### <a name="-scinfo"></a>-scinfo

Affiche des informations sur la carte à puce.

```
certutil [options] -scinfo [readername [CRYPT_DELETEKEYSET]]
```

Où :

- **CRYPT_DELETEKEYSET** supprime toutes les clés de la carte à puce.

```
[-silent] [-split] [-urlfetch] [-t timeout]
```

### <a name="-scroots"></a>-scroots

Gère les certificats racines de carte à puce.

```
certutil [options] -scroots update [+][inputrootfile] [readername]
certutil [options] -scroots save \@in\\outputrootfile [readername]
certutil [options] -scroots view [inputrootfile | readername]
certutil [options] -scroots delete [readername]
```

```
[-f] [-split] [-p Password]
```

### <a name="-verifykeys"></a>-verifykeys

Vérifie un jeu de clés publiques ou privées.

```
certutil [options] -verifykeys [keycontainername cacertfile]
```

Où :

- **KeyContainerName** est le nom du conteneur de clé pour la clé à vérifier. Cette option est définie par défaut sur les clés d’ordinateur. Pour basculer vers les clés utilisateur `-user`, utilisez.

- **CACertFile** signe ou chiffre les fichiers de certificat.

```
[-f] [-user] [-silent] [-config Machine\CAName]
```

#### <a name="remarks"></a>Notes 

- Si aucun argument n’est spécifié, chaque certificat d’autorité de certification de signature est vérifié par rapport à sa clé privée.

- Cette opération ne peut être effectuée que sur une autorité de certification locale ou des clés locales.

### <a name="-verify"></a>-vérifier

Vérifie un certificat, une liste de révocation de certificats (CRL) ou une chaîne de certificats.

```
certutil [options] -verify certfile [applicationpolicylist | - [issuancepolicylist]]
certutil [options] -verify certfile [cacertfile [crossedcacertfile]]
certutil [options] -verify CRLfile cacertfile [issuedcertfile]
certutil [options] -verify CRLfile cacertfile [deltaCRLfile]
```

Où :

- **CertFile** est le nom du certificat à vérifier.

- **ApplicationPolicyList** est la liste facultative séparée par des virgules des ObjectIds de stratégie d’application requis.

- **IssuancePolicyList** est la liste facultative séparée par des virgules des ObjectIds de stratégie d’émission requis.

- **CACertFile** est le certificat d’autorité de certification émettrice facultatif à vérifier.

- **CrossedCACertFile** est le certificat facultatif qui est certifié entre **CertFile**.

- **FichierCRL** est le fichier de liste de révocation de certificats utilisé pour vérifier le **CACertFile**.

- **IssuedCertFile** est le certificat émis facultatif couvert par le FichierCRL.

- **deltaCRLfile** est le fichier de liste de révocation de certificats delta facultatif.

```
[-f] [-enterprise] [-user] [-silent] [-split] [-urlfetch] [-t timeout]
```

#### <a name="remarks"></a>Notes 

- L’utilisation de **ApplicationPolicyList** restreint la génération de chaînes aux chaînes valides pour les stratégies d’application spécifiées.

- L’utilisation de **IssuancePolicyList** restreint la génération de chaînes aux chaînes valides pour les stratégies d’émission spécifiées.

- L’utilisation de **CACertFile** vérifie les champs dans le fichier par rapport à **CertFile** ou **FichierCRL**.

- L’utilisation de **IssuedCertFile** vérifie les champs dans le fichier par rapport à **FichierCRL**.

- L’utilisation de deltaCRLfile vérifie les champs dans le fichier par rapport à **CertFile**.

- Si **CACertFile** n’est pas spécifié, la chaîne complète est générée et vérifiée par rapport à **CertFile**.

- Si **CACertFile** et **CrossedCACertFile** sont tous les deux spécifiés, les champs dans les deux fichiers sont vérifiés par rapport à **CertFile**.

### <a name="-verifyctl"></a>-verifyCTL

Vérifie la liste CTL de certificats AuthRoot ou non autorisés.

```
certutil [options] -verifyCTL CTLobject [certdir] [certfile]
```

Où :

- **CTLobject** identifie la liste de certificats de confiance à vérifier, y compris :

  - **AuthRootWU** : lit le fichier CAB AuthRoot et les certificats correspondants à partir du cache d’URL. Utilisez `-f` pour télécharger à partir de Windows Update à la place.

  - **DisallowedWU** : lit le fichier CAB des certificats non autorisés et le fichier de magasin de certificats non autorisé à partir du cache d’URL. Utilisez `-f` pour télécharger à partir de Windows Update à la place.

  - **AuthRoot** : lit la liste CTL AuthRoot mise en cache dans le registre. Utilisez avec `-f` et un **CertFile** non approuvé pour forcer la mise à jour des listes de certificats de confiance en cache AuthRoot et non autorisés.

  - Non **autorisé** : lit la liste CTL des certificats non autorisés mis en cache par le registre. Utilisez avec `-f` et un **CertFile** non approuvé pour forcer la mise à jour des listes de certificats de confiance en cache AuthRoot et non autorisés.

- **CTLfilename** spécifie le fichier ou le chemin d’accès http du fichier CTL ou CAB.

- **certdir** spécifie le dossier contenant les certificats correspondant aux entrées CTL. La valeur par défaut est le même dossier ou le même site Web que le **CTLobject**. L’utilisation d’un chemin d’accès de dossier http nécessite un séparateur de chemin d’accès à la fin. Si vous ne spécifiez pas **AuthRoot** ou n’est pas **autorisé**, plusieurs emplacements sont recherchés pour les certificats correspondants, y compris les magasins de certificats locaux, les ressources fichier crypt32. dll et le cache d’URL local. Utilisez `-f` pour télécharger à partir de Windows Update, si nécessaire.

- **CertFile** spécifie le ou les certificats à vérifier. Les certificats sont mis en correspondance avec les entrées CTL, affichant les résultats. Cette option supprime la plus grande partie de la sortie par défaut.

```
[-f] [-user] [-split]
```

### <a name="-sign"></a>-Sign

Signe à nouveau une liste de révocation de certificats (CRL) ou un certificat.

```
certutil [options] -sign infilelist | serialnumber | CRL outfilelist [startdate+dd:hh] [+serialnumberlist | -serialnumberlist | -objectIDlist | \@extensionfile]
certutil [options] -sign infilelist | serialnumber | CRL outfilelist [#hashalgorithm] [+alternatesignaturealgorithm | -alternatesignaturealgorithm]
```

Où :

- **infilelist** est la liste séparée par des virgules des fichiers de certificat ou de liste de révocation de certificats à modifier et à signer à nouveau.

- **SerialNumber** est le numéro de série du certificat à créer. La période de validité et d’autres options ne peuvent pas être présentes.

- La **liste CRL** crée une liste de révocation de certificats vide. La période de validité et d’autres options ne peuvent pas être présentes.

- **outfilelist** est la liste séparée par des virgules des fichiers de sortie de certificat ou de liste de révocation de certificats modifiés. Le nombre de fichiers doit correspondre à infilelist.

- **StartDate + JJ : HH** est la nouvelle période de validité pour les fichiers de certificat ou de liste de révocation de certificats, y compris :

  - Date facultative plus

  - période de validité des jours et heures facultatives
  
  Si les deux sont spécifiés, vous devez utiliser un séparateur de signe plus (+). Utilisez `now[+dd:hh]` pour démarrer à l’heure actuelle. Utilisez `never` pour n’avoir aucune date d’expiration (pour les listes de révocation de certificats uniquement).

- **SerialNumberList** est la liste de numéros de série séparés par des virgules des fichiers à ajouter ou supprimer.

- **objectIDlist** est la liste des ObjectId d’extensions séparées par des virgules des fichiers à supprimer.

- ExtensionFile est le fichier INF qui contient les extensions à mettre à jour ou à supprimer. ** \@** Par exemple :

  ```
  [Extensions]
      2.5.29.31 = ; Remove CRL Distribution Points extension
      2.5.29.15 = {hex} ; Update Key Usage extension
      _continue_=03 02 01 86
  ```

- **HashAlgorithm** est le nom de l’algorithme de hachage. Il doit s’agir du texte précédé du `#` signe.

- **alternatesignaturealgorithm** est l’autre spécificateur d’algorithme de signature.

```
[-nullsign] [-f] [-silent] [-cert certID]
```

#### <a name="remarks"></a>Notes 

- L’utilisation du signe moins (-) permet de supprimer les extensions et les numéros de série.

- L’utilisation du signe plus (+) ajoute des numéros de série à une liste de révocation de certificats.

- Vous pouvez utiliser une liste pour supprimer en même temps les numéros de série et les ObjectIDs d’une liste de révocation de certificats.

- L’utilisation du signe moins avant **alternatesignaturealgorithm** vous permet d’utiliser le format de signature hérité. L’utilisation du signe plus vous permet d’utiliser l’autre format de signature. Si vous ne spécifiez pas **alternatesignaturealgorithm**, le format de signature dans le certificat ou la liste de révocation de certificats est utilisé.

### <a name="-vroot"></a>-vroot

Crée ou supprime des racines virtuelles Web et des partages de fichiers.

```
certutil [options] -vroot [delete]
```

### <a name="-vocsproot"></a>-vocsproot

Crée ou supprime des racines virtuelles Web pour un proxy Web OCSP.

```
certutil [options] -vocsproot [delete]
```

### <a name="-addenrollmentserver"></a>-addenrollmentserver

Ajoutez une application serveur d’inscription et un pool d’applications, si nécessaire, pour l’autorité de certification spécifiée. Cette commande n’installe pas les fichiers binaires ou les packages.

```
certutil [options] -addenrollmentserver kerberos | username | clientcertificate [allowrenewalsonly] [allowkeybasedrenewal]
```

Où :

- **addenrollmentserver** vous oblige à utiliser une méthode d’authentification pour la connexion du client au serveur d’inscription de certificats, notamment :

  - **Kerberos** utilise les informations d’identification Kerberos SSL.
  
  - le **nom d’utilisateur** utilise le compte nommé pour les informations d’identification SSL.

  - **ClientCertificate** utilise les informations d’identification SSL du certificat X. 509.

- **allowrenewalsonly** autorise uniquement les soumissions de demande de renouvellement à l’autorité de certification par le biais de l’URL.

- **allowkeybasedrenewal** permet l’utilisation d’un certificat sans compte associé dans Active Directory. Cela s’applique en cas d’utilisation avec le mode **ClientCertificate** et le mode **allowrenewalsonly** .

```
[-config Machine\CAName]
```

### <a name="-deleteenrollmentserver"></a>-deleteenrollmentserver

Supprime une application du serveur d’inscription et un pool d’applications, si nécessaire, pour l’autorité de certification spécifiée. Cette commande n’installe pas les fichiers binaires ou les packages.

```
certutil [options] -deleteenrollmentserver kerberos | username | clientcertificate
```

Où :

- **deleteenrollmentserver** vous oblige à utiliser une méthode d’authentification pour la connexion du client au serveur d’inscription de certificats, notamment :

  - **Kerberos** utilise les informations d’identification Kerberos SSL.
  
  - le **nom d’utilisateur** utilise le compte nommé pour les informations d’identification SSL.

  - **ClientCertificate** utilise les informations d’identification SSL du certificat X. 509.

```
[-config Machine\CAName]
```

### <a name="-addpolicyserver"></a>-addpolicyserver

Ajoutez une application de serveur de stratégie et un pool d’applications, si nécessaire. Cette commande n’installe pas les fichiers binaires ou les packages.

```
certutil [options] -addpolicyserver kerberos | username | clientcertificate [keybasedrenewal]
```

Où :

- **addpolicyserver** vous oblige à utiliser une méthode d’authentification pour la connexion du client au serveur de stratégie de certificat, y compris :

  - **Kerberos** utilise les informations d’identification Kerberos SSL.
  
  - le **nom d’utilisateur** utilise le compte nommé pour les informations d’identification SSL.

  - **ClientCertificate** utilise les informations d’identification SSL du certificat X. 509.

- **keybasedrenewal** permet l’utilisation de stratégies retournées au client contenant les modèles keybasedrenewal. Cette option s’applique uniquement à l’authentification du **nom d’utilisateur** et du **ClientCertificate** .

### <a name="-deletepolicyserver"></a>-deletepolicyserver

Supprime une application de serveur de stratégie et un pool d’applications, si nécessaire. Cette commande ne supprime pas les fichiers binaires ou les packages.

certutil [options]-deletePolicyServer Kerberos | nom d’utilisateur | ClientCertificate [keybasedrenewal]

Où :

- **deletepolicyserver** vous oblige à utiliser une méthode d’authentification pour la connexion du client au serveur de stratégie de certificat, y compris :

  - **Kerberos** utilise les informations d’identification Kerberos SSL.
  
  - le **nom d’utilisateur** utilise le compte nommé pour les informations d’identification SSL.

  - **ClientCertificate** utilise les informations d’identification SSL du certificat X. 509.

- **keybasedrenewal** permet l’utilisation d’un serveur de stratégie keybasedrenewal.

### <a name="-oid"></a>-OID

Affiche l’identificateur d’objet ou définit un nom complet.

```
certutil [options] -oid objectID [displayname | delete [languageID [type]]]
certutil [options] -oid groupID
certutil [options] -oid agID | algorithmname [groupID]
```

Où :

- **ObjectID** affiche ou ajoute le nom complet.

- **GroupID** est le nombre GroupID (décimal) que objectIDs énumère.

- **algID** est l’ID hexadécimal que l’ObjectID recherche.

- **algorithmName** est le nom de l’algorithme que l’ObjectID recherche.

- **DisplayName** affiche le nom à stocker dans DS.

- **supprimer** supprime le nom complet.

- **LanguageID** est la valeur de l’ID de langue (valeur par défaut : 1033).

- **Type** est le type d’objet de service d’annuaire à créer, notamment :

  - `1`-Template (par défaut)
  
  - `2`-Stratégie d’émission

  - `3`-Stratégie d’application

- `-f`crée un objet de service d’annuaire.

### <a name="-error"></a>-erreur

Affiche le texte du message associé à un code d’erreur.

```
certutil [options] -error errorcode
```

### <a name="-getreg"></a>-getreg

Affiche une valeur de registre.

```
certutil [options] -getreg [{ca | restore | policy | exit | template | enroll |chain | policyservers}\[progID\]][registryvaluename]
```

Où :

- l' **autorité de certification** utilise la clé de registre de l’autorité de certification.

- la **restauration** utilise la clé de Registre Restore de l’autorité de certification.

- la **stratégie** utilise la clé de Registre du module de stratégie.

- **Exit** utilise la clé de Registre du premier module de sortie.

- le **modèle** utilise la clé de registre de `-user` modèle (à utiliser pour les modèles utilisateur).

- l' **inscription** utilise la clé de registre d’inscription ( `-user` à utiliser pour le contexte utilisateur).

- la **chaîne** utilise la clé de registre Configuration de la chaîne.

- **policyservers** utilise la clé de Registre serveurs de stratégie.

- **ProgID** utilise le ProgID du module de stratégie ou de sortie (nom de la sous-clé du registre).

- **registryvaluename** utilise le nom de la valeur de `Name*` Registre (à utiliser pour la correspondance de préfixe).

- **valeur** utilise la nouvelle valeur de Registre numérique, de chaîne ou de date ou de nom de fichier. Si une valeur numérique commence par `+` ou `-`, les bits spécifiés dans la nouvelle valeur sont définis ou désactivés dans la valeur de Registre existante.

```
[-f] [-user] [-grouppolicy] [-config Machine\CAName]
```

#### <a name="remarks"></a>Notes 

- Si une valeur de chaîne commence `+` par `-`ou, et que la valeur existante `REG_MULTI_SZ` est une valeur, la chaîne est ajoutée à la valeur de Registre existante ou en est supprimée. Pour forcer la création d' `REG_MULTI_SZ` une valeur, `\n` ajoutez à la fin de la valeur de chaîne.

- Si la valeur commence par `\@`, le reste de la valeur est le nom du fichier contenant la représentation textuelle hexadécimale d’une valeur binaire. Si elle ne fait pas référence à un fichier valide, elle est analysée `[Date][+|-][dd:hh]` comme une date facultative plus ou moins les jours et heures facultatifs. Si les deux sont spécifiés, utilisez un signe plus (+) ou un signe moins (-). Utilisez `now+dd:hh` pour une date relative à l’heure actuelle.

- Utilisez `chain\chaincacheresyncfiletime \@now` pour vider efficacement les listes de révocation de certificats mises en cache.

### <a name="-setreg"></a>-setreg

Définit une valeur de registre.

```
certutil [options] -setreg [{ca | restore | policy | exit | template | enroll |chain | policyservers}\[progID\]]registryvaluename value
```

Où :

- l' **autorité de certification** utilise la clé de registre de l’autorité de certification.

- la **restauration** utilise la clé de Registre Restore de l’autorité de certification.

- la **stratégie** utilise la clé de Registre du module de stratégie.

- **Exit** utilise la clé de Registre du premier module de sortie.

- le **modèle** utilise la clé de registre de `-user` modèle (à utiliser pour les modèles utilisateur).

- l' **inscription** utilise la clé de registre d’inscription ( `-user` à utiliser pour le contexte utilisateur).

- la **chaîne** utilise la clé de registre Configuration de la chaîne.

- **policyservers** utilise la clé de Registre serveurs de stratégie.

- **ProgID** utilise le ProgID du module de stratégie ou de sortie (nom de la sous-clé du registre).

- **registryvaluename** utilise le nom de la valeur de `Name*` Registre (à utiliser pour la correspondance de préfixe).

- **valeur** utilise la nouvelle valeur de Registre numérique, de chaîne ou de date ou de nom de fichier. Si une valeur numérique commence par `+` ou `-`, les bits spécifiés dans la nouvelle valeur sont définis ou désactivés dans la valeur de Registre existante.

```
[-f] [-user] [-grouppolicy] [-config Machine\CAName]
```

#### <a name="remarks"></a>Notes 

- Si une valeur de chaîne commence `+` par `-`ou, et que la valeur existante `REG_MULTI_SZ` est une valeur, la chaîne est ajoutée à la valeur de Registre existante ou en est supprimée. Pour forcer la création d' `REG_MULTI_SZ` une valeur, `\n` ajoutez à la fin de la valeur de chaîne.

- Si la valeur commence par `\@`, le reste de la valeur est le nom du fichier contenant la représentation textuelle hexadécimale d’une valeur binaire. Si elle ne fait pas référence à un fichier valide, elle est analysée `[Date][+|-][dd:hh]` comme une date facultative plus ou moins les jours et heures facultatifs. Si les deux sont spécifiés, utilisez un signe plus (+) ou un signe moins (-). Utilisez `now+dd:hh` pour une date relative à l’heure actuelle.

- Utilisez `chain\chaincacheresyncfiletime \@now` pour vider efficacement les listes de révocation de certificats mises en cache.

### <a name="-delreg"></a>-DelReg

Supprime une valeur de registre.

```
certutil [options] -delreg [{ca | restore | policy | exit | template | enroll |chain | policyservers}\[progID\]][registryvaluename]
```

Où :

- l' **autorité de certification** utilise la clé de registre de l’autorité de certification.

- la **restauration** utilise la clé de Registre Restore de l’autorité de certification.

- la **stratégie** utilise la clé de Registre du module de stratégie.

- **Exit** utilise la clé de Registre du premier module de sortie.

- le **modèle** utilise la clé de registre de `-user` modèle (à utiliser pour les modèles utilisateur).

- l' **inscription** utilise la clé de registre d’inscription ( `-user` à utiliser pour le contexte utilisateur).

- la **chaîne** utilise la clé de registre Configuration de la chaîne.

- **policyservers** utilise la clé de Registre serveurs de stratégie.

- **ProgID** utilise le ProgID du module de stratégie ou de sortie (nom de la sous-clé du registre).

- **registryvaluename** utilise le nom de la valeur de `Name*` Registre (à utiliser pour la correspondance de préfixe).

- **valeur** utilise la nouvelle valeur de Registre numérique, de chaîne ou de date ou de nom de fichier. Si une valeur numérique commence par `+` ou `-`, les bits spécifiés dans la nouvelle valeur sont définis ou désactivés dans la valeur de Registre existante.

```
[-f] [-user] [-grouppolicy] [-config Machine\CAName]
```

#### <a name="remarks"></a>Notes 

- Si une valeur de chaîne commence `+` par `-`ou, et que la valeur existante `REG_MULTI_SZ` est une valeur, la chaîne est ajoutée à la valeur de Registre existante ou en est supprimée. Pour forcer la création d' `REG_MULTI_SZ` une valeur, `\n` ajoutez à la fin de la valeur de chaîne.

- Si la valeur commence par `\@`, le reste de la valeur est le nom du fichier contenant la représentation textuelle hexadécimale d’une valeur binaire. Si elle ne fait pas référence à un fichier valide, elle est analysée `[Date][+|-][dd:hh]` comme une date facultative plus ou moins les jours et heures facultatifs. Si les deux sont spécifiés, utilisez un signe plus (+) ou un signe moins (-). Utilisez `now+dd:hh` pour une date relative à l’heure actuelle.

- Utilisez `chain\chaincacheresyncfiletime \@now` pour vider efficacement les listes de révocation de certificats mises en cache.

### <a name="-importkms"></a>-importKMS

Importe des clés et des certificats d’utilisateur dans la base de données du serveur pour l’archivage des clés.

```
certutil [options] -importKMS userkeyandcertfile [certID]
```

Où :

- **UserKeyAndCertFile** est un fichier de données avec les clés privées de l’utilisateur et les certificats qui doivent être archivés. Ce fichier peut être :

  - Un fichier d’exportation du serveur de gestion de clés (KMS) Exchange.

  - Fichier PFX.

- certID est un jeton de correspondance du certificat de déchiffrement du fichier d’exportation KMS. Pour plus d’informations, consultez `-store` le paramètre dans cet article.

- `-f`importe des certificats non émis par l’autorité de certification.

```
[-f] [-silent] [-split] [-config Machine\CAName] [-p password] [-symkeyalg symmetrickeyalgorithm[,keylength]]
```

### <a name="-importcert"></a>-importcert

Importe un fichier de certificat dans la base de données.

```
certutil [options] -importcert certfile [existingrow]
```

Où :

- **existingrow** importe le certificat à la place d’une demande en attente pour la même clé.

- `-f`importe des certificats non émis par l’autorité de certification.

```
[-f] [-config Machine\CAName]
```

#### <a name="remarks"></a>Notes 

Il se peut également que l’autorité de certification doive être configurée pour prendre en charge les certificats étrangers. Pour ce faire, tapez `import - certutil -setreg ca\KRAFlags +KRAF_ENABLEFOREIGN`.

### <a name="-getkey"></a>-GetKey

Récupère un objet blob de récupération de clé privée archivée, génère un script de récupération ou récupère les clés archivées.

```
certutil [options] -getkey searchtoken [recoverybloboutfile]
certutil [options] -getkey searchtoken script outputscriptfile
certutil [options] -getkey searchtoken retrieve | recover outputfilebasename
```

Où :

- le **script** génère un script pour récupérer et récupérer les clés (comportement par défaut si plusieurs candidats de récupération correspondants sont trouvés, ou si le fichier de sortie n’est pas spécifié).

- la **récupération récupère un** ou plusieurs objets BLOB de récupération de clé (comportement par défaut si un seul candidat de récupération correspondant est trouvé et si le fichier de sortie est spécifié). L’utilisation de cette option tronque toute extension et ajoute la chaîne propre au certificat et l’extension. REC pour chaque objet blob de récupération de clé.  Chaque fichier contient une chaîne de certificats et une clé privée associée, toujours chiffrés sur un ou plusieurs certificats de l’agent de récupération de clé.

- la **récupération** récupère et récupère les clés privées en une seule étape (requiert des certificats et des clés privées de l’agent de récupération de clé). L’utilisation de cette option tronque toute extension et ajoute l’extension. P12.  Chaque fichier contient les chaînes de certificats récupérées et les clés privées associées, stockées en tant que fichier PFX.

- **SearchToken** sélectionne les clés et les certificats à récupérer, y compris :

  - 1. Nom commun du certificat
  
  - 2. Numéro de série du certificat
  
  - 3. Hachage SHA-1 du certificat (empreinte numérique)
  
  - 4. Hachage SHA-1 de KeyId de certificat (identificateur de clé du sujet)
  
  - 5. Nom du demandeur (domaine\utilisateur)
  
  - 6. UPN (domaine\@utilisateur)

- **RecoveryBlobOutFile** génère un fichier avec une chaîne de certificats et une clé privée associée, toujours chiffrés sur un ou plusieurs certificats de l’agent de récupération de clé.

- **outputScriptFile** génère un fichier avec un script de commandes pour récupérer et récupérer les clés privées.

- **outputfilebasename** génère un nom de base de fichier.

```
[-f] [-unicodetext] [-silent] [-config Machine\CAName] [-p password] [-protectto SAMnameandSIDlist] [-csp provider]
```

### <a name="-recoverkey"></a>-recoverkey

Récupérer une clé privée archivée.

```
certutil [options] -recoverkey recoveryblobinfile [PFXoutfile [recipientindex]]
```

```
[-f] [-user] [-silent] [-split] [-p password] [-protectto SAMnameandSIDlist] [-csp provider] [-t timeout]
```

### <a name="-mergepfx"></a>-mergePFX

Fusionne les fichiers PFX.

```
certutil [options] -mergePFX PFXinfilelist PFXoutfile [extendedproperties]
```

Où :

- **PFXinfilelist** est une liste séparée par des virgules de fichiers d’entrée pfx.

- **PFXoutfile** est le nom du fichier de sortie pfx.

- **ExtendedProperties** comprend toutes les propriétés étendues.

```
[-f] [-user] [-split] [-p password] [-protectto SAMnameAndSIDlist] [-csp provider]
```

#### <a name="remarks"></a>Notes 

- Le mot de passe spécifié sur la ligne de commande doit être une liste de mots de passe séparés par des virgules.

- Si plusieurs mots de passe sont spécifiés, le dernier mot de passe est utilisé pour le fichier de sortie. Si un seul mot de passe est fourni ou si le dernier `*`mot de passe est, l’utilisateur est invité à entrer le mot de passe du fichier de sortie.

### <a name="-convertepf"></a>-convertEPF

Convertit un fichier PFX en fichier EPF.

```
certutil [options] -convertEPF PFXinfilelist PFXoutfile [cast | cast-] [V3CAcertID][,salt]
```

Où :


- **PFXinfilelist** est une liste séparée par des virgules de fichiers d’entrée pfx.

- **PFXoutfile** est le nom du fichier de sortie pfx.

- **EPF** est le nom du fichier de sortie EPF.

- **Cast** utilise le chiffrement Cast 64.

- **Cast-utilise le** chiffrement Cast 64 (Export)

- **V3CAcertID** est le jeton de correspondance du certificat d’autorité de certification v3. Pour plus d’informations, consultez `-store` le paramètre dans cet article.

- **Salt** est la chaîne Salt du fichier de sortie EPF.

```
[-f] [-silent] [-split] [-dc DCName] [-p password] [-csp provider]
```

#### <a name="remarks"></a>Notes 

- Le mot de passe spécifié sur la ligne de commande doit être une liste de mots de passe séparés par des virgules.

- Si plusieurs mots de passe sont spécifiés, le dernier mot de passe est utilisé pour le fichier de sortie. Si un seul mot de passe est fourni ou si le dernier `*`mot de passe est, l’utilisateur est invité à entrer le mot de passe du fichier de sortie.

### <a name="-"></a>-?

Affiche la liste des paramètres.

``` 
certutil -?
certutil <name_of_parameter> -?
certutil -? -v
```

Où :

- **-?** affiche la liste complète des paramètres

- **-`<name_of_parameter>` -?** affiche le contenu de l’aide pour le paramètre spécifié.

- **- ?-v** affiche la liste complète des paramètres et options.

## <a name="options"></a>Options

Cette section définit toutes les options que vous êtes en mesure de spécifier, en fonction de la commande. Chaque paramètre contient des informations sur les options qui sont valides pour l’utilisation.

| Options | Description |
| ------- | ----------- |
| -nullsign | Utilisez le hachage des données comme une signature. |
| -f | Forcer le remplacement. |
| -enterprise | Utilisez le magasin de certificats du registre de l’ordinateur local de l’entreprise. |
| -utilisateur | Utilisez les clés de HKEY_CURRENT_USER ou le magasin de certificats. |
| -GroupPolicy | Utilisez le magasin de certificats de la stratégie de groupe. |
| -UT | Affichez les modèles utilisateur. |
| -MT | Affichez les modèles d’ordinateur. |
| -Unicode | Écrire une sortie redirigée en Unicode. |
| -UnicodeText | Écriture du fichier de sortie au format Unicode. |
| -GMT | Afficher les heures en utilisant l’heure GMT. |
| -secondes | Affichez les heures en utilisant des secondes et des millisecondes. |
| -silencieux | Utilisez l' `silent` indicateur pour acquérir le contexte de chiffrement. |
| -Split | Fractionnez les éléments ASN. 1 incorporés et enregistrez-les dans des fichiers. |
| -v | Fournissez des informations plus détaillées (détaillées). |
| -PrivateKey | Affichez les données de mot de passe et de clé privée. |
| -Pin pin | Code confidentiel de la carte à puce. |
| -urlfetch | Récupérer et vérifier les certificats AIA et les listes de révocation de certificats CDP. |
| -config Machine\CAName | Autorité de certification et chaîne de nom d’ordinateur. |
| -policyserver URLorID | URL ou ID du serveur de stratégie. Pour la sélection U/I, `-policyserver`utilisez. Pour tous les serveurs de stratégie, utilisez`-policyserver *`|
| -Anonyme | Utilisez des informations d’identification SSL anonymes. |
| -Kerberos | Utilisez les informations d’identification Kerberos SSL. |
| -ClientCertificate clientcertID | Utilisez les informations d’identification SSL du certificat X. 509. Pour la sélection U/I, `-clientcertificate`utilisez. |
| -username nom_utilisateur | Utilisez le compte nommé pour les informations d’identification SSL. Pour la sélection U/I, `-username`utilisez. |
| -certificat certID | Certificat de signature. |
| -DC DCName | Ciblez un contrôleur de domaine spécifique. |
| -restreindre RestrictionList | Liste de restrictions séparées par des virgules. Chaque restriction comprend un nom de colonne, un opérateur relationnel et un entier constant, une chaîne ou une date. Un nom de colonne peut être précédé d’un signe plus ou moins pour indiquer l’ordre de tri. Par exemple : `requestID = 47`, `+requestername >= a, requestername` ou `-requestername > DOMAIN, Disposition = 21` |
| -out ColumnList | Liste de colonnes séparées par des virgules. |
| -p mot de passe | Mot de passe |
| -protectto SAMnameandSIDlist | Liste des noms SAM/SID séparés par des virgules. |
| -fournisseur CSP | Fournisseur |
| -t délai d’expiration | Délai d’expiration de l’extraction d’URL en millisecondes. |
| -symkeyalg symmetrickeyalgorithm [, KeyLength] | Nom de l’algorithme de clé symétrique avec une longueur de clé facultative. Par exemple : `AES,128` ou`3DES` |

### <a name="additional-references"></a>Références supplémentaires

Pour obtenir des exemples supplémentaires sur l’utilisation de cette commande, consultez.

- [Exemples certutil pour la gestion des services de certificats Active Directory (AD CS) à partir de la ligne de commande](https://social.technet.microsoft.com/wiki/contents/articles/3063.certutil-examples-for-managing-active-directory-certificate-services-ad-cs-from-the-command-line.aspx)

- [Tâches certutil pour la gestion des certificats](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc772898(v=ws.10))

- [Exportation des demandes binaires à l’aide de l’outil en ligne de commande certutil. exe](https://social.technet.microsoft.com/wiki/contents/articles/7573.active-directory-certificate-services-pki-key-archival-and-management.aspx)

- [Renouvellement du certificat d’autorité de certification racine](https://social.technet.microsoft.com/wiki/contents/articles/2016.root-ca-certificate-renewal.aspx)

- [commande certutil](certutil.md)
