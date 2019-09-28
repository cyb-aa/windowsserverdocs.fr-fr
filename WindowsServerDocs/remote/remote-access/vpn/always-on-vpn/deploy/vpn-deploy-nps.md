---
title: Installer et configurer le serveur NPS
description: Le traitement par le serveur NPS des demandes de connexion envoyées par le serveur VPN vérifie que l’utilisateur a l’autorisation de se connecter, l’identité de l’utilisateur et enregistre les aspects de la demande de connexion que vous avez choisie lors de la configuration de la gestion de comptes RADIUS dans NPS.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: 5cb0d342afec9c28259efb7a2e15666358f3cb5b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404253"
---
# <a name="step-4-install-and-configure-the-network-policy-server-nps"></a>Étape 4. Installer et configurer le serveur NPS (Network Policy Server)

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Situé** Étape 3. Configurer le serveur d’accès à distance pour VPN Toujours actif (AlwaysOn)](vpn-deploy-ras.md)
- [**Situé** Étape 5. Configurer DNS et les paramètres de pare-feu](vpn-deploy-dns-firewall.md)

Au cours de cette étape, vous allez installer le serveur NPS (Network Policy Server) pour le traitement des demandes de connexion envoyées par le serveur VPN :

- Effectuez l’autorisation pour vérifier que l’utilisateur est autorisé à se connecter.
- Exécution de l’authentification pour vérifier l’identité de l’utilisateur.
- Exécution de la gestion des comptes pour enregistrer les aspects de la demande de connexion que vous avez choisie lors de la configuration de la gestion de comptes RADIUS dans NPS.

Les étapes de cette section vous permettent de compléter les éléments suivants :

1. Sur l’ordinateur ou la machine virtuelle qui a prévu le serveur NPS et installé sur votre organisation ou votre réseau d’entreprise, vous pouvez installer NPS.

   >[!TIP]
   >Si vous disposez déjà d’un ou plusieurs serveurs NPS sur votre réseau, vous n’avez pas besoin d’effectuer l’installation du serveur NPS. vous pouvez utiliser cette rubrique pour mettre à jour la configuration d’un serveur NPS existant.

> [!NOTE]
> Vous ne pouvez pas installer le service NPS (Network Policy Server) sur Windows Server Core.

2. Sur le serveur NPS de l’organisation/de l’entreprise, vous pouvez configurer NPS pour qu’il s’exécute en tant que serveur RADIUS qui traite les demandes de connexion reçues à partir du serveur VPN.

## <a name="install-network-policy-server"></a>Installer un serveur NPS (Network Policy Server)

Dans cette procédure, vous installez NPS à l’aide de Windows PowerShell ou de l’Assistant Gestionnaire de serveur ajouter des rôles et des fonctionnalités. NPS est un service de rôle du rôle serveur Services de stratégie et d’accès réseau.

>[!TIP]
>Par défaut, le serveur NPS écoute le trafic RADIUS sur les ports 1812, 1813, 1645 et 1646 sur toutes les cartes réseau installées. Lorsque vous installez NPS et activez le pare-feu Windows avec fonctions avancées de sécurité, les exceptions de pare-feu pour ces ports sont créées automatiquement pour le trafic IPv4 et IPv6. Si vos serveurs d’accès réseau sont configurés pour envoyer le trafic RADIUS sur des ports autres que ces paramètres par défaut, supprimez les exceptions créées dans le pare-feu Windows avec fonctions avancées de sécurité lors de l’installation du serveur NPS, et créez des exceptions pour les ports que vous utilisez pour Trafic RADIUS.

**Procédure pour Windows PowerShell :**

Pour effectuer cette procédure à l’aide de Windows PowerShell, exécutez Windows PowerShell en tant qu’administrateur, puis entrez l’applet de commande suivante :

```powershell
Install-WindowsFeature NPAS -IncludeManagementTools
```

**Procédure de Gestionnaire de serveur :**

1.  Dans Gestionnaire de serveur, sélectionnez **gérer**, puis sélectionnez **Ajouter des rôles et des fonctionnalités**.
        L’Assistant Ajout de rôles et de fonctionnalités s’ouvre.

