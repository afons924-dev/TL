# Desenho da Topologia da Rede - Projeto ITC

Este documento descreve a topologia lógica da rede do Instituto Tecnológico de Coimbra (ITC), incluindo a distribuição dos polos, a atribuição de tecnologias WAN e a estrutura interna de cada localização.

## 1. Topologia Geral e Conectividade WAN

A rede é composta por um núcleo central, a **Reitoria e Serviços Centrais (RSC)**, e 7 polos distribuídos. As ligações entre a RSC e os polos utilizam uma combinação de tecnologias WAN para cumprir os requisitos do projeto. Os três routers de gateway na RSC (`RSC-GW1`, `RSC-GW2`, `RSC-GW3`) centralizam estas ligações.

- **RSC-GW1 (Hub Frame Relay):**
  - **Campus de Engenharia (CE):** Ligado via Frame Relay.
  - **Campus de Ciências da Saúde (CCS):** Ligado via Frame Relay.

- **RSC-GW2 (Hub MPLS e PPPoFR):**
  - **Centro de Investigação e Desenvolvimento (CID):** Ligado via MPLS (AToM) para garantir uma ligação moderna e eficiente, adequada a um centro de investigação.
  - **Escola de Ciências e Matemática Aplicada (ECMA):** Ligada via Multilink PPPoFR.
  - **Biblioteca e Centro de Recursos Digitais (BCRD):** Ligada via Multilink PPPoFR.

- **RSC-GW3 (Hub Q-in-Q - Arquitetura Corrigida):**
  - O tráfego dos polos CML e RE é recebido pelo switch `RSC-SW-QinQ`, que aplica a tag de transporte (outer tag).
  - O tráfego com duplo encapsulamento é então enviado para o router `RSC-GW3`, que termina o túnel e encaminha o tráfego para a rede interna.

## 2. Ligação à Internet

O ITC possui duas ligações à Internet para redundância:

- **Ligação Principal (1 Gbps):** Localizada na **RSC** e gerida pelo router `RSC-FW`.
- **Ligação Secundária (100 Mbps):** Localizada no **CID** e gerida pelo router `CID-FW`. Esta ligação funciona como backup e só é ativada em caso de falha da principal.

## 3. Estrutura Interna dos Polos

### Reitoria e Serviços Centrais (RSC)

- **Routers (6):**
  - `RSC-FW`: Router de borda, responsável pela ligação principal à Internet e pela segurança (NAT, ACLs).
  - `RSC-GW1`, `RSC-GW2`, `RSC-GW3`: Routers de gateway para as ligações WAN.
  - `RSC-Core1`, `RSC-Core2`: Routers centrais que gerem o encaminhamento interno da RSC.
- **Switches (10):**
  - `RSC-SWL3-1`, `RSC-SWL3-2`, `RSC-SWL3-3`: Switches de Camada 3 para encaminhamento Inter-VLAN de alto desempenho.
  - `RSC-SW1` a `RSC-SW6`: Switches de Camada 2 para acesso dos utilizadores.
  - `RSC-SW-QinQ`: Switch dedicado para a aplicação do túnel Q-in-Q.
- **VLANs (10):** Serão definidas no plano de endereçamento, incluindo VLANs departamentais, de gestão e de servidores. Incluirá a configuração de PVLANs.

### Restantes Polos (CE, CCS, ECMA, CID, BCRD, CML, RE)

Cada polo segue uma estrutura padronizada para facilitar a gestão:

- **Routers (4):**
  - `[Polo]-GW`: Router de gateway que estabelece a ligação WAN com a RSC.
  - `[Polo]-R1`, `[Polo]-R2`: Routers internos para segmentação e encaminhamento local.
  - `CID-FW`: No caso específico do CID, este é o router de borda para a ligação secundária à Internet. Nos outros polos, este router pode ser usado para segmentação adicional.
- **Switches (4):**
  - `[Polo]-SWL3-1`: Switch de Camada 3 para otimizar o encaminhamento local.
  - `[Polo]-SW1`, `[Polo]-SW2`, `[Polo]-SW3`: Switches de Camada 2 para acesso.
- **Sub-redes (6):** Mínimo de 6 VLANs/sub-redes por polo, destinadas a laboratórios, gabinetes, Wi-Fi, etc.

## 4. Diagrama Lógico

(O diagrama lógico será visualmente representado no GNS3. Esta secção descreve as ligações textualmente.)

- **ISP:** Um router (`ISP-R1`) com uma interface de loopback (`2.2.2.2`) e ligações diretas para `RSC-FW` e `CID-FW`.
- **Core (RSC):** Malha de routers e switches interligados para alta disponibilidade.
- **Distribuição:** Ligações ponto-a-ponto a partir dos gateways da RSC para os gateways de cada polo, utilizando as tecnologias WAN definidas.
- **Acesso:** Topologia em estrela em cada polo, com switches de acesso ligados a um switch de distribuição/agregação (L3).
