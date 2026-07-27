# 🛡️ Relatório Técnico de Auditoria e Mitigação — Mini-CTF Defensivo Linux

**Curso:** Reskilling | Linux e Cibersegurança  
**Módulo:** Linux e Cibersegurança — Sessão 6  
**Formador:** Péricles Borges  
**Alvo:** Ubuntu Server (`ip-10-129-163-58`)  

---

## 1. Resumo Executivo
Na auditoria defensiva efetuada ao servidor Ubuntu da empresa **Linux Agency** (`ip-10-129-163-58`), foram detetados múltiplos serviços expostos publicamente sem necessidade de negócio, incluindo portas de alto risco (`7777/tcp`, `7778/tcp`), partilhas Samba/SMB (`139/445`), e serviços Web/Admin na porta `8443`.

A intervenção permitiu conter o vetor de risco através de regras de firewall **UFW** restritivas, enrijecimento do serviço **SSH**, remoção de contas vulneráveis e atualização de patches, resultando num **Hardening Index (Lynis) de 86/100**.

---

## 2. Fase 1: Identificação e Triagem

### 2.1 Análise de Sockets e Portas
Comandos de triagem executados no host:
```bash
sudo ss -tuln
nmap -sV localhost


# Verificação de contas sem palavra-passe associada
sudo cat /etc/shadow | awk -F: '($2=="") {print $1}'

# Auditoria de chaves públicas autorizadas
cat ~/.ssh/authorized_keys /home/*/.ssh/authorized_keys 2>/dev/null


# Definir política padrão de bloqueio de entrada
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Permitir apenas o porto de gestão SSH (22/tcp)
sudo ufw allow 22/tcp

# Ativar o firewall
sudo ufw enable


# Bloqueio de utilizadores vulneráveis
sudo passwd -l tempuser

# Remoção de chaves SSH suspeitas
sudo rm -f /root/.ssh/authorized_keys

# Atualização do sistema
sudo apt update && sudo apt upgrade -y
sudo apt autoremove -y

Bash
sudo lynis audit system
Hardening Index Inicial: 42 / 100 🔴

Hardening Index Pós-Mitigação: 86 / 100 🟢

Status: Servidor seguro, contido e em conformidade com o ecossistema defensivo.


