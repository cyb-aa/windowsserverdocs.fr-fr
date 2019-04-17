---
ms.assetid: 13fe87d9-75cf-45bc-a954-ef75d4423839
title: "Annexe I - création de comptes de gestion des comptes protégés et des groupes dans ActiveDirectory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 666180adca691d6c9783a43063df76877115fc40
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>AnnexeI Création de gestion des comptes pour les comptes protégés et groupes dans ActiveDirectory

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

  
## <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>AnnexeI Création de gestion des comptes pour les comptes protégés et groupes dans ActiveDirectory  

L’une des difficultés lors de l’implémentation d’un modèle d’ActiveDirectory qui ne repose pas sur l’appartenance permanente aux groupes disposant de privilèges très est qu’il doit y avoir un mécanisme pour renseigner ces groupes lorsqu’une appartenance temporaire dans les groupes est nécessaire. Certaines solutions de gestion des identités privilégié nécessitent que les comptes de service du logiciel ont été accordées à l’appartenance permanente aux groupes comme DA ou administrateurs dans chaque domaine dans la forêt. Toutefois, il est techniquement nécessaire pour les solutions Privileged Identity Management (PIM) exécuter leurs services dans des contextes ces privilèges élevés.  
  
Cette annexe fournit des informations que vous pouvez utiliser des solutions PIM tiers ou en mode natif implémentées pour créer des comptes qui disposent de privilèges limités et peuvent être contrôlés stricte, mais peuvent être utilisées pour remplir des groupes privilégiés dans ActiveDirectory quand l’élévation temporaire est requise. Si vous implémentez PIM comme une solution native, ces comptes peuvent être utilisées par le personnel d’administration pour effectuer la population de groupe temporaire, et si vous implémentez PIM via un logiciel tiers, vous pourrez peut-être adapter ces comptes pour fonctionner en tant que comptes de service.  
  
> [!NOTE]  
> Les procédures décrites dans cette annexe fournissent une approche de la gestion des groupes dotés de privilèges élevés dans ActiveDirectory. Vous pouvez adapter ces procédures pour répondre à vos besoins, des restrictions supplémentaires, ou omettre certains des restrictions qui sont décrits ici.  
  
### <a name="creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Création de gestion de comptes pour les comptes protégés et groupes dans ActiveDirectory  
Création de comptes qui peut servir à gérer l’appartenance des groupes privilégiés sans avoir besoin des comptes de gestion de droits excessive et autorisations se compose de quatre activités générales qui sont décrites dans les instructions pas à pas qui suivent:  
  
1.  Tout d’abord, vous devez créer un groupe qui doit gérer les comptes, étant donné que ces comptes doivent être gérés par un ensemble limité d’utilisateurs approuvés. Si vous ne disposez pas déjà d’une structure d’unité d’organisation qui prend en compte séparant les comptes privilégiés et protégées et les systèmes à partir de la population dans le domaine, vous devez créer. Bien que des instructions spécifiques ne sont pas fournies dans cette annexe, captures d’écran montrent un exemple de ce une hiérarchie d’UO.  
  
2.  Créez les comptes d’administration. Ces comptes doivent être créés en tant que comptes d’utilisateur «normal» et sans droits d’utilisateur au-delà de celles qui sont déjà accordées aux utilisateurs par défaut.  
  
3.  Implémenter des restrictions sur les comptes de gestion qui les rendent utilisable uniquement dans le but spécialisé pour lequel elles ont été créées, en plus de contrôler qui peut activer et utiliser les comptes (le groupe que vous avez créé à la première étape).  
  
4.  Configurer les autorisations sur l’objet AdminSDHolder dans chaque domaine pour les comptes d’administration modifier l’appartenance des groupes privilégiés dans le domaine.  
  
