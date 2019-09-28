---
title: Résolution des problèmes de AD FS-connectivité SQL
description: Ce document décrit comment résoudre les différents aspects de AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 09d61292b91c83466f9770184d431b3e6d627dca
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385437"
---
# <a name="ad-fs-troubleshooting---sql-connectivity"></a>Résolution des problèmes de AD FS-connectivité SQL
AD FS permet d’utiliser des SQL Server distantes pour les données de la batterie de serveurs AD FS.  Vous verrez des problèmes si les serveurs AD FS de votre batterie de serveurs ne peuvent pas communiquer avec les serveurs SQL principaux.  Le document suivant fournit des étapes de base pour tester la communication avec les serveurs principaux.

## <a name="acquire-the-sql-database-connection-string"></a>Acquérir la chaîne de connexion à la base de données SQL
La première chose à tester lors de la vérification de la connectivité SQL est, si AD FS possède les informations de connexion SQL appropriées.  Cette opération peut être effectuée à l’aide de PowerShell.

### <a name="to-acquire-the-sql-connection-string"></a>Pour obtenir la chaîne de connexion SQL
1.  Ouvrir Windows PowerShell
2. Entrez les informations suivantes : `$adfs = gwmi -Namespace root/ADFS -Class SecurityTokenService` et appuyez sur entrée
3. Entrez les informations suivantes : `$adfs.ConfigurationDatabaseConnectionString` et appuyez sur entrée.
4. Vous devez voir les informations de chaîne de connexion.
![](media/ad-fs-tshoot-sql/sql2.png)

## <a name="create-a-universal-data-link-udl-file-to-test-connectivity"></a>Créer un fichier de Universal Data Link (UDL) pour tester la connectivité
Un fichier Universal Data Link ou un fichier UDL est fondamentalement un fichier texte qui contient une chaîne de connexion de base de données.  En utilisant les informations obtenues précédemment, nous pouvons tester si le serveur SQL Server répond aux connexions.

### <a name="to-create-a-udl-file-to-test-connectivity"></a>Pour créer un fichier UDL afin de tester la connectivité

1. Ouvrez le bloc-notes et enregistrez le fichier sous test. UDL.  Assurez-vous que **tous les fichiers** sont sélectionnés dans la liste déroulante pour **type d’enregistrement**.
2. Double-cliquez sur test. udl
3. Renseignez les informations suivantes : a. **Sélectionnez ou entrez un nom de serveur :**  Utilisez la source de données de la chaîne de connexion au-dessus de b. **Entrez les informations pour vous connecter au serveur :**  Utilisez le compte de service AD FS ou un compte disposant des autorisations pour ouvrir une session à distance.  Si le compte est un compte Windows, utilisez l’authentification intégrée. sinon, entrez le nom d’utilisateur et le mot de passe.
    c. **Sélectionnez la base de données sur le serveur :** Utilisez le catalogue initial de la chaîne ci-dessus.  Exemple :  AdfsConfigurationV3.
   ![Test connexion @ no__t-1
1. Cliquez sur **tester la connexion**.</br>
![Success @ no__t-1

## <a name="use-sql-server-management-studio-to-test-connectivity"></a>Utiliser SQL Server Management Studio pour tester la connectivité
Vous pouvez également [Télécharger](https://go.microsoft.com/fwlink/?linkid=864329) et installer SSMS pour tester la connectivité de la base de données.

### <a name="to-test-connectivity-with-ssms"></a>Pour tester la connectivité avec SSMS
1. Téléchargez et installez SQL Server Management Studio.
![Installer](media/ad-fs-tshoot-sql/sql5.png)
1. Ouvrez SSMS, puis entrez le nom du serveur.  Source de données ci-dessus.
2. Utilisez le compte de service AD FS ou un compte disposant des autorisations pour ouvrir une session à distance.  Si le compte est un compte Windows, utilisez l’authentification intégrée. sinon, entrez le nom d’utilisateur et le mot de passe.
![Connect @ no__t-1
1. Vous devez voir le côté gauche rempli.  Développez bases de données et vérifiez que vous voyez les bases de données AD FS.
bases de données @no__t 0AD FS @ no__t-1

## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes AD FS](ad-fs-tshoot-overview.md)