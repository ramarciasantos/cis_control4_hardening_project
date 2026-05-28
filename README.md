# Projeto de Hardening (CIS Control 4)

Projeto prático de auditoria e fortalecimento de sistema Linux baseado nas salvaguardas do controlo CIS 4.

## 🌐 Isolamento de Rede Perimetral (CIS 4.2)

Para garantir a total segurança do sistema anfitrião (host) e da rede local residencial (como o router doméstico, computadores e televisões), o laboratório foi configurado estritamente em modo **Host-only** (Placa de rede exclusiva do hospedeiro).

### Por que razão o modo Host-only é o mais seguro?
* **Isolamento Total:** Cria uma rede virtual fechada onde apenas as máquinas virtuais comunicam entre si.
* **Sem Rota Lateral:** Bloqueia completamente o acesso à Internet. Isso impede que qualquer malware ou ataque interaja com dispositivos reais da sua rede local.
* **Controlo de Tráfego:** Mitiga o risco de fugas de dados ou tráfego malicioso para fora do ambiente de testes.

![Configuração Host-only no VMware](img.isolamento de rede.png)

*(Nota: Caminhos de diretórios locais foram ofuscados por motivos de privacidade e segurança dos dados).*

