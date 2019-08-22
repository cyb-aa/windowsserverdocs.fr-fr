---
title: Créer des articles Windows Server à l’aide de GitHub et Visual Studio Code
description: Comment créer de nouveaux articles relatifs à Windows Server, à l’aide de GitHub et Visual Studio Code, en tant qu’employé Microsoft.
author: eross-msft
ms.author: lizross
ms.date: 05/02/2019
ms.openlocfilehash: f5e7e3d0cd17c64175fddaaac73c12daa2c2a32c
ms.sourcegitcommit: ffd9c42374c7448deb5f53f7a865cb427b5e4e9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69887950"
---
# <a name="create-new-windows-server-articles-using-github-and-visual-studio-code"></a>Créer des articles Windows Server à l’aide de GitHub et Visual Studio Code

Il existe deux emplacements distincts où nous conservons le contenu technique de Windows Server. L’un des emplacements est public (windowsserverdocs) tandis que l’autre est privé (windowsserverdocs-PR). Qui détermine à quel emplacement vous contribuez:

- **Je suis un employé de Microsoft.** En tant qu’employé Microsoft, vous disposez d’options, en fonction de ce que vous essayez de faire:

    - **Créez un article de toute nouvelle appellation.** Pour créer un article de toute nouvelle appellation, vous devez créer et configurer votre compte et vos outils GitHub, brancher et cloner le référentiel windowsserverdocs-PR, configurer votre branche distante, créer l’article et enfin créer une nouvelle requête de tirage pour l’approbation et la publication. Pour obtenir ces instructions, poursuivez la lecture de cet article.

    - **Apportez des modifications importantes à un article existant.** Pour apporter des modifications substantielles à un article existant, vous pouvez suivre les instructions de l’article [modifier un article de serveur Windows existant à l’aide de GitHub et Visual Studio code](edit-existing-using-github.md) .

    - **Apporter des modifications mineures à un article existant.** Pour apporter des modifications mineures à un article existant, vous pouvez suivre les instructions de la rubrique [mettre à jour les articles Windows Server existants à l’aide d’un navigateur Web et](github-browser-updates.md) de l’article github.

- **Je ne suis pas un employé de Microsoft.** En tant qu’employé non-Microsoft, vous devez contribuer à l’emplacement public. Pour plus d’informations sur la procédure à suivre, consultez l’article [contribution à la documentation technique de Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md) .

## <a name="prerequisites"></a>Prérequis

