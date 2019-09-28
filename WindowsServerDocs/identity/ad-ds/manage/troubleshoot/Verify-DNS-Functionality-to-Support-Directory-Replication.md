---
ms.assetid: 709353b0-b913-4367-8580-44745183e2bc
title: Vérifier la fonctionnalité DNS pour prendre en charge la duplication d’annuaire
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.service: ''
ms.suite: na
ms.technology: identity-adds
ms.author: joflore
ms.date: 05/31/2017
ms.tgt_pltfrm: na
author: Femila
ms.openlocfilehash: 066f7ebe1cd8f981344e059797daa9d3f86bdf49
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71409056"
---
# <a name="verify-dns-functionality-to-support-directory-replication"></a>Vérifier la fonctionnalité DNS pour prendre en charge la duplication d’annuaire

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 Pour vérifier les paramètres DNS (Domain Name System) qui peuvent interférer avec la réplication Active Directory, vous pouvez commencer par exécuter le test de base qui garantit que DNS fonctionne correctement pour votre domaine. Après avoir exécuté le test de base, vous pouvez tester d’autres aspects de la fonctionnalité DNS, y compris l’inscription des enregistrements de ressources et la mise à jour dynamique.

Bien que vous puissiez exécuter ce test de la fonctionnalité DNS de base sur n’importe quel contrôleur de domaine, vous exécutez généralement ce test sur les contrôleurs de domaine que vous pensez rencontrer comme des problèmes de réplication, par exemple les contrôleurs de domaine qui signalent les ID d’événement 1844, 1925, 2087 ou 2088 dans le journal DNS du service observateur d’événements Directory.



## <a name="running-the-domain-controller-basic-dns-testtitle"></a>Exécution du test DNS de base du contrôleur de domaine @ no__t-0

Le test DNS de base vérifie les aspects suivants de la fonctionnalité DNS :


- **Connectivité** Le test détermine si les contrôleurs de domaine sont inscrits dans DNS, s’ils peuvent être contactés par la commande ping et s’ils disposent d' <system>une</system> connectivité LDAP/RPC (Lightweight Directory Access Protocol/Remote Procedure Call). Si le test de connectivité échoue sur un contrôleur de domaine, aucun autre test n’est exécuté sur ce contrôleur de domaine. Le test de connectivité est effectué automatiquement avant l’exécution de tout autre test DNS.
- **Services essentiels :** Le test confirme que les services suivants sont en cours d’exécution et disponibles sur le contrôleur de domaine testé : Service client DNS, service Net Logon, service centre de distribution de clés (KDC) et service serveur DNS (si DNS est installé sur le contrôleur de domaine).
- **Configuration du client DNS :**  Le test confirme que les serveurs DNS sur toutes les cartes réseau de l’ordinateur client DNS sont accessibles.
- **Inscriptions des enregistrements de ressources :** Le test confirme que l’enregistrement de ressource hôte (A) de chaque contrôleur de domaine est inscrit sur au moins l’un des serveurs DNS configurés sur l’ordinateur client.
- **Zone et autorité de certification (SOA) :** Si le contrôleur de domaine exécute le service serveur DNS, le test confirme que la zone de domaine Active Directory et l’enregistrement de ressource SOA (Start of Authority) pour la zone de domaine Active Directory sont présents.
- **Zone racine :** Vérifie si la zone racine (.) est présente.

Pour effectuer ces procédures, il est nécessaire d’appartenir au minimum au groupe administrateurs de l’entreprise ou à un groupe équivalent.

Vous pouvez utiliser la procédure suivante pour vérifier la fonctionnalité DNS de base.
     
### <a name="to-verify-basic-dns-functionality"></a>Pour vérifier la fonctionnalité DNS de base :


