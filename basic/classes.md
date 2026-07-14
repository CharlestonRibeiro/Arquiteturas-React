
### Classes, tipos e componentes
1. Componentes de função recebem **PascalCase** e arquivo `.tsx` com o mesmo nome. Nunca criar class components.
1. Hooks, stores e utils recebem **camelCase** e arquivo `.ts`. Todo hook customizado começa com o prefixo `use`.
1. `interface` para forma de objeto/props; `type` para union/alias. Ambos em **PascalCase**.
1. **Não é permitido** o uso de prefixos ou sufixos de nenhuma natureza ao nomear models/entities.
1. As demais classes (repositories, usecases, stores) devem receber **por sufixo** apenas a denominação que representa sua responsabilidade, seguida, se for o caso, do sufixo `Interface`.

Exemplos:
```ts
// component
export function HomePage() { ... }        // home_page → HomePage.tsx

// hook customizado
export function useAuth() { ... }          // useAuth.ts

// store (Zustand)
export const useAuthStore = create<AuthState>(...);   // authStore.ts

// usecase
class LoginCase { ... }                    // loginCase.ts

// repository
class AuthRepository { ... }               // authRepository.ts

// interface
interface AuthRepositoryInterface { ... }  // authRepository.interface.ts

// entity/model
interface User { ... }                     // user.ts

// erro
class ExternalError extends AppError { ... }
```

[<= Voltar](/README.md)
