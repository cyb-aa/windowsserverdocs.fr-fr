---
title: Installer et configurer le serveur NPS
description: Traitement du serveur NPS de demandes de connexion qui sont envoyées par le serveur VPN vérifie que l’utilisateur est autorisé à se connecter, l’identité de l’utilisateur et consigne les aspects de la demande de connexion que vous avez choisies lorsque vous avez configuré la gestion des comptes RADIUS dans NPS.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: ca53ef28497a78f264c60ac1132f721fb6e01c15
ms.sourcegitcommit: 9c142ad4d4321baf328707f29fa104247ff5149a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2019
ms.locfileid: "9035765"
---
# Étape4. Installer et configurer le serveur NPS (Network Policy)

>   S’applique à: Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10


& #171;  [ **Suivant:** étape 3. Configurer le serveur d’accès à distance pour toujours sur le VPN](vpn-deploy-ras.md)<br>
& #187;  [ **Suivant:** étape 5. Configurer les paramètres de pare-feu et de DNS](vpn-deploy-dns-firewall.md)


Dans cette étape, vous installez le serveur NPS (Network Policy Server) pour le traitement des demandes de connexion qui sont envoyées par le serveur VPN:

- Procéder à l’autorisation pour vérifier que l’utilisateur est autorisé à se connecter.
- Exécution de l’authentification pour vérifier l’identité de l’utilisateur.
- Gestion des comptes pour consigner les aspects de la demande de connexion que vous avez choisies lorsque vous avez configuré la gestion des comptes RADIUS dans NPS d’effectuer.

Les étapes décrites dans cette section vous permettent de vous permet d’effectuer les éléments suivants:

1.  Sur l’ordinateur ou un ordinateur virtuel qui prévue pour le serveur NPS et installé sur votre organisation ou votre réseau d’entreprise, vous pouvez installer NPS.

   >[!TIP] 
   >Si vous disposez déjà d’un ou plusieurs serveurs NPS sur votre réseau, vous n’avez pas besoin d’effectuer l’installation de serveur NPS: au lieu de cela, vous pouvez utiliser cette rubrique pour mettre à jour de la configuration d’un serveur NPS existant.

>[!NOTE]  
Vous ne pouvez pas installer le service de serveur NPS sur Windows Server Core.

2.  Sur le serveur NPS organisation / d’entreprise, vous pouvez configurer NPS pour exécuter en tant que serveur RADIUS qui traite les demandes de connexion provenant du serveur VPN.

## Installer un serveur NPS (Network Policy Server)

Dans cette procédure, vous installez NPS à l’aide de Windows PowerShell ou l’ajouter des rôles serveur gestionnaire et l’Assistant de fonctionnalités. NPS est un service de rôle du rôle de serveur stratégie réseau et Services d’accès.

>[!TIP] 
>Par défaut, NPS écoute le trafic RADIUS sur les ports 1812, 1813, 1645 et 1646 sur toutes les cartes réseau installées. Lorsque vous installez NPS et que vous activez le pare-feu Windows avec fonctions avancées de sécurité, des exceptions de pare-feu pour ces ports sont automatiquement créées pour le trafic IPv4 et IPv6. Si vos serveurs d’accès réseau sont configurés pour envoyer le trafic RADIUS sur les ports autres que ces valeurs par défaut, supprimez les exceptions créées dans le pare-feu Windows avec fonctions avancées de sécurité lors de l’installation de NPS et créer des exceptions pour les ports que vous utilisez pour Trafic RADIUS.

**Procédure pour Windows PowerShell:**

Pour effectuer cette procédure à l’aide de Windows PowerShell, exécutez Windows PowerShell en tant qu’administrateur, tapez la commande suivante et appuyez sur ENTRÉE.

`Install-WindowsFeature NPAS -IncludeManagementTools`

**Procédure pour le Gestionnaire de serveur:**

1.  Dans le Gestionnaire de serveur, cliquez sur **Gérer**, puis cliquez sur **Ajouter des rôles et fonctionnalités**. <p>L’Assistant Ajout de rôles et fonctionnalités s’ouvre.

