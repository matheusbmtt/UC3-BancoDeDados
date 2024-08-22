CREATE DATABASE IF NOT EXISTS lojaLeo;

USE lojaLeo;

CREATE TABLE IF NOT EXISTS clientes (
    id_cliente INT PRIMARY KEY,
    nome VARCHAR(100),
    email VARCHAR(100)
);

CREATE TABLE IF NOT EXISTS pedidos (
    id_pedido INT PRIMARY KEY,
    descricao VARCHAR(200),
    valor DECIMAL(10, 2),
    id_cliente INT,
    FOREIGN KEY (id_cliente) REFERENCES clientes(id_cliente)
);


-- Inserindo valores na tabela clientes com INSERT IGNORE INTO
INSERT IGNORE INTO clientes (id_cliente, nome, email) VALUES
(1, 'João Silva', 'joao@example.com'),
(2, 'Maria Oliveira', 'maria@example.com'),
(3, 'Pedro Santos', 'pedro@example.com');

-- Inserindo valores na tabela pedidos com INSERT IGNORE INTO
INSERT IGNORE INTO pedidos (id_pedido, descricao, valor, id_cliente) VALUES
(101, 'Compra de móveis', 1500.00, 1),
(102, 'Pedido de eletrônicos', 2500.50, 2),
(103, 'Serviços de instalação', 500.75, 3);

CREATE TABLE IF NOT EXISTS products (
        id_product INT PRIMARY KEY
        product_name VARCHAR(100),
        unit_price DECIMAL(10, 2)
);

itens do pedidos
id do itens do pedido primary KEY
id do pedido chave estrangeira
id do produto chave estrangeira
quantidade
preço unitario

USE GustavoStore;

CREATE TABLE IF NOT EXISTS products (
        id_product INT PRIMARY KEY,
        product_name VARCHAR(100),
        unit_price DECIMAL(10, 2)
);

CREATE TABLE IF NOT EXISTS order_itens (
		id_order_itens INT PRIMARY KEY,
    	amount INT,
    	id_request INT,
    	id_product INT,
    	unit_price DECIMAL (10,2),
    	FOREIGN KEY (id_request) REFERENCES request (id_request),
    	FOREIGN KEY (id_request) REFERENCES request (id_request)
);

USE GustavoStore;

CREATE TABLE IF NOT EXISTS payment (
		id_payment INT PRIMARY KEY,
       	id_request INT,
    	payment_method VARCHAR (100),
    	amount DECIMAL (10,2),
    	payday DATE,
    	FOREIGN KEY (id_request) REFERENCES request (id_request)
);

USE GustavoStore;

INSERT IGNORE INTO order_itens (amount, id_order_itens, id_product, id_request, unit_price) VALUES
(5, 1002, 1002, 1002, 1600.00),
(6, 1003, 1003, 1003, 1600.00),
(7, 1004, 1004, 1004, 1600.00);

INSERT IGNORE INTO products (id_product, product_name, unit_price) VALUES
(1005, 'Estante', 1500.00),
(1006, 'Geladeira', 2000.00),
(1007, 'Porta', 500.00);

INSERT IGNORE INTO payment (amount, id_payment, id_request, payday, payment_method) VALUES
(2, 456, 122, 2024-03-20, 'Pix'),
(1, 457, 123, 2024-06-22, 'Cartão Débito'),
(3, 457, 121, 2024-08-12, 'Cartão Crédito');

RENAME TABLE clients TO store_clients  ----renomear o nome da tabela

ALTER TABLE payment (nome da tabela)
CHANGE payday pay_day DATE; (nome da coluna, nome novo da coluna, tipo de dado da coluna) ----renomear o nome da coluna

ALTER TABLE store_clients   ----adicionar uma coluna 
ADD COLUMN cellphone VARCHAR(15);

---inserir valores nas novas colunas criadas
UPDATE nome da coluna
SET nome da coluna = valor
WHERE condição;
---------------------------
UPDATE store_clients
SET cellphone = 51983322820
WHERE id_clients = 4

----------------------------------------------------

CREATE TABLE departamentos (
    departamento_id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(50) NOT NULL
);


CREATE TABLE empregados (
    empregados_id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(50) NOT NULL,
    salario DECIMAL(10, 2) NOT NULL,
    departamento_id INT,
    FOREIGN KEY (departamento_id) REFERENCES departamentos(departamento_id)
);



