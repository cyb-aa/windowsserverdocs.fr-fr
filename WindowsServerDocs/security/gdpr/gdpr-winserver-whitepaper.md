---
title: Commencer à appliquer le Règlement général sur la protection des données (RGPD) pour Windows Server 2016
description: Cet article vous explique en quoi consiste le RGPD et présente ce qu'offrent les produits Microsoft pour vous préparer à la mise en conformité.
ms.technology: techgroup-security
ms.topic: article
ms.date: 09/25/2017
ms.author: nirb
author: nirb-ms
ms.openlocfilehash: 51768dc65128f27dcbf78cbfc776500ac3832615
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949834"
---
# <a name="beginning-your-general-data-protection-regulation-gdpr-journey-for-windows-server"></a>Début de votre parcours Règlement général sur la protection des données (RGPD) pour Windows Server 

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cet article vous explique en quoi consiste le RGPD et présente ce qu'offrent les produits Microsoft pour vous préparer à la mise en conformité.

## <a name="introduction"></a>Introduction
Le 25 mai 2018, une loi de confidentialité européenne doit entrer en vigueur. Elle fixe une nouvelle norme mondiale en matière de droits à la vie privée, de sécurité et de conformité.

Le règlement général sur la protection des données (ou RGPD) concerne essentiellement la protection et l’application des droits à la vie privée des utilisateurs. Le RGPD établit des conditions strictes internationales en matière de protection de la vie privée qui régissent votre façon de gérer et de protéger les données personnelles tout en respectant les choix individuels, quel que soit l’endroit où les données sont envoyées, traitées ou stockées.

Microsoft et nos clients sont désormais en voie d'atteindre les objectifs de confidentialité du RGPD. Chez Microsoft, nous pensons que la confidentialité est un droit fondamental, et nous pensons que le RGPD est une étape importante pour clarifier et appliquer les droits individuels en matière de protection de la vie privée. Mais nous savons également que le RGPD nécessite des modifications importantes apportées par les organisations du monde entier.

