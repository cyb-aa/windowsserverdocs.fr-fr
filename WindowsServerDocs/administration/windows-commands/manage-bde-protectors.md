---
title: gérer-bde protecteurs
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f9b22c5-cc93-45df-9165-bedee94998da
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/06/2018
ms.openlocfilehash: c804fe6fa4aca8c55bd6a312536cc453962b3c1c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852840"
---
# <a name="manage-bde-protectors"></a>gérer-bde : protecteurs

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Gère les méthodes de protection utilisées pour la clé de chiffrement BitLocker. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).
## <a name="syntax"></a>Syntaxe
```
manage-bde -protectors [{-get|-add|-delete|-disable|-enable|-adbackup|-aadbackup}] <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|-get|Affiche toutes les méthodes de protection de clé est activées sur le lecteur ainsi que leur type et l’identificateur (ID).|
|-add|Ajoute des méthodes de protection de clé spécifié à l’aide est supplémentaires [ ajouter des paramètres](manage-bde-protectors.md#BKMK_addprotectors).|
|-delete|Supprime les méthodes de protection de clé utilisés par BitLocker. Tous les protecteurs de clé seront retirés un lecteur, sauf si le paramètre facultatif [-supprimer des paramètres](manage-bde-protectors.md#BKMK_deleteprotectors) sont utilisées pour spécifier les logiciels de protection à supprimer. Lorsque le dernier protecteur sur un lecteur est supprimé, la protection BitLocker du lecteur est désactivée pour vous assurer que l’accès aux données n’est pas perdu par inadvertance.|
|-disable|Désactive la protection, ce qui permettra à tout le monde à accéder aux données chiffrées par la mise à disposition la clé de chiffrement non sécurisés sur le lecteur. Aucun protecteur de clé n’est supprimés. Protection de reprise de la prochaine fois que Windows est démarré, sauf si le paramètre facultatif [-désactiver les paramètres](manage-bde-protectors.md#BKMK_disableprot) sont utilisés pour spécifier le nombre de redémarrage.|
|-enable|Active la protection en supprimant la clé de chiffrement non sécurisé à partir du lecteur. Tous les protecteurs de clé configurés sur le lecteur seront appliquées.|
|-adbackup|Sauvegarde de toutes les informations de récupération du lecteur spécifié aux Services de domaine Active Directory (AD DS). Pour sauvegarder une clé de récupération unique uniquement dans les services AD DS, vous devez ajouter le **-id** paramètre et spécifiez l’ID d’une clé de récupération spécifique à sauvegarder.|
|-aadbackup|Sauvegarde de toutes les informations de récupération pour le lecteur spécifié à Azure Active Directory (Azure Ad). Pour sauvegarder uniquement une clé de récupération unique pour Azure AD, ajoutez le **-id** paramètre et spécifiez l’ID d’une clé de récupération spécifique à sauvegarder.|
|<Drive>|Représente une lettre de lecteur suivie par un signe deux-points.|
|-computername|Spécifie que bde.exe gérer permet de modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **- cn** comme une version abrégée de cette commande.|
|<Name>|Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.|
|-? ou /?|Affiche un résumé aide à l’invite de commandes.|
|-help ou-h|Affiche une aide à l’invite de commande complète.|
### <a name="BKMK_addprotectors"></a>-Ajoutez la syntaxe et les paramètres
```
manage-bde  protectors  add [<Drive>] [-forceupgrade] [-recoverypassword <NumericalPassword>] [-recoverykey <pathToExternalKeydirectory>]
[-startupkey <pathToExternalKeydirectory>] [-certificate {-cf <pathToCertificateFile>|-ct <CertificateThumbprint>}] [-tpm] [-tpmandpin] 
[-tpmandstartupkey <pathToExternalKeydirectory>] [-tpmandpinandstartupkey <pathToExternalKeydirectory>] [-password][-adaccountorgroup <securityidentifier> [-computername <Name>] 
[{-?|/?}] [{-help|-h}]
```
|Paramètre|Description|
|-------|--------|
|<Drive>|Représente une lettre de lecteur suivie par un signe deux-points.|
|-recoverypassword|Ajoute un protecteur de mot de passe numérique. Vous pouvez également utiliser **- rp** comme une version abrégée de cette commande.|
|<NumericalPassword>|Représente le mot de passe de récupération.|
|-recoverykey|Ajoute un protecteur de clé externe pour la récupération. Vous pouvez également utiliser **- rk** comme une version abrégée de cette commande.|
|<pathToExternalKeydirectory>|Représente le chemin d’accès à la clé de récupération.|
|-startupkey|Ajoute un protecteur de clé externe pour le démarrage. Vous pouvez également utiliser **-sk** comme une version abrégée de cette commande.|
|<pathToExternalKeydirectory>|Représente le chemin d’accès à la clé de démarrage.|
|-certificat|Ajoute un protecteur de clé publique pour un lecteur de données. Vous pouvez également utiliser **-cert** comme une version abrégée de cette commande.|
|-cf|Spécifie qu’un fichier de certificat sera être utilisé pour fournir le certificat de clé publique.|
|<pathToCertificateFile>|Représente le chemin d’accès au fichier de certificat.|
|-ct|Spécifie qu’un empreinte numérique du certificat sera utilisé pour identifier le certificat de clé publique|
|<CertificateThumbprint>|Spécifie la valeur de la propriété de l’empreinte numérique du certificat que vous souhaitez utiliser. Par exemple, une valeur d’empreinte numérique du certificat de « a9 09 50 2d d8 2 a e4 14 33 e6 f8 38 86 b0 0d 42 77 a3 2 a 7 b » doit être spécifié en tant que « a909502dd82ae41433e6f83886b00d4277a32a7b ».|
|-tpmandpin|Ajoute un Module de plateforme sécurisée (TPM) et un protecteur de (PIN) numéro d’identification personnel pour le lecteur de système d’exploitation. Vous pouvez également utiliser **- tp** comme une version abrégée de cette commande.|
|-tpmandstartupkey|Ajoute un protecteur de clé TPM et de démarrage pour le lecteur de système d’exploitation. Vous pouvez également utiliser **-tâche** comme une version abrégée de cette commande.|
|-tpmandpinandstartupkey|Ajoute un module de plateforme sécurisée, un code confidentiel et un protecteur de clé de démarrage pour le lecteur de système d’exploitation. Vous pouvez également utiliser **- tpsk** comme une version abrégée de cette commande.|
|-password|Ajoute un protecteur de clé de mot de passe pour le lecteur de données. Vous pouvez également utiliser **- pw** comme une version abrégée de cette commande.|
|-adaccountorgroup|Ajoute un identificateur de sécurité (SID)-en fonction de protecteur d’identité pour le volume.  Vous pouvez également utiliser **-sid** comme une version abrégée de cette commande. **IMPORTANT :** Par défaut, vous ne pouvez pas ajouter un protecteur de ADAccountOrGroup à distance à l’aide de WMI ou gérer-bde.  Si votre déploiement requiert la possibilité d’ajouter ce protecteur à distance, vous devez activer la délégation contrainte.|
|-computername|Spécifie que gérer-bde est utilisé pour modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **- cn** comme une version abrégée de cette commande.|
|<Name>|Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.|
### <a name="BKMK_deleteprotectors"></a>-supprimer la syntaxe et les paramètres
```
manage-bde  protectors  delete <Drive> [-type {recoverypassword|externalkey|certificate|tpm|tpmandstartupkey|tpmandpin|tpmandpinandstartupkey|Password|Identity}] 
[-id <KeyProtectorID>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```
|Paramètre|Description|
|-------|--------|
|<Drive>|Représente une lettre de lecteur suivie par un signe deux-points.|
|-type|Identifie le protecteur de clé à supprimer. Vous pouvez également utiliser **-t** comme une version abrégée de cette commande.|
|RecoveryPassword|Spécifie que les protecteurs de clé de mot de passe de récupération doivent être supprimés.|
|ExternalKey|Spécifie que les protecteurs de clé externes associées au lecteur doivent être supprimés.|
|certificate|Spécifie que les protecteurs de clé de certificat associés au lecteur doivent être supprimés.|
|tpm|Spécifie que les protecteurs de clé TPM uniquement associés au lecteur doivent être supprimés.|
|TPMAndStartupKey|Spécifie que n’importe quel module de plateforme sécurisée et de démarrage basé sur clé protecteurs de clé associées au lecteur doivent être supprimés.|
|TPMAndPIN|Spécifie que n’importe quel module de plateforme sécurisée et un code confidentiel basée les protecteurs de clé associées au lecteur doit être supprimée.|
|tpmandpinandstartupkey|Spécifie que n’importe quel module de plateforme sécurisée, code confidentiel et le démarrage basé sur clé protecteurs de clé associées au lecteur doivent être supprimés.|
|password|Spécifie que les protecteurs de clé de mot de passe associés au lecteur doivent être supprimés.|
|identité|Spécifie que les protecteurs de clé identité associées au lecteur doivent être supprimés.|
|-id|Identifie le protecteur de clé à supprimer à l’aide de l’identificateur de clé. Ce paramètre est une alternative à la **-type** paramètre.|
|<KeyProtectorID>|Identifie un protecteur de clé individuel sur le disque à supprimer. Protecteur de clé ID peut être affiché à l’aide de la **gérer-bde-protecteurs-obtenir** commande.|
|-computername|Spécifie que bde.exe gérer permet de modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **- cn** comme une version abrégée de cette commande.|
|<Name>|Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.|
|-? ou /?|Affiche un résumé aide à l’invite de commandes.|
|-help ou-h|Affiche une aide à l’invite de commande complète.|
### <a name="BKMK_disableprot"></a>-désactiver la syntaxe et les paramètres
```
manage-bde  protectors  disable <Drive> [-RebootCount <integer 0 - 15>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```
|Paramètre|Description|
|-------|--------|
|<Drive>|Représente une lettre de lecteur suivie par un signe deux-points.|
|RebootCount|En commençant par Windows 8, spécifie que la protection du volume de système d’exploitation a été interrompue et reprendra une fois que Windows a été redémarré le nombre de fois spécifié dans le paramètre RebootCount. Spécifiez 0 pour suspendre la protection indéfiniment. Si ce paramètre n’est pas spécifié la protection BitLocker sera automatiquement reprendre lors du redémarrage de Windows. Vous pouvez également utiliser **-rc** comme une version abrégée de cette commande.|
|-computername|Spécifie que bde.exe gérer permet de modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **- cn** comme une version abrégée de cette commande.|
|<Name>|Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.|
|-? ou /?|Affiche un résumé aide à l’invite de commandes.|
|-help ou-h|Affiche une aide à l’invite de commande complète.|
## <a name="BKMK_Examples"></a>Exemples
L’exemple suivant illustre l’utilisation de la **-protecteurs** commande pour ajouter un protecteur de clé de certificat identifié par un fichier de certificat pour le lecteur E.
```
manage-bde  protectors  add E: -certificate  cf "c:\File Folder\Filename.cer"
```
L’exemple suivant illustre l’utilisation de la **-protecteurs** commande pour ajouter un **adaccountorgroup** protecteur de clé identifié par nom de domaine et d’utilisateur pour le lecteur E.
```
manage-bde  protectors  add E: -sid DOMAIN\user
```
L’exemple suivant illustre l’utilisation de la ** ** protecteurs de commande pour désactiver la protection tant que l’ordinateur a redémarré de 3 fois.
```
manage-bde  protectors  disable C: -rc 3
```
L’exemple suivant illustre l’utilisation de la **-protecteurs** commande pour supprimer tous les TPM et clé de démarrage en fonction des protecteurs de clé sur le lecteur C.
```
manage-bde  protectors  delete C: -type tpmandstartupkey
```
L’exemple suivant illustre l’utilisation de la **-protecteurs** commande pour sauvegarder toutes les informations de récupération pour le lecteur C dans les services AD DS.
```
manage-bde  protectors  adbackup C:
```
## <a name="additional-references"></a>Références supplémentaires
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [manage-bde](manage-bde.md)
