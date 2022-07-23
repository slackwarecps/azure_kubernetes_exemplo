# azure_kubernetes_exemplo
azure_kubernetes_exemplo

### Azure Kubernetes

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