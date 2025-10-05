# Criando_AMI
 
 # Gerenciamento de Instâncias EC2 na AWS

## Serviços e Conceitos-Chave

Nesta documentação estamos explorando os conceitos de **IaaS (Infraestrutura como Serviço)**, com foco nos seguintes serviços AWS:

* **EC2 (Elastic Compute Cloud):**
    * **O que é:** Uma instância que fornece capacidade de computação na nuvem através de uma máquina virtual.
    * **Para que serve:** Executar aplicações, hospedar sites, criar ambientes de desenvolvimento e muito mais.

* **AMI (Amazon Machine Image):**
    * **O que é:** Uma imagem pré-configurada contendo o sistema operacional, bibliotecas, etc., usada como *template* para lançar instâncias.
    * **Para que serve:** Facilitar a criação de instâncias e criar AMIs a partir de instâncias existentes (personalização).

* **EBS (Elastic Block Store):**
    * **O que é:** Um serviço para fornecer **armazenamento em bloco** persistente, confiável e escalável para uso com instâncias EC2.
    * **Para que serve:** Fornecer armazenamento persistente para o sistema operacional e dados.

*...
* Snapshot
O que é: É como uma "fotografia" do estado de um volume EBS em um determinado momento.
Para que serve: Permite restaurar dados ou voltar ao estado anterior em caso de falhas ou testes (Recuperação de Desastres).

---
##  Passo a Passo da Prática em AWS EC2

### 1. Configuração Inicial e Lançamento

* **AMI e Tipo:** Ubuntu Server 22.04 LTS | Tipo de instância (**t2.micro**).

* **Key Pair:** Par de Chaves criado para o acesso seguro via **SSH**.

* **Security Group:** Configurado para liberar a **Porta 22 (SSH)** [PARA SEU IP/RANGE] e a **Porta 80 (HTTP)** liberada para 0.0.0.0/0 (dependendo do tipo de instácia que configurou).

### 2. Conexão e Comandos na Instância

* **Comando SSH:** Utilizei o seguinte comando, onde o parâmetro `-i` identifica o arquivo de chave privada:
    ```bash
  ssh -i "nome-da-chave.pem" ubuntu@endereço-ip-público
    ```

### 3. Exemplo de Comandos e Boas Práticas

#### Verificação e Atualização do Sistema

...
Após a conexão SSH, realize a atualização dos repositórios de pacotes para garantir que o sistema operacional esteja com a versão mais recente dos softwares:

```bash
sudo apt update
sudo apt upgrade -y

...
## Arquitetura e Contexto de Uso

### 1. Diagrama de Arquitetura

O diagrama a seguir ilustra o fluxo de processamento de arquivos que utilizei para contextualizar o papel da **EC2 Instance (Processing Server)** na arquitetura.

![Diagrama de Arquitetura de Processamento de Arquivos AWS](images/ArquiteturaEC2.png)

*O EC2 atua como o motor de processamento, utilizando volumes EBS dedicados para armazenamento de trabalho (D-EBS) e resultados (E-EBS).*

---

## Insights e Aprendizados Chave

Nesta seção, abordo as conclusões técnicas mais importantes extraídas da prática:

1.  **Gerenciamento de Custos e Ciclo de Vida:**
    O ponto de maior atenção no uso do EC2, especialmente na **Free Tier**, é a distinção clara entre os estados `Stop` (Parar) e `Terminate` (Encerrar). Percebi que o comando `Stop` desliga a máquina virtual, mas o **volume EBS associado continua provisionado e gerando custos**. Para evitar despesas indesejadas, é fundamental a prática de **encerrar (`Terminate`)** a instância quando ela não for mais necessária, liberando assim todos os recursos de computação e armazenamento.

2.  **Segurança em Camadas (Security Group):**
    O **Security Group** é a primeira linha de defesa e atua como um **firewall stateful** no nível da instância. Minha principal conclusão de segurança foi a importância de aplicar o **Princípio do Mínimo Privilégio**. Ao configurar o acesso SSH (`Porta 22`), restrinjo o tráfego de entrada apenas ao meu endereço IP, em vez de liberar para `0.0.0.0/0`. Essa prática minimiza a superfície de ataque da instância.

3.  **Persistência de Dados via EBS em Arquiteturas:**
    A arquitetura do diagrama deixou clara a função do **EBS** como um bloco de armazenamento persistente. Enquanto a instância EC2 fornece a capacidade de **processamento (CPU/RAM)**, o EBS garante a **persistência e a durabilidade** dos dados de trabalho e dos logs de saída. A capacidade de criar um **Snapshot** deste volume é essencial para a estratégia de **Backup e Recuperação de Desastres**.

---
---

## Recursos e Documentação Recomendada

* **[Guia do Usuário do Amazon EC2](https://docs.aws.amazon.com/ec2/userguide/):** Fonte oficial para aprofundamento no EC2, Ciclo de Vida e Security Groups.

* **[Guia do Usuário do EBS](https://docs.aws.amazon.com/ebs/):** Fonte oficial para aprofundamento E Detalhes sobre a persistência e tipos de volumes utilizados na arquitetura.

* **[GitHub Markdown](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax/):** Guia específico para Markdown no GitHub 




