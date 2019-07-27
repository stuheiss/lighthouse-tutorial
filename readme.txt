https://lighthouse-php.com/master/getting-started/tutorial.html#agenda

# install framework
laravel new lighthouse-tutorial
cd lighthouse-tutorial
# use Laravel GraphQL Playground as an IDE for GraphQL queries
# use Lighthouse as the GraphQL Server
composer require nuwave/lighthouse mll-lab/laravel-graphql-playground
# lighthouse
php artisan vendor:publish --provider="Nuwave\Lighthouse\LighthouseServiceProvider"
# playground
php artisan vendor:publish --provider="MLL\GraphQLPlayground\GraphQLPlaygroundServiceProvider"