1. Sur le contrôleur de domaine que vous souhaitez tester ou sur un ordinateur membre du domaine sur lequel des outils de Active Directory Domain Services (AD DS) sont installés, ouvrez une invite de commandes en tant qu’administrateur. Pour ouvrir une invite de commandes en tant qu'administrateur, cliquez sur **Démarrer**. 
2. Dans Démarrer la recherche, tapez invite de commandes. 
3. En haut du menu Démarrer, cliquez avec le bouton droit sur invite de commandes, puis cliquez sur Exécuter en tant qu’administrateur. Si la boîte de dialogue Contrôle de compte d'utilisateur s'affiche, vérifiez que l'action affichée est celle que vous souhaitez, puis cliquez sur Continuer.
4. À l’invite de commandes, tapez la commande suivante, puis appuyez sur entrée : `dcdiag /test:dns /v /s:<DCName> /DnsBasic /f:dcdiagreport.txt`
</br></br>Remplacez le nom unique réel, le nom NetBIOS ou le nom DNS du contrôleur de domaine par &lt;DCName @ no__t-1. Vous pouvez également tester tous les contrôleurs de domaine dans la forêt en tapant/e : au lieu de/s :. Le commutateur/f spécifie un nom de fichier, qui, dans la commande précédente, est dcdiagreport. txt. Si vous souhaitez placer le fichier dans un autre emplacement que le répertoire de travail actuel, vous pouvez spécifier un chemin d’accès de fichier, tel que/f : c :reportsdcdiagreport.txt.

5. Ouvrez le fichier dcdiagreport. txt dans le bloc-notes ou un éditeur de texte similaire. Pour ouvrir le fichier dans le bloc-notes, à l’invite de commandes, tapez Notepad dcdiagreport. txt, puis appuyez sur entrée. Si vous avez placé le fichier dans un autre répertoire de travail, incluez le chemin d’accès au fichier. Par exemple, si vous avez placé le fichier dans c :Reports, tapez notepad c :reportsdcdiagreport.txt, puis appuyez sur entrée.
6. Accédez à la table de résumé en bas du fichier. 
</br></br>Notez les noms de tous les contrôleurs de domaine qui signalent l’État « Warn » ou « Fail » dans la table de résumé.  Essayez de déterminer s’il existe un contrôleur de domaine posant problème en recherchant la section des sections détaillées en recherchant la chaîne «DC : DCName, «où DCName est le nom réel du contrôleur de domaine.

Si vous voyez des modifications de configuration explicites qui sont requises, faites-les, le cas échéant. Par exemple, si vous remarquez que l’un de vos contrôleurs de domaine a une adresse IP incorrecte, vous pouvez le corriger. Ensuite, réexécutez le test.

Pour valider les modifications de configuration, réexécutez la commande Dcdiag/test : DNS/v avec le commutateur/e : ou/s :, selon le cas. Si la version IP 6 (IPv6) n’est pas activée sur le contrôleur de domaine, vous devez vous attendre à ce que la partie de validation de l’hôte (AAAA) du test échoue, mais si vous n’utilisez pas IPv6 sur votre réseau, ces enregistrements ne sont pas nécessaires.
            
## <a name="verifying-resource-record-registration"></a>Vérification de l’inscription des enregistrements de ressources
    
Le contrôleur de domaine de destination utilise l’enregistrement de ressource d’alias DNS (CNAMe) pour localiser son partenaire de réplication de contrôleur de domaine source. Bien que les contrôleurs de domaine exécutant Windows Server (à compter de Windows Server 2003 avec Service Pack 1 (SP1)) puissent localiser les partenaires de réplication source à l’aide de noms de domaine complets (FQDN) ou, en cas d’échec, NetBIOS namesthe la présence de l’alias (CNAMe) l’enregistrement de ressource est attendu et doit être vérifié pour un bon fonctionnement du DNS. 
      
Vous pouvez utiliser la procédure suivante pour vérifier l’inscription des enregistrements de ressources, y compris l’inscription des enregistrements de ressources d’alias (CNAMe).
      
### <a name="to-verify-resource-record-registrationtitle"></a>Pour vérifier l’inscription des enregistrements de ressources @ no__t-0


