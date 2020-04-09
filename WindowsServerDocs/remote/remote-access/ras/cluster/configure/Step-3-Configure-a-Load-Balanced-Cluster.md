---
title: 'Étape 3 : configurer un cluster à charge équilibrée'
description: Cette rubrique fait partie du guide déployer l’accès à distance dans un cluster dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: f000066e-7cf8-4085-82a3-4f4fe1cb3c5c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 335697f012bf4fef8c448de896ec89ad647cb68b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855282"
---
# <a name="step-3-configure-a-load-balanced-cluster"></a>Étape 3 : configurer un cluster à charge équilibrée

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Après avoir préparé les serveurs du cluster, configurez l’équilibrage de charge sur le serveur unique, configurez les certificats requis et déployez le cluster.  
  
|Tâche|Description|  
|----|--------|  
|[3,1 configurer le préfixe IPv6](#BKMK_Prefix)|Si l’environnement d’entreprise est IPv4 + IPv6 ou IPv6 uniquement, sur le serveur d’accès à distance unique, assurez-vous que le préfixe IPv6 attribué aux ordinateurs clients DirectAccess est suffisamment grand pour couvrir tous les serveurs de votre cluster.|  
|[3,2 activer l’équilibrage de charge](#BKMK_NLB)|Activez l’équilibrage de charge sur le serveur d’accès à distance unique.|  
|[3,3 installer le certificat IP-HTTPs](#BKMK_InstallIPHTTP)|Chaque serveur du cluster requiert un certificat de serveur pour authentifier la connexion IP-HTTPs.  Exportez le certificat IP-HTTPs à partir du serveur d’accès à distance unique et déployez-le sur chaque serveur que vous ajouterez au cluster. Cela est requis uniquement si vous utilisez des certificats non auto-signés.|  
|[3,4 Installation du certificat du serveur emplacement réseau](#BKMK_NLS)|Si le serveur d’emplacement réseau est déployé localement sur le serveur unique, vous devez déployer le certificat de serveur emplacement réseau sur chaque serveur du cluster. Si le serveur emplacement réseau est hébergé sur un serveur externe, un certificat n’est pas requis sur chaque serveur. Cela est requis uniquement si vous utilisez des certificats non auto-signés.|  
|[3,5 ajouter des serveurs au cluster](#BKMK_Add)|Ajoutez tous les serveurs au cluster. L’accès à distance ne doit pas être configuré sur les serveurs à ajouter.|  
|[3,6 suppression d’un serveur du cluster](#BKMK_remove)|Instructions pour la suppression d’un serveur du cluster.|  
|[3,7 désactiver l’équilibrage de charge](#BKBK_disable)|Instructions de désactivation de l’équilibrage de charge.|  
  
> [!NOTE]  
> L’adresse IP qui est sélectionnée pour le DIP ne doit pas être utilisée sur les cartes réseau du premier serveur d’accès à distance du cluster. Le démarrage du déploiement de DirectAccess avec l’adresse IP virtuelle et l’adresse DIP ajoutée à la carte réseau entraîne une défaillance.  
  
> [!NOTE]  
> Veillez à ne pas utiliser un DIP qui est déjà présent sur un autre ordinateur sur le réseau.  
  
## <a name="31-configure-the-ipv6-prefix"></a><a name="BKMK_Prefix"></a>3,1 configurer le préfixe IPv6  
  
### <a name="to-configure-the-prefix"></a><a name="configDA"></a>Pour configurer le préfixe  
  
1.  Sur le serveur d’accès à distance, cliquez sur **Démarrer**, puis sur **gestion de l’accès à distance**. Si la boîte de dialogue **Contrôle de compte d'utilisateur** apparaît, confirmez que l'action affichée est bien celle que vous souhaitez effectuer, puis cliquez sur **Oui**.  
  
2.  Dans la Console de gestion de l'accès à distance, cliquez sur **Configuration**.  
  
3.  Dans le volet central de la console, dans la zone **étape 2 : serveur DirectAccess** , cliquez sur **modifier**.  
  
4.  Cliquez sur **configuration du préfixe**. Dans la page **configuration du préfixe** , dans **préfixe IPv6 attribué aux ordinateurs clients DirectAccess**, entrez le préfixe IPv6 utilisé pour les ordinateurs clients DirectAccess avec une longueur de sous-réseau de 59, par exemple, **2001 : DB8:1 : 1000 ::/59**. Si le VPN était également activé avec IPv6, un préfixe IPv6 est affiché et la longueur du sous-réseau doit être remplacée par 59. Cliquez sur **Suivant**.  
  
5.  Dans le volet central de la console, cliquez sur **Terminer**.  
  
6.  Dans la boîte de dialogue vérification de l' **accès à distance** , vérifiez les paramètres de configuration, puis cliquez sur **appliquer**. Dans la boîte de dialogue **Application des paramètres de l’Assistant Configuration de l’accès à distance**, cliquez sur **Fermer**.  
  
## <a name="32-enable-load-balancing"></a><a name="BKMK_NLB"></a>3,2 activer l’équilibrage de charge  
  
#### <a name="to-enable-load-balancing"></a>Pour activer l’équilibrage de charge  
  
1.  Sur le serveur DirectAccess configuré, cliquez sur **Démarrer**, puis sur **gestion de l’accès à distance**. Si la boîte de dialogue **Contrôle de compte d'utilisateur** apparaît, confirmez que l'action affichée est bien celle que vous souhaitez effectuer, puis cliquez sur **Oui**.  
  
2.  Dans la console Gestion de l’accès à distance, dans le volet gauche, cliquez sur **configuration**, puis dans le volet **tâches** , cliquez sur **activer l’équilibrage de charge**.  
  
3.  Dans l’Assistant Activation de l’équilibrage de charge, cliquez sur **suivant**.  
  
4.  En fonction de ce que vous avez choisi dans les étapes de planification :  
  
    1.  Windows NLB : sur la page **méthode d’équilibrage de charge** , cliquez sur utiliser l’équilibrage de **charge réseau Windows (NLB)** , puis cliquez sur **suivant**.  
  
    2.  Équilibrage de charge externe : sur la page **méthode d’équilibrage de charge** , cliquez sur **utiliser un équilibreur de charge externe**, puis cliquez sur **suivant**.  
  
5.  Dans un déploiement à une seule carte réseau, dans la page **adresses IP dédiées** , effectuez les opérations suivantes, puis cliquez sur **suivant**:  
  
    1.  Dans la zone **adresse IPv4** , entrez la nouvelle adresse IPv4 pour ce serveur d’accès à distance ; l’adresse IPv4 actuelle sera l’adresse IP virtuelle (VIP) du cluster à charge équilibrée. Dans la zone **masque de sous-réseau** , entrez le masque de sous-réseau.  
  
    2.  Si l’environnement d’entreprise est natif IPv6, dans la zone **adresse IPv6** , entrez la nouvelle adresse IPv6 pour ce serveur d’accès à distance ; l’adresse IPv6 actuelle sera l’adresse IP virtuelle du cluster à charge équilibrée. Dans la zone **longueur du préfixe du sous-réseau** , entrez la longueur du préfixe du sous-réseau.  
  
6.  Dans un déploiement à deux cartes réseau, dans la page **adresses IP dédiées externes** , effectuez les opérations suivantes, puis cliquez sur **suivant**:  
  
    1.  Dans la zone **adresse IPv4** , entrez la nouvelle adresse IPv4 externe pour ce serveur d’accès à distance ; l’adresse IPv4 actuelle sera l’adresse IP virtuelle (VIP) du cluster d’équilibrage de charge. Dans la zone **masque de sous-réseau** , entrez le masque de sous-réseau.  
  
    2.  Si des adresses IPv6 natives sont actuellement configurées sur la carte réseau Internet du serveur d’accès à distance, dans la zone **adresse IPv6** , entrez la nouvelle adresse IPv6 externe pour ce serveur d’accès à distance ; l’adresse IPv6 actuelle sera l’adresse IP virtuelle du cluster d’équilibrage de charge. Dans la zone **longueur du préfixe du sous-réseau** , entrez la longueur du préfixe du sous-réseau.  
  
7.  Dans un déploiement à deux cartes réseau, dans la page **adresses IP dédiées internes** , effectuez les opérations suivantes, puis cliquez sur **suivant**:  
  
    1.  Dans la zone **adresse IPv4** , entrez la nouvelle adresse IPv4 interne pour ce serveur d’accès à distance ; l’adresse IPv4 actuelle sera l’adresse IP virtuelle du cluster d’équilibrage de charge. Dans la zone **masque de sous-réseau** , entrez le masque de sous-réseau.  
  
    2.  Si l’environnement d’entreprise est IPv6 natif, dans la zone **adresse IPv6** , entrez la nouvelle adresse IPv6 interne pour ce serveur d’accès à distance ; l’adresse IPv6 actuelle sera l’adresse IP virtuelle du cluster d’équilibrage de charge. Dans la zone **longueur du préfixe du sous-réseau** , entrez la longueur du préfixe du sous-réseau.  
  
8.  Sur la page **Résumé** , cliquez sur **valider**.  
  
9. Dans la boîte de dialogue **activer l’équilibrage de charge** , cliquez sur **Fermer**.  
  
10. Dans l’Assistant Activation de l’équilibrage de charge, cliquez sur **Fermer**.  
  
    > [!NOTE]  
    > Si l’équilibrage de charge externe est utilisé, notez les adresses IP virtuelles et fournissez-les comme sur les équilibrages de charge externes.  
  
![les commandes Windows PowerShell](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>équivalentes</em> Windows PowerShell***  
  
La ou les applets de commande Windows PowerShell suivantes ont la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles apparaissent ici sur plusieurs lignes en raison de contraintes de mise en forme.  
  
Si vous avez choisi d’utiliser l’équilibrage de charge réseau Windows dans les étapes de planification, exécutez la commande suivante :  
  
```  
Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress "2.1.1.20/255.255.255.0" -InternalDedicatedIPAddress @("10.1.1.30/255.255.255.0","3ffe::20/64") -InternetVirtualIPAddress @("2.1.1.1/255.255.255.0","2.1.1.2/255.255.255.0") -InternalVirtualIPAddress @("10.1.1.2/255.255.255.0","3ffe::2/64")  
```  
  
Si vous avez choisi d’utiliser un équilibreur de charge externe dans les étapes de planification, exécutez la commande suivante :  
  
```  
Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress "2.1.1.20/255.255.255.0" -InternalDedicatedIPAddress @("10.1.1.30/255.255.255.0","3ffe::20/64") -UseThirdPrtyLoadBalancer  
```  
  
> [!NOTE]  
> Il est recommandé de ne pas inclure les modifications apportées aux paramètres de l’équilibreur de charge avec les modifications apportées à d’autres paramètres, si vous utilisez des objets de stratégie de groupe intermédiaires. Toutes les modifications apportées aux paramètres de l’équilibreur de charge doivent être appliquées en premier, puis d’autres modifications de configuration doivent être apportées. De même, après la configuration de l’équilibreur de charge sur un nouveau serveur DirectAccess, patientez quelques instants pour que les modifications d’adresse IP soient appliquées et répliquées sur les serveurs DNS de l’entreprise, avant de modifier d’autres paramètres DirectAccess liés au nouveau cluster.  
  
## <a name="33-install-the-ip-https-certificate"></a><a name="BKMK_InstallIPHTTP"></a>3,3 installer le certificat IP-HTTPs  
L'appartenance au groupe **Administrateurs** local, ou à un groupe équivalent, est la condition requise minimale pour effectuer cette procédure.  
  
### <a name="to-install-the-ip-https-certificate"></a><a name="IPHTTPSCert"></a>Pour installer le certificat IP-HTTPs  
  
1.  Sur le serveur d’accès à distance configuré, cliquez sur **Démarrer**, tapez **MMC** , puis appuyez sur entrée. Si la boîte de dialogue **Contrôle de compte d'utilisateur** apparaît, confirmez que l'action affichée est bien celle que vous souhaitez effectuer, puis cliquez sur **Oui**.  
  
2.  Dans la console MMC, dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**.  
  
3.  Dans la boîte de dialogue **Ajouter ou supprimer des composants logiciels enfichables** , cliquez sur **certificats**, sur **Ajouter**, sur **compte d’ordinateur**, sur **suivant**, sur **Terminer**, puis sur **OK**.  
  
4.  Dans le volet gauche de la console, accédez à **certificats (ordinateur local) \Personal\Certificates**. Cliquez avec le bouton droit sur le certificat IP-HTTPs, pointez sur **toutes les tâches** , puis cliquez sur **Exporter**.  
  
5.  Dans la page **Bienvenue dans l'Assistant Exportation du certificat**, cliquez sur **Suivant**.  
  
6.  Dans la page **Exportation de la clé privée**, cliquez sur **Oui, exporter la clé privée**, puis sur **Suivant**.  
  
7.  Sur la page **format de fichier d’exportation** , cliquez sur échange d' **informations personnelles-PKCS #12 (. PFX)** , puis cliquez sur **suivant**.  
  
8.  Dans la page **sécurité** , activez la case à cocher **mot** de passe, entrez un mot de passe dans la zone **mot de passe** et confirmez le mot de passe, puis cliquez sur **suivant**.  
  
9. Sur la page **fichier à exporter** , entrez un nom pour le fichier de certificat, puis enregistrez-le sur le bureau, puis cliquez sur **suivant**.  
  
10. Dans la page **Fin de l'Assistant Exportation de certificat**, cliquez sur **Terminer**.  
  
11. Dans la boîte de dialogue **Assistant exportation de certificat** , cliquez sur **OK**.  
  
12. Copiez le certificat sur tous les serveurs qui doivent être membres du cluster.  
  
13. Sur le nouveau serveur DirectAccess, cliquez sur **Démarrer**, tapez **MMC** , puis appuyez sur entrée. Si la boîte de dialogue **Contrôle de compte d'utilisateur** apparaît, confirmez que l'action affichée est bien celle que vous souhaitez effectuer, puis cliquez sur **Oui**.  
  
14. Dans la console MMC, dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**.  
  
15. Dans la boîte de dialogue **Ajouter ou supprimer des composants logiciels enfichables** , cliquez sur **certificats**, sur **Ajouter**, sur **compte d’ordinateur**, sur **suivant**, sur **Terminer**, puis sur **OK**.  
  
16. Dans le volet gauche de la console, accédez à **certificats (ordinateur local) \Personal\Certificates**. Cliquez avec le bouton droit sur le nœud **certificats** , pointez sur **toutes les tâches**, puis cliquez sur **Importer**.  
  
17. Dans la page **Bienvenue** de l'Assistant Importation de certificat, cliquez sur **Suivant**.  
  
18. Sur la page **fichier à importer** , cliquez sur **Parcourir** pour rechercher le certificat. Sélectionnez le certificat, puis cliquez sur **suivant**.  
  
19. Sur la page **protection de clé privée** , dans la zone **mot de passe** , tapez le mot de passe, puis cliquez sur **suivant**.  
  
20. Dans la page **Magasin de certificats**, cliquez sur **Suivant**.  
  
21. Dans la page **Fin de l’Assistant Importation du certificat**, cliquez sur **Terminer**.  
  
22. Dans la boîte de dialogue **Assistant importation de certificat** , cliquez sur **OK**.  
  
23. Répétez les étapes 13-22 sur tous les serveurs qui doivent être membres du cluster.  
  
## <a name="34-install-the-network-location-server-certificate"></a><a name="BKMK_NLS"></a>3,4 Installation du certificat du serveur emplacement réseau  
L'appartenance au groupe **Administrateurs** local, ou à un groupe équivalent, est la condition requise minimale pour effectuer cette procédure.  
  
#### <a name="to-install-a-certificate-for-network-location"></a>Pour installer un certificat pour l’emplacement réseau  
  
1.  Sur le serveur d’accès à distance, cliquez sur **Démarrer**, tapez **MMC**, puis appuyez sur entrée. Si la boîte de dialogue **Contrôle de compte d'utilisateur** apparaît, confirmez que l'action affichée est bien celle que vous souhaitez effectuer, puis cliquez sur **Oui**.  
  
2.  Cliquez sur **Fichier**, puis sur **Ajouter ou supprimer des composants logiciels enfichables**.  
  
3.  Cliquez sur **certificats**, sur **Ajouter**, sur **compte d’ordinateur**, sur **suivant**, sur **ordinateur local**, sur **Terminer**, puis sur **OK**.  
  
4.  Dans l'arborescence de la console du composant logiciel enfichable Certificats, ouvrez **Certificats (ordinateur local)\Personnel\Certificats**.  
  
5.  Cliquez avec le bouton droit sur **Certificats**, pointez sur **Toutes les Tâches**, puis cliquez sur **Demander un nouveau certificat**.  
  
6.  Cliquez deux fois sur **Suivant**.  
  
7.  Sur la page **demander des certificats** , cliquez sur le modèle de certificat de serveur Web, puis cliquez sur l' **inscription pour obtenir ce certificat nécessite des informations supplémentaires**.  
  
    Si le modèle de certificat de serveur Web n’apparaît pas, assurez-vous que le compte d’ordinateur du serveur d’accès à distance dispose des autorisations d’inscription pour le modèle de certificat de serveur Web. Pour plus d’informations, consultez [configurer des autorisations sur le modèle de certificat de serveur Web](https://msdn.microsoft.com/library/ee649249.aspx).  
  
8.  Sous l’onglet **objet** de la boîte de dialogue **Propriétés du certificat** , dans nom de l' **objet**, pour **type**, sélectionnez **nom commun**.  
  
9. Dans **valeur**, tapez le nom de domaine complet (FQDN) pour le nom intranet du site Web du serveur emplacement réseau (par exemple, nls.Corp.contoso.com), puis cliquez sur **Ajouter**.  
  
10. Cliquez sur **OK**, sur **Inscrire**, puis sur **Terminer**.  
  
11. Dans le volet d'informations du composant logiciel enfichable Certificats, vérifiez qu'un nouveau certificat avec le nom de domaine complet a été inscrit avec **Rôles prévus** égal à **Authentification du serveur**.  
  
12. Cliquez avec le bouton droit sur le certificat, puis cliquez sur **Propriétés**.  
  
13. Dans **Nom convivial**, tapez **Certificat Emplacement réseau**, puis cliquez sur **OK**.  
  
    > [!TIP]  
    > Les étapes 12 et 13 sont facultatives, mais vous permettent de sélectionner plus facilement le certificat pour l’emplacement réseau lors de la configuration de l’accès à distance.  
  
14. Répétez cette procédure sur tous les serveurs qui doivent être membres du cluster.  
  
## <a name="35-add-servers-to-the-cluster"></a><a name="BKMK_Add"></a>3,5 ajouter des serveurs au cluster  
 
  
#### <a name="to-add-servers-to-the-cluster"></a>Pour ajouter des serveurs au cluster  
  
1.  Sur le serveur DirectAccess configuré, cliquez sur **Démarrer**, puis sur **gestion de l’accès à distance**. Si la boîte de dialogue **Contrôle de compte d'utilisateur** apparaît, confirmez que l'action affichée est bien celle que vous souhaitez effectuer, puis cliquez sur **Oui**.  
  
2.  Dans la Console de gestion de l'accès à distance, cliquez sur **Configuration**. Dans le volet **tâches** , sous **cluster à charge équilibrée**, cliquez sur **Ajouter ou supprimer des serveurs**.  
  
3.  Dans la boîte de dialogue **Ajouter ou supprimer des serveurs** , cliquez sur **Ajouter un serveur**.  
  
4.  Dans la boîte de dialogue **Ajouter un serveur** , dans la page **Sélectionner un serveur** , entrez le nom du serveur d’accès à distance supplémentaire, puis cliquez sur **suivant**.  
  
5.  Dans la page **cartes réseau** , effectuez l’une des opérations suivantes :  
  
    -   Si vous déployez une topologie avec deux cartes réseau, dans **carte externe**, sélectionnez la carte qui est connectée au réseau externe. Dans **adaptateur interne**, sélectionnez la carte qui est connectée au réseau interne.  
  
    -   Si vous déployez une topologie avec une carte réseau, dans **carte réseau**, sélectionnez la carte qui est connectée au réseau interne.  
  
6.  Dans la page **cartes réseau** , dans **Sélectionner le certificat utilisé pour authentifier les connexions IP-HTTPS**, cliquez sur **Parcourir** pour rechercher et sélectionner le certificat IP-HTTPS, puis cliquez sur **suivant**.  
  
7.  Sur la page **serveur emplacement réseau** , cliquez sur **Parcourir** pour sélectionner le certificat pour le site Web du serveur emplacement réseau en cours d’exécution sur le serveur d’accès à distance, puis cliquez sur **suivant**.  
  
    > [!NOTE]  
    > La page **serveur emplacement réseau** s’affiche uniquement lorsque le site Web du serveur emplacement réseau est en cours d’exécution sur le serveur d’accès à distance.  
  
    > [!NOTE]  
    > Si une connexion VPN était également configurée sur le serveur d’accès à distance, vous devrez ajouter les informations du pool d’adresses IP VPN à ce stade.  
  
8.  Sur la page **Résumé** , cliquez sur **Ajouter**.  
  
9. Sur la page **Fin**, cliquez sur **Fermer**.  
  
10. Répétez cette procédure pour tous les serveurs d’accès à distance à ajouter au cluster.  
  
11. Dans la boîte de dialogue **Ajouter ou supprimer des serveurs** , cliquez sur **valider**.  
  
12. Dans la boîte de dialogue **Ajout et suppression de serveurs** , cliquez sur **Fermer**.  
  
![les commandes Windows PowerShell](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>équivalentes</em> Windows PowerShell***  
  
La ou les applets de commande Windows PowerShell suivantes ont la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles apparaissent ici sur plusieurs lignes en raison de contraintes de mise en forme.  
  
```  
Add-RemoteAccessLoadBalancerNode -RemoteAccessServer <server name>  
```  
  
> [!NOTE]  
> Si le VPN n’a pas été activé dans un cluster à charge équilibrée, vous ne devez pas fournir de plages d’adresses VPN lors de l’ajout d’un nouveau serveur au cluster à l’aide des applets de commande Windows PowerShell. Si vous avez effectué cette opération par erreur, supprimez le serveur du cluster, puis ajoutez-le de nouveau au cluster sans spécifier les plages d’adresses VPN.  
  
## <a name="36-remove-a-server-from-the-cluster"></a><a name="BKMK_remove"></a>3,6 suppression d’un serveur du cluster  
 
  
#### <a name="to-remove-a-server-from-the-cluster"></a>Pour supprimer un serveur du cluster  
  
1.  Sur le serveur d’accès à distance configuré, cliquez sur **Démarrer**, puis sur **gestion de l’accès à distance**. Si la boîte de dialogue **Contrôle de compte d'utilisateur** apparaît, confirmez que l'action affichée est bien celle que vous souhaitez effectuer, puis cliquez sur **Oui**.  
  
2.  Dans la Console de gestion de l'accès à distance, cliquez sur **Configuration**. Dans le volet **tâches** , sous **cluster à charge équilibrée**, cliquez sur **Ajouter ou supprimer des serveurs**.  
  
3.  Dans la boîte de dialogue **Ajouter ou supprimer des serveurs** , sélectionnez le serveur d’accès à distance que vous souhaitez supprimer, puis cliquez sur **supprimer le serveur**.  
  
4.  Dans la boîte de dialogue **Supprimer l’avertissement du serveur** , assurez-vous que vous avez choisi le serveur approprié, puis cliquez sur **OK**.  
  
5.  Répétez cette procédure pour tous les serveurs d’accès à distance à supprimer du cluster.  
  
6.  Dans la boîte de dialogue **Ajouter ou supprimer des serveurs** , cliquez sur **valider**.  
  
7.  Dans la boîte de dialogue **Ajout et suppression de serveurs** , cliquez sur **Fermer**.  
  
![les commandes Windows PowerShell](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>équivalentes</em> Windows PowerShell***  
  
La ou les applets de commande Windows PowerShell suivantes ont la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles apparaissent ici sur plusieurs lignes en raison de contraintes de mise en forme.  
  
```  
Remove-RemoteAccessLoadBalancerNode -RemoteAccessServer <server name>  
```  
  
## <a name="37-disable-load-balancing"></a><a name="BKBK_disable"></a>3,7 désactiver l’équilibrage de charge  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///7a817ca0-2b4a-4476-9d28-9a63ff2453f9)  
  
#### <a name="to-disable-load-balancing"></a>Pour désactiver l’équilibrage de charge  
  
1.  Sur le serveur DirectAccess configuré, cliquez sur **Démarrer**, puis sur **gestion de l’accès à distance**. Si la boîte de dialogue **Contrôle de compte d'utilisateur** apparaît, confirmez que l'action affichée est bien celle que vous souhaitez effectuer, puis cliquez sur **Oui**.  
  
2.  Dans la Console de gestion de l'accès à distance, cliquez sur **Configuration**. Dans le volet **tâches** , sous **cluster à charge équilibrée**, cliquez sur **désactiver l’équilibrage de charge**.  
  
3.  Dans la boîte de dialogue **désactiver l’équilibrage de charge** , cliquez sur **OK**.  
  
4.  Dans la boîte de dialogue **désactiver l’équilibrage de charge** , cliquez sur **Fermer**.  
  
![les commandes Windows PowerShell](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>équivalentes</em> Windows PowerShell***  
  
La ou les applets de commande Windows PowerShell suivantes ont la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles apparaissent ici sur plusieurs lignes en raison de contraintes de mise en forme.  
  
```  
set-RemoteAccessLoadBalancer -disable  
```  
  
La désactivation de l’équilibrage de charge supprime les paramètres d’accès à distance et les paramètres NLB (s’ils sont configurés) de tous les serveurs, sauf le serveur à partir duquel il est exécuté. Sur ce serveur d’accès à distance, les paramètres NLB sont supprimés (s’ils ont été configurés), mais les paramètres d’accès à distance sont conservés.  
  
Cliquer sur **Supprimer les paramètres de configuration** supprime l’accès à distance et l’équilibrage de la charge réseau (s’ils sont configurés) de tous les serveurs du déploiement.  
  
> [!NOTE]  
> -   Si l’accès à distance est désinstallé lorsque l’équilibrage de charge est déployé, tous les serveurs sont laissés avec des adresses DIP. Les adresses IP virtuelles sont supprimées. Cela entraîne l’échec de tous les itinéraires du réseau d’entreprise ciblant les adresses IP virtuelles. Cela affecte également les entrées DNS qui ont été résolues en adresses IP virtuelles, telles que le nom d’objet du certificat du serveur emplacement réseau. Pour éviter ce problème, désactivez l’équilibrage de charge, qui laisse les adresses IP virtuelles sur le dernier serveur d’accès à distance, puis désinstallez l’accès à distance.  
> -   Après avoir utilisé l’applet de commande **Set-RemoteAccessLoadBalancer** pour désactiver l’équilibrage de charge, patientez 2 minutes avant d’exécuter une autre applet de commande. Cela doit également se faire dans tous les scripts qui exécutent une autre applet de commande après l’applet de commande **Set-RemoteAccessLoadBalancer-Disable** .  
> -   La désactivation de l’équilibrage de charge modifie l’adresse IP virtuelle du cluster en une adresse IP dédiée. En conséquence, toute opération qui demande le nom du serveur échouera jusqu’à ce que l’entrée DNS mise en cache sur le serveur expire. Assurez-vous que vous n’exécutez pas d’applets de commande PowerShell d’accès à distance après avoir désactivé l’équilibrage de charge jusqu’à ce que le cache sur le serveur ait expiré. Ce problème est plus courant si vous essayez de désactiver l’équilibrage de charge sur un ordinateur à partir d’un autre ordinateur qui se trouve dans un autre domaine. Cela se produit également si vous désactivez l’équilibrage de charge à partir de la console de gestion de l’accès à distance et risquez d’empêcher le chargement de la configuration. La configuration se chargera une fois que le cache a expiré ou a été vidé.  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Voir aussi  
  
-   [Étape 4 : vérification du cluster](Step-4-Verify-the-Cluster.md)  
  


