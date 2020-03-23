# Atividade Prática – HBase

Atividades relacionadas ao HBase da pós de DataScience da Furb.

## Exercício 1

Agora, de dentro da imagem faça os sefuintes procedimentos:

1. Crie a tabela com 2 famílias de colunas:  a. personal-data b. professional-data 2. Importe o arquivo via linha de comando

```
create 'italians','personal-data','professional-data'
=> Hbase::Table - italians
```

2. Importe o arquivo via linha de comando
```
hbase shell /tmp/italians.txt
```
Agora execute as seguintes operações:

1. Adicione mais 2 italianos mantendo adicionando informações como data de nascimento nas informações pessoais e um atributo de anos de experiência nas informações profissionais;
```
put 'italians', '11', 'personal-data:name',  'Recigio Poffo'
put 'italians', '11', 'personal-data:city',  'Ascurra'
put 'italians', '11', 'personal-data:birthdate',  '28-04-1988'
put 'italians', '11', 'professional-data:role',  'Gestao Comercial'
put 'italians', '11', 'professional-data:years',  '10'

put 'italians', '12', 'personal-data:name',  'Sergio Poffo'
put 'italians', '12', 'personal-data:city',  'Ascurra'
put 'italians', '12', 'personal-data:birthdate',  '28-04-1956'
put 'italians', '12', 'professional-data:role',  'Gestao Comercial'
put 'italians', '12', 'professional-data:years',  '30'
```

2. Adicione o controle de 5 versões na tabela de dados pessoais

```
alter 'italians', NAME => 'personal-data', VERSIONS => 5

Updating all regions with the new schema...
1/1 regions updated.
Done.
```
3. Faça 5 alterações em um dos italianos;
```
put 'italians', '11', 'personal-data:city',  'Blumenau'
put 'italians', '11', 'personal-data:birthdate',  '22-01-1988'
put 'italians', '11', 'professional-data:role',  'Programador'
put 'italians', '11', 'professional-data:years',  '3'
put 'italians', '11', 'personal-data:birthdate',  '28-04-1988'
```
4. Com o operador get, verifique como o HBase armazenou o histórico.

```
get 'italians', '11',{COLUMN=>'personal-data:birthdate',VERSIONS=>3}

COLUMN                                                              CELL
 personal-data:birthdate                                            timestamp=1585000907380, value=28-04-1988
 personal-data:birthdate                                            timestamp=1585000906356, value=22-01-1988
 personal-data:birthdate                                            timestamp=1585000656394, value=28-04-1988
1 row(s)
Took 0.0447 seconds
```
5. Utilize o scan para mostrar apenas o nome e profissão dos italianos.

```
scan 'italians',{COLUMNS=>['personal-data:name','professional-data:role']}
```

6.  Apague os italianos com row id ímpar

```
deleteall 'italians', ['1', '3', '5','7','9','11']
```

7. Crie um contador de idade 55 para o italiano de row id 5
```
incr 'italians','5','personal-data:idade'
```
8. Incremente a idade do italiano em 1
```
incr 'italians','5','personal-data:idade'
```