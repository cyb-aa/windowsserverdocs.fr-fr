---
title: Documentation de référence sur la sécurisation de l’accès privilégié
description: Contrôles de sécurité opérationnelle pour domaines Windows Server Active Directory
ms.prod: windows-server
ms.topic: article
ms.assetid: 22ee9a77-4872-4c54-82d9-98fc73a378c0
ms.date: 02/14/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: 00335fb2ca7a54031430c6c606fb6ffa23a8f7a2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855132"
---
# <a name="active-directory-administrative-tier-model"></a>Modèle de niveau administratif Active Directory

>S'applique à : Windows Server

L’objectif de ce modèle de niveau est de protéger les systèmes d’identité à l’aide d’un ensemble de zones de mémoire tampon entre le contrôle total de l’environnement (niveau 0) et les composants de station de travail à haut risque auxquels les attaques nuisent souvent.

![Diagramme montrant les trois couches du modèle de niveau](../media/securing-privileged-access-reference-material/PAW_RM_Fig1.JPG)

Le modèle de niveau se compose de trois niveaux et inclut uniquement des comptes d’administrateur, pas des comptes d’utilisateur standard :

- **Niveau 0** : contrôle direct des identités d’entreprise dans l’environnement. Le niveau 0 inclut les comptes, groupes et autres ressources qui ont un contrôle administratif direct ou indirect de la forêt Active Directory, des domaines ou des contrôleurs de domaine et de toutes les ressources qui y sont contenues. Le degré de sécurité de toutes les ressources du niveau 0 est équivalent car elles se contrôlent toutes efficacement les unes les autres.
- **Niveau 1** : contrôle des serveurs et applications d’entreprise. Les ressources du niveau 1 incluent des systèmes d’exploitation serveur, des services cloud et des applications d’entreprise. Les comptes d’administrateur du niveau 1 ont un contrôle administratif d’une valeur commerciale significative sur ces ressources. Les administrateurs de serveur qui gèrent ces systèmes d’exploitation avec la possibilité d’exercer un impact sur tous les services d’entreprise sont un exemple de rôle courant.
- **Niveau 2** : contrôle des stations de travail et appareils des utilisateurs. Les comptes d’administrateur du niveau 2 ont un contrôle administratif d’une valeur commerciale significative sur les stations de travail et appareils des utilisateurs. Exemples : les administrateurs du support technique des ordinateurs et de l’assistance car ils peuvent exercer un impact sur l’intégrité de presque toutes les données utilisateur.

> [!NOTE]
> Les niveaux offrent également un moyen simple de définir des priorités pour protéger les ressources administratives, mais il est important de ne pas perdre de vue qu’un attaquant qui contrôle toutes les ressources de tout niveau peut accéder à toutes les ressources de l’entreprise ou à une grande partie. Ce moyen simple de définir des priorités est utile car il rend la tâche de l’attaquant plus difficile et plus coûteuse. En effet, il est plus facile pour un attaquant d’opérer avec un contrôle total de toutes les identités (niveau 0) ou de tous les serveurs et services cloud (niveau 1) que d’accéder à chaque station de travail ou appareil de l’utilisateur (niveau 2) pour obtenir les données de votre organisation.

## <a name="containment-and-security-zones"></a>Confinement et zones de sécurité

Les niveaux sont propres à une zone de sécurité spécifique. Même si elles ont porté de nombreux noms, les zones de sécurité constituent une approche bien établie qui assure le confinement des menaces de sécurité par une isolation de la couche réseau entre elles. Le modèle de niveau complète l’isolation en assurant le confinement des adversaires au sein d’une zone de sécurité où l’isolement réseau n’est pas efficace. Les zones de sécurité peuvent couvrir à la fois l’infrastructure locale et cloud, comme dans l’exemple où les contrôleurs de domaine et les membres du domaine du même domaine sont hébergés localement et sur Azure.

![Diagramme montrant comment les zones de sécurité peuvent couvrir une infrastructure cloud et une infrastructure locale](../media/securing-privileged-access-reference-material/PAW_RM_Fig2.JPG)

Le modèle de niveau empêche l’escalade de privilèges en limitant ce que les administrateurs peuvent contrôler et les emplacements où ils peuvent se connecter (parce que la connexion à un ordinateur accorde le contrôle de ces informations d’identification et de toutes les ressources gérées par ces informations d’identification).

### <a name="control-restrictions"></a>Restrictions de contrôle

Les restrictions de contrôle sont indiquées dans la figure ci-dessous :

![Diagramme des restrictions de contrôle](../media/securing-privileged-access-reference-material/PAW_RM_Fig3.JPG)

### <a name="primary-responsibilities-and-critical-restrictions"></a>Principales responsabilités et restrictions critiques

**Administrateur de niveau 0** : gère le magasin d’identités et un petit nombre de systèmes qui en ont le contrôle effectif, et :

- Peut gérer et contrôler les ressources à tout niveau si besoin
- Peut uniquement se connecter de manière interactive ou accéder aux ressources approuvées au niveau 0

**Administrateur de niveau 1** : gère les serveurs d’entreprise, les services et les applications, et :

- Peut uniquement gérer et contrôler les ressources au niveau 1 ou 2
- Peut uniquement accéder aux ressources (par le biais du type de connexion réseau) qui sont approuvées au niveau 1 ou 0
- Peut uniquement se connecter de façon interactive aux ressources approuvées au niveau 1

**Administrateur de niveau 2** : gère les postes de travail, les ordinateurs portables, les imprimantes et autres appareils d’utilisateur de l’entreprise, et :

- Peut uniquement gérer et contrôler les ressources au niveau 2
- Peut accéder aux ressources (par le biais du type de connexion réseau) à tous les niveaux si besoin
- Peut uniquement se connecter de façon interactive aux ressources approuvées au niveau 2

### <a name="logon-restrictions"></a>Restrictions de connexion

Les restrictions de connexion sont indiquées dans la figure ci-dessous :

![Diagramme des restrictions de connexion](../media/securing-privileged-access-reference-material/PAW_RM_Fig4.JPG)

> [!NOTE]
> Notez que certaines ressources peuvent exercer un impact de niveau 0 sur la disponibilité de l’environnement, sans impacter directement la confidentialité ou l’intégrité des ressources. Celles-ci incluent le service serveur DNS et les périphériques réseau critiques tels que les serveurs proxy Internet.

## <a name="clean-source-principle"></a>Principe de source propre

Le principe de source propre nécessite que toutes les dépendances de sécurité aient au moins le niveau de confiance de l’objet sécurisé.

![Diagramme montrant comment un sujet contrôlant un objet est une dépendance de sécurité de cet objet](../media/securing-privileged-access-reference-material/PAW_RM_Fig5.JPG)

N’importe quel sujet ayant le contrôle d’un objet est une dépendance de sécurité de cet objet. Si un adversaire peut contrôler tout ce qui a le contrôle effectif d’un objet cible, il peut contrôler cet objet cible. C’est pourquoi vous devez veiller à ce que les garanties de toutes les dépendances de sécurité se trouvent au niveau de sécurité souhaité de l’objet lui-même ou au-dessus.

Même si ce principe est simple, son application exige de comprendre les relations de contrôle d’une ressource d’intérêt (objet) et d’en effectuer une analyse de dépendance pour découvrir toutes les dépendances de sécurité (sujets).

Étant donné que le contrôle est transitif, ce principe doit être répété de façon récursive. Par exemple, si A contrôle B et B contrôle C, alors A contrôle également C indirectement.

![Diagramme montrant comment, si A contrôle B et B contrôle C, alors A contrôle également C indirectement](../media/securing-privileged-access-reference-material/PAW_RM_Fig6.JPG)

