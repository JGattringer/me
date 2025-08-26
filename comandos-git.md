# Roteiro 1 – Aula Prática: Git & GitHub

---

## 1. Pré‑requisitos

| Item               | Detalhes                                                         |
| ------------------ | ---------------------------------------------------------------- |
| Conta GitHub       | [https://github.com](https://github.com) (gratuita)              |
| Git bash instalado | [Download](https://git-scm.com/) – verifique com `git --version` |
| Editor de texto    | VS Code, Atom, Sublime ou equivalente                            |

```bash
# Configurar identidade global (substitua pelos seus dados)
git config --global user.name "Seu Nome"
git config --global user.email "voce@email.com"
```

---

## 2. Criar o repositório _de perfil_

1. Acesse `github.com`.
2. Em **Repository name** (digitar próprio usuário), digite ``(ex.:`maria‑dev`).
3. Selecione **Public**.
4. Marque **Add a README file**.
5. (Opcional) Escolha licença _MIT_ e arquivo `.gitignore` para _Node_.
6. Clique **Create repository**.
7. Clone localmente:
   ```bash
   git clone https://github.com/<user>/<user>.git
   cd <user>
   ```
8. Edite `README.md` (bio, skills, badges) e faça:
   ```bash
   git add README.md
   git commit -m "feat: primeira versão do README"
   git push origin main
   ```

> **Dica:** ative **GitHub Pages** em _Settings ▸ Pages_ para exibir seu portfólio (branch `main`, pasta `/root`).

---

## 3. Criar o repositório ``

1. Novamente em **New repository**, use o nome ``.
2. Descrição: _Anotações da disciplina e comandos Git_.
3. Marque **Add a README** e crie.
4. Estrutura de pastas sugerida:

   ```text
   my-github/
   ├─ README.md              # visão geral do repositório
   ├─ comandos-git.md        # "cheat sheet" de comandos

   ```

5. Preencher **comandos-git.md** com tabela inicial:
   | Comando | Descrição |
   | --------------------------- | ----------------------------------- |
   | `git init` | Inicializa um repositório vazio |
   | `git clone <url>` | Clona repositório remoto |
   | `git status` | Mostra estado da árvore de trabalho |
   | `git add <arq>` | Adiciona mudanças ao stage |
   | `git commit -m "msg"` | Registra snapshot |
   | `git push` | Envia commits ao GitHub |
   | `git pull` | Sincroniza e integra mudanças |
   | `git branch` | Lista ou cria branches |
   | `git merge` | Mescla branches |
   | `git log --oneline --graph` | Histórico compacto |

---

## 4. Sincronizar repositórios localmente

```bash
# Diretório de trabalho para todos os projetos
mkdir ~/repos-projeto-iot && cd ~/repos-projeto-iot

git clone https://github.com/<user>/<user>.git
git clone https://github.com/<user>/my-github.git
```

---

## 5. Fluxo de trabalho básico

1. **Create/Change**: editar arquivo.
2. **Review**: `git status`.
3. **Stage**: `git add <arquivo>`.
4. **Commit**: `git commit -m "tipo: mensagem"`.
5. **Push**: `git push origin main`.

> Convenção de mensagens: `feature:`, `hotfix:`, `docs:`…

---

### 5.1 Voltando a um commit (Revert / Reset)

| Método          | Quando usar                             | Comando principal                     |
| --------------- | --------------------------------------- | ------------------------------------- |
| **Nova branch** | Precisa testar mudança sem tocar `main` | `git checkout -b volta-commit <hash>` |
| **git revert**  | Quer preservar histórico (opção segura) | `git revert <hash>`                   |
| **git reset**   | Precisa reescrever histórico local      | `git reset --hard <hash>`             |

1. **Descobrir hash** do commit alvo:
   ```bash
   git log --oneline --graph --decorate -n 10
   ```
2. **Criar branch** (opcional, mas recomendado):
   ```bash
   git checkout -b hotfix/volta-commit <hash>
   ```
3. **Reverter (seguro):**
   ```bash
   git revert <hash>
   git push origin main
   ```
4. **Reset (perigoso):**
   ```bash
   # Desfaz commits locais e move ponteiro; use --soft para manter staging, --mixed (padrão) ou --hard.
   git reset --hard <hash>
   git push --force-with-lease
   ```

> **Dica:** nunca use `git reset --hard` depois que commits já foram enviados e utilizados por outros.

---

## 10. Entregáveis (até 19·ago·2025)

- Repositório do Perfil com README preenchido (about, stacks, bages).
- Repositório contendo pelo menos:
  - `comandos-git.md` com 10+ comandos documentados.

---

## 11. Links e Livros

- [GitHub Docs – Hello World](https://docs.github.com/en/get-started/quickstart)
- [Pro Git Book (pt‑BR)](https://git-scm.com/book/pt-br/v2)
- [Git Cheat Sheet PDF](https://education.github.com/git-cheat-sheet-education.pdf)

---

---

# Roteiro 2 – Branches

## 1) Voltando a um commit (Revert / Reset) - Roteiro 1 (ainda)

| Método          | Quando usar                             | Comando principal                     |
| --------------- | --------------------------------------- | ------------------------------------- |
| **Nova branch** | Precisa testar mudança sem tocar `main` | `git checkout -b volta-commit <hash>` |
| **git revert**  | Quer preservar histórico (opção segura) | `git revert <hash>`                   |
| **git reset**   | Precisa reescrever histórico local      | `git reset --hard <hash>`             |

1. **Descobrir hash** do commit alvo:
   ```bash
   git log --oneline --graph --decorate -n 10
   ```
2. **Criar branch** (opcional, mas recomendado):
   ```bash
   git checkout -b hotfix/volta-commit <hash>
   ```
3. **Reverter (seguro):**
   ```bash
   git revert <hash>
   git push origin main
   ```
4. **Reset (perigoso):**
   ```bash
   # Desfaz commits locais e move ponteiro; use --soft para manter staging, --mixed (padrão) ou --hard.
   git reset --hard <hash>
   git push --force-with-lease
   ```

> **Dica:** nunca use `git reset --hard` depois que commits já foram enviados e utilizados por outros.

---

## 2) Convenções de **Git Semântico** (Conventional Commits)

Use mensagens padronizadas e curtas, com **tipo**, **escopo** (opcional) e **resumo no imperativo**.

**Formato:**

```
<tipo>(<escopo>): <resumo>

[corpo opcional explicando o porquê]
[Refs: #<issue>]
```

**Tipos comuns:**

- `feat` (nova funcionalidade)
- `fix` (correção de bug)
- `docs` (documentação)
- `style` (formatação, sem mudança de lógica)
- `refactor` (refatoração, sem alterar comportamento externo)
- `test` (testes)
- `build` (build, dependências)
- `ci` (pipelines de CI)
- `chore` (tarefas diversas que não afetam src/test)

**Exemplos práticos:**

```bash

feat(readme): inclusão do campo about me
fix(api): tratar null pointer ao criar pedido
docs(contrib): adicionar guia de pull request
refactor(core): extrair serviço de cálculo de frete
chore(deps): atualizar axios para ^1.7.0
```

> **Por que usar?** Facilita _changelog_, leitura do histórico e automações (versionamento semântico, release notes, validações de CI).

## 3) Trabalhando com `git stash`

O `git stash` é usado para **guardar mudanças temporariamente** sem precisar fazer commit, permitindo trocar de branch ou atualizar código sem perder alterações não finalizadas.

**Comandos principais:**

- Guardar alterações:
  ```bash
  git stash
  ```
- Guardar com mensagem:
  ```bash
  git stash save "ajustes parciais no componente header"
  ```
- Listar stashes salvos:
  ```bash
  git stash list
  ```
- Recuperar o último stash (mantendo no histórico):
  ```bash
  git stash apply
  ```
- Recuperar e remover do histórico:
  ```bash
  git stash pop
  ```
- Aplicar stash específico:
  ```bash
  git stash apply stash@{2}
  ```
- Limpar todos os stashes:
  ```bash
  git stash clear
  ```

> Útil quando precisa trocar de branch no meio do desenvolvimento ou atualizar a `main` sem perder o que já começou.

## 4) Convenções de **nomenclatura** de branches

Use nomes curtos, descritivos e com _slug_ (kebab-case). Inclua tipo e, se houver, ID do ticket.

- `feature/<id>-<descricao>` → `feature/1234-criar-endpoint-pedidos`
- `fix/<id>-<descricao>` → `fix/235-bug-null-pointer-checkout`
- `chore/<descricao>` → `chore/atualizar-dependencias`
- `hotfix/<descricao>` → `hotfix/corrigir-regra-frete`
- `docs/<descricao>` → `docs/roteiro-git-pr`

---

## 5) Criar branch a partir da `main`

Sempre atualize sua `main` local antes de ramificar.

```bash
git checkout main
git pull

git checkout -b feature/1234-criar-endpoint-pedidos

```

---

## 6) Ciclo de commits (aplicando Git Semântico)

Mantenha commits pequenos, coesos e com mensagens claras.

**Exemplo completo:**

```bash
git add .
git commit -m "feat(api): criar endpoint POST /pedidos Adiciona validações e integra com serviço de estoque. Refs: #1234"
```

---

## 7) Manter sua branch atualizada (rebase recomendado)

Enquanto desenvolve, replique alterações da `main` para evitar _big bang merges_.

```bash
git fetch origin
git rebase origin/main
# Resolva conflitos (se houver), depois:
git rebase --continue

```

---

## Extra: O que é `--force-with-lease` e por que usar?

Ao reescrever histórico (ex.: `git rebase`) e enviar para o remoto, o comando `git push` normal será rejeitado se houver divergências. O `--force` sobrescreve tudo **sem verificar** se alguém mais atualizou o remoto, podendo apagar trabalho alheio.

O `--force-with-lease` é mais seguro: ele **só força o push se a referência remota ainda estiver como você a viu no último **`**/**`. Se outra pessoa tiver feito um push no intervalo, o comando falha, evitando sobrescrever mudanças sem querer.

**Fluxo recomendado:**

```bash
git fetch origin
git rebase origin/main
git push --force-with-lease
```

> Sempre combine com a equipe antes de usar, especialmente em branches compartilhadas.

---

## 8) Material de estudo extra:

- [Video da explicação do que é branche (legendado)](https://www.youtube.com/watch?v=e9lnsKot_SQ)

---

---

# Roteiro 3 – Branches

## <span style="color:red">1) Voltando a um commit (Revert / Reset) - Roteiro 1 (ainda) ✅</span>

| Método          | Quando usar                             | Comando principal                     |
| --------------- | --------------------------------------- | ------------------------------------- |
| **Nova branch** | Precisa testar mudança sem tocar `main` | `git checkout -b volta-commit <hash>` |
| **git revert**  | Quer preservar histórico (opção segura) | `git revert <hash>`                   |
| **git reset**   | Precisa reescrever histórico local      | `git reset --hard <hash>`             |

1. **Descobrir hash** do commit alvo:
   ```bash
   git log --oneline --graph --decorate -n 10
   ```
2. **Criar branch** (opcional, mas recomendado):
   ```bash
   git checkout -b hotfix/volta-commit <hash>
   ```
3. **Reverter (seguro):**
   ```bash
   git revert <hash>
   git push origin main
   ```
4. **Reset (perigoso):**
   ```bash
   # Desfaz commits locais e move ponteiro; use --soft para manter staging, --mixed (padrão) ou --hard.
   git reset --hard <hash>
   git push --force-with-lease
   ```

> **Dica:** nunca use `git reset --hard` depois que commits já foram enviados e utilizados por outros.

---

## <span style="color:red">2) Convenções de **Git Semântico** (Conventional Commits) ✅</span>

Use mensagens padronizadas e curtas, com **tipo**, **escopo** (opcional) e **resumo no imperativo**.

**Formato:**

```
<tipo>(<escopo>): <resumo>

[corpo opcional explicando o porquê]
[Refs: #<issue>]
```

**Tipos comuns:**

- `feat` (nova funcionalidade)
- `fix` (correção de bug)
- `docs` (documentação)
- `style` (formatação, sem mudança de lógica)
- `refactor` (refatoração, sem alterar comportamento externo)
- `test` (testes)
- `build` (build, dependências)
- `ci` (pipelines de CI)
- `chore` (tarefas diversas que não afetam src/test)

**Exemplos práticos:**

```bash

feat(readme): inclusão do campo about me
fix(api): tratar null pointer ao criar pedido
docs(contrib): adicionar guia de pull request
refactor(core): extrair serviço de cálculo de frete
chore(deps): atualizar axios para ^1.7.0
```

> **Por que usar?** Facilita _changelog_, leitura do histórico e automações (versionamento semântico, release notes, validações de CI).

## <span style="color:red"> 3) Trabalhando com `git stash` ✅</span>

O `git stash` é usado para **guardar mudanças temporariamente** sem precisar fazer commit, permitindo trocar de branch ou atualizar código sem perder alterações não finalizadas.

**Comandos principais:**

- Guardar alterações:
  ```bash
  git stash
  ```
- Guardar com mensagem:
  ```bash
  git stash save "ajustes parciais no componente header"
  ```
- Listar stashes salvos:
  ```bash
  git stash list
  ```
- Recuperar o último stash (mantendo no histórico):
  ```bash
  git stash apply
  ```
- Recuperar e remover do histórico:
  ```bash
  git stash pop
  ```
- Aplicar stash específico:
  ```bash
  git stash apply stash@{2}
  ```
- Limpar todos os stashes:
  ```bash
  git stash clear
  ```

> Útil quando precisa trocar de branch no meio do desenvolvimento ou atualizar a `main` sem perder o que já começou.

## <span style="color:red">4) Convenções de **nomenclatura** de branches ✅</span>

Use nomes curtos, descritivos e com _slug_ (kebab-case). Inclua tipo e, se houver, ID do ticket.

- `feature/<id>-<descricao>` → `feature/1234-criar-endpoint-pedidos`
- `fix/<id>-<descricao>` → `fix/235-bug-null-pointer-checkout`
- `chore/<descricao>` → `chore/atualizar-dependencias`
- `hotfix/<descricao>` → `hotfix/corrigir-regra-frete`
- `docs/<descricao>` → `docs/roteiro-git-pr`

---

## <span style="color:red"> 5) Criar branch a partir da `main` ✅</span>

Sempre atualize sua `main` local antes de ramificar.

```bash
git checkout main
git pull

git checkout -b feature/1234-criar-endpoint-pedidos

```

---

## <span style="color:red">6) Ciclo de commits (aplicando Git Semântico) ✅</span>

Mantenha commits pequenos, coesos e com mensagens claras.

**Exemplo completo:**

```bash
git add .
git commit -m "feat(api): criar endpoint POST /pedidos Adiciona validações e integra com serviço de estoque. Refs: #1234"
```

---

## <span style="color:red">7) Manter sua branch atualizada (rebase recomendado) ✅</span>

Enquanto desenvolve, replique alterações da `main` para evitar _big bang merges_.

```bash
git fetch origin
git rebase origin/main
# Resolva conflitos (se houver), depois:
git rebase --continue

```

---

## Extra: O que é `--force-with-lease` e por que usar?

Ao reescrever histórico (ex.: `git rebase`) e enviar para o remoto, o comando `git push` normal será rejeitado se houver divergências. O `--force` sobrescreve tudo **sem verificar** se alguém mais atualizou o remoto, podendo apagar trabalho alheio.

O `--force-with-lease` é mais seguro: ele **só força o push se a referência remota ainda estiver como você a viu no último **`**/**`. Se outra pessoa tiver feito um push no intervalo, o comando falha, evitando sobrescrever mudanças sem querer.

**Fluxo recomendado:**

```bash
git fetch origin
git rebase origin/main
git push --force-with-lease
```

> Sempre combine com a equipe antes de usar, especialmente em branches compartilhadas.

---

## 8) Material de estudo extra:

- [Video da explicação do que é branche (legendado)](https://www.youtube.com/watch?v=e9lnsKot_SQ)

---

# Roteiro 4 – Merge de Branch & Pull Request (GitHub Flow)

---

## 1) Criando uma feature branch

```bash
git checkout -b feature/resolvendo-conflito
```

- Crie um arquivo `resolucao_conflito.md`.
- **Tarefa:** documente com _prints_ como você resolveu um conflito (antes/depois).

---

## 2) Merge na branch (atualizar com `main`)

### Passo a passo

```bash
# 1) Certifique-se de estar na sua feature
git checkout feature/resolvendo-conflito

# 2) Busque as últimas mudanças da remota
git fetch origin

# 3) Faça merge de main na sua branch
git merge origin/main
```

- **Se não houver conflitos:** o Git fará _fast-forward_ ou criará um _merge commit_. Finalize a mensagem (se abrir o editor) e siga com o push:

  ```bash
  git push -u origin feature/resolvendo-conflito
  ```

- **Se houver conflitos:** o Git marcará os arquivos com trechos como abaixo. Edite e escolha o conteúdo correto, removendo os marcadores.

  ```text
  <<<<<<< HEAD
  (sua versão na branch)
  =======
  (versão que veio de origin/main)
  >>>>>>> origin/main
  ```

  Depois:

  ```bash
  git status                       # veja quais arquivos estão em conflito
  git add resolucao_conflito.md    # e outros que você ajustou
  git commit -m "merge: resolve conflitos com main"
  git push                         # atualiza o PR quando ele existir
  ```

### Dica (opcional)

Prefere história linear? Em vez de `merge`, você poderia rebasear:

```bash
git checkout feature/resolvendo-conflito
git fetch origin
git rebase origin/main   # resolve conflitos e continue com: git rebase --continue
```

### Validando o resultado

```bash
git log --oneline --graph --decorate -n 10
```

Verifique se sua branch ficou adiante de `main` com o _merge commit_ (ou rebase aplicado) e que os testes/execução local seguem funcionando.

---

## 3) Abrindo um Pull Request (PR)

1. No GitHub, clique em **Compare & pull request**.
2. Confirme **base: **` ← **compare: **`.

### Exemplo de _pull request_

```md
## Objetivo

-

## Mudanças

-

## Checklist

- [ ] Testes locais
- [ ] Atualizado docs
```

---

## Estratégias de merge no GitHub

| Estratégia           | Resultado no histórico               | Prós                                | Contras                               | Quando usar                                     |
| -------------------- | ------------------------------------ | ----------------------------------- | ------------------------------------- | ----------------------------------------------- |
| **Merge commit**     | Cria um commit de merge              | Preserva a história exata           | Histórico mais "verboso"              | Projetos que valorizam rastreabilidade completa |
| **Squash and merge** | Condensa todos os commits do PR em 1 | História linear e limpa             | Perde granularidade de commits        | PRs com muitos commits pequenos                 |
| **Rebase and merge** | Reaplica commits sobre `main` (FF)   | História linear sem commit de merge | Pode reescrever SHA; cuidado em times | Times experientes que preferem linearidade      |

Material de estudo extra:

- [COMO FAZER UM PULL REQUEST (legendado)](https://www.youtube.com/watch?v=IMerCpaT_zM)

---

# Roteiro – Merge de Branch & Pull Request (GitHub Flow)

---

## <span style="color:red">1) Criando uma feature branch ✅</span>

```bash
git checkout -b feature/resolvendo-conflito
```

- Crie um arquivo `resolucao_conflito.md`.
- **Tarefa:** documente com _prints_ como você resolveu um conflito (antes/depois).

---

## <span style="color:red">2) Merge na branch (atualizar com `main`) ✅</span>

### Passo a passo

```bash
# 1) Certifique-se de estar na sua feature
git checkout feature/resolvendo-conflito

# 2) Busque as últimas mudanças da remota
git fetch origin

# 3) Faça merge de main na sua branch
git merge origin/main
```

- **Se não houver conflitos:** o Git fará _fast-forward_ ou criará um _merge commit_. Finalize a mensagem (se abrir o editor) e siga com o push:

  ```bash
  git push -u origin feature/resolvendo-conflito
  ```

- **Se houver conflitos:** o Git marcará os arquivos com trechos como abaixo. Edite e escolha o conteúdo correto, removendo os marcadores.

  ```text
  <<<<<<< HEAD
  (sua versão na branch)
  =======
  (versão que veio de origin/main)
  >>>>>>> origin/main
  ```

  Depois:

  ```bash
  git status                       # veja quais arquivos estão em conflito
  git add resolucao_conflito.md    # e outros que você ajustou
  git commit -m "merge: resolve conflitos com main"
  git push                         # atualiza o PR quando ele existir
  ```

### Dica (opcional)

Prefere história linear? Em vez de `merge`, você poderia rebasear:

```bash
git checkout feature/resolvendo-conflito
git fetch origin
git rebase origin/main   # resolve conflitos e continue com: git rebase --continue
```

### Validando o resultado

```bash
git log --oneline --graph --decorate -n 10
```

Verifique se sua branch ficou adiante de `main` com o _merge commit_ (ou rebase aplicado) e que os testes/execução local seguem funcionando.

---

## <span style="color:red">3) Abrindo um Pull Request (PR) ✅</span>

1. No GitHub, clique em **Compare & pull request**.
2. Confirme **base: **` ← **compare: **`.

### Exemplo de _pull request_

```md
## Objetivo

-

## Mudanças

-

## Checklist

- [ ] Testes locais
- [ ] Atualizado docs
```

---

## Estratégias de merge no GitHub

| Estratégia           | Resultado no histórico               | Prós                                | Contras                               | Quando usar                                     |
| -------------------- | ------------------------------------ | ----------------------------------- | ------------------------------------- | ----------------------------------------------- |
| **Merge commit**     | Cria um commit de merge              | Preserva a história exata           | Histórico mais "verboso"              | Projetos que valorizam rastreabilidade completa |
| **Squash and merge** | Condensa todos os commits do PR em 1 | História linear e limpa             | Perde granularidade de commits        | PRs com muitos commits pequenos                 |
| **Rebase and merge** | Reaplica commits sobre `main` (FF)   | História linear sem commit de merge | Pode reescrever SHA; cuidado em times | Times experientes que preferem linearidade      |

Material de estudo extra:

- [COMO FAZER UM PULL REQUEST (legendado)](https://www.youtube.com/watch?v=IMerCpaT_zM)

---
