# Realize o Download e instale o Zabbix

## Instalação do MariaDB 10.3

```sh
apt install mariadb-server mariadb-client
```

> Alterar a senha do usuário root do MariaDB

```sh
mariadb -u root

USE mysql;
UPDATE user SET password=PASSWORD('SUPER_SENHA') WHERE User='root';
UPDATE user SET plugin="mysql_native_password";
FLUSH PRIVILEGES;
quit;
```

## Instalar e configurar o servidor Zabbix para a sua plataforma

### a. Instale o repositório Zabbix

```sh
wget https://repo.zabbix.com/zabbix/5.4/debian/pool/main/z/zabbix-release/zabbix-release_5.4-1+debian10_all.deb
dpkg -i zabbix-release_5.4-1+debian10_all.deb
apt update
```

### b. Instale o servidor, o frontend e o agente Zabbix

```sh
apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent
```

### c. Criar banco de dados inicial

```sh
mysql -uroot -p
[SUPER_SENHA]

create database zabbix character set utf8 collate utf8_bin;
create user zabbix@localhost identified by 'OutraSuperSenha';
grant all privileges on zabbix.* to zabbix@localhost;
quit;
```

> No servidor do Zabbix, importe o esquema inicial e os dados. Vocá será solicitado a inserir a senha que foi criada anteriormente.

```sh
zcat /usr/share/doc/zabbix-sql-scripts/mysql/create.sql.gz | mysql -uzabbix -p zabbix
[OutraSuperSenha]
```

### d. Configure o banco de dados para o servidor Zabbix

> Editar arquivo /etc/zabbix/zabbix_server.conf

```sh
DBPassword=OutraSuperSenha
```

![image](https://user-images.githubusercontent.com/23584038/129354797-693993a3-d71c-4698-ab78-32541bc4b979.png)
