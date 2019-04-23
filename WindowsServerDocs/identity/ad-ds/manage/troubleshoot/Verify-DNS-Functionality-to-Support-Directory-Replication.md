---
ms.assetid: 709353b0-b913-4367-8580-44745183e2bc
title: Vérifier la fonctionnalité DNS pour prendre en charge la duplication d’annuaire
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: ''
ms.suite: na
ms.technology: identity-adds
ms.author: joflore
ms.date: 05/31/2017
ms.tgt_pltfrm: na
author: Femila
ms.openlocfilehash: a55b95ee516abda8bdbae6e9829a161ef060012e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871870"
---
# <a name="verify-dns-functionality-to-support-directory-replication"></a>Vérifier la fonctionnalité DNS pour prendre en charge la duplication d’annuaire

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 Pour vérifier les paramètres de système DNS (Domain Name) qui peuvent interférer avec la réplication Active Directory, vous pouvez commencer en exécutant le test de base qui permet de s’assurer que DNS fonctionne correctement pour votre domaine. Après avoir exécuté le test de base, vous pouvez tester d’autres aspects des fonctionnalités DNS, notamment l’inscription d’enregistrement de ressource et de mise à jour dynamique.

Bien que vous pouvez exécuter ce test de fonctionnalités de DNS de base sur n’importe quel contrôleur de domaine, en général, vous exécutez ce test sur les contrôleurs de domaine que vous pensez rencontre peut-être des problèmes de réplication, par exemple, les contrôleurs de domaine qui signalent des événements ID 1844, 1925, 2087, ou 2088 dans le journal des événements Observateur Directory Service DNS.



## <a name="running-the-domain-controller-basic-dns-testtitle"></a>Le test DNS de base de contrôleur de domaine en cours d’exécution</title>

Le test de base DNS vérifie les aspects suivants du DNS :


- **Connectivité :** Le test détermine si les contrôleurs sont inscrits dans DNS, de domaine peut être contacté par le <system>ping</system> commande, et avez Lightweight Directory Access Protocol / connectivité (LDAP/RPC) d’appel de procédure distante. Si le test de connectivité échoue sur un contrôleur de domaine, aucun autre test n’est exécutées sur ce contrôleur de domaine. Le test de connectivité est effectué automatiquement avant l’exécution de tout autre test DNS.
- **Services essentiels :** Le test vérifie que les services suivants sont en cours d’exécution et disponible sur le contrôleur de domaine testé : Service Client DNS service Net Logon, service de centre de Distribution de clés (KDC) et service de serveur DNS (si DNS est installé sur le contrôleur de domaine).
- **Configuration du client DNS :**  Le test confirme que les serveurs DNS sur toutes les cartes réseau de l’ordinateur de client DNS sont accessibles.
- **Inscription des enregistrements de ressource :** Le test confirme que l’enregistrement de ressource hôte (A) de chaque contrôleur de domaine est enregistrée sur au moins un des serveurs DNS qui est configuré sur l’ordinateur client.
- **Zone et le début d’autorité (principale SOA) :** Si le contrôleur de domaine exécute le service serveur DNS, le test confirme que la zone de domaine Active Directory et le début de l’enregistrement de ressource d’autorité (principale SOA) pour la zone de domaine Active Directory sont présents.
- **Zone racine :** Vérifie si la zone racine (.) est présente.

L’appartenance à des administrateurs de l’entreprise, ou équivalent, est la condition minimale requise pour réaliser ces procédures.

Vous pouvez utiliser la procédure suivante pour vérifier les fonctionnalités de base DNS.
     
### <a name="to-verify-basic-dns-functionality"></a>Pour vérifier les fonctionnalités de base DNS :


