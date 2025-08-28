---
# You can also start simply with 'default'
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: '#3178C6'
# some information about your slides (markdown enabled)
title: TypeScript na Prática
info: |
  ## Explorando recursos para o dia a dia
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# open graph

seoMeta:
  # By default, Slidev will use ./og-image.png if it exists,
  # or generate one from the first slide if not found.
  ogImage: auto
  # ogImage: https://cover.sli.dev
---

# TypeScript na Prática

Explorando recursos para o dia a dia


<div class="abs-br m-6 text-xl">
  <a href="https://github.com/pedrogustavo/typescript-na-pratica" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
layout: center
background: '#3178c6' # azul do TypeScript
class: text-white
---

<div class="flex items-center justify-center gap-12">

  <!-- Foto -->
  <div class="w-1/3">
    <img 
      src="/assets/pedro-perfil.jpeg" 
      alt="Minha foto" 
      class="rounded-2xl shadow-lg"
    />
  </div>

  <!-- Texto -->
  <div class="w-2/3">
    <h1 class="text-4xl font-bold mb-4">Pedro Gustavo</h1>
    <ul class="list-disc pl-6 space-y-2 text-lg">
      <li>Dev. Frontend</li>
      <li>Conta Stone Web</li>
      <li>Time de Crédito</li>
      <li>Desde de 2017 na Stone*</li>
    </ul>
  </div>

</div>

---
transition: slide-left
layout: center
---
<div class="flex items-center justify-center gap-10">

  <!-- GIF à esquerda -->
  <div class="w-1/2 flex justify-center">
    <img 
      src="/assets/what-huh.gif" 
      alt="Animação" 
      class="rounded-xl shadow-lg max-h-[400px]"
    />
  </div>

  <!-- Texto à direita -->
  <div class="w-1/2">
    <h1 class="text-4xl font-bold mb-4">TypeScript???</h1>
    <p class="text-lg leading-relaxed">
      Se alguém ainda não conhece
    </p>
    <ul class="list-disc pl-6 mt-4 space-y-2 text-lg">
      <li>Superconjunto do JavaScript</li>
      <li>Uma linguagem de programação fortemente tipada</li>
      <li>Superset de JavaScript</li>
    </ul>
  </div>

</div>

---
transition: slide-left
---

# Afinal, por que TypeScript?

<div grid="~ cols-2 gap-4">
  <div>

  Se JavaScript já roda em todo lugar, por que colocar “tipos” no meio do caminho?

  - Visualização de erros em tempo de desenvolvimento
  - Melhor legibilidade de código
  - Autocomplete
  - ...

  <br/>

  ```ts
  // TypeScript (te puxa pela orelha)
  function formatBRL(amountCents: number) {
    return (amountCents / 100).toFixed(2);
  }
  formatBRL("1000"); // ❌ Argument of type 'string' is not assignable to 'number'
  ```

  </div>
  <div>
  <img src="/assets/typescript.svg"/>

  </div>
</div>

---
layout: center
background: '#ffffff'
class: text-center
---

# Colocando a mão na massa 👨‍💻
<p class="text-lg leading-relaxed">Como o Typescript pode nos ajudar no dia a dia</p>

<div class="flex justify-center mt-8">
  <img 
    src="https://media.giphy.com/media/qgQUggAC3Pfv687qPC/giphy.gif" 
    alt="Coding gif" 
    class="rounded-2xl shadow-xl max-h-[400px]"
  />
</div>

---
transition: slide-left

---

# Props para componentes
<p class="text-lg leading-relaxed">Podemos utilizar para substituir no bom e velho PropTypes</p>


```ts
type AvatarProps = {
  name: string
  imageUrl?: string // opcional
}

export function Avatar({ name, imageUrl }: AvatarProps) {
  return (
    <div>
      <img src={imageUrl ?? "/default.png"} alt={name} />
      <p>{name}</p>
    </div>
  )
}

```



---
transition: slide-left

---

# enum
<p class="text-lg leading-relaxed">Garante constantes únicas e tipadas evitando typos em strings soltas</p>


```ts
export enum DiscountSettlementSteps {
  SIMULATE = 'simulacao',
  PROPOSAL = 'proposta',
  PIN = 'pin',
  PAYMENT_METHOD = 'forma-de-pagamento',
  REVIEW = 'revisao'
}

const steps: { key: DiscountSettlementSteps; render: () => JSX.Element }[] = [
  {
    key: DiscountSettlementSteps.PROPOSAL,
    render: () => <RenegotiationDiscountAmountSimulation userEmail={userEmail} mode={mode} />
  },
  {
    key: DiscountSettlementSteps.PIN,
    render: () => <RenegotiationChallenge saveSpotOffer={setSpotOffer} />
  }
]

```



