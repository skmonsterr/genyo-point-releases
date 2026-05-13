# Genyo Point Releases

Repositorio publico usado apenas para publicar releases do aplicativo Genyo Point Automation.

O codigo-fonte fica no repositorio privado. Este repositorio deve conter somente documentacao minima e GitHub Releases com os assets de atualizacao.

## Assets obrigatorios por release

Publique estes arquivos como assets da release:

```text
Genyo-Point-Automation-Setup-<versao>.exe
Genyo-Point-Automation-Setup-<versao>.exe.blockmap
latest.yml
```

Asset opcional para download manual:

```text
Genyo-Point-Automation-Portable-<versao>.exe
```

## Fluxo

1. Criar tag `v<versao>`.
2. Criar GitHub Release publica.
3. Anexar os assets gerados pelo `electron-builder`.
4. O app instalado consulta esta release publica para atualizar.