Vous devez soigneusement tester l’ensemble de ces procédures et les modifier comme nécessaire pour votre environnement avant de les mettre en œuvre dans un environnement de production. Vous devez également vérifier que tous les paramètres fonctionnent comme prévu (certaines procédures de tests sont fournies dans cette annexe), et vous devez tester un scénario de récupération d’urgence dans lesquels les comptes de gestion ne sont pas disponibles pour être utilisés pour remplir les groupes protégés à des fins de récupération. Pour plus d’informations sur la sauvegarde et la restauration d’ActiveDirectory, voir la [de sauvegarde et récupération Step-by-Step Guide](https://technet.microsoft.com/library/cc771290(v=ws.10).aspx).  
  
> [!NOTE]  
> En mettant en œuvre les étapes décrites dans cette annexe, vous allez créer des comptes qui seront en mesure de gérer l’appartenance à tous les groupes protégés dans chaque domaine, pas uniquement les groupes ActiveDirectory de privilège le plus élevé, comme EAs, DAs et BAs. Pour plus d’informations sur les groupes protégés dans ActiveDirectory, voir [annexe c: des comptes protégés et groupes dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
#### <a name="step-by-step-instructions-for-creating-management-accounts-for-protected-groups"></a>Step-by-Step Instructions pour la création de comptes de gestion pour les groupes protégés  
  
##### <a name="creating-a-group-to-enable-and-disable-management-accounts"></a>Création d’un groupe pour activer et désactiver la gestion des comptes  
Comptes d’administration doivent avoir leurs mots de passe réinitialisées à chaque utilisation et doivent être désactivés lorsque les activités qu’ils ne soient terminées. Bien que vous pouvez également envisager la mise en œuvre de la configuration requise d’ouverture de session de carte à puce pour ces comptes, il est une configuration facultative, et ces instructions supposent que les comptes de gestion seront configurés avec un nom d’utilisateur et un mot de passe long et complexe que les contrôles minimales. Dans cette étape, vous allez créer un groupe qui dispose des autorisations pour réinitialiser le mot de passe sur les comptes de gestion et pour activer et désactiver les comptes.  
  
Pour créer un groupe pour activer et désactiver les comptes d’administration, effectuez les opérations suivantes:  
  
1.  Dans la structure d’unité d’organisation où vous serez hébergeant les comptes d’administration, cliquez sur l’unité d’organisation où vous souhaitez créer le groupe, cliquez sur **New** et cliquez sur **groupe**.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_115.png)  
  
2.  Dans le **nouvel objet - groupe** boîte de dialogue, entrez un nom pour le groupe. Si vous envisagez d’utiliser ce groupe pour tous les comptes d’administration dans votre forêt «activer», rendre un groupe universel de sécurité. Si vous disposez d’une forêt à domaine unique, ou si vous envisagez de créer un groupe dans chaque domaine, vous pouvez créer un groupe de sécurité global. Cliquez sur **OK** pour créer le groupe.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_116.png)  
  
3.  Cliquez sur le groupe que vous venez de créer, cliquez sur **propriétés**, puis cliquez sur le **objet** onglet. Dans du groupe **propriété de l’objet** boîte de dialogue, sélectionnez **protéger l’objet des suppressions accidentelles**, ce qui empêchera les utilisateurs autorisés de supprimer le groupe, mais également de le déplacer vers une autre unité d’organisation, sauf si le attribut est d’abord désélectionné.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_117.png)  
  
    > [!NOTE]  

    > Si vous avez déjà configuré les autorisations sur le parent du groupe d’unités d’organisation pour limiter l’administration à un ensemble limité d’utilisateurs, vous devrez peut-être pas effectuer les étapes suivantes. Ils sont fournis ici afin que même si vous n’avez pas encore implémenté un contrôle administratif limité sur la structure d’unité d’organisation dans laquelle vous avez créé ce groupe, vous pouvez sécuriser le groupe par rapport à la modification par les utilisateurs non autorisés.  
  
4.  Cliquez sur le **membres** onglet, puis ajoutez les comptes pour les membres de votre équipe qui seront chargés de l’activation de comptes d’administration ou le remplissage protégés groupes lorsque cela est nécessaire.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_118.png)  
  
5.  Si vous n'avez pas déjà fait, dans le **ActiveDirectory Users and Computers** de la console, cliquez sur **affichage** et sélectionnez **fonctionnalités avancées**. Cliquez sur le groupe que vous venez de créer, cliquez sur **propriétés**, puis cliquez sur le **sécurité** onglet. Sur le **sécurité**, cliquez sur **avancé**.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_119.png)  
  
6.  Dans le **Advanced Security Settings for [groupe]** boîte de dialogue, cliquez sur **désactiver l’héritage**. Lorsque vous y êtes invité, cliquez sur **convertir les autorisations héritées en autorisations explicites sur cet objet**, puis cliquez sur **OK** pour revenir à du groupe **sécurité** boîte de dialogue.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_120.png)  
  
