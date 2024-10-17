# Documento do Projeto Terraform 
Leitura do Arquivo
O código Terraform em questão fornece a infraestrutura necessária para lançar uma instância EC2 na AWS, utilizando o Debian 12 como sistema operacional. Ele também gera pares de chaves de acesso SSH e organiza a rede dentro de uma VPC. O código inclui regras de segurança e outputs que facilitam a gestão da infraestrutura criada.

Descrição Técnica
O código consiste nos seguintes componentes principais:

Provider Configuration: Configuração do provedor AWS, especificando a região a ser usada.

Variáveis:

projeto: Nome do projeto, com um valor padrão.
candidato: Nome do responsável, também com valor padrão.
Chave TLS: Gera um par de chaves TLS que será utilizado para permitir o acesso seguro à instância EC2.

Key Pair da AWS: Cria um par de chaves no AWS, usando a chave pública gerada anteriormente.

Configuração da VPC: Criação de uma VPC com um bloco CIDR 10.0.0.0/16.

Subrede: Implementa uma sub-rede na VPC com o bloco CIDR 10.0.1.0/24.

Internet Gateway: Adiciona um gateway de internet à VPC para permitir acesso público à instância.

Rota de Tabela: Cria uma tabela de rotas que direciona o tráfego da sub-rede para o gateway de internet.

Associação de Tabela de Rota: Associa a tabela de rotas à sub-rede.

Grupo de Segurança: Configura um grupo de segurança permitindo acesso SSH (porta 22) de qualquer endereço e todo tráfego de saída.

Fonte de Dados AMI: Recupera a AMI do Debian 12.

Instância EC2: Cria uma instância EC2 na sub-rede especificada, utiliza a chave pública e executa um script de inicialização (user data) para atualizar o sistema.

Outputs:

private_key: Chave privada, sensível, necessária para acesso SSH.
ec2_public_ip: O endereço IP público da instância EC2.
Observações
O código tem um ponto vulnerável na regra de segurança que permite SSH de qualquer endereço IP, o que pode ser uma crítica para ambientes de produção.
É sempre recomendável usar tagging para melhor gerenciamento e organização dos recursos no AWS.

## Descrição Técnica  
Este projeto configura uma infraestrutura básica na AWS usando Terraform. Ele cria uma VPC, sub-rede, gateway de internet, tabela de rotas, grupo de segurança e uma instância EC2 executando Debian 12.  

### Recursos Criados  
- **VPC**: Criação de uma VPC.  
- **Subrede**: Implementação de uma sub-rede.  
- **Internet Gateway**: Adiciona acesso à internet.  
- **Grupo de Segurança**: Permite SSH de um IP específico e todo tráfego de saída.  
- **Instância EC2**: Criação de uma instância para executar um servidor web Nginx.  

## Instruções de Uso  

### Pré-requisitos  
- [Terraform](https://www.terraform.io/downloads.html) instalado.  
- Credenciais AWS configuradas (por exemplo, usando o AWS CLI).  

### Passos para Inicialização  
1. Clone este repositório:  
   ```bash  
   git clone <URL do repositório>  
   cd <diretório do repositório> 
   
Configure o arquivo terraform.tfvars com o seu endereço IP que terá acesso SSH:

hcl
ssh_ip = "SEU_IP_AQUI"  
Inicialize Terraform:

bash
terraform init  
Verifique o plano de execução:

bash
terraform plan  
Aplique as configurações:

bash
terraform apply  
Acesse a instância EC2 utilizando a chave privada gerada.

bash
ssh -i <caminho_para_chave_privada> ubuntu@<IP_PÚBLICO>  
Código Modificado
O código modificado foi incluído em main.tf. As mudanças destacam melhorias em segurança, automação da instalação do Nginx e flexibilidade no tipo de instância. 

### Modificação e Melhoria do Código Terraform
Aplicação de Melhorias de Segurança
Para aumentar a segurança do ambiente, as seguintes alterações foram feitas no grupo de segurança:

Permitir SSH apenas de um IP específico: O acesso SSH será restrito a um único IP ou uma faixa de IPs confiáveis através de uma variável. 

### Automação da Instalação do Nginx
Adicionaremos um script de inicialização (user data) à instância EC2 para instalar e iniciar o servidor Nginx após a criação.

### Outras Melhorias
Uma sugestão para aumentar a gestão e clareza no código é a inclusão de uma variável para definir o tipo de instância.

### Resultados Esperados
A segurança é aprimorada ao restringir o acesso SSH.
O Nginx é instalado e iniciado automaticamente ao aparecer a instância EC2, permitindo que ela esteja pronta para receber tráfego imediatamente.
A flexibilidade na configuração do tipo de instância pode facilitar futuras mudanças no desempenho ou nos requisitos de custo.
