## Imports
As importações em qualquer arquivo do projeto devem ser feitas da seguinte forma:
1. bibliotecas externas (React, Zustand, TanStack Query, etc.), seguidas de uma linha em branco;
1. alias absoluto do projeto (`@/...`), seguido de uma linha em branco;
1. imports relativos de outros arquivos do módulo, seguidos de uma linha em branco.

```ts
import { useState } from 'react';
import { create } from 'zustand';

import { AppError } from '@/core/errors';

import { AuthRepository } from './authRepository';
```

[<= Voltar](/README.md)
