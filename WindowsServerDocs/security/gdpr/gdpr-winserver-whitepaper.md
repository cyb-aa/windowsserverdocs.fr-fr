---
title: Commencer à appliquer le Règlement général sur la protection des données (RGPD) pour Windows Server 2016
description: Cet article vous explique en quoi consiste le RGPD et présente ce qu'offrent les produits Microsoft pour vous préparer à la mise en conformité.
keywords: confidentialité, RGPD
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.technology: techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/25/2017
ms.author: nirb
ms.openlocfilehash: 1c374c00573e87594eeeab620face9ea9acaa531
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284201"
---
# <a name="beginning-your-general-data-protection-regulation-gdpr-journey-for-windows-server"></a>À partir de votre parcours général règlement de Protection des données (RGPD) pour Windows Server 

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cet article vous explique en quoi consiste le RGPD et présente ce qu'offrent les produits Microsoft pour vous préparer à la mise en conformité.

## <a name="introduction"></a>Introduction
Le 25 mai 2018, une loi de confidentialité européenne doit entrer en vigueur. Elle fixe une nouvelle norme mondiale en matière de droits à la vie privée, de sécurité et de conformité.

Le règlement général sur la protection des données (ou RGPD) concerne essentiellement la protection et l’application des droits à la vie privée des utilisateurs. Le RGPD établit des conditions strictes internationales en matière de protection de la vie privée qui régissent votre façon de gérer et de protéger les données personnelles tout en respectant les choix individuels, quel que soit l’endroit où les données sont envoyées, traitées ou stockées.

Microsoft et nos clients sont désormais en voie d'atteindre les objectifs de confidentialité du RGPD. Chez Microsoft, nous pensons que la confidentialité est un droit fondamental, et nous pensons que le RGPD est une étape importante pour clarifier et appliquer les droits individuels en matière de protection de la vie privée. Mais nous savons également que le RGPD nécessite des modifications importantes apportées par les organisations du monde entier.

