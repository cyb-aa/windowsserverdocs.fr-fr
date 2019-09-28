---
title: Résoudre les problèmes de machines virtuelles protégées
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 10/3/2018
ms.openlocfilehash: b0d4338d20238eb528c19221151f380cc154a2db
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386367"
---
# <a name="troubleshoot-shielded-vms"></a>Résoudre les problèmes de machines virtuelles protégées

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

À compter de Windows Server version 1803, le mode de session étendu VMConnect (Virtual Machine Connection) et PS direct sont réactivés pour les machines virtuelles entièrement protégées. L’administrateur de virtualisation requiert encore des informations d’identification invité de machine virtuelle pour accéder à la machine virtuelle, mais cela permet à un hébergeur de dépanner plus facilement une machine virtuelle protégée lorsque sa configuration réseau est interrompue.

Pour activer VMConnect et PS direct pour vos machines virtuelles protégées, il vous suffit de les déplacer vers un ordinateur hôte Hyper-V qui exécute Windows Server version 1803 ou ultérieure. Les appareils virtuels qui autorisent ces fonctionnalités seront réactivés automatiquement. Si une machine virtuelle protégée est déplacée vers un hôte qui exécute et une version antérieure de Windows Server, VMConnect et PS direct sont à nouveau désactivés.

Pour les clients sensibles à la sécurité qui se soucient si les hébergeurs ont accès à la machine virtuelle et souhaitent revenir au comportement d’origine, les fonctionnalités suivantes doivent être désactivées dans le système d’exploitation invité :

- Désactivez le service PowerShell direct sur la machine virtuelle :

  ```powershell
  Stop-Service vmicvmsession
  Set-Service vmicvmsession -StartupType Disabled
  ```

- Le mode de session étendu VMConnect peut uniquement être désactivé si votre système d’exploitation invité est au moins Windows Server 2019 ou Windows 10, version 1809. Ajoutez la clé de Registre suivante à votre machine virtuelle pour désactiver les connexions à la console de session étendue VMConnect :

  ```
  reg add "HKLM\Software\Microsoft\Virtual Machine\Guest" /v DisableEnhancedSessionConsoleConnection /t REG_DWORD /d 1
  ```
