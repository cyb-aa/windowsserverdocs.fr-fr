---
title: "Règles utilisées par l’outil Windows Server Essentials Best Practices Analyzer (BPA)"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 37e1dae7-586c-4dd7-bf83-7e14a9567c8f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c205bc8ff75bf64d4a13a7d799988c9d1ebe1a22
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="rules-used-by-the-windows-server-essentials-best-practices-analyzer-bpa-tool"></a>Règles utilisées par l’outil Windows Server Essentials Best Practices Analyzer (BPA)

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Cet article décrit les règles utilisées par le WindowsServerEssentialsBestPractices Analyzer (BPA). L’outil BPA examine un serveur qui exécute WindowsServerEssentials et présente un rapport qui décrit les problèmes et fournit des recommandations pour les résoudre. Les recommandations sont développées par l’organisation du support technique pour WindowsServerEssentials.  
  
## <a name="using-the-tool"></a>À l’aide de l’outil  
 Il est conseillé, lorsque vous migrez vers WindowsServerEssentials à partir de WindowsServerEssentials2011, WindowsSmallBusinessServer2011Essentials ou WindowsHomeServer2011, pour exécuter l’outil BPA sur le serveur de Destination après avoir terminé la migration de vos paramètres et données. Vous pouvez exécuter l’outil à partir du tableau de bord à tout moment.  
  
#### <a name="to-run-the--windows-server-essentials-bpa-on-the-server"></a>Pour exécuter l’outil BPA WindowsServerEssentials sur le serveur  
  
1.  Ouvrez une session sur le serveur en tant qu’administrateur, puis ouvrez le tableau de bord.  
  
2.  Dans le tableau de bord, cliquez sur le **périphériques** onglet.  
  
3.  Sur le **tâches serveur** volet, cliquez sur **Best Practices Analyzer**.  
  
4.  Passez en revue chaque message BPA et suivez les instructions pour résoudre les problèmes si nécessaire.  
  
## <a name="rules-used-by-the-best-practices-analyzer"></a>Règles utilisées par l’outil Best Practices Analyzer  
  
### <a name="disable-ip-filtering"></a>Désactiver le filtrage IP  
 **Problème:** le filtrage IP est actuellement activé sur le serveur. Vous devez désactiver le filtrage IP.  
  
 **Impact:** si le filtrage IP est activé, le trafic réseau peut être bloqué.  
  
 **Résolution:**  
  
##### <a name="to-disable-ip-filtering"></a>Pour désactiver le filtrage IP  
  
1.  Ouvrez regedit.exe sur le serveur.  
  
2.  Naviguez jusqu'à HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters.  
  
3.  Avec le bouton droit **EnableSecurityFilters**, puis cliquez sur **modifier**.  
  
4.  Dans le **modifier DWORD (32bits) valeur** fenêtre, modifier le **données de la valeur** champ à zéro, puis cliquez sur **OK**.  
  
5.  Pour appliquer la modification, redémarrez le serveur.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-should-be-set-to-start-automatically-by-default"></a>Le service Distributed Transaction Coordinator (MSDTC) doit être configuré pour démarrer automatiquement par défaut  
 **Problème:** le service MSDTC n’est pas configuré pour démarrer automatiquement.  
  
 **Impact:** le service MSDTC ne peut pas démarrer automatiquement lorsque le serveur démarre. Si le service est arrêté, certaines fonctions de SQLServer ou COM pourraient échouer. Par conséquent, les applications qui utilisent des fonctions de MicrosoftSQLServer ou COM pourraient ne pas fonctionnent correctement.  
  
 **Résolution:**  
  
##### <a name="to-configure-the-msdtc-service-to-start-automatically"></a>Pour configurer le service MSDTC pour démarrer automatiquement  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez sur le **Distributed Transaction Coordinator** de service, puis cliquez sur **propriétés**.  
  
3.  Sur le **général**, modifiez le **type de démarrage** à **automatique (début différé)**, puis cliquez sur **OK**.  
  
### <a name="the-netlogon-service-should-be-configured-to-start-automatically-by-default"></a>Le service Netlogon doit être configuré pour démarrer automatiquement par défaut  
 **Problème:** le service Netlogon n’est pas configuré pour démarrer automatiquement.  
  
 **Impact:** le service Netlogon peut pas démarrer automatiquement lorsque le serveur démarre. Si le service est arrêté, le serveur ne peut pas authentifier utilisateurs et les services.  
  
 **Résolution:**  
  
##### <a name="to-configure-the-netlogon-service-to-start-automatically"></a>Pour configurer le service Netlogon pour démarrer automatiquement  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez sur le **Netlogon** de service, puis cliquez sur **propriétés**.  
  
3.  Sur le **général**, modifiez le **type de démarrage** à **automatique**, puis cliquez sur **OK**.  
  
### <a name="the-dns-client-service-should-be-configured-to-start-automatically-by-default"></a>Le service Client DNS doit être configuré pour démarrer automatiquement par défaut  
 **Problème:** le service Client DNS n’est pas configuré pour démarrer automatiquement.  
  
 **Impact:** le service Client DNS ne peut pas démarrer automatiquement lorsque le serveur démarre. Si ce service est arrêté, le serveur ne peut pas être en mesure de résoudre les noms DNS.  
  
 **Résolution:**  
  
##### <a name="to-configure-the-dns-client-service-to-start-automatically"></a>Pour configurer le service Client DNS pour démarrer automatiquement  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez sur le **Client DNS** de service, puis cliquez sur **propriétés**.  
  
3.  Sur le **général**, modifiez le **type de démarrage** à **automatique**, puis cliquez sur **OK**.  
  
### <a name="the-dns-server-service-should-be-configured-to-start-automatically-by-default"></a>Le service serveur DNS doit être configuré pour démarrer automatiquement par défaut  
 **Problème:** le service serveur DNS n’est pas configuré pour démarrer automatiquement.  
  
 **Impact:** le service serveur DNS ne peut pas démarrer automatiquement lorsque le serveur démarre. Si ce service est arrêté, les mises à jour DNS ne produira pas.  
  
 **Résolution:**  
  
##### <a name="to-configure-the-dns-server-service-to-start-automatically"></a>Pour configurer le service serveur DNS pour démarrer automatiquement  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez sur le **serveur DNS** de service, puis cliquez sur **propriétés**.  
  
3.  Sur le **général**, modifiez le **type de démarrage** à **automatique**, puis cliquez sur **OK**.  
  
### <a name="active-directory-web-services-is-not-set-to-the-default-start-mode"></a>Services Web ActiveDirectory n’est pas défini sur le mode de démarrage par défaut  
 **Problème:** Services Web ActiveDirectory n’est pas défini sur le mode de démarrage par défaut automatique.  
  
 **Impact:** Services Web d’ActiveDirectory (ADWS) n’est pas configuré pour le mode de démarrage par défaut automatique. Si les services Web ActiveDirectory sur le serveur est arrêté ou désactivé, les applications clientes telles que le module ActiveDirectory pour Windows PowerShell ou le centre d’administration ActiveDirectory ne peut pas accéder ou gérer les instances du service annuaire qui sont en cours d’exécution sur ce serveur. Pour plus d’informations, voir [Nouveautés dans ADDS: Services Web ActiveDirectory](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) dans la bibliothèque technique Windows Server.  
  
 **Résolution:**  
  
##### <a name="to-configure-the-active-directory-web-services-service-to-start-automatically"></a>Pour configurer le service Services Web ActiveDirectory pour démarrer automatiquement  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez sur le **Services Web ActiveDirectory** de service, puis cliquez sur **propriétés**.  
  
3.  Sur le **général**, modifiez le **type de démarrage** à **automatique**, puis cliquez sur **OK**.  
  
### <a name="the-dhcp-client-service-should-be-configured-to-start-automatically-by-default"></a>Le service Client DHCP doit être configuré pour démarrer automatiquement par défaut  
 **Problème:** le service Client DHCP n’est pas configuré pour démarrer automatiquement.  
  
 **Impact:** le service Client DHCP ne démarre pas automatiquement lorsque le serveur démarre. Si ce service est arrêté, les ordinateurs clients ne peuvent pas recevoir une adresse IP à partir du serveur.  
  
 **Résolution:**  
  
##### <a name="to-configure-the-dhcp-client-service-to-start-automatically"></a>Pour configurer le service Client DHCP pour démarrer automatiquement  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez sur le **Client DHCP** de service, puis cliquez sur **propriétés**.  
  
3.  Sur le **général**, modifiez le **type de démarrage** à **automatique**, puis cliquez sur **OK**.  
  
### <a name="the-iis-admin-service-should-be-configured-to-start-automatically-by-default"></a>Le Service d’administration IIS doit être configuré pour démarrer automatiquement par défaut  
 **Problème:** le Service d’administration IIS n’est pas configuré pour démarrer automatiquement.  
  
 **Impact:** le Service d’administration IIS ne démarre pas automatiquement lorsque le serveur démarre. Si ce service est arrêté, il se peut que vous ne puissiez pas accéder aux sites Internet en cours d’exécution sur le serveur, comme l’accès Web à distance.  
  
 **Résolution:**  
  
##### <a name="to-configure-the-iis-admin-service-to-start-automatically"></a>Pour configurer le service d’administration IIS pour démarrer automatiquement  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Avec le bouton droit **Service d’administration IIS**, puis cliquez sur **propriétés**.  
  
3.  Sur le **général**, modifiez le **type de démarrage** à **automatique**, puis cliquez sur **OK**.  
  
### <a name="the-world-wide-web-publishing-service-should-be-configured-to-start-automatically-by-default"></a>Le Service de publication World Wide Web doit être configuré pour démarrer automatiquement par défaut  
 **Problème:** le Service de publication World Wide Web n’est pas configuré pour démarrer automatiquement.  
  
 **Impact:** le Service de publication World Wide Web ne peut pas démarrer automatiquement lorsque le serveur démarre. Si ce service est arrêté, il se peut que vous ne puissiez pas accéder aux sites Internet en cours d’exécution sur le serveur, comme l’accès Web à distance.  
  
 **Résolution:**  
  
