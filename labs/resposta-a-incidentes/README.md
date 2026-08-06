# Lab: Resposta a Incidentes com pfSense, Snort e QRadar

## Resumo

Este laboratório documenta a construção e a validação de um ambiente virtualizado para práticas defensivas de monitoramento e resposta a incidentes. O cenário integra segmentação de rede, firewall, IDS/IPS, SIEM e hosts Windows e Linux no Oracle VirtualBox.

O trabalho percorre toda a cadeia de detecção: configuração da rede, geração de telemetria, encaminhamento de logs, coleta de eventos e fluxos, criação de regras no Snort e validação de alertas para tráfego ICMP, HTTP e DNS.

## Objetivos

- Montar uma arquitetura virtualizada para testes defensivos
- Separar as interfaces WAN e LAN por meio do pfSense
- Validar a comunicação entre os ativos do laboratório
- Encaminhar logs do Ubuntu ao QRadar com rsyslog
- Confirmar a ingestão de eventos e fluxos no SIEM
- Ativar o Snort na interface LAN do pfSense
- Criar e testar regras para ICMP, HTTP e DNS
- Documentar resultados, limitações e recomendações

## Tecnologias

- Oracle VirtualBox
- pfSense Community Edition
- Snort IDS/IPS
- IBM QRadar Community Edition
- Ubuntu e rsyslog
- Kali Linux
- Windows Server

## Arquitetura do laboratório

| Componente | Função |
| --- | --- |
| pfSense | Roteamento, separação entre WAN e LAN e execução do Snort |
| Snort | Detecção e bloqueio experimental de ICMP, HTTP e DNS |
| QRadar Community Edition | Centralização de eventos, visualização de logs e atividade de rede |
| Ubuntu | Fonte de logs Linux encaminhados por rsyslog |
| Kali Linux | Host da LAN utilizado para testes de conectividade e geração de tráfego |
| Windows Server | Ativo adicional do cenário virtualizado |
| VirtualBox | Plataforma de virtualização e configuração dos adaptadores de rede |

O pfSense utiliza dois adaptadores: uma interface ligada à rede externa do laboratório e uma interface Host-Only dedicada à LAN virtual. Essa separação permite observar o tráfego interno e aplicar as regras do Snort na interface LAN.

## Fluxo de implementação e validação

1. Criação das máquinas virtuais e configuração dos adaptadores
2. Inicialização do pfSense e conferência dos endereços WAN e LAN
3. Validação dos endereços dos hosts e teste de conectividade com o gateway
4. Inicialização do QRadar e conferência do dashboard
5. Configuração e verificação do serviço rsyslog no Ubuntu
6. Confirmação da chegada de eventos Linux no Log Activity do QRadar
7. Análise dos fluxos do Ubuntu no Network Activity
8. Ativação do Snort na interface LAN
9. Criação de regras customizadas para ICMP, HTTP e DNS
10. Geração de tráfego de teste e validação dos alertas no pfSense

## Resultados

| Etapa | Resultado observado |
| --- | --- |
| Segmentação virtual | Interfaces WAN e LAN configuradas no pfSense |
| Conectividade | Kali alcançou o gateway da LAN |
| Coleta de logs | Eventos Linux recebidos e detalhados no QRadar |
| Visibilidade de rede | Fluxos do Ubuntu exibidos no Network Activity |
| IDS/IPS | Snort ativo na interface LAN |
| Teste ICMP | Alertas gerados para tráfego de ping |
| Teste HTTP | Alertas gerados para conexões na porta 80 |
| Teste DNS | Alertas gerados para consultas UDP na porta 53 |

## Limitações

- As regras customizadas são amplas e adequadas apenas ao ambiente acadêmico
- Regras baseadas somente em protocolo e porta podem gerar falsos positivos
- O laboratório valida detecção e telemetria, mas não representa uma operação completa de SOC
- Não foram avaliados alta disponibilidade, retenção prolongada ou grande volume de eventos
- A classificação de um alerta depende da regra e da mensagem configurada; o texto do alerta não substitui a análise do pacote
- Os endereços exibidos pertencem exclusivamente à rede controlada do laboratório

## Recomendações

- Restringir regras do Snort às redes, direções e serviços realmente necessários
- Padronizar mensagens e SIDs para facilitar a investigação
- Separar regras de alerta das ações de bloqueio durante a fase de testes
- Criar casos de uso e correlações no SIEM para reduzir ruído
- Sincronizar o horário de todos os ativos para preservar a linha do tempo
- Definir retenção, severidade e procedimento de escalonamento para os eventos
- Documentar uma linha de base de tráfego antes de ativar bloqueios em produção

## Evidências analisadas

Cada captura abaixo representa uma etapa da implantação ou da validação. O texto informa o procedimento realizado e o significado técnico do resultado. Clique em uma imagem para abri-la no tamanho original.

### 1. Inventário das máquinas virtuais

**Procedimento:** as VMs do cenário foram organizadas no VirtualBox antes da configuração da rede.

