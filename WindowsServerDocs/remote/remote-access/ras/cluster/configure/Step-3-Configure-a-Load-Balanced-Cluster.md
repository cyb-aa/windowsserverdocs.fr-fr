---
title: Étape 3 configurer un Cluster d’équilibrage de charge
description: Cette rubrique fait partie du guide de déploiement des accès à distance dans un Cluster dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f000066e-7cf8-4085-82a3-4f4fe1cb3c5c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f835e27a80e661ff1f066af4779bd7c033cddc99
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446621"
---
# <a name="step-3-configure-a-load-balanced-cluster"></a>Étape 3 configurer un Cluster d’équilibrage de charge

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Après avoir préparé les serveurs pour le cluster, configurez l’équilibrage de charge sur le serveur unique, configurer les certificats requis et déployer le cluster.  
  
|Tâche|Description|  
|----|--------|  
|[3.1 configurer le préfixe IPv6](#BKMK_Prefix)|Si l’environnement d’entreprise IPv6 + IPv4 ou IPv6 uniquement, puis sur le serveur d’accès à distance unique, vérifiez que le préfixe IPv6 attribué aux ordinateurs clients DirectAccess est suffisamment grand pour couvrir tous les serveurs de votre cluster.|  
|[3.2 activer l’équilibrage de charge](#BKMK_NLB)|Activer l’équilibrage de charge sur le serveur d’accès à distance unique.|  
|[3.3 installer le certificat IP-HTTPS](#BKMK_InstallIPHTTP)|Chaque serveur du cluster nécessite un certificat de serveur pour authentifier la connexion IP-HTTPS.  Exporter le certificat IP-HTTPS à partir du serveur d’accès à distance unique et déployez-le sur chaque serveur que vous allez ajouter au cluster. Cela est nécessaire uniquement si vous utilisez des certificats non signés automatiquement.|  
|[3.4 installer le certificat de serveur d’emplacement réseau](#BKMK_NLS)|Si le serveur unique est le serveur d’emplacement réseau déployé localement, vous devez déployer le certificat de serveur d’emplacement réseau sur chaque serveur dans le cluster. Si le serveur emplacement réseau est hébergé sur un serveur externe, un certificat sur chaque serveur n’est pas requis. Cela est nécessaire uniquement si vous utilisez des certificats non signés automatiquement.|  
|[3.5 ajouter des serveurs au cluster](#BKMK_Add)|Ajoutez tous les serveurs au cluster. Accès à distance ne doit pas être configuré sur les serveurs à ajouter.|  
|[3.6 supprimer un serveur du cluster](#BKMK_remove)|Instructions pour la suppression d’un serveur à partir du cluster.|  
|[3.7 désactiver l’équilibrage de charge](#BKBK_disable)|Instructions pour désactiver l’équilibrage de charge.|  
  
> [!NOTE]  
> L’adresse IP sélectionnée pour l’adresse IP dédiée ne doit pas être en cours d’utilisation sur les cartes réseau du premier serveur d’accès à distance dans le cluster. Commencer le déploiement de DirectAccess avec VIP et DIP ajouté à la carte réseau entraîne la défaillance.  
  
> [!NOTE]  
> Évitez d’utilisent une adresse IP dédiée est déjà présente sur un autre ordinateur sur le réseau.  
  
## <a name="BKMK_Prefix"></a>3.1 configurer le préfixe IPv6  
  
### <a name="configDA"></a>Pour configurer le préfixe  
  
1.  Sur le serveur d’accès à distance, cliquez sur **Démarrer**, puis cliquez sur **gestion de l’accès à distance**. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
2.  Dans la Console de gestion de l'accès à distance, cliquez sur **Configuration**.  
  
3.  Dans le volet central de la console, dans le **étape 2 : serveur DirectAccess** zone, cliquez sur **modifier**.  
  
4.  Cliquez sur **Configuration du préfixe**. Sur le **Configuration du préfixe** page **préfixe IPv6 attribué aux ordinateurs clients DirectAccess**, entrez le préfixe IPv6 utilisé pour les ordinateurs clients DirectAccess avec une longueur de sous-réseau de 59, par exemple, **2001:db8:1:1000 :: / 59**. Si VPN ont été également activé avec IPv6, puis un préfixe IPv6 s’affiche, et la longueur du sous-réseau doit être modifiée et 59. Cliquez sur **Suivant**.  
  
5.  Dans le volet central de la console, cliquez sur **Terminer**.  
  
6.  Sur le **révision d’accès à distance** boîte de dialogue, passez en revue les paramètres de configuration, puis cliquez sur **appliquer**. Dans la boîte de dialogue **Application des paramètres de l’Assistant Configuration de l’accès à distance**, cliquez sur **Fermer**.  
  
## <a name="BKMK_NLB"></a>3.2 activer l’équilibrage de charge  
  
#### <a name="to-enable-load-balancing"></a>Pour activer l’équilibrage de charge  
  
1.  Sur le serveur DirectAccess configuré, cliquez sur **Démarrer**, puis cliquez sur **gestion de l’accès à distance**. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
2.  Dans la console de gestion de l’accès à distance, dans le volet gauche, cliquez sur **Configuration**, puis, dans le **tâches** volet, cliquez sur **activer l’équilibrage de charge**.  
  
3.  Dans l’Assistant Activation de charge équilibrage, cliquez sur **suivant**.  
  
4.  Selon ce que vous avez choisi dans les étapes de planification :  
  
    1.  Windows NLB : Sur le **méthode d’équilibrage de charge** , cliquez sur **utilisez Windows NLB Network Load Balancing ()** , puis cliquez sur **suivant**.  
  
    2.  Équilibreur de charge externe : Sur le **méthode d’équilibrage de charge** , cliquez sur **utiliser un équilibreur de charge externe**, puis cliquez sur **suivant**.  
  
5.  Dans un déploiement de carte réseau unique, sur le **des adresses IP dédiées** page, procédez comme suit, puis cliquez sur **suivant**:  
  
    1.  Dans le **adresse IPv4** zone, entrez la nouvelle adresse IPv4 pour ce serveur d’accès à distance ; l’actuel IPv4 adresse est l’adresse IP virtuelle (VIP) du cluster d’équilibrage de charge. Dans le **masque de sous-réseau** , entrez le masque de sous-réseau.  
  
    2.  Si l’environnement d’entreprise est le protocole IPv6 natif, puis dans le **adresse IPv6** zone, entrez la nouvelle adresse IPv6 pour ce serveur d’accès à distance ; l’actuel IPv6 adresse est l’adresse IP virtuelle du cluster d’équilibrage de charge. Dans le **longueur de préfixe de sous-réseau** , entrez la longueur de préfixe de sous-réseau.  
  
6.  Dans les deux déploiement de l’adaptateur, de réseau sur le **externes des adresses IP dédiées** page, procédez comme suit, puis cliquez sur **suivant**:  
  
    1.  Dans le **adresse IPv4** zone, entrez la nouvelle adresse IPv4 externe pour ce serveur d’accès à distance ; l’actuel IPv4 adresse sera l’adresse IP virtuelle (VIP) de l’équilibrage de charge du cluster. Dans le **masque de sous-réseau** , entrez le masque de sous-réseau.  
  
    2.  S’il existe des adresses IPv6 natifs actuellement configurés sur la carte réseau Internet du serveur d’accès à distance, dans le **adresse IPv6** , entrez la nouvelle adresse IPv6 externe pour l’accès à distance serveur ; l’adresse IPv6 en cours adresse sera l’adresse IP virtuelle de la cluster d’équilibrage de charge. Dans le **longueur de préfixe de sous-réseau** , entrez la longueur de préfixe de sous-réseau.  
  
7.  Dans les deux déploiement de l’adaptateur, de réseau sur le **interne des adresses IP dédiées** page, procédez comme suit, puis cliquez sur **suivant**:  
  
    1.  Dans le **adresse IPv4** zone, entrez la nouvelle adresse IPv4 interne pour ce serveur d’accès à distance ; l’actuel IPv4 adresse sera l’adresse IP virtuelle de l’équilibrage de charge du cluster. Dans le **masque de sous-réseau** , entrez le masque de sous-réseau.  
  
    2.  Si l’environnement d’entreprise est le protocole IPv6 natif, puis dans le **adresse IPv6** zone, entrez la nouvelle adresse IPv6 interne pour ce serveur d’accès à distance ; l’actuel IPv6 adresse sera l’adresse IP virtuelle de l’équilibrage de charge du cluster. Dans le **longueur de préfixe de sous-réseau** , entrez la longueur de préfixe de sous-réseau.  
  
8.  Sur le **Résumé** , cliquez sur **valider**.  
  
9. Sur le **activer l’équilibrage de charge** boîte de dialogue, cliquez sur **fermer**.  
  
10. Dans l’Assistant Activation de charge équilibrage, cliquez sur **fermer**.  
  
    > [!NOTE]  
    > Si l’équilibrage de charge externe est utilisé, notez les adresses IP virtuelles et les fournir comme sur les équilibreurs de charge externe.  
  
![Windows PowerShell](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
Si vous avez choisi d’utiliser NLB de Windows dans les étapes de planification, puis exécutez la commande suivante :  
  
```  
Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress "2.1.1.20/255.255.255.0" -InternalDedicatedIPAddress @("10.1.1.30/255.255.255.0","3ffe::20/64") -InternetVirtualIPAddress @("2.1.1.1/255.255.255.0","2.1.1.2/255.255.255.0") -InternalVirtualIPAddress @("10.1.1.2/255.255.255.0","3ffe::2/64")  
```  
  
Si vous avez choisi d’utiliser un équilibreur de charge externe dans les étapes de planification : puis exécutez la commande suivante :  
  
```  
Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress "2.1.1.20/255.255.255.0" -InternalDedicatedIPAddress @("10.1.1.30/255.255.255.0","3ffe::20/64") -UseThirdPrtyLoadBalancer  
```  
  
> [!NOTE]  
> Il est recommandé pour ne pas inclure les modifications apportées aux paramètres de l’équilibrage de charge avec les modifications apportées à tous les autres paramètres, si vous utilisez la stratégie de groupe de mise en lots. Les modifications apportées aux paramètres de l’équilibreur de charge doivent être appliquées en premier, et ensuite les autres modifications de configuration doivent être effectuées. En outre, après avoir configuré l’équilibrage de charge sur un nouveau serveur DirectAccess, attendez quelque temps pour que les modifications d’adresse IP à appliquer et répliquées sur tous les serveurs DNS de l’entreprise, avant de modifier d’autres paramètres de DirectAccess relatifs au nouveau cluster.  
  
## <a name="BKMK_InstallIPHTTP"></a>3.3 installer le certificat IP-HTTPS  
L'appartenance au groupe local **Administrateurs**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.  
  
### <a name="IPHTTPSCert"></a>Pour installer le certificat IP-HTTPS  
  
1.  Sur le serveur d’accès à distance configuré, cliquez sur **Démarrer**, type **mmc** et appuyez sur ENTRÉE. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
2.  Dans la console MMC, dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**.  
  
3.  Sur le **ajouter ou supprimer des composants logiciel enfichables** boîte de dialogue, cliquez sur **certificats**, cliquez sur **ajouter**, cliquez sur **compte d’ordinateur**, cliquez sur  **Suivant**, cliquez sur **Terminer**, puis cliquez sur **OK**.  
  
4.  Dans le volet gauche de la console, accédez à **certificats (ordinateur Local) \Personal\Certificates**. Cliquez sur le certificat IP-HTTPS, pointez sur **toutes les tâches** et cliquez sur **exporter**.  
  
5.  Dans la page **Bienvenue !** , cliquez sur **Suivant**.  
  
6.  Dans la page **Exportation de la clé privée**, cliquez sur **Oui, exporter la clé privée**, puis sur **Suivant**.  
  
7.  Sur le **Format de fichier d’exportation** , cliquez sur **échange d’informations personnelles - PKCS #12 (. PFX)** , puis cliquez sur **suivant**.  
  
8.  Sur le **sécurité** page, sélectionnez le **mot de passe** case à cocher, entrez un mot de passe dans le **mot de passe** zone et confirmer le mot de passe, puis cliquez sur **suivant**.  
  
9. Sur le **fichier à exporter** page, entrez un nom pour le fichier de certificat et enregistrez-le sur le bureau, puis cliquez sur **suivant**.  
  
10. Dans la page **Fin de l'Assistant Exportation de certificat**, cliquez sur **Terminer**.  
  
11. Sur le **Assistant Exportation de certificat** boîte de dialogue, cliquez sur **OK**.  
  
12. Copiez le certificat à tous les serveurs que vous souhaitez être membres du cluster.  
  
13. Sur le nouveau serveur DirectAccess, cliquez sur **Démarrer**, type **mmc** et appuyez sur ENTRÉE. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
14. Dans la console MMC, dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**.  
  
15. Sur le **ajouter ou supprimer des composants logiciel enfichables** boîte de dialogue, cliquez sur **certificats**, cliquez sur **ajouter**, cliquez sur **compte d’ordinateur**, cliquez sur  **Suivant**, cliquez sur **Terminer**, puis cliquez sur **OK**.  
  
16. Dans le volet gauche de la console, accédez à **certificats (ordinateur Local) \Personal\Certificates**. Bouton droit sur le **certificats** nœud, pointez sur **toutes les tâches**, puis cliquez sur **importation**.  
  
17. Dans la page **Bienvenue** de l'Assistant Importation de certificat, cliquez sur **Suivant**.  
  
18. Sur le **fichier à importer** , cliquez sur **Parcourir** pour localiser le certificat. Sélectionnez le certificat, puis cliquez **suivant**.  
  
19. Sur le **protection par clé privée** page, dans le **mot de passe** zone, tapez le mot de passe, puis cliquez sur **suivant**.  
  
20. Dans la page **Magasin de certificats**, cliquez sur **Suivant**.  
  
21. Dans la page **Fin de l’Assistant Importation du certificat**, cliquez sur **Terminer**.  
  
22. Sur le **Assistant Importation de certificat** boîte de dialogue, cliquez sur **OK**.  
  
23. Répétez les étapes 13 à 22 sur tous les serveurs que vous souhaitez être membres du cluster.  
  
## <a name="BKMK_NLS"></a>3.4 installer le certificat de serveur d’emplacement réseau  
L'appartenance au groupe local **Administrateurs**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.  
  
#### <a name="to-install-a-certificate-for-network-location"></a>Pour installer un certificat pour l’emplacement réseau  
  
1.  Sur le serveur d’accès à distance, cliquez sur **Démarrer**, type **mmc**, puis appuyez sur ENTRÉE. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
2.  Cliquez sur **Fichier**, puis sur **Ajouter ou supprimer des composants logiciels enfichables**.  
  
3.  Cliquez sur **certificats**, cliquez sur **ajouter**, cliquez sur **compte d’ordinateur**, cliquez sur **suivant**, cliquez sur **ordinateur Local**, cliquez sur **Terminer**, puis cliquez sur **OK**.  
  
4.  Dans l'arborescence de la console du composant logiciel enfichable Certificats, ouvrez **Certificats (ordinateur local)\Personnel\Certificats**.  
  
5.  Cliquez avec le bouton droit sur **Certificats**, pointez sur **Toutes les Tâches**, puis cliquez sur **Demander un nouveau certificat**.  
  
6.  Cliquez sur **Suivant** deux fois.  
  
7.  Sur le **demander des certificats** page et cliquez sur le modèle de certificat de serveur Web, puis cliquez sur **inscription pour obtenir ce certificat nécessite des informations plus**.  
  
    Si le modèle de certificat de serveur Web n’apparaît pas, vérifiez que le compte d’ordinateur de serveur accès à distance a droits d’inscription pour le modèle de certificat de serveur Web. Pour plus d’informations, consultez [configurer les autorisations sur le modèle de certificat de serveur Web](https://msdn.microsoft.com/library/ee649249.aspx).  
  
8.  Sur le **sujet** onglet de la **propriétés du certificat** boîte de dialogue **nom de l’objet**, pour **Type**, sélectionnez **courantes nom**.  
  
9. Dans **valeur**, tapez le nom de domaine complet (FQDN) pour le nom de l’intranet du site de serveur d’emplacement réseau (par exemple, nls.corp.contoso.com), puis cliquez sur **ajouter**.  
  
10. Cliquez sur **OK**, sur **Inscrire**, puis sur **Terminer**.  
  
11. Dans le volet d'informations du composant logiciel enfichable Certificats, vérifiez qu'un nouveau certificat avec le nom de domaine complet a été inscrit avec **Rôles prévus** égal à **Authentification du serveur**.  
  
12. Cliquez avec le bouton droit sur le certificat, puis cliquez sur **Propriétés**.  
  
13. Dans **Nom convivial**, tapez **Certificat Emplacement réseau**, puis cliquez sur **OK**.  
  
    > [!TIP]  
    > Les étapes 12 et 13 sont facultatifs, mais est plus facile pour vous permet de sélectionner le certificat pour l’emplacement réseau lors de la configuration d’accès à distance.  
  
14. Répétez cette procédure sur tous les serveurs que vous souhaitez être membres du cluster.  
  
## <a name="BKMK_Add"></a>3.5 ajouter des serveurs au cluster  
 
  
#### <a name="to-add-servers-to-the-cluster"></a>Pour ajouter des serveurs au cluster  
  
1.  Sur le serveur DirectAccess configuré, cliquez sur **Démarrer**, puis cliquez sur **gestion de l’accès à distance**. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
2.  Dans la Console de gestion de l'accès à distance, cliquez sur **Configuration**. Dans le **tâches** volet, sous **Cluster avec équilibrage de charge**, cliquez sur **ajouter ou supprimer des serveurs**.  
  
3.  Sur le **ajouter ou supprimer des serveurs** boîte de dialogue, cliquez sur **ajouter un serveur**.  
  
4.  Sur le **ajouter un serveur** boîte de dialogue le **sélectionner un serveur** page, entrez le nom du serveur d’accès à distance supplémentaire, puis cliquez sur **suivant**.  
  
5.  Sur le **cartes réseau** page, effectuez l’une les éléments suivants :  
  
    -   Si vous déployez une topologie avec deux cartes réseau, dans **adaptateur externe**, sélectionnez la carte est connectée au réseau externe. Dans **carte réseau interne**, sélectionnez la carte est connectée au réseau interne.  
  
    -   Si vous déployez une topologie avec une carte réseau, dans **carte réseau**, sélectionnez la carte est connectée au réseau interne.  
  
6.  Sur le **cartes réseau** page **sélectionner le certificat utilisé pour authentifier les connexions IP-HTTPS**, cliquez sur **Parcourir** pour rechercher et sélectionner le certificat IP-HTTPS, puis cliquez sur **suivant**.  
  
7.  Sur le **serveur emplacement réseau** , cliquez sur **Parcourir** à sélectionner le certificat pour le site de serveur d’emplacement réseau en cours d’exécution sur le serveur d’accès à distance, puis cliquez sur **suivant**.  
  
    > [!NOTE]  
    > Le **serveur emplacement réseau** page apparaît uniquement lorsque le site de serveur d’emplacement réseau est en cours d’exécution sur le serveur d’accès à distance.  
  
    > [!NOTE]  
    > Si VPN ont été également configuré sur le serveur d’accès à distance, puis vous serez être invité à ajouter VPN pool informations d’adresse IP à ce stade.  
  
8.  Sur le **Résumé** , cliquez sur **ajouter**.  
  
9. Sur la page **Fin**, cliquez sur **Fermer**.  
  
10. Répétez cette procédure pour tous les serveurs d’accès à distance à ajouter au cluster.  
  
11. Sur le **ajouter ou supprimer des serveurs** boîte de dialogue, cliquez sur **valider**.  
  
12. Sur le **Ajout et suppression de serveurs** boîte de dialogue, cliquez sur **fermer**.  
  
![Windows PowerShell](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
Add-RemoteAccessLoadBalancerNode -RemoteAccessServer <server name>  
```  
  
> [!NOTE]  
> Si VPN n’a pas été activé dans un cluster d’équilibrage de charge, vous ne devez pas fournir les plages d’adresses VPN lors de l’ajout d’un nouveau serveur au cluster à l’aide des applets de commande Windows PowerShell. Si vous avez fait par erreur, supprimez le serveur du cluster et ajoutez-le à nouveau au cluster sans spécifier les plages d’adresses VPN.  
  
## <a name="BKMK_remove"></a>3.6 supprimer un serveur du cluster  
 
  
#### <a name="to-remove-a-server-from-the-cluster"></a>Pour supprimer un serveur à partir du cluster  
  
1.  Sur le serveur d’accès à distance configuré, cliquez sur **Démarrer**, puis cliquez sur **gestion de l’accès à distance**. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
2.  Dans la Console de gestion de l'accès à distance, cliquez sur **Configuration**. Dans le **tâches** volet, sous **Cluster avec équilibrage de charge**, cliquez sur **ajouter ou supprimer des serveurs**.  
  
3.  Sur le **ajouter ou supprimer des serveurs** boîte de dialogue, sélectionnez le serveur d’accès à distance que vous souhaitez supprimer, puis cliquez sur **supprimer le serveur**.  
  
4.  Sur le **supprimer un avertissement Server** boîte de dialogue, assurez-vous que vous avez choisi le serveur adéquat, puis cliquez sur **OK**.  
  
5.  Répétez cette procédure pour tous les serveurs d’accès à distance être supprimé du cluster.  
  
6.  Sur le **ajouter ou supprimer des serveurs** boîte de dialogue, cliquez sur **valider**.  
  
7.  Sur le **Ajout et suppression de serveurs** boîte de dialogue, cliquez sur **fermer**.  
  
![Windows PowerShell](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
Remove-RemoteAccessLoadBalancerNode -RemoteAccessServer <server name>  
```  
  
## <a name="BKBK_disable"></a>3.7 désactiver l’équilibrage de charge  
[Effectuez cette étape à l’aide de Windows PowerShell](assetId:///7a817ca0-2b4a-4476-9d28-9a63ff2453f9)  
  
#### <a name="to-disable-load-balancing"></a>Pour désactiver l’équilibrage de charge  
  
1.  Sur le serveur DirectAccess configuré, cliquez sur **Démarrer**, puis cliquez sur **gestion de l’accès à distance**. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
2.  Dans la Console de gestion de l'accès à distance, cliquez sur **Configuration**. Dans le **tâches** volet, sous **Cluster avec équilibrage de charge**, cliquez sur **désactiver l’équilibrage de charge**.  
  
3.  Sur le **désactiver l’équilibrage de charge** boîte de dialogue, cliquez sur **Ok**.  
  
4.  Sur le **désactiver l’équilibrage de charge** boîte de dialogue, cliquez sur **fermer**.  
  
![Windows PowerShell](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
set-RemoteAccessLoadBalancer -disable  
```  
  
Désactiver l’équilibrage de charge supprimera les paramètres d’accès à distance et les paramètres de l’équilibrage de charge réseau (si configuré) de tous les serveurs à l’exception du serveur à partir de laquelle il est en cours d’exécution. Sur ce serveur d’accès à distance, les paramètres NLB seront supprimés (si elle a été configuré), mais resteront des paramètres d’accès à distance.  
  
En cliquant sur **supprimer les paramètres de configuration** supprimera l’accès à distance et équilibrage de charge réseau (si configuré) de tous les serveurs dans le déploiement.  
  
> [!NOTE]  
> -   Si l’accès à distance est désinstallé lorsque l’équilibrage de charge est déployé, tous les serveurs restent avec les adresses IP dynamiques. Les adresses IP virtuelles sont supprimées. Ainsi, tous les itinéraires du réseau d’entreprise qui sont ciblés pour les adresses des adresses IP virtuelles à échouer. Cela affecte également les entrées DNS qui ont été résolues pour les adresses IP virtuelles, telles que le nom d’objet du certificat du serveur emplacement réseau. Pour éviter ce problème, désactivez l’équilibrage de charge, ce qui laisse les adresses IP virtuelles sur le dernier serveur d’accès à distance et puis désinstallez l’accès à distance.  
> -   Après avoir utilisé le **Set-RemoteAccessLoadBalancer** applet de commande pour désactiver l’équilibrage de charge, veuillez patienter 2 minutes avant d’exécuter n’importe quel autre applet de commande. Cela doit également être effectuée dans tous les scripts qui s’exécutent une autre applet de commande après le **Set-RemoteAccessLoadBalancer-désactiver** applet de commande.  
> -   La désactivation modifications d’équilibrage de charge à l’adresse IP virtuelle du cluster en une adresse IP dédiée. Par conséquent, toute opération qui interroge pour le nom du serveur échoue jusqu'à l’expiration de l’entrée DNS mis en cache sur le serveur. Assurez-vous que vous n’exécutez pas les applets de commande PowerShell pour l’accès à distance après la désactivation de l’équilibrage de charge jusqu'à ce que le cache sur le serveur a expiré. Ce problème est plus courant si vous essayez de désactiver l’équilibrage de charge sur un ordinateur à partir d’un autre ordinateur qui se trouve dans un autre domaine. Cela se produit également si vous désactivez à partir de la console de gestion de l’accès à distance d’équilibrage de charge et que vous empêche peut-être de la configuration à partir de chargement. La configuration sera chargé une fois que le cache a expiré ou a été vidé.  
  
## <a name="BKMK_Links"></a>Voir aussi  
  
-   [Étape 4 : Vérification du cluster](Step-4-Verify-the-Cluster.md)  
  


