# ✅ Como criar um novo Pull Request (PR)

## 🔧 1. Verifique se a branch principal é `main`

Se o repositório usa `main` como a branch principal (e **não `master`**), você precisa garantir que seu PR seja feito com base na `main`.

---

## 🧪 2. Trabalhe em uma nova branch

Crie e mude para uma nova branch a partir da `main`:

```bash
git checkout main
git pull origin main
git checkout -b minha-feature
```

Substitua `minha-feature` pelo nome da sua nova branch de trabalho.

---

## 💾 3. Faça commits normalmente

Adicione e faça commit das mudanças:

```bash
git add .
git commit -m "Minha implementação"
```

---

## 📤 4. Envie a nova branch para o repositório remoto

```bash
git push origin minha-feature
```

---

## 🚀 5. Crie o PR apontando para `main`

Usando GitHub CLI:

```bash
gh pr create --base main --head minha-feature --title "Título do PR" --body "Descrição do PR"
```

Se não tiver a GitHub CLI, você pode ir manualmente até o GitHub e clicar em "Compare & pull request".

---

## 📝 Nota: Se você começou na `master` por engano

Se você fez mudanças na `master`, mas o repositório usa `main`, siga estes passos:

```bash
git checkout master
git rebase main        # ou git merge main, se preferir
git push origin master
gh pr create --base main --head master --title "Título do PR" --body "Descrição do PR"
```
