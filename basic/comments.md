
## Comentários
Em vista do seguimento das determinações acima, é **vetado** o uso de comentários que não sejam de documentação ou TODOs, haja vista que a utilização das boas práticas torna, por si só, o código suficientemente limpo e legível.

Tanto os TODOs como os comentários documentais devem ser feitos em português, visando a sua própria aplicabilidade.
### TODOs
Comentários precedidos pela tag `TODO` são reconhecidos como uma prática eficiente e são amplamente utilizados. Eles servem como marcadores que sinalizam pontos de atenção, pendências, dúvidas ou necessidade de alterações futuras no código.
### Documentação
Funções, hooks e tipos **exportados/públicos** devem ser documentados a nível de código através de JSDoc (`/** */`), visando um melhor aproveitamento das ferramentas (ex.: tooltips de IDE). Itens privados/não-exportados, por serem de uso interno e já cobertos pelas regras de nomenclatura, dispensam documentação salvo quando seu comportamento não for evidente pelo nome.
```ts
interface Circle {
  /** Raio da circunferência, em metros. */
  radius: number;
}

/** Calcula a área da circunferência a partir do raio. */
function calculateArea({ radius }: Circle): number {
  return 3.14 * radius * radius;
}
```

[<= Voltar](/README.md)