Avant de commencer à travailler dans le référentiel, vous devez créer et configurer votre compte GitHub, configurer la vérification à deux facteurs et installer et configurer tous les outils nécessaires. Si vous avez déjà effectué cette opération, vous pouvez passer à la [section duplication du référentiel](#fork-the-repository) de cet article.

1. [Créer un compte et un profil GitHub](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#create-a-github-account-and-set-up-your-profile)

2. [Lier votre compte à votre compte Microsoft et aux organisations Microsoft et MicrosoftDocs](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#link-your-github-and-microsoft-accounts)

3. [Activer la vérification à deux facteurs](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#enable-two-factor-authentication-and-create-an-access-token)

4. [Autoriser le système de génération à accéder à votre compte GitHub](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#authorize-the-ops-build-system-to-access-your-github-account)

5. [Installer Visual Studio Code](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-visual-studio-code)

6. [Installer GitHub et ses outils](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-git-client-tools)

7. [Installer docs Authoring Pack](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-the-docs-authoring-pack)

## <a name="set-up-your-own-version-of-the-repo"></a>Configurer votre propre version du référentiel

Une fois que vous avez créé et configuré votre compte et vos outils GitHub, vous pouvez créer une version personnelle de votre référentiel. C’est là que vous allez créer vos branches et effectuer toutes vos modifications.

### <a name="fork-the-repository"></a>Répliquer le dépôt

Si vous avez besoin d’une copie locale des fichiers sources, vous pouvez créer des demandes de tirage (pull) à partir de votre fourche vers le référentiel de production.

#### <a name="to-fork-the-repository"></a>Pour répliquer le dépôt

1. Connectez-vous à votre compte GitHub et accédez https://github.com/microsoftdocs/windowsserverdocs-pr à.

2. Sélectionnez **fourche**.

    ![Bouton de fourche mis en surbrillance sur la page](media/create-new-using-github/fork-button.png)

3. Sélectionnez votre compte GitHub comme emplacement de la fourche.

    ![Bouton de fourche mis en surbrillance sur la page](media/create-new-using-github/fork-location.png)

### <a name="clone-the-repository"></a>Cloner le dépôt

Vous devez cloner le référentiel obtenir une copie locale du référentiel sur votre appareil local.

#### <a name="to-clone-the-repository"></a>Pour cloner le dépôt

1. Accédez à https://github.com/settings/developers , puis sélectionnez **jetons d’accès personnels** dans le volet gauche.

2. Sélectionnez **générer un nouveau jeton**, attribuez un nom unique et explicite à votre jeton, sélectionnez toutes les étendues disponibles, puis sélectionnez **générer le jeton**.

3. Copiez le jeton et mettez-le dans un endroit sûr. Vous en aurez besoin pour le reste du processus et, une fois que vous aurez quitté la page, vous ne pourrez plus y revenir.

4. Ouvrez une commande git bash et remplacez les répertoires par l’emplacement où vous souhaitez stocker vos référentiel. Nous vous recommandons d' `C:\users\<your_name>\GitHub`utiliser,.

5. Tapez les commandes suivantes à l’aide de vos propres informations, une à la fois, pour cloner vos référentiel et configurer vos branches distantes:

    ```markdown

    git clone https://<your_github_username>:<your_personal_access_token>@github.com/<your_github_username>/windowsserverdocs-pr.git

    cd windowsserverdocs-pr

    git remote add upstream https://<your_github_username>:<your_personal_access_token>@github.com/MicrosoftDocs/windowsserverdocs-pr.git

    git fetch upstream master
    ```

6. Exécutez cette commande pour vous assurer que votre télécommande est correctement configurée:

    `git remote -v`

7. Vous devez voir un résultat semblable à ce qui suit:

    ```markdown
    $ git remote -v

    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (fetch)
    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (push)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (fetch)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (push)
    ```

    Si votre sortie à distance ne ressemble pas à ceci, vous pouvez réessayer en `git remote remove upstream`exécutant d’abord.

## <a name="create-a-branch-and-a-new-article"></a>Créer une branche et un nouvel article

Pour créer un article, procédez comme suit.

### <a name="create-a-new-branch-and-a-new-file"></a>Créer une nouvelle branche et un nouveau fichier

Avant de pouvoir commencer à travailler sur votre contenu, vous devez créer une nouvelle branche dans votre référentiel local.

#### <a name="to-create-a-new-branch-in-git-bash"></a>Pour créer une nouvelle branche dans Git bash

- Ouvrez git bash et tapez les commandes (une à la fois):

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >Nous vous recommandons vivement d’attribuer à votre branche un nom évident et unique, afin de pouvoir la retrouver plus tard.

    Une fois les commandes terminées, vous êtes dans votre nouvelle branche et vous êtes prêt à créer votre nouveau fichier. Vous devez uniquement changer le windowsserverdocs-PR référentiel une seule fois par instance de votre bash git. Si vous fermez le bash git, vous devrez réactiver les répertoires après l’avoir ouvert.

#### <a name="to-create-a-new-file-in-your-branch"></a>Pour créer un nouveau fichier dans votre branche

1. Ouvrez l’Explorateur Windows, accédez à votre répertoire GitHub et créez un nouveau fichier texte dans la structure de dossiers. Le nom de votre fichier doit être en minuscules et délimité par des traits d’Union. Par exemple, _What-is-Windows-Server.MD_.

     Vous devez également remplacer l’extension de fichier. txt par. MD. Dans le message d’erreur qui s’affiche, sélectionnez **Oui** pour enregistrer le fichier avec la nouvelle extension de fichier.

2. Ouvrez Visual Studio Code et accédez à **fichier**, sélectionnez **ouvrir le dossier**, puis accédez à l’emplacement github du fichier que vous avez créé à l’étape 1.

3. Dans le volet **Explorateur** , sélectionnez votre nouveau fichier.

4. Ajoutez votre texte à la page. Si vous créez un article, veillez [à ajouter les balises de métadonnées nécessaires à votre article sur Windows Server](metadata-requirements-for-articles.md).

5. Sélectionnez **fichier** > **Enregistrer**.

### <a name="preview-your-text"></a>Afficher un aperçu de votre texte

Après avoir ajouté votre texte à votre nouveau fichier, vous devez afficher un aperçu de vos modifications pour vous assurer qu’elles s’affichent correctement.

#### <a name="to-preview-your-text"></a>Pour afficher un aperçu de votre texte

1. Dans Visual Studio Code, sélectionnez l’un des boutons d' **Aperçu** dans le coin supérieur droit.

    ![icône du bouton Aperçu](media/create-new-using-github/preview-button-full-page.png): Bascule vers une version préliminaire complète de votre contenu.

    ![icône du bouton Aperçu](media/create-new-using-github/preview-button-side-by-side.png): Ouvre la page d’aperçu en regard de votre page de travail, côte à côte.

2. Assurez-vous que votre article a l’aspect souhaité.

    Une fois que vous êtes certain qu’il semble correct, vous pouvez valider vos modifications et créer une demande de tirage (pull request) pour la publication.

### <a name="commit-your-changes"></a>Valider vos modifications

Après vous être assuré que votre texte semble correct, vous pouvez valider vos modifications dans votre version locale de votre référentiel.

#### <a name="to-commit-your-changes"></a>Pour valider vos modifications

- Ouvrez git bash et tapez les commandes (une à la fois, en supprimant les balises FACULTATIVEs):

    ```markdown
    OPTIONAL: git status

    git add .

    git commit -m “public comment about what change is”

    OPTIONAL: git pull upstream master

    git push origin <name_of_your_new_branch>

    ```

    La commande facultative git status indique les fichiers qui ont été modifiés dans le cadre de cette validation. Le maître d’extraction git facultatif extrait les dernières modifications de contenu à partir de la branche maître MicrosoftDocs, en synchronisant votre contenu local avec le contenu maître principal. Cela vous permet de vous montrer tous les conflits de fusion potentiels à l’avance, afin de pouvoir les corriger avant que vous ne passiez à l’étape PR.

### <a name="submit-a-pull-request-for-review-and-publication"></a>Envoyer une demande de tirage (pull request) pour révision et publication

Une fois que vous avez terminé votre article, vous devez obtenir l’approbation de votre enregistreur (attendez un certain temps) pour la publication.

#### <a name="to-submit-your-pull-request"></a>Pour envoyer votre demande de tirage (pull request)

1. Accédez à https://github.com/MicrosoftDocs/windowsserverdocs-pr et sélectionnez l’onglet **requêtes de tirage** .

2. Dans la zone réviseurs du volet droit, sélectionnez l’icône d’engrenage, puis entrez l’alias _windowsservercontent_ pour la révision.

    Un membre de l’alias _windowsservercontent_ examine vos modifications ou ajoute des commentaires sur les éléments qui doivent être modifiés avant que la fusion puisse se produire.

3. Tapez **#sign** les commentaires afin que les réviseurs sachent que vous êtes en train de vous remettre pour la révision et la publication. Commentaire **#sign** :

    - Met à jour l’étiquette de votre requête de tirage à partir de **do-not-Merge** vers Ready pour la **fusion**.

    - Permet à l’alias et aux enregistreurs de savoir que vous êtes prêt à consulter votre contenu.

    - Permet aux administrateurs de savoir qu’après approbation, votre contenu est prêt à être en ligne.

    >[!Important]
    >Une fois que vous avez ajouté le commentaire #sign, un membre de l’équipe windowsservercontent passe en revue le texte et l’envoie à Master pour qu’il passe à la prochaine publication planifiée en direct (10:30 et 3 30 jours ouvrables).
    >
    >Si vous n’ajoutez pas de #sign en tant que commentaire final à votre demande de tirage, votre contenu restera dans la file d’attente sans être transféré vers le maître et finalement en direct.

## <a name="related-information"></a>Informations connexes

Pour plus d’informations sur GitHub et le langage de démarque, consultez:

### <a name="git-concepts"></a>Concepts git

- [Guides GitHub-introduction au manuel git](https://guides.github.com/introduction/git-handbook/)

- [Guides GitHub-embranchement de projets](https://guides.github.com/activities/forking/)

- [Guides GitHub-comprendre le déroulement du GitHub](https://guides.github.com/introduction/flow/)

- [Découvrir la branche git] (https://learngitbranching.js.org/ (Idéal pour les apprenants visuels!))

### <a name="markdown"></a>Markdown

- [Notre guide de démarque interne](https://review.docs.microsoft.com/help/contribute/markdown-reference?branch=master)

- [Didacticiel externe, GitHub](https://www.markdowntutorial.com/)
