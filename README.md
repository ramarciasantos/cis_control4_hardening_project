
---

## 📋 CIS Control 1: Inventory and Control of Enterprise Assets
*(Inventário e Controlo de Ativos Empresariais)*

**Objective:** Descobrir, monitorizar e gerir ativamente todos os dispositivos (físicos ou virtuais) ligados à rede da organização. Isto impede a ligação de *Unauthorized Devices* (Dispositivos Não Autorizados / Piratas), que reduzem a segurança da infraestrutura.

### Asset Inventory Table *(Tabela de Inventário de Ativos)*


| Asset ID *(ID do Ativo)* | Device Name *(Nome do Dispositivo)* | Asset Function *(Função do Ativo)* | IP Address *(Endereço IP)* | MAC Address *(Endereço MAC)* | Asset Type *(Tipo de Ativo)* | Criticality *(Criticidade)* |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **HW-01** | `Host-Master` | Test Source *(Origem dos Testes)* | *[Inserir IP da Origem]* | *[XX:XX:XX:XX:]* | Virtual Machine *(VM)* | Medium *(Média)* |
| **HW-02** | `Target-Server` | Target Server *(Servidor Alvo)* | `192.XXX.XXX.XXX` | *[XX:XX:XX:XX]* | Virtual Machine *(VM)* | High *(Alta)* |

---

## 📦 CIS Control 2: Inventory and Control of Software Assets
*(Inventário e Controlo de Ativos de Software)*

** Objective:** Garantir que apenas software legítimo, autorizado e atualizado está em execução na empresa. Softwares antigos, sem suporte (*End of Life*) ou não homologados são vetores críticos de ataque para exploração de vulnerabilidades.

### Software Inventory Table *(Tabela de Inventário de Software)*


| Software ID *(ID do SW)* | Software Name / OS *(Nome / Sistema Operativo)* | Installed Version *(Versão Instalada)* | Software Purpose *(Finalidade do Software)* | Security Status *(Estado de Segurança)* |
| :--- | :--- | :--- | :--- | :--- |
| **SW-01** | `kali linux` | Updated *(Atualizado)* |
| **SW-02** | `iputils-ping` | Nativa | ICMP Diagnostic Tool *(Ferramenta de Diagnóstico)* | Authorized *(Autorizado)* |
| **SW-03** | `ARP Daemon` | Nativa | Layer 2 Address Resolution *(Resolução na Camada 2)* | Critical System *(Essencial)* |

---

## 🔐 CIS Control 3: Data Protection
*(Proteção de Dados)*

** Objective:** Identificar, classificar e proteger dados sensíveis contra acessos não autorizados ou roubo (*Data Exfiltration*).

### Data Classification for ICMP Tráfico *(Classificação de Dados para o Tráfego ICMP)*

1. **Data Classification *(Classificação dos Dados):*** 
   O tráfego gerado nos testes do laboratório contém apenas metadados de diagnóstico (Tamanho do pacote, TTL e RTT). É classificado como **Public / Internal Operational Data** *(Dados Públicos / Operacionais Internos)*. Não contém dados confidenciais ou sensíveis de negócio.

2. **Governance Security Rule *(Regra de Segurança da Governação):*** 
   Embora o tráfego não seja secreto, o campo de dados de um pacote ICMP (os 56 bytes enviados) pode ser abusado por atacantes para ocultar malware ou comandos de comando e controlo através de **ICMP Tunneling** *(Túnel ICMP)*.


# CIS Control 4

Projeto prático de auditoria e fortalecimento de sistema Linux baseado nas salvaguardas do controlo CIS 4.

## 🌐 Isolamento de Rede Perimetral (CIS 4.2)

Para garantir a total segurança do sistema anfitrião (host) e da rede local residencial (como o router doméstico, computadores e televisões), o laboratório foi configurado estritamente em modo **Host-only** (Placa de rede exclusiva do hospedeiro).

