# ETL-de-Passivo-Trabalhista-Arquitetura-Bronze-Silver-SSIS-SQL-Server-
Este projeto implementa um pipeline completo de ETL para consolidação do Passivo Trabalhista por dia e por centro de custo (CODCENCUS), utilizando SQL Server Integration Services (SSIS), SQL Server e SQL Agent.

## Arquitetura

O pipeline foi desenhado seguindo uma abordagem em camadas, separando claramente extração, armazenamento e regra de negócio.

Os dados são expostos a partir do ERP Sankhya por meio de uma view (vw_passivo_trabalhista), que serve como fonte para o processo de ETL. A partir dela, o SSIS realiza a extração e carga dos dados em uma camada Bronze (staging), mantendo o dado bruto e historizado para processamento. Em seguida, a camada Silver aplica as regras de negócio e realiza a consolidação dos dados por meio de stored procedure no SQL Server.

Todo o processo é orquestrado via SQL Server Agent, garantindo execução automática e consistente do fluxo.

Sankhya (View)
      ↓
SSIS (Data Flow)
      ↓
Bronze (Staging)
      ↓
Silver (Business Layer)
      ↓
Consumo em BI

Tecnologias utilizadas

SQL Server, SSIS (Integration Services), SQL Server Agent e T-SQL (procedures, views e indexes).

## Regras de negócio

A camada de consolidação (Silver) foi projetada para processamento diário, agrupando os dados por data de referência e centro de custo (CODCENCUS). As principais métricas consolidadas incluem o total de passivo trabalhista, verbas rescisórias, FGTS e a contagem distinta de funcionários por centro de custo.

O projeto adota uma arquitetura em camadas (Bronze/Silver), garantindo separação entre ingestão e regra de negócio, o que facilita manutenção e escalabilidade do pipeline. O ETL foi implementado de forma desacoplada da regra de negócio, com orquestração centralizada no SQL Server Agent.

Durante a implementação, foram enfrentados desafios reais de integração entre ambientes distintos (ERP externo e Data Warehouse), incluindo limitações de Linked Server devido a restrições de TLS/certificados. Como solução, o SSIS foi utilizado como camada de integração, garantindo estabilidade e controle total do fluxo de dados.
