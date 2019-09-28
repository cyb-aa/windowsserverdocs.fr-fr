---
title: Vérifier la configuration SGH
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 8f01df37-f18e-4386-ae73-ecf84feaa9df
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: d219b7aa9ca1e17df3281fd756106a6f07864116
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386348"
---
# <a name="verify-the-hgs-configuration"></a>Vérifier la configuration SGH

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016


Ensuite, nous devons confirmer que les choses fonctionnent comme prévu. Pour ce faire, exécutez la commande suivante dans une console Windows PowerShell avec élévation de privilèges :

```powershell
Get-HgsTrace -RunDiagnostics
```

Étant donné que la configuration SGH ne contient pas encore d’informations sur les ordinateurs hôtes qui se trouvent dans l’infrastructure protégée, les diagnostics indiquent qu’aucun hôte ne pourra encore attester. Ignorez ce résultat et passez en revue les autres informations fournies par les Diagnostics.

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)] 

Exécutez les diagnostics sur chaque nœud de votre cluster SGH.

## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Déployer des hôtes protégés](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)

