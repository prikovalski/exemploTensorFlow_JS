# Exemplo 00 — Classificação de Perfil com TensorFlow.js (Node)

Este projeto demonstra, de forma didática, como treinar e usar uma rede neural simples com `TensorFlow.js` em ambiente Node.js para classificar pessoas em três categorias:

- `premium`
- `medium`
- `basic`

O exemplo utiliza:

- **idade normalizada**
- **cor preferida** (one-hot)
- **localização** (one-hot)

para prever a classe mais provável de uma nova pessoa.

## Objetivo do projeto

Mostrar um fluxo completo e mínimo de Machine Learning com JavaScript:

1. Definição do modelo
2. Preparação dos dados de entrada e saída
3. Treinamento
4. Predição
5. Exibição de probabilidades por classe

## Tecnologias

- Node.js
- `@tensorflow/tfjs`
- `@tensorflow/tfjs-node`

## Estrutura do projeto

```text
.
├── index.js
├── package.json
└── package-lock.json
```

## Como executar

### 1) Instalar dependências

```bash
npm install
```

### 2) Rodar o projeto

```bash
npm start
```

O script `start` está configurado como:

```bash
node --no-warnings --watch index.js
```

## Como o modelo funciona

### Entrada (`inputShape: [7]`)

Cada pessoa é representada por um vetor de 7 posições na ordem:

1. `idade_normalizada`
2. `cor_azul`
3. `cor_vermelho`
4. `cor_verde`
5. `localizacao_sao_paulo`
6. `localizacao_rio`
7. `localizacao_curitiba`

Exemplo do dataset de treino no código:

```js
[
  [0.33, 1, 0, 0, 1, 0, 0],
  [0,    0, 1, 0, 0, 1, 0],
  [1,    0, 0, 1, 0, 0, 1]
]
```

### Saída (3 classes)

As classes são codificadas com one-hot na ordem:

```js
["premium", "medium", "basic"]
```

E os rótulos de treino:

```js
[
  [1, 0, 0], // premium
  [0, 1, 0], // medium
  [0, 0, 1]  // basic
]
```

### Arquitetura da rede

- Camada oculta densa:
  - `units: 80`
  - `activation: "relu"`
  - `inputShape: [7]`
- Camada de saída densa:
  - `units: 3`
  - `activation: "softmax"`

### Compilação

- `optimizer: "adam"`
- `loss: "categoricalCrossentropy"`
- `metrics: ["accuracy"]`

### Treinamento

- `epochs: 100`
- `shuffle: true`
- callback de log por época (`loss`)

## Predição

Após treinar, o script cria um exemplo de pessoa:

```js
{ nome: "Zé", idade: "28", cor: "verde", localizacao: "Curitiba" }
```

Essa pessoa é convertida para o formato numérico esperado (incluindo idade já normalizada) e passada para `model.predict`.

O resultado final é ordenado por probabilidade e exibido em formato legível, por exemplo:

```text
Predictions for Zé: ["basic (94.12%)","medium (4.87%)","premium (1.01%)"]
```

> Os valores exatos variam entre execuções devido à natureza estocástica do treinamento.

## Pontos importantes

- Este é um **exemplo educacional** com dataset extremamente pequeno.
- Com poucos dados, o modelo pode **sobreajustar (overfitting)**.
- A normalização e a ordem das features precisam ser **idênticas** entre treino e predição.

## Melhorias sugeridas

- Separar lógica em módulos (`data`, `model`, `train`, `predict`).
- Aumentar dataset e diversidade de exemplos.
- Criar validação/teste separado do treino.
- Persistir modelo treinado (`model.save`) e carregar depois.
- Medir métricas adicionais (matriz de confusão, precisão por classe).

## Solução de problemas

- Se houver erro ao carregar bindings nativos do TensorFlow no macOS, confira:
  - versão do Node.js compatível
  - reinstalação com `npm install`
  - limpeza de dependências e lockfile em casos de conflito

## Licença

Projeto privado para fins de estudo.
