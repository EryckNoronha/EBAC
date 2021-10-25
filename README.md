# **Exploração e análise de dados de Pontuação de Exames – Exam Scores**

## Os dados: 

Os dados representam informações fictícias de um banco e contam com as seguintes colunas:

* gender = gênero female (feminino) e male (maculino)
* race_ethnicity = raça e etnia dos alunos avaliados, separados por grupos de A até D
* parental_level_of_education = nível de escolaridade dos pais
* lunch = como os alunos se alimentam na hora do almoço
* test_preparation_course = se fez um curso preparatório 
* math_score = nota em matemática
* reading score = nota em leitura
* writing score = nota em redação

A tabela foi criada no **AWS Athena** junto com o **S3 Bucket** com uma versão dos dados disponibilizados em: https://www.kaggle.com/spscientist/students-performance-in-exams?select=StudentsPerformance.csv

## **Exploração de dados:**

Esta primeira exploração tem como objetivo entender os registros. Vamos a analise:

**Qual a quantidade de informações temos na nossa base de dados?**

Query:
> SELECT count (*) as QUANT_DADOS FROM studentsperformance;
  
![Resultado 1](https://github.com/EryckNoronha/EBAC/blob/main/query%201.png?raw=true)

**Como são os dados?**

Query:
> SELECT * FROM studentsperformance LIMIT 10;
  
![Resultado 2](https://github.com/EryckNoronha/EBAC/blob/main/query%202.png?raw=true)
  
**Quais os tipos de cada dado?**

Query:
> DESCRIBE studentsperformance

![Resultado 3](https://github.com/EryckNoronha/EBAC/blob/main/query%203.png?raw=true)

**Quais são os tipos de raça e etnia disponíveis no dataset?**

Query:
> SELECT DISTINCT race_ethnicity FROM studentsperformance;

![Resultado 4](https://github.com/EryckNoronha/EBAC/blob/main/query%204.png?raw=true)

Por ser tratar de dados fictícios os grupos foram nomeados de A a D sem descrever realmente a etnia e raça dos pesquisados.

**Quais são os tipos de escolaridade dos pais disponíveis no dataset?**

Query:
>SELECT DISTINCT parental_level_of_education FROM studentsperformance;

![Resultado 5](https://github.com/EryckNoronha/EBAC/blob/main/query%205.png?raw=true)

Nesta analise podemos observar que os graus de escolaridade vão des de some high school (alguma escolaridade) até masters degree (mestrado) não sendo incluído pessoas com familiares analfabetos e nem com doutorado ou pós doutorado.

**Quais são as opções de tipo de almoço disponíveis no dataset?**

Query:
> SELECT DISTINCT lunch FROM studentsperformance;

![Resultado 6](https://github.com/EryckNoronha/EBAC/blob/main/query%206.png?raw=true)

As opções livre/reduzido e padrão são as únicas opções não havendo na pesquisa pessoas que não possuem alimentação.

**Quais formas de preparação estão disponíveis no dataset?**

![Resultado 7](https://github.com/EryckNoronha/EBAC/blob/main/query%207.png?raw=true)

Query:
> SELECT DISTINCT test_preparation_course FROM studentsperformance;

## **Análise de dados**
 Os contextos social, cultural e econômico são fatores de discussão sobre o desenvolvimento escolar. A indagação sobre sua influência, geram muitos debates sobre o resultado acadêmico dos discentes. Após analisar os dados contidos neste dataset vou indagar algumas questões para associar seus dados a conteúdos de desenvolvimento educacional escolar. Vamos as perguntas:
 
 **Nesse banco de dados, quantos são os alunos do sexo masculino e feminino?**
 
 Query:
>``` select count(*) as QUANTIDADE,
case 
when gender =  'female' then 'F'
when gender =  'male' then 'M' end as SEXO 
from studentsperformance
group by gender;

![Resultado 8](https://github.com/EryckNoronha/EBAC/blob/main/query%208.png?raw=true)

![Gráfico sexo](https://github.com/EryckNoronha/EBAC/blob/main/Grafico%20sexo%20estudantes.png?raw=true)


Pode-se observar que as quetidades de dados do sexo feminino e masculino são quase as mesmas tendo um pouco a mais de dados do sexo feminino.

**Os grupos de raça e etnia estão balanceados como o sexo?**

Query:
>``` select count(*) as QUANT_RACA_ETNIA,race_ethnicity from studentsperformance 
    group by race_ethnicity order by race_ethnicity;
    
![Resultado 9](https://github.com/EryckNoronha/EBAC/blob/main/query%209.png?raw=true)
![Grafico etnias](https://github.com/EryckNoronha/EBAC/blob/main/Quantidade%20Grupos.png?raw=true)

Por tanto a maior parte dos pesquisados é do grupo C e o menor é o grupo A, havendo diferença de quantidade entre os cinco grupos.

**Quantos fizeram preparatórios para as provas?**

Query:
>``` Query: select count(*) as QUANT_PREPARACAO, test_preparation_course 
from studentsperformance 
group by test_preparation_course ;

![Resultado 10](https://github.com/EryckNoronha/EBAC/blob/main/query%2010.png?raw=true)

Podemos observar que a maioria dos estudantes não fez preparatório  o que nos leva a ver o impacto no resultados das avaliações.

**Qual é a maior e menor nota das três disciplinas e o aluno fez ou não preparatório?**

Query:
>```  select max(math_score) as MAIO_MAT,
                 min(math_score) as MENOR_MAT,
                 max(reading_score) as MAIO_LEITURA,
                 min(reading_score) as MENOR_LEITURA, 
                 max(writing_score) as  MAIO_ESCRITA,
                 min(writing_score) as MENOR_ESCRITA, 
                 test_preparation_course as PRETARATORIO 
                 from studentsperformance 
                 GROUP BY test_preparation_course ;

![Resultado 11](https://github.com/EryckNoronha/EBAC/blob/main/query%2011.png?raw=true)

O que se pode observar é que a maior nota de todas as disciplinas ocorreram independente do aluno ter feito preparatório ou não porem a menor notas sempre apresentaram diferença, as menores nota foram maiores em relação a quem não fez preparatório.

**De um modo geral a média de notas dos alunos tem diferença em relação de quem fez preparatório de quem não fez?**

Query:
>```select round(avg(math_score),2) as MEDIA_MAT,
round (avg(reading_score),2) as MEDIA_LEITURA, 
round (avg(writing_score),2) as MEDIA_ESCRITA,
test_preparation_course   
from studentsperformance 
group by test_preparation_course;

![Resultado 12](https://github.com/EryckNoronha/EBAC/blob/main/query%2012.png?raw=true)
![Media notas](https://github.com/EryckNoronha/EBAC/blob/main/media%20notas%20preparatorio.png?raw=true)

  As notas dos alunos em média são maiores em todas as disciplinas não sendo uma diferença tão grande, mas demostra que existe uma diferença de quem tem um preparatório e de quem não tem.
  
  **Em relação ao grupo que pertencem e o grau escolar de seus pais, qual interferência pode ter em seus resultados finais?**
  
  Query:
>```select race_ethnicity as RACA_ETNIA, 
       parental_level_of_education as GRAU_ESC_PAIS,
       round(avg(math_score),2) as MEDIA_MAT,
       round (avg(reading_score),2) as MEDIA_LEITURA,
       round (avg(writing_score),2) as MEDIA_ESCRITA       
       from studentsperformance 
       group by race_ethnicity, parental_level_of_education
       order by race_ethnicity, parental_level_of_education

![Media notas](https://github.com/EryckNoronha/EBAC/blob/main/query%2013.2.png?raw=true)
![Media notas](https://github.com/EryckNoronha/EBAC/blob/main/query%2013.3.png?raw=true)
![Media notas](https://github.com/EryckNoronha/EBAC/blob/main/query%2013.1.png?raw=true)

A query gerou uma tabela com trinta linhas para melhor analise gerei gráficos separados relacionando primeiro os grupos em relação as medias onde podemos observar uma mínima vantagem no grupo E obs. o gráfico foi gerado com a soma das médias.No grau de escolaridade em relação as média já vemos um diferença considerável nos pais que tem mestrado (masters degree) e os que tem alguma escolaridade (high school). O gráfico de dispersão analisa toda a tabela onde temos os melhores resultados dos grupos e grau escolar em leitura e escrita sendo os resultados de matemática menores.

# Conclusão
​
Essas foram **algumas** análises extraídas do dataset Exam Scores.  
​
Alguns insights interessantes:
​
- Os pesquisados tinhas as quantidades de homens e mulheress equiparadas porem a maioria era do grupo C.
- As maiores notas foram obtidas independente do aluno ter feito preparatório ou não.
- As menores notas foram as dos alunos que não fizeram preparatorio.
- Na média geral os alunos que fizeram preparatório se sairam melhor.
- Os grupos a que pertencem não tem impacto no resultados dos entrevistados.
- As melhores notas foram dos alunos que tem familiar com grau escolar mestrado(masters degree)
- As piores notas foram dos alunos que tem familiar com grau escolar alguma escolaridade (high school)
- O grau escolar dos familiares interferiu neste caso nos resultados.
       
    