2.  Dans avant de commencer, cliquez sur **suivant**.

    >[!NOTE] 
    >La page de l’ajout de rôles et fonctionnalités Assistant **Avant de commencer** s’affiche pas si vous aviez précédemment sélectionné **Ignorer cette page par défaut** lors de l’ajout de rôles et fonctionnalités Assistant a été exécuté.

1.  Dans Select Installation Type, assurez-vous que **l’installation en fonction du rôle ou une fonctionnalité** est activée et cliquez sur **suivant**.

2.  Dans le serveur de destination Select, assurez-vous que l’option **Sélectionner un serveur à partir du pool de serveurs** est sélectionnée.

3.  Dans le Pool de serveurs, assurez-vous que l’ordinateur local est sélectionné, puis cliquez sur **suivant**.

4.  Dans Sélectionner des rôles de serveur, dans les **rôles**, sélectionnez **la stratégie réseau et Services d’accès**. Une boîte de dialogue s’ouvre, vous demandant si elle doit ajouter les fonctionnalités requises pour la stratégie réseau et de Services d’accès.

5.  Cliquez sur **Ajouter des fonctionnalités**, puis cliquez sur **suivant**

6.  Fonctionnalités de sélection, cliquez sur **suivant**et dans la stratégie réseau et Services d’accès, passez en revue les informations fournies, puis cliquez sur **suivant**.

7.  Dans les services de rôle Select, cliquez sur **Network Policy Server**.

8.  Pour les fonctionnalités requises pour le serveur NPS, cliquez sur **Ajouter des fonctionnalités** et cliquez sur **suivant**.

9.  Dans les sélections pour confirmer l’installation, cliquez sur **redémarrer le serveur de destination automatiquement si nécessaire**.

10. Cliquez sur **Oui** pour confirmer le texte sélectionné, puis cliquez sur **installer**. <p>La page de progression de l’Installation s’affiche l’état pendant le processus d’installation. Lorsque le processus terminé, le message «Installation réussie sur *nom_ordinateur*» s’affiche, où *nom_ordinateur* est le nom de l’ordinateur sur lequel vous avez installé le serveur NPS.

11. Cliquez sur **Fermer**.

## Configurer un serveur NPS

Une fois que l’installation de serveur NPS, vous configurez NPS pour gérer tous les authentification, l’autorisation et droits de gestion des comptes de connexion demandent qu’il reçoit à partir du serveur VPN.

### Inscrire le serveur NPS dans Active Directory

Dans cette procédure, vous inscrivez le serveur dans Active Directory afin qu’il est autorisé à accéder aux informations de compte d’utilisateur lors du traitement des demandes de connexion.

**Procédure:**

1.  Dans le Gestionnaire de serveur, cliquez sur les **Outils**, puis cliquez sur **Network Policy Server**. La console NPS s’ouvre.

2.  Dans la console NPS, avec le bouton droit **NPS (Local)**, puis cliquez sur **inscrire un serveur dans Active Directory** pour le sélectionner.<p>La boîte de dialogue Network Policy Server s’ouvre.

3.  Dans la boîte de dialogue Network Policy Server, cliquez deux fois sur **OK** .

Pour les autres méthodes d’inscription de serveur NPS, voir [l’inscrire un serveur NPS dans un domaine Active Directory](../../../../../networking/technologies/nps/nps-manage-register.md).

### Configurer la gestion des comptes de serveur NPS (Network Policy Server)

Dans cette procédure, configurez la stratégie serveur Comptabilité réseau à l’aide d’un des types de journalisation suivants:

-   **Journalisation des événements**. Principalement utilisé pour l’audit et la résolution des problèmes de tentatives de connexion. Vous pouvez configurer la journalisation en obtenant les propriétés du serveur NPS dans la console NPS des événements NPS.