##### <a name="to-configure-the-world-wide-web-publishing-service-to-start-automatically"></a>Pour configurer le Service de publication World Wide Web pour démarrer automatiquement  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Avec le bouton droit **Service de publication World Wide Web**, puis cliquez sur **propriétés**.  
  
3.  Sur le **général**, modifiez le **type de démarrage** à **automatique**, puis cliquez sur **OK**.  
  
### <a name="the-remote-registry-service-should-be-configured-to-start-automatically-by-default"></a>Le service de Registre distant doit être configuré pour démarrer automatiquement par défaut  
 **Problème:** le service Registre à distance n’est pas configuré pour démarrer automatiquement.  
  
 **Impact:**  
  
 Le service Registre à distance ne peut pas démarrer automatiquement au démarrage du serveur. Si ce service est arrêté, vous serez peut-être pas en mesure d’effectuer certaines opérations réseau à distance.  
  
 **Résolution:**  
  
##### <a name="to-configure-the-remote-registry-service-to-start-automatically"></a>Pour configurer le service Registre à distance pour démarrer automatiquement  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Avec le bouton droit le **Registre distant** de service, puis cliquez sur **propriétés**.  
  
3.  Sur le **général**, modifiez le **type de démarrage** à **automatique**, puis cliquez sur **OK**.  
  
### <a name="the-remote-desktop-gateway-service-should-be-configured-to-start-automatically-by-default"></a>Le service passerelle des services Bureau à distance doit être configuré pour démarrer automatiquement par défaut.  
 **Problème:** le service passerelle des services Bureau à distance n’est pas configuré pour démarrer automatiquement.  
  
 **Impact:** si ce service est arrêté, les utilisateurs ne peuvent peut-être pas accéder aux ordinateurs à l’aide d’accès Web à distance.  
  
 **Résolution:**  
  
##### <a name="to-configure-the-remote-desktop-gateway-service-to-start-automatically"></a>Pour configurer le service passerelle des services Bureau à distance pour démarrer automatiquement  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez sur le **passerelle des services Bureau à distance** de service, puis cliquez sur **propriétés**.  
  
3.  Sur le **général**, modifiez le **type de démarrage** à **automatique (début différé)**, puis cliquez sur **OK**.  
  
### <a name="the-windows-time-service-should-be-configured-to-start-automatically-by-default"></a>Le service de temps Windows doit être configuré pour démarrer automatiquement par défaut  
 **Problème:** le service de temps Windows n’est pas configuré pour démarrer automatiquement.  
  
 **Impact:** si ce service est arrêté, la synchronisation de données et l’heure ne sont pas disponibles.  
  
 **Résolution:**  
  
##### <a name="to-configure-the-windows-time-service-to-start-automatically"></a>Pour configurer le service de temps Windows pour démarrer automatiquement  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Avec le bouton droit le **temps Windows** de service, puis cliquez sur **propriétés**.  
  
3.  Sur le **général**, modifiez le **type de démarrage** à **automatique**, puis cliquez sur **OK**.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-should-be-started"></a>Le service Distributed Transaction Coordinator (MSDTC) doit être démarré.  
 **Problème:** le service MSDTC n’est pas en cours d’exécution sur le serveur.  
  
 **Impact:** si ce service est arrêté, certaines fonctions de SQLServer ou COM pourraient échouer. Par conséquent, les applications qui utilisent des fonctions de MicrosoftSQLServer ou COM pourraient ne pas fonctionnent correctement.  
  
 **Résolution:**  
  
##### <a name="to-start-the-distributed-transaction-coordinator-service"></a>Pour démarrer le service Distributed Transaction Coordinator  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez sur le **Distributed Transaction Coordinator** de service, puis cliquez sur **Démarrer**.  
  
### <a name="the-netlogon-service-should-be-started"></a>Le service Netlogon devrait démarrer  
 **Problème:** le service Netlogon n’est pas en cours d’exécution sur le serveur.  
  
 **Impact:** si ce service n’est pas démarré, le serveur ne peut pas authentifier utilisateurs et les services.  
  
 **Résolution:**  
  
##### <a name="to-start-the-netlogon-service"></a>Pour démarrer le service Netlogon  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez sur le **Netlogon** de service, puis cliquez sur **Démarrer**.  
  
### <a name="the-dns-client-service-should-be-started"></a>Le service Client DNS devrait démarrer  
 **Problème:** le service Client DNS n’est pas en cours d’exécution sur le serveur.  
  
 **Impact:** si ce service n’est pas démarré, le serveur peut être en mesure de résoudre les noms DNS.  
  
 **Résolution:**  
  
##### <a name="to-start-the-dns-client-service"></a>Pour démarrer le service Client DNS  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez sur le **Client DNS** de service, puis cliquez sur **Démarrer**.  
  
### <a name="the-dns-server-service-should-be-started"></a>Le service serveur DNS doit être démarré.  
 **Problème:** le service serveur DNS n’est pas en cours d’exécution sur le serveur.  
  
 **Impact:** si le service serveur DNS n’est pas démarré, mises à jour DNS ne peuvent pas se produire.  
  
 **Résolution:**  
  
##### <a name="to-start-the-dns-server-service"></a>Pour démarrer le service serveur DNS  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez sur le **serveur DNS** de service, puis cliquez sur **Démarrer**.  
  
### <a name="active-directory-web-services-is-not-started"></a>Services Web ActiveDirectory n’est pas démarré.  
 **Problème:** Services Web ActiveDirectory n’est pas démarré.  
  
 **Impact:** Services Web d’ActiveDirectory (ADWS) n’est pas démarré. Si les services Web ActiveDirectory sur le serveur est arrêté ou désactivé, les applications clientes telles que le module ActiveDirectory pour Windows PowerShell ou le centre d’administration ActiveDirectory ne peut pas accéder ou gérer les instances du service annuaire qui sont en cours d’exécution sur ce serveur. Pour plus d’informations, voir [Nouveautés dans ADDS: Services Web ActiveDirectory](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) dans la bibliothèque technique Windows Server.  
  
 **Résolution:**  
  
##### <a name="to-start-the-active-directory-web-services-service"></a>Pour démarrer le service Services Web ActiveDirectory  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Avec le bouton droit **Services Web ActiveDirectory**, puis cliquez sur **Démarrer**.  
  
### <a name="the-dhcp-client-service-should-be-started"></a>Le service Client DHCP devrait démarrer  
 **Problème:** le service Client DHCP n’est pas en cours d’exécution sur le serveur.  
  
 **Impact:** si ce service est arrêté, les ordinateurs clients ne peuvent pas recevoir une adresse IP du serveur.  
  
 **Résolution:**  
  
##### <a name="to-start-the-dhcp-client-service"></a>Pour démarrer le service Client DHCP  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez sur le **Client DHCP** de service, puis cliquez sur **Démarrer**.  
  
### <a name="the-iis-admin-service-should-be-started"></a>Le Service d’administration IIS devrait démarrer  
 **Problème:** le Service d’administration IIS n’est pas en cours d’exécution sur le serveur.  
  
 **Impact:** si ce service est arrêté, vous ne pourrez peut-être pas accéder aux sites Web en cours d’exécution sur le serveur, comme l’accès Web à distance.  
  
 **Résolution:**  
  
##### <a name="to-start-the-iis-admin-service"></a>Pour démarrer le Service d’administration IIS  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Avec le bouton droit **Service d’administration IIS**, puis cliquez sur **Démarrer**.  
  
### <a name="the-world-wide-web-publishing-service-should-be-started"></a>Le Service de publication World Wide Web devrait démarrer  
 **Problème:** le Service de publication World Wide Web n’est pas en cours d’exécution sur le serveur.  
  
 **Impact:** si ce service est arrêté, vous ne pourrez peut-être pas accéder aux sites Web en cours d’exécution sur le serveur, comme l’accès Web à distance.  
  
 **Résolution:**  
  
##### <a name="to-start-the-world-wide-web-publishing-service"></a>Pour démarrer le Service de publication World Wide Web  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Avec le bouton droit **Service de publication World Wide Web**, puis cliquez sur **Démarrer**.  
  
### <a name="the-remote-desktop-gateway-service-should-be-started"></a>Le service passerelle des services Bureau à distance doit être démarré.  
 **Problème:** le service passerelle des services Bureau à distance n’est pas en cours d’exécution sur le serveur.  
  
 **Impact:** si ce service est arrêté, les utilisateurs ne peuvent peut-être pas accéder aux ordinateurs à l’aide de l’accès Web à distance.  
  
 **Résolution:**  
  
##### <a name="to-start-the-remote-desktop-gateway-service"></a>Pour démarrer le Service de passerelle Bureau à distance  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez sur le **passerelle des services Bureau à distance** de service, puis cliquez sur **Démarrer**.  
  
### <a name="the-windows-time-service-should-be-started"></a>Le service de temps Windows devrait démarrer  
 **Problème:** le service de temps Windows n’est pas en cours d’exécution sur le serveur.  
  
 **Impact:** si ce service est arrêté et l’heure de synchronisation sera pas disponible.  
  
 **Résolution:**  
  
##### <a name="to-start-the-windows-time-service"></a>Pour démarrer le Service de temps Windows  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Avec le bouton droit le **temps Windows** de service, puis cliquez sur **Démarrer**.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-logon-account-should-be-nt-authoritynetwork-service"></a>Le compte d’ouverture de session du service Distributed Transaction Coordinator (MSDTC) doit être NT AUTHORITY\\NETWORK Service  
 **Problème:** le compte d’ouverture de session par défaut pour le service Distributed Transaction Coordinator (MSDTC) a changé.  
  
 **Impact:** le service ne dispose ne peut-être pas les autorisations requises pour fonctionner comme prévu. Par conséquent, les applications qui utilisent des fonctions SQL ou COM pourraient ne pas fonctionnent correctement.  
  
 **Résolution:**  
  
