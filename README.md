**Simulação de Brute Force com Medusa no Kali Linux (WSL2)**

**Descrição do Projeto**
Este projeto demonstra a execução de um ataque de força bruta (Brute Force) em um ambiente controlado. O objetivo foi validar a eficiência de ferramentas automatizadas na descoberta de credenciais e discutir medidas de prevenção contra esse tipo de vulnerabilidade.

Devido ao uso de equipamento alugado, o laboratório foi adaptado para rodar via WSL2 (Windows Subsystem for Linux), garantindo a integridade do sistema operacional hospedeiro sem a necessidade de virtualização pesada (VirtualBox/VMware).

**Tecnologias e Ferramentas**

* Sistema Operacional: Kali Linux (via WSL2).

* Alvo (Target): Servidor FTP local (vsftpd) rodando em 127.0.0.1.

* Gerador de Wordlist: CUPP (Common User Passwords Profiler).

* Ferramenta de Ataque: Medusa v2.2 (Módulo FTP).

 **Metodologia**
1. Configuração do Ambiente
Instalação do serviço de FTP para servir como alvo das tentativas de login:


**sudo apt update && sudo apt install vsftpd -y**
**sudo service vsftpd start**


2. Criação da Wordlist Personalizada
Utilização do CUPP para gerar um dicionário inteligente baseado em perfis (nomes, datas e apelidos):


**sudo apt install cupp -y**
**cupp -i**

**Resultado:** Uma lista com centenas de combinações lógicas de senhas.


3. Execução do Ataque

Utilização do Medusa para testar a wordlist contra o usuário local:

**medusa -u [usuario] -P [lista.txt] -h 127.0.0.1 -M ftp -O resultado.txt**


**Resultados Obtidos**

* Tentativas Processadas: A varredura foi acompanhada até atingir 916 resultados.

* Análise técnica: O Medusa demonstrou alta performance, processando as tentativas sem bloqueios por parte do serviço.

* Interrupção voluntária: O teste foi pausado após validar que o serviço aceitava requisições massivas, confirmando a vulnerabilidade de "falta de limite de tentativas".

**Medidas de Mitigação**

Para proteger sistemas contra o que foi demonstrado neste projeto, recomenda-se:

* Rate Limiting: Implementar bloqueios automáticos após X tentativas falhas (ex: Fail2Ban).

* Senhas Fortes: Evitar o uso de dados pessoais que ferramentas como o CUPP conseguem prever.

* Monitoramento de Logs: Analisar picos de tentativas de login falhas nos logs do sistema.


**AVISO:** Teste feito apenas para fins didáticos.
