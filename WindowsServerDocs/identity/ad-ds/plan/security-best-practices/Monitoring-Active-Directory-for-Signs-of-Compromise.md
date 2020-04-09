---
ms.assetid: a7ef2fba-b05c-4be2-93b2-b9456244c3ad
title: Surveillance des signes de compromission d'Active Directory
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 1d00ab702ab6b4ff4307f96f9e266a1cb3420197
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821142"
---
# <a name="monitoring-active-directory-for-signs-of-compromise"></a>Surveillance des signes de compromission d'Active Directory

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Numéro de loi cinq : externes vigilance est le prix de la sécurité.* - [10 lois immuables de l’administration de la sécurité](https://technet.microsoft.com/library/cc722488.aspx)  
  
Un système de surveillance du journal des événements solide est un élément essentiel de toute conception de Active Directory sécurisée. De nombreuses compromissions de la sécurité de l’ordinateur peuvent être découvertes tôt dans le cas où les victimes d’une surveillance et d’alertes appropriées du journal des événements. Les rapports indépendants ont pris en charge cette conclusion à long terme. Par exemple, le [rapport de violation de données 2009 Verizon](http://www.verizonbusiness.com/resources/security/reports/2009_databreach_rp.pdf) indique les États suivants :  
  
«L’inefficacité apparente de la surveillance des événements et de l’analyse des journaux continue d’être un peu plus d’un Enigma. L’opportunité de la détection est là ; les investigateurs ont noté que 66% des victimes avaient suffisamment d’éléments de preuve disponibles dans leurs journaux pour découvrir la violation de ces ressources.»  
  
Ce manque de surveillance des journaux des événements actifs reste une faille cohérente dans les plans de défense de la sécurité des entreprises. Le [rapport de divulgation de données 2012 Verizon](http://www.verizonbusiness.com/resources/reports/rp_data-breach-investigations-report-2012_en_xg.pdf) a détecté que, même si 85% des violations ont duré plusieurs semaines, 84% des victimes avaient prouvé la violation dans leurs journaux d’événements.  
  
## <a name="windows-audit-policy"></a>Stratégie d’audit Windows

Vous trouverez ci-dessous des liens vers le blog du support Microsoft Official Enterprise. Le contenu de ces blogs fournit des conseils, des conseils et des recommandations sur l’audit qui vous aideront à améliorer la sécurité de votre infrastructure de Active Directory et vous serez une ressource précieuse lors de la conception d’une stratégie d’audit.  
  
* [L’audit d’accès global aux objets est](https://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) un mécanisme de contrôle appelé configuration avancée de la stratégie d’audit qui a été ajouté à Windows 7 et windows Server 2008 R2, qui vous permet de définir les types de données que vous souhaitez auditer facilement, sans jongler avec les scripts et auditpol. exe.  
* [Présentation des modifications d’audit dans windows 2008](https://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx) : présente les modifications apportées aux audits dans windows Server 2008.  
* [Astuces d’audit intéressantes dans Vista et 2008](https://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx) : explique les fonctionnalités d’audit intéressantes de Windows Vista et de windows Server 2008 qui peuvent être utilisées pour résoudre des problèmes ou voir ce qui se passe dans votre environnement.  
* [One-Stop pour l’audit dans Windows server 2008 et Windows Vista](https://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx) : contient une compilation des fonctionnalités d’audit et des informations contenues dans windows Server 2008 et Windows Vista.  
  
Les liens suivants fournissent des informations sur les améliorations apportées à l’audit Windows dans Windows 8 et Windows Server 2012, ainsi que des informations sur l’audit d’AD DS dans Windows Server 2008.  
  
* [Nouveautés de l’audit de sécurité](https://technet.microsoft.com/library/hh849638.aspx) -fournit une vue d’ensemble des nouvelles fonctionnalités d’audit de sécurité dans Windows 8 et windows server 2012.  
* [Guide pas à pas d’audit de AD DS](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx) : décrit la nouvelle fonctionnalité d’audit Active Directory Domain Services (AD DS) dans Windows Server 2008. Il fournit également des procédures permettant d’implémenter cette nouvelle fonctionnalité.  
  
### <a name="windows-audit-categories"></a>Catégories d’audit Windows

Avant Windows Vista et Windows Server 2008, Windows avait seulement neuf catégories de stratégie d’audit des journaux des événements :  
  
* Événements d’ouverture de session de compte  
* Gestion du compte  
* Accès au service d’annuaire  
* Événements de connexion  
* Accès aux objets  
* Modification de la stratégie  
* Utilisation des privilèges  
* Suivi des processus  
* Événements système  
  
Ces neuf catégories d’audit traditionnelles comprennent une stratégie d’audit. Chaque catégorie de stratégie d’audit peut être activée pour les événements de réussite, d’échec ou de réussite et d’échec. Leurs descriptions sont incluses dans la section suivante.  
  
#### <a name="audit-policy-category-descriptions"></a>Descriptions des catégories de stratégie d’audit  
Les catégories de stratégie d’audit activent les types de messages du journal des événements suivants.  
  
##### <a name="audit-account-logon-events"></a>Auditer les événements de connexion au compte  
Signale chaque instance d’un principal de sécurité (par exemple, un compte d’utilisateur, d’ordinateur ou de service) qui se connecte à ou se déconnecte d’un ordinateur dans lequel un autre ordinateur est utilisé pour valider le compte. Les événements d’ouverture de session de compte sont générés lorsqu’un compte de principal de sécurité de domaine est authentifié sur un contrôleur de domaine. L’authentification d’un utilisateur local sur un ordinateur local génère un événement d’ouverture de session qui est consigné dans le journal de sécurité local. Aucun événement de déconnexion de compte n’est enregistré.  
  
Cette catégorie génère beaucoup de « bruit », car Windows dispose constamment de comptes ouvrant une session sur les ordinateurs locaux et distants au cours d’une activité normale. Toutefois, tout plan de sécurité doit inclure la réussite et l’échec de cette catégorie d’audit.  
  
##### <a name="audit-account-management"></a>Auditer la gestion des comptes  
Ce paramètre d’audit détermine s’il faut effectuer le suivi de la gestion des utilisateurs et des groupes. Par exemple, les utilisateurs et les groupes doivent être suivis lorsqu’un compte d’utilisateur ou d’ordinateur, un groupe de sécurité ou un groupe de distribution est créé, modifié ou supprimé ; Lorsqu’un compte d’utilisateur ou d’ordinateur est renommé, désactivé ou activé ; ou lorsque le mot de passe d’un utilisateur ou d’un ordinateur est modifié. Un événement peut être généré pour des utilisateurs ou des groupes qui sont ajoutés ou supprimés d’autres groupes.  
  
##### <a name="audit-directory-service-access"></a>Auditer l’accès au service d’annuaire  

Ce paramètre de stratégie détermine s’il faut auditer l’accès du principal de sécurité à un objet Active Directory qui a sa propre liste de contrôle d’accès système (SACL) spécifiée. En général, cette catégorie doit être activée uniquement sur les contrôleurs de domaine. Lorsqu’il est activé, ce paramètre génère beaucoup de « bruit ».  
  
##### <a name="audit-logon-events"></a>Auditer les événements de connexion  
Les événements d’ouverture de session sont générés lorsqu’une entité de sécurité locale est authentifiée sur un ordinateur local. Les événements d’ouverture de session consignent les ouvertures de session de domaine qui se produisent sur l’ordinateur local. Les événements de fermeture de session de compte ne sont pas générés. Lorsque cette option est activée, les événements d’ouverture de session génèrent beaucoup de « bruit », mais ils doivent être activés par défaut dans n’importe quel plan d’audit de sécurité.  
  
##### <a name="audit-object-access"></a>Auditer l’accès aux objets  
L’accès aux objets peut générer des événements lorsque des objets définis par la suite avec l’audit activé sont accessibles (par exemple, ouvert, lu, renommé, supprimé ou fermé). Une fois la catégorie d’audit principale activée, l’administrateur doit définir individuellement les objets pour lesquels l’audit est activé. De nombreux objets système Windows sont livrés avec l’audit activé. ainsi, l’activation de cette catégorie commencera généralement à générer des événements avant que l’administrateur en ait défini un.  
  
Cette catégorie est très « bruyante » et génère cinq à dix événements pour chaque accès à un objet. Il peut être difficile pour les administrateurs qui débutent dans l’audit d’objets d’obtenir des informations utiles. Elle doit être activée uniquement lorsque cela est nécessaire.  
  
##### <a name="auditing-policy-change"></a>Modification de la stratégie d’audit  
Ce paramètre de stratégie détermine s’il faut auditer chaque incidence d’une modification apportée aux stratégies d’attribution de droits d’utilisateur, stratégies de pare-feu Windows, stratégies d’approbation ou modifications apportées à la stratégie d’audit. Cette catégorie doit être activée sur tous les ordinateurs. Il génère très peu de bruit.  
  
##### <a name="audit-privilege-use"></a>Auditer l’utilisation des privilèges  

Il existe des dizaines de droits d’utilisateur et d’autorisations dans Windows (par exemple, ouverture de session en tant que traitement par lots et agir en tant que partie du système d’exploitation). Ce paramètre de stratégie détermine s’il faut auditer chaque instance d’un principal de sécurité en exerçant un droit d’utilisateur ou un privilège. L’activation de cette catégorie entraîne un « bruit », mais elle peut être utile pour suivre les comptes d’entités de sécurité à l’aide de privilèges élevés.  
  
##### <a name="audit-process-tracking"></a>Auditer le suivi des processus  
Ce paramètre de stratégie détermine s’il faut auditer les informations détaillées de suivi des processus pour les événements tels que l’activation du programme, la sortie du processus, la duplication des handles et l’accès indirect aux objets. Il est utile pour le suivi des utilisateurs malveillants et des programmes qu’ils utilisent.  
  
L’activation du suivi des processus d’audit génère un grand nombre d’événements. il est donc généralement défini sur **aucun audit**. Toutefois, ce paramètre peut fournir un avantage considérable au cours d’une réponse à un incident à partir du journal détaillé des processus démarrés et de l’heure à laquelle ils ont été lancés. Pour les contrôleurs de domaine et d’autres serveurs d’infrastructure à rôle unique, cette catégorie peut être activée en toute sécurité. Les serveurs à rôle unique ne génèrent pas beaucoup de trafic de suivi des processus dans le cadre normal de leurs tâches. Par conséquent, ils peuvent être activés pour capturer les événements non autorisés s’ils se produisent.  
  
##### <a name="system-events-audit"></a>Audit des événements système  

Les événements système sont presque une catégorie générique de rattrapage, qui consiste à inscrire divers événements qui ont un impact sur l’ordinateur, sa sécurité système ou le journal de sécurité. Il comprend des événements pour les arrêts de l’ordinateur et les redémarrages, les pannes d’alimentation, les modifications de l’heure du système, les initialisations du package d’authentification, les effacs du journal d’audit, les problèmes d’emprunt d’identité et un hôte d’autres événements généraux. En général, l’activation de cette catégorie d’audit génère beaucoup de « bruit », mais elle génère suffisamment d’événements très utiles, qu’il est difficile de ne jamais activer.  
  
#### <a name="advanced-audit-policies"></a>Stratégies d’audit avancées

À compter de Windows Vista et de Windows Server 2008, Microsoft a amélioré la façon dont les sélections de catégories du journal des événements peuvent être effectuées en créant des sous-catégories sous chaque catégorie d’audit principale. Les sous-catégories permettent à l’audit d’être beaucoup plus granulaire qu’en utilisant les catégories principales. En utilisant des sous-catégories, vous pouvez activer uniquement des parties d’une catégorie principale particulière et ignorer la génération d’événements pour lesquels vous n’avez pas d’utilisation. Chaque sous-catégorie d’audit peut être activée pour la réussite, l’échec, ou la réussite et l’échec.  
  
Pour répertorier toutes les sous-catégories d’audit disponibles, examinez le conteneur de la stratégie d’audit avancée dans un objet stratégie de groupe ou tapez la commande suivante à l’invite de commandes sur n’importe quel ordinateur exécutant Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008, Windows 8, Windows 7 ou Windows Vista :  
  
`auditpol /list /subcategory:*`
  
Pour obtenir la liste des sous-catégories d’audit actuellement configurées sur un ordinateur exécutant Windows Server 2012, Windows Server 2008 R2 ou Windows 2008, tapez la commande suivante :  
  
`auditpol /get /category:*`
  
La capture d’écran suivante montre un exemple d’auditpol. exe qui répertorie la stratégie d’audit actuelle.  
  
![surveillance d’AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_5.gif)  
  
> [!NOTE]  
> Stratégie de groupe ne signale pas toujours avec précision l’état de toutes les stratégies d’audit activées, contrairement à auditpol. exe. Pour plus d’informations, consultez obtention d’une [stratégie d’audit efficace dans Windows 7 et 2008 R2](https://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) .  
  
Chaque catégorie principale a plusieurs sous-catégories. Vous trouverez ci-dessous une liste des catégories, des sous-catégories et une description de leurs fonctions.  
  
### <a name="auditing-subcategories-descriptions"></a>Descriptions des sous-catégories d’audit  
Les sous-catégories de stratégie d’audit activent les types de message de journal des événements suivants :  
  
#### <a name="account-logon"></a>Ouverture de session de compte  
  
##### <a name="credential-validation"></a>Validation des informations d'identification  
Cette sous-catégorie signale les résultats des tests de validation sur les informations d’identification soumises pour une demande d’ouverture de session de compte d’utilisateur. Ces événements se produisent sur l’ordinateur qui fait autorité pour les informations d’identification. Pour les comptes de domaine, le contrôleur de domaine fait autorité, tandis que pour les comptes locaux, l’ordinateur local fait autorité.  
  
Dans les environnements de domaine, la plupart des événements d’ouverture de session de compte sont consignés dans le journal de sécurité des contrôleurs de domaine faisant autorité pour les comptes de domaine. Toutefois, ces événements peuvent se produire sur d’autres ordinateurs de l’organisation lorsque des comptes locaux sont utilisés pour ouvrir une session.  
  
##### <a name="kerberos-service-ticket-operations"></a>Opérations de ticket de service Kerberos  
Cette sous-catégorie signale les événements générés par les processus de demande de ticket Kerberos sur le contrôleur de domaine faisant autorité pour le compte de domaine.  
  
##### <a name="kerberos-authentication-service"></a>Service d’authentification Kerberos  
Cette sous-catégorie signale les événements générés par le service d’authentification Kerberos. Ces événements se produisent sur l’ordinateur qui fait autorité pour les informations d’identification.  
  
##### <a name="other-account-logon-events"></a>Autres événements d’ouverture de session de compte  
Cette sous-catégorie signale les événements qui se produisent en réponse aux informations d’identification soumises pour une demande d’ouverture de session de compte d’utilisateur qui ne sont pas liées à la validation des informations d’identification ou aux tickets Kerberos. Ces événements se produisent sur l’ordinateur qui fait autorité pour les informations d’identification. Pour les comptes de domaine, le contrôleur de domaine fait autorité, tandis que pour les comptes locaux, l’ordinateur local fait autorité.  
  
Dans les environnements de domaine, la plupart des événements d’ouverture de session de compte sont consignés dans le journal de sécurité des contrôleurs de domaine faisant autorité pour les comptes de domaine. Toutefois, ces événements peuvent se produire sur d’autres ordinateurs de l’organisation lorsque des comptes locaux sont utilisés pour ouvrir une session. Voici quelques exemples :  
  
* Services Bureau à distance des déconnexions de session  
* Nouvelles sessions de Services Bureau à distance  
* Verrouillage et déverrouillage d’une station de travail  
* Appel d’un économiseur d’écran  
* Disparition d’un écran de veille  
* Détection d’une attaque par relecture Kerberos, dans laquelle une demande Kerberos avec des informations identiques est reçue deux fois  
* Accès à un réseau sans fil accordé à un compte d’utilisateur ou d’ordinateur  
* Accès à un réseau 802.1 x câblé accordé à un compte d’utilisateur ou d’ordinateur  
  
#### <a name="account-management"></a>Gestion du compte  
  
##### <a name="user-account-management"></a>Gestion des comptes d’utilisateur  
Cette sous-catégorie signale chaque événement de la gestion des comptes d’utilisateur, par exemple lorsqu’un compte d’utilisateur est créé, modifié ou supprimé ; un compte d’utilisateur est renommé, désactivé ou activé ; ou un mot de passe est défini ou modifié. Si ce paramètre de stratégie d’audit est activé, les administrateurs peuvent suivre les événements pour détecter la création de comptes d’utilisateur malveillants, accidentels et autorisés.  
  
##### <a name="computer-account-management"></a>Gestion des comptes d’ordinateur  
Cette sous-catégorie signale chaque événement de gestion de compte d’ordinateur, par exemple lorsqu’un compte d’ordinateur est créé, modifié, supprimé, renommé, désactivé ou activé.  
  
##### <a name="security-group-management"></a>Gestion des groupes de sécurité  
Cette sous-catégorie signale chaque événement de la gestion des groupes de sécurité, par exemple lorsqu’un groupe de sécurité est créé, modifié ou supprimé ou lorsqu’un membre est ajouté ou supprimé dans un groupe de sécurité. Si ce paramètre de stratégie d’audit est activé, les administrateurs peuvent effectuer le suivi des événements pour détecter les comptes de groupe de sécurité malveillants, accidentels et autorisés.  
  
##### <a name="distribution-group-management"></a>Gestion des groupes de distribution  
Cette sous-catégorie signale chaque événement de la gestion des groupes de distribution, par exemple lorsqu’un groupe de distribution est créé, modifié ou supprimé ou lorsqu’un membre est ajouté ou supprimé dans un groupe de distribution. Si ce paramètre de stratégie d’audit est activé, les administrateurs peuvent suivre les événements pour détecter la création de comptes de groupe malveillants, accidentels et autorisés.  
  
##### <a name="application-group-management"></a>Gestion des groupes d’applications  
Cette sous-catégorie signale chaque événement de gestion des groupes d’applications sur un ordinateur, par exemple lorsqu’un groupe d’applications est créé, modifié ou supprimé ou lorsqu’un membre est ajouté ou supprimé dans un groupe d’applications. Si ce paramètre de stratégie d’audit est activé, les administrateurs peuvent suivre les événements pour détecter la création d’un compte de groupe d’applications malveillant, accidentel et autorisé.  
  
##### <a name="other-account-management-events"></a>Autres événements de gestion des comptes  
Cette sous-catégorie signale d’autres événements de gestion des comptes.  
  
#### <a name="detailed-process-tracking"></a>Suivi détaillé des processus  
  
##### <a name="process-creation"></a>Création de processus  
Cette sous-catégorie signale la création d’un processus et le nom de l’utilisateur ou du programme qui l’a créé.  
  
##### <a name="process-termination"></a>Arrêt du processus  
Cette sous-catégorie signale le moment où un processus se termine.  
  
##### <a name="dpapi-activity"></a>Activité DPAPI  
Cette sous-catégorie signale les appels de chiffrement ou de déchiffrement dans l’interface de programmation d’applications de protection des données (DPAPI). DPAPI est utilisé pour protéger les informations confidentielles, telles que le mot de passe stocké et les informations de clé.  
  
##### <a name="rpc-events"></a>Événements RPC  
Cette sous-catégorie signale les événements de connexion d’appel de procédure distante (RPC).  
  
#### <a name="directory-service-access"></a>Accès au service d’annuaire  
  
##### <a name="directory-service-access"></a>Accès au service d’annuaire  
Cette sous-catégorie signale l’accès à un objet AD DS. Seuls les objets avec des listes SACL configurées provoquent la génération d’événements d’audit et uniquement lorsqu’ils sont accessibles de manière à correspondre aux entrées SACL. Ces événements sont semblables aux événements d’accès au service d’annuaire dans les versions antérieures de Windows Server. Cette sous-catégorie s’applique uniquement aux contrôleurs de domaine.  
  
##### <a name="directory-service-changes"></a>Modifications du service d’annuaire  
Cette sous-catégorie signale les modifications apportées aux objets dans AD DS. Les types de modifications signalés sont les opérations de création, de modification, de déplacement et d’annulation de suppression effectuées sur un objet. L’audit des modifications du service d’annuaire, le cas échéant, indique les anciennes et nouvelles valeurs des propriétés modifiées des objets qui ont été modifiés. Seuls les objets avec SACL entraînent la génération d’événements d’audit et uniquement lorsqu’ils sont accessibles de manière à correspondre à leurs entrées SACL. Certains objets et propriétés n’entraînent pas la génération d’événements d’audit en raison des paramètres de la classe d’objet dans le schéma. Cette sous-catégorie s’applique uniquement aux contrôleurs de domaine.  
  
##### <a name="directory-service-replication"></a>Réplication du service d’annuaire  
Cette sous-catégorie signale que la réplication entre deux contrôleurs de domaine commence et se termine.  
  
##### <a name="detailed-directory-service-replication"></a>Réplication de service d’annuaire détaillée  
Cette sous-catégorie fournit des informations détaillées sur les informations répliquées entre les contrôleurs de domaine. Ces événements peuvent être très élevés en volume.  
  
#### <a name="logonlogoff"></a>Ouverture/fermeture de session  
  
##### <a name="logon"></a>Ouverture de session  
Cette sous-catégorie signale quand un utilisateur tente de se connecter au système. Ces événements se produisent sur l’ordinateur auquel vous accédez. Pour les ouvertures de session interactives, la génération de ces événements se produit sur l’ordinateur qui est connecté à. Si une ouverture de session réseau a lieu pour accéder à un partage, ces événements sont générés sur l’ordinateur qui héberge la ressource accédée. Si ce paramètre est configuré sur **aucun audit**, il est difficile, voire impossible, de déterminer quel utilisateur a accédé ou a tenté d’accéder aux ordinateurs de l’organisation.  
  
##### <a name="network-policy-server"></a>Serveur NPS (Network Policy Server)  
Cette sous-catégorie signale les événements générés par les demandes d’accès utilisateur RADIUS (IAS) et protection d’accès réseau (NAP). Ces demandes peuvent être **accordées**, **refusées**, **ignorées**, **mises en quarantaine**, **verrouillées**et **déverrouillées**. L’audit de ce paramètre entraîne un volume moyen ou élevé d’enregistrements sur les serveurs NPS et IAS.  
  
##### <a name="ipsec-main-mode"></a>Mode principal IPsec  
Cette sous-catégorie signale les résultats du protocole IKE (protocole IKE (Internet Key Exchange) Protocol et protocole Authenticated IP (Authenticated Internet Protocol)) lors des négociations en mode principal.  
  
##### <a name="ipsec-extended-mode"></a>Mode étendu IPsec  
Cette sous-catégorie signale les résultats d’AuthIP lors des négociations en mode étendu.  
  
##### <a name="other-logonlogoff-events"></a>Autres événements d’ouverture/de fermeture de session  
Cette sous-catégorie signale d’autres événements liés à l’ouverture et à la fermeture de session, tels que les déconnexions et les reconnexions de Services Bureau à distance session, à l’aide de RunAs pour exécuter des processus sous un compte différent, et pour le verrouillage et le déverrouillage d’une station de travail.  
  
##### <a name="logoff"></a>Fermeture de session  
Cette sous-catégorie signale qu’un utilisateur se déconnecte du système. Ces événements se produisent sur l’ordinateur auquel vous accédez. Pour les ouvertures de session interactives, la génération de ces événements se produit sur l’ordinateur qui est connecté à. Si une ouverture de session réseau a lieu pour accéder à un partage, ces événements sont générés sur l’ordinateur qui héberge la ressource accédée. Si ce paramètre est configuré sur **aucun audit**, il est difficile, voire impossible, de déterminer quel utilisateur a accédé ou a tenté d’accéder aux ordinateurs de l’organisation.  
  
##### <a name="account-lockout"></a>Verrouillage de compte  
Cette sous-catégorie signale quand un compte d’utilisateur est verrouillé suite à un trop grand nombre de tentatives de connexion ayant échoué.  
  
##### <a name="ipsec-quick-mode"></a>Mode rapide IPsec  
Cette sous-catégorie signale les résultats du protocole IKE et AuthIP lors des négociations en mode rapide.  
  
##### <a name="special-logon"></a>Ouverture de session spéciale  
Cette sous-catégorie signale qu’une ouverture de session spéciale est utilisée. Une ouverture de session spéciale est une ouverture de session disposant de privilèges d’administrateur et qui peut être utilisée pour élever un processus à un niveau supérieur.  
  
#### <a name="policy-change"></a>Modification de la stratégie  
  
##### <a name="audit-policy-change"></a>Modification de la stratégie d’audit  
Cette sous-catégorie signale les modifications apportées à la stratégie d’audit, y compris les modifications SACL.  
  
##### <a name="authentication-policy-change"></a>Modification de la stratégie d’authentification  
Cette sous-catégorie signale les modifications apportées à la stratégie d’authentification.  
  
##### <a name="authorization-policy-change"></a>Modification de la stratégie d’autorisation  
Cette sous-catégorie signale les modifications apportées à la stratégie d’autorisation, y compris les modifications des autorisations (DACL).  
  
##### <a name="mpssvc-rule-level-policy-change"></a>Modification de la stratégie au niveau de la règle MPSSVC  
Cette sous-catégorie signale les modifications apportées aux règles de stratégie utilisées par le service de protection Microsoft (MPSSVC. exe). Ce service est utilisé par le pare-feu Windows.  
  
##### <a name="filtering-platform-policy-change"></a>Filtrage de la modification de stratégie de plateforme  
Cette sous-catégorie signale l’ajout et la suppression d’objets de WFP, y compris les filtres de démarrage. Ces événements peuvent être très élevés en volume.  
  
##### <a name="other-policy-change-events"></a>Autres événements de modification de stratégie  
Cette sous-catégorie signale d’autres types de modifications de stratégie de sécurité, telles que la configuration du Module de plateforme sécurisée (TPM) (TPM) ou des fournisseurs de services de chiffrement.  
  
#### <a name="privilege-use"></a>Utilisation des privilèges  
  
##### <a name="sensitive-privilege-use"></a>Utilisation des privilèges sensibles  
Cette sous-catégorie signale qu’un compte d’utilisateur ou un service utilise un privilège sensible. Un privilège sensible comprend les droits d’utilisateur suivants : agir en tant que partie du système d’exploitation, sauvegarder des fichiers et des répertoires, créer un objet de jeton, déboguer des programmes, activer des comptes d’ordinateur et d’utilisateur à approuver pour la délégation, générer des audits de sécurité, emprunter l’identité d’un client après l’authentification, charger et décharger des pilotes de périphériques, gérer le journal d’audit et de sécurité , restaurer des fichiers et des répertoires et prendre possession de fichiers ou d’autres objets. L’audit de cette sous-catégorie crée un volume élevé d’événements.  
  
##### <a name="nonsensitive-privilege-use"></a>Utilisation des privilèges non sensibles  
Cette sous-catégorie signale qu’un compte d’utilisateur ou un service utilise un privilège non sensible. Les privilèges non sensibles incluent les droits d’utilisateur suivants : accéder au gestionnaire d’informations d’identification en tant qu’appelant approuvé, accéder à cet ordinateur à partir du réseau, ajouter des stations de travail au domaine, ajuster les quotas de mémoire pour un processus, autoriser l’ouverture d’une session locale, autoriser la connexion via Services Bureau à distance, ignorer la vérification de parcours, modifier l’heure système , interdire l’accès à cet ordinateur à partir du réseau, interdire l’ouverture d’une session en tant que tâche, interdire l’ouverture d’une session en tant que service, interdire l’ouverture d’une session locale, interdire l’ouverture de session par Services Bureau à distance, forcer l’arrêt à partir d’un système distant, augmenter une plage de travail de processus, augmenter la priorité de planification, verrouiller les pages en mémoire , effectuez des tâches de maintenance de volume, profilez un processus unique, profilez les performances du système, retirez l’ordinateur de la station d’accueil, arrêtez le système et synchronisez les données du service d’annuaire. L’audit de cette sous-catégorie crée un très gros volume d’événements.  
  
##### <a name="other-privilege-use-events"></a>Autres événements d’utilisation de privilège  
Ce paramètre de stratégie de sécurité n’est pas utilisé actuellement.  
  
#### <a name="object-access"></a>Accès aux objets  
  
##### <a name="file-system"></a>Système de fichiers  
Cette sous-catégorie signale l’accès aux objets du système de fichiers. Seuls les objets de système de fichiers avec SACL entraînent la génération d’événements d’audit, et uniquement quand ils sont accessibles de manière à correspondre à leurs entrées SACL. En soi, ce paramètre de stratégie ne provoque pas l’audit de tous les événements. Elle détermine s’il faut auditer l’événement d’un utilisateur qui accède à un objet de système de fichiers qui a une liste de contrôle d’accès système (SACL) spécifiée, ce qui permet d’effectuer l’audit.  
  
Si le paramètre Auditer l’accès aux objets est configuré sur **succès**, une entrée d’audit est générée chaque fois qu’un utilisateur accède à un objet avec une liste SACL spécifiée. Si ce paramètre de stratégie est configuré sur **échec**, une entrée d’audit est générée chaque fois qu’un utilisateur échoue dans une tentative d’accès à un objet avec une liste SACL spécifiée.  
  
##### <a name="registry"></a>Registre  
Cette sous-catégorie signale les accès aux objets du Registre. Seuls les objets de Registre avec SACL entraînent la génération d’événements d’audit et uniquement lorsqu’ils sont accessibles de manière à correspondre à leurs entrées SACL. En soi, ce paramètre de stratégie ne provoque pas l’audit de tous les événements.  
  
##### <a name="kernel-object"></a>Objet de noyau  
Cette sous-catégorie signale les accès aux objets de noyau tels que les processus et les mutex. Seuls les objets noyau avec SACL entraînent la génération d’événements d’audit, et uniquement lorsqu’ils sont accessibles de manière à correspondre à leurs entrées SACL. En général, les objets de noyau reçoivent uniquement des SACL si les options d’audit AuditBaseObjects ou AuditBaseDirectories sont activées.  
  
##### <a name="sam"></a>SAM  
Cette sous-catégorie signale les accès aux objets de base de données d’authentification SAM (Security Accounts Manager) locaux.  
  
##### <a name="certification-services"></a>Services de certification  
Cette sous-catégorie signale les opérations des services de certification effectuées.  
  
##### <a name="application-generated"></a>Application générée  
Cette sous-catégorie signale quand des applications tentent de générer des événements d’audit à l’aide des interfaces de programmation d’applications (API) d’audit Windows.  
  
##### <a name="handle-manipulation"></a>Manipulation de handles  
Cette sous-catégorie signale quand un handle vers un objet est ouvert ou fermé. Seuls les objets avec SACL entraînent la génération de ces événements, et uniquement si l’opération tentée par le handle correspond aux entrées SACL. Les événements de manipulation de handle sont générés uniquement pour les types d’objets où la sous-catégorie d’accès à l’objet correspondante est activée (par exemple, le système de fichiers ou le registre).  
  
##### <a name="file-share"></a>Partage de fichiers  
Cette sous-catégorie signale l’accès à un partage de fichiers. En soi, ce paramètre de stratégie ne provoque pas l’audit de tous les événements. Elle détermine s’il faut auditer l’événement d’un utilisateur qui accède à un objet partage de fichiers qui a une liste de contrôle d’accès système (SACL) spécifiée, ce qui permet d’effectuer l’audit.  
  
##### <a name="filtering-platform-packet-drop"></a>Filtrage de la suppression de paquets de plateforme  
Cette sous-catégorie signale la suppression des paquets par la plateforme de filtrage Windows (WFP). Ces événements peuvent être très élevés en volume.  
  
##### <a name="filtering-platform-connection"></a>Filtrage de la connexion de plateforme  
Cette sous-catégorie signale quand les connexions sont autorisées ou bloquées par WFP. Ces événements peuvent avoir un volume élevé.  
  
##### <a name="other-object-access-events"></a>Autres événements d’accès aux objets  
Cette sous-catégorie signale d’autres événements liés à l’accès aux objets, tels que les travaux de Planificateur de tâches et les objets COM+.  
  
#### <a name="system"></a>System  
  
##### <a name="security-state-change"></a>Changement d’état de sécurité  
Cette sous-catégorie signale les modifications de l’état de sécurité du système, par exemple lorsque le sous-système de sécurité démarre et s’arrête.  
  
##### <a name="security-system-extension"></a>Extension du système de sécurité  
Cette sous-catégorie signale le chargement du code d’extension, par exemple les packages d’authentification, par le sous-système de sécurité.  
  
##### <a name="system-integrity"></a>Intégrité du système  
Cette sous-catégorie signale les violations de l’intégrité du sous-système de sécurité.  
  
Pilote IPsec  
  
Cette sous-catégorie signale les activités du pilote IPsec (Internet Protocol Security).  
  
##### <a name="other-system-events"></a>Autres événements système  
Cette sous-catégorie signale d’autres événements système.  
  
Pour plus d’informations sur les descriptions des sous-catégories, reportez-vous à l' [outil Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx).  
  
Chaque organisation doit passer en revue les catégories et sous-catégories couvertes précédentes et activer celles qui conviennent le mieux à leur environnement. Les modifications apportées à la stratégie d’audit doivent toujours être testées avant le déploiement dans un environnement de production.  
  
## <a name="configuring-windows-audit-policy"></a>Configuration de la stratégie d’audit Windows

La stratégie d’audit Windows peut être définie à l’aide des stratégies de groupe, auditpol. exe, des API ou des modifications du Registre. Les méthodes recommandées pour configurer la stratégie d’audit pour la plupart des entreprises sont stratégie de groupe ou auditpol. exe. La définition d’une stratégie d’audit du système nécessite des autorisations de compte de niveau administrateur ou les autorisations déléguées appropriées.  
  
> [!NOTE]  
> Le privilège **gérer le journal d’audit et de sécurité** doit être accordé aux principaux de sécurité (les administrateurs l’ont par défaut) pour permettre la modification des options d’audit d’accès aux objets des ressources individuelles, telles que les fichiers, les objets de Active Directory et les clés de registre.  
  
### <a name="setting-windows-audit-policy-by-using-group-policy"></a>Définition de la stratégie d’audit Windows à l’aide de stratégie de groupe

Pour définir la stratégie d’audit à l’aide de stratégies de groupe, configurez les catégories d’audit appropriées situées sous **Configuration ordinateur \ paramètres Windows \ paramètres** de configuration \ stratégie de \ (voir la capture d’écran suivante pour obtenir un exemple de l’éditeur de stratégie de groupe local (gpedit. msc)). Chaque catégorie de stratégie d’audit peut être activée pour les événements de **réussite**, d' **échec**ou de **réussite** et d’échec.  
  
![surveillance d’AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_6.gif)  
  
La stratégie d’audit avancée peut être définie à l’aide de Active Directory ou de stratégies de groupe locales. Pour définir une stratégie d’audit avancée, configurez les sous-catégories appropriées situées sous **ordinateur \ paramètres de configuration \ stratégie d’audit avancée** (consultez la capture d’écran suivante pour obtenir un exemple de l’éditeur de stratégie de groupe local (gpedit. msc)). Chaque sous-catégorie de stratégie d’audit peut être activée pour les événements de **réussite**, d' **échec**ou de **réussite** et d' **échec** .  
  
![surveillance d’AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_7.gif)  
  
### <a name="setting-windows-audit-policy-using-auditpolexe"></a>Définition de la stratégie d’audit Windows à l’aide d’auditpol. exe

Auditpol. exe (pour la définition de la stratégie d’audit Windows) a été introduit dans Windows Server 2008 et Windows Vista. Initialement, seul auditpol. exe pouvait être utilisé pour définir une stratégie d’audit avancée, mais stratégie de groupe peut être utilisé dans Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008, Windows 8 et Windows 7.  
  
Auditpol. exe est un utilitaire de ligne de commande. La syntaxe est la suivante :  
  
`auditpol /set /<Category|Subcategory>:<audit category> /<success|failure:> /<enable|disable>`
  
Exemples de syntaxe d’auditpol. exe :  
  
`auditpol /set /subcategory:"user account management" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"logon" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"IPSEC Main Mode" /failure:enable`
  
> [!NOTE]  
> Auditpol. exe définit localement la stratégie d’audit avancée. Si la stratégie locale est en conflit avec Active Directory ou stratégie de groupe locale, les paramètres de stratégie de groupe prévalent généralement sur les paramètres d’auditpol. exe. Lorsqu’il existe plusieurs conflits de stratégie de groupe ou de stratégie locale, une seule stratégie prévaut (c’est-à-dire, remplacer). Les stratégies d’audit ne seront pas fusionnées.  
  
#### <a name="scripting-auditpol"></a>Écriture de scripts Auditpol

Microsoft fournit un [exemple de script](https://support.microsoft.com/kb/921469) pour les administrateurs qui souhaitent définir une stratégie d’audit avancée à l’aide d’un script au lieu de taper manuellement chaque commande auditpol. exe.  
  
**Remarque** Stratégie de groupe ne signale pas toujours avec précision l’état de toutes les stratégies d’audit activées, contrairement à auditpol. exe. Pour plus d’informations, consultez obtention d’une [stratégie d’audit efficace dans Windows 7 et windows 2008 R2](https://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) .  
  
#### <a name="other-auditpol-commands"></a>Autres commandes Auditpol

Auditpol. exe peut être utilisé pour enregistrer et restaurer une stratégie d’audit locale et pour afficher d’autres commandes associées à l’audit. Voici les autres commandes **Auditpol** .  
  
`auditpol /clear`-utilisé pour effacer et réinitialiser les stratégies d’audit locales  
  
`auditpol /backup /file:<filename>`-utilisé pour sauvegarder une stratégie d’audit locale actuelle dans un fichier binaire  
  
`auditpol /restore /file:<filename>`-utilisé pour importer un fichier de stratégie d’audit précédemment enregistré dans une stratégie d’audit locale  
  
`auditpol /<get/set> /option:<CrashOnAuditFail> /<enable/disable>`-si ce paramètre de stratégie d’audit est activé, il provoque l’arrêt immédiat du système (avec le message STOP : C0000244 {audit Failed}) si un audit de sécurité ne peut pas être enregistré pour une raison quelconque. En règle générale, un événement ne peut pas être consigné lorsque le journal d’audit de sécurité est plein et que la méthode de rétention spécifiée pour le journal de sécurité est **ne pas remplacer les événements** ou **remplacer les événements par jours**. En général, elle est uniquement activée par des environnements nécessitant une plus grande assurance que le journal de sécurité est en journal. Si cette option est activée, les administrateurs doivent observer attentivement la taille du journal de sécurité et faire pivoter les journaux en fonction des besoins. Il peut également être défini avec stratégie de groupe en modifiant l’option de sécurité **auditer : arrêter immédiatement le système s’il n’est pas possible d’enregistrer les audits de sécurité** (valeur par défaut = désactivé).  
  
`auditpol /<get/set> /option:<AuditBaseObjects> /<enable/disable>`-ce paramètre de stratégie d’audit détermine s’il faut auditer l’accès des objets système globaux. Si cette stratégie est activée, les objets système, tels que les mutex, les événements, les sémaphores et les appareils DOS, sont créés avec une liste de contrôle d’accès système (SACL) par défaut. La plupart des administrateurs considèrent que l’audit des objets système globaux est trop « bruyant » et ne l’activera que si le piratage malveillant est suspecté. Seuls les objets nommés reçoivent une liste SACL. Si la stratégie d’audit Auditer l’accès aux objets (ou sous-catégorie audit d’objets noyau) est également activée, l’accès à ces objets système est audité. Lors de la configuration de ce paramètre de sécurité, les modifications ne prennent effet qu’après le redémarrage de Windows. Vous pouvez également définir cette stratégie avec stratégie de groupe en modifiant l’option de sécurité auditer l’accès des objets système globaux (valeur par défaut = désactivé).  
  
`auditpol /<get/set> /option:<AuditBaseDirectories> /<enable/disable>`-ce paramètre de stratégie d’audit spécifie que les objets de noyau nommés (tels que les mutex et les sémaphores) doivent recevoir des listes SACL lors de leur création. AuditBaseDirectories affecte les objets de conteneur tandis que AuditBaseObjects affecte les objets qui ne peuvent pas contenir d’autres objets.  
  
`auditpol /<get/set> /option:<FullPrivilegeAuditing> /<enable/disable>`-ce paramètre de stratégie d’audit spécifie si le client génère un événement lorsqu’un ou plusieurs de ces privilèges sont affectés à un jeton de sécurité utilisateur : AssignPrimaryTokenPrivilege, AuditPrivilege, BackupPrivilege, CreateTokenPrivilege, DebugPrivilege, EnableDelegationPrivilege, ImpersonatePrivilege, LoadDriverPrivilege, RestorePrivilege, SecurityPrivilege, SystemEnvironmentPrivilege, TakeOwnershipPrivilege et TcbPrivilege. Si cette option n’est pas activée (valeur par défaut = désactivé), les privilèges BackupPrivilege et RestorePrivilege ne sont pas enregistrés. L’activation de cette option peut rendre le journal de sécurité extrêmement bruyant (parfois des centaines d’événements par seconde) pendant une opération de sauvegarde. Vous pouvez également définir cette stratégie avec stratégie de groupe en modifiant l’option de sécurité **audit : auditer l’utilisation des privilèges de sauvegarde et de restauration**.  
  
> [!NOTE]  
> Certaines informations fournies ici proviennent du [type d’option d’audit](https://msdn.microsoft.com/library/dd973862(prot.20).aspx) Microsoft et de l’outil Microsoft SCM.  
  
## <a name="enforcing-traditional-auditing-or-advanced-auditing"></a>Application de l’audit traditionnel ou de l’audit avancé

Dans Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows 8, Windows 7 et Windows Vista, les administrateurs peuvent choisir d’activer les neuf catégories traditionnelles ou d’utiliser les sous-catégories. Il s’agit d’un choix binaire qui doit être effectué dans chaque système Windows. Soit les catégories principales peuvent être activées, soit les subcategoriesit ne peuvent pas être les deux à la fois.  
  
Pour empêcher la stratégie traditionnelle de catégorie traditionnelle de remplacer les sous-catégories de stratégie d’audit, vous devez activer les paramètres de la sous-catégorie de stratégie d' **audit forcée (Windows Vista ou version ultérieure) pour remplacer les paramètres de la catégorie de stratégie d’audit** situés sous Configuration de l' **ordinateur \ paramètres Windows**\ paramètres de configuration \ stratégies.  
  
Nous vous recommandons d’activer et de configurer les sous-catégories à la place des neuf catégories principales. Cela nécessite l’activation d’un paramètre stratégie de groupe (pour permettre aux sous-catégories de remplacer les catégories d’audit) et de configurer les différentes sous-catégories qui prennent en charge les stratégies d’audit.  
  
Les sous-catégories d’audit peuvent être configurées à l’aide de plusieurs méthodes, notamment stratégie de groupe et le programme de ligne de commande, auditpol. exe.  
  
## <a name="next-steps"></a>Étapes suivantes :
  
* [Audit de sécurité avancé dans Windows 7 et Windows Server 2008 R2](https://social.technet.microsoft.com/wiki/contents/articles/advanced-security-auditing-in-windows-7-and-windows-server-2008-r2.aspx)  
  
* [Audit et conformité dans Windows Server 2008](https://technet.microsoft.com/magazine/2008.03.auditing.aspx)  
  
* [Comment utiliser stratégie de groupe pour configurer des paramètres d’audit de sécurité détaillés pour les ordinateurs Windows Vista et Windows Server 2008 dans un domaine Windows Server 2008, dans un domaine Windows Server 2003 ou dans un domaine Windows 2000](https://support.microsoft.com/kb/921469)  
  
* [Guide pas à pas de la stratégie d’audit de sécurité avancée](https://technet.microsoft.com/library/dd408940(WS.10).aspx)  
