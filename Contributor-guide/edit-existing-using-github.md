---
title: Modifier un article de serveur Windows existant à l’aide de GitHub et Visual Studio Code
description: Comment modifier un article existant lié à Windows Server, à l’aide de GitHub et Visual Studio Code, en tant qu’employé Microsoft.
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: d681d2fc2b69898e841932a95738b89515ffb51a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865082"
---
# <a name="edit-an-existing-windows-server-article-using-github-and-visual-studio-code"></a>Modifier un article de serveur Windows existant à l’aide de GitHub et Visual Studio Code

Il existe deux emplacements distincts où nous conservons le contenu technique de Windows Server. L’un des emplacements est public (windowsserverdocs) tandis que l’autre est privé (windowsserverdocs-PR). Qui détermine à quel emplacement vous contribuez :

- **Je suis un employé de Microsoft.** En tant qu’employé Microsoft, vous disposez d’options, en fonction de ce que vous essayez de faire :

    - **Créez un article de toute nouvelle appellation.** Pour créer un article de toute nouvelle appellation, vous devez créer et configurer votre compte et vos outils GitHub, brancher et cloner le référentiel windowsserverdocs-PR, configurer votre branche distante, créer l’article et enfin créer une nouvelle requête de tirage pour l’approbation et la publication. Pour ces instructions, vous pouvez suivre les instructions fournies dans l’article [créer des articles Windows Server à l’aide de GitHub et Visual Studio code](create-new-using-github.md) .

    - **Apportez des modifications importantes à un article existant.** Pour apporter des modifications substantielles à un article existant, vous pouvez suivre les instructions de cet article.

    - **Apporter des modifications mineures à un article existant.** Pour apporter des modifications mineures à un article existant, vous pouvez suivre les instructions de la rubrique [mettre à jour les articles Windows Server existants à l’aide d’un navigateur Web et](github-browser-updates.md) de l’article github.

- **Je ne suis pas un employé de Microsoft.** En tant qu’employé non-Microsoft, vous devez contribuer à l’emplacement public. Pour plus d’informations sur la procédure à suivre, consultez l’article [contribution à la documentation technique de Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md) .

## <a name="switch-your-repo-and-create-a-new-branch"></a>Changer de référentiel et créer une branche

Pour modifier un article existant, procédez comme suit.

### <a name="create-a-new-branch-and-locate-the-file-you-want-to-update"></a>Créer une nouvelle branche et localiser le fichier à mettre à jour

Avant de pouvoir commencer à travailler sur votre contenu, vous devez d’abord passer au référentiel windowsserverdocs-PR, puis Rechercher l’article que vous souhaitez mettre à jour.

#### <a name="to-create-a-new-branch-in-git-bash"></a>Pour créer une nouvelle branche dans Git bash

- Ouvrez git bash et tapez les commandes (une à la fois) :

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >Nous vous recommandons vivement d’attribuer à votre branche un nom évident et unique, afin de pouvoir la retrouver plus tard.

    Une fois les commandes terminées, vous êtes dans votre nouvelle branche et vous êtes prêt à modifier votre fichier.

#### <a name="to-locate-your-article-and-make-your-edits"></a>Pour trouver votre article et apporter vos modifications

1. Ouvrez Visual Studio Code et accédez à **fichier**, sélectionnez **ouvrir le dossier**, puis accédez à l’emplacement github du dossier contenant l’article que vous souhaitez modifier.

2. Dans le volet **Explorateur** , sélectionnez le fichier.

3. Mettez à jour les informations de la page, puis sélectionnez **fichier** > **Enregistrer**.

### <a name="preview-your-text"></a>Afficher un aperçu de votre texte

Après avoir mis à jour le texte, vous devez afficher un aperçu de vos modifications pour vous assurer qu’elles s’affichent correctement.

#### <a name="to-preview-your-text"></a>Pour afficher un aperçu de votre texte

1. Dans Visual Studio Code, sélectionnez l’un des boutons d' **Aperçu** dans le coin supérieur droit.

    ![icône du bouton Aperçu](media/create-new-using-github/preview-button-full-page.png): Bascule vers une version préliminaire complète de votre contenu.

    ![icône du bouton Aperçu](media/create-new-using-github/preview-button-side-by-side.png): Ouvre la page d’aperçu en regard de votre page de travail, côte à côte.

2. Assurez-vous que votre article a l’aspect souhaité.

    Une fois que vous êtes certain qu’il semble correct, vous pouvez valider vos modifications et créer une demande de tirage (pull request) pour la publication.

### <a name="commit-your-changes"></a>Valider vos modifications

Après vous être assuré que votre texte semble correct, vous pouvez valider vos modifications dans votre version locale de votre référentiel.

#### <a name="to-commit-your-changes"></a>Pour valider vos modifications

- Ouvrez git bash et tapez les commandes (une à la fois, en supprimant les balises FACULTATIVEs) :

    ```markdown
    OPTIONAL: git status

    git add .

    git commit -m “public comment about what change is”

    OPTIONAL: git pull upstream master

    git push origin <name_of_your_new_branch>

    ```

    La commande facultative git status indique les fichiers qui ont été modifiés dans le cadre de cette validation. Le maître d’extraction git facultatif extrait les dernières modifications de contenu à partir de la branche maître MicrosoftDocs, en synchronisant votre contenu local avec le contenu maître principal. Cela vous permet de vous montrer tous les conflits de fusion potentiels à l’avance, afin de pouvoir les corriger avant que vous ne passiez à l’étape PR.

### <a name="submit-a-pull-request-for-review-and-publication"></a>Envoyer une demande de tirage (pull request) pour révision et publication

Une fois que vous avez terminé vos mises à jour, vous devez obtenir l’approbation de votre enregistreur (attendez un certain temps) pour la publication.

#### <a name="to-submit-your-pull-request"></a>Pour envoyer votre demande de tirage (pull request)

1. Accédez à https://github.com/MicrosoftDocs/windowsserverdocs-pr et sélectionnez l’onglet **requêtes de tirage** .

2. Dans la zone **réviseurs** du volet droit, sélectionnez l’icône d’engrenage, puis entrez l’alias _windowsservercontent_ pour la révision.

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

Pour plus d’informations sur GitHub et le langage de démarque, consultez :

### <a name="git-concepts"></a>Concepts git

- [Guides GitHub-introduction au manuel git](https://guides.github.com/introduction/git-handbook/)

- [Guides GitHub-embranchement de projets](https://guides.github.com/activities/forking/)

- [Guides GitHub-comprendre le déroulement du GitHub](https://guides.github.com/introduction/flow/)

- [Découvrir la branche git] (https://learngitbranching.js.org/ (Idéal pour les apprenants visuels !))

### <a name="markdown"></a>Markdown

- [Notre guide de démarque interne](https://review.docs.microsoft.com/help/contribute/markdown-reference?branch=master)

- [Didacticiel externe, GitHub](https://www.markdowntutorial.com/)