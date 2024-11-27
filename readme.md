# GitHub Action - Monitor de Alterações em Arquivo

Este GitHub Action monitora alterações em um arquivo específico e cria automaticamente um registro em markdown sempre que o arquivo é modificado.

## 🚀 Como Usar

1. Crie um diretório `.github/workflows` no seu repositório (se ainda não existir)
2. Crie um arquivo `monitor-file.yml` dentro deste diretório
3. Copie o código abaixo e cole no arquivo:

```yaml
name: Monitor File Changes

on:
  push:
    paths:
      - 'caminho/para/seu/arquivo.ext'  # Substitua pelo caminho do seu arquivo
  pull_request:
    paths:
      - 'caminho/para/seu/arquivo.ext'  # Substitua pelo caminho do seu arquivo

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout do código
        uses: actions/checkout@v4
      
      - name: Obter data atual
        id: date
        run: echo "date=$(date +'%d/%m/%Y %H:%M:%S')" >> $GITHUB_OUTPUT
      
      - name: Criar arquivo markdown com atualização
        run: |
          echo "# Atualização de Arquivo" > update.md
          echo "" >> update.md
          echo "O arquivo \`caminho/para/seu/arquivo.ext\` foi atualizado em ${{ steps.date.outputs.date }}" >> update.md
          echo "" >> update.md
          echo "**Commit:** ${{ github.sha }}" >> update.md
          echo "**Autor:** ${{ github.actor }}" >> update.md
      
      - name: Commit do arquivo markdown
        run: |
          git config --local user.email "action@github.com"
          git config --local user.name "GitHub Action"
          git add update.md
          git commit -m "docs: atualiza registro de modificação do arquivo"
          git push
```

## ⚙️ Configuração

1. Substitua `caminho/para/seu/arquivo.ext` pelo caminho real do arquivo que você quer monitorar
2. O Action irá monitorar tanto pushes quanto pull requests que modifiquem o arquivo especificado

## 📋 O que o Action faz

Quando o arquivo monitorado é modificado, o Action:
1. Registra a data e hora da modificação
2. Cria um arquivo `update.md` com:
   - Data e hora da atualização
   - Hash do commit
   - Nome do autor da modificação
3. Faz commit e push automático do arquivo `update.md`

## 📝 Formato do Registro

O arquivo `update.md` gerado terá este formato:

```markdown
# Atualização de Arquivo

O arquivo `seu-arquivo.ext` foi atualizado em DD/MM/AAAA HH:MM:SS

**Commit:** 123abc...
**Autor:** nome-do-usuario
```

## 🔒 Permissões Necessárias

Para que o Action funcione corretamente, certifique-se de que:
1. O workflow tem permissão para fazer commits no repositório
2. O token do GitHub tem acesso de escrita ao repositório

## 💡 Dicas

- Você pode personalizar a mensagem do commit alterando o valor em `git commit -m "..."` 
- O formato da data pode ser alterado modificando o parâmetro no comando `date +'%d/%m/%Y %H:%M:%S'`
- Você pode adicionar mais informações ao arquivo markdown editando o bloco `Criar arquivo markdown com atualização`

## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.