### Por que razão o modo Host-only é o mais seguro?
* **Isolamento Total:** Cria uma rede virtual fechada onde apenas as máquinas virtuais comunicam entre si.
* **Sem Rota Lateral:** Bloqueia completamente o acesso à Internet. Isso impede que qualquer malware ou ataque interaja com dispositivos reais da sua rede local.
* **Controlo de Tráfego:** Mitiga o risco de fugas de dados ou tráfego malicioso para fora do ambiente de testes.

![Configuração Host-only no VMware](img.isolamentoderede.png)

*(Nota: Caminhos de diretórios locais foram ofuscados por motivos de privacidade e segurança dos dados).*


---

## 🔑 Gestão de Contas e Credenciais (CIS Control 4.7)

De acordo com a salvaguarda **CIS Control 4.7 (Manage Default Accounts)**, manter credenciais padrão de fábrica em sistemas operativos ativos representa um risco crítico de segurança, pois facilita o acesso inicial não autorizado por parte de atacantes.

### Mitigação Prática Aplicada
Após garantir o isolamento perimetral da rede virtual, foi efetuado o *hardening* (endurecimento) interno do sistema operativo do atacante (Kali Linux) através da revogação imediata das credenciais padrão de instalação (`kali:kali`).

* **Ação:** Utilização do comando `passwd` para a criação de uma nova palavra-passe robusta e exclusiva para o utilizador do sistema.

![Hardening de Senhas no Linux](gestao_credenciais.png)
*(Nota: O print demonstra a confirmação do sistema operativo após a atualização bem-sucedida das credenciais de segurança).*

## Conectividade de Rede.
# Análise de Conectividade de Rede com ICMP (Ping)

Testes de conectividade de rede local utilizando o protocolo ICMP através do comando `ping` num ambiente Linux.

## Diagnóstico de Rede

A imagem abaixo ilustra um teste de eco (*Echo Request/Reply*) bem-sucedido direcionado ao endereço IP `192.xxx.xxx.xxx`:

![Resultado do Comando Ping](protocoloICMP.png)

---

## Resumo Técnico do Estudo

O teste foi executado com sucesso e apresenta estabilidade total na comunicação entre os dispositivos da rede local. Abaixo estão as principais conclusões métricas extraídas da análise:

### 1. Desempenho e Latência (RTT)
* **O que é Latência?**
  É um tempo de atraso que o dado leva para viajar do ponto de origem até o destino. No caso do comando `ping´ medimos o RTT(Round-Trip time) que é o tempo total de ida e volta.
* **Tempo Mínimo (`min`):** 0.839 ms (alta velocidade de resposta).
* **Tempo Máximo (`max`):** 7.251 ms (ocorrido apenas no primeiro pacote devido à resolução ARP inicial).
?? Por que esse tempo de latência tão grande no primeiro pacote?
por causa da ARP(`PROTOCOLO DE RESOLUÇÃO DE ENDEREÇO´).

* **ARP Resquest** faz a pergunta(EM BROADCAST): Quem tem o IP XXX.XXX.XXX.XXX?
* ** ARP REPLY(em unicast) a maquina reconhece seu proprio IP e responde com seu endereço fisico(MAC)

* **Tempo Médio (`avg`):** 2.617 ms.
* **Instabilidade da Rede (`mdev`):** 2.679 ms. O desvio médio baixo confirma que a ligação é estável e livre de oscilações severas (*jitter*).
* **obs:** esses testes sao feitos em uma laboratorio domestico pessoal.
e altera quando se trata de  uma estrutura empresarial.

### 2. Integridade dos Dados
* **Pacotes Transmitidos:** 4
* **Pacotes Recebidos:** 4
* **Perda de Pacotes (`packet loss`):** 0% (comunicação limpa, sem descartes).

### 3. Topologia da Rede
* **O que é mapeamento de salto?**
  É o processo de **descobrir e listar** cada roteador intermediario (cada roteador um salto.)
* **Mapeamento de Saltos:** O valor de **`TTL=64`** permaneceu intacto na resposta. Isto comprova tecnicamente que o dispositivo de destino está localizado no mesmo segmento da **rede local (LAN)**, sem a necessidade de atravessar *roteadores* intermédios.



