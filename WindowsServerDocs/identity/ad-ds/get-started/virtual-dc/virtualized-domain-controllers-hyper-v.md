---
Title: Virtualisation de contrôleurs de domaine à l’aide de Hyper-V
description: Considérations à faire lors de la virtualisation Windows Server Active Directory contrôleurs de domaine dans Hyper-V
author: MicrosoftGuyJFlo
ms.author: joflore
ms.date: 04/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.openlocfilehash: 297c2a26f10503cb68ae241576a72e08aa4e55a0
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469567"
---
# <a name="virtualizing-domain-controllers-using-hyper-v"></a>Virtualisation de contrôleurs de domaine à l’aide de Hyper-V

> S’applique à : Windows Server 2016

Cette rubrique sera actualisée afin de rendre les instructions applicables à Windows Server 2016. Windows Server 2012 introduit de nombreuses améliorations pour les contrôleurs de domaine virtualisés (contrôleurs de domaine), y compris les dispositifs de protection pour empêcher la restauration USN sur des contrôleurs de domaine virtuels et la possibilité de cloner des contrôleurs de domaine virtuels. Pour plus d’informations sur ces améliorations, consultez [Introduction à la virtualisation des Services de domaine Active Directory (AD DS) (niveau 100)](../../introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100.md).

Hyper-V consolide les différents rôles de serveur sur un ordinateur physique unique. Ce guide décrit les contrôleurs de domaine en cours d’exécution en tant que systèmes d’exploitation invités 32 bits ou 64 bits.

## <a name="planning-to-virtualize-domain-controllers"></a>Observations sur la planification des contrôleurs de domaine virtualisés

Cette section décrit la configuration matérielle requise pour le serveur Hyper-v, comment éviter les points uniques de panne, en sélectionnant le type de configuration pour vos serveurs Hyper-V et les machines virtuelles et les décisions de sécurité et de performances approprié.

## <a name="hyper-v-requirements"></a>Configuration requise pour Hyper-V

Pour installer et utiliser le rôle Hyper-V, vous devez disposer des éléments suivants :

   - **Un x64 processeur**
      - Hyper-V est disponible dans les versions x64 64 de Windows Server 2008 ou version ultérieure.  
   - **Virtualisation assistée par matériel**
      - Cette fonction est disponible dans les processeurs qui incluent une option de virtualisation ; plus spécifiquement, Intel VT (Intel Virtualization Technology) ou AMD Virtualization (AMD-V).  
   - **Protection matériel de l’exécution des données (DEP)**
      - La protection DEP matérielle doit être disponible et activée. Plus précisément, vous devez activer le bit Intel XD (execute disable bit) ou le bit AMD NX (aucun bit de non-exécution).  

## <a name="avoid-creating-single-points-of-failure"></a>Éviter de créer des points de défaillance uniques

Vous devez essayer d'éviter de créer des points de défaillance uniques potentiels lorsque vous planifiez votre déploiement de contrôleur de domaine virtuel. Vous pouvez éviter d'introduire des points de défaillance uniques potentiels en implémentant la redondance système. Par exemple, tenez compte des recommandations suivantes tout en gardant à l'esprit le risque d'augmentations du coût d'administration :

1. Exécutez au moins deux contrôleurs de domaine virtualisés par domaine sur des hôtes de virtualisation différents, ce qui réduit le risque de perdre tous les contrôleurs de domaine en cas de défaillance d'un hôte de virtualisation unique.  
2. Comme recommandé pour d'autres technologies, diversifiez le matériel (en utilisant des unités centrales, cartes mères, cartes réseau différentes, ou d'autres matériels) sur lequel les contrôleurs de domaine sont exécutés. La diversification du matériel limite les dommages que risque d'entraîner un dysfonctionnement qui est spécifique à une configuration de fournisseur, à un pilote ou à un composant matériel ou type de matériel unique.  
3. Dans la mesure du possible, les contrôleurs de domaine doivent être exécutés sur du matériel qui se trouve dans différentes régions du monde. Cela permet de réduire l'impact d'un incident ou d'une défaillance qui affecte un site sur lequel les contrôleurs de domaine sont hébergés.  
4. Maintenez des contrôleurs de domaine physiques dans chacun de vos domaines. Cela réduit le risque d'un dysfonctionnement de la plateforme de virtualisation qui affecte tous les systèmes hôtes qui utilisent cette plateforme.  

## <a name="security-considerations"></a>Considérations relatives à la sécurité

L'ordinateur hôte sur lequel sont exécutés les contrôleurs de domaine virtuels doit être administré avec précaution en tant que contrôleur de domaine inscriptible, même s'il ne s'agit que d'un ordinateur de groupe de travail ou appartenant à un domaine. Il s'agit là d'un facteur de sécurité important. Un hôte incorrectement administré est vulnérable aux attaques par élévation de privilèges. Cela se produit lorsqu'un utilisateur malveillant bénéficie de droits d'accès et de privilèges système qui n'ont pas été autorisés ou octroyés de façon légitime. Tout utilisateur malveillant peut utiliser ce type d'attaque pour endommager l'ensemble des ordinateurs virtuels, des domaines et des forêts qu'héberge l'ordinateur concerné.

Tenez compte des points suivants concernant la sécurité si vous prévoyez de virtualiser des contrôleurs de domaine :

   - L'administrateur local d'un ordinateur qui héberge des contrôleurs de domaine inscriptibles virtuels doit être considéré comme l'équivalent, en termes d'informations d'identification, de l'administrateur de domaine par défaut chargé de l'ensemble des domaines et forêts auxquels appartiennent ces contrôleurs.  
   - La configuration recommandée pour éviter les problèmes de performances et la sécurité est un ordinateur hôte exécutant une installation Server Core de Windows Server 2008 ou version ultérieure, sans applications autres que Hyper-V. Cette configuration limite le nombre d’applications et services qui sont installés sur le serveur, ce qui devrait améliorer les performances et moins d’applications et services susceptibles d’être exploitées à des fins malveillantes pour attaquer l’ordinateur ou le réseau. Ce type de configuration fournit ce que l'on nomme une surface d'attaque réduite. Dans une filiale ou tout autre lieu difficile à sécuriser correctement, un contrôleur de domaine en lecture seule (RODC) est recommandé. S'il existe un réseau d'administration distinct, il est recommandé de connecter l'hôte uniquement à ce réseau.  
   - Vous pouvez utiliser Bitlocker avec vos contrôleurs de domaine, depuis Windows Server 2016, vous pouvez utiliser la fonctionnalité de module de plateforme sécurisée virtuel pour également donnent le matériel de clé invité à déverrouiller le volume système.
   - [Service Guardian fabric et des machines virtuelles protégées](/it-server/WindowsServerDocs/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md) peut fournir des contrôles supplémentaires pour protéger vos contrôleurs de domaine.

Pour plus d’informations sur les RODC, consultez [Guide de déploiement et de planification de contrôleur de domaine en lecture seule](../../deploy/rodc/read-only-domain-controller-updates.md).

Pour plus d’informations sur la sécurisation des contrôleurs de domaine, consultez [Guide des meilleures pratiques pour sécuriser les Installations Active Directory](../../plan/security-best-practices/best-practices-for-securing-active-directory.md).

## <a name="security-boundaries-for-different-host-and-guest-configurations"></a>Limites de sécurité pour un hôte différent et les configurations de l’invité

Le recours aux ordinateurs virtuels permet de disposer de plusieurs configurations de contrôleurs de domaine. Il est important de connaître l'incidence des ordinateurs virtuels sur les limites et les approbations de votre topologie Active Directory. Le tableau ci-après décrit plusieurs configurations possibles pour un hôte et contrôleur de domaine Active Directory (serveur Hyper-V) ainsi que pour ses invités (ordinateurs virtuels exécutés sur le serveur Hyper-V).

|Machine|Configuration 1|« Configuration 2 »|
|-------|---------------|---------------|
|Host|Groupe de travail ou ordinateur membre|Groupe de travail ou ordinateur membre|
|Invité|Contrôleur de domaine|Groupe de travail ou ordinateur membre|