**Resultado e interpretação:** a captura registra Kali Linux, Ubuntu, QRadar, Windows Server e pfSense no mesmo ambiente de testes, estabelecendo o inventário dos ativos utilizados.

[![Inventário de máquinas virtuais no VirtualBox](evidencias/figura-01-inventario-virtualbox.jpg)](evidencias/figura-01-inventario-virtualbox.jpg)

### 2. Adaptador WAN do pfSense

**Procedimento:** o primeiro adaptador do pfSense foi ligado à interface externa do host em modo bridge.

**Resultado e interpretação:** essa interface fornece conectividade à rede externa do laboratório e atua como lado WAN do firewall.

[![Adaptador WAN do pfSense](evidencias/figura-02-pfsense-adapter-wan.png)](evidencias/figura-02-pfsense-adapter-wan.png)

### 3. Adaptador LAN do pfSense

**Procedimento:** o segundo adaptador foi configurado como Host-Only.

**Resultado e interpretação:** a LAN fica isolada em uma rede virtual controlada, permitindo que os testes atravessem o gateway pfSense sem expor diretamente os hosts à rede externa.

[![Adaptador LAN do pfSense](evidencias/figura-03-pfsense-adapter-lan.png)](evidencias/figura-03-pfsense-adapter-lan.png)

### 4. Estado das interfaces do pfSense

**Procedimento:** após a inicialização, o console foi consultado para conferir o mapeamento e os endereços das interfaces.

**Resultado e interpretação:** WAN e LAN aparecem ativas com endereços distintos, confirmando a separação lógica necessária para roteamento e inspeção.

[![Estado das interfaces do pfSense](evidencias/figura-04-pfsense-interface-status.png)](evidencias/figura-04-pfsense-interface-status.png)

### 5. Configuração do Windows Server

**Procedimento:** o comando ipconfig foi executado no Windows Server.

**Resultado e interpretação:** a captura registra endereço IPv4, máscara e gateway do host, permitindo validar sua posição no cenário e usar esses dados em correlações futuras.

[![Configuração de rede do Windows Server](evidencias/figura-05-windows-server-ip.png)](evidencias/figura-05-windows-server-ip.png)

### 6. Informações do Ubuntu

**Procedimento:** hostnamectl e ifconfig foram utilizados para registrar sistema operacional e interfaces de rede.

**Resultado e interpretação:** o Ubuntu aparece com interfaces nas redes do laboratório, funcionando como fonte de telemetria Linux e atividade de rede para o QRadar.

[![Informações da máquina Ubuntu](evidencias/figura-06-ubuntu-machine-info.png)](evidencias/figura-06-ubuntu-machine-info.png)

### 7. Informações do Kali Linux

**Procedimento:** os mesmos dados de sistema e rede foram coletados no Kali.

**Resultado e interpretação:** a interface do host está na LAN do pfSense, identificando o equipamento usado nos testes controlados de conectividade e geração de tráfego.

[![Informações da máquina Kali Linux](evidencias/figura-07-kali-machine-info.png)](evidencias/figura-07-kali-machine-info.png)

### 8. Teste de conectividade com o gateway

**Procedimento:** o Kali enviou requisições ICMP ao endereço LAN do pfSense.

**Resultado e interpretação:** as respostas, sem perda aparente na captura, confirmam conectividade básica entre o host de teste e o firewall.

[![Teste de conectividade do Kali com o pfSense](evidencias/figura-08-kali-ping-pfsense.png)](evidencias/figura-08-kali-ping-pfsense.png)

### 9. Terminal do QRadar

**Procedimento:** o appliance QRadar Community Edition foi inicializado e acessado pelo console.

**Resultado e interpretação:** a tela confirma a versão instalada e o funcionamento do sistema-base que hospeda o SIEM.

[![Terminal do QRadar Community Edition](evidencias/figura-09-qradar-terminal.png)](evidencias/figura-09-qradar-terminal.png)

### 10. Dashboard do QRadar

**Procedimento:** a interface web foi acessada para verificar saúde, taxa de eventos e taxa de fluxos.

**Resultado e interpretação:** os gráficos apresentam eventos e flows sendo processados, confirmando que os componentes centrais do QRadar estavam operacionais.

[![Dashboard do QRadar](evidencias/figura-10-qradar-dashboard.png)](evidencias/figura-10-qradar-dashboard.png)

### 11. Encaminhamento de logs com rsyslog

**Procedimento:** o arquivo de configuração do rsyslog foi ajustado para encaminhar mensagens do Ubuntu ao endereço do QRadar na porta 514.

**Resultado e interpretação:** a configuração estabelece o caminho de envio da telemetria Linux. Em um ambiente de produção, deve-se escolher conscientemente entre UDP e TCP e evitar duplicidade de encaminhamento.

[![Arquivo de configuração do rsyslog](evidencias/figura-11-ubuntu-rsyslog-conf.png)](evidencias/figura-11-ubuntu-rsyslog-conf.png)

