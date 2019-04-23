---
title: Vue d’ensemble de l’impression de Cloud hybride Windows Server
description: L’impression de Cloud hybride permet aux professionnels de l’informatique prendre en charge les exigences de l’impression pour BYOD ou domaine appareils joints à un.
ms.prod: w10
ms.mktglfcycl: manage
ms.sitesec: library
ms.pagetype: store
ms.technology: server-general
author: TrudyHa
ms.author: TrudyHa
ms.date: 10/16/2017
ms.openlocfilehash: faa9fde857a9a4ee3f7c03f682b3dbced0340417
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878830"
---
# <a name="windows-server-hybrid-cloud-print-overview"></a>Vue d’ensemble de l’impression de Cloud hybride Windows Server

**S’applique à**
-   Windows Server 2016

## <a name="what-is-hybrid-cloud-print"></a>Nouveautés d’impression de Cloud hybride ?
**Impression de Cloud hybride** est une nouvelle fonctionnalité de Windows Server 2016 disponible via **fonctionnalités à la demande**. Il permet aux professionnels de l’informatique prendre en charge les exigences de l’impression pour les personnes qui apportent leurs propres appareils, ou utiliser des appareils joints à Azure Active Directory. Cela inclut les appareils mobiles tels que Windows phone, ordinateurs portables ou les tablettes exécutant Windows 10 ou Windows Mobile. Il prend en charge l’impression à partir de n’importe quel endroit personnes ont accès à Internet.

Pour les administrateurs informatiques, **impression de Cloud hybride** fournit l’accès utilisateur sécurisée à des imprimantes locales à l’aide multi-Factor d’Azure pour valider l’accès utilisateur. Fonctionnalité de (SSO) à authentification unique simplifie l’expérience utilisateur. **Impression de Cloud hybride** repose sur Windows **serveur d’impression** rôle, ce qui donne aux professionnels de l’informatique une expérience similaire à la gestion des imprimantes et sécurité d’accès utilisateur.

**L’impression de Cloud hybride** permet aux personnes de votre organisation pour imprimer à partir des appareils qu’ils utilisent pour effectuer leur travail - même lorsqu’ils sont en dehors de son bureau ou d’un espace de travail.

**Impression de Cloud hybride** est pris en charge dans Windows 10 Creators Update et Windows 10 S.
 
## <a name="feature-summary"></a>Résumé des fonctionnalités
**Impression de Cloud hybride** se compose de deux principaux composants côté serveur : **Découverte** service, et **Windows Print** service.
- **Découverte** point de terminaison de service en cours d’exécution sur un service IIS prenant en charge le standard de l’industrie Mopria Alliance pour la découverte d’imprimante dans le cloud.
- **Impression de Windows** standard protocole IPP (Internet Printing) pour garantir le système d’exploitation client plus large de point de terminaison de service en cours d’exécution sur un service IIS prenant en charge de secteur d’activité prennent en charge.

## <a name="deployment"></a>Déploiement
**Impression de Cloud hybride** prend en charge deux options de déploiement différents selon où votre organisation requiert une authentification utilisateur. Voici à quoi pourrait ressembler un déploiement :

![Un diagramme montrant une représentation graphique de la solution d’impression de Cloud hybride](../media/hybrid-cloud-print/wshcp-deployment-options.png)

*Diagramme de solution d’impression Cloud hybride*

Le diagramme montre :
- **Impression de Cloud hybride** à l’aide d’Azure Active Directory comme fournisseur d’identité utilisateur. 
- **Impression de Windows** service et **découverte** points de terminaison de service sont enregistrés avec Azure Active Directory pour activer l’appareil client récupérer le jeton d’authentification utilisateur requis pour utiliser par rapport à ces services. 
- Une gestion des appareils mobiles de service, tel que **Microsoft Intune**, provisionne l’appareil client avec les stratégies nécessaires pour se connecter à Azure Active Directory pour **Windows Print** service et **découverte**service.

Cette table a plus d’informations sur les éléments dans le diagramme.  

| Élément | Description |
| ------- | ----------- |
| Azure Active Directory  | Fournit des contrôles et fonctionnalités d’identité et d’autorisation utilisateur |
| Active Directory        | Fournit des contrôles et fonctionnalités d’identité et d’autorisation utilisateur |
| Azure AD Connect  | Synchronise les informations d’identification de l’utilisateur entre Azure AD et AD local. |
| Service de gestion des appareils mobiles (Intune) | Fournit la fonctionnalité d’approvisionnement stratégie appareil pour vous assurer de l’appareil client (appareil BYOD) est conforme aux stratégies d’entreprise. |
| Proxy Azure AD | Fournit une connexion à long terme qui est établie derrière votre pare-feu pour Azure pour autoriser le trafic spécifique application configurée à partir d’Internet au réseau d’entreprise. |
| Azure Web App | Le cœur de la solution d’impression de Cloud hybride. Fournit les points de terminaison web requis pour détecter des imprimantes et envoyer le contenu d’impression pour les appareils joints au domaine non. |
| APPAREIL BYOD / serveur spouleur d’impression de Windows / imprimante | Il s’agit en tant que-est. Aucune modification de fonctionnalité dans le déploiement. |

Il existe deux façons d’installer **impression de Cloud hybride**:
- **Fonctionnalités à la demande** -consultez [configurer les fonctionnalités à la demande dans Windows Server](https://docs.microsoft.com/windows-server/administration/server-manager/configure-features-on-demand-in-windows-server) pour en savoir plus sur l’ajout et suppression des fichiers de rôles et de fonctionnalités. 
- **Paramètres de Windows Server 2016** -les administrateurs peuvent accéder à **paramètres** -> **applications** -> **gestion des fonctionnalités facultatives**  ->  **Ajouter une fonctionnalité** et recherchez les fonctionnalités sur le package à la demande 
- Commandes PowerShell - fenêtre d’administrateur dans un PowerShell, exécutez les commandes suivantes :

```PowerShell
    Install-Module -Name PublishCloudPrinter
    Import-Module PublishCloudPrinter
    ```
