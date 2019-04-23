---
title: Réglage des performances pour HTTP 1.1/2
description: Recommandations pour HTTP 1.1/2 de réglage des performances
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: IvanPash; GMonte
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: bf85efa88e377966135c23a548119f19c39cceba
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866370"
---
# <a name="performance-tuning-http-112"></a>HTTP 1.1/2 de réglage des performances

HTTP/2 est destiné à améliorer les performances côté client (par exemple, temps de chargement page sur un navigateur). Sur le serveur, il peut représenter une légère augmentation de coût UC. Tandis que le serveur ne requiert plus une connexion TCP unique pour chaque demande, certaines de cet état seront désormais conservée dans la couche HTTP. En outre, HTTP/2 a la compression d’en-tête, qui représente la charge d’UC supplémentaire.

Certaines situations nécessitent un secours (réinitialisation de la connexion HTTP/2 et à la place l’établissement d’une nouvelle connexion pour utiliser HTTP/1.1) HTTP/1.1. En particulier, renégociation TLS et l’authentification HTTP (autres que de base et Digest) requièrent secours HTTP/1.1. Bien que cela ajoute une surcharge, ces opérations déjà impliquent un délai et ne sont donc pas particulièrement sensibles aux performances.

## <a name="see-also"></a>Voir aussi
- [Web le réglage des performances de serveur](index.md) 
- [Réglage des performances d’IIS 10.0](tuning-iis-10.md)