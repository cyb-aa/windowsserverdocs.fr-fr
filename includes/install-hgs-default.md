Le Service Guardian hôte doit être installé dans une autre forêt Active Directory.
Assurez-vous que l’ordinateur de SGH est **pas** joint à un domaine avant de commencer et connectez-vous en tant que l’ordinateur local de l’outil.

Exécutez les commandes suivantes pour installer le Service Guardian hôte et configurer son domaine.
Le mot de passe que vous spécifiez ici s’applique uniquement au mot de passe du Mode de réparation des Services annuaire pour Active Directory ; Il sera *pas* modifier le mot de passe de connexion de votre compte d’administrateur.
Vous pouvez fournir n’importe quel nom de domaine de votre choix pour - HgsDomainName.

```powershell
$adminPassword = ConvertTo-SecureString -AsPlainText '<password>' -Force

Install-HgsServer -HgsDomainName 'bastion.local' -SafeModeAdministratorPassword $adminPassword -Restart
```

<!-- Appears in guarded-fabric-install-hgs-default.md and set-up-hgs-for-always-encrypted-in-sql-server.md
-->
