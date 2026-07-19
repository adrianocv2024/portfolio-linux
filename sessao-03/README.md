# Sessão 3: Hardening de Redes Linux e Configuração de Firewalls

Este repositório contém a documentação prática do laboratório de segurança focado na implementação de políticas defensivas estritas utilizando o **UFW (Uncomplicated Firewall)** e **iptables**.

---

## 1. Regras do UFW Ativas

Abaixo encontra-se o output real do comando `sudo ufw status verbose`, confirmando que as políticas padrão foram aplicadas com sucesso e que os serviços necessários estão devidamente autorizados:

```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), deny (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
8443/tcp                   ALLOW IN    Anywhere                  
8443/udp                   ALLOW IN    Anywhere                  
443                        ALLOW IN    Anywhere                  
80/tcp                     ALLOW IN    Anywhere                  
22/tcp                     ALLOW IN    Anywhere                  
8443/tcp (v6)              ALLOW IN    Anywhere (v6)             
8443/udp (v6)              ALLOW IN    Anywhere (v6)             
443 (v6)                   ALLOW IN    Anywhere (v6)             
80/tcp (v6)                ALLOW IN    Anywhere (v6)             
22/tcp (v6)                ALLOW IN    Anywhere (v6)


Chain INPUT (policy DROP 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         
 4708  607K ufw-before-logging-input  all  --  any    any     anywhere             anywhere            
 4708  607K ufw-before-input  all  --  any    any     anywhere             anywhere            
   26  4284 ufw-after-input  all  --  any    any     anywhere             anywhere            
    1    52 ufw-after-logging-input  all  --  any    any     anywhere             anywhere            
    1    52 ufw-reject-input  all  --  any    any     anywhere             anywhere            
    1    52 ufw-track-input  all  --  any    any     anywhere             anywhere            
    0     0 DROP       all  --  any    any     203.0.113.50         anywhere            

Chain FORWARD (policy DROP 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         
    0     0 DOCKER-USER  all  --  any    any     anywhere             anywhere            
    0     0 DOCKER-FORWARD  all  --  any    any     anywhere             anywhere            
    0     0 ufw-before-logging-forward  all  --  any    any     anywhere             anywhere            
    0     0 ufw-before-forward  all  --  any    any     anywhere             anywhere            
    0     0 ufw-after-forward  all  --  any    any     anywhere             anywhere            
    0     0 ufw-after-logging-forward  all  --  any    any     anywhere             anywhere            
    0     0 ufw-reject-forward  all  --  any    any     anywhere             anywhere            
    0     0 ufw-track-forward  all  --  any    any     anywhere             anywhere            

Chain OUTPUT (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         
 5321 7375K ufw-before-logging-output  all  --  any    any     anywhere             anywhere            
 5321 7375K ufw-before-output  all  --  any    any     anywhere             anywhere            
  437 30131 ufw-after-output  all  --  any    any     anywhere             anywhere            
  437 30131 ufw-after-logging-output  all  --  any    any     anywhere             anywhere            
  437 30131 ufw-reject-output  all  --  any    any     anywhere             anywhere            
  437 30131 ufw-track-output  all  --  any    any     anywhere             anywhere
