# Esboço do Relatório do Projeto – TL2526-2022149695

## Título: Planeamento e Configuração de uma Rede de Dados para o Instituto Tecnológico de Coimbra (ITC)

### 1. Introdução (Máx. 1 página)

-   **Objetivo:** Breve descrição do projeto, que consiste no planeamento, simulação e configuração de uma rede alargada para o ITC, de acordo com os requisitos especificados.
-   **Enquadramento:** Referência à importância de um design de rede robusto, seguro e escalável para uma instituição com múltiplos polos.
-   **Ferramentas:** Menção ao uso do GNS3 com imagens Cisco IOU para a simulação.
-   **Estrutura do Documento:** Apresentação sucinta das secções do relatório.

### 2. Desenho da Topologia da Rede (Máx. 2 páginas)

-   **Topologia Lógica:** Apresentação do diagrama de rede completo (pode ser uma captura de ecrã do GNS3), mostrando os 8 polos, o ISP e as interligações.
-   **Equipamentos:** Tabela com os equipamentos utilizados (routers e switches), modelos (imagens IOU) e a sua função na rede (e.g., `RSC-FW` como router de borda).
-   **Tecnologias WAN:** Descrição das tecnologias de ligação WAN implementadas entre a RSC e os polos, justificando as escolhas (Frame Relay, Multilink PPPoFR, MPLS AToM, Q-in-Q).
-   **Ligação à Internet:** Detalhe do esquema de redundância com a ligação principal na RSC e a secundária no CID.

### 3. Plano de Endereçamento IPv4 (Máx. 2 páginas)

-   **Espaço de Endereçamento Atribuído:** Referência aos blocos de endereços calculados com base no número de aluno:
    -   Rede Interna: `194.65.240.0/20`
    -   Ligações ao ISP: `10.20.95.238/30` e `10.30.95.242/30`
-   **Estratégia VLSM:** Explicação de como o bloco `/20` foi dividido para otimizar o uso de endereços, com exemplos da RSC e de um polo.
-   **Tabela de Endereçamento:** Apresentação da tabela de endereçamento resumida, como solicitado no guião, mostrando as principais sub-redes, máscaras e gateways.
-   **Endereçamento Privado:** Justificação para o uso de endereços privados (`10.100.0.0/16`) nas ligações ponto-a-ponto para conservar o espaço público.

### 4. Configuração e Protocolos (Máx. 3 páginas)

-   **Protocolo de Encaminhamento (OSPF):**
    -   Descrição da configuração do OSPF como IGP para toda a rede.
    -   Menção à configuração de uma área única (backbone area 0).
    -   Detalhe da implementação da autenticação MD5 para garantir a segurança das atualizações de encaminhamento.
-   **Configuração de Switching:**
    -   **VLANs e Inter-VLAN Routing:** Explicação de como as VLANs foram segmentadas e como o encaminhamento entre elas é realizado (Router-on-a-stick e SVIs em switches L3).
    -   **PVLANs:** Descrição da implementação de Private VLANs (isolated e community) na RSC para o departamento de Finanças, justificando o aumento de segurança.
    -   **RSTP:** Breve menção à configuração do Rapid Spanning Tree Protocol e das medidas de segurança associadas (`BPDU guard`, `root guard`, `loop guard`).
-   **NAT e Failover:** Explicação de como o NAT foi configurado no `RSC-FW` para permitir o acesso à Internet e como o failover para a ligação secundária no CID seria gerido (e.g., através de rotas flutuantes ou IP SLA).

### 5. Políticas de Segurança (Máx. 1 página)

-   **Segurança dos Equipamentos:** Resumo das políticas de segurança aplicadas a todos os routers e switches:
    -   Acesso remoto seguro via SSH (limitado a uma sessão).
    -   Criação de utilizador local com password encriptada.
    -   Encriptação de todas as passwords no ficheiro de configuração.
    -   Banners de aviso (MOTD).
-   **Segurança de Portas:** Menção à configuração de `port security` nos switches de acesso para mitigar ataques de MAC spoofing.
-   **Autenticação PPP:** Referência à utilização de CHAP nas ligações Multilink PPPoFR.

### 6. Conclusão (Máx. 1 página)

-   **Resumo do Projeto:** Síntese dos objetivos alcançados e das principais funcionalidades implementadas.
-   **Desafios e Opções Tomadas:** Breve discussão sobre as decisões mais importantes tomadas durante o projeto (e.g., escolha de tecnologias WAN, desenho da topologia).
-   **Verificação de Conetividade:** Afirmação de que a conetividade entre todos os polos do ITC e a Internet foi testada e verificada com sucesso.
-   **Otimização do GNS3:** Nota sobre a importância de garantir uma carga de CPU reduzida após o arranque da simulação.
