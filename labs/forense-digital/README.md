# Lab: Investigação Forense Digital - Caso TechSecure

## Resumo

Este laboratório documenta uma investigação forense realizada em um ambiente acadêmico controlado da organização fictícia TechSecure. O cenário envolve acesso a informações confidenciais, uso de dispositivo removível, apagamento de arquivo compactado e comunicação com um serviço externo.

A análise correlaciona evidências de sistema de arquivos, logs do Windows, vestígios de dispositivos USB e tráfego de rede. Os achados sustentam a hipótese de preparação e possível exfiltração de dados, preservando os limites técnicos das evidências disponíveis.

## Objetivos

- Preservar e analisar evidências digitais de forma reproduzível
- Validar a integridade da imagem forense com funções hash
- Identificar arquivos apagados e recuperar conteúdo relevante
- Examinar eventos de auditoria do Windows
- Correlacionar artefatos de dispositivos USB
- Analisar comunicações externas em uma captura de rede
- Avaliar os achados em relação à política interna simulada
- Separar fatos comprovados de hipóteses e inferências

## Ferramentas

- FTK Imager
- HxD
- Autopsy
- Visualizador de Eventos do Windows
- Wireshark
- Foremost
- WinRAR

## Fluxo da investigação

1. Aquisição e preservação da imagem forense em uma cópia de trabalho
2. Validação de integridade por hashes e cálculo independente de SHA-256
3. Ingestão da imagem no Autopsy e análise da estrutura NTFS
4. Identificação e recuperação do arquivo compactado apagado
5. Análise dos eventos de auditoria do Windows relacionados ao arquivo
6. Correlação dos artefatos USBSTOR e setupapi.dev.log
7. Análise temporal das comunicações externas presentes na captura PCAP
8. Tentativa de recuperação por data carving e validação funcional dos resultados
9. Consolidação dos achados, limitações e nível de confiança da conclusão

## Metodologia

### 1. Preservação e integridade

A investigação foi conduzida sobre uma imagem forense segmentada, mantendo a fonte original fora do processo de análise. Valores MD5 e SHA-1 foram verificados durante a aquisição, e um SHA-256 independente foi calculado para reforçar a rastreabilidade do material examinado.

### 2. Análise do sistema de arquivos

O Autopsy foi utilizado para examinar a estrutura NTFS, metadados e arquivos não alocados. Um contêiner RAR relacionado ao cenário foi identificado como apagado, exportado e validado em uma cópia de trabalho.

### 3. Eventos do Windows

Os registros EVTX foram filtrados pelo evento 4663. A análise permitiu associar a conta e o processo registrados ao uso de um direito de acesso sobre o arquivo investigado. Esse evento foi tratado como evidência de acesso ao objeto, sem extrapolar seu significado para afirmar leitura integral ou transferência.

### 4. Vestígios de dispositivo removível

Artefatos do Registro e do setupapi.dev.log foram correlacionados para comprovar o reconhecimento de um dispositivo de armazenamento USB. Como o pendrive físico não foi adquirido, a presença do arquivo específico na mídia não pôde ser comprovada apenas por esses vestígios.

### 5. Tráfego de rede

A captura PCAP foi examinada no Wireshark. Foi identificada uma sessão HTTPS com infraestrutura externa poucos minutos após os eventos de acesso. A criptografia TLS permitiu confirmar a comunicação e seus volumes, mas não o nome ou conteúdo do arquivo transmitido.

### 6. Recuperação e validação

O Foremost foi executado com assinatura RAR5 e produziu candidatos que não passaram pela validação funcional. A recuperação válida ocorreu pelo Autopsy, com base nas estruturas remanescentes do sistema de arquivos. A cópia exportada foi testada e aberta com sucesso.

## Principais achados

| Achado | Avaliação |
| --- | --- |
| Preparação de arquivo compactado | Comprovada no ambiente simulado |
| Acesso ao objeto | Comprovado quanto ao direito registrado no evento 4663 |
| Apagamento do RAR | Comprovado por nome e metadados não alocados |
| Reconhecimento de dispositivo USB | Comprovado por Registro e log de instalação |
| Comunicação HTTPS externa | Comprovada pela captura de rede |
| Transferência do RAR para USB ou serviço externo | Não comprovada diretamente |
| Possível exfiltração | Provável, com confiança moderada |
| Violação da política simulada | Compatível com as premissas do cenário |

## Limitações

