# README: Coins ID - Identificador de Moeda para Pessoas com Deficiência Visual

## Descrição do Projeto

O **Coins ID** é um protótipo desenvolvido como TCC do curso técnico em eletroeletrônica no ano 2018, com o objetivo de criar um dispositivo acessível para pessoas com deficiência visual, permitindo que elas identifiquem moedas por meio de um feedback sonoro. Este arquivo explica detalhadamente como o sistema funciona, a lógica de programação utilizada (em Linguagem C) e os principais componentes envolvidos. Vale ressaltar que o projeto ainda está em fase prototípica e não é um produto finalizado. Algumas melhorias, como o layout, informações em Braille, tamanho do aparelho, e entre outros, são necessárias para a versão final.

O principal foco da avaliação foi a parte elétrica do projeto, que foi concluída com êxito, infelizmente nao tenho o código pro projeto, mas a lógica sera explicada ao decorrer do documento.

Pra melhor compreensão, vamos dividir o funcionamento em 3 partes
---


### 1. Coleta das Informações da Moeda

O **Coins ID** começa coletando duas informações essenciais da moeda: **tamanho** e **cor**. Essas informações são obtidas através de dois componentes.

#### Potenciômetro
O potenciômetro é um componente eletrônico que altera a corrente elétrica de acordo com sua resistência,basta girar seu eixo para ajustar a resistência.
Grudamos uma haste no eixo do potenciômetro, quando a moeda passa pelo trilho, sua haste é movida, alterando a resistência e o sinal elétrico. Quanto maior o raio da moeda, maior a elevação da haste, e por isto, a corrente elétrica é proporcional ao tamanho da moeda.

![haste do Potenciômetro](image/potenciometro1)
Dentro do circulo podemos ver a haste
![Potenciômetro](image/potenciometro2)
Potenciômetro

#### Fototransistor

![fototransistor](image/fototransistor)

Dentro do círculo destacado está o fototransistor, componente que assim como o potenciômetro, é capaz de alterar o sinal de corrente elétrica.
Mas sua diferença é que ele trabalha com a exposição à luz, ele altera o sinal de acordo com a luz ambiente.
Sabe os postes de luz nas ruas? Então, existe algo parecido com isto, é assim que o poste “sabe” quando é dia ou noite, risos.

Em nosso projeto, ele foi usado para distinguir a cor das moedas, pois descobrimos que algumas moedas brasileiras têm o mesmo tamanho, apesar de valores diferentes.

O led que está ao seu lado serve para iluminar a cavidade onde ele está, e com testes, descobrimos que quando a moeda tem cor de bronze, o sensor reage de uma forma, e quando é prata, ele reage de outra, isto porquê a moeda de prata reflete mais a luz do que a de bronze.



Com esses dois sensores, obtemos duas informações: o **tamanho** da moeda e sua **cor**.

---

### 2. Identificação do Valor da Moeda

Agora que temos os sinais de **tamanho** e **cor**, o [microncontolador](https://pt.wikipedia.org/wiki/Microcontrolador) **PIC** entra em ação. Ele é responsável por processar esses sinais e identificar a moeda.

![microcontrolador](image/microcontrolador)

O microcontrolador **PIC** é programado para receber os sinais dos sensores e compará-los com variáveis pré-definidas no código. Cada moeda tem um valor de tamanho e cor específico, que foi mapeado previamente. Não usamos banco de dados, mas sim variáveis simples para armazenar esses valores com uma margem de tolerância.
Cada sensor é conectado a um terminal do microcontrolador, no código, recebemos estes valores em uma variável numérica, com números inteiros, estes números são proporcionais á corrente elétrica que está passando pelos terminais, se forte ou fraca, o valor aumenta ou diminui.
Transformar um sinal de corrente elétrica em uma variável inteira foi uma das partes mais desafiantes do projeto, muito pela falta de experiência.

ps: você pode ter ouvido falar de microcontrolador, porém na marca Arduino, que é a mais famosa.

**Processo de Identificação**:
- O microcontrolador recebe os sinais e transforma em numeros inteiros, que são proporcionais à intensidade da corrente elétrica.
- Após a leitura dos sinais, os valores são comparados com os valores pré-estabelecidos para cada tipo de moeda (como 50 centavos, 1 real, etc.).
- No código, usamos o famoso **IF/ELSE** para identificar qual moeda foi inserida.

Após a identificação, o microcontrolador envia um sinal **binário** para o sistema de som, acionando a faixa correspondente e também um motor que retorna a haste do potenciômetro à sua posição inicial.

---

### 3. Reprodução do Som

A última etapa do processo é a reprodução do áudio que informa o valor da moeda. Para isso, utilizamos um **circuito integrado de som**, como o **ISD-1720**, que é capaz de gravar e reproduzir faixas de áudio.
Não lembro exatamente qual foi o Componente, mas o ISD-1720 cumpre o papel então usarei como exemplo.

**Funcionamento do Sistema de Som**:
- O circuito possui terminais de controle que podem ser acionados por um código binário. Cada combinação binária ativa uma faixa específica de áudio.
- os terminais energizados representam 1 e os não energizados representam 0
- Por exemplo, a quarta faixa corresponde à reprodução de "50 centavos", então para seleciona-la, energizamos apenas o terceiro terminal de controle, fazendo **001**. Se nao estou enganado, sou 4 pinos destinados ao controle das faixas.

![Circuito de Som ISD-1720](image/som)

---

## Simulação do Funcionamento

Vamos ilustrar o funcionamento do sistema com um exemplo de inserção de uma moeda de 50 centavos:

1. O usuário passa uma moeda de 50 centavos pelos trilhos do dispositivo.
2. O **potenciômetro** e o **fototransistor** captam o tamanho e a cor da moeda, respectivamente, e enviam os dados para o microcontrolador.
3. O microcontrolador processa os sinais, armazenando os valores em duas variáveis (tamanho e cor).
4. O sistema compara os valores capturados com as variáveis pré-definidas e identifica que os dados correspondem à moeda de 50 centavos.
5. O microcontrolador envia a combinação binária **001** para o circuito integrado de som, que ativa a quarta faixa de áudio, pronunciando "50 centavos".
6. O motor do potenciômetro é acionado para retornar a haste à sua posição inicial.
7. O sistema está pronto para receber a próxima moeda.

---


## Licença

Este projeto está licenciado sob a [Licença MIT](https://opensource.org/licenses/MIT), permitindo que você use, modifique e distribua o código de forma livre, desde que cite o autor original.