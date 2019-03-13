# OpenvSwitch

*O OpenvSwitch é um switch virtual multi-camadas, ele foi arquitetado para permitir configuração e programação de forma automatizada.Ele foi projetado para permitir a automação de rede por meio de extensão programática,enquanto ainda suporta interfaces e protocolos de gerenciamento padrão (por exemplo, NetFlow, sFlow, IPFIX, RSPAN, CLI, LACP, 802.1ag).*



O OpenVSwitch contém vários programas utilitários. Cada um é responsável por uma parte de sua arquitetura. Na tabela abaixo pode ser verificada uma breve descrição de cada utilitário.


Utilitário | Função
-----------|-------------
ovs-vsctl  | Configurar o OpenVSwitch
ovs-ofctl  | Administrar o switch em modo Openflow
ovs-appctl | Comunicar com os daemons do switch
ovs-dpctl  | Controlar o encaminhamento do switch

## Básicos:


**$ ovs-vsctl show**

-> Exibe o status das bridges com portas associadas a estas e do controlador

**$ ovs-vsctl add-br br0**

-> Adiciona uma bridge com nome br0

**$ ovs-vsctl add-port br0 eth0**

-> Adiciona uma porta eth na bridge br0

**$ ovs-vsctl list-port br0**

-> Lista as portas na bridge br0

**$ ovs-vsctl del-port br0 eth0**

-> Deleta uma porta eth na bridge br0

**$ ovs-vsctl add-port br0 eth1 tag=42**

-> Adicionando uma porta eth com uma tag atuando esta como acesso a _Vlan42_

**$ watch ovs-appctl fdb/show br0**

-> Lista a tabela de encaminhamento da bridge

**$ ovs-dpctl show**

-> Lista as estatisticas das portas

**$ ovs-ofctl add-flow br0 actions=NORMAL**

-> Adicionando um fluxo

**$ ovs-ofctl add-flow br0 priority=0,actions=NORMAL**

 -> Adicionando um fluxo com a mais alta prioridade


###			*Acessando o Switch*
*(necessita de permissão root)*

Caso o equipamento o qual esteja instalado o OpenVSwitch seja um dispositivo remoto use:

**$ ssh root@192.168.2.1**

Realiza uma comunicação utilizando SSH, podendo assim executar os comandos diretamente como root, a utilização de autenticação utilizando certificados depende da configuração e instalação.
*Caso o equipamento esteja instalado em uma maquina local, que permita o acesso direto pode-se executar os comandos direto no terminal*

**O Serviço**

O OpenvSwitch pode ser controlado, concorrer e atuar como um serviço normal dentro do sistema operacional. Essa administração depende do gerenciador de serviços do sistema operacional:

**SystemD**

**$ systemctl start openvswitch.service**

-> Podem ser utilizadas as opções start, stop, restart, status, enable, disable

**SystemV**

**$ service openvswitch start**

-> Podem ser utilizadas as opções start, stop, restart, status, enable, disable

**Habilitando Openflow**

Em seu modo básico o OpenvSwitch atua como um switch Layer 2 virtual (via software). Para habilitar o protocolo Openflow a fim de permitir que um controlador remoto tome as decisões do plano de controle do switch deve ser executar:

**$ ovs-vsctl set-controller br0 tcp:[ip]:[porta]**

Feito isso o switch toma suas decisões baseadas nas tabelas de fluxos que foram povoadas pelo controlador.

Caso deseja certificar-se se existe um controlador atuando, pode utulizar:

**$ sudo ovs-vsctl get-controller br0**

-> Se existir, este retornará a porta e o tipo de protocolo e o ip que é utilizado na comunicação

**$ sudo ovs-vsctl del-controller br0**

-> Desabilita o controlador, fecha a comunicação entre o controlador e o switch

**Habilitando o OVSDB**

Para ter um gerenciador remoto das configurações do switch é necessário habilitar o OVSDB. Para tal, deve executar o seguinte comando:

**$ ovs-vsctl set-manager tcp:[ip]:[porta]**



###  **Usando o ovs-ofctl:**

**$ sudo ovs-ofctl show br0**

-> Se conecta ao switch *br0* e retorna o estado das portas e recursos disponíveis;

**$ ovs-ofctl dump-flows br0 table=4**

-> Listando fluxos da tabela 4 da bridge *br0*

**$ sudo ovs-ofctl dump-flows br0**

-> Exibe os fluxos nas tabelas(necessita iniciar o controlador, caso contrario estará vazia);

**$ ovs-ofctl dump-flows tcp:[ip]:[porta padrão: 6633 + n] (*n é o número do switch*)**

-> Realizando esta conexão TCP, pode-se recuperar o fluxo específico de um switch;

**ovs-ofctl add-flow br0 in_port=1,actions=output:2**

-> Encaminhará os pacotes enviados a porta 1 para porta 2 e vice-e-versa;
(checar a tabela de fluxo para ver o resultado)

**$ sudo ovs-ofctl del-flows s1**

-> Limpa os fluxos
