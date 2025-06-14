# Referências e Valores em C++

Entender a diferença entre **valores** e **referências** é fundamental para programar bem em C++, especialmente ao trabalhar com Unreal Engine, onde a eficiência e o controle sobre o que está na memória são essenciais.

---

## 📌 Valores

Quando você passa ou atribui **valores** em C++, está criando uma **cópia** do dado original.

```cpp
int a = 10;
int b = a;  // b recebe uma cópia de a
b = 20;
// a ainda é 10
```

Nesse exemplo, `a` e `b` são variáveis independentes na memória. Alterar uma não afeta a outra.

---

## 📌 Referências

Uma **referência** é um **apelido** para uma variável existente. Ao modificar a referência, você modifica a variável original.

```cpp
int a = 10;
int& ref = a; // ref é uma referência para a
ref = 20;
// a agora é 20
```

Não existe "referência nula" em C++ puro: uma referência **sempre aponta para algo válido**.

---

## 🧪 Diferença na Prática

Função com valor (cópia):

```cpp
void Soma(int x) 
{
    x += 10; // modifica a cópia
}
```

Função com referência:

```cpp
void Soma(int& x) 
{
    x += 10; // modifica o original
}
```

---

## ⚠️ Cuidado: Referências const

Você pode passar uma referência **constante** para evitar cópia, mas garantir que a função **não altere** o valor:

```cpp
void Imprime(const std::string& texto) 
{
    std::cout << texto << std::endl;
}
```

Isso é muito usado na Unreal, para evitar cópias de `FStrings`, `FVectors`, etc.

---

## 🧠 Por que isso importa na Unreal?

Na Unreal Engine, é comum ver funções como:

```cpp
void SetName(const FString& NewName);
```

Ao usar `const T&`, você evita copiar grandes estruturas de dados, mas ainda assim garante que o conteúdo original **não será modificado**.

Isso melhora o desempenho e deixa seu código mais seguro.

---

## ✅ Resumo

| Tipo               | O que faz?                           | Performance | Segurança |
|--------------------|---------------------------------------|-------------|-----------|
| `T`                | Cópia do valor                        | ❌          | ✅        |
| `T&`               | Referência mutável                    | ✅          | ⚠️        |
| `const T&`         | Referência imutável (mais comum)      | ✅✅         | ✅✅       |

---

Saber quando usar cada um é um dos primeiros passos para escrever C++ eficiente — especialmente em jogos, onde cada frame conta.
