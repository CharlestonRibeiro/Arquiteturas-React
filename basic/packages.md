
## Packages
> [!NOTE]
> Os campos marcados com `***` são intencionalmente deixados em branco: cada projeto deve preenchê-los com o package efetivamente escolhido ao adaptar este template, conforme descrito no [README](/README.md#estruturas).

### Gerenciamento de estados
O projeto utiliza o *** como padrão para o gerenciamento de estado global/cross-módulo. Estado local de componente usa `useState`/`useReducer` nativos do React — Zustand só entra quando o estado precisa ser lido por mais de um componente/módulo.

### Data-fetching e cache
As requisições e o cache de dados assíncronos são feitos através do *** (ex.: TanStack Query), nunca com `useEffect` + `fetch` manual para dados que precisam de cache, retry ou invalidação.

### Gerenciamento de rotas
Para aprimorar a flexibilidade e a independência de pacotes externos, o projeto utiliza uma abstração customizada para navegação sobre o package ***.

As rotas são definidas de forma abstrata e centralizada na pasta `routes`, na camada core, permitindo uma referência clara e consistente aos caminhos utilizados na aplicação.

### Requisições HTTP
As requisições HTTP são feitas através do package ***.

### Componentes
A aplicação deve ser construída com o máximo aproveitamento de elementos HTML/CSS nativos e componentes próprios. É proibida a utilização de bibliotecas de componentes prontos (design systems de terceiros) que venham a ferir este princípio.

### Inserção de novos packages (regra geral)
A inserção de novos packages só é permitida após aprovação **unânime** do grupo de desenvolvedores da aplicação.

[<= Voltar](/README.md)