![](media/virtualized-domain-controller-architecture/Dd363553.f44706fd-317e-4f0b-9578-4243f4db225f(WS.10).gif)

   - L'administrateur de l'ordinateur hôte dispose des mêmes droits d'accès qu'un administrateur de domaine sur les invités du contrôleur de domaine inscriptible, et il doit être considéré comme tel. En présence d'un invité RODC, l'administrateur de l'ordinateur hôte bénéficie des mêmes droits d'accès qu'un administrateur local sur le RODC local.   
   - Un contrôleur de domaine exécuté sur un ordinateur virtuel bénéficie de droits d'administration sur l'hôte si ce dernier appartient au même domaine. Il existe une opportunité pour un utilisateur malveillant de compromettre tous les ordinateurs virtuels si l’utilisateur malveillant parvient à accéder à l’ordinateur 1. Il s’agit comme un vecteur d’attaque. S'il existe des contrôleurs de domaine pour plusieurs domaines ou forêts, ces domaines doivent bénéficier d'une administration centralisée selon laquelle l'administrateur d'un domaine est approuvé sur tous les domaines.  
   - Les possibilités d'attaque à partir de l'ordinateur virtuel 1 existent, même si cet ordinateur est installé en tant que contrôleur de domaine en lecture seule. En effet, bien que l'administrateur d'un RODC ne dispose pas explicitement de droits d'administrateur, le RODC peut servir à envoyer des stratégies à l'ordinateur hôte. Or, ces stratégies peuvent contenir des scripts de démarrage. Si cette opération réussit, l'ordinateur hôte risque d'être compromis et peut alors servir à compromettre les autres systèmes virtuels présents dans l'ordinateur hôte.  

## <a name="security-of-vhd-files"></a>Sécurité des fichiers VHD

Un fichier VHD de contrôleur de domaine virtuel équivaut au disque dur physique d'un contrôleur de domaine physique. Il doit donc être protégé de la même façon que le disque dur d'un contrôleur de domaine physique. Vérifiez que seuls des administrateurs fiables et approuvés sont autorisés à accéder aux fichiers VHD du contrôleur de domaine.

## <a name="rodcs"></a>Contrôleurs RODC

L'un des avantages des contrôleurs de domaine en lecture seule est qu'il est possible de les placer dans des lieux où la sécurité physique ne peut être assurée, comme dans des filiales. Vous pouvez utiliser le chiffrement de lecteur BitLocker Windows pour protéger les fichiers de disque dur virtuel (pas les systèmes de fichiers qui y sont) d’être compromis sur l’ordinateur hôte à un vol du disque physique. 

## <a name="performance"></a>Performances

La nouvelle architecture 64 bits du micronoyau contribue à améliorer notablement les performances Hyper-V par rapport aux plateformes de virtualisation antérieures. Pour optimiser les performances de l’hôte doit être une installation Server Core de Windows Server 2008 ou version ultérieure, et il ne doit comporter que des rôles de serveur autre que Hyper-V est installé.

La performance des ordinateurs virtuels dépend en particulier de la charge de travail. Pour garantir une performance satisfaisante d'Active Directory, testez les topologies spécifiques. Évaluer la charge de travail actuel sur une période de temps avec un outil tel que la fiabilité et l’Analyseur de performances (Perfmon.msc) ou le [Microsoft Assessment and Planning (MAP) toolkit](https://go.microsoft.com/fwlink/?linkid=137077). L'outil MAP peut également s'avérer utile si vous souhaitez faire l'inventaire de tous les serveurs et rôles de serveur de votre réseau.