Nous avons décrit notre engagement en faveur du RGPD, ainsi que notre soutien à nos clients dans le billet de blog [Devenir conforme au RGPD à l'aide du Microsoft Cloud](https://blogs.microsoft.com/on-the-issues/2017/02/15/get-gdpr-compliant-with-the-microsoft-cloud/#hv52B68OZTwhUj2c.99) de notre Responsable de la confidentialité [Brendon Lynch](https://blogs.microsoft.com/on-the-issues/author/brendonlynch/) et le billet de blog [Gagner votre confiance avec les engagements contractuels envers le règlement général sur la protection des données](https://blogs.microsoft.com/on-the-issues/2017/04/17/earning-trust-contractual-commitments-general-data-protection-regulation/#6QbqoGWXCLavGM63.99) de [Rich Sauer](https://blogs.microsoft.com/on-the-issues/author/rsauer/), vice-président du groupe Microsoft et directeur juridique adjoint.

Bien que votre mise en conformité avec le RGPD puisse sembler difficile, nous sommes là pour vous aider. Pour obtenir des informations spécifiques sur le RGPD, nos engagements et comment commencer à le mettre en application, visitez la [section RGPD du Microsoft Trust Center](https://www.microsoft.com/trustcenter/privacy/gdpr).

## <a name="gdpr-and-its-implications"></a>Le RGPD et ses implications
Le RGPD est un règlement complexe qui peut nécessiter des modifications importantes de la façon dont vous recueillez, utilisez et gérez les données personnelles. Depuis longtemps, Microsoft aide ses clients à respecter les réglementations complexes et en ce qui concerne la préparation au RGPD, nous sommes votre partenaire dans cette démarche.

Le RGPD impose des règles aux organisations qui fournissent des biens et des services aux habitants de l’Union européenne (UE) ou qui collectent et analysent des données liées à des résidents de l’Union européenne, quel que soit l’emplacement où se trouvent les entreprises. Les éléments clés du RGPD sont notamment les suivants :

- **Droits de confidentialité personnels améliorés.** Protection des données renforcée pour les résidents de l’Union européenne en garantissant qu’ils ont le droit d’accéder à leurs données personnelles, d'y corriger des inexactitudes, de les effacer, de contester le traitement de leurs données personnelles et de les déplacer.

- **Des droits accrus pour la protection des données personnelles.** Responsabilité renforcée des organisations qui traitent des données personnelles, en apportant plus de clarté sur la responsabilité d'assurer la conformité.

- **Signalement obligatoire des violations de données personnelles.** Les organisations qui contrôlent des données à caractère personnel ont pour obligation de signaler à leurs autorités de contrôle toute violation de ces données qui présente un risque pour les droits et les libertés des individus, sans tarder et, si possible, au plus tard 72 heures après avoir pris connaissance de la violation.

Comme on peut s'y attendre, le RGPD peut avoir un impact significatif sur votre entreprise. Il peut éventuellement vous obliger à mettre à jour les politiques de confidentialité, à implémenter et renforcer les contrôles de protection de données et les procédures de notification de violation, à déployer des stratégies très transparentes, et à effectuer des investissements supplémentaires en informatique et en formation. Microsoft Windows 10 peut vous aider à répondre à certaines de ces exigences de manière efficace.

## <a name="personal-and-sensitive-data"></a>Données personnelles et confidentielles
Dans le cadre de vos efforts pour respecter le RGPD, vous devez comprendre comment le règlement définit les données personnelles et confidentielles et comment ces définitions se rapportent aux données détenues par votre organisation. Sur la base de cette compréhension, vous pourrez découvrir où ces données sont créées, traitées, gérées et stockées.

Le RGPD considère comme données personnelles toutes les informations relatives à une personne physique identifiée ou identifiable. Cela peut inclure une identification directe (par exemple, votre nom légal) et une identification indirecte (comme des informations spécifiques qui font clairement référence à vous). Le RGPD montre clairement que le concept de données personnelles inclut les identificateurs en ligne (par exemple, les adresses IP, les ID d'appareil mobile) et les données de localisation.

Le RGPD introduit des définitions spécifiques pour les données génétiques (telles que, la séquence génique d’un individu) et les données biométriques. Données génétiques et données biométriques, ainsi que d’autres sous-catégories de données personnelles (données personnelles révélant l’origine raciale ou ethnique, les opinions politiques, les convictions religieuses ou philosophiques, l’appartenance aux syndicats, les données relatives à l’intégrité ou les données relatives à un la vie sexuelle ou l’orientation sexuelle de la personne est traitée comme des données personnelles sensibles sous le RGPD. Les données personnelles sensibles sont des protections améliorées et requièrent généralement un consentement explicite de la part de l’individu dans lequel ces données doivent être traitées.

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

-   **Trouvez.** Identifiez les données personnelles que vous détenez et leur emplacement. 

-   **Vos.** Régissez la façon dont les données personnelles sont utilisées et accessibles.

-   **Protéger.** Établissez des contrôles de sécurité pour empêcher les vulnérabilités et les violations de données, les détecter et y réagir.  

-   **Port.** Consignez les demandes de données, signalez les violations de données et conservez les documents requis.

    ![Diagramme sur l'articulation des 4 principales étapes du RGPD](../media/GDPR-Windows-Server-Overview/gdpr-steps-diagram.png)

Pour chacune de ces étapes, nous décrivons des exemples d'outils, de ressources et de fonctionnalités de différentes solutions Microsoft qui permettent de répondre aux exigences propres à chaque étape. Bien que cet article ne soit pas un guide de procédure complet, nous avons inclus des liens pour vous aider à en savoir plus, et d’autres informations sont disponibles dans la [section RGPD du centre de gestion de la confidentialité de Microsoft](https://www.microsoft.com/trustcenter/privacy/gdpr).

## <a name="windows-server-security-and-privacy"></a>Sécurité et confidentialité Windows Server
Le RGPD vous oblige à implémenter des mesures de sécurité techniques et organisationnelles appropriées pour protéger les systèmes de données et de traitement personnels. Dans le contexte de RGPD, vos environnements de serveurs physiques et virtuels sont susceptibles de traiter des données personnelles et sensibles. Le traitement peut signifier toute opération ou ensemble d’opérations, telles que la collecte de données, le stockage et la récupération.

Votre capacité à répondre à cette exigence et à mettre en œuvre les mesures de sécurité techniques appropriées doit refléter les menaces que vous rencontrez dans l’environnement informatique de plus en plus hostile actuel. Aujourd’hui, le paysage des menaces de sécurité est l’une des menaces Tenacious et agressives. Auparavant, la préoccupation majeure des pirates informatiques consistait à gagner la reconnaissance de leur communauté ; ils prenaient ainsi plaisir à lancer des attaques ou à neutraliser temporairement un système entier. Depuis, les motivations de l’attaquant ont été déplacées vers l’argent, y compris le maintien des appareils et des Hostage de données jusqu’à ce que le propriétaire paye les ransomware demandés.

Les attaques modernes consistent de plus en plus en des vols à grande échelle de propriété intellectuelle et en des dégradations d'un système cible, pouvant entraîner une perte financière. Et désormais, le cyberterrorisme menace même la sécurité des personnes, des entreprises ainsi que les intérêts des États du monde entier. Ces pirates sont généralement des personnes très compétentes et expertes en matière de sécurité. Certains d'entre eux travaillent même pour le compte des États, lesquels disposent de budgets conséquents et de ressources humaines quasiment illimitées. De telles menaces nécessitent d'adopter une approche adaptée pour relever ce type de défis.

Non seulement ces menaces mettent en péril votre capacité à garder le contrôle sur toutes les données personnelles ou confidentielles que vous pouvez détenir, mais elles représentent également un risque pour votre activité globale. Considérez les données récentes de McKinsey, Ponemon Institute, Verizon et Microsoft :

- Le coût moyen du type de violation de données que le RGPD vous demande de signaler est de 3,5 M d'USD.

- 63 % de ces violations impliquent des mots de passe faibles ou volés que le RGPD vous demande de traiter.

- Plus de 300 000 nouveaux exemples de programmes malveillants sont créés et propagés tous les jours, ce qui complique encore plus la tâche pour assurer la protection des données.

Comme nous l’avons vu avec les récentes attaques par ransomware, une fois que vous avez appelé la peste noire d’Internet, les attaquants reçoivent des cibles plus grandes qui peuvent se permettre de payer davantage, avec des conséquences catastrophiques. Le RGPD inclut des pénalités qui mettent en effet vos systèmes, y compris les ordinateurs de bureau et les ordinateurs portables, qui contiennent des cibles riches de données personnelles et sensibles.

Deux principes clés ont guidé et continuent à guider le développement de Windows :

- **Sécurité** : Les données de notre magasin de logiciels et de services pour le compte de nos clients doivent être protégées contre les dommages et utilisés ou modifiés uniquement de manière appropriée. Les modèles de sécurité doivent être faciles à comprendre et à intégrer dans leurs applications.

- **Nominative.** Les utilisateurs doivent contrôler la façon dont leurs données sont utilisées. Les stratégies d’utilisation des informations doivent être claires pour l’utilisateur. Les utilisateurs doivent contrôler le moment et le moment où ils reçoivent des informations pour tirer le meilleur parti de leur temps. Il devrait être facile pour les utilisateurs de spécifier l’utilisation appropriée de leurs informations, notamment le contrôle de l’utilisation de l’e-mail qu’ils envoient.

Microsoft est resté inébranlable par rapport à ces principes comme indiqué récemment par le PDG de Microsoft, Satya Nadella, 

> « À_mesure que le monde continue à changer et que les besoins de l’entreprise évoluent, certains éléments sont cohérents : la demande d’un client en matière de sécurité et de confidentialité »._

À mesure que vous travaillez pour vous conformer à la RGPD, comprendre le rôle de vos serveurs physiques et virtuels dans la création, l’accès, le traitement, le stockage et la gestion des données susceptibles d’être considérées comme des données personnelles et potentiellement sensibles dans le RGPD est important. Windows Server fournit des fonctionnalités qui vous aideront à respecter les exigences de RGPD pour mettre en œuvre des mesures de sécurité techniques et organisationnelles appropriées pour protéger les données personnelles.

La position de sécurité de Windows Server 2016 n’est pas un boulon ; Il s’agit d’un principe architectural. Et, il peut être mieux compris dans quatre principaux :

- **Protéger.** Concentration et innovation permanentes sur les mesures préventives ; bloquez les attaques connues et les programmes malveillants connus.

- **Identifie.** Des outils de surveillance complets pour vous aider à repérer les anomalies et à réagir plus rapidement aux attaques.

- **Répondre.** Technologies de réponse et de récupération de pointe, ainsi que compétence de Consulting approfondie.

- **Isoler.** Isolez les composants du système d’exploitation et les secrets des données, limitez les privilèges d’administrateur et mesurez rigoureusement l’intégrité de l’hôte.

Avec Windows Server, votre capacité de protection, de détection et de défense contre les types d’attaques susceptibles d’aboutir à des violations de données est beaucoup améliorée. Étant donné les exigences strictes concernant la notification des violations dans le RGPD, en garantissant une bonne protection de vos systèmes de bureau et portables, vous limitez la probabilité d'être confronté à des risques pouvant entraîner une analyse et une notification coûteuses de la violation.

Dans la section qui suit, vous verrez comment Windows Server fournit des fonctionnalités qui s’ajustent à la phase « protéger » de votre parcours de conformité RGPD. Ces fonctionnalités se répartissent en trois scénarios de protection :

- **Protégez vos informations d’identification et limitez les privilèges d’administrateur.** Windows Server 2016 permet d’implémenter ces modifications, afin d’éviter que votre système ne soit utilisé comme point de lancement pour d’autres intrusions.

- **Sécurisez le système d’exploitation pour exécuter vos applications et votre infrastructure.** Windows Server 2016 fournit des couches de protection qui permettent de bloquer l’exécution de logiciels malveillants par des attaquants externes ou l’exploitation de vulnérabilités.

- **Virtualisation sécurisée.** Windows Server 2016 permet une virtualisation sécurisée, utilisant des machines virtuelles protégées et une structure protégée. Cela vous permet de chiffrer et d’exécuter vos machines virtuelles sur des hôtes approuvés dans votre infrastructure, et de mieux les protéger contre les attaques malveillantes.

Ces fonctionnalités, présentées plus en détail ci-dessous, avec des références à des exigences spécifiques de RGPD, s’appuient sur une protection avancée des appareils qui permet de maintenir l’intégrité et la sécurité du système d’exploitation et des données.

Une configuration de clé au sein du RGPD est la protection des données par conception et par défaut. en vous aidant de votre capacité à répondre à cette configuration, vous disposez de fonctionnalités dans Windows 10, telles que le chiffrement de l’appareil BitLocker. BitLocker utilise la technologie de Module de plateforme sécurisée (TPM) (TPM), qui fournit des fonctions matérielles liées à la sécurité. Cette puce de processeur de chiffrement comprend plusieurs mécanismes de sécurité physique pour le rendre inviolable, et les logiciels malveillants ne peuvent pas altérer les fonctions de sécurité du module de plateforme sécurisée.

La puce comprend plusieurs mécanismes de sécurité physique qui la protègent contre la falsification, et les logiciels malveillants ne peuvent pas falsifier les fonctions de sécurité du TPM. Les principaux avantages de l’utilisation de la technologie TPM sont principalement que vous pouvez :

-   générer, stocker et limiter l’utilisation des clés de chiffrement ;

-   Utilisez la technologie TPM pour l’authentification des appareils de plateforme à l’aide de la clé RSA unique du module de plateforme sécurisée, qui est gravée dans elle-même.

-   garantir l’intégrité de la plateforme en réalisant et en stockant des mesures sur la sécurité.

Pour assurer une protection avancée des appareils supplémentaire, adaptée à votre exploitation et sans violations de données, vous pouvez notamment utiliser le démarrage sécurisé de Windows pour préserver l’intégrité du système en garantissant qu'un programme malveillant ne puisse pas démarrer avant les défenses du système.

## <a name="windows-server-supporting-your-gdpr-compliance-journey"></a>Windows Server : prise en charge de votre parcours de conformité RGPD
Les fonctionnalités clés de Windows Server peuvent vous aider à mettre en œuvre efficacement et efficacement les mécanismes de sécurité et de confidentialité requis par RGPD pour la conformité. Bien que l’utilisation de ces fonctionnalités ne garantisse pas la conformité, elles prendront en charge vos efforts.

Le système d’exploitation serveur repose sur une couche stratégique de l’infrastructure d’une organisation, offrant ainsi de nouvelles opportunités de créer des couches de protection contre les attaques susceptibles de voler des données et d’interrompre votre activité. Les aspects clés des RGPD tels que la confidentialité par conception, la protection des données et les Access Control doivent être traités au sein de votre infrastructure informatique au niveau du serveur.

Pour vous aider à protéger l’identité, le système d’exploitation et les couches de virtualisation, Windows Server 2016 permet de bloquer les vecteurs d’attaque courants utilisés pour obtenir un accès illégal à vos systèmes : des informations d’identification volées, des programmes malveillants et une structure de virtualisation compromise. En plus de réduire les risques pour l’entreprise, les composants de sécurité intégrés à Windows Server 2016 aident à répondre aux exigences de conformité pour les réglementations stratégiques en matière de sécurité et de secteur public. 

Ces protections des identités, des systèmes d’exploitation et des virtualisation vous permettent de mieux protéger votre centre de données exécutant Windows Server en tant que machine virtuelle dans n’importe quel Cloud, et de limiter la capacité des attaquants à compromettre les informations d’identification, à lancer des programmes malveillants et à rester non détectés dans votre réseaux. De même, lorsqu’il est déployé en tant qu’ordinateur hôte Hyper-V, Windows Server 2016 offre une garantie de sécurité pour vos environnements de virtualisation via des machines virtuelles protégées et des fonctionnalités de pare-feu distribuées. Avec Windows Server 2016, le système d’exploitation serveur devient un participant actif dans la sécurité de votre centre de sécurité.

### <a name="protect-your-credentials-and-limit-administrator-privileges"></a>Protégez vos informations d’identification et limitez les privilèges d’administrateur 
Le contrôle de l’accès aux données personnelles et des systèmes qui traitent ces données est une zone avec le RGPD qui a des exigences spécifiques, y compris l’accès des administrateurs. Les identités privilégiées sont des comptes qui ont des privilèges élevés, tels que les comptes d’utilisateurs qui sont membres des administrateurs de domaine, administrateurs d’entreprise, administrateurs locaux ou même utilisateurs avec pouvoir. Ces identités peuvent également inclure des comptes auxquels des privilèges ont été accordés directement, tels que l’exécution de sauvegardes, l’arrêt du système ou d’autres droits indiqués dans le nœud attribution des droits utilisateur de la console Stratégie de sécurité locale.

En tant que principe de contrôle d’accès général et en ligne avec RGPD, vous devez protéger ces identités privilégiées contre toute compromission par des attaquants potentiels. Tout d’abord, il est important de comprendre comment les identités sont compromises ; vous pouvez ensuite prévoir d’empêcher les attaquants d’accéder à ces identités privilégiées.

#### <a name="how-do-privileged-identities-get-compromised"></a>Comment les identités privilégiées sont-elles compromises ?
Les identités privilégiées peuvent être compromises lorsque les organisations n’ont pas de directives pour les protéger. Voici quelques exemples :

- **Plus de privilèges que nécessaire.** L’un des problèmes les plus courants est que les utilisateurs ont plus de privilèges que nécessaire pour exécuter leur fonction. Par exemple, un utilisateur qui gère DNS peut être un administrateur Active Directory. La plupart du temps, cette opération est effectuée pour éviter d’avoir à configurer différents niveaux d’administration. Toutefois, si un tel compte est compromis, l’attaquant a automatiquement des privilèges élevés.

- **Connexion en permanence avec des privilèges élevés.** Un autre problème courant est que les utilisateurs avec des privilèges élevés peuvent l’utiliser pour une durée illimitée. Cela est très fréquent avec les professionnels de l’informatique qui se connectent à un ordinateur de bureau à l’aide d’un compte privilégié, restent connectés et utilisent le compte privilégié pour naviguer sur le Web et utiliser la messagerie électronique (fonctions de travail informatique courantes). Une durée illimitée des comptes privilégiés rend le compte plus vulnérable aux attaques et augmente les risques de compromission du compte.

- **Recherche d’ingénierie sociale.** La plupart des menaces relatives aux informations d’identification commencent par la recherche dans l’organisation, puis par l’ingénierie sociale. Par exemple, une personne malveillante peut effectuer une attaque par hameçonnage pour compromettre les comptes légitimes (mais pas nécessairement les comptes élevés) qui ont accès au réseau d’une organisation. L’attaquant utilise ensuite ces comptes valides pour effectuer des recherches supplémentaires sur votre réseau et pour identifier les comptes privilégiés qui peuvent effectuer des tâches d’administration. 

- **Tirez parti des comptes avec des privilèges élevés.** Même avec un compte d’utilisateur normal et non élevé dans le réseau, des attaquants peuvent accéder aux comptes avec des autorisations élevées. L’une des méthodes les plus courantes consiste à utiliser les attaques Pass-The-hash ou Pass-the-token. Pour plus d’informations sur les techniques Pass-The-hash et autres vols d’informations d’identification, consultez les ressources sur la [page Pass-The-hash (PTH)](https://technet.microsoft.com/dn785092.aspx).

Il existe bien sûr d’autres méthodes que les attaquants peuvent utiliser pour identifier et compromettre les identités privilégiées (avec de nouvelles méthodes créées quotidiennement). Il est donc important d’établir des pratiques permettant aux utilisateurs de se connecter avec des comptes dotés de privilèges minimum pour réduire la capacité des attaquants à accéder aux identités privilégiées. Les sections ci-dessous décrivent les fonctionnalités dans lesquelles Windows Server peut atténuer ces risques.

#### <a name="just-in-time-admin-jit-and-just-enough-admin-jea"></a>Administrateur juste-à-temps (JIT) et juste assez administrateur (JEA)
Bien qu’il soit important de se protéger contre les attaques Pass-The-hash ou Pass-the-ticket, les informations d’identification d’administrateur peuvent toujours être volées par d’autres moyens, y compris l’ingénierie sociale, les employés mécontents et la force brute. Par conséquent, en plus d’isoler autant que possible les informations d’identification, vous avez également besoin d’un moyen de limiter la portée des privilèges de niveau administrateur au cas où ils seraient compromis.

Aujourd’hui, un trop grand nombre de comptes d’administrateur est trop privilégié, même s’ils ne disposent que d’une seule zone de responsabilité. Par exemple, un administrateur DNS, qui a besoin d’un ensemble de privilèges très étroit pour gérer les serveurs DNS, se voit souvent accorder des privilèges au niveau de l’administrateur du domaine. En outre, étant donné que ces informations d’identification sont accordées pour la perpétuelleté, il n’existe aucune limite quant à la durée pendant laquelle elles peuvent être utilisées.

Chaque compte disposant de privilèges d’administrateur de domaine inutiles augmente votre exposition aux attaquants qui cherchent à compromettre les informations d’identification. Pour réduire la surface d’exposition de l’attaque, vous ne devez fournir que le jeu de droits spécifique dont un administrateur a besoin pour effectuer le travail, et uniquement pour la fenêtre de temps nécessaire à sa réalisation.

À l’aide de l’administration juste-à-temps et de l’administration juste-à-temps, les administrateurs peuvent demander les privilèges spécifiques dont ils ont besoin pour la fenêtre de temps exacte requise. Pour un administrateur DNS, par exemple, l’utilisation de PowerShell pour activer juste l’administration vous permet de créer un ensemble limité de commandes qui sont disponibles pour la gestion DNS.

Si l’administrateur DNS doit effectuer une mise à jour sur l’un de ses serveurs, elle demande l’accès pour gérer DNS à l’aide de Microsoft Identity Manager 2016. Le flux de travail de la demande peut inclure un processus d’approbation, tel que l’authentification à deux facteurs, qui peut appeler le téléphone mobile de l’administrateur pour confirmer son identité avant d’accorder les privilèges demandés. Une fois ces autorisations accordées, ces privilèges DNS permettent d’accéder au rôle PowerShell pour DNS pour un laps de temps spécifique.

Imaginez ce scénario si les informations d’identification de l’administrateur DNS ont été volées. Tout d’abord, étant donné que les informations d’identification n’ont pas de privilèges d’administrateur, la personne malveillante ne peut pas accéder au serveur DNS (ou à tout autre système) pour apporter des modifications. Si l’attaquant tente de demander des privilèges pour le serveur DNS, l’authentification de second facteur leur demande de confirmer leur identité. Dans la mesure où il est peu probable que l’attaquant ait le téléphone mobile de l’administrateur DNS, l’authentification échoue. Cela verrouillerait l’attaquant en dehors du système et alertera l’organisation informatique que les informations d’identification pourraient être compromises.

En outre, de nombreuses organisations utilisent la [solution de mot de passe d’administrateur local gratuite (coupe)](https://aka.ms/laps) comme un mécanisme d’administration JIT simple mais puissant pour leurs systèmes serveur et client. La fonctionnalité couvre la gestion des mots de passe de compte local des ordinateurs joints à un domaine. Les mots de passe sont stockés dans Active Directory (AD) et protégés par et Access Control liste (ACL) afin que seuls les utilisateurs éligibles puissent le lire ou demander la réinitialisation.

Comme indiqué dans le [Guide de prévention des vols d’informations d’identification Windows](https://www.microsoft.com/download/confirmation.aspx?id=54095), 

> «_les outils et techniques utilisés par les criminels pour effectuer des attaques de vol et de réutilisation des informations d’identification améliorent, les attaquants malveillants se trouvent plus faciles à atteindre. Les vols d’informations d’identification s’appuient souvent sur des pratiques opérationnelles ou sur l’exposition des informations d’identification de l’utilisateur. les atténuations efficaces nécessitent donc une approche holistique qui résout les personnes, les processus et la technologie. En outre, ces attaques s’appuient sur le pirate qui vole les informations d’identification après avoir compromis un système pour développer ou conserver l’accès, de sorte que les organisations doivent faire face aux violations rapidement en implémentant des stratégies qui empêchent les attaquants de se déplacer librement et non détectées sur un réseau compromis._ »

L’un des points importants à prendre en compte pour Windows Server était l’atténuation des vols d’informations d’identification, en particulier les informations d’identification dérivées. Credential Guard offre une sécurité considérablement améliorée contre le vol et la réutilisation des informations d’identification dérivées en implémentant une modification architecturale importante de Windows conçue pour aider à éliminer les attaques de l’isolation matérielle plutôt que simplement essayer de vous y défendre.

Lorsque vous utilisez les informations d’identification dérivées de Windows Defender Credential Guard, NTLM et Kerberos sont protégées à l’aide de la sécurité basée sur la virtualisation, les techniques et les outils d’attaque contre le vol d’informations d’identification utilisés dans de nombreuses attaques ciblées sont bloqués. Les programmes malveillants exécutés dans le système d’exploitation avec des privilèges administratifs ne peuvent pas extraire les secrets protégés par la sécurité basée sur la virtualisation. Bien que Windows Defender Credential Guard soit une solution puissante, les attaques de menaces persistantes sont susceptibles d’être déplacées vers de nouvelles techniques d’attaque et vous devez également intégrer Device Guard, comme décrit ci-dessous, ainsi que d’autres stratégies et architectures de sécurité.

#### <a name="windows-defender-credential-guard"></a>Windows Defender Credential Guard
Windows Defender Credential Guard utilise la sécurité basée sur la virtualisation pour isoler les informations d’identification, empêchant ainsi les hachages de mot de passe ou les tickets Kerberos d’être interceptés. Il utilise un processus de l’autorité de sécurité locale (LSA, Local Security Authority) entièrement nouveau, qui n’est pas accessible au reste du système d’exploitation. Tous les fichiers binaires utilisés par l’autorité de certification locale isolée sont signés avec des certificats validés avant d’être lancés dans l’environnement protégé, ce qui rend les attaques de type Pass-The-hash totalement inefficaces.

Windows Defender Credential Guard utilise :

- Sécurité basée sur la virtualisation (obligatoire). Également requis :

    - Processeur 64 bits

    - Extensions de virtualisation du processeur, plus tables de pages étendues

    - Hyperviseur Windows

- Démarrage sécurisé (obligatoire)

- TPM 2.0 discret ou microprogramme (recommandé : fournit une liaison au matériel)

Vous pouvez utiliser Windows Defender Credential Guard pour protéger les identités privilégiées en protégeant les informations d’identification et les dérivées des informations d’identification sur Windows Server 2016. Pour plus d’informations sur la configuration requise pour Windows Defender Credential Guard, consultez [protéger les informations d’identification de domaine dérivé avec Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard).

#### <a name="windows-defender-remote-credential-guard"></a>Protection des informations d’identification à distance Windows Defender
Windows Defender Remote Credential Guard sur Windows Server 2016 et la mise à jour anniversaire Windows 10 permettent également de protéger les informations d’identification pour les utilisateurs disposant de connexions Bureau à distance. Auparavant, toute personne utilisant Services Bureau à distance devrait se connecter à son ordinateur local et être tenue à nouveau de se reconnecter lorsqu’elle a effectué une connexion à distance à leur ordinateur cible. Cette deuxième connexion transmet les informations d’identification à l’ordinateur cible, en les exposant aux attaques Pass-The-hash ou Pass-the-ticket.

Avec Windows Defender Remote Credential Guard, Windows Server 2016 implémente l’authentification unique pour les sessions de Bureau à distance, ce qui élimine la nécessité de saisir à nouveau votre nom d’utilisateur et votre mot de passe. Au lieu de cela, il utilise les informations d’identification que vous avez déjà utilisées pour ouvrir une session sur votre ordinateur local. Pour utiliser Windows Defender Remote Credential Guard, le client et le serveur Bureau à distance doivent remplir les conditions suivantes :

- Doit être joint à un domaine Active Directory et se trouver dans le même domaine ou dans un domaine avec une relation d’approbation.

- Doit utiliser l’authentification Kerberos.

- Doit exécuter au moins Windows 10 version 1607 ou Windows Server 2016.  

- L’application Windows classique Bureau à distance est requise. L’application Bureau à distance plateforme Windows universelle ne prend pas en charge Windows Defender Remote Credential Guard.

Vous pouvez activer Windows Defender Remote Credential Guard en utilisant un paramètre de Registre sur le serveur Bureau à distance et stratégie de groupe ou un paramètre Connexion Bureau à distance sur le client Bureau à distance. Pour plus d’informations sur l’activation de Windows Defender Remote Credential Guard, consultez [protéger les informations d’identification Bureau à distance avec Windows Defender Remote Credential Guard](https://docs.microsoft.com/windows/access-protection/remote-credential-guard). Comme avec Windows Defender Credential Guard, vous pouvez utiliser Windows Defender Remote Credential Guard pour protéger les identités privilégiées sur Windows Server 2016.

### <a name="secure-the-operating-system-to-run-your-apps-and-infrastructure"></a>Sécuriser le système d’exploitation pour exécuter vos applications et votre infrastructure
En empêchant les cybercriminels, vous devez également rechercher et bloquer les logiciels malveillants et les attaques qui cherchent à prendre le contrôle en subvertsant les pratiques de fonctionnement standard de votre infrastructure. Si des attaquants peuvent obtenir un système d’exploitation ou une application à exécuter de manière non prédéterminée et non viable, ils utilisent probablement ce système pour effectuer des actions malveillantes. Windows Server 2016 fournit des couches de protection qui bloquent les attaquants externes qui exécutent des logiciels malveillants ou exploitent les vulnérabilités. Le système d’exploitation prend un rôle actif dans la protection de l’infrastructure et des applications en avertissant les administrateurs de l’activité qui indique qu’un système a été violé.

#### <a name="windows-defender-device-guard"></a>Windows Defender Device Guard
Windows Server 2016 comprend Windows Defender Device Guard pour s’assurer que seuls les logiciels approuvés peuvent être exécutés sur le serveur. À l’aide de la sécurité basée sur la virtualisation, il peut limiter les fichiers binaires qui peuvent être exécutés sur le système en fonction de la stratégie de l’organisation. Si, à l’exception des fichiers binaires spécifiés, il tente de s’exécuter, Windows Server 2016 le bloque et enregistre la tentative qui a échoué afin que les administrateurs puissent voir qu’une violation potentielle s’est produite. La notification de violation est un élément essentiel de la configuration requise pour la conformité RGPD.

Windows Defender Device Guard est également intégré à PowerShell afin que vous puissiez autoriser les scripts qui peuvent s’exécuter sur votre système. Dans les versions antérieures de Windows Server, les administrateurs pouvaient contourner l’application de l’intégrité du code en supprimant simplement la stratégie du fichier de code. Avec Windows Server 2016, vous pouvez configurer une stratégie signée par votre organisation afin que seule une personne ayant accès au certificat qui a signé la stratégie puisse modifier la stratégie.

#### <a name="control-flow-guard"></a>Control Flow Guard 
Windows Server 2016 comprend également une protection intégrée contre certaines classes d’attaques de corruption de mémoire. L’application de correctifs à vos serveurs est importante, mais il est toujours possible de développer des programmes malveillants pour une vulnérabilité qui n’a pas encore été identifiée. Certaines des méthodes les plus courantes pour exploiter ces vulnérabilités sont de fournir des données inhabituelles ou extrêmes à un programme en cours d’exécution. Par exemple, une personne malveillante peut exploiter une vulnérabilité de dépassement de mémoire tampon en fournissant plus d’entrées à un programme que prévu et en saturant la zone réservée par le programme pour contenir une réponse. Cela peut corrompre la mémoire adjacente qui peut contenir un pointeur de fonction.

Lorsque le programme appelle par le biais de cette fonction, il peut accéder à un emplacement non souhaité spécifié par l’attaquant. Ces attaques sont également connues sous le nom d’attaques JOP (Jump-Oriented Programming). La protection du contrôle de workflow empêche les attaques JOP en plaçant des restrictions strictes sur le code d’application qui peut être exécuté, en particulier les instructions d’appel indirect. Elle ajoute des vérifications de sécurité légères pour identifier l’ensemble des fonctions dans l’application qui sont des cibles valides pour les appels indirects. Lorsqu’une application s’exécute, elle vérifie que ces cibles d’appel indirect sont valides.

Si la vérification de la protection du workflow de contrôle échoue au moment de l’exécution, Windows Server 2016 met immédiatement fin au programme, ce qui rompt toute attaque qui tente d’appeler indirectement une adresse non valide. La protection de workflow de contrôle fournit une couche de protection supplémentaire importante à Device Guard. Si une application de liste verte a été compromise, elle peut être exécutée non vérifiée par Device Guard, car le filtrage Device Guard peut voir que l’application a été signée et considérée comme approuvée.

Toutefois, étant donné que la protection de Flow Control peut déterminer si l’application s’exécute dans un ordre non prédéterminé et non viable, l’attaque échoue, empêchant l’exécution de l’application compromise. Ensemble, ces protections rendent très difficile pour les attaquants d’injecter des programmes malveillants dans des logiciels s’exécutant sur Windows Server 2016.

Les développeurs qui créent des applications pour lesquelles des données personnelles seront gérées sont encouragés à activer la protection du workflow de contrôle (CFG) dans leurs applications. Cette fonctionnalité est disponible dans Microsoft Visual Studio 2015 et s’exécute sur les versions « compatibles CFG » de Windows, les versions x86 et x64 pour les ordinateurs de bureau et les serveurs de mise à jour de Windows 10 et Windows 8.1 (KB3000850). Vous n’avez pas besoin d’activer CFG pour chaque partie de votre code, car une combinaison de CFG est activée et le code non activé par CFG s’exécute parfaitement. Toutefois, l’activation de CFG pour l’ensemble du code peut entraîner des lacunes dans la protection. En outre, le code CFG activé fonctionne correctement sur les versions « ne connaissant pas CFG » de Windows et est donc entièrement compatible avec eux.

#### <a name="windows-defender-antivirus"></a>Antivirus Windows Defender
Windows Server 2016 comprend les fonctionnalités de détection actives du secteur de Windows Defender pour bloquer les logiciels malveillants connus. L’antivirus Windows Defender (AV) fonctionne avec Windows Defender Device Guard et Control Flow Guard pour empêcher l’installation de code malveillant de tout type sur vos serveurs. Il est activé par défaut : l’administrateur n’a pas besoin d’entreprendre aucune action pour qu’il commence à fonctionner. L’antivirus Windows Defender est également optimisé pour prendre en charge les différents rôles de serveur dans Windows Server 2016. Par le passé, les attaquants utilisaient des shells tels que PowerShell pour lancer du code binaire malveillant. Dans Windows Server 2016, PowerShell est maintenant intégré à l’antivirus Windows Defender pour rechercher les programmes malveillants avant de lancer le code.

L’antivirus Windows Defender est une solution de logiciel anti-programme malveillant intégrée qui offre une gestion de la sécurité et du logiciel anti-programme malveillant pour les ordinateurs de bureau, les ordinateurs portables et les serveurs. L’antivirus Windows Defender a été considérablement amélioré depuis qu’il a été introduit dans Windows 8. L’antivirus Windows Defender dans Windows Server utilise une approche sur plusieurs fronts pour améliorer les logiciels anti-programme malveillant :

- La **Protection via le Cloud** vous aide à détecter et à bloquer les nouveaux programmes malveillants en quelques secondes, dès leur apparition.

- Le**contexte local enrichi** améliore la façon dont les programmes malveillants sont identifiés. Windows Server informe l’AV Windows Defender non seulement du contenu comme les fichiers et les processus, mais également de l’emplacement d’origine du contenu, de l’emplacement où il a été stocké, etc. 

- Des **capteurs globaux étendus** permettent de maintenir l’antivirus Windows Defender à jour et de prendre en compte même les programmes malveillants les plus récents. Ceci, de deux façons : en collectant les données du contexte local enrichi à partir de points de terminaison et en analysant ces données de manière centralisée.

- La vérification de la **falsification** contribue à protéger l’antivirus Windows Defender contre les attaques de programmes malveillants. Par exemple, Windows Defender AV utilise des processus protégés, ce qui empêche les processus non fiables de tenter de falsifier les composants de l’antivirus Windows Defender, ses clés de Registre, etc.

- Les **fonctionnalités au niveau de l’entreprise** offrent aux professionnels de l’informatique les outils et les options de configuration nécessaires pour faire de Windows Defender AV une solution anti-programme malveillant de classe entreprise.

#### <a name="enhanced-security-auditing"></a>Audit de sécurité amélioré 
Windows Server 2016 avertit activement les administrateurs des tentatives de violation potentielles avec un audit de sécurité amélioré qui fournit des informations plus détaillées, qui peuvent être utilisées pour une détection plus rapide des attaques et une analyse de l’investigation. Il journalise les événements à partir de la protection de workflow de contrôle, de Windows Defender Device Guard et d’autres fonctionnalités de sécurité dans un seul emplacement, ce qui permet aux administrateurs de déterminer plus facilement les systèmes qui peuvent être menacés.

Les nouvelles catégories d’événements sont les suivantes :

- **Auditer l’appartenance à un groupe.** Permet d’auditer les informations d’appartenance au groupe dans le jeton de connexion d’un utilisateur. Les événements sont générés lorsque des appartenances aux groupes sont énumérées ou interrogées sur le PC sur lequel la session de connexion a été créée. 
 
- **Activité PnP d’audit.** Permet d’effectuer un audit lorsque la fonctionnalité Plug-and-Play détecte un périphérique externe, qui peut contenir des programmes malveillants. Les événements PnP peuvent être utilisés pour détecter les modifications apportées au matériel système. Une liste d’ID de fournisseur de matériel est incluse dans l’événement.

Windows Server 2016 s’intègre facilement aux systèmes SIEM (Security incident Event Management), tels que Microsoft Operations Management Suite (OMS), qui peuvent incorporer les informations dans des rapports d’intelligence sur les violations potentielles. La profondeur des informations fournies par l’audit amélioré permet aux équipes de sécurité d’identifier et de répondre aux failles potentielles plus rapidement et efficacement.

### <a name="secure-virtualization"></a>Virtualisation sécurisée
Aujourd’hui, les entreprises virtualisent tout ce qu’elles peuvent, de SQL Server à SharePoint à des contrôleurs domaine Active Directory. Les machines virtuelles facilitent simplement le déploiement, la gestion, le service et l’automatisation de votre infrastructure. Toutefois, en ce qui concerne la sécurité, les infrastructures de virtualisation compromises sont devenues un nouveau vecteur d’attaque difficile à défendre, jusqu’à présent. D’un point de vue RGPD, vous devez envisager de protéger les machines virtuelles comme vous protégeriez les serveurs physiques, y compris l’utilisation de la technologie de module de plateforme sécurisée de machine virtuelle.

Windows Server 2016 change fondamentalement la manière dont les entreprises peuvent sécuriser la virtualisation, en incluant plusieurs technologies qui vous permettent de créer des machines virtuelles qui s’exécutent uniquement sur votre propre infrastructure. aider à protéger les appareils de stockage, réseau et hôtes sur lesquels ils s’exécutent.

#### <a name="shielded-virtual-machines"></a>Machines virtuelles dotées d'une protection maximale
Les mêmes choses qui rendent les machines virtuelles si faciles à migrer, sauvegarder et répliquer, les rendent également plus faciles à modifier et à copier. Un ordinateur virtuel est simplement un fichier, donc il n’est pas protégé sur le réseau, dans le stockage, dans les sauvegardes ou ailleurs. Un autre problème est que les administrateurs de structure, qu’il s’agisse d’un administrateur de stockage ou d’un administrateur réseau, ont accès à toutes les machines virtuelles.

Un administrateur compromis sur l’infrastructure peut facilement générer des données compromises entre les machines virtuelles. Tout ce qu’il faut faire, c’est d’utiliser les informations d’identification compromises pour copier les fichiers de machine virtuelle qu’ils aiment sur un lecteur USB et de les parcourir en dehors de l’organisation, où ces fichiers sont accessibles à partir de n’importe quel autre système. Si l’une de ces machines virtuelles volées était un contrôleur de domaine Active Directory, par exemple, l’attaquant pourrait facilement afficher le contenu et utiliser des techniques de force brute facilement disponibles pour déchiffrer les mots de passe dans la base de données Active Directory, en leur donnant accès à tout le reste au sein de votre infrastructure.

Windows Server 2016 introduit des machines virtuelles protégées (machines virtuelles dotées d’une protection maximale) pour vous protéger contre les scénarios comme celui qui vient d’être décrit. Les machines virtuelles protégées incluent un appareil TPM virtuel qui permet aux organisations d’appliquer le chiffrement BitLocker aux machines virtuelles et de s’assurer qu’elles s’exécutent uniquement sur des ordinateurs hôtes approuvés, afin de protéger les administrateurs de stockage, de réseau et d’ordinateur hôte. Les machines virtuelles protégées sont créées à l’aide de machines virtuelles de 2e génération, qui prennent en charge le microprogramme Unified Extensible Firmware Interface (UEFI) et disposent d’un module TPM

#### <a name="host-guardian-service"></a>Service Guardian hôte
Outre les machines virtuelles protégées, le service Guardian hôte est un composant essentiel de la création d’une structure de virtualisation sécurisée. Son travail consiste à attester de l’intégrité d’un hôte Hyper-V avant de permettre à une machine virtuelle protégée de démarrer ou de migrer vers cet ordinateur hôte. Il contient les clés des machines virtuelles protégées et ne les libère pas tant que l’intégrité de la sécurité n’est pas assurée. Il existe deux façons de demander aux hôtes Hyper-V d’attester auprès du service Guardian hôte.

La première et la plus sécurisée sont une attestation approuvée par le matériel. Cette solution nécessite que vos machines virtuelles protégées s’exécutent sur des hôtes qui ont des puces TPM 2,0 et UEFI 2.3.1. Ce matériel est requis pour fournir les informations de démarrage et d’intégrité du noyau du système d’exploitation requises par le service Guardian hôte pour s’assurer que l’hôte Hyper-V n’a pas été falsifié.

Les organisations informatiques ont une alternative à l’utilisation de l’attestation approuvée par l’administrateur, ce qui peut être souhaitable si le matériel TPM 2,0 n’est pas utilisé dans votre organisation. Ce modèle d’attestation est facile à déployer, car les hôtes sont simplement placés dans un groupe de sécurité et le service Guardian hôte est configuré pour permettre l’exécution des machines virtuelles protégées sur les hôtes qui sont membres du groupe de sécurité. Avec cette méthode, il n’existe aucune mesure complexe permettant de s’assurer que l’ordinateur hôte n’a pas été falsifié. Toutefois, vous éliminez le risque que des machines virtuelles non chiffrées ne parcourent la porte sur des lecteurs USB ou que la machine virtuelle s’exécute sur un ordinateur hôte non autorisé. Cela est dû au fait que les fichiers de la machine virtuelle ne s’exécutent pas sur les ordinateurs autres que ceux du groupe désigné. Si vous n’avez pas encore de matériel TPM 2,0, vous pouvez commencer par une attestation approuvée par l’administrateur et basculer vers une attestation approuvée par le matériel lors de la mise à niveau de votre matériel.

#### <a name="virtual-machine-trusted-platform-module"></a>Module de plateforme sécurisée (TPM) d’ordinateur virtuel
Windows Server 2016 prend en charge TPM pour les machines virtuelles, ce qui vous permet de prendre en charge des technologies de sécurité avancées telles que BitLocker® le chiffrement de lecteur dans les machines virtuelles. Vous pouvez activer la prise en charge du module de plateforme sécurisée sur n’importe quel ordinateur virtuel Hyper-V de génération 2 à l’aide du Gestionnaire Hyper-V ou de l’applet de commande Windows PowerShell Enable-VMTPM.

Vous pouvez protéger le module de plateforme sécurisée (vTPM) virtuel à l’aide des clés de chiffrement local stockées sur l’ordinateur hôte ou stockées dans le service Guardian hôte. Ainsi, même si le service Guardian hôte requiert davantage d’infrastructure, il fournit également une protection accrue.

#### <a name="distributed-network-firewall-using-software-defined-networking"></a>Pare-feu de réseau distribué utilisant une mise en réseau définie par logiciel
Une façon d’améliorer la protection dans les environnements virtualisés consiste à segmenter le réseau d’une manière qui permet aux machines virtuelles de communiquer uniquement avec les systèmes spécifiques requis pour fonctionner. Par exemple, si votre application n’a pas besoin de se connecter à Internet, vous pouvez la partitionner, en éliminant ces systèmes comme cibles des attaquants externes. Le SDN (Software-Defined Networking) de Windows Server 2016 comprend un pare-feu de réseau distribué qui vous permet de créer dynamiquement les stratégies de sécurité qui peuvent protéger vos applications contre les attaques provenant de l’intérieur ou de l’extérieur d’un réseau. Ce pare-feu de réseau distribué ajoute des couches à votre sécurité en vous permettant d’isoler vos applications sur le réseau. Les stratégies peuvent être appliquées n’importe où dans l’infrastructure de votre réseau virtuel, en isolant le trafic entre les machines virtuelles, les machines VIRTUELles hôtes ou les machines virtuelles sur Internet, le cas échéant, soit pour les systèmes individuels susceptibles d’avoir été compromis, soit par programmation. plusieurs sous-réseaux. Les fonctionnalités de mise en réseau définies par le logiciel Windows Server 2016 vous permettent également d’acheminer ou de mettre en miroir le trafic entrant vers des appliances virtuelles non-Microsoft. Par exemple, vous pouvez choisir d’envoyer tout le trafic de votre messagerie par le biais d’une appliance virtuelle Barracuda pour une protection supplémentaire contre le filtrage du courrier indésirable. Cela vous permet de facilement effectuer une couche de sécurité supplémentaire en local ou dans le Cloud.

### <a name="other-gdpr-considerations-for-servers"></a>Autres considérations relatives à RGPD pour les serveurs
Le RGPD comprend des exigences explicites pour la notification de violation lorsqu’une divulgation de données personnelle signifie «_une violation de sécurité entraînant une destruction accidentelle ou illégale, une perte, une modification, une divulgation non autorisée ou l’accès à des données personnelles transmises, stockées ou traitées de manière autre._ »  Évidemment, vous ne pouvez pas commencer à répondre aux exigences strictes de notification RGPD dans un délai de 72 heures si vous ne pouvez pas détecter la violation en premier lieu.

Comme indiqué dans le livre blanc Windows Security Center, [billet de blog : gestion des menaces avancées](http://wincom.blob.core.windows.net/documents/Post_Breach_Dealing_with_Advanced_Threats_Whitepaper.pdf)

> «_Contrairement à la préviolation, après une violation, une violation s’est déjà produite, agissant comme un enregistreur de vol et un investigateur de scène criminel (CSI). Après la violation, les équipes de sécurité fournissent les informations et l’ensemble d’outils nécessaires à l’identification, à l’examen et à la réponse aux attaques qui, autrement, ne pourront pas être détectées et inférieures au radar._ »

Dans cette section, nous allons voir comment Windows Server peut vous aider à répondre à vos obligations de notification de violation RGPD. Cela commence par la compréhension des données de menace sous-jacentes disponibles pour Microsoft qui sont collectées et analysées en fonction de votre avantage et de la façon dont les données peuvent être essentielles par le biais de Windows Defender-protection avancée contre les menaces (ATP).

#### <a name="insightful-security-diagnostic-data"></a>Données de diagnostic de sécurité révélatrices
Depuis près de deux décennies, Microsoft a mis des menaces à l’aide d’une intelligence utile qui peut aider à renforcer sa plateforme et à protéger les clients. Aujourd'hui, grâce aux immenses avantages informatiques offerts par le cloud, nous trouvons de nouvelles façons d’utiliser nos moteurs d'analyse enrichis, fondés sur les renseignements concernant les menaces pour protéger nos clients.

En appliquant une combinaison de processus automatisés et manuels, d'apprentissage machine et d'experts humains, nous pouvons créer un Intelligent Security Graph qui apprend de lui-même et évolue en temps réel, afin de réduire notre temps collectif nécessaire pour détecter les nouveaux incidents et y réagir pour l'ensemble de nos produits.

![Graphique de sécurité Microsoft intelligence](../media/GDPR-Windows-Server-Overview/gdpr-intelligent-security-graph.png)

L’étendue des informations sur les menaces de Microsoft s’étend, littéralement, des milliards de points de données : 35 milliards messages analysés tous les mois, 1 milliard clients dans l’entreprise et des segments de grand public accédant à plus de 200 services Cloud et 14 milliards authentifications effectuées quotidienne. Toutes ces données sont rassemblées en votre nom par Microsoft pour créer le Intelligent Security Graph qui peut vous aider à protéger votre porte d’entrée de manière dynamique pour rester sécurisé, rester productif et répondre aux exigences du RGPD.

#### <a name="detecting-attacks-and-forensic-investigation"></a>Détection des attaques et enquêtes scientifiques
Même les meilleures défenses de point de terminaison peuvent finalement présenter des failles, à mesure que les cyberattaques deviennent plus sophistiquées et plus ciblées. Deux fonctionnalités peuvent être utilisées pour faciliter la détection de violations potentielles : Windows Defender-protection avancée contre les menaces (ATP) et Microsoft Advanced Threat Analytics (ATA).

La protection avancée contre les menaces Windows Defender (ATP) vous permet de détecter les attaques avancées et les violations de données sur vos réseaux, à les examiner et à réagir en conséquence. Les types de violations de données que le RGPD s’attend à vous protéger par le biais de mesures de sécurité techniques visant à garantir la confidentialité, l’intégrité et la disponibilité continues des données personnelles et des systèmes de traitement.

L’un des principaux avantages de Windows Defender ATP est le suivant :

- **Détection de l’indétectable.** Des capteurs intégrés au noyau du système d’exploitation, aux experts en sécurité Windows et à des fibres optiques uniques à partir de plus de 1 milliard machines et signalent sur tous les services Microsoft.

- **Intégré, non boulonnée.** Sans agent, avec des performances élevées et un impact minimal, alimenté par le Cloud ; facilité de gestion sans déploiement. 

- **Volet unique pour la sécurité Windows.** Explorez 6 mois de la gamme complète, de la chronologie des machines, en unifiant les événements de sécurité de Windows Defender ATP, de l’antivirus Windows Defender et de Windows Defender Device Guard.

- **La puissance de Microsoft Graph.** Utilise Microsoft Intelligence Security Graph pour intégrer la détection et l’exploration avec l’abonnement Office 365 ATP, pour effectuer le suivi des attaques et y répondre.

Pour en savoir plus [, consultez Nouveautés de l’aperçu de la mise à jour des créateurs Windows Defender ATP](https://blogs.microsoft.com/microsoftsecure/2017/03/13/whats-new-in-the-windows-defender-atp-creators-update-preview/).

ATA est un produit local qui permet de détecter la compromission de l’identité dans une organisation. ATA peut capturer et analyser le trafic réseau pour les protocoles d’authentification, d’autorisation et de collecte d’informations (tels que Kerberos, DNS, RPC, NTLM et d’autres protocoles). ATA utilise ces données pour créer un profil comportemental sur les utilisateurs et d’autres entités sur un réseau afin qu’il puisse détecter les anomalies et les modèles d’attaque connus. Le tableau suivant répertorie les types d’attaques détectés par ATA.


|Type d’attaque |Description |
|---------|---------|
|Attaques malveillantes |Ces attaques sont détectées en recherchant des attaques à partir d’une liste connue de types d’attaques, notamment :<ul><li>Pass-the-Ticket (PtT)</li><li>Pass-the-Hash (PtH)</li><li>Overpass-the-Hash</li><li>Faux PAC (MS14-068)</li><li>Golden Ticket</li><li>Réplications malveillantes</li><li>Reconnaissance</li><li>Force brute</li><li>Exécution distante</li></ul>Pour obtenir la liste complète des attaques malveillantes qui peuvent être détectées et leur description, consultez [Quelles sont les activités suspectes](https://docs.microsoft.com/advanced-threat-analytics/understand-explore/ata-threats)détectées par ATA ?.|
|Comportement anormal |Ces attaques sont détectées à l’aide de l’analyse comportementale et utilisent Machine Learning pour identifier les activités douteuses, notamment :<ul><li>Connexions anormales</li><li>Menaces inconnues</li><li>Partage de mot de passe</li><li>Mouvement latéral</li></ul>|
|Problèmes et risques liés à la sécurité |Ces attaques sont détectées en examinant la configuration actuelle du réseau et du système, notamment :<ul><li>Relation de confiance rompue</li><li>Protocoles faibles</li><li>Vulnérabilités de protocole connues</li></ul>|

Vous pouvez utiliser ATA pour détecter les attaquants tentant de compromettre les identités privilégiées. Pour plus d’informations sur le déploiement d’ATA, consultez les rubriques plan, conception et déploiement dans la [documentation Advanced Threat Analytics](https://docs.microsoft.com/advanced-threat-analytics/).

## <a name="related-content-for-associated-windows-server-2016-solutions"></a>Contenu associé pour les solutions Windows Server 2016 associées

- **Antivirus Windows Defender :** https://www.youtube.com/watch?v=P1aNEy09NaI et https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10

- **Windows Defender-protection avancée contre les menaces :** https://www.youtube.com/watch?v=qxeGa3pxIwg et https://docs.microsoft.com/windows/threat-protection/windows-defender-atp/configure-server-endpoints-windows-defender-advanced-threat-protection

- **Windows Defender Device Guard :** https://www.youtube.com/watch?v=F-pTkesjkhI et https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide

- **Protection des informations d’identification Windows Defender :** https://www.youtube.com/watch?v=F-pTkesjkhI et https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard

- **Protection du workflow de contrôle :** https://msdn.microsoft.com/library/windows/desktop/mt637065(v=vs.85).aspx

- **Sécurité et assurance :** https://docs.microsoft.com/windows-server/security/security-and-assurance

## <a name="disclaimer"></a>Clause d’exclusion de responsabilité
Cet article est un commentaire sur le RGPD, tel que Microsoft l’interprète, à compter de la date de publication. Nous avons passé beaucoup de temps avec RGPD et aimerions que nous ayons pensé à son intention et à sa signification. Mais l’application du RGPD est propre à chaque cas et les aspects et les interprétations du RGPD ne sont pas tous bien fixés.

Par conséquent, cet article est fourni à titre d’information uniquement. Il ne doit pas être considéré comme constituant un avis juridique ni servir à déterminer la façon dont le RGPD peut s’appliquer à vous et à votre organisation. Nous vous encourageons à travailler avec un professionnel juridiquement qualifié pour discuter du RGPD, de la manière dont il s’applique spécifiquement à votre organisation et des meilleures méthodes pour garantir la conformité.

MICROSOFT N’APPORTE AUCUNE GARANTIE, EXPRESSE, IMPLICITE OU LÉGALE, QUANT AU CONTENU DE CET ARTICLE. Cet article est fourni «en l'état». Les informations et idées exprimées dans ce article, y compris les URL et autres références à des sites web sur Internet, peuvent faire l'objet de modifications sans préavis.

Cet article ne vous fournit aucun droit légal de propriété intellectuelle de tout produit Microsoft.  Vous pouvez copier le présent article pour une utilisation interne à des fins de référence uniquement.  

Publié en septembre 2017<br>
Version 1.0<br>
© 2017 Microsoft. Tous droits réservés.


