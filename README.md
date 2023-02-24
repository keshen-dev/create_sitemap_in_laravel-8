# create_sitemap_in_laravel-8
To create a sitemap in Laravel 8, you can follow these steps:

First, you need to install the spatie/laravel-sitemap package. You can do this by running the following command in your terminal:

```
composer require spatie/laravel-sitemap
```

Next, you need to publish the configuration file for the package. You can do this by running the following command:

```
php artisan vendor:publish --provider="Spatie\Sitemap\SitemapServiceProvider" --tag="config"
```

After that, you need to create a new controller or add the following code to an existing controller. This code will generate the sitemap XML file:

```
use Spatie\Sitemap\Sitemap;
use Spatie\Sitemap\Tags\Url;

public function generateSitemap()
{
    $sitemap = Sitemap::create();

    // Add URLs to the sitemap
    $sitemap->add(Url::create('/')->setPriority(1.0));
    $sitemap->add(Url::create('/about')->setPriority(0.8));
    $sitemap->add(Url::create('/contact')->setPriority(0.5));

    // Add dynamic URLs to the sitemap
    $posts = Post::all();
    foreach ($posts as $post) {
        $url = Url::create('/posts/' . $post->slug)
            ->setLastModificationDate($post->updated_at)
            ->setPriority(0.7);
        $sitemap->add($url);
    }

    // Return the sitemap XML file
    return $sitemap->render();
}
```

Finally, you need to add a route for the sitemap. You can do this in your web.php file:
```
Route::get('/sitemap.xml', [SitemapController::class, 'generateSitemap']);
```

That's it! Now you should be able to access the sitemap XML file at the /sitemap.xml URL. Note that you'll need to replace the example URLs and dynamic URLs in the code above with the URLs for your own site.
