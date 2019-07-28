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

edit graphql/schema.graphql:
--cut--
"A datetime string with format `Y-m-d H:i:s`, e.g. `2018-01-01 13:00:00`."
scalar DateTime @scalar(class: "Nuwave\\Lighthouse\\Schema\\Types\\Scalars\\DateTime")

"A date string with format `Y-m-d`, e.g. `2011-05-23`."
scalar Date @scalar(class: "Nuwave\\Lighthouse\\Schema\\Types\\Scalars\\Date")

type Query {
    users: [User!]! @paginate(type: "paginator" model: "App\\User")
    user(id: ID @eq): User @find(model: "App\\User")
    posts: [Post!]! @all
    post(id: ID @eq): Post @find(model: "App\\Post")
    comments: [Comment!]! @all
    comment(id: ID @eq): Comment @find(model: "App\\Comment")
}

type User {
    id: ID!
    name: String!
    email: String!
    created_at: DateTime!
    updated_at: DateTime!
    posts: [Post!] @hasMany
}

type Post {
    id: ID!
    title: String!
    content: String!
    user: User! @belongsTo
    comments: [Comment!]! @hasMany
}

type Comment {
    id: ID!
    reply: String!
    post: Post! @belongsTo
}
--cut--

sample queries:
--cut--
{
  user(id:1) {
    id
    name
    email
    posts {
      id
      title
      content
      comments {
        id
        reply
      }
    }
  }
}

{
  post(id:2) {
    id
    title
    user {
      id
      name
    }
    comments {
      id
      reply
    }
  }
}


users(count:10) {
    data {
      id
      name
      email
    }
  }
}

{
  comment(id:1) {
    id
    reply
  }
}

{
  comments {
    id
    reply
    post {
      id
      title
      user {
        id
        name
      }
    }
  }
}
--cut--