7.  Sur le **sécurité** onglet, supprimez les groupes qui ne doivent pas être autorisés à accéder à ce groupe. Par exemple, si vous ne souhaitez pas que les utilisateurs authentifiés pour être en mesure de lire le nom et les propriétés générales du groupe, vous pouvez supprimer cette entrée. Vous pouvez également supprimer des entrées, telles que celles pour les opérateurs et les versions antérieures de Windows2000 Server compatible accès au compte. Vous devez, toutefois, laisser un ensemble minimal des autorisations d’objet en place. Toucher les entrées suivantes:  
  
    -   AUTOMATIQUE  
  
    -   SYSTÈME  
  
    -   Admins du domaine  
  
    -   Administrateurs de l’entreprise  
  
    -   Administrateurs  
  
    -   Groupe d’accès d’autorisation Windows (le cas échéant)  
  
    -   CONTRÔLEURS DE DOMAINE D’ENTREPRISE  
  
    Bien que cela peut sembler illogique pour permettre à des groupes privilégiés les plus élevés dans ActiveDirectory pour gérer ce groupe, votre objectif de l’implémentation de ces paramètres est ne pas d’empêcher les membres de ces groupes d’apporter des modifications autorisées. Au lieu de cela, l’objectif est de vous assurer que lorsque vous avez l’occasion pour demander des privilèges très élevés, modifications autorisées réussit. C’est pour cette raison que modification par défaut privilégié imbrication de groupes, droits et autorisations ne sont pas conseillées tout au long de ce document. En conservant les structures par défaut et vider l’appartenance des groupes de privilège le plus élevés dans le répertoire, vous pouvez créer un environnement plus sécurisé qui fonctionne toujours comme prévu.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_121.png)  
  
    > [!NOTE]  
    > Si vous n’avez pas déjà configuré les stratégies d’audit pour les objets dans la structure d’unité d’organisation où vous avez créé à ce groupe, vous devez configurer l’audit pour enregistrer les modifications de ce groupe.  
  
8.  Vous avez terminé la configuration du groupe qui vous servira à «découvrir» des comptes d’administration lorsqu’ils sont nécessaires et «archiver» les comptes lorsque leurs activités ont été effectuées.  
  
##### <a name="creating-the-management-accounts"></a>Créer les comptes d’administration  
Vous devez créer au moins un compte qui servira à gérer les membres des groupes privilégiés dans votre installation d’ActiveDirectory et préférence un deuxième compte pour servir de sauvegarde. Si vous choisissez de créer les comptes d’administration dans un domaine unique dans la forêt et leur accorder des fonctionnalités de gestion pour tous les domaines protégés de groupes, ou si vous choisissez d’implémenter des comptes de gestion dans chaque domaine dans la forêt, les procédures sont identiques.  
  
> [!NOTE]  
> Les étapes décrites dans ce document supposent que vous n'avez pas encore implémenté des contrôles d’accès basé sur les rôles et privileged identity management pour ActiveDirectory. Par conséquent, certaines procédures doivent être effectuées par un utilisateur dont le compte est membre du groupe Admins du domaine pour le domaine en question.  
>   
> Lorsque vous utilisez un compte avec des privilèges DA, vous pouvez vous connecter à un contrôleur de domaine pour effectuer les activités de configuration. Les étapes qui ne nécessitent pas de privilèges DA peuvent être effectuées par les comptes de moins de privilèges qui sont connectés à des stations de travail d’administration. Les captures d’écran qui affichent les boîtes de dialogue délimités de couleur bleu clair représentent des activités qui peuvent être effectuées sur un contrôleur de domaine. Les captures d’écran qui affichent les boîtes de dialogue de la couleur bleue plus foncée représentent des activités qui peuvent être effectuées sur des stations de travail d’administration avec les comptes qui disposent de privilèges limités.  
  
Pour créer les comptes de gestion, effectuez les opérations suivantes:  
  
1.  Ouvrez une session sur un contrôleur de domaine avec un compte qui est membre du groupe du domaine DA.  
  
2.  Lancer **ActiveDirectory Users and Computers** et accédez à l’unité d’organisation où vous allez créer le compte d’administration.  
  
3.  Cliquez sur l’unité d’organisation, cliquez sur **New** et cliquez sur **utilisateur**.  
  
4.  Dans le **nouvel objet - utilisateur** boîte de dialogue zone, entrez vos informations d’affectation de noms souhaitées pour le compte, cliquez sur **suivant**.  
  
5.  Ouvrez une session sur un contrôleur de domaine avec un compte qui est membre du groupe du domaine DA.  
  
6.  Lancer **ActiveDirectory Users and Computers** et accédez à l’unité d’organisation où vous allez créer le compte d’administration.  
  
7.  Cliquez sur l’unité d’organisation, cliquez sur **New** et cliquez sur **utilisateur**.  
  
8.  Dans le **nouvel objet - utilisateur** boîte de dialogue zone, entrez vos informations d’affectation de noms souhaitées pour le compte, cliquez sur **suivant**.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_122.png)  
  
9. Fournir un mot de passe initial pour le compte d’utilisateur, désactivez **utilisateur doit changer le mot de passe à la prochaine ouverture de session**, sélectionnez **utilisateur ne peut pas modifier le mot de passe** et **compte est désactivé**, puis cliquez sur **suivant**.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_123.png)  
  
10. Vérifiez que les informations de compte sont correctes, cliquez sur **Terminer**.  
  
