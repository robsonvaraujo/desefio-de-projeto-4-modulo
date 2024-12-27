# desefio-de-projeto-4-modulo
Desafio de projeto

Meu banco de dados foi criado com muitas inconformidades, entra elas, a mistura de idiomas. As tabelas em português e os atributos em inglês. Não achava que isso me daria tanta dor de cabeça em um futuro bem próximo (no desafio de projeto). 

Depois de criado um índice, tive problema em dar continuidade as consultas, por isso decide uniformizar o banco de dados. Aí veio a trabalheira, conforma apresentado nas imagens anexadas. 
Depois de terminar, percebi que deveria excluir o índice e recomeçar. 
Resultado:
-- Tentativa de deletar índice
DROP INDEX idex_funfionários_dep
ON funcionarios (DepartmentID);
-- Tendo falhado, consultei os índices da tabela de empregados para ver se encontrava o erro:
SHOW INDEX FROM employee;
-- Depois de corrigido a ortografia, e não ter tido sucesso, segui a mensagem de erro, e tentei desativar a restrição da foreing key constraint.
SET FOREIGN_KEY_CHECKS = 0;
-- Ainda sem sucesso, reativei a restrição da foreing key constraint
SET FOREIGN_KEY_CHECKS = 1;
-- Novamente tentei deletar o índice. Novamente sem sucesso:
ALTER TABLE employee DROP INDEX idex_funfionários_dep;

-- Tentei o seguinte comando, e mais uma vez não deu certo:
SET FOREIGN_KEY_CHECKS = 0;
DROP INDEX idex_funfionários_dep ON tabela;
SET FOREIGN_KEY_CHECKS = 1;

 -- Então, em uma tentativa "desesperada" tentei mudar o nome do indice e depos ver se consinuaria com a funionalidade original.
ALTER TABLE employee RENAME INDEX idex_funfionários_dep TO idx_employee_department;
Enfim, consegui alguma coisa.
Consegui criar a view com o seguinte script (conforme imagem 3)
CREATE INDEX idex_funfionários_dep
ON funcionarios (DepartmentID);

Utilizei o seguinte script para responder a primeira pergunta (conforme imagem 4)
SELECT D.Name, COUNT(E.id_Employee) AS Quantidade_Functionarios
FROM Employees E
JOIN Departments D ON E.id_Department = D.id_Department
GROUP BY D.Name
ORDER BY Quantidade_Functionarios DESC
LIMIT 1;
-- Pergunta 2 - Quais são os departamentos por cidade?
Criei o index com o código abaixo (imagem 5):
CREATE INDEX idx_Departments_Locations ON Departments (Locations);
Fiz a consulta (imagem 6)
SELECT D.Name, D.Locations
FROM Departments D
USE INDEX (idx_Departments_Locations)
ORDER BY D.Locations;

Pergunta 3 – Relação de empregados por departamento 

Criação do index (Figura 7):
CREATE INDEX idx_Departments_id_Department ON departments (id_Department);
Criando a consulta através do script (imagem 8)

SELECT 
  D.Name AS Departamento, 
  E.Fname AS Nome, 
  E.Lname AS Sobrenome, 
  E.Position AS Cargo
FROM 
  Employees E
INNER JOIN 
  Departments D ON E.id_Department = D.id_Department
ORDER BY 
  D.Name;

Na segunda etapa, criei uma procedure com o comando e em seguida executei (figura 9):
-- Criar
CREATE PROCEDURE sp_EmpregadosPorDepartamento (IN id_Department INT)
SELECT * FROM Employees WHERE id_Department = id_Department;
Executar
CALL sp_EmpregadosPorDepartamento(1);
