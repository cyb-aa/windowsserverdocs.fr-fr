---
title: Règles utilisées par l’outil BPA (Best Practices Analyzer) de Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850690"
---
# <a name="rules-used-by-the-windows-server-essentials-best-practices-analyzer-bpa-tool"></a>Règles utilisées par l’outil BPA (Best Practices Analyzer) de Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Cet article décrit les règles utilisées par le Windows Server Essentials Best Practices Analyzer (BPA). L’outil BPA examine un serveur qui exécute Windows Server Essentials et présente un rapport qui décrit les problèmes et fournit des recommandations pour les résoudre. Les recommandations sont développées par l’organisation de support technique pour Windows Server Essentials.  
  
## <a name="using-the-tool"></a>Utilisation de l’outil  
 Il est conseillé, lorsque vous migrez vers Windows Server Essentials à partir de Windows Server 2011 Essentials, Windows Small Business Server 2011 Essentials ou Windows Home Server 2011, pour exécuter l’outil BPA sur le serveur de Destination après avoir terminé la migration votre paramètres et données. Vous pouvez à tout moment exécuter l’outil à partir du tableau de bord.  
  
#### <a name="to-run-the--windows-server-essentials-bpa-on-the-server"></a>Pour exécuter l’outil BPA de Windows Server Essentials sur le serveur  
  
1.  Connectez-vous au serveur en tant qu’administrateur, puis ouvrez le tableau de bord.  
  
2.  Dans le tableau de bord, cliquez sur l'onglet **Périphériques**.  
  
3.  Dans le volet **Tâches du serveur**, cliquez sur **Best Practices Analyzer**.  
  
4.  Passez en revue chaque message du BPA et suivez les instructions pour résoudre les problèmes si nécessaire.  
  
## <a name="rules-used-by-the-best-practices-analyzer"></a>Règles utilisées par l’outil Best Practices Analyzer  
  
### <a name="disable-ip-filtering"></a>Désactiver le filtrage IP  
 **Problème :** Le filtrage IP est actuellement activé sur le serveur. Vous devez le désactiver.  
  
 **Impact :** Le trafic du réseau peut se bloquer si le filtrage IP est activé.  
  
 **Résolution :**  
  
##### <a name="to-disable-ip-filtering"></a>Pour désactiver le filtrage IP  
  
1.  Ouvrez le fichier regedit.exe sur le serveur.  
  
2.  Naviguez jusqu’à HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters.  
  
3.  Cliquez avec le bouton droit sur **EnableSecurityFilters**, puis cliquez sur **Modifier**.  
  
4.  Dans la fenêtre **Modifier la valeur DWORD 32 bits**, indiquez 0 dans le champ des **données de la valeur**, puis cliquez sur **OK**.  
  
5.  Redémarrez le serveur pour que la modification soit effective.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-should-be-set-to-start-automatically-by-default"></a>Le service MSDTC (Microsoft Distributed Transaction Coordinator) doit être configuré pour démarrer automatiquement par défaut.  
 **Problème :** Le service MSDTC n’est pas configuré pour démarrer automatiquement.  
  
 **Impact :** Il se peut que le service MSDTC ne démarre pas automatiquement au démarrage du serveur. En cas d’arrêt du service, certaines fonctions SQL ou COM pourraient échouer. Par conséquent, les applications utilisant des fonctions SQL ou COM pourraient ne pas fonctionner correctement.  
  
 **Résolution :**  
  
##### <a name="to-configure-the-msdtc-service-to-start-automatically"></a>Pour configurer le service MSDTC en vue d’un démarrage automatique  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur le service **Distributed Transaction Coordinator**, puis cliquez sur **Propriétés**.  
  
3.  Dans l’onglet **Général**, définissez le **Type de démarrage** sur **Automatique (début différé)**, puis cliquez sur **OK**.  
  
### <a name="the-netlogon-service-should-be-configured-to-start-automatically-by-default"></a>Par défaut le service Netlogon doit être configuré pour démarrer automatiquement par défaut  
 **Problème :** Le service Netlogon n’est pas configuré pour démarrer automatiquement.  
  
 **Impact :**  Il se peut que le service Netlogon ne démarre pas automatiquement lorsque le serveur démarre. En cas d’arrêt du service, le serveur pourrait ne plus être en mesure d’authentifier les utilisateurs et les services.  
  
 **Résolution :**  
  
##### <a name="to-configure-the-netlogon-service-to-start-automatically"></a>Pour configurer le service Netlogon pour qu’il démarre automatiquement  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur le service **Netlogon**, puis cliquez sur **Propriétés**.  
  
3.  Dans l’onglet **Général** , modifiez le **Type de démarrage** en spécifiant **Automatique**, puis cliquez sur **OK**.  
  
### <a name="the-dns-client-service-should-be-configured-to-start-automatically-by-default"></a>Par défaut, le service Client DNS doit être configuré pour démarrer automatiquement.  
 **Problème :**  Le service Client DNS n’est pas configuré pour démarrer automatiquement.  
  
 **Impact :**  Il se peut que le service Client DNS ne démarre pas automatiquement au démarrage du serveur. En cas d’arrêt du service, le serveur pourrait ne plus être en mesure de résoudre des noms DNS.  
  
 **Résolution :**  
  
##### <a name="to-configure-the-dns-client-service-to-start-automatically"></a>Pour configurer le service Client DNS en vue d’un démarrage automatique  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur le service **Client DNS**, puis cliquez sur **Propriétés**.  
  
3.  Dans l’onglet **Général** , modifiez le **Type de démarrage** en spécifiant **Automatique**, puis cliquez sur **OK**.  
  
### <a name="the-dns-server-service-should-be-configured-to-start-automatically-by-default"></a>Le service Serveur DNS doit être configuré pour démarrer automatiquement par défaut.  
 **Problème :**  Le service Serveur DNS n’est pas configuré pour démarrer automatiquement.  
  
 **Impact :**  Il se peut que le service Client DNS ne démarre pas automatiquement au démarrage du serveur. En cas d’arrêt du service, les mises à jour DNS ne seront pas exécutées.  
  
 **Résolution :**  
  
##### <a name="to-configure-the-dns-server-service-to-start-automatically"></a>Pour configurer le service Serveur DNS en vue d’un démarrage automatique  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur le service **Serveur DNS**, puis cliquez sur **Propriétés**.  
  
3.  Dans l’onglet **Général** , modifiez le **Type de démarrage** en spécifiant **Automatique**, puis cliquez sur **OK**.  
  
### <a name="active-directory-web-services-is-not-set-to-the-default-start-mode"></a>Les services Web Active Directory ne sont pas configurés en mode de démarrage par défaut  
 **Problème :**  Les services Web Active Directory ne sont pas configurés en mode automatique de démarrage par défaut.  
  
 **Impact :**  Les services Web Active Directory (ADWS) ne sont pas configurés par défaut en mode de démarrage automatique. Si les services Web Active Directory (ADWS) sont arrêtés ou désactivés sur le serveur, les applications clientes comme le module Active Directory pour Windows PowerShell ou le Centre d’administration Active Directory ne peuvent pas accéder aux instances du service d’annuaire exécutées sur ce serveur ou les gérer. Pour plus d'informations, consultez [Nouveautés dans AD DS : Services Web Active Directory](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) dans la bibliothèque technique Windows Server.  
  
 **Résolution :**  
  
##### <a name="to-configure-the-active-directory-web-services-service-to-start-automatically"></a>Pour configurer les Services Web Active Directory en vue d’un démarrage automatique  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur **Services Web Active Directory**, puis cliquez sur **Propriétés**.  
  
3.  Dans l’onglet **Général** , modifiez le **Type de démarrage** en spécifiant **Automatique**, puis cliquez sur **OK**.  
  
### <a name="the-dhcp-client-service-should-be-configured-to-start-automatically-by-default"></a>Le service client DHCP doit être configuré pour démarrer automatiquement par défaut  
 **Problème :**  Le service client DHCP n’est pas configuré pour démarrer automatiquement.  
  
 **Impact :**  Le service Client DHCP ne démarrera pas automatiquement au démarrage du serveur. En cas d’arrêt du service, les ordinateurs client ne seront plus en mesure de recevoir une adresse IP depuis le serveur.  
  
 **Résolution :**  
  
##### <a name="to-configure-the-dhcp-client-service-to-start-automatically"></a>Pour configurer le service client DHCP en vue d’un démarrage automatique  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur **Client DHCP**, puis cliquez sur **Propriétés**.  
  
3.  Dans l’onglet **Général** , modifiez le **Type de démarrage** en spécifiant **Automatique**, puis cliquez sur **OK**.  
  
### <a name="the-iis-admin-service-should-be-configured-to-start-automatically-by-default"></a>Le service d’administration IIS doit être configuré pour démarrer automatiquement par défaut.  
 **Problème :** Le service d’administration IIS n’est pas configuré pour démarrer automatiquement.  
  
 **Impact :** Le service d’administration IIS ne démarrera pas automatiquement au démarrage du serveur. En cas d’arrêt du service, vous ne pourrez peut-être pas accéder aux sites Internet exécutés sur le serveur, comme par exemple l’Accès Web à distance.  
  
 **Résolution :**  
  
##### <a name="to-configure-the-iis-admin-service-to-start-automatically"></a>Pour configurer le service d’administration IIS en vue d’un démarrage automatique  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur **Service d’administration IIS**, puis cliquez sur **Propriétés**.  
  
3.  Dans l’onglet **Général** , modifiez le **Type de démarrage** en spécifiant **Automatique**, puis cliquez sur **OK**.  
  
### <a name="the-world-wide-web-publishing-service-should-be-configured-to-start-automatically-by-default"></a>Le service de publication World Wide Web doit être configuré pour démarrer automatiquement par défaut  
 **Problème :**  Le service de publication World Wide Web n’est pas configuré pour démarrer automatiquement.  
  
 **Impact :**  Il se peut que le service de publication World Wide Web ne démarre pas automatiquement au démarrage du serveur. En cas d’arrêt du service, vous ne pourrez peut-être pas accéder aux sites Internet exécutés sur le serveur, comme par exemple l’Accès Web à distance.  
  
 **Résolution :**  
  