11. Cliquez sur l’objet utilisateur que vous venez de créer, puis cliquez sur **propriétés**.  
  
12. Cliquez sur le **compte** onglet.  
  
13. Dans le **Options de compte** champ, sélectionnez le **compte est sensible et ne peut pas être délégué** indicateur, sélectionnez le **ce compte prend en charge le chiffrement Kerberos AES 128bits** et/ou le **ce compte prend en charge le chiffrement Kerberos AES 256** indicateur, puis cliquez sur **OK**.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_124.png)  
  
    > [!NOTE]  
    > Étant donné que ce compte, comme les autres comptes, aura une fonction limitée, mais puissante, le compte doit uniquement être utilisé sur des hôtes d’administration sécurisés. Pour tous les hôtes d’administration sécurisées dans votre environnement, vous devez envisager d’implémenter le paramètre de stratégie de groupe **sécurité réseau: les types de configurer le chiffrement autorisés pour Kerberos** pour autoriser uniquement les types de chiffrement plus sécurisés, vous pouvez implémenter pour les ordinateurs hôtes sécurisés.  
    >   
    > Implémentation des types de chiffrement plus sécurisés pour les ordinateurs hôtes ne limite pas les attaques de vol d’informations d’identification, l’utilisation appropriée et la configuration des ordinateurs hôtes sécurisés est le cas. Définition des types de chiffrement renforcés pour les ordinateurs hôtes qui ne sont utilisées par les comptes privilégiés simplement permet de réduire la surface d’attaque globale des ordinateurs.  
    >   
    > Pour plus d’informations sur la configuration des types de chiffrement sur les systèmes et les comptes, voir [Configurations Windows pour le Type de chiffrement pris en charge Kerberos](http://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx).  
    >   
    > **Remarque** ces paramètres sont pris en charge uniquement sur les ordinateurs exécutant Windows Server2012, Windows Server2008R2, Windows8 ou Windows7.  
  
14. Sur le **objet** onglet, sélectionnez **protéger l’objet des suppressions accidentelles**. Cela empêchera l’objet soient supprimés (et même par les utilisateurs autorisés), mais l’empêche de soient déplacées vers une autre unité d’organisation de votre hiérarchie de domaine ActiveDirectory, sauf si la case à cocher est désactivée tout d’abord par un utilisateur ayant l’autorisation de modifier l’attribut de.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_125.png)  
  
15. Cliquez sur le **contrôle à distance** onglet.  
  
16. Désactivez le **activer le contrôle à distance** indicateur. Il ne doit jamais être nécessaire pour le personnel du support pour se connecter à des sessions de ce compte pour implémenter des correctifs.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_126.png)  
  
    > [!NOTE]  
    > Chaque objet dans ActiveDirectory doit avoir un propriétaire désigné d’informatique et un propriétaire de l’entreprise désigné, comme décrit dans [planification des compromis](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md). Si vous suivez la propriété des objets de domaine ActiveDirectory dans ActiveDirectory (par opposition à une base de données externe), vous devez entrer les informations de propriété appropriées dans les propriétés de cet objet.  
    >   
    > Dans ce cas, le propriétaire de l’entreprise est très probablement une division informatique, andthere n’est aucune interdiction de propriétaires étant également les propriétaires de l’informatique. Le point de l’établissement de la propriété des objets est à vous permettre d’identifier les contacts lorsque les modifications doivent être apportées aux objets, peut-être ans à partir de leur création initiale.  
  
17. Cliquez sur le **organisation** onglet.  
  
18. Entrez les informations requises dans les normes de votre objet de domaine ActiveDirectory.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_127.png)  
  
19. Cliquez sur le **Dial-in** onglet.  
  
20. Dans le **autorisation d’accès réseau** champ, sélectionnez **refuser l’accès**. Ce compte ne doit jamais besoin de se connecter via une connexion à distance.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_128.png)  
  
    > [!NOTE]  
    > Il est peu probable que ce compte doit être utilisé pour vous connecter à des contrôleurs de domaine en lecture seule (RODC) dans votre environnement. Toutefois, doit circonstance jamais requièrent que le compte pour vous connecter une session sur un RODC, vous devez ajouter ce compte pour le groupe de réplication de mot de passe RODC refusée afin que son mot de passe n’est pas mis en cache sur le RODC.  
    >   
    > Bien que le mot de passe doit être réinitialisé après chaque utilisation et le compte doit être désactivé, mise en œuvre de ce paramètre n’a pas un effet nuisible sur le compte, et cela peut vous aider dans les cas dans lesquels un administrateur oublie de réinitialiser le mot de passe du compte et de le désactiver.  
  
21. Cliquez sur le **membre de** onglet.  
  
22. Cliquez sur **ajouter**.  
  
