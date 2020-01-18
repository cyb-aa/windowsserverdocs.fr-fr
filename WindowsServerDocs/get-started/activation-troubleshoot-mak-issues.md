---
title: Problèmes connus de l’activation MAK
description: Décrit les problèmes courants qui peuvent se produire pendant le processus d’activation MAK (Clé d’activation multiple) et fournit des solutions et des conseils
ms.topic: article
ms.date: 10/3/2019
ms.technology: server-general
ms.assetid: ''
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: 5ed934e52b11db5b83948ba995eaf31e9ff2cce3
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75947625"
---
# <a name="mak-activation-known-issues"></a>Activation MAK : problèmes connus

Cet article décrit les problèmes courants qui peuvent se produire lors des activations MAK, et fournit des conseils pour résoudre ces problèmes.

## <a name="how-can-i-tell-whether-my-computer-is-activated"></a>Comment puis-je savoir si mon ordinateur est activé ?

Sur l’ordinateur, ouvrez le Panneau de configuration **Système** et recherchez **Windows est activé**. Vous pouvez aussi exécuter Slmgr.vbs et utiliser l’option de ligne de commande **/dli**.

## <a name="the-computer-does-not-activate-over-the-internet"></a>L’ordinateur ne s’active pas via Internet

Vérifiez que les ports nécessaires sont ouverts dans le pare-feu. Pour obtenir la liste des ports, consultez le [Guide de déploiement d’activation en volume](https://go.microsoft.com/fwlink/?linkid=150083).

## <a name="internet-and-telephone-activation-fail"></a>Échec de l’activation par Internet et par téléphone

Contactez un Centre d’activation Microsoft local. Pour les numéros de téléphone des centres d’activation Microsoft dans le monde, accédez à [Numéros de téléphone internationaux des Centres d’activation des licences Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers). Veillez à fournir les informations relatives au contrat de licence en volume et la preuve d’achat quand vous appelez.

## <a name="slmgrvbs-ato-returns-an-error-code"></a>Slmgr.vbs /ato retourne un code d’erreur

Si Slmgr.vbs retourne un code d’erreur hexadécimal, déterminez le message d’erreur correspondant en exécutant le script suivant :

```cmd
slui.exe 0x2a 0x <ErrorCode>
```

Pour plus d’informations sur les codes d’erreur spécifiques et sur la manière de les résoudre, consultez [Résolution des codes d’erreur d’activation courants](activation-error-codes.md).
