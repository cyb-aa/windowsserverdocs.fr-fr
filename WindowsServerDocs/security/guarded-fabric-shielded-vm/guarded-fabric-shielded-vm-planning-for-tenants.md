---
title: Structure protégée et machines virtuelles protégées Planning Guide pour les hébergeurs
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 392af37f-a02d-4d40-a25d-384211cbbfdd
manager: dongill
author: nirb-ms
ms.technology: security-guarded-fabric
ms.openlocfilehash: 0a43cedf8ce0138e89624ff3df5bc7088f583252
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875300"
---
# <a name="guarded-fabric-and-shielded-vm-planning-guide-for-tenants"></a>Structure protégée et machines virtuelles protégées Guide de planification de locataires

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique se concentre sur les propriétaires d’ordinateurs virtuels qui souhaitent protéger leurs machines virtuelles (VM) à des fins de sécurité et de la conformité. Indépendamment de si les machines virtuelles s’exécutent sur l’infrastructure protégée d’un fournisseur d’hébergement ou une structure protégée privée, les propriétaires d’ordinateurs virtuels doivent contrôler le niveau de sécurité de leurs machines virtuelles protégées, qui inclut maintenant la possibilité de les déchiffrer si nécessaire.

Il existe trois domaines à prendre en compte lors de l’aide des machines virtuelles protégées :

- Le niveau de sécurité pour les machines virtuelles
- Les clés de chiffrement utilisés pour les protéger
- Les données de protection, les informations sensibles utilisées pour créer des machines virtuelles protégées 

## <a name="security-level-for-the-vms"></a>Niveau de sécurité pour les machines virtuelles

Lorsque vous déployez des machines virtuelles protégées, un des deux niveaux de sécurité doit être sélectionné :

- Protégées 
- Chiffrement pris en charge

Les deux protégées et de chiffrement pris en charge les machines virtuelles ont un TPM virtuel attaché à et celles que l’exécution de Windows sont protégés par BitLocker. La principale différence est que protégées machines virtuelles bloquer l’accès par les administrateurs de structure tandis que le chiffrement est pris en charge les machines virtuelles permettent aux administrateurs de fabric le même niveau d’accès qu’il aurait à une machine virtuelle standard. Pour plus d’informations sur ces différences, consultez [protégées de vue d’ensemble des machines virtuelles et service Guardian fabric](guarded-fabric-and-shielded-vms.md). 

Choisissez **machines virtuelles protégées** si vous souhaitez pour protéger la machine virtuelle à partir d’une structure compromise (y compris les administrateurs compromis). Ils doivent être utilisés dans les environnements où les administrateurs de structure et de l’infrastructure elle-même ne sont pas approuvés. Choisissez **chiffrement pris en charge de machines virtuelles** si vous cherchez à répondre à une barre de conformité qui peut-être nécessiter le chiffrement au repos et chiffrement de la machine virtuelle sur le câble (par exemple, lors de la migration en direct).

Chiffrement pris en charge les machines virtuelles sont idéales dans les environnements où les administrateurs de structure sont entièrement fiables, mais que le chiffrement reste une exigence.

Vous pouvez exécuter un mélange de ceux des machines virtuelles, des machines virtuelles protégées et de chiffrement pris en charge les machines virtuelles sur une structure protégée et même sur le même hôte Hyper-V. 

