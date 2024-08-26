# Projeto 2 | Estudo de Caso de Arquitetura em Camadas: SQLite
## Introdução
Esse projeto é um estudo de caso, contendo a documentação e mapeamento de requisitos e processos referentes à arquitetura em camadas. Esta pesquisa terá como base um sistema público do SQLite, um banco de dados open source.
O sistema do SQLite foi escolhido devido a sua vasta documentação e o fato de ser um sistema um tanto quanto antigo, contendo diversos pontos a ser melhorados.
## 1. Arquitetura Geral do SQLite
  A arquitetura do SQLite é baseada em uma estrutura monolítica, composta por várias camadas que trabalham juntas para fornecer funcionalidade completa de banco de dados. Abaixo estão as principais camadas e seus recursos:

### 1.1. Interface de Aplicação (API)
- Função Principal: A API do SQLite é o ponto de entrada onde as aplicações interagem com o banco de dados. Fornece uma série de funções em C que permitem a execução de comandos SQL e o gerenciamento de conexões com o banco de dados.
- Principais Processos:
  - Abertura e fechamento de conexões com o banco de dados.
  - Preparação, execução e finalização de instruções SQL.
  - Manipulação de resultados de consultas.
### 1.2. Módulo SQL
- Função Principal: Esta camada processa as instruções SQL enviadas pela aplicação. Ela interpreta o SQL, otimiza as consultas e gera o plano de execução.
- Principais Processos:
  - Parser SQL: Analisa as instruções SQL e converte-as em uma árvore de análise sintática.
  - Otimização de Consultas: Melhora o desempenho da consulta reorganizando as operações de maneira eficiente.
  - Gerador de Código: Converte a árvore de análise sintática em bytecode executável pela máquina virtual do SQLite.
### 1.3. Máquina Virtual (VM)
- Função Principal: A VM do SQLite executa o bytecode gerado pela camada SQL. É responsável por realizar operações lógicas e matemáticas baseadas nas instruções SQL.
- Principais Processos:
  - Execução de loops, condições e operações aritméticas.
  - Manipulação de dados em tabelas e índices.
### 1.4. Sistema de Gerenciamento de Armazenamento (B-tree)
- Função Principal: Este componente gerencia a estrutura de armazenamento do SQLite, que utiliza árvores B (B-trees) para organizar dados e índices de maneira eficiente.
- Principais Processos:
  - Gerenciamento de páginas de disco.
  - Inserção, atualização, deleção e leitura de registros.
  - Manutenção de índices para acelerar buscas.
### 1.5. Sistema de Arquivos
- Função Principal: O sistema de arquivos é responsável por ler e escrever dados no disco. No SQLite, cada banco de dados é armazenado como um único arquivo.
- Principais Processos:
  - Manipulação de arquivos para leitura e escrita.
  - Gerenciamento de bloqueios de arquivos para garantir a integridade transacional.
### 1.6. Camada de Transações e Controle de Concorrência
- Função Principal: Garante a integridade e a consistência dos dados, mesmo em situações de falhas. O SQLite utiliza o controle de concorrência através de um sistema de bloqueio de nível de arquivo.
- Principais Processos:
  - Controle de transações (commit e rollback).
  - Gerenciamento de bloqueios para garantir isolamento.
  - Implementação do protocolo ACID (Atomicidade, Consistência, Isolamento, Durabilidade).
