# DecolaTech-DIO-SpringTaskBoard

# TaskBoard - Gerenciamento de Tarefas

![image](https://github.com/user-attachments/assets/be770640-ba44-46bf-abc1-66fff3516173)
![image](https://github.com/user-attachments/assets/4838c981-1684-4411-8f9e-69eb19e5eaf0)
![image](https://github.com/user-attachments/assets/72b9bb98-996c-457a-aade-2eab773855c9)
![image](https://github.com/user-attachments/assets/bcb8cb12-3173-427b-b8ea-3a747cfa9b21)




## Descrição do Projeto
O TaskBoard é um sistema de gerenciamento de tarefas desenvolvido em Java com Spring Boot e MySQL, criado como parte de um projeto educacional da DIO. O objetivo é permitir que os usuários criem e gerenciem quadros (boards) de tarefas, organizando-as em colunas e cards, de forma semelhante a ferramentas como Jira. O sistema é interativo, rodando no console, e permite criar boards, colunas, cards, mover cards entre colunas, bloquear/desbloquear cards e listar tudo, com os dados persistidos em um banco de dados MySQL.
## Funcionalidades
-Criar, listar e excluir boards.

-Criar colunas dentro de um board (ex.: "Inicial", "Em progresso", "Final").

-Criar cards (tarefas) dentro de colunas, com título, descrição e data de criação.

-Mover cards entre colunas.

-Bloquear/desbloquear cards, informando um motivo.

-Listar colunas e cards de um board.

-Persistência de dados no MySQL.

## Pré-requisitos
Antes de executar o projeto, você precisa ter as seguintes ferramentas instaladas:
-Java: Versão 17 ou superior.

-Maven: Para gerenciar dependências e compilar o projeto.

-MySQL: Banco de dados.

-IDE: Visual Studio Code (usado no desenvolvimento)

## Instalação e Configuração
### Clone o Repositório:

https://github.com/MeloRods/DecolaTech-DIO-SpringTaskBoard

### Configure o MySQL:
Crie um banco de dados chamado taskboard:

CREATE DATABASE taskboard;


Configure as credenciais do MySQL no arquivo src/main/resources/application.properties:
properties

<sub>spring.datasource.url=jdbc:mysql://localhost:3306/taskboard 

<sub>spring.datasource.username=root 

<sub>spring.datasource.password="sua-senha" 

<sub>spring.jpa.hibernate.ddl-auto=update 

<sub>spring.jpa.show-sql=true </sub>



<sub>**Atenção na linha onde contém "sua-senha" é senha do MySQL da sua instalação.

### Compile e Execute o Projeto:
No IDE, execute:

O programa iniciará e exibirá um menu interativo no console.

## Como Usar
### Menu Principal:
![image](https://github.com/user-attachments/assets/749a81a9-4f17-4713-8e04-333f64a71d29)

1 - Criar novo board: Crie um board com nome e responsável.

2 - Selecionar board: Liste os boards e selecione um para gerenciar.

3 - Excluir board: Exclua um board pelo ID.

4 - Sair: Encerre o programa.

### Submenu do Board (após selecionar um board(Opção 2)):

![image](https://github.com/user-attachments/assets/98bd2c43-8817-434d-9e63-c33c418a3645)

1 - Criar coluna: Crie uma nova coluna (ex.: "Inicial").

2 - Criar card: Crie um card em uma coluna, com título e descrição.

3 - Mover card: Mova um card de uma coluna para outra.

4 - Bloquear/Desbloquear card: Bloqueie ou desbloqueie um card, informando um motivo.

![image](https://github.com/user-attachments/assets/01131745-02f5-49bd-b9c2-d8333c81071b)

5 - Listar colunas e cards: Veja todas as colunas e cards do board.

![image](https://github.com/user-attachments/assets/5ccbbc44-aae9-4827-b887-b35b95f3504b)


6 - Voltar ao menu principal: Retorne ao menu principal.



# Desafios Enfrentados Durante o Desenvolvimento
Durante o desenvolvimento do projeto, enfrentei vários desafios técnicos e conceituais, que foram superados com ajustes no código, configurações e banco de dados. Abaixo está uma lista detalhada de cada desafio e como foi resolvido:
### 1. Erro de Sintaxe no Nome da Classe Principal
Desafio: Ao criar a classe principal TaskboardApplication, o nome do arquivo (Board.java) não correspondia ao nome da classe pública (TaskBoardApplication)['B' invés de 'b'], causando o erro: "The public type TaskBoardApplication must be defined in its own file".

Solução: Mudei o nome da classe de TaskBoardApplication para TaskboardApplication.java para corresponder ao nome da classe pública, pesquisando um pouco mais, descobrir que essa prática está errada com a conveção do Java, o correto deveria renomear o arquivo para TaskBoardApplication.java e não modificar o nome da classe dentro do código.

### 2. Erro de Digitação na Coluna responsible no MySQL
Desafio: A coluna responsible foi criada como resposible (sem o 'n') no MySQL, causando um erro de mapeamento no Hibernate, que esperava a coluna responsible com base na classe Board.

Solução:
Exclui a tabela board no MySQL:

"DROP TABLE board;"

Recriei a tabela com o nome correto:

CREATE TABLE board (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    responsible VARCHAR(255) NOT NULL DEFAULT 'Desconhecido'
);

### 3. Erro de Sintaxe ao Listar Colunas Devido a Palavra Reservada
Desafio: Ao tentar criar um card (opção 2 do submenu) e listar colunas e cards (opção 5), o MySQL retornou o erro: "You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'column c1_0' at line 1". Isso ocorreu porque a tabela column (mapeada pela classe Column) usava uma palavra reservada do MySQL.

Solução:
Inicialmente, tentei renomear a tabela para columns usando a anotação @Table(name = "columns") na classe Column.

Posteriormente, e permanecendo alguns erros, decidi renomear a própria classe Column para TaskColumn(task_column -> MySQL), evitando completamente o uso de palavras reservadas. Isso incluiu:
Renomear a classe Column.java para TaskColumn.java.

Renomear o repositório ColumnRepository.java para TaskColumnRepository.java.

O VSCode refaturou os nomes, com isso reduziu a complexidade nas classes e relacionamentos em casos como esse.

Recriei as tabelas no MySQL para refletir as mudanças:

DROP TABLE card;
DROP TABLE column;
DROP TABLE board;

O Hibernate recriou as tabelas automaticamente com os novos nomes (card, task_column, board).

### 4. Logs Excessivos do Hibernate
Por último, durante a execução, logs detalhados do Hibernate (como consultas SQL) apareciam no console a cada ação, tornando o terminal menos limpo. Isso ocorria porque a propriedade spring.jpa.show-sql=true está ativada.

Solução:
Desativei a exibição de consultas SQL alterando spring.jpa.show-sql=false no application.properties.

## Contribuições
Contribuições são bem-vindas! Sinta-se à vontade para abrir issues ou pull requests com melhorias, correções de bugs ou novas funcionalidades.

