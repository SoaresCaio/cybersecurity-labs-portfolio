# Lab: Resposta a Incidentes com pfSense, Snort e QRadar

## Resumo

Este laboratório documenta a construção de um ambiente virtualizado para práticas defensivas de resposta a incidentes. O cenário integra firewall, IDS/IPS, SIEM e hosts Windows e Linux em uma infraestrutura controlada no Oracle VirtualBox.

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

- Criação e organização das máquinas virtuais do laboratório
- Configuração das interfaces de rede do pfSense
- Validação de conectividade entre os ativos
- Encaminhamento de logs Linux ao QRadar com rsyslog
- Consulta de eventos e fluxos no QRadar
- Configuração do Snort na interface LAN
- Criação e teste de regras para ICMP, HTTP e DNS

## Resultados

O ambiente permitiu acompanhar eventos Linux e atividade de rede no QRadar. As regras customizadas do Snort também geraram alertas para os três tipos de tráfego testados, confirmando a visibilidade do laboratório e o funcionamento da cadeia de detecção.

## Evidências

### Ambiente e ativos

![Ambiente virtualizado e ativos do laboratório](evidencias/galeria-01-ambiente-e-ativos.jpg)

### Hosts e conectividade

![Informações dos hosts e testes de conectividade](evidencias/galeria-02-hosts-e-conectividade.jpg)

### Coleta e análise no QRadar

![Configuração de logs e análise no QRadar](evidencias/galeria-03-qradar.jpg)

### Detecções com Snort

![Configuração e alertas do Snort](evidencias/galeria-04-snort-deteccoes.jpg)

## Nota ética

Todo o trabalho foi executado em ambiente controlado, com ativos próprios e finalidade acadêmica. As evidências são apresentadas para documentação técnica e desenvolvimento de competências defensivas.