Une attaque qui compromet A accède à tout ce que A contrôle (y compris B) et à tout ce que B contrôle (y compris C). En utilisant la terminologie des dépendances de sécurité sur ce même exemple, B et A sont tous les deux des dépendances de sécurité de C et doivent être sécurisés au niveau de garantie souhaité de C pour que C ait ce niveau d’assurance.

Pour les systèmes d’infrastructure informatique et d’identité, ce principe doit être appliqué aux moyens de contrôle les plus courants, notamment au matériel sur lequel sont installés les systèmes, au support d’installation des systèmes, à l’architecture et à la configuration du système, ainsi qu’aux opérations quotidiennes.

## <a name="clean-source-for-installation-media"></a>Source propre pour le support d’installation

![Diagramme montrant une source propre pour le support d’installation](../media/securing-privileged-access-reference-material/PAW_RM_CSIM.JPG)

L’application du principe de source propre au support d’installation vous oblige à veiller à ce que le support d’installation n’ait pas été manipulé depuis que le fabricant vous l’a fourni (autant que faire se peut). Cette figure illustre une attaque qui utilise cette voie pour compromettre un ordinateur :

![Figure montrant une attaque qui utilise une voie pour compromettre un ordinateur](../media/securing-privileged-access-reference-material/PAW_RM_Fig8.JPG)

Si vous appliquez le principe de source propre au support d’installation, vous devez valider l’intégrité des logiciels pendant tout le temps où vous les avez en possession, depuis leur acquisition, leur stockage et leur transfert jusqu’à leur utilisation.

### <a name="software-acquisition"></a>Acquisition des logiciels

La source des logiciels doit être validée par l’un des moyens suivants :

- Les logiciels sont obtenus à partir d’un support physique provenant du fabricant ou d’une source digne de confiance. En général, le support fabriqué est envoyé par un fournisseur.
- Les logiciels sont obtenus à partir d’Internet et validés par des hachages de fichiers fournis par le fournisseur.
- Les logiciels sont obtenus à partir d’Internet et validés en téléchargeant, puis en comparant deux copies indépendantes :
   - Téléchargez sur deux hôtes sans aucune relation de sécurité (ils ne sont pas dans le même domaine et ils ne sont pas gérés par les mêmes outils), de préférence à partir de connexions Internet distinctes.
   - Comparez les fichiers téléchargés à l’aide d’un utilitaire comme certutil :  `certutil -hashfile <filename>`