INSERT INTO departamentos (nome) VALUES
('Vendas'),
('Marketing'),
('TI'),
('Recursos Humanos');

INSERT INTO empregados (nome, salario, departamento_id) VALUES
('Maicou Diécson', 5000.00, 1),
('Vandercleison', 6000.00, 1),
('Kerolaine', 4000.00, 2),
('Wanderneidson', 5500.00, 3),
('Kellen', 7000.00, 3),
('Chico', 3000.00, 4),
('Greice Kelly', 4500.00, 2),
('Xonas', 3500.00, 1);

-----------------------------------------------------------------------

SUBCONSULTA


Uma subconsulta é uma consulta interna colocada dentro de uma consulta externa. A subconsulta pode retornar um único valor, um conjunto de valores, ou uma tabela de resultados, dependendo de como é usada.

Tipos de Subconsultas


1. Subconsultas Escalares
Retornam um único valor.

Consulta para Encontrar Empregados com Salário Acima da Média


SELECT nome, salario
FROM empregados
WHERE salario > (SELECT AVG(salario) FROM empregados);


Neste exemplo, a subconsulta (SELECT AVG(salario) FROM empregados) calcula o salário médio, e a consulta externa seleciona empregados com salários acima da média.


ALTER TABLE departamentos ADD localizacao VARCHAR(50);

UPDATE departamentos SET localizacao = 'São Paulo' WHERE nome = 'Vendas';
UPDATE departamentos SET localizacao = 'Rio de Janeiro' WHERE nome = 'Marketing';
UPDATE departamentos SET localizacao = 'São Paulo' WHERE nome = 'TI';
UPDATE departamentos SET localizacao = 'Belo Horizonte' WHERE nome = 'Recursos Humanos';



2. Subconsultas de Coluna Única
Retornam um conjunto de valores de uma única coluna e são usadas com operadores como IN, ANY, ALL.


Exemplo com IN

SELECT nome
FROM empregados
WHERE departamento_id IN (SELECT departamento_id FROM departamentos WHERE localizacao = 'São Paulo');

Aqui, a subconsulta retorna todos os departamento_id de departamentos localizados em São Paulo, e a consulta externa retorna os nomes dos empregados que pertencem a esses departamentos.

--------------------------------------------------------------------------------------------

Exemplo com ANY

Exemplo: Salário Maior que Qualquer Salário no Departamento de TI

SELECT nome, salario
FROM empregados
WHERE salario > ANY (SELECT salario FROM empregados WHERE departamento_id = (SELECT departamento_id FROM departamentos WHERE nome = 'TI'));

O operador ANY retorna verdadeiro se qualquer valor na subconsulta satisfizer a condição. Vamos supor que queremos encontrar empregados cujo salário é maior do que qualquer salário no departamento de TI.


A subconsulta (SELECT id FROM departamentos WHERE nome = 'TI') encontra o id do departamento de TI.

A subconsulta aninhada (SELECT salario FROM empregados WHERE departamento_id = (SELECT id FROM departamentos WHERE nome = 'TI')) retorna todos os salários dos empregados no departamento de TI.

O operador ANY compara o salário de cada empregado com qualquer um dos salários retornados pela subconsulta.

------------------------------------------------------------------------------------------

Exemplo com ALL
O operador ALL retorna verdadeiro se todos os valores na subconsulta satisfizerem a condição. Vamos supor que queremos encontrar empregados cujo salário é maior do que todos os salários no departamento de TI.

Exemplo: Salário Maior que Todos os Salários no Departamento de TI


SELECT nome, salario
FROM empregados
WHERE salario > ALL (SELECT salario FROM empregados WHERE departamento_id = (SELECT id FROM departamentos WHERE nome = 'TI'));


A subconsulta (SELECT id FROM departamentos WHERE nome = 'TI') encontra o id do departamento de TI.

A subconsulta aninhada (SELECT salario FROM empregados WHERE departamento_id = (SELECT id FROM departamentos WHERE nome = 'TI')) retorna todos os salários dos empregados no departamento de TI.

O operador ALL compara o salário de cada empregado com todos os salários retornados pela subconsulta, retornando apenas aqueles cujo salário é maior que todos os salários no departamento de TI.

--------------------------------------------------------------------------------------------------------

ALTER TABLE empregados ADD titulo VARCHAR(50);

