---
title: ÉTAPE 12 Test la connectivité DirectAccess
description: 'Cette rubrique fait partie du Guide de laboratoire de Test : illustrer un déploiement Multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 65ac1c23-3a47-4e58-888d-9dde7fba1586
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9c87f1823140fd6c92cf7df1f9d807545b50504e
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281536"
---
# <a name="step-12-test-directaccess-connectivity"></a>ÉTAPE 12 Test la connectivité DirectAccess

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Avant de tester la connectivité entre les ordinateurs clients lorsqu’ils se trouvent sur les réseaux Internet ou réseau domestique, il se peut que vous devez vous assurer qu’ils ont les paramètres de stratégie de groupe approprié.  
  
- Pour vérifier que les clients ont la stratégie de groupe correcte  
  
- Tester la connectivité DirectAccess à partir d’Internet via EDGE1  
  
- Déplacer le CLIENT2 au groupe de sécurité Win7_Clients_Site2  
  
- Tester la connectivité DirectAccess à partir d’Internet via EDGE1-2  
  
## <a name="prerequisites"></a>Prérequis  
Connecter les deux ordinateurs clients au réseau du réseau d’entreprise et redémarrez les deux ordinateurs clients.  
  
## <a name="policy"></a>Vérifiez que les clients ont la stratégie de groupe approprié  
  
1.  Sur CLIENT1, cliquez sur **Démarrer**, type **powershell.exe**, avec le bouton droit **powershell**, cliquez sur **avancé**, puis cliquez sur **Exécuter en tant qu’administrateur**. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
2.  Dans la fenêtre Windows PowerShell, tapez **ipconfig** et appuyez sur ENTRÉE.  
  
    Assurez-vous que l’adresse IPv4 de carte réseau d’entreprise commence par 10.0.0.  
  
3.  Dans la fenêtre Windows PowerShell, tapez **Get-DnsClientNrptPolicy** et appuyez sur ENTRÉE. Les entrées de la table de stratégie de résolution de noms (NRPT) pour DirectAccess sont affichées.  
  
    -   . corp.contoso.com-ces paramètres indiquent que toutes les connexions corp.contoso.com doivent être résolues par un des serveurs DNS de DirectAccess, avec le 2001:db8:1::2 d’adresse IPv6 ou 2001:db8:2::20.  
  
    -   NLS.corp.contoso.com de ces paramètres indiquent qu’il existe une exemption pour le nom nls.corp.contoso.com.  
  
4.  Laissez la fenêtre Windows PowerShell ouverte pour la procédure suivante.  
  
5.  Sur CLIENT2, cliquez sur **Démarrer**, cliquez sur **tous les programmes**, cliquez sur **Accessoires**, cliquez sur **Windows PowerShell**, avec le bouton droit **Windows PowerShell**, puis cliquez sur **exécuter en tant qu’administrateur**. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
6.  Dans la fenêtre Windows PowerShell, tapez **ipconfig** et appuyez sur ENTRÉE.  
  
    Assurez-vous que l’adresse IPv4 de carte réseau d’entreprise commence par 10.0.0.  
  
7.  Dans la fenêtre Windows PowerShell, tapez **espace de noms netsh afficher stratégie** et appuyez sur ENTRÉE.  
  
    Dans la sortie, il doit y avoir deux sections :  
  
    -   . corp.contoso.com-ces paramètres indiquent que toutes les connexions corp.contoso.com doivent être résolues par le serveur DirectAccess DNS, avec le 2001:db8:1::2 d’adresse IPv6.  
  
    -   NLS.corp.contoso.com de ces paramètres indiquent qu’il existe une exemption pour le nom nls.corp.contoso.com.  
  
8.  Laissez la fenêtre Windows PowerShell ouverte pour la procédure suivante.  
  
## <a name="EDGE1"></a>Tester la connectivité DirectAccess à partir d’Internet via EDGE1  
  
1. Débranchez 2-EDGE1 à partir du réseau Internet.  
  
2. Débranchez CLIENT1 et CLIENT2 du commutateur de réseau d’entreprise et de les connecter au commutateur Internet. Patientez 30 secondes.  
  
