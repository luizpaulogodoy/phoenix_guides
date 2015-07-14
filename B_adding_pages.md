Nossa tarefa para este guia é adicionar duas novas páginas para a nossa aplicação Phoenix. Uma delas será uma página puramente estática, e os outros vão tomar parte do caminho da url como entrada e passá-lo através de um template para o display. Ao longo do caminho, vamos ganhar familiaridade com os componentes básicos de um aplicativo de Phoenix: as rotas, controllers, views e templates.


Quando Phoenix gera uma nova aplicação para nós, ele constrói uma estrutura de diretório de nível superior como isto.

```text
├── _build
├── config
├── deps
├── lib
├── priv
├── test
├── web
```

A maior parte do nosso trabalho neste guia estará no diretório `web`, que se parece com isso quando expandido.

```text
├── channels
├── controllers
│   └── page_controller.ex
├── models
├── router.ex
├── templates
│   ├── layout
│   │   └── app.html.eex
│   └── page
│       └── index.html.eex
└── views
|   ├── error_view.ex
|   ├── layout_view.ex
|   └── page_view.ex
└── web.ex
```

Todos os arquivos que estão atualmente nos diretórios controllers, models e views estão lá para criar a página "Welcome to Phoenix!" que vimos no último guia. Vamos ver como podemos voltar a usar parte desse código em breve.

Todos os assets estáticos da nossa aplicação estão em `priv/static` no diretório apropriado para cada tipo de arquivo - css, imagens ou js. Nós colocamos assets que exigem uma fase de build em `web/static`, e os arquivos de origem são construídos em suas respectivas`app.js` / `app.css` pacotes dentro do `priv/static`. Nós não iremos fazer quaisquer alterações aqui, por hora, mas é bom saber onde estão para referência futura.

```text
priv
└── static
    └── images
        └── phoenix.png
```

```text
web
└── static
    ├── css
    |   └── app.scss
    ├── js
    │   └── app.js
    └── vendor
        └── phoenix.js
```

O diretório `lib` também contém arquivos que deve conhecer. O endpoint de Nossa aplicação fica em `lib/hello_phoenix/endpoint.ex`, e nosso arquivo de aplicativo (que começa a nossa aplicação e sua árvore de supervisão) está em `lib/ hello_phoenix.ex`.

```text
lib
├── hello_phoenix
|   ├── endpoint.ex
│   └── repo.ex
└── hello_phoenix.ex
```
Preparado o suficiente, vamos começar com a nossa primeira nova página Phoenix!

### Uma nova rota

Routes mapeia pares únicos de HTTP verbo/path para controlador/ação que irá lidar com ele.  Phoenix gera um arquivo de rotas para nós em novas aplicações em `web/router.ex`. Este é o lugar onde nós estaremos trabalhando para esta seção.

