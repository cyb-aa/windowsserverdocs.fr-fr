---
title: ÉTAPE 12-tester la connectivité DirectAccess
description: 'Cette rubrique fait partie du Guide de laboratoire de test : illustrer un déploiement multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 65ac1c23-3a47-4e58-888d-9dde7fba1586
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 85aff4ae9e117359f8827abb15dce47728a30da4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857282"
---
# <a name="step-12-test-directaccess-connectivity"></a>ÉTAPE 12-tester la connectivité DirectAccess

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Avant de pouvoir tester la connectivité à partir des ordinateurs clients lorsqu’ils se trouvent sur Internet ou sur des réseaux HomeNet, vous devez vous assurer qu’ils disposent des paramètres de stratégie de groupe appropriés.  
  
- Pour vérifier que les clients possèdent la stratégie de groupe appropriée  
  
- Tester la connectivité DirectAccess à partir d’Internet via EDGE1  
  
- Déplacer CLIENT2 vers le groupe de sécurité Win7_Clients_Site2  
  
- Tester la connectivité DirectAccess à partir d’Internet jusqu’à 2 EDGE1  
  
## <a name="prerequisites"></a>Composants requis  
Connectez les deux ordinateurs clients au réseau Corpnet, puis redémarrez les deux ordinateurs clients.  
  
## <a name="verify-clients-have-the-correct-group-policy"></a><a name="policy"></a>Vérifier que les clients possèdent la stratégie de groupe appropriée  
  
1.  Sur CLIENT1, cliquez sur **Démarrer**, **tapez PowerShell. exe**, cliquez avec le bouton droit sur **PowerShell**, cliquez sur **avancé**, puis sur **exécuter en tant qu’administrateur**. Si la boîte de dialogue **Contrôle de compte d'utilisateur** apparaît, confirmez que l'action affichée est bien celle que vous souhaitez effectuer, puis cliquez sur **Oui**.  
  
2.  Dans la fenêtre Windows PowerShell, tapez **ipconfig** , puis appuyez sur entrée.  
  
    Assurez-vous que l’adresse IPv4 de l’adaptateur corpnet commence par la version10.0.0.  
  
3.  Dans la fenêtre Windows PowerShell, tapez **« obtient-DnsClientNrptPolicy »** et appuyez sur entrée. Les entrées de la table de stratégie de résolution de noms (NRPT) pour DirectAccess sont affichées.  
  
    -   . corp.contoso.com-ces paramètres indiquent que toutes les connexions à corp.contoso.com doivent être résolues par l’un des serveurs DNS DirectAccess, avec l’adresse IPv6 2001 : DB8:1 :: 2 ou 2001 : DB8:2 :: 20.  
  
    -   nls.corp.contoso.com : ces paramètres indiquent qu’il existe une exemption pour le nom nls.corp.contoso.com.  
  
4.  Laissez la fenêtre Windows PowerShell ouverte pour la procédure suivante.  
  
5.  Sur CLIENT2, cliquez sur **Démarrer**, sur **tous les programmes**, sur **accessoires**, sur **Windows PowerShell**, cliquez avec le bouton droit sur **Windows PowerShell**, puis cliquez sur **exécuter en tant qu’administrateur**. Si la boîte de dialogue **Contrôle de compte d'utilisateur** apparaît, confirmez que l'action affichée est bien celle que vous souhaitez effectuer, puis cliquez sur **Oui**.  
  
6.  Dans la fenêtre Windows PowerShell, tapez **ipconfig** , puis appuyez sur entrée.  
  
    Assurez-vous que l’adresse IPv4 de l’adaptateur corpnet commence par la version10.0.0.  
  
7.  Dans la fenêtre Windows PowerShell, tapez **netsh namespace show Policy** et appuyez sur entrée.  
  
    Dans la sortie, il doit y avoir deux sections :  
  
    -   . corp.contoso.com-ces paramètres indiquent que toutes les connexions à corp.contoso.com doivent être résolues par le serveur DNS DirectAccess, avec l’adresse IPv6 2001 : DB8:1 :: 2.  
  
    -   nls.corp.contoso.com : ces paramètres indiquent qu’il existe une exemption pour le nom nls.corp.contoso.com.  
  
