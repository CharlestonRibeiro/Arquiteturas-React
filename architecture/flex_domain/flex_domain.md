# Flex Domain (React)

> Espelho web do Flex Domain Flutter: mesmas fronteiras de camada e mesma direção de dependência (`UI → INTERACTOR ← DATA`), com implementação idiomática de React (hooks + Zustand em vez de Cubit).

A proposta de desacoplar as camadas se baseia em:

```
DATA - INTERACTOR - UI
```

### DATA
Dá suporte à camada **Interactor**, implementando suas interfaces. Adapta dados externos para cumprir os contratos do domínio. Nessa camada implementamos datasources e repositories que podem depender de dados externos (API, cache local). Deve conter tudo aquilo que tem grande chance de mudar sem que o programador precise mexer na lógica interna do projeto.

### INTERACTOR
Hospeda as regras de negócio da aplicação e o estado. O núcleo é Dart-puro-equivalente: TypeScript puro, sem imports de React, de client HTTP ou de qualquer detalhe de framework. O repository aqui é só a interface (abstração); a implementação fica na camada mais baixa (DATA).

### UI
Responsável por declarar entradas, saídas e interações da aplicação: hospeda os componentes e páginas React que consomem o `INTERACTOR` através de hooks.

## Modularize

```
src/
└── modules/
    ├── auth/
    │   ├── data/
    │   ├── interactor/
    │   └── ui/
    ├── home/
    │   ├── data/
    │   ├── interactor/
    │   └── ui/
    └── ...
```

## Estrutura

### Core
Componentes centrais do código, compartilhados entre os módulos:
- **errors:** interface de erro `AppError` da qual devem descender todos os subtipos de erro tratados na aplicação;
- **routes:** definição e gerenciamento de rotas;
- **theme:** tema e componentes de UI;
- **utils:** constantes, enums, helpers e validadores de formulário;
- **components:** componentes utilizados por mais de um [module](#modules).

### Modules

#### data/
- **datasources/**
  `searchDatasource.ts`: implementação genérica para fontes de dados.
  `searchDatasource.interface.ts`: interface genérica para fontes de dados.
- **models/**
  `exampleModel.ts`: modelo de dados para os resultados, usado na serialização/deserialização.
- **repositories/**
  `exampleRepository.ts`: implementação do repositório que abstrai o acesso aos dados.
- **errors/**
  `errors.ts`: erros específicos.

#### interactor/
- **store/**
  `exampleStore.ts`: store Zustand — componente de lógica de negócios para o gerenciamento de estado, equivalente ao `*_bloc.dart`.
- **entities/**
  `exampleEntity.ts`: entidade de domínio que representa o resultado.
- **errors/**
  `errors.ts`: erros específicos.
- **repositories/**
  `exampleRepository.interface.ts`: interface do repositório utilizada pela lógica de negócios.
- **hooks/**
  `useExample.ts`: hook que conecta a store ao componente — equivalente ao `BlocBuilder`.

#### ui/
- `ExamplePage.tsx`: tela de interface do usuário.
- **components:** componentes visuais utilizados apenas pelo módulo referido.

## Convenções de nome de arquivo

| Sufixo | Camada | Papel |
|:--|:--|:--|
| `Entity.ts` | interactor | entidade de domínio |
| `.interface.ts` | interactor | interface (contrato) |
| `Store.ts` | interactor | store Zustand |
| `use<Feature>.ts` | interactor | hook que conecta store/query ao componente |
| `Model.ts` | data | DTO (json) |
| `Datasource.ts` / `Datasource.interface.ts` | data | fonte externa |
| `Repository.ts` | data | implementação do contrato |
| `Page.tsx` | ui | página raiz do módulo |
| `.routes.tsx` | módulo | definição de rotas do módulo |

## Tratamento de erros

- `AppError` é a interface base; todo erro tratado descende dela.
- Cada camada encapsula exceções genéricas e um erro prefixado por `Unspecified`.
```ts
// arquivos no diretório 'errors'
abstract class ExternalError implements AppError {}

class NetworkConnectionError extends ExternalError { ... }

class UnspecifiedExternalError extends ExternalError { ... }

// arquivo apiClient.ts
class ApiClient implements ClientInterface {
  async create(endpoint: string, data: Record<string, unknown>) {
    try {
      const response = await axios.post(endpoint, data);
      return response.data;
    } catch (e) {
      if (axios.isAxiosError(e)) throw new NetworkConnectionError(e);
      throw new UnspecifiedExternalError(e);
    }
  }
}
```
Caso ocorra uma exceção em alguma camada anterior, deve-se relançar (`throw`) para que o erro seja propagado e tratado de forma apropriada pela camada seguinte.

## Packages default da incubadora

> Defaults; troca exige aprovação **unânime** do time.

- **Estado:** Zustand
- **Data-fetching/cache:** TanStack Query
- **Rotas:** React Router
- **HTTP:** axios

---

[<= Voltar](/README.md)
