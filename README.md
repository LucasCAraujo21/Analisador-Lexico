# LUCAS CARVALHO DE ARAÚJO

# 🎟️ Analisador Léxico de Compra de Ingressos

Projeto acadêmico desenvolvido para a construção de um **Analisador Léxico utilizando a biblioteca Lark**, com o objetivo de reconhecer e classificar os elementos presentes em uma linguagem simples para realização de pedidos de ingressos.

O sistema recebe um pedido escrito em texto, realiza a análise léxica e transforma a entrada em uma sequência de **tokens**, apresentando informações como categoria, lexema, linha e coluna.

Além da análise dos tokens, o projeto possui uma interface interativa baseada em **Jupyter/ipywidgets**, permitindo visualizar o texto analisado, os tokens reconhecidos, um recibo da compra e estatísticas da análise.

---

## 📌 Objetivo

O principal objetivo do projeto é demonstrar na prática o funcionamento de um **analisador léxico** dentro do processo de construção de um compilador.

O analisador identifica elementos da linguagem de pedidos de ingressos, como:

* Palavras reservadas;
* Quantidades;
* Datas;
* Horários;
* Preços;
* Nomes de eventos;
* Setores;
* Lotes;
* Formas de pagamento;
* Números;
* Comentários.

A entrada pode conter, por exemplo:

```text
INGRESSO 2x "Show Slayer" SETOR pista LOTE 2
MEIA NOME Lucas Carvalho R$ 200,00 EM 10/12/2026
PAGAMENTO pix
```

O analisador transforma essa entrada em tokens, como:

```text
INGRESSO          -> INGRESSO
QTD               -> 2x
ITEM              -> "Show Slayer"
SETOR             -> SETOR
SETOR_ESCOLHIDO   -> pista
LOTE              -> LOTE
NUMERO            -> 2
TIPO_ESCOLHIDO    -> MEIA
NOME              -> NOME
COMPRADOR         -> Lucas
COMPRADOR         -> Carvalho
PRECO             -> R$ 200,00
EM                -> EM
DATA              -> 10/12/2026
PAGAMENTO         -> PAGAMENTO
FORMA_PGTO        -> pix
```

---

## 🧠 O que é um analisador léxico?

O analisador léxico é responsável pela primeira etapa do processamento de uma linguagem.

Ele recebe uma sequência de caracteres e identifica os **lexemas**, agrupando-os em categorias chamadas **tokens**.

Neste projeto, por exemplo:

```text
2x
```

é reconhecido como:

```text
QTD
```

Enquanto:

```text
R$ 200,00
```

é reconhecido como:

```text
PRECO
```

E:

```text
10/12/2026
```

é reconhecido como:

```text
DATA
```

O projeto utiliza o modo `basic` do Lark para realizar a leitura dos caracteres da esquerda para a direita e produzir os tokens.

---

# 🛠️ Tecnologias utilizadas

* **Python**
* **Lark**
* **Jupyter Notebook**
* **IPython**
* **ipywidgets**
* **HTML/CSS**

### Bibliotecas Python

```text
lark
ipywidgets
ipykernel
```

Também são utilizados módulos nativos do Python, como:

```python
html
collections
```

---

# 🔤 Tokens reconhecidos

A linguagem possui diferentes categorias de tokens.

## Palavras reservadas

| Token             | Exemplos      |
| ----------------- | ------------- |
| `COMPRAR`         | comprar       |
| `INGRESSO`        | ingresso      |
| `EVENTO`          | evento        |
| `TIPO`            | tipo          |
| `TIPO_ESCOLHIDO`  | meia, inteira |
| `FORMA_PGTO`      | pix, cartão   |
| `PAGAMENTO`       | pagamento     |
| `CONFIRMAR`       | confirmar     |
| `CANCELAR`        | cancelar      |
| `SETOR`           | setor         |
| `SETOR_ESCOLHIDO` | pista, banco  |
| `LOTE`            | lote          |
| `EM`              | em            |
| `AS`              | as            |
| `NOME`            | nome          |

---

## Valores e literais

| Token    | Exemplo         |
| -------- | --------------- |
| `QTD`    | `2x`            |
| `DATA`   | `10/12/2026`    |
| `HORA`   | `14:32`         |
| `PRECO`  | `R$ 200,00`     |
| `NUMERO` | `2`             |
| `ITEM`   | `"Show Slayer"` |

---

##  TABELA GERAL DE TOKENS