##### <a name="to-change-the-logon-account-for-the-service"></a>Pour modifier le compte d’ouverture de session pour le service  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez sur le **Distributed Transaction Coordinator** de service, puis cliquez sur **propriétés**.  
  
3.  Sur le **ouverture de session** onglet, sélectionnez **ce compte**, type **NT AUTHORITY\Network Service**, puis cliquez sur **OK**.  
  
### <a name="the-netlogon-service-should-use-the-local-system-account-as-its-logon-account"></a>Le service Netlogon doit utiliser le compte système Local comme compte d’ouverture de session  
 **Problème:** le compte d’ouverture de session par défaut pour le service Netlogon a changé.  
  
 **Impact:** le service ne dispose ne peut-être pas les autorisations requises pour fonctionner comme prévu. Par conséquent, le serveur ne peut pas authentifier utilisateurs et les services.  
  
 **Résolution:**  
  
##### <a name="to-change-the-netlogon-service-logon-account"></a>Pour changer le compte d’ouverture de session du service Netlogon  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez sur le **Netlogon** de service, puis cliquez sur **propriétés**.  
  
3.  Sur le **ouverture de session** onglet, sélectionnez **compte système Local**.  
  
### <a name="the-dns-client-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>Le service Client DNS doit utiliser le compte NT AUTHORITY\\NETWORK Service comme compte d’ouverture de session  
 **Problème:** le compte d’ouverture de session par défaut pour le service Client DNS a changé.  
  
 **Impact:** le service ne dispose ne peut-être pas les autorisations requises pour fonctionner comme prévu. Par conséquent, le serveur peut être impossible de résoudre les noms DNS.  
  
 **Résolution:**  
  
##### <a name="to-change-the-dns-client-service-logon-account"></a>Pour changer le compte d’ouverture de session du service Client DNS  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez sur le **Client DNS** de service, puis cliquez sur **propriétés**.  
  
3.  Sur le **ouverture de session** onglet, sélectionnez **ce compte**, puis tapez **NT AUTHORITY\Network Service**.  
  
### <a name="the-dns-server-service-should-use-the-local-system-account-as-its-logon-account"></a>Le service serveur DNS doit utiliser le compte système Local comme compte d’ouverture de session  
 **Problème:** le compte d’ouverture de session par défaut pour le service serveur DNS a changé.  
  
 **Impact:** le service ne dispose ne peut-être pas les autorisations requises pour fonctionner comme prévu. Par conséquent, les mises à jour DNS ne soient pas exécutées.  
  
 **Résolution:**  
  
##### <a name="to-change-the-dns-server-service-logon-account"></a>Pour modifier le compte d’ouverture de session du service serveur DNS  
  
1.  Ouvrez services.msc sur le serveur  
  
2.  Cliquez sur le **serveur DNS** de service, puis cliquez sur **propriétés**.  
  
3.  Sur le **ouverture de session** onglet, sélectionnez **compte système Local**.  
  
### <a name="active-directory-web-services-is-not-the-default-logon-account"></a>Services Web ActiveDirectory n’est pas le compte d’ouverture de session par défaut  
 **Problème:** Services Web ActiveDirectory n’est pas le compte d’ouverture de session par défaut. Par défaut, le compte d’ouverture de session est défini sur **compte système Local**.  
  
 **Impact:** Services Web d’ActiveDirectory (ADWS) n’est pas démarré. Si les services Web ActiveDirectory sur le serveur est arrêté ou désactivé, les applications clientes telles que le module ActiveDirectory pour Windows PowerShell ou le centre d’administration ActiveDirectory ne peut pas accéder ou gérer les instances du service annuaire qui sont en cours d’exécution sur ce serveur. Pour plus d’informations, voir [Nouveautés dans ADDS: Services Web ActiveDirectory](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) dans la bibliothèque technique Windows Server.  
  
 **Résolution:**  
  
##### <a name="to-change-the-active-directory-web-services-logon-account"></a>Pour modifier le compte d’ouverture de session de Services Web ActiveDirectory  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Avec le bouton droit **Services Web ActiveDirectory**, puis cliquez sur **propriétés**.  
  
3.  Modifier le **type de démarrage** à **automatique**, puis cliquez sur **OK**.  
  
4.  Dans les Services Web ActiveDirectory **propriétés**, cliquez sur le **ouverture de session** onglet.  
  
5.  Sélectionnez le **compte système Local** option, puis cliquez sur **OK**.  
  
### <a name="the-windows-update-service-should-use-the-local-system-account-as-its-logon-account"></a>Le service de mise à jour Windows doit utiliser le compte système Local comme compte d’ouverture de session  
 **Problème:** le compte d’ouverture de session par défaut pour le service mises à jour automatiques a changé.  
  
 **Impact:** le service ne dispose ne peut-être pas les autorisations requises pour fonctionner comme prévu. Par conséquent, le serveur ne recevront pas de mises à jour automatiques.  
  
 **Résolution:**  
  
##### <a name="to-change-the-windows-update-service-logon-account"></a>Pour modifier le compte d’ouverture de session du service Windows Update  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Avec le bouton droit le **mise à jour Windows** de service, puis cliquez sur Propriétés.  
  
3.  Sur le **ouverture de session** onglet, sélectionnez **compte système Local**.  
  
### <a name="the-dhcp-client-service-should-use-the-nt-authoritylocalservice-account-as-its-logon-account"></a>Le service Client DHCP doit utiliser le compte NT authority\\localservice comme compte d’ouverture de session  
 **Problème:** le compte d’ouverture de session par défaut pour le service Client DHCP a changé.  
  
 **Impact:** le service ne dispose ne peut-être pas les autorisations requises pour fonctionner comme prévu. Par conséquent, l’ordinateur client recevra pas les adresses IP à partir du serveur.  
  
 **Résolution:**  
  
##### <a name="to-change-the-dhcp-client-service-logon-account"></a>Pour modifier le compte d’ouverture de session du service Client DHCP  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez sur le **Client DHCP** de service, puis cliquez sur **propriétés**.  
  
3.  Sur le **ouverture de session** onglet, sélectionnez **ce compte**, puis tapez **NT AUTHORITY\Local Service**.  
  
### <a name="the-iis-admin-service-should-use-the-local-system-account-as-its-logon-account"></a>Le service d’administration IIS doit utiliser le compte système Local comme compte d’ouverture de session  
 **Problème:** le compte d’ouverture de session par défaut pour le service d’administration IIS a changé.  
  
 **Impact:** le service ne dispose ne peut-être pas les autorisations requises qui sont requises pour fonctionner comme prévu. Par conséquent, il se peut que vous ne puissiez pas accéder aux sites Internet en cours d’exécution sur le serveur, comme l’accès Web à distance.  
  
 **Résolution:**  
  
##### <a name="to-change-the-service-logon-account"></a>Pour modifier le compte d’ouverture de session du service  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Avec le bouton droit **service d’administration IIS**, puis cliquez sur **propriétés**.  
  
3.  Sur le **ouverture de session** onglet, sélectionnez **compte système Local**.  
  
### <a name="the-world-wide-web-publishing-service-should-use-the-local-system-account-as-its-logon-account"></a>Le Service de publication World Wide Web doit utiliser le compte système Local comme compte d’ouverture de session  
 **Problème:** le compte d’ouverture de session par défaut pour le Service de publication World Wide Web a changé.  
  
 **Impact:** le service ne dispose ne peut-être pas les autorisations requises pour fonctionner comme prévu. Par conséquent, il se peut que vous ne puissiez pas accéder aux sites Internet en cours d’exécution sur le serveur, comme l’accès Web à distance.  
  
 **Résolution:**  
  
##### <a name="to-change-the-world-wide-web-publishing-service-logon-account"></a>Pour modifier le compte d’ouverture de session de Service de publication World Wide Web  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Avec le bouton droit **Service de publication World Wide Web**, puis cliquez sur **propriétés**.  
  
3.  Sur le **ouverture de session** onglet, sélectionnez **compte système Local**.  
  
### <a name="the-remote-desktop-gateway-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>Le service de passerelle des services Bureau à distance doit utiliser le compte NT AUTHORITY\\NETWORK Service comme compte d’ouverture de session  
 **Problème:** le compte d’ouverture de session par défaut pour le service passerelle des services Bureau à distance a changé.  
  
 **Impact:** le service ne dispose ne peut-être pas les autorisations appropriées à fonctionner comme prévu. Par conséquent, les utilisateurs ne peuvent pas en mesure d’accéder aux ordinateurs à l’aide de l’accès Web à distance.  
  
 **Résolution:**  
  
##### <a name="to-change-the-remote-desktop-gateway-service-logon-account"></a>Pour modifier le compte d’ouverture de session du service passerelle des services Bureau à distance  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez sur le **passerelle des services Bureau à distance** de service, puis cliquez sur **propriétés**.  
  
3.  Sur le **ouverture de session** onglet, sélectionnez **ce compte**, puis tapez **NT AUTHORITY\Network Service**.  
  
### <a name="the-windows-time-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>Le service de temps Windows doit utiliser le compte NT AUTHORITY\\NETWORK Service comme compte d’ouverture de session  
 **Problème:** le compte d’ouverture de session par défaut pour le service de temps Windows a changé.  
  
 **Impact:** le service ne dispose ne peut-être pas les autorisations appropriées à fonctionner comme prévu. Par conséquent, date et heure de synchronisation sont peut-être pas disponible.  
  
 **Résolution:**  
  
##### <a name="to-change-the-windows-time-service-logon-account"></a>Pour modifier le compte de connexion de service de temps Windows  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Avec le bouton droit le **temps Windows** de service, puis cliquez sur **propriétés**.  
  
3.  Sur le **ouverture de session** onglet, sélectionnez **ce compte**, puis tapez **NT AUTHORITY\Local Service**.  
  