- A evidência correspondia a um volume preparado para o laboratório, não ao disco integral do sistema
- O dispositivo USB físico não foi submetido a aquisição forense
- O conteúdo da sessão de rede estava protegido por TLS
- Não havia logs do serviço externo para confirmação independente
- A memória RAM não integrou o escopo da análise
- Os candidatos recuperados por data carving não foram validados como arquivos íntegros

## Evidências analisadas

As capturas abaixo registram as etapas da investigação. Cada item descreve o procedimento executado, o achado observado e o alcance probatório da evidência. Clique em qualquer imagem para abri-la no tamanho original.

### 1. Validação SHA-256 da imagem forense

**Procedimento:** a imagem TechSecure_Evidencia.001 foi aberta no HxD e submetida ao cálculo de SHA-256.

**Observação:** o hash obtido foi registrado para identificar de forma única a cópia examinada.

**Interpretação:** a verificação permite detectar alterações posteriores e sustenta a integridade da evidência durante a análise.

[![Validação SHA-256 da imagem forense](evidencias/figura-01-sha256-imagem-forense.png)](evidencias/figura-01-sha256-imagem-forense.png)

### 2. Estrutura da evidência e recuperação no Autopsy

**Procedimento:** a imagem foi adicionada ao Autopsy, que reconstruiu os volumes e a estrutura NTFS. O diretório TechSecure/Arquivos foi examinado e o RAR apagado foi exportado para uma cópia de trabalho.

**Observação:** o arquivo Clientes_Confidenciais.rar aparece ao lado de outros documentos relacionados ao cenário e pôde ser aberto após a exportação.

**Interpretação:** a recuperação pelo sistema de arquivos foi bem-sucedida porque ainda existiam metadados suficientes para localizar o conteúdo.

[![Estrutura da evidência e recuperação no Autopsy](evidencias/figura-02-autopsy-estrutura-e-recuperacao.png)](evidencias/figura-02-autopsy-estrutura-e-recuperacao.png)

### 3. Metadados do arquivo apagado

**Procedimento:** os metadados do RAR foram inspecionados no Autopsy.

**Observação:** o nome do arquivo, o caminho, o tamanho e os horários permanecem visíveis, enquanto File Name Allocation e Metadata Allocation aparecem como Unallocated.

**Interpretação:** o registro comprova que o arquivo foi apagado do sistema de arquivos. A exclusão lógica, porém, não implica destruição imediata dos dados.

[![Metadados do arquivo apagado no Autopsy](evidencias/figura-03-autopsy-rar-apagado.png)](evidencias/figura-03-autopsy-rar-apagado.png)

### 4. Evento 4663 relacionado ao arquivo

**Procedimento:** o log de Segurança do Windows foi filtrado pelo evento 4663 e pelo caminho do arquivo investigado.

**Observação:** o evento registra a conta do cenário, o processo explorer.exe, o caminho E:\TechSecure\Arquivos\Clientes_Confidenciais.rar e o direito READ_CONTROL.

**Interpretação:** o evento demonstra uma operação de acesso ao objeto. Isoladamente, READ_CONTROL não prova que todo o conteúdo foi lido, copiado ou transferido.

[![Evento 4663 relacionado ao arquivo investigado](evidencias/figura-04-event-viewer-evento-4663.png)](evidencias/figura-04-event-viewer-evento-4663.png)

### 5. Sequência de eventos de auditoria

**Procedimento:** os eventos 4663 foram organizados por data e hora para identificar repetição e proximidade temporal.

**Observação:** há múltiplos registros do mesmo tipo em uma janela curta, incluindo ocorrências associadas ao arquivo compactado.

**Interpretação:** a sequência reforça que houve atividade sobre o objeto e fornece uma linha do tempo para correlação com USB e rede, mas não identifica por si só o destino dos dados.

[![Registros de auditoria filtrados pelo evento 4663](evidencias/figura-05-event-viewer-eventos-4663.png)](evidencias/figura-05-event-viewer-eventos-4663.png)

### 6. Detalhes do acesso ao arquivo compactado

**Procedimento:** um evento individual foi aberto para conferir os campos de sujeito, objeto, processo e máscara de acesso.

**Observação:** o caminho do RAR e o processo explorer.exe aparecem no registro, com acesso READ_CONTROL e resultado de auditoria bem-sucedida.

**Interpretação:** os campos permitem atribuir a operação registrada ao contexto de usuário e processo do cenário. Eles não registram o conteúdo manipulado nem uma eventual cópia.

[![Detalhes do acesso ao arquivo compactado](evidencias/figura-06-event-viewer-detalhe-rar.png)](evidencias/figura-06-event-viewer-detalhe-rar.png)

