---
title: Installer et configurer le serveur NPS
description: Traitement du serveur NPS de demandes de connexion qui sont envoyés par le serveur VPN vérifie que l’utilisateur a l’autorisation de se connecter, l’identité de l’utilisateur et les aspects de la demande de connexion que vous avez choisi lorsque vous avez configuré la gestion des comptes RADIUS dans NPS.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: ca52aeeed8c4872e4d9a433c55bddc65a74d9dc0
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749617"
---
# <a name="step-4-install-and-configure-the-network-policy-server-nps"></a>Étape 4. Installer et configurer le serveur de stratégie réseau (NPS)

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**prochain :** Étape 3. Configurer le serveur d’accès à distance pour VPN Toujours actif (AlwaysOn)](vpn-deploy-ras.md)
- [**prochain :** Étape 5. Configurer DNS et les paramètres de pare-feu](vpn-deploy-dns-firewall.md)

Dans cette étape, vous allez installer le serveur NPS (Network Policy Server) pour le traitement des demandes de connexion qui sont envoyés par le serveur VPN :

- Procéder à l’autorisation pour vérifier que l’utilisateur est autorisé à se connecter.
- Exécution de l’authentification pour vérifier l’identité de l’utilisateur.
- Exécution de comptabilité pour consigner les aspects de la demande de connexion que vous avez choisi lorsque vous avez configuré la gestion des comptes RADIUS dans NPS.

Les étapes décrites dans cette section vous autoriser à terminer les éléments suivants :

1. Sur la machine virtuelle qui a étudié le serveur NPS et installé sur votre organisation ou votre réseau d’entreprise ou d’un ordinateur, vous pouvez installer NPS.

   >[!TIP]
   >Si vous avez déjà un ou plusieurs serveurs NPS sur votre réseau, vous n’avez pas besoin effectuer l’installation de serveur NPS : au lieu de cela, vous pouvez utiliser cette rubrique pour mettre à jour la configuration d’un serveur NPS.

> [!NOTE]
> Vous ne pouvez pas installer le service de serveur NPS sur Windows Server Core.

2. Sur le serveur NPS organisation / d’entreprise, vous pouvez configurer NPS à effectuer en tant qu’un serveur RADIUS qui traite les demandes de connexion provenant du serveur VPN.

## <a name="install-network-policy-server"></a>Installer un serveur NPS (Network Policy Server)

Dans cette procédure, vous installez NPS à l’aide de Windows PowerShell ou l’Assistant de fonctionnalités et un gestionnaire serveur ajouter des rôles. NPS est un service de rôle du rôle serveur Services de stratégie et d’accès réseau.

>[!TIP]
>Par défaut, le serveur NPS écoute le trafic RADIUS sur les ports 1812, 1813, 1645 et 1646 sur toutes les cartes réseau installées. Lorsque vous installez NPS, et que vous activez le pare-feu Windows avec fonctions avancées de sécurité, les exceptions de pare-feu pour ces ports sont automatiquement créées pour le trafic IPv4 et IPv6. Si vos serveurs d’accès réseau sont configurés pour envoyer le trafic RADIUS sur des ports autres que ces valeurs par défaut, supprimez les exceptions créées dans le pare-feu Windows avec fonctions avancées de sécurité pendant l’installation du serveur NPS et créer des exceptions pour les ports que vous n’utilisez pas pour Trafic RADIUS.

**Procédure pour Windows PowerShell :**

Pour effectuer cette procédure à l’aide de Windows PowerShell, exécutez Windows PowerShell en tant qu’administrateur, entrez l’applet de commande suivante :

```powershell
Install-WindowsFeature NPAS -IncludeManagementTools
```

**Procédure pour le Gestionnaire de serveur :**

1.  Dans le Gestionnaire de serveur, sélectionnez **gérer**, puis sélectionnez **Ajout de rôles et fonctionnalités**.
        L’Assistant Ajout de rôles et de fonctionnalités s’ouvre.