1. Ouvrez une invite de commandes en tant qu’administrateur. Pour ouvrir une invite de commandes en tant qu’administrateur, cliquez sur Démarrer. Dans Démarrer la recherche, tapez invite de commandes. 
2. En haut du menu Démarrer, cliquez avec le bouton droit sur invite de commandes, puis cliquez sur Exécuter en tant qu’administrateur. Si la boîte de dialogue Contrôle de compte d'utilisateur s'affiche, vérifiez que l'action affichée est celle que vous souhaitez, puis cliquez sur Continuer.  </br></br>Vous pouvez utiliser l’outil Dcdiag pour vérifier l’inscription de tous les enregistrements de ressources qui sont essentiels pour l’emplacement du contrôleur de domaine en exécutant la commande `dcdiag /test:dns /DnsRecordRegistration`.

Cette commande vérifie l’inscription des enregistrements de ressources suivants dans DNS :


- **alias (CNAME) :** l’enregistrement de ressource d’identificateur global unique (Guid) qui localise un partenaire de réplication
- **hôte (A) :** enregistrement de ressource hôte qui contient l’adresse IP du contrôleur de domaine
- **LDAP SRV :** enregistrements de ressource de service (SRV) qui localisent les serveurs LDAP
- **GC SRV**: enregistrements de ressource de service (SRV) qui localisent les serveurs de catalogue global
- **PDC SRV**: enregistrements de ressource de service (SRV) qui localisent les maîtres d’opérations de l’émulateur de contrôleur de domaine principal (PDC)

Vous pouvez utiliser la procédure suivante pour vérifier la seule inscription des enregistrements de ressources d’alias (CNAMe).

### <a name="to-verify-alias-cname-resource-record-registration"></a>Pour vérifier l’inscription d’un enregistrement de ressource alias (CNAMe)

1. Ouvrez le composant logiciel enfichable DNS. Pour ouvrir DNS, cliquez sur Démarrer. Dans Démarrer la recherche, tapez dnsmgmt. msc, puis appuyez sur entrée. Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, vérifiez qu’elle affiche l’action souhaitée, puis cliquez sur continuer.
2. Utilisez le composant logiciel enfichable DNS pour rechercher un contrôleur de domaine qui exécute le service serveur DNS, où le serveur héberge la zone DNS portant le même nom que le domaine Active Directory du contrôleur de domaine.
3. Dans l’arborescence de la console, cliquez sur la zone intitulée _ msdcs. Dns_Domain_Name.
4. Dans le volet d’informations, vérifiez que les enregistrements de ressources suivants sont présents : un enregistrement de ressource alias (CNAMe) nommé Dsa_Guid. _ msdcs. <placeholder>Dns_Domain_Name</placeholder> et un enregistrement de ressource hôte (a) correspondant pour le nom du serveur DNS.

Si l’enregistrement de ressource alias (CNAMe) n’est pas inscrit, vérifiez que la mise à jour dynamique fonctionne correctement. Utilisez le test de la section suivante pour vérifier la mise à jour dynamique.
    
## <a name="verifying-dynamic-update"></a>Vérification de la mise à jour dynamique
    
Si le test DNS de base indique que les enregistrements de ressource n’existent pas dans DNS, utilisez le test de mise à jour dynamique pour déterminer la raison pour laquelle le service accès réseau n’a pas inscrit automatiquement les enregistrements de ressource. Pour vérifier que la zone de domaine Active Directory est configurée pour accepter des mises à jour dynamiques sécurisées et pour effectuer l’inscription d’un enregistrement de test (_dcdiag_test_record), utilisez la procédure suivante. L’enregistrement de test est supprimé automatiquement après le test.

### <a name="to-verify-dynamic-updatetitle"></a>Pour vérifier la mise à jour dynamique @ no__t-0


