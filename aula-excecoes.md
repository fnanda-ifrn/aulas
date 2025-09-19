# Dominando o Tratamento de Exceções em Python 🐍

Uma Aula Prática e Ilustrada para Desenvolvedores

Apresentado por: Fernanda (Profissional de TI)

------------------------------------------------------------------------

## 1. Introdução: Quando as Coisas Dão Errado?

No mundo da programação, nem tudo sai como o planejado. Um arquivo pode
não existir, o usuário pode digitar texto onde era esperado um número, a
conexão com a internet pode cair...

Essas situações que interrompem o fluxo normal de um programa são
chamadas de **exceções**.

Sem tratamento, elas causam erros e travam o software. O tratamento de
exceções nos permite "capturar" esses problemas e reagir a eles de forma
controlada.

*Ilustração Conceitual:* Pneu furado no carro. Sem estepe vs. Com estepe
(`try-except`).

------------------------------------------------------------------------

## 2. O Básico: `try` e `except` 🛠️

A estrutura fundamental para tratamento de exceções em Python é o bloco
`try...except`.

-   **`try`:** O código que *pode* levantar uma exceção é colocado
    dentro deste bloco.\
-   **`except`:** Se uma exceção ocorrer, o Python *salta* para o bloco
    `except`, onde você lida com o erro.

### Exemplo Prático: Divisão por Zero

``` python
print("Início do programa.")

try:
    resultado = 10 / 0  # Esta linha vai gerar uma exceção
    print(f"O resultado é: {resultado}")
except ZeroDivisionError:
    print("Erro: Não é possível dividir um número por zero!")

print("Fim do programa (continuou funcionando!).")
```

*Ilustração:* Fluxograma simples: TRY -\> SUCESSO / ERRO -\> EXCEPT.

------------------------------------------------------------------------

## 3. Lidando com Múltiplas Exceções 🚥

Um bloco `try` pode ser seguido por múltiplos blocos `except`, cada um
tratando um tipo específico de exceção.

É uma boa prática ser o mais específico possível nos blocos `except`.

### Exemplo Prático: Entrada do Usuário

``` python
try:
    num1 = int(input("Digite o primeiro número: "))
    num2 = int(input("Digite o segundo número: "))
    resultado = num1 / num2
    print(f"O resultado é: {resultado}")
except ValueError:
    print("Erro: Entrada inválida! Digite apenas números.")
except ZeroDivisionError:
    print("Erro: Não é possível dividir por zero!")
except Exception as e:
    print(f"Ocorreu um erro inesperado: {e}")
```

*Ilustração:* Árvore de decisões: TRY -\> Se ValueError / Se
ZeroDivisionError / Se Outra Exceção.

------------------------------------------------------------------------

## 4. O Bloco `else`: Quando Tudo Dá Certo ✅

O bloco `else` (opcional) é executado *apenas* se o código dentro do
bloco `try` for executado sem levantar *nenhuma* exceção.

Ideal para colocar código que depende do sucesso das operações dentro do
`try`.

### Exemplo Prático: Leitura de Arquivo

``` python
try:
    nome_arquivo = "meu_arquivo.txt"
    with open(nome_arquivo, 'r') as arquivo:
        conteudo = arquivo.read()
except FileNotFoundError:
    print(f"Erro: O arquivo '{nome_arquivo}' não encontrado.")
except PermissionError:
    print(f"Erro: Você não tem permissão para ler o arquivo '{nome_arquivo}'.")
else:
    print(f"Arquivo '{nome_arquivo}' lido com sucesso!")
    print(f"Conteúdo:\n{conteudo[:50]}...")
```

*Ilustração:* Caminho bifurcado: TRY -\> ERRO (EXCEPT) / SUCESSO (ELSE).

------------------------------------------------------------------------

## 5. O Bloco `finally`: A Limpeza Essencial 🧹

O bloco `finally` (opcional) contém código que *sempre* será executado,
independentemente de uma exceção ter ocorrido ou não.

É ideal para operações de limpeza, como fechar arquivos, liberar
recursos, etc.

### Exemplo Prático: Fechando um Recurso

``` python
arquivo = None
try:
    arquivo = open("dados.txt", "r")
    conteudo = arquivo.read()
    print("Conteúdo:", conteudo)
except FileNotFoundError:
    print("Erro: Arquivo não encontrado.")
finally:
    if arquivo:
        arquivo.close()
        print("Arquivo fechado (sempre executado).")
```

*Ilustração:* Uma porta de saída que todos precisam passar.

------------------------------------------------------------------------

## 6. Levantando Exceções: `raise` 📢

