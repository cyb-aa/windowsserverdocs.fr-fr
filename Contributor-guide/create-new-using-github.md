---
title: Créer de nouveaux articles de Windows Server à l’aide de GitHub et Visual Studio Code
description: Comment créer de nouveaux articles liés à Windows Server, à l’aide de GitHub et Visual Studio Code, comme un employé de Microsoft.
author: eross-msft
ms.author: lizross
ms.date: 05/02/2019
ms.openlocfilehash: d75bf135266a4783ab2617977b344782cea679ef
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624562"
---
# <a name="create-new-windows-server-articles-using-github-and-visual-studio-code"></a>Créer de nouveaux articles de Windows Server à l’aide de GitHub et Visual Studio Code

Il existe deux emplacements distincts, où nous conservons le contenu technique de Windows Server. Un des emplacements est public (windowsserverdocs), tandis que l’autre est privée (windowsserverdocs-pr). Qui vous êtes détermine à quel emplacement vous contribuer à :

- **Je suis un employé de Microsoft.** Un employé de Microsoft, vous disposez des options, selon ce que vous essayez d’effectuer :

    - **Créer un tout nouvel article.** Pour créer un tout nouvel article, vous devez créer et configurer votre compte GitHub et les outils, répliquer et cloner le dépôt windowsserverdocs-pr, configurer votre branche distante, créer l’article et enfin créer une nouvelle demande de tirage pour approbation et la publication. Pour ces instructions, poursuivez la lecture de cet article.

    - **Apporter des modifications importantes à un article existant.** Pour apporter des modifications importantes à un article existant, vous pouvez suivre les instructions dans le [modifier un article existant de Windows Server à l’aide de GitHub et Visual Studio Code](edit-existing-using-github.md) article.

    - **Apporter des modifications mineures à un article existant.** Pour apporter des modifications mineures à un article existant, vous pouvez suivre les instructions dans le [mettre à jour des articles de Windows Server existants à l’aide d’un navigateur web et le GitHub](github-browser-updates.md) article.

- **Je ne suis pas un employé de Microsoft.** En tant qu’un employé non Microsoft, vous devez contribuer à l’emplacement public. Pour savoir comment procéder, consultez la [contribuant à la documentation technique de Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md) article.

## <a name="prerequisites"></a>Prérequis

Avant de pouvoir commencer à travailler dans le référentiel, vous devez créer et configurer votre compte GitHub, configurer la vérification à deux facteurs et installer et configurer tous les outils nécessaires. Si vous avez déjà fait, vous pouvez ignorer jusqu'à la [dupliquer (fork) de la section de référentiel](#fork-the-repository) de cet article.