3. Sur CLIENT1, dans la fenêtre Windows PowerShell, tapez **ipconfig/all** et appuyez sur ENTRÉE.  
  
4. Examinez la sortie à partir de la commande ipconfig.  
  
   L’ordinateur client est maintenant connecté à Internet et a une adresse IPv4 publique. Lorsque le client DirectAccess a une adresse IPv4 publique, il utilise les technologies de transition Teredo ou IP-HTTPS IPv6 pour transférer les messages IPv6 via un Internet IPv4 entre le client DirectAccess et le serveur d’accès à distance. Notez que Teredo est la technologie de transition par défaut.  
  
5. Dans la fenêtre Windows PowerShell, tapez **ipconfig /flushdns** et appuyez sur ENTRÉE. Vide les entrées de résolution de noms susceptibles d’être restées dans le cache DNS client depuis lorsque l’ordinateur client a été connecté au réseau d’entreprise.  
  
6. Désactiver l’interface Teredo pour vous assurer que l’ordinateur client utilise IP-HTTPS pour se connecter au réseau d’entreprise avec la commande suivante :  
  
   ```  
   netsh interface teredo set state disable  
   ```  
  
7. Vérifiez que vous êtes connecté via EDGE1. Type **netsh interface httpstunnel afficher les interfaces** et appuyez sur ENTRÉE.  
  
   La sortie doit contenir l’URL : https://edge1.contoso.com:443/IPHTTPS.  
  
   > [!TIP]  
   > Sur CLIENT1, vous pouvez également exécuter la commande Windows PowerShell suivante : **Get-NetIPHTTPSConfiguration**. La sortie affiche les connexions d’URL de serveur disponibles et le profil actuellement actif.  
  
8. Dans la fenêtre Windows PowerShell, tapez **un test ping sur app1** et appuyez sur ENTRÉE. Vous devez voir des réponses à partir de l’adresse IPv6 affectée à APP1, qui est dans ce cas 2001:db8:1::3.  
  
9. Dans la fenêtre Windows PowerShell, tapez **ping 2-app1** et appuyez sur ENTRÉE. Vous devez voir des réponses à partir de l’adresse IPv6 affectée à 2-APP1, qui est dans ce cas 2001:db8:2::3.  
  
10. Dans la fenêtre Windows PowerShell, tapez **un test ping sur app2** et appuyez sur ENTRÉE. Vous devez voir des réponses à partir de l’adresse NAT64 affectée par EDGE1 à APP2, qui est dans ce cas fd**c9:9f4e:eb1b**: 7777::a00:4. Notez que les valeurs en gras peut varier en raison de la façon dont l’adresse est générée.  
  
    La possibilité d’un test ping sur APP2 est importante, car la réussite indique que vous étiez en mesure d’établir une connexion à l’aide de NAT64/DNS64, comme APP2 est une ressource uniquement IPv4.  
  
11. Ouvrez Internet Explorer, dans la barre d’adresses Internet Explorer, entrez **https://app1/** et appuyez sur ENTRÉE. Vous allez voir le site web IIS par défaut sur APP1.  
  
12. Dans la barre d’adresses Internet Explorer, entrez **https://2-app1/** et appuyez sur ENTRÉE. Vous allez voir le site Web par défaut sur 2-APP1.  
  
13. Dans la barre d’adresses Internet Explorer, entrez **https://app2/** et appuyez sur ENTRÉE. Vous allez voir le site web par défaut sur APP2.  
  
14. Sur le **Démarrer** , tapez<strong>\\\2-App1\Files</strong>, puis appuyez sur ENTRÉE. Double-cliquez sur l’exemple de fichier texte.  
  
    Cet exemple montre que vous avez pu se connecter au serveur de fichiers dans le domaine corp2.corp.contoso.com lorsque connecté via EDGE1.  
  
15. Sur le **Démarrer** , tapez<strong>\\\App2\Files</strong>, puis appuyez sur ENTRÉE. Double-cliquez sur le fichier Nouveau document texte.  
  
    Cet exemple montre que vous avez pu se connecter à un serveur IPv4 uniquement à l’aide de SMB pour obtenir une ressource dans le domaine de ressources.  
  
