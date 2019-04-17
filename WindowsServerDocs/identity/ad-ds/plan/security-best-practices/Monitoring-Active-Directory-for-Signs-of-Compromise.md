---
ms.assetid: a7ef2fba-b05c-4be2-93b2-b9456244c3ad
title: "Surveillance des signes de compromission d’ActiveDirectory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 096f2fa58b9aae53a06bf26c2107eb4cee3aa5d9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="monitoring-active-directory-for-signs-of-compromise"></a>Surveillance des signes de compromission d’ActiveDirectory

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

*La loi du nombre de cinq: vigilance externe est le prix de sécurité.* - [10lois immuables de l’Administration de la sécurité](https://technet.microsoft.com/library/cc722488.aspx)  
  
Un journal des événements solid surveillance du système est une partie essentielle de toute conception d’Active Directory sécurisée. Nombreuses violations de sécurité d’ordinateur peuvent être détectées au début de l’événement si victimes mises en œuvre le journal des événements approprié surveillance et d’alerte. Rapports indépendants ont temps pris en charge cette conclusion. Par exemple, le [2009 Verizon données violation rapport](http://www.verizonbusiness.com/resources/security/reports/2009_databreach_rp.pdf) états:  
  
«L’inefficacité apparente d’analyse de surveillance et le journal des événements continue d’être un peu d’enigma. La possibilité pour la détection est enquêteurs de noter que 66 % de leurs victimes a suffisamment d’éléments disponible dans les journaux pour découvrir la violation s’ils avaient été plus soucieux de l’analyse de ces ressources.»  
  
Ce manque de journaux d’événements active reste une faille cohérente dans les plans de défense de sécurité de nombreuses sociétés. Le [rapport de divulgation de données Verizon 2012](http://www.verizonbusiness.com/resources/reports/rp_data-breach-investigations-report-2012_en_xg.pdf) trouvé que bien que 85 % des violations a eu plusieurs semaines pour être remarqué, 84 % des victimes avait preuve de la violation dans les journaux des événements.  
  
## <a name="windows-audit-policy"></a>Stratégie d’Audit de Windows  
Voici des liens vers le blog de prise en charge officielle d’entreprise Microsoft. Le contenu de ces blogs fournit des conseils, des conseils et des recommandations sur l’audit qui vous aideront à améliorer la sécurité de votre infrastructure Active Directory et sont une ressource précieuse lors de la conception d’une stratégie d’audit.  
  
-   [Audit d’accès objet global est magique](http://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) -décrit un mécanisme de contrôle appelé Advanced Configuration stratégie d’Audit qui a été ajouté à Windows 7 et Windows Server 2008 R2 qui permet de vous définir les types de données que vous voulez auditer facilement et pas jongler avec scripts et auditpol.exe.  
  
-   [Présentation de l’audit des modifications dans Windows 2008](http://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx) -introduit l’audit apportées dans Windows Server 2008.  
  
-   [Refroidir astuces de l’audit dans Vista et 2008](http://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx) -explique les fonctionnalités d’audit intéressantes de Windows Vista et Windows Server 2008 qui peut être utilisé pour la résolution des problèmes ou pour voir ce qui se passe dans votre environnement.  
  
-   [Emplacement centralisé pour l’audit dans Windows Server 2008 et Windows Vista](http://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx) -contient une compilation de l’audit des fonctionnalités et les informations contenues dans Windows Server 2008 et Windows Vista.  
  
Les liens suivants fournissent des informations sur les améliorations apportées à Windows de l’audit dans Windows 8 et Windows Server 2012 et sur les services AD DS de l’audit dans Windows Server 2008.  
  
-   [Nouveautés de l’audit de sécurité](https://technet.microsoft.com/library/hh849638.aspx) -fournit une vue d’ensemble des nouvelles fonctionnalités dans Windows 8 et Windows Server 2012 d’audit de sécurité.  
  
-   [Guide pas à pas l’audit d’AD DS](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx) -décrit la nouvelle fonctionnalité d’audit de Services de domaine Active Directory (AD DS) dans Windows Server 2008. Il fournit également des procédures pour implémenter cette nouvelle fonctionnalité.  
  
### <a name="windows-audit-categories"></a>Catégories d’Audit de Windows  
Avant Windows Vista et Windows Server 2008, Windows avait neuf uniquement les catégories de stratégie d’audit journal des événements:  
  
-   Événements d’ouverture de session de compte  
  
-   Gestion des comptes  
  
-   Accès au Service d’annuaire  
  
-   Événements d’ouverture de session  
  
-   Accès aux objets  
  
-   Modification de la stratégie  
  
-   Utilisation de privilèges  
  
-   Suivi des processus  
  
-   Événements du système  
  
Ces catégories d’audit traditionnel neuf constituent une stratégie d’audit. Chaque catégorie de stratégie d’audit peut être activé pour la réussite, échec, ou réussite et Échec événements. Leurs descriptions sont incluses dans la section suivante.  
  
#### <a name="audit-policy-category-descriptions"></a>Descriptions des catégories de stratégie d’audit  
Les catégories de stratégie d’audit activer les types de message de journal des événements suivants.  
  
##### <a name="audit-account-logon-events"></a>Auditer les événements d’ouverture de session comptes  
Chaque instance d’une entité de sécurité (par exemple, utilisateur, ordinateur ou compte de service) qui est de connexion ou de déconnexion d’un ordinateur dans lequel un autre ordinateur est utilisé pour valider le compte de rapports. Événements d’ouverture de session de compte sont générés lorsqu’un compte de sécurité de domaine principal est authentifié sur un contrôleur de domaine. Authentification d’un utilisateur local sur un ordinateur local génère un événement d’ouverture de session qui est consigné dans le journal de sécurité local. Aucun événement de fermeture de session de compte n’est enregistrés.  
  
Cette catégorie génère un grand nombre de «bruit» dans la mesure où Windows est constamment comptes une session sur et désactivés sur les ordinateurs locaux et distants en cours normal de l’entreprise. Toujours, n’importe quel plan de sécurité doit inclure la réussite ou l’échec de cette catégorie d’audit.  
  
##### <a name="audit-account-management"></a>Auditer la gestion des comptes  
Ce paramètre d’audit détermine s’il faut suivre la gestion des groupes et utilisateurs. Par exemple, les utilisateurs et les groupes doivent être suivis lorsqu’un utilisateur ou compte d’ordinateur, un groupe de sécurité ou un groupe de distribution est créé, modifié ou supprimé. Lorsqu’un compte d’utilisateur ou l’ordinateur est renommé, désactivé ou activé; ou qu’un mot de passe utilisateur ou d’ordinateur est modifié. Un événement peut être généré pour les utilisateurs ou groupes qui sont ajoutés ou supprimés à partir d’autres groupes.  
  
##### <a name="audit-directory-service-access"></a>Auditer l’accès au Service d’annuaire  

Ce paramètre de stratégie détermine si à auditer l’accès principal de sécurité à un objet Active Directory qui possède sa propre liste de contrôle d’accès spécifié système (SACL). En règle générale, cette catégorie doit uniquement être activée sur les contrôleurs de domaine. Lorsque activé, ce paramètre génère un grand nombre de «bruit».  
  
##### <a name="audit-logon-events"></a>Auditer les événements d’ouverture de session  
Événements d’ouverture de session sont générés lorsqu’une entité de sécurité local est authentifiée sur un ordinateur local. Événements d’ouverture de session enregistre les ouvertures de session de domaine qui se produisent sur l’ordinateur local. Événements de fermeture de session de compte ne sont pas générés. Lorsque activé, les événements d’ouverture de session génère un grand nombre de «bruit», mais ils doivent être activés par défaut dans n’importe quel plan d’audit de sécurité.  
  
##### <a name="audit-object-access"></a>Auditer l’accès aux objets  
Accès aux objets peut générer des événements lorsque vous accédez à des objets définis par la suite à l’audit est activé (par exemple, Opened, lecture, renommé, supprimé ou fermée). Une fois la catégorie d’audit principale est activée, l’administrateur doit définir individuellement les objets sera l’audit activé. Nombre d’objets système Windows sont fournis avec l’audit est activé, afin de l’activation de cette catégorie généralement commence à générer des événements avant que l’administrateur a défini les.  
  
Cette catégorie est très «bruyante» et génère des événements de 5 à 10 pour chaque accès à l’objet. Il peut être difficile pour les administrateurs à nouveau à l’audit des objets obtenir des informations utiles. Il doit être activé uniquement si nécessaire.  
  
##### <a name="auditing-policy-change"></a>Modification de la stratégie d’audit  
Ce paramètre de stratégie détermine s’il convient d’auditer chaque instance d’un changement de stratégies de droits d’utilisateur, les stratégies de pare-feu Windows, les stratégies d’approbation ou les modifications apportées à la stratégie d’audit. Cette catégorie doit être activée sur tous les ordinateurs. Il génère très peu de bruit.  
  
##### <a name="audit-privilege-use"></a>Utilisation des privilèges d’audit  

Il existe des dizaines de droits et autorisations dans Windows (par exemple, d’ouverture de session en tant que traitement par lots et agir en tant que partie du système d’exploitation). Ce paramètre de stratégie détermine s’il faut auditer chaque instance d’une entité de sécurité en testant un droit d’utilisateur ou un privilège. L’activation de résultats de cette catégorie dans un grand nombre de «bruit», mais il peut être utile à effectuer le suivi des comptes de sécurité principal à l’aide de privilèges élevés.  
  
##### <a name="audit-process-tracking"></a>Suivi des processus  
Ce paramètre de stratégie détermine s’il faut auditer le suivi des informations pour les événements tels que l’activation de programme, sortie de processus, duplication de pointeur et accès indirect aux objets de processus détaillé. Il est utile pour le suivi des utilisateurs malveillants et les programmes qu’ils utilisent.  
  
Activer le suivi des processus d’Audit génère un grand nombre d’événements, il est généralement **aucun audit**. Toutefois, ce paramètre peut fournir un énorme avantage pendant une réponse aux incidents à partir du journal détaillée de processus démarrés et le temps qu’ils ont été lancées. Pour les contrôleurs de domaine et les autres serveurs d’infrastructure de rôle unique, cette catégorie peut être en toute sécurité allumée tout le temps. Les serveurs de rôle unique ne génèrent pas de processus beaucoup du trafic de suivi des cours normal de leurs fonctions. Par conséquent, ils peuvent être activés pour capturer les événements non autorisés s’ils se produisent.  
  
##### <a name="system-events-audit"></a>Audit des événements système  

Événements du système est presque une catégorie fourre-tout générique, inscrire des différents événements ayant un impact sur l’ordinateur, sa sécurité système ou le journal de sécurité. Il inclut des événements pour les arrêts de l’ordinateur et redémarre, pannes d’alimentation, les changements d’heure système, initialisations de package d’authentification, clearings du journal d’audit, problèmes d’emprunt d’identité et une multitude d’autres événements générales. En règle générale, l’activation de cette catégorie d’audit génère un grand nombre de «bruit», mais il génère suffisamment d’événements très utiles qu’il est difficile de jamais ne recommandons pas l’activer.  
  
#### <a name="advanced-audit-policies"></a>Stratégies d’Audit avancée  
À partir de Windows Vista et Windows Server 2008, Microsoft a amélioré la façon de sélections de catégories du journal des événements peuvent être effectuées en créant des sous-catégories sous chaque catégorie d’audit principale. Les sous-catégories permettent à l’audit d’être beaucoup plus granulaire qu’il a pu sinon en utilisant les catégories principales. À l’aide de sous-catégories, vous pouvez activer uniquement les parties d’une catégorie principale particulière et ignorer la génération des événements pour lesquels vous ne disposez d’aucune utilité. Chaque sous-catégorie de stratégie d’audit peut être activé pour la réussite, échec, ou réussite et Échec événements.  
  
Pour répertorier toutes les sous-catégories d’audit disponibles, passez en revue le conteneur de stratégie d’Audit avancée dans un objet de stratégie de groupe, ou tapez la commande suivante à l’invite de commande sur n’importe quel ordinateur exécutant Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008, Windows 8, Windows 7 ou Windows Vista:  
  
**AuditPol /list/SubCategory: \ ***  
  
Pour obtenir une liste des sous-catégories d’audit actuellement configurés sur un ordinateur exécutant Windows Server 2012, Windows Server 2008 R2 ou Windows 2008, tapez la commande suivante:  
  
**AuditPol /Get / Category: \ ***  
  
La capture d’écran suivante montre un exemple de auditpol.exe description dans la stratégie d’audit actuelle.  
  
![analyser Active Directory.](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_5.gif)  
  
> [!NOTE]  
> Stratégie de groupe ne signale pas toujours avec précision l’état de toutes les stratégies d’audit activés, contrairement aux auditpol.exe. Voir [mise en route de la stratégie d’Audit actuelle dans Windows 7 et 2008 R2](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) pour plus d’informations.  
  
Chaque catégorie principale a plusieurs sous-catégories. Vous trouverez ci-dessous une liste des catégories, leurs sous-catégories et une description de leurs fonctions.  
  
### <a name="auditing-subcategories-descriptions"></a>Descriptions des sous-catégories d’audit  
Les sous-catégories de stratégie d’audit activer les types de message de journal des événements suivants:  
  
#### <a name="account-logon"></a>Ouverture de session de compte  
  
##### <a name="credential-validation"></a>Validation des informations d’identification  
Cette sous-catégorie signale les résultats des tests de validation sur les informations d’identification soumises à une demande d’ouverture de session du compte utilisateur. Ces événements se produisent sur l’ordinateur qui fait autorité pour les informations d’identification. Pour les comptes de domaine, le contrôleur de domaine fait autorité, tandis que pour les comptes locaux, l’ordinateur local est faisant autorité.  
  
Dans les environnements de domaine, la plupart des événements d’ouverture de session de compte est consignée dans le journal de sécurité des contrôleurs de domaine faisant autorité pour les comptes de domaine. Toutefois, ces événements peuvent se produire sur d’autres ordinateurs de l’organisation lorsque les comptes locaux sont utilisés pour se connecter.  
  
##### <a name="kerberos-service-ticket-operations"></a>Opérations de Ticket de Service Kerberos  
Cette sous-catégorie signale les événements générés par le processus de demande de ticket Kerberos du contrôleur de domaine faisant autorité pour le compte de domaine.  
  
##### <a name="kerberos-authentication-service"></a>Service d’authentification Kerberos  
Cette sous-catégorie signale les événements générés par le service d’authentification Kerberos. Ces événements se produisent sur l’ordinateur qui fait autorité pour les informations d’identification.  
  
##### <a name="other-account-logon-events"></a>Autres événements d’ouverture de session de compte  
Cette sous-catégorie signale les événements qui se produisent en réponse à des informations d’identification soumis à une demande d’ouverture de session du compte utilisateur qui ne sont pas liées à la validation des informations d’identification ou les tickets Kerberos. Ces événements se produisent sur l’ordinateur qui fait autorité pour les informations d’identification. Pour les comptes de domaine, le contrôleur de domaine fait autorité, tandis que pour les comptes locaux, l’ordinateur local est faisant autorité.  
  
Dans les environnements de domaine, la plupart des événements d’ouverture de session de compte sont enregistrés dans le journal de sécurité des contrôleurs de domaine faisant autorité pour les comptes de domaine. Toutefois, ces événements peuvent se produire sur d’autres ordinateurs de l’organisation lorsque les comptes locaux sont utilisés pour se connecter. Voici quelques exemples peuvent:  
  
-   Déconnexions de session de Services Bureau à distance  
  
-   Nouvelles sessions des Services Bureau à distance  
  
-   Verrouillage et déverrouillage d’une station de travail  
  
-   Appel d’un écran de veille  
  
-   Masquage d’un écran de veille  
  
-   La détection d’un Kerberos relire attaque, dans laquelle une demande de Kerberos avec des informations identiques est reçue deux fois  
  
-   Accès à un réseau sans fil accordé à un compte d’utilisateur ou d’ordinateur  
  
-   Accès à un filaire 802. 1 x réseau accordé à un compte d’utilisateur ou d’ordinateur  
  
#### <a name="account-management"></a>Gestion des comptes  
  
##### <a name="user-account-management"></a>Gestion des comptes utilisateur  
Cette sous-catégorie signale chaque événement de gestion des comptes utilisateur, par exemple, quand un compte d’utilisateur est créé, modifié ou supprimé. un compte d’utilisateur est renommé, désactivé ou activé; ou un mot de passe est défini ou modifié. Si ce paramètre de stratégie d’audit est activé, les administrateurs peuvent suivre les événements pour détecter malveillante, accidentelle et autorisée la création des comptes d’utilisateur.  
  
##### <a name="computer-account-management"></a>Gestion de compte d’ordinateur  
Cette sous-catégorie signale chaque événement de la gestion de compte d’ordinateur, par exemple, quand un compte d’ordinateur est créé, modifié, supprimé, renommé, désactivé ou activé.  
  
##### <a name="security-group-management"></a>Gestion de groupe de sécurité  
Cette sous-catégorie signale chaque événement de la gestion de groupe de sécurité, par exemple, lorsqu’un groupe de sécurité est créé, modifié ou supprimé ou lorsqu’un membre est ajouté ou supprimé d’un groupe de sécurité. Si ce paramètre de stratégie d’audit est activé, les administrateurs peuvent suivre les événements pour détecter malveillante, accidentelle et autorisée, la création de comptes de groupe de sécurité.  
  
##### <a name="distribution-group-management"></a>Gestion de groupe de distribution  
Cette sous-catégorie signale chaque événement de la gestion de groupe de distribution, par exemple, lorsqu’un groupe de distribution est créé, modifié ou supprimé ou lorsqu’un membre est ajouté ou supprimé d’un groupe de distribution. Si ce paramètre de stratégie d’audit est activé, les administrateurs peuvent suivre les événements pour détecter malveillante, accidentelle et autorisée, la création de comptes de groupe.  
  
##### <a name="application-group-management"></a>Gestion de groupe de l’application  
Cette sous-catégorie rapports chaque événement d’application Gestion de groupe sur un ordinateur, par exemple, lorsqu’un groupe d’applications est créé, modifié ou supprimé ou lorsqu’un membre est ajouté ou supprimé à partir d’un groupe d’applications. Si ce paramètre de stratégie d’audit est activé, les administrateurs peuvent suivre les événements pour détecter malveillante, accidentelle et autorisée la création de comptes de groupe d’application.  
  
##### <a name="other-account-management-events"></a>Autres événements de gestion des comptes  
Cette sous-catégorie signale les autres événements de gestion des comptes.  
  
#### <a name="detailed-process-tracking"></a>Suivi des processus détaillé  
  
##### <a name="process-creation"></a>Création de processus  
Cette sous-catégorie signale la création d’un processus et le nom de l’utilisateur ou un programme qui l’a créé.  
  
##### <a name="process-termination"></a>Arrêt de processus  
Cette sous-catégorie signale un processus se termine.  
  
##### <a name="dpapi-activity"></a>Activité DPAPI  
Cette sous-catégorie rapports chiffrer ou déchiffrer appelle le données protection application programming interface (DPAPI). DPAPI est utilisé pour protéger les informations secrètes comme mot de passe stocké et informations de clé.  
  
##### <a name="rpc-events"></a>Événements RPC  
Cette procédure à distance de rapports sous-catégorie appeler les événements de connexion (RPC).  
  
#### <a name="directory-service-access"></a>Accès au Service d’annuaire  
  
##### <a name="directory-service-access"></a>Accès au Service d’annuaire  
Cette sous-catégorie signale un objet de domaine Active Directory est accessible. Seuls les objets avec SACL configuré provoquent des événements d’audit et d’être généré, uniquement lorsqu’ils sont accessibles de manière qui correspond aux liste SACL entrées. Ces événements sont similaires aux événements de l’accès de service d’annuaire dans les versions antérieures de Windows Server. Cette sous-catégorie s’applique uniquement aux contrôleurs de domaine.  
  
##### <a name="directory-service-changes"></a>Modifications du Service d’annuaire  
Cette sous-catégorie signale les modifications apportées aux objets dans AD DS. Les types de modifications qui sont signalées sont créer, modifier, déplacer et restaurer des opérations qui sont effectuées sur un objet. Répertoire service audit de modification, le cas échéant, indique les valeurs anciennes et nouvelles des propriétés modifiées des objets qui ont été modifiés. Uniquement les objets ayant des listes SACL provoquent des événements d’audit et d’être généré, uniquement lorsqu’ils sont accessibles de manière qui correspond à leurs entrées de liste SACL. Certains objets et les propriétés ne provoquent pas les événements d’audit être générés en raison de paramètres sur la classe d’objet dans le schéma. Cette sous-catégorie s’applique uniquement aux contrôleurs de domaine.  
  
##### <a name="directory-service-replication"></a>Réplication du Service d’annuaire  
Cette sous-catégorie signale que la réplication entre deux contrôleurs de domaine commence et se termine.  
  
##### <a name="detailed-directory-service-replication"></a>Réplication du Service d’annuaire détaillé  
Cette sous-catégorie signale des informations détaillées sur les informations répliquées entre les contrôleurs de domaine. Ces événements peuvent être très élevées en volume.  
  
#### <a name="logonlogoff"></a>Ouverture/fermeture de session  
  
##### <a name="logon"></a>Ouverture de session  
Cette sous-catégorie signale un utilisateur tente d’ouvrir une session sur le système. Ces événements se produisent sur l’ordinateur auquel vous accédez. Pour les ouvertures de session interactive, la génération de ces événements se produit sur l’ordinateur qui a ouvert une session. Si une ouverture de session réseau s’effectue pour accéder à un partage, ces événements génèrent sur l’ordinateur qui héberge la ressource utilisée. Si ce paramètre est configuré pour **aucun audit**, il est difficile voire impossible de déterminer quel utilisateur a accédé ou a tenté d’accéder aux ordinateurs de l’organisation.  
  
##### <a name="network-policy-server"></a>Serveur de stratégie réseau  
Cette sous-catégorie signale les événements générés par les demandes d’accès utilisateur RADIUS (IAS) et la Protection d’accès réseau (NAP). Ces requêtes peuvent être **Grant**, **refuser**, **ignorer**, **quarantaine**, **verrouillage**, et **Unlock**. L’audit de ce paramètre entraîne un volume moyen ou élevé d’enregistrements sur les serveurs NPS et IAS.  
  
##### <a name="ipsec-main-mode"></a>Mode principal IPsec  
Cette sous-catégorie signale les résultats de la clé Exchange protocole IKE (Internet) et Internet AuthIP (Authenticated Protocol) durant les négociations de Mode principal.  
  
##### <a name="ipsec-extended-mode"></a>Mode étendu IPsec  
Cette sous-catégorie signale les résultats du AuthIP lors des négociations de Mode étendu.  
  
##### <a name="other-logonlogoff-events"></a>D’autres événements d’ouverture/fermeture de session  
Cette sous-catégorie rapports autres d’ouverture de session et les événements liés à la fermeture de session, tels que des Services Bureau à distance session se déconnecte et se reconnecte, à l’aide d’identification pour exécuter des processus sous un compte différent et le verrouillage et déverrouillage d’une station de travail.  
  
##### <a name="logoff"></a>Fermeture de session  
Cette sous-catégorie signale un utilisateur se déconnecte du système. Ces événements se produisent sur l’ordinateur auquel vous accédez. Pour les ouvertures de session interactive, la génération de ces événements se produit sur l’ordinateur qui a ouvert une session. Si une ouverture de session réseau s’effectue pour accéder à un partage, ces événements génèrent sur l’ordinateur qui héberge la ressource utilisée. Si ce paramètre est configuré pour **aucun audit**, il est difficile voire impossible de déterminer quel utilisateur a accédé ou a tenté d’accéder aux ordinateurs de l’organisation.  
  
##### <a name="account-lockout"></a>Verrouillage de compte  
Cette sous-catégorie signale un compte d’utilisateur est verrouillé suite à un trop grand nombre de tentatives de connexion.  
  
##### <a name="ipsec-quick-mode"></a>Mode rapide IPsec  
Cette sous-catégorie signale les résultats du protocole IKE et AuthIP lors des négociations de Mode rapide.  
  
##### <a name="special-logon"></a>Ouverture de session spéciale  
Cette sous-catégorie signale une ouverture de session spéciale est utilisé. Une ouverture de session spéciale est une ouverture de session qui dispose des privilèges d’administrateur équivalent et peut être utilisé pour élever un processus à un niveau supérieur.  
  
#### <a name="policy-change"></a>Modification de la stratégie  
  
##### <a name="audit-policy-change"></a>Modification de la stratégie d’audit  
Cette sous-catégorie signale les modifications apportées dans la stratégie d’audit, y compris les modifications de la liste SACL.  
  
##### <a name="authentication-policy-change"></a>Modification de la stratégie d’authentification  
Cette sous-catégorie signale les modifications apportées dans la stratégie d’authentification.  
  
##### <a name="authorization-policy-change"></a>Modification de la stratégie d’autorisation  
Cette sous-catégorie signale les modifications apportées dans la stratégie d’autorisation, y compris les modifications apportées aux autorisations (DACL).  
  
##### <a name="mpssvc-rule-level-policy-change"></a>Modification de la stratégie de niveau règle MPSSVC  
Cette sous-catégorie signale les modifications apportées dans les règles des stratégies utilisés par le Service de Protection Microsoft (MPSSVC.exe). Ce service est utilisé par le pare-feu Windows.  
  
##### <a name="filtering-platform-policy-change"></a>Modification de la stratégie plateforme de filtrage  
Cette sous-catégorie signale l’ajout et la suppression d’objets à partir de la protection des fichiers Windows, y compris les filtres de démarrage. Ces événements peuvent être très élevées en volume.  
  
##### <a name="other-policy-change-events"></a>Autres événements de modification de stratégie  
Cette sous-catégorie signale les autres types de modifications de stratégie de sécurité telles que la configuration du Module de plateforme sécurisée (TPM) ou fournisseurs de chiffrement.  
  
#### <a name="privilege-use"></a>Utilisation de privilèges  
  
##### <a name="sensitive-privilege-use"></a>Utilisation de privilèges sensibles  
Cette sous-catégorie signale qu’un compte d’utilisateur ou le service utilise un privilège sensible. Un privilège sensible inclut les droits d’utilisateur suivants: agir en tant que partie du système d’exploitation, sauvegarder des fichiers et répertoires, créer un objet-jeton, de déboguer des programmes, activer des comptes d’utilisateur et d’ordinateur à être approuvé pour délégation, générer des audits de sécurité, emprunter l’identité d’un client après l’authentification, charger et décharger des pilotes de périphérique, gérer l’audit et le journal de sécurité, modifier les valeurs d’environnement de microprogramme, remplacer un jeton de niveau processus , restaurer des fichiers et répertoires et prendre possession de fichiers ou d’autres objets. Cette sous-catégorie d’audit créera un volume élevé d’événements.  
  
##### <a name="nonsensitive-privilege-use"></a>Utilisation des privilèges affecter  
Cette sous-catégorie signale qu’un compte d’utilisateur ou le service utilise un privilège affecter. Un privilège affecter inclut les droits d’utilisateur suivants: accéder au gestionnaire d’informations d’identification en tant qu’un appelant de confiance, accéder à cet ordinateur à partir du réseau, ajouter des stations de travail au domaine, ajuster les quotas de mémoire pour un processus, autoriser, permettre le par le biais des Services Bureau à distance, contourner la vérification de parcours, modifier l’heure système, créez un fichier d’échange, créer des objets globaux, créer des objets partagés permanents, créer des liens symboliques , refuser l’accès à cet ordinateur à partir du réseau, refuse la connexion en tant que traitement par lots, refuse la connexion en tant que service, refuse la connexion locale, refuse la connexion par le biais des Services Bureau à distance, forcer l’arrêt à partir d’un système distant, augmenter une plage de travail de processus, augmenter la priorité de planification, verrouillage des pages en mémoire, connectez-vous en tant que tâche, ouvrez une session en tant que service, modifier un nom d’objet , effectuer des tâches de maintenance de volume, processus unique du profil, optimiser les performances système, supprimez l’ordinateur à partir de la station d’accueil, arrêter le système et synchroniser les données de service d’annuaire. Cette sous-catégorie d’audit créera un volume d’événements très élevé.  
  
##### <a name="other-privilege-use-events"></a>Autres événements d’utilisation des privilèges  
Ce paramètre de stratégie de sécurité n’est pas actuellement utilisé.  
  
#### <a name="object-access"></a>Accès aux objets  
  
##### <a name="file-system"></a>Système de fichiers  
Cette sous-catégorie signale lorsque les objets de système de fichiers sont accessibles. Uniquement les objets de système de fichiers avec des listes SACL provoquent des événements d’audit et d’être généré, uniquement lorsqu’ils sont accessibles de manière leurs entrées de liste SACL correspondantes. En lui-même, ce paramètre de stratégie n’entraîne pas de tous les événements d’audit. Il indique s’il convient d’auditer l’événement d’un utilisateur qui accède à un objet de système de fichiers qui a une liste de contrôle d’accès spécifié système (SACL) efficacement l’activation de l’audit doit avoir lieu.  
  
Si le paramètre Auditer l’accès aux objets est configuré pour **réussite**, une entrée d’audit est générée chaque fois qu’un utilisateur accède correctement à un objet avec une SACL spécifiée. Si ce paramètre de stratégie est configuré pour **échec**, une entrée d’audit est générée chaque fois qu’un utilisateur ne parvient pas à accéder à un objet avec une SACL spécifiée.  
  
##### <a name="registry"></a>Registre  
Cette sous-catégorie signale lorsque les objets de Registre sont accessibles. Uniquement les objets de Registre avec SACL provoquent des événements d’audit et d’être généré, uniquement lorsqu’ils sont accessibles de manière leurs entrées de liste SACL correspondantes. En lui-même, ce paramètre de stratégie n’entraîne pas de tous les événements d’audit.  
  
##### <a name="kernel-object"></a>Objet de noyau  
Cette sous-catégorie signale les objets de noyau tels que les processus et les mutex sont accessibles. Uniquement les objets du noyau avec SACL provoquent des événements d’audit et d’être généré, uniquement lorsqu’ils sont accessibles de manière leurs entrées de liste SACL correspondantes. En règle générale des objets de noyau reçoivent uniquement SACL si les options d’audit AuditBaseObjects ou AuditBaseDirectories sont activées.  
  
##### <a name="sam"></a>SAM  
Cette sous-catégorie de rapports lors de l’accès aux objets de base de données de l’authentification Gestionnaire de comptes de sécurité (SAM) local.  
  
##### <a name="certification-services"></a>Services de certification  
Cette sous-catégorie signale les opérations de Services de Certification sont effectuées.  
  
##### <a name="application-generated"></a>Application générée  
Cette sous-catégorie signale applications essaient de générer des événements d’audit à l’aide de l’audit des interfaces de programmation d’application (API) de Windows.  
  
##### <a name="handle-manipulation"></a>Manipulation de handle  
Cette sous-catégorie signale lorsqu’un handle vers un objet est ouverte ou fermé. Uniquement les objets ayant des listes SACL provoquent ces événements d’être généré et uniquement si l’opération tentée handle correspond aux entrées de liste SACL. Gérer les événements de Manipulation sont générées uniquement des types d’objet dans lequel la sous-catégorie d’accès objet correspondante est activée (par exemple, système de fichiers ou du Registre).  
  
##### <a name="file-share"></a>Partage de fichiers  
Cette sous-catégorie signale un partage de fichiers est accessible. En lui-même, ce paramètre de stratégie n’entraîne pas de tous les événements d’audit. Elle détermine s’il faut auditer l’événement d’un utilisateur qui accède à un objet de partage de fichier qui a une liste de contrôle d’accès spécifié système (SACL), efficacement l’activation de l’audit doit avoir lieu.  
  
##### <a name="filtering-platform-packet-drop"></a>Rejet de paquet par plateforme de filtrage  
Cette sous-catégorie signale les paquets sont ignorés par le plateforme de filtrage de Windows (WFP). Ces événements peuvent être très élevées en volume.  
  
##### <a name="filtering-platform-connection"></a>Connexion de la plateforme de filtrage  
Cette sous-catégorie signale de connexions autorisées ou bloquées par WFP. Ces événements peuvent être élevés en volume.  
  
##### <a name="other-object-access-events"></a>Autres événements d’accès aux objets  
Cette sous-catégorie signale les autres événements liés à l’accès objet tels que les travaux du Planificateur de tâches et les objets COM +.  
  
#### <a name="system"></a>Système  
  
##### <a name="security-state-change"></a>Changement d’état de sécurité  
Cette sous-catégorie signale les modifications apportées dans l’état de la sécurité du système, par exemple, lorsque le sous-système de sécurité démarre et s’arrête.  
  
##### <a name="security-system-extension"></a>Extension du système de sécurité  
Cette sous-catégorie signale le chargement du code d’extension tels que les packages d’authentification par le sous-système de sécurité.  
  
##### <a name="system-integrity"></a>Intégrité du système  
Cette sous-catégorie de rapports sur les violations de l’intégrité du sous-système de sécurité.  
  
Pilote IPsec  
  
Cette sous-catégorie de rapports sur les activités du pilote Internet Protocol security (IPsec).  
  
##### <a name="other-system-events"></a>Autres événements système  
Cette sous-catégorie de rapports sur les autres événements système.  
  
Pour plus d’informations sur les descriptions de la sous-catégorie, reportez-vous à la [outil Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx).  
  
Chaque organisation doit passer en revue les catégories couverts précédentes et sous-catégories et activez les celles qui correspondent le mieux à leur environnement. Modifications apportées à la stratégie d’audit doivent toujours être testées avant le déploiement dans un environnement de production.  
  
### <a name="configuring-windows-audit-policy"></a>Stratégie d’Audit de la configuration Windows  
Stratégie d’audit de Windows peut être défini à l’aide des modifications de stratégies, auditpol.exe, API ou du Registre de groupe. Les méthodes recommandées pour la configuration de la stratégie d’audit pour la plupart des entreprises sont la stratégie de groupe ou auditpol.exe. Définition de la stratégie d’audit d’un système nécessite des autorisations de compte de niveau administrateur ou les autorisations déléguées appropriées.  
  
> [!NOTE]  
> Le **gérer le journal d’audit et de sécurité** doit disposer de privilèges à principaux de sécurité (les administrateurs en disposent par défaut) pour permettre la modification des options des ressources, telles que les fichiers, les objets Active Directory et les clés de Registre d’audit l’accès aux objets.  
  
#### <a name="setting-windows-audit-policy-by-using-group-policy"></a>Paramètre Windows de stratégie d’Audit à l’aide de stratégie de groupe  
Pour définir la stratégie d’audit à l’aide de stratégies de groupe, configurez les catégories d’audit appropriées situés sous **Configuration ordinateur\Paramètres Windows\Paramètres de sécurité\Stratégies Locales\stratégie** (voir la capture d’écran suivante pour obtenir un exemple à partir de l’éditeur de stratégie de groupe de locale (gpedit.msc)). Chaque catégorie de stratégie d’audit peut être activé pour **réussite**, **échec**, ou **réussite** et les événements d’échec.  
  
![analyser Active Directory.](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_6.gif)  
  
Stratégie d’Audit avancée peut être défini à l’aide d’Active Directory ou des stratégies de groupe local. Pour définir la stratégie d’Audit avancée, configurez les sous-catégories appropriés situés sous **ordinateur Configuration ordinateur\Paramètres Windows\Paramètres de stratégie avancée d’Audit** (voir la capture d’écran suivante pour obtenir un exemple à partir de l’éditeur de stratégie de groupe de locale (gpedit.msc)). Chaque sous-catégorie de stratégie d’audit peut être activé pour **réussite**, **échec**, ou **réussite** et **échec** événements.  
  
![analyser Active Directory.](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_7.gif)  
  
#### <a name="setting-windows-audit-policy-using-auditpolexe"></a>Stratégie d’Audit de paramètre Windows à l’aide d’Auditpol.exe  
Auditpol.exe (pour définir la stratégie d’audit de Windows) a été introduit dans Windows Server 2008 et Windows Vista. Initialement, auditpol.exe seulement peut être utilisé pour définir la stratégie d’Audit avancée, mais la stratégie de groupe peut être utilisée dans Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008, Windows 8 et Windows 7.  
  
Auditpol.exe est un utilitaire de ligne de commande. La syntaxe est la suivante:  
  
**AuditPol /set / < catégorie | Sous-catégorie >:<audit category> / < réussite | échec: > / < activer | désactiver >**  
  
Exemples de syntaxe Auditpol.exe:  
  
**AuditPol/SubCategory: «gestion de compte d’utilisateur» /success:enable /failure:enable**  
  
**AuditPol/SubCategory: «d’ouverture de session «/success:enable /failure:enable**  
  
**AuditPol/SubCategory: «IPSEC Main Mode» /failure:enable**  
  
> [!NOTE]  
> Auditpol.exe définit stratégie d’Audit avancée localement. Si la stratégie locale est en conflit avec Active Directory ou de stratégie de groupe locale, les paramètres de stratégie de groupe l’emporte généralement sur paramètres auditpol.exe. Lorsque plusieurs conflits de stratégie locale ou le groupe existent, une seule stratégie prévalent (qui est, remplacez). Stratégies d’audit ne seront pas fusionner.  
  
#### <a name="scripting-auditpol"></a>Écriture de scripts Auditpol  
Microsoft fournit un [exemple de script](https://support.microsoft.com/kb/921469) pour les administrateurs qui souhaitent pour définir la stratégie d’Audit avancée à l’aide d’un script au lieu de taper manuellement dans chaque commande auditpol.exe.  
  
**Remarque** stratégie de groupe ne signale pas toujours avec précision l’état de toutes les stratégies d’audit activés, contrairement aux auditpol.exe. Voir [mise en route de la stratégie d’Audit actuelle dans Windows 7 et Windows 2008 R2](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) pour plus d’informations.  
  
#### <a name="other-auditpol-commands"></a>Autres commandes Auditpol  
Auditpol.exe peut être utilisé pour enregistrer et restaurer une stratégie d’audit local et pour afficher d’autres commandes associées audit. Voici les autres **auditpol** commandes.  
  
**AuditPol /clear** - utilisés pour supprimer et réinitialiser des stratégies d’audit local  
  
**AuditPol /backup/file:<filename> ** - utilisés pour sauvegarder une stratégie d’audit local actuel dans un fichier binaire  
  
**AuditPol /Restore / file:<filename> ** - utilisés pour importer un fichier de stratégie d’audit précédemment enregistré sur une stratégie d’audit local  
  
**AuditPol / < get/set >/option:<CrashOnAuditFail> / < activer/désactiver >** -si ce paramètre de stratégie d’audit est activé, il entraîne immédiatement l’arrêt du système (avec arrêt: C0000244 message {Échec d’Audit}) si un audit de sécurité ne peut pas être enregistré pour une raison quelconque. En règle générale, un événement ne peut pas être consigné lorsque le journal d’audit de sécurité est plein et la méthode de rétention spécifiée pour le journal de sécurité est **ne pas remplacer les événements** ou **remplacer les événements par jours**. En règle générale elle est activée uniquement par les environnements nécessitant un niveau d’assurance supérieur qui enregistre le journal de sécurité. S’il est activé, les administrateurs doivent étroitement regarder la taille du journal de sécurité et faire pivoter les journaux en fonction des besoins. Elle peut également être définie avec la stratégie de groupe en modifiant l’option de sécurité **Audit: arrêter immédiatement le système se ne peut pas se connecter aux audits de sécurité** (par défaut = désactivé).  
  
**AuditPol / < get/set >/option:<AuditBaseObjects> / < activer/désactiver >** -ce paramètre de stratégie d’audit détermine s’il faut auditer l’accès aux objets système globaux. Si cette stratégie est activée, les objets système, tels que les mutex, événements, sémaphores et périphériques DOS sont créés avec une liste de contrôle d’accès système (SACL) par défaut. La plupart des administrateurs prendre en compte l’audit des objets système globaux pour être trop «bruyant», et elles seront uniquement activer si suspicion de piratage malveillants. Uniquement les objets nommés reçoivent une liste SACL. Si l’audit objet accès d’audit stratégie (ou sous-catégorie d’audit de l’objet de noyau) est également activé, l’accès à ces objets système est audité. Lorsque vous configurez ce paramètre de sécurité, modifications ne prendront effet qu’après le redémarrage de Windows. Cette stratégie peut également être définie avec la stratégie de groupe en modifiant l’option de sécurité Auditer l’accès des objets système globaux (par défaut = désactivé).  
  
**AuditPol / < get/set >/option:<AuditBaseDirectories> / < activer/désactiver >** -ce paramètre de stratégie d’audit spécifie que les objets du noyau nommés (par exemple, les mutex et les sémaphores) doivent être donné SACL lorsqu’ils sont créés. AuditBaseDirectories affecte les objets du conteneur pendant AuditBaseObjects affecte les objets qui ne peut pas contenir d’autres objets.  
  
**AuditPol / < get/set >/option:<FullPrivilegeAuditing> / < activer/désactiver >** -  
  
Ce paramètre de stratégie d’audit spécifie si le client génère un événement lorsqu’un ou plusieurs de ces privilèges sont attribués à un jeton de sécurité: AssignPrimaryTokenPrivilege, AuditPrivilege, BackupPrivilege, CreateTokenPrivilege, DebugPrivilege, EnableDelegationPrivilege, ImpersonatePrivilege, LoadDriverPrivilege, RestorePrivilege, SecurityPrivilege, SystemEnvironmentPrivilege, TakeOwnershipPrivilege et TcbPrivilege. Si cette option n’est pas activée (par défaut = désactivé), les privilèges BackupPrivilege et RestorePrivilege ne sont pas enregistrées. L’activation de cette option peut rendre le journal de sécurité sont extrêmement bruyant (parfois des centaines d’événements une deuxième) pendant une opération de sauvegarde. Cette stratégie peut également être définie avec la stratégie de groupe en modifiant l’option de sécurité **Audit: auditer l’utilisation des privilèges de sauvegarde et restauration**.  
  
> [!NOTE]  
> Certaines informations fournies ici a été effectuées à partir de Microsoft [Type d’Option d’Audit](https://msdn.microsoft.com/library/dd973862(prot.20).aspx) et l’outil Microsoft SCM.  
  
### <a name="enforcing-traditional-auditing-or-advanced-auditing"></a>En appliquant l’audit traditionnel ou audit avancée  
Dans Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows 8, Windows 7 et Windows Vista, les administrateurs peuvent choisir d’activer les neuf catégories traditionnels ou utiliser les sous-catégories. Il est un choix binaire qui doit être effectué dans chaque système Windows. Les principales catégories peuvent être activées ou le subcategoriesit ne peut pas être à la fois.  
  
Pour empêcher la stratégie héritée catégorie traditionnel d’écraser les sous-catégories de stratégie d’audit, vous devez activer le **paramètres de sous-catégorie de stratégie d’audit (Windows Vista ou version ultérieure) pour remplacer les paramètres de catégorie de stratégie d’audit** paramètre de stratégie se trouve sous **Configuration ordinateur\Paramètres Windows\Paramètres de sécurité\Stratégies Locales\options de sécurité**.  
  
Nous recommandons que les sous-catégories être activés et configurés au lieu des neuf catégories principales. Cela nécessite qu’un paramètre de stratégie de groupe soit activé (pour autoriser des sous-catégories remplacer les catégories d’audit) ainsi que la configuration de la liste des sous-catégories différents qui prennent en charge les stratégies d’audit.  
  
Sous-catégories d’audit peut être configuré à l’aide de plusieurs méthodes, notamment la stratégie de groupe et le programme de ligne de commande auditpol.exe.  
  
Pour plus d’informations sur l’audit de Windows, consultez les articles suivants:  
  
-   [Fonctions avancées de sécurité d’audit dans Windows7 et Windows Server2008R2](https://social.technet.microsoft.com/wiki/contents/articles/advanced-security-auditing-in-windows-7-and-windows-server-2008-r2.aspx)  
  
-   [L’audit et conformité dans Windows Server2008](https://technet.microsoft.com/magazine/2008.03.auditing.aspx)  
  
-   [Comment utiliser la stratégie de groupe pour configurer les paramètres pour les ordinateurs basés sur Windows Server 2008 et Windows Vista dans un domaine Windows Server 2008, dans un domaine Windows Server 2003 ou dans un domaine Windows 2000 d’audit de sécurité détaillés](https://support.microsoft.com/kb/921469)  
  
-   [Stratégie d’Audit de sécurité avancée Step-by-Step Guide](https://technet.microsoft.com/library/dd408940(WS.10).aspx)  
  


