---
title: Concepts de l’authentification Windows
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 46a0f6c4c08146f1d8fcf9de45446b974e48ecac
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402335"
---
# <a name="windows-authentication-concepts"></a>Concepts de l’authentification Windows

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique de vue d’ensemble de référence décrit les concepts basés sur l’authentification Windows.

L’authentification est un processus de vérification de l’identité d’un objet ou d’une personne. Lorsque vous authentifiez un objet, l’objectif est de vérifier que celui-ci est authentique. Lorsque vous authentifiez une personne, l’objectif est de vérifier que la personne n’est pas un imposteur.

Dans un contexte de réseau, l’authentification est le fait de prouver une identité à une application ou une ressource réseau. En général, l’identité est prouvée par une opération de chiffrement qui utilise une clé uniquement que l’utilisateur connaît (comme avec le chiffrement à clé publique) ou une clé partagée. Le côté serveur de l’échange d’authentification compare les données signées avec une clé de chiffrement connue pour valider la tentative d’authentification.

Le stockage des clés de chiffrement dans un emplacement centralisé sécurisé rend le processus d’authentification évolutif et facile à gérer. Active Directory est la technologie par défaut recommandée pour le stockage des informations d’identité, notamment les clés de chiffrement qui sont les informations d’identification de l’utilisateur. Active Directory est requis pour les implémentations NTLM et Kerberos par défaut.

Les techniques d’authentification vont d’une ouverture de session simple à un système d’exploitation ou d’une connexion à un service ou une application, qui identifie les utilisateurs en fonction de ce que seul l’utilisateur connaît, comme un mot de passe, à des mécanismes de sécurité plus puissants qui utilisent un l’utilisateur has’such en tant que jetons, certificats de clé publique, images ou attributs biologiques. Dans un environnement d’entreprise, les utilisateurs peuvent accéder à plusieurs applications sur plusieurs types de serveurs dans un même emplacement ou dans plusieurs emplacements. C’est pourquoi l’authentification doit prendre en charge des environnements pour d’autres plateformes et pour d’autres systèmes d’exploitation Windows.

## <a name="authentication-and-authorization-a-travel-analogy"></a>Authentification et autorisation : Une analogie avec les voyages
Une analogie de voyages peut vous aider à expliquer le fonctionnement de l’authentification. Quelques tâches préparatoires sont généralement nécessaires pour commencer le trajet. Le voyageur doit prouver sa véritable identité à ses autorités hôtes. Cette preuve peut se présenter sous la forme d’une preuve de citoyenneté, d’un lieu de naissance, d’un bon personnel, de photographies ou de toute autre obligation de la loi du pays hôte. L’identité du voyageur est validée par l’émission d’un Passport, qui est analogue à un compte système émis et administré par une organisation, le principal de sécurité. Le passeport et la destination prévue sont basés sur un ensemble de règles et de réglementations émises par l’autorité gouvernementale.

**Le trajet**

Lorsque le voyageur arrive à la frontière internationale, une protection des frontières demande des informations d’identification et le voyageur présente son passeport. Le processus est à deux plis :

-   La protection authentifie le Passport en vérifiant qu’il a été émis par une autorité de sécurité approuvée par l’administration locale (approuve, au moins, pour émettre des passeports) et en vérifiant que le Passport n’a pas été modifié.

-   La protection authentifie le voyageur en vérifiant que la face correspond à la face de la personne représenté sur le passeport et que les autres informations d’identification requises sont en bon ordre.

Si le passeport est valable et que le voyageur s’avère être son propriétaire, l’authentification réussit et le voyageur peut être autorisé à accéder à travers la frontière.

L’approbation transitive entre les autorités de sécurité est la base de l’authentification ; le type d’authentification qui a lieu au niveau d’une frontière internationale est basé sur la confiance. Le gouvernement local ne connaît pas le voyageur, mais il fait confiance à l’administration de l’hôte. Lorsque le gouvernement de l’hôte a émis le Passport, il n’a pas eu connaissance de la voyageur. Il a approuvé l’Agence qui a émis le certificat de naissance ou une autre documentation. L’Agence qui a émis le certificat de naissance a, à son tour, approuvé le médecin qui a signé le certificat. Le médecin a fait la naissance du voyageur et a marqué le certificat avec une preuve directe de l’identité, dans ce cas avec l’empreinte de l’Newborn. L’approbation qui est transférée de cette manière, par le biais d’intermédiaires approuvés, est transitive.

