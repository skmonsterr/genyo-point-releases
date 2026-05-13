# Genyo Point Automation

Genyo Point Automation e um aplicativo para Windows que ajuda a executar e agendar o registro de ponto no Genyo a partir do seu computador.

Ele foi pensado para uso diario, com tela simples, configuracao local, modo de teste seguro, execucao em segundo plano e aviso de atualizacoes.

## Para que serve

Com o aplicativo voce pode:

- configurar seus dados de acesso ao Genyo;
- testar o fluxo de entrada e saida sem registrar ponto;
- executar entrada ou saida manualmente quando necessario;
- deixar o Scheduler ativo para rodar nos horarios configurados;
- acompanhar logs simples pela tela do aplicativo;
- receber notificacoes do Windows quando o Scheduler iniciar uma operacao;
- atualizar o aplicativo quando uma nova versao estiver disponivel.

## Instalacao

1. Abra a pagina de Releases deste repositorio.
2. Baixe o instalador mais recente do Windows.
3. O arquivo segue este padrao de nome:

```text
genyo-point-automation-setup-v<versao>-win32-x64.exe
```

4. Execute o instalador.
5. Escolha a pasta de instalacao quando o instalador solicitar.
6. Abra o aplicativo pelo atalho criado no Windows.

Nao e necessario baixar arquivos como `latest.yml` ou `.blockmap`. Eles sao usados internamente pelo sistema de atualizacao.

## Primeira configuracao

Ao abrir o aplicativo pela primeira vez, clique em **Configurar**.

Preencha os campos obrigatorios no arquivo de configuracao exibido pela tela:

```text
GENYO_COMPANY_CODE=
GENYO_ACCESS_NUMBER=
TWOCAPTCHA_API_KEY=
```

Essas informacoes sao necessarias para que o aplicativo consiga acessar o Genyo e executar o fluxo corretamente.

Tambem revise estes campos:

```text
HEADLESS=true
SCHEDULE_ENTRY=0 9 * * 1-5
SCHEDULE_EXIT=0 18 * * 1-5
TIMEZONE=America/Sao_Paulo
```

Uso recomendado:

- `HEADLESS=true`: executa o navegador em segundo plano, recomendado para uso diario.
- `SCHEDULE_ENTRY`: horario agendado para entrada.
- `SCHEDULE_EXIT`: horario agendado para saida.
- `TIMEZONE`: fuso horario usado pelo agendamento.

Depois de editar, clique em **Salvar**.

Se precisar acompanhar visualmente o navegador durante um teste, altere temporariamente para `HEADLESS=false`. Para o uso diario com Scheduler, mantenha `HEADLESS=true`.

## Modos do aplicativo

O aplicativo possui dois modos principais.

### Safe Mode

Use o **Safe Mode** para testar.

Nesse modo, o aplicativo percorre o fluxo, mas nao confirma o clique final de registro. E o modo mais seguro para validar credenciais, navegador e comportamento geral.

Recomendacao: sempre teste em Safe Mode antes de usar o modo real.

### Real Mode

Use o **Real Mode** somente quando quiser executar a entrada ou saida de verdade.

Nesse modo, o aplicativo pode concluir o registro real no Genyo.

O Scheduler so deve ser iniciado em Real Mode. Se o Safe Mode estiver ativo, o aplicativo bloqueia o Scheduler para evitar uma configuracao incorreta.

## Uso diario

Existem dois usos principais no dia a dia.

### Disparo manual

Use quando quiser registrar entrada ou saida manualmente, sem deixar o Scheduler ativo.

1. Abra o aplicativo.
2. Confirme se a configuracao esta preenchida.
3. Teste em Safe Mode, se necessario.
4. Altere para Real Mode.
5. Clique em **Rodar entrada em Real Mode** ou **Rodar saida em Real Mode**.
6. Aguarde a operacao terminar e confira o console de logs.

### Scheduler em segundo plano

Use quando quiser deixar o aplicativo aguardando os horarios configurados.

1. Abra o aplicativo.
2. Confirme se a configuracao esta preenchida.
3. Mantenha `HEADLESS=true` para execucao em segundo plano.
4. Altere para Real Mode.
5. Clique em **Iniciar** no painel do Scheduler.
6. O aplicativo sera enviado para segundo plano e continuara ativo na bandeja do Windows.

Voce precisa iniciar o Scheduler uma vez. Depois disso, ele continua rodando ate voce decidir parar manualmente pelo aplicativo ou pela bandeja do Windows.

Quando o Scheduler estiver ativo, ele aguardara os horarios de entrada e saida configurados.

A tela mostra:

- se o Scheduler esta ativo;
- a ultima operacao de entrada e saida;
- a proxima operacao prevista;
- se a ultima execucao foi concluida com sucesso ou falhou.

## Bandeja do Windows

Ao iniciar o Scheduler, o aplicativo pode ficar em segundo plano na bandeja do Windows.

Pela bandeja voce pode:

- abrir novamente a tela do aplicativo;
- parar o Scheduler;
- sair do aplicativo.

Se o computador for reiniciado, o aplicativo tenta reativar o Scheduler automaticamente, desde que ele tenha sido iniciado anteriormente e nao tenha sido parado manualmente.

## Atualizacoes

O aplicativo verifica se existe uma nova versao disponivel.

Na tela, use **Verificar atualizacoes** para consultar manualmente.

Boas praticas antes de atualizar:

1. Pare o Scheduler.
2. Feche operacoes em andamento.
3. Baixe e instale a atualizacao pelo proprio aplicativo quando ela aparecer.
4. Abra o aplicativo novamente e confira a configuracao.

O aplicativo preserva os arquivos locais de configuracao e logs durante atualizacoes normais.

## Boas praticas

- Mantenha seus dados de configuracao protegidos.
- Nao compartilhe o arquivo `genyo-config.txt`.
- Use Safe Mode depois de qualquer alteracao na configuracao.
- Evite fechar o navegador enquanto uma operacao estiver em andamento.
- Pare o Scheduler antes de instalar atualizacoes.
- Confira os logs na tela se alguma operacao falhar.
- Mantenha o computador ligado e conectado a internet nos horarios agendados.

## Problemas comuns

### O Scheduler nao inicia

Verifique se:

- os campos obrigatorios foram preenchidos em **Configurar**;
- o aplicativo esta em Real Mode;
- os horarios de entrada e saida estao configurados;
- o navegador configurado esta disponivel no Windows.

### O navegador nao abriu

Abra **Configurar** e revise as opcoes relacionadas ao navegador.

Em muitos casos, usar:

```text
PLAYWRIGHT_BROWSER_CHANNEL=msedge
HEADLESS=true
```

usa o Microsoft Edge instalado no Windows em segundo plano. Para diagnostico visual, use `HEADLESS=false` temporariamente.

### A operacao falhou

Confira o console de logs na tela do aplicativo. Ele mostra mensagens simplificadas para orientar o uso.

Se necessario, tente executar primeiro em Safe Mode para validar o fluxo sem registrar ponto.

### Fechei o navegador durante a execucao

O aplicativo tratara isso como uma operacao interrompida ou falha. Execute novamente quando estiver pronto.

## Arquivos locais

O aplicativo guarda configuracoes e logs na pasta de dados do usuario no Windows, normalmente em:

```text
AppData/Roaming/genyo-point-automation
```

Arquivos importantes:

- `config/genyo-config.txt`: configuracao do aplicativo;
- `logs/`: logs operacionais e tecnicos;
- `state/`: estado interno do Scheduler.

Nao apague esses arquivos sem necessidade. Eles ajudam o aplicativo a manter configuracoes, historico e funcionamento em segundo plano.
