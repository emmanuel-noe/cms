# API simple pour le developeur

S'inspirer d'API de type Laravel / Eloquent ORM pour éviter de devoir écrire du code MySQL. La récupération de contenu se fait simplement et est plus "fun".

```php
// Récuperer tous les posts WP
$posts = Post::published()->get();
$posts = Post::status('publish')->get();

// Récuperer un post spécifique
$post = Post::find(31);
echo $post->post_title;

// Filtrer les posts par meta/custom field
$posts = Post::published()->hasMeta('field')->get();
$posts = Post::hasMeta('acf')->get();
```
Source : https://github.com/corcel/corcel

Source : https://laravel.com/docs/5.4/eloquent
