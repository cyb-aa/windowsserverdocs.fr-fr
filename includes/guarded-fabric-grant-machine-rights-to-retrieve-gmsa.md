1. Avoir un administrateur de services Active directory à ajouter le compte d’ordinateur pour votre nouveau nœud au groupe de sécurité qui contient tous les serveurs SGH qui est autorisé à autoriser ces serveurs utiliser le compte de service administré de groupe SGH.

2. Redémarrez le nouveau nœud pour obtenir un nouveau ticket Kerberos qui inclut l’appartenance de l’ordinateur dans ce groupe de sécurité. Une fois le redémarrage terminé, connectez-vous à une identité de domaine qui appartient au groupe Administrateurs local sur l’ordinateur.

3. Installer le compte de service administrés de groupe SGH sur le nœud.

   ```powershell
   Install-ADServiceAccount -Identity <HGSgMSAAccount>
   ```
