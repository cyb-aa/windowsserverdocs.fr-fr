---
title: Modifier un article existant de Windows Server à l’aide de GitHub et Visual Studio Code
description: Comment modifier un existant articles relatifs à Windows Server, à l’aide de GitHub et Visual Studio Code, comme un employé de Microsoft.
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: d2d95cc28089ceb74bf9690f6bd78611e7d9a27a
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624583"
---
# <a name="edit-an-existing-windows-server-article-using-github-and-visual-studio-code"></a>Modifier un article existant de Windows Server à l’aide de GitHub et Visual Studio Code

Il existe deux emplacements distincts, où nous conservons le contenu technique de Windows Server. Un des emplacements est public (windowsserverdocs), tandis que l’autre est privée (windowsserverdocs-pr). Qui vous êtes détermine à quel emplacement vous contribuer à :

- **Je suis un employé de Microsoft.** Un employé de Microsoft, vous disposez des options, selon ce que vous essayez d’effectuer :

    - **Créer un tout nouvel article.** Pour créer un tout nouvel article, vous devez créer et configurer votre compte GitHub et les outils, répliquer et cloner le dépôt windowsserverdocs-pr, configurer votre branche distante, créer l’article et enfin créer une nouvelle demande de tirage pour approbation et la publication. Pour ces instructions, vous pouvez suivre les instructions fournies dans le [créer de nouveaux articles de Windows Server à l’aide de GitHub et Visual Studio Code](create-new-using-github.md) article.

    - **Apporter des modifications importantes à un article existant.** Pour apporter des modifications importantes à un article existant, vous pouvez suivre les instructions fournies dans cet article.

    - **Apporter des modifications mineures à un article existant.** Pour apporter des modifications mineures à un article existant, vous pouvez suivre les instructions dans le [mettre à jour des articles de Windows Server existants à l’aide d’un navigateur web et le GitHub](github-browser-updates.md) article.

- **Je ne suis pas un employé de Microsoft.** En tant qu’un employé non Microsoft, vous devez contribuer à l’emplacement public. Pour savoir comment procéder, consultez la [contribuant à la documentation technique de Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md) article.

## <a name="switch-your-repo-and-create-a-new-branch"></a>Basculer votre dépôt et créer une branche

Suivez ces étapes pour modifier un article existant.

### <a name="create-a-new-branch-and-locate-the-file-you-want-to-update"></a>Créer une branche et recherchez le fichier que vous souhaitez mettre à jour

Avant de commencer à travailler sur votre contenu, vous devez tout d’abord modifier dans le référentiel windowsserverdocs-pr et recherchez l’article que vous souhaitez mettre à jour.

#### <a name="to-create-a-new-branch-in-git-bash"></a>Pour créer une nouvelle branche dans Git Bash

- Ouvrez Git Bash et tapez les commandes (une à la fois) :

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >Nous recommandons vivement d’affectation de noms votre branche quelque chose évidente et unique afin de trouver plus tard.

    Une fois les commandes, dans votre nouvelle branche et vous prêt à modifier votre fichier.

#### <a name="to-locate-your-article-and-make-your-edits"></a>Pour localiser votre article et apportez vos modifications

1. Ouvrez Visual Studio Code et accédez à **fichier**, sélectionnez **ouvrir le dossier**, puis accédez à l’emplacement GitHub du dossier qui a l’article que vous souhaitez modifier.

2. À partir de la **Explorer** volet, sélectionnez le fichier.

3. Mettre à jour les informations sur la page, puis sélectionnez **fichier** > **enregistrer**.

### <a name="preview-your-text"></a>Afficher un aperçu de votre texte

Une fois que vous mettez à jour le texte, vous devez afficher un aperçu vos modifications pour vous assurer qu’ils s’affichent correctement.

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

Une fois que vous avez terminé vos mises à jour, vous devez obtenir l’approbation de votre graveur (laisser le temps pour cela) pour la publication.

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