-   **Enregistrement des demandes de gestion des comptes dans un fichier local et de l’authentification des utilisateurs**. À des fins de facturation et principalement utilisés pour l’analyse de la connexion. Également utilisé comme un outil d’enquête de sécurité, car il vous fournit une méthode de suivi de l’activité d’un utilisateur malveillant après une attaque. Vous pouvez configurer la journalisation de fichiers local à l’aide de l’Assistant Configuration de la prise en compte.

-   **Enregistrement des demandes de gestion des comptes à une base de données Microsoft SQL Server compatible XML et de l’authentification des utilisateurs**. Utilisé pour permettre à plusieurs serveurs NPS de disposer d’une source de données en cours d’exécution. Fournit également les avantages de l’utilisation d’une base de données relationnelle. Vous pouvez configurer la journalisation SQL Server à l’aide de l’Assistant Configuration de la prise en compte.

Pour configurer la prise en compte de serveur de stratégie réseau, consultez [Configurer réseau stratégie serveur prise en compte](../../../../../networking/technologies/nps/nps-accounting-configure.md).

### Ajoutez le serveur VPN en tant que Client RADIUS

Dans la section [configurer le serveur d’accès à distance pour toujours actif (AlwaysOn)](vpn-deploy-ras.md) , vous installé et configuré votre serveur VPN. Lors de la configuration du serveur VPN, vous avez ajouté un secret partagé RADIUS sur le serveur VPN. 

Dans cette procédure, vous utilisez la même chaîne de texte secret partagé pour configurer le serveur VPN en tant que client RADIUS dans NPS. Utilisez la même chaîne de texte que vous avez utilisé sur le serveur VPN ou Échec de la communication entre le serveur NPS et le serveur VPN.

>[!IMPORTANT] 
>Lorsque vous ajoutez un nouveau serveur d’accès réseau (VPN serveur, point d’accès sans fil, commutateur d’authentification ou serveur d’accès à distance) à votre réseau, vous devez ajouter le serveur en tant que client RADIUS dans NPS afin que NPS connaît et peut communiquer avec le serveur d’accès réseau.

**Procédure:**

1.  Sur le serveur NPS, dans la console NPS, double-cliquez sur les **Clients et serveurs RADIUS**.

2.  Avec le bouton droit de **Clients RADIUS** , puis cliquez de **Nouveau**. La boîte de dialogue Ajouter un Client RADIUS s’ouvre.

3.  Vérifiez que la case à cocher **Activer ce client RADIUS** est sélectionnée.

4.  Dans la zone **nom convivial**, entrez un nom d’affichage pour le serveur VPN.

5.  Dans la zone **adresse (IP ou DNS)**, entrez l’adresse IP du NAS ou le nom de domaine complet.<p>Si vous entrez le nom de domaine complet, cliquez sur **Vérifier** si vous souhaitez vérifier que le nom est correct et correspond à une adresse IP valide.

6.  Dans la **clé secrète partagée**, effectuez:

    1.  Assurez-vous que l’option **manuelle** est sélectionné.

    2.  Entrez la chaîne de texte forte que vous avez entré également sur le serveur VPN.

    3.  Saisir le secret partagé dans Confirmer le secret partagé.

7.  Cliquez sur **OK**. Le serveur VPN s’affiche dans la liste des clients RADIUS configuré sur le serveur NPS.

## Configurer un serveur NPS en tant qu’un rayon pour les connexions VPN

Dans cette procédure, vous configurez NPS en tant que serveur RADIUS sur le réseau de votre organisation. Sur le serveur NPS, vous devez définir une stratégie qui autorise uniquement les utilisateurs dans un groupe spécifique à accéder au réseau d’entreprise/entreprise via le serveur VPN -, puis uniquement lorsque vous utilisez un certificat d’utilisateur valide dans une demande d’authentification PEAP.

**Procédure:**

1.  Dans la console NPS, dans une Configuration Standard, assurez-vous que le **serveur RADIUS pour l’accès à distance ou des connexions VPN** est activée.

2.  Cliquez sur **configurer VPN ou accès à distance**.<p>L’Assistant Configurer une connexion VPN ou d’accès à distance s’ouvre.

