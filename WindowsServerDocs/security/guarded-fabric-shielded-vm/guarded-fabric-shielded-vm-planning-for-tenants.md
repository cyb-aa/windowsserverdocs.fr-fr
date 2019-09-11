---
title: Guide de planification de l’infrastructure protégée et de la machine virtuelle protégée pour les hébergeurs
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 392af37f-a02d-4d40-a25d-384211cbbfdd
manager: dongill
author: nirb-ms
ms.technology: security-guarded-fabric
ms.openlocfilehash: d361dfee0cbb06c4b7908b80145c09327dac3956
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870430"
---
# <a name="guarded-fabric-and-shielded-vm-planning-guide-for-tenants"></a>Guide de planification de l’infrastructure protégée et de la machine virtuelle protégée pour les locataires

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique se concentre sur les propriétaires d’ordinateurs virtuels qui souhaitent protéger leurs machines virtuelles à des fins de conformité et de sécurité. Que les machines virtuelles s’exécutent sur une infrastructure protégée d’un fournisseur d’hébergement ou une infrastructure protégée privée, les propriétaires de machines virtuelles doivent contrôler le niveau de sécurité de leurs machines virtuelles protégées, ce qui implique de conserver la possibilité de les déchiffrer si nécessaire.

Il y a trois points à prendre en compte lors de l’utilisation de machines virtuelles protégées :

- Niveau de sécurité pour les machines virtuelles
- Clés de chiffrement utilisées pour les protéger
- Données de protection : informations sensibles utilisées pour créer des machines virtuelles dotées d’une protection maximale 

## <a name="security-level-for-the-vms"></a>Niveau de sécurité pour les machines virtuelles

Lors du déploiement de machines virtuelles protégées, vous devez sélectionner l’un des deux niveaux de sécurité suivants :

- Protégées 
- Chiffrement pris en charge

Les machines virtuelles compatibles avec protection et chiffrement sont associées à un module de plateforme sécurisée virtuel et celles qui exécutent Windows sont protégées par BitLocker. La principale différence réside dans le fait que les machines virtuelles protégées bloquent l’accès par les administrateurs de structure tandis que les machines virtuelles prises en charge par le chiffrement permettent aux administrateurs de structure d’accéder au même niveau d’accès qu’à une machine virtuelle normale. Pour plus d’informations sur ces différences, consultez [vue d’ensemble de l’infrastructure protégée et des machines virtuelles protégées](guarded-fabric-and-shielded-vms.md). 

Choisissez **machines virtuelles** protégées si vous souhaitez protéger la machine virtuelle d’une infrastructure compromise (y compris les administrateurs compromis). Ils doivent être utilisés dans les environnements où les administrateurs de structure et la structure elle-même ne sont pas approuvés. Choisissez **chiffrement des machines virtuelles prises en charge** si vous envisagez de rencontrer une barre de conformité qui peut nécessiter le chiffrement au repos et le chiffrement de la machine virtuelle sur le réseau (par exemple, lors d’une migration dynamique).

Les machines virtuelles prises en charge par le chiffrement sont idéales dans les environnements où les administrateurs de structure sont entièrement fiables, mais le chiffrement reste une exigence.

Vous pouvez exécuter un mélange de machines virtuelles standard, de machines virtuelles protégées et de machines virtuelles prises en charge par le chiffrement sur une infrastructure protégée et même sur le même hôte Hyper-V. 

