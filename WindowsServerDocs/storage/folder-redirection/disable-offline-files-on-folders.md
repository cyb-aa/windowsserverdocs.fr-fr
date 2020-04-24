---
title: Désactiver la fonctionnalité Fichiers hors connexion sur des dossiers individuels redirigés
description: Guide pratique pour désactiver la mise en cache des fichiers hors connexion sur des dossiers individuels redirigés vers des partages réseau à l’aide de la redirection de dossiers.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: c2614c0180b32a0215454f2d725d6a962986ef1f
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "71394401"
---
# <a name="disable-offline-files-on-individual-redirected-folders"></a>Désactiver la fonctionnalité Fichiers hors connexion sur des dossiers individuels redirigés

>S'applique à : Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows (Canal semi-annuel)

Cette rubrique explique comment désactiver la mise en cache des fichiers hors connexion sur des dossiers individuels redirigés vers des partages réseau à l’aide de la redirection de dossiers. Vous pouvez ainsi spécifier les dossiers à exclure de la mise en cache locale, ce qui réduit la taille du cache des fichiers hors connexion et le temps nécessaire à la synchronisation des fichiers hors connexion.

>[!NOTE]
>Cette rubrique inclut des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [Concepts de base de Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/fundamental/windows-powershell-basics?view=powershell-6).

## <a name="prerequisites"></a>Prérequis

Pour permettre la désactivation de la mise en cache des fichiers hors connexion de dossiers redirigés spécifiques, votre environnement doit répondre aux prérequis suivants.

- Un domaine AD DS (Active Directory Domain Services) avec des ordinateurs clients membres du domaine. Aucune condition n’est requise au niveau fonctionnel de la forêt ou du domaine, ni en ce qui concerne le schéma.
- Ordinateurs clients exécutant Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows (Canal semi-annuel).
- Un ordinateur sur lequel la Gestion des stratégies de groupe est installée.

## <a name="disabling-offline-files-on-individual-redirected-folders"></a>Désactivation de la fonctionnalité Fichiers hors connexion sur des dossiers redirigés individuels

Pour désactiver la mise en cache des fichiers hors connexion de dossiers redirigés spécifiques, utilisez une stratégie de groupe afin d’activer le paramètre de stratégie **Ne pas automatiquement rendre disponibles hors connexion des dossiers redirigés spécifiques** de l’objet GPO (objet de stratégie de groupe) approprié. L’affectation de la valeur **Désactivé** ou **Non configuré** à ce paramètre de stratégie rend tous les dossiers redirigés disponibles hors connexion.

>[!NOTE]
>Seuls les administrateurs de domaine, les administrateurs d’entreprise et les membres du groupe Propriétaires créateurs de la stratégie de groupe peuvent créer des objets GPO.

### <a name="to-disable-offline-files-on-specific-redirected-folders"></a>Pour désactiver la fonctionnalité Fichiers hors connexion sur des dossiers redirigés spécifiques

1. Ouvrez **Gestion des stratégies de groupe**.
2. Pour créer éventuellement un objet GPO qui spécifie les utilisateurs dont les dossiers redirigés ne doivent pas être rendus disponibles hors connexion, cliquez avec le bouton droit sur le domaine ou l’UO (unité d’organisation) de votre choix, puis sélectionnez **Créer un objet GPO dans ce domaine, et le lier ici**.
3. Dans l’arborescence de la console, cliquez avec le bouton droit sur l’objet GPO dont vous souhaitez configurer les paramètres de redirection de dossiers, puis sélectionnez **Modifier**. L’Éditeur de gestion des stratégies de groupe apparaît.
4. Dans l’arborescence de la console, sous **Configuration utilisateur**, développez **Stratégies**, **Modèles d’administration**, **Système**, puis **Redirection de dossiers**.
5. Cliquez avec le bouton droit sur **Ne pas automatiquement rendre disponibles hors connexion des dossiers redirigés spécifiques**, puis sélectionnez **Modifier**. La fenêtre **Ne pas automatiquement rendre disponibles hors connexion des dossiers redirigés spécifiques** s’affiche.
6. Sélectionnez **Activé**. Dans le volet **Options**, sélectionnez les dossiers qui ne doivent pas être rendus disponibles hors connexion, en cochant les cases appropriées. Sélectionnez **OK**.

### <a name="windows-powershell-equivalent-commands"></a>Commandes Windows PowerShell équivalentes

Les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure décrite dans [Désactivation de la fonctionnalité Fichiers hors connexion sur des dossiers redirigés individuels](#disabling-offline-files-on-individual-redirected-folders). Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

Cet exemple crée un objet GPO nommé *Offline Files Settings* dans l’unité d’organisation *MyOu* du domaine *contoso.com* (le nom unique LDAP est « ou=MyOU,dc=contoso,dc=com »). Il désactive ensuite la fonctionnalité Fichiers hors connexion pour le dossier redirigé Vidéos.

```PowerShell
New-GPO -Name "Offline Files Settings" | New-Gplink -Target "ou=MyOu,dc=contoso,dc=com" -LinkEnabled Yes

Set-GPRegistryValue –Name "Offline Files Settings" –Key
"HKCU\Software\Policies\Microsoft\Windows\NetCache\{18989B1D-99B5-455B-841C-AB7C74E4DDFC}" -ValueName DisableFRAdminPinByFolder –Type DWORD –Value 1
```

Consultez le tableau suivant pour voir la liste des noms de clés de Registre (GUID de dossier) à utiliser pour chaque dossier redirigé.

|Dossier redirigé|Nom de clé de Registre (GUID de dossier)|
|---|---|
|AppData(Roaming)|{3EB685DB-65F9-4CF6-A03A-E3EF65729F3D}|
|Desktop (Expérience utilisateur)|{B4BFCC3A-DB2C-424C-B029-7FE99A87C641}|
|Menu Démarrer|{625B53C3-AB48-4EC1-BA1F-A1EF4146FC19}|
|Documents|{FDD39AD0-238F-46AF-ADB4-6C85480369C7}|
|Images|{33E28130-4E1E-4676-835A-98395C3BC3BB}|
|Musique|{4BD8D571-6D19-48D3-BE97-422220080E43}|
|Vidéos|{18989B1D-99B5-455B-841C-AB7C74E4DDFC}|
|Favoris|{1777F761-68AD-4D8A-87BD-30B759FA33DD}|
|Contacts|{56784854-C6CB-462b-8169-88E350ACB882}|
|Téléchargements|{374DE290-123F-4565-9164-39C4925E467B}|
|Liens|{BFB9D5E0-C6A9-404C-B2B2-AE6DB6AF4968}|
|Recherches|{7D1D3A04-DEBB-4115-95CF-2F29DA2920DA}|
|Parties enregistrées|{4C5C32FF-BB9D-43B0-B5B4-2D72E54EAAA4}|

## <a name="more-information"></a>Autres informations

- [Vue d’ensemble de la redirection de dossiers, des fichiers hors connexion et des profils utilisateur itinérants](folder-redirection-rup-overview.md)
- [Déployer la redirection de dossiers avec Fichiers hors connexion](deploy-folder-redirection.md)