---
transition: slide-left

---

# União de literais de string
<p class="text-lg leading-relaxed">Define que a variável só pode assumir um entre valores exatos. Ótimo para evitar typos, ganhar autocomplete e usar como discriminador em switch</p>


```ts
type UserType = 'Customer' | 'Operator' | 'Leadership';

function canApprove(u: UserType) {
  return u === 'Leadership';
}

let u: UserType = 'Operator';   // ok
// u = 'Operater';              // ❌ erro: valor fora da união


```




---
transition: slide-left

---

# Interseção
<p class="text-lg leading-relaxed">`&` em TypeScript é o operador de interseção: o valor precisa satisfazer todos os tipos ao mesmo tempo.</p>


```ts
type Identifiable = { id: string };
type Timestamped  = { createdAt: string; updatedAt: string };

type Payment = Identifiable & Timestamped & {
  amountCents: number;
  status: 'authorized' | 'captured' | 'refused';
};

// precisa ter todos os campos
const p: Payment = {
  id: 'p1',
  createdAt: '2025-08-01',
  updatedAt: '2025-08-10',
  amountCents: 1299,
  status: 'captured',
};

```


---
transition: slide-left

---

# Interfaces aninhadas
<p class="text-lg leading-relaxed">Você modela um objeto complexo compondo interfaces menores (reutilizáveis). Isso melhora coesão, reuso e evolução independente dos “blocos” do domínio.</p>


```ts
export interface InstallmentPlan {
  amount: number;
  count: number;
  totalSum: number;
  paymentReferenceDate: string;
  gracePeriod?: number;
  daysToDownPayment?: number;
}

export interface Renegotiation {
  securities: SecurityRenegotiation[];
  installmentPlan: InstallmentPlan;
}

export interface RenegotiationProposal {
  id: string;
  offerId: string;
  type: string;
  status: string;
  expirationDate: string;
  creationDate: string;
  lastUpdateDate?: string;
  discountSettlement?: ProposalDiscountSettlement;
  customer: NegotiationsCustomer;
  renegotiation: Renegotiation; // composição
}


```



---
transition: slide-left

---

# Tipando respostas da API
<p class="text-lg leading-relaxed">Podemos utilizar para substituir no bom e velho PropTypes</p>


```ts
export interface ApiResponse {
  success: boolean
  message?: string
  data?: {
    id: string
    amountCents: number
    status: 'authorized' | 'captured' | 'refused'
    createdAt: string
  }
}

export async function getPayment(id: string): Promise<APApiResponseI> {
  const res = await http.get<ApiResponse>(`/payments/${id}`);
  return res.data.data;
}
```


---
transition: slide-left

---

# type x interface
<p class="text-lg leading-relaxed">Podemos utilizar para substituir no bom e velho PropTypes</p>

- Use `interface` para APIs de objeto que podem evoluir (ex.: modelos, props de componentes, contratos públicos, Window, “augment” de libs).

- Use `type` para unions, tuples, funções, mapped types e utilitários; também é ótimo para compor tipos rapidamente com & e para aliases simples.

*Na prática:*
<blockquote>

- Props de componente → tanto faz, mas interface costuma ficar mais legível e extensível em libs.

- Tipos de domínio (Money, IDs, DTOs) → qualquer um. Se precisar de utilitários/mapeados, prefira type.

- Chaves i18n/rotas/eventos (template literal/union) → type.
</blockquote>



---
transition: slide-left
layout: center
---
# Generics
<p class="text-lg leading-relaxed">Aqui as coisas começam a ficar interessantes</p>


**Generics** são uma forma de criar **funções, classes ou tipos que funcionam com vários tipos diferentes**, mas **sem perder a segurança de tipo**.

É como se você quisesse dizer:

> “Não sei ainda que tipo vou receber, mas quando eu souber, vou trabalhar com ele corretamente.”
>


---
layout: center
background: '#ffffff'
class: text-left
---
# Generics

<div class="grid grid-cols-2 gap-6 my-6">

  <div>

  ```ts
  function identidade(valor: any): any {
    return valor
  }
  ```
  Esse código funciona perfeitamente, porém, não temos segurança nenhuma. Se você passar uma string, pode receber um número e o Typescript não vai reclamar de nada.
  </div>

  <div>

  ```ts
  function identidade<T>(valor: T): T {
    return valor
  }

  const a = identidade<string>("Olá")  // a é do tipo string
  const b = identidade<number>(10)     // b é do tipo number
  const idade = identidade(25)         // TypeScript vai inferir o tipo number
  ```
  Nesse exemplo, `T` é um tipo genérico (que pode ser qualquer tipo: string, number, object, etc.).
  </div>