| Token             | Descrição                  | Regex                            | Exemplo         | Prioridade |
| ----------------- | -------------------------- | -------------------------------- | --------------- | ---------: |
| `FORMA_PGTO`      | Forma de pagamento         | `/(pix\|cart[aã]o)\b/i`          | `pix`           |          3 |
| `PAGAMENTO`       | Palavra-chave de pagamento | `/pagamento\b/i`                 | `pagamento`     |          3 |
| `TIPO`            | Palavra-chave do tipo      | `/tipo\b/i`                      | `tipo`          |          3 |
| `TIPO_ESCOLHIDO`  | Tipo de ingresso           | `/(meia\|inteira)\b/i`           | `meia`          |          3 |
| `COMPRAR`         | Comando de compra          | `/comprar\b/i`                   | `comprar`       |          3 |
| `INGRESSO`        | Identifica ingresso        | `/ingresso\b/i`                  | `ingresso`      |          3 |
| `EVENTO`          | Identifica evento          | `/evento\b/i`                    | `evento`        |          3 |
| `CONFIRMAR`       | Confirma a operação        | `/confirmar\b/i`                 | `confirmar`     |          3 |
| `CANCELAR`        | Cancela a operação         | `/cancelar\b/i`                  | `cancelar`      |          3 |
| `SETOR`           | Palavra-chave do setor     | `/setor\b/i`                     | `setor`         |          3 |
| `SETOR_ESCOLHIDO` | Setor escolhido            | `/(pista\|banco)\b/i`            | `pista`         |          3 |
| `LOTE`            | Palavra-chave do lote      | `/lote\b/i`                      | `lote`          |          3 |
| `EM`              | Precede a data             | `/em\b/i`                        | `em`            |          3 |
| `AS`              | Palavra-chave de horário   | `/as\b/i`                        | `as`            |          3 |
| `NOME`            | Indica nome do comprador   | `/nome\b/i`                      | `nome`          |          3 |
| `COMPRADOR`       | Nome do comprador          | `/[A-Za-zÀ-ÿ]+/`                 | `Lucas`         |          0 |
| `QTD`             | Quantidade                 | `/\d+x/`                         | `2x`            |          2 |
| `DATA`            | Data                       | `/\d{2}\/\d{2}\/\d{4}/`          | `10/12/2026`    |          2 |
| `HORA`            | Horário                    | `/\d{2}:\d{2}/`                  | `14:32`         |          2 |
| `PRECO`           | Preço                      | `/R\$ ?\d{1,3}(\.\d{3})*,\d{2}/` | `R$ 200,00`     |          2 |
| `ITEM`            | Nome do evento/item        | `/"[^"]+"/`                      | `"Show Slayer"` |          0 |
| `NUMERO`          | Número do lote             | `/\d+/`                          | `2`             |          0 |
| `COMENTARIO`      | Comentário ignorado        | `/#[^\n]*/`                      | `# compra`      |          0 |


## Dados do comprador

O token `COMPRADOR` é utilizado para reconhecer palavras que representam o nome do comprador.

Exemplo:

```text
Lucas Carvalho
```

pode ser identificado como:

```text
COMPRADOR -> Lucas
COMPRADOR -> Carvalho
```

---

# 🧾 Exemplo de pedido

Uma entrada válida pode ser:

```text
INGRESSO 2x "Show Slayer" SETOR pista LOTE 2 tipo meia nome Lucas Carvalho de Araújo R$ 200,00 EM 10/12/2026
PAGAMENTO pix
```

O sistema identifica os diferentes elementos da entrada e apresenta os tokens correspondentes.

---

# 💰 Cálculo do recibo

Após a análise léxica, o projeto também consegue utilizar os tokens reconhecidos para montar um recibo da compra.

O valor de cada item é calculado utilizando:

```python
quantidade * preço
```

Por exemplo:

```text
2x "Show Slayer" → R$ 200,00
```

resulta em:

```text
2 × 200 = R$ 400,00
```

O valor total do pedido é obtido pela soma dos valores individuais:

```python
total = sum(
    item["qtd"] * item["preco"]
    for item in itens
)
```

Para uma compra com:

```text
2 × R$ 200,00 = R$ 400,00
1 × R$ 150,00 = R$ 150,00
3 × R$ 1.500,00 = R$ 4.500,00
```

o recibo apresenta:

```text
TOTAL: R$ 5.050,00
```

---

# 🎨 Interface

O projeto possui uma interface interativa utilizando `ipywidgets`.

A interface disponibiliza exemplos de pedidos e permite analisar a entrada.

São apresentadas quatro áreas principais:

### 🎨 Colorido

Apresenta o texto original com os lexemas coloridos de acordo com sua categoria.

### 📋 Tokens

Exibe uma tabela contendo:

* Número do token;
* Tipo;
* Lexema;
* Linha;
* Coluna.

### 🧾 Recibo

Apresenta os ingressos identificados e calcula:

* Quantidade;
* Evento;
* Preço unitário;
* Total do item;
* Data;
* Forma de pagamento;
* Total da compra.

### 📊 Estatísticas

Apresenta a frequência de cada categoria de token encontrada no pedido.

---

# 🎨 Cores dos tokens

Os tokens são apresentados com diferentes cores para facilitar a visualização.

| Categoria                | Cor      |
| ------------------------ | -------- |
| Palavras reservadas      | Azul     |
| Datas e horários         | Laranja  |
| Preços e quantidades     | Verde    |
| Textos e identificadores | Ciano    |
| Outros tokens            | Vermelho |
| Chaves PIX               | Rosa     |

A função:

```python
cor_do_token(tipo)
```

é responsável por buscar a cor correspondente à categoria do token.

---

# ⚠️ Tratamento de erros

O analisador também possui mensagens de ajuda para alguns erros comuns.