3.  Cliquez sur **les connexions réseau privé virtuel (VPN)**, puis cliquez sur **suivant**.

4.  Spécifier l’accès à distance ou serveur VPN, les clients RADIUS, sélectionnez le nom du serveur VPN que vous avez ajouté à l’étape précédente.<p>Par exemple, si votre nom NetBIOS du serveur VPN est RAS1, cliquez sur **RAS1**.

5.  Cliquez sur **Suivant**.

6.  Dans la configuration des méthodes d’authentification, procédez comme suit:

    1.  Désactivez la case à cocher **Authentification chiffrée Microsoft version 2 (MS-CHAPv2)** .

    2.  Cliquez sur la case à cocher **Extensible Authentication Protocol** pour le sélectionner.

    3.  Dans le Type (en fonction de la méthode de configuration d’accès et le réseau), cliquez sur **Microsoft: PEAP (Protected EAP)**, puis cliquez sur **configurer**.<p>La boîte de dialogue Modifier les propriétés EAP protégé s’ouvre.

    4.  Cliquez sur **Supprimer** pour supprimer le type de protocole EAP de mot de passe sécurisé (EAP-MSCHAP v2).

    5.  Cliquez sur **Ajouter**. La boîte de dialogue Ajouter un EAP s’ouvre.

    6.  Cliquez sur la **carte à puce ou autre certificat**, puis cliquez sur **OK**.

    7.  Cliquez sur **OK** pour fermer modifier les propriétés EAP protégé.

7.  Cliquez sur **Suivant**.

8.  De spécifier des groupes d’utilisateurs, procédez comme suit:

    1.  Cliquez sur **Ajouter**. La boîte de dialogue Sélectionner les utilisateurs, les ordinateurs, les comptes de Service ou groupes s’ouvre.

    2.  **Les utilisateurs VPN** et cliquez sur **OK**.

    3.  Cliquez sur **Suivant**.

9.  Spécifier les filtres de IP, cliquez sur **suivant**.

10. De spécifier les paramètres de chiffrement, cliquez sur **suivant**. Ne mets pas toutes les modifications.<p>Ces paramètres s’appliquent uniquement aux connexions de chiffrement de point à point Microsoft (MPPE), ce scénario ne prend pas en charge.

11. Dans spécifier un nom de domaine, cliquez sur **suivant**.

12. Cliquez sur **Terminer** pour fermer l’Assistant.

## Inscription automatique de certificat du serveur NPS

Dans cette procédure, vous actualisez la stratégie de groupe sur le serveur NPS local manuellement. Lorsque les actualisations de stratégie de groupe, si l’inscription automatique de certificat est configuré et fonctionne correctement, l’ordinateur local est inscrit automatiquement un certificat par l’autorité de certification (CA).

>[!NOTE]  
>La stratégie de groupe actualisées automatiquement lorsque vous redémarrez l’ordinateur membre du domaine, ou quand un utilisateur ouvre une session sur un ordinateur membre du domaine. En outre, la stratégie de groupe actualise régulièrement. Par défaut, cette actualisation périodique se produit toutes les 90 minutes avec un décalage aléatoire de jusqu'à 30 minutes.

L’appartenance à des **administrateurs**, ou l’équivalent, est le minimum requis pour effectuer cette procédure.

**Procédure:**

1.  Sur le serveur NPS, ouvrez Windows PowerShell.

2.  À l’invite Windows PowerShell, tapez **gpupdate**et appuyez sur ENTRÉE.

## Étape suivante
[Étape 5. Configurer les paramètres DNS et le pare-feu pour toujours actif (AlwaysOn)](vpn-deploy-dns-firewall.md): dans cette étape, vous installez le serveur NPS (Network Policy Server) à l’aide de Windows PowerShell ou l’ajouter des rôles serveur gestionnaire et l’Assistant de fonctionnalités. Vous également configurez un serveur NPS pour gérer l’authentification, l’autorisation et les droits de gestion des comptes de connexion toutes les requêtes qu’il reçoit à partir du serveur VPN.



---

