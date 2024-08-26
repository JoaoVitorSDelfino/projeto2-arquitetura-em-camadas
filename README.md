# Projeto 2 | Estudo de Caso de Arquitetura em Camadas: SQLite
## Introdução
Esse projeto é um estudo de caso, contendo a documentação e mapeamento de requisitos e processos referentes à arquitetura em camadas. Esta pesquisa terá como base um sistema público do SQLite, um banco de dados open source.
O sistema do SQLite foi escolhido devido a sua vasta documentação e o fato de ser um sistema um tanto quanto antigo, contendo diversos pontos a ser melhorados.

## 1. Arquitetura Geral do SQLite
  O SQLite adota um padrão arquitetural de "arquitetura embutida" (embedded architecture) ou "monolítica" (monolithic architecture).
  Primeiramente, vale-se destacar que uma arquitetura embutida e monolítica proporciona uma solução de banco de dados leve, rápida e de fácil integração, ideal para aplicações onde a simplicidade e a portabilidade são prioritárias. Ao contrário de sistemas de banco de dados que seguem uma arquitetura cliente-servidor ou distribuída, o SQLite é projetado para ser uma solução única e compacta que é diretamente embutida na aplicação.
  Com isso em mente, segue com uma descrição um pouco mais detalhada sobre os padrões utilizados e suas vantagens no sistema em questão:
### 1.1. Arquitetura Embutida
  Características:
    - Integrado na Aplicação: O SQLite é integrado diretamente no código da aplicação como uma biblioteca. Não requer a instalação ou execução de um servidor de banco de dados separado.
    - Compilação Estática: O SQLite é compilado diretamente em aplicações, o que significa que todas as funcionalidades do banco de dados são acessíveis diretamente através das interfaces da biblioteca.
    - Portabilidade: Por ser embutido, o SQLite pode ser facilmente portado para diferentes sistemas operacionais e plataformas, mantendo a mesma funcionalidade sem necessidade de ajustes adicionais.
  Vantagens:
    - Simplicidade: Facilita a configuração e o deployment, pois não há necessidade de gerenciar servidores ou configurações complexas.
    - Desempenho: Reduz a sobrecarga associada à comunicação cliente-servidor, pois a aplicação interage diretamente com a biblioteca do banco de dados.
### 1.2. Arquitetura Monolítica
  Características:
    - Componente Único: O SQLite é implementado como uma única biblioteca monolítica. Toda a lógica de gerenciamento de banco de dados, incluindo a análise SQL, execução, e armazenamento, está contida em um único conjunto de código.
  - Desacoplamento Mínimo: Não há separação clara entre as camadas de controle, processamento e armazenamento. Em vez disso, todas essas responsabilidades são gerenciadas dentro da mesma biblioteca.
  Vantagens:
  - Eficiência: A integração monolítica permite otimizações que seriam mais difíceis de implementar em uma arquitetura de múltiplos componentes.
  - Simplicidade de Design: Mantém uma base de código mais simples e coesa, o que pode facilitar a manutenção e o desenvolvimento.

## 2. Arquitetura em Camadas
  Tendo conhecimento sobre a arquitura monolítica do SQLite, vale-se agora explicar em maior detalhe suas camadas específicas:
### 2.1. Interface de Aplicação (API)
- Função Principal: A API do SQLite é o ponto de entrada onde as aplicações interagem com o banco de dados. Fornece uma série de funções em C que permitem a execução de comandos SQL e o gerenciamento de conexões com o banco de dados.
- Principais Processos:
  - Abertura e fechamento de conexões com o banco de dados.
  - Preparação, execução e finalização de instruções SQL.
  - Manipulação de resultados de consultas.
### 2.2. Módulo SQL
- Função Principal: Esta camada processa as instruções SQL enviadas pela aplicação. Ela interpreta o SQL, otimiza as consultas e gera o plano de execução.
- Principais Processos:
  - Parser SQL: Analisa as instruções SQL e converte-as em uma árvore de análise sintática.
  - Otimização de Consultas: Melhora o desempenho da consulta reorganizando as operações de maneira eficiente.
  - Gerador de Código: Converte a árvore de análise sintática em bytecode executável pela máquina virtual do SQLite.
### 2.3. Máquina Virtual (VM)
- Função Principal: A VM do SQLite executa o bytecode gerado pela camada SQL. É responsável por realizar operações lógicas e matemáticas baseadas nas instruções SQL.
- Principais Processos:
  - Execução de loops, condições e operações aritméticas.
  - Manipulação de dados em tabelas e índices.
