
# TypeScript na Prática

Slides da talk **“TypeScript na Prática”** com exemplos de tipagem do dia a dia (enums, uniões, &, interfaces, generics, conditional types e APIs) feitos em Slidev.

## Conteúdo
- Por que usar TypeScript
- Props tipadas para componentes
- `enum` e uniões de literais
- Interseção (`&`) e composição de tipos
- Interfaces aninhadas e DTOs
- Tipando respostas de API
- `type` vs `interface`
- Generics, constraints e `keyof`
- Conditional Types

## Como rodar localmente
```bash
# 1) Instale dependências
npm install

# 2) Rode os slides em modo dev
npm run dev
# (alternativa direta)
npx slidev slides.md --open
````

## Build estático

Gera a versão estática para publicação (pasta `dist/`):

```bash
npm run build
```