8.  Laissez la fenêtre Windows PowerShell ouverte pour la procédure suivante.  
  
## <a name="test-directaccess-connectivity-from-the-internet-through-edge1"></a><a name="EDGE1"></a>Tester la connectivité DirectAccess à partir d’Internet via EDGE1  
  
1. Débranchez 2-EDGE1 à partir du réseau Internet.  
  
2. Débranchez CLIENT1 et CLIENT2 du commutateur corpnet et connectez-les au commutateur Internet. Attendez 30 secondes.  
  
3. Sur CLIENT1, dans la fenêtre Windows PowerShell, tapez **ipconfig/all** et appuyez sur entrée.  
  
4. Examinez la sortie de la commande ipconfig.  
  
   L’ordinateur client est maintenant connecté à Internet et possède une adresse IPv4 publique. Lorsque le client DirectAccess possède une adresse IPv4 publique, il utilise les technologies de transition Teredo ou IP-HTTPs IPv6 pour transférer les messages IPv6 sur un réseau Internet IPv4 entre le client DirectAccess et le serveur d’accès à distance. Notez que Teredo est la technologie de transition préférée.  
  
5. Dans la fenêtre Windows PowerShell, tapez **ipconfig/flushdns** et appuyez sur entrée. Cette opération vide les entrées de résolution de noms qui peuvent encore exister dans le cache DNS du client à partir de lorsque l’ordinateur client a été connecté au Corpnet.  
  
6. Désactivez l’interface Teredo pour vous assurer que l’ordinateur client utilise IP-HTTPs pour se connecter à corpnet à l’aide de la commande suivante :  
  
   ```  
   netsh interface teredo set state disable  
   ```  
  
7. Vérifiez que vous êtes connecté via EDGE1. Tapez **netsh interface httpstunnel show interfaces** et appuyez sur entrée.  
  
   La sortie doit contenir URL : https://edge1.contoso.com:443/IPHTTPS.  
  
   > [!TIP]  
   > Sur CLIENT1, vous pouvez également exécuter la commande Windows PowerShell suivante : **NetIPHTTPSConfiguration**. La sortie affiche les connexions d’URL du serveur disponibles et le profil actuellement actif.  
  
8. Dans la fenêtre Windows PowerShell, tapez « **ping App1** » et appuyez sur entrée. Vous devez voir les réponses de l’adresse IPv6 assignée à APP1, qui, dans ce cas, est 2001 : DB8:1 :: 3.  
  
9. Dans la fenêtre Windows PowerShell, tapez **ping 2-App1** , puis appuyez sur entrée. Vous devez voir les réponses de l’adresse IPv6 assignée à 2-APP1, qui, dans ce cas, est 2001 : DB8:2 :: 3.  
  
10. Dans la fenêtre Windows PowerShell, tapez **ping App2** et appuyez sur entrée. Vous devez voir les réponses de l’adresse NAT64 assignée par EDGE1 à APP2, qui dans ce cas est FD**C9:9f4e : eb1b**: porte :: A00:4. Notez que les valeurs en gras varient en fonction de la façon dont l’adresse est générée.  
  
    La possibilité d’exécuter une commande ping APP2 est importante, car la réussite indique que vous avez pu établir une connexion à l’aide de NAT64/DNS64, car APP2 est une ressource IPv4 uniquement.  
  
11. Ouvrez Internet Explorer, dans la barre d’adresses d’Internet Explorer, entrez **https://app1/** , puis appuyez sur entrée. Vous allez voir le site web IIS par défaut sur APP1.  
  
12. Dans la barre d’adresses d’Internet Explorer, entrez **https://2-app1/** , puis appuyez sur entrée. Vous verrez le site Web par défaut sur 2-APP1.  
  
13. Dans la barre d’adresses d’Internet Explorer, entrez **https://app2/** , puis appuyez sur entrée. Vous allez voir le site web par défaut sur APP2.  
  
14. Dans l’écran d' **Accueil** , tapez<strong>\\\2-App1\Files</strong>, puis appuyez sur entrée. Double-cliquez sur l’exemple de fichier texte.  
  
    Cela démontre que vous avez pu vous connecter au serveur de fichiers dans le domaine corp2.corp.contoso.com lorsqu’il est connecté via EDGE1.  
  
