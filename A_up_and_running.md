O objetivo deste primeiro guia é obter uma aplicação Phoenix rodando o mais rápido possível.

Antes de começar, por favor, tire um minuto para ler o [Guia de instalação](http://www.phoenixframework.org/docs/installation). Ao instalar todas as dependências necessárias de antemão, nós vamos ser capazes de obter o nosso aplicativo instalado e funcionando sem problemas.

Neste ponto, devemos ter Elixir, Erlang, Hex, eo  Phoenix instalado. Nós também devemos ter PostgreSQL e node.js instalado para construir um aplicativo padrão.

Ok, vamos lá!

Nós podemos rodar `mix phoenix.new` de qualquer diretório em ordem para criar nossa aplicação Phoenix. Phoenix aceitará um caminho absoluto ou relativo para o diretório do nosso novo projeto. Supondo-se que o nome de nosso aplicativo é `ola_phoenix`, qualquer um destes comandos irá funcionar.

```console
$ mix phoenix.new /Users/me/work/elixir-stuff/hello_phoenix
```

```console
$ mix phoenix.new hello_phoenix
```

> A note about [Brunch.io](http://brunch.io/) before we begin: Phoenix will use Brunch.io for asset management by default. Brunch.io's dependencies are installed via the node package manager, not mix. Phoenix will prompt us to install them at the end of the `mix phoenix.new` task. If we say "no" at that point, and if we don't install those dependencies later with `npm install`, our application will raise errors when we try to start it, and our assets may not load properly. If we don't want to use Brunch.io at all, we can simply pass `--no-brunch` to `mix phoenix.new`.

Now that we're ready, let's call `phoenix.new` with a relative path.

```console
$ mix phoenix.new hello_phoenix
* creating hello_phoenix/README.md
. . .
```

Phoenix generates the directory structure and all the files we will need for our application. When it's done, it will ask us if we want it to install our dependencies for us. Let's say yes to that.

```console
Fetch and install dependencies? [Yn] y
* running npm install
* running mix deps.get
```

Once our dependencies are installed, the task will prompt us to change into our project directory and start our application.

```console
We are all set! Run your Phoenix application:

$ cd hello_phoenix
$ mix phoenix.server

You can also run it inside IEx (Interactive Elixir) as:

$ iex -S mix phoenix.server
```

Let's do that now.

```console
$ cd hello_phoenix
$ mix phoenix.server
```

> Note: if this is the first time you are running this command, Phoenix may also ask to install Rebar. Go ahead with the installation as Rebar is used to build Erlang packages.

By default Phoenix accepts requests on port 4000. If we point our favorite web browser at [http://localhost:4000](http://localhost:4000), we should see the Phoenix Framework welcome page.

![Phoenix Welcome Page](/images/welcome-to-phoenix.png)

If your screen looks like the image above, congratulations! You now have a working Phoenix application.

Locally, our application is running in an iex session. To stop it, we hit ctrl-c twice, just as we would to stop iex normally.

The next step is customizing our application just a bit to give us a sense of how a Phoenix app is put together.