1. Sur le contrôleur de domaine que vous souhaitez tester ou sur un ordinateur membre du domaine qui est installés des outils de Services de domaine Active Directory (AD DS), ouvrez une invite de commandes en tant qu’administrateur. Pour ouvrir une invite de commandes en tant qu'administrateur, cliquez sur **Démarrer**. 
2. Dans Rechercher, tapez l’invite de commandes. 
3. En haut du menu Démarrer, cliquez sur invite de commandes, puis cliquez sur Exécuter en tant qu’administrateur. Si la boîte de dialogue Contrôle de compte d'utilisateur s'affiche, vérifiez que l'action affichée est celle que vous souhaitez, puis cliquez sur Continuer.
4. À l’invite de commandes, tapez la commande suivante, puis appuyez sur ENTRÉE : `dcdiag /test:dns /v /s:<DCName> /DnsBasic /f:dcdiagreport.txt`
</br></br>Remplacez par le nom distinctif réel, le nom NetBIOS ou le nom DNS du contrôleur de domaine pour &lt;DCName&gt;. Comme alternative, vous pouvez tester tous les contrôleurs de domaine dans la forêt en tapant/e: au lieu / s:. Le commutateur /f Spécifie un nom de fichier, ce qui, dans la commande précédente est dcdiagreport.txt. Si vous souhaitez placer le fichier dans un emplacement autre que le répertoire de travail actuel, vous pouvez spécifier un chemin d’accès de fichier, telles que /f:c:reportsdcdiagreport.txt.

5. Ouvrez le fichier dcdiagreport.txt dans le bloc-notes ou un éditeur de texte similaire. Pour ouvrir le fichier dans le bloc-notes, à l’invite de commandes, tapez notepad dcdiagreport.txt, puis appuyez sur ENTRÉE. Si vous avez placé le fichier dans un autre répertoire de travail, incluez le chemin d’accès au fichier. Par exemple, si vous avez placé le fichier dans c:reports, tapez notepad c:reportsdcdiagreport.txt, puis appuyez sur ENTRÉE.
6. Faites défiler vers la table de résumé vers le bas du fichier. 
</br></br>Notez les noms de tous les contrôleurs de domaine de ce rapport état « Avertissement » ou « Échec » dans la table de résumé.  Essayez de déterminer s’il existe un contrôleur de domaine du problème en recherchant la section détail en recherchant la chaîne « contrôleur de domaine : DCName, « où DCName est le nom réel du contrôleur de domaine.

Si vous voyez les modifications de configuration évident qui sont requises, les rendre, comme il convient. Par exemple, si vous remarquez qu’un de vos contrôleurs de domaine dispose d’une adresse IP incorrecte à l’évidence, vous pouvez la corriger. Ensuite, réexécutez le test.

Pour valider les modifications de configuration, réexécutez la commande de /v Dcdiag/test : DNS avec le commutateur/e: ou/s:, comme il convient. Si vous n’avez pas de IP version 6 (IPv6) est activé sur le contrôleur de domaine, vous devez vous attendre à la partie de la validation d’hôte (AAAA) de l’échec du test, mais si vous n’utilisez pas IPv6 sur votre réseau, ces enregistrements ne sont pas nécessaires.
            
## <a name="verifying-resource-record-registration"></a>Vérification de l’enregistrement de ressource enregistrement
    
Le contrôleur de domaine de destination utilise l’enregistrement de ressource alias (CNAME) DNS pour localiser son partenaire de réplication du contrôleur de domaine source. Bien que les contrôleurs de domaine exécutant Windows Server (à partir de Windows Server 2003 avec Service Pack 1 (SP1)) peuvent localiser des partenaires de réplication source à l’aide de noms de domaine complet (FQDN) ou, si cette tentative échoue, présence de cheminLe NetBIOS de l’alias (CNAME) enregistrement de ressource est attendue et doit être vérifié pour DNS bon fonctionnement. 
      
Vous pouvez utiliser la procédure suivante pour vérifier l’inscription d’enregistrement de ressources, y compris l’inscription enregistrement de ressource alias (CNAME).
      
### <a name="to-verify-resource-record-registrationtitle"></a>Pour vérifier l’inscription des enregistrements de ressource</title>


