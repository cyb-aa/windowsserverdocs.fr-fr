---
title: La connexion TCP est abandonnée pendant la négociation de validation
description: Explique comment dépanner le problème SMB lorsque la connexion TCP est abandonnée lors de l’instruction Validate Negotiate.
author: Deland-Han
manager: dcscontentpm
audience: ITPro
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 3455b4ac0a2706f80702378dda02c1877af219ca
ms.sourcegitcommit: 8cf04db0bc44fd98f4321dca334e38c6573fae6c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75654620"
---
# <a name="tcp-connection-is-aborted-during-validate-negotiate"></a>La connexion TCP est abandonnée pendant la négociation de validation

Dans le suivi réseau pour le problème SMB, vous remarquez qu’une annulation de réinitialisation TCP s’est produite pendant le processus de validation de négociation. Cet article explique comment résoudre le problème.

## <a name="cause"></a>Cause

Ce problème peut être dû à un échec de validation de la négociation. Cela se produit généralement lorsqu’un accélérateur WAN modifie le paquet d’origine SMB NEGOTIATE.

Microsoft n’autorise plus la modification du paquet de négociation de la validation pour une raison quelconque. Cela est dû au fait que ce comportement crée un risque sérieux pour la sécurité.

Les exigences suivantes s’appliquent au paquet Validate Negotiate :

- Le processus de validation de négociation utilise la commande FSCTL\_VALIDATe\_NEGOTIATE\_INFO.

- La réponse de validation de négociation doit être signée. Dans le cas contraire, la connexion est abandonnée.

- Vous devez comparer le FSCTL\_valider\_négocier des messages d’informations\_aux messages Negotiate pour vous assurer qu’aucun élément n’a été modifié.

## <a name="workaround"></a>Solution

Vous pouvez désactiver temporairement le processus de validation de négociation. Pour ce faire, recherchez la sous-clé de Registre suivante :

**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\services\\les paramètres\\LanmanWorkstation**

Sous la clé **Parameters** , affectez à **RequireSecureNegotiate** la valeur **0**.

Dans Windows PowerShell, vous pouvez exécuter la commande suivante pour définir cette valeur :

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" RequireSecureNegotiate -Value 0 -Force
```

> [!NOTE]
> Le processus de validation de négociation ne peut pas être désactivé dans Windows 10, Windows Server 2016 ou les versions ultérieures de Windows.

Si le client ou le serveur ne peut pas prendre en charge la commande valider Negotiate, vous pouvez contourner ce problème en définissant la signature SMB sur obligatoire. La signature SMB est considérée comme plus sécurisée que valider Negotiate. Toutefois, il peut également y avoir une dégradation des performances si la signature est requise.

## <a name="reference"></a>Référence

Pour plus d’informations, consultez les articles suivants :

[3.3.5.15.12 gestion d’une demande Validate info Negotiate](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/0b7803eb-d561-48a4-8654-327803f59ec6)

[3.2.5.14.12 gestion d’une réponse Validate Negotiate info](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/6a5bc90d-3c08-4498-905b-e7dab30b2e0e)
