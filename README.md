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
6. Entre no arquivo demo.ipynb e execute as células (*Atenção: Não executar as celulas de iteracoes, pois vai afetar nossa base de dados*)
7. Para desativar o ambiente virtual, basta executar o comando no terminal:
`deactivate`

### Teoria:

Os autovalores e autovetores são importantes para decompor matrizes. O autovetor e seu autovalor são definidos por:

$$
AX = \lambda X
$$

Onde $A$ é uma matriz, $X$ é a matriz de autovetores e $\lambda$ é o autovalor correspondente.

Para decompor a matriz A em autovalores e autovetores, é necessário encontrar os valores de $\lambda$ e $X$ que satisfazem a equação acima. Assim:

$$
A = X \Lambda X^{-1}
$$

Para encontrar tais valores, é necessário resolver o problema de autovalores e autovetores. Para isso, vamos utilizar a decomposição de valores singulares (SVD).

O SVD é uma técnica usada para decompor uma matriz. É um tipo de decomposição de matriz que permite a representação de uma matriz em termos de suas matrizes de valores singulares. 
Valores singulares são os valores que são maiores que zero e que são obtidos a partir da decomposição de uma matriz. Fornecem informações sobre a estrutura da matriz, especialmente se for muito grande, como é o caso que trabalharemos aqui. Permitem que se reduza a dimensão da matriz, representando apenas seus eixos mais importantes.
Em resumo, são uma medida numérica da "importância" de cada coluna da matriz em termos de contribuição para a variabilidade global da matriz. A decomposição de uma matriz em valores singulares é feita da seguinte forma:

Vamos supor uma matriz A de notas representada por 3 usuários (linhas) e 4 filmes (colunas). A matriz A é representada por:

$$
A = \begin{bmatrix}
1 & 2 & 3 & 4 \\
5 & 3 & 2 & 3 \\
1 & 4 & 1 & 5
\end{bmatrix}
$$

Nessa matriz temos que para o filme 1, o usuário 1 deu nota 1, o usuário 2 deu nota 5 e o usuário 3 deu nota 1. Para o filme 2, o usuário 1 deu nota 2, o usuário 2 deu nota 3 e o usuário 3 deu nota 4. E assim por diante.

Vamos supor que nao soubéssemos que o usuario 1 tenha dado nota 3 para o filme 3. Então, vamos remover essa nota da matriz A e atribuir um valor aleatorio entre 1 e 5 para essa nota. A matriz A ficaria assim:

$$
A = \begin{bmatrix}
1 & 2 & X & 4 \\
5 & 3 & 2 & 3 \\
1 & 4 & 1 & 5
\end{bmatrix}
$$

Vamos dizer que nosso X é 1. Entao, a matriz A ficaria assim:

$$
A = \begin{bmatrix}
1 & 2 & 1 & 4 \\
5 & 3 & 2 & 3 \\
1 & 4 & 1 & 5
\end{bmatrix}
$$

Agora, vamos decompor a matriz A em valores singulares. Para isso, vamos utilizar as bibliotecas `numpy` e `scipy`. A decomposicao de valores singulares é feita da seguinte forma:

$$
A = U \Sigma V^T
$$

- As colunas de U sao auto-vetores da matriz $A^TA$
- As linhas de $V^T$ sao auto-vetores da matriz $AA^T$
- $\Sigma$ é uma matriz em que $s_{ii}$ é a raiz quadrada dos auto-valores de $A^TA$ ou $AA^T$

Inserimos um valor aleatório em nossa matriz original, que agora está com um ruído. A decomposição SVD nos permite reduzir tais ruídos.
A ideia básica é que a matriz original pode ser aproximada por uma matriz de menor dimensão, que contém apenas os valores singulares mais importantes.
Essa matriz pode ser obtida a partir da trincagem da matriz $\Sigma$ em três matrizes: $U$, $\Sigma$ e $V^T$. 
A matriz resultante será menos sensível ao ruído e, portanto, mais fácil de trabalhar.
Isso nos permite obter uma aproximação da matriz original, mas com uma dimensão menor, podendo obter uma aproximação, no exemplo acima, da nota do usuário 1 para o filme 3.