##### <a name="to-configure-the-world-wide-web-publishing-service-to-start-automatically"></a>Pour configurer le service de publication World Wide Web en vue d’un démarrage automatique  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur **Service de publication World Wide Web**, puis cliquez sur **Propriétés**.  
  
3.  Dans l’onglet **Général** , modifiez le **Type de démarrage** en spécifiant **Automatique**, puis cliquez sur **OK**.  
  
### <a name="the-remote-registry-service-should-be-configured-to-start-automatically-by-default"></a>Le service d’accès à distance au Registre doit être configuré pour démarrer automatiquement par défaut  
 **Problème :**  Le service d’accès à distance au Registre n’est pas configuré pour démarrer automatiquement.  
  
 **Impact :**  
  
 Il se peut que le service d’accès à distance au Registre ne démarre pas automatiquement au démarrage du serveur. En cas d’arrêt du service, vous ne pourrez peut-être pas exécuter certaines des opérations réseau à distance.  
  
 **Résolution :**  
  
##### <a name="to-configure-the-remote-registry-service-to-start-automatically"></a>Pour configurer le service Registre à distance en vue d’un démarrage automatique  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur le service **Registre à distance**, puis cliquez sur **Propriétés**.  
  
3.  Dans l’onglet **Général** , modifiez le **Type de démarrage** en spécifiant **Automatique**, puis cliquez sur **OK**.  
  
### <a name="the-remote-desktop-gateway-service-should-be-configured-to-start-automatically-by-default"></a>Le service Passerelle des services Bureau à distance doit être configuré pour démarrer automatiquement par défaut  
 **Problème :**  Le service Passerelle des services Bureau à distance n’est pas configuré pour démarrer automatiquement.  
  
 **Impact :**  En cas d’arrêt du service, il se peut que les utilisateurs ne soient pas en mesure d’accéder aux ordinateurs qui utilisent l’accès Web à distance.  
  
 **Résolution :**  
  
##### <a name="to-configure-the-remote-desktop-gateway-service-to-start-automatically"></a>Pour configurer le service Passerelle des services Bureau à distance en vue d’un démarrage automatique  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur le service **Passerelle des services Bureau à distance**, puis cliquez sur **Propriétés**.  
  
3.  Dans l’onglet **Général**, définissez le **Type de démarrage** sur **Automatique (début différé)**, puis cliquez sur **OK**.  
  
### <a name="the-windows-time-service-should-be-configured-to-start-automatically-by-default"></a>Service de temps Windows doit être configuré pour démarrer automatiquement par défaut  
 **Problème :**  Service de temps Windows n’est pas configuré pour démarrer automatiquement.  
  
 **Impact :**  En cas d’arrêt du service, la synchronisation horaire et la synchronisation des données ne seront pas disponibles.  
  
 **Résolution :**  
  
##### <a name="to-configure-the-windows-time-service-to-start-automatically"></a>Pour configurer le service Horloge Windows en vue d’un démarrage automatique  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur **Horloge Windows**, puis cliquez sur **Propriétés**.  
  
3.  Dans l’onglet **Général** , modifiez le **Type de démarrage** en spécifiant **Automatique**, puis cliquez sur **OK**.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-should-be-started"></a>Le service MSDTC (Distributed Transaction Coordinator) devrait démarrer  
 **Problème :**  Le service MSDTC n’est pas en cours d’exécution sur le serveur.  
  
 **Impact :**  En cas d’arrêt du service, certaines fonctions SQL ou COM pourraient échouer. Par conséquent, les applications utilisant des fonctions SQL ou COM pourraient ne pas fonctionner correctement.  
  
 **Résolution :**  
  
##### <a name="to-start-the-distributed-transaction-coordinator-service"></a>Pour démarrer le service MSDTC (Microsoft Distributed Transaction Coordinator)  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur le service **Distributed Transaction Coordinator**, puis cliquez sur **Démarrer**.  
  
### <a name="the-netlogon-service-should-be-started"></a>Le service Netlogon devrait démarrer  
 **Problème :**  Le service Netlogon n’est pas en cours d’exécution sur le serveur.  
  
 **Impact :**  Si le service n’est pas démarré, le serveur pourrait ne pas être en mesure d’authentifier les utilisateurs et les services.  
  
 **Résolution :**  
  
##### <a name="to-start-the-netlogon-service"></a>Pour démarrer le service Netlogon  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur le service **Netlogon**, puis cliquez sur **Démarrer**.  
  
### <a name="the-dns-client-service-should-be-started"></a>Le service Client DNS devrait démarrer  
 **Problème :**  Le service Client DNS n’est pas en cours d’exécution sur le serveur.  
  
 **Impact :**  Si le service n’est pas démarré, le serveur pourrait ne pas être en mesure de résoudre des noms DNS.  
  
 **Résolution :**  
  
##### <a name="to-start-the-dns-client-service"></a>Pour démarrer le service Client DNS  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur le service **Client DNS**, puis cliquez sur **Démarrer**.  
  
### <a name="the-dns-server-service-should-be-started"></a>Le service Serveur DNS devrait démarrer  
 **Problème :**  Le service Serveur DNS n’est pas en cours d’exécution sur le serveur.  
  
 **Impact :**  Si le service Serveur DNS n’est pas démarré, il se peut que les mises à jour DNS ne soient pas exécutées.  
  
 **Résolution :**  
  
##### <a name="to-start-the-dns-server-service"></a>Pour démarrer le service Serveur DNS  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez sur le compte du service **Serveur DNS**, puis sur **Démarrer**.  
  
### <a name="active-directory-web-services-is-not-started"></a>Les Services Web Active Directory ne se sont pas lancés  
 **Problème :**  Les Services Web Active Directory ne se sont pas démarrés.  
  
 **Impact :**  Les services Web Active Directory (ADWS) ne sont pas démarrés. Si les services Web Active Directory (ADWS) sont arrêtés ou désactivés sur le serveur, les applications clientes comme le module Active Directory pour Windows PowerShell ou le Centre d’administration Active Directory ne peuvent pas accéder aux instances du service d’annuaire exécutées sur ce serveur ou les gérer. Pour plus d'informations, consultez [Nouveautés dans AD DS : Services Web Active Directory](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) dans la bibliothèque technique Windows Server.  
  
 **Résolution :**  
  
##### <a name="to-start-the-active-directory-web-services-service"></a>Pour démarrer les Services Web Active Directory  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur **Services Web Active Directory**, puis cliquez sur **Démarrer**.  
  
### <a name="the-dhcp-client-service-should-be-started"></a>Le service Client DHCP devrait démarrer  
 **Problème :**  Le service Client DHCP n’est pas en cours d’exécution sur le serveur.  
  
 **Impact :**  En cas d’arrêt du service, les ordinateurs client ne seront plus en mesure de recevoir une adresse IP depuis le serveur.  
  
 **Résolution :**  
  
##### <a name="to-start-the-dhcp-client-service"></a>Pour démarrer le service Client DHCP  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur le service **Client DHCP**, puis cliquez sur **Démarrer**.  
  
### <a name="the-iis-admin-service-should-be-started"></a>Le service d’administration IIS devrait démarrer  
 **Problème :**  Le service d’administration IIS n’est pas en cours d’exécution sur le serveur.  
  
 **Impact :**  En cas d’arrêt du service, vous ne pourrez peut-être pas accéder aux sites Internet exécutés sur le serveur, comme par exemple l’Accès Web à distance.  
  
 **Résolution :**  
  
##### <a name="to-start-the-iis-admin-service"></a>Pour démarrer le service d’administration IIS  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur **Service d’administration IIS**, puis cliquez sur **Démarrer**.  
  
### <a name="the-world-wide-web-publishing-service-should-be-started"></a>Le service de publication World Wide Web devrait démarrer  
 **Problème :**  Le service de publication World Wide Web n’est pas en cours d’exécution sur le serveur.  
  
 **Impact :**  En cas d’arrêt du service, vous ne pourrez peut-être pas accéder aux sites Internet exécutés sur le serveur, comme par exemple l’Accès Web à distance.  
  
 **Résolution :**  
  
##### <a name="to-start-the-world-wide-web-publishing-service"></a>Pour démarrer le service de publication World Wide Web  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur **Service de publication World Wide Web**, puis cliquez sur **Démarrer**.  
  
### <a name="the-remote-desktop-gateway-service-should-be-started"></a>Le service Passerelle des services Bureau à distance devrait démarrer.  
 **Problème :**  Le service Passerelle des services Bureau à distance n’est pas en cours d’exécution sur le serveur.  
  
 **Impact :**  En cas d’arrêt du service, il se peut que les utilisateurs ne soient pas en mesure d’accéder aux ordinateurs qui utilisent l’accès Web à distance.  
  
 **Résolution :**  
  
##### <a name="to-start-the-remote-desktop-gateway-service"></a>Pour démarrer le Service de passerelle Bureau à distance  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur le service **Passerelle des services Bureau à distance**, puis cliquez sur **Démarrer**.  
  
### <a name="the-windows-time-service-should-be-started"></a>Le service de temps Windows devrait démarrer  
 **Problème :**  Le service de temps Windows n’est pas en cours d’exécution sur le serveur.  
  
 **Impact :**  En cas d’arrêt du service, la synchronisation horaire et la synchronisation des données ne seront pas disponibles.  
  
 **Résolution :**  
  
##### <a name="to-start-the-windows-time-service"></a>Pour démarrer le Service de temps Windows  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur **Horloge Windows**, puis cliquez sur **Démarrer**.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-logon-account-should-be-nt-authoritynetwork-service"></a>Le compte d’ouverture de session du service MSDTC (Distributed Transaction Coordinator) devrait être NT AUTHORITY\\Network Service  
 **Problème :**  Le compte d’ouverture de session par défaut pour le service MSDTC (Distributed Transaction Coordinator) a changé.  
  
 **Impact :**  Le service ne dispose peut-être pas des permissions appropriées requises pour fonctionner correctement. Par conséquent, les applications qui utilisent des fonctions SQL ou COM peuvent ne pas fonctionnent correctement.  
  
 **Résolution :**  
  
