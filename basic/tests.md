
### Testes
1. Todo teste deve ficar ao lado do arquivo testado (`Componente.test.tsx`, `useHook.test.ts`) ou em `__tests__/` espelhando o caminho de `src/`, recebendo **por sufixo** `.test.ts`/`.test.tsx`.
1. Testes de unidade cobrem uma única função/hook/store (usecases, repositories, hooks customizados) e devem depender apenas de interfaces (`*Interface`), nunca de implementações concretas, permitindo o uso de mocks/fakes conforme a regra de DIP (`solid.md`).
1. Testes de componente cobrem `*Page` e componentes, verificando renderização e interação via Testing Library, sem depender de dados reais de rede.
1. Testes de integração cobrem o fluxo completo de um módulo (da UI até o repositório) e ficam isolados em `src/__tests__/integration/`.
1. Cada `describe` deve nomear a unidade testada; cada `it`/`test` deve descrever o comportamento esperado, não a implementação.

Exemplos:
```ts
// src/modules/auth/interactor/hooks/useAuth.test.ts
import { renderHook, waitFor } from '@testing-library/react';

class MockAuthRepository implements AuthRepositoryInterface {
  signIn = vi.fn();
}

describe('useAuth', () => {
  it('deve marcar status como success quando o login for bem-sucedido', async () => {
    const repository = new MockAuthRepository();
    repository.signIn.mockResolvedValue({ token: '...' });

    const { result } = renderHook(() => useAuth(repository));
    await result.current.signIn('user@email.com', 'password');

    await waitFor(() => expect(result.current.status).toBe('success'));
  });
});
```

[<= Voltar](/README.md)
