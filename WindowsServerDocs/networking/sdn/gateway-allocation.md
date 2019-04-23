---
title: Allocation de bande passante de la passerelle
description: ''
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: 3c259b96e1a8ee27888a5cccc50b285a2f7cb8c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850430"
---
# <a name="gateway-bandwidth-allocation"></a>Allocation de bande passante de la passerelle

>S’applique à : Windows Server

Dans Windows Server 2016, la bande passante de tunnel individuels pour IPsec, GRE et L3 était un ratio de la capacité totale de passerelle. Par conséquent, les clients fournit la capacité de la passerelle de la bande de passante TCP standard cela attendu en dehors de la machine virtuelle de passerelle.

En outre, la bande passante maximale de tunnel IPsec sur la passerelle était limitée (3/20)\*capacité de la passerelle fourni par le client. Par conséquent, par exemple, si vous définissez la capacité de la passerelle à 100 Mbits/s, alors la capacité de tunnel IPsec est 150 Mbits/s. Les rapports équivalents pour les tunnels GRE et L3 sont 1/5 et 1/2, respectivement.

Bien que cela a fonctionné pour la majorité des déploiements, le modèle de rapport fixe n’était pas approprié pour les environnements de débit élevé. Même lorsque le taux de transfert de données ont été élevé (par exemple, supérieure à 40 Gbits/s), le débit maximal de tunnels de passerelle SDN limitée en raison de facteurs internes.

Dans Windows Server 2019, pour un type de tunnel, le débit maximal est fixe :

-   IPsec = 5 Gbits/s

-   GRE = 15 Gbit/s

-   L3 = 5 Gbits/s

Par conséquent, même si votre passerelle/machine virtuelle hôte prend en charge les cartes réseau avec un débit beaucoup plus élevé, le débit maximal disponible dans le tunnel est fixe. Un autre problème, de qu'il s’occupe est arbitrairement un approvisionnement excessif passerelles, ce qui se produit lorsque vous fournissez un nombre très élevé pour la capacité de la passerelle.

## <a name="gateway-capacity-calculation"></a>Calcul de capacité de passerelle

Dans l’idéal, vous définissez la capacité de débit de passerelle sur le débit disponible pour la machine virtuelle de passerelle. Par conséquent, par exemple, si vous avez une seule machine virtuelle de passerelle et le débit de carte réseau hôte sous-jacent est 25 Gbits/s, le débit de passerelle peut être défini à 25 Gbits/s ainsi.

Si vous utilisez une passerelle uniquement pour les connexions IPsec, la capacité fixe maximale disponible est de 5 Gbit/s. Par conséquent, par exemple, si vous configurez des connexions IPsec sur la passerelle, vous ne pouvez configurer pour une bande passante globale (entrante + sortante) en tant que 5 Gbits/s.

Si vous utilisez la passerelle pour la connectivité IPsec et GRE, vous pouvez approvisionner le débit maximal 5 Gbits/s de la sécurité IPsec ou le débit maximal 15 Gbits/s de GRE. Par conséquent, par exemple, si vous configurez le débit de 2 Gbits/s de la sécurité IPsec, vous avez 3 Gbit/s de la sécurité IPsec débit restant à approvisionner sur la passerelle ou un débit Gbits/s de GRE 9 gauche.

Pour mettre cela en termes plus mathématiques :

- Capacité totale de la passerelle = 25 Gbits/s

- Nombre total de capacité IPsec disponible = 5 Gbits/s (fixe)

- Nombre total de capacité disponible de GRE = 15 Gbit/s (fixe)

- Taux de débit d’IPsec pour cette passerelle = 25/5 = 5 Gbits/s

- Taux de débit GRE pour cette passerelle = 25/15 = 5/3 Gbit/s

Par exemple, si vous allouez un débit de 2 Gbits/s de la sécurité IPsec à un client :

Capacité disponible sur la passerelle restante = capacité totale de la passerelle – taux de débit IPsec * IPsec débit alloué (capacité utilisée)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;25 – 5 * 2 = 15 Gbit/s

Débit IPsec restant que vous pouvez allouer sur la passerelle 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5-2 = 3 Gbit/s

Débit GRE restant que vous pouvez allouer sur la passerelle = capacité restante de taux de débit de passerelle/GRE 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;15 * 3/5 = 9 Gbits/s

Le taux de débit varie en fonction de la capacité totale de la passerelle. Une chose à noter est que vous devez définir la capacité totale de la bande passante TCP pour la machine virtuelle de passerelle. Si vous avez plusieurs machines virtuelles hébergées sur la passerelle, vous devez ajuster la capacité totale de la passerelle en conséquence.

En outre, si la capacité de la passerelle est inférieure à la capacité de tunnel disponible totale, la capacité de tunnel disponible total est définie à la capacité de la passerelle. Par exemple, si vous définissez la capacité de la passerelle à 4 Gbits/s, la capacité totale disponible pour IPsec et GRE L3 est définie à 4 Gbits/s, en laissant le taux de débit de chaque tunnel à 1 Gbit/s.

## <a name="windows-server-2016-behavior"></a>Comportement de Windows Server 2016

L’algorithme de calcul de capacité de passerelle pour Windows Server 2016 reste inchangé. Dans Windows Server 2016, la bande passante de tunnel IPsec maximale a été limitée (3/20)\*capacité de la passerelle sur une passerelle. Les rapports équivalents pour les tunnels GRE et L3 sont 1/5 et 1/2, respectivement.

Si vous mettez à niveau à partir de Windows Server 2016 pour Windows Server 2019 :

1.  **Tunnels GRE et L3 :** La logique de répartition de Windows Server 2019 en vigueur une fois que les nœuds de contrôleur de réseau mis à jour pour Windows Server 2019

2.  **Tunnels IPSec :** La logique de l’allocation de passerelle Windows Server 2016 continue de fonctionner jusqu'à ce que toutes les passerelles dans le pool de passerelles mis à niveau vers Windows Server 2019. Pour toutes les passerelles dans le pool de passerelle, vous devez définir le service de passerelle Azure **automatique**.

>[!NOTE]
>Il est possible qu’après la mise à niveau vers Windows Server 2019, une passerelle devient surconfigurée (comme la modification de la logique de l’allocation à partir de Windows Server 2016 pour Windows Server 2019). Dans ce cas, les connexions existantes sur la passerelle continuent d’exister. La ressource REST pour la passerelle lève un avertissement indiquant que la passerelle est surapprovisionnée. Dans ce cas, vous devez déplacer certaines connexions vers une autre passerelle.

---