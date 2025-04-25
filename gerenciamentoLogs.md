# PASSO A PASSO: Configuração do Servidor de Log com rsyslog
1. Designar o servidor de log
Escolha um dos 3 PCs como o servidor de log (ex: PC1).

2. Configuração no Servidor (PC1)
a. Instale o rsyslog (caso não esteja instalado):
bash
Copiar
Editar
sudo apt update
sudo apt install rsyslog
b. Edite o arquivo de configuração:
bash
Copiar
Editar
sudo nano /etc/rsyslog.conf
Descomente ou adicione as seguintes linhas para ativar o recebimento de logs por UDP e TCP:

lua
Copiar
Editar
module(load="imudp")
input(type="imudp" port="514")

module(load="imtcp")
input(type="imtcp" port="514")
c. Permitir o acesso no firewall (se aplicável):
bash
Copiar
Editar
sudo ufw allow 514/udp
sudo ufw allow 514/tcp
d. Reinicie o rsyslog:
bash
Copiar
Editar
sudo systemctl restart rsyslog
3. Configuração nos Clientes (PC2, PC3)
a. Instale o rsyslog:
bash
Copiar
Editar
sudo apt update
sudo apt install rsyslog
b. Edite o arquivo de configuração de logs remotos:
bash
Copiar
Editar
sudo nano /etc/rsyslog.d/50-default.conf
Adicione a seguinte linha ao final do arquivo (substitua IP_DO_SERVIDOR pelo IP do PC1):

ruby
Copiar
Editar
*.* @IP_DO_SERVIDOR:514       # Envia logs via UDP
# *.* @@IP_DO_SERVIDOR:514    # Use @@ se quiser TCP
c. Reinicie o rsyslog:
bash
Copiar
Editar
sudo systemctl restart rsyslog
4. Verificar o Funcionamento
No servidor (PC1), verifique se os logs estão chegando:

bash
Copiar
Editar
sudo tail -f /var/log/syslog
Você deve ver entradas com os nomes dos clientes (PC2 e PC3).

5. Depuração (se necessário)
Verifique se os serviços rsyslog estão ativos:

bash
Copiar
Editar
sudo systemctl status rsyslog
Confirme se o servidor está escutando na porta 514:

bash
Copiar
Editar
sudo netstat -an | grep 514
Verifique o firewall em todos os PCs.

📄 EXEMPLO DE DOCUMENTAÇÃO (arquivo .md ou .txt)
markdown
Copiar
Editar
# Projeto RedesBR - Centralização de Logs com rsyslog

## Objetivo
Centralizar os arquivos de log de três computadores em um único servidor para facilitar o monitoramento da rede da empresa fictícia RedesBR.

---

## Equipamentos Utilizados
- PC1: Servidor de log (rsyslog)
- PC2 e PC3: Clientes que enviam logs para o servidor

---

## Configuração do Servidor (PC1)

1. Instalação:
sudo apt update sudo apt install rsyslog

markdown
Copiar
Editar

2. Habilitação de escuta em UDP e TCP:
- Arquivo: `/etc/rsyslog.conf`
module(load="imudp") input(type="imudp" port="514") module(load="imtcp") input(type="imtcp" port="514")

markdown
Copiar
Editar

3. Firewall:
sudo ufw allow 514/udp sudo ufw allow 514/tcp

markdown
Copiar
Editar

4. Reiniciar serviço:
sudo systemctl restart rsyslog

yaml
Copiar
Editar

---

## Configuração dos Clientes (PC2, PC3)

1. Instalação:
sudo apt install rsyslog

markdown
Copiar
Editar

2. Envio de logs para o servidor:
- Arquivo: `/etc/rsyslog.d/50-default.conf`
. @192.168.0.10:514

markdown
Copiar
Editar

3. Reiniciar serviço:
sudo systemctl restart rsyslog

yaml
Copiar
Editar

---

## Teste e Verificação

- Comando no servidor:
sudo tail -f /var/log/syslog

yaml
Copiar
Editar
- Resultado esperado:
Logs enviados dos clientes aparecendo no terminal com os respectivos nomes.

---

## Conclusão

A configuração foi realizada com sucesso. Os logs dos computadores clientes estão sendo recebidos pelo servidor central, facilitando o monitoramento e auditoria da rede.

---

## Alunos:
- Nome 1
- Nome 2
- Nome 3
