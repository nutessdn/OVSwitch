# OpenvSwitch 

*O OpenvSwitch é um switch virtual multi-camadas, ele foi arquitetado para permitir configuração e programação de forma automatizada.Ele foi projetado para permitir a automação de rede por meio de extensão programática,enquanto ainda suporta interfaces e protocolos de gerenciamento padrão (por exemplo, NetFlow, sFlow, IPFIX, RSPAN, CLI, LACP, 802.1ag).*



O OpenVSwitch contém vários programas utilitários. Cada um é responsável por uma parte de sua arquitetura. Na tabela abaixo pode ser verificada uma breve descrição de cada utilitário. 


Utilitário | Função
-----------|------------- 
ovs-vsctl  | Configurar o OpenVSwitch
ovs-ofctl  | Administrar o switch em modo Openflow
ovs-appctl | Comunicar com os deamons do switch
ovs-dpctl  | Controlar o encaminhamento do switch

## Básicos:


**$ ovs-vsctl show** -> Listar as portas e bridges

**$ ovs-vsctl add-br br0** -> Adicionando uma bridge

**$ ovs-vsctl add-port br0 eth0** -> Adicionando uma porta eth na bridge 

**$ ovs-vsctl add-port br0 eth1 tag=42** -> Adicionando uma porta eth com uma tag

**$ ovs-ofctl dump-flows br0** -> Listando fluxos

**$ ovs-ofctl dump-flows br0 table=4** -> Listando fluxos




###			*Acessando o Switch*
      * (necessita de permissão root) *








**Usando o ovs-ofctl:**

**$ sudo ovs-ofctl show s1**
-> Se conecta ao switch e retorna o estado das portas e recursos disponíveis;

**$ sudo ovs-ofctl dump-flows s1**
-> Exibe os fluxos nas tabelas(necessita iniciar o controlador, caso contrario estará vazia);

**$ ovs-ofctl dump-flows tcp:{ip}:{porta padrão: 6633 + n} (n é o número do switch)**
-> Realizando esta conexão TCP, pode-se recuperar o fluxo específico de um switch;

**ovs-ofctl add-flow s1 in_port=1,actions=output:2**
-> Encaminhará os pacotes enviados a porta 1 para porta 2 e vice-e-versa;
(checar a tabela de fluxo para ver o resultado)

**$ sudo ovs-ofctl del-flows s1**
-> Limpa os fluxos

