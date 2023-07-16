# Atividade no Linux

- Objetivo : Criar um ambiente AWS com uma instância EC2 e configurar o NFS para armazenar dados.

- Escopo : A atividade incluirá a geração de uma chave pública de acesso, criação de uma instância EC2 com o sistema operacional Amazon Linux 2, geração de um endereço IP elástico e anexá-lo à instância EC2, saída de portas de comunicação para acesso público, configuração do NFS, criação de um diretório com o nome do usuário no filesystem do NFS, instalação e configuração do Apache, criação de um script para validar se o serviço está online e enviar o resultado para o diretório NFS, e configuração da execução automatizada do script a cada 5 minutos.

# Requisitos
Instância AWS:
- Chave pública para acesso ao ambiente
AmazonLinux 2
t3.small
SSD de 16 GB
1 Elastic IP associado a instância
Portas de comunicação liberadas
22/TCP (SSH)
111/TCP e UDP (RPC)
2049/TCP/UDP (NFS)
80/TCP (HTTP)
443/TCP (HTTPS)
ConfiguraçõesLinux:
Configurar o NFS entregue;
Crie um diretório dentro do filesystem do NFS com seu nome;
Subir um apache no servidor - o apache deve estar online e rodando;
Crie um script que valide se o serviço esta online e envie o resultado da validação para o seu diretório no nfs;
O script deve conter - Data HORA + nome do serviço + Status + mensagem personalizada de ONLINE ou offline;
O script deve gerar 2 arquivos de saida: 1 para o serviço online e 1 para o serviço OFFLINE;
Execução automatizada do script a cada 5 minutos.
Instruções de Execução
Gerar uma chave pública de acesso na AWS e anexá-la a uma nova instância EC2.
Acesse a AWS na pagina do serviço EC2, e clique em "Pares de chaves" no menu lateral esquerdo.
Clique em "Criar par de chaves".
Insira um nome para a chave e clique em "Criar par de chaves".
Salve o arquivo .pem gerado em um seguro local.
Clique em "Instâncias" no menu lateral esquerdo.
Clique em "Executar instâncias".
Configure as Tags da instância (Name, Project e CostCenter) para instâncias e volumes.
selecione a imagem Amazon Linux 2 AMI (HVM), SSD Volume Type.
selecione o tipo de instância t3.small.
selecione a chave gerada anteriormente.
Coloque 16 GB de armazenamento gp2 (SSD).
Clique em "Executar instância".
Alocar um endereço IP elástico à instância EC2.
Acesse a AWS na pagina do serviço EC2, e clique em "IPs elásticos" no menu lateral esquerdo.
Clique em "Alocar endereço IP elástico".
selecione o ip alocado e clique em "Ações" > "Associar endereço IP elástico".
selecione a instância EC2 criada anteriormente e clique em "Associar".
Configurar gateway de internet.
Acesse a AWS na pagina do serviço VPC, e clique em "Gateways de internet" no menu lateral esquerdo.
Clique em "Criar gateway de internet".
Defina um nome para o gateway e clique em "Criar gateway de internet".
selecione o gateway criado e clique em "Ações" > "Associar à VPC".
selecione a VPC da instância EC2 criada anteriormente e clique em "Associar".
Configurar rota de internet.
Acesse a AWS na pagina do serviço VPC, e clique em "Tabelas de rotas" no menu lateral esquerdo.
selecione a tabela de rotas da VPC da instância EC2 criada anteriormente.
Clique em "Ações" > "Editar escalas".
Clique em "Adicionar rota".
Configure da seguinte forma:
Destino: 0.0.0.0/0
Alvo: selecione o gateway de internet criado anteriormente
Clique em "Salvar alterações".
Configurar regras de segurança.
Acesse a AWS na página do serviço EC2, e clique em "Segurança" > "Grupos de segurança" no menu lateral esquerdo.
selecione o grupo de segurança da instância EC2 criada anteriormente.
Clique em "Editar regras de entrada".
Configure as seguintes regras:
Tipo	Protocolo	Intervalo de portas	origem	Descrição
SSH	TCP	22	0.0.0.0/0	SSH
TCP personalizado	TCP	80	0.0.0.0/0	HTTP
TCP personalizado	TCP	443	0.0.0.0/0	HTTPS
TCP personalizado	TCP	111	0.0.0.0/0	RPC
UDP personalizado	UDP	111	0.0.0.0/0	RPC
TCP personalizado	TCP	2049	0.0.0.0/0	NFS
UDP personalizado	UDP	2049	0.0.0.0/0	NFS
Configurar o NFS com o IP fornecido
Crie um novo diretório para o NFS usando o comando sudo mkdir /mnt/nfs.
Montar o NFS no diretório criado usando o comando sudo mount IP_OU_DNS_DO_NFS:/ /mnt/nfs.
Verifique se o NFS foi montado usando o comando df -h.
Configure o NFS para montar automaticamente no boot usando o comando sudo nano /etc/fstab.
Adicione a seguinte linha no arquivo /etc/fstab:
IP_OU_DNS_DO_NFS:/ /mnt/nfs nfs defaults 0 0
Salvar o arquivo /etc/fstab.
Crie um novo diretório para o usuário alexlopes usando o comando sudo mkdir /mnt/nfs/alexlopes.
Configurar o Apache.
Executar o comando sudo yum update -ypara atualizar o sistema.
Execute o comando sudo yum install httpd -ypara instalar o apache.
Execute o comando sudo systemctl start httpdpara iniciar o apache.
Execute o comando sudo systemctl enable httpdpara habilitar o apache para iniciar automaticamente.
Execute o comando sudo systemctl status httpdpara verificar o status do apache.
Configurações adicionais do apache podem ser feitas no arquivo /etc/httpd/conf/httpd.conf.
para parar o apache, executar o comando sudo systemctl stop httpd.
