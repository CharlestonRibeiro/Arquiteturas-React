
# APLICAÇÃO DOS PRINCÍPIOS SOLID
Toda a arquitetura do presente projeto se baseia nos princípios do SOLID:
## Responsabilidade Única (Single Responsibility Principle - SRP)
Um módulo/hook/componente deve ter apenas uma razão para mudar, significando que ele deve ter apenas uma responsabilidade.
```ts
// code smell
function useUser(id: string) {
  const [user, setUser] = useState<User | null>(null);

  useEffect(() => {
    fetch(`/api/users/${id}`).then((r) => r.json()).then(setUser);
  }, [id]);

  function formatUserName() { ... }
  function sendConfirmationEmail() { ... }

  return { user, formatUserName, sendConfirmationEmail };
}
```
```ts
// good code
function useUser(id: string) {
  const [user, setUser] = useState<User | null>(null);

  useEffect(() => {
    fetch(`/api/users/${id}`).then((r) => r.json()).then(setUser);
  }, [id]);

  return user;
}
```
No exemplo acima, o hook `useUser` deve lidar apenas com a obtenção do usuário, sem assumir responsabilidades adicionais como formatação ou envio de e-mail.
> **Todos** os módulos do projeto devem assumir somente a sua responsabilidade específica dentro da responsabilidade geral da camada a que pertencem, conforme a estrutura de camadas definida pela [arquitetura](/README.md#estruturas) escolhida para o projeto.
## Aberto/Fechado (Open/Closed Principle)
Os módulos devem ser abertos para extensão, mas fechados para modificação.
```ts
// code smell
interface Shape {
  kind: 'rectangle' | 'circle';
  width?: number;
  height?: number;
  radius?: number;
}

function calculateArea(shape: Shape): number {
  if (shape.kind === 'rectangle') {
    return shape.width! * shape.height!;
  } else {
    return 3.14 * shape.radius! * shape.radius!;
  }
}
```
```ts
// good code
interface Shape {
  area(): number;
}

class Circle implements Shape {
  constructor(private readonly radius: number) {}

  area(): number {
    return 3.14 * this.radius * this.radius;
  }
}

class Rectangle implements Shape {
  constructor(
    private readonly width: number,
    private readonly height: number,
  ) {}

  area(): number {
    return this.width * this.height;
  }
}
```
O exemplo acima mostra como podemos estender a funcionalidade criando novas implementações de `Shape`, sem necessidade de alterar `calculateArea` para cada novo formato.
> **Todos** os tipos declarados devem conter em si quaisquer parâmetros absolutos e relativos referentes àquela abstração. Parâmetros referentes a uma abstração específica **nunca** devem ser declarados fora dela.
## Substituição de Liskov (Liskov Substitution Principle - LSP)
As implementações devem ser substituíveis pela abstração que representam, sem afetar a corretude do programa.
```ts
// code smell
class Rectangle {
  constructor(public width: number, public height: number) {}
}

class Square extends Rectangle {
  constructor(length: number) {
    super(length, length);
  }
}

const strangeSquare = new Square(3);
strangeSquare.width = 4; // quebra a invariante de Square
```
```ts
// good code
interface Bird {
  fly(): void;
}

class Sparrow implements Bird {
  fly(): void {
    console.log('O pardal está voando.');
  }
}

class Parrot implements Bird {
  fly(): void {
    console.log('O papagaio está voando.');
  }
}

function makeBirdFly(bird: Bird): void {
  bird.fly();
}

makeBirdFly(new Sparrow());
makeBirdFly(new Parrot());
```
Este princípio é ilustrado aqui pela capacidade de passar qualquer implementação de `Bird` para `makeBirdFly`, sem alterar o comportamento esperado. O primeiro exemplo não respeita o LSP, pois permite que exista um "quadrado" com lados de tamanhos diferentes.
> **Todas** as implementações declaradas no projeto devem implementar **sem erros** quaisquer métodos contidos em suas interfaces. Quando isso não se fizer possível, o tipo não poderá ser considerado como uma implementação daquela interface.
## Segregação de Interface (Interface Segregation Principle - ISP)
Nenhum módulo deve ser forçado a depender de métodos que não utiliza.
```ts
// code smell
interface Device {
  printContent(): void;
  scanContent(): void;
}

class Printer implements Device {
  printContent(): void { ... }
  scanContent(): void {} // ???
}
```
```ts
// good code
interface Printable {
  printContent(): void;
}

interface Scannable {
  scanContent(): void;
}

class Printer implements Printable {
  printContent(): void { ... }
}
```
Este exemplo demonstra o ISP pela criação de interfaces específicas (`Printable` e `Scannable`), evitando a necessidade de a classe `Printer` implementar métodos que não são pertinentes ao seu propósito.
> **Todos** os tipos-base declarados no projeto devem conter **apenas** parâmetros e métodos comuns a todas as suas implementações.
## Inversão de Dependência (Dependency Inversion Principle - DIP)
Módulos de alto nível não devem depender de módulos de baixo nível. Ambos devem depender de abstrações.
```ts
interface DataRepository {
  saveData(data: string): Promise<void>;
}

class CloudStorage implements DataRepository { ... }
class LocalStorage implements DataRepository { ... }

// code smell
class DataManager {
  constructor(private readonly repository: LocalStorage) {}

  saveData(data: string): Promise<void> {
    return this.repository.saveData(data);
  }
}

// good code
class DataManager {
  constructor(private readonly repository: DataRepository) {}

  saveData(data: string): Promise<void> {
    return this.repository.saveData(data);
  }
}
```
O `DataManager` deve depender da abstração `DataRepository`, permitindo a substituição de `LocalStorage` por `CloudStorage` sem alterar o `DataManager`, ilustrando a inversão de dependência.
> As implementações concretas devem possuir uma interface que as represente sempre que houver mais de uma possibilidade de implementação no código (ou seja, tipos **client**, **service**, **repository**, **dto** e **store**).
>
> A injeção de dependências (via parâmetro de hook, factory ou Context) deve **sempre** ser feita utilizando tais interfaces e **nunca** suas implementações.
