# JVM Argumentos (Em edição)

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

A jvm não compreende java e sim um código chamado de ByteCode, que é gerado pelo seu compilador
javac. O bytecode é code que será traduzido para a máquina que a aplicação está hospedada. Quando
surgiu a jvm a mesma interpretava apenas um Bytecode por vez. Atualmente a jvm possui um sistema de
compilação chamado JIT (Just in Time), assim ele cria os "hot spots", que são códigos executados
com maior frequencia para serem otimizados para aumento de performance. Isso ocorre a medida
que a aplicação é executada e o código reinterpretado novamente.
E o mais legal é que a JVM é uma especificação de software e qualquer pessoa pode implementa-la, 
dentre as implementações mais importantes disponiveis no mercado possuimos:
* Azul Zulu - Suportada atualmente pela Azul Systems;
* Codename One - usa o ParparVM para sua implementação;
* Eclipse OpenJ9 — da IBM, para Windows, AIX, Linux (x86, Power, e Z), macOS, MVS, OS/400, 
  Pocket PC, z/OS.
* GraalVM — é baseado no HotSpot/OpenJDK, mas tem a adição de ser poliglota.
* HotSpot — a implementação open-source da Oracle.
* JamVM — desenvolvida para ser uma virtual machine mínima e desenhara para usar o GNU Classpath. 
  Suporta diversas arquiteturas. GPL.
* Jikes RVM (Jikes Research Virtual Machine) — projeto de pesquisa. PPC e IA-32. Suporta Apache Harmony
  e GNU Classpath libraries. Eclipse Public License.
* leJOS — para Robótica, serve para substituir o Lego Mindstorms provendo um ambiente de 
  programação para o Lego Mindstorms RCX e NXT robots. 
* Maxine — meta-circular open source research VM da Oracle Labs e a Universidade de Manchester.  

##JDK x JRE (Melhorar tópico)

JDK - Java Development Kit - utilizado pelo desenvolvedor para desenvolvimento de aplicações,
possuindo a JVM dentro dele e algumas bibliotecas para auxiliar o desenvolvedor.
JRE - Java Runtime Enviroment - JVM para instalação nas máquina apenas com intuito de executar
a aplicação java.

## Arquitetura JVM

Para entendermos o funcionamento interno da JVM iremos repassar pelos pontos apresentados na imagem :

<img src="/img/arquitetura_jvm.png" alt="Coesão" width="400" />

### Classloader

O Classloader é conhecido como um subsistema da JVM que é utilizado para carregamento de arquivos das classes.
Quando executamos um programa em java, o mesmo é o primeiro a ser carregado.

* Bootstrap carregador de classes : é o primeiro a carregar os arquivos das classes, onde ele é carregador das classes
Extension. Responsável por carregar o jar, que possui todos os arquivos de classe Java Standard Edition, que são as 
  classes localizadas nos pacotes java.lang, java.net, java.util, java.io, java.sql, entre outras.
* Extension carregador de classes: responsável por carregar as classes filhos do Bootstrap e carregar as classes pai do 
System classloader. Normalmente os arquivos que ele carrega podem ser localizados no caminho : $JAVA_HOME/jre/lib/ext
  (JAVA_HOME é o caminho de instalação de sua jvm).
* Sistema/ Aplicativo carregador de classe : responsável por carregar classes filhos do Extension e arquivos
de classe do classpath. É possível alterar o caminho da classe usando a opção "-cp" ou "-classpath". Além disso ele é
  conhecido como carregador de classes de aplicativos.

  ```
  //Vamos ver um exemplo para imprimir o nome do carregador de classe
  public class ClassLoaderExample{
    public static void main(String[] args){
      // Vamos imprimir o nome do carregador de classes da classe atual.
      // O carregador de classe Aplicativo / Sistema carregará esta classe
      Class c=ClassLoaderExample.class;
      System.out.println(c.getClassLoader());
      //Se imprimirmos o nome do carregador de classes de String, ele imprimirá nulo porque é uma  
      // classe incorporada que é encontrada em rt.jar, portanto é carregada pelo carregador de classe Bootstrap  
      System.out.println(String.class.getClassLoader());
    }    
  }
  ```     
  Saída:
  ```
  sun.misc.Launcher$AppClassLoader@4e0e2f2a
  null
  ```  
  Na saida acima vemos o carregador utilizado para o pequeno exemplo utilizado acima, caso deseje criar seu
próprio carregador, se faz necessário estender ClassLoader.
  
### Área de classe (Método)
Classe (método) A área armazena estruturas por classe, como o pool constante de tempo de execução, dados de campo e método, o código para métodos.

### Heap
É a área de dados de tempo de execução na qual os objetos são alocados.

### Pilha
Java Stack armazena quadros. Ele contém variáveis locais e resultados parciais e desempenha um papel na invocação e no retorno do método.
Cada encadeamento possui uma pilha JVM privada, criada ao mesmo tempo que encadeamento.
Um novo quadro é criado toda vez que um método é chamado. Um quadro é destruído quando sua chamada de método é concluída.

###Registro do contador de programa
O registro do PC (contador de programa) contém o endereço da instrução da máquina virtual Java atualmente em execução.

###Pilha de método nativo
Ele contém todos os métodos nativos usados no aplicativo.

###Interface nativa Java
Java Interface Nativa (JNI) é uma estrutura que fornece uma interface para se comunicar com outro aplicativo escrito em outra linguagem como C, C ++, Assembly etc. Java usa a estrutura JNI para enviar saída ao console ou interagir com as bibliotecas do sistema operacional.


##Compilador JIT
