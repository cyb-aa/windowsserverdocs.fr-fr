---
ms.assetid: 709353b0-b913-4367-8580-44745183e2bc
title: "Vérifier la fonctionnalité DNS pour prendre en charge la réplication d’annuaire"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: 
ms.suite: na
ms.technology: identity-adds
ms.author: billmath
ms.date: 05/31/2017
ms.tgt_pltfrm: na
author: Femila
ms.openlocfilehash: 49f2795e1042438a50850fcb7fd8224eff2cca37
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="verify-dns-functionality-to-support-directory-replication"></a>Vérifier la fonctionnalité DNS pour prendre en charge la réplication d’annuaire

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

 Pour vérifier les paramètres de système DNS (Domain Name) qui peuvent interférer avec la réplication Active Directory, vous pouvez commencer par exécuter le test de base qui permet de s’assurer que DNS fonctionne correctement pour votre domaine. Une fois que vous exécutez le test de base, vous pouvez tester d’autres aspects de la fonctionnalité DNS, y compris l’enregistrement de ressource et de mise à jour dynamique.

Vous pouvez exécuter ce test des fonctionnalités de base DNS sur n’importe quel contrôleur de domaine, généralement exécuter ce test sur les contrôleurs de domaine que vous pensez que peuvent rencontrer des problèmes de réplication, par exemple, les contrôleurs de domaine ce rapport 1844 d’ID d’événement, 1925, 2087 et 2088 dans le journal du Service d’annuaire Observateur DNS événements.



## <a name="running-the-domain-controller-basic-dns-testtitle"></a>Le test DNS de base de contrôleur de domaine en cours d’exécution</title>

Le test DNS base vérifie les aspects suivants de la fonctionnalité DNS:


- **Connectivité:** le test détermine si les contrôleurs sont inscrits dans DNS, de domaine peut être contacté par le <system>ping</system> de commandes et avez Lightweight Directory Access Protocol / de l’appel de procédure distante connectivité LDAP/RPC (). Si le test de connectivité échoue sur un contrôleur de domaine, aucune des autres tests ne sont exécutés sur ce contrôleur de domaine. Le test de connectivité est effectué automatiquement avant l’exécution de tout autre test DNS.
- **Services essentiels:** le test confirme que les services suivants sont en cours d’exécution et disponibles sur le contrôleur de domaine testé: service Client DNS, service Net Logon, service Centre de Distribution de clés (KDC) et service de serveur DNS (si DNS est installé sur le contrôleur de domaine).
- **Configuration du client DNS:** le test confirme que les serveurs DNS sur toutes les cartes réseau de l’ordinateur client DNS sont accessibles.
- **Enregistrements de ressource:** le test confirme que l’enregistrement de ressource hôte (A) de chaque contrôleur de domaine est enregistré sur au moins un des serveurs DNS qui est configuré sur l’ordinateur client.
- **Zone et (SOA):** si le contrôleur de domaine exécutant le service serveur DNS, le test confirme que la zone de domaine Active Directory et le début de l’enregistrement de ressource d’autorité (principale SOA) pour la zone de domaine Active Directory sont présents.
- **Zone racine:** vérifie si la zone racine (.) est présente.

L’appartenance à des administrateurs de l’entreprise, ou équivalent, est la condition minimale requise pour effectuer ces procédures.

Vous pouvez utiliser la procédure suivante pour vérifier les fonctionnalités de base DNS.
     
### <a name="to-verify-basic-dns-functionality"></a>Pour vérifier les fonctionnalités de base DNS:


