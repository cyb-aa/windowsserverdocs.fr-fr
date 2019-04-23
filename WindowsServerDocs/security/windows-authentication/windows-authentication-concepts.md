---
title: Concepts de l’authentification Windows
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 29d1db15-cae0-4e3d-9d8e-241ac206bb8b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: ee3fa19efa8c5098008749a467e649edcb5bb716
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876500"
---
# <a name="windows-authentication-concepts"></a>Concepts de l’authentification Windows

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique de vue d’ensemble de référence décrit les concepts sur lesquels repose l’authentification Windows.

L’authentification est un processus de vérification de l’identité d’un objet ou d’une personne. Lorsque vous authentifiez un objet, l’objectif est de vérifier que celui-ci est authentique. Lorsque vous authentifiez une personne, l’objectif est de vérifier que la personne n’est pas un imposteur.

Dans un contexte de réseau, l’authentification est le fait de prouver une identité à une application ou une ressource réseau. En règle générale, l’identité est prouvée par une opération de chiffrement qui utilise une clé seulement l’utilisateur sait (comme avec chiffrement à clé publique) ou une clé partagée. Le côté serveur de l’échange d’authentification compare les données signées avec une clé de chiffrement connue pour valider la tentative d’authentification.

Le stockage des clés de chiffrement dans un emplacement centralisé sécurisé rend le processus d’authentification évolutif et facile à gérer. Active Directory est l’architecture recommandée et la technologie par défaut pour le stockage des informations d’identité, qui incluent le chiffrement des clés qui sont des informations d’identification de l’utilisateur. Active Directory est requis pour les implémentations NTLM et Kerberos par défaut.

Techniques d’authentification allant de la simple ouverture de session à un système d’exploitation ou une connexion à un service ou une application, qui identifie les utilisateurs en fonction de quelque chose qui ne sait que l’utilisateur, comme un mot de passe, à des mécanismes de sécurité plus puissants qui utilisent des éléments qui has'such d’utilisateur en tant que jetons, des certificats de clé publique, des images ou des attributs biologiques. Dans un environnement d’entreprise, les utilisateurs peuvent accéder à plusieurs applications sur plusieurs types de serveurs dans un même emplacement ou dans plusieurs emplacements. C’est pourquoi l’authentification doit prendre en charge des environnements pour d’autres plateformes et pour d’autres systèmes d’exploitation Windows.

## <a name="authentication-and-authorization-a-travel-analogy"></a>Authentification et autorisation : Une analogie de voyage
Une analogie de voyages peut aider à expliquer le fonctionne de l’authentification. Certaines tâches préparatoires sont généralement nécessaires pour commencer le parcours. La découverte du doit prouver leur identité true à leurs autorités d’hôte. Cette preuve peut être sous la forme de preuve de citoyenneté, lieu de naissance, un chèque-cadeau personnel, photographies ou tout ce qui est requis par la loi du pays hôte. L’identité du voyageur est validée par l’émission d’un compte passport, ce qui est analogue à un compte système émis et administré par une organisation--le principal de sécurité. Le passport et la destination prévue sont basés sur un ensemble de règles et réglementations émises par l’autorité gouvernementale.

**Le parcours**

Lorsque la découverte du arrive à la frontière internationale, un garde bordure demande des informations d’identification et la découverte du présente ses propres passport. Le processus est constitué de deux étapes :

-   Le module de protection authentifie le passport en vérifiant qu’il a été délivré par une autorité de sécurité que le gouvernement local approuve (approbations, au moins, pour émettre des passeports) et en vérifiant que le passeport n’a pas été modifié.

-   Le module de protection authentifie le voyageur en vérifiant que la face correspond à la face de la personne représentée Passport et que les autres informations d’identification requises sont en bon état.

Si le passeport prouve qu’il soit valide et que le voyageur se révèle son propriétaire, l’authentification est réussie, et la découverte peut être autorisé à accès entre la bordure.

Approbation transitive entre les autorités de sécurité est le fondement de l’authentification ; le type d’authentification qui a lieu dans une frontière internationale est basé sur la confiance. Le gouvernement local ne connaît pas la découverte, mais qu’il approuve que le gouvernement de l’hôte ne. Lorsque le gouvernement hôte délivré le passeport, il ne sait pas soit le voyage. Il approuvé l’agence qui a émis le certificat de naissance ou d’autres documents. L’agence qui a émis le certificat de naissance, à son tour, le médecin qui a signé le certificat de confiance. Le médecin observée naissance du voyageur et marqué le certificat avec la preuve directe de l’identité, dans ce cas avec un encombrement de la nouveau-né. Approbation qui est transférée de cette façon, via des intermédiaires approuvés, doit être transitive.