23. Type **groupe de réplication de mot de passe RODC refusée** dans les **sélectionnez utilisateurs, Contacts, ordinateurs** boîte de dialogue, puis cliquez sur **vérifier les noms**. Lorsque le nom du groupe est souligné dans le sélecteur d’objets, cliquez sur **OK** et vérifiez que le compte est désormais un membre des deux groupes affichées dans la capture d’écran suivante. N’ajoutez pas le compte à un groupe protégé.  
  
24. Cliquez sur **OK**.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_129.png)  
  
25. Cliquez sur le **sécurité** onglet et cliquez sur **avancé**.  
  
26. Dans le **paramètres de sécurité avancés** boîte de dialogue, cliquez sur **désactiver l’héritage** et copier les autorisations héritées en autorisations explicites, puis cliquez sur **ajouter**.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_130.png)  
  
27. Dans le **entrée d’autorisation pour [compte]** boîte de dialogue, cliquez sur **sélectionnez un principal** et ajoutez le groupe que vous avez créé dans la procédure précédente. Faites défiler vers le bas de la boîte de dialogue, puis cliquez sur **Effacer tout** pour supprimer toutes les autorisations par défaut.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_131.png)  
  
28. Faites défiler vers le haut de la **entrée d’autorisation** boîte de dialogue. Vérifiez que le **Type** liste déroulante est définie sur **autoriser**et dans le **s’applique aux** la liste déroulante, sélectionnez-le **cet objet uniquement**.  
  
29. Dans le **autorisations** champ, sélectionnez **lire toutes les propriétés**, **les autorisations de lecture**, et **réinitialisation de mot de passe**.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_132.png)  
  
30. Dans le **propriétés** champ, sélectionnez **lire userAccountControl** et **écrire userAccountControl**.  
  
31. Cliquez sur **OK**, **OK** à nouveau dans le **paramètres de sécurité avancés** boîte de dialogue.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_133.png)  
  
    > [!NOTE]  
    > Le **userAccountControl** attribut contrôle plusieurs options de configuration de compte. Vous ne pouvez pas autorisation à modifier uniquement certaines des options de configuration lorsque vous accordez l’accès en écriture à l’attribut.  
  
32. Dans le **noms d’utilisateur ou groupe** champ de la **sécurité** onglet, supprimez les groupes qui ne doivent pas être autorisés à accéder ou gérer le compte. Ne supprimez pas tous les groupes qui ont été configurés avec les entrées ACE refusées, telles que le groupe tout le monde et le libre-service calculées compte (cette entrée a été définie lorsque la **utilisateur ne peut pas modifier le mot de passe** indicateur a été activée lors de la création du compte. Également ne supprimez pas le groupe que vous venez d’ajouter, le compte système ou des groupes tels que EA, DA, BA ou le groupe d’accès d’autorisation Windows.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_134.png)  
  
33. Cliquez sur **avancé** et vérifiez que la boîte de dialogue Paramètres de sécurité avancés est similaire à la capture d’écran suivante.  
  
34. Cliquez sur **OK**, et **OK** nouveau pour fermer la boîte de dialogue Propriétés du compte.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_135.png)  
  
35. Le programme d’installation du premier compte d’administration est maintenant terminée. Vous allez tester le compte dans une procédure ultérieure.  
  
###### <a name="creating-additional-management-accounts"></a>Création de comptes d’administration supplémentaire.  
Vous pouvez créer des comptes d’administration supplémentaires en répétant les étapes précédentes, en copiant le compte que vous venez de créer ou en créant un script pour créer des comptes avec vos paramètres de configuration souhaitée. Toutefois, notez que si vous copiez le compte que vous venez de créer, la plupart des paramètres personnalisés et ACL ne seront pas copiées vers le nouveau compte et vous devez répéter la plupart des étapes de configuration.  
  
Vous pouvez créer un groupe auquel vous déléguez les droits à remplir et unpopulate groupes protégés, mais vous devez sécuriser le groupe et les comptes que vous placez dans celui-ci. Dans la mesure où il doit y avoir très peu de comptes dans votre annuaire qui est accordées à la possibilité de gérer les membres de groupes protégés, la création de comptes individuels peut être l’approche la plus simple.  
  