2.  Dans avant de commencer, sélectionnez **suivant**.

    >[!NOTE] 
    >Le **avant de commencer** page d’ajout de rôles et fonctionnalités Assistant s’affiche pas si vous aviez sélectionné précédemment **ignorer cette page par défaut** lorsque l’ajout de rôles et fonctionnalités Assistant exécutaient.

3.  Dans Sélectionner Type d’Installation, vérifiez que **installation en fonction du rôle ou une fonctionnalité** est sélectionné, puis sélectionnez **suivant**.

4.  Sélectionnez serveur de destination, vérifiez que **sélectionner un serveur du pool de serveurs** est sélectionné.

5.  Dans le Pool de serveurs, assurez-vous que l’ordinateur local est sélectionné, puis sélectionnez **suivant**.

6.  Dans Sélectionner des rôles de serveur, dans **rôles**, sélectionnez **stratégie réseau et Services d’accès**. Une boîte de dialogue s’ouvre, vous demandant si elle doit ajouter les fonctionnalités requises pour la stratégie de réseau et les Services d’accès.

7.  Sélectionnez **ajouter des fonctionnalités**, puis sélectionnez **suivant**

8.  Dans Sélectionner les fonctionnalités, sélectionnez **suivant**, dans la stratégie de réseau et accéder aux Services, examinez les informations fournies, puis sélectionnez **suivant**.

9.  Dans les services de sélectionner un rôle, sélectionnez **Network Policy Server**.

10. Pour les fonctionnalités requises pour le serveur NPS, sélectionnez **ajouter des fonctionnalités**, puis sélectionnez **suivant**.

11. Dans Confirmer les sélections d’installation, sélectionnez **redémarrer automatiquement le serveur de destination si nécessaire**.

12. Sélectionnez **Oui** pour confirmer le texte sélectionné, puis sélectionnez **installer**.
    
    La page de progression d’Installation affiche l’état pendant le processus d’installation. La fin du processus, le message « Installation réussie sur *ComputerName*» s’affiche, où *ComputerName* est le nom de l’ordinateur sur lequel vous avez installé le serveur de stratégie réseau.

13. Sélectionnez **Fermer**.

## <a name="configure-nps"></a>Configurer NPS

Une fois l’installation de serveur NPS, vous configurez NPS pour gérer toutes les authentifications, d’autorisation, et demandent des droits de gestion des comptes de connexion reçoit à partir du serveur VPN.

### <a name="register-the-nps-server-in-active-directory"></a>Inscrire le serveur NPS dans Active Directory

Dans cette procédure, vous inscrivez le serveur dans Active Directory afin qu’il a l’autorisation d’accéder aux informations de compte d’utilisateur lors du traitement des demandes de connexion.

**Procédure :**

1.  Dans le Gestionnaire de serveur, sélectionnez **outils**, puis sélectionnez **Network Policy Server**. La console NPS s’ouvre.

2.  Dans la console NPS, cliquez sur **NPS (Local)** , puis sélectionnez **enregistrer un serveur dans Active Directory**.
   
     La boîte de dialogue serveur de stratégie réseau s’ouvre.

3.  Dans la boîte de dialogue serveur NPS, sélectionnez **OK** à deux reprises.

Pour les autres méthodes d’inscription de serveur NPS, consultez [inscrire un serveur NPS dans un domaine Active Directory](../../../../../networking/technologies/nps/nps-manage-register.md).

### <a name="configure-network-policy-server-accounting"></a>Configurer la gestion des comptes de serveur NPS (Network Policy Server)

Dans cette procédure, configurez la stratégie serveur Comptabilité réseau à l’aide d’un des types de journalisation suivants :

- **Journalisation des événements**. Principalement utilisée pour l’audit et résolution des problèmes de tentatives de connexion. Vous pouvez configurer la journalisation en obtenant les propriétés du serveur NPS dans la console NPS des événements NPS.

