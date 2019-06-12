---
title: certutil
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 7a602472d30f19cb2d4a802423635e5788e78a43
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434552"
---
# <a name="certutil"></a>certutil

Certutil.exe est un programme de ligne de commande qui est installé dans le cadre des Services de certificats. Vous pouvez utiliser Certutil.exe pour vider et afficher des informations de configuration autorité de certification, configurer les Services de certificats, sauvegarder et restaurer les composants de l’autorité de certification et vérifier les certificats, les paires de clés et les chaînes de certificat.

Quand certutil est exécutée sur une autorité de certification sans paramètres supplémentaires, il affiche la configuration actuelle de l’autorité de certification. Lorsque cerutil est exécuté sur une autorité de certification non, la commande par défaut est en cours d’exécution le certutil [-vidage](#-dump) verbe.

> [!WARNING]
> Les versions antérieures de certutil ne peuvent pas fournir toutes les options sont décrites dans ce document. Vous pouvez voir toutes les options fournies par une version spécifique de certutil en exécutant les commandes indiquées dans le [notations de syntaxe](#syntax-notations) section.

## <a name="menu"></a>Menu

Les principales sections de ce document sont :

- [Verbs](#verbs)
- [Notations de syntaxe](#syntax-notations)
- [Options](#options)
- [Obtenir des exemples supplémentaires de certutil](#additional-certutil-examples)

## <a name="verbs"></a>Verbes

Le tableau suivant décrit les verbes qui peuvent être utilisés avec la commande certutil.

|Verbes|Description|
|-----|-----------|
|[-dump](#-dump)|Informations de configuration ou les fichiers de vidage|
|[-asn](#-asn)|Analyser le fichier ASN.1|
|[-decodehex](#-decodehex)|Décoder le fichier codé en hexadécimal|
|[-decode](#-decode)|Décoder un fichier codé en Base64|
|[-encode](#-encode)|Encoder un fichier en Base64|
|[-deny](#-deny)|Refuser une demande de certificat en attente|
|[-resubmit](#-resubmit)|Soumettez à nouveau une demande de certificat en attente|
|[-setattributes](#-setattributes)|Définir les attributs d’une demande de certificat en attente|
|[-setextension](#-setextension)|Définir une extension pour une demande de certificat en attente|
|[-revoke](#-revoke)|Révoquer un certificat|
|[-isvalid](#-isvalid)|Afficher la disposition du certificat actuel|
|[-getconfig](#-getconfig)|Obtenir la chaîne de configuration par défaut|
|[-ping](#-ping)|Tentative de contact de l’interface de demande Services certificat Active Directory|
|-pingadmin|Tentative de contact de l’interface d’administration des Services de certificats Active Directory|
|[-CAInfo](#-cainfo)|Afficher des informations sur l’autorité de certification|
|[-ca.cert](#-cacert)|Récupérer le certificat pour l’autorité de certification|
|[-ca.chain](#-cachain)|Récupérer la chaîne de certificats pour l’autorité de certification|
|[-GetCRL](#-getcrl)|Obtenir une liste de révocation de certificats (CRL)|
|[-CRL](#-crl)|Publier les nouvelles listes de révocation de certificats (CRL) [ou uniquement les listes CRL delta]|
|[-shutdown](#-shutdown)|Arrêter les Services de certificats Active Directory|
|[-installCert](#-installcert)|Installer un certificat d’autorité de certification|
|[-renewCert](#-renewcert)|Renouveler un certificat d’autorité de certification|
|[-schema](#-schema)|Vider le schéma pour le certificat|
|[-vue](#-view)|Vider l’affichage du certificat|
|[-db](#-db)|Vidage de la base de données brutes|
|[-deleterow](#-deleterow)|Supprimer une ligne à partir de la base de données du serveur|
|[-backup](#-backup)|Services de certificats de sauvegarde Active Directory|
|[-backupDB](#-backupdb)|Sauvegarde la base de données des Services de certificats Active Directory|
|[-backupKey](#-backupkey)|Sauvegarder la clé privée et le certificat de Services de certificats Active Directory|
|[-restore](#-restore)|Restaurer les Services de certificats Active Directory|
|[-restoreDB](#-restoredb)|Restaurer la base de données des Services de certificats Active Directory|
|[-restoreKey](#-restorekey)|Restaurer la clé privée et le certificat de Services de certificats Active Directory|
|[-importPFX](#-importpfx)|Importer un certificat et la clé privée|
|[-dynamicfilelist](#-dynamicfilelist)|Afficher une liste de fichiers dynamiques|
|[-databaselocations](#-databaselocations)|Afficher les emplacements de la base de données|
|[-hashfile](#-hashfile)|Générer et afficher un hachage cryptographique sur un fichier|
|[-store](#-store)|Vider le magasin de certificats|
|[-addstore](#-addstore)|Ajouter un certificat au magasin|
|[-delstore](#-delstore)|Supprimer un certificat à partir du magasin|
|[-verifystore](#-verifystore)|Vérifier un certificat dans le magasin|
|[-repairstore](#-repairstore)|Réparer une association de clé ou de mettre à jour les propriétés du certificat ou le descripteur de sécurité clés|
|[-viewstore](#-viewstore)|Vider le magasin de certificats|
|[-viewdelstore](#-viewdelstore)|Supprimer un certificat à partir du magasin|
|[-dsPublish](#-dspublish)|Publier un certificat ou une liste de révocation de certificats (CRL) dans Active Directory|
|[-ADTemplate](#-adtemplate)|Modèles d’affichage AD|
|[-Template](#-template)|Afficher les modèles de certificat|
|[-TemplateCAs](#-templatecas)|Afficher les autorités de certification (CA) pour un modèle de certificat|
|[-CATemplates](#-catemplates)|Modèles d’affichage pour l’autorité de certification|
|[-SetCASites](#-setcasites)|Gérer les noms de Site pour les autorités de certification|
|[-enrollmentServerURL](#-enrollmentserverurl)|Afficher, ajouter ou supprimer les URL de serveur d’inscription associés à une autorité de certification|
|[-ADCA](#-adca)|Afficher les autorités de certification AD|
|[-CA](#-ca)|Afficher les autorités de certification de stratégie d’inscription|
|[-Policy](#-policy)|Afficher la stratégie d’inscription|
|[-PolicyCache](#-policycache)|Afficher ou supprimer des entrées de Cache de la stratégie d’inscription|
|[-CredStore](#-credstore)|Afficher, ajouter ou supprimer des entrées d’informations d’identification Store|
|[-InstallDefaultTemplates](#-installdefaulttemplates)|Installer les modèles de certificats par défaut|
|[-URLCache](#-urlcache)|Afficher ou supprimer des entrées de cache d’URL|
|[-pulse](#-pulse)|Événements de l’inscription automatique Pulse|
|[-MachineInfo](#-machineinfo)|Afficher des informations sur l’objet d’ordinateur Active Directory|
|[-DCInfo](#-dcinfo)|Afficher des informations sur le contrôleur de domaine|
|[-EntInfo](#-entinfo)|Afficher des informations sur une autorité de certification d’entreprise|
|[-TCAInfo](#-tcainfo)|Afficher des informations sur l’autorité de certification|
|[-SCInfo](#-scinfo)|Afficher des informations sur la carte à puce|
|[-SCRoots](#-scroots)|Gérer les certificats racine de carte à puce|
|[-verifykeys](#-verifykeys)|Vérifier un jeu de clés publique ou privé|
|[-verify](#-verify)|Vérifier un certificat, une liste de révocation de certificats (CRL) ou une chaîne de certificats|
|[-verifyCTL](#-verifyctl)|Vérifiez AuthRoot ou liste CTL de certificats non autorisés|
|[-sign](#-sign)|Signer à nouveau un certificat ou une liste de révocation de certificats (CRL)|
|[-vroot](#-vroot)|Créer ou supprimer des racines virtuelles web et des partages de fichiers|
|[-vocsproot](#-vocsproot)|Créer ou supprimer des racines virtuelles web pour un proxy web d’OCSP|
|[-addEnrollmentServer](#-addenrollmentserver)|Ajouter une application serveur d’inscription|
|[-deleteEnrollmentServer](#-deleteenrollmentserver)|Supprimer une application serveur d’inscription|
|[-addPolicyServer](#-addpolicyserver)|Ajouter une application de serveur de stratégie|
|[-deletePolicyServer](#-deletepolicyserver)|Supprimer une application de serveur de stratégie|
|[-oid](#-oid)|Afficher l’identificateur d’objet ou de définir un nom d’affichage|
|[-error](#-error)|Afficher le texte du message associé à un code d’erreur|
|[-getreg](#-getreg)|Afficher une valeur de Registre|
|[-setreg](#-setreg)|Définir une valeur de Registre|
|[-delreg](#-delreg)|Supprimer une valeur de Registre|
|[-ImportKMS](#-importkms)|Importer des clés de l’utilisateur et les certificats dans la base de données du serveur pour l’archivage des clés|
|[-ImportCert](#-importcert)|Importer un fichier de certificat dans la base de données|
|[-GetKey](#-getkey)|Récupérer un objet blob de récupération de clés privées archivées|
|[-RecoverKey](#-recoverkey)|Récupérer une clé privée archivée|
|[-MergePFX](#-mergepfx)|Fusionner les fichiers PFX|
|[-ConvertEPF](#-convertepf)|Convertir un fichier PFX en fichier EPF|
|-?|Affiche la liste des verbes|
|- *\<verb>* -?|Affiche l’aide pour le verbe spécifié.|
|-? -v|Affiche une liste complète des verbes et|

Retour à [Menu](#menu)

## <a name="syntax-notations"></a>Notations de syntaxe

- Pour la syntaxe de ligne de commande de base, exécutez `certutil -?`
- Pour connaître la syntaxe sur l’utilisation de certutil avec un verbe spécifique, exécutez **certutil**  *\<verbe >* **- ?**
- Pour envoyer l’ensemble de la syntaxe de certutil dans un fichier texte, exécutez les commandes suivantes :  
  - `certutil -v -? > certutilhelp.txt`
  - `notepad certutilhelp.txt`

Le tableau suivant décrit la notation utilisée pour indiquer la syntaxe de ligne de commande.


|            Notation             |                  Description                  |
|---------------------------------|-----------------------------------------------|
| Texte sans crochets ou accolades |         Vous devez taper comme des éléments          |
|  \<Texte à l’intérieur de crochets pointus >  | Espace réservé pour lequel vous devez fournir une valeur |
|  [Texte à l’intérieur des crochets]  |                Éléments facultatifs                 |
|      {Texte entre accolades}       |       Ensemble d’éléments requis ; Choisissez une       |
|         Barre (verticale          |                       )                       |
|          Points de suspension (…)           |          Éléments qui peuvent être répétés           |

Retour à [Menu](#menu)

## <a name="-dump"></a>-vidage

CertUtil [Options] [-vidage]

CertUtil [Options] [-vidage] fichier

Informations de configuration ou les fichiers de vidage

[-f]. [-silencieux] [-Fractionner] [--p mot de passe] [-t délai d’attente]

Retour à [Menu](#menu)

## <a name="-asn"></a>-asn

CertUtil [Options] - asn fichier [type]

Analyser le fichier ASN.1

type : numérique CRYPT\_chaîne\_ \* décodage de type

Retour à [Menu](#menu)

## <a name="-decodehex"></a>-decodehex

CertUtil [Options] -decodehex InFile OutFile [type]

type : numérique CRYPT\_chaîne\_ \* type d’encodage

[-f]

Retour à [Menu](#menu)

## <a name="-decode"></a>-decode

CertUtil [Options] -decode InFile OutFile

Décoder le fichier codé en Base64

[-f]

Retour à [Menu](#menu)

## <a name="-encode"></a>-encode

CertUtil [Options] - Encoder InFile OutFile

Encoder le fichier en Base64

[-f] [-UnicodeText]

Retour à [Menu](#menu)

## <a name="-deny"></a>-deny

CertUtil [Options] - deny RequestId

Refuser la demande en attente

[-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-resubmit"></a>-renvoyer

CertUtil [Options] - resoumettre RequestId

Resoumettre la demande en attente

[-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-setattributes"></a>-setattributes

CertUtil [Options] - setattributes RequestId Chaîne_attribut

Définir les attributs de demande en attente

RequestId--demande Id numérique de demande en attente

AttributeString--Demander des paires nom / valeur d’attribut

- Noms et valeurs sont séparés par des points.
- Nom de plusieurs paires de valeur sont séparées par un saut de ligne.
- Exemple : «CertificateTemplate:User\nEMail:User@Domain.com»
- Chaque séquence « \n » est convertie en un séparateur de saut de ligne.

[-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-setextension"></a>-setextension

CertUtil [Options] - setextension RequestId ExtensionName indicateurs {Long | Date | Chaîne | @InFile}

Définir l’extension pour demande en attente

RequestId--Id de demande numérique d’une demande en attente

ExtensionName--ObjectId chaîne de l’extension

Indicateurs--0 est recommandé.  1 rend l’extension critique, il désactive les 2, 3 effectue ces deux tâches.

Si le dernier paramètre est numérique, il est considéré comme un Long.

Si elle peut être analysée en tant que date, il est considéré comme une Date.

Si elle commence par ' @', le reste du jeton est le nom de fichier contenant des données binaires ou un vidage hexadécimal en texte ascii.

Tout autre élément est considéré comme une chaîne.

[-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-revoke"></a>-révoquer

CertUtil [Options] - révoquer SerialNumber [Reason]

Révoquer le certificat

SerialNumber (Numéro_série) : Liste des numéros de série de certificat à révoquer séparées par des virgules

Raison : motif de révocation numérique ou symbolique

- 0: CRL_REASON_UNSPECIFIED : Non spécifié (par défaut)
- 1 : CRL_REASON_KEY_COMPROMISE : Clé Compromise
- 2 : CRL_REASON_CA_COMPROMISE : Autorité de certification Compromise
- 3 : CRL_REASON_AFFILIATION_CHANGED : Affiliation
- 4: CRL_REASON_SUPERSEDED : Remplacée
- 5: CRL_REASON_CESSATION_OF_OPERATION: Cessation de l’activité
- 6: CRL_REASON_CERTIFICATE_HOLD : Certificat retenu
- 8: CRL_REASON_REMOVE_FROM_CRL : Supprimer à partir de la révocation de certificats
- -1 : Annuler la révocation : Annuler la révocation

[-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-isvalid"></a>-isvalid

CertUtil [Options] - isvalid SerialNumber | CertHash

Disposition du certificat actuel affichage

[-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-getconfig"></a>-getconfig

CertUtil [Options] -getconfig

Obtenir la chaîne de configuration par défaut

[-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-ping"></a>-ping

CertUtil [Options] - ping [MaxSecondsToWait | CAMachineList]

Ping Active Directory Services interface de requête certificats

CAMachineList--Liste de noms de machine autorité de certification séparées par des virgules

1. Pour une seule machine, utilisez une virgule de fin
2. Affiche le coût de site pour chaque ordinateur d’autorité de certification

[-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-cainfo"></a>-CAInfo

CertUtil [Options] -CAInfo [InfoName [Index | ErrorCode]]

Informations de l’autorité de certification complet

InfoName--Indique la propriété de l’autorité de certification pour afficher (voir ci-dessous). Utilisez «\*» pour toutes les propriétés.

Index--index de propriété de base zéro facultatif

ErrorCode--code d’erreur numérique

[-f]. [-Fractionner] [-config Machine\CAName]

Syntaxe de l’argument InfoName :

- Fichier : Version du fichier
- produit : Version du produit
- exitcount : Nombre de modules de sortie
- sortie [Index] : Description du module de sortie
- stratégie : Description du module de stratégie
- Nom : Nom de l’autorité de certification
- sanitizedname : Nom d’autorité de certification expurgé
- %{dsname/} : Nom court expurgé autorité de certification (nom DS)
- sharedfolder: Dossier partagé
- code d’erreur Error1 : Texte du message d’erreur
- code d’erreur Error2 : Texte du message d’erreur et le code d’erreur
- Type : Type d’autorité de certification
- Info : Informations de l’autorité de certification
- Parent : Autorité de certification parente
- certcount : Nombre de certificats d’autorité de certification
- xchgcount : Nombre de certificats d’autorité de certification exchange
- kracount : Nombre de certificats KRA
- kraused : Nombre de certificats utilisé KRA
- propidmax : Autorité de certification maximale PropId
- certstate [Index] : Certificat d’autorité de certification
- certversion [Index] : Version de certificat d’autorité de certification
- certstatuscode [Index] : État de vérification du certificat d’autorité de certification
- crlstate [Index] : RÉVOCATION DE CERTIFICATS
- krastate [Index] : Certificat KRA
- crossstate + [Index] : Certification croisée ascendante
- crossstate-[Index] : Certification croisée descendante
- CERT [Index] : Certificat d’autorité de certification
- certchain [Index] : Chaîne de certificats d’autorité de certification
- certcrlchain [Index] : Chaîne de certificats d’autorité de certification avec des CRL
- xchg [Index] : Certificat d’échange d’autorité de certification
- xchgchain [Index] : Chaîne de certificats d’autorité de certification exchange
- xchgcrlchain [Index] : Chaîne de certificat exchange autorité de certification avec des CRL
- KRA [Index] : Certificat KRA
- Cross + [Index] : Certification croisée ascendante
- Cross-[Index] : Certification croisée descendante
- Révocation de certificats [Index] : CRL de base
- deltacrl [Index] : Liste CRL delta
- crlstatus [Index] : État de publication CRL
- deltacrlstatus [Index] : État de publication de la liste CRL delta
- DNS : Nom DNS
- Rôle : Séparation des rôles
- annonces : Advanced Server
- modèles : Modèles
- CSP [Index] : URL OCSP
- AIA [Index] : URL AIA
- CDP [Index] : URL de la CDP
- localename : Nom des paramètres régionaux autorité de certification
- subjecttemplateoids : OID de modèle d’objet

Retour à [Menu](#menu)

## <a name="-cacert"></a>-ca.cert

CertUtil [Options] -ca.cert OutCACertFile [Index]

Récupérer le certificat de l’autorité de certification

Fichier Cert CA sortie : fichier de sortie

Index : Index renouvellement du certificat d’autorité de certification (par défaut plus récente)

[-f]. [-Fractionner] [-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-cachain"></a>-ca.chain

CertUtil [Options] -ca.chain OutCACertChainFile [Index]

Récupérer la chaîne de certificats de l’autorité de certification

OutCACertChainFile : fichier de sortie

Index : Index renouvellement du certificat d’autorité de certification (par défaut plus récente)

[-f]. [-Fractionner] [-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-getcrl"></a>-GetCRL

CertUtil [Options] -GetCRL OutFile [Index] [delta]

Obtenir la liste de révocation

Index : Liste de révocation index ou clé d’index (par défaut CRL pour la clé la plus récente)

delta : la liste CRL delta (valeur par défaut est la liste CRL de base)

[-f]. [-Fractionner] [-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-crl"></a>-CRL

[Options] de CertUtil - CRL [hh | republier] [delta]

Publier de nouvelles listes CRL [ou uniquement les listes CRL delta]

hh--Nouvelle période de validité de révocation de certificats dans les jours et heures

republier--republier les listes de révocation plus récente

--delta listes CRL delta uniquement (valeur par défaut est CRL de base et delta)

[-Fractionner] [-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-shutdown"></a>-shutdown

CertUtil [Options] - arrêt

Arrêter les Services de certificats Active Directory

[-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-installcert"></a>-installCert

CertUtil [Options] - installCert [fichier Cert CA]

Installer le certificat d’autorité de Certification

[-f]. [-silencieux] [-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-renewcert"></a>-renewCert

CertUtil [Options] -renewCert [ReuseKeys] [Machine\ParentCAName]

Renouveler le certificat d’autorité de Certification

Utilisez-f pour ignorer une demande de renouvellement en suspens et générer une nouvelle demande.

[-f]. [-silencieux] [-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-schema"></a>-schema

CertUtil [Options] - schéma [Ext | Attrib | RÉVOCATION DE CERTIFICATS]

Schéma de certificat de vidage

Valeurs par défaut pour la table de requête et de certificat

Ext : Table d’extension

Attrib : Table d’attributs

RÉVOCATION DE CERTIFICATS : Table de révocation de certificats

[-Fractionner] [-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-view"></a>-vue

CertUtil [Options] - vue [file d’attente | Journal | LogFail | Révoqué | Ext | Attrib | Révocation de certificats] [csv]

Vider l’affichage du certificat

File d’attente : File d’attente de la demande

Journal : Les certificats émis ou révoqués, ainsi que des demandes ayant échoué

LogFail : Demandes ayant échoué

Révoqué : Certificats révoqués

Ext : Table d’extension

Attrib : Table d’attributs

RÉVOCATION DE CERTIFICATS : Table de révocation de certificats

csv: Sortie comme valeurs séparées par des virgules

Pour afficher la colonne StatusCode pour toutes les entrées :-out StatusCode

Pour afficher toutes les colonnes de la dernière entrée :-limiter « RequestId == $»

Pour afficher le RequestId et la Disposition pour les demandes de trois :-limiter « RequestId > = 37, RequestId\<40 »-out « RequestId, Disposition »

Pour afficher les ID de ligne et les numéros de révocation de certificats pour toutes les listes CRL de Base :-restreindre « CRLMinBase = 0 »-out « CRLRowId CRLNumber » CRL

Pour afficher le numéro 3 CRL de Base : - v-restreindre « CRLMinBase = 0, CRLNumber = 3 »-out « CRLRawCRL « CRL

Pour afficher la table entière de la révocation de certificats : RÉVOCATION DE CERTIFICATS

Utilisez « Date [+ |-hh] » pour les restrictions de date

Utilisez « now + hh » pour une date par rapport à l’heure actuelle

[-silencieux] [-Fractionner] [-config Machine\CAName] [-restreindre RestrictionList] [-out ColumnList]

Retour à [Menu](#menu)

## <a name="-db"></a>-db

CertUtil [Options] -db

Vidage de base de données brutes

[-config Machine\CAName] [-restreindre RestrictionList] [-out ColumnList]

Retour à [Menu](#menu)

## <a name="-deleterow"></a>-deleterow

CertUtil [Options] - deleterow RowId | Date [demande | CERT | Ext | Attrib | RÉVOCATION DE CERTIFICATS]

Supprimer la ligne de base de données de serveur

La demande : Échec et en attente de demandes (date de soumission)

Cert: Certificats expirés et révoqués (date d’expiration)

Ext : Table d’extension

Attrib : Table d’attributs

RÉVOCATION DE CERTIFICATS : Table de révocation de certificats (date d’expiration)

Pour supprimer ayant échoué et en attente de demandes envoyées par le 22 janvier 2001 : 22/1/2001 demande

Pour supprimer tous les certificats qui a expiré avant le 22 janvier 2001 : 22/1/2001 cert

Pour supprimer la ligne de certificat, les attributs et les extensions pour RequestId 37 : 37

Pour supprimer les listes de révocation a expiré avant le 22 janvier 2001 : LISTE DE RÉVOCATION DE 1/22/2001

[-f]. [-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-backup"></a>-sauvegarde

CertUtil [Options] - sauvegarde BackupDirectory [incrémentielle] [KeepLog]

Services de certificats de sauvegarde Active Directory

BackupDirectory : répertoire pour stocker les données sauvegardées

Incrémentielle : effectuer une sauvegarde incrémentielle uniquement (valeur par défaut est la sauvegarde complète)

KeepLog : conserver les fichiers de journaux de base de données (valeur par défaut est de tronquer les fichiers journaux)

[-f]. [-config Machine\CAName] [--p mot de passe]

Retour à [Menu](#menu)

## <a name="-backupdb"></a>-backupDB

BackupDirectory - backupDB CertUtil [Options] [incrémentielle] [KeepLog]

Base de données de sauvegarde Active Directory Certificate Services

BackupDirectory : répertoire pour stocker les fichiers sauvegardés

Incrémentielle : effectuer une sauvegarde incrémentielle uniquement (valeur par défaut est la sauvegarde complète)

KeepLog : conserver les fichiers de journaux de base de données (valeur par défaut est de tronquer les fichiers journaux)

[-f]. [-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-backupkey"></a>-backupKey

CertUtil [Options] - backupKey BackupDirectory

Sauvegarde du certificat Active Directory Certificate Services et la clé privée

BackupDirectory : répertoire pour stocker les fichier PFX sauvegardé

[-f]. [-config Machine\CAName] [--p mot de passe] [-t délai d’attente]

Retour à [Menu](#menu)

## <a name="-restore"></a>-restaurer

CertUtil [Options] - restaurer BackupDirectory

Restaurer les Services de certificats Active Directory

BackupDirectory : répertoire contenant les données à restaurer

[-f]. [-config Machine\CAName] [--p mot de passe]

Retour à [Menu](#menu)

## <a name="-restoredb"></a>-restoreDB

CertUtil [Options] - restoreDB BackupDirectory

Restaurer la base de données des Services de certificats Active Directory

BackupDirectory : répertoire contenant les fichiers de base de données à restaurer

[-f]. [-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-restorekey"></a>-restoreKey

CertUtil [Options] - restoreKey BackupDirectory | PFXFile

Restaurer le certificat de Services de certificats Active Directory et la clé privée

BackupDirectory : répertoire contenant le fichier PFX à restaurer

PFXFile : Fichier PFX à restaurer

[-f]. [-config Machine\CAName] [--p mot de passe]

Retour à [Menu](#menu)

## <a name="-importpfx"></a>-importPFX

[Options] de CertUtil - importPFX [Nom_magasin_certificats] PFXFile [modificateurs]

Importer un certificat et la clé privée

CertificateStoreName: Nom de magasin de certificats.  Consultez [-stocker](#-store).

PFXFile : Fichier PFX à importer

Modificateurs : Liste séparée par des virgules d’un ou plusieurs des opérations suivantes :

1. AT_SIGNATURE : Modifier le KeySpec pour Signature
2. AT_KEYEXCHANGE : Modifier le KeySpec pour l’échange de clés
3. NoExport : Vérifiez la clé privée non exportable
4. NoCert : Ne pas importer le certificat
5. NoChain : N’importez pas la chaîne de certificats
6. NoRoot : Ne pas importer le certificat racine
7. Protéger : Protéger les clés avec mot de passe
8. NoProtect : Ne pas le mot de passe protège les clés

Valeurs par défaut pour le magasin personnel.

[-f]. [-utilisateur] [--p mot de passe] [-csp fournisseur]

Retour à [Menu](#menu)

## <a name="-dynamicfilelist"></a>-dynamicfilelist

CertUtil [Options] -dynamicfilelist

Afficher la liste dynamique de fichiers

[-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-databaselocations"></a>-databaselocations

CertUtil [Options] -databaselocations

Afficher les emplacements de la base de données

[-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-hashfile"></a>-hashfile

CertUtil [Options] - hashfile InFile [HashAlgorithm]

Générer et afficher le hachage cryptographique sur un fichier

Retour à [Menu](#menu)

## <a name="-store"></a>-store

CertUtil [Options] -store [CertificateStoreName [CertId [OutputFile]]]

Vider le magasin de certificats

CertificateStoreName: Nom de magasin de certificats. Exemples :

- « My », « CA » (valeur par défaut), « Root »,
- « ldap : / / / CN = autorités de Certification, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? cACertificate ? un ? objectClass = certificationAuthority » (afficher les certificats racine)
- « ldap : / / / CN = CAName, CN = autorités de Certification, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? cACertificate ? base ? objectClass = certificationAuthority » (modifier les certificats racine)
- « ldap : / / / CN = CAName, CN = MachineName, CN = CDP, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? certificateRevocationList ? base ? objectClass = cRLDistributionPoint » (vue CRL)
- « ldap : / / / CN = NTAuthCertificates, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? cACertificate ? base ? objectClass = certificationAuthority » (certificats d’autorité de certification d’entreprise)
- LDAP : (Certificats d’objet AD de l’ordinateur)
- -utilisateur ldap : (Certificats d’objet AD de l’utilisateur)

ID de certificat : Certificat ou jeton de correspondance de révocation de certificats.  Cela peut être un numéro de série, un SHA-1 certificat, CRL, CTL ou hachage de clé publique, un index de certificat numérique (0, 1 et ainsi de suite), un index numérique de la révocation de certificats (. 0,.1 et ainsi de suite), un index numérique de la liste CTL (.. 0... 1 et ainsi de suite), une clé publique, signature ou extension ObjectId, un objet du certificat nom commun, une adresse de messagerie UPN ou le nom DNS, un nom de conteneur de clé ou nom de fournisseur de services cryptographiques, un nom de modèle ou ObjectId, une extension EKU ou ObjectId de stratégies d’Application ou un nom commun d’émetteur de révocation de certificats. Nombre d'entre eux peuvent entraîner plusieurs correspondances.

OutputFile : fichier pour enregistrer le certificat correspondant

Utilisez - utilisateur à accéder à un magasin de l’utilisateur au lieu d’un magasin de l’ordinateur.

Utilisez - entreprise à accéder à un magasin d’entreprise de machine.

Utilisez - service pour accéder à un magasin de service de machine.

Utilisez - grouppolicy pour accéder à un magasin de stratégies de groupe ordinateur.

Exemples :

- -enterprise NTAuth
- -37 de la racine d’entreprise
- -utilisateur mon 26e0aaaf000000000004
- CA .11

[-f] [-enterprise] [-user] [-GroupPolicy] [-silent] [-split] [-dc DCName]

Retour à [Menu](#menu)

## <a name="-addstore"></a>-addstore

CertUtil [Options] -addstore CertificateStoreName InFile

Ajouter le certificat au magasin

CertificateStoreName: Nom de magasin de certificats.  Consultez [-stocker](#-store).

InFile : Fichier de certificat ou de révocation de certificats à ajouter au magasin.

[-f] [-enterprise] [-user] [-GroupPolicy] [-dc DCName]

Retour à [Menu](#menu)

## <a name="-delstore"></a>-delstore

CertUtil [Options] - delstore Nom_magasin_certificats CertId

Supprimer le certificat à partir du magasin

CertificateStoreName: Nom de magasin de certificats.  Consultez [-stocker](#-store).

ID de certificat : Certificat ou jeton de correspondance de révocation de certificats.  Consultez [-stocker](#-store).

[-enterprise] [-utilisateur] [-GroupPolicy] [-dc DCName]

Retour à [Menu](#menu)

## <a name="-verifystore"></a>-verifystore

CertUtil [Options] - verifystore Nom_magasin_certificats [CertId]

Vérifier le certificat dans le magasin

CertificateStoreName: Nom de magasin de certificats.  Consultez [-stocker](#-store).

ID de certificat : Certificat ou jeton de correspondance de révocation de certificats.  Consultez [-stocker](#-store).

[-enterprise] [-utilisateur] [-GroupPolicy] [-silencieux] [-Fractionner] [-dc DCName] [-t délai d’attente]

Retour à [Menu](#menu)

## <a name="-repairstore"></a>-repairstore

CertUtil [Options] - repairstore Nom_magasin_certificats CertIdList [PropertyInfFile | SDDLSecurityDescriptor]

Réparer l’association de clé ou de mettre à jour le descripteur de sécurité des propriétés ou de la clé de certificat

CertificateStoreName: Nom de magasin de certificats.  Consultez [-stocker](#-store).

CertIdList : liste séparée par des virgules des jetons de correspondance de certificat ou de révocation de certificats. Consultez [-stocker](#-store) CertId description.

PropertyInfFile--INF fichier contenant des propriétés externes :

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

[-f]. [-enterprise] [-utilisateur] [-GroupPolicy] [-silencieux] [-Fractionner] [-csp fournisseur]

Retour à [Menu](#menu)

## <a name="-viewstore"></a>-viewstore

CertUtil [Options] -viewstore [CertificateStoreName [CertId [OutputFile]]]

Vider le magasin de certificats

CertificateStoreName: Nom de magasin de certificats. Exemples :

- « My », « CA » (valeur par défaut), « Root »,
- « ldap : / / / CN = autorités de Certification, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? cACertificate ? un ? objectClass = certificationAuthority » (afficher les certificats racine)
- « ldap : / / / CN = CAName, CN = autorités de Certification, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? cACertificate ? base ? objectClass = certificationAuthority » (modifier les certificats racine)
- « ldap : / / / CN = CAName, CN = MachineName, CN = CDP, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? certificateRevocationList ? base ? objectClass = cRLDistributionPoint » (vue CRL)
- « ldap : / / / CN = NTAuthCertificates, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? cACertificate ? base ? objectClass = certificationAuthority » (certificats d’autorité de certification d’entreprise)
- LDAP : (Certificats d’objet AD de l’ordinateur)
- -utilisateur ldap : (Certificats d’objet AD de l’utilisateur)

ID de certificat : Certificat ou jeton de correspondance de révocation de certificats. Cela peut être un numéro de série, un SHA-1 certificat, CRL, CTL ou hachage de clé publique, un index de certificat numérique (0, 1 et ainsi de suite), un index numérique de la révocation de certificats (. 0,.1 et ainsi de suite), un index numérique de la liste CTL (.. 0... 1 et ainsi de suite), une clé publique, signature ou extension ObjectId, un objet du certificat nom commun, une adresse de messagerie UPN ou le nom DNS, un nom de conteneur de clé ou nom de fournisseur de services cryptographiques, un nom de modèle ou ObjectId, une extension EKU ou ObjectId de stratégies d’Application ou un nom commun d’émetteur de révocation de certificats. Nombre d'entre eux peuvent entraîner plusieurs correspondances.

OutputFile : fichier pour enregistrer le certificat correspondant

Utilisez - utilisateur à accéder à un magasin de l’utilisateur au lieu d’un magasin de l’ordinateur.

Utilisez - entreprise à accéder à un magasin d’entreprise de machine.

Utilisez - service pour accéder à un magasin de service de machine.

Utilisez - grouppolicy pour accéder à un magasin de stratégies de groupe ordinateur.

Exemples :

1. -enterprise NTAuth
2. -37 de la racine d’entreprise
3. -utilisateur mon 26e0aaaf000000000004
4. CA .11

[-f] [-enterprise] [-user] [-GroupPolicy] [-dc DCName]

Retour à [Menu](#menu)

## <a name="-viewdelstore"></a>-viewdelstore

CertUtil [Options] -viewdelstore [CertificateStoreName [CertId [OutputFile]]]

Supprimer le certificat à partir du magasin

CertificateStoreName: Nom de magasin de certificats. Exemples :

- « My », « CA » (valeur par défaut), « Root »,
- « ldap : / / / CN = autorités de Certification, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? cACertificate ? un ? objectClass = certificationAuthority » (afficher les certificats racine)
- « ldap : / / / CN = CAName, CN = autorités de Certification, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? cACertificate ? base ? objectClass = certificationAuthority » (modifier les certificats racine)
- « ldap : / / / CN = CAName, CN = MachineName, CN = CDP, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? certificateRevocationList ? base ? objectClass = cRLDistributionPoint » (vue CRL)
- « ldap : / / / CN = NTAuthCertificates, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com ? cACertificate ? base ? objectClass = certificationAuthority » (certificats d’autorité de certification d’entreprise)
- LDAP : (Certificats d’objet AD de l’ordinateur)
- -utilisateur ldap : (Certificats d’objet AD de l’utilisateur)

ID de certificat : Certificat ou jeton de correspondance de révocation de certificats. Cela peut être un numéro de série, un SHA-1 certificat, CRL, CTL ou hachage de clé publique, un index de certificat numérique (0, 1 et ainsi de suite), un index numérique de la révocation de certificats (. 0,.1 et ainsi de suite), un index numérique de la liste CTL (.. 0... 1 et ainsi de suite), une clé publique, signature ou extension ObjectId, un objet du certificat nom commun, une adresse de messagerie UPN ou le nom DNS, un nom de conteneur de clé ou nom de fournisseur de services cryptographiques, un nom de modèle ou ObjectId, une extension EKU ou ObjectId de stratégies d’Application ou un nom commun d’émetteur de révocation de certificats. Nombre d'entre eux peuvent entraîner plusieurs correspondances.

OutputFile : fichier pour enregistrer le certificat correspondant

Utilisez - utilisateur à accéder à un magasin de l’utilisateur au lieu d’un magasin de l’ordinateur.

Utilisez - entreprise à accéder à un magasin d’entreprise de machine.

Utilisez - service pour accéder à un magasin de service de machine.

Utilisez - grouppolicy pour accéder à un magasin de stratégies de groupe ordinateur.

Exemples :

1. -enterprise NTAuth
2. -37 de la racine d’entreprise
3. -utilisateur mon 26e0aaaf000000000004
4. CA .11

[-f] [-enterprise] [-user] [-GroupPolicy] [-dc DCName]

Retour à [Menu](#menu)

## <a name="-dspublish"></a>-dsPublish

[Options] de CertUtil - dsPublish CertFile [NTAuthCA | RootCA | Autorité de certification subordonnée | CrossCA | KRA | Utilisateur | Machine]

CertUtil [Options] -dsPublish CRLFile [DSCDPContainer [DSCDPCN]]

Publier le certificat ou révocation de certificats dans Active Directory

CertFile : fichier de certificat à publier

NTAuthCA : Publier le certificat dans le magasin de DS Enterprise

RootCA : Publier le certificat au magasin racine approuvé de DS

Autorité de certification subordonnée : Publier le certificat d’autorité de certification à l’objet de l’autorité de certification DS

CrossCA : Publication croisée de certificat à l’objet de l’autorité de certification DS

KRA : Publier le certificat à l’objet de service d’annuaire Key Recovery Agent

Utilisateur : Publier de certificat sur objet DS de l’utilisateur

Ordinateur : Publier cert à l’objet ordinateur DS

À CRLFile : Fichier CRL à publier

DSCDPContainer : Conteneur CDP de DS CN, généralement le nom de machine d’autorité de certification

DSCDPCN : DS CDP l’objet CN, généralement basé sur le nom court d’autorité de certification expurgé et l’index de clé

F - permet de créer l’objet DS.

[-f]. [-utilisateur] [-dc DCName]

Retour à [Menu](#menu)

## <a name="-adtemplate"></a>-ADTemplate

CertUtil [Options] - ADTemplate [modèle]

Modèles d’affichage AD

[-f]. [-utilisateur] [-ut] [-mt] [-dc DCName]

## <a name="-template"></a>-Modèle

CertUtil [Options]-[modèle]

Afficher les modèles de stratégie d’inscription

[-f]. [-utilisateur] [-silencieux] [-PolicyServer URLOrId] [-Anonyme] [-Kerberos] [-ClientCertificate ClientCertId] [-Nom d’utilisateur du nom d’utilisateur] [--p mot de passe]

Retour à [Menu](#menu)

## <a name="-templatecas"></a>-TemplateCAs

Modèle de - TemplateCAs CertUtil [Options]

Affichage des autorités de certification pour le modèle

[-f]. [-utilisateur] [-dc DCName]

Retour à [Menu](#menu)

## <a name="-catemplates"></a>-CATemplates

CertUtil [Options] - CATemplates [modèle]

Modèles d’affichage pour l’autorité de certification

[-f]. [-utilisateur] [-ut] [-mt] [-config Machine\CAName] [-dc DCName]

Retour à [Menu](#menu)

## <a name="-setcasites"></a>-SetCASites

CertUtil [Options] -SetCASites [set] [SiteName]

CertUtil [Options] - SetCASites vérifier [nom_site]

CertUtil [Options] SetCASites - delete

Ensemble, vérifiez ou autorité de certification supprimer les noms de sites

- Utilisez l’option - config pour cibler une autorité de certification (valeur par défaut est toutes les autorités de certification)
- *SiteName* est autorisée uniquement quand vous ciblez une autorité de certification
- Utilisez-f pour remplacer des erreurs de validation spécifié *SiteName*
- Utilisez-f pour supprimer tous les noms de site d’autorité de certification

[-f]. [-config Machine\CAName] [-dc DCName]

> [!NOTE]
> Pour plus d’informations sur la configuration des autorités de certification pour la reconnaissance des sites de Services de domaine Active Directory (AD DS), consultez [la reconnaissance des sites AD DS pour les clients AD CS et PKI](https://social.technet.microsoft.com/wiki/contents/articles/14106.ad-ds-site-awareness-for-ad-cs-and-pki-clients.aspx).

Retour à [Menu](#menu)

## <a name="-enrollmentserverurl"></a>-enrollmentServerURL

CertUtil [Options] - enrollmentServerURL [URL AuthenticationType [priorité] [modificateurs]]

Suppression de CertUtil [Options] enrollmentServerURL - URL

Afficher, ajouter ou supprimer les URL de serveur d’inscription associés à une autorité de certification

AuthenticationType : Spécifiez l’une des méthodes d’authentification client suivantes lors de l’ajout d’une URL

1. Kerberos : Utilisez les informations d’identification Kerberos SSL
2. Nom d’utilisateur : Utiliser un compte nommé pour les informations d’identification SSL
3. ClientCertificate : Utilisez les informations d’identification du certificat X.509 SSL
4. Anonymous : Utilisez les informations d’identification SSL anonymes

Supprimer : supprime l’URL spécifiée associée à l’autorité de certification

Priorité : valeur par défaut est '1' Si non spécifié lors de l’ajout d’une URL

Modificateurs--Séparées par des virgules liste d’une ou plusieurs des opérations suivantes :

1. AllowRenewalsOnly: Seules les demandes de renouvellement peuvent être envoyées à cette autorité de certification via cette URL
2. AllowKeyBasedRenewal: Autorise l’utilisation d’un certificat qui ne dispose d’aucun compte associé dans Active Directory. Cela s’applique uniquement avec ClientCertificate et AllowRenewalsOnly Mode

[-config Machine\CAName] [-dc DCName]

Retour à [Menu](#menu)

## <a name="-adca"></a>-ADCA

CertUtil [Options] -ADCA [CAName]

Afficher les autorités de certification AD

[-f]. [-Fractionner] [-dc DCName]

Retour à [Menu](#menu)

## <a name="-ca"></a>-CA

CA - CertUtil [Options] [CAName | TemplateName]

Afficher les autorités de certification de stratégie d’inscription

[-f]. [-utilisateur] [-silencieux] [-Fractionner] [-PolicyServer URLOrId] [-Anonyme] [-Kerberos] [-ClientCertificate ClientCertId] [-Nom d’utilisateur du nom d’utilisateur] [--p mot de passe]

Retour à [Menu](#menu)

## <a name="-policy"></a>-Policy

Afficher la stratégie d’inscription

[-f]. [-utilisateur] [-silencieux] [-Fractionner] [-PolicyServer URLOrId] [-Anonyme] [-Kerberos] [-ClientCertificate ClientCertId] [-Nom d’utilisateur du nom d’utilisateur] [--p mot de passe]

Retour à [Menu](#menu)

## <a name="-policycache"></a>-PolicyCache

CertUtil [Options] -PolicyCache [delete]

Afficher ou supprimer des entrées de Cache de la stratégie d’inscription

Supprimer : supprimer les entrées du cache de serveur de stratégie

-f: utilisez-f pour supprimer toutes les entrées de cache

[-f]. [-utilisateur] [-PolicyServer URLOrId]

Retour à [Menu](#menu)

## <a name="-credstore"></a>-CredStore

CertUtil [Options] - CredStore [URL]

Ajouter des URL de CertUtil [Options] - CredStore

Supprimer des URL de CertUtil [Options] - CredStore

Afficher, ajouter ou supprimer des entrées d’informations d’identification Store

URL : URL de cible.  Utilisez \* pour faire correspondre toutes les entrées. Utilisez https://machine\* pour correspondre à un préfixe d’URL.

ajouter : ajouter une entrée d’informations d’identification Store. Informations d’identification SSL doivent également être spécifiées.

Supprimer : supprimer des entrées d’informations d’identification Store

-f: utilisez-f pour remplacer une entrée ou de supprimer plusieurs entrées.

[-f]. [-utilisateur] [-silencieux] [-Anonyme] [-Kerberos] [-ClientCertificate ClientCertId] [-Nom d’utilisateur du nom d’utilisateur] [--p mot de passe]

Retour à [Menu](#menu)

## <a name="-installdefaulttemplates"></a>-InstallDefaultTemplates

CertUtil [Options] -InstallDefaultTemplates

Installer les modèles de certificats par défaut

[-dc DCName]

Retour à [Menu](#menu)

## <a name="-urlcache"></a>-URLCache

CertUtil [Options] -URLCache [URL | CRL | \* [delete]]

Afficher ou supprimer des entrées de cache d’URL

URL : URL de mise en cache

Révocation de certificats : fonctionnent sur toutes les URL mises en cache CRL uniquement

\*: opèrent sur des URL toutes les mises en cache

Supprimer : supprimer les URL pertinentes à partir du cache local de l’utilisateur actuel

Utilisez-f pour forcer l’extraction d’une URL spécifique et la mise à jour le cache.

[-f] [-split]

Retour à [Menu](#menu)

## <a name="-pulse"></a>-pulse

CertUtil [Options] - pulse

Événements de l’inscription automatique d’impulsion

[-user]

Retour à [Menu](#menu)

## <a name="-machineinfo"></a>-MachineInfo

CertUtil [Options] - MachineInfo Nomdomaine\nomordinateur$

Afficher les informations relatives aux objets ordinateur Active Directory

Retour à [Menu](#menu)

## <a name="-dcinfo"></a>-DCInfo

CertUtil [Options] - DCInfo [domaine] [vérifier | DeleteBad | DeleteAll]

Afficher les informations de contrôleur de domaine

Par défaut consiste à afficher les certificats de contrôleur de domaine sans vérification

[-f]. [-utilisateur] [-urlfetch] [-dc DCName] [-t délai d’attente]

> [!TIP]
> La possibilité de spécifier un domaine Active Directory Domain Services (AD DS) **[domaine]** et pour spécifier un contrôleur de domaine ( **-dc**) a été ajouté dans Windows Server 2012. Pour exécuter la commande, vous devez utiliser un compte qui est membre du **Admins du domaine** ou **administrateurs de l’entreprise**. Les modifications de comportement de cette commande sont les suivantes :</br>> 1.  Si un domaine n’est pas spécifié et un contrôleur de domaine spécifique n’est pas spécifié, cette option retourne une liste des contrôleurs de domaine à traiter à partir du contrôleur de domaine par défaut.</br>> 2.  Si un domaine n’est pas spécifié, mais un contrôleur de domaine est spécifié, un rapport des certificats sur le contrôleur de domaine spécifié est généré.</br>> 3.  Si un domaine est spécifié, mais un contrôleur de domaine n’est pas spécifié, une liste des contrôleurs de domaine est générée en même temps que les rapports sur les certificats pour chaque contrôleur de domaine dans la liste.</br>> 4.  Si le domaine et le contrôleur de domaine sont spécifiés, une liste de contrôleurs de domaine est générée à partir du contrôleur de domaine ciblé. Un rapport des certificats pour chaque contrôleur de domaine dans la liste est également généré.

Supposons par exemple, il existe un domaine nommé CPANDL avec un contrôleur de domaine nommé CPANDL-DC1. Vous pouvez exécuter la commande suivante pour extraire une liste des contrôleurs de domaine et leurs certificats qui de CPANDL-DC1 : certutil -dc-dc1 cpandl - dcinfo cpandl

Retour à [Menu](#menu)

## <a name="-entinfo"></a>-EntInfo

CertUtil [Options] -EntInfo DomainName\MachineName$

[-f] [-user]

Retour à [Menu](#menu)

## <a name="-tcainfo"></a>-TCAInfo

-TCAInfo CertUtil [Options] [DomainDN |-]

Informations de l’autorité de certification complet

[-f]. [-enterprise] [-utilisateur] [-urlfetch] [-dc DCName] [-t délai d’attente]

Retour à [Menu](#menu)

## <a name="-scinfo"></a>-SCInfo

CertUtil [Options] -SCInfo [ReaderName [CRYPT_DELETEKEYSET]]

Afficher les informations de carte à puce

CRYPT_DELETEKEYSET : Supprimez toutes les clés sur la carte à puce

[-silencieux] [-Fractionner] [-urlfetch] [-t délai d’attente]

Retour à [Menu](#menu)

## <a name="-scroots"></a>-SCRoots

Mettre à jour de SCRoots - CertUtil [Options] [+] [InputRootFile] [nom_lecteur]

Enregistrer CertUtil [Options] - SCRoots @OutputRootFile [nom_lecteur]

Vue de SCRoots - CertUtil [Options] [InputRootFile | Nom_lecteur]

CertUtil [Options] - SCRoots supprimer [nom_lecteur]

Gérer les certificats racine de carte à puce

[-f]. [-Fractionner] [--p mot de passe]

Retour à [Menu](#menu)

## <a name="-verifykeys"></a>-verifykeys

CertUtil [Options] - verifykeys [fichier de Cert CA]

Vérifiez le jeu de clés publique/privée

Nom de conteneur de clé : nom de conteneur de clé de la clé à vérifier. Par défaut, les clés d’ordinateurs.  Utilisez - user pour les clés de l’utilisateur.

CACertFile : fichier de certificat de signature ou chiffrement

Si aucun argument n’est spécifié, chaque certificat d’autorité de certification de signature est vérifiée par rapport à sa clé privée.

Cette opération peut uniquement être effectuée par rapport à une autorité de certification locale ou des clés locales.

[-f]. [-utilisateur] [-silencieux] [-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-verify"></a>-vérifier

CertUtil [Options] -verify CertFile [ApplicationPolicyList | - [IssuancePolicyList]]

CertUtil [Options] -verify CertFile [CACertFile [CrossedCACertFile]]

CertUtil [Options] - vérifier à CRLFile CACertFile [IssuedCertFile]

CertUtil [Options] - vérifier à CRLFile CACertFile [DeltaCRLFile]

Vérifier le certificat, de révocation de certificats ou de chaîne

CertFile : Certificat de vérification

ApplicationPolicyList : facultatif liste séparée par virgules de requis ObjectID de stratégie d’Application

IssuancePolicyList : facultatif liste séparée par virgules de requis ObjectID de stratégie d’émission

CACertFile : facultatif de certification émettrice pour vérifier par rapport à

CrossedCACertFile : facultatives du certificat certification croisée par CertFile

À CRLFile : Révocation de certificats pour vérifier

IssuedCertFile : certificat émis facultatif couvert par à CRLFile

DeltaCRLFile : la liste CRL facultative delta

Si ApplicationPolicyList est spécifié, génération de la chaîne est limitée aux chaînes valides pour les stratégies d’Application spécifié.

Si IssuancePolicyList est spécifié, la génération de la chaîne est limitée aux chaînes valides pour les stratégies d’émission spécifiées.

Si CACertFile est spécifié, les champs de CACertFile sont vérifiés par rapport à CertFile ou CRLFile.

Si CACertFile n’est pas spécifié, CertFile est utilisé pour générer et vérifier une chaîne complète.

Si CACertFile et CrossedCACertFile sont spécifiés, les champs de CACertFile et CrossedCACertFile sont comparés à CertFile.

Si IssuedCertFile est spécifié, les champs de IssuedCertFile sont vérifiés par rapport à CRLFile.

Si DeltaCRLFile est spécifié, les champs de DeltaCRLFile sont vérifiés par rapport à CRLFile.

[-f]. [-enterprise] [-utilisateur] [-silencieux] [-Fractionner] [-urlfetch] [-t délai d’attente]

Retour à [Menu](#menu)

## <a name="-verifyctl"></a>-verifyCTL

CertUtil [Options] -verifyCTL CTLObject [CertDir] [CertFile]

Vérifiez AuthRoot ou liste CTL de certificats non autorisés

CTLObject : Identifie la liste CTL pour vérifier :

- AuthRootWU : lire AuthRoot CAB et les certificats correspondants à partir du cache d’URL. Pour télécharger à partir de la mise à jour de Windows au lieu de cela, utilisez -f.
- DisallowedWU : CAB de certificats non autorisés de lire et interdit de fichier de magasin de certificats à partir du cache d’URL.  Pour télécharger à partir de la mise à jour de Windows au lieu de cela, utilisez -f.
- AuthRoot : mise en cache de lecture de Registre AuthRoot CTL.  Utiliser avec -f et un CertFile qui n’est pas déjà approuvé pour forcer la mise à jour le Registre mis en cache AuthRoot et CTL de certificats non autorisés.
- Non autorisé : mise en cache de lecture de Registre CTL de certificats non autorisés. f - a le même comportement à l’instar de AuthRoot.
- CTLFileName : fichier ou http : chemin d’accès à la liste CTL ou CAB

CertDir : dossier contenant les certificats des entrées de liste CTL correspondantes. Http : chemin d’accès du dossier doit se terminer par un séparateur de chemin d’accès. Si un dossier n’est pas spécifié avec AuthRoot ou non autorisé, plusieurs emplacements à rechercher des certificats de mise en correspondance : les magasins de certificats local, les ressources de fichier crypt32.dll et le cache local de l’URL. Utilisez-f pour télécharger à partir de la mise à jour de Windows lorsque cela est nécessaire. Sinon, par défaut est le même dossier ou site web en tant que le CTLObject.

CertFile : fichier contenant l’ou les certificats à vérifier. Certificats seront mis en correspondance avec les entrées de la liste CTL et correspondent aux résultats affichés. Supprime la majeure partie de la sortie par défaut.

[-f] [-user] [-split]

Retour à [Menu](#menu)

## <a name="-sign"></a>-sign

CertUtil [Options] - signer InFileList | SerialNumber (Numéro_série) | Liste de révocation OutFileList [StartDate + hh] [+ SerialNumberList | - SerialNumberList | - ObjectIdList | @ExtensionFile]

CertUtil [Options] - signer InFileList | SerialNumber (Numéro_série) | Liste de révocation OutFileList [#HashAlgorithm] [+ AlternateSignatureAlgorithm | - AlternateSignatureAlgorithm]

Signer à nouveau certificat ou révocation de certificats

InFileList : séparées par des virgules liste des fichiers de certificat ou de révocation de certificats à modifier et signer à nouveau

SerialNumber (Numéro_série) : Numéro de série du certificat à créer. Période de validité et d’autres options ne doivent pas être présentes.

RÉVOCATION DE CERTIFICATS : Créer une liste de révocation vide. Période de validité et d’autres options ne doivent pas être présentes.

OutFileList : liste séparée par des virgules des fichiers de sortie certificat ou de révocation de certificats modifiés. Le nombre de fichiers doit correspondre à InFileList.

Date de début + hh : nouvelle période de validité : date facultatif plus (+) ; jours facultatifs et la période de validité heures ; Si les deux sont spécifiés, utilisez un séparateur de signe plus (+). Utilisez « maintenant [+ hh] » pour démarrer à l’heure actuelle. Utilisez « jamais » pour n’avoir aucune date d’expiration (pour les CRL uniquement).

SerialNumberList : séparées par des virgules liste de numéro de série pour ajouter ou supprimer

ObjectIdList : séparées par des virgules extension une liste d’ID d’objet à supprimer

@ExtensionFile: Fichier INF contenant les extensions pour mettre à jour ou supprimer :

```
[Extensions]
     2.5.29.31 = ; Remove CRL Distribution Points extension
     2.5.29.15 = "{hex}" ; Update Key Usage extension
     _continue_="03 02 01 86"
```

Élément HashAlgorithm impossible : Nom de l’algorithme de hachage précédée du signe #

AlternateSignatureAlgorithm : autre spécificateur d’algorithme de Signature

Un signe moins entraîne des numéros de série et les extensions à supprimer. Un signe plus provoque des numéros de série à ajouter à une liste de révocation. Lors de la suppression des éléments à partir d’une liste de révocation, la liste peut contenir des numéros de série et ID d’objet. Un signe moins avant AlternateSignatureAlgorithm provoque le format de signature hérité à utiliser. Un signe plus avant que AlternateSignatureAlgorithm provoque le format de signature alternature à utiliser. Si AlternateSignatureAlgorithm n’est pas spécifié le format de signature dans le certificat ou la révocation de certificats est utilisé.

[-nullsign] [-f]. [-silencieux] [-Cert CertId]

Retour à [Menu](#menu)

## <a name="-vroot"></a>-vroot

CertUtil [Options] -vroot [delete]

Créer/supprimer des racines virtuelles web et des partages de fichiers

Retour à [Menu](#menu)

## <a name="-vocsproot"></a>-vocsproot

CertUtil [Options] -vocsproot [delete]

Créer/supprimer des racines virtuelles web pour le proxy web OCSP

Retour à [Menu](#menu)

## <a name="-addenrollmentserver"></a>-addEnrollmentServer

CertUtil [Options] -addEnrollmentServer Kerberos | UserName | ClientCertificate [AllowRenewalsOnly] [AllowKeyBasedRenewal]

Ajouter une application serveur d’inscription

Ajouter une application de serveur d’inscription et le pool d’applications si nécessaire, pour l’autorité de certification spécifiée. Cette commande n’installe pas les fichiers binaires ou packages. L’une des méthodes d’authentification suivantes avec lequel le client se connecte à un serveur d’inscription de certificat.

- Kerberos : Utilisez les informations d’identification Kerberos SSL
- Nom d’utilisateur : Utiliser un compte nommé pour les informations d’identification SSL
- ClientCertificate : Utilisez les informations d’identification du certificat X.509 SSL
- AllowRenewalsOnly: Seules les demandes de renouvellement peuvent être envoyées à cette autorité de certification via cette URL
- AllowKeyBasedRenewal--Autorise l’utilisation d’un certificat qui ne dispose d’aucun compte associé dans Active Directory. Cela s’applique uniquement avec le mode ClientCertificate et AllowRenewalsOnly.

[-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-deleteenrollmentserver"></a>-deleteEnrollmentServer

CertUtil [Options] - deleteEnrollmentServer Kerberos | Nom d’utilisateur | ClientCertificate

Supprimer une application serveur d’inscription

Supprimer une application de serveur d’inscription et le pool d’applications si nécessaire, pour l’autorité de certification spécifiée. Cette commande ne supprime pas les fichiers binaires ou packages. L’une des méthodes d’authentification suivantes avec lequel le client se connecte à un serveur d’inscription de certificat.

1. Kerberos : Utilisez les informations d’identification Kerberos SSL
2. Nom d’utilisateur : Utiliser un compte nommé pour les informations d’identification SSL
3. ClientCertificate : Utilisez les informations d’identification du certificat X.509 SSL

[-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-addpolicyserver"></a>-addPolicyServer

CertUtil [Options] - addPolicyServer Kerberos | Nom d’utilisateur | ClientCertificate [KeyBasedRenewal]

Ajouter une application de serveur de stratégie

Ajouter une application de serveur NPS et le pool d’applications si nécessaire. Cette commande n’installe pas les fichiers binaires ou packages. L’une des méthodes d’authentification suivantes avec lequel le client se connecte à un serveur de stratégie de certificat :

- Kerberos : Utilisez les informations d’identification Kerberos SSL
- Nom d’utilisateur : Utiliser un compte nommé pour les informations d’identification SSL
- ClientCertificate : Utilisez les informations d’identification du certificat X.509 SSL
- KeyBasedRenewal: Seules les stratégies qui contiennent des modèles de KeyBasedRenewal sont retournées au client. Cet indicateur s’applique uniquement pour l’authentification de nom d’utilisateur et ClientCertificate.

Retour à [Menu](#menu)

## <a name="-deletepolicyserver"></a>-deletePolicyServer

CertUtil [Options] - deletePolicyServer Kerberos | Nom d’utilisateur | ClientCertificate [KeyBasedRenewal]

Supprimer une application de serveur de stratégie

Supprimer une application de serveur NPS et le pool d’applications si nécessaire. Cette commande ne supprime pas les fichiers binaires ou packages. L’une des méthodes d’authentification suivantes avec lequel le client se connecte à un serveur de stratégie de certificat :

1. Kerberos : Utilisez les informations d’identification Kerberos SSL
2. Nom d’utilisateur : Utiliser un compte nommé pour les informations d’identification SSL
3. ClientCertificate : Utilisez les informations d’identification du certificat X.509 SSL
4. KeyBasedRenewal: Serveur de stratégie KeyBasedRenewal

Retour à [Menu](#menu)

## <a name="-oid"></a>-oid

CertUtil [Options] - oid ObjectId [DisplayName | supprimer [LanguageId [Type]]]

CertUtil [Options] - oid GroupId

CertUtil [Options] -oid AlgId | AlgorithmName [GroupId]

Afficher les ID d’objet ou définir le nom d’affichage

- ObjectId : ObjectId pour afficher ou pour ajouter le nom d’affichage
- GroupId--GroupId décimale pour les ID d’objet à énumérer
- AlgId--hexadécimal AlgId pour l’ID d’objet à rechercher
- AlgorithmName--Nom de l’algorithme pour les ID d’objet à rechercher
- DisplayName : Nom d’affichage à stocker dans le service d’annuaire
- Supprimer : supprimer le nom d’affichage
- LanguageId--Id de langue (valeur par défaut est en cours : 1033)
- Type--DS type d’objets à créer : 1 pour le modèle (par défaut), 2 pour la stratégie d’émission, 3 pour la stratégie d’Application
- F - permet de créer l’objet DS.

[-f]

Retour à [Menu](#menu)

## <a name="-error"></a>-erreur

CertUtil [Options] - Erreur ErrorCode

Afficher le texte du code d’erreur

Retour à [Menu](#menu)

## <a name="-getreg"></a>-getreg

[Options] de CertUtil - getreg [{autorité de certification | restaurer | stratégie | quitter | modèle | inscrire | chaîne | PolicyServers}\[ProgId\]] [Nom_valeur_registre]

Afficher la valeur de Registre

autorité de certification : Clé de Registre de l’autorité de certification

Restauration : Clé de Registre de restauration de l’autorité de certification

stratégie : Utiliser la clé de Registre du module de stratégie

sortie : Utilisez quitter tout d’abord la clé de Registre du module

Modèle : Utiliser le modèle de clé de Registre (utilisez - utilisateur pour les modèles de l’utilisateur)

inscrire : Utiliser la clé de Registre de l’inscription (utilisez - utilisateur pour le contexte de l’utilisateur)

chaîne : Utiliser la clé de Registre de configuration de chaîne

PolicyServers : Utiliser la clé de Registre de serveurs de stratégie

ProgId : Utilisez la stratégie ou de quitter le ProgId du module (nom de sous-clé de Registre)

Nom_valeur_registre : nom de valeur de Registre (utilisez « nom\*» pour la correspondance de préfixe)

Valeur : nouvelle numérique, chaîne ou date de valeur de Registre ou nom de fichier. Si une valeur numérique commence par « + » ou «- », les bits spécifiés dans la nouvelle valeur sont définies ou effacés de la valeur de Registre existante.

Si une valeur de chaîne commence par « + » ou «- » et la valeur existante est une valeur REG_MULTI_SZ, la chaîne est ajoutée ou supprimée à partir de la valeur de Registre existante. Pour forcer la création d’une valeur REG_MULTI_SZ, ajoutez un « \n » à la fin de la valeur de chaîne.

Si la valeur commence par « @ », le reste de la valeur est le nom du fichier contenant la représentation hexadécimale d’une valeur binaire. Si elle ne fait pas référence à un fichier valide, il est analysé à la place en tant que [Date] [+ |-] [hh]--une date facultative plus ou moins facultatifs jours et heures. Si les deux sont spécifiés, utilisez un signe plus (+) ou un séparateur de signe moins (-). Utilisez « now + hh » pour une date par rapport à l’heure actuelle.

Utilisez « chain\ChainCacheResyncFiletime @now» pour vider efficacement CRL mis en cache.

[-f]. [-utilisateur] [-GroupPolicy] [-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-setreg"></a>-setreg

[Options] de CertUtil - setreg [{autorité de certification | restaurer | stratégie | quitter | modèle | inscrire | chaîne | PolicyServers}\[ProgId\]] Nom_valeur_registre valeur

Valeur de Registre définie

autorité de certification : Clé de Registre de l’autorité de certification

Restauration : Clé de Registre de restauration de l’autorité de certification

stratégie : Utiliser la clé de Registre du module de stratégie

sortie : Utilisez quitter tout d’abord la clé de Registre du module

Modèle : Utiliser le modèle de clé de Registre (utilisez - utilisateur pour les modèles de l’utilisateur)

inscrire : Utiliser la clé de Registre de l’inscription (utilisez - utilisateur pour le contexte de l’utilisateur)

chaîne : Utiliser la clé de Registre de configuration de chaîne

PolicyServers : Utiliser la clé de Registre de serveurs de stratégie

ProgId : Utilisez la stratégie ou de quitter le ProgId du module (nom de sous-clé de Registre)

Nom_valeur_registre : nom de valeur de Registre (utilisez « nom\*» pour la correspondance de préfixe)

Valeur : nouvelle numérique, chaîne ou date de valeur de Registre ou nom de fichier. Si une valeur numérique commence par « + » ou «- », les bits spécifiés dans la nouvelle valeur sont définies ou effacés de la valeur de Registre existante.

Si une valeur de chaîne commence par « + » ou «- » et la valeur existante est une valeur REG_MULTI_SZ, la chaîne est ajoutée ou supprimée à partir de la valeur de Registre existante. Pour forcer la création d’une valeur REG_MULTI_SZ, ajoutez un « \n » à la fin de la valeur de chaîne.

Si la valeur commence par « @ », le reste de la valeur est le nom du fichier contenant la représentation hexadécimale d’une valeur binaire. Si elle ne fait pas référence à un fichier valide, il est analysé à la place en tant que [Date] [+ |-] [hh]--une date facultative plus ou moins facultatifs jours et heures. Si les deux sont spécifiés, utilisez un signe plus (+) ou un séparateur de signe moins (-). Utilisez « now + hh » pour une date par rapport à l’heure actuelle.

Utilisez « chain\ChainCacheResyncFiletime @now» pour vider efficacement CRL mis en cache.

[-f]. [-utilisateur] [-GroupPolicy] [-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-delreg"></a>-delreg

Delreg - CertUtil [Options] [{autorité de certification | restaurer | stratégie | quitter | modèle | inscrire | chaîne | PolicyServers}\[ProgId\]] [Nom_valeur_registre]

Supprimer la valeur de Registre

autorité de certification : Clé de Registre de l’autorité de certification

Restauration : Clé de Registre de restauration de l’autorité de certification

stratégie : Utiliser la clé de Registre du module de stratégie

sortie : Utilisez quitter tout d’abord la clé de Registre du module

Modèle : Utiliser le modèle de clé de Registre (utilisez - utilisateur pour les modèles de l’utilisateur)

inscrire : Utiliser la clé de Registre de l’inscription (utilisez - utilisateur pour le contexte de l’utilisateur)

chaîne : Utiliser la clé de Registre de configuration de chaîne

PolicyServers : Utiliser la clé de Registre de serveurs de stratégie

ProgId : Utilisez la stratégie ou de quitter le ProgId du module (nom de sous-clé de Registre)

Nom_valeur_registre : nom de valeur de Registre (utilisez « nom\*» pour la correspondance de préfixe)

Valeur : nouvelle numérique, chaîne ou date de valeur de Registre ou nom de fichier. Si une valeur numérique commence par « + » ou «- », les bits spécifiés dans la nouvelle valeur sont définies ou effacés de la valeur de Registre existante.

Si une valeur de chaîne commence par « + » ou «- » et la valeur existante est une valeur REG_MULTI_SZ, la chaîne est ajoutée ou supprimée à partir de la valeur de Registre existante. Pour forcer la création d’une valeur REG_MULTI_SZ, ajoutez un « \n » à la fin de la valeur de chaîne.

Si la valeur commence par « @ », le reste de la valeur est le nom du fichier contenant la représentation hexadécimale d’une valeur binaire. Si elle ne fait pas référence à un fichier valide, il est analysé à la place en tant que [Date] [+ |-] [hh]--une date facultative plus ou moins facultatifs jours et heures. Si les deux sont spécifiés, utilisez un signe plus (+) ou un séparateur de signe moins (-). Utilisez « now + hh » pour une date par rapport à l’heure actuelle.

Utilisez « chain\ChainCacheResyncFiletime @now» pour vider efficacement CRL mis en cache.

[-f]. [-utilisateur] [-GroupPolicy] [-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-importkms"></a>-ImportKMS

CertUtil [Options] -ImportKMS UserKeyAndCertFile [CertId]

Importer des clés utilisateur et les certificats dans la base de données de serveur pour l’archivage des clés

UserKeyAndCertFile--Fichier de données contenant des clés privées d’utilisateur et des certificats à archiver.  Il peut s’agir d’une des opérations suivantes :

- Fichier d’exportation de serveur de gestion de clés (KMS) Exchange
- Fichier PFX

ID de certificat : Jeton de correspondance de certificat de déchiffrement de fichier d’exportation KMS.  Consultez [-stocker](#-store).

Utilisez -f pour importer des certificats non émis par l’autorité de certification.

[-f]. [-silencieux] [-Fractionner] [-config Machine\CAName] [--p mot de passe] [-symkeyalg SymmetricKeyAlgorithm [, KeyLength]]

Retour à [Menu](#menu)

## <a name="-importcert"></a>-ImportCert

CertUtil [Options] ImportCert - Certfile [ExistingRow]

Importer un fichier de certificat dans la base de données

Utilisez ExistingRow pour importer le certificat à la place d’une demande en attente pour la même clé.

Utilisez -f pour importer des certificats non émis par l’autorité de certification.

L’autorité de certification devrez peut-être également être configuré pour prendre en charge l’importation du certificat étrangère : certutil - setreg ca\KRAFlags + KRAF_ENABLEFOREIGN

[-f]. [-config Machine\CAName]

Retour à [Menu](#menu)

## <a name="-getkey"></a>-GetKey

SearchToken - GetKey de CertUtil [Options] [RecoveryBlobOutFile]

CertUtil [Options] - GetKey SearchToken script OutputScriptFile

Récupérer CertUtil [Options] - GetKey SearchToken | récupérer OutputFileBaseName

Récupérer l’objet blob de récupération de clés privées archivées, de générer un script de récupération ou de récupérer les clés archivées

script : générer un script pour récupérer et restaurer des clés (comportement par défaut si plusieurs candidats de récupération correspondant sont trouvés, ou si le fichier de sortie n’est pas spécifié).

récupérer : récupérer un ou plusieurs Blobs de récupération de clé (comportement par défaut si exactement un candidat de récupération correspondant est trouvé, et si le fichier de sortie est spécifié)

récupérer : récupérer et restaurer des clés privées en une seule étape (nécessite des certificats de Key Recovery Agent et les clés privées)

SearchToken : Permet de sélectionner les clés et les certificats à récupérer.

peut être une des opérations suivantes :

1. Nom commun du certificat
2. Numéro de série du certificat
3. Hachage SHA-1 du certificat (empreinte numérique)
4. Hachage KeyId SHA-1 du certificat (identificateur de clé sujet)
5. Nom du demandeur (domaine\utilisateur)
6. UPN (user@domain)

RecoveryBlobOutFile : fichier de sortie contenant une chaîne de certificat et une clé privée associée, toujours chiffrée avec un ou plusieurs certificats de Key Recovery Agent.

OutputScriptFile : fichier de sortie qui contient un script de commandes pour récupérer et restaurer les clés privées.

OutputFileBaseName : nom base du fichier de sortie. Pour extraire, n’importe quelle extension est tronquée et une chaîne de certificat et l’extension .rec sont ajoutées pour chaque objet blob de récupération de clé.  Chaque fichier contient une chaîne de certificat et une clé privée associée, toujours chiffrée avec un ou plusieurs certificats de Key Recovery Agent. Pour la récupération, n’importe quelle extension est tronquée et l’extension .p12 est ajoutée.  Contient les chaînes de certificats récupérés et les clés privées associées, stockés dans un fichier PFX.

[-f]. [-UnicodeText] [-silencieux] [-config Machine\CAName] [--p mot de passe] [-ProtectTo SAMNameAndSIDList] [-csp fournisseur]

Retour à [Menu](#menu)

## <a name="-recoverkey"></a>-RecoverKey

CertUtil [Options] -RecoverKey RecoveryBlobInFile [PFXOutFile [RecipientIndex]]

Récupérer la clé privée archivée

[-f] [-user] [-silent] [-split] [-p Password] [-ProtectTo SAMNameAndSIDList] [-csp Provider] [-t Timeout]

Retour à [Menu](#menu)

## <a name="-mergepfx"></a>-MergePFX

CertUtil [Options] - MergePFX PFXInFileList Fichier_sortie_pfx [ExtendedProperties]

PFXInFileList: Liste des fichiers d’entrée de PFX séparés par des virgules

Fichier_sortie_pfx : Fichier de sortie PFX

ExtendedProperties : Inclure les propriétés étendues

Le mot de passe spécifié sur la ligne de commande est une liste de mot de passe séparées par des virgules.  Si plus d’un mot de passe est spécifié, le dernier mot de passe est utilisé pour le fichier de sortie.  Si seul un mot de passe est spécifié ou si le dernier mot de passe est «\*», l’utilisateur sera invité pour le mot de passe de fichier de sortie.

[-f]. [-utilisateur] [-Fractionner] [--p mot de passe] [-ProtectTo SAMNameAndSIDList] [-csp fournisseur]

Retour à [Menu](#menu)

## <a name="-convertepf"></a>-ConvertEPF

CertUtil [Options] -ConvertEPF PFXInFileList EPFOutFile [cast | cast-] [V3CACertId][,Salt]

Convertir les fichiers PFX en fichier EPF

PFXInFileList: Liste des fichiers d’entrée de PFX séparés par des virgules

EPF : Fichier de sortie EPF

cast : Utiliser le chiffrement CAST 64

cast- : Utiliser le chiffrement CAST 64 (exportation)

V3CACertId : Jeton de correspondance du certificat d’autorité de certification V3.  Consultez [-stocker](#-store) CertId description.

Salt : Chaîne salt du fichier de sortie EPF

Le mot de passe spécifié sur la ligne de commande est une liste de mot de passe séparées par des virgules. Si plus d’un mot de passe est spécifié, le dernier mot de passe est utilisé pour le fichier de sortie.  Si seul un mot de passe est spécifié ou si le dernier mot de passe est «\*», l’utilisateur sera invité pour le mot de passe de fichier de sortie.

[-f]. [-silencieux] [-Fractionner] [-dc DCName] [--p mot de passe] [-csp fournisseur]

Retour à [Menu](#menu)

## <a name="options"></a>Options

Cette section définit les options que vous pouvez spécifier avec la commande.

|Options|Description|
|-------|-----------|
|-nullsign|Utiliser le hachage de données comme signature|
|-f|Forcer le remplacement|
|-enterprise|Utiliser le magasin de certificats ordinateur local Enterprise du Registre|
|-utilisateur|Utiliser les clés HKEY_CURRENT_USER ou le magasin de certificats|
|-GroupPolicy|Magasin de certificats de stratégie de groupe de l’utilisation|
|-ut|Afficher les modèles de l’utilisateur|
|-mt|Afficher les modèles d’ordinateur|
|-Unicode|Écrire la sortie redirigée en Unicode|
|-UnicodeText|Écrire le fichier de sortie au format Unicode|
|-gmt|Afficher les heures GMT|
|-secondes|Afficher le temps en secondes et millisecondes|
|-silent|Utiliser un indicateur muet pour obtenir le contexte|
|-split|Éléments intégrés ASN.1 et enregistrer dans des fichiers|
|-v|Fonctionnement en mode détaillé|
|-privatekey|Afficher les données de clé privées et le mot de passe|
|-épingler des PIN|Code confidentiel de carte à puce|
|-urlfetch|Récupérer et vérifier les certificats AIA et CDP CRL|
|-config Machine\CAName|Chaîne de nom d’autorité de certification et d’ordinateur|
|-PolicyServer URLOrId|URL du serveur de stratégie ou de code. Pour la sélection U / I, utilisez - PolicyServer. Pour tous les serveurs de stratégie, utilisez - PolicyServer \*|
|-Anonyme|Utilisez les informations d’identification SSL anonymes|
|-Kerberos|Utilisez les informations d’identification Kerberos SSL|
|-ClientCertificate ClientCertId|Utilisez les informations d’identification du certificat X.509 SSL. Pour la sélection U / I, utilisez - clientCertificate.|
|Nom d’utilisateur - nom d’utilisateur|Utilisez le compte nommé pour les informations d’identification SSL. Pour la sélection U / I, utilisez - UserName.|
|-Cert CertId|Certificat de signature|
|-dc DCName|Cible un contrôleur de domaine spécifique|
|-restreindre RestrictionList|Liste de restrictions séparées par des virgules. Chacune des restrictions se compose d’un nom de colonne, un opérateur relationnel et une constante entière, chaîne ou date. Un nom de colonne peut être précédé par un signe plus ou moins pour indiquer l’ordre de tri. Exemples :</br>"RequestId = 47"</br>« + RequesterName > = a, RequesterName < b »</br>«-RequesterName > domaine, Disposition = 21 »|
|-out ColumnList|Liste de colonnes séparées par des virgules|
|mot de passe -p|Mot de passe|
|-ProtectTo SAMNameAndSIDList|Liste de nom/SID SAM séparées par des virgules|
|csp - fournisseur|Fournisseur|
|t - délai d’attente|Délai d’attente d’URL fetch en millisecondes|
|-symkeyalg SymmetricKeyAlgorithm [, KeyLength]|Nom de l’algorithme de clé symétrique avec la longueur de clé facultatif, exemple : AES 128 ou 3DES|

Retour à [Menu](#menu)

## <a name="additional-certutil-examples"></a>Obtenir des exemples supplémentaires de certutil

Pour obtenir des exemples montrant comment utiliser cette commande, consultez

1. [Exemples de Certutil pour la gestion des Services de certificats Active Directory (AD CS) à partir de la ligne de commande](https://social.technet.microsoft.com/wiki/contents/articles/3063.certutil-examples-for-managing-active-directory-certificate-services-ad-cs-from-the-command-line.aspx)
2. [Tâches Certutil pour la gestion des certificats](https://technet.microsoft.com/library/cc772898.aspx)
3. [Exportation de requête binaire à l’aide de la procédure d’outil de ligne de commande CertUtil.exe](https://social.technet.microsoft.com/wiki/contents/articles/7573.active-directory-certificate-services-pki-key-archival-and-management.aspx)
4. [Renouvellement de certificat d’autorité de certification racine](https://social.technet.microsoft.com/wiki/contents/articles/2016.root-ca-certificate-renewal.aspx)
5. [Certutil](https://msdn.microsoft.com/subscriptions/cc773087.aspx)

Retour à [Menu](#menu)
