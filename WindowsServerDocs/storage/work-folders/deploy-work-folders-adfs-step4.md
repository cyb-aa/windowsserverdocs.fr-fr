---
title: 'Déployer Dossiers de travail avec AD FS et le proxy d’application Web : Étape 4 : Configurer le proxy d’application Web'
ms.prod: windows-server
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 6/24/2017
ms.assetid: 4a11ede0-b000-4188-8190-790971504e17
ms.openlocfilehash: 17adf89d3a26767bbc736a31da7b7b2b204570a2
ms.sourcegitcommit: e817a130c2ed9caaddd1def1b2edac0c798a6aa2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74945239"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-4-set-up-web-application-proxy"></a>Déployer Dossiers de travail avec AD FS et le proxy d’application Web : étape 4, configurer le proxy d’application Web

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit la quatrième étape du déploiement de Dossiers de travail avec les services de fédération Active Directory (AD FS) et le proxy d’application Web. Vous pouvez trouver les autres étapes de ce processus dans ces rubriques :  
  
-   [Déployer des dossiers de travail avec AD FS et le proxy d’application Web : vue d’ensemble](deploy-work-folders-adfs-overview.md)  
  
-   [Déployer des dossiers de travail avec AD FS et le proxy d’application Web : étape 1, configurer AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [Déployer des dossiers de travail avec des AD FS et un proxy d’application Web : étape 2, AD FS le travail après configuration](deploy-work-folders-adfs-step2.md)  
  
-   [Déployer des dossiers de travail avec des AD FS et un proxy d’application Web : étape 3, configuration de dossiers de travail](deploy-work-folders-adfs-step3.md)  
  
-   [Déployer des dossiers de travail avec AD FS et le proxy d’application Web : étape 5, configurer des clients](deploy-work-folders-adfs-step5.md)  

> [!NOTE]
>   Les instructions décrites dans cette section concernent un environnement Windows Server 2019 ou Windows Server 2016. Si vous utilisez Windows Server 2012 R2, suivez les [instructions pour Windows Server 2012 R2](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx).

Pour configurer le proxy d’application Web pour l’utiliser avec Dossiers de travail, utilisez les procédures suivantes.  
  
## <a name="install-the-ad-fs-and-work-folder-certificates"></a>Installer les certificats AD FS et Dossiers de travail  
Vous devez installer les certificats AD FS et Dossiers de travail que vous avez créés précédemment (à l’étape 1, Configurer AD FS, et à l’étape 3, Configurer Dossiers de travail) dans le magasin de certificats de l’ordinateur local sur l’ordinateur où le rôle Proxy d’application Web sera installé.  
  
Étant donné que vous installez des certificats auto-signés qui ne permettent pas que l’on remonte à un éditeur dans le magasin de certificats Autorités de certification racines de confiance, vous devez également copier les certificats vers ce magasin.  
  
Pour installer les certificats, procédez comme suit :  
  
1.  Cliquez sur **Démarrer**, puis sur **Exécuter**.  
  
2.  Tapez **MMC**.  
  
3.  Dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**.  
  
4.  Dans la zone **Composants logiciels enfichables disponibles**, cliquez sur **Certificats**, puis sur **Ajouter**. L’assistant Composant logiciel enfichable Certificats démarre.  
  
5.  Sélectionnez **Un compte d’ordinateur**, puis cliquez sur **Suivant**.  
  
6.  Sélectionnez **Ordinateur local (l’ordinateur sur lequel cette console s’exécute)** , puis cliquez sur **Terminer**.  
  
7.  Cliquez sur **OK**.  
  
8.  Développez le dossier **Racine de la console\Certificats\(Ordinateur local)\Personnel\Certificats**.  
  
9. Cliquez avec le bouton droit sur **Certificats**, cliquez sur **Toutes les tâches**, puis cliquez sur **Importer**.  
  
10. Accédez au dossier contenant le certificat AD FS et suivez les instructions de l’Assistant pour importer le fichier et le placer dans le magasin de certificats.  
  
11. Répétez les étapes 9 et 10, cette fois en accédant au certificat de dossiers de travail et en l’important.  
  
12. Développez le dossier **Racine de la console\Certificats\(Ordinateur local)\Autorités de certification racines de confiance\Certificats**.  
  
13. Cliquez avec le bouton droit sur **Certificats**, cliquez sur **Toutes les tâches**, puis cliquez sur **Importer**.  
  
14. Accédez au dossier contenant le certificat AD FS, puis suivez les instructions de l’assistant pour importer le fichier et le placer dans le magasin des autorités de certification racines de confiance.  
  
15. Répétez les étapes 13 et 14, cette fois en accédant au certificat Dossiers de travail et en l’important.  
  
## <a name="install-web-application-proxy"></a>Installer le proxy d’application Web  
Pour installer le proxy d’application Web, procédez comme suit :  
  
1.  Sur le serveur où vous prévoyez d’installer le proxy d’application Web, ouvrez **Gestionnaire de serveur** et démarrez l’Assistant **Ajout de rôles et fonctionnalités**.  
  
2.  Cliquez sur **Suivant** sur les première et deuxième pages de l’Assistant.  
  
3.  Dans la page **Sélection du serveur**, sélectionnez votre serveur, puis cliquez sur **Suivant**.  
  
4.  Dans la page **Rôle serveur**, sélectionnez le rôle **Accès à distance**, puis cliquez sur **Suivant**.  
  
5.  Dans la page Fonctionnalités et dans la page Accès à distance, cliquez sur **Suivant**.  
  
6.  Dans la page **Services de rôle**, sélectionnez **Proxy d’application web**, cliquez sur **Ajouter des fonctionnalités**, puis cliquez sur **Suivant**.

7.  Dans la page **Confirmer les sélections d’installation** , cliquez sur **Installer**.  
  
## <a name="configure-web-application-proxy"></a>Configurer le proxy d’application Web  
Pour configurer le proxy d’application Web, procédez comme suit :  
  
1.  Cliquez sur l’indicateur d’avertissement en haut du Gestionnaire de serveur, puis cliquez sur le lien pour ouvrir l’Assistant Configuration du proxy d’application Web.  
  
2.  Dans la page Bienvenue, cliquez sur **Suivant**.  
  
3.  Dans la page **Serveur de fédération**, entrez le nom du service de fédération. Dans l’exemple de test, il s’agit de **blueadfs.contoso.com**.  
  
4.  Entrez les informations d’identification d’un compte d’administrateur local sur les serveurs de fédération. N’entrez pas les informations d’identification de domaine (par exemple, contoso\administrateur), mais les informations d’identification locales (par exemple, administrateur).  
  
5.  Dans la page **Certificats de proxy AD FS**, sélectionnez le certificat AD FS que vous avez importé précédemment. Dans l’exemple de test, il s’agit de **blueadfs.contoso.com**. Cliquez sur **Suivant**.  
  
6.  La page de confirmation affiche la commande Windows PowerShell qui s’exécutera pour configurer le service. Cliquez sur **Configurer**.  
  
## <a name="publish-the-work-folders-web-application"></a>Publier l’application web Dossiers de travail  
L’étape suivante consiste à publier une application web qui rendra Dossiers de travail disponibles pour les clients. Pour publier l’application web Dossiers de travail, procédez comme suit :  
  
1. Ouvrez **Gestionnaire de serveur**, puis dans le menu **Outils**, cliquez sur **Gestion de l’accès à distance** pour ouvrir la Console de gestion de l’accès à distance.  
  
2. Sous **Configuration**, cliquez sur **Proxy d’application Web**.  
  
3. Sous **Tâches**, cliquez sur **Publier**. L’Assistant Publication d’une nouvelle application s’ouvre.  
  
4. Dans la page Bienvenue, cliquez sur **Suivant**.  
  
5. Dans la page **Pré-authentification**, sélectionnez **Services de fédération Active Directory (AD FS)** , puis cliquez sur **Suivant**.  
  
6. Dans la page **Support Clients**, sélectionnez **OAuth2**, puis cliquez sur **Suivant**.

7. Dans la page **Partie de confiance**, sélectionnez **Dossiers de travail**, puis cliquez sur **Suivant**. Cette liste est publiée sur le proxy d’application Web à partir des services AD FS.  
  
8. Dans la page **Paramètres de publication**, entrez les éléments suivants, puis cliquez sur **Suivant** :  
  
   -   Le nom que vous souhaitez utiliser pour l’application web  
  
   -   L’URL externe pour Dossiers de travail  
  
   -   Le nom du certificat Dossiers de travail  
  
   -   L’URL de serveur principal pour Dossiers de travail  
  
   Par défaut, l’URL de serveur principal est identique à l’URL externe.  
  
   Dans l’exemple de test, utilisez ces valeurs :  
  
   Nom : **WorkFolders**  
  
   URL externe : **https://workfolders.contoso.com**  
  
   Certificat externe : **Le certificat Dossiers de travail que vous avez installé précédemment**  
  
   URL du serveur principal : **https://workfolders.contoso.com**  
  
9. La page de confirmation indique la commande Windows PowerShell qui s’exécutera pour publier l’application. Cliquez sur **Publier**.  
  
10. Dans la page **Résultats**, vous devez voir que l’application a été correctement publiée.
    >[!NOTE]
    > Si vous avez plusieurs serveurs Dossiers de travail, vous devez publier une application web Dossiers de travail pour chaque serveur Dossiers de travail (Répétez les étapes 1 à 10).  
  
Étape suivante : [Déployer Dossiers de travail avec AD FS et le proxy d’application Web : Étape 5, Configurer les clients](deploy-work-folders-adfs-step5.md)  
  
## <a name="see-also"></a>Articles associés  
[Vue d’ensemble des dossiers de travail](Work-Folders-Overview.md)  
  

