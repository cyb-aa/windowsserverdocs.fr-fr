# <a name="quality-criteria-for-pull-request-review"></a>Passez en revue des critères de qualité pour la demande de tirage

Demandes de tirage doivent répondre aux critères minimum d’être acceptés pour l’intégration dans le référentiel. Les évaluateurs uniquement ce qui est nouveau ou modifié, à l’aide du blocage et non bloquante qualité révision figurant dans cet article pour évaluer les modifications des requêtes de tirage. Les réviseurs peuvent demander pour plus d’informations ou pour un demandeur résoudre les problèmes avant d’accepter une demande de tirage. Le demandeur est chargé du modifier.

>[!IMPORTANT] 
>Ouvrez une demande pull déclenche des contrôles de qualité et de publication à notre site intermédiaire interne. Il vous incombe pour passer en revue les deux, à partir des liens dans les commentaires \(sous l’onglet de la Conversation de la demande de tirage, cliquez sur **afficher toutes les vérifications** > **détails**\). Une fois cette opération effectuée, l’indiquer en ajoutant le **« prêt pour la fusion »** étiquette à la demande de tirage. \(Cliquez sur **étiquettes** ou l’engrenage à droite du flux de commentaires dans la demande de tirage.) Si vous ne pouvez pas ajouter l’étiquette \(proviennent contributeurs ont signalé ce), ajoutez « prêt pour la fusion » un commentaire à la demande de tirage et envoyer un e-mail à l’alias du réviseur, wssc-pra@microsoft.com.

## <a name="blocking-content-quality-items"></a>Blocage des éléments de la qualité du contenu

Les mises à jour doivent respecter les critères suivants devant être fusionnées.

| Category | Élément de révision de qualité |
|----------|---------------------|
|Prérequis| La demande de tirage n’a pas :<br>-erreurs de validation ou des avertissements<br>-les conflits de fusion<br>-régression évidente du contenu<br>-incorporé dépôt ou fichiers inhabituels superflus|
|Intégrité du référentiel |Demande de tirage contient moins de 100 fichiers modifiés, sauf si elle est mise à jour une branche de version à partir de master ou pour effectuer une mise à jour en bloc similaires comme un correctif de métadonnées. (Vraiment, elle doit en contenir beaucoup moins, mais après 100 fichiers modifiés, GitHub n’affiche pas les différences).|
|Intégrité du référentiel| Demande de tirage ne pas obtenir fusionnée par la personne qui a créé la demande de tirage.|
|Appellation |Les noms de fichier pour les nouveaux fichiers suivent les [les instructions d’affectation de noms de fichiers](file-names-and-locations.md).|
|Appellation |Nouveaux dossiers introduits dans le référentiel suivent le [dossier règles de dénomination](file-names-and-locations.md#folder-names-in-the-repo).|
|/ SEO d’affectation de noms|Le titre H1 possède suffisamment d’informations pour décrire le contenu de l’article, pour le différencier des autres articles et pour mapper des mots-clés client probables. Par exemple, un titre H1 de « Présentation » ne répond pas à ce critère.|
|Contenu|Il est un document technique. Consultez le [où vont les données des conseils](content-channel-guidance.md).|
|Contenu|Il a un paragraphe d’introduction et un corps procédural ou conceptuel du contenu. Il a assez de contenu pour mériter en tant qu’article et n’est pas un petit fragment d’informations.|
|Contenu| Il suit la mise en forme standard et conception de contenu : éléments qui doivent être des listes numérotées sont numérotés, les éléments qui doivent être des listes non ordonnées sont à puces. Il s’agit de facilité d’utilisation de base.|
|Contenu| Il n’a graphiques inhabituels, architecture d’informations ou structures ni non standards.|
|Fonctionnalités du site/design| L’article n’a pas une table des matières créé manuellement dans le contenu. L’article doit s’appuyer sur H2s pour sa table des matières sur la page.|
|Fonctionnalités du site/design| Si des en-têtes H2 sont utilisés, il existe au moins deux. Les têtes H3 sont précédées d’un en-tête H2. À l’aide d’un en-tête H2 crée une table des matières de l’article seul élément. Utilisez des en-têtes H2 avant un titre H3 pour assurer la que création d’une table des matières.|
|Markdown| L’article ne contient pas de blocs de code HTML ; inline mineure HTML est autorisée – comme exposant, indice, des caractères spéciaux et autres mineure une mise en forme Markdown ne prend pas en charge. Tableaux HTML sont autorisés uniquement si la table contient une liste à puces ou des listes numérotées, mais généralement, qui indique le contenu peut être simplifié afin qu’il puisse être codée en Markdown.|
|Markdown   |Des éléments markdown personnalisés sont utilisés le cas échéant. Ex : Remarques sont codées à l’aide de la ! Notez l’extension, pas en tant que texte brut.|
|Images|Les images incluent le texte de remplacement. **Il s’agit d’une exigence d’accessibilité qui doit être remplie dans le cadre des critères de déclenchement.** [Instructions détaillées ici](https://worldready.cloudapp.net/Styleguide/Read?id=2665&topicid=28349). |
|Images |Fichiers GIF animés ne sont pas acceptés dans le référentiel.|
|Images | Images ont une résolution claire, sont de mots mal orthographiés et ne contiennent aucune information privée [voir l’image d’instructions](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md). |
|Création intermédiaire| L’article a été prévisualisé en phase intermédiaire et ne contient pas d’éventuels problèmes flagrants mise en forme :<br>-Une liste à puce ou numérotée qui apparaît sous la forme d’un paragraphe <br> -Le code dans un bloc de code apparaissant en partie dans le bloc de code et en partie à l’extérieur <br>-Étapes mal numérotées en raison d’une mauvaise indentation|

## <a name="non-blocking-content-quality-items"></a>Non bloquant les éléments de qualité de contenu

Pour ces éléments, réviseurs des demandes de tirage fournissent leurs commentaires et instructions pour l’auteur de suivi avec des correctifs dans une requête de tirage ultérieure. Toutefois, ces commentaires ne bloque pas la décision de fusion. Les auteurs doivent suivre des sous 3 jours ouvrables avec des correctifs.

| Category | Élément de révision de qualité |
|----------|---------------------|
|Contenu|Articles doivent disposer d’un « étapes suivantes » à la fin avec 1 à 3 étapes suivantes pertinentes et convaincantes. Bref texte doit être inclus pour aider le client pour comprendre pourquoi les étapes suivantes sont pertinentes. (Nouveaux articles uniquement).
|Images|Images utilisent le style de légende approprié et la couleur et les captures d’écran utilisent le style de bordure et d’espace réservé correct. [Reportez-vous à l’aide de l’image](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md).|
|Fonctionnalités du site/design|Les en-têtes H2, lors du rendu dans la table des matières sur la page, doivent idéalement envelopper pas plus de 2 lignes. En-têtes plus longs rendent la table des matières de l’article plus difficile à analyser.|
|Conventions de style|Tous les titres et en-têtes adoptent une majuscule.|
|Process|Lorsque vous supprimez ou renommez un article, vérifiez que vous suivez le processus. Réviseurs doivent ajouter le commentaire et le lien suivant dans un commentaire de demande de tirage :<br><br>*Vérifiez que vous avez suivi le processus dans le guide du contributeur : [Modifier le nom d’un article ou la supprimer](rename-or-retire.md).*|