Si possible, tous les logiciels d’application, comme les outils et programmes d’installation d’application, doivent être signés numériquement et vérifiés à l’aide de Windows Authenticode avec l’outil [Windows Sysinternals](https://www.microsoft.com/sysinternals), *sigcheck.exe*, avec une vérification de la révocation. Certains logiciels peuvent être nécessaires si le fournisseur ne donne pas ce type de signature numérique.

### <a name="software-storage-and-transfer"></a>Stockage et transfert des logiciels

Après avoir obtenu les logiciels, vous devez les stocker à un emplacement protégé de toute modification, notamment par des hôtes connectés à Internet ou du personnel de confiance à un niveau inférieur à celui des systèmes où seront installés les logiciels ou le système d’exploitation. Ce stockage peut être un support physique ou un emplacement électronique sécurisé.

### <a name="software-usage"></a>Utilisation des logiciels

Dans l’idéal, les logiciels doivent être validés au moment où ils sont utilisés, par exemple quand ils sont installés manuellement, empaquetés pour un outil de gestion de configuration ou importés dans un outil de gestion de configuration.

## <a name="clean-source-for-architecture-and-design"></a>Source propre pour l’architecture et la conception

L’application du principe de source propre à l’architecture système vous oblige à veiller à ce que le système ne dépende pas de systèmes d’un niveau de confiance inférieur. Un système peut dépendre d’un système d’un niveau de confiance plus élevé, mais pas d’un système d’un niveau de confiance inférieur avec des normes de sécurité inférieures.

Par exemple, vous pouvez accepter qu’Active Directory contrôle le poste de travail d’un utilisateur standard, mais vous prenez un risque réel en raison de l’escalade des privilèges, si vous laissez le poste de travail d’un utilisateur standard contrôler Active Directory.

![Diagramme montrant comment un système peut dépendre d’un système ayant un niveau de confiance plus élevé, mais pas d’un système ayant un niveau de confiance inférieur avec des normes de sécurité inférieures](../media/securing-privileged-access-reference-material/PAW_RM_Fig09.JPG)

La relation de contrôle peut être introduite par plusieurs moyens, notamment les listes de contrôle d’accès (ACL) de sécurité sur des objets tels que les systèmes de fichiers, l’appartenance au groupe Administrateurs local sur un ordinateur ou les agents installés sur un ordinateur s’exécutant comme système (avec la possibilité d’exécuter des scripts et du code arbitraire).

Un exemple souvent négligé est l’exposition lors de la connexion, laquelle crée une relation de contrôle en exposant les informations d’identification d’administration d’un système à un autre système. C’est la raison pour laquelle les attaques visant à voler des informations d’identification, comme les attaques de type pass-the-hash, sont si puissantes. Quand un administrateur se connecte au poste de travail d’un utilisateur standard avec des informations d’identification de niveau 0, il les expose à ce poste de travail, lui permet de contrôler Active Directory et crée une voie d’escalade de privilèges vers Active Directory. Pour plus d’informations sur ces attaques, voir [cette page](https://technet.microsoft.com/security/dn785092).

Parce que les ressources qui dépendent de systèmes d’identité tels qu’Active Directory sont très nombreuses, vous devez réduire le nombre de systèmes dont dépendent votre Active Directory et vos contrôleurs de domaine.

![Diagramme montrant que vous devez réduire le nombre de systèmes sur lesquels dépendent vos contrôleurs de domaine et Active Directory](../media/securing-privileged-access-reference-material/PAW_RM_Fig010.JPG)

Pour plus d’informations sur la réduction des principaux risques liés à Active Directory, voir [cette page](https://aka.ms/hardenAD).

## <a name="operational-standards-based-on-clean-source-principle"></a>Normes opérationnelles basées sur le principe de source propre

Cette section décrit les normes opérationnelles et les attentes relatives au personnel d’administration. Ces normes sont conçues pour sécuriser le contrôle administratif des systèmes informatiques d’une organisation contre les risques éventuellement créés par les pratiques et processus opérationnels.

![Diagramme montrant comment les normes sont conçues pour sécuriser le contrôle administratif des systèmes informatiques d’une organisation contre les risques éventuellement créés par les pratiques et processus opérationnels](../media/securing-privileged-access-reference-material/PAW_RM_Fig11.JPG)

### <a name="integrating-the-standards"></a>Intégration des normes

Vous pouvez intégrer ces normes aux normes et pratiques globales de votre organisation. Vous pouvez les adapter à vos besoins spécifiques, aux outils disponibles et au goût du risque de votre organisation. Néanmoins, nous vous recommandons quelques modifications minimes pour réduire les risques. Nous vous conseillons d’utiliser les valeurs par défaut indiquées dans ces instructions comme point de référence pour votre état final idéal et de gérer les deltas comme des exceptions à traiter par ordre de priorité.

Ces instructions s’articulent autour des sections suivantes :

- Hypothèses
- Comité consultatif sur les modifications
- Pratiques opérationnelles
   - Résumé
   - Détails des normes

### <a name="assumptions"></a>Hypothèses

Les normes indiquées dans cette section supposent que l’organisation dispose des attributs suivants :

- La plupart ou la totalité des serveurs et stations de travail inclus dans l’étendue sont joints à Active Directory.
- Tous les serveurs à gérer exécutent Windows Server 2008 R2 ou ultérieur et ont activé le mode RestrictedAdmin du protocole RDP.
- Toutes les stations de travail à gérer exécutent Windows 7 ou ultérieur et ont activé le mode RestrictedAdmin du protocole RDP.

   > [!NOTE]
   > Pour activer le mode RestrictedAdmin du protocole RDP, voir [cette page](https://aka.ms/RDPRA).

- Des cartes à puce sont disponibles et délivrées à tous les comptes d’administration.
- Le compte *Builtin\Administrator* de chaque domaine a été désigné comme compte d’accès d’urgence.
- Une solution de gestion des identités d’entreprise est déployée.
- [LAPS](https://aka.ms/laps) a été déployé sur les serveurs et stations de travail pour gérer le mot de passe du compte administrateur local.
- Une solution de gestion de l’accès privilégié, comme Microsoft Identity Manager, est déjà en place ou en passe d’être adoptée.
- Des membres du personnel sont désignés pour surveiller les alertes de sécurité et y répondre.
- La capacité technique à appliquer rapidement des mises à jour de sécurité Microsoft est disponible.
- Aucun contrôleur de gestion de la carte de base n’est utilisé sur les serveurs, ou ils respectent des contrôles de sécurité stricts.
- Les comptes d’administrateur et groupes pour les serveurs (administrateurs de niveau 1) et les stations de travail (administrateurs de niveau 2) sont gérés par les administrateurs de domaine (niveau 0).
- Un comité consultatif sur les modifications, ou une autre autorité désignée, est en place pour l’approbation des modifications Active Directory.

### <a name="change-advisory-board"></a>Comité consultatif sur les modifications

Un comité consultatif sur les modifications est un forum de discussion et une autorité d’approbation des modifications pouvant avoir un impact sur le profil de sécurité de l’organisation. Toutes les exceptions à ces normes doivent être soumises au comité avec une évaluation des risques et une justification.

Chaque norme indiquée dans le présent document est classée selon sa criticité pour un niveau donné.

![Diagramme montrant la norme des niveaux donnés](../media/securing-privileged-access-reference-material/PAW_RM_Fig12.JPG)

Toutes les exceptions relatives aux éléments obligatoires (signalés par un octogone rouge ou un triangle orange dans le présent document) sont considérées comme temporaires et doivent être approuvées par le comité. Les recommandations incluent les points suivants :

- La demande initiale exige une justification et une acceptation du risque signées par le superviseur direct du personnel. Elle expire au bout de six mois.
- Les renouvellements exigent une justification et une acceptation du risque signées par un responsable de division. Ils expirent au bout de six mois.

Toutes les exceptions relatives aux éléments recommandés (signalés par un cercle jaune dans le présent document) sont considérées comme temporaires et doivent être approuvées par le comité. Les recommandations incluent les points suivants :

- La demande initiale exige une justification et une acceptation du risque signées par le superviseur direct du personnel. Elle expire au bout de 12 mois.
- Les renouvellements exigent une justification et une acceptation du risque signées par un responsable de division. Ils expirent au bout de 12 mois.

### <a name="operational-practices-standards-summary"></a>Résumé des normes liées aux pratiques opérationnelles

Les colonnes Niveau de ce tableau font référence au niveau du compte d’administration, dont le contrôle affecte généralement toutes les ressources qu’il inclut.

![Tableau montrant les niveaux du compte d’administration](../media/securing-privileged-access-reference-material/PAW_RM_Fig13.JPG)

Des décisions opérationnelles prises régulièrement sont essentielles pour maintenir la sécurité de l’environnement. Ces normes pour les processus et les pratiques garantissent qu’une erreur opérationnelle ne mène pas à une vulnérabilité opérationnelle exploitable dans l’environnement.

#### <a name="administrator-enablement-and-accountability"></a>Responsabilité et habilitation des administrateurs

Les administrateurs doivent être informés, habilités, formés et tenus responsables afin de faire fonctionner l’environnement de façon aussi sécurisée que possible.

##### <a name="administrative-personnel-standards"></a>Normes relatives au personnel administratif

Le personnel administratif doit être contrôlé pour garantir sa fiabilité et a besoin de privilèges administratifs :

- Vérifiez les antécédents du personnel avant de lui attribuer des privilèges administratifs.
- Chaque trimestre, passez en revue les privilèges administratifs pour déterminer quels membres du personnel ont toujours un besoin légitime de disposer d’un accès administratif.

##### <a name="administrative-security-briefing-and-accountability"></a>Responsabilité et briefing sur la sécurité administrative

Les administrateurs doivent être informés et tenus responsables des risques encourus par l’organisation, et conscients de leur rôle dans la gestion de ces risques. Les administrateurs doivent être formés tous les ans sur les aspects suivants :

- Menaces générales pour l’environnement
   - Adversaires identifiés
   - Techniques d’attaque de type pass-the-hash et vol d’informations d’identification
- Incidents et menaces spécifiques de l’organisation
- Rôles de l’administrateur dans la protection contre les attaques
   - Gestion de l’exposition des informations d’identification avec le modèle de niveau
   - Utilisation de stations de travail administratives
   - Utilisation du mode RestrictedAdmin du protocole RDP (Remote Desktop Protocol)
- Pratiques administratives spécifiques de l’organisation
   - Passer en revue toutes les recommandations opérationnelles de cette norme
   - Implémenter les principales règles suivantes :
      - Utiliser des comptes d’administration uniquement sur des stations de travail administratives
      - Ne pas désactiver ni entraver les contrôles de sécurité sur le compte ou les stations de travail (par exemple, les restrictions de connexion ou les attributs exigés pour les cartes à puce)
      - Signaler les problèmes ou les activités suspectes

Pour garantir l’adhésion à ces règles, tous les membres du personnel dotés de comptes administratifs doivent signer un code de conduite relatifs aux privilèges administratifs indiquant qu’ils acceptent de suivre les pratiques stipulées dans la stratégie d’administration propre à l’organisation.

##### <a name="provisioning-and-deprovisioning-processes-for-administrative-accounts"></a>Octroi et suppression de privilèges d’accès pour les comptes administratifs

Les normes suivantes doivent être respectées pour satisfaire les exigences liées au cycle de vie.

- Tous les comptes administratifs doivent être approuvés par l’autorité approbatrice indiquée dans le tableau ci-après.
   - L’approbation doit uniquement être octroyée si le personnel a un besoin légitime de disposer de privilèges administratifs.
   - Une approbation de privilèges administratifs ne doit pas dépasser six mois.
- L’accès à des privilèges administratifs doit être immédiatement supprimé dans les situations suivantes :
   - L’employé change de poste.
   - L’employé quitte l’organisation.
- Les comptes doivent être immédiatement désactivés une fois que l’employé a quitté l’organisation.
- Les comptes désactivés doivent être supprimés dans un délai de six mois et l’enregistrement de leur suppression doit être consigné auprès du comité consultatif sur les modifications.
- Tous les mois, passez en revue tous les membres dotés d’un compte privilégié pour vérifier qu’aucune autorisation inadéquate n’a été accordée. Ce processus peut être remplacé par un outil automatisé qui avertit des modifications.

|Niveau de privilège du compte|Autorité approbatrice|Fréquence de vérification des octrois|
|--------------|------------|----------------|
|Administrateur de niveau 0|Comité consultatif sur les modifications|Mensuelle ou automatisée|
|Administrateur de niveau 1|Administrateurs de niveau 0 ou sécurité|Mensuelle ou automatisée|
|Administrateur de niveau 2|Administrateurs de niveau 0 ou sécurité|Mensuelle ou automatisée|

#### <a name="operationalize-least-privilege"></a>Privilèges minimum

Ces normes permettent d’atteindre les privilèges minimum en réduisant le nombre d’administrateurs en place et leur durée de détention de ces privilèges.

> [!NOTE]
> Instaurer les privilèges minimum dans votre organisation nécessite de comprendre les rôles organisationnels, leurs exigences et leurs mécanismes de conception pour vous assurer qu’ils sont en mesure d’accomplir leur travail avec lesdits privilèges minimum. Instaurer des privilèges minimum dans un modèle d’administration exige souvent l’utilisation de plusieurs approches :
>
> - Limiter le nombre d’administrateurs ou de membres de groupes privilégiés
> - Déléguer moins de privilèges aux comptes
> - Fournir des privilèges temporaires sur demande
> - Offrir la capacité à d’autres membres du personnel d’effectuer des tâches (approche de la « concierge »)
> - Fournir des processus pour l’accès d’urgence et les scénarios d’utilisation rare

##### <a name="limit-count-of-administrators"></a>Limiter le nombre d’administrateurs

Au moins deux personnes qualifiées doivent être affectées à chaque rôle administratif pour garantir la continuité de l’activité.

Si le nombre de personnes affectées à un rôle est supérieur à deux, le comité consultatif sur les modifications doit approuver les raisons particulières de l’attribution de privilèges à chaque personne (y compris les deux d’origine). La justification de l’approbation doit préciser :

- les tâches techniques exécutées par les administrateurs qui requièrent des privilèges administratifs,
- la fréquence d’exécution de ces tâches,
- la raison précise pour laquelle ces tâches ne peuvent pas être exécutées par un autre administrateur en leur nom,
- la description de toutes les autres approches connues pouvant remplacer l’octroi de privilèges et les raisons pour lesquelles elles ne sont pas acceptables.

##### <a name="dynamically-assign-privileges"></a>Affecter dynamiquement des privilèges

Les administrateurs doivent obtenir des autorisations « juste-à-temps » pour les utiliser au moment où ils effectuent des tâches. Aucune autorisation n’est attribuée définitivement à des comptes administratifs.

> [!NOTE]
> Les privilèges administratifs attribués définitivement créent naturellement une stratégie des « privilèges maximum », car le personnel administratif a besoin d’un accès rapide aux autorisations pour maintenir la disponibilité opérationnelle en cas de problème. Les autorisations juste-à-temps permettent de :
>
> - affecter des autorisations de façon plus fine, en se rapprochant des privilèges minimum,
> - réduire le temps d’exposition des privilèges,
> - suivre l’utilisation des autorisations pour détecter les attaques ou les abus.

#### <a name="manage-risk-of-credential-exposure"></a>Gérer le risque d’exposition des informations d’identification

Utilisez les pratiques suivantes pour gérer correctement le risque d’exposition des informations d’identification.

##### <a name="separate-administrative-accounts"></a>Séparer les comptes administratifs

Tous les employés autorisés à détenir des privilèges administratifs doivent avoir des comptes distincts pour leurs fonctions administratives, différents de leurs comptes d’utilisateur.

- **Comptes d’utilisateur standard** : privilèges d’utilisateur standard octroyés pour les tâches d’utilisateur standard, comme l’e-mail, la navigation web et l’utilisation des applications métier. Ces comptes ne doivent pas bénéficier de privilèges administratifs.
- **Comptes administratifs** : comptes distincts créés pour les personnes qui disposent des privilèges administratifs appropriés. Un administrateur qui doit gérer les ressources de chaque niveau doit avoir un compte distinct pour chaque niveau. Ces comptes ne doivent pas avoir accès à l’e-mail ni à l’Internet public.

##### <a name="administrator-logon-practices"></a>Pratiques pour la connexion des administrateurs

Pour qu’un administrateur puisse se connecter à un hôte de façon interactive (localement par protocole RDP standard, à l’aide d’un compte d’identification ou à l’aide de la console de virtualisation), cet hôte doit atteindre ou dépasser la norme correspondant au niveau du compte d’administrateur (ou à un niveau supérieur).

Les administrateurs peuvent uniquement se connecter pour administrer des stations de travail avec leurs comptes administratifs. Les administrateurs se connectent uniquement à des ressources gérées en utilisant la technologie de support approuvée décrite dans la section suivante.

> [!NOTE]
> Cette approche est obligatoire car la connexion à un hôte de manière interactive accorde le contrôle des informations d’identification à cet hôte.
>
> Consultez [Outils d’administration et types d’ouverture de session](https://aka.ms/admintoolsecurity) pour plus d’informations sur les types d’ouverture de session, les outils d’administration courants et l’exposition des informations d’identification.

##### <a name="use-of-approved-support-technology-and-methods"></a>Utilisation de technologies et méthodes de support approuvées

Les administrateurs qui prennent en charge des systèmes et utilisateurs distants doivent suivre ces recommandations pour empêcher tout adversaire ayant le contrôle de l’ordinateur distant de voler leurs informations d’identification d’administration.

- Les options de support principales doivent être utilisées si elles sont disponibles.
- Les options de support secondaires doivent être utilisées uniquement si l’option principale n’est pas disponible.
- Les méthodes de support interdites ne doivent jamais être utilisées.
- Un compte administratif ne doit jamais utiliser de navigation Internet ni accéder à des e-mail.

###### <a name="tier-0-forest-domain-and-dc-administration"></a>Administration de forêt, domaine et contrôleur de domaine de niveau 0

Vérifiez que les pratiques suivantes sont appliquées pour ce scénario :

- **Support de serveur distant** : lors de l’accès à distance à un serveur, les administrateurs de niveau 0 doivent suivre ces recommandations :
  - **Option principale (outil)**  : Outils de contrôle à distance qui utilisent des ouvertures de session réseau (type 3). Pour plus d’informations, voir [Outils d’administration et types d’ouverture de session](https://aka.ms/admintoolsecurity).
  - **Option principale (interactive)**  : Utilisez une session en mode RestrictedAdmin ou Standard du protocole RDP à partir d’une station de travail administrative avec un compte de domaine.

    > [!NOTE]
    > Si vous disposez d’une solution de gestion des privilèges de niveau 0, ajoutez « qui utilise des autorisations obtenues juste-à-temps à partir d’une solution de gestion de l’accès privilégié ».

- **Support de serveur physique** : Quand ils se trouvent physiquement dans une console de serveur ou une console de machine virtuelle (outils Hyper-V ou VMWare), ces comptes ne font pas l’objet de restrictions quant à l’utilisation d’outils d’administration spécifiques. Seules les restrictions générales liées aux tâches d’utilisateur standard comme l’e-mail et la navigation sur l’Internet ouvert s’appliquent.

   > [!NOTE]
   > L’administration de niveau 0 diffère de celle des autres niveaux, car toutes les ressources de niveau 0 disposent déjà d’un contrôle direct ou indirect de toutes les ressources. Par exemple, une attaque ayant le contrôle d’un contrôleur de domaine n’a pas besoin de voler des informations d’identification auprès des administrateurs connectés puisqu’elle a déjà accès à toutes les informations d’identification de domaine dans la base de données.

###### <a name="tier-1-server-and-enterprise-application-support"></a>Support d’applications de serveur et d’entreprise de niveau 1

Vérifiez que les pratiques suivantes sont appliquées pour ce scénario :

- **Support de serveur distant** : lors de l’accès à distance à un serveur, les administrateurs de niveau 1 doivent suivre ces recommandations :
   - **Option principale (outil)**  : Outils de contrôle à distance qui utilisent des ouvertures de session réseau (type 3). Pour plus d’informations, voir [Mitigating Pass-the-Hash and Other Credential Theft (Atténuation des attaques de type Pass-the-Hash et autres vols d’informations d’identification)](https://www.microsoft.com/pth) v1 (pages 42 à 47).
   - **Option principale (interactive)**  : Utilisez le mode RestrictedAdmin du protocole RDP à partir d’une station de travail d’administration avec un compte de domaine qui utilise des autorisations obtenues juste-à-temps à partir d’une solution de gestion de l’accès privilégié.
   - **Option secondaire** : Ouvrez une session sur le serveur à l’aide d’un mot de passe de compte local défini par LAPS à partir d’une station de travail d’administration.
   - **Options interdite** : Le protocole RDP standard ne doit pas être utilisé avec un compte de domaine.
   - **Option interdite** : Utilisation des informations d’identification du compte de domaine en cours de session (par exemple, en utilisant un compte *d’identification* ou une authentification auprès d’un partage). Cette pratique expose les informations d’identification d’ouverture de session au risque de vol.
- **Support de serveur physique** : Quand les serveurs se trouvent physiquement dans une console de serveur ou une console de machine virtuelle (outils Hyper-V ou VMWare), les administrateurs de niveau 1 doivent extraire le mot de passe du compte local de LAPS avant d’accéder au serveur.
   - **Option principale** : Récupérez le mot de passe du compte local défini par LAPS à partir d’une station de travail d’administration avant l’ouverture de session sur le serveur.
   - **Option interdite** : Une ouverture de session avec un compte de domaine n’est pas autorisée dans ce scénario.
   - **Option interdite** : Utilisation des informations d’identification du compte de domaine en cours de session (par exemple, compte d’identification ou authentification auprès d’un partage). Cette pratique expose les informations d’identification d’ouverture de session au risque de vol.

###### <a name="tier-2-help-desk-and-user-support"></a>Support utilisateur et support technique de niveau 2

Des organisations de support technique et de support utilisateur assurent la prise en charge des utilisateurs finaux (qui n’ont pas besoin de privilèges administratifs) et de leurs stations de travail (qui ont besoin de privilèges administratifs).

**Support utilisateur** : Les tâches incluent l’assistance des utilisateurs pour effectuer des tâches qui ne nécessitent aucune modification de la station de travail, souvent en leur montrant comment utiliser une fonctionnalité d’application ou de système d’exploitation.

- **Support utilisateur sur place** : Le personnel de support de niveau 2 se trouve physiquement dans l’espace de travail de l’utilisateur.
   - **Option principale** : Un support de type « Procuration de privilège » peut être fourni sans aucun outil.
   - **Option interdite** : L’ouverture de session avec les informations d’identification d’administration du compte de domaine n’est pas autorisée dans ce scénario. Basculez vers un support station de travail côté bureau si des privilèges administratifs sont exigés.
- **Support utilisateur à distance** : Le personnel de support de niveau 2 est physiquement éloigné de l’utilisateur.
   - **Option principale** : Peuvent être utilisés l’Assistance à distance, Skype Entreprise ou un partage d’écran utilisateur similaire. Pour plus d’informations, voir [Qu’est-ce que l’Assistance à distance Windows ?](https://windows.microsoft.com/windows/what-is-windows-remote-assistance)
   - **Option interdite** : L’ouverture de session avec les informations d’identification d’administration du compte de domaine n’est pas autorisée dans ce scénario. Basculez vers un support station de travail si des privilèges administratifs sont exigés.
- **Support station de travail** : Les tâches incluent des opérations de maintenance ou de dépannage d’une station de travail, qui requièrent un accès à un système permettant de consulter des journaux, d’installer des logiciels, de mettre à jour des pilotes, etc.
   - **Support station de travail sur place** : Le personnel de support de niveau 2 se trouve physiquement devant la station de travail de l’utilisateur.
      - **Option principale** : Récupérez le mot de passe du compte local défini par LAPS à partir d’une station de travail d’administration avant la connexion à la station de travail de l’utilisateur.
      - **Option interdite** : L’ouverture de session avec les informations d’identification d’administration du compte de domaine n’est pas autorisée dans ce scénario.
   - **Support station de travail à distance** : Le personnel de support de niveau 2 est physiquement éloigné de la station de travail.
      - **Option principale** : Utilisez le mode RestrictedAdmin du protocole RDP à partir d’une station de travail d’administration avec un compte de domaine qui utilise des autorisations obtenues juste-à-temps à partir d’une solution de gestion de l’accès privilégié.
      - **Option secondaire** : Récupérez un mot de passe du compte local défini par LAPS à partir d’une station de travail d’administration avant la connexion à la station de travail de l’utilisateur.
      - **Option interdite** : Utilisez le protocole RDP standard avec un compte de domaine.

###### <a name="no-browsing-the-public-internet-with-admin-accounts-or-from-admin-workstations"></a>Absence de navigation sur l’Internet public avec des comptes d’administration ou des stations de travail d’administration

Le personnel administratif ne peut pas naviguer sur l’Internet ouvert en étant connecté avec un compte d’administration ou en ayant ouvert une session sur une station de travail d’administration. Les seules exceptions autorisées sont l’utilisation d’un navigateur web pour administrer un service cloud.

###### <a name="no-accessing-email-with-admin-accounts-or-from-admin-workstations"></a>Absence d’accès à des e-mails avec des comptes d’administration ou à partir de stations de travail d’administration

Le personnel administratif ne peut pas accéder à des e-mails en étant connecté avec un compte d’administration ou en ayant ouvert une session sur une station de travail d’administration.

###### <a name="store-service-and-application-account-passwords-in-a-secure-location"></a>Stocker les mots de passe des comptes de service et d’application à un emplacement sécurisé

Les recommandations suivantes doivent être utilisées pour les processus de sécurité physique qui contrôlent l’accès au mot de passe :

- Mettez sous clé les mots de passe de compte de service dans un coffre fort physique.
- Assurez-vous que seul le personnel approuvé situé au niveau ou au-dessus de la classification du compte a accès aux mots de passe de compte.
- Limitez au maximum le nombre de personnes qui peuvent accéder aux mots de passe.
- Veillez à ce que tous les accès à des mots de passe soient consignés, suivis et surveillés par un tiers désintéressé, par exemple, un responsable qui n’est pas formé pour effectuer l’administration informatique.

##### <a name="strong-authentication"></a>Authentification renforcée

Utilisez les pratiques suivantes pour configurer correctement une authentification renforcée.

###### <a name="enforce-smartcard-multi-factor-authentication-mfa-for-all-admin-accounts"></a>Mettre en place une authentification multifacteur par carte à puce pour tous les comptes d’administrateur

Aucun compte d’administrateur n’est autorisé à utiliser un mot de passe à des fins d’authentification. Les seules exceptions autorisées sont les comptes d’accès d’urgence qui sont protégés par les processus appropriés.

Liez tous les comptes administratifs à une carte à puce et activez l’attribut « **Une carte à puce est nécessaire pour ouvrir une session interactive** ».

Un script doit être implémenté pour réinitialiser automatiquement et régulièrement la valeur de hachage du mot de passe aléatoire par désactivation et réactivation immédiate de l’attribut « **Une carte à puce est nécessaire pour ouvrir une session interactive** ».

N’autorisez aucune exception pour les comptes utilisés par le personnel humain en dehors des comptes d’accès d’urgence.

###### <a name="enforce-multi-factor-authentication-for-all-cloud-admin-accounts"></a>Mettre en place Multi-Factor Authentication pour tous les comptes d’administrateur cloud

Tous les comptes dotés de privilèges administratifs dans un service cloud, comme Microsoft Azure et Office 365, doivent utiliser une authentification multifacteur.

##### <a name="rare-use-emergency-procedures"></a>Procédures d’urgence rarement utilisées

Les pratiques opérationnelles doivent prendre en charge les normes suivantes :

- Veillez à ce que les interruptions soient rapidement résolues.
- Veillez à ce que les tâches à privilèges élevés rares puissent être effectuées si besoin.
- Veillez à ce que des procédures sécurisées soient utilisées pour protéger les informations d’identification et les privilèges.
- Veillez à ce que des processus de suivi et d’approbation appropriés soient appliqués.

###### <a name="correctly-follow-appropriate-processes-for-all-emergency-access-accounts"></a>Suivre correctement les processus appropriés à tous les comptes d’accès d’urgence

Veillez à ce que chaque compte d’accès d’urgence dispose d’une feuille de suivi dans le coffre fort.

La procédure décrite sur la feuille de suivi du mot de passe doit être observée pour chaque compte, laquelle implique de modifier le mot de passe après chaque utilisation et de fermer la session sur toutes les stations de travail ou tous les serveurs utilisés une fois l’opération terminée.

Toute utilisation des comptes d’accès d’urgence doit être approuvée par le comité consultatif sur les modifications, à l’avance ou après les faits, en tant qu’utilisation d’urgence approuvée.

###### <a name="restrict-and-monitor-usage-of-emergency-access-accounts"></a>Limiter et surveiller l’utilisation des comptes d’accès d’urgence

Pour toute utilisation des comptes d’accès d’urgence :

- Seuls les administrateurs de domaine autorisés peuvent accéder aux comptes d’accès d’urgence avec des privilèges administratifs de domaine.
- Les comptes d’accès d’urgence peuvent être utilisés uniquement sur des contrôleurs de domaine et d’autres hôtes de niveau 0.
- Ces comptes doivent uniquement être utilisés pour :
  - résoudre et corriger les problèmes techniques qui empêchent l’utilisation des comptes d’administration appropriés,
  - effectuer des tâches rares, comme :
    - l’administration du schéma,
    - Tâches à l’échelle de la forêt qui exigent des privilèges administratifs d’entreprise

      > [!NOTE]
      > La gestion de la topologie, notamment du site et du sous-réseau Active Directory, est déléguée pour limiter l’utilisation de ces privilèges.

- Toute utilisation de l’un de ces comptes doit faire l’objet d’une autorisation écrite par le responsable du groupe de sécurité.
- La procédure indiquée sur la feuille de suivi pour chaque compte d’accès d’urgence exige de modifier le mot de passe à chaque utilisation. Un membre de l’équipe de sécurité doit valider le bon déroulement de cette opération.

###### <a name="temporarily-assign-enterprise-admin-and-schema-admin-membership"></a>Attribuer temporairement une appartenance à l’administrateur d’entreprise et l’administrateur de schéma

Les privilèges doivent être ajoutés selon les besoins et supprimés après utilisation. Le compte d’urgence doit posséder ces privilèges seulement pendant la durée de la tâche et pour un maximum de 10 heures. Toute utilisation et toute durée de ces privilèges doivent être consignées dans un enregistrement du comité consultatif sur les modifications, une fois la tâche terminée.

## <a name="esae-administrative-forest-design-approach"></a>Approche de la conception de forêt administrative ESAE

Cette section contient une approche destinée à une forêt administrative basée sur l’architecture de référence ESEA (Enhanced Security Administration Environnement) déployée par les équipes des services professionnels de cybersécurité de Microsoft afin de protéger leurs clients contre les attaques de cybersécurité.

Les forêts administratives dédiées permettent aux organisations d’héberger des comptes, stations de travail et groupes administratifs dans un environnement dont les contrôles de sécurité sont renforcés par rapport à ceux de l’environnement de production.

Cette architecture permet plusieurs contrôles de sécurité qui ne sont pas possibles ou facilement configurés dans une architecture à forêt unique, même si elle est gérée avec des stations de travail à accès privilégié. Cette approche permet l’approvisionnement de comptes en tant qu’utilisateurs sans privilèges standard dans la forêt d’administration, qui ont des privilèges très élevés dans l’environnement de production, permettant ainsi une meilleure mise en œuvre technique de la gouvernance. Cette architecture permet également d’utiliser la fonctionnalité d’authentification sélective d’une approbation comme un moyen de limiter les ouvertures de session (et l’exposition des informations d’identification) aux seuls hôtes autorisés. Dans les situations où un niveau supérieur de garantie est souhaité pour la forêt de production sans pour autant impliquer le coût et la complexité d’une recréation complète, une forêt d’administration peut fournir un environnement qui augmente le niveau de garantie de l’environnement de production.

Bien que cette approche ajoute une forêt à un environnement Active Directory, le coût et la complexité sont limités par la conception fixe, le petit encombrement matériel/logiciel et le petit nombre d’utilisateurs.

> [!NOTE]
> Cette approche fonctionne bien pour administrer Active Directory, mais de nombreuses applications ne sont pas compatibles avec une administration par des comptes provenant d’une forêt externe qui utilise une approbation standard.

Cette figure illustre une forêt ESAE utilisée pour l’administration de ressources de niveau 0 et une forêt PRIV configurée pour une utilisation avec la fonctionnalité Privileged Identity Management de Microsoft Identity Manager. Pour plus d’informations sur le déploiement d’une instance MIM PAM, voir l’article [Privileged Identity Management pour les services de domaine Active Directory (AD DS)](https://technet.microsoft.com/library/mt150258.aspx).

![Figure montrant une forêt ESAE utilisée pour l’administration de ressources du niveau 0 et une forêt PRIV configurée pour une utilisation avec la fonctionnalité Privileged Identity Management de Microsoft Identity Manager](../media/securing-privileged-access-reference-material/PAW_RM_Fig14.JPG)

Une forêt d’administration dédiée est une forêt Active Directory de domaine unique standard, dédiée à la fonction de gestion d’Active Directory. Les forêts et les domaines d’administration peuvent être renforcés de façon plus stricte que les forêts de production, en raison du nombre limité de cas d’utilisation.

La conception d’une forêt d’administration doit inclure les considérations suivantes :

- **Étendue limitée** : la valeur principale d’une forêt d’administration est le niveau élevé de garantie de sécurité et la surface d’attaque réduite aboutissant à un risque résiduel inférieur. La forêt peut être utilisée pour héberger des fonctions et des applications de gestion supplémentaires, sachant que chaque augmentation de son étendue augmente la surface d’attaque de la forêt et de ses ressources. L’objectif est de limiter les fonctions de la forêt et des utilisateurs administrateurs qui s’y trouvent pour conserver une surface d’attaque minimale : chaque augmentation de l’étendue doit donc être envisagée avec précaution.
- **Configurations d’approbation** : configurez l’approbation à partir de forêts ou de domaines gérés vers la forêt d’administration.
   - Une approbation à sens unique est requise depuis l'environnement de production vers la forêt d'administration. Il peut s’agir d’une approbation de domaine ou d’une approbation de forêt. Le domaine/La forêt d’administration n’a pas besoin d’approuver les domaines/forêts gérés pour gérer Active Directory, même si d’autres applications peuvent nécessiter une relation d’approbation bidirectionnelle, la validation de la sécurité et des tests.
   - L’authentification sélective doit être utilisée pour limiter les comptes de la forêt d’administration à la seule connexion aux hôtes de production appropriés. Pour la gestion des contrôleurs de domaine et la délégation de droits dans Active Directory, ceci nécessite généralement d’accorder le droit « Autorisé à ouvrir une session » pour les contrôleurs de domaine à des comptes d’administrateur désignés de niveau 0 dans la forêt d’administration. Pour plus d’informations, voir Configuration des paramètres de l’authentification sélective.
- **Privilèges et renforcement de domaines** : la forêt d’administration doit être configurée avec des privilèges minimum, en fonction de la configuration requise pour l’administration d’Active Directory.
   - L’octroi de droits d’administrer des contrôleurs de domaine et de déléguer des autorisations exige d’ajouter de comptes de forêt d’administration au groupe local BUILTIN\Administrators du domaine. Cet ajout est nécessaire car le groupe global Administrateurs de domaine n’accepte pas de membres provenant d’un domaine externe.
   - Le seul inconvénient à l’utilisation de ce groupe pour octroyer des droits est qu’il n’a pas d’accès administratif aux nouveaux objets de stratégie de groupe, par défaut. Vous pouvez y remédier en suivant la procédure indiquée dans [cet article de la Base de connaissances](https://support.microsoft.com/kb/321476) pour modifier les autorisations par défaut du schéma.
   - Les comptes de la forêt d’administration utilisés pour administrer l’environnement de production ne doivent pas recevoir de privilèges d’administration pour la forêt d’administration, ni pour les domaines et les stations de travail qu’elle contient.
   - Les privilèges d’administration sur la forêt d’administration doivent être étroitement contrôlés par un processus hors connexion, afin de réduire la possibilité qu’un attaquant ou un utilisateur interne malveillant effacent les journaux d’audit. Ceci limite la possibilité que le personnel utilisant des comptes d’administration de production assouplisse les restrictions sur leurs comptes, augmentant ainsi les risques pour l’organisation.
   - La forêt d’administration doit respecter les configurations de Microsoft Security Compliance Baseline (SCB) pour le domaine, notamment les configurations renforcées des protocoles d’authentification.
   - Tous les hôtes de la forêt d’administration doivent être automatiquement mis à jour avec les mises à jour de sécurité. Si cette configuration peut aboutir à un risque d’interruption des opérations de maintenance du contrôleur de domaine, elle permet également une limitation importante des risques de sécurité liés à des vulnérabilités non corrigées.

      > [!NOTE]
      > Une instance Windows Server Update Services dédiée peut être configurée pour approuver automatiquement les mises à jour. Pour plus d’informations, voir la section « Approuver automatiquement les mises à jour pour l’installation » dans Approbation des mises à jour.

- **Renforcement des stations de travail** : créez les stations de travail d’administration à l’aide des [stations de travail à accès privilégié](../securing-privileged-access/privileged-access-workstations.md) (par le biais de la phase 3), mais changez l’appartenance au domaine par la forêt d’administration au lieu de l’environnement de production.
- **Renforcement des serveurs et contrôleurs de domaine** : pour tous les contrôleurs de domaine et serveurs de la forêt d’administration :
   - Vérifiez que tous les supports sont validés en utilisant les instructions données dans [Source propre pour le support d’installation](https://aka.ms/cleansource).
   - Vérifiez que les serveurs de la forêt d’administration disposent des derniers systèmes d’exploitation installés, même si cela n’est pas faisable en production.
   - Les hôtes de la forêt d’administration doivent être automatiquement mis à jour avec les mises à jour de sécurité.

      > [!NOTE]
      > Windows Server Update Services peut être configuré pour approuver automatiquement les mises à jour. Pour plus d’informations, voir la section « Approuver automatiquement les mises à jour pour l’installation » dans Approbation des mises à jour.

   - Des bases de référence de sécurité doivent être utilisées comme configurations de démarrage.

      > [!NOTE]
      > Les clients peuvent utiliser Microsoft Security Compliance Toolkit (SCT) pour configurer les bases de référence sur les hôtes d’administration.

   - Démarrage sécurisé pour atténuer les risques face à des attaquants ou des programmes malveillants tentant de charger du code non signé dans le processus de démarrage.

      > [!NOTE]
      > Cette fonctionnalité a été introduite dans Windows 8 pour tirer parti de l’interface UEFI (Unified Extensible Firmware Interface).

   - Chiffrement intégral des volumes pour limiter les risques liés à la perte physique d’ordinateurs, comme des ordinateurs portables d’administration utilisés à distance.

      > [!NOTE]
      > Pour plus d’informations, voir [BitLocker](https://technet.microsoft.com/library/dn641993.aspx).

   - Restrictions USB pour protéger contre les vecteurs d’infection physique.

      > [!NOTE]
      > Pour plus d’informations, voir [Contrôler l’accès en lecture ou écriture aux dispositifs ou médias amovibles](https://technet.microsoft.com/library/cc730808(v=ws.10).aspx).

   - Isolement du réseau pour protéger contre les attaques réseau et les actions d’administration inappropriées effectuées par inadvertance. Les pare-feu des hôtes doivent bloquer toutes les connexions entrantes, sauf celles qui sont explicitement requises, et bloquer tout accès Internet sortant.

   - Logiciel anti-programme malveillant pour protéger contre les menaces et les programmes malveillants connus.

   - Analyse de la surface des attaques pour empêcher l’introduction de nouveaux vecteurs d’attaque dans Windows lors de l’installation de nouveaux logiciels.

      > [!NOTE]
      > L’utilisation d’outils comme [Attack Surface Analyzer (ASA)](https://www.microsoft.com/download/details.aspx?id=24487) permet d’évaluer les paramètres de configuration sur un hôte et d’identifier les vecteurs d’attaque introduits par des modifications logicielles ou de configuration.

- Renforcement des comptes
   - Une authentification multifacteur doit être configurée pour tous les comptes de la forêt d’administration, sauf un. Au moins un compte administratif doit s’appuyer sur un mot de passe pour garantir le fonctionnement de l’accès en cas de rupture du processus d’authentification multifacteur. Ce compte doit être protégé par un processus de contrôle physique draconien.
   - Les comptes configurés pour l’authentification multifacteur doivent être configurés pour définir régulièrement un nouveau hachage NTLM sur les comptes. Pour ce faire, vous pouvez désactiver et activer l’attribut de compte Une carte à puce est nécessaire pour ouvrir une session interactive.

      > [!NOTE]
      > Vous pouvez ainsi interrompre des opérations en cours qui utilisent ce compte, alors ce processus doit uniquement être lancé quand les administrateurs ne vont pas utiliser le compte, par exemple pendant la nuit ou le week-end.

- Contrôles de détection
   - Les contrôles de détection pour la forêt d’administration doivent être conçus pour alerter en cas d’anomalies dans cette forêt. Le nombre limité de scénarios et d’activités autorisés peut aider à spécifier ces contrôles de façon plus précise que dans l’environnement de production.

Pour plus d’informations sur les services Microsoft de conception et déploiement d’un ESAE pour votre environnement, voir [cette page](https://www.microsoft.com/services).

## <a name="tier-0-equivalency"></a>Équivalence du niveau 0

La plupart des organisations contrôle l’appartenance aux puissants groupes Active Directory de niveau 0 comme les administrateurs, les administrateurs de domaine et les administrateurs de l’entreprise.  De nombreuses organisations négligent le risque lié aux autres groupes dont les privilèges sont pourtant équivalents dans un environnement Active Directory standard. Ces groupes représentent une voie d’escalade relativement facile pour un attaquant vers les mêmes privilèges de niveau 0 explicites grâce à diverses méthodes d’attaque.

Par exemple, un opérateur de serveur peut accéder à un média de sauvegarde d’un contrôleur de domaine et extraire toutes les informations d’identification des fichiers situés sur ce média pour les utiliser à des fins d’escalade de privilèges.

Les organisations doivent contrôler et surveiller l’appartenance dans tous les groupes de niveau 0 (y compris l’appartenance imbriquée), comme les groupes suivants :

- Administrateurs de l’entreprise
- Administrateurs du domaine
- Administrateurs de schéma
- BUILTIN\Administrators
- Opérateurs de compte
- Opérateurs de sauvegarde
- Opérateurs d'impression
- Opérateurs de serveur
- Contrôleurs de domaine
- Contrôleurs de domaine en lecture seule
- Propriétaires créateurs de la stratégie de groupe
- Opérateurs de chiffrement
- Utilisateurs du modèle COM distribué
- Les autres groupes délégués font référence aux groupes personnalisés susceptibles d’être créés par votre organisation pour gérer des opérations d’annuaire pouvant aussi disposer d’un accès de niveau 0 effectif.

## <a name="administrative-tools-and-logon-types"></a>Outils d’administration et types d’ouverture de session

Les informations de référence ci-après vous aident à identifier le risque d’exposition des informations d’identification associé à l’utilisation de différents outils d’administration pour l’administration à distance.

Dans un scénario d’administration à distance, les informations d’identification sont toujours exposées sur l’ordinateur source si bien qu’il est toujours recommandé d’utiliser une station de travail à accès privilégié digne de confiance pour les comptes sensibles ou à impact élevé. Le fait que les informations d’identification soient exposées ou non à un vol potentiel sur l’ordinateur (distant) cible dépend principalement du type d’ouverture de session Windows utilisé par la méthode de connexion.

Ce tableau comprend des recommandations pour les outils d’administration et les méthodes de connexion les plus courants :

|Méthode de connexion|Type d’ouverture de session|Informations d’identification réutilisables sur la destination|Commentaires|
|-----------|-------|--------------------|------|
|Ouverture de session sur console|Interactive (Interactif)|v|Inclut l’accès à distance au matériel/des cartes d’extinction et des KVM réseau.|
|RUNAS|Interactive (Interactif)|v||
|RUNAS /NETWORK|NewCredentials|v|Clone la session LSA en cours pour un accès local, mais utilise de nouvelles informations d’identification pour la connexion aux ressources réseau.|
|Bureau à distance (réussite)|RemoteInteractive|v|Si le client Bureau à distance est configuré pour partager des ressources et appareils locaux, ceux-ci peuvent être aussi compromis.|
|Bureau à distance (échec - type d’ouverture de session refusé)|RemoteInteractive|-|Par défaut, si l’ouverture de session RDP échoue, les informations d’identification sont uniquement stockées très brièvement. Cela peut ne pas être le cas si l’ordinateur est compromis.|
|Net use * \\\SERVER|Network (Réseau)|-||
|Net use * \\\SERVER /u:user|Network (Réseau)|-||
|Composants logiciels enfichables MMC sur ordinateur distant|Network (Réseau)|-|Exemple : Gestion de l’ordinateur, Observateur d’événements, Gestionnaire de périphériques, Services|
|PowerShell WinRM|Network (Réseau)|-|Exemple : Enter-PSSession server|
|PowerShell WinRM avec CredSSP|NetworkClearText|v|New-PSSession server<br />-Authentication Credssp<br />-Credential cred|
|PsExec sans informations d’identification explicites|Network (Réseau)|-|Exemple : PsExec \\\server cmd|
|PsExec avec informations d’identification explicites|Réseau + Interactif|v|PsExec \\\server -u user -p pwd cmd<br />Crée plusieurs sessions d’ouverture de session.|
|Accès à distance au Registre|Network (Réseau)|-||
|Passerelle des services Bureau à distance|Network (Réseau)|-|Authentification auprès de la passerelle des services Bureau à distance.|
|Tâche planifiée|Batch (Fichier de commandes)|v|Le mot de passe est également enregistré en tant que secret LSA sur le disque.|
|Exécuter des outils en tant que service|Service|v|Le mot de passe est également enregistré en tant que secret LSA sur le disque.|
|Analyseurs de vulnérabilité|Network (Réseau)|-|La plupart des analyseurs utilisent par défaut des ouvertures de session réseau, bien que certains fournisseurs puissent implémenter des ouvertures de session non-réseau et introduire plus de risque de vol d’informations d’identification.|

Pour l’authentification web, utilisez la référence indiquée dans le tableau ci-dessous :

|Méthode de connexion|Type d’ouverture de session|Informations d’identification réutilisables sur la destination|Commentaires|
|-----------|-------|--------------------|------|
|Authentification de base IIS|NetworkCleartext<br />(IIS 6.0+)<p>Interactive (Interactif)<br />(avant IIS 6.0)|v||
|Authentification Windows intégrée IIS|Network (Réseau)|-|Fournisseurs NTLM et Kerberos.|

Définitions des colonnes :

- **Type d’ouverture de session** identifie le type d’ouverture de session lancé par la connexion.
- **Informations d’identification réutilisables sur la destination** indique que les types suivants d’informations d’identification sont stockés dans la mémoire de processus LSASS sur l’ordinateur de destination où le compte spécifié est connecté en local :
   - Hachages LM et NT
   - TGT Kerberos
   - Mots de passe en texte en clair (le cas échéant)

Les symboles utilisés dans ce tableau ont la signification suivante :

- (-) indique que les informations d’identification ne sont pas exposées.
- (-) indique que les informations d’identification sont exposées.

Pour les applications de gestion qui ne figurent pas dans ce tableau, vous pouvez déterminer le type d’ouverture de session à partir du champ correspondant dans Auditer les événements de connexion. Pour plus d’informations, voir [Auditer les événements de connexion](https://technet.microsoft.com/library/cc787567(v=ws.10).aspx).

Sur les ordinateurs Windows, toutes les authentifications sont traitées comme l’un des nombreux types d’ouverture de session, quel que soit le protocole d’authentification ou l’authentificateur utilisé. Ce tableau inclut les types d’ouverture de session les plus courants et leurs attributs relatifs au vol d’informations d’identification :

|Type d’ouverture de session|#|Authentificateurs acceptés|Informations d’identification réutilisables dans une session LSA|Exemples|
|-------|---|--------------|--------------------|------|
|Interactif (également appelé ouverture de session locale)|2|Mot de passe, carte à puce,<br />autre|Oui|Ouverture de session console<br />RUNAS<br />Solutions de contrôle à distance du matériel (par exemple, KVM réseau ou accès à distance/carte d’extinction dans le serveur)<br />Authentification de base IIS (avant IIS 6.0)|
|Network (Réseau)|3|Mot de passe,<br />hachage NT,<br />ticket Kerberos|Non (sauf si une délégation est activée, alors des tickets Kerberos sont présents)|NET USE,<br />appels RPC,<br />Registre à distance,<br />authentification Windows intégrée IIS,<br />authentification Windows SQL|
|Batch (Fichier de commandes)|4|Mot de passe (généralement stocké en tant que secret LSA)|Oui|Tâches planifiées|
|Service|5|Mot de passe (généralement stocké en tant que secret LSA)|Oui|Windows Services|
|NetworkCleartext|8|Mot de passe|Oui|Authentification de base IIS (IIS 6.0 et versions ultérieures)<br />Windows PowerShell avec CredSSP|
|NewCredentials|9|Mot de passe|Oui|RUNAS /NETWORK|
|RemoteInteractive|10|Mot de passe, carte à puce,<br />autre|Oui|Bureau à distance (anciennement services Terminal Server)|

Définitions des colonnes :

- **Type d’ouverture de session** est le type d’ouverture de session demandé.
- **#** est l’identifiant numérique du type d’ouverture de session signalé dans les événements d’audit dans le journal des événements de sécurité.
- **Authentificateurs acceptés** indique les types d’authentificateurs capables de lancer une ouverture de session de ce type.
- **Informations d’identification réutilisables dans une session LSA** indique si le type d’ouverture de session entraîne la conservation des informations d’identification par la session LSA, comme les mots de passe en texte en clair, les hachages NT ou les tickets Kerberos pouvant être utilisés pour s’authentifier auprès d’autres ressources réseau.
- **Exemples** répertorie des scénarios courants dans lesquels le type d’ouverture de session est utilisé.

> [!NOTE]
> Pour plus d’informations sur les types d’ouverture de session, voir [SECURITY_LOGON_TYPE (énumération)](https://technet.microsoft.com/library/aa380129(VS.85).aspx).