1. Sur le contrôleur de domaine que vous souhaitez tester ou sur un ordinateur membre du domaine qui est installés des outils de Services de domaine Active Directory (AD DS), ouvrez une invite de commandes en tant qu’administrateur. Pour ouvrir une invite de commandes en tant qu’administrateur, cliquez sur **Démarrer**. 
2. Dans la zone Rechercher, tapez l’invite de commandes. 
3. En haut du menu Démarrer, cliquez sur invite de commandes, puis cliquez sur Exécuter en tant qu’administrateur. Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, vérifiez que l’action affichée est ce que vous souhaitez, puis cliquez sur Continuer.
4. À l’invite de commandes, tapez la commande suivante et appuyez sur ENTRÉE: `dcdiag /test:dns /v /s: &lt;DCName&gt; /DnsBasic f:/dcdiagreport.txt`
</br></br>Remplacez le nom unique, le nom NetBIOS ou le nom DNS du contrôleur de domaine pour &lt;DCName&gt;. Comme alternative, vous pouvez tester tous les contrôleurs de domaine dans la forêt en tapant e: au lieu de/s:. Le commutateur /f Spécifie un nom de fichier, qui est dcdiagreport.txt dans la commande précédente. Si vous souhaitez placer le fichier dans un emplacement autre que le répertoire de travail actuel, vous pouvez spécifier un chemin d’accès du fichier, par exemple, /f:c:reportsdcdiagreport.txt.

5. Ouvrez le fichier dcdiagreport.txt dans le bloc-notes ou un éditeur de texte similaire. Pour ouvrir le fichier dans le bloc-notes, à l’invite de commandes, tapez dcdiagreport.txt le bloc-notes, puis appuyez sur ENTRÉE. Si vous avez placé le fichier dans un répertoire de travail différent, incluent le chemin d’accès au fichier. Par exemple, si vous avez placé le fichier dans c:reports, tapez c:reportsdcdiagreport.txt le bloc-notes, puis appuyez sur ENTRÉE.
6. Faites défiler vers le tableau récapitulatif près du bas du fichier. 
</br></br>Notez les noms de tous les contrôleurs de domaine que l’état «Avertissement» ou «Échec» dans le tableau récapitulatif du rapport.  Essayez de déterminer s’il existe un contrôleur de domaine de problème la recherche de la section détail en recherchant la chaîne «Contrôleur de domaine: DCName,» où DCName est le nom réel du contrôleur de domaine.

Si vous voyez les modifications de configuration évident qui sont requises, les rendre, selon le cas. Par exemple, si vous remarquez qu’un de vos contrôleurs de domaine dispose d’une adresse IP incorrecte bien évidemment, vous pouvez le corriger. Ensuite, réexécutez le test.

Pour valider les modifications de configuration, réexécutez la commande de /v Dcdiag/test: DNS avec le commutateur/e: ou/s:, selon le cas. Si vous n’avez pas IP version 6 (IPv6) est activé sur le contrôleur de domaine, vous devez vous attendre à la partie de validation d’hôte (AAAA) de l’échec du test, mais si vous n’utilisez pas IPv6 sur votre réseau, ces enregistrements ne sont pas nécessaires.
            
## <a name="verifying-resource-record-registration"></a>Vérification de l’enregistrement de ressource enregistrement
    
Le contrôleur de domaine de destination utilise l’enregistrement de ressource alias (CNAME) DNS pour localiser son partenaire de réplication du contrôleur de domaine source. Bien que les contrôleurs de domaine exécutant Windows Server (à partir de Windows Server 2003 avec Service Pack 1 (SP1)) peuvent localiser des partenaires de réplication source à l’aide de noms de domaine complet (FQDN) ou, en cas d’échec, NetBIOS cheminLe présence de l’enregistrement de ressource alias (CNAME) est prévu et doit être vérifiée pour DNS bon fonctionnement. 
      
Vous pouvez utiliser la procédure suivante pour vérifier les inscriptions d’enregistrement de ressource, notamment l’enregistrement de ressource alias (CNAME).
      
### <a name="to-verify-resource-record-registrationtitle"></a>Pour vérifier l’inscription des enregistrements de ressource</title>


