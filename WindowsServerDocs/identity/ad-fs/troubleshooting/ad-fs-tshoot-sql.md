---
title: Résolution des problèmes d’AD FS - connectivité SQL
description: Ce document explique comment résoudre les divers aspects des services AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4b09094b6e305bc85b38e94d11fbc8845d555437
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443934"
---
# <a name="ad-fs-troubleshooting---sql-connectivity"></a>Résolution des problèmes d’AD FS - connectivité SQL
AD FS offre la possibilité d’utiliser SQL Server distant pour les données de la batterie de serveurs AD FS.  Vous verrez des problèmes si les serveurs AD FS dans votre batterie de serveurs ne peuvent pas communiquer avec les serveurs principaux SQL Server.  Le document suivant fournit certaines étapes de base pour tester la communication avec les serveurs principaux.

## <a name="acquire-the-sql-database-connection-string"></a>Obtenir la chaîne de connexion de base de données SQL
La première chose à tester lors de la vérification de connectivité SQL est, si AD FS a les informations de connexion SQL correctes.  Cela est possible à l’aide de PowerShell.

### <a name="to-acquire-the-sql-connection-string"></a>Pour obtenir la chaîne de connexion SQL
1.  Ouvrez Windows PowerShell
2. Entrez les informations suivantes : `$adfs = gwmi -Namespace root/ADFS -Class SecurityTokenService` et appuyez sur entrée
3. Entrez les informations suivantes : `$adfs.ConfigurationDatabaseConnectionString` puis appuyez sur ENTRÉE.
4. Vous devez voir les informations de chaîne de connexion.
![](media/ad-fs-tshoot-sql/sql2.png)

## <a name="create-a-universal-data-link-udl-file-to-test-connectivity"></a>Créez un fichier de lien UDL (Universal Data) pour tester la connectivité
Un fichier UDL ou un fichier UDL est en fait un fichier texte qui contient la chaîne de connexion de base de données.  En utilisant les informations que nous avons obtenue ci-dessus, nous pouvons tester si le serveur SQL server répond aux connexions ou non.

### <a name="to-create-a-udl-file-to-test-connectivity"></a>Pour créer un fichier udl pour tester la connectivité

1. Ouvrez le bloc-notes et enregistrez le fichier sous test.udl.  Assurez-vous que vous avez **tous les fichiers** sélectionné dans la liste déroulante pour **enregistrer en tant que type**.
2. Double-cliquez sur test.udl
3. Renseignez les informations suivantes : un. **Sélectionnez ou entrez un nom de serveur :**  Utilisez la Source de données à partir de la chaîne de connexion ci-dessus b. **Entrez les informations pour vous connecter au serveur :**  Utiliser le compte de service AD FS ou un compte qui dispose des autorisations pour se connecter à distance.  Si le compte est une utilisation du compte windows intégrée authentification sinon entrer le nom d’utilisateur et le mot de passe.
    c. **Sélectionnez la base de données sur le serveur :** Utiliser le catalogue Initial de la chaîne ci-dessus.  Exemple :  AdfsConfigurationV3.
   ![Tester la connexion](media/ad-fs-tshoot-sql/sql4.png)
1. Cliquez sur **tester la connexion**.</br>
![Réussite](media/ad-fs-tshoot-sql/sql3.png)

## <a name="use-sql-server-management-studio-to-test-connectivity"></a>Utilisez SQL Server Management Studio pour tester la connectivité
Vous pouvez également [télécharger](https://go.microsoft.com/fwlink/?linkid=864329) et installation de SSMS pour tester la connectivité de base de données.

### <a name="to-test-connectivity-with-ssms"></a>Pour tester la connectivité avec SSMS
1. Téléchargez et installez SQL Server Management Studio.
![Installer](media/ad-fs-tshoot-sql/sql5.png)
1. Ouvrez SSMS, entrez le nom du serveur.  La source de données à partir du haut.
2. Utiliser le compte de service AD FS ou un compte qui dispose des autorisations pour se connecter à distance.  Si le compte est une utilisation du compte windows intégrée authentification sinon entrer le nom d’utilisateur et le mot de passe.
![Se connecter](media/ad-fs-tshoot-sql/sql6.png)
1. Vous devez voir le côté gauche est renseigné.  Développez bases de données et vérifiez que vous voyez les bases de données AD FS.
![Bases de données AD FS](media/ad-fs-tshoot-sql/sql7.png)

## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes AD FS](ad-fs-tshoot-overview.md)