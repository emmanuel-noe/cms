# API simple pour le developeur

S'inspirer d'API de type Laravel / Eloquent ORM pour éviter de devoir écrire du code MySQL ou du code PHP inutile. Rendre la récupération de contenu plus simple et beaucoup plus "fun".

#### Exemple de syntax à s'inspirer
###### Corcel pour WP

```php
// Récuperer tous les posts WP
$posts = Post::published()->get();
$posts = Post::status('publish')->get();

// Récuperer un post spécifique
$post = Post::find(31);
echo $post->post_title;

// Filtrer les posts par meta/custom field
$posts = Post::published()
              ->hasMeta('field')
              ->get();
$posts = Post::hasMeta('acf')->get();
```
Source : [Librairie Corcel pour WP](https://github.com/corcel/corcel)

###### Eloquent ORM
```php
$flights = Flight::where('active', 1)
                  ->orderBy('name', 'desc')
                  ->take(10)
                  ->get();

```
Avec des updates ultra simple. Le code qui suit récupère la donnée, et la met à jour directement en base de donnée, pas d'autre code nécessaire.

```php
$flight = Flight::find(1);
$flight->name = 'New Flight Name';
$flight->save();
```


Source : [Eloquent ORM]( https://laravel.com/docs/5.4/eloquent)

###### Laravel Medialibrary
Le code suivant upload et lie automatiquement un média à une news.

```php
$newsItem = News::find(1);
$newsItem->addMedia($cheminVerFichier)->toMediaCollection('images');
```

Source : [Laravel Medialibrary](https://docs.spatie.be/laravel-medialibrary/v5/introduction)

Tous ces exemples sont la pour montrer que le developpement pourrai être beaucoup plus rapide en PHP si l'API est directement pensée pour le developpeur qui va ensuite devoir la manipulé et le temps nécessaire à cette manipulation. On le voit beaucoup de choses existent et rien ne sert de réinventer des solutions à chaques fois, qui ne sont pas performances, pas utilisables et pas robustes. 



# Création de plugin simplifié
S'inspirer de Themosis, de _flat CMS_ comme KirbyCMS ou Statamic ou même Podio.


```php
PostType::make('slider')->set([
    'supports' => ['title', 'editor']
```