Si une machine virtuelle est protégée ou chiffrement pris en charge est déterminée par les données de protection qui sont sélectionnées lors de la création de la machine virtuelle. Propriétaires d’ordinateurs virtuels configurer le niveau de sécurité lors de la création des données des protection (voir la [les données de protection](#shielding-data) section).
Notez qu’une fois ce choix a été effectué, il ne peut pas être modifié tandis que la machine virtuelle reste sur la structure de virtualisation.

## <a name="cryptographic-keys-used-for-shielded-vms"></a>Clés de chiffrement utilisées pour les machines virtuelles protégées

Machines virtuelles protégées sont protégés contre la virtualisation fabric vecteurs d’attaque à l’aide de disques chiffrés et divers autres éléments chiffrés qui peuvent uniquement être déchiffrées par :

- Une clé de propriétaire : il s’agit d’une clé de chiffrement gérée par le propriétaire de machine virtuelle qui est généralement utilisé pour la récupération de dernier recours ou de la résolution des problèmes. Machine virtuelle propriétaires sont responsables du propriétaire des clés dans un emplacement sécurisé.
- Un ou plusieurs des gardiens (clés de la surveillance des hôtes) – chaque Guardian représente une structure de virtualisation sur lequel un propriétaire n’autorise pas à exécuter des machines virtuelles protégées. Souvent, les entreprises ont un serveur principal et une structure de virtualisation de récupération d’urgence d’urgence et autoriseraient généralement leurs machines virtuelles protégées pour s’exécuter sur les deux. Dans certains cas, l’infrastructure (DR) secondaire peut être hébergé par un fournisseur de cloud public. Les clés privées pour toute infrastructure protégée sont conservées uniquement sur la structure de virtualisation, tandis que ses clés publiques peuvent être téléchargées et sont contenus dans ses Guardian. 

**Comment créer une clé de propriétaire ?** Une clé de propriétaire est représentée par deux certificats. Un certificat pour le chiffrement et un certificat de signature. Vous pouvez créer ces deux certificats à l’aide de votre propre infrastructure à clé publique ou obtenir des certificats SSL à partir d’une autorité de certification publique (CA). À des fins de test, vous pouvez également créer un certificat auto-signé sur n’importe quel ordinateur à compter de Windows 10 ou Windows Server 2016.

**Combien de clés propriétaire si vous avez effectué ?** Vous pouvez utiliser une clé de propriétaire unique ou plusieurs clés de propriétaire. Les meilleures pratiques recommandent une clé unique propriétaire d’un groupe de machines virtuelles qui partagent la même sécurité, niveau de confiance ou un risque et pour le contrôle administratif. Vous pouvez partager une clé unique propriétaire pour tous vos joints au domaine des machines virtuelles protégées et cette clé propriétaire qui seront gérés par les administrateurs de domaine du tiers de confiance.

**Puis-je utiliser mes propres clés pour la surveillance des hôtes ?** Oui, vous pouvez la clé « Bring Your Own » pour l’hébergement fournisseur et l’utilisation essentielles pour vos machines virtuelles protégées. Cela vous permet d’utiliser vos clés spécifiques (par opposition à l’aide de la clé du fournisseur hébergement) et peut être utilisé lorsque vous avez sécurité spécifiques ou dont vous avez besoin de respecter les réglementations. À des fins d’hygiène de la clé, les clés de la surveillance des hôtes doivent être différents de celui de la clé de propriétaire.

## <a name="shielding-data"></a>Les données de protection

Les données de protection contient les clés secrètes nécessaires au déploiement de machines virtuelles protégées ou chiffrement pris en charge. Il est également utilisé lors de la conversion de ceux des machines virtuelles à des machines virtuelles protégées.

Les données de protection sont créée à l’aide de l’Assistant de protection d’un fichier de données et sont stockée dans les fichiers PDK laquelle téléchargement des propriétaires d’ordinateurs virtuels à l’infrastructure service Guardian.

Machines virtuelles protégées vous aider à protéger contre les attaques à partir d’une structure de virtualisation compromis, afin que nous avons besoin d’un mécanisme sécurisé pour transmettre des données sensibles de l’initialisation, telles que le mot de passe, les informations d’identification de jonction de domaine ou les certificats RDP, l’administrateur sans révéler à la structure de virtualisation lui-même ou à ses administrateurs. En outre, les données de protection contient les éléments suivants :

1. Sécurité de niveau – protection ou le chiffrement est pris en charge
2. Propriétaire et liste de confiance gardiens hôte dans lequel la machine virtuelle peut s’exécuter
3. Données de l’initialisation de machine virtuelle (unattend.xml, certificat RDP)
4. Liste des disques de modèle signé approuvé pour la création de la machine virtuelle dans l’environnement de virtualisation 

Lorsque la création d’une machine virtuelle protégée ou chiffrement pris en charge ou la conversion d’une machine virtuelle existante, vous devez sélectionner les données de protection au lieu d’être invité à entrer les informations sensibles.

**Le nombre de fichiers de data protection ai-je besoin ?** Un fichier de données de protection unique peut être utilisé pour créer chaque machine virtuelle protégée. Si, toutefois, une machine virtuelle protégée donnée nécessite que les quatre éléments soient différents, un fichier de données de protection supplémentaire est nécessaire. Par exemple, peut avoir un fichier de données de protection pour votre service informatique et un autre fichier de données de protection pour le département Ressources humaines, car leur mot de passe administrateur initial et les certificats RDP est différent.

Bien qu’il soit possible d’à l’aide de fichiers de données de protection distincts pour chaque machine virtuelle protégée, il n’est pas nécessairement le meilleur choix et doit être effectuée pour les bonnes raisons. Par exemple, si chaque machine virtuelle protégée doit avoir un mot de passe d’administrateur différent, envisagez plutôt qu’une gestion de mot de passe de service ou outil tel que [Local administrateur Password Solution (LAPS de Microsoft)](https://www.microsoft.com/en-us/download/details.aspx?id=46899).

## <a name="creating-a-shielded-vm-on-a-virtualization-fabric"></a>Création d’une machine virtuelle protégée sur une structure de virtualisation

Il existe plusieurs options pour créer une machine virtuelle protégée sur une structure de virtualisation (les éléments suivants sont applicables pour les machines virtuelles protégées et de chiffrement pris en charge) :

1. Créer une machine virtuelle protégée dans votre environnement et le télécharger à l’infrastructure de virtualisation
2. Créer une machine virtuelle protégée à partir d’un modèle signé sur la structure de virtualisation
3. Bouclier une machine virtuelle existante (la machine virtuelle existante doit être de génération 2 et doit exécuter Windows Server 2012 ou version ultérieure)

Création de nouvelles machines virtuelles à partir d’un modèle consiste à normal. Toutefois, étant donné que le disque de modèle qui est utilisé pour créer la machine virtuelle protégée réside sur la structure de virtualisation, des mesures supplémentaires sont nécessaires pour vous assurer qu’il n'a pas été falsifié par un administrateur de structure malveillants ou par des programmes malveillants en cours d’exécution sur l’infrastructure. Ce problème est résolu à l’aide de disques de modèle signés — disques de modèle signés et leurs signatures de disque sont créées par les administrateurs de confiance ou le propriétaire de la machine virtuelle. Création d’une machine virtuelle protégée, signature du disque de modèle est comparée avec les signatures contenues dans le fichier de données de protection spécifié. Si une des signatures du nom du fichier de données de protection correspond signature du disque de modèle, le processus de déploiement se poursuit. Si aucune correspondance ne peut être trouvé, le processus de déploiement est abandonné, en garantissant que les secrets de machine virtuelle ne seront pas compromises en raison d’un disque de modèle non fiables.

Lors de l’utilisation de disques de modèle signés pour créer des machines virtuelles protégées, deux options sont disponibles :

1. Utiliser un disque de modèle signé existant qui est fourni par votre fournisseur de virtualisation. Dans ce cas, le fournisseur de virtualisation gère les disques de modèle signés.
2. Télécharger un disque de modèle signé à l’infrastructure de virtualisation. Le propriétaire de la machine virtuelle est responsable du maintien des disques de modèle signés. 


