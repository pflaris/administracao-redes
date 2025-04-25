# Atividade  - Centralização de Logs com rsyslog

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

2. Habilitação de escuta em UDP e TCP:
- Arquivo: `/etc/rsyslog.conf`
- remover os comentários:
  `module(load="imudp")`
  `input(type="imudp" port="514")`
  `module(load="imtcp")`
  `input(type="imtcp" port="514")`

3. Firewall:
`sudo ufw allow 514/udp`
`sudo ufw allow 514/tcp`

5. Reiniciar serviço:
`sudo systemctl restart rsyslog`

---

## Configuração dos Clientes (PC2, PC3)

1. Instalação:
`sudo apt install rsyslog`

2. Envio de logs para o servidor:
- Arquivo: `/etc/rsyslog.d/50-default.conf`
. @192.168.0.10:514

3. Reiniciar serviço:
`sudo systemctl restart rsyslog`

---

## Teste e Verificação

- Comando no servidor:
`sudo tail -f /var/log/syslog`

- Resultado esperado/obtido:
Logs enviados dos clientes aparecendo no terminal com os respectivos nomes.

---

A configuração foi concluída com sucesso. 
Os logs dos computadores clientes estão sendo recebidos pelo servidor central, facilitando o monitoramento e auditoria da rede.

---

## Alunos:
- Larissa
- Lucas Leonardo
