---
title: "Concepts de l’authentification Windows"
description: "Sécurité de Windows Server"
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
ms.openlocfilehash: 27e124c971926edd33f102fe6009a0c552c6d814
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="windows-authentication-concepts"></a>Concepts de l’authentification Windows

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique de vue d’ensemble de référence décrit les concepts sur lesquels repose l’authentification Windows.

L’authentification est un processus de vérification de l’identité d’un objet ou une personne. Lorsque vous authentifiez un objet, l’objectif est de vérifier que l’objet est authentique. Lorsque vous authentifiez une personne, l’objectif est de vérifier que la personne n’est pas un imposteur.

Dans un contexte de mise en réseau, l’authentification est le fait de prouver l’identité pour une application de réseau ou une ressource. En règle générale, identité est prouvée par une opération de chiffrement qui utilise une clé seulement l’utilisateur connaît (comme avec chiffrement à clé publique) ou une clé partagée. Le côté serveur de l’échange d’authentification compare les données signées avec une clé de chiffrement connue pour valider la tentative d’authentification.

Stockage de clés de chiffrement dans un emplacement centralisé sécurisé rend le processus d’authentification évolutif et facile à gérer. Active Directory est l’architecture recommandée et la technologie par défaut pour le stockage des informations d’identité, qui incluent le chiffrement clés qui sont des informations d’identification de l’utilisateur. Active Directory est requis pour les implémentations Kerberos et les par défaut NTLM.

Techniques d’authentification vont à partir d’une simple ouverture de session à un système d’exploitation ou d’une connexion à un service ou une application, qui identifie les utilisateurs en fonction quelque chose que seul l’utilisateur connaît, par exemple, un mot de passe, des mécanismes de sécurité plus puissants qui utilisent des éléments que l’utilisateur has'such jetons, les certificats de clé publique, des images ou des attributs biologiques. Dans un environnement d’entreprise, les utilisateurs peuvent accéder à plusieurs applications sur plusieurs types de serveurs dans un même emplacement ou dans plusieurs emplacements. Pour ces raisons, l’authentification doit prendre en charge les environnements pour d’autres plateformes et autres systèmes d’exploitation Windows.

## <a name="authentication-and-authorization-a-travel-analogy"></a>Authentification et autorisation: une analogie de voyage
Une analogie voyage peut aider à expliquer le fonctionne de l’authentification. Certaines tâches préparatoires sont généralement nécessaires pour commencer le voyage. La découverte doit prouver leur identité true aux autorités de leur ordinateur hôte. Cette démonstration peut être sous la forme d’une preuve de votre nationalité, place de naissance, un document personnel, photographies ou tout ce qui est requis par la loi du pays hôte. L’identité de la découverte est validée par l’émission d’un compte passport, qui est similaire à un compte système émis et gérés par une organisation--l’entité de sécurité. Le compte passport et la destination sont basés sur un ensemble de règles et les réglementations émises par l’autorité publique.

**Le voyage**

Lorsque la découverte arrive à la frontière internationale, un garde bordure vous demande d’informations d’identification et la découverte présente son passport. Le processus est double:

-   La protection authentifie le passport en vérifiant qu’il a été émis par une autorité de sécurité que le gouvernement local approbations (approbations, au moins, pour émettre des passeports) et en vérifiant que le compte passport n’a pas été modifié.

-   La protection s’authentifie le voyage en vérifiant que la face correspond à la face de la personne représentée sur le compte passport et que les autres informations d’identification requises sont en bon état.

Si le compte passport se révèle valide et la découverte s’avère pour être le propriétaire, l’authentification est réussie, et la découverte peut être autorisé à accéder entre la bordure.

Approbation transitive entre les autorités de sécurité constitue le fondement d’authentification; le type d’authentification qui a lieu à une bordure internationale est basé sur l’approbation. Le gouvernement local ne sait pas la découverte, mais il approuve que le gouvernement hôte ne. Lorsque le gouvernement hôte émis le passport, il ne savais pas la découverte soit. Il approuvé l’organisme qui a émis le certificat de naissance ou d’autres documents. L’organisme qui a émis le certificat de naissance, à son tour, les médecins qui a signé le certificat de confiance. Les médecins surveillée naissance de la découverte et horodatée dans ce cas le certificat direct preuve d’identité, encombrement de le nouveau-né. Approbation qui est transférée de cette façon, par le biais d’intermédiaires approuvés, doit être transitive.