2.  Dans avant de commencer, sélectionnez **suivant**.

    >[!NOTE] 
    >La page **avant de commencer** de l’Assistant Ajout de rôles et de fonctionnalités ne s’affiche pas si vous avez déjà sélectionné **ignorer cette page par défaut** lors de l’exécution de l’Assistant Ajout de rôles et de fonctionnalités.

3.  Dans sélectionner le type d’installation, vérifiez que installation basée sur **un rôle ou une fonctionnalité** est sélectionné, puis sélectionnez **suivant**.

4.  Dans sélectionner le serveur de destination, assurez-vous que **l’option Sélectionner un serveur du pool de serveurs** est sélectionnée.

5.  Dans pool de serveurs, assurez-vous que l’ordinateur local est sélectionné, puis sélectionnez **suivant**.

6.  Dans sélectionner des rôles de serveurs, dans **rôles**, sélectionnez **services de stratégie et d’accès réseau**. Une boîte de dialogue s’ouvre et vous demande s’il doit ajouter des fonctionnalités requises pour les services de stratégie et d’accès réseau.

7.  Sélectionnez **Ajouter des fonctionnalités**, puis cliquez sur **suivant** .

8.  Dans sélectionner des fonctionnalités, sélectionnez **suivant**, puis dans services de stratégie et d’accès réseau, vérifiez les informations fournies, puis sélectionnez **suivant**.

9.  Dans sélectionner des services de rôle, sélectionnez **serveur NPS (Network Policy Server**).

10. Pour les fonctionnalités requises pour le serveur NPS, sélectionnez **Ajouter des fonctionnalités**, puis cliquez sur **suivant**.

11. Dans confirmer les sélections d’installation, sélectionnez **redémarrer automatiquement le serveur de destination, si nécessaire**.

12. Sélectionnez **Oui** pour confirmer la sélection, puis sélectionnez **installer**.
    
    La page progression de l’installation affiche l’État au cours du processus d’installation. Une fois le processus terminé, le message « installation réussie sur *nomordinateur*» s’affiche, où *nomordinateur* est le nom de l’ordinateur sur lequel vous avez installé le serveur de stratégie réseau.

13. Sélectionnez **Fermer**.

## <a name="configure-nps"></a>Configurer NPS

Après avoir installé NPS, vous configurez NPS pour gérer toutes les tâches d’authentification, d’autorisation et de comptabilité pour la demande de connexion qu’il reçoit du serveur VPN.

### <a name="register-the-nps-server-in-active-directory"></a>Inscrire le serveur NPS dans Active Directory

Dans cette procédure, vous inscrivez le serveur dans Active Directory afin qu’il soit autorisé à accéder aux informations de compte d’utilisateur lors du traitement des demandes de connexion.

**Procédures**

1.  Dans Gestionnaire de serveur, sélectionnez **Outils**, puis sélectionnez **serveur NPS (Network Policy Server**). La console NPS s’ouvre.

2.  Dans la console NPS, cliquez avec le bouton droit sur **NPS (local)** , puis sélectionnez **inscrire le serveur dans Active Directory**.
   
     La boîte de dialogue serveur NPS (Network Policy Server) s’ouvre.

3.  Dans la boîte de dialogue serveur de stratégie réseau, cliquez deux fois sur **OK** .

Pour connaître les autres méthodes d’inscription de NPS, consultez [inscrire un serveur NPS dans un domaine Active Directory](../../../../../networking/technologies/nps/nps-manage-register.md).

### <a name="configure-network-policy-server-accounting"></a>Configurer la gestion des comptes de serveur NPS (Network Policy Server)

Dans cette procédure, configurez Network Policy Server Accounting à l’aide de l’un des types de journalisation suivants :

- **Journalisation des événements**. Utilisé principalement pour l’audit et le dépannage des tentatives de connexion. Vous pouvez configurer la journalisation des événements NPS en obtenant les propriétés du serveur NPS dans la console NPS.

