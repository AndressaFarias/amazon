doc : https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.PostgreSQL.CommonDBATasks.Roles.html

Quando uma instancia de banco de dados RDS para PostgresSQL é criada. Por padrão é criado um usuário administrador, por padrao o nome desse usuáiro é `postgres`;

O nome do usuário pode ser alterado - porém devem ser seguidas algumas premissas para criação do nome do usuário;

essa primeira conta é membro do grupo 'rds_superuser` e tem privilégios rds_superuser.


## Understanding the rds_superuser role

No PostgreSQL uma `role` pode definir um usuário, um grupo, ou um conjunto de permissões especificas concedidas a um grupo ou um usuário no banco de dados.

No Postgres os comandos `CREATE USER` e `CREATE GROUP` foram substituidos pelo mais geral `CREATE ROLE` com propriedades especificas para distinguir os usuários do banco de dados. Um usuário de banco de dados pode ser considerado uma role com privilégios de LOGIN. 

As propriedades `NOSUPERUSER`, `NOREPLICATION`, `INHERIT` e `VALID UNTIL 'infinity'` são as opções padrão para `CREATE ROLE`, a menos que especificado de outra forma.


Por padrão, o `postgres` tem privilégios concedidos à role `rds_superuser`. A função `rds_superuser` permite que o usuário postgres faça o seguinte:

- Adicione extensões disponíveis para uso com o Amazon RDS. Working with PostgreSQL features supported by Amazon RDS for PostgreSQL - https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/PostgreSQL.Concepts.General.FeatureSupport.html

- Criar role para usuários e conceda privilégios aos usuários.
    - ROLE : https://www.postgresql.org/docs/current/sql-createrole.html
    - GRANT : https://www.postgresql.org/docs/14/sql-grant.html

- Criar bancos de dados.

- Conceda privilégios `rds_superuser` a roles de usuário que não tenham esses privilégios e revogue os privilégios conforme necessário.

- Conceda (e revogue) a role `rds_replication` aos usuários do banco de dados que não possuem a função `rds_superuser`.

- Conceda (e revogue) a role `rds_password` aos usuários do banco de dados que não possuem a função `rds_superuser`.

- Obtem informações de status sobre todas as conexões de banco de dados usando a view `pg_stat_activity`. Quando necessário, `rds_superuser` pode interromper qualquer conexão usando `pg_terminate_backend` ou `pg_cancel_backend`.


O RDS for PostgreSQL é um serviço gerenciado, portanto, você não pode acessar o sistema operacional host e não pode se conectar usando a conta de superusuário do PostgreSQL. 

Para obter mais informações sobre como conceder privilégios, consulte GRANT na documentação do PostgreSQL. - https://www.postgresql.org/docs/current/sql-grant.html


A role `rds_superuser` é uma das várias roles predefinidas em uma instância de banco de dados RDS for PostgreSQL.


Na lista a seguir, você encontra algumas das outras funções predefinidas que são criadas automaticamente para uma nova instância de banco de dados RDS for PostgreSQL. Funções predefinidas e seus privilégios não podem ser alterados. Você não pode descartar, renomear ou modificar privilégios para essas funções predefinidas.


**rds_password** : Uma role que pode alterar senhas e configurar restrições de senha para usuários do banco de dados. A role `rds_superuser` recebe essa role por padrão e pode conceder a função aos usuários do banco de dados.
Controel de acesso de usuários ao banco de dados : https://docs.aws.amazon.com/pt_br/AmazonRDS/latest/UserGuide/Appendix.PostgreSQL.CommonDBATasks.Roles.html#Appendix.PostgreSQL.CommonDBATasks.Access



**rdsadmin** : Uma role criada para lidar com muitas das tarefas de gerenciamento que o administrador com privilégios de `superusuário` executaria em um banco de dados PostgreSQL independente. Essa função é usada internamente pelo RDS for PostgreSQL para muitas tarefas de gerenciamento.


**rdstopmgr** : Um role usada internamente pelo RDS para oferecer suporte a implantações Multi-AZ



Para ver todas as roles predefinidas, você pode se conectar à sua instância de banco de dados RDS for PostgreSQL e usar o metacomando `psql \du`. A saída é a seguinte:


Na saída, você pode ver que `rds_superuser`` não é uma role de usuário do banco de dados (não pode fazer login), mas tem os privilégios de muitas outras funções.
Você também pode ver que o usuário do banco de dados `postgres` é um membro da role `rds_superuser`.

Conforme mencionado anteriormente, `postgres` é o valor padrão na página Criar banco de dados do console do Amazon RDS. Se você escolher outro nome, esse nome será mostrado na lista de funções.


## Controlar o acesso de usuários ao banco de dados PostgreSQL

### Para configurar role e privilégios para uma nova instância de banco de dados

Suponha que você esteja configurando um banco de dados em uma instância de banco de dados RDS for PostgreSQL recém-criada para uso por vários researchers (pesquisadores), todos os quais precisam de acesso de leitura e gravação ao banco de dados.

1 - Use psql (ou pgAdmin) para se conectar à sua instância de banco de dados RDS for PostgreSQL:

`psql --host=your-db-instance.666666666666.aws-region.rds.amazonaws.com --port=5432 --username=postgres --password`

Quando solicitado, digite sua senha. O client psql se conecta e exibe o banco de dados de conexão administrativa padrão, `postgres=>`, como prompt.



2 - Para evitar que os usuários do banco de dados criem objetos no esquema público, faça o seguinte:

```sql
    postgres=>  CREATE ON SCHEMA public FROM PUBLIC;
    REVOKE 
```

3 - Em seguida, você cria uma nova instância de banco de dados:
~~~sql 
    postgres=> CREATE DATABASE lab_db;
    CREATE DATABASE
~~~

4 - Revogue todos os privilégios do schema PUBLIC neste novo banco de dados.


5 - Crie uma role para usuários do banco de dados.
~~~sql 
    postgres=> CREATE ROLE lab_tech;
    CREATE ROLE

    CREATE ROLE sreteste;
~~~

6- Dê aos usuários do banco de dados que têm essa role  a capacidade de se conectar ao banco de dados.

~~~sql
    postgres=> GRANT CONNECT ON DATABASE lw_transport TO sreteste;
    GRANT
~~~


x7 - Conceda a todos os usuários com a role lab_tech todos os privilégios neste banco de dados.
~~~sql
postgres=> GRANT ALL PRIVILEGES ON DATABASE lab_db TO lab_tech;
GRANT
~~~


8 - Crie usuários de banco de dados, como segue:
~~~sql
CREATE ROLE teste LOGIN PASSWORD 'OeB6dohR9aiwaine3eegh1vu7dah1o';

CREATE ROLE lab_user2 LOGIN PASSWORD 'change_me';
~~~

DROP ROLE sreteste;



9 - Conceda a esses dois usuários os privilégios associados à função lab_tech:
~~~sql
    GRANT sreteste TO teste;

    GRANT lab_tech TO lab_user2;
~~~


GRANT sreteste TO teste;

Neste ponto, lab_user1 e lab_user2 podem se conectar ao banco de dados lab_db.


---


CREATE ROLE debezium LOGIN PASSWORD 'aQKd4pDo4VbS4QI6rHVUnNbnZ8PSKxTeeRsJ7vj2tRE=';

