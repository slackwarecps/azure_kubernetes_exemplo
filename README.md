# azure_kubernetes_exemplo
azure_kubernetes_exemplo

### Azure Kubernetes

    az login

    az aks get-credentials --resource-group alurasports-rg --name k8s

#### conecte no banco
    kubectl get pods    
    kubectl exec -it deployment-mysql-5c4d88fc5-g4w7s sh

## Rode os comandos no mysql




    mysql -u root

    use loja
    create table produtos (id integer auto_increment primary key, nome varchar(255), preco decimal(10,2));
    alter table produtos add column usado boolean default false;
    alter table produtos add column descricao varchar(255);
    create table categorias (id integer auto_increment primary key, nome varchar(255));
    insert into categorias (nome) values ("Futebol"), ("Volei"), ("Tenis");
    alter table produtos add column categoria_id integer;
    update produtos set categoria_id = 1;

    exit

    exit

## Suba os yaml


    kubectl create -f servico-aplicacao.yaml
    kubectl create -f deployment.yaml
    kubectl create -f statefulset.yaml
    kubectl create -f servico-banco.yaml
    kubectl create -f permissoes.yaml
    kubectl exec -it [nome do Pod do mysql] sh

Para testar a aplicação, execute 
    
    kubectl get services
    minikube service servico-aplicacao.


## Segredos exemplo

kubectl create secret docker-registry alurasportsfabiao.secret --docker-server alurasportsfabiao.azurecr.io --docker-username alurasportsfabiao --docker-password HHO3oXC+v1c9tMQczTcVyARS1sv8ToRM

    alurasportsfabiao
    HHO3oXC+v1c9tMQczTcVyARS1sv8ToRM

### Comandos Uteis
    kubectl version
    
az aks create --name heman-k8s --kubernetes-version "1.22.11" --node-count 2 --resource-group grayskull-rg --location eastus --generate-ssh-keys


az group create --name grayskull-rg --location eastus


az aks create `
    --name az-k8s `
    --resource-group az-alurasports-rg `
    --node-count 4 `
    --location eastus `
    --kubernetes-version 1.10.8 `
    --generate-ssh-keys

az acr create --name azregistrygrayskull  --resource-group grayskull-rg --sku Basic  --location eastus

## Mudar o número de nós em nosso cluster:
az aks scale --node-count 3 --name az-k8s --resource-group az-alurasports-rg

Obter as atualizações disponíveis ao cluster:
    az aks get-upgrades --name az-k8s --resource-group az-alurasports-rg --output table

    az aks upgrade --name az-k8s --resource-group alurasports-rg --kubernetes-version 1.11.3

## Criar registro de imagens de containers:
    az acr create --name azregistryalura --resource-group az-alurasports-rg --sku Basic --location eastus --admin-enabled true

# conectar no kubectl
    az aks get-credentials --resource-group grayskull-rg --name heman-k8s
# 

    az acr credential show --name azregistrygrayskull --output table

## doc


https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest

    az acr login --name azregistrygrayskull
    docker pull rafanercessian/aplicacao-loja:v1 
    docker tag rafanercessian/aplicacao-loja:v1 azregistrygrayskull.azurecr.io/loja/aplicacao-loja:v1
    docker push azregistrygrayskull.azurecr.io/loja/aplicacao-loja:v1

# Criando um segredo
    kubectl create secret docker-registry <NOME_DO_SEGREDO> --docker-server <NOME_DO_REGISTRO>.azurecr.io --docker-username=<USUARIO> --docker-password <SENHA> --docker-email <SEU_EMAIL>


    kubectl create secret docker-registry alura.registry.secret --docker-server registryalura.azurecr.io --docker-password GoatFGjDPccdIZUtmsqA7RTK6b=4Klq2 --docker-username=registryalura --docker-email alura.guicosta@outlook.com


# COMANDOS 

Estes são os comandos executados em aula. Note que estou utilizando o nome do registro de imagens registryalura. Quando for executar em sua máquina, lembre-se de trocar o nome do registro para o nome do seu registro em sua conta do Azure.

Para fazer o login em nosso registro, executamos:

    az acr login --name registryalura
Realizamos o pull da imagem rafanercessian/aplicacao-loja com a tag v1 do docker hub:

    docker pull rafanercessian/aplicacao-loja:v1 
Em seguida, criamos a tag adicionando o nome do registro registryalura.azurecr.io:

    docker tag rafanercessian/aplicacao-loja:v1 registryalura.azurecr.io/loja/aplicacao-loja:v1

Para fazer o upload ao nosso registro do Azure, usamos:

    docker push registryalura.azurecr.io/loja/aplicacao-loja:v1
Agora, para utilizarmos esta imagem em nosso arquivo YAML devemos criar um secret contendo a senha de acesso ao registro. No portal do Azure, navegue até seu recurso de registro, vá em Chaves de acesso e copie a senha e o usuário para usar no comando:

    kubectl create secret docker-registry <NOME_DO_SEGREDO> --docker-server <NOME_DO_REGISTRO>.azurecr.io --docker-username=<USUARIO> --docker-password <SENHA> --docker-email <SEU_EMAIL>
Na aula, usei o comando com os argumentos a seguir:

    kubectl create secret docker-registry gatoguerreiro.registry.secret --docker-server azregistrygrayskull.azurecr.io --docker-password K0+iWX2DJaFGceTChvywip4oAATUnCfQ --docker-username=azregistrygrayskull 


Feito isso, no seu arquivo YAML, adicione em spec, no mesmo nível de containers, o campo imagePullSecrets com o nome do segredo:

    imagePullSecrets:
        - name: alura.registry.secret
E altere o nome da imagem:

    spec:
    containers:
        - name: container-aplicacao-loja
        image: registryalura.azurecr.io/alurasports/aplicacao-loja:v1
Para atualizar o deployment já realizado, use o comando apply:

kubectl apply -f deployment.yaml
 DISCUTIR NO FORUM


{
  "passwords": [
    {
      "name": "password",
      "value": "K0+iWX2DJaFGceTChvywip4oAATUnCfQ"
    },
    {
      "name": "password2",
      "value": "Qqydj=EH7IsaNeZKSxo0Fv80/TtnK6x5"
    }
  ],
  "username": "azregistrygrayskull"
}