</div>


---
layout: center
background: '#ffffff'
class: text-left
---

## Por que eu deveria usar Generics?

- Reutilização: Você escreve uma função que serve para vários tipos.
- Segurança: mantém o tipo correto durante a execução do código.
- Clareza: facilita a legibilidade e entendimento do que esperar da função e do seu retorno.

### Com arrays

```tsx
function primeiroElemento<T>(array: T[]): T {
  return array[0]
}

const num = primeiroElemento([1, 2, 3])     // number
const str = primeiroElemento(["a", "b"])    // string
```

### Com interfaces

```tsx
interface Caixa<T> {
  valor: T
}

const caixaDeString: Caixa<string> = { valor: "Texto" }
const caixaDeNumero: Caixa<number> = { valor: 123 }
```


---
layout: center
background: '#ffffff'
class: text-left
---

## Constraints

Constraints são limites/regras em genéricos (via extends) que dizem quais tipos são aceitos por um parâmetro de tipo.

```tsx
function getName<T extends { name: string }>(obj: T): string {
  return obj.name;
}

getName({ nome: "João", idade: 30 }) // ✅
getName({ idade: 30 })               // ❌ erro: falta o nome
```

- `T extends { name: string }` é uma <b>constraint</b> do genérico `T`: ela diz que qualquer tipo usado como `T` precisa ser atribuível a `{ name: string }`.

- `T` <strong>deve ter pelo menos</strong> a propriedade `name` do tipo `string`. Ele pode ter campos extras, mas não pode faltar `name`.

- Como consequência, dentro da função, obj.name é seguro e o retorno pode ser string sem undefined.

---
layout: center
background: '#ffffff'
class: text-left
---

### `keyof` com Generics

Você pode criar funções que **acessam propriedades dinamicamente**

```tsx
function getValue<T, K extends keyof T>(obj: T, keyName: K): T[K] {
  return obj[keyName]
}

const pessoa = { nome: "Ana", idade: 25 }

const nome = getValue(pessoa, "nome")  // tipo: string
const idade = getValue(pessoa, "idade") // tipo: number

getValue(pessoa, "altura") // ❌ erro: 'altura' não existe em 'pessoa'
```

---
layout: center
background: '#ffffff'
class: text-left
---


## E aqui as coisas podem ficar bem confusas

Então, vamos por partes

<img src="/assets/generics-constrainst.png"  />


---
layout: default
transition: fade
---

# Conditional Types (if/else para tipos)

- Decidem **qual shape usar em tempo de tipo** com `T extends U ? X : Y`.  
- É como um **if** do TypeScript: adapta interfaces/campos conforme condição (status, método, flag), com autocomplete e **bloqueio de combinações inválidas**.

<br />

```ts
type Choose<T> = T extends string ? 'é string' : 'não é string';
```
<br />

```ts
interface ApiState<Loading extends boolean> {
  loading: Loading;
  data:  Loading extends true ? null : { id: string };
  error: Loading extends true ? null : string | null;
}
```

---
layout: center
background: '#ffffff'
class: text-left
---

## Conditional Types Exemplo


<div class="grid grid-cols-2 gap-6 my-6">

  <div>

  ```ts
   type Method = 'pix' | 'boleto';

    interface PaymentCommon {
      id: string;
      amountCents: number;
    }

    interface PixFields {
      method: 'pix';
      pixCode: string;   // E2E do PIX
      payerKey: string;     // chave PIX (telefone, email, EVP etc.)
    }

    interface BoletoFields {
      method: 'boleto';
      barcode: string;      // código de barras
      dueDate: string;      // ISO date
    }
  ```
  
  </div>

  <div>

  ```ts
  // Conditional type escolhe o shape certo conforme M
    type Payment<M extends Method> =
      M extends 'pix'
        ? PaymentCommon & PixFields
        : PaymentCommon & BoletoFields;

    /** Exemplos de uso */
    const p1: Payment<'pix'> = {
      id: 'p1',
      amountCents: 1299,
      method: 'pix',
      pixCode: 'E123...',
      payerKey: 'email@dominio.com',
    };

    const p2: Payment<'boleto'> = {
      id: 'p2',
      amountCents: 2590,
      method: 'boleto',
      barcode: '23793...',
      dueDate: '2025-09-01',
    };
  ```
 
  </div>

</div>


---
layout: center
background: '#3178c6'
class: text-center text-white
---

# ❓ Dúvidas

Espaço aberto para perguntas e discussões.


---
layout: center
background: '#3178c6'
class: text-center text-white
---

# 🙏 Obrigado!

## Foi um prazer compartilhar esse conteúdo com vocês  
### Vamos continuar aprendendo e explorando TypeScript 🚀