1. Ouvrez une invite de commandes en tant qu’administrateur. Pour ouvrir une invite de commandes en tant qu’administrateur, cliquez sur Démarrer. Dans Rechercher, tapez l’invite de commandes. 
2. En haut du menu Démarrer, cliquez sur invite de commandes, puis cliquez sur Exécuter en tant qu’administrateur. Si la boîte de dialogue Contrôle de compte d'utilisateur s'affiche, vérifiez que l'action affichée est celle que vous souhaitez, puis cliquez sur Continuer.  </br></br>Vous pouvez utiliser l’outil Dcdiag pour vérifier l’inscription de tous les enregistrements de ressources qui sont essentielles pour l’emplacement de contrôleur de domaine en exécutant la `dcdiag /test:dns /DnsRecordRegistration` commande.

Cette commande vérifie l’inscription des enregistrements de ressource suivants dans DNS :


- **alias (CNAME) :** l’identificateur global unique (GUID)-en fonction d’enregistrement de ressource qui localise un partenaire de réplication
- **hôte (A) :** l’enregistrement de ressource d’hôte qui contient l’adresse IP du contrôleur de domaine
- **LDAP SRV :** les enregistrements de ressources de service (SRV) qui localiser les serveurs LDAP
- **GC SRV**: les enregistrements de ressources de service (SRV) recherchez global des serveurs de catalogue
- **PDC SRV**: les enregistrements de ressources de service (SRV) que vous recherchez des maîtres d’opérations émulateur domaine principal (PDC) de contrôleur

Vous pouvez utiliser la procédure suivante pour vérifier les alias (CNAME) enregistrement de ressource uniquement.

### <a name="to-verify-alias-cname-resource-record-registration"></a>Pour vérifier l’inscription des enregistrements de ressource alias (CNAME)

1. Ouvrez le composant logiciel enfichable DNS. Pour ouvrir DNS, cliquez sur Démarrer. Dans Rechercher, tapez dnsmgmt.msc, puis appuyez sur ENTRÉE. Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, vérifiez qu’il affiche l’action de votre choix, puis cliquez sur Continuer.
2. Utilisez le composant logiciel enfichable DNS pour localiser n’importe quel contrôleur de domaine qui exécute le service serveur DNS, dans laquelle le serveur héberge la zone DNS avec le même nom que le domaine Active Directory du contrôleur de domaine.
3. Dans l’arborescence de la console, cliquez sur la zone _msdcs nommé. Nom_domaine DNS.
4. Dans le volet de détails, vérifiez que les enregistrements de ressource suivants sont présents : un enregistrement de ressource alias (CNAME) nommé Dsa_Guid._msdcs. <placeholder>Nom_domaine DNS</placeholder> et correspondante d’hôte (A) enregistrement de ressource pour le nom du serveur DNS.

Si l’enregistrement de ressource alias (CNAME) n’est pas inscrit, vérifiez que cette mise à jour dynamique fonctionne correctement. Utilisez le test dans la section suivante pour vérifier la mise à jour dynamique.
    
## <a name="verifying-dynamic-update"></a>Vérification de la mise à jour dynamique
    
Si le test DNS base indique que les enregistrements de ressources n’existent pas dans DNS, utilisez le test de mise à jour dynamique pour déterminer pourquoi le service Net Logon n’a pas inscrit les enregistrements de ressources automatiquement. Pour vérifier que la zone de domaine Active Directory est configurée pour accepter les mises à jour dynamiques sécurisées et pour effectuer l’inscription d’un enregistrement test (_dcdiag_test_record), utilisez la procédure suivante. L’enregistrement de test est automatiquement supprimé une fois le test.

### <a name="to-verify-dynamic-updatetitle"></a>Pour vérifier la mise à jour dynamique</title>