### 12. Serviço rsyslog em execução

**Procedimento:** o estado do serviço foi verificado após a alteração da configuração.

**Resultado e interpretação:** o status active (running) confirma que o daemon estava ativo; a entrega efetiva dos eventos é validada nas capturas seguintes do QRadar.

[![Status do serviço rsyslog](evidencias/figura-12-ubuntu-rsyslog-running.png)](evidencias/figura-12-ubuntu-rsyslog-running.png)

### 13. Eventos Linux no Log Activity

**Procedimento:** o Log Activity foi acompanhado em tempo real e filtrado pela fonte Linux.

**Resultado e interpretação:** eventos do Ubuntu aparecem no SIEM com endereço de origem e magnitude, confirmando ingestão e normalização básica dos registros.

[![Log Activity no QRadar](evidencias/figura-13-qradar-log-activity.png)](evidencias/figura-13-qradar-log-activity.png)

### 14. Detalhamento de um evento

**Procedimento:** um evento Linux foi aberto para examinar campos normalizados e payload.

**Resultado e interpretação:** a tela mostra nome, categoria, horários, origem, destino e mensagem original, demonstrando como o analista pode sair da visão agregada para investigar um registro específico.

[![Detalhes de evento no QRadar](evidencias/figura-14-qradar-log-detail.png)](evidencias/figura-14-qradar-log-detail.png)

### 15. Atividade de rede no QRadar

**Procedimento:** o Network Activity foi filtrado pelo endereço do Ubuntu.

**Resultado e interpretação:** a lista apresenta destinos, portas, protocolos, bytes e pacotes, oferecendo visibilidade de fluxos mesmo quando o conteúdo da aplicação não está disponível.

[![Network Activity no QRadar](evidencias/figura-15-qradar-network-activity.png)](evidencias/figura-15-qradar-network-activity.png)

### 16. Snort habilitado na interface LAN

**Procedimento:** o pacote Snort foi associado e iniciado na interface LAN do pfSense.

**Resultado e interpretação:** o status ativo confirma que o IDS/IPS estava inspecionando a rede interna escolhida para os testes.

[![Snort habilitado na interface LAN](evidencias/figura-16-snort-lan-interface.png)](evidencias/figura-16-snort-lan-interface.png)

### 17. Regras customizadas do Snort

**Procedimento:** foram criadas regras acadêmicas para ICMP, TCP na porta 80 e UDP na porta 53, com ações de alerta e bloqueio.

**Resultado e interpretação:** as assinaturas definem os três casos de teste. Por serem abrangentes, servem para validação funcional e não devem ser aplicadas em produção sem restrição de rede, direção e contexto.

[![Regras customizadas do Snort](evidencias/figura-17-snort-lan-rules.png)](evidencias/figura-17-snort-lan-rules.png)

### 18. Validação do alerta ICMP

**Procedimento:** foi gerado tráfego de ping entre ativos do laboratório.

**Resultado e interpretação:** o painel de alertas registra PING ICMP NA REDE, demonstrando que o Snort reconheceu o protocolo e acionou a regra correspondente.

[![Alerta ICMP no Snort](evidencias/figura-18-snort-ping-alert.png)](evidencias/figura-18-snort-ping-alert.png)

### 19. Validação do alerta HTTP

**Procedimento:** foi iniciada uma conexão TCP de teste destinada à porta 80.

**Resultado e interpretação:** o alerta REQUISICAO HTTP aparece no painel, confirmando o disparo da regra. A porta 80, isoladamente, não garante que o conteúdo seja HTTP válido, por isso regras reais devem incluir mais contexto.

[![Alerta HTTP no Snort](evidencias/figura-19-snort-http-alert.png)](evidencias/figura-19-snort-http-alert.png)

### 20. Validação do alerta DNS

**Procedimento:** foram realizadas consultas DNS para gerar tráfego UDP destinado à porta 53.

**Resultado e interpretação:** o painel registra ALERTA DE DNS com origem e destino, concluindo a validação dos três tipos de tráfego previstos.

[![Alerta DNS no Snort](evidencias/figura-20-snort-dns-alert.png)](evidencias/figura-20-snort-dns-alert.png)

## Conclusão

O laboratório confirmou o funcionamento da cadeia de visibilidade e detecção: os ativos comunicaram-se pela arquitetura virtual, o Ubuntu encaminhou logs ao QRadar, os eventos e fluxos puderam ser investigados no SIEM e o Snort gerou alertas para os três casos de teste.

O resultado comprova a integração funcional dos componentes no ambiente acadêmico. A evolução natural para um cenário mais realista seria refinar as regras, criar correlações no QRadar, definir severidades e documentar procedimentos de triagem e contenção.

## Nota ética

Todo o trabalho foi executado em ambiente controlado, com ativos próprios e finalidade acadêmica. As evidências são apresentadas para documentação técnica e desenvolvimento de competências defensivas.
