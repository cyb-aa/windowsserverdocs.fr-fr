---
title: Vue d’ensemble de Windows Server hybride Cloud Print
description: Le Cloud hybride permet aux professionnels de l’informatique de prendre en charge les exigences d’impression pour les BYOD ou les appareils joints à un domaine.
ms.prod: windows-server
ms.technology: server-general
author: trudyha
ms.author: trudyha
ms.date: 10/16/2017
ms.openlocfilehash: f448e8709f9e73165ba1a477c59567fcff4a2008
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852002"
---
# <a name="windows-server-hybrid-cloud-print-overview"></a>Vue d’ensemble de Windows Server hybride Cloud Print

**S’applique à**
-   Windows Server 2016

## <a name="what-is-hybrid-cloud-print"></a>Qu’est-ce que le Cloud hybride ?
L’impression sur le **Cloud hybride** est une nouvelle fonctionnalité de Windows Server 2016 disponible par **le biais de fonctionnalités à la demande**. Elle permet aux professionnels de l’informatique de prendre en charge les exigences d’impression pour les personnes qui apportent leurs propres appareils, ou d’utiliser des appareils joints à votre Azure Active Directory. Cela comprend les appareils mobiles tels que Windows Phone, les ordinateurs portables ou les tablettes exécutant Windows 10 ou Windows Mobile. Il assure la prise en charge de l’impression de partout où les utilisateurs ont accès à Internet.

Pour les administrateurs informatiques, le **Cloud hybride** offre un accès sécurisé aux imprimantes locales à l’aide d’Azure Multi-Factor Authentication pour valider l’accès des utilisateurs. La fonctionnalité d’authentification unique (SSO) simplifie l’expérience utilisateur. L’impression sur le **Cloud hybride** repose sur le rôle **serveur d’impression** Windows, offrant aux professionnels de l’informatique une expérience similaire à la gestion des imprimantes et de la sécurité de l’accès utilisateur.

L’impression sur le **Cloud hybride** permet aux personnes de votre organisation d’imprimer depuis les appareils qu’ils utilisent pour effectuer leur travail, même lorsqu’ils sont éloignés de leur bureau ou de leur lieu de travail.

L’impression sur le **Cloud hybride** est prise en charge dans Windows 10 Creators Update et Windows 10 S.
 
## <a name="feature-summary"></a>Résumé des fonctionnalités
L’impression sur le **Cloud hybride** se compose de deux composants principaux côté serveur : service de **découverte** et service d' **impression Windows** .
- Point de terminaison du service de **découverte** exécuté sur un service IIS prenant en charge Mopria Alliance Industry Standard pour la découverte d’imprimantes dans le Cloud.
- Point de terminaison du service d' **impression Windows** exécuté sur un service IIS prenant en charge le protocole IPP (Internet Printing Protocol) standard pour garantir la prise en charge de système d’exploitation client la plus large.

## <a name="deployment"></a>Déploiement
L’impression sur le **Cloud hybride** prend en charge deux options de déploiement différentes en fonction de l’endroit où votre organisation requiert l’authentification des utilisateurs. Voici à quoi peut ressembler un déploiement :

![Diagramme illustrant une représentation graphique de la solution d’impression du Cloud hybride](../media/hybrid-cloud-print/wshcp-deployment-options.png)

*Diagramme de solution d’impression sur le Cloud hybride*

Le diagramme illustre les éléments suivants :
- **Impression sur le Cloud hybride** à l’aide d’Azure Active Directory en tant que fournisseur d’identité utilisateur. 
- Les points de terminaison du service d’impression et de **découverte** de **Windows** sont inscrits auprès de Azure Active Directory pour permettre à l’appareil client de récupérer le jeton d’authentification utilisateur requis à utiliser avec ces services. 
- Un service MDM, tel que **Microsoft Intune**, configure l’appareil client avec les stratégies nécessaires pour se connecter Azure Active Directory au service de **découverte** et de service d' **impression Windows** .

Cette table contient plus d’informations sur les éléments du diagramme.  

| Élément | Description |
| ------- | ----------- |
| Azure Active Directory  | Fournit et contrôle les fonctionnalités d’identité et d’autorisation des utilisateurs |
| Active Directory        | Fournit et contrôle les fonctionnalités d’identité et d’autorisation des utilisateurs |
| Azure AD Connect  | Synchronise les informations d’identification de l’utilisateur entre Azure AD et l’annuaire Active Directory local. |
| Service MDM (Intune) | Fournit la fonctionnalité d’approvisionnement de stratégie d’appareil pour garantir que le périphérique client (BYOD) est conforme aux stratégies d’entreprise. |
| Proxy Azure AD | Fournit une connexion à long terme établie de derrière votre pare-feu vers Azure pour permettre au trafic d’application configuré spécifique de circuler depuis Internet vers le réseau d’entreprise. |
| Application Web Azure | Le cœur de la solution d’impression du Cloud hybride. Fournit les points de terminaison Web requis pour découvrir des imprimantes et envoyer du contenu d’impression pour les appareils non joints à un domaine. |
| Périphérique BYOD/spouleur du serveur d’impression Windows/imprimante | Ils sont tels quels. Aucune modification de la fonctionnalité dans le déploiement. |

Il existe deux façons d’installer le **Cloud hybride**:
- \* * Fonctionnalités à la demande, voir [configurer des fonctionnalités à la demande dans Windows Server](https://docs.microsoft.com/windows-server/administration/server-manager/configure-features-on-demand-in-windows-server) pour en savoir plus sur l’ajout et la suppression de fichiers de rôles et de fonctionnalités. 
- \* * Paramètres Windows Server 2016, que les administrateurs peuvent atteindre **paramètres** -> **apps** -> **gérer les fonctionnalités facultatives** -> **Ajouter une fonctionnalité** et rechercher le package fonctionnalités à la demande 
- Commandes PowerShell-dans une fenêtre Administrateur PowerShell, exécutez les commandes suivantes :

```PowerShell
    Install-Module -Name PublishCloudPrinter
    Import-Module PublishCloudPrinter
    ```