1. [Créer un compte GitHub et un profil](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#create-a-github-account-and-set-up-your-profile)

2. [Liez votre compte à votre compte Microsoft et pour les entreprises de Microsoft et MicrosoftDocs](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#link-your-github-and-microsoft-accounts)

3. [Activer la vérification de deux facteurs](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#enable-two-factor-authentication-and-create-an-access-token)

4. [Autoriser le système de génération pour accéder à votre compte GitHub](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#authorize-the-ops-build-system-to-access-your-github-account)

5. [Installer Visual Studio Code](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-visual-studio-code)

6. [Installez GitHub et ses outils](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-git-client-tools)

7. [Installer Docs Authoring Pack](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-the-docs-authoring-pack)

## <a name="set-up-your-own-version-of-the-repo"></a>Configurer votre propre version du référentiel

Une fois que vous avez créé et configuré votre compte GitHub et les outils, vous pouvez créer une version personnelle de votre référentiel. Voici où vous créerez vos branches et effectuer toutes vos modifications.

### <a name="fork-the-repository"></a>Répliquer le dépôt

Vous avez besoin d’une copie locale des fichiers source, afin de créer des requêtes d’extraction à partir de votre branchement dans le référentiel de production.

#### <a name="to-fork-the-repository"></a>Dupliquer le référentiel

1. Connectez-vous à votre compte GitHub et accédez à https://github.com/microsoftdocs/windowsserverdocs-pr.

2. Sélectionnez **branchement**.

    ![Bouton de branchement mis en surbrillance sur la page](media/create-new-using-github/fork-button.png)

3. Sélectionnez votre compte GitHub en tant que l’emplacement de branche.

    ![Bouton de branchement mis en surbrillance sur la page](media/create-new-using-github/fork-location.png)

### <a name="clone-the-repository"></a>Cloner le dépôt

Vous devez cloner la get référentiel une copie locale du référentiel sur votre appareil local.

#### <a name="to-clone-the-repository"></a>Pour cloner le référentiel

1. Accédez à https://github.com/settings/developers, puis sélectionnez **jetons d’accès personnels** dans le volet gauche.

2. Sélectionnez **générer un nouveau jeton**, nommez votre jeton explicites et uniques, sélectionnez toutes les étendues disponibles, puis **générer un jeton**.

3. Copiez le jeton et le placer dans un endroit sûr. Vous en aurez besoin pour le reste du processus et une fois que vous quittez la page, vous ne pourrez pas accéder à ces informations.

4. Ouvrez une commande Git Bash et accédez au répertoire où vous souhaitez stocker votre référentiel. Nous vous recommandons d’utiliser, `C:\users\<your_name>\GitHub`.

5. Tapez les commandes suivantes à l’aide des informations spécifiques à votre, un à la fois, à cloner votre référentiel et configurer vos branches à distance :

    ```markdown

    git clone https://<your_github_username>:<your_personal_access_token>@github.com/<your_github_username>/windowsserverdocs-pr.git

    cd windowsserverdocs-pr

    git remote add upstream https://<your_github_username>:<your_personal_access_token>@github.com/MicrosoftDocs/windowsserverdocs-pr.git

    git fetch upstream master
    ```

6. Exécutez la commande suivante pour vérifier votre à distance est correctement configuré :

    `git remote -v`

7. Vous devez voir quelque chose comme cette sortie :

    ```markdown
    $ git remote -v

    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (fetch)
    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (push)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (fetch)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (push)
    ```

    Si votre sortie distant ne ressemble pas à cela, vous pouvez essayer à nouveau en premier en cours d’exécution `git remote remove upstream`.

## <a name="create-a-branch-and-a-new-article"></a>Créer une branche et un nouvel article

Suivez ces étapes pour créer un article.

### <a name="create-a-new-branch-and-a-new-file"></a>Créer une nouvelle branche et un nouveau fichier

Avant de commencer à travailler sur votre contenu, vous devez créer une nouvelle branche dans votre référentiel local.

#### <a name="to-create-a-new-branch-in-git-bash"></a>Pour créer une nouvelle branche dans Git Bash

- Ouvrez Git Bash et tapez les commandes (une à la fois) :

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >Nous recommandons vivement d’affectation de noms votre branche quelque chose évidente et unique afin de trouver plus tard.

    Une fois les commandes, dans votre nouvelle branche et vous prêt à créer votre nouveau fichier. Vous devez uniquement modifier dans le référentiel windowsserverdocs-demande de tirage une fois par instance de votre interpréteur de commandes Git. Si vous fermez Git Bash, vous devrez modifier les répertoires à nouveau une fois que vous l’ouvrez.

#### <a name="to-create-a-new-file-in-your-branch"></a>Pour créer un nouveau fichier dans votre branche

1. Ouvrez l’Explorateur Windows, accédez à votre répertoire de GitHub et créer un nouveau fichier texte dans la structure de dossiers. Votre nom de fichier doit être tout en minuscules et séparés par des traits d’union. Par exemple, _what-est-windows-Server.md_.

     Vous devez également modifier l’extension de fichier à partir de .txt à. md. Dans le message d’erreur qui s’affiche, sélectionnez **Oui** pour enregistrer le fichier avec la nouvelle extension de fichier.

2. Ouvrez Visual Studio Code et accédez à **fichier**, sélectionnez **ouvrir le dossier**, puis accédez à l’emplacement GitHub du fichier que vous avez créé à l’étape 1.

3. À partir de la **Explorer** volet, sélectionnez votre nouveau fichier.

4. Ajoutez votre texte à la page, puis sélectionnez **fichier** > **enregistrer**.

### <a name="preview-your-text"></a>Afficher un aperçu de votre texte

Une fois que vous ajoutez votre texte à votre nouveau fichier, vous devez afficher un aperçu vos modifications pour vous assurer qu’ils s’affichent correctement.

#### <a name="to-preview-your-text"></a>Pour afficher un aperçu de votre texte

1. Dans Visual Studio Code, sélectionnez une de la **aperçu** boutons à partir de l’angle supérieur droit.

    ![icône de bouton d’aperçu](media/create-new-using-github/preview-button-full-page.png): Bascule vers un pleine page Aperçu de votre contenu.

    ![icône de bouton d’aperçu](media/create-new-using-github/preview-button-side-by-side.png): Ouvre la page d’aperçu en regard de la page de votre travail, côte à côte.

2. Assurez-vous que votre article recherche la façon dont vous prévoyez qu’il recherche.

    Une fois que vous êtes sûr de que son aspect, vous pouvez valider vos modifications et créer une demande de tirage pour publication.

### <a name="commit-your-changes"></a>Validez vos modifications

Une fois que vous vérifiez que votre texte est correcte, vous pouvez valider vos modifications à votre version locale de votre référentiel.

#### <a name="to-commit-your-changes"></a>Pour valider vos modifications

- Ouvrez Git Bash et tapez les commandes (une à la fois, en supprimant les balises facultatifs) :

    ```markdown
    OPTIONAL: git status

    git add .

    git commit -m “public comment about what change is”

    OPTIONAL: git pull upstream master

    git push origin <name_of_your_new_branch>

    ```

    La commande d’état git facultatif vous montre à quels fichiers ont été modifiés dans le cadre de cette validation. Le git facultatif tirage des extractions master en amont vers le bas les dernières modifications de contenu à partir de la branche principale MicrosoftDocs, la synchronisation de votre contenu avec le contenu maître principal local. Cela permet de vous montrer les éventuels conflits de fusion avance afin de les corriger avant de passer à l’étape de la demande de tirage.

### <a name="submit-a-pull-request-for-review-and-publication"></a>Soumettre une demande de tirage pour révision et de publication

Une fois que vous avez terminé votre article, vous devez obtenir l’approbation de votre graveur (laisser le temps pour cela) pour la publication.

#### <a name="to-submit-your-pull-request"></a>Pour soumettre votre demande de tirage

1. Accédez à https://github.com/MicrosoftDocs/windowsserverdocs-pr et sélectionnez le **demandes de tirage** onglet.

2. Dans le **réviseurs** zone du volet droit, sélectionnez l’icône d’engrenage, puis entrez le _windowsservercontent_ alias pour la révision.

    Un membre de la _windowsservercontent_ alias seront passez en revue vos modifications ou ajouter des commentaires sur les éléments qui doivent être modifiés avant la fusion peut se produire.

3. Type **#sign-off** dans les commentaires afin que les réviseurs connaissent vous remettez pour révision et la publication. Le **#sign-off** commentaire :

    - Met à jour l’étiquette de votre demande de tirage de **ne pas fusionner** à **prêt pour fusion**.

    - Permet les alias et les enregistreurs de savoir que vous êtes prêt à recevoir votre contenu consulté.

    - Permet les administrateurs savent qu’après l’approbation, votre contenu est prêt à aller en direct.

    >[!Important]
    >Après avoir ajouté le commentaire #sign-off, un membre de l’équipe windowsservercontent sera Lisez le texte et transmettez-le à maîtriser afin qu’il accède à la prochaine planifiée publier pour live (10 h 30 et 3 h 30 en semaine).
    >
    >Si vous n’ajoutez #sign-off sous forme de commentaire finale à votre demande de tirage, votre contenu restera dans la file d’attente sans poussé à maîtriser et finalement de vie.

## <a name="related-information"></a>Informations connexes

Pour plus d’informations sur GitHub et le langage markdown, consultez :

### <a name="git-concepts"></a>Concepts de GIT

- [Présentation du manuel Guides GitHub-Git](https://guides.github.com/introduction/git-handbook/)

- [Projets de duplication de Guides de GitHub](https://guides.github.com/activities/forking/)

- [Guides de GitHub-comprendre le flux GitHub](https://guides.github.com/introduction/flow/)

- [Découvrez Git Branching](https://learngitbranching.js.org/ (idéal pour les étudiants d’imaginer !))

### <a name="markdown"></a>Markdown

- [Nos conseils markdown interne](https://review.docs.microsoft.com/help/contribute/markdown-reference?branch=master)

- [Externe, didacticiel de GitHub](https://www.markdowntutorial.com/)