Por exemplo, caso o usuário utilize aspas sem fechá-las, o sistema pode apresentar uma mensagem indicando o problema.

Alguns exemplos de dicas implementadas:

### Aspas

```text
Parece que você abriu aspas e não fechou.
Todo texto precisa de "abre" e "fecha".
```

### Preço

```text
Preço inválido. Informe o preço com "R$",
por exemplo: R$ 25,90.
```

### Quantidade

```text
A quantidade deve ser informada como número
seguido de "x", por exemplo: 2x ou 10x.
```

O sistema também informa a **linha e coluna** onde o erro léxico foi encontrado.

---

# 💬 Comentários

A linguagem permite comentários utilizando `#`.

Exemplo:

```text
# Compra de ingressos - Sympla

INGRESSO 2x "Show Slayer" SETOR pista
```

Os comentários são ignorados pelo analisador através da regra:

```python
COMENTARIO: /#[^\n]*/

%ignore COMENTARIO
```

Dessa forma, o comentário não aparece como token na análise final.

---

# ⚙️ Regras léxicas

Alguns exemplos das expressões regulares utilizadas:

### Quantidade

```regex
\d+x
```

Reconhece:

```text
2x
10x
100x
```

### Data

```regex
\d{2}/\d{2}/\d{4}
```

Reconhece:

```text
10/12/2026
```

### Horário

```regex
\d{2}:\d{2}
```

Reconhece:

```text
14:32
```

### Preço

```regex
R\$ ?\d{1,3}(\.\d{3})*,\d{2}
```

Reconhece formatos como:

```text
R$ 25,90
R$ 200,00
R$ 1.500,00
```

### Item

```regex
"[^"]+"
```

Reconhece textos entre aspas, como:

```text
"Show Slayer"
"Show Korn"
"Show Slipknot"
```

---

# 🔢 Prioridade dos tokens

Alguns tokens possuem prioridade explícita no Lark.

Por exemplo:

```python
QTD.2: /\d+x/
DATA.2: /\d{2}\/\d{2}\/\d{4}/
PRECO.2: /R\$ ?\d{1,3}(\.\d{3})*,\d{2}/
```

As prioridades ajudam a resolver situações em que diferentes expressões regulares poderiam reconhecer partes semelhantes da entrada.

As palavras reservadas utilizam prioridade `3`, enquanto alguns literais utilizam prioridade `2`.

---

# 🚀 Como executar

## 1. Clone o projeto

```bash
git clone URL_DO_REPOSITORIO
```

Entre na pasta:

```bash
cd nome-do-projeto
```

## 2. Instale as dependências

```bash
python -m pip install lark ipywidgets ipykernel
```

## 3. Abra o projeto no VS Code

```bash
code .
```

## 4. Execute como Jupyter Notebook

Como o projeto utiliza `ipywidgets` e `IPython.display`, recomenda-se executá-lo em um ambiente **Jupyter Notebook**.

No VS Code, instale as extensões:

* Python
* Jupyter

Depois abra o arquivo `.ipynb` e execute as células.

---

# 📁 Estrutura sugerida

```text
analisador-lexico/
│
├── analisador_lexico.ipynb
├── README.md
└── requirements.txt
```

O arquivo `requirements.txt` pode conter:

```text
lark
ipywidgets
ipykernel
```

Para instalar:

```bash
python -m pip install -r requirements.txt
```

---

# 🧪 Exemplos disponíveis

O projeto possui exemplos para testar diferentes situações:

### Compra com meia-entrada

```text
INGRESSO 2x "Show Slayer" SETOR pista LOTE 2 MEIA
NOME Lucas Carvalho R$ 200,00 EM 10/12/2026
PAGAMENTO pix
```

### Compra com entrada inteira

```text
INGRESSO 2x "Show Slayer" SETOR pista LOTE 2 inteira
NOME Lucas Carvalho R$ 200,00 EM 10/12/2026
PAGAMENTO cartão
```

### Erro de aspas

```text
INGRESSO 1x "Show Slayer R$ 19,90
PAGAMENTO cartão
```

### Formato de preço incorreto

```text
INGRESSO 2x "Show Slayer" 8.50
```

### Tipo de ingresso incorreto

```text
INGRESSO 2x "Show Slayer" tipo meio
```

---

# 📚 Conceitos demonstrados

Este projeto demonstra conceitos importantes relacionados à construção de compiladores e linguagens:

* Análise léxica;
* Tokens;
* Lexemas;
* Expressões regulares;
* Prioridade de tokens;
* Reconhecimento de palavras reservadas;
* Tratamento de erros léxicos;
* Ignorar comentários;
* Linha e coluna dos tokens;
* Construção de uma gramática com Lark;
* Visualização de tokens;
* Processamento dos tokens para geração de um recibo.

---

# 👨‍💻 Autor

Projeto desenvolvido como atividade acadêmica de **Ciência da Computação**, com foco no estudo de **Compiladores e Análise Léxica**.

**Lucas Carvalho de Araújo**

GitHub:
https://github.com/LucasCAraujo21

---

## 📄 Licença

Projeto desenvolvido para fins acadêmicos e educacionais.