- **Enregistrement des demandes de comptabilité et de l’authentification des utilisateurs dans un fichier local**. Utilisé principalement pour les fins de facturation et d’analyse de connexion. Également utilisé comme un outil d’enquête de sécurité, car il fournit une méthode de suivi de l’activité d’un utilisateur malveillant après une attaque. Vous pouvez configurer la journalisation de fichier local à l’aide de l’Assistant Configuration de comptabilité.

- **Journalisation de l’authentification des utilisateurs et les demandes de gestion dans une base de données Microsoft SQL Server compatible XML**. Utilisé pour permettre à plusieurs serveurs NPS pour avoir une source de données en cours d’exécution. Fournit également les avantages de l’utilisation d’une base de données relationnelle. Vous pouvez configurer la journalisation de SQL Server à l’aide de l’Assistant Configuration de comptabilité.

Pour configurer la comptabilité de serveur de stratégie réseau, consultez [configurer réseau stratégie serveur comptabilité](../../../../../networking/technologies/nps/nps-accounting-configure.md).

### <a name="add-the-vpn-server-as-a-radius-client"></a>Ajoutez le serveur VPN comme Client RADIUS

Dans le [configurer le serveur d’accès à distance pour VPN Always On](vpn-deploy-ras.md) section, vous avez installé et configuré votre serveur VPN. Lors de la configuration du serveur VPN, vous avez ajouté un secret partagé RADIUS sur le serveur VPN.

Dans cette procédure, vous utilisez la même chaîne de texte secret partagé pour configurer le serveur VPN en tant que client RADIUS dans NPS. Utilisez la même chaîne de texte que vous avez utilisé sur le serveur VPN ou l’échec de la communication entre le serveur NPS et le serveur VPN.

>[!IMPORTANT] 
>Lorsque vous ajoutez un nouveau serveur d’accès réseau (VPN server, point d’accès sans fil, commutateur d’authentification ou serveur à distance) à votre réseau, vous devez ajouter le serveur en tant que client RADIUS dans NPS, afin que NPS connaît et peut communiquer avec le serveur d’accès réseau.

**Procédure :**

1. Sur le serveur NPS, dans la console NPS, double-cliquez sur **Clients et serveurs RADIUS**.

2. Avec le bouton droit **Clients RADIUS** et sélectionnez **New**. La boîte de dialogue Nouveau Client RADIUS s’ouvre.

3. Vérifiez que le **activer ce client RADIUS** case à cocher est activée.

4. Dans **nom convivial**, entrez un nom d’affichage pour le serveur VPN.

5. Dans **adresse (IP ou DNS)** , entrez le nom de domaine complet ou l’adresse IP du NAS.
     
     Si vous entrez le nom de domaine complet, sélectionnez **Vérifiez** si vous souhaitez vérifier que le nom est correct et qu’il est mappé à une adresse IP valide.

6. Dans **secret partagé**, faire :

    1. Vérifiez que **manuel** est sélectionné.

    2. Entrez la chaîne de texte fort que vous avez entré également sur le serveur VPN.

    3. Retapez le secret partagé dans Confirmer le secret partagé.

7. Sélectionnez **OK**. Le serveur VPN apparaît dans la liste de clients RADIUS configuré sur le serveur NPS.

## <a name="configure-nps-as-a-radius-for-vpn-connections"></a>Configurer NPS en tant qu’un rayon pour les connexions VPN

Dans cette procédure, vous configurez NPS comme serveur RADIUS sur le réseau de votre organisation. Sur le serveur NPS, vous devez définir une stratégie qui autorise uniquement les utilisateurs dans un groupe spécifique puissent accéder au réseau d’entreprise organisation via le serveur VPN - puis uniquement lorsque vous utilisez un certificat d’utilisateur valide dans une demande d’authentification PEAP.

**Procédure :**

1. Dans la console NPS, dans une Configuration Standard, vérifiez que **serveur RADIUS pour l’accès à distance ou les connexions VPN** est sélectionné.

2. Sélectionnez **configurer à distance ou VPN**.
        
    L’Assistant de configurer une connexion VPN ou d’accès à distance s’ouvre.

