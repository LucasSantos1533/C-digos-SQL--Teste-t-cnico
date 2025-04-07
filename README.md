# Codigos-SQL--Teste-tecnico
Nesse repositorio esta armazenado teste técnico na linguagem SQL
1- Questão 3 tabelas
a)
SELECT 
  S.nome AS escola,
  st.enrolled_at AS data_matricula,
  COUNT(st.id) AS quantidade_alunos,
  SUM(c.price) AS valor_total_matriculas
  FROM estudantes st
  JOIN cursos c ON st.cursos_id = c.id
  JOIN escola s ON c.escola_id =s.id
  WHERE	c.nome ILIKE 'Data% ' 
  GROUP BY s.nome, st.enrolled_at
  ORDER BY st.enrolled_at DESC; 

  b)
 SELECT 
    escola,
	enrolled_at,
	quantidade_alunos,
	SUM(quantidade_alunos) OVER (
  PARTITION BY escola
  ORDER BY enrolled_at
  ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
	) AS soma_acumulada,
 	AVG(quantidade_alunos) OVER (
   PARTITION BY escola
    ORDER BY enrolled_at
	ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
	) AS media_movel7_dias,
	 AVG(quantidade_alunos) OVER (
        PARTITION BY escola 
        ORDER BY enrolled_at
        ROWS BETWEEN 29 PRECEDING AND CURRENT ROW
    ) AS media_movel_30_dias
   FROM matriculas
  ORDER BY escola, enrolled_at;

  2) SELECT 
    d.nome AS departamento,  -- nome departamento   
    COUNT(e.matr) AS quantidade_empregados,  -- conta quantos empregados tem em cada departamento
    ROUND(AVG(COALESCE(v.valor, 0)), 2) AS media_salarial, -- calcula a média salarial dentro de cada departamento
    COALESCE(MAX(v.valor), 0) AS maior_salario,  -- pega o mairo salário dentro do departamento
    COALESCE(MIN(v.valor), 0) AS menor_salario   -- pega o menor salário dentro do departamento
FROM departamento d  -- pega a tabela departamento
LEFT JOIN empregado e ON d.cod_dep = e.lotacao  --  junta a tabela departamento(d) com empregado(e) pelo campo cod_dep
LEFT JOIN emp_venc ev ON e.matr = ev.matr  -- junta os empregados com os vencimentos deles
LEFT JOIN vencimento v ON ev.cod_venc = v.cod_venc  -- junta os vencimentos para pegar os valores do salario
GROUP BY d.nome  -- Agrupa os resultados por departamento para calcular a estatisticas de cada um.
ORDER BY media_salarial DESC; --Organiza os departamentos do maior para o menor salário médio