### <a name="the-built-in-administrators-group-does-not-have-the-right-to-log-on-as-batch-job"></a>Le groupe Administrateurs intégré ne dispose pas le droit d’ouvrir une session en tant que traitement par lots  
 **Problème:** au groupe Administrateurs intégré ne dispose pas le droit d’ouvrir une session en tant que traitement par lots.  
  
 **Impact:** si l’administrateur crée une alerte et configure l’alerte à s’exécuter lorsque l’administrateur n’est pas connecté, l’alerte échouera avec un code d’erreur de 2147943785.  
  
 **Résolution:** pour plus d’informations sur la façon de donner les administrateurs intégrés groupe l’autorisation d’ouvrir une session en tant que traitement par lots, voir [donner au groupe administrateur intégré le droit d’ouvrir une session en tant que traitement par lots](https://technet.microsoft.com/library/jj635076) (https://technet.microsoft.com/library/jj635076).  
  
### <a name="the-windows-firewall-is-turned-off"></a>Le pare-feu Windows est désactivé.  
 **Problème:** pare-feu Windows est désactivé. La valeur par défaut est activé.  
  
 **Impact:** en fonction de vos paramètres de pare-feu, le pare-feu Windows peut aider à protéger votre serveur et réseau des activités malveillantes en bloquant certaines informations qui passent par le serveur.  
  
 **Résolution:**  
  
##### <a name="to-turn-on-windows-firewall-on-the-server"></a>Pour activer le pare-feu Windows sur le serveur  
  
1.  Ouvrez le panneau de configuration sur le serveur.  
  
2.  Dans le panneau de configuration, cliquez sur **système et sécurité**, puis cliquez sur **pare-feu Windows**.  
  
3.  Dans le pare-feu Windows, cliquez sur **activer ou désactiver le pare-feu Windows**, sélectionnez le **activer le pare-feu Windows** option, puis cliquez sur **OK**.  
  
### <a name="the-internal-network-adapter-is-not-configured-to-register-ip-address-in-dns"></a>La carte réseau interne n’est pas configurée pour enregistrer les adresses IP dans DNS  
 **Problème:** la carte réseau interne n’est pas configurée pour enregistrer son adresse IP dans DNS.  
  
 **Impact:** si l’adresse IP de la carte réseau interne n’est pas enregistré dans DNS, il ne peut-être pas possible d’accéder au serveur à l’aide du nom de l’ordinateur serveur s.  
  
 **Résolution:** Vérifiez que la carte réseau interne est configurée pour enregistrer dans DNS.  
  
### <a name="dns-the-values-for-the-dns-forwardingtimeout-and-recursiontimeout-registry-key-are-identical"></a>DNS: Les valeurs de la clé de Registre DNS ForwardingTimeout et RecursionTimeout sont identiques  
 **Problème:** la valeur de la clé de Registre ForwardingTimeout du DNS ne doit pas être identique à la valeur de la clé de Registre RecursionTimeout.  
  
 **Impact:** vous n’êtes peut-être pas en mesure d’accéder aux ressources Internet par nom.  
  
 **Résolution:** définir la valeur de la clé de Registre RecursionTimeout est supérieure à la valeur de la clé ForwardingTimeout, situé dans le Registre au niveau du HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters.  
  
### <a name="the-forward-dns-zone-for-your-active-directory-domain-does-not-allow-secure-updates"></a>La zone de recherche directe DNS pour votre domaine ActiveDirectory n’autorise pas les mises à jour sécurisées  
 **Problème:** vous devez configurer la zone de recherche directe pour autoriser uniquement les mises à jour dynamiques sécurisées.  
  
 **Impact:** lorsque vous activez des mises à jour dynamiques sécurisées, seuls les utilisateurs autorisés et les hôtes peuvent apporter des modifications aux enregistrements.  
  
 **Résolution:**  
  
##### <a name="to-configure-the-forward-lookup-zone-for-your-active-directory-domain"></a>Pour configurer la zone de recherche directe pour votre domaine ActiveDirectory  
  
1.  Ouvrez dnsmgmt.msc sur le serveur.  
  
2.  Avec le bouton droit de la zone de recherche directe pour votre domaine ActiveDirectory, puis cliquez sur **propriétés**.  
  
3.  Dans le **mises à jour dynamiques** la liste déroulante, sélectionnez-le **sécurisé uniquement**, puis cliquez sur **OK**.  
  
### <a name="the-forward-dns-zone-does-not-allow-secure-updates"></a>Le transfert de zone DNS n’autorise pas sécurisée des mises à jour  
 **Problème:** vous devez configurer la zone de recherche directe pour la zone _msdcs.* n’autorise uniquement les mises à jour dynamiques sécurisées.  
  
 **Impact:** lorsque vous activez des mises à jour dynamiques sécurisées, seuls les utilisateurs autorisés et les hôtes peuvent apporter des modifications aux enregistrements dans la zone msdcs.*.  
  
 **Résolution:**  
  
##### <a name="to-allow-secure-updates-in-the-msdcs-zone"></a>Pour autoriser les mises à jour sécurisées dans la zone _msdcs  
  
1.  Ouvrez dnsmgmt.msc sur le serveur.  
  
2.  Avec le bouton droit de la zone de recherche directe pour la zone _msdcs, puis cliquez sur **propriétés**.  
  
3.  Dans le **mises à jour dynamiques** la liste déroulante, sélectionnez-le **sécurisé uniquement**, puis cliquez sur **OK**.  
  
### <a name="internet-explorer-enhanced-security-configuration-is-not-enabled"></a>Configuration de sécurité renforcée d’Internet Explorer n’est pas activée.  
 **Problème:** Internet Explorer Configuration de sécurité renforcée (IE ESC) n’est actuellement pas activée pour le groupe Administrateurs.  
  
 **Impact:** si la Configuration de sécurité renforcée d’Internet Explorer n’est pas activée pour le groupe Administrateurs, votre serveur et Internet Explorer ont augmenté exposition aux attaques malveillantes qui peuvent se produire par le biais de Web contenu et les scripts d’application.  
  
 **Résolution:**  
  
##### <a name="to-enable-internet-explorer-enhanced-security-configuration"></a>Pour activer la Configuration de sécurité renforcée d’Internet Explorer  
  
1.  Ouvrez **le Gestionnaire de serveur** sur le serveur et puis cliquez sur **serveur Local**.  
  
2.  Sur le **propriétés** volet, définissez le paramètre **la Configuration de sécurité renforcée d’Internet Explorer** à **sur**, puis cliquez sur **OK**.  
  
### <a name="internet-explorer-enhanced-security-configuration-is-not-enabled"></a>Configuration de sécurité renforcée d’Internet Explorer n’est pas activée.  
 **Problème:** Internet Explorer Configuration de sécurité renforcée (IE ESC) n’est actuellement pas activée pour le groupe d’utilisateurs.  
  
 **Impact:** si la Configuration de sécurité renforcée d’Internet Explorer n’est pas activée pour le groupe d’utilisateurs, votre serveur et Internet Explorer ont augmenté exposition aux attaques malveillantes qui peuvent se produire par le biais de Web contenu et les scripts d’application.  
  
 **Résolution:**  
  
##### <a name="to-enable-internet-explorer-enhanced-security-configuration"></a>Pour activer la Configuration de sécurité renforcée d’Internet Explorer  
  
1.  Ouvrez **le Gestionnaire de serveur**, puis cliquez sur **serveur Local**.  
  
2.  Sur le **propriétés** volet, définissez le paramètre **la Configuration de sécurité renforcée d’Internet Explorer** à **sur**, puis cliquez sur **OK**.  
  
### <a name="the-source-server-remains-in-active-directory-sites-and-services"></a>Le serveur source demeure dans les Services et Sites ActiveDirectory  
 **Problème:** le serveur source qui exécute WindowsSmallBusinessServer existe toujours dans les Services et Sites ActiveDirectory dans le Default-First-Site-Name.  
  
 **Impact:** si le serveur source demeure dans les Services et Sites ActiveDirectory, les ordinateurs clients peuvent rencontrer des problème de connectivité: s.  
  
 **Résolution:** vous devez rétrograder le serveur source, supprimez-le du domaine et puis supprimez le serveur source à partir de Sites ActiveDirectory et Services et des utilisateurs ActiveDirectory et les ordinateurs.  
  
### <a name="source-server-remains-in-sbscomputer-ou"></a>Serveur source demeure dans SBSComputer OU  
 **Problème:** le serveur source qui exécute WindowsSmallBusinessServer existe toujours dans ActiveDirectory Users and Computers.  
  
 **Impact:** si le serveur source demeure dans ActiveDirectory Users and Computers, les ordinateurs clients peuvent rencontrer des problème de connectivité: s.  
  
 **Résolution:** vous devez rétrograder le serveur source, supprimez-le du domaine et puis supprimez le serveur source à partir de Sites ActiveDirectory et Services et des utilisateurs ActiveDirectory et les ordinateurs.  
  
### <a name="a-group-policy-is-missing"></a>Il manque une stratégie de groupe  
 **Problème:** la stratégie de groupe de stratégie de domaine par défaut est manquante.  
  
 **Impact:** la stratégie de domaine par défaut est requise pour les fonctions de domaine approprié.  
  
 **Résolution:**  
  
##### <a name="to-restore-a-missing-group-policy"></a>Pour restaurer une stratégie de groupe manquante  
  
1.  Ouvrez gpmc.msc sur le serveur.  
  
2.  Dans le Gestionnaire de stratégie de groupe, développez la forêt de domaines et rechercher l’arborescence de la console pour le **stratégie de domaine par défaut** objet de stratégie de groupe.  
  
3.  Si la stratégie n’apparaît pas dans l’arborescence, restaurez-la à partir d’une sauvegarde de l’état système.  
  
### <a name="no-dns-name-server-resource-records"></a>Serveur de nom sans DNS des enregistrements de ressource  
 **Problème:** n’existe aucun enregistrement de ressource de serveur (NS) de nom DNS dans la zone de recherche directe pour votre serveur.  
  
 **Impact:** si aucun enregistrement de ressource DNS nom serveur (NS) n’existe dans la zone de recherche directe pour le domaine ActiveDirectory, les utilisateurs ne peuvent pas en mesure d’accéder aux ressources sur le réseau ou sur Internet.  
  
 **Résolution:**  
  
##### <a name="to-restore-missing-dns-name-server-resource-records"></a>Pour restaurer manquant serveur de nom DNS des enregistrements de ressource  
  
1.  Ouvrez dnsmgmt.msc sur le serveur.  
  
2.  Dans le Gestionnaire DNS, avec le bouton droit de la zone de recherche directe pour le domaine ActiveDirectory, puis cliquez sur **propriétés**.  
  
3.  Sur le **serveurs de noms**, vérifiez que les paramètres sont corrects.  
  
4.  Apportez les modifications nécessaires, puis cliquez sur **OK** pour enregistrer les paramètres.  
  
### <a name="no-dns-name-server-records"></a>Aucun enregistrement de serveur de nom DNS  
 **Problème:** aucun serveur de noms (NS) DNS ne des enregistrements de ressource dans la zone _msdcs pour votre serveur (par exemple: _msdcs.contoso.local).  
  
 **Impact:** si aucun enregistrement de ressource DNS nom serveur (NS) n’existe dans la zone _msdcs pour le domaine ActiveDirectory, les utilisateurs ne peuvent pas en mesure d’accéder aux ressources sur le réseau ou sur Internet.  
  
 **Résolution:**  
  
##### <a name="to-restore-missing-dns-name-server-records"></a>Pour restaurer les enregistrements de serveur de noms DNS manquants  
  
1.  Ouvrez dnsmgmt.msc sur le serveur.  
  
2.  Dans le Gestionnaire DNS, avec le bouton droit de la zone de recherche directe pour la zone _msdcs, puis cliquez sur **propriétés**.  
  
3.  Sur le **serveurs de noms**, vérifiez que les paramètres sont corrects.  
  
4.  Apportez les modifications nécessaires, puis cliquez sur **OK** pour enregistrer les paramètres.  
  
### <a name="no-dns-name-server-records"></a>Aucun enregistrement de serveur de nom DNS  
 **Problème:** n’existe aucun serveur de noms (NS) DNS des enregistrements de ressources pour la zone _msdcs déléguée transférer la zone de recherche.  
  
 **Impact:** si aucun enregistrement de ressource DNS nom serveur (NS) n’existe pour la zone _msdcs déléguée transférer la zone de recherche, le service serveur DNS ne peut pas résoudre les enregistrements de ressource DNS pour le domaine et ne pourra pas démarrer.  
  
 **Résolution:**  
  
##### <a name="to-reconfigure-missing-dns-name-server-records"></a>Pour reconfigurer les enregistrements de serveur de noms DNS manquants  
  
1.  Ouvrez dnsmgmt.msc sur le serveur.  
  
2.  Dans le Gestionnaire DNS, développez le nom de votre serveur, puis développez **Zones de recherche directe**.  
  
3.  Cliquez sur la zone de recherche directe pour votre domaine ActiveDirectory (par exemple: contoso.local).  
  
4.  La zone _msdcs déléguée apparaît sous la forme d’un dossier grisé. Avec le bouton droit de la zone _msdcs, puis cliquez sur **propriétés**.  
  
5.  Sur le **serveurs de noms**, vérifiez que les paramètres sont corrects.  
  
6.  Apportez les modifications nécessaires, puis cliquez sur **OK** pour enregistrer les paramètres.  
  
### <a name="authenticated-users-not-a-member-of-the-pre-windows-2000-compatible-access-group"></a>Utilisateurs authentifiés pas un membre du groupe accès Compatible pré-Windows2000  
 **Problème:** le groupe utilisateurs authentifiés n’est pas membre du groupe accès Compatible pré-Windows2000.  
  
 **Impact:** si le groupe des utilisateurs authentifiés n’est pas membre du groupe accès Compatible pré-Windows2000, les utilisateurs de réseau peuvent rencontrer des erreurs «Accès refusé».  
  
 **Résolution:**  
  
##### <a name="to-add-authenticated-users-to-the-pre-windows-2000-compatible-access-group"></a>Pour ajouter des utilisateurs authentifiés au groupe accès Compatible pré-Windows2000  
  
1.  Ouvrez dsa.msc sur le serveur.  
  
2.  Dans le **Builtin** dossier, faites un clic droit **accès Compatible pré-Windows2000**, puis cliquez sur **propriétés**.  
  
3.  Cliquez sur **ajouter**, type **utilisateurs authentifiés**, puis cliquez sur **OK** deux fois.  
  
### <a name="dns-client-not-configured"></a>Client DNS non configuré  
 **Problème:** le client DNS n’est pas configuré pour pointer uniquement vers l’adresse IP interne du serveur.  
  
 **Impact:** si le client DNS n’est pas configuré pour pointer uniquement vers l’adresse IP interne du serveur, la résolution de noms DNS peut échouer.  
  
 **Résolution:**  
  
##### <a name="to-configure-dns-to-point-only-to-the-server-s-internal-ip-address"></a>Pour configurer DNS pour pointer uniquement vers l’adresse IP interne du serveur s  
  
1.  À partir de l’ordinateur client, ouvrez le **propriétés** page de la connexion réseau.  
  
2.  Assurez-vous que le DNS est configuré pour pointer uniquement vers l’adresse IP interne du serveur.  
  
### <a name="default-application-pool-value-changed"></a>Valeur du Pool d’applications par défaut modifié  
 **Problème:** le nombre maximal de processus de travail pour le Pool d’Application DefaultAppPool n’est pas défini sur 1 la valeur par défaut.  
  
 **Impact:** les utilisateurs ne sont peut-être pas en mesure de se connecter à des services web de WindowsSmallBusinessServer.  
  
 **Résolution:**  
  
##### <a name="to-reset-maximum-worker-processes-for-the-default-application-pool"></a>Pour réinitialiser le nombre maximal de processus de travail pour le pool d’applications par défaut  
  
1.  Ouvrez le Gestionnaire Internet Information Services (IIS) sur le serveur.  
  
2.  Dans le Gestionnaire des services Internet, développez le nom de votre serveur, puis **Pools d’applications**.  
  
3.  Dans **Pools d’applications**, avec le bouton droit **DefaultAppPool**, puis cliquez sur **paramètres avancés**.  
  
4.  Dans **paramètres avancés**, modifiez la valeur de **nombre maximal de processus de travail** sur 1, puis cliquez sur **OK**.  
  
5.  Fermer **paramètres avancés**, avec le bouton droit **DefaultAppPool**, puis arrêtez et redémarrez le pool d’applications.  
  
### <a name="application-pool-for-remote-web-access-does-not-use-the-default-account"></a>Pool d’applications pour accès Web à distance n’utilise pas le compte par défaut  
 **Problème:** le pool d’applications RemoteAppPool n’est pas exécuté avec le compte par défaut.  
  
 **Impact:** utilisateurs du réseau n’est peut-être pas en mesure d’accéder au site Web accès Web à distance.  
  
 **Résolution:**  
  
##### <a name="to-reset-the-remote-application-pool-to-use-the-default-account"></a>Pour réinitialiser le pool d’applications à distance pour utiliser le compte par défaut  
  
1.  Ouvrez le Gestionnaire Internet Information Services (IIS) sur le serveur.  
  
2.  Dans le Gestionnaire des services Internet, développez le nom de votre serveur, puis **Pools d’applications**.  
  
3.  Dans **Pools d’applications**, avec le bouton droit **RemoteAppPool**, puis cliquez sur **paramètres avancés**.  
  
4.  Dans **paramètres avancés**, modifiez le **identité** à **NetworkService**, puis cliquez sur **OK**.  
  
5.  Fermer **paramètres avancés**, avec le bouton droit **RemoteAppPool**, puis arrêtez et redémarrez le pool d’applications.  
  
### <a name="application-pool-for-remote-web-access-does-not-use-the-default-net-framework-version"></a>Pool d’applications pour accès Web à distance n’utilise pas la version de .NETFramework par défaut  
 **Problème:** le pool d’applications RemoteAppPool n’est pas exécuté avec la version par défaut de Microsoft .NETFramework.  
  
 **Impact:** utilisateurs du réseau n’est peut-être pas en mesure d’accéder au site Web accès Web à distance.  
  
 **Résolution:**  
  
##### <a name="to-change-the-net-framework-version-used-by-the-remoteapppool"></a>Pour modifier la version de .NETFramework utilisée par RemoteAppPool  
  
1.  Ouvrez le Gestionnaire Internet Information Services (IIS) sur le serveur.  
  
2.  Dans le Gestionnaire des services Internet, développez le nom de votre serveur, puis **Pools d’applications**.  
  
3.  Dans **Pools d’applications**, avec le bouton droit **RemoteAppPool**, puis cliquez sur **paramètres avancés**.  
  
4.  Dans **paramètres avancés**, modifiez le **.NETFramework Version** sur v4.0, puis cliquez sur **OK**.  
  
5.  Fermer **paramètres avancés**, avec le bouton droit **RemoteAppPool**, puis arrêtez et redémarrez le pool d’applications.  
  
### <a name="the-remoteaccesslog-file-is-larger-than-1-gb-in-size"></a>Le fichier RemoteAccess.log est supérieur à 1Go  
 **Problème:** si la taille du fichier Remoteaccess.log dépasse 1Go, vous pouvez rencontrer des erreurs d’espace disque insuffisant sur le lecteur système.  
  
 **Impact:** le fichier Remoteaccess.log est trop importante, il peut éventuellement provoquer de problème d’espace libre: s sur le lecteur C:.  
  
 **Résolution:** une fois que vous sauvegardez le serveur, vous pouvez supprimer le fichier Remoteaccess.log qui se trouve dans le dossier %ProgramData%\Microsoft\Windows Server\Logs\WebApps.  
  
### <a name="default-web-sites-log-directory-is-over-1-gb-in-size"></a>Répertoire des journaux du site Web par défaut est de plus de 1Go  
 **Problème:** si la taille du dossier de journalisation du site Web par défaut dépasse 1Go, vous pouvez rencontrer des erreurs d’espace disque insuffisant sur le lecteur système.  
  
 **Impact:** dossier de journalisation du site Web par défaut est trop importante, peut entraîner l’espace libre problème: s sur le lecteur C:  
  
 **Résolution:** une fois que vous sauvegardez le serveur, et lorsque le site Web par défaut est arrêté, vous pouvez supprimer les fichiers journaux dans le dossier C:\inetpub\logs\LogFiles\W3SVC1. Démarrez ensuite le site Web par défaut.  
  
### <a name="no-binding-for-ssl-on-all-ip-addresses"></a>Aucune liaison pour SSL sur toutes les adresses IP  
 **Problème:** aucune liaison pour SSL (Secure Sockets Layer) sur toutes les adresses IP sur le serveur.  
  
 **Impact:** si SSL n’est pas lié à toutes les adresses IP sur le serveur, certains sites Web ne sera pas disponible pour les utilisateurs.  
  
 **Résolution:**  
  
##### <a name="to-bind-ssl-to-all-ip-addresses-on-the-server"></a>Pour effectuer une liaison SSL à toutes les adresses IP sur le serveur  
  
1.  Ouvrez le Gestionnaire Internet Information Services (IIS) sur le serveur.  
  
2.  Dans le Gestionnaire IIS, sur le **connexions** volet, développez votre serveur, **Sites**, avec le bouton droit **site Web par défaut**, puis cliquez sur **modifier les liaisons**.  
  
3.  Dans **liaisons de Site**, cliquez sur **ajouter**, puis sélectionnez les paramètres suivants:  
  
    -   **Type** = **https**  
  
    -   **Adresse IP** = **toutes non attribuées**  
  
    -   **Port** = 443  
  
4.  Sélectionnez un certificat SSL, puis cliquez sur **OK** pour enregistrer vos modifications.  
  
### <a name="no-binding-for-ssl-on-the-default-web-site"></a>Aucune liaison pour SSL sur le site Web par défaut  
 **Problème:** aucune liaison pour SSL sur le site Web par défaut.  
  
 **Impact:** si SSL n’est pas lié au site Web par défaut, certains sites Web ne peut pas être disponible pour les utilisateurs.  
  
 **Résolution:**  
  
##### <a name="to-bind-ssl-to-the-default-website"></a>Pour lier SSL au site Web par défaut  
  
1.  Ouvrez le Gestionnaire Internet Information Services (IIS) sur le serveur.  
  
2.  Dans le Gestionnaire IIS, sur le **connexions** volet, développez votre serveur, **Sites**, avec le bouton droit **site Web par défaut**, puis cliquez sur **modifier les liaisons**.  
  
3.  Dans **liaisons de Site**, cliquez sur **ajouter**, puis sélectionnez les options suivantes:  
  
    -   **Type** = **https**  
  
    -   **Adresse IP** = **toutes non attribuées**  
  
    -   **Port** = 443  
  
    > [!NOTE]
    >  Si une liaison HTTPS pour le port 443 existe pour une adresse IP spécifique, modifiez le **adresse IP** attribut pour cette liaison sur **toutes non attribuées**. L’exception à cela est l’adresse IP 127.0.0.1. Ne modifiez pas la liaison pour 127.0.0.1.  
  
4.  Sélectionnez un certificat SSL, puis cliquez sur **OK** pour enregistrer vos modifications.  
  
### <a name="a-certificate-expires-within-30-days"></a>Un certificat expire dans 30jours  
 **Problème:** votre certificat de serveur expire dans 30jours.  
  
 **Impact:** le serveur ne peut pas utiliser un certificat expiré. Si le certificat expire, les utilisateurs ne peuvent pas être en mesure d’utiliser les fonctions d’accès en tout lieu.  
  
 **Résolution:** pour empêcher le certificat de point d’expirer, renouveler le certificat avec votre autorité de Certification.  
  
### <a name="certificate-subject-does-not-match-the-name-configured-by-domain-name-wizard"></a>Objet du certificat ne correspond pas au nom configuré par l’Assistant nom de domaine  
 **Problème:** l’objet du certificat ne correspond pas au nom configuré par l’Assistant nom du domaine.  
  
 **Impact:** si l’objet du certificat ne correspond pas à celui qui a été configuré par l’Assistant de nom de domaine, certains sites Web ne pourront pas être initialisés. Autres sites afficheront l’erreur «Il est un problème avec le certificat de sécurité de ce site Web».  
  
 **Résolution:** pour résoudre ce problème:, réexécutez l’Assistant de l’accès en tout lieu de configurer et fournissez le nom de domaine correct pour le certificat ou acheter un nouveau certificat qui correspond au nom de domaine que vous souhaitez utiliser.  
  
### <a name="one-or-more-user-accounts-have-duplicate-cn-names"></a>Un ou plusieurs comptes d’utilisateur ont des noms CN dupliqués  
 **Problème:** un ou plusieurs comptes d’utilisateur ont des noms CN dupliqués: {0}.  
  
 **Impact:** si les comptes d’utilisateur ont des noms CN dupliqués, les utilisateurs ne soient pas en mesure d’ouvrir une session sur le réseau. En outre, les recherches ActiveDirectory pour les utilisateurs peuvent renvoyer des valeurs incorrectes.  
  
 **Résolution:** pour résoudre ce problème:, assurez-vous que les comptes d’utilisateur réseau n’ont pas de doublons «CN = «noms. Pour ce faire, exportez les contenus d’Active dans un fichier texte pour passer en revue. Pour plus d’informations sur la procédure à suivre, voir [à l’aide de LDIFDE pour importer et exporter des objets d’annuaire vers ActiveDirectory (article de la Base de connaissances 237677)](https://support.microsoft.com/kb/237677) (https://support.microsoft.com/kb/237677).  
  
### <a name="nt-backup-is-installed"></a>NT Backup est installé.  
 **Problème:** le programme WindowsNTBackup est installé sur le serveur.  
  
 **Impact:** WindowsServerEssentials utilise la sauvegarde de Windows Server. Si le programme de sauvegarde de Windows NT est également installé, les conflits peuvent survenir entre les deux programmes de sauvegarde. Cela peut entraîner l’échec du processus de sauvegarde de Windows Server. Les conflits peuvent également vous empêcher d’utiliser une sauvegarde pour restaurer le serveur.  
  
 **Résolution:** pour résoudre ce problème:, désinstallez le programme NT Backup à partir du serveur.  
  
### <a name="iis-does-not-own-port-80-000080-or-port-443-0000443"></a>IIS n’appartient pas au port 80 (0.0.0.0:80) ou 443 (0.0.0.0: 443)  
 **Problème:** Internet Information Services (IIS) ne possède pas le port 80 (0.0.0.0:80) ou le Port 443. Ces ports sont actuellement liés par d’autres applications.  
  
 **Impact:** applications web de WindowsServerEssentials nécessitent l’utilisation du port 80 et le port 443 pour que les services disponibles pour les utilisateurs. Si un autre processus ou une application utilise déjà le port 80 ou 443, les applications web de WindowsServerEssentials ne peut pas s’exécuter. Si cela se produit, accès Web à distance et autres applications ne sont pas disponibles pour les utilisateurs.  
  
 **Résolution:** pour résoudre ce problème:, soit désinstaller l’application qui est déjà à l’aide du port 80 ou 443, ou affecter cette application à un autre port.  
  
### <a name="the-default-website-is-not-running"></a>Le site Web par défaut n’est pas en cours d’exécution.  
 **Problème:** le site Web par défaut n’est pas en cours d’exécution dans votre environnement WindowsServerEssentials.  
  
 **Impact:** applications web de WindowsServerEssentials nécessitent l’utilisation du site Web par défaut. Si le site Web par défaut n’est pas en cours d’exécution, accès Web à distance et autres applications ne sont pas disponibles pour les utilisateurs.  
  
 **Résolution:**  
  
##### <a name="to-start-the-default-website"></a>Pour démarrer le site Web par défaut  
  
1.  Ouvrez le Gestionnaire Internet Information Services (IIS) sur le serveur.  
  
2.  Dans le Gestionnaire des services Internet, développez le nom de votre serveur, puis **Sites**.  
  
3.  Avec le bouton droit **site Web par défaut**, pointez sur **gérer le site Web**, puis cliquez sur **Démarrer**.  
  
### <a name="read-and-script-permissions-for-the-remote-virtual-directory-are-incorrect"></a>Autorisations de lecture et Script pour le répertoire virtuel /Remote sont incorrectes  
 **Problème:** autorisations lecture et Script ne sont pas attribuées au répertoire virtuel /Remote.  
  
 **Impact:** si les autorisations de lecture et Script pour le répertoire virtuel /Remote sont incorrectes, les utilisateurs ne peuvent pas utiliser accès Web à distance. Lorsqu’ils essaient d’accès Web à distance permet de naviguer sur Internet, ils peuvent rencontrer l’erreur «HTTP Error 403.1 interdit.»  
  
 **Résolution:**  
  
##### <a name="to-assign-read-and-script-permissions-to-the-remote-directory"></a>Pour affecter des autorisations de lecture et Script dans le répertoire /Remote  
  
1.  Ouvrez le Gestionnaire Internet Information Services (IIS) sur le serveur.  
  
2.  Dans le Gestionnaire des services Internet, développez le nom de votre serveur, puis **Sites**.  
  
3.  Développez **site Web par défaut**, puis développez **distant**.  
  
4.  Dans **affichage des fonctionnalités**, double-cliquez sur **mappages de gestionnaires**.  
  
5.  Sur le **Actions** volet, cliquez sur **modifier les autorisations de fonctionnalité**.  
  
6.  Sélectionnez le **lecture** et **Script** cases à cocher, puis cliquez sur **OK**.  
  
### <a name="http-redirect-is-either-set-or-inherited-on-the-remote-virtual-directory"></a>Redirection HTTP est définie ou héritée sur le répertoire virtuel /Remote  
 **Problème:** l’attribut de redirection HTTP est inattendu définie ou héritée sur le répertoire virtuel /Remote.  
  
 **Impact:** si l’attribut de redirection HTTP est défini sur le répertoire virtuel /Remote, Remote Web Workplace ne fonctionne pas correctement.  
  
 **Résolution:**  
  
##### <a name="to-remove-the-http-redirect-attribute"></a>Pour supprimer l’attribut de redirection HTTP  
  
1.  Ouvrez le Gestionnaire Internet Information Services (IIS) sur le serveur.  
  
2.  Dans le Gestionnaire des services Internet, développez le nom de votre serveur, puis **Sites**.  
  
3.  Développez **site Web par défaut**, puis développez **distant**.  
  
4.  Dans **affichage des fonctionnalités**, double-cliquez sur **redirection HTTP**.  
  
5.  Désactivez le **rediriger les demandes vers cette destination** case à cocher, puis cliquez sur **appliquer** sur le **Actions** volet.  
  
### <a name="a-host-name-exists-for-port-80-on-the-default-website"></a>Un nom d’hôte existe pour le port 80 sur le site Web par défaut  
 **Problème:** un nom d’hôte est affecté au port 80 sur le site Web par défaut.  
  
 **Impact:** si un nom d’hôte est affecté au port 80 sur le site Web par défaut, vous ne serez peut-être pas en mesure de se connecter à certaines applications web de WindowsServerEssentials. Un nom d’hôte n’est pas nécessaire et n’est pas recommandé dans cette situation  
  
 **Résolution:**  
  
##### <a name="to-clear-the-host-name-entry-for-port-80-on-the-default-website"></a>Pour effacer l’entrée de nom d’hôte pour le port 80 sur le site Web par défaut  
  
1.  Ouvrez le Gestionnaire Internet Information Services (IIS) sur le serveur.  
  
2.  Dans le Gestionnaire des services Internet, développez le nom de votre serveur, puis **Sites**.  
  
3.  Dans **affichage des fonctionnalités**, avec le bouton droit **site Web par défaut**, puis cliquez sur **liaisons**.  
  
4.  Dans **liaisons de Site**, sélectionnez le **http pour le port 80** définition, puis cliquez sur **modifier**.  
  
5.  Dans **modifier la liaison de Site**, désactivez le **nom d’hôte** entrée, puis cliquez sur **OK**.  
  
### <a name="backup-does-not-succeed-because-of-a-hidden-partition"></a>Échec de la sauvegarde en raison d’une partition cachée  
 **Problème:** une partition non-NTFS est planifiée pour la sauvegarde par la sauvegarde de Windows Server.  
  
 **Impact:** sauvegarde de Windows Server peut uniquement sauvegarder les partitions au format NTFS.  
  
 **Résolution:** ne configurez pas de sauvegarde de Windows Server pour sauvegarder les partitions non-NTFS. Pour plus d’informations, voir [ID d’événements 12290 et 16387enregistrés en cas d’échec de sauvegarde de l’état système sur un ordinateur Windows Server2008 (article de la Base de connaissances 968128)](https://support.microsoft.com/kb/968128) (https://support.microsoft.com/kb/968128).  
  
### <a name="the-most-recent-backup-did-not-succeed"></a>La dernière sauvegarde a échoué.  
 **Problème:** la dernière tentative de sauvegarde n’a pas abouti.  
  
 **Impact:** l’état de sauvegarde pour le système n’est pas correcte.  
  
 **Résolution:** passer en revue les journaux des événements et les journaux de sauvegarde pour les erreurs qui s’est produite lors de la sauvegarde la plus récente.  
  
### <a name="the-startup-type-for-the-file-replication-service-is-not-set-to-automatic"></a>Le type de démarrage pour le Service de réplication de fichiers n’est pas défini sur automatique  
 **Problème:** le Service de réplication de fichiers (FRS) peut ne pas démarrer si le type de démarrage n’est pas défini sur la valeur par défaut automatique.  
  
 **Impact:** si le Service de réplication de fichiers n’est pas en cours d’exécution, le contrôleur de domaine pourra cesser de publier ses services. Cela peut entraîner d’autres problèmes tels que des erreurs d’ouverture de session et de stratégie de groupe.  
  
 **Résolution:**  
  
##### <a name="to-configure-the-file-replication-service-for-automatic-startup"></a>Pour configurer le Service de réplication pour un démarrage automatique  
  
1.  Ouvrez la console Services.  
  
2.  Dans la liste des services, double-cliquez sur **la réplication de fichiers**.  
  
3.  Pour **type de démarrage**, sélectionnez **automatique**, puis cliquez sur **appliquer**.  
  
### <a name="the-file-replication-service-is-not-running"></a>Le Service de réplication de fichiers n’est pas en cours d’exécution.  
 **Problème:** le Service de réplication de fichiers n’est pas en cours d’exécution.  
  
 **Impact:** si le Service de réplication de fichiers n’est pas en cours d’exécution, le contrôleur de domaine pourra cesser de publier ses services. Ce comportement peut entraîner d’autres problèmes tels que des erreurs d’ouverture de session et de stratégie de groupe.  
  
 **Résolution:**  
  
##### <a name="to-start-the-file-replication-service"></a>Pour démarrer le Service de réplication de fichiers  
  
1.  Ouvrez la console Services.  
  
2.  Dans la liste des services, double-cliquez sur **Service de réplication de fichiers**.  
  
3.  Cliquez sur **Démarrer**.  
  
### <a name="the-logon-account-for-the-file-replication-service-is-not-set-to-use-the-local-system-account"></a>Le compte d’ouverture de session pour le Service de réplication de fichiers n’est pas configuré pour utiliser le compte système Local  
 **Problème:** le Service de réplication de fichiers n’est pas configuré pour utiliser le compte système Local comme compte d’ouverture de session par défaut.  
  
 **Impact:** si le Service de réplication de fichiers n’utilise pas de système Local comme compte d’ouverture de session par défaut, vous pouvez rencontrer des erreurs d’autorisation. Ces erreurs peuvent déclencher d’autres erreurs et peuvent conduire le contrôleur de domaine à cesser de publier ses services.  
  
 **Résolution:**  
  
##### <a name="to-configure-local-system-as-the-default-logon-account-for-file-replication"></a>Pour configurer le système Local comme compte d’ouverture de session par défaut pour la réplication de fichiers  
  
1.  Ouvrez la console Services.  
  
2.  Dans la liste des services, double-cliquez sur **la réplication de fichiers**.  
  
3.  Sur le **propriétés du Service**, cliquez sur le **ouverture de session** onglet.  
  
4.  Sélectionnez le **compte système Local** option, puis cliquez sur **appliquer**.  
  
5.  Redémarrez le service.  
  
### <a name="the-startup-type-for-the-dfs-replication-service-is-not-set-to-automatic"></a>Le type de démarrage pour le service de réplication DFS n’est pas défini sur automatique  
 **Problème:** le service de réplication DFS ne peut pas démarrer si le type de démarrage n’est pas défini sur la valeur par défaut automatique.  
  
 **Impact:** si le service de réplication DFS n’est pas en cours d’exécution, le contrôleur de domaine pourra cesser de publier ses services. Cela peut entraîner d’autres problèmes tels que des erreurs d’ouverture de session et de stratégie de groupe.  
  
 **Résolution:**  
  
##### <a name="to-configure-the-dfs-replication-service-for-automatic-startup"></a>Pour configurer le service de réplication DFS pour un démarrage automatique  
  
1.  Ouvrez la console Services.  
  
2.  Dans la liste des services, double-cliquez sur **la réplication DFS**.  
  
3.  Pour **type de démarrage**, sélectionnez **automatique**, puis cliquez sur **appliquer**.  
  
### <a name="the-dfs-replication-service-is-not-running"></a>Le service de réplication DFS n’est pas en cours d’exécution.  
 **Problème:** le service de réplication DFS n’est pas en cours d’exécution.  
  
 **Impact:** si le service de réplication DFS n’est pas en cours d’exécution, le contrôleur de domaine pourra cesser de publier ses services. Ce comportement peut entraîner d’autres problèmes tels que des erreurs d’ouverture de session et de stratégie de groupe.  
  
 **Résolution:**  
  
##### <a name="to-start-the-dfs-replication-service"></a>Pour démarrer le service de réplication DFS  
  
1.  Ouvrez la console Services.  
  
2.  Dans la liste des services, double-cliquez sur **la réplication DFS**.  
  
3.  Cliquez sur **Démarrer**.  
  
### <a name="the-dfs-replication-service-is-not-is-not-set-to-use-the-local-system-account"></a>Le service n’est pas la réplication DFS n’est pas configurée pour utiliser le compte système Local  
 **Problème:** le service de réplication DFS n’est pas configuré pour utiliser le compte système Local comme compte d’ouverture de session par défaut.  
  
 **Impact:** si le service de réplication DFS n’utilise pas de système Local comme compte d’ouverture de session par défaut, vous pouvez rencontrer des erreurs d’autorisation. Ces erreurs peuvent déclencher d’autres erreurs et peuvent conduire le contrôleur de domaine à cesser de publier ses services.  
  
 **Résolution:**  
  
##### <a name="to-configure-dfs-replication-to-use-local-system-as-the-default-logon-account"></a>Pour configurer la réplication DFS pour utiliser le système Local comme compte d’ouverture de session par défaut  
  
1.  Ouvrez la console Services.  
  
2.  Dans la liste des services, double-cliquez sur **la réplication DFS**.  
  
3.  Sur le **propriétés du Service**, cliquez sur le **ouverture de session** onglet.  
  
4.  Sélectionnez le **compte système Local** option, puis cliquez sur **appliquer**.  
  
5.  Redémarrez le service.  
  
### <a name="the-windows-server-office-365-integration-service-is-not-set-to-use-the-local-system-account"></a>Le Service d’intégration WindowsServerOffice 365 n’est pas configuré pour utiliser le compte système Local  
 **Problème:** le Service d’intégration WindowsServerOffice 365 n’est pas défini pour utiliser le compte système Local comme compte d’ouverture de session par défaut.  
  
 **Impact:** si le Service d’intégration Office 365 pour Windows Server n’utilise pas de système Local comme compte d’ouverture de session par défaut, certaines fonctionnalités d’Office 365 ne peuvent pas fonctionner correctement. Vous pouvez également rencontrer des erreurs d’autorisation.  
  
 **Résolution:**  
  
##### <a name="to-configure-the-office-365-integration-service-to-use-local-system-as-the-default-logon-account"></a>Pour configurer le Service d’intégration Office 365 pour utiliser le système Local comme compte d’ouverture de session par défaut  
  
1.  Ouvrez la console Services.  
  
2.  Dans la liste des services, double-cliquez sur **Service d’intégration Office 365 pour Windows Server**.  
  
3.  Sur le **propriétés du Service**, cliquez sur le **ouverture de session** onglet.  
  
4.  Sélectionnez le **compte système Local** option, puis cliquez sur **appliquer**.  
  
5.  Redémarrez le service.  
  
### <a name="the-windows-server-office-365-integration-service-is-not-running"></a>Le Service d’intégration WindowsServerOffice 365 n’est pas en cours d’exécution.  
 **Problème:** le Service d’intégration WindowsServerOffice 365 n’est pas en cours d’exécution.  
  
 **Impact:** si le Service d’intégration WindowsServerOffice 365 n’est pas en cours d’exécution, les fonctions basées sur le cloud d’Office 365 ne sont pas disponibles.  
  
 **Résolution:**  
  
##### <a name="to-start-the-windows-server-office-365-integration-service"></a>Pour démarrer le Service d’intégration WindowsServerOffice 365  
  
1.  Ouvrez la console Services.  
  
2.  Dans la liste des services, double-cliquez sur **Service d’intégration Office 365 pour Windows Server**.  
  
3.  Cliquez sur **Démarrer**.  
  
### <a name="the-startup-type-for-the-windows-server-office-365-integration-service-is-not-set-to-automatic"></a>Le type de démarrage pour le Service d’intégration WindowsServerOffice 365 n’est pas défini sur automatique  
 **Problème:** le Service d’intégration WindowsServerOffice 365 ne peut pas démarrer si le type de démarrage n’est pas défini sur la valeur par défaut automatique.  
  
 **Impact:** si le Service d’intégration WindowsServerOffice 365 n’est pas en cours d’exécution, les fonctions basées sur le cloud d’Office 365 ne sont pas disponibles.  
  
 **Résolution:**  
  
##### <a name="to-configure-the-office-365-integration-service-for-automatic-startup"></a>Pour configurer le Service d’intégration Office 365 pour un démarrage automatique  
  
1.  Ouvrez la console Services.  
  
2.  Dans la liste des services, double-cliquez sur **Service d’intégration Office 365 pour Windows Server**.  
  
3.  Pour **type de démarrage**, sélectionnez **automatique**, puis cliquez sur **appliquer**.  
  
### <a name="a-registry-value-is-missing-or-set-incorrectly"></a>Une valeur de Registre est manquante ou mal définie  
 **Problème:** une clé de Registre sous HKEY_LOCAL_MACHINE \Software\Microsoft\Rpc\RpcProxy contient des valeurs incorrectes ou n’existe pas.  
  
 **Impact:** si la clé de Registre RPCProxy est mal configurée, vous pouvez recevoir un message d’erreur semblable au suivant: «votre ordinateur ne peut pas se connecter à l’ordinateur distant car le serveur de passerelle Bureau à distance n’est temporairement pas disponible. Essayez de vous reconnecter ultérieurement ou contactez votre administrateur réseau».  
  
 **Résolution:**  
  
##### <a name="to-correct-the-registry-setting"></a>Pour corriger le paramètre de Registre  
  
1.  Ouvrez l’Éditeur du Registre.  
  
2.  Accédez à la clé de Registre suivante:  
  
     Niveau de HKEY_LOCAL_MACHINE\Software\Microsoft\Rpc\RpcProxy  
  
3.  Vérifiez que la chaîne appelée «Site Web» a une valeur de données de site Web par défaut:  
  
    -   Si la valeur de données est incorrecte, modifiez la chaîne pour utiliser la valeur correcte.  
  
    -   Si la chaîne n’existe pas, créez une nouvelle chaîne appelée «Site Web» et définissez la valeur de données sur le site Web par défaut.»  
  
### <a name="the-startup-type-for-the-block-level-backup-engine-service-is-not-set-to-manual"></a>Le type de démarrage pour le Service de moteur de sauvegarde de niveau bloc n’est pas défini sur manuel  
 **Problème:** le Service de moteur de sauvegarde de niveau bloc n’utilise pas le type de démarrage par défaut du manuel.  
  
 **Impact:** le Service de moteur de sauvegarde de niveau bloc ne peut pas démarrer si le type de démarrage n’est pas défini sur manuel. Ce problème: peut entraîner l’échec des tâches de sauvegarde de Windows Server.  
  
 **Résolution:**  
  
##### <a name="to-configure-the-block-level-backup-engine-service-for-manual-startup"></a>Pour configurer le Service de moteur de sauvegarde de niveau bloc pour un démarrage manuel  
  
1.  Ouvrez la console Services.  
  
2.  Dans la liste des services, double-cliquez sur **Service moteur de sauvegarde en mode bloc**.  
  
3.  Pour **type de démarrage**, sélectionnez **manuel**, puis cliquez sur **appliquer**.  
  
### <a name="the-logon-account-for-the-block-level-backup-engine-service-is-not-set-to-use-the-local-system-account"></a>Le compte d’ouverture de session pour le Service de moteur de sauvegarde de niveau bloc n’est pas configuré pour utiliser le compte système Local  
 **Problème:** le Service de moteur de sauvegarde de niveau bloc n’est pas défini pour utiliser le compte système Local comme compte d’ouverture de session par défaut.  
  
 **Impact:** si le Service moteur de sauvegarde en mode bloc n’utilise pas de système Local comme compte d’ouverture de session par défaut, vous pouvez rencontrer des erreurs d’autorisation. Ces erreurs peuvent empêcher les tâches de sauvegarde de Windows Server avec succès.  
  
 **Résolution:**  
  
##### <a name="to-configure-the-block-level-backup-engine-service-to-use-local-system-as-the-default-logon-account"></a>Pour configurer le Service de moteur de sauvegarde de niveau bloc pour utiliser le système Local comme compte d’ouverture de session par défaut  
  
1.  Ouvrez la console Services.  
  
2.  Dans la liste des services, double-cliquez sur **Service moteur de sauvegarde en mode bloc**.  
  
3.  Sur le **propriétés du Service**, cliquez sur le **ouverture de session** onglet.  
  
4.  Sélectionnez le **compte système Local** option, puis cliquez sur **appliquer**.  
  
5.  Redémarrez le service.  
  
### <a name="the-common-name-on-the-certificate-that-is-bound-to-the-wss-certificate-web-service-website-does-not-match-the-server-name"></a>Le nom commun du certificat qui est lié au site Web de Service Web certificat WSS ne correspond pas au nom de serveur  
 **Problème:** un certificat non valide est lié au site Web de Service Web certificat WSS dans IIS. Le nom commun sur ce certificat ne correspond pas le nom du serveur.  
  
 **Impact:** si vous liez un certificat non valide pour le site Web du Service Web certificat WSS, l’Assistant de connexion ne peut pas fonctionner correctement.  
  
 **Résolution:**  
  
##### <a name="to-configure-a-valid-certificate-for-the-wss-certificate-web-service"></a>Pour configurer un certificat valide pour le Service Web de certificat WSS  
  
1.  Ouvrez le Gestionnaire Internet Information Services (IIS) sur le serveur.  
  
2.  Dans le Gestionnaire des services Internet, développez le nom de votre serveur, puis **Sites**.  
  
3.  Avec le bouton droit **Service Web certificat WSS**, puis cliquez sur **modifier les liaisons**.  
  
4.  Dans **liaisons de Site**, cliquez sur **HTTPS**, puis cliquez sur **modifier**.  
  
5.  Dans **modifier la liaison de Site**, pour **certificat SSL**, sélectionnez le certificat qui a le même nom que votre serveur.  
  
6.  Si plusieurs entrées de certificat possède le même nom que votre serveur, cliquez sur **affichage** pour déterminer quel certificat est valide, puis sélectionnez le certificat approprié.  
  
### <a name="there-appears-to-be-a-problem-with-the-certificate-binding-for-the-remote-desktop-gateway-service"></a>Il doit s’agir d’un problème avec la liaison de certificat pour le service passerelle des services Bureau à distance  
 **Problème:** le certificat pour le service passerelle des services Bureau à distance semble être mal lié.  
  
 **Impact:** si le certificat pour le service passerelle des services Bureau à distance n’est pas configuré correctement, les utilisateurs ne peuvent pas se connecter à accès Web à distance.  
  
 **Résolution:**  
  
##### <a name="to-fix-the-binding-for-the-remote-desktop-gateway-service"></a>Pour corriger la liaison pour le service passerelle des services Bureau à distance  
  
-   Ouvrez une invite de commandes en tant qu’administrateur et entrez les commandes suivantes:  
  
    ```  
    REG ADD HKLM\SYSTEM\CurrentControlSet\services\HTTP\Parameters\SslBindingInfo\0.0.0.0:443  /v DefaultFlags /t REG_DWORD /d 1 /f  
    net stop tsgateway  
    net start tsgateway  
    ```  
  
     Pour plus d’informations, voir [comment gérer le Service de passerelle Bureau à distance dans WindowsServerEssentials (article de la Base de connaissances 2472211)](https://support.microsoft.com/kb/2472211) (https://support.microsoft.com/kb/2472211).
