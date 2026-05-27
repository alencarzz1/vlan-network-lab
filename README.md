# VLAN Network Lab

## Objetivo

Este laboratório teve como objetivo implementar uma rede segmentada utilizando VLANs, trunking, Router-on-a-Stick e DHCP centralizado.

O foco principal foi compreender:

- Segmentação de redes com VLANs
- Comunicação entre VLANs
- Funcionamento do trunk 802.1Q
- DHCP centralizado no roteador
- Troubleshooting de rede
- Fluxo de pacotes entre camada 2 e camada 3

---

# Topologia

topology

Estrutura utilizada:

```text
PCs -> Switch -> Trunk -> Router
```

---

# VLANs Utilizadas

| VLAN | Nome | Rede |
|---|---|---|
| 10 | ADMINISTRACAO | 192.168.10.0/24 |
| 20 | CONTABILIDADE | 192.168.20.0/24 |
| 30 | JURIDICO | 192.168.30.0/24 |

---

# Benefícios das VLANs

## Redução de Broadcast

As VLANs dividem a rede em múltiplos domínios de broadcast.

Sem VLANs, todos os dispositivos da rede receberiam broadcasts desnecessários, aumentando:

- consumo de banda
- processamento nos hosts
- tráfego desnecessário
- possibilidade de congestionamento

Com VLANs, o broadcast fica limitado apenas aos dispositivos pertencentes à mesma VLAN.

Exemplo:

- PCs da VLAN 10 não recebem broadcasts da VLAN 20
- PCs da VLAN 20 não recebem broadcasts da VLAN 30

Isso melhora significativamente o desempenho da rede.

---

## Segmentação Lógica da Rede

As VLANs permitem separar departamentos logicamente utilizando o mesmo switch físico.

Neste laboratório foram criadas VLANs para:

- Administração
- Contabilidade
- Jurídico

Essa segmentação facilita:

- organização
- gerenciamento
- separação de setores
- controle da infraestrutura

---

## Maior Segurança

As VLANs ajudam a reduzir exposição desnecessária entre dispositivos.

Sem VLANs:
- todos os hosts estariam na mesma rede aumentando a superfice de ataques
- qualquer dispositivo poderia se comunicar diretamente com outro

Com VLANs:
- a comunicação fica isolada
- o tráfego entre redes depende do roteador
- políticas de segurança podem ser aplicadas

Isso permite implementar:

- ACLs
- filtragem de tráfego
- controle de acesso
- segmentação de usuários
- isolamento de dispositivos críticos

---

## Melhor Desempenho da Rede

Ao reduzir broadcasts e segmentar o tráfego, as VLANs ajudam a melhorar:

- eficiência da comunicação
- utilização da banda
- estabilidade da rede
- processamento dos hosts

A rede fica mais organizada e menos sobrecarregada.

---

## Escalabilidade

A utilização de VLANs facilita crescimento da infraestrutura sem necessidade de criar múltiplas redes físicas.

Novos setores podem ser adicionados facilmente apenas criando:

- nova VLAN
- nova subinterface
- novo pool DHCP

Isso torna a infraestrutura mais flexível e expansível.

---

## Facilidade de Gerenciamento

As VLANs permitem administrar departamentos separadamente mesmo utilizando o mesmo switch.

Isso facilita:

- troubleshooting
- organização
- identificação de problemas
- gerenciamento de usuários
- administração da rede

---

## Controle de Tráfego

Com VLANs é possível controlar melhor o fluxo de tráfego entre setores.

A comunicação entre VLANs depende de roteamento.

Isso permite:

- monitorar tráfego
- registrar comunicação
- aplicar políticas
- limitar acessos específicos

---

## Aproximação de Ambientes Corporativos Reais

A segmentação por VLAN é amplamente utilizada em empresas para separar:

- usuários
- servidores
- dispositivos IoT
- visitantes
- sistemas críticos
- redes administrativas

Este laboratório simula conceitos utilizados em ambientes enterprise reais.

---

# Configuração do Switch

## Criação das VLANs

```bash
vlan 10
 name ADMINISTRACAO

vlan 20
 name CONTABILIDADE

vlan 30
 name JURIDICO
```

---

## Configuração das portas access

```bash
interface range fa0/1-4
 switchport mode access
 switchport access vlan 10

interface range fa0/5-8
 switchport mode access
 switchport access vlan 20

interface range fa0/9-12
 switchport mode access
 switchport access vlan 30
```

---

## Configuração do trunk

```bash
interface fa0/24
 switchport mode trunk
```

---

# Configuração do Roteador

## Subinterfaces

```bash
interface g0/0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

interface g0/0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0

interface g0/0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
```

---

# DHCP

## Configuração DHCP

```bash

ip dhcp pool VLAN10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8

ip dhcp pool VLAN20
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 8.8.8.8

ip dhcp pool VLAN30
 network 192.168.30.0 255.255.255.0
 default-router 192.168.30.1
 dns-server 8.8.8.8
```

---

# Verificações Realizadas

## Verificação das VLANs

```bash
show vlan brief
```

---

## Verificação do trunk

```bash
show interfaces trunk
```

---

## Verificação das interfaces do roteador

```bash
show ip interface brief
```

---

# Troubleshooting

Durante o laboratório o DHCP não estava funcionando corretamente.

Etapas realizadas durante o troubleshooting:

- Verificação das VLANs
- Verificação das portas access
- Verificação do trunk
- Verificação das subinterfaces
- Verificação das interfaces do roteador
- Análise do DHCP

Comando utilizado:

```bash
show running-config | section dhcp
```

Problema identificado:

- Ausência da configuração DHCP no roteador

Solução aplicada:

- Criação dos pools DHCP para cada VLAN
- Definição do gateway padrão
- Configuração do DNS


---

# Testes Realizados

## Testes de DHCP

- PCs receberam IP automaticamente
- Gateway configurado corretamente
- DNS configurado corretamente

---

## Testes de Comunicação

- Ping entre hosts da mesma VLAN
- Ping entre VLANs diferentes
- Comunicação com gateway
- Verificação de inter-VLAN routing

---

# Conceitos Aplicados

Durante o laboratório foram aplicados conceitos como:

- VLAN
- Switching
- Trunking
- DHCP
- Inter-VLAN Routing
- Router-on-a-Stick
- Troubleshooting
- Broadcast Domain
- Gateway
- Encapsulation dot1Q

---

# Ferramentas Utilizadas

- Cisco Packet Tracer
- Cisco IOS CLI

---

# Estrutura do Projeto

```text
vlan-network-lab/
│
├── README.md
├── topology/
│   └── topologia.png
├── configs/
│   ├── switch-config.txt
│   └── router-config.txt
├── screenshots/
│   ├── vlan-config.png
│   ├── trunk-config.png
│   ├── dhcp-working.png
│   └── ping-tests.png
└── packet-tracer/
    └── lab-vlan.pkt
```

---


---

# Aprendizados

Este laboratório permitiu compreender na prática:

- Funcionamento de VLANs
- Comunicação entre redes
- Funcionamento do trunk
- DHCP centralizado
- Inter-VLAN routing
- Troubleshooting de redes
- Fluxo de pacotes entre camada 2 e camada 3

Também foi possível compreender como o DHCP depende de broadcast e como o roteador atua como gateway para múltiplas redes.
