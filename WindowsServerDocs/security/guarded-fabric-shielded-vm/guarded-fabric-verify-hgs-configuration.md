---
title: Vérifier la configuration SGH
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 8f01df37-f18e-4386-ae73-ecf84feaa9df
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 43b2a90eaa987e96b716e10259f75ffaf9f136a4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832570"
---
# <a name="verify-the-hgs-configuration"></a>Vérifier la configuration SGH

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016


Ensuite, nous devons confirmer que tout fonctionne comme prévu. Pour ce faire, exécutez la commande suivante dans une console Windows PowerShell avec élévation de privilèges :

```powershell
Get-HgsTrace -RunDiagnostics
```

Étant donné que la configuration de SGH ne contient pas encore d’informations sur les hôtes qui se trouvent dans la structure protégée, les tests de diagnostic indique qu’aucun ordinateur hôte n’est en mesure d’attester encore correctement. Ignorer ce résultat et examinez les autres informations fournies par les diagnostics.

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)] 

<!-- When a link is available for an updated troubleshooting guide, add a sentence like the following and create a link to the troubleshooting guide:
If failures did occur, please review the remediation steps provided or see the Troubleshooting Guide.
-->

Exécutez les diagnostics sur chaque nœud dans votre cluster SGH.

## <a name="next-step"></a>Étape suivante

>[!div class="nextstepaction"]
[Déployer des hôtes service Guardian](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)