La protection et la prise en charge d’une machine virtuelle sont déterminées par les données de protection sélectionnées lors de la création de la machine virtuelle. Les propriétaires de machines virtuelles configurent le niveau de sécurité lors de la création des données de protection (voir la section [données de protection](#shielding-data) ).
Notez qu’une fois ce choix effectué, il ne peut pas être modifié tant que la machine virtuelle reste sur la structure de virtualisation.

## <a name="cryptographic-keys-used-for-shielded-vms"></a>Clés de chiffrement utilisées pour les machines virtuelles protégées

Les machines virtuelles dotées d’une protection maximale sont protégées des vecteurs d’attaque de la structure de virtualisation à l’aide de disques chiffrés et d’autres éléments chiffrés qui peuvent être déchiffrés uniquement par :

- Une clé propriétaire : il s’agit d’une clé de chiffrement gérée par le propriétaire de la machine virtuelle, généralement utilisée pour la récupération ou le dépannage en dernier recours. Les propriétaires de machines virtuelles sont responsables de la gestion des clés de propriétaire dans un emplacement sécurisé.
- Un ou plusieurs gardiens (clés Guardian hôtes) : chaque gardien représente une structure de virtualisation sur laquelle un propriétaire autorise l’exécution de machines virtuelles protégées. Les entreprises disposent souvent d’une structure de virtualisation principale et de récupération d’urgence (DR). elles autorisent généralement l’exécution de leurs machines virtuelles protégées sur les deux. Dans certains cas, l’infrastructure secondaire (DR) peut être hébergée par un fournisseur de cloud public. Les clés privées d’une infrastructure protégée sont conservées uniquement sur la structure de virtualisation, tandis que ses clés publiques peuvent être téléchargées et contenues dans son gardien. 

**Comment faire créer une clé de propriétaire ?** Une clé propriétaire est représentée par deux certificats. Un certificat pour le chiffrement et un certificat pour la signature. Vous pouvez créer ces deux certificats à l’aide de votre propre infrastructure d’infrastructure à clé publique ou obtenir des certificats SSL auprès d’une autorité de certification publique. À des fins de test, vous pouvez également créer un certificat auto-signé sur n’importe quel ordinateur à partir de Windows 10 ou Windows Server 2016.

**Combien de clés de propriétaire devez-vous utiliser ?** Vous pouvez utiliser une clé propriétaire unique ou plusieurs clés de propriétaire. Les meilleures pratiques recommandent une clé de propriétaire unique pour un groupe de machines virtuelles qui partagent le même niveau de sécurité, d’approbation ou de risque, et pour le contrôle administratif. Vous pouvez partager une clé de propriétaire unique pour toutes vos machines virtuelles protégées par un domaine et faire confiance à la clé de propriétaire pour qu’elles soient gérées par les administrateurs de domaine.

**Puis-je utiliser mes propres clés pour le gardien hôte ?** Oui, vous pouvez « apporter votre propre clé » au fournisseur d’hébergement et utiliser cette clé pour vos machines virtuelles protégées. Cela vous permet d’utiliser vos clés spécifiques (par rapport à l’utilisation de la clé du fournisseur d’hébergement) et peut être utilisé lorsque vous avez des réglementations ou des sécurités spécifiques que vous devez respecter. À des fins d’hygiène clé, les clés du gardien hôte doivent être différentes de la clé propriétaire.

## <a name="shielding-data"></a>Données de protection

Les données de protection contiennent les secrets nécessaires au déploiement de machines virtuelles protégées ou prises en charge par le chiffrement. Il est également utilisé lors de la conversion de machines virtuelles standard en machines virtuelles protégées.

Les données de protection sont créées à l’aide de l’Assistant fichier de données de protection et sont stockées dans des fichiers PDK que les propriétaires de machines virtuelles chargent sur l’infrastructure protégée.

Les machines virtuelles protégées permettent de se protéger contre les attaques d’une structure de virtualisation compromise. nous avons donc besoin d’un mécanisme sécurisé pour transmettre des données d’initialisation sensibles, telles que le mot de passe de l’administrateur, les informations d’identification de jonction de domaine ou les certificats RDP, sans les divulguer à structure de virtualisation elle-même ou à ses administrateurs. En outre, les données de protection contiennent les éléments suivants :

1. Niveau de sécurité – protégé ou chiffré-pris en charge
2. Propriétaire et liste des gardiens hôtes approuvés où la machine virtuelle peut s’exécuter
3. Données d’initialisation de la machine virtuelle (Unattend. xml, certificat RDP)
4. Liste des disques de modèles signés approuvés pour la création de la machine virtuelle dans l’environnement de virtualisation 

Lors de la création d’une machine virtuelle protégée par un chiffrement ou protégée, ou de la conversion d’une machine virtuelle existante, vous êtes invité à sélectionner les données de protection au lieu d’être invité à fournir les informations sensibles.

**De combien de fichiers de données de protection ai-je besoin ?** Un seul fichier de données de protection peut être utilisé pour créer chaque machine virtuelle protégée. Toutefois, si une machine virtuelle protégée donnée exige que l’un des quatre éléments soit différent, un fichier de données de protection supplémentaire est nécessaire. Par exemple, vous pouvez avoir un fichier de données de protection pour votre service informatique et un autre fichier de données de protection pour le service RH, car leur mot de passe d’administrateur initial et les certificats RDP sont différents.

Bien qu’il soit possible d’utiliser des fichiers de données de protection distincts pour chaque machine virtuelle protégée, ce n’est pas nécessairement le choix optimal et doit être effectué pour les bonnes raisons. Par exemple, si chaque machine virtuelle dotée d’une protection maximale doit avoir un mot de passe d’administrateur différent, utilisez plutôt un service de gestion des mots de passe ou un outil tel que [la solution de mot de passe d’administrateur local de Microsoft (laps)](https://www.microsoft.com/en-us/download/details.aspx?id=46899).

## <a name="creating-a-shielded-vm-on-a-virtualization-fabric"></a>Création d’une machine virtuelle protégée sur une structure de virtualisation

Il existe plusieurs options pour créer une machine virtuelle protégée sur une infrastructure de virtualisation (les éléments suivants s’appliquent à la fois aux machines virtuelles protégées et aux machines virtuelles prises en charge par le chiffrement) :

1. Créer une machine virtuelle protégée dans votre environnement et la charger dans la structure de virtualisation
2. Créer une machine virtuelle protégée à partir d’un modèle signé sur la structure de virtualisation
3. Protéger une machine virtuelle existante (la machine virtuelle existante doit être de génération 2 et doit exécuter Windows Server 2012 ou version ultérieure)

La création de nouvelles machines virtuelles à partir d’un modèle est une pratique normale. Toutefois, étant donné que le disque de modèle utilisé pour créer de nouvelles machines virtuelles protégées réside sur la structure de virtualisation, des mesures supplémentaires sont nécessaires pour s’assurer qu’il n’a pas été falsifié par un administrateur de structure malveillante ou par un programme malveillant s’exécutant sur l’infrastructure. Ce problème est résolu à l’aide de disques de modèle signés : les disques de modèle signés et leurs signatures de disque sont créés par des administrateurs approuvés ou le propriétaire de la machine virtuelle. Lorsqu’une machine virtuelle dotée d’une protection maximale est créée, la signature du disque de modèle est comparée aux signatures contenues dans le fichier de données de protection spécifié. Si l’une des signatures du fichier de données de protection correspond à la signature du disque de modèle, le processus de déploiement se poursuit. Si aucune correspondance n’est trouvée, le processus de déploiement est abandonné, garantissant ainsi que les secrets des machines virtuelles ne seront pas compromis en raison d’un disque de modèle non fiable.

Lorsque vous utilisez des disques de modèle signés pour créer des machines virtuelles protégées, deux options sont disponibles :

1. Utilisez un disque de modèle signé existant fourni par votre fournisseur de virtualisation. Dans ce cas, le fournisseur de virtualisation gère les disques de modèle signés.
2. Chargez un disque de modèle signé dans la structure de virtualisation. Le propriétaire de la machine virtuelle est chargé de gérer les disques de modèle signés. 


