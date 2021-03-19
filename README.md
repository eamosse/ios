# UITableViewController
Dans ce TP, vous allez apprendre à manipuler les TableView. En iOS, les TableViews sont utilisés pour afficher une collection d'objets sous forme de liste scrollable. 

![](image1.png?raw=true)

## Créer une nouvelle application 

1. Sur Xcode, créez une nouvelle application, appelez-la TP2_Groupe_[Numero] (où Numero représente le numéro de votre groupe)

2. Ajoutez un UiTableViewController dans le storyboard
> View > Show Library > Recherchez Table > Selectionnez UITableViewController

![](image2.png?raw=true)

3. Vous devriez avoir 2 écrans dans storyboard, cliquez sur le premier et appuyez sur le bouton back de votre clavier pour le supprimer

4. Configurez la UITableViewController comme vue par défault. 
 ![](image3.png?raw=true)

5. Dans le panel de gauche, cliquez sur TableView

6. Dans le panel de droite, Changez le type de Content en Static Cells et Style en Grouped
![](image4.png?raw=true)

7. Si vous déployez TableViewCell vous retrouverez trois cellules vide.

8. Cliquez sur TableViewSection (dans le panel de gauche), et dans le panel de droite écrivez Coutries dans la valeur de Header

9. Selectionnez une à une les cellules (sous Countries) et changez leur Style en Basic. 
> Une cellule basique à un Label par défaut

10. Modifiez les label de chaque cellule comme suit : 
- Cellule 1: France
- Cellule 2: Allemagne
- Cellule 3: Belgique

![](image5.png?raw=true)

11. Compilez. 
> Vous devriez avoir un tableau avec trois cellueles éléments. 

## Ajoutez les cellules dynamiquement
Dans cette section, vous allez apprendre comment ajouter dynamiquement des celluels dans une table 

1. Ajoutez un ViewController pour gérer la UITableView 
File > New File > Cocoa Touch Class
![](image6.png?raw=true)

2. Nommez le nouveau fichier UITableViewController, Dans l'option Subclass Of, choisissez UITableViewController et dans Language choisissez Swift. 
![](image7.png?raw=true)

3. Associez la UITableView du storyboard à son view controller
- Ouvrez le storyboard 
- Cliquez sur TableViewController dans le panel de gauche 
- Selectionnez l'onglet identiry dans le panel de droite 
- Choisissez le nom de la classe que vous avez créé dans la section précédente 
![](image8.png?raw=true)

4. Dans le panel de gauche, cliquez sur TableView et changez content (panel de droite) en Dynamic Prototypes
et style a Plain 
![](image9.png?raw=true)

5. Changer le nombre de prototype cells de 3 à 1

6. Selectionnez le prototype cell (panel de gauche), entrez la valeur CountryCell dans la section Identifier (panel de droite)

7. Configurez une source de données pour la UITableView

> Ouvrez le fichier CountriesTableViewController

- Modifiez la méthode numberOfSections pour qu'elle retourne le nombre de section (3) et le nombre de ligne à afficher dans chaque section (5)

 ```Swift
override func numberOfSections(in tableView: UITableView) -> Int {
        return 3
}

override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return 5
    }
```

8. Decommentez la méthode tableView:cellForRowAtIndexPath et modifiez-la pour qu'elle retourne une cellule à afficher dans la table view.
> Remplacez ```reuseIdentifier``` par ```countryCell```, savez-vous pourquoi ? 

```Swift
override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {

    let cell = tableView.dequeueReusableCell(withIdentifier: "CountryCell", for: indexPath)

    cell.textLabel?.text = "Section \(indexPath.section) Row \(indexPath.row)"

    return cell
}
```

9. Surchargez la méthode ```tableView:titleForHeaderInSection``` pour définir un titre pour chaque section
> Le titre d'une section doit ressembler à ```Section [Number]``` où Number est le numéro de section.

10. Testez
![](image10.png?raw=true)


## 3. Gestion des données 
Dans cette section, nous allons modifier la datasource pour générer une liste de pays à afficher dans la Table View. 

1. Crér le modèle de données
- Cliquez droit sur le dossier principal du projet et choisissez New Group; nommez le groupe models. 

![](image11.png?raw=true)

- Cliquez droit sur models > Add File > Swift et nommez le fichier Country

```Swift
struct Country {
    var isoCode: String
    var name: String
}
``` 
- Ajoutez un nouveau Group à la racine du projet appelez-le data. Ajoutez un nouveau fichier Countries.swift dans le group data. 
Créez un tableau de pays dans Countries. 

```Swift
let countries = [
    Country(isoCode: "at", name: "Austria"),
    Country(isoCode: "be", name: "Belgium"),
    Country(isoCode: "de", name: "Germany"),
    Country(isoCode: "el", name: "Greece"),
    Country(isoCode: "fr", name: "France"),
]
``` 

2. Mofiez le controller pour retourner une liste de pays. 

```Swift
override func numberOfSections(in tableView: UITableView) -> Int {
        return 1
    }

    override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return countries.count
    }

    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {

        let cell = tableView.dequeueReusableCell(withIdentifier: "CountryCell", for: indexPath)
        
        let country = countries[indexPath.row]
        cell.textLabel?.text = country.name

        return cell
    }
```

3. Compilez et testez
![](image12.png?raw=true)

4. Ajoutez quelques pays dans la liste puis retestez sans modifier les sources du controller. 
Que remarquez-vous ? 

5. Avez-vous remarqué les cellules vides ? Faites en sortes qu'elles n'apparaissent pas. 

## 4. Personnalisez les cellules 
Dans cette section, nous allons modifier les cellules de la table pour afficher les codes iso des pays en plus de leurs noms. 

1. Il y a quatre types de cellules : Detail(gauche, droite) et Subtitle. 
![](image13.png?raw=true)

2. Changez le type des cellules de la table en Detail

3. Définissez une hauteur fixe pour les cellules. 
![](image14.png?raw=true)

4. <a id="raw-url" href="https://github.com/ralfebert/country-images/archive/master.zip">Téléchargez</a> une liste d'images pour chaque pays.
> Décompressez le fichier .zip 

5. Dans les fichiers du projet, selectionnez ```Assets.xcassets``` puis draguez les images dans le panel de gauche
![](image15.png?raw=true)

6. Modifiez l'implémentation de la méthode ```tableView(cellForRowAt:) ``` pour ajouter le code du pays ainsi qu'une image dans la cellule. 

```Swift
override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let cell = tableView.dequeueReusableCell(withIdentifier: "CountryCell", for: indexPath)
            
    let country = countries[indexPath.row]
    cell.textLabel?.text = country.name
    cell.detailTextLabel?.text = country.isoCode
    cell.imageView?.image = UIImage(named: country.isoCode)
    return cell
}

```

7. Compilez et testez.
> Il faudra peut-être ajouter des images pour les pays que vous avez rajouté à la section 3.4. 

## 5. A vous de jouer 

1. Modifier le modèle ```Country``` pour y ajouter un attribut ```continent```

2. Modifiez le controller pour regrouper les pays par continents. 
- Le nombre de section dépendra du nombre de continents unique dans la liste de pays. 
- Les pays se trouvant dans un même continent doivent être affichés sous la même section

> Hint : Vous pouvez utiliser la fonction group by