---
title: Désactiver les fichiers hors connexion sur des dossiers redirigés individuels
description: Comment désactiver la mise en cache des fichiers hors connexion sur des dossiers individuels qui sont redirigées vers les partages réseau à l’aide de la Redirection de dossiers.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: b006742c9256c357d9aff3fb1b765dbed087383a
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475881"
---
# <a name="disable-offline-files-on-individual-redirected-folders"></a>Désactiver les fichiers hors connexion sur des dossiers redirigés individuels

>S’applique à : Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows (canal semi-annuel)

Cette rubrique décrit comment désactiver la mise en cache des fichiers hors connexion sur des dossiers individuels qui sont redirigées vers les partages réseau à l’aide de la Redirection de dossiers. Cela permet de spécifier les dossiers à exclure de la mise en cache localement, ce qui réduit le cache de fichiers hors connexion, taille et le temps nécessaire pour synchroniser des fichiers hors connexion.

>[!NOTE]
>Cette rubrique inclut des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [principes fondamentaux de Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/fundamental/windows-powershell-basics?view=powershell-6).

## <a name="prerequisites"></a>Prérequis

Pour désactiver la mise en cache des fichiers hors connexion des dossiers redirigés spécifiques, votre environnement doit respecter les conditions préalables suivantes.

- Un domaine de Services de domaine Active Directory (AD DS), avec les ordinateurs clients joints au domaine. Il n’existe aucune exigences de niveau fonctionnel de forêt ou domaine ou les spécifications de schéma.
- Ordinateurs clients exécutant Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows (canal semi-annuel).
- Un ordinateur avec la gestion des stratégies de groupe installée.

## <a name="disabling-offline-files-on-individual-redirected-folders"></a>La désactivation de la fonctionnalité fichiers hors connexion sur des dossiers redirigés

Pour désactiver la mise en cache des fichiers hors connexion des dossiers redirigés spécifiques, utilisez la stratégie de groupe pour activer la **ne pas rendre automatiquement des dossiers redirigés disponibles hors connexion** le paramètre de stratégie pour l’objet de stratégie de groupe (GPO) l’approprié. Configuration de ce paramètre de stratégie à **désactivé** ou **pas configuré** rend tous les dossiers redirigés disponibles hors connexion.

>[!NOTE]
>Uniquement les administrateurs de domaine, les administrateurs d’entreprise et les membres de la stratégie de groupe Propriétaires créateurs de groupe peuvent créer des GPO.

### <a name="to-disable-offline-files-on-specific-redirected-folders"></a>Pour désactiver les fichiers hors connexion sur des dossiers redirigés spécifiques

1. Ouvrez **gestion de stratégie de groupe**.
2. Pour éventuellement créer un nouvel objet GPO qui spécifie les utilisateurs qui doivent avoir redirigé dossiers exclus de la mise à disposition hors connexion, cliquez sur le domaine approprié ou l’unité d’organisation (UO), puis sélectionnez **créer un objet GPO dans ce domaine et le lier informatique ici**.
3. Dans l’arborescence de la console, cliquez sur l’objet de stratégie de groupe pour lequel vous souhaitez configurer les paramètres de la Redirection de dossiers, puis sélectionnez **modifier**. L’éditeur de gestion de stratégie de groupe s’affiche.
4. Dans l’arborescence de la console, sous **Configuration utilisateur**, développez **stratégies**, développez **modèles d’administration**, développez **système**, et Développez **la Redirection de dossiers**.
5. Avec le bouton droit **ne pas rendre automatiquement des dossiers redirigés disponibles hors connexion** , puis sélectionnez **modifier**. Le **ne pas rendre automatiquement des dossiers redirigés disponibles hors connexion** fenêtre s’affiche.
6. Sélectionnez **Activé**. Dans le **Options** volet Sélectionner les dossiers qui ne doivent pas être disponibles hors connexion en sélectionnant les cases à cocher appropriées. Sélectionnez **OK**.

### <a name="windows-powershell-equivalent-commands"></a>Commandes Windows PowerShell équivalentes

L’applet de commande Windows PowerShell ou les applets de commande suivantes exécutent la même fonction que la procédure décrite dans [la désactivation de fichiers hors connexion sur des dossiers redirigés](#disabling-offline-files-on-individual-redirected-folders). Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

Cet exemple crée un nouvel objet GPO nommé *paramètres fichiers hors connexion* dans le *MyOu* unité d’organisation dans le *contoso.com* domaine (le nom unique LDAP est « unité d’organisation = MyOU, dc = Contoso, dc = com »). Ensuite, elle désactive des fichiers hors connexion pour le dossier redirigés de vidéos.

```PowerShell
New-GPO -Name "Offline Files Settings" | New-Gplink -Target "ou=MyOu,dc=contoso,dc=com" -LinkEnabled Yes

Set-GPRegistryValue –Name "Offline Files Settings" –Key
"HKCU\Software\Policies\Microsoft\Windows\NetCache\{18989B1D-99B5-455B-841C-AB7C74E4DDFC}" -ValueName DisableFRAdminPinByFolder –Type DWORD –Value 1
```

Consultez le tableau suivant pour obtenir la liste de noms de clé de Registre (GUID de dossier) à utiliser pour chaque dossier redirigé.

|Dossier redirigé|Nom de clé de Registre (GUID du dossier)|
|---|---|
|AppData (Roaming)|{3EB685DB-65F9-4CF6-A03A-E3EF65729F3D}|
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
|Parties enregistrées|{4C5C32FF-BB9D-43B0-B5B4-2D72E54EAAA4}|

## <a name="more-information"></a>Informations supplémentaires

- [Vue d’ensemble de la Redirection de dossiers, fichiers hors connexion et les profils utilisateur itinérants](folder-redirection-rup-overview.md)
- [Déployer la Redirection de dossiers des fichiers hors connexion](deploy-folder-redirection.md)