##### <a name="to-change-the-logon-account-for-the-service"></a>Pour changer le compte d’ouverture de session pour du service  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur le service **Distributed Transaction Coordinator**, puis cliquez sur **Propriétés**.  
  
3.  Dans l’onglet **Ouvrir une session**, sélectionnez **Ce compte**, entrez **NT AUTHORITY\Network Service**, puis cliquez sur **OK**.  
  
### <a name="the-netlogon-service-should-use-the-local-system-account-as-its-logon-account"></a>Le service Netlogon doit utiliser le compte Système local comme compte d’ouverture de session  
 **Problème :**  Le compte d’ouverture de session par défaut pour le service Netlogon a changé.  
  
 **Impact :**  Le service ne dispose peut-être pas des permissions appropriées requises pour fonctionner correctement. Par conséquent, il se peut que le serveur ne soit pas en mesure d’authentifier les utilisateurs et les services.  
  
 **Résolution :**  
  
##### <a name="to-change-the-netlogon-service-logon-account"></a>Pour changer le compte d’ouverture de session du service Netlogon  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur le service **Netlogon**, puis cliquez sur **Propriétés**.  
  
3.  Dans l’onglet **Ouvrir une session**, sélectionnez **Compte système Local**.  
  
### <a name="the-dns-client-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>Le service Client DNS doit utiliser le compte NT AUTHORITY\\Network Service comme compte d’ouverture de session  
 **Problème :**  Le compte d’ouverture de session par défaut pour le service Client DNS a changé.  
  
 **Impact :**  Le service ne dispose peut-être pas des permissions appropriées requises pour fonctionner correctement. Par conséquent, il se peut que le serveur ne soit pas en mesure de résoudre des noms DNS.  
  
 **Résolution :**  
  
##### <a name="to-change-the-dns-client-service-logon-account"></a>Pour changer le compte d’ouverture de session du service Client DNS  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur le service **Client DNS**, puis cliquez sur **Propriétés**.  
  
3.  Dans l’onglet **Ouvrir une session**, cliquez sur **Ce compte**, puis entrez **NT AUTHORITY\Network Service**.  
  
### <a name="the-dns-server-service-should-use-the-local-system-account-as-its-logon-account"></a>Le service serveur DNS doit utiliser le compte système Local comme compte d’ouverture de session  
 **Problème :**  Le compte d’ouverture de session par défaut pour le service Serveur DNS a changé.  
  
 **Impact :**  Le service ne dispose peut-être pas des permissions appropriées requises pour fonctionner correctement. Par conséquent, les mises à jour DNS ne seront peut-être pas exécutées.  
  
 **Résolution :**  
  
##### <a name="to-change-the-dns-server-service-logon-account"></a>Pour changer le compte d’ouverture de session du service Serveur DNS  
  
1.  Sur le serveur, ouvrez services.msc  
  
2.  Cliquez avec le bouton droit sur le service **Serveur DNS**, puis cliquez sur **Propriétés**.  
  
3.  Dans l’onglet **Ouvrir une session**, sélectionnez **Compte système Local**.  
  
### <a name="active-directory-web-services-is-not-the-default-logon-account"></a>Le compte Services Web Active Directory n’est pas le compte d’ouverture de session par défaut  
 **Problème :**  Le compte Services Web Active Directory n’est pas le compte d’ouverture de session par défaut. Par défaut, le compte d’ouverture de session est défini sur **Compte système local**.  
  
 **Impact :**  Les services Web Active Directory (ADWS) ne sont pas démarrés. Si les services Web Active Directory (ADWS) sont arrêtés ou désactivés sur le serveur, les applications clientes comme le module Active Directory pour Windows PowerShell ou le Centre d’administration Active Directory ne peuvent pas accéder aux instances du service d’annuaire exécutées sur ce serveur ou les gérer. Pour plus d'informations, consultez [Nouveautés dans AD DS : Services Web Active Directory](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) dans la bibliothèque technique Windows Server.  
  
 **Résolution :**  
  
##### <a name="to-change-the-active-directory-web-services-logon-account"></a>Pour changer le compte d’ouverture de session des Services Web Active Directory  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur **Services Web Active Directory**, puis cliquez sur **Propriétés**.  
  
3.  Définissez le **Type de démarrage** sur **Automatique**, puis cliquez sur **OK**.  
  
4.  Cliquez sur l’onglet **Ouvrir une session** dans les **propriétés** des Services Web Active Directory.  
  
5.  Sélectionnez l’option **Compte système local**, puis cliquez sur **OK**.  
  
### <a name="the-windows-update-service-should-use-the-local-system-account-as-its-logon-account"></a>Le service Windows Update doit utiliser le compte Système local comme compte d’ouverture de session  
 **Problème :**  Le compte d’ouverture de session par défaut pour le service des mises à jour automatiques a changé.  
  
 **Impact :**  Le service ne dispose peut-être pas des permissions appropriées requises pour fonctionner correctement. Par conséquent, il se peut que le serveur ne soit pas en mesure de recevoir les mises à jour automatiques.  
  
 **Résolution :**  
  
##### <a name="to-change-the-windows-update-service-logon-account"></a>Pour changer le compte d’ouverture de session du service Windows Update  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur le service **Windows Update**, puis cliquez sur Propriétés.  
  
3.  Dans l’onglet **Ouvrir une session**, sélectionnez **Compte système Local**.  
  
### <a name="the-dhcp-client-service-should-use-the-nt-authoritylocalservice-account-as-its-logon-account"></a>Le service Client DHCP doit utiliser le compte NT AUTHORITY\\LocalService comme compte d’ouverture de session  
 **Problème :**  Le compte d’ouverture de session par défaut pour le service Client DHCP a changé.  
  
 **Impact :**  Le service ne dispose peut-être pas des permissions appropriées requises pour fonctionner correctement. Par conséquent, l’ordinateur client ne pourra pas recevoir d’adresses IP du serveur.  
  
 **Résolution :**  
  
##### <a name="to-change-the-dhcp-client-service-logon-account"></a>Pour changer le compte d’ouverture de session du service Client DHCP  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur **Client DHCP**, puis cliquez sur **Propriétés**.  
  
3.  Dans l’onglet **Ouvrir une session**, cliquez sur **Ce compte**, puis tapez **NT AUTHORITY\Local Service**.  
  
### <a name="the-iis-admin-service-should-use-the-local-system-account-as-its-logon-account"></a>Le service d’administration IIS doit utiliser le compte Système local comme compte d’ouverture de session  
 **Problème :**  Le compte d’ouverture de session par défaut pour le service Admin IIS a changé.  
  
 **Impact :**  Le service ne dispose peut-être pas des permissions appropriées requises pour fonctionner correctement. Par conséquent, il se peut que vous ne puissiez pas accéder aux sites Internet exécutés sur le serveur, comme par exemple l’accès Web à distance.  
  
 **Résolution :**  
  
##### <a name="to-change-the-service-logon-account"></a>Pour changer le compte d’ouverture de session du service  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur **Service d’administration IIS**, puis cliquez sur **Propriétés**.  
  
3.  Dans l’onglet **Ouvrir une session**, sélectionnez **Compte système Local**.  
  
### <a name="the-world-wide-web-publishing-service-should-use-the-local-system-account-as-its-logon-account"></a>Le service de publication World Wide Web doit utiliser le compte Système local comme compte d’ouverture de session  
 **Problème :**  Le compte d’ouverture de session par défaut pour le service de publication World Wide Web a changé.  
  
 **Impact :**  Le service ne dispose peut-être pas des permissions appropriées requises pour fonctionner correctement. Par conséquent, il se peut que vous ne puissiez pas accéder aux sites Internet exécutés sur le serveur, comme par exemple l’accès Web à distance.  
  
 **Résolution :**  
  
##### <a name="to-change-the-world-wide-web-publishing-service-logon-account"></a>Pour changer le compte d’ouverture de session par défaut du service de publication World Wide Web  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur **Service de publication World Wide Web**, puis cliquez sur **Propriétés**.  
  
3.  Dans l’onglet **Ouvrir une session**, sélectionnez **Compte système Local**.  
  
### <a name="the-remote-desktop-gateway-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>e service Passerelle des services Bureau à distance doit utiliser le compte NT AUTHORITY\\Network Service comme compte d’ouverture de session  
 **Problème :**  Le compte d’ouverture de session par défaut pour le service Passerelle des services Bureau à distance a changé.  
  
 **Impact :**  Le service ne dispose peut-être pas des permissions appropriées requises pour fonctionner correctement. Par conséquent, il se peut que vous ne soyez pas en mesure d’accéder aux ordinateurs utilisant l’Accès Web à distance.  
  
 **Résolution :**  
  
##### <a name="to-change-the-remote-desktop-gateway-service-logon-account"></a>Pour changer le compte d’ouverture de session du service Passerelle des services Bureau à distance  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur le service **Passerelle des services Bureau à distance**, puis cliquez sur **Propriétés**.  
  
3.  Dans l’onglet **Ouvrir une session**, cliquez sur **Ce compte**, puis entrez **NT AUTHORITY\Network Service**.  
  
### <a name="the-windows-time-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>Le service de temps Windows doit utiliser le compte NT AUTHORITY\\Network Service comme compte d’ouverture de session  
 **Problème :**  Le compte d’ouverture de session par défaut pour le service de temps Windows a changé.  
  
 **Impact :**  Le service ne dispose peut-être pas des permissions appropriées requises pour fonctionner correctement. Par conséquent, il se peut que la synchronisation de la date et de l’heure ne soit pas disponible.  
  
 **Résolution :**  
  
##### <a name="to-change-the-windows-time-service-logon-account"></a>Pour changer le compte d’ouverture de session du service Horloge Windows  
  
1.  Ouvrez services.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur **Horloge Windows**, puis cliquez sur **Propriétés**.  
  
3.  Dans l’onglet **Ouvrir une session**, cliquez sur **Ce compte**, puis tapez **NT AUTHORITY\Local Service**.  
  
