# Netflix challenge

### Autores:
- [Marcelo Rabello Barranco](https://github.com/Maraba23)
- [Thomas Chiari Ciocchetti de Souza](https://github.com/thomaschiari)

### Descrição:

O objetivo deste trabalho é implementar um sistema de recomendação de filmes tendo uma base de dados como referência. Nessa base de dados, devemos escolher de forma aleatória alguma nota escolhida por algum usuario e remover essa nota. Em seguida atribuimos um valor aleatório entre 0 e 5 para essa nota. O sistema deve tentar prever qual seria a nota que o usuário daria para aquele filme. Para isso, utilizamos o algoritmo SVD.

### Requisitos e Instruções:
1. Certifique-se de ter instalado Python na versão 3.9 ou superior.
2. Clone o repositório do projeto através do comando no terminal: 
`git clone https://github.com/thomaschiari/Alpha-Netflix-Challenge.git`
3. (Recomendado) Crie um ambiente virtual para o projeto através do comando no terminal:
`python -m venv venv`
4. Ative o ambiente virtual através do comando no terminal:
`source env/bin/activate` (Linux)
`env\Scripts\activate` (Windows)
5. Instale as dependências do projeto através do comando no terminal:
`pip install -r requirements.txt`
6. Entre no arquivo demo.ipynb e execute as células (**Atenção: Nao executar as cedulas de iteracoes, pois vai afetar nossa base de dados**)
7. Para desativar o ambiente virtual, basta executar o comando no terminal:
`deactivate`

### Teoria:

O SVD eh um tipo de decomposicao de matriz que permite a representacao de uma matriz em termos de suas matrizes de valores singulares. Valores singulares sao os valores que sao maiores que zero e que sao obtidos a partir da decomposicao de uma matriz, são uma medida numérica da "importância" de cada coluna da matriz em termos de contribuição para a variabilidade global da matriz. A decomposicao de uma matriz em valores singulares eh feita da seguinte forma:

Vamos supor uma matriz A de notas representada por 3 usuarios (linhas) e 4 filmes (colunas). A matriz A eh representada por:

$$
A = \begin{bmatrix}
1 & 2 & 3 & 4 \\
5 & 3 & 2 & 3 \\
1 & 4 & 1 & 5
\end{bmatrix}
$$

Nessa matriz temos que para o filme 1, o usuario 1 deu nota 1, o usuario 2 deu nota 5 e o usuario 3 deu nota 1. Para o filme 2, o usuario 1 deu nota 2, o usuario 2 deu nota 3 e o usuario 3 deu nota 4. E assim por diante.

Vamos supor que nao soubessemos que o usuario 1 tenha dado nota 3 para o filme 3. Entao, vamos remover essa nota da matriz A e atribuir um valor aleatorio entre 1 e 5 para essa nota. A matriz A ficaria assim:

$$
A = \begin{bmatrix}
1 & 2 & X & 4 \\
5 & 3 & 2 & 3 \\
1 & 4 & 1 & 5
\end{bmatrix}
$$

Vamos dizer q nosso X eh 1. Entao, a matriz A ficaria assim:

$$
A = \begin{bmatrix}
1 & 2 & 1 & 4 \\
5 & 3 & 2 & 3 \\
1 & 4 & 1 & 5
\end{bmatrix}
$$

Agora, vamos decompor a matriz A em valores singulares. Para isso, vamos utilizar a biblioteca numpy. A decomposicao de valores singulares eh feita da seguinte forma:

$$
A = U \Sigma V^T
$$

- As colunas de U sao auto-vetores da matriz $A^TA$
- As linhas de $V^T$ sao auto-vetores da matriz $AA^T$
- $\Sigma$ é uma matriz onde $s_{ii}$ é a raiz quadrada dos auto-valores de $A^TA$ ou $AA^T$

para mais informacoes sobre como funciona a decomposicao de valores singulares, veja os links:

- [Repositório de Algebra linear](https://github.com/tiagoft/alglin-alunos/blob/cc3ac830eecda90f5d80c88e7adb3a95a1d19b5e//demonstracao_svd.md)

- [Wikipedia](https://en.wikipedia.org/wiki/Singular_value_decomposition)

<hr style="margin: 0 20% 20% ">

## Celulas:

Para cada celula do jupter nootebook, temos uma explicacao do que ela faz em markdown. Eh recomendado que nao execute as celulas de iteracoes, pois vai afetar nossa base de dados.

Para o nosso codigo em `demo.ipynb`, foi escolhido $K=150$ para a decomposicao de valores singulares.

## Sobre o K

K representa o número de valores singulares mais importantes que você deseja manter na decomposição SVD da matriz. Ao definir o valor de K, você está especificando quantos dos valores singulares mais importantes deseja manter na decomposição. Ou seja, você está comprimindo a matriz original para uma matriz de dimensão reduzida, mantendo apenas os K valores singulares mais importantes.

Para definirmos o nosso K, fizemos 20 iteracoes, com 5 Ks diferentes:

- K = 300
- K = 200
- K = 150
- K = 100
- K = 50

Mesmo sabendo que 20 iteracoes nao eh o suficiente para definir o melhor K, fizemos isso porque nao tinhamos tempo suficiente para um teste mais elaborado. Para visualizar o resultado desses testes va para o arquivo `testes.ipynb` **Lembre-se de que nao execute as celulas de iteracoes, pois vai afetar nossa base de dados.**

Na ultima cedula desse arquivo, plotamos um grafico que mostra que $K=150$ eh um bom valor para a decomposicao de valores singulares, visto que a partir de $K=150$ o erro nao diminui muito mais.
