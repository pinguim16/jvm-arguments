# JVM Argumentos

Recentemente em um projeto que passei tivemos que extrair o máximo de performe de uma aplicação Spring
Boot, com isso precisei me aprofundar na JVM e em todos os seus recursos.
Com base nessas pesquisas resolvi fazer um sintese reunindo toda a informação que consegui aprender
sobre a JVM, tendo em vista em ajudar alguém que talvez possa estar passando pela mesma dificuldade.

##JVM

Antes de apronfundarmos em o que é a JVM, vamos entender um pouco de seu conceito.
Quando a linguagem java foi criada em 1991 a sua ideia não era somente criar mais uma linguagem 
de programação, mas sim criar uma linguagem que pudesse ser utilizada em vários equipamentos, além
dos computadores.
Em 1995, a Sun trouxe para o mundo a introdução da plataforma Java, onde a mesma era constituída:
* a linguagem Java;
* JVM (Java Virtual Machine);
* E o inicio das apis/bibliotecas;

Para entendermos um pouco mais sobre a JVM, precisamos entender seu conceito de máquina virtual.
Uma máquina virtual simula todos os comportamentos de uma máquina real independente do hardware
utilizado, como gereciamento de memória, processador e outros recursos.

<img src="/img/jvm.png" alt="Coesão" width="400" />

Se observamos em linguagens mais antigas como C, quando compilamos um código para um determinado
sistema operacional, esse código poderá ser executado somente nesse tipo de sistema operacional.
Caso precisamos executar esse código em outro sistema operacional, precisaremos adaptar as
bibliotecas e recompilar o código novamente, pois o código assembler gerado é diferente para os
dois sistemas operacionais.

<img src="/img/funcionamento_c.png" alt="Coesão" width="400" />

Na imagem acima o "linker" é responsável por linkar as bibliotecas externas no código compilado
para gerar o código executável. Normalmente são as bibliotecas que alteramos para o funcionamento 
do código em outro sistema operacional.

Agora no Java não temos esse problema, independente do sistema operacional, podemos rodar o código,
pois o responsável pela comunicação com o sistema operacional é a JVM, assim o desenvolvedor fica
livre para desenvolver sem pensar em qual sistema operacional o código será executado.
O JVM não é um interpretador de código, lembrando novamente, ela é responsável pelo gerenciamento de
memória, threads e muito mais funções. Além disso temos uma função especifica que é o 
Garbage Collector, responsável por remover todo o "lixo" (classes, threads, 
entre outros que estão alocando memoria e não estão sendo mais utilizados) gerado pelo seu sistema.

<img src="/img/funcionamento_jvm.png" alt="Coesão" width="400" />