### <a name="the-built-in-administrators-group-does-not-have-the-right-to-log-on-as-batch-job"></a>Le groupe Administrateurs intégré ne dispose pas des droits pour se connecter en tant que traitement par lot  
 **Problème :**  Le groupe Administrateurs intégré ne dispose pas des droits pour se connecter en tant que traitement par lot.  
  
 **Impact :**  Si l’administrateur génère une alerte et configure cette alerte pour qu’elle s’exécute lorsque l’Administrateur n’est pas connecté, l’alerte échouera avec le code d’erreur 2147943785.  
  
 **Résolution :**  Pour plus d’informations sur la façon d’autoriser les administrateurs intégrés groupe à une session en tant que traitement par lots, consultez [donner au groupe administrateur intégré le droit Ouvrir une session en tant que traitement par lots](https://technet.microsoft.com/library/jj635076) (https://technet.microsoft.com/library/jj635076).  
  
### <a name="the-windows-firewall-is-turned-off"></a>Le Pare-feu Windows est désactivé  
 **Problème :**  Le pare-feu de Windows est désactivé. Sa valeur par défaut est Activé.  
  
 **Impact :**  En fonction des configurations de votre pare-feu, le Pare-feu Windows peut vous aider à protéger votre serveur et votre réseau des activités malveillantes en bloquant certaines informations qui passent par votre serveur.  
  
 **Résolution :**  
  
##### <a name="to-turn-on-windows-firewall-on-the-server"></a>Pour activer le Pare-feu Windows  
  
1.  Ouvrez le Panneau de configuration sur le serveur.  
  
2.  Dans le Panneau de configuration, cliquez sur **Système et sécurité**, puis cliquez sur **Pare-feu Windows**.  
  
3.  Dans Pare-feu Windows, cliquez sur **Activer ou désactiver le Pare-feu Windows**, cliquez sur **Activer le Pare-feu Windows**, puis cliquez sur **OK**.  
  
### <a name="the-internal-network-adapter-is-not-configured-to-register-ip-address-in-dns"></a>La carte réseau interne n’est pas configurée pour enregistrer une adresse IP dans le DNS  
 **Problème :**  La carte réseau interne n’est pas configurée pour enregistrer son adresse IP dans le DNS.  
  
 **Impact :**  Si l’adresse IP de la carte réseau interne n’est pas inscrit dans DNS, il n’est peut-être pas possible d’accéder au serveur à l’aide du nom de l’ordinateur serveur.  
  
 **Résolution :**  Vérifiez que votre carte réseau interne est configurée pour être enregistrée dans le DNS.  
  
### <a name="dns-the-values-for-the-dns-forwardingtimeout-and-recursiontimeout-registry-key-are-identical"></a>DNS : les valeurs des clés de registre ForwardingTimeout et RecursionTimeout du DNS sont identiques  
 **Problème :**  La valeur de la clé de registre ForwardingTimeout du DNS doit être différente de la valeur de la clé de registre RecursionTimeout.  
  
 **Impact :**  Vous ne pourrez peut-être pas accéder aux ressources Internet par nom.  
  
 **Résolution :**  Définissez la valeur de la clé de Registre RecursionTimeout de façon à ce qu’elle soit supérieure à celle de la clé ForwardingTimeout située dans le registre sous HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters.  
  
### <a name="the-forward-dns-zone-for-your-active-directory-domain-does-not-allow-secure-updates"></a>La zone de transfert DNS de votre domaine Active Directory ne vous permet pas d’effectuer des mises à jour de sécurité  
 **Problème :**  Vous devez configurer la zone de recherche directe pour n’autoriser que les mises à jour dynamiques sécurisées.  
  
 **Impact :**  Lorsque les mises à jour dynamiques sécurisées sont activées, seuls les utilisateurs et les hôtes autorisés peuvent modifier les enregistrements.  
  
 **Résolution :**  
  
##### <a name="to-configure-the-forward-lookup-zone-for-your-active-directory-domain"></a>Pour configurer la zone de recherche directe pour votre domaine Active Directory  
  
1.  Ouvrez dnsmgmt.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur la zone de recherche directe pour votre domaine Active Directory, puis cliquez sur **Propriétés**.  
  
3.  Dans la liste déroulante **Mises à jour dynamiques**, sélectionnez **Sécurisé uniquement**, puis cliquez sur **OK**.  
  
### <a name="the-forward-dns-zone-does-not-allow-secure-updates"></a>La zone de transfert DNS ne permet pas l’exécution de mises à jour sécurisées  
 **Problème :**  Vous devez configurer la zone de recherche directe pour que la zone _msdcs.* n’autorise que les mises à jour dynamiques sécurisées.  
  
 **Impact :**  Lorsque les mises à jour dynamiques sécurisées sont activées, seuls les utilisateurs et les hôtes autorisés peuvent modifier les enregistrements dans la zone msdcs.*.  
  
 **Résolution :**  
  
##### <a name="to-allow-secure-updates-in-the-msdcs-zone"></a>Pour autoriser les mises à jour sécurisées dans la zone _msdcs  
  
1.  Ouvrez dnsmgmt.msc sur le serveur.  
  
2.  Cliquez avec le bouton droit sur la zone de recherche directe pour la zone _msdcs, puis cliquez sur **Propriétés**.  
  
3.  Dans la liste déroulante **Mises à jour dynamiques**, sélectionnez **Sécurisé uniquement**, puis cliquez sur **OK**.  
  
### <a name="internet-explorer-enhanced-security-configuration-is-not-enabled"></a>La Configuration de sécurité renforcée d’Internet Explorer n’est pas activée.  
 **Problème :**  Internet Explorer Configuration de sécurité renforcée (IE ESC) n’est actuellement pas activée pour le groupe Administrateurs.  
  
 **Impact :**  Si la Configuration de sécurité renforcée d’Internet Explorer n’est pas activée pour le groupe Administrateurs, votre serveur et Internet Explorer sont plus exposés aux attaques malveillantes provenant de contenus Internet et des scripts des applications.  
  
 **Résolution :**  
  
##### <a name="to-enable-internet-explorer-enhanced-security-configuration"></a>Pour activer la Configuration de sécurité renforcée d’Internet Explorer  
  
1.  Ouvrez le **Gestionnaire de serveur** sur le serveur, puis cliquez sur **Serveur local**.  
  
2.  Dans le volet **Propriétés**, définissez le paramètre de la **Configuration de sécurité renforcée d’Internet Explorer** sur **Activé**, puis cliquez sur **OK**.  
  
### <a name="internet-explorer-enhanced-security-configuration-is-not-enabled"></a>La Configuration de sécurité renforcée d’Internet Explorer n’est pas activée.  
 **Problème :**  Internet Explorer Configuration de sécurité renforcée (IE ESC) n’est actuellement pas activée pour le groupe d’utilisateurs.  
  
 **Impact :**  Si la Configuration de sécurité renforcée d’Internet Explorer n’est pas activée pour le groupe Administrateurs, votre serveur et Internet Explorer sont plus exposés aux attaques malveillantes provenant du contenus Internet et de scripts des applications.  
  
 **Résolution :**  
  
##### <a name="to-enable-internet-explorer-enhanced-security-configuration"></a>Pour activer la Configuration de sécurité renforcée d’Internet Explorer  
  
1.  Ouvrez le **Gestionnaire de serveur**, puis cliquez sur **Serveur local**.  
  
2.  Dans le volet **Propriétés**, définissez le paramètre de la **Configuration de sécurité renforcée d’Internet Explorer** sur **Activé**, puis cliquez sur **OK**.  
  
### <a name="the-source-server-remains-in-active-directory-sites-and-services"></a>Le serveur source demeure dans les Sites et services Active Directory  
 **Problème :**  Le serveur source exécutant Windows Small Business Server existe toujours dans les Sites et services Active Directory dans le Nom-premier-site-par-défaut.  
  
 **Impact :**  Si le serveur source demeure dans les Sites Active Directory et Services, les ordinateurs clients peuvent rencontrer des problème de connectivité : s.  
  
 **Résolution :**  Vous devez abaisser d’un niveau le serveur source, le retirer du domaine, puis supprimer le serveur source des Sites et services Active Directory et des Utilisateurs et ordinateurs Active Directory.  
  
### <a name="source-server-remains-in-sbscomputer-ou"></a>Le serveur source demeure dans l’ordinateur OU SBS  
 **Problème :**  Le serveur source exécutant Windows Small Business Server existe toujours dans les Utilisateurs et ordinateurs Active Directory.  
  
 **Impact :**  Si le serveur source demeure dans Active Directory Users and Computers, les ordinateurs clients peuvent rencontrer des problème de connectivité : s.  
  
 **Résolution :**  Vous devez abaisser d’un niveau le serveur source, le retirer du domaine, puis supprimer le serveur source des Sites et services Active Directory et des Utilisateurs et ordinateurs Active Directory.  
  
### <a name="a-group-policy-is-missing"></a>Il manque une stratégie de groupe  
 **Problème :**  Il manque une stratégie de groupe pour le domaine par défaut.  
  
 **Impact :**  La stratégie du domaine par défaut est requise pour un fonctionnement correct du domaine.  
  
 **Résolution :**  
  
##### <a name="to-restore-a-missing-group-policy"></a>Pour restaurer une stratégie de groupe manquante  
  
1.  Ouvrez gpmc.msc sur le serveur.  
  
2.  Dans le Gestionnaire de stratégie de groupe, étendez la forêt de domaine et recherchez l’arborescence de la console de l’objet de la stratégie de groupe de la **stratégie de groupe du domaine par défaut**.  
  
3.  Si la stratégie n’apparaît pas dans l’arborescence, vous devrez la restaurer depuis une sauvegarde de l’état du système.  
  
### <a name="no-dns-name-server-resource-records"></a>Aucun enregistrement de ressource du serveur de nom DNS  
 **Problème :**  Il n’existe aucun enregistrement de ressource de serveur de nom DNS dans la zone de recherche directe pour votre serveur.  
  
 **Impact :**  Si aucun enregistrement de ressource du serveur de nom DNS n’existe dans la zone de recherche directe pour le domaine Active Directory, il se peut que les utilisateurs ne puissent pas accéder aux ressources du réseau ou à Internet.  
  
 **Résolution :**  
  
##### <a name="to-restore-missing-dns-name-server-resource-records"></a>Pour restaurer les enregistrements de ressources de serveurs de noms DNS manquants  
  
1.  Ouvrez dnsmgmt.msc sur le serveur.  
  
2.  Dans le Gestionnaire DNS, cliquez avec le bouton droit sur la zone de recherche directe pour votre domaine Active Directory, puis cliquez sur **Propriétés**.  
  
3.  Vérifiez que les paramètres dans l’onglet **Serveurs de noms** sont corrects.  
  
4.  Effectuez le cas échéant les modifications nécessaires, puis cliquez sur **OK** pour enregistrer les paramètres.  
  
### <a name="no-dns-name-server-records"></a>Aucun enregistrement du serveur de nom DNS  
 **Problème :**  Il n’existe aucun enregistrement de ressources du serveur de nom DNS dans la zone _msdcs pour votre serveur (par exemple : _msdcs.contoso.local).  
  
 **Impact :**  Si aucun enregistrement de ressource du serveur de nom n’existe dans la zone _msdcs pour le domaine Active Directory, il se peut que les utilisateurs ne puissent pas accéder aux ressources du réseau ou à Internet.  
  
 **Résolution :**  
  
##### <a name="to-restore-missing-dns-name-server-records"></a>Pour restaurer les enregistrements de serveurs de noms DNS manquants  
  
1.  Ouvrez dnsmgmt.msc sur le serveur.  
  
2.  Dans le Gestionnaire DNS, cliquez avec le bouton droit sur la zone de recherche directe pour la zone _msdcs, puis cliquez sur **Propriétés**.  
  
3.  Vérifiez que les paramètres dans l’onglet **Serveurs de noms** sont corrects.  
  
4.  Effectuez le cas échéant les modifications nécessaires, puis cliquez sur **OK** pour enregistrer les paramètres.  
  
### <a name="no-dns-name-server-records"></a>Aucun enregistrement du serveur de nom DNS  
 **Problème :**  Il n’existe aucun enregistrement de ressource du serveur de nom DNS dans la zone de recherche directe _msdcs déléguée.  
  
 **Impact :**  Si aucun enregistrement de ressource du serveur de nom n’existe dans la zone de recherche directe _msdcs déléguée, le service du serveur DNS ne pourra résoudre les enregistrements de ressource DNS pour le domaine et ne pourra pas démarrer.  
  
 **Résolution :**  
  
##### <a name="to-reconfigure-missing-dns-name-server-records"></a>Pour reconfigurer les enregistrements de serveurs de noms DNS manquants  
  
1.  Ouvrez dnsmgmt.msc sur le serveur.  
  
2.  Dans le Gestionnaire DNS, étendez votre nom de serveur, puis étendez les **zones de recherche directe**.  
  
3.  Cliquez sur la zone de recherche directe pour votre domaine Active Directory (par exemple : contoso.local).  
  
4.  La zone _msdcs déléguée s’affiche dans un dossier grisé. Cliquez avec le bouton droit sur la zone _msdcs, puis cliquez sur **Propriétés**.  
  
5.  Vérifiez que les paramètres dans l’onglet **Serveurs de noms** sont corrects.  
  
6.  Effectuez le cas échéant les modifications nécessaires, puis cliquez sur **OK** pour enregistrer les paramètres.  
  
### <a name="authenticated-users-not-a-member-of-the-pre-windows-2000-compatible-access-group"></a>Les utilisateurs authentifiés ne sont pas membres du groupe Accès compatible pré-Windows 2000  
 **Problème :**  Le groupe des utilisateurs authentifiés n’est pas membre du groupe Accès compatible pré-Windows 2000.  
  
 **Impact :**  Si le groupe des utilisateurs authentifiés n’est pas membre du groupe Accès compatible pré-Windows 2000, les utilisateurs du réseau peuvent rencontrer des erreurs « Accès refusé ».  
  
 **Résolution :**  
  
##### <a name="to-add-authenticated-users-to-the-pre-windows-2000-compatible-access-group"></a>Pour ajouter les utilisateurs authentifiés au groupe Accès compatible pré-Windows 2000  
  
1.  Ouvrez dsa.msc sur le serveur.  
  
2.  Dans le dossier **intégré**, cliquez avec le bouton droit sur **Accès compatible pré-Windows 2000**, puis cliquez sur **Propriétés**.  
  
3.  Cliquez sur **Ajouter**, saisissez **Utilisateurs authentifiés** et cliquez deux fois sur **OK**.  
  
### <a name="dns-client-not-configured"></a>Le client DNS n’est pas configuré  
 **Problème :**  Le Client DNS n’est pas configuré pour pointer uniquement vers l’adresse IP interne du serveur.  
  
 **Impact :**  Si le Client DNS n’est pas configuré pour pointer uniquement vers l’adresse IP interne du serveur, la résolution du nom DNS peut échouer.  
  
 **Résolution :**  
  
##### <a name="to-configure-dns-to-point-only-to-the-server-s-internal-ip-address"></a>Pour configurer DNS pour pointer uniquement vers l’adresse IP interne de serveur s  
  
1.  Ouvrez la page des **Propriétés** de la connexion réseau depuis l’ordinateur client.  
  
2.  Assurez-vous que le DNS est configuré pour pointer uniquement vers l’adresse IP interne du serveur.  
  
### <a name="default-application-pool-value-changed"></a>La valeur du pool d’applications par défaut a changé  
 **Problème :**  Le nombre de processus de travail maximum pour le pool d’application DefaultAppPool n’est pas défini sur la valeur 1 par défaut.  
  
 **Impact :**  Les utilisateurs ne seront peut-être pas en mesure de se connecter aux services basés sur le Web de Windows SBS.  
  
 **Résolution :**  
  
##### <a name="to-reset-maximum-worker-processes-for-the-default-application-pool"></a>Pour réinitialiser le nombre maximal de processus de travail pour le pool d’applications par défaut  
  
1.  Ouvrez le Gestionnaire d’Internet Information Services (IIS) sur le serveur.  
  
2.  Dans le gestionnaire IIS, étendez votre nom de serveur, puis cliquez sur **Pools d’applications**.  
  
3.  Dans **Pools d’applications**, cliquez avec le bouton droit sur **DefaultAppPool**, puis cliquez sur **Paramètres avancés**.  
  
4.  Dans les **Paramètres avancés**, définissez la valeur du **Nombre maximal de processus de travail** sur 1, puis cliquez sur **OK**.  
  
5.  Fermez les **Paramètres avancés**, cliquez avec le bouton droit sur **DefaultAppPool**, puis arrêtez et redémarrez le pool d’applications.  
  
### <a name="application-pool-for-remote-web-access-does-not-use-the-default-account"></a>Le pool d’application pour l’Accès web à distance n’utilise pas le compte par défaut  
 **Problème :**  Le pool d’applications RemoteAppPool n’est pas exécuté avec le compte par défaut.  
  
 **Impact :**  Les utilisateurs du réseau ne seront peut-être pas en mesure de se connecter au site de l’Accès web à distance.  
  
 **Résolution :**  
  
##### <a name="to-reset-the-remote-application-pool-to-use-the-default-account"></a>Pour réinitialiser le pool d’applications pour qu’il utilise le compte par défaut.  
  
1.  Ouvrez le Gestionnaire d’Internet Information Services (IIS) sur le serveur.  
  
2.  Dans le gestionnaire IIS, étendez votre nom de serveur, puis cliquez sur **Pools d’applications**.  
  
3.  Dans **Pools d’applications**, cliquez avec le bouton droit sur **RemoteAppPool**, puis cliquez sur **Paramètres avancés**.  
  
4.  Dans **Paramètres avancés**, remplacez le paramètre **Identité** par **ServiceRéseau**, puis cliquez sur**OK**.  
  
5.  Fermez **Paramètres avancés**, cliquez avec le bouton droit sur **RemoteAppPool**, puis arrêtez et redémarrez le pool d’applications.  
  
### <a name="application-pool-for-remote-web-access-does-not-use-the-default-net-framework-version"></a>Le pool d’applications pour l’Accès web à distance n’utilise pas la version de .NET Framework par défaut  
 **Problème :**  Le pool d’applications RemoteAppPool n’est pas exécuté avec la version par défaut de .NET Framework.  
  
 **Impact :**  Les utilisateurs du réseau ne seront peut-être pas en mesure de se connecter au site de l’Accès web à distance.  
  
 **Résolution :**  
  
##### <a name="to-change-the-net-framework-version-used-by-the-remoteapppool"></a>Pour modifier la version de .NET Framework utilisée par RemoteAppPool  
  
1.  Ouvrez le Gestionnaire d’Internet Information Services (IIS) sur le serveur.  
  
2.  Dans le gestionnaire IIS, étendez votre nom de serveur, puis cliquez sur **Pools d’applications**.  
  
3.  Dans **Pools d’applications**, cliquez avec le bouton droit sur **RemoteAppPool**, puis cliquez sur **Paramètres avancés**.  
  
4.  Dans **Paramètres avancés**, remplacez la **Version de .NET Framework** par v4.0, puis cliquez sur **OK**.  
  
5.  Fermez **Paramètres avancés**, cliquez avec le bouton droit sur **RemoteAppPool**, puis arrêtez et redémarrez le pool d’applications.  
  
### <a name="the-remoteaccesslog-file-is-larger-than-1-gb-in-size"></a>La taille du fichier RemoteAccess.log dépasse 1 Go  
 **Problème :**  Si le fichier Remoteaccess.log excède 1 Go, vous aurez peut-être des problèmes d’espace disque sur le lecteur système.  
  
 **Impact :**  Si le fichier Remoteaccess.log est trop volumineux, il risque de problème d’espace libre : s sur le lecteur C:.  
  
 **Résolution :**  Une fois le serveur sauvegardé, vous pouvez supprimer le fichier Remoteaccess.log qui se trouve sous %ProgramData%\Microsoft\Windows Server\Logs\WebApps.  
  
### <a name="default-web-sites-log-directory-is-over-1-gb-in-size"></a>La taille du répertoire des journaux du site Web par défaut dépasse 1 Go.  
 **Problème :**  Si la taille du dossier de journal du site Web par défaut dépasse 1 Go, vous pouvez rencontrer des erreurs d’espace disque faible sur le lecteur système.  
  
 **Impact :**  Dossier du journal du site Web par défaut est trop volumineux, peut entraîner l’espace libre problème : s sur le lecteur C:  
  
 **Résolution :**  Une fois la sauvegarde du serveur effectuée et le site Internet par défaut arrêté, vous pouvez supprimer les fichiers journaux dans le répertoire C:\inetpub\logs\LogFiles\W3SVC1. Démarrez ensuite le site Internet par défaut.  
  
### <a name="no-binding-for-ssl-on-all-ip-addresses"></a>Aucune liaison pour SSL sur toutes les adresses IP  
 **Problème :**  Aucune liaison pour SSL sur toutes les adresses IP sur le serveur.  
  
 **Impact :**  Si le SSL n’est pas lié à l’ensemble des adresses IP sur le serveur, certains sites web ne seront pas disponibles pour les utilisateurs.  
  
 **Résolution :**  
  
##### <a name="to-bind-ssl-to-all-ip-addresses-on-the-server"></a>Pour lier SSL à l’ensemble des adresses IP sur le serveur  
  
1.  Ouvrez le Gestionnaire d’Internet Information Services (IIS) sur le serveur.  
  
2.  Dans le volet **Connexions** du Gestionnaire IIS, étendez votre serveur, étendez **Sites**, cliquez avec le bouton droit sur **Site Web par défaut**, puis cliquez sur **Modifier les liaisons**.  
  
3.  Dans les **Liaisons de site**, cliquez sur **Ajouter**, puis sélectionnez les options suivantes :  
  
    -   **Type** = **https**  
  
    -   **Adresse IP** = **toutes non attribuées**  
  
    -   **Port** = 443  
  
4.  Sélectionnez un certificat SSL, puis cliquez sur **OK** pour enregistrer vos modifications.  
  
### <a name="no-binding-for-ssl-on-the-default-web-site"></a>Aucune liaison pour SSL sur le site Web par défaut  
 **Problème :**  Il n’existe aucune liaison pour SSL sur le site Web par défaut.  
  
 **Impact :**  Si le SSL n’est pas lié au site web par défaut, les utilisateurs pourraient ne pas avoir accès à certains sites web.  
  
 **Résolution :**  
  
##### <a name="to-bind-ssl-to-the-default-website"></a>Pour lier SSL au site web par défaut  
  
1.  Ouvrez le Gestionnaire d’Internet Information Services (IIS) sur le serveur.  
  
2.  Dans le volet **Connexions** du Gestionnaire IIS, étendez votre serveur, étendez **Sites**, cliquez avec le bouton droit sur **Site Web par défaut**, puis cliquez sur **Modifier les liaisons**.  
  
3.  Dans les **Liaisons de site**, cliquez sur **Ajouter**, puis sélectionnez les options suivantes :  
  
    -   **Type** = **https**  
  
    -   **Adresse IP** = **toutes non attribuées**  
  
    -   **Port** = 443  
  
    > [!NOTE]
    >  Si une liaison HTTPS pour le port 443 existe pour une adresse IP spécifique, définissez l’attribut d’**Adresse IP** pour cette liaison sur **Tous les non affectés**. L’adresse IP 127.0.0.1 constitue une exception. Ne modifiez pas la liaison pour 127.0.0.1.  
  
4.  Sélectionnez un certificat SSL, puis cliquez sur **OK** pour enregistrer vos modifications.  
  
### <a name="a-certificate-expires-within-30-days"></a>Un certificat expire dans 30 jours  
 **Problème :**  Votre certificat de serveur expirera dans 30 jours.  
  
 **Impact :**  Le serveur ne peut pas utiliser un certificat expiré. Si le certificat expire, il se peut que les utilisateurs ne puissent pas utiliser les fonctionnalités d’Accès en tout lieu.  
  
 **Résolution :**  Afin d’empêcher l’expiration du certificat, renouvelez-le à l’aide de votre autorité de certification.  
  
### <a name="certificate-subject-does-not-match-the-name-configured-by-domain-name-wizard"></a>L’objet du certificat ne correspond pas au nom configuré par l’Assistant Nom du domaine  
 **Problème :**  L’objet du certificat ne correspond pas au nom configuré par l’Assistant Nom du domaine.  
  
 **Impact :**  Si l’objet du certificat ne correspond pas au nom configuré par l’assistant Nom du domaine, certains sites Web ne pourront pas être initialisés. Autres sites afficheront l’erreur « Il est un problème avec le certificat de sécurité de ce site Web. »  
  
 **Résolution :**  Pour résoudre ce problème :, réexécutez l’Assistant d’accès en tout lieu de configurer et indiquez le nom de domaine correct pour le certificat ou achetez un nouveau certificat qui correspond au nom de domaine que vous souhaitez utiliser.  
  
### <a name="one-or-more-user-accounts-have-duplicate-cn-names"></a>Un ou plusieurs comptes utilisateur ont des noms CN dupliqués  
 **Problème :**  Un ou plusieurs comptes d’utilisateur ont des noms CN dupliqués : {0}.  
  
 **Impact :**  Si des comptes utilisateur ont des noms CN dupliqués, les utilisateurs ne pourront peut-être pas se connecter au réseau. De plus, les recherches Active Directory d’utilisateurs peuvent renvoyer des valeurs incorrectes.  
  
 **Résolution :**  Pour résoudre ce problème :, vérifiez que les comptes d’utilisateur réseau n’ont pas de doublon « CN = « noms. Pour vous faciliter la tâche, exportez les contenus d’Active Directory au format fichier texte pour les passer en revue. Pour savoir comment procéder, consultez [à l’aide de LDIFDE pour importer et exporter des objets d’annuaire vers Active Directory (article 237677 de la Base de connaissances)](https://support.microsoft.com/kb/237677) (https://support.microsoft.com/kb/237677).  
  
### <a name="nt-backup-is-installed"></a>NT Backup est installé  
 **Problème :**  Le programme Windows NT Backup est installé sur le serveur.  
  
 **Impact :**   Windows Server Essentials utilise la sauvegarde de Windows Server. Si le programme NT Backup est déjà installé, des conflits peuvent survenir entre les deux programmes de sauvegarde. Cela peut entraîner l’échec du processus de Sauvegarde de Windows Server. Les conflits peuvent également vous empêcher d’utiliser une sauvegarde pour restaurer le serveur.  
  
 **Résolution :** Pour résoudre ce problème :, désinstallez le programme NT Backup à partir du serveur.  
  
### <a name="iis-does-not-own-port-80-000080-or-port-443-0000443"></a>Les ports 80 (0.0.0.0:80) et 443 (0.0.0.0:443) n’appartiennent pas à IIS.  
 **Problème :**  Les ports 80 (0.0.0.0:80) et 443 n’appartiennent pas à Internet Information Services (IIS). Ces ports sont actuellement affectés à d’autres applications.  
  
 **Impact :**   Applications web de Windows Server Essentials nécessitent l’utilisation des ports 80 et 443 pour rendre les services disponibles aux utilisateurs. Si un autre processus ou une application utilise déjà le port 80 ou 443, Impossible d’exécuter les applications web de Windows Server Essentials. Dans ce cas, l’accès Web à distance et d’autres applications ne seront pas accessibles aux utilisateurs.  
  
 **Résolution :**  Pour résoudre ce problème :, soit désinstaller l’application qui est déjà à l’aide du port 80 ou 443, ou affecter cette application à un autre port.  
  
### <a name="the-default-website-is-not-running"></a>Le site Web par défaut ne fonctionne pas  
 **Problème :**  Le site Web par défaut n’est pas en cours d’exécution dans votre environnement Windows Server Essentials.  
  
 **Impact :**   Applications web de Windows Server Essentials nécessitent l’utilisation du site Web par défaut. Si le site Web par défaut ne fonctionne pas, l’accès Web à distance et d’autres applications ne seront pas accessibles aux utilisateurs.  
  
 **Résolution :**  
  
##### <a name="to-start-the-default-website"></a>Pour démarrer le site web par défaut  
  
1.  Ouvrez le Gestionnaire d’Internet Information Services (IIS) sur le serveur.  
  
2.  Dans le Gestionnaire des services Internet (IIS), développez le nom de votre serveur, puis cliquez sur **Sites**.  
  
3.  Cliquez avec le bouton droit sur **Site Web par défaut**, sélectionnez **Gérer le site web**, puis cliquez sur **Démarrer**.  
  
### <a name="read-and-script-permissions-for-the-remote-virtual-directory-are-incorrect"></a>Les autorisations Lecture et Script pour le /Répertoire virtuel distant sont incorrectes  
 **Problème :**  Les autorisations Lecture et Script ne sont pas attribuées au /Répertoire virtuel distant.  
  
 **Impact :**  Si les autorisations Lecture et Script pour le /Répertoire virtuel distant sont incorrectes, les utilisateurs ne pourront pas utiliser l’accès Web à distance. Lorsque vous essayez d’utiliser l’accès Web à distance pour naviguer sur Internet, ils peuvent rencontrer l’erreur « HTTP Error 403.1 interdit. »  
  
 **Résolution :**  
  
##### <a name="to-assign-read-and-script-permissions-to-the-remote-directory"></a>Pour attribuer des autorisations Lecture et Script au /Répertoire virtuel distant  
  
1.  Ouvrez le Gestionnaire d’Internet Information Services (IIS) sur le serveur.  
  
2.  Dans le Gestionnaire des services Internet (IIS), développez le nom de votre serveur, puis cliquez sur **Sites**.  
  
3.  Développez **Site Web par défaut**, puis **Distant**.  
  
4.  Dans **l’affichage Fonctionnalités**, double-cliquez sur **Mappages de gestionnaires**.  
  
5.  Dans le volet **Actions**, cliquez sur **Modifier les autorisations de fonctions**.  
  
6.  Cochez les cases **Lecture** et **Script**, puis cliquez sur **OK**.  
  
### <a name="http-redirect-is-either-set-or-inherited-on-the-remote-virtual-directory"></a>La redirection HTTP est définie ou héritée sur le /Répertoire virtuel distant  
 **Problème :**  L’attribut de redirection HTTP est défini ou hérité de façon inattendue sur le /Répertoire virtuel distant.  
  
 **Impact :**  Si l’attribut de redirection HTTP est défini sur le /Répertoire virtuel distant, Remote Web Workplace ne fonctionnera pas correctement.  
  
 **Résolution :**  
  
##### <a name="to-remove-the-http-redirect-attribute"></a>Pour supprimer l’attribut Redirection HTTP  
  
1.  Ouvrez le Gestionnaire d’Internet Information Services (IIS) sur le serveur.  
  
2.  Dans le Gestionnaire des services Internet (IIS), développez le nom de votre serveur, puis cliquez sur **Sites**.  
  
3.  Développez **Site Web par défaut**, puis **Distant**.  
  
4.  Dans **l’affichage Fonctionnalités**, double-cliquez sur **Redirection HTTP**.  
  
5.  Décochez la case **Rediriger les demandes vers cette destination**, puis cliquez sur **Appliquer** dans le volet **Actions**.  
  
### <a name="a-host-name-exists-for-port-80-on-the-default-website"></a>Un nom d’hôte existe pour le port 80 du site Web par défaut  
 **Problème :**  Un nom d’hôte est affecté au port 80 du site Web par défaut.  
  
 **Impact :**  Si un nom d’hôte est affecté au port 80 sur le site Web par défaut, vous n’êtes peut-être pas en mesure de se connecter à certaines applications web de Windows Server Essentials. Dans ce cas, il n’est pas obligatoire ni recommandé d’avoir un nom d’hôte.  
  
 **Résolution :**  
  
##### <a name="to-clear-the-host-name-entry-for-port-80-on-the-default-website"></a>Pour effacer l’entrée de nom d’hôte du port 80 sur le site web par défaut  
  
1.  Ouvrez le Gestionnaire d’Internet Information Services (IIS) sur le serveur.  
  
2.  Dans le Gestionnaire des services Internet (IIS), développez le nom de votre serveur, puis cliquez sur **Sites**.  
  
3.  Dans l’**affichage Fonctionnalités**, cliquez avec le bouton droit sur **Site Web par défaut**, puis cliquez sur **Liaisons**.  
  
4.  Dans **Liaisons de site**, sélectionnez le paramètre **HTTP pour le port 80**, puis cliquez sur **Modifier**.  
  
5.  Dans **Modifier la liaison de site**, supprimez l’entrée du **Nom d’hôte**, puis cliquez sur **OK**.  
  
### <a name="backup-does-not-succeed-because-of-a-hidden-partition"></a>Échec de la Sauvegarde en raison d’une partition cachée  
 **Problème :**  La sauvegarde par la Sauvegarde Windows Server d’une partition non-NTFS est planifiée.  
  
 **Impact :**  La Sauvegarde Windows Server peut uniquement sauvegarder les partitions au format NTFS.  
  
 **Résolution :**  Ne configurez pas la Sauvegarde Windows Server pour sauvegarder des partitions autres que NTFS. Pour plus d’informations, consultez [ID d’événements 12290 et 16387 enregistrés en cas d’échec de sauvegarde de l’état système sur un ordinateur Windows Server 2008 (article 968128 de la Base de connaissances)](https://support.microsoft.com/kb/968128) (https://support.microsoft.com/kb/968128).  
  
### <a name="the-most-recent-backup-did-not-succeed"></a>La dernière sauvegarde a échoué  
 **Problème :**  La dernière tentative de sauvegarde a échoué.  
  
 **Impact :**  L’état de la sauvegarde pour le système est incorrect.  
  
 **Résolution :**  Examinez les journaux des événements et les journaux de sauvegarde pour identifier les erreurs survenues durant la dernière sauvegarde.  
  
### <a name="the-startup-type-for-the-file-replication-service-is-not-set-to-automatic"></a>Le type de démarrage pour le service de réplication de fichiers n’est pas défini sur Automatique  
 **Problème :**  Il se peut que le service de réplication de fichiers ne démarre pas si le type de démarrage n’est pas défini sur la valeur Automatique par défaut.  
  
 **Impact :**  Si le service de réplication de fichiers ne fonctionne pas, le contrôleur de domaine pourra cesser de publier ses services. Cela peut entraîner d’autres problèmes, tels que des erreurs d’ouverture de session ou des erreurs de stratégie de groupe.  
  
 **Résolution :**  
  
##### <a name="to-configure-the-file-replication-service-for-automatic-startup"></a>Pour configurer le service de réplication de fichiers en vue d’un démarrage automatique  
  
1.  Ouvrez la Console des services.  
  
2.  Dans la liste des services, double-cliquez sur **Réplication de fichiers**.  
  
3.  Pour **Type de démarrage**, sélectionnez **Automatique**, puis cliquez sur **Appliquer**.  
  
### <a name="the-file-replication-service-is-not-running"></a>Le service de réplication de fichiers ne fonctionne pas  
 **Problème :**  Le service de réplication de fichiers ne fonctionne pas.  
  
 **Impact :**  Si le service de réplication de fichiers ne fonctionne pas, le contrôleur de domaine pourra cesser de publier ses services. Cela peut entraîner d’autres problèmes, tels que des erreurs d’ouverture de session ou des erreurs de stratégie de groupe.  
  
 **Résolution :**  
  
##### <a name="to-start-the-file-replication-service"></a>Pour démarrer le service de réplication de fichiers  
  
1.  Ouvrez la Console des services.  
  
2.  Dans la liste des services, double-cliquez sur **Service de réplication de fichiers**.  
  
3.  Cliquez sur **Démarrer**.  
  
### <a name="the-logon-account-for-the-file-replication-service-is-not-set-to-use-the-local-system-account"></a>Le compte d’ouverture de session pour le service de Réplication de fichiers n’est pas configuré pour utiliser le Compte système local  
 **Problème :**  Le Service de réplication de fichiers n’est pas configuré pour utiliser le Compte système local comme compte d’ouverture de session par défaut.  
  
 **Impact :**  Si le Service de réplication de fichiers n’utilise pas le compte Système local comme compte d’ouverture de session par défaut, des erreurs d’autorisation pourraient se produire. Des erreurs peuvent déclencher d’autres erreurs, qui pourrait conduire le contrôleur de domaine à cesser de publier ses services.  
  
 **Résolution :**  
  
##### <a name="to-configure-local-system-as-the-default-logon-account-for-file-replication"></a>Pour configurer le compte Système local comme compte d’ouverture de session par défaut pour la réplication de fichiers  
  
1.  Ouvrez la Console des services.  
  
2.  Dans la liste des services, double-cliquez sur **Réplication de fichiers**.  
  
3.  Dans la page **Propriétés du service**, cliquez sur l’onglet **Ouvrir une session**.  
  
4.  Sélectionnez l’option **Compte Système local**, puis cliquez sur **Appliquer**.  
  
5.  Redémarrez le service.  
  
### <a name="the-startup-type-for-the-dfs-replication-service-is-not-set-to-automatic"></a>Le type de démarrage pour le service de réplication DFS n’est pas défini sur Automatique  
 **Problème :**  Il se peut que le service de réplication DFS ne démarre pas si le type de démarrage n’est pas défini sur la valeur par défaut Automatique.  
  
 **Impact :**  Si le service de réplication DFS ne fonctionne pas, le contrôleur de domaine pourra cesser de publier ses services. Cela peut entraîner d’autres problèmes, tels que des erreurs d’ouverture de session ou des erreurs de stratégie de groupe.  
  
 **Résolution :**  
  
##### <a name="to-configure-the-dfs-replication-service-for-automatic-startup"></a>Pour configurer le service de réplication DFS en vue d’un démarrage automatique  
  
1.  Ouvrez la Console des services.  
  
2.  Dans la liste des services, double-cliquez sur **Réplication DFS**.  
  
3.  Pour **Type de démarrage**, sélectionnez **Automatique**, puis cliquez sur **Appliquer**.  
  
### <a name="the-dfs-replication-service-is-not-running"></a>Le service de réplication DFS ne fonctionne pas  
 **Problème :**  Le service de réplication DFS ne fonctionne pas actuellement.  
  
 **Impact :**  Si le service de réplication DFS ne fonctionne pas, le contrôleur de domaine pourra cesser de publier ses services. Cela peut entraîner d’autres problèmes, tels que des erreurs d’ouverture de session ou des erreurs de stratégie de groupe.  
  
 **Résolution :**  
  
##### <a name="to-start-the-dfs-replication-service"></a>Pour démarrer le service de réplication DFS  
  
1.  Ouvrez la Console des services.  
  
2.  Dans la liste des services, double-cliquez sur **Réplication DFS**.  
  
3.  Cliquez sur **Démarrer**.  
  
### <a name="the-dfs-replication-service-is-not-is-not-set-to-use-the-local-system-account"></a>Le service de réplication DFS n’est pas configuré pour utiliser le Compte système local  
 **Problème :**  Le service de Réplication DFS n’est pas configuré pour utiliser le Compte système local comme compte d’ouverture de session par défaut.  
  
 **Impact :**  Si le Service de réplication DFS n’utilise pas le compte Système local comme compte d’ouverture de session par défaut, des erreurs d’autorisation pourraient se produire. Des erreurs peuvent déclencher d’autres erreurs, qui pourrait conduire le contrôleur de domaine à cesser de publier ses services.  
  
 **Résolution :**  
  
##### <a name="to-configure-dfs-replication-to-use-local-system-as-the-default-logon-account"></a>Pour configurer le compte Système local comme compte d’ouverture de session par défaut pour la réplication DFS  
  
1.  Ouvrez la Console des services.  
  
2.  Dans la liste des services, double-cliquez sur **Réplication DFS**.  
  
3.  Dans la page **Propriétés du service**, cliquez sur l’onglet **Ouvrir une session**.  
  
4.  Sélectionnez l’option **Compte Système local**, puis cliquez sur **Appliquer**.  
  
5.  Redémarrez le service.  
  
### <a name="the-windows-server-office-365-integration-service-is-not-set-to-use-the-local-system-account"></a>Le Service d’intégration d’Office 365 pour Windows Server n’est pas configuré pour utiliser le Compte système local  
 **Problème :**  Le Service d’intégration d’Office 365 pour Windows Server n’est pas configuré pour utiliser le Compte système local comme compte d’ouverture de session par défaut.  
  
 **Impact :**  Si le Service d’intégration d’Office 365 pour Windows Server n’est pas configuré pour utiliser le Compte système local comme compte d’ouverture de session par défaut, certaines fonctions d’Office 365 pourraient ne pas fonctionner correctement. De même, des erreurs d’autorisation pourraient se produire.  
  
 **Résolution :**  
  
##### <a name="to-configure-the-office-365-integration-service-to-use-local-system-as-the-default-logon-account"></a>Pour configurer le service Intégration d’Office 365 pour qu’il utilise le compte Système local comme compte d’ouverture de session par défaut  
  
1.  Ouvrez la Console des services.  
  
2.  Dans la liste des services, double-cliquez sur **Service Intégration d’Office 365 pour Windows Server**.  
  
3.  Dans la page **Propriétés du service**, cliquez sur l’onglet **Ouvrir une session**.  
  
4.  Sélectionnez l’option **Compte Système local**, puis cliquez sur **Appliquer**.  
  
5.  Redémarrez le service.  
  
### <a name="the-windows-server-office-365-integration-service-is-not-running"></a>Le Service d’intégration d’Office 365 pour Windows Server ne fonctionne pas  
 **Problème :**  Le Service d’intégration d’Office 365 pour Windows Server ne fonctionne pas actuellement.  
  
 **Impact :**  Si le Service d’intégration d’Office 365 pour Windows Server ne fonctionne pas, les fonctions d’Office 365 en nuage ne seront pas disponibles.  
  
 **Résolution :**  
  
##### <a name="to-start-the-windows-server-office-365-integration-service"></a>Pour démarrer le service d’intégration d’Office 365 pour Windows Server  
  
1.  Ouvrez la Console des services.  
  
2.  Dans la liste des services, double-cliquez sur **Service Intégration d’Office 365 pour Windows Server**.  
  
3.  Cliquez sur **Démarrer**.  
  
### <a name="the-startup-type-for-the-windows-server-office-365-integration-service-is-not-set-to-automatic"></a>Le type de démarrage pour le service d’intégration d’Office 365 pour Windows Server n’est pas défini sur Automatique  
 **Problème :**  Il se peut que le Service d’intégration d’Office 365 pour Windows Server ne démarre pas si le type de démarrage n’est pas défini sur la valeur par défaut Automatique.  
  
 **Impact :**  Si le Service d’intégration d’Office 365 pour Windows Server ne fonctionne pas, les fonctions d’Office 365 en nuage ne seront pas disponibles.  
  
 **Résolution :**  
  
##### <a name="to-configure-the-office-365-integration-service-for-automatic-startup"></a>Pour configurer le démarrage automatique du service d’intégration d’Office 365  
  
1.  Ouvrez la Console des services.  
  
2.  Dans la liste des services, double-cliquez sur **Service Intégration d’Office 365 pour Windows Server**.  
  
3.  Pour **Type de démarrage**, sélectionnez **Automatique**, puis cliquez sur **Appliquer**.  
  
### <a name="a-registry-value-is-missing-or-set-incorrectly"></a>Une valeur de registre est manquante ou mal définie  
 **Problème :**  Une clé de registre sous HKEY_LOCAL_MACHINE \Software\Microsoft\Rpc\RpcProxy contient des valeurs incorrectes ou n’existe pas.  
  
 **Impact :**  Si la clé de registre RPCProxy est mal définie, vous pourrez recevoir un message d’erreur de type : « Votre ordinateur ne peut pas se connecter à un ordinateur distant en raison d’une indisponibilité provisoire du serveur de passerelle Bureau à distance. Essayez de vous reconnecter plus tard ou contactez votre administrateur réseau pour obtenir de l’aide. »  
  
 **Résolution :**  
  
##### <a name="to-correct-the-registry-setting"></a>Pour corriger le paramètre dans le registre :  
  
1.  Ouvrez l’Éditeur du Registre.  
  
2.  Accédez à la clé de Registre suivante :  
  
     HKEY_LOCAL_MACHINE\Software\Microsoft\Rpc\RpcProxy  
  
3.  Assurez-vous que la chaîne nommée « Site Web » a une valeur de données de Site Web par défaut :  
  
    -   Si la valeur de données est incorrecte, modifiez la chaîne afin d’utiliser la bonne valeur.  
  
    -   Si la chaîne n’existe pas, créez une nouvelle chaîne appelée « Site Web » et définissez la valeur de données sur Site Web par défaut. »  
  
### <a name="the-startup-type-for-the-block-level-backup-engine-service-is-not-set-to-manual"></a>Le type de démarrage pour le Service de moteur de sauvegarde en mode bloc n’est pas défini sur Manuel  
 **Problème :**  Le Service de moteur de sauvegarde en mode bloc n’utilise pas le type de démarrage par défaut Manuel.  
  
 **Impact :**  Le Service de moteur de sauvegarde en mode bloc pourrait ne pas démarrer si le type de démarrage n’est pas défini sur Manuel. Ce problème : peut entraîner l’échec des tâches de sauvegarde de Windows Server.  
  
 **Résolution :**  
  
##### <a name="to-configure-the-block-level-backup-engine-service-for-manual-startup"></a>Pour configurer le Service de moteur de sauvegarde en mode bloc pour le démarrage manuel  
  
1.  Ouvrez la Console des services.  
  
2.  Dans la liste des services, double-cliquez sur **Service de moteur de sauvegarde en mode bloc**.  
  
3.  Pour **Type de démarrage**, sélectionnez **Manuel**, puis cliquez sur **Appliquer**.  
  
### <a name="the-logon-account-for-the-block-level-backup-engine-service-is-not-set-to-use-the-local-system-account"></a>Le compte d’ouverture de session pour le Service de moteur de sauvegarde en mode bloc n’est pas configuré pour utiliser le Compte système local  
 **Problème :**  Le Service de moteur de sauvegarde en mode bloc n’est pas configuré pour utiliser le Compte système local comme compte d’ouverture de session par défaut.  
  
 **Impact :**  Si le Service de moteur de sauvegarde en mode bloc n’est pas configuré pour utiliser le Compte système local comme compte d’ouverture de session par défaut, des erreurs d’autorisation pourraient se produire. Ces erreurs pourraient empêcher la bonne exécution des tâches de Sauvegarde de Windows Server.  
  
 **Résolution :**  
  
##### <a name="to-configure-the-block-level-backup-engine-service-to-use-local-system-as-the-default-logon-account"></a>Pour configurer le Service de moteur de sauvegarde en mode bloc pour qu’il utilise le compte Système local comme compte d’ouverture de session par défaut  
  
1.  Ouvrez la Console des services.  
  
2.  Dans la liste des services, double-cliquez sur **Service de moteur de sauvegarde en mode bloc**.  
  
3.  Dans la page **Propriétés du service**, cliquez sur l’onglet **Ouvrir une session**.  
  
4.  Sélectionnez l’option **Compte Système local**, puis cliquez sur **Appliquer**.  
  
5.  Redémarrez le service.  
  
### <a name="the-common-name-on-the-certificate-that-is-bound-to-the-wss-certificate-web-service-website-does-not-match-the-server-name"></a>Le nom commun sur le certificat qui est lié au site Web du Service Web de certificat WSS ne correspond pas au nom du serveur  
 **Problème :**  Un certificat non valide est lié au site Web du Service Web de certificat WSS dans IIS. Le nom commun sur ce certificat ne correspond pas au nom du serveur.  
  
 **Impact :**  Si vous liez un certificat non valide au site Web du Service Web de certificat WSS, l’assistant de connexion risque de ne pas fonctionner correctement.  
  
 **Résolution :**  
  
##### <a name="to-configure-a-valid-certificate-for-the-wss-certificate-web-service"></a>Pour configurer un certificat valide pour le Service Web de certificat WSS  
  
1.  Ouvrez le Gestionnaire d’Internet Information Services (IIS) sur le serveur.  
  
2.  Dans le Gestionnaire des services Internet (IIS), développez le nom de votre serveur, puis cliquez sur **Sites**.  
  
3.  Cliquez avec le bouton droit sur **Service Web de certificat WSS**, puis cliquez sur **Modifier les liaisons**.  
  
4.  Dans **Liaisons du site**, cliquez sur **HTTPS**, puis sur **Modifier**.  
  
5.  Dans **Modifier les liaisons du site**, pour le **certificat SSL**, sélectionnez le certificat portant le même nom que votre serveur.  
  
6.  Si plus d’une entrée de certificat est identique au nom de votre serveur, cliquez sur **Afficher** pour déterminer quel certificat est valide, puis sélectionnez le certificat qui convient.  
  
### <a name="there-appears-to-be-a-problem-with-the-certificate-binding-for-the-remote-desktop-gateway-service"></a>La liaison de certificat pour le service Passerelle des services Bureau à distance semble poser problème  
 **Problème :**  Le certificat pour le service Passerelle des services Bureau à distance semble être mal lié.  
  
 **Impact :**  Si le certificat pour le service Passerelle des services Bureau à distance n’est pas configuré correctement, les utilisateurs ne pourront pas se connecter à l’Accès Web à distance.  
  
 **Résolution :**  
  
##### <a name="to-fix-the-binding-for-the-remote-desktop-gateway-service"></a>Pour corriger la liaison du service Passerelle des services Bureau à distance  
  
-   Ouvrez une invite de commandes en tant qu’administrateur et tapez les commandes suivantes :  
  
    ```  
    REG ADD HKLM\SYSTEM\CurrentControlSet\services\HTTP\Parameters\SslBindingInfo\0.0.0.0:443  /v DefaultFlags /t REG_DWORD /d 1 /f  
    net stop tsgateway  
    net start tsgateway  
    ```  
  
     Pour plus d’informations, consultez [comment gérer le Service de passerelle Bureau à distance dans Windows Server Essentials (article 2472211 de la Base de connaissances)](https://support.microsoft.com/kb/2472211) (https://support.microsoft.com/kb/2472211).