- **Enregistrement des demandes d’authentification des utilisateurs et de gestion des comptes dans un fichier local**. Principalement utilisé pour l’analyse des connexions et la facturation. Également utilisé comme outil d’investigation de sécurité, car il vous offre une méthode de suivi de l’activité d’un utilisateur malveillant après une attaque. Vous pouvez configurer la journalisation des fichiers locaux à l’aide de l’Assistant Configuration de la gestion des comptes.

- **Enregistrement des demandes d’authentification des utilisateurs et de gestion des comptes dans une base de données conforme à la norme XML Microsoft SQL Server**. Permet d’autoriser plusieurs serveurs exécutant NPS à avoir une seule source de données. Offre également les avantages de l’utilisation d’une base de données relationnelle. Vous pouvez configurer SQL Server la journalisation à l’aide de l’Assistant Configuration de la gestion des comptes.

Pour configurer la gestion des comptes de serveur de stratégie réseau, consultez [configurer la gestion des serveurs de stratégie réseau](../../../../../networking/technologies/nps/nps-accounting-configure.md).

### <a name="add-the-vpn-server-as-a-radius-client"></a>Ajouter le serveur VPN en tant que client RADIUS

Dans la section [configurer le serveur d’accès à distance pour Always on VPN](vpn-deploy-ras.md) , vous avez installé et configuré votre serveur VPN. Au cours de la configuration du serveur VPN, vous avez ajouté un secret partagé RADIUS sur le serveur VPN.

Dans cette procédure, vous utilisez la même chaîne de texte secret partagé pour configurer le serveur VPN en tant que client RADIUS dans NPS. Utilisez la même chaîne de texte que celle utilisée sur le serveur VPN ou la communication entre le serveur NPS et le serveur VPN échoue.

>[!IMPORTANT] 
>Lorsque vous ajoutez un nouveau serveur d’accès réseau (serveur VPN, point d’accès sans fil, commutateur d’authentification ou serveur d’accès à distance) à votre réseau, vous devez ajouter le serveur en tant que client RADIUS dans NPS afin que NPS prenne en charge et puisse communiquer avec le serveur d’accès réseau.

**Procédures**

1. Sur le serveur NPS, dans la console NPS, double-cliquez sur **clients et serveurs RADIUS**.

2. Cliquez avec le bouton droit sur **clients RADIUS** et sélectionnez **nouveau**. La boîte de dialogue Nouveau client RADIUS s’ouvre.

3. Vérifiez que la case à cocher **activer ce client RADIUS** est activée.

4. Dans **nom convivial**, entrez un nom d’affichage pour le serveur VPN.

5. Dans **adresse (IP ou DNS)** , entrez l’adresse IP ou le nom de domaine complet du NAS.
     
     Si vous entrez le nom de domaine complet, sélectionnez **vérifier** si vous souhaitez vérifier que le nom est correct et qu’il correspond à une adresse IP valide.

6. Dans **secret partagé**, procédez comme suit :

    1. Vérifiez que **Manuel** est sélectionné.

    2. Entrez la chaîne de texte fort que vous avez également entrée sur le serveur VPN.

    3. Entrez de nouveau le secret partagé dans confirmer le secret partagé.

7. Sélectionnez **OK**. Le serveur VPN apparaît dans la liste des clients RADIUS configurés sur le serveur NPS.

## <a name="configure-nps-as-a-radius-for-vpn-connections"></a>Configurer NPS en tant que RADIUS pour les connexions VPN

Dans cette procédure, vous configurez NPS en tant que serveur RADIUS sur le réseau de votre organisation. Sur le serveur NPS, vous devez définir une stratégie qui autorise uniquement les utilisateurs d’un groupe spécifique à accéder au réseau de l’organisation/de l’entreprise par le biais du serveur VPN, puis uniquement lors de l’utilisation d’un certificat utilisateur valide dans une demande d’authentification PEAP.

**Procédures**

1. Dans la console NPS, dans configuration standard, vérifiez que l’option **serveur RADIUS pour les connexions d’accès à distance ou VPN** est sélectionnée.