Quelle que soit la façon dont vous choisissez Créer un groupe dans lequel vous placez les comptes d’administration, vous devez vous assurer que chaque compte est sécurisé comme décrit précédemment. Vous devez également envisager la mise en œuvre des restrictions de stratégie de groupe similaires à ceux décrits dans [annexe d: sécurisation des comptes d’administrateur intégrés dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
###### <a name="auditing-management-accounts"></a>Gestion des comptes d’audit  
Vous devez configurer l’audit sur le compte pour vous connecter, au minimum, toutes les écritures dans le compte. Cette option permet de que vous non seulement identifier l’activation du compte et de réinitialiser son mot de passe pendant les utilisations autorisées, mais également identifier les tentatives par les utilisateurs non autorisés de manipuler le compte réussie. Échec des écritures sur le compte doivent être capturées dans votre système les informations de sécurité et de surveillance événements (SIEM) (le cas échéant) et doivent déclencher des alertes qui fournissent une notification à l’équipe responsable de l’examen des violations potentielles.  
  
Les solutions SIEM prennent des informations sur l’événement à partir de sources d’impliquée sécurité (par exemple, les journaux des événements, données d’application, flux de réseau, produits anti-programme malveillant et sources de détection des intrusions), la collecte des données et tentent d’apporter des vues intelligents et des mesures proactives. Il existe de nombreuses solutions SIEM commerciales et de nombreuses entreprises créer implémentations privées. Un SIEM bien conçue et correctement implémentée peut considérablement améliorer les fonctionnalités de réponse aux incidents et de surveillance de la sécurité. Toutefois, fonctionnalités et précision varier considérablement entre les solutions. Solutions SIEM est dépassent le cadre de ce document, mais les recommandations de l’événement spécifique contenues font par n’importe quel implémenteur SIEM.  
  
Pour plus d’informations sur les paramètres de configuration recommandée d’audit pour les contrôleurs de domaine, voir [analyse ActiveDirectory pour les signes de compromission](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). Paramètres de configuration spécifique du contrôleur de domaine sont fournies dans [analyse ActiveDirectory pour les signes de compromission](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md).  
  
##### <a name="enabling-management-accounts-to-modify-the-membership-of-protected-groups"></a>L’activation de la gestion des comptes modifier l’appartenance aux groupes protégés  
Dans cette procédure, vous allez configurer les autorisations sur l’objet AdminSDHolder du domaine pour autoriser les comptes d’administration nouvellement créé modifier l’appartenance aux groupes protégés dans le domaine. Cette procédure ne peut pas être effectuée via une interface graphique utilisateur (GUI).  
  
Comme indiqué dans [annexe c: des comptes protégés et groupes dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md), la liste ACL sur AdminSDHolder d’un domaine objet est effectivement «copié» pour les objets protégés lorsque la tâche SDProp s’exécute. Comptes et groupes protégés n’héritent pas leurs autorisations de l’objet AdminSDHolder; les autorisations sont explicitement définies pour correspondre à celles de l’objet AdminSDHolder. Par conséquent, lorsque vous modifiez les autorisations sur l’objet AdminSDHolder, vous devez les modifier pour les attributs qui sont appropriés pour le type de l’objet protégé que vous ciblez.  
  
Dans ce cas, vous autorisez les comptes d’administration nouvellement créé pour permettre à lire et écrire les membres de l’attribut sur les objets de groupe. Toutefois, l’objet AdminSDHolder n’est pas un objet de groupe et attributs d’un groupe ne sont pas exposées dans l’éditeur ACL graphique. C’est pour cette raison que vous allez implémenter les modifications d’autorisations via l’utilitaire de ligne de commande Dsacls. Pour accorder des autorisations des comptes pour modifier l’appartenance aux groupes protégés la gestion (désactivée), procédez comme suit:  
  
1.  Ouvrez une session sur un contrôleur de domaine, de préférence, le contrôleur de domaine détenant le rôle d’émulateur de contrôleur de domaine principal (PDCE), avec les informations d’identification d’un compte d’utilisateur qui a été effectué un membre du groupe DA dans le domaine.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_136.png)  
  
2.  Ouvrez une invite de commandes avec élévation de privilèges en cliquant sur **invite de commandes** et cliquez sur **exécuter en tant qu’administrateur**.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_137.gif)  
  
