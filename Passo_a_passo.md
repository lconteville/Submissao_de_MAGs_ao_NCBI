# Submissão de MAGs ao NCBI

## Introdução
Este manual tem como objetivo oferecer uma visão prática sobre o processo de envio de MAGs ao NCBI e reflete exclusivamente a minha experiência pessoal. Portanto, o mesmo deve ser adaptado para atender as necessidades específicas de cada caso.

## Passo 1: Acesse o portal do NCBI
- Registre-se ou faça login em [https://www.ncbi.nlm.nih.gov/](https://www.ncbi.nlm.nih.gov/).

## Passo 2: Inicie uma nova submissão de genomas
- Acesse o seguinte link: [https://submit.ncbi.nlm.nih.gov/about/genome/](https://submit.ncbi.nlm.nih.gov/about/genome/)
- Pressione o botão **Submit**.
- Na nova página, pressione o botão **New submission**.

## Passo 3: Selecione o tipo de submissão
- No meu caso, tínhamos quase 1000 MAGs a serem submetidos, então escolhi a opção para múltiplos genomas. No entanto, como o sistema aceita no máximo 400 genomas por submissão, precisei dividir o processo em 3 submissões.
- **How do you want to submit your data?**
  - [x] Batch/multiple genomes (maximum 400 per submission)
- Pressione o botão **Continue**.
  
## Passo 4: Preencha os formulários com as informações necessárias
### Submitter
  - Preencha os seus dados pessoais no formulário.
### General Information
  - **BioProject:** Insira o código do BioProject das reads utilizadas para montar os MAGs.
    - Exemplo: PRJNA987743
    - Nota: Caso o NCBI não reconheça o BioProject, verifique se há um espaço extra no final do código e remova-o.
  - **BioSamples:** Crie novos BioSamples para cada MAG:
    - Do you already have BioSample accession numbers for these samples?
      - No (BioSamples will be created within this submission)
  - **Data:** Adicione uma data futura para que esses MAGs sejam disponibilizados ao público.
  - **Título da Submissão:** Escolha um título que represente a submissão dos MAGs.
    - Exemplo: Metagenome-assembled genomes from the rumen and fecal microbiomes of Brazilian Nelore Cattle
### Gaps
  - Como usei o programa MEGAHIT para montar as reads pareadas em contigs, respondi as perguntas da seguinte forma: 
    - **Did you randomly merge the sequences into a single sequence (for example, maybe you just linked the sequences together by size without using an assembler program)?**
      - No
    - **Appropriate minimum number of Ns in a row (0-10) that represents a gap:**
      - 10
    - **Do any of the N's represents gaps of completely unknown size (the gap size was NOT estimated by an assembly program and a single value, eg 100, was used)?**
      - No, all gaps are of estimated size (even if a particular size was used for small gaps (eg, 10 N's))
    - **What type of evidence was used to assert linkage across the assembly gaps?**
      - Paired-ends: Paired sequences from the two ends of a DNA fragment, mate-pairs and molecular-barcoding.
### Genome Info
  - **Sample Type:** MIMAG Metagenome-assembled Genome
      - host-associated
  - **BioSample Attributes** 
    - Um exemplo da tabela que eu submeti nesse tópico pode ser encontrada [aqui](MIMAG.host-associated_exemplo.xlsx).
    - Os nomes dos organismos nessa tabela devem estar no nível taxonômico mais baixo possível e corresponder à taxonomia do NCBI.
    - Como utilizei o GTDB-tk para classificar os MAGs, as taxonomias que eu tinha eram baseadas no banco de dados GTDB. Portanto, **antes de registrar as BioSamples**, eu tive que enviar um e-mail para genomas@ncbi.nlm.nih.gov com essas taxonomias. O NCBI respondeu esse email com a informação taxonômica que deveria ser usada para cada MAG, seguindo a Taxonomia do NCBI. Detalhes de como enviar esse email estão no [manual](https://submit.ncbi.nlm.nih.gov/about/genome/).
  - **Genome Info**
    - Como os MAGs apresentavam mais de um contig, respondi as perguntas da seguinte forma: 
    - **All the genomes within a batch submission must be of the same type. Which type are these genomes?**
      - One or more chromosomes are in multiple pieces and/or some sequences are not assembled into chromosomes
    - **Are these submissions expected to represent the full genomes?**
      - Yes (even for draft genomes or if a prokaryotic genome assembly may not include plasmids)
    - **Is this expected to be the final version of these genomes?**
      - Yes
    - **Annotate this prokaryotic genome in the NCBI Prokaryotic Annotation Pipeline (PGAP) before its release?**
      - No
    - **[ ] Do not automatically trim or remove sequences identified as contamination**
      - Deixei desmarcado.
    - Um exemplo do Genome Metadata que eu submeti nesse tópico pode ser encontrada [aqui](Template_GenomeBatch_exemplo.xlsx).
### Files
  - **How do you want to provide files for this submission?**
    - FTP or Aspera Command Line file preload
    
## Passo 5: Submeta os arquivos dos MAGs
  - Pressione o botão **"Request Preload Folder"**.
  - Siga as instruções para transferência via FTP.
   - No terminal, execute os seguintes comandos:  
     1. Navegue até o diretório local onde estão os arquivos que deseja transferir:  
        ```bash
        cd [nome_do_diretorio]
        ```  
     2. Inicie o FTP:  
        ```bash
        ftp -i
        ```  
        > **Nota:** A opção `-i` evita que você precise confirmar manualmente o envio de cada arquivo. Se não usar esta opção, será necessário digitar "yes" repetidamente, e no meu caso, se eu demorava um pouquinho, a conexão fechava.
     3. Conecte-se ao servidor FTP do NCBI:  
        ```bash
        open ftp-private.ncbi.nlm.nih.gov
        ```  
     4. Insira o **login** e a **senha** fornecidos pelo NCBI na seção **"FTP instructions"**.
     5. Acesse o diretório gerado pelo NCBI com o comando `cd`. Exemplo:  
        ```bash
        cd uploads/[seu_email]_[codigo]
        ```  
     6. Crie um novo diretório dentro desse diretório usando o comando `mkdir`. Exemplo:  
        ```bash
        mkdir Nelore_mags
        ```  
     7. Entre no diretório recém-criado:  
        ```bash
        cd Nelore_mags
        ```  
     8. Para listar os arquivos no diretório local:  
        ```bash
        !ls
        ```  
     9. Copie os arquivos `.fasta` para o diretório remoto:  
        ```bash
        mput *fasta
        ```  
     10. Caso precise alternar para outro diretório local para enviar mais arquivos:  
         ```bash
         lcd [diretório]
         ```  
     11. Caso precise deletar arquivos do diretório remoto:  
         ```bash
         delete [arquivo]
         ```
  - Voltando ao site do NCBI:
    - Pressione o botão **"Select preload folder"**.
    - Encontre o diretório remoto que você criou (ex.: `Nelore_mags`). 
    - Verifique se o campo **"files"** indica o número correto de arquivos enviados. Caso contrário, pressione **"Refresh Folders"** até que todos os arquivos sejam contabilizados.  
    - Selecione o diretório remoto e pressione **"Use Selected Folder"**.
      
## Passo 6: Revise e finalize a submissão
### References
  - Adicione as informações sobre o artigo que descreve esses MAGs (se houver).
  - Pressione **Submit** para concluir a submissão.
  - Aguarde comunicações do NCBI por e-mail para confirmar o sucesso do envio ou para receber instruções adicionais caso seja necessário corrigir algum aspecto.  

____________________________________

## Pós submissão:

### Recebimento de e-mails e acompanhamento no site 

1. **Mensagens automáticas após a submissão:**  
  - **21/08/24:** Logo após a submissão recebi um e-mail confirmando que a submissão foi registrada com sucesso no banco de dados **BioSample**.
  - **26/08/24:** E-mail com os códigos de acesso (**BioSample accessions**) gerados para cada MAG submetido.
  - **29/08/24:** E-mail confirmando que a submissão foi recebida com sucesso pelo sistema de processamento de Genomas.
  - **30/08/24:** E-mails informando que a submissão passou nas verificações iniciais de validação e que foram gerados números de acesso para cada MAG.

2. **Notificação de erros:**  
  - Em **30/08/24**, recebi um e-mail com o título **"Error in Genome submission..."**.
  - Para checar os erros:
    - Acesse a página de submissão [https://submit.ncbi.nlm.nih.gov/subs/genome/](https://submit.ncbi.nlm.nih.gov/subs/genome/).
    - O status da submissão estava como **"Genomes: Processing"**.
    - Cliquei em **"See detailed report"**  para visualizar o status de cada MAG.
      - Na coluna **Message**, observei três padrões: 
        - Células em branco: indicam que o MAG passou sem problemas. 
        - Arquivos **"fixed foreign contaminations"** e **"modified fasta sequences"**: mostram modificações automáticas feitas pelo NCBI para remover ou ajustar sequências identificadas como contaminações. 
        - Um MAG apresentou um terceiro arquivo nessa coluna, cujo nome iniciava com **"remaining"**, que indica as regiões de contaminação no interior das sequências, e portanto eu deveria realizar a modificação.

### Alterações realizadas nos arquivos submetidos
1. **Alterações automáticas feitas pelo NCBI:**  
   - Como não selecionei a opção **"Do not automatically trim or remove sequences identified as contamination"** durante a submissão, o NCBI automaticamente removeu sequências marcadas para excluir e/ou trimou contaminações identificadas nas extremidades das sequências.

2. **Alterações manuais e ressubmissão:**  
   - Para contaminações no meio das sequências, as modificações devem ser feitas por quem submeteu os MAGs.
   - As informações de quais regiões devem ser removidas são detalhadas no arquivo **"remaining"**.  
   - Após corrigir os problemas apontados no arquivo **"remaining"**, deve-se ressubmeter o MAG que foi alterado.
   - No e-mail de erro, o NCBI instruía usar o botão **"Fix"** no portal para ressubmeter o arquivo, mas inicialmente eu não encontrei esse botão. Ele só ficou disponível após eu entrar em contato com a equipe do NCBI por e-mail. E no e-mail haviam orientações sobre como realizar a ressubmissão do arquivo.
  - Logo após a ressubmissão recebi um e-mail confirmando que a submissão foi registrada com sucesso.

### Liberação dos dados ao público ###
- Após algum tempo, recebi um e-mail da revista para a qual submeti o artigo referente a esses MAGs, informando que o artigo seria aceito somente se os dados estivessem ativos e disponíveis no NCBI. Eles não poderiam emitir uma aceitação formal enquanto essa condição não fosse atendida.
- Para resolver isso, enviei um e-mail para genomes@ncbi.nlm.nih.gov solicitando a divulgação pública dos dados associados aos números de submissão SUBXXXX, SUBXXXX e SUBXXX.
- Três dias depois, recebi um e-mail automático para cada número de submissão, confirmando que os dados haviam sido liberados.
- Vale ressaltar que os e-mails informavam que existe um atraso entre o momento em que um genoma é liberado e quando ele aparece em todos os recursos públicos do NCBI. Inicialmente, o genoma está disponível apenas por meio de pesquisa do número de acesso no banco de dados de nucleotídeos. Pode levar vários dias, ou até mais de uma semana, para que ele seja vinculado a todos os bancos de dados relevantes, como o BioProject.
- Embora eu não tenha acompanhado a data exata em que os MAGs ficaram visíveis na página do BioProject, isso ocorreu em menos de um mês.

### Acesso aos Dados ###
- Os dados cuja submissão foi descrita neste passo a passo estão disponíveis no seguinte link: [PRJNA987743 - BioProject no NCBI](https://www.ncbi.nlm.nih.gov/bioproject?term=PRJNA987743).  
- O artigo relacionado a esses dados já foi publicado em acesso aberto e pode ser acessado neste link: [Nature Scientific Data](https://www.nature.com/articles/s41597-024-04271-3).  


