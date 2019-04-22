Si vous avez fourni tous les certificats à SGH à l’aide d’empreintes numériques, vous serez invité à accorder un accès en lecture à la clé privée de ces certificats SGH. Sur un serveur avec la fonctionnalité Expérience Bureau installée, procédez comme suit :

1.  Ouvrez le Gestionnaire de certificats ordinateur local (**certlm.msc**)
2.  Rechercher l’ou les certificats > avec le bouton droit > toutes les tâches > Gérer les clés privées
3.  Cliquez sur **Ajouter**.
4.  Dans la fenêtre du sélecteur objet, cliquez sur **types d’objet** et activer **comptes de service**
5.  Entrez le nom du compte de service mentionné dans le texte d’avertissement à partir de `Initialize-HgsServer`
6.  Vérifiez que le compte gMSA dispose d’un accès « Lecture » à la clé privée.

Sur server core, vous devez télécharger un module PowerShell pour vous aider à définir les autorisations de clé privées.

1.  Exécutez `Install-Module GuardedFabricTools` sur le serveur SGH s’il a une connectivité Internet, ou exécutez `Save-Module GuardedFabricTools` sur un autre ordinateur et copie le module sur le serveur SGH.
2.  Exécutez `Import-Module GuardedFabricTools`. Cette opération ajoute des propriétés supplémentaires pour les objets de certificat se trouvent dans PowerShell.
3.  Rechercher votre empreinte de certificat dans PowerShell avec `Get-ChildItem Cert:\LocalMachine\My`
4.  Mise à jour de l’ACL, en remplaçant l’empreinte numérique par les vôtres et le compte de service administré de groupe dans le code ci-dessous avec le compte répertorié dans le texte d’avertissement de `Initialize-HgsServer`.

    ```powershell
    $certificate = Get-Item "Cert:\LocalMachine\1A2B3C..."
    $certificate.Acl = $certificate.Acl | Add-AccessRule "HgsSvc_1A2B3C" Read Allow
    ```

Si vous utilisez des certificats reposant sur les HSM ou les certificats stockés dans un fournisseur de stockage de clés tiers, ces étapes peuvent s’appliquent pas à vous. Consultez la documentation de votre fournisseur de stockage de clés pour savoir comment gérer les autorisations sur votre clé privée. Dans certains cas, il n’y a aucune autorisation, ou l’autorisation est fournie pour l’ensemble de l’ordinateur lorsque le certificat est installé.

<!-- Appears in guarded-fabric-initialize-hgs-ad-mode-default.md and guarded-fabric-initialize-hgs-tpm-mode-default.md
-->