Approbation transitive constitue le fondement de la sécurité réseau dans une architecture client/serveur de Windows. Une relation d’approbation circule dans un ensemble de domaines, par exemple, une arborescence de domaine et établit une relation entre un domaine et tous les domaines qui approuvent ce domaine. Par exemple, si le domaine a une approbation transitive avec le domaine B, et si le domaine B approuve le domaine C, alors le domaine A approuve le domaine C.

Il existe une différence entre l’authentification et d’autorisation. Avec l’authentification, le système s’avère que vous êtes qui vous prétendez qu'être. Avec l’autorisation, le système vérifie que vous disposez des droits ce que vous voulez faire. Pour tirer l’analogie bordure à l’étape suivante, simplement l’authentification que la découverte est le propriétaire d’un compte passport valide approprié n’autorise pas les nécessairement le voyage à entrer un pays. Résidents d’un pays donné sont autorisés à entrer un autre pays en présentant simplement un compte passport uniquement dans les situations où le pays entré accorde à un nombre illimité d’autorisation pour tous les citoyens de ce pays particulier à entrer.

De même, vous pouvez accorder tous les utilisateurs à partir d’une certaines autorisations de domaine pour accéder à une ressource. Tout utilisateur qui appartient à ce domaine a accès à la ressource, comme Canada permet d’entrer du Canada citoyens aux États-Unis. Toutefois, les citoyens américains tentant d’entrer Brésil ou Inde trouvent que qu’ils ne peuvent pas entrer ces pays simplement en présentant un compte passport, car les deux de ces pays nécessitent visitant citoyens américains pour avoir un visa valide. Par conséquent, l’authentification ne garantit pas accès aux ressources ou d’autorisation d’utiliser des ressources.

## <a name="credentials"></a>Informations d’identification
Un compte passport et visa éventuellement associées est les informations d’identification acceptées pour un voyage. Toutefois, ces informations d’identification pas permet à un voyage Entrez ou d’accéder à toutes les ressources dans un pays. Par exemple, les informations d’identification supplémentaires sont requis pour participer à une conférence. Dans Windows, les informations d’identification peuvent être gérées afin de permettre aux détenteurs de compte accéder aux ressources sur le réseau sans avoir à plusieurs reprises à fournir leurs informations d’identification. Ce type d’accès permet aux utilisateurs d’être authentifié une fois par le système pour accéder à toutes les applications et des sources de données qu’ils sont autorisés à utiliser sans avoir à entrer un autre identificateur de compte ou mot de passe. La plateforme Windows tire parti de la possibilité d’utiliser une identité d’utilisateur unique (gérée par Active Directory) sur le réseau en mettant en cache localement les informations d’identification utilisateur autorité de sécurité locale (LSA) dans le système d’exploitation. Lorsqu’un utilisateur ouvre une session sur le domaine, les packages d’authentification Windows utilisent en toute transparence les informations d’identification pour fournir l’ouverture de session unique lors de l’authentification les informations d’identification aux ressources réseau. Pour plus d’informations sur les informations d’identification, voir [processus des informations d’identification dans l’authentification Windows](credentials-processes-in-windows-authentication.md).

Une forme d’authentification multifacteur pour la découverte peut être la configuration requise pour exécuter et de présenter plusieurs documents pour authentifier son identité tels que des informations d’inscription d’un passport et de conférence. Windows implémente ce formulaire ou une authentification par le biais de cartes à puce, les cartes à puce virtuelles et technologies biométriques. 