### 7. Artefato USBSTOR

**Procedimento:** o arquivo de Registro USBSTOR foi localizado e analisado no Autopsy.

**Observação:** o artefato contém a identificação de um dispositivo SanDisk Cruzer Blade reconhecido pelo Windows.

**Interpretação:** o registro comprova que o sistema enumerou esse dispositivo removível. Sem a aquisição da mídia, não é possível demonstrar que o RAR investigado foi gravado nela.

[![Artefato USBSTOR analisado no Autopsy](evidencias/figura-07-autopsy-usbstor.png)](evidencias/figura-07-autopsy-usbstor.png)

### 8. Registro de instalação do dispositivo

**Procedimento:** o setupapi.dev.log foi examinado em busca de entradas relacionadas a armazenamento USB.

**Observação:** o log contém referências ao identificador do dispositivo e ao driver de disco instalado pelo sistema.

**Interpretação:** esse achado corrobora o USBSTOR por uma fonte independente e confirma o reconhecimento técnico do dispositivo pelo Windows.

[![Registro de instalação do dispositivo em setupapi.dev.log](evidencias/figura-08-autopsy-setupapi.png)](evidencias/figura-08-autopsy-setupapi.png)

### 9. Conversas de rede no Wireshark

**Procedimento:** a captura PCAP foi filtrada e as conversas TCP foram ordenadas para examinar destinos, portas, horários e volumes.

**Observação:** a estação analisada estabeleceu sessões HTTPS externas, incluindo uma conversa de maior volume temporalmente próxima aos eventos de acesso.

**Interpretação:** a captura confirma comunicação externa criptografada. Como o conteúdo está protegido por TLS, não é possível identificar o arquivo transmitido ou afirmar que ocorreu exfiltração apenas com o PCAP.

[![Análise de conversas TCP no Wireshark](evidencias/figura-09-wireshark-conversas.png)](evidencias/figura-09-wireshark-conversas.png)

### 10. Política de segurança simulada

**Procedimento:** a política interna recuperada da imagem foi comparada com os comportamentos observados no cenário.

**Observação:** o documento proíbe dispositivos removíveis não autorizados e o envio de dados corporativos a serviços pessoais de nuvem.

**Interpretação:** os achados são compatíveis com uma possível violação de política. A conclusão disciplinar dependeria da confirmação da transferência e de outras fontes corporativas.

[![Política de segurança analisada no Autopsy](evidencias/figura-10-autopsy-politica-seguranca.png)](evidencias/figura-10-autopsy-politica-seguranca.png)

### 11. Tentativa de data carving com Foremost

**Procedimento:** o Foremost foi configurado com uma assinatura RAR5 e executado sobre a imagem forense.

**Observação:** a auditoria registra dois candidatos extraídos, mas os arquivos resultantes não passaram pela validação funcional.

**Interpretação:** a quantidade de arquivos recuperados por assinatura não equivale a arquivos válidos. Os candidatos foram tratados como falsos positivos ou fragmentos incompletos.

[![Resultado da tentativa de data carving com Foremost](evidencias/figura-11-foremost-audit.png)](evidencias/figura-11-foremost-audit.png)

### 12. Validação do arquivo recuperado

**Procedimento:** o RAR exportado pelo Autopsy foi aberto e submetido ao teste de integridade do WinRAR.

**Observação:** o conteúdo foi listado e o teste terminou sem erros.

**Interpretação:** o resultado confirma que a recuperação baseada nos metadados do sistema de arquivos produziu uma cópia funcional, ao contrário dos candidatos obtidos por carving.

[![Teste de integridade do arquivo recuperado](evidencias/figura-12-validacao-rar.png)](evidencias/figura-12-validacao-rar.png)

## Conclusão

A investigação confirmou a criação, o acesso e o apagamento de um arquivo compactado com informações sensíveis no cenário acadêmico. Também confirmou o reconhecimento de um dispositivo USB e uma comunicação HTTPS externa temporalmente próxima.

As evidências não permitem afirmar de forma conclusiva que o mesmo arquivo foi gravado no pendrive ou transmitido pela sessão criptografada. A conclusão tecnicamente adequada é de possível exfiltração, com confiança moderada, respeitando as limitações da aquisição e da análise.

## Nota ética

Este caso utiliza uma organização fictícia e dados produzidos exclusivamente para fins acadêmicos. O laboratório foi executado em ambiente controlado para desenvolver competências de preservação, análise e comunicação de evidências digitais.