L’approbation transitive est la base de la sécurité réseau dans l’architecture client/serveur Windows. Une relation d’approbation circule dans un ensemble de domaines, par exemple une arborescence de domaine, et constitue une relation entre un domaine et tous les domaines qui approuvent ce domaine. Par exemple, si le domaine A a une approbation transitive avec le domaine B et si le domaine B approuve le domaine C, le domaine A approuve le domaine C.

Il existe une différence entre l’authentification et l’autorisation. Avec l’authentification, le système prouve que vous êtes bien celui que vous dites. Avec l’autorisation, le système vérifie que vous disposez des droits nécessaires pour effectuer ce que vous voulez faire. Pour effectuer l’analogie avec la frontière à l’étape suivante, il suffit d’authentifier que le voyageur est le propriétaire approprié d’un Passport valide n’autorise pas nécessairement le voyageur à entrer dans un pays. Les résidents d’un pays donné sont autorisés à entrer dans un autre pays en présentant simplement un passeport uniquement dans les cas où le pays en cours de saisie accorde une autorisation illimitée à tous les citoyens de ce pays particulier.

De même, vous pouvez accorder à tous les utilisateurs d’une certaine autorisation de domaine pour accéder à une ressource. Tout utilisateur qui appartient à ce domaine a accès à la ressource, tout comme le Canada permet aux citoyens américains de saisir le Canada. Toutefois, les citoyens américains tentant de pénétrer au Brésil ou en Inde peuvent constater qu’ils ne peuvent pas entrer dans ces pays simplement en présentant un passeport, car ces deux pays imposent aux citoyens américains d’avoir un visa valide. Ainsi, l’authentification ne garantit pas l’accès aux ressources ou l’autorisation d’utiliser des ressources.

## <a name="credentials"></a>Informations d’identification
Un compte Passport et éventuellement des visas associés sont les informations d’identification acceptées pour un voyageur. Toutefois, ces informations d’identification peuvent ne pas permettre à un voyageur d’entrer ou d’accéder à toutes les ressources au sein d’un pays. Par exemple, des informations d’identification supplémentaires sont requises pour assister à une conférence. Dans Windows, les informations d’identification peuvent être gérées pour permettre aux titulaires de comptes d’accéder aux ressources sur le réseau sans avoir à fournir leurs informations d’identification à plusieurs reprises. Ce type d’accès permet aux utilisateurs d’être authentifiés une fois par le système pour accéder à toutes les applications et sources de données qu’ils sont autorisés à utiliser sans entrer un autre identificateur de compte ou mot de passe. La plate-forme Windows capitalise sur la possibilité d’utiliser une identité d’utilisateur unique (conservée par Active Directory) sur le réseau en mettant localement en cache les informations d’identification de l’utilisateur dans l’autorité de sécurité locale (LSA) du système d’exploitation. Lorsqu’un utilisateur ouvre une session sur le domaine, les packages d’authentification Windows utilisent en toute transparence les informations d’identification pour fournir une authentification unique lors de l’authentification des informations d’identification auprès des ressources réseau. Pour plus d’informations sur les informations d’identification, consultez [processus d’informations d’identification dans l’authentification Windows](credentials-processes-in-windows-authentication.md).

Une forme de Multi-Factor Authentication pour le voyageur peut être l’obligation de transporter et présenter plusieurs documents pour authentifier son identité, par exemple un passeport et des informations d’inscription de conférence. Windows implémente ce formulaire ou cette authentification via des cartes à puce, des cartes à puce virtuelles et des technologies biométriques. 

