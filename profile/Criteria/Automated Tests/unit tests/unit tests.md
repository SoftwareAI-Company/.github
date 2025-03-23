* Todo código novo ou alterado deve incluir unit tests 
**testes unitários**.
* Seguir a estrutura recomendada de testes:

```bash
tests/
├── test_modulo_x.py
├── test_modulo_y.py
└── conftest.py # Configurações do pytest
```

* Utilizar **pytest** e **mocks** quando necessário.


# ⚡ Melhores Frameworks de Testes Unitários para Python

Aqui estão alguns dos melhores frameworks para testes unitários em Python:

## 1️⃣ **pytest** (Recomendado)

- Simples e poderoso, permite escrever testes de forma intuitiva.
- Suporta **parametrização**, **fixtures** e **plugins**.
- Exemplo:
  ```python
  import pytest

  def soma(a, b):
      return a + b

  def test_soma():
      assert soma(2, 3) == 5
  ```

## 2️⃣ **unittest** (Nativo do Python)

- Vem embutido no Python, inspirado no JUnit do Java.
- Mais verboso que `pytest`, mas confiável.
- Exemplo:
  ```python
  import unittest

  class TestSoma(unittest.TestCase):
      def test_soma(self):
          self.assertEqual(soma(2, 3), 5)

  if __name__ == "__main__":
      unittest.main()
  ```

## 3️⃣ **doctest** (Testes Embutidos na Documentação)

- Permite escrever testes dentro da docstring das funções.
- ótimo para testar pequenos códigos sem criar arquivos separados.
- Exemplo:
  ```python
  def soma(a, b):
      """
      Soma dois números.
      
      >>> soma(2, 3)
      5
      >>> soma(-1, 1)
      0
      """
      return a + b

  if __name__ == "__main__":
      import doctest
      doctest.testmod()
  ```

## 4️⃣ **hypothesis** (Testes Baseados em Propriedades)

- Gera automaticamente casos de testes aleatórios para encontrar falhas.
- ótimo para testar funções matemáticas e algoritmos.
- Exemplo:
  ```python
  from hypothesis import given, strategies as st

  @given(st.integers(), st.integers())
  def test_soma(a, b):
      assert soma(a, b) == a + b
  ```

---

# 🎯 Conclusão

Os testes unitários garantem qualidade, reduzem custos e facilitam a manutenção do código em projetos reais. Frameworks como **pytest** e **unittest** tornam a implementação simples e eficaz. Implementar testes desde a pré-produção é essencial para evitar problemas em produção e manter um sistema confiável. 🚀

