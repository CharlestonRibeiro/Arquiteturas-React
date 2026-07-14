# MVC+R (React)

> Espelho web do MVC+R Flutter: mesmas fronteiras de camada e mesma direção de dependência, com implementação idiomática de React (hooks em vez de Cubit, sem DI framework).

Fluxo mínimo: o dado vem do **repository**, passa pelo **hook** e é exibido na **página**.

```
Repository ───▶ Hook ───▶ Page
```

## Estrutura de pastas

```
src/
├── core/
│   ├── routes/
│   ├── theme/
│   ├── utils/
│   ├── components/
│   └── errors/
├── data/
│   ├── clients/
│   │   └── apiClient.ts
│   ├── repositories/
│   │   └── userRepository.ts
│   └── services/
├── models/
│   └── user.ts
└── modules/
    └── home/
        ├── components/
        ├── views/
        ├── useHome.ts          # hook: chama a repository, expõe estado e ações
        ├── HomePage.tsx        # página raiz do módulo
        └── home.routes.tsx     # definição de rota(s) do módulo
```

### Core
Componentes centrais do código, compartilhados entre todos os módulos:
- **routes:** definição e gerenciamento centralizado de rotas;
- **theme:** tema e componentes de UI compartilhados;
- **utils:** constantes, enums, helpers e validadores de formulário;
- **components:** componentes utilizados por mais de um [module](#modules);
- **errors:** interface `AppError` da qual devem descender todos os erros tratados na aplicação.

### Data
Camada de abstração para acesso a dados, responsável pela comunicação com fontes de dados externas:
- **clients:** implementam as chamadas HTTP (axios/fetch);
- **repositories:** expõem operações de dados para os hooks; retornam `models` ou lançam `AppError`. Não precisam de interface própria quando há apenas uma implementação (regra geral de DIP).
- **services:** implementam requisições a serviços diversos de integração externos à aplicação.

### Models
- Tipos/interfaces que representam entidades de negócio usadas em toda a aplicação (DTOs — espelham o formato retornado pela API).

### Modules
Funcionalidades da aplicação, onde individualmente se encontram:
- **components:** componentes visuais utilizados apenas pelo módulo referido;
- **views:** subpáginas do módulo (blocos de navegação internos);
- **`use<Feature>.ts`:** hook customizado que chama a repository e expõe estado + ações — equivalente ao `*_controller.dart` (Cubit);
- **`<Feature>Page.tsx`:** página raiz do módulo, consome o hook acima;
- **`<feature>.routes.tsx`:** definição das (sub)rotas do módulo.

## Convenções de nome de arquivo

| Sufixo | Camada | Papel |
|:--|:--|:--|
| (nenhum) `.ts` | models | DTO |
| `Repository.ts` | data | acesso a dados |
| `use<Feature>.ts` | módulo | hook: orquestra repository + estado |
| `<Feature>Page.tsx` | módulo | página raiz |
| `<feature>.routes.tsx` | módulo | rotas do módulo |

## Tratamento de erro

- `AppError` é a base; a repository traduz falhas de rede/parse em subtipos de `AppError`.
- O hook faz `try/catch`, captura o `AppError` e expõe um estado de erro (`status: 'error'`).
- A página decide como exibir cada estado de erro.

## Packages default da incubadora

> Defaults; troca exige aprovação **unânime** do time.

- **Estado local/orquestração:** hooks nativos (`useState`/`useReducer`)
- **Data-fetching/cache:** TanStack Query
- **Rotas:** React Router
- **HTTP:** axios

---

[<= Voltar](/README.md)