Pour obtenir une idée générale des performances des contrôleurs de domaine virtualisés, les tests de performances suivants ont été effectués avec le [Active Directory Performance Testing Tool (ADTest.exe)](https://go.microsoft.com/fwlink/?linkid=137088).

Des tests LDAP (Lightweight Directory Access Protocol) ont été réalisés sur un contrôleur de domaine physique avec ADTest.exe, puis sur un ordinateur virtuel hébergé sur un serveur identique au contrôleur de domaine physique. Un seul processeur logique a été utilisé pour l'ordinateur physique, de même qu'un seul processeur virtuel pour l'ordinateur virtuel, de façon à atteindre facilement une utilisation à 100 % du processeur. Dans le tableau suivant, la lettre et un nombre entre parenthèses après chaque test indiquent le test spécifique dans ADTest.exe. Comme le montre ces données, les performances du contrôleur de domaine virtualisés a été 88 à 98 % des performances de contrôleur de domaine physique.

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Mesure</th>
<th>Tester</th>
<th>Physique</th>
<th>Virtuel</th>
<th>Delta</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Recherches/s</p></td>
<td><p>Recherche de nom commun dans l'étendue de base (L1)</p></td>
<td><p>11508</p></td>
<td><p>10276</p></td>
<td><p>-10.71%</p></td>
</tr>
<tr class="even">
<td><p>Recherches/s</p></td>
<td><p>Recherche de groupe d'attributs dans l'étendue de base (L2)</p></td>
<td><p>10123</p></td>
<td><p>9005</p></td>
<td><p>-11.04%</p></td>
</tr>
<tr class="odd">
<td><p>Recherches/s</p></td>
<td><p>Recherche de tous les attributs dans l'étendue de base (L3)</p></td>
<td><p>1284</p></td>
<td><p>1242</p></td>
<td><p>-3.27%</p></td>
</tr>
<tr class="even">
<td><p>Recherches/s</p></td>
<td><p>Recherche de nom commun dans l'étendue de sous-arborescence (L6)</p></td>
<td><p>8613</p></td>
<td><p>7904</p></td>
<td><p>-8.23%</p></td>
</tr>
<tr class="odd">
<td><p>Liaisons réussies/s</p></td>
<td><p>Réalisation de liaisons rapides (B1)</p></td>
<td><p>1438</p></td>
<td><p>1374</p></td>
<td><p>-4.45%</p></td>
</tr>
<tr class="even">
<td><p>Liaisons réussies/s</p></td>
<td><p>Réalisation de liaisons simples (B2)</p></td>
<td><p>611</p></td>
<td><p>550</p></td>
<td><p>-9.98%</p></td>
</tr>
<tr class="odd">
<td><p>Liaisons réussies/s</p></td>
<td><p>Utilisation de NTLM pour réaliser les liaisons (B5)</p></td>
<td><p>1068</p></td>
<td><p>1056</p></td>
<td><p>-1.12%</p></td>
</tr>
<tr class="even">
<td><p>Écritures/s</p></td>
<td><p>Écriture de plusieurs attributs (W2)</p></td>
<td><p>6467</p></td>
<td><p>5885</p></td>
<td><p>-9.00%</p></td>
</tr>
</tbody>
</table>

Pour garantir une performance adéquate, des composants d'intégration (IC) ont été installés afin de permettre au système d'exploitation invité d'utiliser des « états d'éveil à la présence d'un environnement virtualisé » ou des pilotes synthétiques prenant en charge les hyperviseurs. Au cours du processus d'installation, il peut être nécessaire d'utiliser des pilotes de cartes réseau ou des pilotes IDE (Integrated Drive Electronics) émulés. Dans un environnement de production, remplacez ces pilotes émulés par des pilotes synthétiques afin d'accroître les performances.

Lorsque vous contrôlez les performances des ordinateurs virtuels avec l'analyseur de fiabilité et de performances (Perfmon.msc), les informations sur l'unité centrale, dans l'ordinateur virtuel, ne sont pas exactes en raison de la façon dont l'UC est planifiée sur le processeur physique. Si vous souhaitez obtenir des informations sur l'unité centrale d'un ordinateur virtuel exécuté sur un serveur Hyper-V, utilisez le processeur logique de l'hyperviseur Hyper-V dans la partition hôte.

Pour plus d’informations sur les performances, réglage d’AD DS et Hyper-V, consultez [performances réglage des instructions pour Windows Server 2016](../../../../administration/performance-tuning/index.md).

Par ailleurs, ne prévoyez pas d'utiliser un disque de différenciation VHD sur un ordinateur virtuel configuré en tant que contrôleur de domaine : cela risquerait de faire chuter les performances. Pour en savoir plus sur les types de disque Hyper-V, y compris la différenciation des disques, consultez [Assistant du nouveau disque dur virtuel](http://go.microsoft.com/fwlink/?linkid=137279).

Pour obtenir des informations supplémentaires sur les services AD DS dans les environnements d’hébergement virtuels, consultez [choses à prendre en compte lorsque vous hébergez des contrôleurs de domaine Active Directory dans les environnements d’hébergement virtuels](https://go.microsoft.com/fwlink/?linkid=141292) dans la Base de connaissances Microsoft.

## <a name="deployment-considerations-for-virtualized-domain-controllers"></a>Observations sur le déploiement de contrôleurs de domaine virtualisés

Il existe plusieurs pratiques courantes machine virtuelle que vous ne devez pas lorsque vous déployez des contrôleurs de domaine et des considérations spéciales pour le stockage et la synchronisation date / heure.

## <a name="virtualization-deployment-practices-to-avoid"></a>Pratiques à éviter lors du déploiement d'ordinateurs virtuels

Les plateformes de virtualisation, telles que Hyper-V, proposent plusieurs fonctions pratiques qui facilitent la gestion, l'entretien, la sauvegarde et la migration des ordinateurs. Toutefois, les pratiques de déploiement courantes suivantes et les fonctionnalités doivent être pas utilisées pour les contrôleurs de domaine virtuel :

- Pour garantir la durabilité des écritures de Active Directory, ne déployez pas de fichiers de base de données d’un contrôleur de domaine virtuel (Active Directory base de données (NTDS. DIT), journaux et SYSVOL) sur les disques virtuels IDE. Au lieu de cela, créez un deuxième disque dur virtuel attaché à un contrôleur SCSI virtuel et vous assurer que la base de données, les journaux et SYSVOL sont placés sur le disque SCSI de la machine virtuelle lors de l’installation de contrôleur de domaine.  
- Ne recourez pas aux disques durs virtuels de différenciation (VHD) sur un ordinateur virtuel que vous configurez en tant que contrôleur de domaine. Il deviendrait trop aisé de revenir à une version antérieure, et les performances s'en trouveraient réduites. Pour plus d’informations sur les types de disque dur virtuel, consultez [Assistant du nouveau disque dur virtuel](https://go.microsoft.com/fwlink/?linkid=137279).  
- Ne déployez pas de nouveaux domaines Active Directory et les forêts sur une copie d’un système d’exploitation Windows Server qui a été préparée pas tout d’abord à l’aide de l’outil de préparation système (Sysprep). Pour plus d’informations sur l’exécution de Sysprep, consultez [présentation de Sysprep (préparation du système)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)

   > [!WARNING]
   > Exécution de Sysprep sur un contrôleur de domaine n’est pas pris en charge.

- Afin d'éviter la restauration potentielle d'un numéro de séquence de mise à jour (USN), n'utilisez pas les copies d'un fichier VHD représentant un contrôleur de domaine déjà déployé pour déployer d'autres contrôleurs de domaine. Pour plus d’informations sur la restauration USN, consultez [USN et restauration USN](#usn-and-usn-rollback).
   - Windows Server 2012 et versions ultérieures permet aux administrateurs de cloner les images de contrôleur de domaine si préparé correctement lorsqu’ils souhaitent déployer des contrôleurs de domaine supplémentaires
- N'utilisez pas la fonction d'exportation Hyper-V pour exporter un ordinateur virtuel qui exécute un contrôleur de domaine.
  - Avec Windows Server 2012 et versions ultérieures, une exportation et l’importation d’un invité virtuel de contrôleur de domaine est gérée comme une restauration ne faisant pas autorité comme il détecte une modification de l’ID de génération, et il n’est pas configuré pour le clonage.
  - Vérifiez que vous n’utilisez pas l’invité que vous avez exporté le plus.
    - Vous pouvez utiliser la réplication Hyper-V pour conserver une deuxième copie inactive d’un contrôleur de domaine. Si vous démarrez l’image répliquée, vous devez également effectuer un nettoyage approprié, pour la même raison que ne pas à l’aide de la source après l’exportation d’une image d’invité de contrôleur de domaine.

## <a name="physical-to-virtual-migration"></a>Migration d'un ordinateur physique vers un ordinateur virtuel

System Center Virtual Machine Manager (VMM) 2008 fournit un moyen d'administrer les ordinateurs physiques et virtuels de manière unifiée. Il permet en outre de migrer un ordinateur physique vers un ordinateur virtuel. Ce processus porte le nom de « conversion P2V » (système physique vers système virtuel). Pendant le processus de conversion P2V, le nouvel ordinateur virtuel et le contrôleur de domaine physique qui est en cours de migration ne doivent pas s’exécuter en même temps, afin d’éviter une situation de restauration USN, comme décrit dans [USN et restauration USN](#usn-and-usn-rollback).

Effectuez la conversion P2V en mode hors connexion, de façon à garantir la cohérence des données d'annuaire lors de la mise sous tension du contrôleur de domaine. Le mode hors connexion est proposé et recommandé dans l'Assistant Conversion de serveur physique. Pour obtenir une description de la différence entre les modes en ligne et hors connexion, consultez [P2V : conversion d'ordinateurs physiques en ordinateurs virtuels dans VMM](https://go.microsoft.com/fwlink/?linkid=155072). Au cours de la conversion P2V, l'ordinateur virtuel ne doit pas être connecté au réseau. La carte réseau de la machine virtuelle doit être activée uniquement une fois le processus de conversion P2V terminé et vérifié. À ce stade, l'ordinateur physique source est hors connexion. Ne reconnectez pas l'ordinateur source au réseau avant de reformater le disque dur.

> [!NOTE]
> Il existe des options plus sûres pour créer de nouveaux contrôleurs de domaine virtuels qui n’exécutent pas les risques de la création d’une restauration USN. Vous pouvez configurer un nouveau contrôleur de domaine virtuel par promotion régulière, la promotion à partir de l’installation à partir du support (IfM), ainsi que le clonage de contrôleur de domaine, si vous disposez déjà d’au moins un contrôleur de domaine virtuel.
Cela permet également d’éviter les problèmes avec le matériel ou rencontrent des problèmes liés à la plateforme P2V converti des invités virtuels.

> [!WARNING]
> Pour éviter les problèmes de réplication Active Directory, assurez-vous que seule une instance (physique ou virtuelle) d’un domaine donné contrôleur existe sur un réseau donné à n’importe quel point dans le temps.
> Vous pouvez réduire la probabilité de l’ancien clone en cours d’un problème :
> 
> - Lorsque le nouveau contrôleur de domaine virtuel est en cours d’exécution, modifiez le mot de passe de l’ordinateur à l’aide de deux fois : netdom resetpwd/Server : < contrôleur de domaine >...
> - Exporter et importer la nouveau invité virtuelle pour qu’elle devienne un nouvel ID de génération forcer et par conséquent, un appel de la base de données ID.
> 

## <a name="using-p2v-migration-to-create-test-environments"></a>Utilisation de la migration P2V en vue de créer des environnements de test

Vous pouvez procéder à une migration P2V via VMM en vue de créer des environnements de test. Il est possible de migrer des contrôleurs de domaine de production à partir d'ordinateurs physiques vers des ordinateurs virtuels de façon à créer un environnement de test sans avoir à interrompre les contrôleurs de domaine de production de manière permanente. Cependant, l'environnement de test doit se trouver sur un réseau différent de l'environnement de production s'il existe deux instances du même contrôleur de domaine. Lors de la création d'un environnement de test avec une migration P2V, veillez à éviter les restaurations USN. Celles-ci risquent d'avoir une incidence néfaste sur vos environnements de test et de production. Vous trouverez ci-après une méthode de création d'environnement de test avec P2V.

Un contrôleur de domaine d’en production à partir de chaque domaine est migré vers une machine virtuelle de test à l’aide de P2V selon les instructions indiquées dans la section de migration physique-à-virtuel. Les ordinateurs de production physiques et les ordinateurs de test virtuels doivent se trouver sur des réseaux distincts lorsqu'ils passent de nouveau en ligne. Pour éviter les restaurations USN dans l'environnement de test, mettez hors connexion tous les contrôleurs de domaine qui doivent faire l'objet d'une migration P2V. (Pour ce faire, vous pouvez arrêter le service NTDS ou redémarrer l'ordinateur en mode de restauration des services d'annuaire (DSRM).) Une fois les contrôleurs de domaine hors connexion, aucune nouvelle mise à jour ne doit être introduite dans l'environnement. Les ordinateurs doivent demeurer hors connexion pendant la migration P2V ; aucun système ne doit être reconnecté tant que tous les ordinateurs n'ont pas été entièrement migrés. Pour en savoir plus sur la restauration USN, consultez USN et restauration USN.

Les contrôleurs de domaine test qui suivent doivent être promus comme réplicas dans l'environnement de test.

## <a name="time-service"></a>Service de temps

Pour les machines virtuelles qui sont configurés en tant que contrôleurs de domaine, il est recommandé de désactiver la synchronisation horaire entre le système et invité système d’exploitation hôte agissant comme un contrôleur de domaine. Ainsi, votre contrôleur de domaine invité synchroniser l’heure à partir de la hiérarchie de domaine.

Pour désactiver le fournisseur de synchronisation de temps de Hyper-V, arrêtez la machine virtuelle et désactivez la case à cocher synchronisation sous Integration Services.

> [!NOTE]
> Ce guide a été récemment mis à jour pour refléter la recommandation actuelle pour synchroniser l’heure pour le contrôleur de domaine invité à partir de la hiérarchie de domaine uniquement, plutôt que la recommandation précédente pour désactiver la synchronisation horaire entre l’hôte partiellement contrôleur de domaine pour le système et l’invité.

## <a name="storage"></a>Stockage

Pour optimiser les performances de l’ordinateur virtuel de contrôleur de domaine et garantir la durabilité des écritures de Active Directory, utilisez les recommandations suivantes pour le stockage du système d’exploitation, Active Directory et les fichiers de disque dur virtuel :

- **Stockage invité**. Store le fichier de base de données Active Directory (Ntds.dit), les fichiers journaux et les fichiers SYSVOL sur un disque virtuel distinct à partir des fichiers de système d’exploitation. Créer un deuxième disque dur virtuel attaché à un contrôleur SCSI virtuel et stocker la base de données, les journaux et SYSVOL sur disque SCSI virtuel de la machine virtuelle. Disques SCSI virtuels fournissent des performances accrues par rapport au virtuel IDE et ils prennent en charge forcé Unit Access (FUA). FUA garantit que le système d’exploitation écrit et lit les données directement à partir du support en ignorant tous les mécanismes de mise en cache.

  > [!NOTE]
  > Si vous envisagez d’utiliser Bitlocker pour l’invité virtuelle de contrôleur de domaine, vous devez vous assurer que les volumes supplémentaires sont configurés pour « déverrouillage automatique ».
  > Plus d’informations sur la configuration automatique déverrouillage se trouve dans [Enable-BitLockerAutoUnlock](https://docs.microsoft.com/powershell/module/bitlocker/enable-bitlockerautounlock)

- **Stockage hôte des fichiers VHD**. Recommandations : ces recommandations concernent le stockage des fichiers VHD. Pour bénéficier d'une performance optimale, ne stockez pas les fichiers VHD sur un disque utilisé fréquemment par d'autres services ou applications, comme le disque système sur lequel est installé le système d'exploitation Windows hôte. Stockez chaque fichier VHD sur une partition distincte de celle du système d'exploitation et de tout autre fichier VHD. La configuration idéale consiste à stocker chaque fichier VHD sur un disque physique distinct.  

  Le système de disque physique hôte doit également satisfaire **au moins un** des critères suivants pour satisfaire les besoins de la charge de travail virtualisée d’intégrité des données :  

   - Le système utilise des disques de classe du serveur (SCSI, Fibre Channel).  
   - Le système permet de s’assurer que les disques sont connectés à un batterie de secours mise en cache adaptateur de bus hôte (HBA).  
   - Le système utilise un contrôleur de stockage (par exemple, un système RAID) en tant que le périphérique de stockage.  
   - Le système permet de s’assurer que l’alimentation sur le disque est protégée par un onduleur (UPS).  
   - Le système permet de s’assurer que la fonctionnalité de cache d’écriture du disque est désactivée.  

- **VHD fixe et disques pass-through**. Plusieurs méthodes permettent de configurer le stockage pour les ordinateurs virtuels. En cas d'utilisation de fichiers VHD, les VHD de taille fixe sont plus efficaces que les VHD dynamiques, car leur mémoire est allouée au moment de leur création. Les disques pass-through, que les ordinateurs virtuels peuvent utiliser pour accéder aux supports de stockage physiques, sont davantage optimisés en termes de performances. Il s'agit essentiellement de disques physiques ou de numéros d'unité logique (LUN) rattachés à un ordinateur virtuel. Ces disques ne prennent pas en charge la fonction d'instantanés. Les disques pass-through constituent par conséquent la configuration optimale, étant donné que l'utilisation d'instantanés est déconseillée avec les contrôleurs de domaine.  

Pour réduire le risque d’altération des données Active Directory, utilisez des contrôleurs SCSI virtuels :

   - Utilisez des lecteurs SCSI (plutôt que des disques IDE/ATA) sur les serveurs Hyper-V qui hébergent des contrôleurs de domaine virtuels. Si vous ne pouvez pas utiliser de lecteurs SCSI, assurez-vous que le cache en écriture est désactivé sur les lecteurs ATA/IDE qui hébergent des contrôleurs de domaine virtuels. Pour plus d’informations, consultez [Event ID 1539 intégrité de la base de données](https://go.microsoft.com/fwlink/?linkid=162419).
   - Pour garantir la durabilité d’Active Directory écrit, la base de données Active Directory, journaux et SYSVOL doit être placé sur un disque virtuel SCSI. Les disques SCSI virtuels prennent en charge forcé Unit Access (FUA). FUA garantit que le système d’exploitation écrit et lit les données directement à partir du support en ignorant tous les mécanismes de mise en cache.  

## <a name="operational-considerations-for-virtualized-domain-controllers"></a>Observations sur le fonctionnement des contrôleurs de domaine virtualisés

Certaines restrictions opérationnelles applicables aux contrôleurs de domaine exécutés sur des ordinateurs virtuels ne s'appliquent pas aux contrôleurs de domaine exécutés sur des systèmes physiques. Lorsque vous utilisez un contrôleur de domaine virtualisé, n'utilisez pas les fonctions suivantes des logiciels de virtualisation :

   - L'état enregistré d'un contrôleur de domaine ne doit pas être mis en pause, arrêté ou stocké dans un ordinateur virtuel pour une durée supérieure à la durée de vie de désactivation de la forêt, puis sorti du mode pause ou enregistré. Cela risque de perturber la réplication. Pour savoir comment déterminer la durée de vie de désactivation de la forêt, voir [déterminer la durée de vie de désactivation de la forêt](https://go.microsoft.com/fwlink/?linkid=137177).  
   - Les disques durs virtuels (VHD) ne doivent être ni copiés, ni clonés. Même avec les dispositifs de protection en place pour la machine virtuelle invitée, les disques durs virtuels individuels peuvent toujours être copiées et entraînent la restauration USN.
   - Aucun instantané de contrôleur de domaine virtuel ne doit être créé ou utilisé. Il est techniquement possible avec Windows Server 2012 et versions ultérieures, il n’est pas un remplacement pour une bonne stratégie de sauvegarde. Il existe plusieurs raisons de captures instantanées de contrôleur de domaine ou restaure les captures instantanées.
   - Aucun disque de différenciation VHD ne doit être utilisé sur un ordinateur virtuel configuré en tant que contrôleur de domaine. Il deviendrait trop aisé de revenir à une version antérieure, et les performances s'en trouveraient réduites.  
   - La fonction d'exportation ne doit pas être utilisée sur un ordinateur virtuel qui exécute un contrôleur de domaine.  
   - La restauration d'un contrôleur de domaine et du contenu d'une base de données Active Directory ne doit être effectuée qu'à partir d'une sauvegarde prise en charge. Pour plus d’informations, consultez [sauvegarde et les considérations relatives à la restauration pour les contrôleurs de domaine virtualisés](#backup-and-restore-practices-to-avoid).  

Ces recommandations visent à éviter l'éventualité d'une restauration d'un numéro de séquence de mise à jour (USN). Pour plus d’informations sur la restauration USN, consultez USN et restauration USN.

## <a name="backup-and-restore-considerations-for-virtualized-domain-controllers"></a>Observations sur la sauvegarde et la restauration des contrôleurs de domaine virtualisés

La sauvegarde de contrôleurs de domaine est une obligation indispensable pour tout environnement. Les sauvegardes protègent les systèmes d'une perte de données en cas de défaillance d'un contrôleur de domaine ou d'une erreur de l'administrateur. Lorsqu'un tel événement se produit, il est nécessaire de restaurer un état système du contrôleur de domaine antérieur à la panne ou à l'erreur. Pour restaurer un état sain d'un contrôleur de domaine, la méthode prise en charge consiste à exécuter une application de sauvegarde compatible avec Active Directory, telle que la Sauvegarde de Windows Server, de façon à restaurer une sauvegarde de l'état du système générée depuis l'installation actuelle du contrôleur de domaine. Pour plus d’informations sur l’utilisation de la sauvegarde de Windows Server avec les Services de domaine Active Directory (AD DS), consultez le [AD DS Guide de sauvegarde et récupération pas à pas](https://go.microsoft.com/fwlink/?LinkId=138501).

Avec la technologie de virtualisation, certaines conditions liées aux opérations de restauration Active Directory changent de manière significative. Par exemple, si vous restaurez un contrôleur de domaine à l'aide d'une copie du fichier de disque dur virtuel (VHD), vous contournez l'étape cruciale consistant à mettre à jour la version de la base de données d'un contrôleur de domaine à l'issue de sa restauration. La réplication continue avec des numéros de suivi erronés, produisant une base de données incohérente entre les réplicas de contrôleurs de domaine. Dans la plupart des cas, le système de réplication ne détecte pas ce problème et aucune erreur n'est signalée, malgré la présence d'incohérences entre les contrôleurs de domaine.

Il existe un moyen pris en charge pour effectuer la sauvegarde et la restauration d’un contrôleur de domaine virtualisé :

1. Exécutez la Sauvegarde Windows Server dans le système d'exploitation invité.  

Avec Windows Server 2012 et les hôtes Hyper-V plus récente et les invités, vous pouvez tirer des sauvegardes prises en charge des contrôleurs de domaine à l’aide de captures instantanées, invité machine virtuelle d’exportation et importation et également la réplication Hyper-V. Tous ces toutefois ne sont pas une solution parfaitement adaptée pour la création d’un historique de sauvegarde approprié, à l’exception de légères d’exportation de machine virtuelle invité.

Avec Windows Server 2016 Hyper-V est prise en charge des « instantanés de production » où le serveur Hyper-V déclenche une sauvegarde VSS de l’invité et lorsque l’invité est terminé avec l’instantané, l’hôte extrait les disques durs virtuels et les stocke dans l’emplacement de sauvegarde.

Bien que cela fonctionne avec Windows Server 2012 et versions ultérieures, il existe une incompatibilité avec Bitlocker :

- Lorsque vous effectuez une capture d’alignement de VSS, AD souhaite effectuer une tâche de post-instantané pour marquer la base de données comme entrée à partir d’une sauvegarde, ou dans le cas de la préparation d’une source d’IFM pour les RODC, supprimer les informations d’identification à partir de la base de données.
- Lorsque Hyper-V monte le volume convertis en instantanés pour cette tâche, il n’existe aucune fonctionnalité qui serait de déverrouiller le Volume pour un accès non chiffré. Par conséquent, le moteur de base de données AD ne parvient pas accéder à la base de données et finit par échoue l’instantané.

> [!NOTE]
> Le projet de machine virtuelle protégé mentionné a précédemment un hôte Hyper-V piloté par sauvegarde comme un objectif de protection des données maximale de la machine virtuelle invitée.

## <a name="backup-and-restore-practices-to-avoid"></a>Pratiques à éviter en matière de sauvegarde et de restauration

Comme indiqué plus haut, certaines restrictions applicables aux contrôleurs de domaine exécutés sur des ordinateurs virtuels ne s'appliquent pas aux contrôleurs de domaine exécutés sur des systèmes physiques. Lorsque vous sauvegardez ou que vous restaurez un contrôleur de domaine virtuel, n'utilisez pas les fonctions suivantes des logiciels de virtualisation :

   - Ne choisissez pas de copier ou cloner les fichiers VHD des contrôleurs de domaine plutôt que de réaliser des sauvegardes régulières. Un fichier VHD copié ou cloné devient en effet obsolète. Ensuite, si le disque dur virtuel est démarré en mode normal, vous rencontrerez une restauration USN. Il est préférable d'effectuer des sauvegardes prises en charge par les services de domaine Active Directory (AD DS), par exemple au moyen de la fonction de sauvegarde Windows Server.  
   - N'utilisez pas la fonction d'instantané en tant que sauvegarde pour restaurer un ordinateur virtuel configuré comme contrôleur de domaine. Des problèmes seront produit avec la réplication lorsque vous restaurez l’ordinateur virtuel à un état antérieur avec Windows Server 2008 R2 et versions antérieures. Pour plus d’informations, consultez [USN et restauration USN](#usn-and-usn-rollback). Bien que l'utilisation d'un instantané pour restaurer un contrôleur de domaine en lecture seule (RODC) ne provoque pas de problèmes de réplication, cette méthode de restauration reste déconseillée.  

## <a name="restoring-a-virtual-domain-controller"></a>Restauration d'un contrôleur de domaine virtuel

Pour être en mesure de restaurer un contrôleur de domaine défaillant, vous devez sauvegarder l'état du système régulièrement. L'état du système inclut les données et fichiers journaux d'Active Directory, le registre, le volume système (dossier SYSVOL), ainsi que divers éléments du système d'exploitation. Cette exigence est le même pour les contrôleurs de domaine physiques et virtuels. Les procédures de restauration de l'état du système que réalisent les applications de sauvegarde compatibles avec Active Directory visent à garantir la cohérence des bases de données Active Directory locales et répliquées à l'issue d'un processus de restauration. Elles signalent en outre aux partenaires de réplication les réinitialisations d'ID d'invocation. Cependant, les administrateurs qui utilisent des environnements d'hébergement virtuels et des applications d'acquisition d'images de disque ou de système d'exploitation sont capables de contourner les contrôles et les validations qui ont généralement lieu lorsque l'état système d'un contrôleur de domaine est restauré.

Lorsque l'ordinateur virtuel d'un contrôleur de domaine tombe en panne, sans entraîner la restauration d'un numéro de séquence de mise à jour (USN), deux situations sont prises en charge pour la restauration de l'ordinateur virtuel :

   - S'il existe une sauvegarde des données sur l'état du système, valide et antérieure à la panne, vous pouvez restaurer l'état du système au moyen de l'option de restauration de l'utilitaire qui a servi à créer la sauvegarde. La sauvegarde des données sur l'état du système doit avoir été créée à l'aide d'un utilitaire compatible avec Active Directory dans la limite de la durée de vie de désactivation, qui par défaut est de 180 jours maximum. Sauvegardez vos contrôleurs de domaine au moins deux fois par durée de vie de désactivation. Pour obtenir des instructions sur la façon de déterminer la durée de vie de désactivation de votre forêt, consultez [déterminer la durée de vie de désactivation de la forêt](https://go.microsoft.com/fwlink/?linkid=137177).  
   - S'il existe une copie du fichier VHD mais qu'aucune sauvegarde de l'état du système n'est disponible, vous pouvez supprimer l'ordinateur virtuel existant. Restaurez-le ensuite à l'aide d'une copie antérieure du fichier VHD, sans oublier de le démarrer en mode de restauration des services d'annuaire (DSRM), et configurez le registre de manière appropriée en suivant la procédure de la section ci-après. Redémarrez ensuite le contrôleur de domaine en mode normal.

Suivez le processus de l'illustration suivante pour déterminer le meilleur moyen de restaurer le contrôleur de domaine virtualisé.

![](media/virtualized-domain-controller-architecture/Dd363553.85c97481-7b95-4705-92a7-006e48bc29d0(WS.10).gif)

Pour les RODC, le processus de restauration et les choix à effectuer sont plus simples.

![](media/virtualized-domain-controller-architecture/Dd363553.4c5c5eda-df95-4c6b-84e0-d84661434e5d(WS.10).gif)

## <a name="restoring-the-system-state-backup-of-a-virtual-domain-controller"></a>Restauration de la sauvegarde de l'état système d'un contrôleur de domaine virtuel

Si vous possédez une sauvegarde valide de l'état système de l'ordinateur virtuel du contrôleur de domaine, il vous suffit pour la restaurer de suivre la procédure de l'outil de sauvegarde qui vous a permis de sauvegarder le fichier VHD.

> [!IMPORTANT]
> Pour restaurer correctement le contrôleur de domaine, démarrez-le en mode de restauration des services d'annuaire (DSRM). Ne le laissez pas démarrer en mode normal. Si vous n'êtes pas parvenu à choisir le mode DSRM lors du démarrage du système, éteignez l'ordinateur virtuel du contrôleur de domaine avant qu'il ne démarre entièrement en mode normal. Il est primordial de démarrer le contrôleur de domaine en mode DSRM, car un démarrage en mode normal entraînerait une incrémentation de ses numéros USN, même s'il était déconnecté du réseau. Pour plus d’informations sur la restauration USN, consultez USN et restauration USN. 

## <a name="to-restore-the-system-state-backup-of-a-virtual-domain-controller"></a>Pour restaurer la sauvegarde de l'état système d'un contrôleur de domaine virtuel

1. Démarrez l'ordinateur virtuel du contrôleur de domaine, puis appuyez sur F5 pour accéder à l'écran du Gestionnaire de démarrage Windows. Si vous êtes invité à entrer des informations d'identification de connexion, cliquez immédiatement sur le bouton **Pause** de l'ordinateur virtuel afin d'interrompre le démarrage. Entrez alors vos informations d'identification de connexion et cliquez sur le bouton **Lecture** de l'ordinateur virtuel. Cliquez dans la fenêtre de l'ordinateur virtuel, puis appuyez sur F5.

   Si l'écran du Gestionnaire de démarrage Windows ne s'affiche pas et que le contrôleur de domaine démarre en mode normal, éteignez l'ordinateur virtuel afin d'éviter que le démarrage n'aboutisse. Recommencez cette étape autant de fois que nécessaire, jusqu'à ce que vous parveniez à accéder à l'écran du Gestionnaire de démarrage Windows. Il n'est pas possible d'accéder au mode DSRM à partir du menu Récupération d'erreurs Windows. Par conséquent, éteignez l'ordinateur virtuel et recommencez l'opération si ce menu apparaît.

2. Dans l'écran du Gestionnaire de démarrage Windows, appuyez sur F8 pour accéder aux options de démarrage avancées.
3. Dans l'écran **Options de démarrage avancées**, sélectionnez **Mode restauration des services d'annuaire** et appuyez sur Entrée. Le contrôleur de domaine démarre alors en mode de restauration des services d'annuaire.
4. Utilisez la méthode de restauration appropriée de l'outil qui vous a servi à créer la sauvegarde de l'état du système. Si vous avez utilisé la sauvegarde Windows Server, consultez [effectuer une restauration ne faisant pas autorité des services AD DS](https://go.microsoft.com/fwlink/?linkid=132637).

## <a name="restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available"></a>Restauration d'un contrôleur de domaine virtuel en l'absence d'une sauvegarde de l'état du système

Si vous ne possédez aucune sauvegarde des données sur l'état du système antérieure à la panne de l'ordinateur virtuel, vous pouvez utiliser un fichier VHD précédent pour restaurer un contrôleur de domaine exécuté sur un ordinateur virtuel. Si possible, faites une copie du disque dur virtuel, afin de pouvoir faire une nouvelle tentative avec le disque dur copié si vous rencontrez un problème pendant la procédure ou si vous manquez une étape.

> [!IMPORTANT]
> - Vous ne devez pas envisager de suivre la procédure suivante en remplacement des sauvegardes régulièrement planifiées.
> - **Les restaurations effectuées avec la procédure suivante ne sont pas pris en charge par Microsoft et doivent être utilisées uniquement lorsqu’il n’existe aucune alternative.**
> - Ne recourez pas à cette méthode si la copie du disque dur virtuel que vous êtes sur le point de restaurer a été démarrée en mode normal par un ordinateur virtuel quelconque.

## <a name="to-restore-a-previous-version-of-a-virtual-domain-controller-vhd-without-system-state-data-backup"></a>Pour restaurer la version antérieure d'un VHD de contrôleur de domaine virtuel en l'absence de sauvegarde des données sur l'état du système

1. À l'aide du VHD précédent, démarrez le contrôleur de domaine virtuel en mode DSRM, comme décrit dans la section précédente. Ne le laissez pas démarrer en mode normal. Si l'écran du Gestionnaire de démarrage Windows ne s'affiche pas et que le contrôleur de domaine démarre en mode normal, éteignez l'ordinateur virtuel afin d'éviter que le démarrage n'aboutisse. Reportez-vous à la section précédente pour plus de détails sur le démarrage en mode DSRM.
2. Ouvrez l’Éditeur du Registre. Pour ce faire, cliquez sur **Démarrer** pus sur **Exécuter**, tapez **regedit**, puis cliquez sur OK. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**. Dans l'Éditeur du Registre, développez le chemin d'accès suivant : **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\NTDS\\Parameters**. Recherchez une valeur nommée **DSA Previous Restore Count**. Si cette valeur existe, notez-en le paramètre. Si elle n'existe pas, le paramètre par défaut est utilisé, c'est-à-dire zéro. N'ajoutez pas de valeur si vous n'en voyez pas ici.
3. Cliquez avec le bouton droit sur la clé **Parameters**, cliquez sur **Nouveau**, puis cliquez sur **Valeur DWORD 32 bits**.
4. Tapez un nouveau nom, comme **Database restored from backup**, puis appuyez sur Entrée.
5. Double-cliquez sur la valeur que vous venez de créer afin d'ouvrir la boîte de dialogue **Modifier la valeur DWORD 32 bits**, puis tapez **1** dans la zone de texte **Données de la valeur**. Le **base de données restaurée à partir de l’entrée de sauvegarde** option est disponible sur les contrôleurs de domaine qui exécutent Windows 2000 Server avec Service Pack 4 (SP4), Windows Server 2003 avec les mises à jour qui sont inclus dans [comment détecter et récupérer à partir d’une restauration USN dans Windows Server 2003, Windows Server 2008 et Windows Server 2008 R2](https://go.microsoft.com/fwlink/?linkid=137182) dans la Base de connaissances Microsoft installé et Windows Server 2008.
6. Redémarrez le contrôleur de domaine en mode normal.
7. Ouvrez ensuite l'Observateur d'événements. Pour ouvrir l'Observateur d'événements, cliquez sur **Démarrer**, puis sur **Panneau de configuration**, double-cliquez sur **Outils d'administration**, puis sur **Observateur d'événements**.
8. Développez **Journaux des applications et des services**, puis cliquez sur le journal **Services d'annuaire**. Vérifiez que les événements apparaissent dans le volet d'informations.
9. Cliquez avec le bouton droit sur le journal **Services d'annuaire**, puis cliquez sur **Rechercher**. Dans **Rechercher**, tapez **1109**, puis cliquez sur **Suivant**.
10. La recherche doit renvoyer au moins une entré pour l'ID d'événement 1109. Si cette entrée n'apparaît pas, passez à l'étape suivante. Sinon, double-cliquez sur cette entrée et passez en revue le texte confirmant la mise à jour de l'attribut InvocationID:

    ```
    Active Directory has been restored from backup media, or has been configured to host an application partition. 
    The invocationID attribute for this directory server has been changed. 
    The highest update sequence number at the time the backup was created is <time>

    InvocationID attribute (old value):<Previous InvocationID value>
    InvocationID attribute (new value):<New InvocationID value>
    Update sequence number:<USN>

    The InvocationID is changed when a directory server is restored from backup media or is configured to host a writeable application directory partition.
    ```

11. Fermez l’Observateur d’événements.
12. Dans l'Éditeur du Registre, vérifiez que la valeur de **DSA Previous Restore Count** est égale à la valeur précédente, plus un. Si cette valeur est incorrecte et que vous ne parvenez à trouver aucune entrée pour l'ID d'événement 1109 dans l'Observateur d'événements, vérifiez que les Service Packs du contrôleur de domaine sont actuels. Vous ne pouvez pas réessayer cette procédure sur le même disque dur virtuel. Vous pouvez réessayer sur une copie du disque dur virtuel ou sur un disque dur virtuel différent qui n'a pas été démarré en mode normal, en recommençant à l'étape 1.
13. Fermez l’Éditeur du Registre.

## <a name="usn-and-usn-rollback"></a>USN et restauration USN

Cette section décrit les problèmes de réplication qui peuvent se produire suite à une restauration incorrecte de la base de données Active Directory avec une version antérieure d’une machine virtuelle. Pour plus d’informations sur le processus de réplication Active Directory, consultez [les Concepts de réplication Active Directory](../replication/active-directory-replication-concepts.md)

## <a name="usns"></a>USN

Les services de domaine Active Directory (AD DS) ont recours à des numéros de séquence de mise à jour (USN) pour assurer le suivi de la réplication des données entre les contrôleurs de domaine. Chaque fois que les données de l'annuaire sont modifiées, le numéro USN est incrémenté de façon à indiquer qu'un changement a eu lieu.

Pour chaque partition d’annuaire qui stocke un contrôleur de domaine de destination, les numéros USN est utilisés pour effectuer le suivi de la dernière mise à jour sa base de données, ainsi que l’état de chaque contrôleur de domaine qui stocke un réplica d’un contrôleur de domaine a présenté le partition d’annuaire. Lorsque les contrôleurs de domaine répliquent les modifications entre eux, ils interrogent leurs partenaires de réplication des modifications USN est supérieurs à l’USN de la dernière modification reçu par le contrôleur de domaine à partir de chaque partenaire.

Les deux tables de métadonnées de réplication suivantes contiennent des USN. Les contrôleurs de domaine sources et cibles s'en servent pour filtrer les mises à jour requises par les contrôleurs de domaine de destination.

1. **Vecteur de mise à jour**: Une table qui tient à jour le contrôleur de domaine de destination pour les mises à jour à l’origine qui sont reçus à partir de tous les contrôleurs de domaine source de suivi. Lorsqu'un contrôleur de domaine cible demande des modifications pour une partition d'annuaire, il fournit son vecteur de mise à jour au contrôleur de domaine source. Le contrôleur de domaine source utilise ensuite cette valeur pour filtrer les mises à jour qu’elle envoie au contrôleur de domaine de destination. Le contrôleur de domaine source envoie son vecteur de mise à la destination à la fin d’un cycle de réplication réussi afin de garantir que le contrôleur de domaine de destination sache qu’il a synchronisé avec chaque contrôleur de domaine des mises à jour et les mises à jour sont au même niveau que la source.  
2. **Borne haute** : Une valeur qui tient à jour le contrôleur de domaine de destination pour effectuer le suivi des modifications les plus récentes qu’il a reçus à partir d’un contrôleur de domaine source spécifique pour une partition spécifique. La borne haute empêche le contrôleur de domaine source d’envoyer les modifications par le contrôleur de domaine de destination a déjà reçu à partir de celui-ci.  

## <a name="directory-database-identity"></a>Identité de la base de données de l'annuaire

Outre les USN, les contrôleurs de domaine conservent une trace de la base de données d'annuaire des partenaires de réplication sources. L'identité de la base de données d'annuaire exécutée sur le serveur est gérée séparément de l'identité de l'objet serveur lui-même. L’identité de base de données d’annuaire sur chaque contrôleur de domaine est stockée dans le **invocationID** attribut de l’objet Paramètres NTDS, qui se trouve sous le chemin d’accès suivant Lightweight Directory Access Protocol (LDAP) : cn = NTDS Paramètres, cn = ServerName, cn = Servers, cn =*SiteName*, cn = Sites, cn = Configuration, dc =*DomaineRacineForêt*. L'identité de l'objet serveur est stockée dans l'attribut **objectGUID** de l'objet Paramètres NTDS. Cette identité ne change jamais. Toutefois, l’identité des base de données des modifications d’annuaire lorsqu’une procédure de restauration d’état système se produit sur le serveur, ou lors de l’ajout d’une partition d’annuaire d’application, puis supprimé et rajouté ultérieurement à partir du serveur. (autre scénario : quand une instance de Hyper-v déclenche ses enregistreurs VSS sur une partition contenant le disque dur virtuel d’un contrôleur de domaine virtuel, l’invité à son tour déclenche ses propres enregistreurs VSS (le même mécanisme que celui utilisé par la sauvegarde/restauration ci-dessus) résultant dans un autre moyen par lequel l’attribut invocationID est réinitialisation)

De fait, l'attribut **invocationID** fait référence à un ensemble de mises à jour sur un contrôleur de domaine avec une version spécifique de la base de données d'annuaire. Le vecteur de mise à jour et la borne haute utilisation de tables de marquer le **invocationID** et GUID de contrôleur de domaine respectivement pour ce domaine contrôleurs savoir de quelle copie de l’annuaire Active Directory de base de données les informations de réplication est à venir.

L'attribut **invocationID** est un identificateur global unique (GUID) visible dans la partie supérieure de la sortie après l'exécution de la commande **repadmin /showrepl**. Le texte suivant représente un exemple de sortie de cette commande :

   ```
   Repadmin: running command /showrepl against full DC local host
   Default-First-Site-Name\VDC1
   DSA Options: IS_GC
   DSA object GUID: 966651f3-a544-461f-9f2c-c30c91d17818
   DSA invocationID: b0d9208b-8eb6-4205-863d-d50801b325a9
   ```

Lorsque les services AD DS est correctement restauré sur un contrôleur de domaine, le **invocationID** est réinitialisé. Suite à cette modification, vous rencontrerez une augmentation du trafic de réplication – dont la durée est relative à la taille de la partition en cours de réplication

Par exemple, supposons que VDC1 et DC2 sont deux contrôleurs de domaine appartenant au même domaine. La figure suivante illustre la manière dont DC2 reconnaît VDC1 lorsque la valeur de l'attribut invocationID est réinitialisée dans une situation de restauration normale.

![](media/virtualized-domain-controller-architecture/Dd363553.ca71fc12-b484-47fb-991c-5a0b7f516366(WS.10).gif)

## <a name="usn-rollback"></a>Restauration USN

La restauration USN se produit lorsque les mises à jour d'USN normales sont contournées et qu'un contrôleur de domaine tente d'utiliser un USN inférieur à celui de sa dernière mise à jour. La restauration USN est détectée et réplication va être arrêtée avant la création de divergence dans la forêt, dans la plupart des cas. 

Plusieurs facteurs sont susceptibles de provoquer une restauration USN. Par exemple, si vous utilisez d'anciens fichiers de disque dur virtuel (VHD) ou si vous procédez à la réalisation d'une conversion P2V (ordinateur physique en ordinateur virtuel) sans vous assurer que l'ordinateur physique reste hors connexion de façon permanente après la conversion. Prenez les précautions suivantes pour éviter les restaurations USN :

   - Lors de le n'exécution pas Windows Server 2012 ou ultérieure, ne prendre ou utiliser un instantané d’un ordinateur virtuel de contrôleur de domaine.
   - Ne copiez pas le fichier VHD du contrôleur de domaine.  
   - Lors de le n'exécution pas Windows Server 2012 ou version ultérieure, n’exportez pas la machine virtuelle qui exécute un contrôleur de domaine.  
   - Ne restaurez pas un contrôleur de domaine et ne tentez pas de restaurer le contenu d'une base de données Active Directory autrement qu'à l'aide d'une solution de sauvegarde prise en charge, telle que la Sauvegarde Windows Server.  

Dans certains cas, la restauration USN passe inaperçue. Dans d'autres cas, elle peut provoquer des erreurs dans d'autres applications. Il est alors nécessaire d'identifier l'étendue du problème et d'y remédier sans tarder. Pour plus d’informations sur la suppression des objets en attente qui peuvent se produire suite à la restauration USN, consultez [objets Active Directory obsolètes génèrent l’ID d’événement 1988 dans Windows Server 2003](https://go.microsoft.com/fwlink/?linkid=137185) dans la Base de connaissances Microsoft.

## <a name="usn-rollback-detection"></a>Détection des restaurations USN

Dans la majorité des cas, les restaurations USN qui se produisent en raison d'une procédure de restauration incorrecte, sans que l'attribut **invocationID** soit réinitialisé, sont détectées. Windows Server 2008 fournit des moyens de protection contre les réplications incorrectes dues à une opération de restauration de contrôleur de domaine inadéquate. Ces protections sont déclenchées par le fait qu'une restauration inappropriée génère des USN inférieurs représentant des modifications que les partenaires de réplication ont déjà reçues.

Dans Windows Server 2008 et Windows Server 2003 SP1, lorsqu'un contrôleur de domaine de destination demande des modifications en utilisant un USN déjà utilisé, la réponse provenant de son partenaire de réplication source laisse supposer que ses métadonnées de réplication sont obsolètes. Cela indique qu'un état antérieur de la base de données Active Directory du contrôleur de domaine source a été restauré. Par exemple, une version antérieure du fichier VHD d'un ordinateur virtuel a été restaurée. Dans ce cas, le contrôleur de domaine de destination lance les mesures de quarantaine suivantes sur le contrôleur de domaine qui a été identifié comme ayant fait l'objet d'une restauration inappropriée :

   - AD DS interrompt le service Net Logon, empêchant la modification des mots de passe des comptes utilisateur et ordinateur. Cela évite que ces modifications soient perdues si elles se produisent après une restauration inadéquate.  
   - AD DS désactive la réplication Active Directory en entrée et en sortie.  
   - AD DS génère l'ID d'événement 2095 dans le journal d'événements du service d'annuaire, de façon à signaler la condition.  

L'illustration suivante représente la séquence d'événements qui se produit lorsqu'une restauration USN est détectée sur VDC2, le contrôleur de domaine de destination exécuté sur un ordinateur virtuel. Sur cette illustration, la détection de la restauration USN se produit sur VDC2 lorsqu'un partenaire de réplication détecte que VDC2 a envoyé une valeur USN de mise à jour déjà vue par le contrôleur de domaine de destination. Cela signifie en effet qu'une version antérieure erronée de la base de données de VDC2 a été restaurée.

![](media/virtualized-domain-controller-architecture/Dd363553.373b0504-43fc-40d0-9908-13fdeb7b3f14(WS.10).gif)

Si le journal d'événements du service d'annuaire signale l'ID d'événement 2095, effectuez immédiatement les opérations ci-après.

## <a name="to-resolve-event-id2095"></a>Pour résoudre l'ID d'événement 2095

1. Isoler la machine virtuelle qui a enregistré l’erreur à partir du réseau.
2. Tentez de déterminer si des modifications ont été issues de ce contrôleur de domaine et propagées sur d'autres contrôleurs de domaine. Si l'événement est dû à l'instantané ou à la copie d'un ordinateur virtuel en cours de démarrage, tentez de déterminer l'heure à laquelle a eu lieu la restauration USN. Vous pouvez ensuite vérifier les partenaires de réplication de ce contrôleur de domaine en vue de déterminer si une réplication a eu lieu depuis.

   Vous pouvez pour cela exécuter l'outil Repadmin. Pour plus d’informations sur l’utilisation de Repadmin, consultez [surveillance et dépannage de réplication Active Directory à l’aide de Repadmin](https://go.microsoft.com/fwlink/?linkid=122830). Si vous n’êtes pas en mesure de déterminer vous-même, contactez [Support Microsoft](https://support.microsoft.com) pour obtenir une assistance.

3. Procédez à une rétrogradation forcée du contrôleur de domaine. Cela implique le nettoyage de ses métadonnées ainsi que la prise des rôles de maître d'opérations (également appelés « rôles d'opérations à maître unique flottant » ou « FSMO »). Pour plus d’informations, consultez la section « Récupération à partir de la restauration USN » de [comment détecter et à récupérer à partir d’une restauration USN dans Windows Server 2003, Windows Server 2008 et Windows Server 2008 R2](https://go.microsoft.com/fwlink/?linkid=137182) dans la Base de connaissances Microsoft.
4. Supprimez tous les anciens fichiers VHD du contrôleur de domaine.

## <a name="undetected-usn-rollback"></a>Restauration USN non détectée

La restauration USN risque de ne pas être détectée dans l'un des deux cas de figure suivants :

1. Le fichier VHD est joint à d'autres ordinateurs virtuels exécutés simultanément dans plusieurs emplacements.  
2. Le numéro USN du contrôleur de domaine restauré a augmenté et est supérieur au dernier USN que l'autre contrôleur de domaine a reçu.  

Dans le premier cas, la réplication peut avoir lieu entre les autres contrôleurs de domaine et l'un des ordinateurs virtuels, et des modifications peuvent être effectuées dans l'un des ordinateurs virtuels sans être répliquées vers l'autre. Cette divergence dans la forêt est difficile à détecter et entraîne des réactions imprévisibles de l'annuaire. Une telle situation peut se produire à la suite d'une migration P2V si les ordinateurs physique et virtuel sont exécutés sur le même réseau. Elle peut également avoir lieu si plusieurs contrôleurs de domaine virtuels sont créés à partir du même contrôleur de domaine physique, puis exécutés sur le même réseau.

Dans le second cas de figure, une série de numéros USN s'applique à deux ensembles de modifications distincts. Cette situation peut se produire sur de longues périodes sans être détectée. Dès qu'un objet est créé pendant cette période, un objet en attente est détecté et signalé en tant qu'ID d'événement 1988 dans l'Observateur d'événements. L'illustration suivante montre une restauration USN non détectée dans ce type de circonstance.

![](media/virtualized-domain-controller-architecture/Dd363553.63565fe0-d970-4b4e-b5f3-9c76bc77e2d4(WS.10).gif)

## <a name="read-only-domain-controllers"></a>Contrôleurs de domaine en lecture seule

Ces contrôleurs sont des contrôleurs de domaine qui hébergent des copies en lecture seule des partitions dans une base de données Active Directory. Les RODC contribuent à éviter la plupart des problèmes de restauration USN, car ils ne répliquent pas les modifications sur les autres contrôleurs de domaine. Toutefois, lorsqu'un RODC est répliqué à partir d'un contrôleur de domaine inscriptible ayant subi une restauration USN, ce RODC est également touché.

Il est déconseillé de restaurer un RODC à l'aide d'un instantané. Recourez plutôt à une application de sauvegarde compatible avec Active Directory. De plus, prenez garde, comme c'est le cas avec les contrôleurs de domaine inscriptibles, de ne pas laisser un RODC hors connexion pendant une durée supérieure à la durée de vie de désactivation. Cette condition pourrait en effet entraîner la présence d'objets en attente sur le RODC.

Pour plus d’informations sur les RODC, voir la [Guide de déploiement et de planification de contrôleur de domaine en lecture seule](../../deploy/rodc/read-only-domain-controller-updates.md).