Approbation transitive constitue la base de la sécurité réseau dans l’architecture de client/serveur Windows. Une relation d’approbation circule dans un ensemble de domaines, tels que d’une arborescence de domaine et établit une relation entre un domaine et tous les domaines qui approuvent ce domaine. Par exemple, si le domaine a une approbation transitive avec le domaine B, et si le domaine B approuve le domaine C, alors le domaine A approuve le domaine C.

Il existe une différence entre l’authentification et d’autorisation. Avec l’authentification, le système prouve que vous êtes bien qui vous prétendez qu'être. Avec l’autorisation, le système vérifie que vous disposez des droits à faire ce que vous voulez faire. Pour tirer l’analogie de la bordure à l’étape suivante, simplement l’authentification que la découverte est le propriétaire approprié d’un compte passport valide ne permet pas nécessairement du voyage d’entrer un pays. Résidents d’un pays particulier sont autorisés à entrer un autre pays en présentant simplement un passeport uniquement dans les cas où le pays qui sont entré accorde une autorisation illimitée pour tous les citoyens de ce pays particulier à entrer.

De même, vous pouvez accorder tous les utilisateurs à partir d’une certaines autorisations de domaine pour accéder à une ressource. Tout utilisateur qui appartient à ce domaine a accès à la ressource, tout comme le Canada permet aux États-Unis d’entrer au Canada. Toutefois, aux États-Unis de saisir le Brésil ou Inde trouvent que qu’ils ne peuvent pas entrer ces pays simplement en présentant un passeport, car les deux de ces pays nécessitent la visite pour avoir un visa valid aux États-Unis. Par conséquent, l’authentification ne garantit pas l’accès aux ressources ou d’autorisation d’utiliser des ressources.

## <a name="credentials"></a>Informations d’identification
Un compte passport et visa éventuellement associés est les informations d’identification acceptées pour un voyage. Toutefois, ces informations d’identification ne peuvent pas permettre à un voyageur Entrez ou accéder à toutes les ressources dans un pays. Par exemple, les informations d’identification supplémentaires sont requises afin d’assister à une conférence. Dans Windows, les informations d’identification peuvent être gérées pour le rendre possible pour les titulaires de compte accéder aux ressources sur le réseau sans avoir à plusieurs reprises fournir leurs informations d’identification. Ce type d’accès permet aux utilisateurs d’être authentifié une fois par le système pour accéder à toutes les applications et sources de données qu’ils sont autorisés à utiliser sans entrer un autre identificateur de compte ou mot de passe. La plateforme Windows tire parti de la possibilité d’utiliser une identité d’utilisateur unique (gérée par Active Directory) sur le réseau en cache localement les informations d’identification utilisateur autorité de sécurité locale (LSA) dans le système d’exploitation. Lorsqu’un utilisateur ouvre une session sur le domaine, les packages d’authentification Windows utilisent en toute transparence les informations d’identification pour fournir l’authentification unique lors de l’authentification les informations d’identification aux ressources réseau. Pour plus d’informations sur les informations d’identification, consultez [processus des informations d’identification dans l’authentification Windows](credentials-processes-in-windows-authentication.md).

Un formulaire d’authentification multifacteur pour la découverte peut être l’obligation d’effectuer et de présenter plusieurs documents pour authentifier son identité telles que les informations d’inscription une conférence et passport. Windows implémente ce formulaire ou l’authentification via des cartes à puce, des cartes à puce virtuelles et des technologies biométriques. 