Às vezes, você precisa *forçar* uma exceção a ocorrer em seu código para
indicar uma condição de erro que deve ser tratada.

Para isso, usamos a palavra-chave `raise`.

### Exemplo Prático: Validando Entrada

``` python
def verificar_idade(idade):
    if not isinstance(idade, int):
        raise TypeError("A idade deve ser um número inteiro.")
    if idade < 0:
        raise ValueError("A idade não pode ser negativa.")
    print("Idade válida!")

try:
    verificar_idade(-5) # Levanta ValueError
except (ValueError, TypeError) as e:
    print(f"Erro ao verificar idade: {e}")
```

*Ilustração:* Um megafone ou sinal de alerta vermelho.

------------------------------------------------------------------------

## 7. Classes de Exceções Comuns no Python 📚

Python possui uma hierarquia rica de classes de exceções embutidas
(built-in exceptions).

Todas as exceções herdam de `BaseException`, e a maioria das que você
usará herdam de `Exception`.

### Alguns Tipos Comuns:

-   **`TypeError`:** Operação com tipo inadequado (ex: `len(123)`).\
-   **`ValueError`:** Tipo certo, mas valor inadequado (ex:
    `int("abc")`).\
-   **`FileNotFoundError`:** Arquivo não encontrado (ex:
    `open("não_existe.txt")`).\
-   **`IndexError`:** Índice fora do alcance (ex: `lista[10]` em lista
    de 3 itens).\
-   **`KeyError`:** Chave não encontrada em dicionário (ex:
    `dict["chave_inválida"]`).\
-   **`AttributeError`:** Atributo/método inexistente em objeto (ex:
    `minha_string.nao_existe()`).\
-   **`NameError`:** Nome (variável/função) não definido (ex:
    `print(var_inexistente)`).\
-   **`ZeroDivisionError`:** Divisão por zero (ex: `10 / 0`).

*Ilustração:* Uma árvore genealógica ou organograma de exceções.

------------------------------------------------------------------------

## 8. Exceções Personalizadas: Seus Próprios Erros 🎨

Para tornar seu código mais legível e para lidar com condições de erro
específicas, você pode criar suas próprias classes de exceção.

Elas devem herdar de `Exception` (ou de uma de suas subclasses).

### Exemplo Prático: Estoque Insuficiente

``` python
class EstoqueInsuficienteError(Exception):
    def __init__(self, produto, disp, sol):
        super().__init__(f"Estoque insuficiente para '{produto}'. Disponível: {disp}, Solicitado: {sol}")

def processar_pedido(item, qtd, estoque):
    if qtd > estoque:
        raise EstoqueInsuficienteError(item, estoque, qtd)
    print(f"Pedido de {qtd} {item} processado.")

try:
    processar_pedido("camiseta", 15, 10) # Levanta EstoqueInsuficienteError
except EstoqueInsuficienteError as e:
    print(f"ERRO DE ESTOQUE: {e}")
```

*Ilustração:* Um molde ou impressora 3D criando um "tipo de erro" único.

------------------------------------------------------------------------

## 9. Boas Práticas no Tratamento de Exceções 💡

-   **Seja Específico:** Capture exceções específicas, não genéricas.\
-   **Não Abafe Erros:** Evite `except: pass`. Faça algo com a exceção.\
-   **Minimize o Bloco `try`:** Coloque apenas o código que *pode*
    falhar.\
-   **Use `else` para Sucesso:** Código que só executa se o `try` for
    bem-sucedido.\
-   **Use `finally` para Limpeza:** Garanta que recursos sejam sempre
    fechados.\
-   **Documente Suas Exceções:** Se criar, explique o que elas
    significam.\
-   **Logs são Seus Amigos:** Use um módulo de logging para registrar
    exceções.

*Ilustração:* Lista de "DOs e DONT's" com ícones.

------------------------------------------------------------------------

## 10. Desafios e Exercícios para a Turma 🧠

Para fixar o conteúdo, propomos alguns desafios:

-   **Calculadora Segura:** Crie uma calculadora que use `try-except`
    para lidar com `ValueError` (entrada não numérica) e
    `ZeroDivisionError`.\
-   **Gerenciador de Tarefas:** Desenvolva um programa que salve e
    carregue tarefas de um arquivo. Lide com `FileNotFoundError`.\
-   **Login Simples:** Crie uma função de login que levante uma exceção
    personalizada `LoginInvalidoError` se as credenciais estiverem
    erradas.

**Pratique para Dominar!**

------------------------------------------------------------------------

# Obrigado! Dúvidas? 🤔

Espero que esta aula tenha sido útil para entender e aplicar o
tratamento de exceções em Python!

Se tiver mais perguntas, estou à disposição.
