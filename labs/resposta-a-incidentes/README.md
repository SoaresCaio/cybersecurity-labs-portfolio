# Lab: Resposta a Incidentes com pfSense, Snort e QRadar

## Resumo

Este laboratório documenta a construção de um ambiente virtualizado para práticas defensivas de resposta a incidentes. O cenário integra firewall, IDS/IPS, SIEM e hosts Windows e Linux no Oracle VirtualBox.

O objetivo foi gerar telemetria, encaminhar logs, acompanhar atividade de rede e validar alertas de segurança para tráfego ICMP, HTTP e DNS.

## Tecnologias

- Oracle VirtualBox
- pfSense Community Edition
- Snort IDS/IPS
- IBM QRadar Community Edition
- Ubuntu e rsyslog
- Kali Linux
- Windows Server

## Atividades realizadas

- Criação e organização das máquinas virtuais
- Configuração das interfaces de rede do pfSense
- Validação da conectividade entre os ativos
- Encaminhamento de logs Linux ao QRadar com rsyslog
- Consulta de eventos e fluxos no QRadar
- Configuração do Snort na interface LAN
- Criação e teste de regras para ICMP, HTTP e DNS

## Evidências

As capturas abaixo são exibidas individualmente, preservando a proporção e a resolução de cada arquivo. Clique em uma imagem para visualizá-la em tamanho completo.

### 1. Inventário de máquinas virtuais

[![Inventário de máquinas virtuais no VirtualBox](evidencias/figura-01-inventario-virtualbox.jpg)](evidencias/figura-01-inventario-virtualbox.jpg)

### 2. Adaptador WAN do pfSense

[![Adaptador WAN do pfSense](evidencias/figura-02-pfsense-adapter-wan.png)](evidencias/figura-02-pfsense-adapter-wan.png)

### 3. Adaptador LAN do pfSense

[![Adaptador LAN do pfSense](evidencias/figura-03-pfsense-adapter-lan.png)](evidencias/figura-03-pfsense-adapter-lan.png)

### 4. Estado das interfaces do pfSense

[![Estado das interfaces do pfSense](evidencias/figura-04-pfsense-interface-status.png)](evidencias/figura-04-pfsense-interface-status.png)

### 5. Configuração do Windows Server

[![Configuração de rede do Windows Server](evidencias/figura-05-windows-server-ip.png)](evidencias/figura-05-windows-server-ip.png)

### 6. Informações do Ubuntu

[![Informações da máquina Ubuntu](evidencias/figura-06-ubuntu-machine-info.png)](evidencias/figura-06-ubuntu-machine-info.png)

### 7. Informações do Kali Linux

[![Informações da máquina Kali Linux](evidencias/figura-07-kali-machine-info.png)](evidencias/figura-07-kali-machine-info.png)

### 8. Teste de conectividade com o pfSense

[![Teste de conectividade do Kali com o pfSense](evidencias/figura-08-kali-ping-pfsense.png)](evidencias/figura-08-kali-ping-pfsense.png)

### 9. Terminal do QRadar

[![Terminal do QRadar Community Edition](evidencias/figura-09-qradar-terminal.png)](evidencias/figura-09-qradar-terminal.png)

### 10. Dashboard do QRadar

[![Dashboard do QRadar](evidencias/figura-10-qradar-dashboard.png)](evidencias/figura-10-qradar-dashboard.png)

### 11. Configuração do rsyslog

[![Arquivo de configuração do rsyslog](evidencias/figura-11-ubuntu-rsyslog-conf.png)](evidencias/figura-11-ubuntu-rsyslog-conf.png)

### 12. Serviço rsyslog em execução

[![Status do serviço rsyslog](evidencias/figura-12-ubuntu-rsyslog-running.png)](evidencias/figura-12-ubuntu-rsyslog-running.png)

### 13. Log Activity no QRadar

[![Log Activity no QRadar](evidencias/figura-13-qradar-log-activity.png)](evidencias/figura-13-qradar-log-activity.png)

### 14. Detalhes de evento no QRadar

[![Detalhes de evento no QRadar](evidencias/figura-14-qradar-log-detail.png)](evidencias/figura-14-qradar-log-detail.png)

### 15. Network Activity no QRadar

[![Network Activity no QRadar](evidencias/figura-15-qradar-network-activity.png)](evidencias/figura-15-qradar-network-activity.png)

### 16. Snort habilitado na interface LAN

[![Snort habilitado na interface LAN](evidencias/figura-16-snort-lan-interface.png)](evidencias/figura-16-snort-lan-interface.png)

### 17. Regras customizadas do Snort

[![Regras customizadas do Snort](evidencias/figura-17-snort-lan-rules.png)](evidencias/figura-17-snort-lan-rules.png)

### 18. Alerta ICMP no Snort

[![Alerta ICMP no Snort](evidencias/figura-18-snort-ping-alert.png)](evidencias/figura-18-snort-ping-alert.png)

### 19. Alerta HTTP no Snort

[![Alerta HTTP no Snort](evidencias/figura-19-snort-http-alert.png)](evidencias/figura-19-snort-http-alert.png)

### 20. Alerta DNS no Snort

[![Alerta DNS no Snort](evidencias/figura-20-snort-dns-alert.png)](evidencias/figura-20-snort-dns-alert.png)

## Resultados

O ambiente permitiu acompanhar eventos Linux e atividade de rede no QRadar. As regras customizadas do Snort geraram alertas para os três tipos de tráfego testados, confirmando o funcionamento da cadeia de detecção.

## Nota ética

Todo o trabalho foi executado em ambiente controlado, com ativos próprios e finalidade acadêmica. As evidências são apresentadas para documentação técnica e desenvolvimento de competências defensivas.