Nous avons décrit notre engagement en faveur du RGPD, ainsi que notre soutien à nos clients dans le billet de blog [Devenir conforme au RGPD à l'aide du Microsoft Cloud](https://blogs.microsoft.com/on-the-issues/2017/02/15/get-gdpr-compliant-with-the-microsoft-cloud/#hv52B68OZTwhUj2c.99) de notre Responsable de la confidentialité [Brendon Lynch](https://blogs.microsoft.com/on-the-issues/author/brendonlynch/) et le billet de blog [Gagner votre confiance avec les engagements contractuels envers le règlement général sur la protection des données](https://blogs.microsoft.com/on-the-issues/2017/04/17/earning-trust-contractual-commitments-general-data-protection-regulation/#6QbqoGWXCLavGM63.99) de [Rich Sauer](https://blogs.microsoft.com/on-the-issues/author/rsauer/), vice-président du groupe Microsoft et directeur juridique adjoint.

Bien que votre mise en conformité avec le RGPD puisse sembler difficile, nous sommes là pour vous aider. Pour obtenir des informations spécifiques sur le RGPD, nos engagements et comment commencer à le mettre en application, visitez la [section RGPD du Microsoft Trust Center](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr).

## <a name="gdpr-and-its-implications"></a>Le RGPD et ses implications
Le RGPD est un règlement complexe qui peut nécessiter des modifications importantes de la façon dont vous recueillez, utilisez et gérez les données personnelles. Depuis longtemps, Microsoft aide ses clients à respecter les réglementations complexes et en ce qui concerne la préparation au RGPD, nous sommes votre partenaire dans cette démarche.

Le RGPD impose des règles aux organisations qui fournissent des biens et des services aux habitants de l’Union européenne (UE) ou qui collectent et analysent des données liées à des résidents de l’Union européenne, quel que soit l’emplacement où se trouvent les entreprises. Les éléments clés du RGPD sont notamment les suivants :

- **Droits de confidentialité améliorée.** Protection des données renforcée pour les résidents de l’Union européenne en garantissant qu’ils ont le droit d’accéder à leurs données personnelles, d'y corriger des inexactitudes, de les effacer, de contester le traitement de leurs données personnelles et de les déplacer.

- **Droit supérieur pour la protection des données personnelles.** Responsabilité renforcée des organisations qui traitent des données personnelles, en apportant plus de clarté sur la responsabilité d'assurer la conformité.

- **Les rapports de violation des données personnelles obligatoires.** Les organisations qui contrôlent des données à caractère personnel ont pour obligation de signaler à leurs autorités de contrôle toute violation de ces données qui présente un risque pour les droits et les libertés des individus, sans tarder et, si possible, au plus tard 72 heures après avoir pris connaissance de la violation.

Comme on peut s'y attendre, le RGPD peut avoir un impact significatif sur votre entreprise. Il peut éventuellement vous obliger à mettre à jour les politiques de confidentialité, à implémenter et renforcer les contrôles de protection de données et les procédures de notification de violation, à déployer des stratégies très transparentes, et à effectuer des investissements supplémentaires en informatique et en formation. Microsoft Windows 10 peut vous aider à répondre à certaines de ces exigences de manière efficace.

## <a name="personal-and-sensitive-data"></a>Données personnelles et confidentielles
Dans le cadre de vos efforts pour respecter le RGPD, vous devez comprendre comment le règlement définit les données personnelles et confidentielles et comment ces définitions se rapportent aux données détenues par votre organisation. Selon que présentation, que vous serez en mesure de découvrir où ces données sont créées, traitée, gérés et stockés.

Le RGPD considère comme données personnelles toutes les informations relatives à une personne physique identifiée ou identifiable. Cela peut inclure une identification directe (par exemple, votre nom légal) et une identification indirecte (comme des informations spécifiques qui font clairement référence à vous). Le RGPD montre clairement que le concept de données personnelles inclut les identificateurs en ligne (par exemple, les adresses IP, les ID d'appareil mobile) et les données de localisation.

Le RGPD introduit des définitions spécifiques pour les données génétiques (par exemple, la séquence génétique d'un individu) et les données biométriques. Les données génétiques et les données biométriques, ainsi que d’autres sous-catégories de données personnelles (les données personnelles révélant l’origine raciale ou ethnique, les opinions politiques, les convictions religieuses ou philosophiques ou l’adhésion à un syndicat ; les données concernant la santé ; ou les données concernant la vie sexuelle ou l’orientation sexuelle d'une personne) sont traitées comme des données personnelles sensibles dans le cadre du RGPD. Les données personnelles sensibles doivent bénéficier de protections renforcées et nécessitent généralement le consentement explicite d’un individu sur l'endroit où ces données doivent être traitées.

### <a name="examples-of-info-relating-to-an-identified-or-identifiable-natural-person-data-subject"></a>Exemples d’informations relatives à une personne physique identifiée ou identifiable (personne concernée)
Cette liste fournit des exemples de différents types d’informations régies par le RGPD. Cette liste n’est pas exhaustive.

-   Nom

-   Numéro d’identification (par exemple, SSN)

-   Données de localisation (par exemple, adresse du domicile)

-   Identificateur en ligne (par exemple, adresse de messagerie, noms d’écran, adresse IP, ID d'appareil)

-   Données pseudonymes (par exemple, utilisation d’une clé pour identifier des individus)

-   Données génétiques (par exemple, des échantillons biologiques d’un individu)

-   Données biométriques (par exemple, des empreintes digitales, la reconnaissance faciale)

## <a name="getting-started-on-the-journey-towards-gdpr-compliance"></a>Préparation au processus de mise en conformité avec le RGPD
Étant donné tout ce qu'implique la conformité au RGPD, nous vous recommandons vivement de ne pas attendre sa mise en vigueur pour vous préparer. Vous devez réviser vos pratiques de gestion de la confidentialité et des données personnelles dès à présent. Nous vous conseillons de commencer votre processus de mise en conformité avec le RGPD en vous concentrant sur quatre étapes principales :

-   **Découvrir.** Identifiez les données personnelles que vous détenez et leur emplacement. 

-   **Gérer.** Régissez la façon dont les données personnelles sont utilisées et accessibles.

-   **Protéger.** Établissez des contrôles de sécurité pour empêcher les vulnérabilités et les violations de données, les détecter et y réagir.  

-   **Rapport.** Consignez les demandes de données, signalez les violations de données et conservez les documents requis.

    ![Diagramme sur l'articulation des 4 principales étapes du RGPD](../media/GDPR-Windows-Server-Overview/gdpr-steps-diagram.png)

Pour chacune de ces étapes, nous décrivons des exemples d'outils, de ressources et de fonctionnalités de différentes solutions Microsoft qui permettent de répondre aux exigences propres à chaque étape. Bien que cet article n’est pas un guide complet « procédure », nous avons inclus des liens obtenir plus de détails, et plus d’informations est disponible dans le [section RGPD de Microsoft Trust Center](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr).

## <a name="windows-server-security-and-privacy"></a>Confidentialité et la sécurité de Windows Server
Le RGPD vous oblige à implémenter les mesures de sécurité techniques et organisationnelles appropriées pour protéger les données personnelles et les systèmes de traitement. Dans le contexte de la législation GDPR, vos environnements de serveurs physiques et virtuels potentiellement traitent des données personnelles et sensibles. Traitement peut signifier toute opération ou un ensemble d’opérations, telles que la collecte de données, stockage et la récupération.

Votre capacité à répondre à cette exigence et d’implémenter des mesures de sécurité techniques appropriées doit refléter les menaces que vous êtes confronté à de plus en plus hostile environnement informatique actuel. Les menaces de sécurité actuelles sont agressives et tenaces. Auparavant, la préoccupation majeure des pirates informatiques consistait à gagner la reconnaissance de leur communauté ; ils prenaient ainsi plaisir à lancer des attaques ou à neutraliser temporairement un système entier. Les motivations de ces pirates ont depuis évolué, ces derniers ayant réalisé que leurs attaques peuvent leur permettre de gagner de l'argent, par exemple en prenant en otage des appareils ou des données en échange d'une rançon.

Les attaques modernes consistent de plus en plus en des vols à grande échelle de propriété intellectuelle et en des dégradations d'un système cible, pouvant entraîner une perte financière. Et désormais, le cyberterrorisme menace même la sécurité des personnes, des entreprises ainsi que les intérêts des États du monde entier. Ces pirates sont généralement des personnes très compétentes et expertes en matière de sécurité. Certains d'entre eux travaillent même pour le compte des États, lesquels disposent de budgets conséquents et de ressources humaines quasiment illimitées. De telles menaces nécessitent d'adopter une approche adaptée pour relever ce type de défis.

Non seulement ces menaces mettent en péril votre capacité à garder le contrôle sur toutes les données personnelles ou confidentielles que vous pouvez détenir, mais elles représentent également un risque pour votre activité globale. Tenez compte des données récentes de McKinsey, Ponemon Institute, Verizon et Microsoft :

- Le coût moyen du type de violation de données que le RGPD vous demande de signaler est de 3,5 M d'USD.

- 63 % de ces violations impliquent des mots de passe faibles ou volés que le RGPD vous demande de traiter.

- Plus de 300 000 nouveaux exemples de programmes malveillants sont créés et propagés tous les jours, ce qui complique encore plus la tâche pour assurer la protection des données.

Comme avec les récentes attaques de Ransomware, appelés une fois le handicape encore noir d’Internet, des personnes malveillantes vont AfterTargets plus volumineuses que les moyens de plus, avec les conséquences potentiellement catastrophiques, tant. Le RGPD inclut des sanctions qui rendent vos systèmes, y compris les ordinateurs de bureau et ordinateurs portables, qui contiennent en effet les cibles de riches des données personnelles et sensibles.

Deux principes clés ont guidé et continuent guider le développement de Windows :

- **Sécurité.** Les données de que nos logiciels et services stockent pour le compte de nos clients doivent protégées contre les dommages et utilisées ou modifiées uniquement de manière appropriée. Modèles de sécurité devraient être faciles pour les développeurs à comprendre et à générer dans leurs applications.

- **Confidentialité.** Les utilisateurs doivent être dans le contrôle de l’utilisation de leurs données. Stratégies pour l’utilisation des informations doivent être claires à l’utilisateur. Les utilisateurs doivent être dans le contrôle de quand et si l’utilisateur reçoit des informations pour tirer meilleur parti de leur temps. Il doit être facile aux utilisateurs de spécifier l’utilisation appropriée de leurs informations, y compris le contrôle de l’utilisation de courrier électronique, ils envoient.

Microsoft est resté fermes contre ces principes comme l’a récemment indiqués par le PDG de Microsoft, Satya Nadella, 

> «_Comme le monde ne cesse d’évoluer et l’évoluent des besoins de l’entreprise, certaines choses sont cohérents : à la demande d’un client pour la sécurité et de confidentialité._ »

Comme vous travaillez pour se conformer au RGPD, comprendre le rôle de vos serveurs physiques et virtuels dans la création, l’accès à, traitement, stockage et la gestion des données qui peuvent être qualifié personnelles et des données potentiellement sensibles sous le RGPD sont importantes. Windows Server fournit des fonctionnalités qui vous aideront à répondent aux exigences RGPD pour implémenter des mesures de sécurité techniques et organisationnelles appropriées pour protéger les données personnelles.

L’état de sécurité de Windows Server 2016 n’est pas un bolt-on ; Il s’agit d’un principe d’architecture. Et bien, il peut être de mieux comprendre dans quatre principaux :

- **Protéger.** Le focus en cours et l’innovation sur des mesures préventives ; bloquer les attaques connues et les logiciels malveillants connus.

- **Détecter.** Complète d’outils pour vous aider à repérer anomalies et répondre aux attaques plus rapides de surveillance.

- **Répondre.** Début de réponse et technologies de récupération plus profond une expertise-Conseil.

- **Isoler.** Isoler les secrets de données et les composants du système d’exploitation, limiter les privilèges d’administrateur et mesurer rigoureusement intégrité de l’hôte.

Avec Windows Server, votre capacité à protéger, détecter et se défendre contre les types d’attaques pouvant conduire à des violations de données est considérablement améliorée. Étant donné les exigences strictes concernant la notification des violations dans le RGPD, en garantissant une bonne protection de vos systèmes de bureau et portables, vous limitez la probabilité d'être confronté à des risques pouvant entraîner une analyse et une notification coûteuses de la violation.

Dans la section qui suit, vous verrez comment Windows Server fournit des fonctionnalités qui correspondent essentiellement à l’étape « Protéger » de votre voyage vers la conformité RGPD. Ces fonctionnalités se répartissent en trois scénarios de protection :

- **Protéger vos informations d’identification et de limiter les privilèges d’administrateur.** Windows Server 2016 permet d’implémenter ces modifications, afin d’empêcher votre système d’être utilisé comme un point de lancement pour davantage d’intrusions.

- **Sécuriser le système d’exploitation pour exécuter vos applications et votre infrastructure.** Windows Server 2016 offre des couches de protection, ce qui vous aide à bloquer les intrus externes à partir des logiciels malveillants en cours d’exécution ou exploitent les vulnérabilités.

- **Virtualisation sécurisée.** Windows Server 2016 permet la virtualisation sécurisée, à l’aide des Machines virtuelles protégées et infrastructure service Guardian. Cela vous permet de chiffrer et d’exécuter vos machines virtuelles sur des hôtes approuvés dans votre infrastructure, mieux les protéger contre les attaques malveillantes.

Ces fonctionnalités, décrites plus en détail ci-dessous avec des références aux exigences du RGPD spécifiques, sont appuient sur la protection avancée des appareils qui permet de maintenir l’intégrité et la sécurité du système d’exploitation et des données.

Une disposition de clé dans le RGPD est la protection des données à la conception et, par défaut et aider avec votre capacité à répondre à cette disposition sont des fonctionnalités dans Windows 10, telles que le chiffrement d’appareil BitLocker. BitLocker utilise la technologie de Module de plateforme sécurisée (TPM), qui fournit des fonctions liées à la sécurité basée sur le matériel. Ce processeur de chiffrement inclut plusieurs mécanismes de sécurité physique pour le rendre inviolables, et logiciels malveillants ne peut pas falsifier les fonctions de sécurité du module TPM.

La puce comprend plusieurs mécanismes de sécurité physique qui la protègent contre la falsification, et les logiciels malveillants ne peuvent pas falsifier les fonctions de sécurité du TPM. Les principaux avantages de la technologie de TPM sont que vous pouvez :

-   générer, stocker et limiter l’utilisation des clés de chiffrement ;

-   utiliser la technologie TPM pour l’authentification des appareils de la plateforme à l’aide de la clé RSA unique du TPM, qui est gravée sur elle-même ;

-   garantir l’intégrité de la plateforme en réalisant et en stockant des mesures sur la sécurité.

Pour assurer une protection avancée des appareils supplémentaire, adaptée à votre exploitation et sans violations de données, vous pouvez notamment utiliser le démarrage sécurisé de Windows pour préserver l’intégrité du système en garantissant qu'un programme malveillant ne puisse pas démarrer avant les défenses du système.

## <a name="windows-server-supporting-your-gdpr-compliance-journey"></a>Windows Server : Prise en charge de votre voyage vers la conformité RGPD
Les fonctionnalités clés dans Windows Server peuvent vous aider à efficacement implémenter les mécanismes de sécurité et confidentialité que du RGPD nécessite la conformité. Tandis que l’utilisation de ces fonctionnalités ne garantit pas la conformité, ils prendront en charge vos efforts à faire.

Le système d’exploitation se trouve sur une couche de niveau stratégique dans l’infrastructure une organisation, offrant de nouvelles opportunités pour créer des couches de protection contre les attaques susceptible de voler des données et d’interruption de votre entreprise. Principaux aspects de la législation GDPR tels que Privacy by Design, de Protection des données et de contrôle d’accès doivent être traitées au sein de votre infrastructure informatique au niveau du serveur.

Utilisation pour aider à protéger l’identité, système d’exploitation et les couches de virtualisation, Windows Server 2016 vous aide à bloquer les vecteurs d’attaque courants utilisés pour obtenir un accès illégal à vos systèmes : vol des informations d’identification, les logiciels malveillants et une structure de virtualisation compromis. En plus de réduire les risques commerciaux, les composants de sécurité intégrée à Windows Server 2016 aide répondre aux exigences de conformité pour les clés gouvernementales et industrielles règles de sécurité. 

Ces identités, système d’exploitation et les protections de la virtualisation vous permettent de mieux protéger votre centre de données exécutant Windows Server comme une machine virtuelle dans n’importe quel cloud et limiter la capacité des pirates à compromettre les informations d’identification, lancer des programmes malveillants et ne soit pas détectée dans votre réseau. De même, quand déployée comme un hôte Hyper-V, Windows Server 2016 offre la garantie de sécurité pour vos environnements de virtualisation par le biais des Machines virtuelles protégées et les fonctionnalités de pare-feu distribué. Avec Windows Server 2016, le système d’exploitation devient un participant actif de la sécurité de votre centre de données.

### <a name="protect-your-credentials-and-limit-administrator-privileges"></a>Protéger vos informations d’identification et de limiter les privilèges d’administrateur 
Contrôler l’accès aux données personnelles et les systèmes qui traitent ces données, est une zone avec le RGPD qui a des exigences spécifiques, y compris l’accès par les administrateurs. Identités privilégiées sont tous les comptes qui ont des privilèges, tels que les comptes d’utilisateur qui sont membres du domaine Administrateurs, les administrateurs d’entreprise, administrateurs locaux ou même les groupes d’utilisateurs avec pouvoir élevés. Ces identités peuvent également inclure les comptes qui disposent des privilèges directement, tels que l’exécution de sauvegardes, en cours d’arrêt du système ou d’autres droits répertoriés dans le nœud d’attribution des droits utilisateur dans la console de stratégie de sécurité locale.

En tant que principe de contrôle d’accès général et en ligne avec le RGPD, vous devez protéger ces identités privilégiées d’être compromis par des attaquants potentiels. Tout d’abord, il est important de comprendre comment les identités sont compromises ; Vous pouvez ensuite planifier empêcher les attaquants d’accéder à ces identités privilégiées.

#### <a name="how-do-privileged-identities-get-compromised"></a>Comment compromises identités privilégiées ?
Identités privilégiées peuvent être compromises lorsque les organisations n’ont des instructions pour les protéger. Voici quelques exemples :

- **Plus de privilèges que nécessaire.** Un des problèmes plus courants est que les utilisateurs ont plus de privilèges que nécessaire pour effectuer leur fonction. Par exemple, un utilisateur qui gère les DNS peut être un administrateur Active Directory. En règle générale, cela pour éviter la nécessité de configurer les différents niveaux d’administration. Toutefois, si un tel compte est compromis, l’attaquant automatiquement dispose de privilèges élevés.

- **Connecté en permanence avec des privilèges élevés.** Un autre problème courant est que les utilisateurs ayant des privilèges élevés peuvent l’utiliser pour une durée illimitée. Cela est très courant avec les professionnels de l’informatique se connecter à un ordinateur de bureau à l’aide d’un compte privilégié, restent connecté et utilisent le compte privilégié pour naviguer sur le web et utiliser le courrier électronique (travail informatique type fonctions). Une durée illimitée des comptes privilégiés rend le compte plus vulnérables aux attaques et augmente les chances que le compte sera compromis.

- **Recherche de l’ingénierie sociale.** La plupart des menaces d’informations d’identification commencent par la recherche de l’organisation et puis menées via l’ingénierie sociale. Par exemple, une personne malveillante peut effectuer une attaque par phishing e-mail compromission des comptes légitimes (mais pas nécessairement avec élévation de privilèges de comptes) qui ont accès à un réseau d’entreprise. L’attaquant utilise ensuite ces comptes valides pour effectuer des recherches supplémentaires sur votre réseau et pour identifier des comptes privilégiés qui peuvent effectuer des tâches administratives. 

- **Utilisez des comptes avec des privilèges élevés.** Même avec un compte d’utilisateur normal, et non avec élévation de privilèges dans le réseau, les attaquants peuvent accéder aux comptes avec des autorisations élevées. Une des méthodes plus courantes de cette opération est donc à l’aide de la Pass-the-Hash ou les attaques Pass-the-Token. Pour plus d’informations sur la Pass-the-Hash et autres techniques de vol d’informations d’identification, consultez les ressources sur le [pagedePass-the-Hash (PtH)](https://technet.microsoft.com/dn785092.aspx).

Il existe bien sûr des autres méthodes que les pirates peuvent utiliser pour identifier et de compromettre les identités privilégiées (avec de nouvelles méthodes qui est créés chaque jour). Il est donc important d’établir des pratiques pour les utilisateurs de se connecter avec des comptes à moindres privilèges afin de réduire la capacité des pirates d’accéder à des identités privilégiées. Les sections suivantes abordent les fonctionnalités où Windows Server peut atténuer ces risques.

#### <a name="just-in-time-admin-jit-and-just-enough-admin-jea"></a>L’administration juste-à-temps (JIT) et Just Enough administration (JEA)
Alors que la protection contre la Pass-the-Hash ou les attaques Pass-the-Ticket est importante, les informations d’identification d’administrateur peuvent toujours être volées par d’autres moyens, y compris l’ingénierie sociale, les employés mécontents et par force brute. Par conséquent, en plus de l’isolation des informations d’identification autant que possibles, vous également un moyen utile pour limiter la portée de privilèges de niveau administrateur dans le cas où ils sont compromis.

Trop de comptes administrateur sommes superprivilégié, même s’ils n'ont qu’un seul domaine de responsabilité. Par exemple, un administrateur DNS, qui nécessite un ensemble très restreint de privilèges pour gérer les serveurs DNS, est accordé souvent les privilèges de niveau administrateur de domaine. En outre, étant donné que ces informations d’identification sont accordées pour perpétuité, est illimité sur la durée pendant laquelle ils peuvent être utilisés.

Chaque compte avec des privilèges de niveau administrateur de domaine inutiles augmente votre exposition aux attaquants qui cherchent à compromettre les informations d’identification. Pour réduire la surface d’attaque, que vous souhaitez fournir uniquement l’ensemble spécifique de droits d’administrateur a besoin d’effectuer la tâche – et uniquement pour la fenêtre de temps nécessaire pour la terminer.

À l’aide de Just Enough Administration et juste-à-temps Administration, les administrateurs peuvent imposer les privilèges spécifiques dont ils ont besoin pour la fenêtre exacte du temps nécessaire. Pour l’administrateur DNS, par exemple, à l’aide de PowerShell pour activer Just Enough Administration vous permet de créer un ensemble limité de commandes qui sont disponibles pour la gestion du DNS.

Si l’administrateur DNS a besoin effectuer une mise à jour à un de ses serveurs, elle demande l’accès à gérer DNS à l’aide de Microsoft Identity Manager 2016. Le workflow de demande peut inclure un processus d’approbation, telles que l’authentification à deux facteurs, ce qui pourrait appeler téléphone mobile de l’administrateur pour vérifier son identité avant d’accorder les privilèges requis. Une fois accordé, ces privilèges DNS fournissent un accès au rôle de PowerShell pour DNS pour un intervalle de temps spécifique.

Imaginons le scénario si les informations d’identification de l’administrateur DNS ont été volées. Tout d’abord, étant donné que les informations d’identification sans privilèges d’administrateur qui leur sont attachées, l’attaquant ne pourra pas accéder au serveur DNS – ou tout autre système – pour apporter des modifications. Si l’attaquant a essayé de demander des privilèges pour le serveur DNS, authentification de second facteur invitait à confirmer leur identité. Dans la mesure où il est peu probable que l’attaquant a téléphone mobile de l’administrateur DNS, l’authentification échoue. Cela serait verrouiller l’attaquant du système et le service informatique d’alertes que les informations d’identification peuvent être compromises.

En outre, de nombreuses organisations utilisent la version gratuite [Solution de mot de passe d’administrateur Local (LAPS)](http://aka.ms/laps) comme un mécanisme d’administration JIT simple et puissant pour leurs systèmes serveur et client. La fonctionnalité LAPS assure la gestion des mots de passe de compte local des ordinateurs joints au domaine. Les mots de passe sont stockés dans Active Directory (AD) et protégés par et liste de contrôle d’accès (ACL) donc les utilisateurs éligibles uniquement peuvent lire ou demander sa réinitialisation.

Comme indiqué dans le [Guide d’atténuation contre le vol des informations d’identification Windows](https://www.microsoft.com/en-us/download/confirmation.aspx?id=54095), 

> «_les criminels d’outils et techniques utilisent pour effectuer le vol d’informations d’identification et améliorent les attaques de réutilisation, des attaquants malveillants trouvent qu’il est plus facile à atteindre leurs objectifs. Le vol d’informations d’identification s’appuie souvent sur des pratiques opérationnelles ou l’exposition d’informations d’identification des utilisateurs, et ainsi les atténuations efficaces nécessitent une approche holistique qui traite des personnes, processus et technologie. En outre, ces attaques s’appuient sur l’attaquant vol des informations d’identification après compromettre un système pour développer ou de conserver des accès, afin des organisations doivent contenir rapidement des violations en implémentant des stratégies qui empêchent les pirates de se déplacer librement et sans être détecté dans un réseau compromis._ »

Une considération de conception importantes pour Windows Server a été atténuation contre le vol d’informations d’identification, en particulier, dérivée des informations d’identification. Credential Guard fournit améliore considérablement la sécurité contre le vol des informations d’identification dérivée et la réutilisation en implémentant des modifications architecturales importantes dans Windows conçu pour aider à éliminer les attaques d’isolation basés sur le matériel plutôt que de simplement s’efforcer de défendre de celles-ci.

Alors que le vol d’informations d’identification à l’aide de Windows Defender Credential Guard, NTLM et Kerberos, les informations d’identification dérivées sont protégées à l’aide de la sécurité basée sur la virtualisation, des attaques techniques et outils utilisés dans de nombreuses attaques ciblées sont bloquées. Les programmes malveillants exécutés dans le système d’exploitation avec des privilèges administratifs ne peuvent pas extraire les secrets protégés par la sécurité basée sur la virtualisation. Tandis que Windows Defender Credential Guard est une solution puissante, contre les menaces persistantes attaques va probablement MAJ à nouvelles techniques d’attaque et vous doit aussi intégrer Device Guard, comme décrit ci-dessous, ainsi que d’autres stratégies de sécurité et les architectures.

#### <a name="windows-defender-credential-guard"></a>Windows Defender Credential Guard
Windows Defender Credential Guard utilise la sécurité basée sur la virtualisation pour isoler les informations d’identification, empêchant les hachages de mot de passe ou les tickets Kerberos d’être intercepté. Il utilise un processus autorité de sécurité locale (LSA) isolé entièrement nouveau, ce qui n’est pas accessible au reste du système d’exploitation. Utilisé par l’autorité LSA isolé de tous les fichiers binaires sont signés avec des certificats sont validés avant de les lancer dans l’environnement protégé, effectuer des attaques de type Pass-the-Hash complètement inefficace.

Windows Defender Credential Guard utilise :

- Sécurité basée sur la virtualisation (obligatoire). Également requis :

    - Processeur 64 bits

    - Extensions de virtualisation de processeur, ainsi que de tables de la page étendue

    - Hyperviseur Windows

- Démarrage sécurisé (obligatoire)

- TPM 2.0 discret ou microprogramme (recommandé : fournit une liaison au matériel)

Vous pouvez utiliser Windows Defender Credential Guard pour vous aider à protéger les identités privilégiées en protégeant les informations d’identification et dérivés des informations d’identification sur Windows Server 2016. Pour plus d’informations sur la configuration requise de Windows Defender Credential Guard, consultez [protéger dérivés des informations d’identification de domaine avec Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard).

#### <a name="windows-defender-remote-credential-guard"></a>Windows Defender Credential Guard à distance
Windows Defender à distance Credential Guard sur Windows Server 2016 et de mise à jour anniversaire de Windows 10 vous permet également de protéger les informations d’identification pour les utilisateurs disposant de connexions Bureau à distance. Auparavant, toute personne utilisant des Services Bureau à distance devrait ouvrir une session sur leur ordinateur local et puis être requis pour se connecter à nouveau quand elle a effectué une connexion à distance à leur ordinateur cible. Cette deuxième connexion peut passer des informations d’identification sur l’ordinateur cible, les exposer à Pass-the-Hash ou Pass-the-Ticket attaques.

Avec Windows Defender à distance Credential Guard, Windows Server 2016 implémente l’authentification unique pour les sessions Bureau à distance, en éliminant la nécessité de les entrer à nouveau votre nom d’utilisateur et le mot de passe. Au lieu de cela, elle s’appuie sur les informations d’identification que vous avez déjà utilisé pour ouvrir une session sur votre ordinateur local. Pour utiliser Credential Guard à distance de Windows Defender, le client Bureau à distance et le serveur doivent remplir les conditions suivantes :

- Doit être joint à un domaine Active Directory et se trouver dans le même domaine ou un domaine avec une relation d’approbation.

- Doit utiliser l’authentification Kerberos.

- Doit être en cours d’exécution au moins Windows 10 version 1607 ou Windows Server 2016.  

- L’application de Windows classique de bureau à distance est obligatoire. L’application de plateforme de Windows universelle à distance Desktop ne prend pas en charge de Credential Guard à distance de Windows Defender.

Vous pouvez activer Credential Guard à distance de Windows Defender à l’aide d’un paramètre de Registre sur le serveur Bureau à distance et d’une stratégie de groupe ou d’un paramètre de connexion Bureau à distance sur le client Bureau à distance. Pour plus d’informations sur l’activation de Credential Guard à distance de Windows Defender, consultez [informations d’identification de protéger le Bureau à distance avec Credential Guard à distance de Windows Defender](https://docs.microsoft.com/windows/access-protection/remote-credential-guard). Avec Windows Defender Credential Guard, vous pouvez utiliser Credential Guard à distance de Windows Defender pour aider à protéger les identités privilégiées sur Windows Server 2016.

### <a name="secure-the-operating-system-to-run-your-apps-and-infrastructure"></a>Sécuriser le système d’exploitation pour exécuter vos applications et l’infrastructure
Empêcher les menaces informatiques nécessite également la recherche et les logiciels malveillants et les attaques qui cherchent à prendre le contrôle par Subversion les pratiques de fonctionnement standards de votre infrastructure de blocage. Si des personnes malveillantes peuvent obtenir un système d’exploitation ou une application de s’exécuter de manière non prédéterminés, non viable, ils utilisent probablement ce système pour effectuer des actions malveillantes. Windows Server 2016 fournit des couches de protection qui bloquent les logiciels malveillants en cours d’exécution ou exploitent les vulnérabilités des attaquants externes. Le système d’exploitation prend un rôle actif dans la protection des applications et infrastructure par les alertes aux administrateurs d’activité qui indique un système a été enfreint.

#### <a name="windows-defender-device-guard"></a>Windows Defender Device Guard
Windows Server 2016 inclut Windows Defender Device Guard pour vous assurer que seuls les logiciels approuvés peuvent être exécuté sur le serveur. À l’aide de la sécurité basée sur la virtualisation, il peut limiter les fichiers binaires peuvent s’exécuter sur le système basé sur la stratégie de l’organisation. Si quoi que ce soit, autres que les fichiers binaires spécifiés tente d’exécuter Windows Server 2016 il bloque et enregistre l’échec de la tentative afin que les administrateurs peuvent voir qu’une violation potentielle a été. Notification de violation est un élément essentiel de la configuration requise pour la conformité RGPD.

Windows Defender Device Guard est également intégré avec PowerShell afin que vous pouvez autoriser les scripts peuvent être exécutés sur votre système. Dans les versions antérieures de Windows Server, les administrateurs pourraient contourner la mise en œuvre de l’intégrité du code en supprimant simplement la stratégie à partir du fichier de code. Avec Windows Server 2016, vous pouvez configurer une stratégie qui est signée par votre organisation afin que seulement une personne ayant accès au certificat qui a signé la stratégie peut modifier la stratégie.

#### <a name="control-flow-guard"></a>Protection du flux de contrôle 
Windows Server 2016 inclut également une protection intégrée contre certaines classes d’attaques de corruption de mémoire. Il est important de mise à jour corrective de vos serveurs, mais il existe toujours un risque que les logiciels malveillants pouvant être développés pour une vulnérabilité qui n’a pas encore été identifiée. Certaines des méthodes plus courantes pour ces vulnérabilités sont pour fournir des données inhabituelles ou extrêmes à un programme en cours d’exécution. Par exemple, un attaquant peut exploiter une vulnérabilité de dépassement de capacité de mémoire tampon en fournissant plus d’entrée à un programme que prévu et de dépassement de la zone réservée par le programme pour contenir une réponse. Ceci peut corrompre la mémoire adjacent qui peut contenir un pointeur de fonction.

Lorsque le programme appelle via cette fonction, il peut ensuite accéder à un emplacement inattendu spécifié par l’attaquant. Ces attaques sont également appelés orienté sur le renvoi des attaques par programmation (JOP). Protection du flux de contrôle empêche les attaques JOP en plaçant de sévères restrictions sur quel code d’application peut être exécutée : particulièrement indirecte d’instructions d’appel. Il ajoute des vérifications de sécurité léger pour identifier l’ensemble de fonctions dans l’application qui sont des cibles valides pour les appels indirects. Quand une application s’exécute, il vérifie que ces cibles d’appels indirects sont valides.

Si la vérification de la protection du flux de contrôle échoue lors de l’exécution, Windows Server 2016 immédiatement met fin au programme, avec rupture n’importe quel code malveillant qui tente d’appeler indirectement une adresse non valide. Protection du flux de contrôle fournit une couche de protection supplémentaire importante pour Device Guard. Si une application autorisés a été compromise, il serait en mesure d’exécuter non vérifiées par Device Guard, étant donné que le Device Guard filtrage verriez que l’application a été signée et qu’il est considéré comme approuvé.

Mais parce que la protection du flux de contrôle peut identifier si l’application s’exécute dans un ordre non viable non prédéterminé, l’attaque échoue, empêche l’application compromise en cours d’exécution. Ensemble, ces protections rendent très difficile pour les attaquants d’injecter des logiciels malveillants dans logiciel s’exécutant sur Windows Server 2016.

Les développeurs qui créent des applications où les données personnelles seront gérées sont encouragés à activer la protection de flux de contrôle (CFG) dans leurs applications. Cette fonctionnalité est disponible dans Microsoft Visual Studio 2015 et s’exécute sur « CFG prenant en charge les » versions de Windows, le x86 et x64 libère pour le bureau et serveur de Windows 10 et mise à jour de Windows 8.1 (KB3000850). Vous n’êtes pas obligé d’activer CFG pour toutes les parties de votre code, comme un mélange de CFG activé et non-CFG activé le code exécuté correctement. Mais ne parvient pas à activer CFG pour tout le code peut ouvrir les lacunes dans la protection. En outre, CFG activé code fonctionne correctement sur « CFG dépendant pas des » versions de Windows et est donc entièrement compatible avec eux.

#### <a name="windows-defender-antivirus"></a>Antivirus Windows Defender
Windows Server 2016 inclut les secteur non significatif, active fonctionnalités de détection de Windows Defender pour bloquer les logiciels malveillants connus. Windows Defender Antivirus (AV) fonctionne avec Windows Defender Device Guard et protection du flux de contrôle pour empêcher l’installation sur vos serveurs de code malveillant d’aucune sorte. Elle est activée par défaut, l’administrateur n’a pas besoin à intervenir pour pouvoir commencer à travailler. Antivirus Windows Defender est également optimisé pour prendre en charge les différents rôles de serveur dans Windows Server 2016. Dans le passé, les attaquants utilisé interpréteurs de commandes, tels que PowerShell pour lancer des codes binaires malveillants. Dans Windows Server 2016, PowerShell est maintenant intégré à l’antivirus Windows Defender pour rechercher les logiciels malveillants avant de lancer le code.

Antivirus Windows Defender est une solution anti-programme malveillant intégrée qui offre une gestion de sécurité et les logiciels anti-programme malveillant pour postes de travail, ordinateurs portables et les serveurs. Antivirus Windows Defender a été considérablement améliorée dans la mesure où il a été introduit dans Windows 8. Antivirus Windows Defender dans Windows Server utilise une approche à plusieurs fronts pour améliorer le logiciel anti-programme malveillant :

- La **Protection via le Cloud** vous aide à détecter et à bloquer les nouveaux programmes malveillants en quelques secondes, dès leur apparition.

- Le**contexte local enrichi** améliore la façon dont les programmes malveillants sont identifiés. Windows Server informe l’antivirus Windows Defender non seulement sur le contenu comme des fichiers et des processus, mais également le contenu d'où proviennent, où il a été stocké et bien plus encore. 

- **Capteurs globales complètes** garder antivirus Windows Defender actuel et en prenant en charge de même les plus récentes contre les programmes malveillants. Ceci, de deux façons : en collectant les données du contexte local enrichi à partir de points de terminaison et en analysant ces données de manière centralisée.

- **Les falsifications** vous aide à protéger l’antivirus Windows Defender lui-même contre les attaques de programmes malveillants. Par exemple, les antivirus Windows Defender utilise des processus protégés, ce qui empêche les processus non approuvés à partir de la tentative de falsifier des composants de l’antivirus Windows Defender, ses clés de Registre et ainsi de suite.

- **Fonctionnalités de niveau entreprise** donner aux professionnels de l’informatique les outils et les options de configuration nécessaires pour rendre Windows Defender AV une solution d’entreprise contre les logiciels malveillants.

#### <a name="enhanced-security-auditing"></a>L’audit de sécurité améliorée 
Windows Server 2016 avertit activement les administrateurs aux tentatives de violation potentielle avec l’audit de sécurité renforcée qui fournit des informations plus détaillées, ce qui peuvent être utilisées pour la détection des attaques plus rapide et l’analyse d’investigation. Il enregistre les événements à partir de la protection du flux de contrôle, Windows Defender Device Guard et autres fonctionnalités de sécurité dans un seul emplacement, ce qui facilite pour les administrateurs afin de déterminer les systèmes qui peuvent être menacée.

Nouvelles catégories d’événements sont les suivantes :

- **Appartenance au groupe d’audit.** Vous permet d’auditer les informations d’appartenance de groupe dans le jeton de connexion d’un utilisateur. Événements sont générés lorsque des appartenances aux groupes sont énumérées ou interrogés sur l’ordinateur où l’ouverture de session a été créé. 
 
- **Auditer l’activité PnP.** Vous permet d’auditer lorsque plug-and-play détecte un périphérique externe – qui peut contenir des logiciels malveillants. Événements PnP peuvent être utilisés pour effectuer le suivi des modifications dans le matériel du système. Une liste de codes de fournisseurs de matériel est incluse dans l’événement.

Windows Server 2016 s’intègre facilement avec les systèmes de gestion (SIEM) d’événements incident de sécurité, telles que Microsoft Operations Management Suite (OMS), qui peut intégrer les informations dans les rapports d’intelligence sur les failles potentielles. La profondeur des informations fournies par l’audit amélioré permet aux équipes de sécurité identifier et répondre aux failles potentielles plus rapidement et efficacement.

### <a name="secure-virtualization"></a>Virtualisation sécurisée
Les entreprises aujourd'hui virtualiser tout ce dont ils peuvent, à partir de SQL Server à SharePoint pour les contrôleurs de domaine Active Directory. Machines virtuelles (VM) rendent simplement plus facile à déployer, gérer, service et automatiser votre infrastructure. Mais lorsqu’il s’agit à la sécurité, les structures de virtualisation compromis sont devenus un nouveau vecteur d’attaque est difficile de se défendre contre – jusqu'à présent. À partir d’un point de vue RGPD, vous devez considérer sur la protection des machines virtuelles que vous devez protéger les serveurs physiques notamment l’utilisation de la technologie TPM de VM.

Windows Server 2016 change radicalement la façon dont les entreprises peuvent sécuriser la virtualisation, en incluant plusieurs technologies qui vous permettent de créer des machines virtuelles qui s’exécutera uniquement sur votre propre structure ; Contribuez à protéger contre le stockage, réseau et elles s’exécutent sur des appareils hôte.

#### <a name="shielded-virtual-machines"></a>Machines virtuelles dotées d’une protection maximale
Les mêmes choses que mettre donc facile de migrer des machines virtuelles, sauvegarde et réplication, également les rendent plus facile à modifier et à copier. Une machine virtuelle étant simplement un fichier, il n’est pas protégé sur le réseau, dans le stockage, dans les sauvegardes ou ailleurs. Un autre problème est que les administrateurs de structure – qu’ils soient un administrateur de stockage ou un administrateur réseau – ont accès à toutes les machines virtuelles.

Un administrateur compromis sur l’infrastructure peut facilement provoquer compromis des données entre les machines virtuelles. Tout ce que l’attaquant doit faire est d’utiliser les informations d’identification compromises pour copier les fichiers de machine virtuelle ils le souhaitent sur un lecteur USB et de remonter hors de l’organisation, où les fichiers de machine virtuelle sont accessible à partir de n’importe quel autre système. Si l’une de ces machines virtuelles volés était un contrôleur de domaine Active Directory, par exemple, l’attaquant peut facilement afficher le contenu et utilisez les techniques disponibles par force brute pour craquer les mots de passe dans la base de données Active Directory, finalement leur donner accès à tout le reste de votre infrastructure.

Windows Server 2016 introduit protégées des Machines virtuelles (machines virtuelles protégées) pour vous protéger contre les scénarios comme celui que nous venons de décrire. Machines virtuelles protégées incluent un appareil TPM virtuel, ce qui permet aux organisations d’appliquer le chiffrement BitLocker sur les ordinateurs virtuels et vous assurer qu’ils s’exécutent uniquement sur des hôtes approuvés pour vous protéger contre les compromis stockage, réseau et les administrateurs d’hôtes. Machines virtuelles protégées sont créés à l’aide de la génération 2 machines virtuelles qui prennent en charge du microprogramme Unified Extensible Firmware Interface (UEFI) et disposer du module TPM virtuel.

#### <a name="host-guardian-service"></a>Service Guardian hôte
En même temps que les machines virtuelles protégées, le Service Guardian hôte est un composant essentiel pour la création d’une structure de virtualisation sécurisée. Son travail consiste à attester de l’intégrité d’un ordinateur hôte Hyper-V avant d’autoriser machine virtuelle protégée pour démarrer ou pour migrer à l’hôte. Il concerne les clés pour les machines virtuelles protégées et ne les libère jusqu'à ce que l’intégrité de la sécurité est assurée. Il existe deux méthodes que vous pouvez exiger que les hôtes Hyper-V d’attester le Service Guardian hôte.

La première et la plus sûre, sont l’attestation approuvée par le matériel. Cette solution nécessite que vos machines virtuelles protégées sont en cours d’exécution sur les ordinateurs hôtes qui ont les puces TPM 2.0 et UEFI 2.3.1. Ce matériel est requis pour fournir le démarrage mesuré et informations de l’intégrité du noyau de système d’exploitation requises par le Service Guardian hôte pour vous assurer de l’hôte Hyper-V n’a pas été falsifiées.

Les organisations informatiques ont l’alternative à l’aide de l’attestation approuvée par l’administrateur, ce qui peut être souhaitable si le matériel de TPM 2.0 n’est pas en cours d’utilisation dans votre organisation. Ce modèle d’attestation est facile à déployer, car les ordinateurs hôtes sont simplement placés dans un groupe de sécurité et le Service Guardian hôte est configuré pour autoriser les machines virtuelles protégées à exécuter sur les ordinateurs hôtes qui sont membres du groupe de sécurité. Avec cette méthode, il n’existe aucune mesure complexe pour vous assurer que l’ordinateur hôte n’a pas été falsifié. Toutefois, vous n’éliminez la possibilité de non chiffré de disques de machines virtuelles prennent la porte sur USB, ou que la machine virtuelle s’exécutera sur un hôte non autorisé. Il s’agit, car les fichiers de machine virtuelle ne s’exécute sur n’importe quel ordinateur autre que ceux du groupe désigné. Si vous n’avez pas encore de matériel de TPM 2.0, vous pouvez commencer avec attestation approuvée par l’administrateur et basculez vers l’attestation approuvée par le matériel lors de la mise à niveau de votre matériel.

#### <a name="virtual-machine-trusted-platform-module"></a>Module de plateforme sécurisée de Machine virtuelle
Windows Server 2016 prend en charge le module de plateforme sécurisée pour les machines virtuelles, qui vous permet de prendre en charge des technologies de sécurité avancées telles que le chiffrement de lecteur BitLocker® dans les machines virtuelles. Vous pouvez activer la prise en charge du module de plateforme sécurisée sur des ordinateurs virtuels de génération 2 Hyper-V à l’aide de Hyper-V Manager ou l’applet de commande Enable-VMTPM Windows PowerShell.

Vous pouvez protéger le module de plateforme sécurisée virtuelle (vTPM) en utilisant les clés de chiffrement stockées sur l’ordinateur hôte ou stockés dans le Service Guardian hôte locales. Par conséquent, tandis que le Service Guardian hôte nécessite une infrastructure plus, il fournit également une protection plus.

#### <a name="distributed-network-firewall-using-software-defined-networking"></a>Pare-feu de réseau distribué à l’aide d’accès réseau défini par le logiciel
Un pour améliorer la protection dans des environnements virtualisés consiste à segmenter le réseau d’une façon qui permet aux machines virtuelles de communiquer avec uniquement les systèmes spécifiques requises pour fonctionner. Par exemple, si votre application n’a pas besoin pour se connecter à Internet, vous pouvez partitionner, éliminant ces systèmes en tant que cibles des attaquants externes. La définition logicielle mise en réseau (SDN) dans Windows Server 2016 inclut un pare-feu de réseau distribué qui vous permet de créer dynamiquement les stratégies de sécurité capable de protéger vos applications contre les attaques provenant de l’intérieur ou hors d’un réseau. Ce pare-feu de réseau distribué ajoute des couches à votre sécurité en vous permettant d’isoler vos applications dans le réseau. Stratégies peuvent être appliquées de n’importe où dans votre infrastructure de réseau virtuel, d’isoler le trafic de machine virtuelle à, le trafic de machine virtuelle pour héberger ou le trafic de machine virtuelle à Internet lorsque cela est nécessaire, pour les systèmes individuels qui ont peut-être été compromis ou par programme sur plusieurs sous-réseaux. Fonctionnalités réseau de Windows Server 2016 défini par logiciel vous permettent également de routage ou de mettre en miroir le trafic entrant vers les appliances virtuelles non Microsoft. Par exemple, vous pouvez choisir envoyer tout le trafic de courrier électronique via une appliance virtuelle Barracuda pour la protection du filtrage de spam supplémentaire. Cela vous permet de facilement couche de sécurité supplémentaire à la fois en local ou dans le cloud.

### <a name="other-gdpr-considerations-for-servers"></a>Autres considérations RGPD pour les serveurs
Le RGPD inclut des exigences explicites pour la notification de violation où une violation des données personnelles signifie, «_une violation de sécurité, ce qui conduit à la destruction accidentelle ou illicite, perte, la modification, la divulgation non autorisée, ou accès à, personnel données transmises, stockées ou traitées dans le cas contraire._ »  Évidemment, ne peut pas commencer à déplacer vers le bas les exigences strictes RGPD notification dans les 72 heures si vous ne peut pas détecter la violation en premier lieu.

Comme indiqué dans le livre blanc Windows Security Center, [Post violation : Traiter les menaces avancées](http://wincom.blob.core.windows.net/documents/Post_Breach_Dealing_with_Advanced_Threats_Whitepaper.pdf)

> «_Contrairement au préalable violation de la sécurité post-faille suppose une violation s’est déjà produite – agissant en tant que boîte noire et Crime scène investigateur CSI (). Post-faille fournit aux équipes de sécurité les informations et ensemble d’outils nécessaire pour identifier, d’examiner et de répondre aux attaques qui sinon reste non détectés et ci-dessous l’en radar._ »

Dans cette section, nous allons examiner comment Windows Server peut vous aider à répondre à vos obligations de notification de violation de RGPD. Cela commence par déterminer les données sous-jacentes de menace disponibles à Microsoft qui est collecté et analysée pour votre bénéfice et comment procéder, via Windows Defender Advanced Threat Protection (ATP), ces données peuvent être essentielles pour vous.

#### <a name="insightful-security-diagnostic-data"></a>Données de diagnostic de sécurité révélatrices
Pour presque 20 ans, Microsoft a été activation de menaces en informations utiles qui peuvent aider à renforcer sa plateforme et de protéger les clients. Aujourd'hui, grâce aux immenses avantages informatiques offerts par le cloud, nous trouvons de nouvelles façons d’utiliser nos moteurs d'analyse enrichis, fondés sur les renseignements concernant les menaces pour protéger nos clients.

En appliquant une combinaison de processus automatisés et manuels, d'apprentissage machine et d'experts humains, nous pouvons créer un Intelligent Security Graph qui apprend de lui-même et évolue en temps réel, afin de réduire notre temps collectif nécessaire pour détecter les nouveaux incidents et y réagir pour l'ensemble de nos produits.

![Security Intelligence de Microsoft Graph](../media/GDPR-Windows-Server-Overview/gdpr-intelligent-security-graph.png)

L’étendue de menaces de Microsoft s’étend, littéralement, des milliards de points de données : 35 milliards de messages analysés tous les mois, les clients de 1 milliard sur des segments entreprise et grand public de l’accès aux services de cloud 200 + et 14 milliards d’authentifications effectuées quotidiennement. Toutes ces données sont rassemblées à votre place par Microsoft pour créer l’Intelligent Security Graph qui peuvent vous aider à protéger votre porte d’entrée de manière dynamique à rester en sécurité, rester productif et répondre aux exigences du RGPD.

#### <a name="detecting-attacks-and-forensic-investigation"></a>Détection des attaques et enquêtes scientifiques
Même les meilleures défenses de point de terminaison peuvent finalement présenter des failles, à mesure que les cyberattaques deviennent plus sophistiquées et plus ciblées. Deux fonctionnalités peuvent être utilisées pour faciliter la détection de violation potentielle - Windows Defender Advanced Threat Protection (ATP) et Microsoft Advanced Threat Analytique (ATA).

La protection avancée contre les menaces Windows Defender (ATP) vous permet de détecter les attaques avancées et les violations de données sur vos réseaux, à les examiner et à réagir en conséquence. Les types de violation de données le RGPD suppose que vous protéger contre par des mesures de sécurité technique pour garantir la confidentialité en cours, l’intégrité et disponibilité des données personnelles et des systèmes de traitement.

Voici quelques-uns des avantages de Windows Defender ATP sont les suivantes :

- **Détecter l’indétectables.** Capteurs intégrées approfondie dans le noyau du système d’exploitation, des experts en sécurité de Windows et optique unique provenant de plus de 1 milliard de machines et des signaux entre tous les services Microsoft.

- **Intégrées, et non rajoutée.** Sans agent, avec des performances élevées et un impact minimal sur le cloud ; faciliter la gestion sans aucun déploiement. 

- **Volet unique pour la sécurité de Windows.** Explorez les 6 mois de chronologie-riche, machine, unification des événements de sécurité à partir de Windows Defender ATP, Antivirus Windows Defender et Windows Defender Device Guard.

- **Puissance de Microsoft graph.** S’appuie sur la sécurité d’Intelligence Microsoft Graph pour intégrer la détection et l’exploration avec abonnement Office 365 ATP, pour effectuer le suivi de retour et répondre aux attaques.

En savoir plus sur [Nouveautés dans la version d’évaluation de Windows Defender ATP Creators Update](https://blogs.microsoft.com/microsoftsecure/2017/03/13/whats-new-in-the-windows-defender-atp-creators-update-preview/).

ATA est un produit local qui permet de détecter abusifs d’identité dans une organisation. ATA peut capturer et analyser le trafic réseau pour l’authentification, l’autorisation et protocoles (tels que Kerberos, DNS, RPC, NTLM et autres protocoles) de la collecte d’informations. ATA utilise ces données pour créer un profil de comportement sur les utilisateurs et les autres entités sur un réseau afin qu’il peut détecter les anomalies et les modèles d’attaque connus. Le tableau suivant répertorie les types d’attaques détectées par ATA.


|Type d’attaque |Description |
|---------|---------|
|Attaques malveillantes |Ces attaques sont détectés en recherchant des attaques à partir d’une liste des types d’attaques, notamment :<ul><li>Pass-the-Ticket (PtT)</li><li>Pass-the-Hash (PtH)</li><li>Overpass-the-Hash</li><li>Faux PAC (MS14-068)</li><li>Golden Ticket</li><li>Réplications malveillantes</li><li>Reconnaissance</li><li>Force brute</li><li>Exécution à distance</li></ul>Pour une liste complète des attaques malveillantes qui peuvent être détectés et leur description, consultez [que suspectes les activités peuvent détectées par ATA ?](https://docs.microsoft.com/advanced-threat-analytics/understand-explore/ata-threats).|
|Comportement anormal |Ces attaques sont détectés à l’aide d’une analyse comportementale et utilisent machine learning pour identifier les activités douteuses, y compris :<ul><li>Connexions anormales</li><li>Menaces inconnues</li><li>Partage de mot de passe</li><li>Mouvement latéral</li></ul>|
|Les risques et problèmes de sécurité |Ces attaques sont détectés en examinant réseau actuel et la configuration du système, y compris :<ul><li>Relation de confiance rompue</li><li>Protocoles faibles</li><li>Vulnérabilités de protocole connues</li></ul>|

Vous pouvez utiliser ATA pour aider à détecter des personnes malveillantes tentant de compromettre les identités privilégiées. Pour plus d’informations sur le déploiement d’ATA, consultez les rubriques de Plan, la conception et déploiement de la [documentation d’Analytique de menaces avancées](https://docs.microsoft.com/advanced-threat-analytics/).

## <a name="related-content-for-associated-windows-server-2016-solutions"></a>Contenu associé pour les solutions Windows Server 2016 associées

- **Antivirus Windows Defender :** https://www.youtube.com/watch?v=P1aNEy09NaI et https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10

- **Windows Defender Advanced Threat Protection :** https://www.youtube.com/watch?v=qxeGa3pxIwg et https://docs.microsoft.com/windows/threat-protection/windows-defender-atp/configure-server-endpoints-windows-defender-advanced-threat-protection

- **Windows Defender Device Guard :** https://www.youtube.com/watch?v=F-pTkesjkhI et https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide

- **Windows Defender Credential Guard :** https://www.youtube.com/watch?v=F-pTkesjkhI et https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard

- **Protection du flux de contrôle :** https://msdn.microsoft.com/library/windows/desktop/mt637065(v=vs.85).aspx

- **Sécurité et Assurance :** https://docs.microsoft.com/windows-server/security/security-and-assurance

## <a name="disclaimer"></a>Clause d’exclusion de responsabilité
Cet article est un commentaire sur le RGPD, tel que Microsoft l’interprète, à compter de la date de publication. Nous avons investi beaucoup de temps à étudier le RGPD et nous pensons avoir bien compris son objectif et sa signification. Mais l’application du RGPD est propre à chaque cas et les aspects et les interprétations du RGPD ne sont pas tous bien fixés.

Par conséquent, cet article est fourni à titre d’information uniquement. Il ne doit pas être considéré comme constituant un avis juridique ni servir à déterminer la façon dont le RGPD peut s’appliquer à vous et à votre organisation. Nous vous encourageons à travailler avec un professionnel juridiquement qualifié pour discuter du RGPD, de la manière dont il s’applique spécifiquement à votre organisation et des meilleures méthodes pour garantir la conformité.

MICROSOFT N’APPORTE AUCUNE GARANTIE, EXPRESSE, IMPLICITE OU LÉGALE, QUANT AU CONTENU DE CET ARTICLE. Cet article est fourni «en l'état». Les informations et idées exprimées dans ce article, y compris les URL et autres références à des sites web sur Internet, peuvent faire l'objet de modifications sans préavis.

Cet article ne vous fournit aucun droit légal de propriété intellectuelle de tout produit Microsoft.  Vous pouvez copier le présent article pour une utilisation interne à des fins de référence uniquement.  

Publié en septembre 2017<br>
Version 1.0<br>
© 2017 Microsoft. Tous droits réservés.