1. Ouvrez une invite de commandes en tant qu’administrateur. Pour ouvrir une invite de commandes en tant qu’administrateur, cliquez sur Démarrer. Dans la zone Rechercher, tapez l’invite de commandes. 
2. En haut du menu Démarrer, cliquez sur invite de commandes, puis cliquez sur Exécuter en tant qu’administrateur. Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, vérifiez que l’action affichée est ce que vous souhaitez, puis cliquez sur Continuer.  </br></br>Vous pouvez utiliser l’outil Dcdiag pour vérifier l’inscription de tous les enregistrements de ressource qui sont essentielles pour l’emplacement de contrôleur de domaine en exécutant le `dcdiag /test:dns /DnsRecordRegistration` commande.

Cette commande vérifie l’inscription des enregistrements de ressource suivants dans DNS:


- **alias (CNAME):** l’identificateur global unique (GUID)-en fonction d’enregistrement de ressource qui localise un partenaire de réplication
- **hôte (A):** l’enregistrement de ressource hôte qui contient l’adresse IP du contrôleur de domaine
- **SRV LDAP:** les enregistrements de ressource de service (SRV) qui localiser les serveurs LDAP
- **Dans le catalogue global SRV**: les enregistrements de ressource de service (SRV) localiser global des serveurs de catalogue
- **SRV PDC**: les enregistrements de ressource de service (SRV) que vous recherchez des maîtres d’opérations émulateur domaine principal (PDC) de contrôleur

Vous pouvez utiliser la procédure suivante pour vérifier les alias (CNAME) enregistrement de ressource uniquement.

### <a name="to-verify-alias-cname-resource-record-registration"></a>Pour vérifier l’inscription des enregistrements de ressource alias (CNAME)

1. Ouvrez le composant logiciel enfichable DNS. Pour ouvrir DNS, cliquez sur Démarrer. Dans la zone Rechercher, tapez dnsmgmt.msc et appuyez sur ENTRÉE. Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, vérifiez qu’il affiche l’action de votre choix, puis cliquez sur Continuer.
2. Utilisez le composant logiciel enfichable DNS pour localiser un contrôleur de domaine qui exécute le service serveur DNS, où le serveur héberge la zone DNS avec le même nom que le domaine Active Directory du contrôleur de domaine.
3. Dans l’arborescence de la console, cliquez sur la zone _msdcs nommé. Nom_domaine DNS.
4. Dans le volet d’informations, vérifiez que les enregistrements de ressource suivants sont présents: un enregistrement de ressource alias (CNAME) nommé Dsa_Guid._msdcs. <placeholder>Nom_domaine DNS</placeholder> et correspondante d’hôte (A) enregistrement de ressource pour le nom du serveur DNS.

Si l’enregistrement de ressource alias (CNAME) n’est pas enregistré, vérifiez que cette mise à jour dynamique fonctionne correctement. Utilisez le test de la section suivante pour vérifier la mise à jour dynamique.
    
## <a name="verifying-dynamic-update"></a>Vérification de la mise à jour dynamique
    
Si le test DNS base indique que les enregistrements de ressources n’existent pas dans DNS, utilisez le test de mise à jour dynamique pour déterminer pourquoi le service Net Logon n’est pas enregistré les enregistrements de ressource automatiquement. Pour vérifier que la zone de domaine Active Directory est configurée pour accepter les mises à jour dynamiques sécurisées et pour exécuter l’inscription d’un enregistrement test (_dcdiag_test_record), utilisez la procédure suivante. L’enregistrement de test est automatiquement supprimé après le test.

### <a name="to-verify-dynamic-updatetitle"></a>Pour vérifier la mise à jour dynamique</title>


1. Ouvrez une invite de commandes en tant qu’administrateur. Pour ouvrir une invite de commandes en tant qu’administrateur, cliquez sur Démarrer. Dans la zone Rechercher, tapez l’invite de commandes. En haut du menu Démarrer, cliquez sur invite de commandes, puis cliquez sur Exécuter en tant qu’administrateur. Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, vérifiez que l’action affichée est ce que vous souhaitez, puis cliquez sur Continuer.
2. À l’invite de commandes, tapez la commande suivante et appuyez sur ENTRÉE: 

    / s: de dcdiag/test: DNS /v&lt;DCName&gt; /DnsDynamicUpdate