3.  Lorsque vous êtes invité à approuver l’élévation, cliquez sur **Oui**.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_138.gif)  
  
    > [!NOTE]  
    > Pour plus d’informations sur l’utilisateur et élévation de contrôle de compte (UAC) dans Windows, voir [processus et Interactions UAC](https://technet.microsoft.com/library/dd835561(v=WS.10).aspx) sur le site Web TechNet.  
  
4.  À l’invite de commandes, tapez (en remplaçant vos informations spécifiques à un domaine) **Dsacls [nom unique de l’objet AdminSDHolder dans votre domaine] /G [compte d’administration UPN]: RPWP; membre**.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_139.gif)  
  
    La commande précédente (qui ne respecte pas la casse) fonctionne comme suit:  
  
    -   DSACLS définit ou affiche les entrées sur les objets ActiveDirectory  
  
    -   CN = AdminSDHolder, CN = System, DC = TailSpinToys, DC = msft identifie l’objet à modifier  
  
    -   /G indique qu’un as grant est configuré  
  
    -   PIM001@tailspintoys.msftest le nom Principal d’utilisateur (UPN) de l’entité de sécurité qui auront ACE  
  
    -   Accorde RPWP propriété de lecture et en écriture de propriété  
  
    -   Membre est le nom de la propriété (attribut) sur lequel les autorisations sont définies  
  
    Pour plus d’informations sur l’utilisation de **Dsacls**, tapez Dsacls sans aucun paramètre à une invite de commandes.  
  
    Si vous avez créé plusieurs comptes d’administration pour le domaine, vous devez exécuter la commande Dsacls pour chaque compte. Lorsque vous avez terminé la configuration de la liste ACL sur l’objet AdminSDHolder, vous devez forcer SDProp exécuter ou attendre la fin de son exécution planifiée. Pour plus d’informations sur Forcer SDProp à exécuter, voir «En cours d’exécution SDProp Manually» dans [annexe c: des comptes protégés et groupes dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
    Lorsque SDProp a exécuté, vous pouvez vérifier que les modifications apportées à l’objet AdminSDHolder ont été appliquées aux groupes protégés dans le domaine. Vous ne pouvez pas le vérifier en affichant la liste ACL sur l’objet AdminSDHolder pour les raisons décrites précédemment, mais vous pouvez vérifier que les autorisations ont été appliquées en consultant les ACL sur les groupes protégés.  
  
5.  Dans **ActiveDirectory Users and Computers**, vérifiez que vous avez activé **fonctionnalités avancées**. Pour ce faire, cliquez sur **affichage**, recherchez le **Admins du domaine** de groupe, cliquez sur le groupe, cliquez sur **propriétés**.  
  
6.  Cliquez sur le **sécurité** onglet et cliquez sur **avancé** pour ouvrir le **paramètres de sécurité avancés pour Admins du domaine** boîte de dialogue.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_140.gif)  
  
7.  Sélectionnez **ACE autorisée pour le compte d’administration** et cliquez sur **modifier**. Vérifier que le compte a été accordé uniquement **lecture des membres** et **écriture des membres** autorisations sur le groupe DA, puis cliquez sur **OK**.  
  
8.  Cliquez sur **OK** dans les **paramètres de sécurité avancés** boîte de dialogue, puis cliquez sur **OK** nouveau pour fermer la boîte de dialogue Propriétés pour le groupe DA.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_141.gif)  
  
9. Vous pouvez répéter les étapes précédentes pour d’autres groupes protégés dans le domaine. les autorisations doivent être les mêmes pour tous les groupes protégés. Vous avez maintenant terminé la création et la configuration des comptes d’administration pour les groupes protégés dans ce domaine.  
  
    > [!NOTE]  
    > N’importe quel compte qui a l’autorisation d’écrire l’appartenance à un groupe dans ActiveDirectory peut également lui-même ajouter au groupe. Ce comportement est normal et ne peut pas être désactivé. Pour cette raison, vous devez toujours garder désactivés lors de pas l’utilisation des comptes de gestion et devez surveiller attentivement les comptes lorsqu’ils sont désactivés et lorsqu’ils sont en cours d’utilisation.  
  
##### <a name="verifying-group-and-account-configuration-settings"></a>Vérification des paramètres de Configuration de compte et de groupe  
Maintenant que vous avez créé et configuré des comptes de gestion qui peuvent modifier l’appartenance aux groupes protégés dans le domaine (qui inclut les groupes dotés de privilèges plus élevés EA et DA BA), vous devez vérifier que les comptes et leur groupe d’administration ont été créés correctement. Vérification consiste à ces tâches générales:  
  
1.  Tester le groupe qui peut activer et désactiver les comptes d’administration pour vérifier que les membres de groupe peut activent et désactiver les comptes et réinitialiser leurs mots de passe, mais ne peut pas effectuer d’autres activités d’administration sur les comptes de gestion.  
  
2.  Les comptes de gestion pour vérifier qu’ils peuvent ajouter et supprimer des membres à protégé des groupes dans le domaine, mais ne peut pas modifier d’autres propriétés des comptes protégés et des groupes de test.  
  
###### <a name="test-the-group-that-will-enable-and-disable-management-accounts"></a>Le groupe de s’activer et désactiver la gestion des comptes de test  
  
1.  Pour tester l’activation d’un compte d’administration et de réinitialiser son mot de passe, connectez-vous à une station d’administration sécurisée avec un compte qui est membre du groupe vous avez créé dans [annexe i: création de gestion des comptes pour des comptes protégés et les groupes dans ActiveDirectory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_142.gif)  
  
2.  Ouvrez **ActiveDirectory Users and Computers**, cliquez sur le compte d’administration, puis cliquez sur **activer le compte**.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_143.gif)  
  
