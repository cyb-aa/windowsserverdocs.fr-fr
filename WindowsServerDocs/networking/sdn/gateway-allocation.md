---
title: Allocation de bande passante de la passerelle
description: ''
manager: dougkim
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: fc59fc9d7dc22b9c5567bae314b4312d76fcff19
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406123"
---
# <a name="gateway-bandwidth-allocation"></a>Allocation de bande passante de la passerelle

>S’applique à : Windows Server

Dans Windows Server 2016, la bande passante de tunnel individuelle pour IPsec, GRE et L3 était un rapport entre la capacité totale de la passerelle. Par conséquent, les clients fourniraient la capacité de la passerelle en fonction de la bande passante TCP standard qui attendait ce de la machine virtuelle de la passerelle.

En outre, la bande passante maximale du tunnel IPsec sur la passerelle était limitée à (3/20)\*la capacité de la passerelle fournie par le client. Par exemple, si vous définissez la capacité de la passerelle sur 100 Mbits/s, la capacité du tunnel IPsec est de 150 Mbits/s. Les ratios équivalents pour les tunnels GRE et L3 sont respectivement 1/5 et 1/2.

Bien que cela fonctionnait pour la majorité des déploiements, le modèle de rapport fixe n’était pas approprié pour les environnements à débit élevé. Même lorsque les taux de transfert de données étaient élevés (par exemple, plus de 40 Gbits/s), le débit maximal de tunnels de passerelle SDN plafonné en raison de facteurs internes.

Dans Windows Server 2019, pour un type de tunnel, le débit maximal est fixe :

-   IPsec = 5 Gbits/s

-   GRE = 15 Gbits/s

-   L3 = 5 Gbits/s

Ainsi, même si votre hôte/machine virtuelle de passerelle prend en charge les cartes réseau avec un débit bien plus élevé, le débit de tunnel maximal disponible est fixe. Un autre problème qui se pose concerne les passerelles de provisionnement de façon arbitraire, ce qui se produit lorsque vous fournissez un nombre très élevé de la capacité de la passerelle.

## <a name="gateway-capacity-calculation"></a>Calcul de la capacité de la passerelle

Dans l’idéal, vous définissez la capacité de débit de la passerelle sur le débit disponible pour la machine virtuelle de la passerelle. Par exemple, si vous avez une seule machine virtuelle de passerelle et que le débit de la carte réseau de l’hôte sous-jacent est de 25 Gbit/s, le débit de la passerelle peut également être défini à 25 Gbits/s.

Si vous utilisez une passerelle uniquement pour les connexions IPsec, la capacité fixe maximale disponible est de 5 Gbits/s. Par exemple, si vous configurez des connexions IPsec sur la passerelle, vous pouvez uniquement configurer une bande passante globale (entrant + sortant) de 5 Gbits/s.

Si vous utilisez la passerelle pour la connectivité IPsec et GRE, vous pouvez approvisionner jusqu’à 5 Gbits/s de débit IPsec ou 15 Gbits/s de débit GRE au maximum. Par exemple, si vous approvisionnez 2 Gbits/s de débit IPsec, vous disposez de 3 Gbits/s de débit IPsec pour approvisionner la passerelle ou 9 Gbits/s de débit GRE restant.

Pour mettre cela en termes mathématiques :

- Capacité totale de la passerelle = 25 Gbits/s

- Capacité IPsec totale disponible = 5 Gbits/s (fixe)

- Capacité GRE totale disponible = 15 Gbits/s (fixe)

- Taux de débit IPsec pour cette passerelle = 25/5 = 5 Gbits/s

- Ratio de débit GRE pour cette passerelle = 25/15 = 5/3 Gbits/s

Par exemple, si vous allouez 2 Gbits/s de débit IPsec à un client :

Capacité disponible restante sur la passerelle = capacité totale de la passerelle – taux de débit IPsec * débit IPsec alloué (capacité utilisée)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;25 – 5 * 2 = 15 Gbits/s

Débit IPsec restant que vous pouvez allouer sur la passerelle 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5-2 = 3 Gbits/s

Débit GRE restant que vous pouvez allouer sur la passerelle = capacité restante du ratio de débit de la passerelle/GRE 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;15 * 3/5 = 9 Gbits/s

Le taux de débit varie en fonction de la capacité totale de la passerelle. Il est important de noter que vous devez définir la capacité totale sur la bande passante TCP disponible pour la machine virtuelle de la passerelle. Si vous avez plusieurs machines virtuelles hébergées sur la passerelle, vous devez ajuster la capacité totale de la passerelle en conséquence.

En outre, si la capacité de la passerelle est inférieure à la capacité totale disponible du tunnel, la capacité totale disponible du tunnel est définie sur la capacité de la passerelle. Par exemple, si vous définissez la capacité de la passerelle à 4 Gbits/s, la capacité totale disponible pour IPsec, L3 et GRE est définie à 4 Gbit/s, ce qui laisse le taux de débit pour chaque tunnel à 1 Gbit/s.

## <a name="windows-server-2016-behavior"></a>Comportement de Windows Server 2016

L’algorithme de calcul de la capacité de la passerelle pour Windows Server 2016 reste inchangé. Dans Windows Server 2016, la bande passante maximale du tunnel IPsec était limitée à (3/20)\*la capacité de la passerelle sur une passerelle. Les ratios équivalents pour les tunnels GRE et L3 étaient respectivement 1/5 et 1/2.

Si vous effectuez une mise à niveau de Windows Server 2016 vers Windows Server 2019 :

1.  **Tunnels GRE et L3 :** La logique d’allocation de Windows Server 2019 prend effet une fois que les nœuds du contrôleur de réseau sont mis à jour vers Windows Server 2019

2.  **Tunnels IPSec :** La logique d’allocation de la passerelle Windows Server 2016 continue de fonctionner jusqu’à ce que toutes les passerelles du pool de passerelle soient mises à niveau vers Windows Server 2019. Pour toutes les passerelles dans le pool de passerelles, vous devez définir le service de passerelle Azure sur **automatique**.

>[!NOTE]
>Il est possible qu’après la mise à niveau vers Windows Server 2019, une passerelle soit surapprovisionnée (car la logique d’allocation passe de Windows Server 2016 à Windows Server 2019). Dans ce cas, les connexions existantes sur la passerelle continuent d’exister. La ressource REST de la passerelle lève un avertissement indiquant que la passerelle est trop approvisionnée. Dans ce cas, vous devez déplacer certaines connexions vers une autre passerelle.

---