</br></br>Remplacez le nom unique, le nom NetBIOS ou le nom DNS du contrôleur de domaine pour &lt;DCName&gt;. Comme alternative, vous pouvez tester tous les contrôleurs de domaine dans la forêt en tapant e: au lieu de/s:. Si vous n’avez pas IPv6 activé sur le contrôleur de domaine, vous devez vous attendre la partie enregistrement ressource hôte (AAAA) de l’échec, le test qui est une condition normale lorsque IPv6 n’est pas activé.

Si les mises à jour dynamiques ne sont pas configurés, vous pouvez utiliser la procédure suivante pour les configurer.

### <a name="to-enable-secure-dynamic-updates"></a>Pour activer les mises à jour dynamiques sécurisées


1. Ouvrez le composant logiciel enfichable DNS. Pour ouvrir DNS, cliquez sur Démarrer. 
2. Dans la zone Rechercher, tapez dnsmgmt.msc et appuyez sur ENTRÉE. Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, vérifiez qu’il affiche l’action, puis cliquez sur Continuer.
3. Dans l’arborescence de la console, avec le bouton droit de la zone concernée, puis cliquez sur Propriétés.
4. Sous l’onglet Général, vérifiez que le type de zone est intégré à Active Directory.
5. Dans les mises à jour dynamiques, cliquez sur sécuriser uniquement.

## <a name="registering-dns-resource-records"></a>Inscription des enregistrements de ressource DNS
    
Si les enregistrements de ressource DNS n’apparaissent pas dans DNS pour le contrôleur de domaine source, vous avez vérifié les mises à jour dynamiques et vous souhaitez inscrire des enregistrements de ressource DNS immédiatement, vous pouvez forcer manuellement l’enregistrement à l’aide de la procédure suivante. Le service accès réseau sur un contrôleur de domaine inscrit les enregistrements de ressources DNS requis pour le contrôleur de domaine doit être situé sur le réseau. Le service Client DNS inscrit l’enregistrement de ressource hôte (A) vers l’enregistrement d’alias (CNAME).

### <a name="to-register-dns-resource-records-manuallytitle"></a>Pour inscrire manuellement les enregistrements de ressource DNS</title>


1. Ouvrez une invite de commandes en tant qu’administrateur. Pour ouvrir une invite de commandes en tant qu’administrateur, cliquez sur Démarrer. 
2. Dans la zone Rechercher, tapez l’invite de commandes. 
3. En haut de l’écran de démarrage, cliquez sur invite de commandes, puis cliquez sur Exécuter en tant qu’administrateur. Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, vérifiez que l’action affichée est ce que vous souhaitez, puis cliquez sur Continuer.
4. Pour lancer l’enregistrement de ressource de localisateur de contrôleur de domaine enregistrements manuellement sur le contrôleur de domaine source, à l’invite de commandes, tapez la commande suivante et appuyez sur ENTRÉE: `net stop net logon &amp; net start net logon`
5. Pour lancer l’inscription de l’ordinateur hôte (A) enregistrement de ressource manuellement, à l’invite de commandes, tapez la commande suivante et appuyez sur ENTRÉE: `ipconfig /flushdns &amp; ipconfig /registerdns`
6. À l’invite de commandes, tapez la commande suivante et appuyez sur ENTRÉE: / s: de dcdiag/test: DNS /v&lt;DCName&gt; </br>,</br>Remplacez le nom unique, le nom NetBIOS ou le nom DNS du contrôleur de domaine pour &lt;DCName&gt;. Examinez la sortie du test pour vous assurer que les tests DNS transmises. Si vous n’avez pas IPv6 activé sur le contrôleur de domaine, vous devez vous attendre la partie enregistrement ressource hôte (AAAA) de l’échec, le test qui est une condition normale lorsque IPv6 n’est pas activé.
