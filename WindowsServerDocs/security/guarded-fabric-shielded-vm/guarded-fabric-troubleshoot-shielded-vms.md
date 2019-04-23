---
title: Résoudre les problèmes de machines virtuelles protégées
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 10/3/2018
ms.openlocfilehash: 13ff0dad1519d394ce74a91efbfcc9e2f237e4a5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850030"
---
# <a name="troubleshoot-shielded-vms"></a>Résoudre les problèmes de machines virtuelles protégées

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

À compter de Windows Server version 1803, mode de session étendu de connexion à une Machine virtuelle (VMConnect) et PS Direct sont réactivés pour les machines virtuelles entièrement protégées. L’administrateur de la virtualisation nécessite toujours des informations d’identification d’ordinateur virtuel invité pour accéder à la machine virtuelle, mais cela rend plus facile pour un hébergeur résoudre les problèmes d’une machine virtuelle protégée lors de sa configuration de réseau est interrompue.

Pour activer VMConnect et PS Direct pour vos machines virtuelles protégées, simplement les déplacer vers un hôte Hyper-V qui exécute Windows Server version 1803 ou version ultérieure. Les appareils virtuels permettant à ces fonctionnalités seront réactivés automatiquement. Si une machine virtuelle protégée se déplace vers un hôte qui s’exécute et d’une version antérieure de Windows Server, VMConnect et PS directe seront de nouveau désactivé.

Pour les clients sensibles à la sécurité qui vous inquiétez pas si les hébergeurs aient accès à la machine virtuelle et à retourner au comportement d’origine, les fonctionnalités suivantes doivent être désactivées dans le système d’exploitation invité :

- Désactiver le service de PowerShell Direct dans la machine virtuelle :

  ```powershell
  Stop-Service vmicvmsession
  Set-Service vmicvmsession -StartupType Disabled
  ```

- Mode de Session amélioré de VMConnect ne peut être désactivé si votre système d’exploitation invité est au moins Windows Server 2019 ou Windows 10, version 1809. Ajoutez la clé de Registre suivante sur votre machine virtuelle pour désactiver les connexions de console de Session de VMConnect étendue :

  ```
  reg add "HKLM\Software\Microsoft\Virtual Machine\Guest" /v DisableEnhancedSessionConsoleConnection /t REG_DWORD /d 1
  ```
