Sessão 1: Introdução ao Linux para Segurança e Comandos de RedeEste repositório contém a documentação da prática guiada de auditoria de sistemas, focada na identificação de interfaces de rede locais, análise de portas em escuta e mapeamento de ativos remotos utilizando o Nmap.  🖥️ 1. Ambiente Local (KillerCoda)A. Identificação da Interface de Rede PrincipalPara identificar o endereço IP da interface principal no ambiente local do KillerCoda, foi executado o comando ip a:  Baship a
Output obtido:Plaintext1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc fq_codel state UP group default qlen 1000
    link/ether 96:8a:df:eb:ca:8d brd ff:ff:ff:ff:ff:ff
    inet 172.30.1.2/24 brd 172.30.1.255 scope global dynamic noprefixroute enp1s0
       valid_lft 86307266sec preferred_lft 75518066sec
    inet6 fe80::53c3:3164:d82d:4f16/64 scope link 
       valid_lft forever preferred_lft forever
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1454 qdisc noqueue state DOWN group default 
    link/ether ba:44:a7:54:4b:7b brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
Interface Principal: enp1s0  Endereço IP (IPv4): 172.30.1.2  B. Portas e Serviços em Escuta LocalPara listar todas as portas TCP e UDP abertas e em estado de escuta no ambiente local, foi utilizado o comando ss -tuln:  Bashss -tuln
Output obtido:PlaintextNetid      State        Recv-Q       Send-Q             Local Address:Port              Peer Address:Port      Process
udp        UNCONN       0            0                  127.0.0.54:53                        0.0.0.0:*                 
udp        UNCONN       0            0               127.0.0.53%lo:53                        0.0.0.0:*                 
udp        UNCONN       0            0                  172.30.1.2:68                        0.0.0.0:*                 
udp        UNCONN       0            0             172.30.1.2%enp1s0:68                        0.0.0.0:*                 
udp        UNCONN       0            0         [fe80::53c3:3164:d82d:4f16]%enp1s0:546                 [::]:*                 
tcp        LISTEN       0            4096               127.0.0.54:53                        0.0.0.0:*                 
tcp        LISTEN       0            128                   0.0.0.0:40200                     0.0.0.0:*                 
tcp        LISTEN       0            511                   0.0.0.0:40205                     0.0.0.0:*                 
tcp        LISTEN       0            4096            127.0.0.53%lo:53                        0.0.0.0:*                 
tcp        LISTEN       0            4096                  0.0.0.0:22                        0.0.0.0:*                 
tcp        LISTEN       0            4096                127.0.0.1:42627                     0.0.0.0:*                 
tcp        LISTEN       0            4096                        *:40300                           *:*                 
tcp        LISTEN       0            4096                        *:40305                           *:*                 
tcp        LISTEN       0            4096                     [::]:22                           [::]:*                 
🎯 2. Mapeamento do Alvo Remoto (TryHackMe - Further Nmap)Após iniciar a máquina alvo com o IP 10.128.190.192, foi realizado o scan recomendado para deteção de versões de serviços e execução de scripts padrão (-sV -sC):  Bashnmap -sV -sC 10.128.190.192
📊 Resultados do Scan NmapNúmero de portas abertas identificadas: 5 portas TCP abertas.  Tabela de Serviços e Versões DetetadasPortaEstadoServiçoVersão Exata DetetadaObservações / Scripts standard21/tcpAbertaftpFileZilla ftpd  Permite login anónimo (Anonymous FTP login allowed), mas o comando para listar diretórios falhou por timeout.  53/tcpAbertadomainSimple DNS Plus  Serviço de DNS ativo.  80/tcpAbertahttpMicrosoft IIS httpd 10.0  Servidor Web IIS ativo. O método TRACE (potencialmente de risco) está habilitado.  135/tcpAbertamsrpcMicrosoft Windows RPC  Serviço de RPC do Windows ativo.  3389/tcpAbertams-wbt-serverMicrosoft Terminal Services  Serviço de Ambiente de Trabalho Remoto (RDP). Nome do host detetado: WIN-SCAN.  Nota do Auditor: O alvo é claramente uma máquina Windows Server (confirmado pelo IIS 10.0 e a presença dos serviços RPC/RDP). A existência de login FTP anónimo permitido (porta 21) é um vetor que deve ser investigado ou desativado em ambientes de produção para mitigar a exposição de dados.  📝 Output Completo do Scan NmapPlaintextNmap scan report for ip-10-128-190-192.eu-west-3.compute.internal (10.128.190.192)
Host is up (0.00025s latency).
Not shown: 995 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
21/tcp   open  ftp           FileZilla ftpd
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
| ftp-syst: 
|_  SYST: UNIX emulated by FileZilla
53/tcp   open  domain        Simple DNS Plus
80/tcp   open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
| http-methods: 
|_  Potentially risky methods: TRACE
135/tcp  open  msrpc         Microsoft Windows RPC
3389/tcp open  ms-wbt-server Microsoft Terminal Services
|_ssl-date: 2026-07-14T00:05:57+00:00; -1s from scanner time.
| ssl-cert: Subject: commonName=win-scan
| Not valid before: 2026-07-12T23:59:07
|_Not valid after:  2027-01-11T23:59:07
| rdp-ntlm-info: 
|   Target_Name: WIN-SCAN
|   NetBIOS_Domain_Name: WIN-SCAN
|   NetBIOS_Computer_Name: WIN-SCAN
|   DNS_Domain_Name: win-scan
|   DNS_Computer_Name: win-scan
|   Product_Version: 10.0.17763
|_  System_Time: 2026-07-14T00:05:26+00:00
MAC Address: 06:EA:57:A8:3E:03 (Unknown)
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: -1s, deviation: 0s, median: -1s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 47.22 seconds