CREATE TABLE cargos (
    cargos_id INT AUTO_INCREMENT PRIMARY KEY,
    titulo VARCHAR(50) NOT NULL,
    departamento_id INT,
    salario DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (departamento_id) REFERENCES departamentos(departamento_id)
);

ALTER TABLE empregados DROP salario;


UPDATE empregados SET titulo = 'Gerente' WHERE nome = 'Maicou Diécson';
UPDATE empregados SET titulo = 'Assistente' WHERE nome = 'Vandercleison';
UPDATE empregados SET titulo = 'Analista' WHERE nome = 'Kerolaine';
UPDATE empregados SET titulo = 'Desenvolvedor' WHERE nome = 'Wanderneidson';
UPDATE empregados SET titulo = 'Engenheiro' WHERE nome = 'Kellen';
UPDATE empregados SET titulo = 'Assistente' WHERE nome = 'Chico';
UPDATE empregados SET titulo = 'Analista' WHERE nome = 'Greice Kelly';
UPDATE empregados SET titulo = 'Estagiário' WHERE nome = 'Xonas';


INSERT INTO cargos (titulo, departamento_id, salario) VALUES
('Gerente', 1, 8000.00),
('Assistente', 1, 4000.00),
('Analista', 2, 4500.00),
('Desenvolvedor', 3, 6000.00),
('Engenheiro', 3, 7000.00),
('Assistente', 4, 3500.00),
('Analista', 2, 4600.00),
('Estagiário', 1, 2000.00);

-------------------------------------------------------------------------------------------

3. Subconsultas de Múltiplas Colunas
Retornam múltiplas colunas e podem ser usadas para comparar conjuntos de colunas.

SELECT nome
FROM empregados
WHERE (departamento_id, titulo) IN (SELECT departamento_id, titulo FROM cargos WHERE salario > 5000);


Esta subconsulta retorna combinações de departamento_id e titulo de cargos com salário acima de 5000, e a consulta externa seleciona os nomes dos empregados que correspondem a essas combinações.


Cláusula HAVING
A cláusula HAVING é usada para filtrar os resultados de uma consulta de agrupamento (GROUP BY) com base em uma condição especificada. É similar à cláusula WHERE, mas a diferença principal é que HAVING opera após os dados serem agrupados, enquanto WHERE filtra as linhas antes do agrupamento.

SELECT coluna1, função_agregada(coluna2)
FROM tabela
GROUP BY coluna1
HAVING condição;


SELECT departamentos.nome, AVG(cargos.salario)
FROM empregados
JOIN cargos ON empregados.departamento_id = cargos.departamento_id
JOIN departamentos ON empregados.departamento_id = departamentos.departamento_id
GROUP BY departamentos.nome
HAVING AVG(cargos.salario) > 5000.00;

Explicação da Consulta
Selecionar Colunas:

departamentos.nome: O nome do departamento.
AVG(cargos.salario): A média salarial dos cargos nesse departamento.


Junções:
JOIN departamentos ON empregados.departamento_id = departamentos.id: Junção entre empregados e departamentos para obter o nome do departamento.

Agrupamento:
GROUP BY departamentos.nome: Agrupa os resultados pelo nome do departamento.
Condição de Agrupamento:

HAVING AVG(cargos.salario) > 5000.00: Filtra os grupos para incluir apenas aqueles onde a média salarial é superior a R$ 5000,00.

---------------------------------------------------------------------
1)
SELECT departamento_id, 
SUM(salario) AS soma_salarios
FROM cargos
GROUP BY departamento_id
HAVING SUM(salario) > 12000.00;

2)
SELECT titulo, 
COUNT(empregados_id) AS numero_empregados
FROM empregados
GROUP BY titulo
HAVING COUNT(empregados_id) > 2;

3)
SELECT departamento_id, 
AVG(salario) AS media_salarial
FROM cargos
GROUP BY departamento_id
HAVING AVG(salario) > 4500.00;

4)
SELECT departamento_id, 
MAX(salario) AS maior_salario
FROM cargos
GROUP BY departamento_id
HAVING MAX(salario) > 6000.00;

5)
SELECT departamento_id, 
MIN(salario) AS menor_salario
FROM cargos
GROUP BY departamento_id
HAVING MIN(salario) > 2500.00;

-------------------------------------------

1) DELIMITER //