A rota para o nosso "Welcome to Phoenix!" A página do  guia "Up And Running" anterior se parece com isso.
```elixir
get "/", PageController, :index
```
Vamos ver o que esse caminho nos diz. Visitar [http://localhost:4000/] (http://localhost:4000/) emite uma solicitação HTTP GET para o caminho raiz. Todos os pedidos como este serão tratados pela função `index` no módulo `HelloPhoenix.PageController` definido em `web/controllers/page_controller.ex`.

A página que está indo para construir vai simplesmente dizer "Hello World, from Phoenix!" quando apontamos o nosso navegador para [http://localhost:4000/hello] (http://localhost:4000/hello).

A primeira coisa que precisamos fazer para criar essa página é definir uma rota para ela. Vamos abrir `web/router.ex` em um editor de texto. Deve atualmente se parecer com isso.

```elixir
defmodule HelloPhoenix.Router do
  use HelloPhoenix.Web, :router

  pipeline :browser do
    plug :accepts, ["html"]
    plug :fetch_session
    plug :fetch_flash
    plug :protect_from_forgery
  end

  pipeline :api do
    plug :accepts, ["json"]
  end

  scope "/", HelloPhoenix do
    pipe_through :browser # Use the default browser stack

    get "/", PageController, :index
  end

  # Other scopes may use custom stacks.
  # scope "/api", HelloPhoenix do
  #   pipe_through :api
  # end
end

```
For now, we'll ignore the pipelines and the use of `scope` here and just focus on adding a route. (We cover these topics in the [Routing Guide](http://www.phoenixframework.org/docs/routing), if you're curious.)

Let's add a new route to the router that maps a `GET` request for `/hello` to the `index` action of a soon-to-be-created `HelloPhoenix.HelloController`.

```elixir
get "/hello", HelloController, :index
```

The `scope "/"` block of our `router.ex` file should now look like this.

```elixir
scope "/", HelloPhoenix do
  pipe_through :browser # Use the default browser stack

  get "/", PageController, :index
  get "/hello", HelloController, :index
end
```

### A New Controller

Controllers are Elixir modules, and actions are Elixir functions defined in them. The purpose of actions is to gather any data and perform any tasks needed for rendering. Our route specifies that we need a `HelloPhoenix.HelloController` module with an `index/2` action.

To make that happen, let's create a new `web/controllers/hello_controller.ex` file, and make it look like the following.

```elixir
defmodule HelloPhoenix.HelloController do
  use HelloPhoenix.Web, :controller

  def index(conn, _params) do
    render conn, "index.html"
  end
end
```
We'll save a discussion of `use HelloPhoenix.Web, :controller` for the [Controllers Guide](http://www.phoenixframework.org/docs/controllers). For now, let's focus on the `index/2` action.

All controller actions take two arguments. The first is `conn`, a struct which holds a ton of data about the request. The second is `params`, which are the request parameters. Here, we are not using `params`, and we avoid compiler warnings by adding the leading `_`.

The core of this action is `render conn, "index.html"`. This tells Phoenix to find a template called `index.html.eex` and render it. Phoenix will look for the template in a directory named after our controller, so `web/templates/hello`.

Note: Using an atom as the template name will also work here, `render conn, :index`, but the template will be chosen based off the Accept headers, ie `"index.html"` or `"index.json"`, etc.

The modules responsible for rendering are views, and we'll make a new one of those next.

### A New View

Phoenix views have several important jobs. They render templates. They also act as a presentation layer for raw data from the controller, preparing it for use in a template. Functions which perform this transformation should go in a view.

As an example, say we have a data structure which represents a user with a `first_name` field and a `last_name` field, and in a template, we want to show the user's full name. We could write code in the template to merge those fields into a full name, but the better approach is to write a function in the view to do it for us, then call that function in the template. The result is a cleaner and more legible template.

In order to render any templates for our `HelloController`, we need a `HelloView`. The names are significant here - the first part of the names of the view and controller must match. Let's create an empty one for now, and leave a more detailed description of views for later. Create `web/views/hello_view.ex` and make it look like this.

```elixir
defmodule HelloPhoenix.HelloView do
  use HelloPhoenix.Web, :view
end
```

### A New Template

Phoenix templates are just that, templates into which data can be rendered. The standard templating engine Phoenix uses is eex, which stands for [Embedded Elixir](http://elixir-lang.org/docs/stable/eex/). All of our template files will have the `.eex` file extension.

Templates are scoped to a view, which are scoped to controller. In practice, this simply means that we create a directory named after the controller in the `web/templates` directory. For our hello page, that means we need to create a `hello` directory under `web/templates` and then create an `index.html.eex` file within it.

Let's do that now. Create `web/templates/hello/index.html.eex` and make it look like this.

```html
<div class="jumbotron">
  <h2>Hello World, from Phoenix!</h2>
</div>
```

Now that we've got the route, controller, view and template, we should be able to point our browsers at [http://localhost:4000/hello](http://localhost:4000/hello) and see our greeting from Phoenix! (In case you stopped the server along the way, the task to restart it is `mix phoenix.server`.)

![Phoenix Greets Us](/images/hello-from-phoenix.png)

There are a couple of interesting things to notice about what we just did. We didn't need to stop and re-start the server while we made these changes. Yes, Phoenix has hot code re-loading! Also, even though our `index.html.eex` file consisted of only a single div tag, The page we get is a full html document. Our index template is rendered into the application layout - `web/templates/layout/app.html.eex`. If you open it, you'll see a tag that looks like this: `<%= @inner %>`, which is what injects our rendered template into the layout before the html is sent off to the browser.

## Another New Page

Let's add just a little complexity to our application. We're going to add a new page that will recognize a piece of the url, label it as a "messenger" and pass it through the controller into the template so our messenger can say hello.

As we did last time, the first thing we'll do is create a new route.

### A New Route

For this exercise, we're going to re-use the `HelloController` we just created and just add a new `show` action. We'll add a line just below our last route, like this.

```elixir
scope "/", HelloPhoenix do
  pipe_through :browser # Use the default browser stack.

  get "/", PageController, :index
  get "/hello", HelloController, :index
  get "/hello/:messenger", HelloController, :show
end
```
Notice that we put the atom `:messenger` in the path. Phoenix will take whatever value that appears in that position in the url and pass a [Dict](http://elixir-lang.org/docs/stable/elixir/Dict.html) with the key `messenger` pointing to that value to the controller.

For example, if we point the browser at: [http://localhost:4000/hello/Frank](http://localhost:4000/hello/Frank), the value of ":messenger" will be "Frank".

### A New Action

Requests to our new route will be handled by the `HelloPhoenix.HelloController` `show` action. We already have the controller at `web/controllers/hello_controller.ex`, so all we need to do is edit that file and add a `show` action to it. This time, we'll need to keep the params that get passed into the action so that we can pass the messenger to the template. To do that, we add this show function to the controller.

```elixir
def show(conn, %{"messenger" => messenger}) do
  render conn, "show.html", messenger: messenger
end
```
There are a couple of things to notice here. We pattern match against the params passed into the show function so that the `messenger` variable will be bound to the value we put in the `:messenger` position in the url. For example, if our url is [http://localhost:4000/hello/Frank](http://localhost:4000/hello/Frank), the messenger variable would be bound to `Frank`.

Within the body of the `show` action, we also pass a third argument into the render function, a key value pair where `:messenger` is the key, and the messenger variable is passed as the value.

It's good to remember that the keys to the params [Dict](http://elixir-lang.org/docs/stable/elixir/Dict.html) will always be strings.

### A New Template

For the last piece of this puzzle, we'll need a new template. Since it is for the `show` action of the `HelloController`, it will go into the `web/templates/hello` directory and be called `show.html.eex`. It will look surprisingly like our `index.html.eex` template, except that we will need to display the name of our messenger.

To do that, we'll use the special eex tags for executing Elixir expressions - `<%=  %>`. Notice that the initial tag has an equals sign like this: `<%=` . That means that any Elixir code that goes between those tags will be executed, and the resulting value will replace the tag. If the equals sign were missing, the code would still be executed, but the value would not appear on the page.

And this is what the template should look like.

```html
<div class="jumbotron">
  <h2>Hello World, from <%= @messenger %>!</h2>
</div>
```

Our messenger appears as `@messenger`. In this case, this is not a module attribute. It is special bit of metaprogrammed syntax which stands in for `Dict.get(assigns, :messenger)`. The result is much nicer on the eyes and much easier to work with in a template.

We're done. If you point your browser here: [http://localhost:4000/hello/Frank](http://localhost:4000/hello/Frank), you should see a page that looks like this:

![Frank Greets Us from Phoenix](/images/hello-world-from-frank.png)

Play around a bit. Whatever you put after /hello/ will appear on the page as your messenger.