15. Dans l’écran d' **Accueil** , tapez<strong>\\\App2\Files</strong>, puis appuyez sur entrée. Double-cliquez sur le fichier Nouveau document texte.  
  
    Cela démontre que vous avez pu vous connecter à un serveur IPv4 uniquement à l’aide de SMB pour obtenir une ressource dans le domaine de ressources.  
  
16. Dans l’écran d' **Accueil** , tapez**WF. msc**, puis appuyez sur entrée.  
  
17. Dans la console **pare-feu Windows avec fonctions avancées de sécurité** , Notez que seul le **profil public** est actif. Le pare-feu Windows doit être activé pour que DirectAccess fonctionne correctement. Si le pare-feu Windows est désactivé, la connectivité DirectAccess ne fonctionne pas.  
  
18. Dans le volet gauche de la console, développez le nœud **analyse** , puis cliquez sur le nœud **règles de sécurité de connexion** . Vous devez voir les règles de sécurité de connexion actives : **Stratégie DirectAccess-ClientToCorp**, **Stratégie DirectAccess-ClientToDNS64NAT64PrefixExemption**, **Stratégie DirectAccess-ClientToInfra**et **Stratégie DirectAccess-ClientToNlaExempt**. Faites défiler le volet central vers la droite pour afficher les **premières méthodes d’authentification** et les colonnes de la **2e méthode d’authentification** . Notez que la première règle (ClientToCorp) utilise Kerberos V5 pour établir le tunnel intranet et que la troisième règle (ClientToInfra) utilise NTLMv2 pour établir le tunnel d’infrastructure.  
  
19. Dans le volet gauche de la console, développez le nœud **associations de sécurité** , puis cliquez sur le nœud **mode principal** . Notez les associations de sécurité de tunnel d’infrastructure utilisant NTLMv2 et l’Association de sécurité de tunnel intranet à l’aide de Kerberos V5. Cliquez avec le bouton droit sur l’entrée qui affiche **utilisateur (Kerberos V5)** comme **deuxième méthode d’authentification** , puis cliquez sur **Propriétés**. Sous l’onglet **général** , Notez que le **deuxième ID local d’authentification** est **corp\user1.** , ce qui indique que User1 a pu s’authentifier auprès du domaine Corp à l’aide de Kerberos.  
  
20. Répétez cette procédure à partir de l’étape 3 sur CLIENT2.  
  
## <a name="move-client2-to-the-win7_clients_site2-security-group"></a><a name="secgroup"></a>Déplacer CLIENT2 vers le groupe de sécurité Win7_Clients_Site2  
  
1.  Sur DC1, cliquez sur **Démarrer**, tapez **DSA. msc**, puis appuyez sur entrée.  
  
2.  Dans la console utilisateurs et ordinateurs Active Directory, ouvrez **Corp.contoso.com/users** et double-cliquez sur **Win7_Clients_Site1**.  
  
3.  Dans la boîte de dialogue Propriétés de la **Win7_Clients_Site1** , cliquez sur l’onglet **membres** , sur **client2**, sur **supprimer**, sur **Oui**, puis sur **OK**.  
  
4.  Double-cliquez sur **Win7_Clients_Site2**, puis dans la boîte de dialogue **Propriétés de Win7_Clients_Site2** , cliquez sur l’onglet **membres** .  
  
5.  Cliquez sur **Ajouter**, puis dans la boîte de dialogue **Sélectionner les utilisateurs, les contacts, les ordinateurs ou les comptes de service** , cliquez sur types d' **objets**, sélectionnez **ordinateurs**, puis cliquez sur **OK**.  
  
6.  Dans **Entrez les noms des objets à sélectionner**, tapez **client2**, puis cliquez sur **OK**.  
  
7.  Redémarrez CLIENT2 et connectez-vous à l’aide du compte Corp/utilisateur1.  
  
8.  Sur CLIENT2, ouvrez une fenêtre Windows PowerShell avec élévation de privilèges, tapez **netsh namespace show Policy** et appuyez sur entrée.  
  
    Dans la sortie, il doit y avoir deux sections :  
  
    -   . corp.contoso.com-ces paramètres indiquent que toutes les connexions à corp.contoso.com doivent être résolues par le serveur DNS DirectAccess, avec l’adresse IPv6 2001 : DB8:2 :: 20.  
  
    -   nls.corp.contoso.com : ces paramètres indiquent qu’il existe une exemption pour le nom nls.corp.contoso.com.  
  