3. Sélectionnez **des connexions de réseau privé virtuel (VPN, Virtual Private Network)** , puis sélectionnez **suivant**.

4. Dans spécifier l’accès à distance ou VPN Server, les clients RADIUS, sélectionnez le nom du serveur VPN que vous avez ajouté à l’étape précédente. Par exemple, si le nom NetBIOS de votre serveur VPN est RAS1, sélectionnez **RAS1**.

5. Sélectionnez **Suivant**.

6. Dans configurer des méthodes d’authentification, procédez comme suit :

    1. Effacer la **authentification chiffrée Microsoft version 2 (MS-CHAPv2)** case à cocher.

    2. Sélectionnez le **Extensible Authentication Protocol** case à cocher pour le sélectionner.

    3. Dans Type (selon la méthode de configuration d’accès et réseau), sélectionnez **Microsoft : PEAP (Protected EAP)** , puis sélectionnez **configurer**.
      
        La boîte de dialogue Modifier les propriétés EAP protégées s’ouvre.

    4. Sélectionnez **supprimer** pour supprimer le type EAP du mot de passe sécurisé (EAP-MSCHAP v2).

    5. Sélectionnez **ajouter**. La boîte de dialogue Ajouter un EAP s’ouvre.

    6. Sélectionnez **carte à puce ou autre certificat**, puis sélectionnez **OK**.

    7. Sélectionnez **OK** pour modifier les propriétés EAP protégées de fermer.

7. Sélectionnez **Suivant**.

8. Spécifier des groupes d’utilisateurs, procédez comme suit :

    1. Sélectionnez **ajouter**. La boîte de dialogue Sélectionner des utilisateurs, les ordinateurs, les comptes de Service ou les groupes s’ouvre.

    2. Entrez **utilisateurs VPN**, puis sélectionnez **OK**.

    3. Sélectionnez **Suivant**.

9. Dans spécifier des filtres IP, sélectionnez **suivant**.

10. Dans spécifier les paramètres de chiffrement, sélectionnez **suivant**. N’apportez pas de modifications.

    Ces paramètres s’appliquent uniquement aux connexions de normes Microsoft point à point Encryption (MPPE), ce qui ce scénario ne prend pas en charge.

11. Dans spécifier un nom de domaine, sélectionnez **suivant**.

12. Sélectionnez **Terminer** pour fermer l’Assistant.

## <a name="autoenroll-the-nps-server-certificate"></a>Inscription automatique de certificat du serveur NPS

Dans cette procédure, vous actualisez la stratégie de groupe sur le serveur NPS local manuellement. Lorsque des actualisations de stratégie de groupe, si l’inscription automatique est configurée et fonctionne correctement, l’ordinateur local est automatique-inscrit un certificat par l’autorité de certification (CA).

>[!NOTE]  
>Stratégie de groupe actualisée automatiquement lorsque vous redémarrez l’ordinateur membre du domaine, ou quand un utilisateur ouvre une session sur un ordinateur membre du domaine. En outre, stratégie de groupe actualise régulièrement. Par défaut, cette actualisation périodique se produit toutes les 90 minutes avec un décalage aléatoire de 30 minutes.

Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.

**Procédure :**

1. Sur le serveur NPS, ouvrez Windows PowerShell.

2. À l’invite Windows PowerShell, tapez **gpupdate**, puis appuyez sur ENTRÉE.

## <a name="next-steps"></a>Étapes suivantes

[Étape 5. Configurer les paramètres DNS et de pare-feu pour VPN Always On](vpn-deploy-dns-firewall.md): Dans cette étape, vous installez le serveur NPS (Network Policy Server) à l’aide de Windows PowerShell ou l’Assistant de fonctionnalités et un gestionnaire serveur ajouter des rôles. Vous configurez également NPS pour gérer l’authentification, l’autorisation et les droits de gestion des comptes de connexion toutes les demandes qu’il reçoit à partir du serveur VPN.