1. Ouvrez une invite de commandes en tant qu’administrateur. Pour ouvrir une invite de commandes en tant qu’administrateur, cliquez sur Démarrer. Dans Démarrer la recherche, tapez invite de commandes. En haut du menu Démarrer, cliquez avec le bouton droit sur invite de commandes, puis cliquez sur Exécuter en tant qu’administrateur. Si la boîte de dialogue Contrôle de compte d'utilisateur s'affiche, vérifiez que l'action affichée est celle que vous souhaitez, puis cliquez sur Continuer.
2. À l’invite de commandes, tapez la commande suivante, puis appuyez sur entrée : `dcdiag /test:dns /v /s:<DCName> /DnsDynamicUpdate`
   </br></br>Remplacez le nom unique, le nom NetBIOS ou le nom DNS du contrôleur de domaine pour @no__t 0DCName @ no__t-1. Vous pouvez également tester tous les contrôleurs de domaine dans la forêt en tapant/e : au lieu de/s :. Si IPv6 n’est pas activé sur le contrôleur de domaine, vous devez vous attendre à ce que la partie d’enregistrement de ressource hôte (AAAA) du test échoue, ce qui est une condition normale quand IPv6 n’est pas activé.

Si les mises à jour dynamiques sécurisées ne sont pas configurées, vous pouvez utiliser la procédure suivante pour les configurer.

### <a name="to-enable-secure-dynamic-updates"></a>Pour activer les mises à jour dynamiques sécurisées


1. Ouvrez le composant logiciel enfichable DNS. Pour ouvrir DNS, cliquez sur Démarrer. 
2. Dans Démarrer la recherche, tapez dnsmgmt. msc, puis appuyez sur entrée. Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, vérifiez qu’elle affiche l’action souhaitée, puis cliquez sur continuer.
3. Dans l’arborescence de la console, cliquez avec le bouton droit sur la zone correspondante, puis cliquez sur Propriétés.
4. Sous l’onglet général, vérifiez que le type de zone est intégré à Active Directory.
5. Dans mises à jour dynamiques, cliquez sur sécurisé uniquement.

## <a name="registering-dns-resource-records"></a>Inscription des enregistrements de ressources DNS
    
Si les enregistrements de ressources DNS n’apparaissent pas dans DNS pour le contrôleur de domaine source, si vous avez vérifié les mises à jour dynamiques et si vous souhaitez enregistrer les enregistrements de ressources DNS immédiatement, vous pouvez forcer l’inscription manuellement à l’aide de la procédure suivante. Le service accès réseau sur un contrôleur de domaine inscrit les enregistrements de ressources DNS requis pour que le contrôleur de domaine se trouve sur le réseau. Le service client DNS inscrit l’enregistrement de ressource hôte (A) vers lequel pointe l’enregistrement d’alias (CNAMe).

### <a name="to-register-dns-resource-records-manuallytitle"></a>Pour inscrire manuellement les enregistrements de ressources DNS @ no__t-0


1. Ouvrez une invite de commandes en tant qu’administrateur. Pour ouvrir une invite de commandes en tant qu’administrateur, cliquez sur Démarrer. 
2. Dans Démarrer la recherche, tapez invite de commandes. 
3. En haut du bouton Démarrer, cliquez avec le bouton droit sur invite de commandes, puis cliquez sur Exécuter en tant qu’administrateur. Si la boîte de dialogue Contrôle de compte d'utilisateur s'affiche, vérifiez que l'action affichée est celle que vous souhaitez, puis cliquez sur Continuer.
4. Pour initialiser manuellement l’inscription des enregistrements de ressource du localisateur de contrôleur de domaine sur le contrôleur de domaine source, à l’invite de commandes, tapez la commande suivante, puis appuyez sur entrée : `net stop netlogon && net start netlogon`
5. Pour initier manuellement l’inscription de l’enregistrement de ressource hôte (A), à l’invite de commandes, tapez la commande suivante, puis appuyez sur entrée : `ipconfig /flushdns && ipconfig /registerdns`
6. À l’invite de commandes, tapez la commande suivante, puis appuyez sur entrée : `dcdiag /test:dns /v /s:<DCName>` </br></br>Remplacez le nom unique, le nom NetBIOS ou le nom DNS du contrôleur de domaine pour @no__t 0DCName @ no__t-1. Examinez la sortie du test pour vous assurer que les tests DNS ont réussi. Si IPv6 n’est pas activé sur le contrôleur de domaine, vous devez vous attendre à ce que la partie d’enregistrement de ressource hôte (AAAA) du test échoue, ce qui est une condition normale quand IPv6 n’est pas activé.