16. Sur le **Démarrer** , tapez**wf.msc**, puis appuyez sur ENTRÉE.  
  
17. Dans le **pare-feu Windows avec fonctions avancées de sécurité** de la console, notez que seule la **profil Public** est actif. Le pare-feu Windows doit être activé pour DirectAccess fonctionne correctement. Si le pare-feu Windows est désactivé, la connectivité DirectAccess ne fonctionne pas.  
  
18. Dans le volet gauche de la console, développez le **surveillance** nœud, puis cliquez sur le **les règles de sécurité de connexion** nœud. Vous devez voir les règles de sécurité de connexion active : **Stratégie DirectAccess-ClientToCorp**, **stratégie DirectAccess-ClientToDNS64NAT64PrefixExemption**, **stratégie DirectAccess-ClientToInfra**, et **DirectAccess Stratégie-ClientToNlaExempt**. Faites défiler le volet du milieu vers la droite pour afficher le **des méthodes d’authentification 1er** et **2nd méthodes d’authentification** colonnes. Notez que la première règle (ClientToCorp) utilise Kerberos V5 pour établir le tunnel intranet et la troisième règle (ClientToInfra) utilise NTLMv2 pour établir le tunnel d’infrastructure.  
  
19. Dans le volet gauche de la console, développez le **Associations de sécurité** nœud, puis cliquez sur le **Mode principal** nœud. Notez que les associations de sécurité de tunnel infrastructure à l’aide de NTLMv2 et l’association de sécurité du tunnel intranet à l’aide de Kerberos V5. Cliquez sur l’entrée qui affiche **utilisateur (Kerberos V5)** en tant que le **2e méthode d’authentification** et cliquez sur **propriétés**. Sur le **général** onglet, notez le **seconde authentification ID Local** est **CORP\User1**, indiquant que User1 a réussi à s’authentifier auprès du domaine CORP à l’aide Kerberos.  
  
20. Répétez cette procédure à l’étape 3 sur le CLIENT2.  
  
## <a name="secgroup"></a>Déplacer le CLIENT2 au groupe de sécurité Win7_Clients_Site2  
  
1.  Sur DC1, cliquez sur **Démarrer**, type **DSA.msc**, puis appuyez sur ENTRÉE.  
  
2.  Dans la console Active Directory Users and Computers, ouvrez **corp.contoso.com/Users** et double-cliquez sur **Win7_Clients_Site1**.  
  
3.  Sur le **Win7_Clients_Site1 propriétés** boîte de dialogue, cliquez sur le **membres** , cliquez sur **CLIENT2**, cliquez sur **supprimer**, cliquez sur **Oui**, puis cliquez sur **OK**.  
  
4.  Double-cliquez sur **Win7_Clients_Site2**, puis, dans le **Win7_Clients_Site2 propriétés** boîte de dialogue, cliquez sur le **membres** onglet.  
  
5.  Cliquez sur **ajouter**, puis, dans le **sélectionner des utilisateurs, Contacts, ordinateurs ou comptes de Service** boîte de dialogue, cliquez sur **Types d’objets**, sélectionnez **ordinateurs**, puis cliquez sur **OK**.  
  
6.  Dans **Entrez les noms des objets à sélectionner**, type **CLIENT2**, puis cliquez sur **OK**.  
  
7.  Redémarrez CLIENT2 et connectez-vous à l’aide du compte corp/utilisateur1.  
  
8.  Sur CLIENT2, ouvrez une fenêtre Windows PowerShell avec élévation de privilèges, type **espace de noms netsh afficher stratégie** et appuyez sur ENTRÉE.  
  
    Dans la sortie, il doit y avoir deux sections :  
  
    -   . corp.contoso.com-ces paramètres indiquent que toutes les connexions corp.contoso.com doivent être résolues par le serveur DirectAccess DNS, avec le 2001:db8:2::20 d’adresse IPv6.  
  
    -   NLS.corp.contoso.com de ces paramètres indiquent qu’il existe une exemption pour le nom nls.corp.contoso.com.  
  