## <a name="security-principals-and-accounts"></a>Comptes et entités de sécurité
Dans Windows, n’importe quel utilisateur, un service, un groupe ou un ordinateur qui peut initier l’action est une entité de sécurité. Entités de sécurité ont des comptes, ce qui peuvent être local sur un ordinateur ou basés sur un domaine. Par exemple, les ordinateurs clients joints au domaine Windows peuvent participer à un domaine de réseau en communiquant avec un contrôleur de domaine, même lorsque aucun utilisateur humain n’est connecté. Pour établir des communications, l’ordinateur doit avoir un compte actif dans le domaine. Avant d’accepter les communications à partir de l’ordinateur, l’autorité de sécurité local sur le contrôleur de domaine authentifie l’identité de l’ordinateur et il définit ensuite le contexte de sécurité de l’ordinateur comme il le ferait pour un principal de sécurité humaine. Ce contexte de sécurité définit l’identité et les capacités d’un utilisateur ou un service sur un ordinateur particulier ou un utilisateur, service, groupe, ordinateur ou sur un réseau. Par exemple, il définit les ressources, comme un partage de fichiers ou imprimante, qui sont accessibles et les actions, telles que la lecture, d’écriture ou de modification, qui peut être effectuée par un utilisateur, un service ou un ordinateur sur cette ressource. Pour plus d’informations, consultez [principaux de sécurité](https://technet.microsoft.com/itpro/windows/keep-secure/security-principals).

Un compte est un moyen pour identifier un demandeur--l’utilisateur humain ou le service--demande d’accès ou ressources. La découverte du qui détient le passeport authentique possède un compte avec le pays hôte. Les utilisateurs, groupes d’utilisateurs, les objets et les services peuvent tous avoir des comptes individuels ou partager des comptes. Comptes peuvent être membre des groupes et autorisations et droits spécifiques peuvent être alloués. Comptes peuvent être limités à l’ordinateur local, le groupe de travail, le réseau, ou être affectés à un domaine.

Les comptes intégrés et les groupes de sécurité, dont ils sont membres, sont définis sur chaque version de Windows. À l’aide de groupes de sécurité, vous pouvez affecter les mêmes autorisations de sécurité à de nombreux utilisateurs qui sont authentifiés avec succès, ce qui simplifie l’administration de l’accès. Règles d’émission de passeports peuvent nécessiter que la découverte du être affectée à certains groupes, tels que business, ou de tourisme ou government. Ce processus garantit des autorisations de sécurité cohérente entre tous les membres d’un groupe. À l’aide de groupes de sécurité pour affecter des autorisations signifie que le contrôle d’accès des ressources reste constante et facile à gérer et d’audit. En ajoutant et supprimant des utilisateurs qui doivent accéder à partir de groupes de sécurité appropriés en fonction des besoins, vous pouvez réduire la fréquence des modifications apportées au contrôle d’accès (ACL).

Autonome des comptes de service administrés et les comptes virtuels ont été introduits dans Windows Server 2008 R2 et Windows 7 pour fournir des applications nécessaires, telles que Microsoft Exchange Server et Internet Information Services (IIS), avec l’isolation de son propre domaine comptes, tout en éliminant le besoin d’un administrateur pour administrer manuellement le nom de principal du service (SPN) et les informations d’identification pour ces comptes. Comptes de service administrés de groupe ont été introduits dans Windows Server 2012 et fournit les mêmes fonctionnalités au sein du domaine, mais également les étend sur plusieurs serveurs. Lors de la connexion à un service hébergé sur une batterie de serveurs, tel que le service Équilibrage de la charge réseau, les protocoles d’authentification prenant en charge l’authentification mutuelle exigent que toutes les instances des services utilisent le même principal.

Pour plus d’informations sur les comptes, consultez :

-   [Comptes Active Directory](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-accounts)

-   [Groupes de sécurité Active Directory](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-security-groups)

-   [Comptes locaux](https://technet.microsoft.com/itpro/windows/keep-bastion.local-accounts)

-   [Comptes Microsoft](https://technet.microsoft.com/itpro/windows/keep-secure/microsoft-accounts)

-   [Comptes de service](https://technet.microsoft.com/itpro/windows/keep-secure/service-accounts)

-   [Identités spéciales](https://technet.microsoft.com/itpro/windows/keep-secure/special-identities)

## <a name="delegated-authentication"></a>Authentification déléguée
Pour utiliser l’analogie de voyage, pays peuvent émettre le même accès à tous les membres d’une délégation gouvernementale officielle, tant que les délégués sont bien connus. Cette délégation permet à un seul membre agissent sur l’autorité d’un autre membre. Dans Windows, la délégation d’authentification se produit lorsqu’un service de réseau accepte une demande d’authentification d’un utilisateur et utilise l’identité de cet utilisateur afin de lancer une nouvelle connexion à un deuxième service de réseau. Pour prendre en charge l’authentification déléguée, vous devez établir des serveurs frontaux ou de premier niveau, tels que les serveurs web, qui sont responsables de la gestion des demandes d’authentification client et les serveurs principaux ou des applications multicouches, telles que des bases de données volumineuses, qui sont responsables de stockage des informations. Vous pouvez déléguer le droit de configurer l’authentification déléguée aux utilisateurs de votre organisation afin de réduire la charge administrative de vos administrateurs.

En établissant un service ou un ordinateur comme approuvé pour la délégation, vous permettent de ce service ou cet ordinateur d’effectuer l’authentification déléguée, recevoir un ticket pour l’utilisateur qui effectue la demande et ensuite accéder aux informations de cet utilisateur. Ce modèle limite l’accès de données sur les serveurs principaux simplement pour les utilisateurs ou les services que présentes informations d’identification avec les jetons de contrôle d’accès correct. En outre, il permet l’audit de l’accès de ces ressources back-end. En exigeant que toutes les données accessibles au moyen des informations d’identification déléguées pour le serveur à utiliser pour le compte du client, vous vous assurer que le serveur ne peut pas être compromis et que vous pouvez accéder à des informations sensibles qui est stockée sur d’autres serveurs. Authentification déléguée est utile pour les applications multicouches qui sont conçues pour utiliser les fonctionnalités d’authentification uniques sur plusieurs ordinateurs.

### <a name="authentication-in-trust-relationships-between-domains"></a>Authentification dans les relations d’approbation entre domaines
La plupart des organisations qui ont plusieurs domaines légitimement besoin pour les utilisateurs d’accéder aux ressources partagées qui sont trouvent dans un domaine différent, tout comme la découverte est autorisée voyage vers différentes régions du pays. Contrôle de cet accès requiert que les utilisateurs dans un domaine peuvent également être authentifiés et autorisés à utiliser des ressources dans un autre domaine. Pour fournir des fonctionnalités d’authentification et d’autorisation entre clients et serveurs dans des domaines différents, une relation d’approbation doit exister entre les deux domaines. Les approbations sont la technologie sous-jacente par lequel les communications sécurisées de Active Directory se produisent et sont un composant de sécurité intégrale de l’architecture de réseau Windows Server.

Lorsqu’il existe une relation d’approbation entre deux domaines, les mécanismes d’authentification pour chaque domaine d’approbation les authentifications en provenance de l’autre domaine. Approbations assurent de contrôle d’accès aux ressources partagées dans un domaine de ressources--le domaine d’approbation--en vérifiant que l’authentification entrante les demandes proviennent d’une autorité approuvée, le domaine approuvé. De cette façon, les approbations servent de ponts qui permettent uniquement validé le voyage de demandes d’authentification entre domaines.

Façon dont une approbation spécifique transmet les demandes d’authentification dépend de la façon dont elle est configurée. Relations d’approbation peuvent être unidirectionnelle, en fournissant un accès aux ressources du domaine d’approbation du domaine approuvé ou bidirectionnelle, en fournissant un accès aux ressources dans l’autre domaine de chaque domaine. Approbations sont également soit non transitive, dans lequel cas approbation existe uniquement entre les domaines de partenaires de confiance de deux, ou transitif, auquel cas approbation s’étend automatiquement à toutes les autres domaines qui fait confiance à celui des partenaires.

Pour plus d’informations sur le fonctionne d’une relation d’approbation, consultez [How Domain and Forest Trusts Work](https://technet.microsoft.com/library/cc773178(v=ws.10).aspx).

### <a name="protocol-transition"></a>Transition de protocole
Transition de protocole aide les concepteurs d’applications en laissant les applications prennent en charge différents mécanismes d’authentification au niveau de l’authentification utilisateur et en basculant vers le protocole Kerberos pour les fonctionnalités de sécurité, telles que l’authentification mutuelle et la délégation contrainte, dans les niveaux d’application suivantes.

Pour plus d’informations sur la transition de protocole, consultez [Kerberos Protocol Transition and Constrained Delegation](https://technet.microsoft.com/library/cc758097(v=ws.10).aspx).

### <a name="constrained-delegation"></a>Délégation contrainte
La délégation contrainte donne aux administrateurs la possibilité de spécifier et d’appliquer des limites d’approbation application en limitant l’étendue dans laquelle les services d’application peuvent agir pour le compte d’un utilisateur. Vous pouvez spécifier des services particuliers à partir de laquelle un ordinateur qui est approuvé pour la délégation peut demander des ressources. La possibilité de limiter les droits d’autorisation pour les services vous aide à améliorer la conception de sécurité d’application en réduisant les opportunités de compromission de services non approuvés.

Pour plus d’informations sur la délégation, consultez [présentation de la délégation Kerberos contrainte](../kerberos/kerberos-constrained-delegation-overview.md).

## <a name="see-also"></a>Voir aussi
[Vue d’ensemble technique d’ouverture de session Windows et authentification](https://technet.microsoft.com/library/dn269029.aspx)


