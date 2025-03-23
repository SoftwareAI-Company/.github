
## 🔍 2. Revisão de Código (Code Review)
* Todo **código deve passar por revisão** antes do merge na branch principal.
* A revisão deve considerar:
  ✅ Clareza e legibilidade
  ✅ Coerência com o restante do projeto
  ✅ Segurança (evitar injeções SQL, XSS, etc.)
  ✅ Tratamento de erros adequado
  ✅ Código testável e sem duplicação

## 🔥 3. Commits e Mensagens de Commit

### 📖 Seguir a Conventional Commits
* Formato:

```yaml
tipo: descrição curta (72 caracteres no máximo)

[mensagem opcional detalhando a mudança]
```

* Tipos comuns de commits:
   * **feat:** Nova funcionalidade
   * **fix:** Correção de bug
   * **docs:** Documentação
   * **style:** Alteração de formatação (sem mudanças no código)
   * **refactor:** Refatoração sem mudança de funcionalidade
   * **test:** Adição/alteração de testes
   * **chore:** Tarefas de manutenção (configs, scripts, etc.)

* Exemplo:

```yaml
feat: adicionar suporte a múltiplos usuários no login

Agora o sistema permite múltiplos usuários simultaneamente, resolvendo #23.
```