## <a name="DAConnect"></a>Tester la connectivité DirectAccess à partir d’Internet via EDGE1-2  
  
1. Se connecter à 2-EDGE1 au réseau Internet.  
  
2. Débranchez EDGE1 à partir du réseau Internet.  
  
3. Sur CLIENT1, ouvrez une fenêtre Windows PowerShell avec élévation de privilèges.  
  
4. Dans la fenêtre Windows PowerShell, tapez **ipconfig /flushdns** et appuyez sur ENTRÉE. Vide les entrées de résolution de noms susceptibles d’être restées dans le cache DNS client depuis lorsque l’ordinateur client a été connecté au réseau d’entreprise.  
  
5. Vérifiez que vous êtes connecté via EDGE1-2. Type **netsh interface httpstunnel afficher les interfaces** et appuyez sur ENTRÉE.  
  
   La sortie doit contenir l’URL : https://2-edge1.contoso.com:443/IPHTTPS.  
  
   > [!TIP]  
   > Sur CLIENT1, vous pouvez également exécuter la commande suivante : **Get-NetIPHTTPSConfiguration**. La sortie affiche les connexions d’URL de serveur disponibles et le profil actuellement actif.  
  
   > [!NOTE]  
   > CLIENT1 modifie automatiquement le serveur par le biais duquel il se connecte aux ressources d’entreprise. Si la sortie de la commande indique une connexion à EDGE1, patientez environ cinq minutes et réessayez.  
  
6. Dans la fenêtre Windows PowerShell, tapez **un test ping sur app1** et appuyez sur ENTRÉE. Vous devez voir des réponses à partir de l’adresse IPv6 affectée à APP1, qui est dans ce cas 2001:db8:1::3.  
  
7. Dans la fenêtre Windows PowerShell, tapez **ping 2-app1** et appuyez sur ENTRÉE. Vous devez voir des réponses à partir de l’adresse IPv6 affectée à 2-APP1, qui est dans ce cas 2001:db8:2::3.  
  
8. Dans la fenêtre Windows PowerShell, tapez **un test ping sur app2** et appuyez sur ENTRÉE. Vous devez voir des réponses à partir de l’adresse NAT64 affectée par EDGE1 à APP2, qui est dans ce cas fd**c9:9f4e:eb1b**: 7777::a00:4. Notez que les valeurs en gras peut varier en raison de la façon dont l’adresse est générée.  
  
   La possibilité d’un test ping sur APP2 est importante, car la réussite indique que vous étiez en mesure d’établir une connexion à l’aide de NAT64/DNS64, comme APP2 est une ressource uniquement IPv4.  
  
9. Ouvrez Internet Explorer, dans la barre d’adresses Internet Explorer, entrez **https://app1/** et appuyez sur ENTRÉE. Vous allez voir le site web IIS par défaut sur APP1.  
  
10. Dans la barre d’adresses Internet Explorer, entrez **https://2-app1/** et appuyez sur ENTRÉE. Vous allez voir le site web par défaut sur APP2.  
  
11. Dans la barre d’adresses Internet Explorer, entrez **https://app2/** et appuyez sur ENTRÉE. Vous allez voir le site Web par défaut sur APP3.  
  
12. Sur le **Démarrer** , tapez<strong>\\\App1\Files</strong>, puis appuyez sur ENTRÉE. Double-cliquez sur l’exemple de fichier texte.  
  
    Cet exemple montre que vous avez pu se connecter au serveur de fichiers dans le domaine corp.contoso.com lorsque connecté via EDGE1-2.  
  
13. Sur le **Démarrer** , tapez<strong>\\\App2\Files</strong>, puis appuyez sur ENTRÉE. Double-cliquez sur le fichier Nouveau document texte.  
  
    Cet exemple montre que vous avez pu se connecter à un serveur IPv4 uniquement à l’aide de SMB pour obtenir une ressource dans le domaine de ressources.  
  
14. Répétez cette procédure sur le CLIENT2 à l’étape 3.  
  