CREATE PROCEDURE InserirEmpregado (
    IN p_departamento_id INT,
    IN p_nome VARCHAR(255),
    IN p_titulo VARCHAR(255)
)
BEGIN
    INSERT INTO empregados (departamento_id, nome, titulo)
    VALUES (p_departamento_id, p_nome, p_titulo);
END //

DELIMITER ;



2) DELIMITER //

CREATE PROCEDURE ExcluirEmpregado (
    IN p_empregados_id INT
)
BEGIN
    DELETE FROM empregados
    WHERE empregados_id = p_empregados_id;
END //

DELIMITER ;



3) DELIMITER //

CREATE PROCEDURE AtualizarTituloEmpregado (
    IN p_empregados_id INT,
    IN p_novo_titulo VARCHAR(255)
)
BEGIN
    UPDATE empregados
    SET titulo = p_novo_titulo
    WHERE empregados_id = p_empregados_id;
END //

DELIMITER ;

-----------------------------------------

DELIMITER//

CREATE FUNCTION CalcularSalarioBruto(
    emp_id INT
)
RETURNS DECIMAL(10,2)
BEGIN
    DECLARE salario1 DECIMAL(10,2);

    -- Obtém o salário do cargo do empregado com o ID fornecido
    SELECT cargos.salario 
    INTO salario1
    FROM empregados
    JOIN cargos ON empregados.titulo = cargos.titulo AND empregados.departamento_id = cargos.departamento_id
    WHERE empregados.empregados_id = emp_id;

    -- Retorna o salário
    RETURN salario1;
END //

DELIMITER ;

Para consultar:

SELECT nome, CalcularSalarioBruto(empregados_id) AS salario_bruto
FROM empregados
WHERE empregados_id = 1;

-----------------------------------------------

1) DELIMITER //

CREATE FUNCTION CalcularSalarioAnual (
    p_cargos_id INT
)
RETURNS DECIMAL(10, 2) -- Especifica o tipo de dado que será retornado pela função.

BEGIN
    DECLARE salario_anual DECIMAL(10, 2); -- Declara uma variavel local.

    SELECT salario * 12  -- Seleciona o salário multiplicado por 12.
    INTO salario_anual -- Armazena o resultado na variavel.
    FROM cargos -- Informa da onde é obtido os dados
    WHERE cargos_id = p_cargos_id; -- Filtra o cargo baseado no cargos_id

    RETURN salario_anual; -- Retorna o salario anual 
END //

DELIMITER ;

SELECT CalcularSalarioAnual(1);

2) DELIMITER //

CREATE FUNCTION ContarEmpregadosDepartamento (
    p_departamento_id INT
)
RETURNS INT

BEGIN
    DECLARE numero_empregados INT;

    SELECT COUNT(*) -- Conta o número de registros (empregados) na tabela. pode ser usado também Empregados.nome ao inves de verificar todas as tabelas 
    INTO numero_empregados
    FROM empregados
    WHERE departamento_id = p_departamento_id;

    RETURN numero_empregados;
END //

DELIMITER ;

SELECT ContarEmpregadosDepartamento(1);

3) DELIMITER //

CREATE FUNCTION ObterNomeDepartamento (
    p_departamento_id INT
)
RETURNS VARCHAR(255)

BEGIN
    DECLARE nome_departamento VARCHAR(255);

    SELECT nome
    INTO nome_departamento
    FROM departamentos
    WHERE departamento_id = p_departamento_id;

    RETURN nome_departamento;
END //

DELIMITER ;

SELECT ObterNomeDepartamento(1);

4) DELIMITER //

CREATE FUNCTION ObterTituloEmpregado (
    p_empregados_id INT
)
RETURNS VARCHAR(255)

BEGIN
    DECLARE titulo_empregado VARCHAR(255);

    SELECT titulo
    INTO titulo_empregado
    FROM empregados
    WHERE empregados_id = p_empregados_id;

    RETURN titulo_empregado;
END //

DELIMITER ;

SELECT ObterTituloEmpregado(1);

5) DELIMITER //

CREATE FUNCTION CalcularSalarioTotalDepartamento (
    p_departamento_id INT
)
RETURNS DECIMAL(10, 2)

BEGIN
    DECLARE salario_total DECIMAL(10, 2);

    SELECT SUM(salario) -- Calcula a soma dos salários dos cargos.
    INTO salario_total
    FROM cargos
    WHERE departamento_id = p_departamento_id;

    RETURN salario_total;
END //

DELIMITER ;

SELECT CalcularSalarioTotalDepartamento(1);