## <a name="test-directaccess-connectivity-from-the-internet-through-2-edge1"></a><a name="DAConnect"></a>Tester la connectivité DirectAccess à partir d’Internet jusqu’à 2 EDGE1  
  
1. Connectez-vous 2-EDGE1 au réseau Internet.  
  
2. Débranchez EDGE1 à partir du réseau Internet.  
  
3. Sur CLIENT1, ouvrez une fenêtre Windows PowerShell avec élévation de privilèges.  
  
4. Dans la fenêtre Windows PowerShell, tapez **ipconfig/flushdns** et appuyez sur entrée. Cette opération vide les entrées de résolution de noms qui peuvent encore exister dans le cache DNS du client à partir de lorsque l’ordinateur client a été connecté au Corpnet.  
  
5. Assurez-vous que vous êtes connecté à 2 EDGE1. Tapez **netsh interface httpstunnel show interfaces** et appuyez sur entrée.  
  
   La sortie doit contenir URL : https://2-edge1.contoso.com:443/IPHTTPS.  
  
   > [!TIP]  
   > Sur CLIENT1, vous pouvez également exécuter la commande suivante : **NetIPHTTPSConfiguration**. La sortie affiche les connexions d’URL du serveur disponibles et le profil actuellement actif.  
  
   > [!NOTE]  
   > CLIENT1 change automatiquement le serveur via lequel il se connecte aux ressources de l’entreprise. Si la sortie de la commande indique une connexion à EDGE1, attendez environ cinq minutes, puis réessayez.  
  
6. Dans la fenêtre Windows PowerShell, tapez « **ping App1** » et appuyez sur entrée. Vous devez voir les réponses de l’adresse IPv6 assignée à APP1, qui, dans ce cas, est 2001 : DB8:1 :: 3.  
  
7. Dans la fenêtre Windows PowerShell, tapez **ping 2-App1** , puis appuyez sur entrée. Vous devez voir les réponses de l’adresse IPv6 assignée à 2-APP1, qui, dans ce cas, est 2001 : DB8:2 :: 3.  
  
8. Dans la fenêtre Windows PowerShell, tapez **ping App2** et appuyez sur entrée. Vous devez voir les réponses de l’adresse NAT64 assignée par EDGE1 à APP2, qui dans ce cas est FD**C9:9f4e : eb1b**: porte :: A00:4. Notez que les valeurs en gras varient en fonction de la façon dont l’adresse est générée.  
  
   La possibilité d’exécuter une commande ping APP2 est importante, car la réussite indique que vous avez pu établir une connexion à l’aide de NAT64/DNS64, car APP2 est une ressource IPv4 uniquement.  
  
9. Ouvrez Internet Explorer, dans la barre d’adresses d’Internet Explorer, entrez **https://app1/** , puis appuyez sur entrée. Vous allez voir le site web IIS par défaut sur APP1.  
  
10. Dans la barre d’adresses d’Internet Explorer, entrez **https://2-app1/** , puis appuyez sur entrée. Vous allez voir le site web par défaut sur APP2.  
  
11. Dans la barre d’adresses d’Internet Explorer, entrez **https://app2/** , puis appuyez sur entrée. Vous verrez le site Web par défaut sur APP3.  
  
12. Dans l’écran d' **Accueil** , tapez<strong>\\\App1\Files</strong>, puis appuyez sur entrée. Double-cliquez sur l’exemple de fichier texte.  
  
    Cela démontre que vous avez pu vous connecter au serveur de fichiers dans le domaine corp.contoso.com lors de la connexion à 2-EDGE1.  
  
13. Dans l’écran d' **Accueil** , tapez<strong>\\\App2\Files</strong>, puis appuyez sur entrée. Double-cliquez sur le fichier Nouveau document texte.  
  
    Cela démontre que vous avez pu vous connecter à un serveur IPv4 uniquement à l’aide de SMB pour obtenir une ressource dans le domaine de ressources.  
  
14. Répétez cette procédure sur CLIENT2 à l’étape 3.  
  


