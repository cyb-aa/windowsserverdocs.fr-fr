---
title: Réglage des performances pour HTTP 1.1/2
description: Recommandations en matière de réglage des performances pour HTTP 1.1/2
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: ivanpash; gmonte
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 5e62f7428f015193896aba5c7d9c146bd11e7225
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851682"
---
# <a name="performance-tuning-http-112"></a>Réglage des performances HTTP 1.1/2

HTTP/2 est conçu pour améliorer les performances côté client (par exemple, le temps de chargement de la page sur un navigateur). Sur le serveur, il peut représenter une légère augmentation du coût de l’UC. Alors que le serveur n’a plus besoin d’une connexion TCP unique pour chaque demande, une partie de cet État est désormais conservée dans la couche HTTP. En outre, HTTP/2 a une compression d’en-tête, qui représente une charge de processeur supplémentaire.

Certaines situations requièrent une solution de secours HTTP/1.1 (en réinitialisant la connexion HTTP/2 et en établissant une nouvelle connexion pour utiliser HTTP/1.1). En particulier, la renégociation TLS et l’authentification HTTP (autres que Basic et Digest) requièrent une solution de secours HTTP/1.1. Même si cela ajoute une surcharge, ces opérations impliquent déjà un certain délai et ne sont donc pas particulièrement sensibles aux performances.

## <a name="see-also"></a>Voir aussi
- [Réglage des performances du serveur Web](index.md) 
- [Optimisation des performances d’IIS 10.0](tuning-iis-10.md)