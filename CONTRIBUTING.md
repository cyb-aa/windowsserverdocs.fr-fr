# <a name="contributing-to-windows-server-technical-documentation"></a>Contribuez à la documentation technique de Windows Server

Nous vous remercions de votre intérêt dans la documentation technique de Windows Server. Les commentaires, modifications et ajouts que vous apportez à notre documentation nous sont précieux. Il existe deux emplacements distincts, où nous conservons le contenu technique de Windows Server. Un des emplacements est public (windowsserverdocs), tandis que l’autre est privée (windowsserverdocs-pr). Qui vous êtes détermine à quel emplacement vous contribuer à :

- **Je ne suis pas un employé de Microsoft.** En tant qu’un employé non Microsoft, vous devez contribuer à l’emplacement public. Pour plus d’informations sur la marche à suivre, poursuivez la lecture de cet article.

- **Je suis un employé de Microsoft.** Un employé de Microsoft, vous disposez des options, selon ce que vous essayez d’effectuer :

    - **Créer un tout nouvel article.** Pour créer un tout nouvel article, vous devez créer et configurer votre compte GitHub et les outils, répliquer et cloner le dépôt windowsserverdocs-pr, configurer votre branche distante, créer l’article et enfin créer une nouvelle demande de tirage pour approbation et la publication. Pour ces instructions, consultez le [créer de nouveaux articles de Windows Server à l’aide de GitHub et Visual Studio Code](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/Contributor-guide/create-new-using-github.md) article.

    - **Apporter des modifications importantes à un article existant.** Pour apporter des modifications importantes à un article existant, vous pouvez suivre les instructions dans le [modifier un article existant de Windows Server à l’aide de GitHub et Visual Studio Code](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/Contributor-guide/edit-existing-using-github.md) article.

    - **Apporter des modifications mineures à un article existant.** Pour apporter des modifications mineures à un article existant, vous pouvez suivre les instructions dans le [mettre à jour des articles de Windows Server existants à l’aide d’un navigateur web et le GitHub](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/Contributor-guide/github-browser-updates.md) article.

## <a name="sign-a-cla"></a>Signature d’un CLA

Tous les contributeurs qui sont ***pas*** un employé de Microsoft doit [signer un Microsoft licences accord de contribution](https://cla.microsoft.com/) avant de modifier des référentiels de Microsoft. Si vous avez déjà modifié au sein des référentiels de Microsoft dans le passé, Félicitations !
Vous avez déjà effectué cette étape.

## <a name="editing-topics"></a>Modification des rubriques

Nous avons essayé de simplifier la modification d’un fichier existant, publique aussi simple que possible.

### <a name="to-edit-a-topic"></a>Pour modifier une rubrique

1. Accédez à la page https://docs.microsoft.com/windows-server que vous souhaitez mettre à jour, puis sélectionnez **modifier**.

    ![Web de GitHub, montrant le lien d’édition](media/contribute-link.png)

2. Se connecter à (ou vous inscrire pour) un compte GitHub.

    Vous devez disposer d’un compte GitHub pour accéder à la page qui vous permet de modifier une rubrique.

3. Sélectionnez le **crayon** icône (en rouge) pour modifier le contenu.

    ![Web de GitHub, affichant l’icône de crayon dans la zone rouge](media/pencil-icon.png)

4. À l’aide du langage Markdown, apportez vos modifications à la rubrique. Pour plus d’informations sur la modification du contenu à l’aide de Markdown, consultez :

    - **Si vous êtes lié à l’organisation de Microsoft dans GitHub :** [Guide du contributeur de Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs-pr/tree/master/Contributor-guide)

    - **Si vous n’êtes externes à Microsoft :** [Maîtrise de Markdown](https://guides.github.com/features/mastering-markdown/)

5. Vérifiez votre modification suggérée, puis sélectionnez **aperçu des modifications** s’assurer que tout semble correct.

    ![Web de GitHub, montrant l’onglet d’aperçu des modifications](media/preview-changes.png)

6. Lorsque vous avez terminé à la modification de la rubrique, faites défiler vers le bas de la page, tapez un nom descriptif pour votre embranchement, puis cliquez sur **proposer un changement de fichier** pour créer le branchement dans votre compte GitHub personnel.

    ![Web de GitHub, montrant le bouton de modification de fichier proposer](media/propose-file-change.png)

    Le **comparaison des modifications apportées** écran s’affiche pour voir quels sont les modifications entre votre duplication et le contenu d’origine.

7. Sur le **comparaison des modifications apportées** écran, vous voyez s’il existe des problèmes avec le fichier que vous archivez.

    S’il n’existe aucun problème, vous verrez le message, **en mesure de fusionner**.

    ![Web de GitHub, montrant la comparaison modifie l’écran](media/compare-changes.png)

8. Sélectionnez **créer une demande de tirage**.

    Requêtes de tirage permettent d’indiquer à d’autres utilisateurs sur les modifications que vous avez envoyée à une branche dans un référentiel sur GitHub. Après l’ouverture d’une demande de tirage, vous pouvez discuter et passez en revue les modifications potentielles avec des collaborateurs et ajouter des validations suivies avant que vos modifications sont fusionnées dans la branche de base. Pour plus d’informations, consultez [sur les demandes de tirage](https://help.github.com/articles/about-pull-requests)

9. Entrez un titre et une description pour donner à l’approbateur le contexte approprié sur les nouveautés dans la demande.

10. Faites défiler vers le bas de la page, en veillant à ce que seuls vos fichiers modifiés sont dans cette requête de tirage. Sinon, vous risquez de remplacer les modifications à partir d’autres personnes.

11. Sélectionnez **créer une demande de tirage** à nouveau à soumettre la demande de tirage.

    La demande de tirage est envoyée vers le writer de la rubrique et vos modifications sont examinées. Si votre demande est acceptée, vos mises à jour sont publiées.

## <a name="resources"></a>Ressources

- Vous pouvez utiliser votre éditeur de texte pour modifier le code Markdown. Nous vous recommandons de [Visual Studio Code](https://code.visualstudio.com/), un éditeur léger open source gratuit de Microsoft.