# Modelagem de Dados - CursoÁgil

# CursoÁgil: Modelo Relacional Normalizado

## Visão geral

| Tabela | PK | FKs | Tipo |
|---|---|---|---|
| professores | professor_id | — | Entidade |
| cursos | curso_id | professor_id → professores | Entidade |
| aula | aula_id | curso_id → cursos | Entidade |
| alunos | aluno_id | — | Entidade |
| telefones_alunos | (aluno_id, telefone) | aluno_id → alunos | Atributo multivalorado |
| matriculas | (aluno_id, curso_id) | aluno_id → alunos; curso_id → cursos | Associativa (N:N) |
| aula_concluida | (aluno_id, aula_id) | aluno_id → alunos; aula_id → aula | Associativa (N:N) |

---

## professores

| Coluna | Tipo | Restrições |
|---|---|---|
| **professor_id** | INT | **PK**, auto incremento |
| nome | VARCHAR(100) | NOT NULL |
| email | VARCHAR(150) | NOT NULL, UNIQUE |

## cursos

| Coluna | Tipo | Restrições |
|---|---|---|
| **curso_id** | INT | **PK**, auto incremento |
| nome | VARCHAR(100) | NOT NULL |
| carga_horaria | INT | NOT NULL |
| *professor_id* | INT | **FK** → professores(professor_id), NOT NULL |

## aula

| Coluna | Tipo | Restrições |
|---|---|---|
| **aula_id** | INT | **PK**, auto incremento |
| *curso_id* | INT | **FK** → cursos(curso_id), NOT NULL |
| numero | INT | NOT NULL, UNIQUE (curso_id, numero) |
| titulo | VARCHAR(150) | NOT NULL |
| duracao_minutos | INT | NOT NULL |

## alunos

| Coluna | Tipo | Restrições |
|---|---|---|
| **aluno_id** | INT | **PK**, auto incremento |
| nome | VARCHAR(100) | NOT NULL |
| email | VARCHAR(150) | NOT NULL, UNIQUE |

## telefones_alunos

| Coluna | Tipo | Restrições |
|---|---|---|
| **aluno_id** | INT | **PK** (composta), **FK** → alunos(aluno_id) |
| **telefone** | VARCHAR(20) | **PK** (composta), NOT NULL |

## matriculas

| Coluna | Tipo | Restrições |
|---|---|---|
| **aluno_id** | INT | **PK** (composta), **FK** → alunos(aluno_id) |
| **curso_id** | INT | **PK** (composta), **FK** → cursos(curso_id) |
| data_matricula | DATE | NOT NULL |
| nota_final | DECIMAL(4,2) | NULL enquanto o curso não termina |

## aula_concluida

| Coluna | Tipo | Restrições |
|---|---|---|
| **aluno_id** | INT | **PK** (composta), **FK** → alunos(aluno_id) |
| **aula_id** | INT | **PK** (composta), **FK** → aula(aula_id) |
| data_conclusao | DATE | NOT NULL |

---

## Regras que o banco não impõe sozinho

1. **Aluno só conclui aula de curso em que está matriculado.** Implementar com trigger em `aula_concluida`, verificando se existe linha em `matriculas` para o aluno e o curso da aula.
2. **Curso tem pelo menos uma aula** e **aluno tem pelo menos um telefone** (mínimo 1 do enunciado). Exigem validação na aplicação ou trigger.
