DELIMITER // 
CREATE PROCEDURE AumentarSalario(
  IN dep_id INT,
  IN percentual DECIMAL(5,2)
)
BEGIN
  UPDATE cargos
  SET salario = salario + (salario *(percentual / 100))
  WHERE departamento_id = dep_id;
END //
DELIMITER ;
------------------------------------------------------------
1. CERTO
DELIMITER //

CREATE PROCEDURE InserirEmpregado (
    IN p_nome VARCHAR(100),
    IN p_cargo VARCHAR(50),
    IN p_salario DECIMAL(10, 2),
    IN p_data_admissao DATE
)
BEGIN
    INSERT INTO empregados (nome, cargo, salario, data_admissao)
    VALUES (p_nome, p_cargo, p_salario, p_data_admissao);
END //

DELIMITER ;


CALL InserirEmpregado('João Silva', 'Desenvolvedor', 5000.00, '2024-08-01');
--------------------------------------------------------------
2. ERRO
DELIMITER //

CREATE PROCEDURE ExcluirEmpregado (
    IN p_id INT
)
BEGIN
    -- Verifica se o empregado existe antes de tentar excluir
    IF EXISTS (SELECT 1 FROM empregados WHERE id = p_id) THEN
        DELETE FROM empregados WHERE id = p_id;
    ELSE
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Empregado com o ID fornecido não encontrado.';
    END IF;
END //

DELIMITER ;

CALL ExcluirEmpregado(123);
---------------------------------------------------------------
3. DEU ERRO NO CALL
DELIMITER //

CREATE PROCEDURE AtualizarCargoEmpregado (
    IN p_id INT,
    IN p_novo_cargo VARCHAR(50)
)
BEGIN
    -- Verifica se o empregado existe antes de tentar atualizar
    IF EXISTS (SELECT 1 FROM empregados WHERE id = p_id) THEN
        UPDATE empregados
        SET cargo = p_novo_cargo
        WHERE id = p_id;
    ELSE
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Empregado com o ID fornecido não encontrado.';
    END IF;
END //

DELIMITER ;

CALL AtualizarCargoEmpregado(123, 'Gerente de Projeto');