1. Ouvrez une invite de commandes en tant qu’administrateur. Pour ouvrir une invite de commandes en tant qu’administrateur, cliquez sur Démarrer. Dans Rechercher, tapez l’invite de commandes. En haut du menu Démarrer, cliquez sur invite de commandes, puis cliquez sur Exécuter en tant qu’administrateur. Si la boîte de dialogue Contrôle de compte d'utilisateur s'affiche, vérifiez que l'action affichée est celle que vous souhaitez, puis cliquez sur Continuer.
2. À l’invite de commandes, tapez la commande suivante, puis appuyez sur ENTRÉE : `dcdiag /test:dns /v /s:<DCName> /DnsDynamicUpdate`
   </br></br>Remplacez par le nom unique, le nom NetBIOS ou le nom DNS du contrôleur de domaine pour &lt;DCName&gt;. Comme alternative, vous pouvez tester tous les contrôleurs de domaine dans la forêt en tapant/e: au lieu / s:. Si vous n’avez pas IPv6 est activé sur le contrôleur de domaine, vous devriez la partie enregistrement ressource hôte (AAAA) de l’échec, le test qui est une condition normale lorsque IPv6 n’est pas activé.

Si des mises à jour dynamiques sécurisées ne sont pas configurés, vous pouvez utiliser la procédure suivante pour les configurer.

### <a name="to-enable-secure-dynamic-updates"></a>Pour activer les mises à jour dynamiques sécurisées


1. Ouvrez le composant logiciel enfichable DNS. Pour ouvrir DNS, cliquez sur Démarrer. 
2. Dans Rechercher, tapez dnsmgmt.msc, puis appuyez sur ENTRÉE. Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, vérifiez qu’il affiche l’action, puis cliquez sur Continuer.
3. Dans l’arborescence de la console, cliquez sur la zone concernée, puis cliquez sur Propriétés.
4. Sous l’onglet Général, vérifiez que le type de zone est intégré à Active Directory.
5. Dans les mises à jour dynamiques, cliquez sur sécurisé uniquement.

## <a name="registering-dns-resource-records"></a>L’inscription des enregistrements de ressource DNS
    
Si les enregistrements de ressource DNS n’apparaissent pas dans DNS pour le contrôleur de domaine source, vous avez vérifié les mises à jour dynamiques et vous souhaitez enregistrer immédiatement les enregistrements de ressource DNS, vous pouvez forcer manuellement l’inscription à l’aide de la procédure suivante. Le service accès réseau sur un contrôleur de domaine inscrit les enregistrements de ressources DNS requis pour le contrôleur de domaine à se trouver sur le réseau. Le service Client DNS inscrit l’enregistrement de ressource hôte (A) qui pointe l’enregistrement d’alias (CNAME).

### <a name="to-register-dns-resource-records-manuallytitle"></a>Pour inscrire manuellement des enregistrements de ressource DNS</title>


1. Ouvrez une invite de commandes en tant qu’administrateur. Pour ouvrir une invite de commandes en tant qu’administrateur, cliquez sur Démarrer. 
2. Dans Rechercher, tapez l’invite de commandes. 
3. En haut du début, cliquez sur invite de commandes, puis cliquez sur Exécuter en tant qu’administrateur. Si la boîte de dialogue Contrôle de compte d'utilisateur s'affiche, vérifiez que l'action affichée est celle que vous souhaitez, puis cliquez sur Continuer.
4. Pour lancer l’enregistrement de ressource de localisateur de contrôleur de domaine enregistrements manuellement sur le contrôleur de domaine source, à l’invite de commandes, tapez la commande suivante, puis appuyez sur ENTRÉE : `net stop netlogon && net start netlogon`
5. Pour lancer l’inscription de l’hôte (A) enregistrement de ressource manuellement, à l’invite de commandes, tapez la commande suivante, puis appuyez sur ENTRÉE : `ipconfig /flushdns && ipconfig /registerdns`
6. À l’invite de commandes, tapez la commande suivante, puis appuyez sur ENTRÉE : `dcdiag /test:dns /v /s:<DCName>` </br></br>Remplacez par le nom unique, le nom NetBIOS ou le nom DNS du contrôleur de domaine pour &lt;DCName&gt;. Examinez la sortie du test pour vous assurer que les tests DNS ont réussi. Si vous n’avez pas IPv6 est activé sur le contrôleur de domaine, vous devriez la partie enregistrement ressource hôte (AAAA) de l’échec, le test qui est une condition normale lorsque IPv6 n’est pas activé.