## <a name="security-principals-and-accounts"></a>Principaux de sécurité et comptes
Dans Windows, tout utilisateur, service, groupe ou ordinateur pouvant lancer une action est un principal de sécurité. Les principaux de sécurité ont des comptes, qui peuvent être locaux à un ordinateur ou être basés sur un domaine. Par exemple, les ordinateurs appartenant à un domaine Windows client peuvent participer à un domaine réseau en communiquant avec un contrôleur de domaine même si aucun utilisateur humain n’est connecté. Pour initier des communications, l’ordinateur doit disposer d’un compte actif dans le domaine. Avant d’accepter les communications à partir de l’ordinateur, l’autorité de sécurité locale sur le contrôleur de domaine authentifie l’identité de l’ordinateur, puis définit le contexte de sécurité de l’ordinateur de la même façon que pour un principal de sécurité humain. Ce contexte de sécurité définit l’identité et les fonctionnalités d’un utilisateur ou d’un service sur un ordinateur particulier ou un utilisateur, un service, un groupe ou un ordinateur sur un réseau. Par exemple, il définit les ressources, telles qu’un partage de fichiers ou une imprimante, accessibles et les actions, telles que la lecture, l’écriture ou la modification, qui peuvent être effectuées par un utilisateur, un service ou un ordinateur sur cette ressource. Pour plus d’informations, consultez [principaux de sécurité](https://technet.microsoft.com/itpro/windows/keep-secure/security-principals).

Un compte est un moyen d’identifier un demandeur (le service ou l’utilisateur humain) qui demande l’accès ou les ressources. Le voyageur qui détient le Passport authentique possède un compte avec le pays hôte. Les utilisateurs, les groupes d’utilisateurs, les objets et les services peuvent tous avoir des comptes individuels ou partager des comptes. Les comptes peuvent être membres de groupes et peuvent recevoir des autorisations et des droits spécifiques. Les comptes peuvent être limités à l’ordinateur local, au groupe de travail, au réseau ou être affectés à un domaine.

Les comptes intégrés et les groupes de sécurité, dont ils sont membres, sont définis sur chaque version de Windows. À l’aide de groupes de sécurité, vous pouvez attribuer les mêmes autorisations de sécurité à de nombreux utilisateurs qui sont authentifiés, ce qui simplifie l’administration de l’accès. Les règles d’émission de passeports peuvent exiger que le voyageur soit attribué à certains groupes, tels que les activités commerciales, touristiques ou gouvernementaux. Ce processus garantit des autorisations de sécurité cohérentes pour tous les membres d’un groupe. En utilisant des groupes de sécurité pour affecter des autorisations, le contrôle d’accès des ressources reste constant et facile à gérer et audit. En ajoutant et en supprimant des utilisateurs qui requièrent l’accès à partir des groupes de sécurité appropriés, vous pouvez réduire la fréquence des modifications apportées aux listes de contrôle d’accès (ACL).

Les comptes de service administrés autonomes et les comptes virtuels ont été introduits dans Windows Server 2008 R2 et Windows 7 pour fournir les applications nécessaires, telles que Microsoft Exchange Server et Internet Information Services (IIS), avec l’isolation de leur propre domaine comptes, tout en éliminant le besoin d’un administrateur pour administrer manuellement le nom de principal du service (SPN) et les informations d’identification pour ces comptes. Les comptes de service administrés de groupe ont été introduits dans Windows Server 2012 et offrent les mêmes fonctionnalités dans le domaine, mais étendent également cette fonctionnalité sur plusieurs serveurs. Lors de la connexion à un service hébergé sur une batterie de serveurs, tel que le service Équilibrage de la charge réseau, les protocoles d’authentification prenant en charge l’authentification mutuelle exigent que toutes les instances des services utilisent le même principal.

Pour plus d’informations sur les comptes, consultez :

-   [Comptes de Active Directory](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-accounts)

-   [Active Directory des groupes de sécurité](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-security-groups)

-   [Comptes locaux](https://technet.microsoft.com/itpro/windows/keep-bastion.local-accounts)

-   [Comptes Microsoft](https://technet.microsoft.com/itpro/windows/keep-secure/microsoft-accounts)

-   [Comptes de service](https://technet.microsoft.com/itpro/windows/keep-secure/service-accounts)

-   [Identités spéciales](https://technet.microsoft.com/itpro/windows/keep-secure/special-identities)

## <a name="delegated-authentication"></a>Authentification déléguée
Pour utiliser l’analogie des voyages, les pays peuvent émettre le même accès à tous les membres d’une délégation gouvernementale officielle, à condition que les délégués soient bien connus. Cette délégation permet à un membre d’agir sur l’autorité d’un autre membre. Dans Windows, l’authentification déléguée se produit lorsqu’un service réseau accepte une demande d’authentification d’un utilisateur et utilise l’identité de cet utilisateur pour initialiser une nouvelle connexion à un deuxième service réseau. Pour prendre en charge l’authentification déléguée, vous devez établir des serveurs frontaux ou de premier niveau, tels que des serveurs Web, qui sont responsables de la gestion des demandes d’authentification du client et des serveurs principaux ou multicouches, tels que les bases de données volumineuses, qui sont responsables de stockage des informations. Vous pouvez déléguer le droit de configurer l’authentification déléguée aux utilisateurs de votre organisation afin de réduire la charge administrative sur vos administrateurs.

En établissant un service ou un ordinateur approuvé pour la délégation, vous laissez ce service ou cet ordinateur terminer l’authentification déléguée, recevoir un ticket pour l’utilisateur qui effectue la demande, puis accéder aux informations de cet utilisateur. Ce modèle restreint l’accès aux données sur les serveurs principaux uniquement aux utilisateurs ou services qui présentent les informations d’identification avec les jetons de contrôle d’accès corrects. En outre, il permet l’audit d’accès de ces ressources principales. En exigeant que toutes les données soient accessibles au moyen des informations d’identification déléguées au serveur pour une utilisation pour le compte du client, vous vous assurez que le serveur ne peut pas être compromis et que vous pouvez accéder à des informations sensibles stockées sur d’autres serveurs. L’authentification déléguée est utile pour les applications multicouches conçues pour utiliser les fonctionnalités d’authentification unique sur plusieurs ordinateurs.

### <a name="authentication-in-trust-relationships-between-domains"></a>Authentification dans les relations d’approbation entre les domaines
La plupart des organisations qui ont plusieurs domaines ont un besoin légitime que les utilisateurs accèdent aux ressources partagées qui se trouvent dans un domaine différent, tout comme le voyageur est autorisé à se déplacer vers différentes régions du pays. Le contrôle de cet accès nécessite que les utilisateurs d’un domaine puissent également être authentifiés et autorisés à utiliser des ressources dans un autre domaine. Pour fournir des fonctionnalités d’authentification et d’autorisation entre les clients et les serveurs dans des domaines différents, il doit exister une relation d’approbation entre les deux domaines. Les approbations sont la technologie sous-jacente par laquelle les communications Active Directory sécurisées se produisent et sont un composant de sécurité intégrale de l’architecture réseau de Windows Server.

Lorsqu’il existe une approbation entre deux domaines, les mécanismes d’authentification de chaque domaine approuvent les authentifications provenant de l’autre domaine. Les approbations permettent un accès contrôlé aux ressources partagées dans un domaine de ressource (le domaine d’approbation) en vérifiant que les demandes d’authentification entrantes proviennent d’une autorité approuvée (le domaine approuvé). De cette façon, les approbations agissent comme des ponts qui permettent uniquement aux demandes d’authentification validées de circuler entre les domaines.

La façon dont une approbation spécifique passe les demandes d’authentification dépend de la façon dont elles sont configurées. Les relations d’approbation peuvent être unidirectionnelles, en fournissant l’accès à partir du domaine approuvé aux ressources du domaine d’approbation, ou bidirectionnel, en fournissant un accès à partir de chaque domaine aux ressources de l’autre domaine. Les approbations sont également non transitives, auquel cas l’approbation existe uniquement entre les deux domaines de partenaire d’approbation, ou transitive, auquel cas l’approbation s’étend automatiquement à tous les autres domaines approuvés par l’un des partenaires.

Pour plus d’informations sur le fonctionnement d’une approbation, consultez fonctionnement des [approbations de domaine et de forêt](https://technet.microsoft.com/library/cc773178(v=ws.10).aspx).

### <a name="protocol-transition"></a>Transition de protocole
La transition de protocole aide les concepteurs d’applications en permettant aux applications de prendre en charge différents mécanismes d’authentification au niveau de l’authentification utilisateur et en basculant sur le protocole Kerberos pour les fonctionnalités de sécurité, telles que l’authentification mutuelle et délégation avec restriction, dans les couches d’application suivantes.

Pour plus d’informations sur la transition de protocole, consultez [transition de protocole Kerberos et délégation avec restriction](https://technet.microsoft.com/library/cc758097(v=ws.10).aspx).

### <a name="constrained-delegation"></a>Délégation contrainte
La délégation contrainte permet aux administrateurs de spécifier et d’appliquer les limites d’approbation d’application en limitant l’étendue dans laquelle les services d’application peuvent agir pour le compte d’un utilisateur. Vous pouvez spécifier des services particuliers à partir desquels un ordinateur approuvé pour la délégation peut demander des ressources. La flexibilité pour limiter les droits d’autorisation pour les services permet d’améliorer la conception de la sécurité des applications en réduisant les risques de compromission par des services non fiables.

Pour plus d’informations sur la délégation avec restriction, consultez [vue d’ensemble de la délégation Kerberos Kerberos](../kerberos/kerberos-constrained-delegation-overview.md).

## <a name="see-also"></a>Voir aussi
[Présentation technique de l’authentification et de l’ouverture de session Windows](https://technet.microsoft.com/library/dn269029.aspx)