2. Sélectionnez **configurer le VPN ou l’accès à distance**.
        
    L’Assistant Configuration de VPN ou d’accès à distance s’ouvre.

3. Sélectionnez **connexions de réseau privé virtuel (VPN)** , puis sélectionnez **suivant**.

4. Dans spécifier le serveur d’accès à distance ou VPN, dans clients RADIUS, sélectionnez le nom du serveur VPN que vous avez ajouté à l’étape précédente. Par exemple, si le nom NetBIOS de votre serveur VPN est RAS1, sélectionnez **RAS1**.

5. Sélectionnez **Suivant**.

6. Dans configurer les méthodes d’authentification, procédez comme suit :

    1. Désactivez la case à cocher **Microsoft Encrypted Authentication version 2 (MS-CHAPv2)** .

    2. Activez la case à cocher **protocole EAP (Extensible Authentication Protocol** ) pour la sélectionner.

    3. Dans type (en fonction de la méthode d’accès et de la configuration réseau **), sélectionnez Microsoft : PEAP (Protected EAP**), puis sélectionnez **configurer**.
      
        La boîte de dialogue Modifier les propriétés EAP protégées s’ouvre.

    4. Sélectionnez **supprimer** pour supprimer le type EAP de mot de passe sécurisé (EAP-MSCHAP v2).

    5. Sélectionnez **Ajouter**. La boîte de dialogue Ajouter un EAP s’ouvre.

    6. Sélectionnez **carte à puce ou autre certificat**, puis cliquez sur **OK**.

    7. Sélectionnez **OK** pour fermer modifier les propriétés EAP protégées.

7. Sélectionnez **Suivant**.

8. Dans spécifier des groupes d’utilisateurs, procédez comme suit :

    1. Sélectionnez **Ajouter**. La boîte de dialogue Sélectionner les utilisateurs, les ordinateurs, les comptes de service ou les groupes s’ouvre.

    2. Entrez **les utilisateurs VPN**, puis sélectionnez **OK**.

    3. Sélectionnez **Suivant**.

9. Dans spécifier des filtres IP, sélectionnez **suivant**.

10. Dans spécifier les paramètres de chiffrement, sélectionnez **suivant**. N’apportez aucune modification.

    Ces paramètres s’appliquent uniquement aux connexions MPPE (Microsoft Point-to-Point Encryption), ce scénario ne prend pas en charge.

11. Dans spécifier un nom de domaine, sélectionnez **suivant**.

12. Sélectionnez **Terminer** pour fermer l’Assistant.

## <a name="autoenroll-the-nps-server-certificate"></a>Inscrire automatiquement le certificat de serveur NPS

Dans cette procédure, vous actualisez stratégie de groupe manuellement sur le serveur NPS local. Lorsque stratégie de groupe est actualisé, si l’inscription automatique des certificats est configurée et fonctionne correctement, l’ordinateur local est inscrit automatiquement auprès de l’autorité de certification (CA).

>[!NOTE]  
>Stratégie de groupe actualisé automatiquement lorsque vous redémarrez l’ordinateur membre du domaine, ou lorsqu’un utilisateur ouvre une session sur un ordinateur membre du domaine. En outre, stratégie de groupe régulièrement actualisé. Par défaut, cette actualisation périodique se produit toutes les 90 minutes avec un décalage aléatoire pouvant atteindre 30 minutes.

Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.

**Procédures**

1. Sur le serveur NPS, ouvrez Windows PowerShell.

2. À l’invite de commandes Windows PowerShell, tapez **gpupdate**, puis appuyez sur entrée.

## <a name="next-steps"></a>Étapes suivantes

[Étape 5. Configurer les paramètres DNS et de pare-](vpn-deploy-dns-firewall.md)feu pour Always on VPN : Dans cette étape, vous installez le serveur NPS (Network Policy Server) à l’aide de Windows PowerShell ou de l’Assistant Gestionnaire de serveur ajouter des rôles et des fonctionnalités. Vous configurez également NPS pour gérer toutes les tâches d’authentification, d’autorisation et de comptabilité pour les demandes de connexion qu’il reçoit du serveur VPN.