3.  Une boîte de dialogue doit afficher, confirmant que le compte a été activé.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_144.gif)  
  
4.  Ensuite, réinitialiser le mot de passe sur le compte d’administration. Pour ce faire, cliquez à nouveau sur le compte, puis cliquez sur **réinitialiser le mot de passe**.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_145.gif)  
  
5.  Tapez un nouveau mot de passe pour le compte dans le **nouveau mot de passe** et **confirmer le mot de passe** champs, puis cliquez sur **OK**.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_146.gif)  
  
6.  Une boîte de dialogue doit apparaître, confirmant que le mot de passe pour le compte a été réinitialisé.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_147.gif)  
  
7.  Tenter de modifier d’autres propriétés du compte d’administration. Cliquez sur le compte, puis cliquez sur **propriétés**, puis cliquez sur le **contrôle à distance** onglet.  
  
8.  Sélectionnez **activer le contrôle à distance** et cliquez sur **appliquer**. L’opération échoue et un **accès refusé** message d’erreur doit s’afficher.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_148.gif)  
  
9. Cliquez sur le **compte** onglet pour le compte et essayez de modifier le nom du compte, les heures d’ouverture de session ou les stations de travail d’ouverture de session. Tous les doivent échouer et le compte d’options qui ne sont pas contrôlées par le **userAccountControl** attribut doit être grisée et non disponible pour la modification.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_149.gif)  
  
10. Essayez d’ajouter le groupe d’administration à un groupe protégé tels que le groupe DA. Lorsque vous cliquez sur **OK**, un message doit s’afficher, vous informant que vous ne disposez pas des autorisations pour modifier le groupe.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_150.gif)  
  
11. Effectuer des tests supplémentaires nécessaires pour vérifier que vous ne pouvez pas configurer quoi que ce soit sur le compte d’administration à l’exception **userAccountControl** paramètres et les réinitialisations de mot de passe.  
  
    > [!NOTE]  
    > Le **userAccountControl** attribut contrôle plusieurs options de configuration de compte. Vous ne pouvez pas autorisation à modifier uniquement certaines des options de configuration lorsque vous accordez l’accès en écriture à l’attribut.  
  
###### <a name="test-the-management-accounts"></a>Les gestion des comptes de test  
Maintenant que vous avez activé un ou plusieurs comptes que vous peuvent modifier l’appartenance aux groupes protégés, vous pouvez tester les comptes pour vous assurer qu’ils peuvent modifier l’appartenance au groupe protégé, mais ne peut pas effectuer les autres modifications sur les comptes protégés et groupes.  
  
1.  Ouvrez une session sur un ordinateur hôte d’administration sécurisé en tant que le premier compte d’administration.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_151.gif)  
  
2.  Lancer **ActiveDirectory Users and Computers** et recherchez le **groupe Admins du domaine**.  
  
3.  Cliquez sur le **Admins du domaine** de groupe, puis cliquez sur **propriétés**.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_152.gif)  
  
4.  Dans le **propriétés d’Admins du domaine**, cliquez sur le **membres** onglet et **cliquez** ajouter. Entrez le nom d’un compte qui reçoivent des privilèges d’administrateurs du domaine temporaires et cliquez sur **vérifier les noms**. Lorsque le nom du compte est souligné, cliquez sur **OK** pour revenir à la **membres** onglet.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_153.gif)  
  
5.  Sur le **membres** onglet pour le **propriétés d’Admins du domaine** boîte de dialogue, cliquez sur **appliquer**. Après avoir cliqué sur **appliquer**, le compte doit rester un membre du groupe DA et vous ne devriez recevoir aucun message d’erreur.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_154.gif)  
  
6.  Cliquez sur le **géré par** onglet dans le **propriétés d’Admins du domaine** boîte de dialogue zone et vérifiez que vous ne pouvez pas entrer du texte dans les champs et tous les boutons sont grisés.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_155.gif)  
  
7.  Cliquez sur le **général** onglet dans le **propriétés d’Admins du domaine** boîte de dialogue zone et vérifiez que vous ne pouvez pas modifier les informations sur cet onglet.  
  
    ![Création de comptes d’administration](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_156.gif)  
  
8.  Répétez ces étapes pour les groupes protégés supplémentaires en fonction des besoins. Lorsque vous avez terminé, ouvrez une session sur un hôte d’administration sécurisé avec un compte qui est membre du groupe que vous avez créé pour activer et désactiver les comptes de gestion. Réinitialisez le mot de passe sur le compte d’administration vous venez de Testez et de désactivez le compte. Vous avez terminé le programme d’installation, les comptes de gestion et du groupe qui sera chargé de l’activation et désactivation de comptes.  
  


