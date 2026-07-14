### Variáveis
1. Toda variável deve ser nomeada com substantivo que descreva o que representa dentro do escopo a que pertence.

```ts
// good code
const userAge: number = 32;
const shippingCost: number = 12.5;
const productNames: string[] = [];

// code smell
const ua: number = 32;
const sc: number = 12.5;
const pN: string[] = [];
```

2. Abreviações somente são permitidas em casos de amplo reconhecimento, de forma que não atrapalhem a legibilidade do código. (ex.: `num`, `info`, `tel`, `max`, `min`...)

```ts
// good code
const numPages: number = 10;
const userInfo: string = '...';
const maxWeight: number = 80;

// code smell
const nPg: number = 10;
const usrInf: string = '...';
const mxWt: number = 80;
```

3. Sempre preferir `const`; usar `let` apenas quando houver reatribuição real. `var` é proibido.

4. Funções com mais de um parâmetro só podem usar parâmetros posicionais quando forem hooks/factories de injeção de dependência (ex.: repository recebendo um client). Toda outra função com mais de um parâmetro deve receber um único objeto desestruturado (named params).

```ts
// good code
class UserRepository {
  constructor(
    private readonly client: HttpClient,
    private readonly logger: Logger,
  ) {}
}

function updateUserInfo({ name, email, address }: UpdateUserInfoParams): void {
  // Implementação
}

// code smell
function updateUserInfo(name: string, email: string, address: string): void {
  // Implementação
}
```

5. Nunca mutar props ou state recebido — sempre criar uma nova referência.

```ts
// code smell
function addItem(items: Item[], item: Item) {
  items.push(item); // muta o array recebido
  return items;
}

// good code
function addItem(items: Item[], item: Item): Item[] {
  return [...items, item];
}
```

[<= Voltar](/README.md)
