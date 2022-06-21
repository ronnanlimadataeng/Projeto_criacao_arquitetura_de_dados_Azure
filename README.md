CONTÉUDO DESTE ARQUIVO PREVE OS TÓPICOS ABAIXO:
---------------------

 * Introdução
 * Requerimentos
 * Instalação e Configuração
 * Manutenção
 * Agradecimentos




INTRODUÇÃO
------------

O projeto visa a migração dos dados de um ambiente on-premises para o data lake, transformando os arquivos que estão em on-premises no formato .csv convertendo-os para o formato .parquet em compressão snappy, que gera uma grande redução do tamanho e também aumenta a velocidade no momento de realizar leituras

NOME DOS RECURSOS | RECURSO
------------ | ------------ |
rg-project-rox-case		|		Resource Group
rdlsprojectroxcase		|		Storage account
adf-project-rox-case		|		Data factory (V2)
rkv-project-rox-case			|	Key vault
rsqldb-project-rox-case		|		SQL database
rsql-server-project-rox-case	|		SQL server



INSTALAÇÃO E CONFIGURAÇÃO
------------

1) Criação do grupo de recursos e definição dos recursos a serem criados.


2) Criação o Datalake no Microsoft Azure e nomeando com inicio padrão azure "dls"

Detalhes: criado com copia local = Locally-redudant storage(LRS), Enable hierarchical namespace - hierarquia de nivel de sub-pasta, copia local de backup mais economica.


2.1) Criação containers dentro do DataLake
		
    Nome do Container : transient
		Camada do Container: dados em transição
			
		Nome do Container : bronze(raw)
		Camada do Container: armazenamento de dados brutos

		Nome do Container : silver(trusted)
		Camada do Container: armazenamento de dados semi-tratados

		Nome do Container : gold(refined)
		Camada do Container: armazenamento de dados refinados, na sua forma limpa, agrupados.



3) Criação o Data Factory no Microsoft Azure e nomeando com inicio padrão azure "adf"

Detalhes:responsavel pela ingestão de dados nas camadas do datalake



4) Criação Key Valts para a segurança dos recursos do azure e nomeando com inicio padrão azure "kv"

Detalhes:*custa centavos provisionar, verificar que o arquivo ficara na lixeira por 90 dias, enable pode excluir.
verificar o acess policy para adicionar as permissões e adicionar outras pessoas.


5) O Banco de dados

			    criando sqlserver
					nome do servidor: sql-server-project-rox-case.database.windows.net
					usuário: project-rox-case
					senha: @#1proj-rx-cs
				  !alerta, configurar os basic para pequenos bancos de dados.
				  Locally-redundant backup storage
				  Liberar o firewall(permitir que serviços acessem este servidor) - 177.85.1.0



6) Criação o sqlserver para receber os dados e tabelas e nomeando com inicio padrão azure "sql"

Detalhes: instancia do sql server onde posso ter varios databases



7) Conectando integration run time ir-selfhosted-project-rox-case

Integration runtime, responsável pela conexão com a maquina local.



8) Criação repositorio do GitHub para conectar com os versionamentos do Azure

		definir a colaboration branch, principal é main.
		faço em dev e confirma na main
		sempre subir primeiro na branch "dev"



9) Criação os linked service
            
            ls_datalake  
            ls_azure_sqldb
            ls_key_valt
            ls_file_system
            ls_file_system_parquet
            ls_sqlserver



10) Melhorando a Seguranaça com Keyvalt - criando secrets

          •	secret-db-azure - conexão com um Azure SQL Databas
          •	secret-user-local - conexão com um diretório local
          •	secret-db-local - conexão com um banco de dados local
          •	secret-key-datalake - conexão com um azure data lake



11) migrando os linked service para acessar com keyvalt

12) criação pastas dentro do data set

Nome do dataset|	Campos setados dinamicamente|	Descrição
------------ |------------ |------------ |
ds_generic_delimited_text|	containerName,directoryname,fileName,separatorFile|	referente a manipulação de arquivos de origem do Azure data lake(servidor da nuvem)
ds_generic_delimited_text_filesystem|	fileName,separatorFile	|referente a manipulação de arquivos de origem do Filesystem(computador)
ds_generic_delimited_text_filesystem_parquet|	fileName	|referente a manipulação de arquivos de origem do Filesystem(computador)
ds_generic_parquet|	containerName,fileName	|referente a manipulação de arquivos de origem do Azure data lake(servidor da nuvem)
ds_generic_parquet_dataflow|	containerName	|referente a manipulação de arquivos de origem do Azure data lake(servidor da nuvem)
ds_generic_azure_db|	schemaName,tableName|	referente a manipulação de arquivos de origem do Azure SQL (servidor da nuvem)
ds_generic_sqlserver_db|	schemaName,tableName|	referente a manipulação de arquivos de origem do SQL SERVER (computador)



13) Criando pipelines

          copydata_costumer
          copydata_person
          copydata_product
          copydata_salesOrderDetail
          copydata_salesOrderHeader
          copydata_SpecialOfferProduct



14) criação de ambiente databricks

              dbw-project-rox-case
              ct_sn_rox_case-> single node -> standard_F4 (30 minutos)



MANUTENÇÃO
-----------

 * É necessario o acompanhamento dos recursos provisionados e atentar a criação de novos recursos pois deve-se sempre analisar a necessidade de alta disponibildiade, espaço e tamanho.

SUPORTE:

 * Ronnan Lima - (https://github.com/ronnanlimao)

AGRADECIMENTOS
-----------
Meus agradecimentos á oportunidade de realizar este projeto!

