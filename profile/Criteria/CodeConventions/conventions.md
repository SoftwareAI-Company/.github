# 📜 Convenções para Aprovação de Código em Repositório Git

## 📌 1. Estilo de Código

### 📖 Seguir a PEP 8
* Linhas em branco entre classes e funções para melhorar a leitura.
* Uso consistente de espaços ao redor de operadores e depois de vírgulas.
* Imports devem ser organizados na ordem:
   1. **Módulos embutidos do Python**
   2. **Bibliotecas externas**
   3. **Módulos internos do projeto**
   * Separar cada grupo com uma linha em branco.

### 📖 Seguir a PEP 257 para Docstrings
* Toda função, classe e módulo deve ter uma **docstring explicativa**.
* Docstrings de várias linhas devem seguir o formato:

```python
def minha_funcao(param):
    """
    Explica brevemente a função.
    
    Args:
        param (tipo): Descrição do parâmetro.
    
    Returns:
        tipo: O que a função retorna.
    """
```
