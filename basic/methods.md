### Métodos e funções
1. Toda função deve ser nomeada com verbo ou expressão verbal que descreva exatamente o que executa.
```ts
const PI = 3.14;

function calculateCircleArea(radius: number): number {
  return PI * radius * radius;
}

function calculateCirclePerimeter(radius: number): number {
  return 2 * PI * radius;
}
```
2. Devem ser evitadas descrições redundantes e implícitas pelo escopo em que a função se encontra.
```ts
// Por pertencerem à classe Circle, é necessariamente implícito que os métodos retornem
// valores de área e perímetro referentes àquela classe.

class Circle extends Shape {
  get calculateArea(): number { ... }
  get calculatePerimeter(): number { ... }
}
```
3. Nenhuma função pode executar ações que não sejam intrínsecas ao que descreve sua nomenclatura. Se um comportamento previsto interliga mais de uma ação, ambas devem ser encapsuladas.
```ts
// code smell
function stockProduct(qtd: number, price: number): void {
  addCashOnCashDesk(qtd * price);
  decreaseProductFromStock(qtd);
}

// good code
function sellProduct(qtd: number, price: number): void {
  addCashOnCashDesk(qtd * price);
  decreaseProductFromStock(qtd);
}
```
4. Toda função de uso exclusivamente interno de um módulo/classe não deve ser exportada (sem `export`), exceto se precisar ser testada isoladamente.
```ts
export class Dog {
  showJoy(): void {
    this.bark();
    this.jump();
  }

  private bark(): void { ... }
  private jump(): void { ... }
}
```
5. Hooks seguem as regras de hooks do React: só são chamados no nível mais alto do componente/hook, nunca dentro de `if`, laço ou callback condicional.
```ts
// code smell
function useUser(id?: string) {
  if (id) {
    const [user, setUser] = useState<User | null>(null); // hook condicional
  }
}

// good code
function useUser(id: string | undefined, repository: UserRepositoryInterface) {
  const [user, setUser] = useState<User | null>(null);

  useEffect(() => {
    if (!id) return;
    repository.getById(id).then(setUser);
  }, [id, repository]);

  return user;
}
```
> A busca em si delega para a `repository` (nunca `fetch` direto no hook — ver [Pacotes](/basic/packages.md#data-fetching-e-cache)); o exemplo acima ilustra apenas a regra de hooks condicionais.

[<= Voltar](/README.md)
