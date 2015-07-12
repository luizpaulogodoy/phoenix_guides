O objetivo deste primeiro guia é obter uma aplicação Phoenix rodando o mais rápido possível.

Antes de começar, por favor, tire um minuto para ler o [Guia de instalação](http://www.phoenixframework.org/docs/installation). Ao instalar todas as dependências necessárias de antemão, nós vamos ser capazes de obter o nosso aplicativo instalado e funcionando sem problemas.

Neste ponto, devemos ter Elixir, Erlang, Hex, eo  Phoenix instalado. Nós também devemos ter PostgreSQL e node.js instalado para construir um aplicativo padrão.

Ok, vamos lá!

Nós podemos rodar `mix phoenix.new` de qualquer diretório em ordem para criar nossa aplicação Phoenix. Phoenix aceitará um caminho absoluto ou relativo para o diretório do nosso novo projeto. Supondo-se que o nome de nosso aplicativo é `ola_phoenix`, qualquer um destes comandos irá funcionar.

```console
$ mix phoenix.new /Users/me/work/elixir-stuff/ola_phoenix
```

```console
$ mix phoenix.new ola_phoenix
```

> Uma nota sobre [Brunch.io](http://brunch.io/) antes de começarmos: Phoenix usará Brunch.io para gerenciamento de assets por padrão. Dependências de Brunch.io são instalados através do gerenciador de pacotes do node, não pelo mix. Phoenix irá pedir-nos para instalá-los no final do comando `mix phoenix.new`. Se dizemos "no" a esse ponto, e se nós não instalarmos essas dependências mais tarde com `npm install`, a nossa aplicação vai gerar erros quando for iniciada, e os nossos assets podem não carregar corretamente. Se não quiser usar Brunch.io em tudo, podemos simplesmente passar `--no-brunch` para `mix phoenix.new`.

Agora que estamos prontos, vamos chamar `phoenix.new` com um caminho relativo.

```console
$ mix phoenix.new ola_phoenix
* creating ola_phoenix/README.md
. . .
```

Phoenix gera a estrutura de diretórios e todos os arquivos que vamos precisar para a nossa aplicação. Quando isto é feito, ele vai perguntar se queremos  instalar dependências. Vamos dizer "yes" para isso.

```console
Fetch and install dependencies? [Yn] y
* running npm install
* running mix deps.get
```

Uma vez que nossas dependências estão instaladas, a tarefa vamos entrar em nosso diretório do projeto e começar a nossa aplicação.

```console
We are all set! Run your Phoenix application:

$ cd hello_phoenix
$ mix phoenix.server

You can also run it inside IEx (Interactive Elixir) as:

$ iex -S mix phoenix.server
```

Agora faça isso.

```console
$ cd hello_phoenix
$ mix phoenix.server
```

> Nota: se esta é a primeira vez que você estiver executando este comando, Phoenix também pode pedir para instalar Rebar. Vá em frente com a instalação como Rebar é usado para construir pacotes de Erlang.

Por padrão Phoenix roda na porta 4000. Acessando pelo navegador [http://localhost:4000](http://localhost:4000), você deverá ver a página de boas-vindas Phoenix Framework.

![Phoenix Welcome Page](/images/welcome-to-phoenix.png)

Se a tela se parecer com a imagem acima, parabéns! Agora você tem um aplicativo Phoenix rodando.

Localmente, o nosso aplicativo está sendo executado em uma sessão iex. Para pará-lo, nós pressionamos Ctrl+C duas vezes, assim como faríamos para parar iex normalmente.

O próximo passo é personalizar a sua aplicação apenas um pouco para nos dar uma noção de como um aplicativo Phoenix é montado.
