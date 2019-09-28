---
title: manage-bde
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 276a7841-7289-48d4-a57d-bc7c300affbb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7ca23e5f4499672f1e4bfcca6b9ad27f4e84039b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373766"
---
# <a name="manage-bde"></a>manage-bde



Utilisé pour activer ou désactiver BitLocker, spécifier les mécanismes de déverrouillage, mettre à jour les méthodes de récupération et déverrouiller les lecteurs de données protégés par BitLocker. Cet outil en ligne de commande peut être utilisé à la place du **chiffrement de lecteur BitLocker** élément du panneau de configuration. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
manage-bde [-status] [–on] [–off] [–pause] [–resume] [–lock] [–unlock] [–autounlock] [–protectors] [–tpm] 
[–SetIdentifier] [-ForceRecovery] [–changepassword] [–changepin] [–changekey] [-KeyPackage] [–upgrade] [-WipeFreeSpace] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[Manage-bde: status](manage-bde-status.md)|Fournit des informations sur tous les lecteurs de l’ordinateur, qu’ils soient ou non protégés par BitLocker.|
|[Manage-bde: on](manage-bde-on.md)|Chiffre le lecteur et active BitLocker.|
|[Manage-bde: off](manage-bde-off.md)|Déchiffre le lecteur et désactive BitLocker. Tous les protecteurs de clé sont supprimés lorsque le déchiffrement est terminé.|
|[Manage-bde: pause](manage-bde-pause.md)|Suspend le chiffrement ou le déchiffrement.|
|[Manage-bde: resume](manage-bde-resume.md)|Reprend le chiffrement ou le déchiffrement.|
|[Manage-bde: lock](manage-bde-lock.md)|Empêche l’accès aux données protégées par BitLocker.|
|[Manage-bde: unlock](manage-bde-unlock.md)|Autorise l’accès aux données protégées par BitLocker avec un mot de passe de récupération ou une clé de récupération.|
|[Manage-bde: autounlock](manage-bde-autounlock.md)|Gère le déverrouillage automatique des lecteurs de données.|
|[Manage-bde: protectors](manage-bde-protectors.md)|Gère les méthodes de protection pour la clé de chiffrement.|
|[Manage-bde: tpm](manage-bde-tpm.md)|Configure le Module de plateforme sécurisée (TPM) de l’ordinateur (TPM). Cette commande n’est pas prise en charge sur les ordinateurs exécutant Windows 8 ou **win8_server_2**. Pour gérer le module de plateforme sécurisée sur ces ordinateurs, utilisez le composant logiciel enfichable MMC Gestion du module de plateforme sécurisée ou les applets de commande de gestion du module de plateforme sécurisée pour Windows PowerShell.|
|[Manage-bde: setidentifier](manage-bde-setidentifier.md)|Définit le champ d’identificateur de lecteur sur le lecteur à la valeur spécifiée dans le paramètre **fournir les identificateurs uniques pour votre organisation** stratégie de groupe.|
|[Manage-bde: ForceRecovery](manage-bde-forcerecovery.md)|Force un lecteur protégé par BitLocker en mode de récupération au redémarrage. Cette commande supprime tous les protecteurs de clé liés au module de plateforme sécurisée du lecteur. Lorsque l’ordinateur redémarre, seul un mot de passe de récupération ou une clé de récupération peut être utilisé pour déverrouiller le lecteur.|
|[Manage-bde: changepassword](manage-bde-changepassword.md)|Modifie le mot de passe d’un lecteur de données.|
|[Manage-bde: changepin](manage-bde-changepin.md)|Modifie le code confidentiel d’un lecteur de système d’exploitation.|
|[Manage-bde: changekey](manage-bde-changekey.md)|Modifie la clé de démarrage pour un lecteur de système d’exploitation.|
|[Manage-bde: KeyPackage](manage-bde-keypackage.md)|Génère un package de clé pour un lecteur.|
|[Manage-bde: upgrade](manage-bde-upgrade.md)|Met à niveau la version de BitLocker.|
|[Manage-bde: WipeFreeSpace](manage-bde-wipefreespace.md)|Nettoie l’espace libre sur un lecteur.|
|-? ou /?|Affiche une brève aide à l’invite de commandes.|
|-Help ou-h|Affiche l’aide complète à l’invite de commandes.|

## <a name="BKMK_Examples"></a>Illustre

L’exemple suivant affiche les lecteurs sur l’ordinateur et indique s’ils sont protégés par BitLocker et l’état de chiffrement actuel.
```
manage-bde -status
```
L’exemple suivant illustre l’activation de BitLocker sur le lecteur C avec l’option de mot de passe de récupération. Le mot de passe de récupération sera généré par BitLocker et affiché à l’écran afin que vous puissiez l’enregistrer.
```
manage-bde –on C: -recoverypassword
```
L’exemple suivant illustre le déverrouillage d’un lecteur protégé par BitLocker à l’aide d’un mot de passe de récupération.
```
manage-bde –unlock E: -recoverypassword 111111-222222-333333-444444-555555-666666-777777-888888
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Activation de BitLocker à l’aide de la ligne de commande](https://technet.microsoft.com/library/dd894351(v=ws.10).aspx)