## <a name="security-principals-and-accounts"></a>Comptes et les entités de sécurité
Dans Windows, n’importe quel utilisateur, un service, un groupe ou un ordinateur qui permettre initier une action est une entité de sécurité. Entités de sécurité disposent de comptes, ce qui peuvent se trouver sur un ordinateur ou être basés sur un domaine. Par exemple, les ordinateurs clients joints au domaine Windows peuvent participer à un domaine de réseau en communiquant avec un contrôleur de domaine, même lorsque aucun utilisateur humaine est connecté à. Pour établir des communications, l’ordinateur doit avoir un compte actif dans le domaine. Avant d’accepter les communications à partir de l’ordinateur, l’autorité de sécurité local sur le contrôleur de domaine authentifie l’identité de l’ordinateur et il définit ensuite le contexte de sécurité de l’ordinateur comme il le ferait pour un principal de sécurité humaine. Ce contexte de sécurité définit l’identité et les fonctionnalités d’un utilisateur ou un service sur un ordinateur particulier ou un utilisateur, service, groupe ou un ordinateur sur un réseau. Par exemple, il définit les ressources, par exemple, un partage de fichiers ou l’imprimante, qui sont accessibles et les actions, telles que la lecture, écriture ou de modification, qui peut être effectuée par un utilisateur, un service ou un ordinateur sur cette ressource. Pour plus d’informations, voir [principaux de sécurité](https://technet.microsoft.com/itpro/windows/keep-secure/security-principals).

Un compte est un moyen d’identifier requérant--l’utilisateur humaine ou le service--demande d’accès ou ressources. La découverte qui détient le passeport authentique possède un compte avec le pays de l’ordinateur hôte. Les utilisateurs, groupes d’utilisateurs, les objets et les services peuvent tous avoir des comptes individuels ou partager des comptes. Comptes peuvent être membres de groupes et peuvent recevoir des autorisations et droits spécifiques. Comptes peuvent être limités à l’ordinateur local, le groupe de travail, le réseau, ou être affectés l’appartenance à un domaine.

Comptes intégrés et les groupes de sécurité, dont ils sont membres, sont définies sur chaque version de Windows. À l’aide de groupes de sécurité, vous pouvez attribuer les mêmes autorisations de sécurité à de nombreux utilisateurs authentifiés avec succès, ce qui simplifie l’administration de l’accès. Règles d’émission de passeports peuvent nécessiter que la découverte être attribué à certains groupes, tels que les entreprises, ou touristiques ou gouvernement. Ce processus garantit les autorisations de sécurité cohérente sur tous les membres d’un groupe. À l’aide de groupes de sécurité pour affecter des autorisations signifie que le contrôle des ressources d’accès reste constante et facile à gérer et d’audit. En ajoutant ou supprimant des utilisateurs qui souhaitent accéder à partir de groupes de sécurité appropriés en fonction des besoins, vous pouvez réduire la fréquence des modifications apportées au contrôle d’accès (ACL).

Autonome des comptes de service administrés et les comptes virtuels ont été introduits dans Windows Server 2008 R2 et Windows 7 pour fournir les applications nécessaires, telles que Microsoft Exchange Server et Internet Information Services (IIS), d’isoler leurs propres comptes de domaine, alors qu’en éliminant la nécessité pour un administrateur manuellement administrer le nom principal de service (SPN) et les informations d’identification de ces comptes. Comptes de service administrés de groupe ont été introduits dans Windows Server 2012 et fournit les mêmes fonctionnalités au sein du domaine, mais également les étend sur plusieurs serveurs. Lors de la connexion à un service hébergé sur une batterie de serveurs, par exemple, l’équilibrage de la charge réseau, les protocoles d’authentification prenant en charge l’authentification mutuelle nécessitent que toutes les instances des services utilisent le même principal.

Pour plus d’informations sur les comptes, voir:

-   [Les comptes Active Directory](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-accounts)

-   [Groupes de sécurité Active Directory](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-security-groups)

-   [Comptes locaux](https://technet.microsoft.com/itpro/windows/keep-secure/local-accounts)

-   [Comptes Microsoft](https://technet.microsoft.com/itpro/windows/keep-secure/microsoft-accounts)

-   [Comptes de service](https://technet.microsoft.com/itpro/windows/keep-secure/service-accounts)

-   [Identités spéciales](https://technet.microsoft.com/itpro/windows/keep-secure/special-identities)

## <a name="delegated-authentication"></a>Authentification déléguée
Pour utiliser le voyage analogie, pays peuvent émettre des mêmes droits d’accès à tous les membres d’une délégation gouvernementale officielle, tant que les délégués sont bien connus. Cette délégation permet un membre d’agir sur l’autorité d’un autre membre. Dans Windows, l’authentification déléguée se produit lorsqu’un service de réseau accepte une demande d’authentification d’un utilisateur et l’identité de cet utilisateur, il est supposé afin de lancer une nouvelle connexion à un deuxième service réseau. Pour prendre en charge l’authentification déléguée, vous devez établir les serveurs frontaux ou de premier niveau, tels que les serveurs web, qui sont chargés de traiter les demandes d’authentification client et les serveurs principaux ou multicouche, tels que les bases de données volumineuses, qui sont chargés pour le stockage des informations. Vous pouvez déléguer le droit de configurer l’authentification déléguée aux utilisateurs de votre organisation afin de réduire la charge administrative de vos administrateurs.

En établissant un service ou un ordinateur comme approuvé pour délégation, vous permettent de ce service ou un ordinateur effectuer l’authentification déléguée, recevoir un ticket pour l’utilisateur qui effectue la demande et ensuite accéder aux informations de cet utilisateur. Ce modèle limite accès aux données sur les serveurs principaux uniquement pour les utilisateurs ou les services que présent informations d’identification avec les jetons de contrôle d’accès correctes. En outre, il permet l’audit d’accès de ces ressources principal. En exigeant que toutes les données accessibles au moyen des informations d’identification déléguées pour le serveur à utiliser pour le compte du client, vous vous assurer que le serveur ne peut pas être compromis et que vous pouvez accéder aux informations sensibles qui est stocké sur d’autres serveurs. L’authentification déléguée est utile pour les applications à plusieurs couches qui sont conçues pour utiliser les fonctions d’authentification uniques sur plusieurs ordinateurs.

### <a name="authentication-in-trust-relationships-between-domains"></a>Authentification dans les relations d’approbation entre domaines
La plupart des organisations qui ont plusieurs domaines légitimement besoin pour les utilisateurs d’accéder aux ressources partagées qui sont trouvent dans un domaine différent, tout comme la découverte est autorisé voyage dans différentes régions, dans le pays. Contrôle de cet accès nécessite que les utilisateurs dans un domaine peuvent également être authentifiés et autorisés à utiliser des ressources dans un autre domaine. Pour fournir des fonctionnalités d’authentification et d’autorisation entre les clients et serveurs dans des domaines différents, il doit être une relation d’approbation entre les deux domaines. Les approbations sont la technologie sous-jacente par lequel des communications sécurisées Active Directory se produisent et sont un composant de sécurité intégrale de l’architecture de réseau Windows Server.

Lorsqu’il existe une relation d’approbation entre deux domaines, les mécanismes d’authentification pour chaque domaine approuvent les authentifications en provenance de l’autre domaine. Approbations vous permettent de contrôle d’accès aux ressources partagées dans un domaine de ressources--du domaine d’approbation--en vérifiant que l’authentification entrante demandes proviennent d’une autorité approuvée--le domaine approuvé. De cette façon, les approbations servent de ponts qui permettent uniquement validé voyage de demandes d’authentification entre domaines.

Comment une relation d’approbation spécifique transmet les demandes d’authentification dépend de la façon dont il est configuré. Relations d’approbation peuvent être à sens unique, en fournissant un accès aux ressources du domaine d’approbation du domaine approuvé ou bidirectionnelle, en fournissant un accès aux ressources de l’autre domaine de chaque domaine. Approbations sont également soit non transitive, dans laquelle il existe une approbation cas uniquement entre les domaines de partenaire de deux confiance, ou transitive, auquel cas approbation étend automatiquement à toutes les autres domaines approbations d’un des partenaires.

Pour plus d’informations sur le fonctionne d’une relation d’approbation, voir [comment le domaine et le fonctionnement des approbations de forêt](https://technet.microsoft.com/library/cc773178(v=ws.10).aspx).

### <a name="protocol-transition"></a>Transition de protocole
Transition de protocole permet aux concepteurs d’applications en laissant les applications prennent en charge différents mécanismes d’authentification au niveau de l’authentification utilisateur et en adoptant le protocole Kerberos pour les fonctionnalités de sécurité, telles que l’authentification mutuelle et les niveaux de l’application suivante de la délégation contrainte.

Pour plus d’informations sur la transition de protocole, voir [Transition du protocole Kerberos et délégation contrainte](https://technet.microsoft.com/library/cc758097(v=ws.10).aspx).

### <a name="constrained-delegation"></a>Délégation contrainte
La délégation contrainte permet aux administrateurs la possibilité de spécifier et d’appliquer des délimitations d’approbation d’application en limitant l’étendue dans laquelle les services d’application peuvent agir au nom d’un utilisateur. Vous pouvez spécifier des services particuliers à partir de laquelle un ordinateur qui est approuvé pour délégation peut demander des ressources. La possibilité de restreindre les droits d’autorisation pour les services permet d’améliorer la conception de la sécurité application en réduisant les possibilités de compromission par les services non approuvés.

Pour plus d’informations sur la délégation contrainte, voir [présentation de la délégation contrainte Kerberos](../kerberos/kerberos-constrained-delegation-overview.md).

## <a name="see-also"></a>Voir aussi
[Vue d’ensemble technique d’ouverture de session Windows et l’authentification](https://technet.microsoft.com/library/dn269029.aspx)


