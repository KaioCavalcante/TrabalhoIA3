# Trabalho Final – Raciocínio Espacial Neuro-Simbólico com LTNtorch

# Equipe:
# Kaio Cavalcante de Carvalho 22351230
# Luis Henrique de Carvalho Ribeiro 22352930
# Jorge harrison de Oliveira Pereira Junior 22551159

Este repositório contém a implementação do **Trabalho Final da disciplina Fundamentos de Inteligência Artificial (ICC260)**, cujo objetivo é desenvolver um agente **Neuro-Simbólico (NeSy)** utilizando **Logic Tensor Networks (LTN)** para aprendizado e raciocínio espacial em um ambiente 2D inspirado no dataset **CLEVR**.

O projeto combina aprendizado neural supervisionado com regras lógicas fuzzy, permitindo avaliar não apenas predição, mas também **satisfatibilidade lógica** de fórmulas simbólicas.

---

## 1. Neuro-Simbólico (NeSy) e Logic Tensor Networks (LTN)

Abordagens **Neuro-Simbólicas (NeSy)** integram aprendizado estatístico (redes neurais) com raciocínio simbólico (lógica). Diferentemente do Deep Learning tradicional, que aprende apenas relações entrada-saída, o paradigma NeSy permite impor **restrições lógicas explícitas** durante o treinamento.

As **Logic Tensor Networks (LTN)** implementam lógica de primeira ordem em um espaço contínuo, onde:
- Predicados são modelados por redes neurais;
- Fórmulas lógicas são avaliadas de forma diferenciável;
- A satisfação das regras é incorporada como parte da função de perda.

Neste trabalho, foi utilizado o framework **LTNtorch** para implementar predicados espaciais e axiomas lógicos.

---

## 2. Dataset CLEVR Simplificado

O ambiente é uma versão simplificada do **CLEVR**, representado por vetores de características (feature vectors), evitando o uso direto de imagens.

Cada objeto é descrito por um vetor de dimensão **11**, conforme abaixo:

| Índices | Descrição |
|------|----------|
| 0,1 | Coordenadas espaciais (x, y) normalizadas |
| 2,3,4 | Cor (one-hot: vermelho, verde, azul) |
| 5–9 | Forma (one-hot: círculo, quadrado, cilindro, cone, triângulo) |
| 10 | Tamanho (0 = pequeno, 1 = grande) |

Foram utilizados:
- **Datasets sintéticos** gerados aleatoriamente (25 objetos por cenário);
- **Um dataset manual**, fornecido a partir de tabela do enunciado.

---

## 3. Predicados e Axiomas Lógicos

### 3.1 Predicados Unários
- `isCircle(x)`
- `isSquare(x)`
- `isCylinder(x)`
- `isCone(x)`
- `isTriangle(x)`
- `isSmall(x)`
- `isBig(x)`

### 3.2 Predicados Binários
- `leftOf(x, y)`
- `rightOf(x, y)`
- `below(x, y)`
- `above(x, y)`
- `closeTo(x, y)`

### 3.3 Predicado Ternário
- `inBetween(x, y, z)`

---

### 3.4 Axiomas Lógicos

Foram adicionadas à Base de Conhecimento (KB) as seguintes regras:

- **Exclusividade de forma**  
  Um objeto não pode possuir duas formas simultaneamente.

- **Cobertura de formas**  
  Todo objeto pertence a exatamente uma forma.

- **Irreflexividade espacial**  
  Um objeto não pode estar à esquerda ou à direita de si mesmo.

- **Assimetria**  
  Se `x` está à esquerda de `y`, então `y` não está à esquerda de `x`.

- **Inverso**  
  `leftOf(x, y) ⇔ rightOf(y, x)`  
  `below(x, y) ⇔ above(y, x)`

- **Transitividade**  
  Se `x` está à esquerda de `y` e `y` está à esquerda de `z`, então `x` está à esquerda de `z`.

- **Regra de Empilhamento (`canStack`)**  
  Um objeto pode ser empilhado sobre outro se não for cone nem triângulo e se houver estabilidade geométrica.

---

## 4. Consultas Lógicas Avaliadas

Foram avaliadas consultas simbólicas de maior complexidade, incluindo:

- **Objeto mais à esquerda**  
  \[
  \exists x \; \forall y \; leftOf(x, y)
  \]

- **Objeto mais à direita**  
  \[
  \exists x \; \forall y \; rightOf(x, y)
  \]

- **Consulta Composta**  
  “Existe um objeto pequeno que está abaixo de um cilindro e à esquerda de um quadrado?”

- **Objeto entre dois outros (`inBetween`)**

A satisfação dessas consultas é avaliada de forma fuzzy, resultando em valores contínuos no intervalo \([0,1]\).

---

## 5. Experimentos e Resultados

### 5.1 Execuções

Foram realizadas:
- **5 execuções independentes**
- **5 datasets aleatórios distintos**
- Cada execução envolve treinamento completo e avaliação no conjunto de teste

### 5.2 Métricas Reportadas

Para cada execução, foram calculadas:

- **Satisfatibilidade lógica (`satAgg`)**
- **Acurácia**
- **Precisão**
- **Recall**
- **F1-Score**

Essas métricas foram reportadas tanto para:
- Predicados **unários** (formas e tamanho);
- Predicados **binários** e consultas lógicas.

Os resultados são exibidos diretamente no notebook em forma de **tabela consolidada**, conforme solicitado no enunciado.

---

## 6. Organização do Repositório

- `LTN_Tutorial.ipynb`  
  Notebook principal contendo:
  - Geração dos datasets
  - Definição dos predicados
  - Axiomas lógicos
  - Treinamento
  - Avaliação
  - Tabelas de resultados

---

## 7. Conclusão

Os resultados mostram que:
- Predicados unários (formas e tamanho) atingem métricas elevadas;
- Relações espaciais simples são aprendidas com boa consistência;
- Consultas compostas apresentam valores menores de satisfação, refletindo a maior complexidade do raciocínio relacional.

O trabalho demonstra a viabilidade do uso de **Logic Tensor Networks** para integrar aprendizado neural e raciocínio simbólico em tarefas espaciais.

---

