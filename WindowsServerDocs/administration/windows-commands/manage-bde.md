---
title: manage-bde
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8923177b03f378f8252c532ec386f1808e516e1e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874500"
---
# <a name="manage-bde"></a>manage-bde



Utilisé pour activer ou désactiver BitLocker, spécifier mécanismes de déverrouillage, mettre à jour les méthodes de récupération et déverrouiller des lecteurs de données protégés par BitLocker. Cet outil de ligne de commande peut être utilisé à la place de la **BitLocker Drive Encryption** élément du panneau. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
manage-bde [-status] [–on] [–off] [–pause] [–resume] [–lock] [–unlock] [–autounlock] [–protectors] [–tpm] 
[–SetIdentifier] [-ForceRecovery] [–changepassword] [–changepin] [–changekey] [-KeyPackage] [–upgrade] [-WipeFreeSpace] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[gérer-bde : état](manage-bde-status.md)|Fournit des informations sur tous les lecteurs sur l’ordinateur, qu’ils soient protégés par BitLocker ou non.|
|[gérer-bde : sur](manage-bde-on.md)|Chiffre le lecteur et active BitLocker.|
|[gérer-bde : désactivé](manage-bde-off.md)|Déchiffre le disque et comment désactiver BitLocker. Tous les protecteurs de clé sont supprimés lorsque le déchiffrement est terminé.|
|[gérer-bde : suspendre](manage-bde-pause.md)|Suspend le chiffrement ou déchiffrement.|
|[gérer-bde : reprendre](manage-bde-resume.md)|Reprend le chiffrement ou déchiffrement.|
|[gérer-bde : verrouillage](manage-bde-lock.md)|Empêche l’accès aux données protégées par BitLocker.|
|[gérer-bde : déverrouiller](manage-bde-unlock.md)|Autorise l’accès aux données protégées par BitLocker avec un mot de passe de récupération ou une clé de récupération.|
|[gérer-bde : autounlock](manage-bde-autounlock.md)|Gère le déverrouillage automatique de lecteurs de données.|
|[gérer-bde : protecteurs](manage-bde-protectors.md)|Gère les méthodes de protection pour la clé de chiffrement.|
|[gérer-bde : module de plateforme sécurisée](manage-bde-tpm.md)|Configure le Module de plateforme sécurisée (TPM) de l’ordinateur. Cette commande n’est pas pris en charge sur les ordinateurs exécutant Windows 8 ou **win8_server_2**. Pour gérer le module de plateforme sécurisée sur ces ordinateurs, utilisez le composant logiciel enfichable MMC de gestion de module de plateforme sécurisée ou les applets de commande de gestion du module de plateforme sécurisée pour Windows PowerShell.|
|[gérer-bde : setidentifier](manage-bde-setidentifier.md)|Définit le champ d’identificateur de lecteur sur le lecteur à la valeur spécifiée dans le **fournir les identificateurs uniques pour votre organisation** paramètre de stratégie de groupe.|
|[gérer-bde : ForceRecovery](manage-bde-forcerecovery.md)|Force un lecteur protégé par BitLocker en mode de récupération lors du redémarrage. Cette commande supprime tous les protecteurs de clés liés à TPM à partir du lecteur. Lorsque l’ordinateur redémarre, seul un mot de passe de récupération ou une clé de récupération peut être utilisée pour déverrouiller le lecteur.|
|[gérer-bde : changepassword](manage-bde-changepassword.md)|Modifie le mot de passe pour un lecteur de données.|
|[gérer-bde : changepin met](manage-bde-changepin.md)|Modifie le code confidentiel pour un lecteur de système d’exploitation.|
|[gérer-bde : changekey](manage-bde-changekey.md)|Modifie la clé de démarrage pour un lecteur de système d’exploitation.|
|[gérer-bde : KeyPackage](manage-bde-keypackage.md)|Génère un package de clés pour un lecteur.|
|[gérer-bde : mise à niveau](manage-bde-upgrade.md)|Met à niveau la version de BitLocker.|
|[gérer-bde : WipeFreeSpace](manage-bde-wipefreespace.md)|Effacer le contenu de l’espace libre sur un lecteur.|
|-? ou /?|Affiche un résumé aide à l’invite de commandes.|
|-help ou-h|Affiche une aide complète à l’invite de commandes.|

## <a name="BKMK_Examples"></a>Exemples

L’exemple suivant affiche les lecteurs sur l’ordinateur et identifie qu’ils soient ou non protégés par BitLocker et l’état de chiffrement.
```
manage-bde -status
```
L’exemple suivant illustre l’activation de BitLocker sur le lecteur C avec l’option d’un mot de passe de récupération. Le mot de passe de récupération sera généré par BitLocker et affichée à l’écran afin que vous pouvez l’enregistrer.
```
manage-bde –on C: -recoverypassword
```
L’exemple suivant illustre le déverrouillage d’un lecteur protégé par BitLocker à l’aide d’un mot de passe de récupération.
```
manage-bde –unlock E: -recoverypassword 111111-222222-333333-444444-555555-666666-777777-888888
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [L’activation de BitLocker à l’aide de la ligne de commande](https://technet.microsoft.com/library/dd894351(v=ws.10).aspx)
