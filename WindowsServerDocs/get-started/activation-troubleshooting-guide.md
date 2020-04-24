---
title: Résolution des problèmes d’activation en volume Windows
description: Liste des ressources qui fournissent des informations sur les bonnes pratiques en matière d’activation en volume, ainsi que des informations sur la résolution des problèmes d’activation.
ms.topic: troubleshooting
ms.date: 09/24/2019
ms.technology: server-general
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: 09206b90dc8b829aaa70d0cca34bd05a9e0eb693
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "71963045"
---
# <a name="troubleshooting-windows-volume-activation"></a>Résolution des problèmes d’activation en volume Windows

L’activation du produit est le processus qui consiste à valider le logiciel après son installation sur un ordinateur spécifique. L’activation confirme que le produit est authentique (qu’il ne s’agit pas d’une copie frauduleuse), et que la clé de produit ou le numéro de série est valide et n’a pas été compromis ou révoqué. L’activation établit également un lien ou une relation entre la clé de produit et l’installation.

L’activation en volume est le processus qui consiste à activer des produits avec licence en volume. Pour devenir un client du programme de licence en volume, une organisation doit mettre en place un contrat de licence en volume avec Microsoft. Microsoft propose des programmes de licence en volume personnalisés adaptés à la taille et aux préférences d’achat de l’organisation. Pour plus d’informations, consultez le [Centre de gestion des licences en volume Microsoft](https://www.microsoft.com/Licensing/servicecenter/default.aspx).

Le [Guide d’activation de Windows Server 2016](server-2016-activation.md) se concentre sur la technologie d’activation du service de gestion de clés (KMS, Key Management Service). Cette section traite des problèmes courants et fournit des instructions de résolution des problèmes pour le service KMS et plusieurs autres technologies d’activation en volume.

## <a name="best-practices-for-volume-activation"></a>Bonnes pratiques en matière d’activation en volume

Les articles suivants fournissent des informations techniques et des bonnes pratiques pour les technologies d’activation en volume de Microsoft.

### <a name="key-management-service-kms"></a>Service de gestion de clés (KMS)

- [Planifier l’activation en volume](https://docs.microsoft.com/windows/deployment/volume-activation/plan-for-volume-activation-client)
- [Présentation du service KMS](https://docs.microsoft.com/previous-versions/tn-archive/ff793434(v=technet.10))
- [Déploiement de l’activation KMS](https://docs.microsoft.com/previous-versions/tn-archive/ff793409%28v=technet.10%29)
- [Configuration des hôtes KMS](https://docs.microsoft.com/previous-versions/tn-archive/ff793407%28v%3dtechnet.10%29)
- [Configuration du système DNS](https://docs.microsoft.com/previous-versions/tn-archive/ff793405%28v%3dtechnet.10%29)
- [Activer à l’aide du service de gestion de clés](https://docs.microsoft.com/windows/deployment/volume-activation/activate-using-key-management-service-vamt)

### <a name="active-directory-based-activation-adba"></a>Activation basée sur Active Directory

- [Déployer l’activation basée sur Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn502534%28v%3Dws.11%29)
- [Effectuer une activation basée sur Active Directory](https://docs.microsoft.com/windows/deployment/volume-activation/activate-using-active-directory-based-activation-client)
- [Vue d’ensemble de l’activation basée sur Active Directory](https://docs.microsoft.com/windows/deployment/volume-activation/active-directory-based-activation-overview)

### <a name="multiple-activation-key-mak-activation"></a>Activation basée sur une clé d’activation multiple (MAK)

- [Utilisation de l’activation MAK](https://docs.microsoft.com/previous-versions/tn-archive/ff793438%28v=technet.10%29)
- [Présentation de l’activation MAK](https://docs.microsoft.com/previous-versions/tn-archive/ff793435%28v%3dtechnet.10%29)
- [Activation des clients MAK](https://docs.microsoft.com/previous-versions/tn-archive/ff793398%28v%3dtechnet.10%29)

### <a name="subscription-activation"></a>Activation d’abonnement

- [Activation d’abonnement Windows 10](https://docs.microsoft.com/windows/deployment/windows-10-subscription-activation)
- [Déployer des licences Windows 10 Entreprise](https://docs.microsoft.com/windows/deployment/deploy-enterprise-licenses)
- [Windows 10 Entreprise E3 dans le programme Fournisseur de solutions Cloud](https://docs.microsoft.com/windows/deployment/windows-10-enterprise-e3-overview)

## <a name="resources-for-troubleshooting-activation-issues"></a>Ressources pour la résolution des problèmes d’activation

Les articles suivants fournissent des instructions et des informations sur les outils permettant de résoudre les problèmes d’activation en volume :

- [Instructions pour le dépannage du service de gestion de clés (KMS)](activation-troubleshoot-kms-general.md)
- [Options de Slmgr.vbs pour obtenir des informations sur l’activation en volume](activation-slmgr-vbs-options.md)
- [Exemple : Résolution des problèmes liés aux clients ADBA qui ne s’activent pas](activation-troubleshoot-adba-clients.md)

Les articles suivants fournissent des conseils pour résoudre des problèmes d’activation plus spécifiques :

- [Résolution des codes d’erreur d’activation courants](activation-error-codes.md)
- [Activation KMS : problèmes connus](activation-troubleshoot-KMS-issues.md)
- [Activation MAK : problèmes connus](activation-troubleshoot-MAK-issues.md)
- [Instructions pour la résolution des problèmes d’activation liés à DNS](common-troubleshooting-procedures-kms-dns.md)
- [Guide pratique pour regénérer le fichier Tokens.dat](activation-rebuild-tokens-dat-file.md)
