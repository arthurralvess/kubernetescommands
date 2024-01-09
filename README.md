# kubernetescommands

Kubernetes


- kubectl run ngnix --image nginx  #criar pod
- kubectl get pods  #listar os pods
- kubectl describe pod <nome-pod>  #detalhes de um pod
- kubectl delete pod <nome-pod>   #remover um pod

[Arquivo de definição kubernetes yml]
apiVersion: apps/v1           # apiVersion - Qual a versão de API do objeto que será usado no Kubernetes para criar esse objeto.          
kind: Deployment              # kind - Qual tipo de objeto pretende criar.
metadata:                        # metadata - Dados que ajudam a identificar de forma única o objeto, incluindo uma string nome, UID e um namespace.
  name: nginx-deployment
  labels:
        app: nginx
        tier: frontend
spec:                              # spec - Que estado deseja para o objeto.
    containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80

- kubectl create -f <nome-arquivo-yml>  #criar um pod, um deployment ou service a partir de um arquivo yml
- kubectl delete -f <nome-arquivo-yml>  #remover um pod, um deployment ou service a partir de um arquivo yml


[Arquivo de definição com o uso do replicaset (executa várias instâncias de um pod e mantém o número especificado de pods constante)] 
apiVersion: apps/v1
kind: ReplicaSet
metadata:
    name: myapp-rs
    labels: 
        app: myapp
        type: front-end
spec:
    template:
        metadata:
           name: myapp-pod
           labels: 
             app: myapp
             type: pod 
        spec:
           containers: 
           - name: nginx-container
             image: nginx
    replicas: 3
    selector:
        matchLabels:
           type: front-end 

- kubectl get replicaset  #listar replicasets
- kubectl delete replicaset [nome do arquivo]
- kubectl describe replicaset [nome] 
- kubectl replace -f [nome do arquivo]  #atualizar mudanças feitas para que possam ser implementadas
- kubectl scale --replicas=6 -f [nome do arquivo]  #alterar algo no arquivo por meio de linha de comando


[Deployment (Permite gerenciar um conjunto de Pods identificados por uma label.)(Deployment sempre cria e gerencia um Replicaset.)]

apiVersion: apps/v1
kind: Deployment
metadata:
    name: myapp-rs
    labels: 
        app: myapp
        type: front-end
spec:
    template:
        metadata:
           name: myapp-pod
           labels: 
             app: myapp
             type: pod 
        spec:
           containers: 
           - name: nginx-container
             image: nginx
    selector:
        matchLabels:
           type: front-end 
    replicas: 3


- kubectl get deployment  #listar deployments
- kubectl rollout status deployment/nginx-deployment  #status de implementação da implantação
- kubectl create -f deploy.yaml --record  #salvar linha no historico
- kubectl rollout status deploy myapp-deployment  #verificar o status 
- kubectl rollout history deploy myapp-deployment  #verificar o histórico
- kubectl rollout undo deployment/app --to-revision=2  #reverter para uma versão específica


Tipos de atualização 

RollingUpdate: Novos pods são adicionados gradualmente e pods antigos são encerrados gradualmente
Recriar:  todos os pods antigos são encerrados antes que novos pods sejam adicionados



Kubernetes e Redes 

- LoadBalancer: é o método padrão para conectar um serviço externamente à Internet. Neste cenário, um balanceador de carga de rede encaminha todo o tráfego externo para um serviço. Cada serviço tem seu próprio endereço IP. Funciona exatamente como o NodePort.
     - name: http
         targetPort: 80
         port: 80
         nodePort: 30042
    type: LoadBalancer

- ClusterIP: é o serviço padrão do Kubernetes para comunicações internas. Entretanto, o tráfego externo pode acessar o serviço padrão ClusterIP do Kubernetes por um proxy. Isso pode ser útil para depurar serviços ou exibir painéis internos.
    ports:
       - name: nginx
         port: 80
    type: ClusterIP
 
- NodePort: abre as portas nos nós ou nas máquinas virtuais, e o tráfego é encaminhado das portas para o serviço.
    ports:
       - name: http (Valor Opcional)
         targetPort: 80 (Valor Opcional)
         port: 80
         nodePort: 30090 (Valor Opcional)
    type: NodePort


Kubernetes e Namespaces

Ex:     apiVersion: v1
          kind: Namespace
          metadata:
              name: vote


