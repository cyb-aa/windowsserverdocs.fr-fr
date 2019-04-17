---
title: Désactiver les fichiers hors connexion sur des dossiers redirigés
description: Comment désactiver la mise en cache des fichiers hors connexion sur les dossiers individuels qui sont redirigés vers des partages réseau à l’aide de la Redirection de dossiers.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: adc93906cb7ff958fc1db7b00abdc557623e764e
ms.sourcegitcommit: 9ed4c9fe04ebf3ef488170503c9a354c992b6fde
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4339307"
---
# Désactiver les fichiers hors connexion sur des dossiers redirigés

>S’applique à: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

Cette rubrique explique comment désactiver la mise en cache des fichiers hors connexion sur les dossiers individuels qui sont redirigés vers des partages réseau à l’aide de la Redirection de dossiers. Cela offre la possibilité de spécifier des dossiers à exclure de la mise en cache localement, réduisant ainsi le cache de fichiers hors connexion taille et de temps requis pour synchroniser les fichiers hors connexion.

>[!NOTE]
>Cette rubrique inclut des applets de commande exemple Windows PowerShell que vous pouvez utiliser pour automatiser une partie des procédures décrites. Pour plus d’informations, voir les [Notions de base de Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/fundamental/windows-powershell-basics?view=powershell-6).

## Conditions préalables

Pour désactiver la mise en cache des fichiers hors connexion des dossiers redirigés spécifiques, votre environnement doit satisfaire les conditions préalables suivantes.

- Un domaine Active Directory Domain Services (AD DS), avec les ordinateurs clients joints au domaine. Il n’existe aucune exigences d’au niveau fonctionnel de forêt ou de domaine ou les spécifications de schéma.
- Ordinateurs clients exécutant Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.
- Un ordinateur avec la gestion de la stratégie de groupe installée.

## La désactivation des fichiers hors connexion sur des dossiers redirigés

Pour désactiver la mise en cache des fichiers hors connexion des dossiers redirigés spécifiques, utilisez la stratégie de groupe pour activer le paramètre de stratégie **ne pas rendre automatiquement les dossiers redirigés spécifiques disponibles en mode hors connexion** pour l’objet de stratégie de groupe (GPO) l’approprié. Configurer ce paramètre de stratégie sur **désactivé** ou **Non configuré** rend tous les dossiers redirigés disponibles hors connexion.

>[!NOTE]
>Seuls les administrateurs de domaine, les administrateurs d’entreprise et membres du groupe de propriétaires de créateur de la stratégie de groupe peuvent créer des objets GPO.

### Pour désactiver les fichiers hors connexion sur des dossiers redirigés spécifiques

1. Ouvrez **Gestion des stratégies de groupe**.
2. Pour créer éventuellement un nouvel objet de stratégie de groupe qui spécifie les utilisateurs qui doivent avoir redirigés dossiers exclus d’être rendues accessibles en mode hors connexion, avec le bouton droit de l’unité d’organisation (UO) ou du domaine approprié et ensuite sélectionner **créer un objet GPO dans ce domaine et le lier ici **.
3. Dans l’arborescence de la console, cliquez sur l’objet de stratégie de groupe pour lequel vous souhaitez configurer les paramètres de la Redirection de dossiers, puis sélectionnez **Modifier**. L’éditeur de gestion des stratégies de groupe s’affiche.
4. Dans l’arborescence de la console, sous **Configuration utilisateur**, développez **stratégies**, développez **Modèles d’administration**, développez **système**, puis **La Redirection de dossiers**.
5. Avec le bouton droit de **ne pas rendre automatiquement les dossiers redirigés spécifiques disponibles en mode hors connexion** , puis sélectionnez **Modifier**. La fenêtre de **ne pas rendre automatiquement les dossiers redirigés spécifiques disponibles en mode hors connexion** s’affiche.
6. Sélectionnez **Activé**. Dans le volet **Options** sélectionnez les dossiers qui ne doivent pas être disponibles en mode hors connexion en sélectionnant les cases à cocher. Sélectionnez **OK**.

### Commandes équivalentes de Windows PowerShell

L’applet de commande Windows PowerShell ou les applets de commande suivantes effectuent la même fonction que la procédure décrite dans [La désactivation de fichiers hors connexion sur des dossiers redirigés](#disabling-offline-files-on-individual-redirected-folders). Entrez chaque applet de commande sur une seule ligne, même si elles pourront apparaître renvoyées à travers plusieurs lignes ici en raison des contraintes de mise en forme.

Cet exemple crée un nouvel objet GPO nommé *Paramètres des fichiers hors connexion* dans l’unité d’organisation dans le domaine *contoso.com* *MyOu* (le nom unique LDAP est «unité d’organisation = MyOU, dc = contoso, dc = com»). Ensuite, elle désactive fichiers hors connexion pour le dossier redirigé vers des vidéos.

```PowerShell
New-GPO -Name "Offline Files Settings" | New-Gplink -Target "ou=MyOu,dc=contoso,dc=com" -LinkEnabled Yes

Set-GPRegistryValue –Name "Offline Files Settings" –Key
"HKCU\Software\Policies\Microsoft\Windows\NetCache\{18989B1D-99B5-455B-841C-AB7C74E4DDFC}" -ValueName DisableFRAdminPinByFolder –Type DWORD –Value 1
```

Consultez le tableau suivant pour une liste des noms de clés de Registre (dossier GUID) à utiliser pour chaque dossier redirigé.

|Dossier redirigé|Nom de clé de Registre (dossier GUID)|
|---|---|
|AppData|{3EB685DB-65F9-4CF6-A03A-E3EF65729F3D}|
|Bureau|{B4BFCC3A-DB2C-424C-B029-7FE99A87C641}|
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
|Jeux enregistrés|{4C5C32FF-BB9D-43B0-B5B4-2D72E54EAAA4}|

## Informations supplémentaires

- [Vue d’ensemble de la Redirection de dossiers, fichiers hors connexion et les profils utilisateur itinérants](folder-redirection-rup-overview.md)
- [Déployer la Redirection de dossiers avec des fichiers hors connexion](deploy-folder-redirection.md)