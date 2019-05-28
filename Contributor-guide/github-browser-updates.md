---
title: Modifiez des articles de Windows Server existants à l’aide d’un navigateur web et GitHub
description: Comment apporter des modifications rapides à la documentation de Windows Server existante à l’aide d’un navigateur web et GitHub, comme un employé de Microsoft.
author: eross-msft
ms.author: lizross
ms.date: 05/02/2019
ms.openlocfilehash: d4574de7774a43092815a3d154020559c50fdcf9
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624588"
---
# <a name="update-existing-windows-server-articles-using-a-web-browser-and-github"></a>Mettre à jour des articles de Windows Server existants à l’aide d’un navigateur web et GitHub

Il existe deux emplacements distincts, où nous conservons le contenu technique de Windows Server. Un des emplacements est public (windowsserverdocs), tandis que l’autre est privée (windowsserverdocs-pr). Qui vous êtes détermine à quel emplacement vous contribuer à :

- **Je ne suis pas un employé de Microsoft.** En tant qu’un employé non Microsoft, vous devez contribuer à l’emplacement public. Pour savoir comment procéder, consultez la [Contributing.md](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md) fichier.

- **Je suis un employé de Microsoft.** Un employé de Microsoft, vous disposez des options, selon ce que vous essayez d’effectuer :

    - **Créer un tout nouvel article.** Pour créer un tout nouvel article, vous devez créer et configurer votre compte GitHub et les outils, répliquer et cloner le dépôt windowsserverdocs-pr, configurer votre branche distante, créer l’article et enfin créer une nouvelle demande de tirage pour approbation et la publication. Pour ces instructions, consultez le [créer de nouveaux articles de Windows Server à l’aide de GitHub et Visual Studio Code](create-new-using-github.md) article.

    - **Apporter des modifications importantes à un article existant.** Pour apporter des modifications importantes à un article existant, vous pouvez suivre les instructions dans le [modifier un article existant de Windows Server à l’aide de GitHub et Visual Studio Code](edit-existing-using-github.md) article.

    - **Apporter des modifications mineures à un article existant.** Pour apporter des modifications mineures à un article existant, vous pouvez suivre les instructions fournies dans cet article.

    > [!IMPORTANT]
    > Tous les dépôts qui publient sur docs.microsoft.com ont adopté le [Microsoft Source Code de conduite Open](https://opensource.microsoft.com/codeofconduct/) ou [.NET Foundation Code de conduite de](https://dotnetfoundation.org/code-of-conduct). Pour plus d’informations, consultez le [Code de conduite Forum aux questions](https://opensource.microsoft.com/codeofconduct/faq/). Ou contactez [ opencode@microsoft.com ](mailto:opencode@microsoft.com), ou [ conduct@dotnetfoundation.org ](mailto:conduct@dotnetfoundation.org) envoyer vos questions ou commentaires.
    >
    > Corrections mineures ou les clarifications pour des exemples de code et de la documentation dans les référentiels publics sont couverts par la [docs.microsoft.com conditions d’utilisation](https://docs.microsoft.com/legal/termsofuse). Nouveautés ou modifications significatives génèrent un commentaire dans la demande de tirage, vous demandant d’envoyer un contrat de licence de Contribution (CLA) en ligne si vous n’êtes pas un employé de Microsoft. Nous vous demandons pour remplir le formulaire en ligne avant que nous pouvons examiner ou accepter votre demande de tirage.

## <a name="quick-edits-to-existing-articles-using-github-and-a-web-browser"></a>Modifications rapides à des articles existants à l’aide de GitHub et un navigateur web

Modifications rapides rationalisent le processus pour signaler et corriger les erreurs et les omissions dans les documents. Malgré tous les efforts, grammaire petit et fautes d’orthographe _faire_ font place dans nos documents publiés.

1. Accédez à https://github.com/MicrosoftDocs/windowsserverdocs-pr/tree/master/WindowsServerDocs.

2. Accédez à l’article que vous souhaitez modifier, puis sélectionnez le **modifier ce fichier** bouton.

   ![Modifier ce bouton de fichier](media/github-browser-updates/edit-this-file.png)

3. Modifier la rubrique, puis faites défiler vers le bas, décrivez brièvement les modifications, puis sélectionnez **valider les modifications**.

    ![Valider les modifications avec les informations de titre](media/github-browser-updates/commit-changes.png)

## <a name="submit-the-pull-request"></a>Envoyez la demande de tirage

Après avoir créé votre demande de tirage, vous devez l’envoyer pour approbation et la publication.

### <a name="to-submit-your-pull-request"></a>Pour soumettre votre demande de tirage

1. Sur le **ouvrir une demande de tirage** page, mettez à jour de votre message de validation pour le rendre plus appropriée pour une demande de tirage. Exemple : Corriger les fautes de frappe dans le premier paragraphe.

2. Assurez-vous que seuls les validations et les fichiers que vous pensez être inclus sont inclus. Vérifiez également que la demande de tirage accède à la branche correcte dans le référentiel en amont, master (généralement) ou (parfois) d’une branche de version.

3. Dans le **réviseurs** zone du volet droit, sélectionnez l’icône d’engrenage, puis entrez _windowsservercontent_. Un membre de l’alias est sur le point de passer en revue vos modifications et soit fusionner votre collecteur demander ou ajouter des commentaires sur les éléments à modifier avant la fusion.

4. Sélectionnez **créer une demande de tirage**. La nouvelle demande de tirage est lié à votre branche de travail de votre fourche. Jusqu'à ce que la demande de tirage est fusionnée, les nouvelles validations que vous envoyez à la même branche de travail de votre fourche sont automatiquement incluses dans la demande de tirage. Une nouvelle étiquette est ajoutée à votre demande de tirage qui indique, **ne pas fusionner**. Cela signifie simplement que votre contenu est toujours en cours et ne doit pas être passé en revue ou transmis sur le site actif.

5. Lorsque vous êtes prêt pour un membre de l’alias pour passer en revue votre contenu, vous devez ajouter le texte, **#sign-off** aux commentaires. Ce commentaire :

    - Met à jour l’étiquette de votre demande de tirage de **ne pas fusionner** à **prêt pour fusion**.

    - Permet les alias et les enregistreurs de savoir que vous êtes prêt à recevoir votre contenu consulté.

    - Permet les administrateurs savent qu’après l’approbation, votre contenu est prêt à aller en direct.