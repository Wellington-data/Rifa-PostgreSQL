# 🎟️ Sistema de Rifa – PostgreSQL

Projeto de Banco de Dados Relacional para controle de uma rifa, desenvolvido em PostgreSQL,
com foco em portfólio profissional, estudo e avaliação técnica (Júnior / Estágio).

## 📌 Sobre o Projeto

Este projeto implementa um sistema completo de rifa, permitindo:

📋 Cadastro de participantes  
🔢 Controle de números de 1 a 100  
💳 Gerenciamento de pagamentos  
📊 Consultas analíticas  
🏆 Identificação do ganhador  
✅ Todas as regras de negócio são garantidas no banco de dados, assegurando integridade e confiabilidade.  

## 🧠 Regras de Negócio

1.Cada pessoa pode comprar vários números.  
2.Cada número só pode ser vendido uma única vez.  
3.Todo número pertence ao intervalo 1 a 100.  
4.Todo número vendido possui:  
  >Um comprador.

  >Um valor fixo (R$ 10,00).

  >Status de pagamento.  

5.Ao excluir uma pessoa, suas compras também são removidas (ON DELETE CASCADE).  

## 📂 Estrutura do Repositório:

📦 rifa-postgresql  
├── 📁 sql  
│   ├── 01_criacao_banco.sql                   
│   ├── 02_modelagem_tabelas.sql                 
│   ├── 03_carga_dados.sql                
│   ├── 04_consultas.sql                  
│
├── 📁 docs  
│   ├── DER.png                            
│   ├── dicionario_dados.md                
│   └── regras_negocio.md                
│
├── README.md  
└── .gitignore  

## 🛠️ Tecnologias Utilizadas:

1. 🐘 PostgreSQL  
2. 🧠 SQL (DDL e DML)  
3. 🗂️ Modelagem Relacional  
4. 🔗 Chaves Primárias e Estrangeiras  

 
## 🗄️ Scripts SQL  

  ### 📄 sql/01_criacao_banco.sql
  
      CREATE DATABASE rifa;  
  
  ### 📄 sql/02_tabelas.sql

  
    \c rifa;

    CREATE TABLE pessoas (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL
    );

    CREATE TABLE numeros (
    numero INT PRIMARY KEY
    );

    INSERT INTO numeros (numero)
    SELECT generate_series(1,100);

    CREATE TABLE rifa (
    id SERIAL PRIMARY KEY,
    pessoa_id INT NOT NULL,
    numero INT NOT NULL,
    pago BOOLEAN NOT NULL,
    valor NUMERIC(10,2) DEFAULT 10.00,

    CONSTRAINT fk_pessoa
        FOREIGN KEY (pessoa_id)
        REFERENCES pessoas(id)
        ON DELETE CASCADE,

    CONSTRAINT fk_numero
        FOREIGN KEY (numero)
        REFERENCES numeros(numero)
        ON DELETE CASCADE,

    CONSTRAINT numero_unico UNIQUE (numero)
    );
 
 ### 📄 sql/03_inserts.sql

 Inserção de Pessoas:  
 
     INSERT INTO pessoas (nome) VALUES
    ('Andreza'), ('Juliana'), ('Palloma'), ...;
 
 Inserção das Compras:  

     INSERT INTO rifa (pessoa_id, numero, pago)
     VALUES (1,69,FALSE), (1,55,FALSE), ...;

 
 ### 📄 sql/04_consultas.sql  

 Números vendidos:  
 
    SELECT p.nome, r.numero, r.pago, r.valor
    FROM rifa r
    JOIN pessoas p ON p.id = r.pessoa_id
    ORDER BY r.numero;

 Pessoas que não pagaram:  

    SELECT p.nome, r.numero
    FROM rifa r
    JOIN pessoas p ON p.id = r.pessoa_id
    WHERE r.pago = FALSE
    ORDER BY r.numero;

Total arrecadado:  

    SELECT SUM(valor) AS total_arrecadado
    FROM rifa
    WHERE pago = TRUE;

Quantidade de números por pessoa:  

    SELECT p.nome, COUNT(r.numero) AS quantidade
    FROM rifa r
    JOIN pessoas p ON p.id = r.pessoa_id
    GROUP BY p.nome
    ORDER BY quantidade DESC;

Números disponíveis:  

    SELECT n.numero
    FROM numeros n
    LEFT JOIN rifa r ON n.numero = r.numero
    WHERE r.numero IS NULL
    ORDER BY n.numero;

Buscar ganhador (substituir o número sorteado):

    SELECT p.nome, r.numero
    FROM rifa r
    JOIN pessoas p ON p.id = r.pessoa_id
    WHERE r.numero = 69;

 
### 📄 docs/DER.png

Diagrama Entidade-Relacionamento contendo:
- Pessoas 1:N Rifa  
- Números 1:1 Rifa  
<img width="1536" height="1024" alt="Diagrama ER - Sistema de Rifa" src="https://github.com/user-attachments/assets/021d07da-3f55-4b62-ac4d-bb5d1fcf80a3" />



### 📄 docs/dicionario_dados.md

<img width="1536" height="1024" alt="dicionário de dados" src="https://github.com/user-attachments/assets/488a674d-156a-4c2e-9a73-6317d39403fd" />  



## 📝 README.md  

# 🎟️ Sistema de Rifa – PostgreSQL  

Projeto de banco de dados relacional para controle de uma rifa, desenvolvido em PostgreSQL.  

## 📌 Funcionalidades  
- Cadastro de participantes  
- Controle de números (1 a 100)  
- Registro de vendas  
- Controle de pagamento  
- Consultas analíticas  
- Identificação de ganhador  


## 🛠️ Tecnologias  
- PostgreSQL  
- SQL  


## ▶️ Como Executar  

Execute os scripts na ordem:

01_criacao_banco.sql  
02_tabelas.sql  
03_inserts.sql  
04_consultas.sql  


## 📊 Consultas Incluídas

- Total arrecadado?  
- Pessoas inadimplentes?  
- Números disponíveis?  
- Quantidade por participante?  
- Quem é o ganhador?  

## 📈 Possíveis Evoluções

Ideias para tornar o projeto ainda mais robusto:  
📊 Views para relatórios financeiros  
⚙️ Triggers para validações automáticas  
🎲 Função para sorteio automático  
📉 Dashboard no Power BI  

## 👤 Autor

### Wellington dos Santos  
Estudante e entusiasta em Banco de Dados e Análise de Dados<br>
Projeto desenvolvido para fins de aprendizado, prática e portfólio profissional.
