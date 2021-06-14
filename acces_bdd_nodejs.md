#Accès a une BDD depuis Node

## 1 Téléchargements nécessaires
1. SQL Server 2019 Developer Edition [ici](https://www.microsoft.com/fr-fr/sql-server/sql-server-downloads)
1. SQL Server Management Studio (SSMS) [ici](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)
2. Node.js [ici](https://nodejs.org/en/download/)
## 2 Configuration de SSMS
On lance SSMS et on peux se logger en mode **"Authentification Windows"** et on rentre le nom du serveur (ici localhost)
![GitHub Logo](https://i.ibb.co/mXZb774/1.png)

Ensuite clique droit sur notre serveur et **"Propriétés"**
![GitHub Logo](https://i.ibb.co/dB57nTn/2.png)

Dans la page **"Sécurité"** il faut cocher **"Mode d'authentification SQL Server et Windows"**
Puis **"OK"**
![GitHub Logo](https://i.ibb.co/TkHMBtC/3.png)

Ensuite il faut changer le mdp de l'utilisateur admin, pour cela il faut se rendre dans **"Sécurité"** puis **"Connexions"** et clique droit sur **"sa"**
![GitHub Logo](https://i.ibb.co/KVFKZCS/5.png)

Ensuite il faut d'abord activer et autoriser la connexion dans la page **"Etat"**
![GitHub Logo](https://i.ibb.co/ZXfgCRG/7.png)

On peut ensuite aller sur la page **"Général"** pour changer le mdp
![GitHub Logo](https://i.ibb.co/8dH9k6t/6.png)

Vous pourrez normalement vous connecter en utilisant **"Authentification SQL Server"** avec **"sa"** en connexion et votre mdp rentré précédemment
![GitHub Logo](https://i.ibb.co/mDgvDWJ/8.png)
## 3 Configuration du protocole TCP/IP
Appuyez sur Touche Windows+R simultanément pour ouvrir l'Executer windows et rentrez _**compmgmt.msc**_ comme ci-dessous puis "OK"
![GitHub Logo](https://i.ibb.co/KrBhGYS/9.png)

Nous voila dans la Gestion de l'ordinateur, cliquez sur **"Services et applications"** puis **"Gestionnaire de configuration SQL Server"**
![GitHub Logo](https://i.ibb.co/C8dnVVH/10.png)

Puis cliquez sur **"Configuration du réseau SQL Server"**
![GitHub Logo](https://i.ibb.co/nPqs017/11.png)

Puis cliquez sur **"Protocoles pour MSSQLSERVER"**
![GitHub Logo](https://i.ibb.co/rpjNmkJ/12.png)

Faites clique droit sur TCP/IP et cliquez sur Activer
![GitHub Logo](https://i.ibb.co/0MRRsbT/13.png)

## 4 Redémarrage du service SQL
Ecrivez _**Services**_ dans la recherche windows et ouvrez l'application comme ci-dessous
![GitHub Logo](https://i.ibb.co/qCHqG0x/14.png)

Déscendez pas mal vers le bas, selectionnez **"SQL Server (MSSQLSERVER)"** et cliquez sur redémarrer
![GitHub Logo](https://i.ibb.co/wc9fKMq/15.png)

Ensuite il faut voir si le **"SQL Server Browser"** est en route pour ça on clique droit dessus puis propriétés
![GitHub Logo](https://i.ibb.co/3mmJTGC/16.png)

Sélectionnez le type de démarrage **"Automatique"**
![GitHub Logo](https://i.ibb.co/dcSmC2R/17.png)
## 5 Vérification que tout les services requits sont en route
Comme au dessus appuyez sur Touche Windows+R simultanément pour ouvrir l'Executer windows et rentrez _**compmgmt.msc**_ comme ci-dessous puis **"OK"**
![GitHub Logo](https://i.ibb.co/KrBhGYS/9.png)

Puis allez dans **"Services et applications"** puis **"Gestionnaire de configuration SQL Server"**
![GitHub Logo](https://i.ibb.co/C8dnVVH/10.png)

Cliquez sur **"Services SQL Server"**
![GitHub Logo](https://i.ibb.co/PghkHYM/18.png)

Verifiez que les trois services sont en cours d'éxecution, sinon faite clique droit et **"démarrer"** sur ceux qui ne le sont pas
![GitHub Logo](https://i.ibb.co/9wBDY5k/19.png)

Vous pouvez maintenant créer ou joindre une base de données
![GitHub Logo](https://i.ibb.co/qyZShtT/4.png)
## 5 Connection à la DB depuis Node
Pour se connecter à sa BDD avec Node il nous faut utiliser les packages nodeJs **“mssql”** et **“express”**
Pour ça on ouvre un terminal dans le répertoire ou l’on veut faire notre app et on rentre : 
_**npm install mssql**_
_**npm install express**_

Ensuite on peut créer notre fichier (ici app.js).

Vous devriez avoir deux fichiers et un répertoire :
![GitHub Logo](https://i.ibb.co/611MC7b/20.png)

Ensuite vous pouvez prendre ce code en remplacant ma config par celle de votre BDD et utilisateur, rentrez votre requête
```javascript
var express = require('express');
var app = express();

app.get('/', function (req, res) {
   
    var sql = require("mssql");

    // config pour votre base de données
    var config = {
        user: 'sa',
        password: '<votre MDP>',
        server: 'localhost', 
        database: '<nom de votre BDD>' 
    };

    // connectez-vous à votre base de données
    sql.connect(config, function (err) {
    
        if (err) console.log(err);

        // créer un objet de requête
        var request = new sql.Request();
           
        // interroger la base de données et obtenir le recordset
        request.query('<votre requète>', function (err, recordset) {
            
            if (err) console.log(err)

            // envoyer les records en réponse
            res.send(recordset);
            
        });
    });
});

var server = app.listen(5000, function () {
    console.log('Server is running..');
});
```
Puis il vous suffit de faire _**node app.js**_ dans le terminal (pour ma part c'était server.js)
![GitHub Logo](https://i.ibb.co/HD0Qmbj/21.png)

Et lorsque vous allez à l'adresse [http://localhost:5000/](http://localhost:5000/)
Vous pouvez voir le résultat de votre requête.
![GitHub Logo](https://i.ibb.co/W6kscq0/22.png)
