---
title: certutil
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c264ccf0-ba1e-412b-9dd3-d77dd9345ad9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45c9946cc53fe3a901c3f6ee53f082a5b3d086c0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379650"
---
# <a name="certutil"></a>certutil

Certutil. exe est un programme de ligne de commande installé dans le cadre des services de certificats. Vous pouvez utiliser certutil. exe pour vider et afficher les informations de configuration de l’autorité de certification, configurer les services de certificats, sauvegarder et restaurer les composants de l’autorité de certification et vérifier les certificats, les paires de clés et les chaînes de certificats.

Lorsque certutil est exécutée sur une autorité de certification sans paramètres supplémentaires, elle affiche la configuration de l’autorité de certification actuelle. Quand cerutil est exécuté sur une autorité de certification qui n’est pas une autorité de certification, la commande utilise par défaut le verbe certutil [-dump](#-dump) .

> [!WARNING]
> Les versions antérieures de Certutil peuvent ne pas fournir toutes les options décrites dans ce document. Vous pouvez voir toutes les options fournies par une version spécifique de Certutil en exécutant les commandes présentées dans la section [notations de syntaxe](#syntax-notations) .

## <a name="menu"></a>Menu

Les principales sections de ce document sont les suivantes :

- [Verbes](#verbs)
- [Notations de syntaxe](#syntax-notations)
- [Options](#options)
- [Exemples supplémentaires de Certutil](#additional-certutil-examples)

## <a name="verbs"></a>Verbes

Le tableau suivant décrit les verbes qui peuvent être utilisés avec la commande certutil.

|Verbes|Description|
|-----|-----------|
|[-dump](#-dump)|Vider les informations de configuration ou les fichiers|
|[-ASN](#-asn)|Analyser le fichier ASN. 1|
|[-decodehex](#-decodehex)|Décoder un fichier encodé au format hexadécimal|
|[-Decode](#-decode)|Décoder un fichier encodé en base64|
|[-encode](#-encode)|Encoder un fichier en base64|
|[-refuser](#-deny)|Refuser une demande de certificat en attente|
|[-renvoyer](#-resubmit)|Soumettre à nouveau une demande de certificat en attente|
|[-SetAttributes](#-setattributes)|Définir les attributs d’une demande de certificat en attente|
|[-setextension](#-setextension)|Définir une extension pour une demande de certificat en attente|
|[-Revoke](#-revoke)|Révoquer un certificat|
|[-IsValid](#-isvalid)|Afficher la disposition du certificat actuel|
|[-GetConfig](#-getconfig)|Obtient la chaîne de configuration par défaut|
|[-ping](#-ping)|Tentative de contact de l’interface de demande Active Directory Certificate Services|
|-pingadmin|Tentative de contact de l’interface d’administration des services de certificats Active Directory|
|[-Infos](#-cainfo)|Afficher des informations sur l’autorité de certification|
|[-ca. cert](#-cacert)|Récupérer le certificat de l’autorité de certification|
|[-ca. Chain](#-cachain)|Récupérer la chaîne de certificats pour l’autorité de certification|
|[-GetCRL](#-getcrl)|Obtenir une liste de révocation de certificats (CRL)|
|[-CRL](#-crl)|Publier de nouvelles listes de révocation de certificats (CRL) [ou uniquement les listes de révocation de certificats delta]|
|[-Shutdown](#-shutdown)|Arrêter Active Directory les services de certificats|
|[-installCert](#-installcert)|Installer un certificat d’autorité de certification|
|[-renewCert](#-renewcert)|Renouveler un certificat d’autorité de certification|
|[-schéma](#-schema)|Vider le schéma pour le certificat|
|[-afficher](#-view)|Vider la vue de certificat|
|[-DB](#-db)|Vider la base de données brute|
|[-deleteRow](#-deleterow)|Supprimer une ligne de la base de données du serveur|
|[-sauvegarde](#-backup)|Sauvegarde Active Directory les services de certificats|
|[-backupDB](#-backupdb)|Sauvegarder la base de données des services de certificats Active Directory|
|[-backupKey](#-backupkey)|Sauvegarder le certificat et la clé privée des services de certificats Active Directory|
|[-restaurer](#-restore)|Restaurer les services de certificats Active Directory|
|[-restoreDB](#-restoredb)|Restaurer la base de données des services de certificats Active Directory|
|[-restoreKey](#-restorekey)|Restaurer le certificat et la clé privée des services de certificats Active Directory|
|[-importPFX](#-importpfx)|Importer un certificat et une clé privée|
|[-dynamicfilelist](#-dynamicfilelist)|Afficher une liste de fichiers dynamiques|
|[-databaselocations](#-databaselocations)|Afficher les emplacements des bases de données|
|[-hashfile](#-hashfile)|Générer et afficher un hachage de chiffrement sur un fichier|
|[-Store](#-store)|Vider le magasin de certificats|
|[-AddStore](#-addstore)|Ajouter un certificat au magasin|
|[-delstore](#-delstore)|Supprimer un certificat du magasin|
|[-verifystore](#-verifystore)|Vérifier un certificat dans le magasin|
|[-repairstore](#-repairstore)|Réparer une association de clé ou mettre à jour des propriétés de certificat ou le descripteur de sécurité de clé|
|[-viewstore](#-viewstore)|Vider le magasin de certificats|
|[-viewdelstore](#-viewdelstore)|Supprimer un certificat du magasin|
|[-dsPublish](#-dspublish)|Publier un certificat ou une liste de révocation de certificats (CRL) pour Active Directory|
|[-ADTemplate](#-adtemplate)|Afficher les modèles AD|
|[-Modèle](#-template)|Afficher les modèles de certificats|
|[-TemplateCAs](#-templatecas)|Afficher les autorités de certification (ca) pour un modèle de certificat|
|[-Camodèles](#-catemplates)|Afficher les modèles pour l’autorité de certification|
|[-SetCASites](#-setcasites)|Gérer les noms de site pour les autorités de certification|
|[-enrollmentServerURL](#-enrollmentserverurl)|Afficher, ajouter ou supprimer des URL de serveur d’inscription associées à une autorité de certification|
|[-ADCA](#-adca)|Afficher les autorités de certification AD|
|[-CA](#-ca)|Afficher les autorités de certification de stratégie d’inscription|
|[-Stratégie](#-policy)|Afficher la stratégie d’inscription|
|[-PolicyCache](#-policycache)|Afficher ou supprimer les entrées du cache de stratégie d’inscription|
|[-CredStore](#-credstore)|Afficher, ajouter ou supprimer des entrées de la Banque d’informations d’identification|
|[-InstallDefaultTemplates](#-installdefaulttemplates)|Installer les modèles de certificats par défaut|
|[-URLCache](#-urlcache)|Afficher ou supprimer les entrées du cache d’URL|
|[-Pulse](#-pulse)|Événements d’inscription automatique par impulsions|
|[-MachineInfo](#-machineinfo)|Afficher des informations sur l’objet machine Active Directory|
|[-DCInfo](#-dcinfo)|Afficher des informations sur le contrôleur de domaine|
|[-EntInfo](#-entinfo)|Afficher des informations sur une autorité de certification d’entreprise|
|[-TCAInfo](#-tcainfo)|Afficher des informations sur l’autorité de certification|
|[-SCInfo](#-scinfo)|Afficher des informations sur la carte à puce|
|[-SCRoots](#-scroots)|Gérer les certificats racines de carte à puce|
|[-verifykeys](#-verifykeys)|Vérifier un ensemble de clés publiques ou privées|
|[-vérifier](#-verify)|Vérifier un certificat, une liste de révocation de certificats (CRL) ou une chaîne de certificats|
|[-verifyCTL](#-verifyctl)|Vérifier la liste CTL des certificats AuthRoot ou non autorisés|
|[-Sign](#-sign)|Signer à nouveau une liste de révocation de certificats (CRL) ou un certificat|
|[-vroot](#-vroot)|Créer ou supprimer des racines virtuelles Web et des partages de fichiers|
|[-vocsproot](#-vocsproot)|Créer ou supprimer des racines virtuelles Web pour un proxy Web OCSP|
|[-addEnrollmentServer](#-addenrollmentserver)|Ajouter une application de serveur d’inscription|
|[-deleteEnrollmentServer](#-deleteenrollmentserver)|Supprimer une application de serveur d’inscription|
|[-addPolicyServer](#-addpolicyserver)|Ajouter une application de serveur de stratégie|
|[-deletePolicyServer](#-deletepolicyserver)|Supprimer une application de serveur de stratégie|
|[-OID](#-oid)|Afficher l’identificateur d’objet ou définir un nom complet|
|[-erreur](#-error)|Afficher le texte du message associé à un code d’erreur|
|[-getreg](#-getreg)|Afficher une valeur de Registre|
|[-setreg](#-setreg)|Définir une valeur de Registre|
|[-DelReg](#-delreg)|Supprimer une valeur de Registre|
|[-ImportKMS](#-importkms)|Importer des clés et des certificats d’utilisateur dans la base de données du serveur pour l’archivage des clés|
|[-ImportCert](#-importcert)|Importer un fichier de certificat dans la base de données|
|[-GetKey](#-getkey)|Récupérer un objet blob de récupération de clé privée archivée|
|[-RecoverKey](#-recoverkey)|Récupérer une clé privée archivée|
|[-MergePFX](#-mergepfx)|Fusionner les fichiers PFX|
|[-ConvertEPF](#-convertepf)|Convertir un fichier PFX en fichier EPF|
|-?|Affiche la liste des verbes|
|- *\<verbe >* - ?|Affiche l’aide pour le verbe spécifié.|
|-? -v|Affiche la liste complète des verbes et|

Revenir au [menu](#menu)

## <a name="syntax-notations"></a>Notations de syntaxe

- Pour la syntaxe de ligne de commande de base, exécutez `certutil -?`
- Pour obtenir la syntaxe d’utilisation de Certutil avec un verbe spécifique, exécutez **certutil** *\<Verb >* **- ?**
- Pour envoyer l’ensemble de la syntaxe certutil dans un fichier texte, exécutez les commandes suivantes :  
  - `certutil -v -? > certutilhelp.txt`
  - `notepad certutilhelp.txt`

Le tableau suivant décrit la notation utilisée pour indiquer la syntaxe de la ligne de commande.


|            Conventions             |                  Description                  |
|---------------------------------|-----------------------------------------------|
| Texte sans crochets ou accolades |         Éléments que vous devez taper comme indiqué          |
|  \<texte à l’intérieur des crochets pointus >  | Espace réservé pour lequel vous devez fournir une valeur |
|  [Texte à l’intérieur des crochets]  |                Éléments facultatifs                 |
|      {Text à l’intérieur des accolades}       |       Ensemble d’éléments requis ; choisir un       |
|         Barre verticale (          |                       )                       |
|          Points de suspension (…)           |          Éléments pouvant être répétés           |

Revenir au [menu](#menu)

## <a name="-dump"></a>-dump

CertUtil [options] [-dump]

CertUtil [options] [-dump] fichier

Vider les informations de configuration ou les fichiers

[-f] [-Silent] [-Split] [-p mot de passe] [-t délai d’expiration]

Revenir au [menu](#menu)

## <a name="-asn"></a>-ASN

CertUtil [options]-fichier ASN [type]

Analyser le fichier ASN. 1

type : Numeric CRYPT\_STRING\_\* le type de décodage

Revenir au [menu](#menu)

## <a name="-decodehex"></a>-decodehex

CertUtil [options]-decodehex INFILE out [type]

type : Numeric CRYPT\_STRING\_type d’encodage \*

[-f]

Revenir au [menu](#menu)

## <a name="-decode"></a>-Decode

CertUtil [options]-décoder le fichier out

Décoder un fichier encodé en base64

[-f]

Revenir au [menu](#menu)

## <a name="-encode"></a>-encode

CertUtil [options]-encode out INFILE

Encoder un fichier en base64

[-f] [-UnicodeText]

Revenir au [menu](#menu)

## <a name="-deny"></a>-refuser

CertUtil [options]-refuser RequestId

Refuser la demande en attente

[-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-resubmit"></a>-renvoyer

CertUtil [options]-soumettre à nouveau RequestId

Soumettre à nouveau une demande en attente

[-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-setattributes"></a>-SetAttributes

CertUtil [options]-SetAttributes RequestId AttributeString

Définir les attributs pour la demande en attente

RequestId-ID de demande numérique de demande en attente

AttributeString--paires nom et valeur d’attribut de requête

- Les noms et les valeurs sont séparés par des points-virgules.
- Plusieurs paires nom/valeur sont séparées par des sauts de ligne.
- Exemple : «CertificateTemplate:User\nEMail:User@Domain.com»
- Chaque séquence « \n » est convertie en séparateur de saut de ligne.

[-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-setextension"></a>-setextension

CertUtil [options]-SetExtension RequestId ExtensionName Flags {long | Date | Chaîne | \@INFILE}

Définir l’extension pour la demande en attente

RequestId--ID de demande numérique d’une demande en attente

ExtensionName--chaîne ObjectId de l’extension

Flags--0 est recommandé.  1 rend l’extension critique, 2 la désactive, 3 les deux.

Si le dernier paramètre est numérique, il est considéré comme long.

Si elle peut être analysée en tant que date, elle est considérée comme une date.

S’il commence par «\@», le reste du jeton est le nom de fichier contenant des données binaires ou un vidage hexadécimal de texte ASCII.

Tout autre chose est pris comme une chaîne.

[-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-revoke"></a>-Revoke

CertUtil [options]-Revoke SerialNumber [raison]

Révoquer un certificat

SerialNumber : liste séparée par des virgules des numéros de série de certificats à révoquer

Raison : valeur numérique ou raison de la révocation symbolique

- 0 : CRL_REASON_UNSPECIFIED : non spécifié (par défaut)
- 1 : CRL_REASON_KEY_COMPROMISE : clé compromise
- 2 : CRL_REASON_CA_COMPROMISE : violation de l’autorité de certification
- 3 : CRL_REASON_AFFILIATION_CHANGED : affiliation modifiée
- 4 : CRL_REASON_SUPERSEDED : remplacé
- 5 : CRL_REASON_CESSATION_OF_OPERATION : arrêt de l’opération
- 6 : CRL_REASON_CERTIFICATE_HOLD : certificat suspendu
- 8 : CRL_REASON_REMOVE_FROM_CRL : supprimer de la liste de révocation des certificats
- -1 : annuler la révocation : annuler la révocation

[-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-isvalid"></a>-IsValid

CertUtil [options]-IsValid SerialNumber | CertHash

Afficher la disposition actuelle du certificat

[-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-getconfig"></a>-GetConfig

CertUtil [options]-GetConfig

Obtient la chaîne de configuration par défaut

[-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-ping"></a>-ping

CertUtil [options]-ping [MaxSecondsToWait | CAMachineList]

Ping Active Directory interface de demande des services de certificats

CAMachineList--liste de noms d’ordinateurs de l’autorité de certification séparée par des virgules

1. Pour un seul ordinateur, utilisez une virgule de fin
2. Affiche le coût du site pour chaque ordinateur de l’autorité de certification

[-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-cainfo"></a>-Infos

CertUtil [options]-cainfo [InfoName [index | ErrorCode]]

Afficher les informations de l’autorité de certification

InfoName : indique la propriété de l’autorité de certification à afficher (voir ci-dessous). Utilisez «\*» pour toutes les propriétés.

Index--index de propriété de base zéro facultatif

ErrorCode--code d’erreur numérique

[-f] [-Split] [-config Machine\CAName]

Syntaxe de l’argument InfoName :

- fichier : version du fichier
- produit : version du produit
- exitcount : quitter le nombre de modules
- Exit [index] : sortie du module Description
- stratégie : description du module de stratégie
- nom : nom de l’autorité de certification
- sanitizedname : nom de l’autorité de certification expurgée
- dsname : nom abrégé de l’autorité de certification expurgé (nom DS)
- SharedFolder : dossier partagé
- Error1 ErrorCode : texte du message d’erreur
- Error2 ErrorCode : texte du message d’erreur et code d’erreur
- type : AC type
- info : informations sur l’autorité de certification
- parent : autorité de certification parente
- certcount : nombre de certificats d’autorité de certification
- xchgcount : nombre de certificats d’échange d’autorités de certification
- kracount : nombre de certificats KRA
- kraused : nombre d’utilisations du certificat KRA utilisé
- propidmax : ID d’autorité de certification maximal
- certstate [index] : certificat d’autorité de certification
- certversion [index] : version du certificat de l’autorité de certification
- certstatuscode [index] : état de vérification du certificat d’autorité de certification
- crlstate [index] : liste de révocation de certificats
- krastate [index] : certificat KRA
- crossstate + [index] : transférer le certificat croisé
- crossstate-[index] : certificat croisé inverse
- CERT [index] : CA Cert
- certchain [index] : chaîne du certificat d’autorité de certification
- certcrlchain [index] : chaîne du certificat CA avec les listes de révocation de certificats
- Xchg [index] : certificat d’échange d’autorité de certification
- xchgchain [index] : chaîne de certificat d’échange d’autorité de certification
- xchgcrlchain [index] : chaîne de certificat d’échange d’autorité de certification avec CRL
- kra [index] : certificat KRA
- Cross + [index] : transférer le certificat croisé
- Cross-[index] : Cross Cross CERT
- CRL [index] : liste de révocation de certificats de base
- DeltaCRL [index] : liste de révocation de certificats delta
- crlstatus [index] : état de publication de la liste de révocation de certificats
- deltacrlstatus [index] : état de publication de la liste de révocation de certificats delta
- DNS : nom DNS
- rôle : séparation des rôles
- publicités : serveur avancé
- modèles : modèles
- CSP [index] : URL OCSP
- AIA [index] : URL AIA
- CDP [index] : URL CDP
- localename : nom des paramètres régionaux de l’autorité de certification
- subjecttemplateoids : OID de modèle subject

Revenir au [menu](#menu)

## <a name="-cacert"></a>-ca. cert

CertUtil [options]-ca. cert OutCACertFile [index]

Récupérer le certificat de l’autorité de certification

OutCACertFile : fichier de sortie

Index : index de renouvellement du certificat d’autorité de certification (par défaut, le plus récent)

[-f] [-Split] [-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-cachain"></a>-ca. Chain

CertUtil [options]-ca. Chain OutCACertChainFile [index]

Récupérer la chaîne de certificats de l’autorité de certification

OutCACertChainFile : fichier de sortie

Index : index de renouvellement du certificat d’autorité de certification (par défaut, le plus récent)

[-f] [-Split] [-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-getcrl"></a>-GetCRL

CertUtil [options]-GetCRL out [index] [Delta]

Récupérer la liste de révocation

Index : index de la liste de révocation de certificats ou index de clé (liste de révocation par défaut pour la clé la plus récente)

Delta : delta CRL (la valeur par défaut est la liste de révocation de certificats de base)

[-f] [-Split] [-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-crl"></a>-CRL

CertUtil [options]-CRL [JJ : HH | Republish] [Delta]

Publier de nouvelles listes de révocation de certificats [ou delta CRL uniquement]

JJ : HH--nouvelle période de validité de la liste de révocation de certificats en jours et heures

republier--republier les listes de révocation de certificats les plus récentes

Delta--listes CRL Delta uniquement (les listes de révocation de certificats de base et Delta sont par défaut)

[-Split] [-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-shutdown"></a>-Shutdown

CertUtil [options]-Shutdown

Arrêter Active Directory les services de certificats

[-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-installcert"></a>-installCert

CertUtil [options]-installCert [CACertFile]

Installer le certificat d’autorité de certification

[-f] [-Silent] [-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-renewcert"></a>-renewCert

CertUtil [options]-renewCert [ReuseKeys] [Machine\ParentCAName]

Renouveler le certificat de l’autorité de certification

Utilisez-f pour ignorer une demande de renouvellement en suspens et générer une nouvelle requête.

[-f] [-Silent] [-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-schema"></a>-schéma

CertUtil [options]-Schema [ext | Attrib | RÉVOCATION

Vider le schéma de certificat

La valeur par défaut est la table de demandes et de certificats

Ext : table d’extension

Attrib : attribute table

Liste de révocation de certificats : table CRL

[-Split] [-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-view"></a>-afficher

CertUtil [options]-afficher [file d’attente | Journal | LogFail | Révoqué | Ext | Attrib | Liste de révocation des certificats] [csv]

Vider l’affichage du certificat

Queue : file d’attente des demandes

JRN : certificats émis ou révoqués, et demandes ayant échoué

LogFail : échecs de demandes

Révoqué : certificats révoqués

Ext : table d’extension

Attrib : attribute table

Liste de révocation de certificats : table CRL

CSV : sortie sous forme de valeurs séparées par des virgules

Pour afficher la colonne StatusCode pour toutes les entrées :-out StatusCode

Pour afficher toutes les colonnes de la dernière entrée :-Restrict "RequestId = = $"

Pour afficher RequestId et disposition pour trois requêtes :-Restrict "RequestId > = 37, RequestId\<40"-out "RequestId, disposition"

Pour afficher les ID de ligne et les numéros de liste de révocation de certificats pour toutes les listes de révocation de certificats de base :-restreindre « CRLMinBase = 0 »-out « CRLRowId, CRLNumber »

Pour afficher la liste de révocation de certificats de base numéro 3 :-v-restreindre « CRLMinBase = 0, CRLNumber = 3 »-out « CRLRawCRL » CRL

Pour afficher l’intégralité de la table de liste de révocation de certificats : CRL

Utilisez « date [+ |-JJ : HH] » pour les restrictions de date

Utilisez « Now + DD : HH » pour une date relative à l’heure actuelle

[-Silent] [-Split] [-config Machine\CAName] [-Restrict RestrictionList] [-out ColumnList]

Revenir au [menu](#menu)

## <a name="-db"></a>-DB

CertUtil [options]-DB

Vider la base de données brute

[-config Machine\CAName] [-Restrict RestrictionList] [-out ColumnList]

Revenir au [menu](#menu)

## <a name="-deleterow"></a>-deleteRow

CertUtil [options]-deleteRow RowId | Date [demande | Certificat | Ext | Attrib | RÉVOCATION

Supprimer la ligne de base de données du serveur

Demande : échecs et demandes en attente (date d’envoi)

Certificat : certificats expirés et révoqués (date d’expiration)

Ext : table d’extension

Attrib : attribute table

Liste de révocation de certificats : table CRL (date d’expiration)

Pour supprimer les demandes en échec et en attente soumises par le 22 janvier 2001:1/22/2001 demande

Pour supprimer tous les certificats expirés avant le 22 janvier 2001:1/22/2001 CERT

Pour supprimer la ligne, les attributs et les extensions du certificat pour RequestId 37:37

Pour supprimer les listes de révocation de certificats expirées avant le 22 janvier 2001:1/22/2001 CRL

[-f] [-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-backup"></a>-sauvegarde

CertUtil [options]-Backup répertoire_sauvegarde [Incremental] [KeepLog]

Sauvegarde Active Directory les services de certificats

BackupDirectory : répertoire dans lequel stocker les données sauvegardées

Incrémentielle : effectuer une sauvegarde incrémentielle uniquement (sauvegarde complète par défaut)

KeepLog : conserver les fichiers journaux de base de données (par défaut, tronquer les fichiers journaux)

[-f] [-config Machine\CAName] [-p mot de passe]

Revenir au [menu](#menu)

## <a name="-backupdb"></a>-backupDB

CertUtil [options]-backupDB répertoire_sauvegarde [Incremental] [KeepLog]

Sauvegarder Active Directory base de données des services de certificats

BackupDirectory : répertoire dans lequel stocker les fichiers de base de données sauvegardés

Incrémentielle : effectuer une sauvegarde incrémentielle uniquement (sauvegarde complète par défaut)

KeepLog : conserver les fichiers journaux de base de données (par défaut, tronquer les fichiers journaux)

[-f] [-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-backupkey"></a>-backupKey

CertUtil [options]-backupKey répertoire_sauvegarde

Sauvegarde Active Directory certificat et la clé privée des services de certificats

BackupDirectory : répertoire dans lequel stocker le fichier PFX sauvegardé

[-f] [-config Machine\CAName] [-p mot de passe] [-t délai d’expiration]

Revenir au [menu](#menu)

## <a name="-restore"></a>-restaurer

CertUtil [options]-Restore répertoire_sauvegarde

Restaurer les services de certificats Active Directory

BackupDirectory : répertoire contenant les données à restaurer

[-f] [-config Machine\CAName] [-p mot de passe]

Revenir au [menu](#menu)

## <a name="-restoredb"></a>-restoreDB

CertUtil [options]-restoreDB répertoire_sauvegarde

Restaurer Active Directory base de données des services de certificats

BackupDirectory : répertoire contenant les fichiers de base de données à restaurer

[-f] [-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-restorekey"></a>-restoreKey

CertUtil [options]-restoreKey répertoire_sauvegarde | PFXFile

Restaurer Active Directory certificat et la clé privée des services de certificats

BackupDirectory : répertoire contenant le fichier PFX à restaurer

PFXFile : fichier PFX à restaurer

[-f] [-config Machine\CAName] [-p mot de passe]

Revenir au [menu](#menu)

## <a name="-importpfx"></a>-importPFX

CertUtil [options]-importPFX [CertificateStoreName] PFXFile [modificateurs]

Importer un certificat et une clé privée

CertificateStoreName : nom du magasin de certificats.  Voir [-Store](#-store).

PFXFile : fichier PFX à importer

Modificateurs : liste séparée par des virgules d’un ou plusieurs des éléments suivants :

1. AT_SIGNATURE : remplacez KeySpec par signature
2. AT_KEYEXCHANGE : remplacez KeySpec par Key Exchange
3. Noexport : rendre la clé privée non exportable
4. NoCert : ne pas importer le certificat
5. Nochaîne : ne pas importer la chaîne de certificats
6. Noracine : ne pas importer le certificat racine
7. Protéger : protéger les clés avec un mot de passe
8. Noprotect : ne pas protéger les clés de mot de passe

La valeur par défaut est Personal machine Store.

[-f] [-utilisateur] [-p mot de passe] [-fournisseur CSP]

Revenir au [menu](#menu)

## <a name="-dynamicfilelist"></a>-dynamicfilelist

CertUtil [options]-dynamicfilelist

Afficher la liste des fichiers dynamiques

[-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-databaselocations"></a>-databaselocations

CertUtil [options]-databaselocations

Afficher les emplacements des bases de données

[-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-hashfile"></a>-hashfile

CertUtil [options]-hashfile INFILE [HashAlgorithm]

Générer et afficher le hachage de chiffrement sur un fichier

Revenir au [menu](#menu)

## <a name="-store"></a>-Store

CertUtil [options]-Store [CertificateStoreName [CertId [FichierSortie]]]

Vider le magasin de certificats

CertificateStoreName : nom du magasin de certificats. Exemples :

- « My », « CA » (par défaut), « root »,
- « ldap:///CN=Certification autorités, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? caCertificate ? One ? objectClass = certificationAuthority » (afficher les certificats racine)
- « ldap:///CN=CAName,CN=Certification autorités, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? caCertificate ? base ? objectClass = certificationAuthority » (modifier les certificats racines)
- « ldap:///CN=CAName,CN=MachineName,CN=CDP,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? certificateRevocationList ? base ? objectClass = cRLDistributionPoint » (afficher les listes de révocation de certificats)
- « ldap:///CN=NTAuthCertificates,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? caCertificate ? base ? objectClass = certificationAuthority » (certificats d’autorité de certification d’entreprise)
- LDAP : (certificats de l’objet ordinateur AD)
- -User LDAP : (AD User Object Certificates)

CertId : jeton de correspondance du certificat ou de la liste de révocation des certificats.  Il peut s’agir d’un numéro de série, d’un certificat SHA-1, d’une liste de révocation de certificats ou d’un hachage de clé publique, d’un index de certificat numérique (0, 1, etc.), d’un index de liste de révocation de certificats numérique (. 0, 1, etc.), d’un index CTL numérique (.. 0,.. 1, et ainsi de suite), une clé publique, une signature ou une extension ObjectId, un nom commun d’objet de certificat, une adresse de messagerie, un UPN ou un nom DNS, un nom de conteneur de clé ou un nom CSP, un nom de modèle ou un ID d’objet, un ID d’application améliorée ou des stratégies d’application ou un nom commun d’émetteur de liste La plupart d’entre eux peuvent entraîner des correspondances multiples.

FichierSortie : fichier pour enregistrer le certificat correspondant

Utilisez-user pour accéder à un magasin d’utilisateurs au lieu d’un magasin d’ordinateurs.

Utilisez-Enterprise pour accéder à une machine Enterprise Store.

Utilisez-service pour accéder à un magasin de services de l’ordinateur.

Utilisez-GroupPolicy pour accéder à un magasin de stratégies de groupe d’ordinateurs.

Exemples :

- -entreprise NTAuth
- -Racine d’entreprise 37
- -User My 26e0aaaf000000000004
- CA. 11

[-f] [-Enterprise] [-utilisateur] [-GroupPolicy] [-Silent] [-Split] [-DC DCName]

Revenir au [menu](#menu)

## <a name="-addstore"></a>-AddStore

CertUtil [options]-AddStore CertificateStoreName INFILE

Ajouter un certificat au magasin

CertificateStoreName : nom du magasin de certificats.  Voir [-Store](#-store).

INFILE : fichier de certificat ou de liste de révocation de certificats à ajouter au magasin.

[-f] [-Enterprise] [-utilisateur] [-GroupPolicy] [-DC DCName]

Revenir au [menu](#menu)

## <a name="-delstore"></a>-delstore

CertUtil [options]-delstore CertificateStoreName CertId

Supprimer le certificat du magasin

CertificateStoreName : nom du magasin de certificats.  Voir [-Store](#-store).

CertId : jeton de correspondance du certificat ou de la liste de révocation des certificats.  Voir [-Store](#-store).

[-Enterprise] [-utilisateur] [-GroupPolicy] [-DC DCName]

Revenir au [menu](#menu)

## <a name="-verifystore"></a>-verifystore

CertUtil [options]-verifystore CertificateStoreName [CertId]

Vérifier le certificat dans le magasin

CertificateStoreName : nom du magasin de certificats.  Voir [-Store](#-store).

CertId : jeton de correspondance du certificat ou de la liste de révocation des certificats.  Voir [-Store](#-store).

[-Enterprise] [-utilisateur] [-GroupPolicy] [-Silent] [-Split] [-DC DCName] [-t délai d’expiration]

Revenir au [menu](#menu)

## <a name="-repairstore"></a>-repairstore

CertUtil [options]-repairstore CertificateStoreName CertIdList [PropertyInfFile | SDDLSecurityDescriptor]

Réparer l’Association de clé ou mettre à jour les propriétés de certificat ou le descripteur de sécurité de clé

CertificateStoreName : nom du magasin de certificats.  Voir [-Store](#-store).

CertIdList : liste séparée par des virgules de jetons de correspondance de certificat ou de liste de révocation de certificats. Voir [-Description de la boutique](#-store) CertId.

PropertyInfFile--fichier INF contenant les propriétés externes :

```
[Properties]
     19 = Empty ; Add archived property, OR:
     19 =       ; Remove archived property

     11 = "{text}Friendly Name" ; Add friendly name property

     127 = "{hex}" ; Add custom hexadecimal property
         _continue_ = "00 01 02 03 04 05 06 07 08 09 0a 0b 0c 0d 0e 0f"
         _continue_ = "10 11 12 13 14 15 16 17 18 19 1a 1b 1c 1d 1e 1f"

     2 = "{text}" ; Add Key Provider Information property
       _continue_ = "Container=Container Name&"
       _continue_ = "Provider=Microsoft Strong Cryptographic Provider&"
       _continue_ = "ProviderType=1&"
       _continue_ = "Flags=0&"
       _continue_ = "KeySpec=2"

     9 = "{text}" ; Add Enhanced Key Usage property
       _continue_ = "1.3.6.1.5.5.7.3.2,"
       _continue_ = "1.3.6.1.5.5.7.3.1,"
```

[-f] [-Enterprise] [-utilisateur] [-GroupPolicy] [-Silent] [-Split] [-fournisseur CSP]

Revenir au [menu](#menu)

## <a name="-viewstore"></a>-viewstore

CertUtil [options]-viewstore [CertificateStoreName [CertId [FichierSortie]]]

Vider le magasin de certificats

CertificateStoreName : nom du magasin de certificats. Exemples :

- « My », « CA » (par défaut), « root »,
- « ldap:///CN=Certification autorités, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? caCertificate ? One ? objectClass = certificationAuthority » (afficher les certificats racine)
- « ldap:///CN=CAName,CN=Certification autorités, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? caCertificate ? base ? objectClass = certificationAuthority » (modifier les certificats racines)
- « ldap:///CN=CAName,CN=MachineName,CN=CDP,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? certificateRevocationList ? base ? objectClass = cRLDistributionPoint » (afficher les listes de révocation de certificats)
- « ldap:///CN=NTAuthCertificates,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? caCertificate ? base ? objectClass = certificationAuthority » (certificats d’autorité de certification d’entreprise)
- LDAP : (certificats de l’objet ordinateur Active Directory)
- -User LDAP : (AD User Object Certificates)

CertId : jeton de correspondance du certificat ou de la liste de révocation des certificats. Il peut s’agir d’un numéro de série, d’un certificat SHA-1, d’une liste de révocation de certificats ou d’un hachage de clé publique, d’un index de certificat numérique (0, 1, etc.), d’un index de liste de révocation de certificats numérique (. 0, 1, etc.), d’un index CTL numérique (.. 0,.. 1, et ainsi de suite), une clé publique, une signature ou une extension ObjectId, un nom commun d’objet de certificat, une adresse de messagerie, un UPN ou un nom DNS, un nom de conteneur de clé ou un nom CSP, un nom de modèle ou un ID d’objet, un ID d’application améliorée ou des stratégies d’application ou un nom commun d’émetteur de liste La plupart d’entre eux peuvent entraîner des correspondances multiples.

FichierSortie : fichier pour enregistrer le certificat correspondant

Utilisez-user pour accéder à un magasin d’utilisateurs au lieu d’un magasin d’ordinateurs.

Utilisez-Enterprise pour accéder à une machine Enterprise Store.

Utilisez-service pour accéder à un magasin de services de l’ordinateur.

Utilisez-GroupPolicy pour accéder à un magasin de stratégies de groupe d’ordinateurs.

Exemples :

1. -entreprise NTAuth
2. -Racine d’entreprise 37
3. -User My 26e0aaaf000000000004
4. CA. 11

[-f] [-Enterprise] [-utilisateur] [-GroupPolicy] [-DC DCName]

Revenir au [menu](#menu)

## <a name="-viewdelstore"></a>-viewdelstore

CertUtil [options]-viewdelstore [CertificateStoreName [CertId [FichierSortie]]]

Supprimer le certificat du magasin

CertificateStoreName : nom du magasin de certificats. Exemples :

- « My », « CA » (par défaut), « root »,
- « ldap:///CN=Certification autorités, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? caCertificate ? One ? objectClass = certificationAuthority » (afficher les certificats racine)
- « ldap:///CN=CAName,CN=Certification autorités, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? caCertificate ? base ? objectClass = certificationAuthority » (modifier les certificats racines)
- « ldap:///CN=CAName,CN=MachineName,CN=CDP,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? certificateRevocationList ? base ? objectClass = cRLDistributionPoint » (afficher les listes de révocation de certificats)
- « ldap:///CN=NTAuthCertificates,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? caCertificate ? base ? objectClass = certificationAuthority » (certificats d’autorité de certification d’entreprise)
- LDAP : (certificats de l’objet ordinateur Active Directory)
- -User LDAP : (AD User Object Certificates)

CertId : jeton de correspondance du certificat ou de la liste de révocation des certificats. Il peut s’agir d’un numéro de série, d’un certificat SHA-1, d’une liste de révocation de certificats ou d’un hachage de clé publique, d’un index de certificat numérique (0, 1, etc.), d’un index de liste de révocation de certificats numérique (. 0, 1, etc.), d’un index CTL numérique (.. 0,.. 1, et ainsi de suite), une clé publique, une signature ou une extension ObjectId, un nom commun d’objet de certificat, une adresse de messagerie, un UPN ou un nom DNS, un nom de conteneur de clé ou un nom CSP, un nom de modèle ou un ID d’objet, un ID d’application améliorée ou des stratégies d’application ou un nom commun d’émetteur de liste La plupart d’entre eux peuvent entraîner des correspondances multiples.

FichierSortie : fichier pour enregistrer le certificat correspondant

Utilisez-user pour accéder à un magasin d’utilisateurs au lieu d’un magasin d’ordinateurs.

Utilisez-Enterprise pour accéder à une machine Enterprise Store.

Utilisez-service pour accéder à un magasin de services de l’ordinateur.

Utilisez-GroupPolicy pour accéder à un magasin de stratégies de groupe d’ordinateurs.

Exemples :

1. -entreprise NTAuth
2. -Racine d’entreprise 37
3. -User My 26e0aaaf000000000004
4. CA. 11

[-f] [-Enterprise] [-utilisateur] [-GroupPolicy] [-DC DCName]

Revenir au [menu](#menu)

## <a name="-dspublish"></a>-dsPublish

CertUtil [options]-dsPublish CertFile [NTAuthCA | RootCA | SubCA | CrossCA | KRA | Utilisateur | Usinage

CertUtil [options]-dsPublish FichierCRL [DSCDPContainer [DSCDPCN]]

Publier le certificat ou la liste de révocation de certificats sur Active Directory

CertFile : fichier de certificat à publier

NTAuthCA : publier le certificat sur DS Enterprise Store

RootCA : publier le certificat dans le magasin racine approuvé DS

SubCA : publier le certificat d’autorité de certification sur l’objet d’autorité de certification DS

CrossCA : publier le certificat croisé sur l’objet de l’autorité de certification DS

KRA : publier le certificat sur l’objet agent de récupération de clé DS

Utilisateur : publier le certificat sur l’objet DS de l’utilisateur

Machine : publier le certificat sur l’objet DS de l’ordinateur

FichierCRL : fichier de liste de révocation de certificats à publier

DSCDPContainer : nom commun du conteneur DS CDP, généralement le nom de l’ordinateur de l’autorité de certification

DSCDPCN : nom commun de l’objet CDP DS, généralement basé sur le nom abrégé et l’index de clé de l’autorité de certification nettoyée

Utilisez-f pour créer un objet de service d’annuaire.

[-f] [-utilisateur] [-DC DCName]

Revenir au [menu](#menu)

## <a name="-adtemplate"></a>-ADTemplate

CertUtil [options]-ADTemplate [template]

Afficher les modèles AD

[-f] [-utilisateur] [-ut] [-MT] [-DC DCName]

## <a name="-template"></a>-Modèle

CertUtil [options]-modèle [template]

Afficher les modèles de stratégie d’inscription

[-f] [-utilisateur] [-Silent] [-PolicyServer URLOrId] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName nom_utilisateur] [-p mot de passe]

Revenir au [menu](#menu)

## <a name="-templatecas"></a>-TemplateCAs

CertUtil [options]-modèle TemplateCAs

Afficher les autorités de certification pour le modèle

[-f] [-utilisateur] [-DC DCName]

Revenir au [menu](#menu)

## <a name="-catemplates"></a>-Camodèles

CertUtil [options]-catemplates [modèle]

Afficher les modèles pour l’autorité de certification

[-f] [-utilisateur] [-ut] [-MT] [-config Machine\CAName] [-DC DCName]

Revenir au [menu](#menu)

## <a name="-setcasites"></a>-SetCASites

CertUtil [options]-SetCASites [Set] [SiteName]

CertUtil [options]-SetCASites Verify [SiteName]

CertUtil [options]-SetCASites supprimer

Définir, vérifier ou supprimer des noms de site d’autorité de certification

- Utilisez l’option-config pour cibler une seule autorité de certification (par défaut, toutes les autorités de certification)
- *SiteName* est autorisé uniquement lors du ciblage d’une seule autorité de certification
- Utilisez-f pour remplacer les erreurs de validation pour le *SiteName* spécifié
- Utilisez-f pour supprimer tous les noms de site de l’autorité de certification

[-f] [-config Machine\CAName] [-DC DCName]

> [!NOTE]
> Pour plus d’informations sur la configuration des autorités de certification pour la reconnaissance des sites Active Directory Domain Services (AD DS), consultez [AD DS la reconnaissance des sites pour les clients AD CS et PKI](https://social.technet.microsoft.com/wiki/contents/articles/14106.ad-ds-site-awareness-for-ad-cs-and-pki-clients.aspx).

Revenir au [menu](#menu)

## <a name="-enrollmentserverurl"></a>-enrollmentServerURL

CertUtil [options]-enrollmentServerURL [URL AuthenticationType [Priority] [modificateurs]]

CertUtil [options]-enrollmentServerURL URL supprimer

Afficher, ajouter ou supprimer des URL de serveur d’inscription associées à une autorité de certification

AuthenticationType : spécifiez l’une des méthodes d’authentification client suivantes lors de l’ajout d’une URL

1. Kerberos : utiliser les informations d’identification Kerberos SSL
2. Nom d’utilisateur : utiliser le compte nommé pour les informations d’identification SSL
3. ClientCertificate : utiliser les informations d’identification SSL du certificat X. 509
4. Anonyme : utiliser des informations d’identification SSL anonymes

supprimer : supprime l’URL spécifiée associée à l’autorité de certification.

Priorité : la valeur par défaut est « 1 » si elle n’est pas spécifiée lors de l’ajout d’une URL

Modificateurs : liste séparée par des virgules d’un ou plusieurs des éléments suivants :

1. AllowRenewalsOnly : seules les demandes de renouvellement peuvent être envoyées à cette autorité de certification par le biais de cette URL
2. AllowKeyBasedRenewal : autorise l’utilisation d’un certificat qui n’a pas de compte associé dans la publicité. Cela s’applique uniquement avec le mode ClientCertificate et le mode AllowRenewalsOnly

[-config Machine\CAName] [-DC DCName]

Revenir au [menu](#menu)

## <a name="-adca"></a>-ADCA

CertUtil [options]-ADCA [CAName]

Afficher les autorités de certification AD

[-f] [-Split] [-DC DCName]

Revenir au [menu](#menu)

## <a name="-ca"></a>-CA

CertUtil [options]-CA [CAName | TemplateName

Afficher les autorités de certification de stratégie d’inscription

[-f] [-utilisateur] [-Silent] [-Split] [-PolicyServer URLOrId] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName nom_utilisateur] [-p mot de passe]

Revenir au [menu](#menu)

## <a name="-policy"></a>-Stratégie

Afficher la stratégie d’inscription

[-f] [-utilisateur] [-Silent] [-Split] [-PolicyServer URLOrId] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName nom_utilisateur] [-p mot de passe]

Revenir au [menu](#menu)

## <a name="-policycache"></a>-PolicyCache

CertUtil [options]-PolicyCache [Supprimer]

Afficher ou supprimer les entrées du cache de stratégie d’inscription

supprimer : supprimer les entrées du cache du serveur de stratégie

-f : utiliser-f pour supprimer toutes les entrées de cache

[-f] [-utilisateur] [-PolicyServer URLOrId]

Revenir au [menu](#menu)

## <a name="-credstore"></a>-CredStore

CertUtil [options]-CredStore [URL]

CertUtil [options]-CredStore URL Add

CertUtil [options]-CredStore URL supprimer

Afficher, ajouter ou supprimer des entrées de la Banque d’informations d’identification

URL : URL cible.  Utilisez \* pour faire correspondre toutes les entrées. Utilisez https://machine\* pour correspondre à un préfixe d’URL.

Ajouter : ajouter une entrée de magasin d’informations d’identification. Les informations d’identification SSL doivent également être spécifiées.

supprimer : supprimer les entrées de la Banque d’informations d’identification

-f : utilisez-f pour remplacer une entrée ou pour supprimer plusieurs entrées.

[-f] [-utilisateur] [-Silent] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName nom_utilisateur] [-p mot de passe]

Revenir au [menu](#menu)

## <a name="-installdefaulttemplates"></a>-InstallDefaultTemplates

CertUtil [options]-InstallDefaultTemplates

Installer les modèles de certificats par défaut

[-DC DCName]

Revenir au [menu](#menu)

## <a name="-urlcache"></a>-URLCache

CertUtil [options]-URLCache [URL | LISTE DE RÉVOCATION DES CERTIFICATS | \* [Supprimer]]

Afficher ou supprimer les entrées du cache d’URL

URL : URL mise en cache

CRL : agir sur toutes les URL de liste de révocation de certificats mises en cache uniquement

\*: faire fonctionner sur toutes les URL mises en cache

supprimer : supprimer les URL pertinentes du cache local de l’utilisateur actuel

Utilisez-f pour forcer l’extraction d’une URL spécifique et la mise à jour du cache.

[-f] [-Split]

Revenir au [menu](#menu)

## <a name="-pulse"></a>-Pulse

CertUtil [options]-Pulse

Événements d’inscription automatique par impulsions

[-utilisateur]

Revenir au [menu](#menu)

## <a name="-machineinfo"></a>-MachineInfo

CertUtil [options]-MachineInfo DomainName\MachineName $

Afficher les informations de l’objet Active Directory ordinateur

Revenir au [menu](#menu)

## <a name="-dcinfo"></a>-DCInfo

CertUtil [options]-DCInfo [domaine] [verify | DeleteBad | DeleteAll

Afficher les informations du contrôleur de domaine

La valeur par défaut est d’afficher les certificats DC sans vérification

[-f] [-utilisateur] [-urlfetch] [-DC DCName] [-t délai d’expiration]

> [!TIP]
> La possibilité de spécifier un domaine Active Directory Domain Services (AD DS) **[domaine]** et de spécifier un contrôleur de domaine ( **-DC**) a été ajoutée dans Windows Server 2012. Pour exécuter la commande, vous devez utiliser un compte membre du **groupe Admins du domaine** ou administrateurs de l' **entreprise**. Les modifications de comportement de cette commande sont les suivantes :</br>> 1.  Si aucun domaine n’est spécifié et qu’aucun contrôleur de domaine spécifique n’est spécifié, cette option renvoie une liste des contrôleurs de domaine à traiter à partir du contrôleur de domaine par défaut.</br>> 2.  Si aucun domaine n’est spécifié, mais qu’un contrôleur de domaine est spécifié, un rapport des certificats sur le contrôleur de domaine spécifié est généré.</br>> 3.  Si un domaine est spécifié, mais qu’aucun contrôleur de domaine n’est spécifié, une liste de contrôleurs de domaine est générée, ainsi que des rapports sur les certificats pour chaque contrôleur de domaine de la liste.</br>> 4.  Si le domaine et le contrôleur de domaine sont spécifiés, une liste de contrôleurs de domaine est générée à partir du contrôleur de domaine ciblé. Un rapport des certificats de chaque contrôleur de domaine dans la liste est également généré.

Par exemple, supposons qu’il existe un domaine nommé CPANDL avec un contrôleur de domaine nommé CPANDL-DC1. Vous pouvez exécuter la commande suivante pour récupérer une liste de contrôleurs de domaine et leurs certificats provenant de CPANDL-DC1 : certutil-dc cpandl-DC1-dcinfo cpandl

Revenir au [menu](#menu)

## <a name="-entinfo"></a>-EntInfo

CertUtil [options]-EntInfo DomainName\MachineName $

[-f] [-utilisateur]

Revenir au [menu](#menu)

## <a name="-tcainfo"></a>-TCAInfo

CertUtil [options]-TCAInfo [DomainDN |-]

Afficher les informations de l’autorité de certification

[-f] [-Enterprise] [-utilisateur] [-urlfetch] [-DC DCName] [-t délai d’expiration]

Revenir au [menu](#menu)

## <a name="-scinfo"></a>-SCInfo

CertUtil [options]-SCInfo [ReaderName [CRYPT_DELETEKEYSET]]

Afficher les informations de la carte à puce

CRYPT_DELETEKEYSET : supprimer toutes les clés de la carte à puce

[-Silent] [-Split] [-urlfetch] [-t délai d’expiration]

Revenir au [menu](#menu)

## <a name="-scroots"></a>-SCRoots

CertUtil [options]-mise à jour SCRoots [+] [InputRootFile] [ReaderName]

CertUtil [options]-SCRoots Save \@OutputRootFile [ReaderName]

CertUtil [options]-vue SCRoots [InputRootFile | ReaderName]

CertUtil [options]-SCRoots delete [ReaderName]

Gérer les certificats racines de carte à puce

[-f] [-Split] [-p mot de passe]

Revenir au [menu](#menu)

## <a name="-verifykeys"></a>-verifykeys

CertUtil [options]-verifykeys [KeyContainerName CACertFile]

Vérifier le jeu de clés publiques/privées

KeyContainerName : nom du conteneur de clé de la clé à vérifier. La valeur par défaut est clés d’ordinateur.  Utilisez-user pour les clés utilisateur.

CACertFile : fichier de signature ou de certificat de chiffrement

Si aucun argument n’est spécifié, chaque certificat d’autorité de certification de signature est vérifié par rapport à sa clé privée.

Cette opération ne peut être effectuée que sur une autorité de certification locale ou des clés locales.

[-f] [-utilisateur] [-Silent] [-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-verify"></a>-vérifier

CertUtil [options]-Verify CertFile [ApplicationPolicyList |-[IssuancePolicyList]]

CertUtil [options]-Verify CertFile [CACertFile [CrossedCACertFile]]

CertUtil [options]-Verify FichierCRL CACertFile [IssuedCertFile]

CertUtil [options]-Verify FichierCRL CACertFile [DeltaCRLFile]

Vérifier le certificat, la liste de révocation de certificats ou la chaîne

CertFile : certificat à vérifier

ApplicationPolicyList : liste séparée par des virgules de la stratégie d’application requise ObjectIds

IssuancePolicyList : liste séparée par des virgules de la stratégie d’émission requise ObjectIds

CACertFile : certificat de l’autorité de certification émettrice facultatif à vérifier

CrossedCACertFile : certificat facultatif croisé par CertFile

FichierCRL : liste de révocation de certificats à vérifier

IssuedCertFile : certificat émis facultatif couvert par FichierCRL

DeltaCRLFile : liste de révocation de certificats delta facultative

Si ApplicationPolicyList est spécifié, la génération de la chaîne est limitée aux chaînes valides pour les stratégies d’application spécifiées.

Si IssuancePolicyList est spécifié, la génération de la chaîne est limitée aux chaînes valides pour les stratégies d’émission spécifiées.

Si CACertFile est spécifié, les champs de CACertFile sont vérifiés par rapport à CertFile ou FichierCRL.

Si CACertFile n’est pas spécifié, CertFile est utilisé pour générer et vérifier une chaîne complète.

Si CACertFile et CrossedCACertFile sont tous les deux spécifiés, les champs dans CACertFile et CrossedCACertFile sont vérifiés par rapport à CertFile.

Si IssuedCertFile est spécifié, les champs de IssuedCertFile sont vérifiés par rapport à FichierCRL.

Si DeltaCRLFile est spécifié, les champs de DeltaCRLFile sont vérifiés par rapport à FichierCRL.

[-f] [-Enterprise] [-utilisateur] [-Silent] [-Split] [-urlfetch] [-t délai d’expiration]

Revenir au [menu](#menu)

## <a name="-verifyctl"></a>-verifyCTL

CertUtil [options]-verifyCTL CTLObject [CertDir] [CertFile]

Vérifier la liste CTL des certificats AuthRoot ou non autorisés

CTLObject : identifie la liste de certificats de confiance à vérifier :

- AuthRootWU : Lisez AuthRoot CAB et les certificats correspondants à partir du cache d’URL. Utilisez-f pour télécharger à partir de Windows Update à la place.
- DisallowedWU : Lisez le fichier CAB des certificats non autorisés et le fichier de magasin de certificats non autorisé du cache d’URL.  Utilisez-f pour télécharger à partir de Windows Update à la place.
- AuthRoot : lecture de la liste CTL AuthRoot du registre en cache.  Utilisez avec-f et un CertFile qui n’est pas déjà approuvé pour forcer la mise à jour du Registre AuthRoot mis en cache et des listes CTL de certificats non autorisés.
- Non autorisé : lire la liste CTL des certificats non autorisés mis en cache par le registre. -f a le même comportement qu’avec AuthRoot.
- CTLFileName : file ou http : chemin d’accès à la liste CTL ou CAB

CertDir : dossier contenant les certificats correspondant aux entrées CTL. Un chemin d’accès http : Folder doit se terminer par un séparateur de chemin d’accès. Si un dossier n’est pas spécifié avec AuthRoot ou n’est pas autorisé, plusieurs emplacements sont recherchés pour les certificats correspondants : les magasins de certificats locaux, les ressources fichier crypt32. dll et le cache des URL locales. Utilisez-f pour télécharger à partir de Windows Update si nécessaire. Sinon, la valeur par défaut est le même dossier ou site Web que le CTLObject.

CertFile : fichier contenant le ou les certificats à vérifier. Les certificats sont mis en correspondance avec les entrées CTL et les résultats de correspondance affichés. Supprime la plus grande partie de la sortie par défaut.

[-f] [-utilisateur] [-Split]

Revenir au [menu](#menu)

## <a name="-sign"></a>-Sign

CertUtil [options]-Sign InFileList | SerialNumber | CRL OutFileList [StartDate + DD : HH] [+ SerialNumberList |-SerialNumberList |-ObjectIdList | \@ExtensionFile]

CertUtil [options]-Sign InFileList | SerialNumber | Liste de révocation des OutFileList [#HashAlgorithm] [+ AlternateSignatureAlgorithm |-AlternateSignatureAlgorithm]

Signer à nouveau la liste de révocation de certificats ou le certificat

InFileList : liste séparée par des virgules de fichiers de certificats ou de listes de révocation de certificats à modifier et à signer à nouveau

SerialNumber : numéro de série du certificat à créer. La période de validité et d’autres options ne doivent pas être présentes.

CRL : créez une liste de révocation de certificats vide. La période de validité et d’autres options ne doivent pas être présentes.

OutFileList : liste séparée par des virgules des fichiers de sortie de certificat ou de liste de révocation de certificats modifiés. Le nombre de fichiers doit correspondre à InFileList.

StartDate + JJ : HH : nouvelle période de validité : date facultative plus ; période de validité des jours et heures facultatives ; Si les deux sont spécifiés, utilisez un séparateur de signe plus (+). Utilisez « Now [+ DD : HH] » pour commencer à l’heure actuelle. Utilisez « Never » pour n’avoir aucune date d’expiration (pour les listes de révocation de certificats uniquement).

SerialNumberList : liste de numéros de série séparés par des virgules à ajouter ou à supprimer

ObjectIdList : liste d’ObjectId d’extension séparés par des virgules à supprimer

\@ExtensionFile : fichier INF contenant les extensions à mettre à jour ou à supprimer :

```
[Extensions]
     2.5.29.31 = ; Remove CRL Distribution Points extension
     2.5.29.15 = "{hex}" ; Update Key Usage extension
     _continue_="03 02 01 86"
```

HashAlgorithm : nom de l’algorithme de hachage précédé d’un signe dièse (#)

AlternateSignatureAlgorithm : spécificateur d’algorithme de signature alternatif

Un signe moins entraîne la suppression des numéros de série et des extensions. Un signe plus entraîne l’ajout de numéros de série à une liste de révocation de certificats. Lorsque vous supprimez des éléments d’une liste de révocation de certificats, la liste peut contenir des numéros de série et des ObjectIds. Un signe moins avant AlternateSignatureAlgorithm provoque l’utilisation du format de signature hérité. Un signe plus avant AlternateSignatureAlgorithm entraîne l’utilisation du format de signature alternature. Si AlternateSignatureAlgorithm n’est pas spécifié, le format de signature dans le certificat ou la liste de révocation de certificats est utilisé.

[-nullsign] [-f] [-Silent] [-CERT CertId]

Revenir au [menu](#menu)

## <a name="-vroot"></a>-vroot

CertUtil [options]-vroot [Supprimer]

Créer/supprimer des racines virtuelles Web et des partages de fichiers

Revenir au [menu](#menu)

## <a name="-vocsproot"></a>-vocsproot

CertUtil [options]-vocsproot [Supprimer]

Créer/supprimer des racines virtuelles Web pour le proxy Web OCSP

Revenir au [menu](#menu)

## <a name="-addenrollmentserver"></a>-addEnrollmentServer

CertUtil [options]-addEnrollmentServer Kerberos | Nom d’utilisateur | ClientCertificate [AllowRenewalsOnly] [AllowKeyBasedRenewal]

Ajouter une application de serveur d’inscription

Ajoutez une application serveur d’inscription et un pool d’applications, si nécessaire, pour l’autorité de certification spécifiée. Cette commande n’installe pas les fichiers binaires ou les packages. L’une des méthodes d’authentification suivantes avec lesquelles le client se connecte à un serveur d’inscription de certificats.

- Kerberos : utiliser les informations d’identification Kerberos SSL
- Nom d’utilisateur : utiliser le compte nommé pour les informations d’identification SSL
- ClientCertificate : utiliser les informations d’identification SSL du certificat X. 509
- AllowRenewalsOnly : seules les demandes de renouvellement peuvent être envoyées à cette autorité de certification par le biais de cette URL
- AllowKeyBasedRenewal : autorise l’utilisation d’un certificat qui n’a pas de compte associé dans la publicité. Cela s’applique uniquement à l’aide du mode ClientCertificate et AllowRenewalsOnly.

[-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-deleteenrollmentserver"></a>-deleteEnrollmentServer

CertUtil [options]-deleteEnrollmentServer Kerberos | Nom d’utilisateur | ClientCertificate

Supprimer une application de serveur d’inscription

Supprimer une application du serveur d’inscription et un pool d’applications, si nécessaire, pour l’autorité de certification spécifiée. Cette commande ne supprime pas les fichiers binaires ou les packages. L’une des méthodes d’authentification suivantes avec lesquelles le client se connecte à un serveur d’inscription de certificats.

1. Kerberos : utiliser les informations d’identification Kerberos SSL
2. Nom d’utilisateur : utiliser le compte nommé pour les informations d’identification SSL
3. ClientCertificate : utiliser les informations d’identification SSL du certificat X. 509

[-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-addpolicyserver"></a>-addPolicyServer

CertUtil [options]-addPolicyServer Kerberos | Nom d’utilisateur | ClientCertificate [KeyBasedRenewal]

Ajouter une application de serveur de stratégie

Ajoutez une application serveur de stratégie et un pool d’applications si nécessaire. Cette commande n’installe pas les fichiers binaires ou les packages. L’une des méthodes d’authentification suivantes avec lesquelles le client se connecte à un serveur de stratégie de certificat :

- Kerberos : utiliser les informations d’identification Kerberos SSL
- Nom d’utilisateur : utiliser le compte nommé pour les informations d’identification SSL
- ClientCertificate : utiliser les informations d’identification SSL du certificat X. 509
- KeyBasedRenewal : seules les stratégies qui contiennent des modèles KeyBasedRenewal sont retournées au client. Cet indicateur s’applique uniquement à l’authentification du nom d’utilisateur et du ClientCertificate.

Revenir au [menu](#menu)

## <a name="-deletepolicyserver"></a>-deletePolicyServer

CertUtil [options]-deletePolicyServer Kerberos | Nom d’utilisateur | ClientCertificate [KeyBasedRenewal]

Supprimer une application de serveur de stratégie

Supprimer un pool d’applications et d’applications de serveur de stratégie si nécessaire. Cette commande ne supprime pas les fichiers binaires ou les packages. L’une des méthodes d’authentification suivantes avec lesquelles le client se connecte à un serveur de stratégie de certificat :

1. Kerberos : utiliser les informations d’identification Kerberos SSL
2. Nom d’utilisateur : utiliser le compte nommé pour les informations d’identification SSL
3. ClientCertificate : utiliser les informations d’identification SSL du certificat X. 509
4. KeyBasedRenewal : serveur de stratégie KeyBasedRenewal

Revenir au [menu](#menu)

## <a name="-oid"></a>-OID

CertUtil [options]-OID ObjectId [DisplayName | delete [LanguageId [type]]]

CertUtil [options]-OID GroupId

CertUtil [options]-OID AlgId | AlgorithmName [GroupId]

Afficher l’ObjectId ou définir le nom d’affichage

- ObjectId--ObjectId à afficher ou pour ajouter un nom complet
- GroupId--nombre de GroupId décimal pour l’énumération de ObjectIds
- AlgId--AlgId hexadécimal pour l’ObjectId à rechercher
- AlgorithmName--nom de l’algorithme pour l’ObjectId à rechercher
- DisplayName--nom complet à stocker dans DS
- supprimer--supprimer le nom complet
- LanguageId--ID de langue (valeur par défaut : 1033)
- Type--type d’objet DS à créer : 1 pour le modèle (par défaut), 2 pour la stratégie d’émission, 3 pour la stratégie d’application
- Utilisez-f pour créer un objet de service d’annuaire.

[-f]

Revenir au [menu](#menu)

## <a name="-error"></a>-erreur

CertUtil [options]-erreur ErrorCode

Afficher le texte du message de code d’erreur

Revenir au [menu](#menu)

## <a name="-getreg"></a>-getreg

CertUtil [options]-getreg [{ca | restaurer | stratégie | quitter | modèle | inscrire | chaîne | PolicyServers}\[ProgId\]] [RegistryValueName]

Afficher la valeur de Registre

autorité de certification : utilisez la clé de registre de l’autorité de certification

restauration : utilisez la clé de registre de restauration de l’autorité de certification

stratégie : utiliser la clé de Registre du module de stratégie

quitter : utiliser la clé de Registre du premier module de sortie

modèle : utiliser la clé de Registre Template (use-user pour les modèles utilisateur)

inscrire : utiliser la clé de registre d’inscription (use-user pour user Context)

chaîne : utiliser la clé de registre de configuration de la chaîne

PolicyServers : utiliser la clé de registre des serveurs de stratégie

ProgId : utiliser la stratégie ou le ProgId du module de sortie (nom de la sous-clé du registre)

RegistryValueName : nom de la valeur de Registre (utilisez « Name\*» pour la correspondance de préfixe)

Valeur : nouvelle valeur de Registre numérique, de chaîne ou de date ou de nom de fichier. Si une valeur numérique commence par « + » ou « - », les bits spécifiés dans la nouvelle valeur sont définis ou désactivés dans la valeur de Registre existante.

Si une valeur de chaîne commence par « + » ou « - » et que la valeur existante est une REG_MULTI_SZ valeur, la chaîne est ajoutée à la valeur de Registre existante ou en est supprimée. Pour forcer la création d’une valeur REG_MULTI_SZ, ajoutez un « \n » à la fin de la valeur de chaîne.

Si la valeur commence par «\@», le reste de la valeur est le nom du fichier contenant la représentation textuelle hexadécimale d’une valeur binaire. S’il ne fait pas référence à un fichier valide, il est analysé en tant que [date] [+ |-] [DD : HH]--une date facultative plus ou moins les jours et heures facultatifs. Si les deux sont spécifiés, utilisez un signe plus (+) ou un signe moins (-). Utilisez « Now + DD : HH » pour une date relative à l’heure actuelle.

Utilisez « chain\ChainCacheResyncFiletime \@Now » pour vider efficacement les listes de révocation de certificats mises en cache.

[-f] [-utilisateur] [-GroupPolicy] [-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-setreg"></a>-setreg

CertUtil [options]-setreg [{ca | restaurer | stratégie | quitter | modèle | inscrire | chaîne | PolicyServers}\[ProgId\]] valeur RegistryValueName

Définir la valeur de Registre

autorité de certification : utilisez la clé de registre de l’autorité de certification

restauration : utilisez la clé de registre de restauration de l’autorité de certification

stratégie : utiliser la clé de Registre du module de stratégie

quitter : utiliser la clé de Registre du premier module de sortie

modèle : utiliser la clé de Registre Template (use-user pour les modèles utilisateur)

inscrire : utiliser la clé de registre d’inscription (use-user pour user Context)

chaîne : utiliser la clé de registre de configuration de la chaîne

PolicyServers : utiliser la clé de registre des serveurs de stratégie

ProgId : utiliser la stratégie ou le ProgId du module de sortie (nom de la sous-clé du registre)

RegistryValueName : nom de la valeur de Registre (utilisez « Name\*» pour la correspondance de préfixe)

Valeur : nouvelle valeur de Registre numérique, de chaîne ou de date ou de nom de fichier. Si une valeur numérique commence par « + » ou « - », les bits spécifiés dans la nouvelle valeur sont définis ou désactivés dans la valeur de Registre existante.

Si une valeur de chaîne commence par « + » ou « - » et que la valeur existante est une REG_MULTI_SZ valeur, la chaîne est ajoutée à la valeur de Registre existante ou en est supprimée. Pour forcer la création d’une valeur REG_MULTI_SZ, ajoutez un « \n » à la fin de la valeur de chaîne.

Si la valeur commence par «\@», le reste de la valeur est le nom du fichier contenant la représentation textuelle hexadécimale d’une valeur binaire. S’il ne fait pas référence à un fichier valide, il est analysé en tant que [date] [+ |-] [DD : HH]--une date facultative plus ou moins les jours et heures facultatifs. Si les deux sont spécifiés, utilisez un signe plus (+) ou un signe moins (-). Utilisez « Now + DD : HH » pour une date relative à l’heure actuelle.

Utilisez « chain\ChainCacheResyncFiletime \@Now » pour vider efficacement les listes de révocation de certificats mises en cache.

[-f] [-utilisateur] [-GroupPolicy] [-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-delreg"></a>-DelReg

CertUtil [options]-DelReg [{ca | restaurer | stratégie | quitter | modèle | inscrire | chaîne | PolicyServers}\[ProgId\]] [RegistryValueName]

Supprimer la valeur de Registre

autorité de certification : utilisez la clé de registre de l’autorité de certification

restauration : utilisez la clé de registre de restauration de l’autorité de certification

stratégie : utiliser la clé de Registre du module de stratégie

quitter : utiliser la clé de Registre du premier module de sortie

modèle : utiliser la clé de Registre Template (use-user pour les modèles utilisateur)

inscrire : utiliser la clé de registre d’inscription (use-user pour user Context)

chaîne : utiliser la clé de registre de configuration de la chaîne

PolicyServers : utiliser la clé de registre des serveurs de stratégie

ProgId : utiliser la stratégie ou le ProgId du module de sortie (nom de la sous-clé du registre)

RegistryValueName : nom de la valeur de Registre (utilisez « Name\*» pour la correspondance de préfixe)

Valeur : nouvelle valeur de Registre numérique, de chaîne ou de date ou de nom de fichier. Si une valeur numérique commence par « + » ou « - », les bits spécifiés dans la nouvelle valeur sont définis ou désactivés dans la valeur de Registre existante.

Si une valeur de chaîne commence par « + » ou « - » et que la valeur existante est une REG_MULTI_SZ valeur, la chaîne est ajoutée à la valeur de Registre existante ou en est supprimée. Pour forcer la création d’une valeur REG_MULTI_SZ, ajoutez un « \n » à la fin de la valeur de chaîne.

Si la valeur commence par «\@», le reste de la valeur est le nom du fichier contenant la représentation textuelle hexadécimale d’une valeur binaire. S’il ne fait pas référence à un fichier valide, il est analysé en tant que [date] [+ |-] [DD : HH]--une date facultative plus ou moins les jours et heures facultatifs. Si les deux sont spécifiés, utilisez un signe plus (+) ou un signe moins (-). Utilisez « Now + DD : HH » pour une date relative à l’heure actuelle.

Utilisez « chain\ChainCacheResyncFiletime \@Now » pour vider efficacement les listes de révocation de certificats mises en cache.

[-f] [-utilisateur] [-GroupPolicy] [-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-importkms"></a>-ImportKMS

CertUtil [options]-ImportKMS UserKeyAndCertFile [CertId]

Importer des clés et des certificats d’utilisateur dans la base de données du serveur pour l’archivage de clé

UserKeyAndCertFile--fichier de données contenant les clés privées de l’utilisateur et les certificats à archiver.  Il peut s’agir de l’un des éléments suivants :

- Fichier d’exportation du serveur de gestion de clés (KMS) Exchange
- Fichier PFX

CertId : jeton de correspondance du certificat de déchiffrement du fichier d’exportation KMS.  Voir [-Store](#-store).

Utilisez-f pour importer des certificats non émis par l’autorité de certification.

[-f] [-Silent] [-Split] [-config Machine\CAName] [-p mot de passe] [-symkeyalg SymmetricKeyAlgorithm [, KeyLength]]

Revenir au [menu](#menu)

## <a name="-importcert"></a>-ImportCert

CertUtil [options]-ImportCert CertFile [ExistingRow]

Importer un fichier de certificat dans la base de données

Utilisez ExistingRow pour importer le certificat à la place d’une demande en attente pour la même clé.

Utilisez-f pour importer des certificats non émis par l’autorité de certification.

Il se peut également que l’autorité de certification doive être configurée pour prendre en charge l’importation de certificats étrangers : certutil-setreg ca\KRAFlags + KRAF_ENABLEFOREIGN

[-f] [-config Machine\CAName]

Revenir au [menu](#menu)

## <a name="-getkey"></a>-GetKey

CertUtil [options]-GetKey SearchToken [RecoveryBlobOutFile]

CertUtil [options]-GetKey SearchToken script OutputScriptFile

CertUtil [options]-GetKey SearchToken Retrieve | récupérer OutputFileBaseName

Récupérer l’objet blob de récupération de clé privée archivée, générer un script de récupération ou récupérer des clés archivées

script : générer un script pour récupérer et récupérer les clés (comportement par défaut si plusieurs candidats de récupération correspondants sont trouvés, ou si le fichier de sortie n’est pas spécifié).

récupérer : récupérer un ou plusieurs objets BLOB de récupération de clé (comportement par défaut si un seul candidat de récupération correspondant est trouvé et si le fichier de sortie est spécifié)

récupérer : récupérer et récupérer les clés privées en une seule étape (requiert des certificats et des clés privées de l’agent de récupération de clé)

SearchToken : permet de sélectionner les clés et les certificats à récupérer.

Peut être l’un des éléments suivants :

1. Nom commun du certificat
2. Numéro de série du certificat
3. Hachage SHA-1 du certificat (empreinte numérique)
4. Hachage SHA-1 de KeyId de certificat (identificateur de clé du sujet)
5. Nom du demandeur (domaine\utilisateur)
6. UPN (utilisateur\@domaine)

RecoveryBlobOutFile : fichier de sortie contenant une chaîne de certificats et une clé privée associée, toujours chiffrés sur un ou plusieurs certificats de l’agent de récupération de clé.

OutputScriptFile : fichier de sortie contenant un script de commandes pour récupérer et récupérer les clés privées.

OutputFileBaseName : nom de base du fichier de sortie. Pour la récupération, toute extension est tronquée et une chaîne spécifique au certificat et l’extension. Rec sont ajoutées pour chaque objet blob de récupération de clé.  Chaque fichier contient une chaîne de certificats et une clé privée associée, toujours chiffrés sur un ou plusieurs certificats de l’agent de récupération de clé. Pour la récupération, toute extension est tronquée et l’extension. P12 est ajoutée.  Contient les chaînes de certificats récupérées et les clés privées associées, stockées en tant que fichier PFX.

[-f] [-UnicodeText] [-Silent] [-config Machine\CAName] [-p mot de passe] [-ProtectTo SAMNameAndSIDList] [-fournisseur CSP]

Revenir au [menu](#menu)

## <a name="-recoverkey"></a>-RecoverKey

CertUtil [options]-RecoverKey RecoveryBlobInFile [PFXOutFile [RecipientIndex]]

Récupérer la clé privée archivée

[-f] [-utilisateur] [-Silent] [-Split] [-p mot de passe] [-ProtectTo SAMNameAndSIDList] [-fournisseur CSP] [-t délai d’expiration]

Revenir au [menu](#menu)

## <a name="-mergepfx"></a>-MergePFX

CertUtil [options]-MergePFX PFXInFileList PFXOutFile [ExtendedProperties]

PFXInFileList : liste des fichiers d’entrée PFX séparés par des virgules

PFXOutFile : fichier de sortie PFX

ExtendedProperties : inclure les propriétés étendues

Le mot de passe spécifié sur la ligne de commande est une liste de mots de passe séparés par des virgules.  Si plusieurs mots de passe sont spécifiés, le dernier mot de passe est utilisé pour le fichier de sortie.  Si un seul mot de passe est fourni ou si le dernier mot de passe est «\*», l’utilisateur est invité à entrer le mot de passe du fichier de sortie.

[-f] [-utilisateur] [-Split] [-p mot de passe] [-ProtectTo SAMNameAndSIDList] [-fournisseur CSP]

Revenir au [menu](#menu)

## <a name="-convertepf"></a>-ConvertEPF

CertUtil [options]-ConvertEPF PFXInFileList EPFOutFile [Cast | cast-] [V3CACertId] [, Salt]

Convertir des fichiers PFX en fichier EPF

PFXInFileList : liste des fichiers d’entrée PFX séparés par des virgules

Fichier de sortie EPF : EPF

Cast : utiliser le chiffrement CAST 64

Cast- : utiliser le chiffrement CAST 64 (Export)

V3CACertId : jeton de correspondance du certificat d’autorité de certification v3.  Voir [-Description de la boutique](#-store) CertId.

Salt : chaîne Salt du fichier de sortie EPF

Le mot de passe spécifié sur la ligne de commande est une liste de mots de passe séparés par des virgules. Si plusieurs mots de passe sont spécifiés, le dernier mot de passe est utilisé pour le fichier de sortie.  Si un seul mot de passe est fourni ou si le dernier mot de passe est «\*», l’utilisateur est invité à entrer le mot de passe du fichier de sortie.

[-f] [-Silent] [-Split] [-DC DCName] [-p mot de passe] [-fournisseur CSP]

Revenir au [menu](#menu)

## <a name="options"></a>Options

Cette section définit les options que vous pouvez spécifier à l’aide de la commande.

|Options|Description|
|-------|-----------|
|-nullsign|Utiliser le hachage des données comme signature|
|-f|Forcer le remplacement|
|-entreprise|Utiliser le magasin de certificats du registre de l’ordinateur local de l’entreprise|
|-utilisateur|Utiliser des clés de HKEY_CURRENT_USER ou un magasin de certificats|
|-GroupPolicy|Utiliser stratégie de groupe magasin de certificats|
|-UT|Afficher les modèles utilisateur|
|-MT|Afficher les modèles d’ordinateur|
|-Unicode|Écrire une sortie redirigée en Unicode|
|-UnicodeText|Écrire le fichier de sortie au format Unicode|
|-GMT|Afficher les heures au format GMT|
|-secondes|Afficher les temps avec les secondes et les millisecondes|
|-silencieux|Utiliser un indicateur Silent pour acquérir le contexte de chiffrement|
|-Split|Fractionner les éléments ASN. 1 incorporés et les enregistrer dans des fichiers|
|-v|Opération détaillée|
|-PrivateKey|Afficher les données de mot de passe et de clé privée|
|-Pin pin|Code confidentiel de la carte à puce|
|-urlfetch|Récupération et vérification des certificats AIA et des listes CRL CDP|
|-config Machine\CAName|Autorité de certification et chaîne de nom d’ordinateur|
|-PolicyServer URLOrId|URL ou ID du serveur de stratégie. Pour la sélection U/I, utilisez-PolicyServer. Pour tous les serveurs de stratégie, utilisez-PolicyServer \*|
|-Anonyme|Utiliser des informations d’identification SSL anonymes|
|-Kerberos|Utiliser les informations d’identification Kerberos SSL|
|-ClientCertificate ClientCertId|Utilisez les informations d’identification SSL du certificat X. 509. Pour la sélection U/I, utilisez-clientCertificate.|
|-UserName nom_utilisateur|Utilisez le compte nommé pour les informations d’identification SSL. Pour la sélection U/I, utilisez-UserName.|
|-Certificat CertId|Certificat de signature|
|-DC DCName|Cibler un contrôleur de domaine spécifique|
|-restreindre RestrictionList|Liste de restrictions séparée par des virgules. Chaque restriction comprend un nom de colonne, un opérateur relationnel et un entier constant, une chaîne ou une date. Un nom de colonne peut être précédé d’un signe plus ou moins pour indiquer l’ordre de tri. Exemples :</br>« RequestId = 47 »</br>« + RequesterName > = a, RequesterName < b »</br>« -RequesterName > domaine, disposition = 21 »|
|-out ColumnList|Liste de colonnes séparées par des virgules|
|-p mot de passe|Mot de passe|
|-ProtectTo SAMNameAndSIDList|Liste des noms SAM/SID séparés par des virgules|
|-Fournisseur CSP|Fournisseur|
|-t délai d’expiration|Délai d’expiration de l’extraction d’URL en millisecondes|
|-symkeyalg SymmetricKeyAlgorithm [, KeyLength]|Nom de l’algorithme de clé symétrique avec une longueur de clé facultative, par exemple : AES, 128 ou 3DES|

Revenir au [menu](#menu)

## <a name="additional-certutil-examples"></a>Exemples supplémentaires de Certutil

Pour obtenir des exemples d’utilisation de cette commande, consultez.

1. [Exemples certutil pour la gestion des services de certificats Active Directory (AD CS) à partir de la ligne de commande](https://social.technet.microsoft.com/wiki/contents/articles/3063.certutil-examples-for-managing-active-directory-certificate-services-ad-cs-from-the-command-line.aspx)
2. [Tâches certutil pour la gestion des certificats](https://technet.microsoft.com/library/cc772898.aspx)
3. [Exportation des demandes binaires à l’aide de l’outil en ligne de commande CertUtil. exe](https://social.technet.microsoft.com/wiki/contents/articles/7573.active-directory-certificate-services-pki-key-archival-and-management.aspx)
4. [Renouvellement du certificat d’autorité de certification racine](https://social.technet.microsoft.com/wiki/contents/articles/2016.root-ca-certificate-renewal.aspx)
5. [Certutil](https://msdn.microsoft.com/subscriptions/cc773087.aspx)

Revenir au [menu](#menu)
