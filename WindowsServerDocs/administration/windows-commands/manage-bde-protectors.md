---
title: gérer-protecteurs BDE
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f9b22c5-cc93-45df-9165-bedee94998da
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/06/2018
ms.openlocfilehash: 1a2e2c851ec9bc93ec434a35f14c6f92ec831876
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839952"
---
# <a name="manage-bde-protectors"></a>Manage-bde : protecteurs

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Gère les méthodes de protection utilisées pour la clé de chiffrement BitLocker. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).
## <a name="syntax"></a>Syntaxe
```
manage-bde -protectors [{-get|-add|-delete|-disable|-enable|-adbackup|-aadbackup}] <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]
```
#### <a name="parameters"></a>Paramètres

|   Paramètre   |                                                                                                                                                                                           Description                                                                                                                                                                                            |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     -Obtient      |                                                                                                                                            Affiche toutes les méthodes de protection de clé activées sur le lecteur et fournit leur type et leur identificateur (ID).                                                                                                                                             |
|     -Ajouter      |                                                                                                                                   Ajoute des méthodes de protection de clé comme spécifié à l’aide de [paramètres d’ajout](manage-bde-protectors.md#BKMK_addprotectors)supplémentaires.                                                                                                                                    |
|    -delete    | supprime les méthodes de protection de clé utilisées par BitLocker. Tous les protecteurs de clé seront supprimés d’un lecteur, sauf si les paramètres facultatifs [-Delete](manage-bde-protectors.md#BKMK_deleteprotectors) sont utilisés pour spécifier les protecteurs à supprimer. Lorsque le dernier protecteur sur un lecteur est supprimé, la protection BitLocker du lecteur est désactivée pour garantir que l’accès aux données n’est pas perdu par inadvertance. |
|   -disable    |                      Désactive la protection, ce qui permet à quiconque d’accéder aux données chiffrées en rendant la clé de chiffrement disponible non sécurisée sur le lecteur. Aucun protecteur de clé n’est supprimé. La protection sera reprise lors du démarrage suivant de Windows, sauf si les [paramètres](manage-bde-protectors.md#BKMK_disableprot) facultatifs sont utilisés pour spécifier le nombre de redémarrages.                       |
|    -enable    |                                                                                                                             Active la protection en supprimant la clé de chiffrement non sécurisée du lecteur. Tous les protecteurs de clé configurés sur le lecteur seront appliqués.                                                                                                                             |
|   -adbackup   |                                                                          Sauvegarde toutes les informations de récupération pour le lecteur spécifié à Active Directory Domain Services (AD DS). Pour sauvegarder une seule clé de récupération à AD DS, ajoutez le paramètre **-ID** et spécifiez l’ID d’une clé de récupération spécifique à sauvegarder.                                                                           |
|  -aadbackup   |                                                                            Sauvegarde toutes les informations de récupération du lecteur spécifié pour Azure Active Directory (Azure AD). Pour sauvegarder une seule clé de récupération à Azure AD, ajoutez le paramètre **-ID** et spécifiez l’ID d’une clé de récupération spécifique à sauvegarder.                                                                             |
|    <Drive>    |                                                                                                                                                                          Représente une lettre de lecteur suivie par un signe deux-points.                                                                                                                                                                          |
| -ComputerName |                                                                                                              Spécifie que Manage-bde. exe sera utilisé pour modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **-CN** comme version abrégée de cette commande.                                                                                                              |
|    <Name>     |                                                                                                                 Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Les valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.                                                                                                                  |
|   -? ou /?    |                                                                                                                                                                            Affiche une brève aide à l’invite de commandes.                                                                                                                                                                            |
|  -Help ou-h  |                                                                                                                                                                          Affiche l’aide complète à l’invite de commandes.                                                                                                                                                                           |

### <a name="-add-syntax-and-parameters"></a><a name=BKMK_addprotectors></a>-Ajouter une syntaxe et des paramètres
```
manage-bde  -protectors  -add [<Drive>] [-forceupgrade] [-recoverypassword <NumericalPassword>] [-recoverykey <pathToExternalKeydirectory>]
[-startupkey <pathToExternalKeydirectory>] [-certificate {-cf <pathToCertificateFile>|-ct <CertificateThumbprint>}] [-tpm] [-tpmandpin] 
[-tpmandstartupkey <pathToExternalKeydirectory>] [-tpmandpinandstartupkey <pathToExternalKeydirectory>] [-password][-adaccountorgroup <securityidentifier> [-computername <Name>] 
[{-?|/?}] [{-help|-h}]
```

|          Paramètre           |                                                                                                                                                                                   Description                                                                                                                                                                                   |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           <Drive>            |                                                                                                                                                                 Représente une lettre de lecteur suivie par un signe deux-points.                                                                                                                                                                  |
|      -RecoveryPassword       |                                                                                                                                    Ajoute un protecteur de mot de passe numérique. Vous pouvez également utiliser **-RP** comme version abrégée de cette commande.                                                                                                                                     |
|     <NumericalPassword>      |                                                                                                                                                                        Représente le mot de passe de récupération.                                                                                                                                                                        |
|         -recoverykey         |                                                                                                                                Ajoute un protecteur de clé externe pour la récupération. Vous pouvez également utiliser **-RK** comme version abrégée de cette commande.                                                                                                                                 |
| <pathToExternalKeydirectory> |                                                                                                                                                               Représente le chemin d’accès du répertoire à la clé de récupération.                                                                                                                                                                |
|         -clé          |                                                                                                                                 Ajoute un protecteur de clé externe pour le démarrage. Vous pouvez également utiliser **-SK** comme version abrégée de cette commande.                                                                                                                                 |
| <pathToExternalKeydirectory> |                                                                                                                                                                Représente le chemin d’accès au répertoire de la clé de démarrage.                                                                                                                                                                |
|         -certificat         |                                                                                                                               Ajoute un protecteur de clé publique pour un lecteur de données. Vous pouvez également utiliser **-CERT** comme version abrégée de cette commande.                                                                                                                               |
|             -cf              |                                                                                                                                              Spécifie qu’un fichier de certificat sera utilisé pour fournir le certificat de clé publique.                                                                                                                                              |
|   <pathToCertificateFile>    |                                                                                                                                                             Représente le chemin d’accès au répertoire du fichier de certificat.                                                                                                                                                              |
|             -CT              |                                                                                                                                           Spécifie qu’une empreinte de certificat sera utilisée pour identifier le certificat de clé publique                                                                                                                                           |
|   <CertificateThumbprint>    |                                                       Spécifie la valeur de la propriété empreinte numérique du certificat que vous souhaitez utiliser. Par exemple, la valeur de l’empreinte numérique de certificat a9 09 50 2d D8 2A E4 14 33 e6 f8 38 86 b0 0d 42 77 a3 2A 7B doit être spécifiée en tant que a909502dd82ae41433e6f83886b00d4277a32a7b.                                                        |
|          -tpmandpin          |                                                                                           Ajoute une Module de plateforme sécurisée (TPM) (TPM) et un protecteur de code confidentiel (PIN) pour le lecteur du système d’exploitation. Vous pouvez également utiliser **-TP** comme version abrégée de cette commande.                                                                                           |
|      -tpmandstartupkey       |                                                                                                                    Ajoute un module de plateforme sécurisée et un protecteur de clé de démarrage pour le lecteur de système d’exploitation. Vous pouvez également utiliser **-tsk** comme version abrégée de cette commande.                                                                                                                    |
|   -tpmandpinandstartupkey    |                                                                                                                Ajoute un protecteur de module de plateforme sécurisée, de code confidentiel et de clé de démarrage pour le lecteur de système d’exploitation. Vous pouvez également utiliser **-tpsk** comme version abrégée de cette commande.                                                                                                                 |
|          -password           |                                                                                                                              Ajoute un protecteur de clé de mot de passe pour le lecteur de données. Vous pouvez également utiliser **-PW** comme version abrégée de cette commande.                                                                                                                              |
|      -adaccountorgroup       | Ajoute un protecteur d’identité basé sur un identificateur de sécurité (SID) pour le volume.  Vous pouvez également utiliser **-sid** comme version abrégée de cette commande. **Important :** Par défaut, vous ne pouvez pas ajouter un protecteur ADAccountOrGroup à distance à l’aide de WMI ou de Manage-bde.  Si votre déploiement nécessite la possibilité d’ajouter ce protecteur à distance, vous devez activer la délégation avec restriction. |
|        -ComputerName         |                                                                                                       Spécifie que Manage-bde est utilisé pour modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **-CN** comme version abrégée de cette commande.                                                                                                       |
|            <Name>            |                                                                                                         Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Les valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.                                                                                                         |

### <a name="-delete-syntax-and-parameters"></a><a name=BKMK_deleteprotectors></a>-supprimer la syntaxe et les paramètres
```
manage-bde  -protectors  -delete <Drive> [-type {recoverypassword|externalkey|certificate|tpm|tpmandstartupkey|tpmandpin|tpmandpinandstartupkey|Password|Identity}] 
[-id <KeyProtectorID>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

|       Paramètre        |                                                                              Description                                                                               |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        <Drive>         |                                                             Représente une lettre de lecteur suivie par un signe deux-points.                                                             |
|         -type          |                               Identifie le protecteur de clé à supprimer. Vous pouvez également utiliser **-t** comme version abrégée de cette commande.                               |
|    RecoveryPassword    |                                                 Spécifie que tous les protecteurs de clé de mot de passe de récupération doivent être supprimés.                                                 |
|      externalkey       |                                        Spécifie que tous les protecteurs de clé externe associés au lecteur doivent être supprimés.                                         |
|      certificate       |                                       Spécifie que tous les protecteurs de clé de certificat associés au lecteur doivent être supprimés.                                       |
|          TPM           |                                        Spécifie que tous les protecteurs de clés TPM uniquement associés au lecteur doivent être supprimés.                                         |
|    tpmandstartupkey    |                                Spécifie que tous les protecteurs de clés de module de plateforme sécurisée et de clé de démarrage associés au lecteur doivent être supprimés.                                |
|       tpmandpin        |                                    Spécifie que tous les protecteurs de clés TPM et PIN associés au lecteur doivent être supprimés.                                    |
| tpmandpinandstartupkey |                             Spécifie que tous les protecteurs de clés de module de plateforme sécurisée, de code confidentiel et de clé de démarrage associés au lecteur doivent être supprimés.                             |
|        mot de passe        |                                        Spécifie que tous les protecteurs de clé de mot de passe associés au lecteur doivent être supprimés.                                         |
|        identité        |                                        Spécifie que tous les protecteurs de clés d’identité associés au lecteur doivent être supprimés.                                         |
|          -ID           |                Identifie le protecteur de clé à supprimer à l’aide de l’identificateur de clé. Ce paramètre est une autre option pour le paramètre **-type** .                 |
|    <KeyProtectorID>    |        Identifie un protecteur de clé individuel sur le lecteur à supprimer. Les ID de protecteur de clé peuvent être affichés à l’aide de la commande **Manage-bde-protectors-obten** .         |
|     -ComputerName      | Spécifie que Manage-bde. exe sera utilisé pour modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **-CN** comme version abrégée de cette commande. |
|         <Name>         |    Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Les valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.     |
|        -? ou /?        |                                                               Affiche une brève aide à l’invite de commandes.                                                               |
|      -Help ou-h       |                                                             Affiche l’aide complète à l’invite de commandes.                                                              |

### <a name="-disable-syntax-and-parameters"></a><a name=BKMK_disableprot></a>-désactiver la syntaxe et les paramètres
```
manage-bde  -protectors  -disable <Drive> [-RebootCount <integer 0 - 15>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

|   Paramètre   |                                                                                                                                                                                                                   Description                                                                                                                                                                                                                    |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <Drive>    |                                                                                                                                                                                                  Représente une lettre de lecteur suivie par un signe deux-points.                                                                                                                                                                                                  |
|  RebootCount  | À compter de Windows 8, spécifie que la protection du volume du système d’exploitation a été interrompue et reprendra après le redémarrage de Windows le nombre de fois spécifié dans le paramètre RebootCount. Spécifiez 0 pour suspendre la protection indéfiniment. Si ce paramètre n’est pas spécifié, la protection BitLocker reprend automatiquement lorsque Windows est redémarré. Vous pouvez également utiliser **-RC** comme version abrégée de cette commande. |
| -ComputerName |                                                                                                                                      Spécifie que Manage-bde. exe sera utilisé pour modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **-CN** comme version abrégée de cette commande.                                                                                                                                      |
|    <Name>     |                                                                                                                                         Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Les valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.                                                                                                                                          |
|   -? ou /?    |                                                                                                                                                                                                    Affiche une brève aide à l’invite de commandes.                                                                                                                                                                                                    |
|  -Help ou-h  |                                                                                                                                                                                                  Affiche l’aide complète à l’invite de commandes.                                                                                                                                                                                                   |

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre
L’exemple suivant illustre l’utilisation de la commande **-protections** pour ajouter un protecteur de clé de certificat identifié par un fichier de certificat au lecteur E.
```
manage-bde  -protectors  -add E: -certificate  -cf c:\File Folder\Filename.cer
```
L’exemple suivant illustre l’utilisation de la commande **-protections** pour ajouter un protecteur de clé **adaccountorgroup** identifié par le nom de domaine et le nom d’utilisateur au lecteur E.
```
manage-bde  -protectors  -add E: -sid DOMAIN\user
```
L’exemple suivant illustre l’utilisation de la commande **Protectors** pour désactiver la protection jusqu’à ce que l’ordinateur ait redémarré 3 fois.
```
manage-bde  -protectors  -disable C: -rc 3
```
L’exemple suivant illustre l’utilisation de la commande **-Protectors** pour supprimer tous les protecteurs de clés TPM et de clé de démarrage sur le lecteur C.
```
manage-bde  -protectors -delete C: -type tpmandstartupkey
```
L’exemple suivant illustre l’utilisation de la commande **-Protectors** pour sauvegarder toutes les informations de récupération du lecteur C sur AD DS.
```
manage-bde  -protectors  -adbackup C:
```
## <a name="additional-references"></a>Références supplémentaires
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [manage-bde](manage-bde.md)
