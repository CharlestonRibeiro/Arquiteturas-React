# Clean Modular (React)

> Espelho web do Clean Modular Flutter: mesmas fronteiras de camada e mesma direção de dependência, com implementação idiomática de React (hooks + Zustand em vez de Cubit).

### Core
Componentes centrais do código, compartilhados entre os módulos:
- **errors:** interface de erro `AppError` da qual devem descender todos os subtipos de erro tratados na aplicação;
- **routes:** definição e gerenciamento de rotas;
- **theme:** tema e componentes de UI;
- **utils:** constantes, enums, helpers e validadores de formulário;
- **components:** componentes utilizados por mais de um [module](#modules).

### Data
Camada responsável pelo processamento de dados: a transcrição de dados brutos em objetos modelados e vice-versa.
- **errors:** erros relativos ao gerenciamento de dados;
- **mappings:** funções `fromResponse`/`toRequest` para as classes de [model](#domain), responsáveis pela conversão de dados;
- **repositories:** implementam as interfaces das quais dependem os [usecases](#domain);
- **sources:** interfaces de recursos externos que serão implementadas na camada [external](#external).

### Domain
Domínio da aplicação, onde se encontram as abstrações de todos os cenários reais que ela abrange:
- **errors:** erros disfuncionais, relativos a problemas de lógica interna às regras de negócio;
- **interfaces:** interfaces responsáveis pelo gerenciamento dos dados emitidos pelo domínio;
- **models:** tipos que representam entidades de dados reais modeladas para uso na aplicação;
- **usecases:** classes que implementam as regras de negócio específicas de cada caso de uso.

### External
Todos os recursos de persistência externos à aplicação:
- **localdb:** implementam requisições de armazenamento local em cache (ex.: IndexedDB);
- **clients:** implementam requisições de API e bancos em nuvem;
- **errors:** erros relacionados a estado de conexão ou falhas de requisição;
- **services:** implementadores de requisições de dados simples em persistência local.

### Modules
Funcionalidades da aplicação, onde individualmente se encontram:
- **components:** componentes visuais utilizados apenas pelo módulo referido;
- **views:** subpáginas do módulo (blocos de navegação internos);
- **`<feature>Store.ts`:** store Zustand que realiza o gerenciamento de estado do módulo e a chamada dos [usecases](#domain);
- **`<Feature>Page.tsx`:** página raiz do módulo, para onde se refere a raiz da navegação do módulo referido;
- **`use<Feature>.ts`:** hook que conecta a store ao componente;
- **`<feature>.routes.tsx`:** definição das (sub)rotas do módulo.

## Packages default da incubadora

> Defaults; troca exige aprovação **unânime** do time.

- **Estado:** Zustand
- **Data-fetching/cache:** TanStack Query
- **Rotas:** React Router
- **HTTP:** axios

---

[<= Voltar](/README.md)