### 2.4. Sistema de Gerenciamento de Armazenamento (B-tree)
- Função Principal: Este componente gerencia a estrutura de armazenamento do SQLite, que utiliza árvores B (B-trees) para organizar dados e índices de maneira eficiente.
- Principais Processos:
  - Gerenciamento de páginas de disco.
  - Inserção, atualização, deleção e leitura de registros.
  - Manutenção de índices para acelerar buscas.
### 2.5. Sistema de Arquivos
- Função Principal: O sistema de arquivos é responsável por ler e escrever dados no disco. No SQLite, cada banco de dados é armazenado como um único arquivo.
- Principais Processos:
  - Manipulação de arquivos para leitura e escrita.
  - Gerenciamento de bloqueios de arquivos para garantir a integridade transacional.
### 2.6. Camada de Transações e Controle de Concorrência
- Função Principal: Garante a integridade e a consistência dos dados, mesmo em situações de falhas. O SQLite utiliza o controle de concorrência através de um sistema de bloqueio de nível de arquivo.
- Principais Processos:
  - Controle de transações (commit e rollback).
  - Gerenciamento de bloqueios para garantir isolamento.
  - Implementação do protocolo ACID (Atomicidade, Consistência, Isolamento, Durabilidade).

## 3. Funcionamento Interno
O SQLite segue um fluxo simplificado para o processamento de operações de banco de dados:

- Recepção de SQL: A API recebe uma instrução SQL e passa para a camada SQL.
- Análise e Otimização: A instrução é analisada e otimizada na camada SQL.
- Execução: O bytecode gerado é executado pela Máquina Virtual.
- Acesso a Dados: Se a execução envolver dados, a camada B-tree acessa os registros correspondentes através do sistema de arquivos.
- Resultado: O resultado da execução é retornado à API, que o entrega à aplicação.

## 4. Proposta de melhoria
  SQLite é um sistema um tanto quanto antigo, com diversas partes do código entiquetadas como 'legado', e apesar de sua tamanha compatibilidade até mesmo em sistemas atuais, ainda tem pontos de possíveis melhorias.

### 4.1. Visualização de tabelas e interface
- Sendo um banco de dados que é rodado juntamente a aplicação, ele não tem uma forma explícita de visualizar de forma detalhada as tabelas e seus conteúdos de forma intuitiva/facilitada como outros bancos de dados que rodam com servidores ou até mesmo em cloud (como o Firebase)
- A única maneira de manusear uma tabela de forma intuitiva é por meio do [SQLite Browser](https://sqlitebrowser.org/), uma boa ferramenta para testes mas sendo necessário download externo ao SQLite e possui uma interface um tanto quanto desatualizada e antiga.
#### Solução
- Modernizar a interface do [SQLite Browser](https://sqlitebrowser.org/) para se tornar mais amigável a novos usuários, com uma possível implementação de representação visual de tabelas e diagramas igual vemos em outros SQLs como [MySQL](https://www.mysql.com)

### 4.2. Escalabilidade para sistemas de maior porte
- A ideia do SQLite é criar um banco de dados "de bolso" assim por dizer, o que significa que não foi muito bem pensado para aplicação de grande porte.
- É citado na documentação oficial do SQLite ["The Amalgamation"](https://www.sqlite.org/amalgamation.html/) (traduzido do inglês, "A Amálgama"), que é uma possível solução para sistemas maiores, que consiste em uma técnica de agrupar todo o código fonte do SQLite em um só arquivo de 200 mil linhas de código, obtendo então uma melhoria de até 5% em velocidade.
#### Solução
- Mesmo não sendo o objetivo do SQLite armazenar databases muito grandes, ainda seria interessante possuir uma alternativa melhor do que "The Amalgamation", tendo em vista a sua baixíssima legibilidade e melhoria descartável (apesar de 5% escalar melhor quanto mais dados, ainda é uma melhoria pequena a preço de sacrificar a arquitetura inteira do código).
- Tendo isso em mente, a solução seria criar uma versão para sistemas de médio até baixo-grande porte mantendo ainda a arquitetura projetada originalmente pelo SQLite.

### 4.3. Atualizar a documentação
- Atualmente, a documentação do SQLite, é um pouco desatualizada em vista aos novos avanços em diversas áreas da programação, uma vez que não contém informações sobre possíveis integrações com diversos outros serviços como React.js, Flask, Djamgo (Sendo esses dois últimos escritos em Python, que vem ganhado muita popularidade nos últimos anos)
#### Solução
- Atualizar a documentação de acordo, contendo um possível guia para integração com diversas outras tecnologias como é feito em documentações mais recentes como [Google Cloud](https://cloud.google.com/docs?hl=pt-br) e [AWS](https://cloud.google.com/docs?hl=pt-br)
