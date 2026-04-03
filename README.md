# 🏛️ Infraestrutura Full Stack na Microsoft Azure

Este repositório documenta o provisionamento de uma arquitetura básica completa na Azure, integrando computação, rede e banco de dados dentro de um único grupo de recursos.

## 📊 Arquitetura da Solução
A estrutura foi desenhada para separar a camada de aplicação (VM) da camada de dados (SQL Server), garantindo organização e segurança.

* **Grupo de Recursos:** `MyLowCostVM_group` ou `my_sql_server`
* **Localização:** Brazil South (ou região de preferência)
* **Ambiente:** Desenvolvimento / Baixo Custo

<img width="1919" height="908" alt="image" src="https://github.com/user-attachments/assets/8c64b64c-164e-4c08-af0c-6cf56a2e2366" />


---

## 🛠️ Componentes Provisionados

### 1. Camada de Rede (Networking)
A base de toda a comunicação interna.
* **Virtual Network (VNet):** Define o espaço de endereçamento IP.
* **Subnet:** Divisão da rede para isolar a VM.
* **Network Security Group (NSG):** Firewall que controla o tráfego de entrada (Portas 22 para SSH e 1433 para SQL).

### 2. Camada de Computação (Virtual Machine)
* **Recurso:** `Vm1`
* **OS:** Ubuntu 24.04 LTS
* **Função:** Hospedar a aplicação, servidores web ou scripts de automação.
* **Acesso:** IP Público associado para administração remota.

### 3. Camada de Dados (Azure SQL)
* **Servidor Lógico:** `my-low-cost-sql-server`
* **Banco de Dados:** SQL Database (PaaS)
* **Função:** Armazenamento persistente de dados relacionais com backup gerenciado pela Azure.

---

## 🚀 Guia de Implantação (Passo a Passo)

### Passo 1: O Grupo de Recursos
Antes de tudo, criamos o **Resource Group**. Ele serve como um "container" lógico. Se você deletar o grupo, todos os recursos dentro dele (VM, VNet, SQL) serão excluídos juntos, facilitando a limpeza.

### Passo 2: Rede e VM
1. Criamos a **Virtual Machine**.
2. Durante a criação, a Azure solicita a configuração de rede. Aceitamos a criação da **VNet** e do **IP Público**.
3. Definimos o par de chaves SSH para acesso seguro.

### Passo 3: Banco de Dados SQL
1. Criamos o **Azure SQL Database**.
2. Configuramos o Servidor Lógico.
3. **Importante:** Nas configurações de rede do SQL, habilitamos a opção *"Allow Azure services to access this server"*, permitindo que a nossa VM Ubuntu consiga consultar o banco de dados.

---

## 🔐 Segurança e Conectividade

| Origem | Destino | Porta | Protocolo |
| :--- | :--- | :--- | :--- |
| Internet | VM | 22 | SSH |
| VM | SQL Server | 1433 | TDS (SQL) |
| Internet | SQL Server | 1433 | Bloqueado (Permitir apenas IPs específicos) |

---

## 💰 Gerenciamento de Custos
Para manter este ambiente "Low Cost":
1. **VM:** Utilize instâncias da série **B** (B-Series Burstable).
2. **SQL:** Utilize a camada **Basic** ou **Serverless**.
3. **Automação:** Configure o desligamento automático da VM às 18h (ou fim do expediente).

---
*Documentação de infraestrutura como serviço (IaaS) e plataforma como serviço (PaaS).*
