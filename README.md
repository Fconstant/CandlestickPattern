# Candlestick Pattern

Candlestick Pattern é um projeto desenvolvido pelos alunos da Fatec Carapicuíba 5ºADSM 2016: Felipe Cavalcante Constantino e Luciano Aragão Chiarelli.
O sistema processa os dados de forma distribuída (Cliente-Mestre-Escravo). 

## Introdução
O projeto foi baseado em um artigo da Journal of Banking & Finance (LINK AQUI) que aborda formas de como obter lucro na bolsa de valores, escolhendo melhor qual empresa comprar ações, e quando vender. Para isso, é abordado pelo artigo fórmulas e procedimentos que se devem realizar para alcançar tal objetivo.

Primeiramente, é apresentado ao leitor a ideia de Candlestick: uma representação gráfica de um ativo a ser analisado (No caso do artigo, ele define cada Candlestick como sendo um dia de 24h). Tendo os Candlesticks em mãos, dados um certo conjunto sequencial de Candlesticks, encontraremos alguns padrões, no qual chamaremos de Candlestick Patterns (No artigo, são abordados somente oito principais e mais importantes). Achados todos os Candlestick Patterns, é necessário saber qual a tendência pela qual o mercado seguirá (Trends) e, baseado nessa tendência, será definido uma estratégia de arrendamento (Holding Strategy) para saber em que momento se deve vender ou não as ações.

Com isso, foi desenvolvido um programa em _Java_ na tentativa de utilizar tais procedimentos em arquivos de histórico de ações.
Somente foi abordado uma Trend e uma Holding Strategy básica: Analisar se o preço de fechamento (Close) do último dia de um Candlestick Pattern, e então verificar se a venda de cada terço desta ação nos próximos três dias retornará lucro.
Serão aceitos qualquer Candlestick Pattern, que atendam uma grámatica (definido abaixo), representados por um conjunto de expressões.

## Uso
Testa condições/expressões dadas pelo Cliente em arquivos financeiros .csv (comma-separated values), verificando se a condição inserida é verdadeira, e, se for, verificando se a Holding Strategy adotada atende ao requisito e retorna um lucro viável para cada .

## Organização
O proejto se divide em 3 componentes:
- **Cliente:** Representado pela classe _Client_. Conecta-se com o Mestre, e logo que a conexão for bem sucedida, manda conjuntos de condições/expressões para a máquina Mestre.
- **Mestre:** Primeiramente, identifica os escravos na rede, e quando encontrado, envia todos os arquivos .csv, e após o envio, os coloca numa fila de escravos disponíveis (ociosos). Em seguida recebe um conjunto de expressões do cliente e as manda para um escravo disponível (Esse é um ciclo interminável até que o Cliente pare de enviar expressões.
- **Escravo:** Representado pela classe _Slave_. Conecta-se com o mestre, e então fica ocioso até que um conjunto de expressões tenha chegado até ele. Assim que receber, ele começa o trabalho de percorrer o(s) arquivo(s) .csv a fim de testar o conjunto de expressões e retornar um resultado.
 
O resultado completo (Representado pela classe _GainStatistics_) é retornado ao Mestre, que é retornado de volta ao cliente e então printado na tela do mesmo.

## Como utilizar
No mestre, é preciso ter os arquivos em formato .csv com o padrão de colunas pré definido: Date, Open, High, Low, Close, mas não necessariamente nesta ordem.
Estes devem estar contidos em um diretório com o nome de _finance_ no mesmo diretório do executável. Esse tipo de arquivo pode ser facilmente obtido por meio do [Yahoo! Finance](finance.yahoo.com/).

Atendidos tais requisitos, siga o passo a passo:
1. Inicie o Mestre, ele começará a disparar pacotes de requisição na rede afim de encontrar Escravos. É necessário iniciar o Mestre antes do Cliente.
2. Inicie o(s) Escravo(s), e então ele começará a receber os arquivos .csv do Mestre.
3. Inicie o Cliente, e forneça o IP do Mestre
4. A partir do Cliente, forneça um conjunto de expressões que serão testadas.
5. Para ir para o próximo conjunto de expressões, simplemente digite PROX.
6. Quando terminar, digite SAIR. E então o Cliente somente aguardará os resultados para serem printados na tela.
7. Após o término dos resultados, o sistema terminará.

OBS: O Escravo não precisa necessariamente ser iniciado antes do Cliente, a única restrição é que o Mestre deve ser iniciado antes do Cliente.

## Exemplo de expressões
Quando o cliente for iniciado com sucesso, e já conectado com o Mestre, será solicitado que entre com um conjunto de expressões.
Siga o exemplo abaixo, que simula uma entrada de dados feita por parte do Cliente:
```
exp: P1_CLOSE > P2_CLOSE
exp: P3_OPEN + P3_CLOSE <= P2_CLOSE
exp: PROX
exp: P3_OPEN = P2_OPEN
exp: P4_CLOSE - P1_OPEN >= P1_HIGH * 3
exp: SAIR
```
No exemplo acima, temos dois conjuntos de expressões, o primeiro antes do PROX e o segundo depois de PROX. O comando PROX define um limitador entre conjuntos de expressões. E o comando SAIR, um encerrador.