No arquivo `Desafio2.ipynb`, temos uma explicação e aplicação do SVD.
Primeiro, atribuímos um valor aleatório de uma nota para uma linha aleatória da matriz original, a fim de simular um ruído.
Em seguida, realizamos a decomposição SVD da matriz original, obtendo as matrizes $U$, $S$ e $VT$.
Tais valores serão usados em uma compressão para dois componentes, reconstruindo uma matriz aproximada da matriz original, que deverá conter um valor aproximado da nota original do usuário. 
O objetivo de tal parte é demonstrar como a SVD pode ser usada parareduzir a dimensão de uma matriz, removendo ruídos e reduzindo a complexidade de um problema.

Em seguida, faremos diversas iterações para obter diversas simulações, para um número de componentes (K) fixo, a ser explicado abaixo, verificando, assim, a distribuição dos erros cometidos na reconstrução da matriz original.
Os resultados são salvos em um arquivo `csv`, para que sejam lidos no arquivo `demo.ipynb`, onde será possível realizar uma demonstração da SVD e uma análise dos resultados obtidos.

## Células:

Para cada célula do Notebook, temos uma explicação do que ela faz em Markdown. É recomendado que não execute as celulas de iterações, pois vai afetar nossa base de dados.

Para o nosso código em `demo.ipynb`, foi escolhido $K=150$ para a decomposição de valores singulares.

## Sobre o valor de K escolhido:

K representa o número de valores singulares mais importantes que você deseja manter na decomposição SVD da matriz. Ao definir o valor de K, você está especificando quantos dos valores singulares mais importantes deseja manter na decomposição. Ou seja, você está comprimindo a matriz original para uma matriz de dimensão reduzida, mantendo apenas os K valores singulares mais importantes.

Para definirmos o nosso K, fizemos 20 iterações, com 5 K's diferentes:

- K = 300
- K = 200
- K = 150
- K = 100
- K = 50

Mesmo sabendo que 20 iteracoes podem não ser suficientes para definir o melhor K, nos permitiu visualizar qual valor de K seria o mais adequado para a nossa base de dados, com base nos erros absolutos médios das notas preditas e seus desvios-padrão. Com K=150, obtivemos os menores valores para ambos.
Para visualizar o resultado desses testes, vá para o arquivo `testes.ipynb`. **Executar as células com iterações pode modificar nossa base de dados, além de demorar um tempo considerável.**

Na ultima cedula desse arquivo, plotamos um gráfico que mostra que $K=150$ é um bom valor para a decomposição de valores singulares, visto que a partir de $K=150$ o erro não diminui mais significativamente.

### Conclusão:

Obtivemos, ao final da demonstração, uma matriz de notas preditas, que pode ser usada para recomendação de filmes.
O erro absoluto médio foi de, aproximadamente, 1.8, com desvio padrão de, aproximadamente, 1.2.
Os resultados obtidos poderiam ser alterados se testássemos as 1000 iterações realizadas para diferentes valores de K. 
Contudo, o resultado é aparentemente satisfatório. O erro absoluto médio pode ser considerado baixo, visto que as notas variam de 0 a 5 e, portanto, sendo um valor abaixo de 2.5 (metade da escala de notas), consideramos o erro baixo.
Ademais, analisando o histograma de distribuição dos erros absolutos médios das notas preditas, a maior densidade de erros se encontra abaixo de 1, valor ainda mais baixo do erro absoluto médio, que pode ter sido puxado para cima por uma minoria de erros maiores de 4.
Portanto, levando em consideração as informações acima expostas, acreditamos que nosso sistema é satisfatório e poderia ser utilizado em produção para reduzir a complexidade de um problema de recomendação de filmes.
