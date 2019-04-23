---
ms.assetid: a7ef2fba-b05c-4be2-93b2-b9456244c3ad
title: Surveillance des signes de compromission d'Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 40d0d06f8d6d25c2c1dbf4662d3296a996d22055
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882940"
---
# <a name="monitoring-active-directory-for-signs-of-compromise"></a>Surveillance des signes de compromission d'Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*La loi numéro cinq : Vigilance externe est le prix de la sécurité.* - [10 lois immuables de l’Administration de la sécurité](https://technet.microsoft.com/library/cc722488.aspx)  
  
Un solid journal des événements système de surveillance est un élément essentiel de toute conception Active Directory sécurisée. Nombreuses violations de sécurité ordinateur pourrait être détectées au début de l’événement si victimes mises en œuvre la surveillance et les alertes journal d’événements approprié. Rapports indépendants ont en charge depuis longtemps à cette conclusion. Par exemple, le [rapport de violation des données Verizon 2009](http://www.verizonbusiness.com/resources/security/reports/2009_databreach_rp.pdf) états :  
  
« L’inefficacité apparente de l’analyse de surveillance et de journaux des événements reste quelque sorte une énigme. L’opportunité pour la détection est enquêteurs de noter que 66 % des victimes avait suffisamment d’éléments disponible au sein de leurs journaux pour découvrir la violation s’ils avaient été plus soucieux de l’analyse de ces ressources. »  
  
Ce manque de surveillance des journaux d’événements actives reste une faiblesse cohérente dans les plans de défense de sécurité de nombreuses sociétés. Le [rapport de divulgation de données Verizon 2012](http://www.verizonbusiness.com/resources/reports/rp_data-breach-investigations-report-2012_en_xg.pdf) trouvé que bien que 85 % des violations a duré plusieurs semaines pour être remarqué, 84 % des victimes avait preuve de la violation dans leurs journaux des événements.  
  
## <a name="windows-audit-policy"></a>Stratégie d’Audit de Windows

Voici des liens vers le blog de support technique de Microsoft enterprise officiel. Le contenu de ces blogs fournit des conseils, des instructions et des recommandations sur l’audit qui vous aideront à améliorer la sécurité de votre infrastructure Active Directory et sont des ressources précieuses lors de la conception d’une stratégie d’audit.  
  
* [Audit d’accès objet global est magique](http://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) -décrit un mécanisme de contrôle appelé Advanced Configuration stratégie d’Audit qui a été ajouté à Windows 7 et Windows Server 2008 R2 qui permet de vous définir quels types de données que vous souhaitez auditer facilement et pas jongler avec les scripts et auditpol.exe.  
* [Présentation de l’audit des modifications dans Windows 2008](http://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx) -introduit les modifications audit apportées dans Windows Server 2008.  
* [Refroidir l’audit des astuces dans Vista et 2008](http://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx) -explique les fonctionnalités d’audit intéressantes de Windows Vista et Windows Server 2008 qui peut être utilisé pour la résolution des problèmes ou pour voir ce qui se passe dans votre environnement.  
* [Guichet unique pour l’audit dans Windows Server 2008 et Windows Vista](http://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx) -contient une compilation de l’audit des fonctionnalités et les informations contenues dans Windows Server 2008 et Windows Vista.  
  
Les liens suivants fournissent plus d’informations sur les améliorations apportées à Windows de l’audit dans Windows 8 et Windows Server 2012 et des informations sur les services AD DS l’audit dans Windows Server 2008.  
  
* [Quelles sont les nouveautés dans l’audit de sécurité](https://technet.microsoft.com/library/hh849638.aspx) -fournit une vue d’ensemble de fonctionnalités dans Windows 8 et Windows Server 2012 d’audit de sécurité de nouveau.  
* [Guide pas à pas d’audit d’AD DS](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx) -décrit la nouvelle fonctionnalité audit de Services de domaine Active Directory (AD DS) dans Windows Server 2008. Il fournit également des procédures pour implémenter cette nouvelle fonctionnalité.  
  
### <a name="windows-audit-categories"></a>Catégories d’Audit de Windows

Avant Windows Vista et Windows Server 2008, Windows avait uniquement neuf catégories de stratégie journal des événements d’audit :  
  
* Événements d’ouverture de session de compte  
* Gestion du compte  
* Accès au Service  
* Événements d’ouverture de session  
* Accès aux objets  
* Modification de la stratégie  
* Utilisation des privilèges  
* Suivi des processus  
* Événements du système  
  
Ces neuf catégories d’audit traditionnel constituent une stratégie d’audit. Chaque catégorie de stratégie d’audit peut être activé pour la réussite, échec et d’échec ou réussite événements. Leurs descriptions sont incluses dans la section suivante.  
  
#### <a name="audit-policy-category-descriptions"></a>Descriptions de catégorie de stratégie d’audit  
Les catégories de stratégie d’audit activer les types de message de journal des événements suivants.  
  
##### <a name="audit-account-logon-events"></a>Auditer les événements d’ouverture de session comptes  
Signale chaque instance d’un principal de sécurité (par exemple, utilisateur, ordinateur ou compte de service) qui est à connexion ou déconnexion d’un ordinateur dans lequel un autre ordinateur est utilisé pour valider le compte. Événements d’ouverture de session de compte sont générés lorsqu’un compte de principal de sécurité de domaine est authentifié sur un contrôleur de domaine. Authentification d’un utilisateur local sur un ordinateur local génère un événement d’ouverture de session qui est consigné dans le journal de sécurité local. Aucun événement de fermeture de session de compte n’est enregistrés.  
  
Cette catégorie génère un grand nombre de « bruit », car Windows rencontre constamment des comptes d’ouverture de session sur et désactivés sur les ordinateurs locaux et distants en cours normal de l’entreprise. Malgré tout, tout plan de sécurité doit inclure la réussite et Échec de cette catégorie d’audit.  
  
##### <a name="audit-account-management"></a>Gestion des comptes d’audit  
Ce paramètre d’audit détermine s’il faut effectuer le suivi de la gestion des utilisateurs et groupes. Par exemple, utilisateurs et groupes doivent être suivies lorsqu’un utilisateur ou compte d’ordinateur, un groupe de sécurité ou un groupe de distribution est créé, modifié ou supprimé. Quand un utilisateur ou compte d’ordinateur est renommé, désactivé ou activé ; ou qu’un mot de passe d’utilisateur ou d’ordinateur est modifié. Un événement peut être généré pour les utilisateurs ou groupes qui sont ajoutés ou supprimés à partir d’autres groupes.  
  
##### <a name="audit-directory-service-access"></a>Auditer l’accès au service d’annuaire  

Ce paramètre de stratégie détermine si pour auditer l’accès au principal de sécurité à un objet Active Directory qui a sa propre liste de contrôle d’accès spécifiée du système (SACL). En règle générale, cette catégorie doit uniquement être activée sur les contrôleurs de domaine. Lorsque l’option est activée, ce paramètre génère un grand nombre de « bruit ».  
  
##### <a name="audit-logon-events"></a>Auditer les événements d’ouverture de session  
Les événements d’ouverture de session sont générés lorsqu’un principal de sécurité local est authentifié sur un ordinateur local. Les événements d’ouverture de session enregistre les ouvertures de session de domaine qui se produisent sur l’ordinateur local. Événements de fermeture de session de compte ne sont pas générés. Lorsque l’option est activée, les événements d’ouverture de session génère un grand nombre de « bruit », mais elles doivent être activées par défaut dans n’importe quel plan d’audit de sécurité.  
  
##### <a name="audit-object-access"></a>Audit Object Access  
Accès aux objets peut générer des événements lorsque des objets définis par la suite avec l’audit est activé sont accessibles (par exemple, Opened, lecture, renommé, supprimé ou fermé). Une fois la catégorie d’audit principale est activée, l’administrateur doit définir individuellement les objets qui sera l’audit activé. Nombre d’objets système Windows sont fournis avec l’audit est activé, afin de l’activation de cette catégorie généralement commence à générer des événements avant que l’administrateur a défini une.  
  
Cette catégorie est très « bruyante » et génère des événements de cinq à dix chaque accès à un objet. Il peut être difficile pour les administrateurs de nouveau à l’audit d’objets obtenir des informations utiles. Elle doit être activée uniquement si nécessaire.  
  
##### <a name="auditing-policy-change"></a>L’audit des modifications de stratégie  
Ce paramètre de stratégie détermine s’il faut auditer chaque instance d’une modification de stratégies de droits d’utilisateur, les stratégies de pare-feu de Windows, les stratégies d’approbation ou les modifications apportées à la stratégie d’audit. Cette catégorie doit être activée sur tous les ordinateurs. Il génère très peu de bruit.  
  
##### <a name="audit-privilege-use"></a>Utilisation des privilèges d’audit  

Il existe des dizaines de droits d’utilisateur et les autorisations dans Windows (par exemple, d’ouverture de session en tant que traitement par lots et agir en tant que partie du système d’exploitation). Ce paramètre de stratégie détermine s’il faut auditer chaque instance d’une entité de sécurité en exerçant un droit d’utilisateur ou un privilège. L’activation des résultats de cette catégorie dans un grand nombre de « bruit », mais il peut être utile dans le suivi des comptes de principal de sécurité avec des privilèges élevés.  
  
##### <a name="audit-process-tracking"></a>Suivi du processus d’audit  
Ce paramètre de stratégie détermine s’il faut auditer le processus détaillé suivi des informations relatives aux événements tels que l’activation de programme, la sortie du processus, duplication de handle et accès indirect aux objets. Il est utile pour le suivi des utilisateurs malveillants et les programmes qu’ils utilisent.  
  
Activer le suivi des processus d’Audit génère un grand nombre d’événements, par conséquent, en général, elle est définie sur **aucun audit**. Toutefois, ce paramètre peut fournir un énorme avantage au cours de la réponse aux incidents à partir du journal détaillé des processus démarré et le temps qu’ils ont été lancées. Pour les contrôleurs de domaine et d’autres serveurs d’infrastructure du seul rôle, cette catégorie peut être activée en toute sécurité sur tout le temps. Serveurs de rôle unique ne génèrent pas de processus beaucoup suivi du trafic en cours normal de leurs fonctions. Par conséquent, être activés pour capturer les événements non autorisés s’ils se produisent.  
  
##### <a name="system-events-audit"></a>Audit des événements système  

Événements du système est presque une catégorie fourre-tout générique, l’inscription des différents événements ayant un impact sur l’ordinateur, sa sécurité système ou le journal de sécurité. Il inclut des événements pour arrêts de l’ordinateur redémarre, pannes d’alimentation, changement d’heure système, initialisations de package d’authentification, clearings de journal d’audit, des problèmes de l’emprunt d’identité et une multitude d’autres événements générales. En règle générale, l’activation de cette catégorie d’audit génère un grand nombre de « bruit », mais il génère suffisamment très utile d’événements qu’il est difficile à jamais ne recommandons pas l’activer.  
  
#### <a name="advanced-audit-policies"></a>Stratégies d’Audit avancée

À partir de Windows Vista et Windows Server 2008, Microsoft a amélioré la façon de sélections de catégorie de journal des événements peuvent être effectuées en créant des sous-catégories sous chaque catégorie d’audit principale. Les sous-catégories permettent à être beaucoup plus granulaire qu’il est impossible dans le cas contraire à l’aide les principales catégories d’audit. À l’aide de sous-catégories, vous pouvez activer uniquement les parties d’une catégorie principale particulière et ignorer la génération d’événements pour lesquels vous ne disposez d’aucune utilité. Chaque sous-catégorie d’audit peut être activée pour la réussite, l’échec, ou la réussite et l’échec.  
  
Pour répertorier toutes les sous-catégories d’audit disponibles, passez en revue le conteneur de stratégie d’Audit avancée dans un objet de stratégie de groupe, ou tapez la commande suivante à une invite de commandes sur n’importe quel ordinateur exécutant Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008, Windows 8, Windows 7 ou Windows Vista :  
  
`auditpol /list /subcategory:\*`
  
Pour obtenir une liste de sous-catégories d’audit actuellement configurés sur un ordinateur exécutant Windows Server 2012, Windows Server 2008 R2 ou Windows 2008, tapez la commande suivante :  
  
`auditpol /get /category:\*`
  
La capture d’écran suivante montre un exemple de auditpol.exe répertoriant la stratégie d’audit actuelle.  
  
![surveillance d’Active Directory](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_5.gif)  
  
> [!NOTE]  
> Stratégie de groupe ne signale pas toujours avec précision l’état de toutes les stratégies d’audit est activées, contrairement à auditpol.exe. Consultez [l’obtention de la stratégie d’Audit efficace dans Windows 7 et 2008 R2](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) pour plus d’informations.  
  
Chaque catégorie principale a plusieurs sous-catégories. Voici une liste de catégories, leurs sous-catégories et une description de leurs fonctions.  
  
### <a name="auditing-subcategories-descriptions"></a>Descriptions des sous-catégories d’audit  
Sous-catégories de stratégie d’audit activer les types de message de journal des événements suivants :  
  
#### <a name="account-logon"></a>Connexion au compte  
  
##### <a name="credential-validation"></a>Validation des informations d’identification  
Cette sous-catégorie signale les résultats des tests de validation sur les informations d’identification soumises à une demande d’ouverture de session du compte utilisateur. Ces événements se produisent sur l’ordinateur qui fait autorité pour les informations d’identification. Pour les comptes de domaine, le contrôleur de domaine fait autorité, tandis que pour les comptes locaux, l’ordinateur local est faisant autorité.  
  
Dans les environnements de domaine, la plupart des événements d’ouverture de session de compte est enregistrée dans le journal de sécurité des contrôleurs de domaine faisant autorité pour les comptes de domaine. Toutefois, ces événements peuvent se produire sur d’autres ordinateurs de l’organisation lorsque les comptes locaux sont utilisés pour se connecter.  
  
##### <a name="kerberos-service-ticket-operations"></a>Opérations de Ticket de Service Kerberos  
Cette sous-catégorie signale les événements générés par le processus de demande de ticket Kerberos du contrôleur de domaine qui fait autorité pour le compte de domaine.  
  
##### <a name="kerberos-authentication-service"></a>Service d’authentification Kerberos  
Cette sous-catégorie signale les événements générés par le service d’authentification Kerberos. Ces événements se produisent sur l’ordinateur qui fait autorité pour les informations d’identification.  
  
##### <a name="other-account-logon-events"></a>Autres événements d’ouverture de session de compte  
Cette sous-catégorie signale les événements qui se produisent en réponse aux informations d’identification soumis à une demande d’ouverture de session du compte utilisateur qui ne sont pas liées à la validation des informations d’identification ou les tickets Kerberos. Ces événements se produisent sur l’ordinateur qui fait autorité pour les informations d’identification. Pour les comptes de domaine, le contrôleur de domaine fait autorité, tandis que pour les comptes locaux, l’ordinateur local est faisant autorité.  
  
Dans les environnements de domaine, la plupart des événements d’ouverture de session de compte sont enregistrés dans le journal de sécurité des contrôleurs de domaine faisant autorité pour les comptes de domaine. Toutefois, ces événements peuvent se produire sur d’autres ordinateurs de l’organisation lorsque les comptes locaux sont utilisés pour se connecter. Exemples peuvent être les suivants :  
  
* Déconnexions de session Services Bureau à distance  
* Nouvelles sessions de Services Bureau à distance  
* Verrouillage et déverrouillage d’une station de travail  
* Appel d’un écran de veille  
* Faire disparaître un économiseur d’écran  
* Détection d’un Kerberos relire l’attaque, dans lequel une requête Kerberos avec des informations identiques est reçue à deux reprises  
* Accès à un réseau sans fil accordé à un compte d’utilisateur ou d’ordinateur  
* Accès à un câblé 802. 1 x réseau accordé à un utilisateur ou compte d’ordinateur  
  
#### <a name="account-management"></a>Gestion du compte  
  
##### <a name="user-account-management"></a>Gestion des comptes d’utilisateur  
Cette sous-catégorie signale chaque événement de gestion des comptes utilisateur, par exemple quand un compte d’utilisateur est créé, modifié ou supprimé ; un compte d’utilisateur est renommé, désactivé ou activé ; ou un mot de passe est défini ou modifié. Si ce paramètre de stratégie d’audit est activé, les administrateurs peuvent suivre les événements pour détecter malveillante, accidentelle et autorisée la création de comptes d’utilisateur.  
  
##### <a name="computer-account-management"></a>Gestion de compte d’ordinateur  
Cette sous-catégorie signale chaque événement de gestion de compte d’ordinateur, telles que quand un compte d’ordinateur est créé, modifié, supprimé, renommé, désactivé ou activé.  
  
##### <a name="security-group-management"></a>Gestion de groupe de sécurité  
Cette sous-catégorie signale chaque événement de la gestion de groupe de sécurité, telles que lorsqu’un groupe de sécurité est créé, modifié ou supprimé ou lorsqu’un membre est ajouté ou supprimé d’un groupe de sécurité. Si ce paramètre de stratégie d’audit est activé, les administrateurs peuvent suivre les événements pour détecter malveillante, accidentelle et autorisée la création de comptes de groupe de sécurité.  
  
##### <a name="distribution-group-management"></a>Gestion des groupes de distribution  
Cette sous-catégorie signale chaque événement de la gestion de groupe de distribution, par exemple quand un groupe de distribution est créé, modifié ou supprimé ou lorsqu’un membre est ajouté ou supprimé d’un groupe de distribution. Si ce paramètre de stratégie d’audit est activé, les administrateurs peuvent suivre les événements pour détecter malveillante et accidentelle autorisée la création de comptes de groupe.  
  
##### <a name="application-group-management"></a>Gestion de groupe d’applications  
Cette sous-catégorie signale chaque événement de groupe de gestion des applications sur un ordinateur, par exemple quand un groupe d’application est créé, modifié ou supprimé ou lorsqu’un membre est ajouté ou supprimé à partir d’un groupe d’applications. Si ce paramètre de stratégie d’audit est activé, les administrateurs peuvent suivre les événements pour détecter malveillante et accidentelle autorisée la création du groupe de comptes d’application.  
  
##### <a name="other-account-management-events"></a>Autres événements de gestion des comptes  
Cette sous-catégorie signale les autres événements de gestion des comptes.  
  
#### <a name="detailed-process-tracking"></a>Processus détaillé suivi  
  
##### <a name="process-creation"></a>Création de processus  
Cette sous-catégorie signale la création d’un processus et le nom de l’utilisateur ou d’un programme qui l’a créée.  
  
##### <a name="process-termination"></a>Arrêt du processus  
Cette sous-catégorie signale quand un processus se termine.  
  
##### <a name="dpapi-activity"></a>Activité DPAPI  
Cette sous-catégorie signale les chiffrer ou déchiffrer appelle le data protection application programming interface (DPAPI). DPAPI est utilisé pour protéger les informations secrètes comme mot de passe stockée et les informations de clé.  
  
##### <a name="rpc-events"></a>Événements RPC  
Cette procédure à distance de rapports de sous-catégorie appeler des événements de connexion (RPC).  
  
#### <a name="directory-service-access"></a>Accès au Service  
  
##### <a name="directory-service-access"></a>Accès au Service  
Cette sous-catégorie signale lors de l’accès à un objet AD DS. Seuls les objets avec des listes SACL configuré entraînent être généré et uniquement lorsqu’ils sont accessibles de manière qui correspond aux entrées de la liste SACL d’événements d’audit. Ces événements sont similaires aux événements de l’accès de service d’annuaire dans les versions antérieures de Windows Server. Cette sous-catégorie s’applique uniquement aux contrôleurs de domaine.  
  
##### <a name="directory-service-changes"></a>Modifications du Service d’annuaire  
Cette sous-catégorie signale les modifications apportées aux objets dans AD DS. Les types de modifications qui sont signalées sont créer, modifier, déplacer et restaurer les opérations qui sont effectuées sur un objet. Directory service l’audit des modifications, le cas échéant, indique les valeurs anciennes et nouvelles des propriétés modifiées des objets qui ont été modifiés. Seuls les objets avec des listes SACL entraînent être généré et uniquement lorsqu’ils sont accessibles d’une manière qui correspond à leurs entrées de la liste SACL d’événements d’audit. Certains objets et les propriétés ne provoquent pas de génération en raison des paramètres sur la classe d’objet dans le schéma d’événements d’audit. Cette sous-catégorie s’applique uniquement aux contrôleurs de domaine.  
  
##### <a name="directory-service-replication"></a>Réplication du Service d’annuaire  
Cette sous-catégorie signale lors de la réplication entre deux contrôleurs de domaine commence et se termine.  
  
##### <a name="detailed-directory-service-replication"></a>Réplication du Service d’annuaire détaillé  
Cette sous-catégorie signale des informations détaillées sur les informations répliquées entre les contrôleurs de domaine. Ces événements peuvent être très élevées dans le volume.  
  
#### <a name="logonlogoff"></a>Logon/Logoff  
  
##### <a name="logon"></a>Ouverture de session  
Cette sous-catégorie signale quand un utilisateur tente de se connecter au système. Ces événements se produisent sur l’ordinateur accessible. Pour les ouvertures de session interactive, la génération de ces événements se produit sur l’ordinateur qui a ouvert une session. Si une ouverture de session réseau se produit pour accéder à un partage, ces événements génèrent sur l’ordinateur qui héberge la ressource accessible. Si ce paramètre est configuré pour **aucun audit**, il est difficile ou impossible de déterminer quel utilisateur a accédé ou tenté d’accéder aux ordinateurs de l’organisation.  
  
##### <a name="network-policy-server"></a>Serveur NPS (Network Policy Server)  
Cette sous-catégorie signale les événements générés par les demandes d’accès utilisateur RADIUS (IAS) et la Protection d’accès réseau (NAP). Ces demandes peuvent être **Grant**, **Deny**, **ignorer**, **mise en quarantaine**, **verrou**et **Déverrouiller**. L’audit de ce paramètre entraîne un volume moyen ou élevé d’enregistrements sur les serveurs NPS et IAS.  
  
##### <a name="ipsec-main-mode"></a>Mode principal IPsec  
Cette sous-catégorie signale les résultats de la clé Exchange protocole IKE (Internet) et Internet AuthIP (Authenticated Protocol) au cours des négociations de Mode principal.  
  
##### <a name="ipsec-extended-mode"></a>Mode étendu IPsec  
Cette sous-catégorie signale les résultats d’AuthIP à celle des négociations en Mode étendu.  
  
##### <a name="other-logonlogoff-events"></a>Autres événements d’ouverture de session/fermeture de session  
Les rapports de cette sous-catégorie autres d’ouverture de session et les événements liés à la fermeture de session, tels que les Services Bureau à distance session se déconnecte et se reconnecte, avec la commande RunAs pour exécuter des processus sous un compte différent et le verrouillage et déverrouillage d’une station de travail.  
  
##### <a name="logoff"></a>Fermer la session  
Cette sous-catégorie signale quand un utilisateur se déconnecte du système. Ces événements se produisent sur l’ordinateur accessible. Pour les ouvertures de session interactive, la génération de ces événements se produit sur l’ordinateur qui a ouvert une session. Si une ouverture de session réseau se produit pour accéder à un partage, ces événements génèrent sur l’ordinateur qui héberge la ressource accessible. Si ce paramètre est configuré pour **aucun audit**, il est difficile ou impossible de déterminer quel utilisateur a accédé ou tenté d’accéder aux ordinateurs de l’organisation.  
  
##### <a name="account-lockout"></a>Verrouillage de compte  
Cette sous-catégorie signale quand un compte d’utilisateur est verrouillé en raison de trop nombreuses tentatives d’ouverture de session ayant échoué.  
  
##### <a name="ipsec-quick-mode"></a>Mode rapide IPsec  
Cette sous-catégorie signale les résultats du protocole IKE et AuthIP lors des négociations de Mode rapide.  
  
##### <a name="special-logon"></a>Ouverture de session spéciale  
Cette sous-catégorie signale quand une ouverture de session spéciale est utilisée. Une ouverture de session spéciale est une ouverture de session qui a des privilèges d’administrateur équivalent et peut être utilisé pour élever un processus à un niveau plus élevé.  
  
#### <a name="policy-change"></a>Modification de la stratégie  
  
##### <a name="audit-policy-change"></a>Modification de la stratégie d’audit  
Cette sous-catégorie signale les modifications apportées dans la stratégie d’audit, y compris les modifications de la liste SACL.  
  
##### <a name="authentication-policy-change"></a>Modification de la stratégie d’authentification  
Cette sous-catégorie signale les modifications apportées dans la stratégie d’authentification.  
  
##### <a name="authorization-policy-change"></a>Modification de la stratégie d’autorisation  
Cette sous-catégorie signale les modifications apportées dans la stratégie d’autorisation, y compris les modifications apportées aux autorisations (DACL).  
  
##### <a name="mpssvc-rule-level-policy-change"></a>Modification de la stratégie de niveau règle MPSSVC  
Cette sous-catégorie signale les modifications apportées dans les règles de stratégie utilisés par le Service de Protection de Microsoft (MPSSVC.exe). Ce service est utilisé par le pare-feu Windows.  
  
##### <a name="filtering-platform-policy-change"></a>Changement de stratégie de plateforme de filtrage  
Cette sous-catégorie signale l’ajout et la suppression d’objets à partir de la protection des fichiers Windows, notamment les filtres de démarrage. Ces événements peuvent être très élevées dans le volume.  
  
##### <a name="other-policy-change-events"></a>Autres événements de modification de stratégie  
Cette sous-catégorie signale les autres types de modifications de stratégie de sécurité telles que la configuration du Module de plateforme sécurisée (TPM) ou des fournisseurs de chiffrement.  
  
#### <a name="privilege-use"></a>Utilisation des privilèges  
  
##### <a name="sensitive-privilege-use"></a>Utilisation des privilèges sensibles  
Cette sous-catégorie signale quand un compte d’utilisateur ou le service utilise un privilège sensible. Un privilège sensible inclut les droits d’utilisateur suivants : agir en tant que partie du système d’exploitation, sauvegarder des fichiers et répertoires, créer un objet du jeton, de déboguer des programmes, permettre aux comptes d’ordinateur et utilisateur être approuvé pour la délégation, générer des audits de sécurité, emprunter l’identité d’un client après authentification, charger et décharger des pilotes de périphérique, gérer le journal d’audit et de sécurité, modifier les valeurs d’environnement du microprogramme, remplacer un jeton au niveau du processus, restaurer des fichiers et répertoires et prendre possession de fichiers ou d’autres objets. L’audit de cette sous-catégorie créera un volume élevé d’événements.  
  
##### <a name="nonsensitive-privilege-use"></a>Utilisation des privilèges affecter  
Cette sous-catégorie signale quand un compte d’utilisateur ou le service utilise un privilège affecter. Un privilège affecter inclut les droits d’utilisateur suivants : accéder au gestionnaire d’informations d’identification en tant qu’appelant approuvé, accéder à cet ordinateur à partir du réseau, ajouter des stations de travail au domaine, ajuster les quotas de mémoire pour un processus, autoriser la connexion localement, autoriser la connexion via à distance Desktop Services, le contournement du contrôle, transversal modifier l’heure système, créer un fichier d’échange, créer des objets globaux, créer des objets partagés permanents, créer des liens symboliques, refuser l’accès à cet ordinateur à partir du réseau, refuse la connexion en tant que traitement par lots, refuse la connexion en tant que service, refuse la connexion locale, refuse la connexion via les Services Bureau à distance, forcer l’arrêt à partir d’un système distant, augmenter une plage de travail de processus, augmenter la priorité de planification, verrouiller les pages en mémoire, connectez-vous en tant qu’un traitement par lots, ouvrez une session en tant que service, modifier une étiquette de l’objet, effectuer le volume tâches de maintenance, processus unique du profil, les performances du système de profil, supprimer l’ordinateur à partir de la station d’accueil, arrêter le système et synchroniser les données de service d’annuaire. L’audit de cette sous-catégorie créera un volume très élevé d’événements.  
  
##### <a name="other-privilege-use-events"></a>Autres événements d’utilisation des privilèges  
Ce paramètre de stratégie de sécurité n’est pas actuellement utilisé.  
  
#### <a name="object-access"></a>Accès aux objets  
  
##### <a name="file-system"></a>Système de fichiers  
Cette sous-catégorie signale lorsque les objets de système de fichiers sont accessibles. Uniquement les objets de système de fichiers avec des listes SACL provoquent des événements d’audit être généré et uniquement lorsqu’ils sont accessibles de manière que leurs entrées SACL correspondantes. En soi, ce paramètre de stratégie n’entraîne pas l’audit de tous les événements. Il détermine s’il faut auditer les événements d’un utilisateur qui accède à un objet de système de fichiers qui a une liste de contrôle d’accès spécifiée du système (SACL), efficacement l’activation de l’audit ait lieu.  
  
Si le paramètre Auditer l’accès aux objets est configuré pour **réussite**, une entrée d’audit est générée chaque fois qu’un utilisateur accède à un objet avec une SACL spécifiée. Si ce paramètre de stratégie est configuré pour **échec**, une entrée d’audit est générée chaque fois qu’un utilisateur ne parvient pas à accéder à un objet avec une SACL spécifiée.  
  
##### <a name="registry"></a>Registre  
Cette sous-catégorie signale lors de l’accès à des objets de Registre. Uniquement les objets de Registre avec des listes SACL provoquent des événements d’audit être généré et uniquement lorsqu’ils sont accessibles de manière que leurs entrées SACL correspondantes. En soi, ce paramètre de stratégie n’entraîne pas l’audit de tous les événements.  
  
##### <a name="kernel-object"></a>Objet de noyau  
Cette sous-catégorie signale lorsque des objets de noyau tels que les processus et les mutex sont accessibles. Uniquement les objets du noyau avec SACL provoquent des événements d’audit être généré et uniquement lorsqu’ils sont accessibles de manière que leurs entrées SACL correspondantes. En général, les objets de noyau reçoivent uniquement SACL si les options d’audit AuditBaseObjects ou AuditBaseDirectories sont activées.  
  
##### <a name="sam"></a>SAM  
Cette sous-catégorie signale lors de l’accès des objets de base de données de l’authentification Gestionnaire de comptes de sécurité (SAM) local.  
  
##### <a name="certification-services"></a>Services de certification  
Cette sous-catégorie signale lorsque les opérations de Services de Certification sont effectuées.  
  
##### <a name="application-generated"></a>Application générée  
Cette sous-catégorie génère quand des applications tentent de générer des événements d’audit à l’aide de l’audit des interfaces de programmation d’applications (API) de Windows.  
  
##### <a name="handle-manipulation"></a>Manipulation de handle  
Cette sous-catégorie signale quand un handle vers un objet est ouvert ou fermé. Seuls les objets avec des listes SACL déclencher ces événements être généré et uniquement si l’opération tentée handle correspond aux entrées de la liste SACL. Gérer les événements de Manipulation sont générées uniquement pour les types d’objets où la sous-catégorie d’accès objet correspondant est activée (par exemple, système de fichiers ou du Registre).  
  
##### <a name="file-share"></a>Partage de fichiers  
Cette sous-catégorie signale quand un partage de fichiers est accessible. En soi, ce paramètre de stratégie n’entraîne pas l’audit de tous les événements. Il détermine s’il faut auditer les événements d’un utilisateur qui accède à un objet de partage de fichier qui a une liste de contrôle d’accès spécifiée du système (SACL), efficacement l’activation de l’audit ait lieu.  
  
##### <a name="filtering-platform-packet-drop"></a>Rejet de paquets de plateforme de filtrage  
Cette sous-catégorie signale lorsque les paquets sont ignorés par le plateforme de filtrage de Windows (WFP). Ces événements peuvent être très élevées dans le volume.  
  
##### <a name="filtering-platform-connection"></a>Connexion de la plateforme de filtrage  
Cette sous-catégorie signale lorsque les connexions sont autorisées ou bloquées par WFP. Ces événements peuvent être élevées dans le volume.  
  
##### <a name="other-object-access-events"></a>Autres événements d’accès aux objets  
Cette sous-catégorie signale les autres événements de dépendant de l’accès d’objet tels que les travaux du Planificateur de tâches et des objets COM +.  
  
#### <a name="system"></a>System  
  
##### <a name="security-state-change"></a>Changement d’état de sécurité  
Cette sous-catégorie signale les modifications apportées dans l’état de sécurité du système, notamment lorsque le sous-système de sécurité démarre et s’arrête.  
  
##### <a name="security-system-extension"></a>Extension du système de sécurité  
Cette sous-catégorie signale le chargement du code d’extension tels que les packages de l’authentification par le sous-système de sécurité.  
  
##### <a name="system-integrity"></a>Intégrité du système  
Cette sous-catégorie de rapports sur les violations de l’intégrité du sous-système de sécurité.  
  
Pilote IPsec  
  
Cette sous-catégorie de rapports sur les activités du pilote Internet Protocol security (IPsec).  
  
##### <a name="other-system-events"></a>Autres événements système  
Cette sous-catégorie de rapports sur les autres événements système.  
  
Pour plus d’informations sur les descriptions de la sous-catégorie, reportez-vous à la [Microsoft Security Compliance Manager outil](https://technet.microsoft.com/library/cc677002.aspx).  
  
Chaque organisation doit consulter les catégories couvertes précédentes et les sous-catégories et activez celles qui correspondent le mieux à leur environnement. Modifications apportées à la stratégie d’audit doivent toujours être testées avant le déploiement dans un environnement de production.  
  
## <a name="configuring-windows-audit-policy"></a>Stratégie d’Audit de configuration Windows

Stratégie d’audit de Windows peut être définie à l’aide des modifications de stratégies, auditpol.exe, API ou du Registre de groupe. Les méthodes recommandées pour la configuration de stratégie d’audit pour la plupart des entreprises sont la stratégie de groupe ou auditpol.exe. Définition de la stratégie d’audit d’un système requiert des autorisations de compte de niveau administrateur ou les autorisations déléguées appropriées.  
  
> [!NOTE]  
> Le **gérer le journal d’audit et de sécurité** privilège doit être accordée à des principaux de sécurité (les administrateurs en disposent par défaut) pour autoriser la modification de l’audit des options des ressources individuelles, telles que les fichiers, Active l’accès aux objets Objets d’annuaire et les clés de Registre.  
  
### <a name="setting-windows-audit-policy-by-using-group-policy"></a>Définition de Windows la stratégie d’Audit à l’aide de stratégie de groupe

Pour définir la stratégie d’audit à l’aide de stratégies de groupe, configurez les catégories d’audit approprié situés sous **Computer Configuration\Windows Settings\Security Settings\Local Policies\Audit Policy** (voir la capture d’écran suivante pour un exemple à partir de l’éditeur de stratégie de groupe de Local (gpedit.msc)). Chaque catégorie de stratégie d’audit peut être activée pour **réussite**, **échec**, ou **réussite** et événements d’échec.  
  
![surveillance d’Active Directory](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_6.gif)  
  
Stratégie d’Audit avancée peut être définie à l’aide d’Active Directory ou des stratégies de groupe local. Pour définir la stratégie d’Audit avancée, configurez les sous-catégories appropriés, situés sous **ordinateur Configuration ordinateur\Paramètres Windows\Paramètres de stratégie avancée d’Audit** (voir la capture d’écran suivante pour obtenir un exemple à partir de l’ordinateur Local Éditeur stratégies de groupe (gpedit.msc)). Chaque sous-catégorie de stratégie d’audit peut être activée pour **réussite**, **échec**, ou **réussite** et **échec** événements.  
  
![surveillance d’Active Directory](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_7.gif)  
  
### <a name="setting-windows-audit-policy-using-auditpolexe"></a>Stratégie d’Audit de paramètre Windows à l’aide d’Auditpol.exe

Auditpol.exe (pour définir la stratégie d’audit de Windows) a été introduite dans Windows Server 2008 et Windows Vista. Initialement, auditpol.exe uniquement peut servir à définir la stratégie d’Audit avancée, mais la stratégie de groupe peut être utilisée dans Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008, Windows 8 et Windows 7.  
  
Auditpol.exe est un utilitaire de ligne de commande. La syntaxe est la suivante :  
  
`auditpol /set /<Category|Subcategory>:<audit category> /<success|failure:> /<enable|disable>`
  
Exemples de syntaxe Auditpol.exe :  
  
`auditpol /set /subcategory:"user account management" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"logon" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"IPSEC Main Mode" /failure:enable`
  
> [!NOTE]  
> Auditpol.exe définit stratégie d’Audit avancée localement. Si la stratégie locale est en conflit avec Active Directory ou de stratégie de groupe locale, les paramètres de stratégie de groupe prévalent généralement les paramètres auditpol.exe. Lorsque plusieurs conflits de stratégie locale ou le groupe existent, qu’une seule stratégie prévaut (qui est, remplacez). Stratégies d’audit ne seront pas fusionner.  
  
#### <a name="scripting-auditpol"></a>Écriture de scripts Auditpol

Microsoft fournit un [exemple de script](https://support.microsoft.com/kb/921469) aux administrateurs qui souhaitent pour définir la stratégie d’Audit avancée à l’aide d’un script au lieu de taper manuellement dans chaque commande auditpol.exe.  
  
**Remarque** stratégie de groupe ne signale pas toujours avec précision l’état de toutes les stratégies d’audit est activées, contrairement à auditpol.exe. Consultez [l’obtention de la stratégie d’Audit efficace dans Windows 7 et Windows 2008 R2](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) pour plus d’informations.  
  
#### <a name="other-auditpol-commands"></a>Autres commandes Auditpol

Auditpol.exe peut être utilisé pour enregistrer et restaurer une stratégie d’audit local et pour afficher d’autres commandes connexes d’audit. Voici l’autre **auditpol** commandes.  
  
`auditpol /clear` -Utilisé pour supprimer et réinitialiser les stratégies d’audit local  
  
`auditpol /backup /file:<filename>` -Utilisé pour sauvegarder une stratégie d’audit local en cours dans un fichier binaire  
  
`auditpol /restore /file:<filename>` -Utilisé pour importer un fichier de stratégie d’audit précédemment enregistré dans une stratégie d’audit local  
  
`auditpol /<get/set> /option:<CrashOnAuditFail> /<enable/disable>` -Si ce paramètre de stratégie d’audit est activé, il provoque le système immédiatement (avec arrêt : C0000244 {Échec d’Audit} message) si une sécurité d’audit ne peut pas être connecté pour une raison quelconque. En règle générale, un événement ne peut pas être consigné quand le journal d’audit de sécurité est plein et la méthode de rétention spécifiée pour le journal de sécurité est **ne pas remplacer les événements** ou **remplacer les événements par jours**. En général, il est activé uniquement par les environnements qui doivent garantir que le journal de sécurité se connecte. Si activé, les administrateurs doivent étroitement regarder la taille de journal de sécurité et faire tourner les journaux en fonction des besoins. Elle peut également être définie avec la stratégie de groupe en modifiant l’option de sécurité **Audit : Arrêter le système immédiatement s’il est impossible d’enregistrer les audits de sécurité** (par défaut = désactivé).  
  
`auditpol /<get/set> /option:<AuditBaseObjects> /<enable/disable>` : Ce paramètre de stratégie d’audit détermine s’il faut auditer l’accès des objets système globaux. Si cette stratégie est activée, les objets du système, telles que les mutex, les événements, les sémaphores et les périphériques de déni de service sont créés avec une liste de contrôle d’accès système (SACL) par défaut. La plupart des administrateurs d’envisager l’audit des objets système globaux pour être trop « bruyantes », et ils seront uniquement Activez-le si piratage malveillant est suspectée. Uniquement les objets nommés bénéficient d’une liste SACL. Si l’audit objet accès d’audit stratégie (ou objet de noyau d’audit sous-catégorie) est également activé, l’accès à ces objets système est audité. Lorsque vous configurez ce paramètre de sécurité, modifications ne prendront effet qu’après le redémarrage de Windows. Cette stratégie peut également être définie avec la stratégie de groupe en modifiant l’option de sécurité Auditer l’accès des objets système globaux (par défaut = désactivé).  
  
`auditpol /<get/set> /option:<AuditBaseDirectories> /<enable/disable>` : Ce paramètre de stratégie d’audit spécifie que les objets du noyau nommés (par exemple, les mutex et les sémaphores) sont indiquées SACL lorsqu’elles sont créées. AuditBaseDirectories affecte les objets conteneur tandis que AuditBaseObjects affecte les objets qui ne peut pas contenir d’autres objets.  
  
`auditpol /<get/set> /option:<FullPrivilegeAuditing> /<enable/disable>` : Ce paramètre de stratégie d’audit spécifie si le client génère un événement lorsqu’un ou plusieurs de ces privilèges sont attribués à un jeton de sécurité : AssignPrimaryTokenPrivilege, AuditPrivilege, BackupPrivilege, CreateTokenPrivilege, DebugPrivilege, EnableDelegationPrivilege, ImpersonatePrivilege, LoadDriverPrivilege, RestorePrivilege, SecurityPrivilege, SystemEnvironmentPrivilege, TakeOwnershipPrivilege et TcbPrivilege. Si cette option n’est pas activée (valeur par défaut = désactivé), les privilèges BackupPrivilege et RestorePrivilege ne sont pas enregistrés. L’activation de cette option peut rendre le journal de sécurité sont très bruyant (parfois des centaines d’événements une seconde) pendant une opération de sauvegarde. Cette stratégie peut également être définie avec la stratégie de groupe en modifiant l’option de sécurité **Audit : Auditer l’utilisation des privilèges de sauvegarde et restauration**.  
  
> [!NOTE]  
> Certaines informations fournies ici a été effectuées à partir de Microsoft [Type d’Option d’Audit](https://msdn.microsoft.com/library/dd973862(prot.20).aspx) et l’outil Microsoft SCM.  
  
## <a name="enforcing-traditional-auditing-or-advanced-auditing"></a>Mettre en œuvre l’audit traditionnel ou audit avancé

Dans Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows 8, Windows 7 et Windows Vista, les administrateurs peuvent choisir activer les neuf catégories traditionnels ou utiliser les sous-catégories. Il est un choix binaire qui doit être effectué dans chaque système Windows. Les principales catégories peuvent être activés ou le subcategoriesit ne peut pas être à la fois.  
  
Pour empêcher la stratégie héritée catégorie traditionnel de remplacer des sous-catégories de stratégie d’audit, vous devez activer le **paramètres de sous-catégorie de stratégie d’audit (Windows Vista ou version ultérieure) pour remplacer les paramètres de catégorie de stratégie d’audit** paramètre de stratégie se trouve sous **Configuration ordinateur\Paramètres Windows\Paramètres de sécurité\Stratégies Locales\options de sécurité**.  
  
Nous recommandons que les sous-catégories être activées et configurées au lieu des neuf catégories principales. Cela nécessite qu’un paramètre de stratégie de groupe (pour permettre des sous-catégories remplacer les catégories d’audit) ainsi que la configuration des différentes sous-catégories qui prennent en charge les stratégies d’audit.  
  
Sous-catégories d’audit peut être configuré à l’aide de plusieurs méthodes, notamment la stratégie de groupe et que le programme de ligne de commande, auditpol.exe.  
  
## <a name="next-steps"></a>Étapes suivantes
  
* [Fonctions avancées de sécurité d’audit dans Windows 7 et Windows Server 2008 R2](https://social.technet.microsoft.com/wiki/contents/articles/advanced-security-auditing-in-windows-7-and-windows-server-2008-r2.aspx)  
  
* [Audit et conformité dans Windows Server 2008](https://technet.microsoft.com/magazine/2008.03.auditing.aspx)  
  
* [Comment utiliser la stratégie de groupe pour configurer les paramètres pour les ordinateurs basés sur Windows Server 2008 et Windows Vista dans un domaine Windows Server 2008, dans un domaine Windows Server 2003 ou dans un domaine Windows 2000 d’audit de sécurité détaillés](https://support.microsoft.com/kb/921469)  
  
* [Advanced Guide pas à pas des stratégie d’Audit de sécurité](https://technet.microsoft.com/library/dd408940(WS.10).aspx)  
