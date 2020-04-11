---
title: Services Bureau à distance - Multi-Factor Authentication
description: Informations de planification pour l’utilisation de MFA avec les services Bureau à distance.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 09ea784e-5644-417a-a3d9-bdbcebc313f9
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: c46ad24c62510b4a100a89b5c10a8f52c1a66151
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857352"
---
# <a name="remote-desktop-services---multi-factor-authentication"></a>Services Bureau à distance - Multi-Factor Authentication

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Tirez parti de la puissance d’Active Directory avec Multi-Factor Authentication pour appliquer un niveau de protection élevé à la sécurité des ressources de votre entreprise.

Pour les utilisateurs finaux qui se connectent à leurs bureaux et applications, l’expérience utilisateur est similaire à celle qu’ils connaissent déjà quand ils effectuent une deuxième authentification pour se connecter à la ressource souhaitée :
- Lancer un bureau ou un programme RemoteApp à partir d’un fichier RDP ou via une application cliente Bureau à distance
- Au moment de la connexion à la passerelle des services Bureau à distance pour un accès à distance sécurisé, recevez une demande d’authentification par stimulation MFA via un SMS ou une application mobile
- Authentifiez-vous correctement pour vous connecter à la ressource souhaitée !

Pour plus d’informations sur le processus de configuration, consultez [Intégrez votre infrastructure de passerelle des services Bureau à distance à l’aide de l’extension NPS (Network Policy Server) et d’Azure AD](https://docs.microsoft.com/azure/multi-factor-authentication/nps-extension-remote-desktop-gateway).
