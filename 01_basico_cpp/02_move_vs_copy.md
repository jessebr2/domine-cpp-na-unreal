# Move vs Copy em C++

Um dos recursos mais poderosos (e subestimados) do C++ moderno é **semântica de movimento** — ou seja, a capacidade de **mover** recursos em vez de copiá-los. Isso é essencial para otimizar performance, especialmente em jogos.

---

## 📋 Cópia: `T(const T&)`

Quando você copia um objeto, o conteúdo completo é duplicado. Isso pode ser custoso:

```cpp
std::string a = "texto muito grande";
std::string b = a;  // cópia
```

Neste caso, `b` faz uma cópia do conteúdo de `a`. Isso implica alocação de memória, duplicação de dados, etc.

---

## 🚚 Movimento: `T(T&&)`

Ao mover um objeto, você transfere seus **recursos internos** (ponteiros, buffers, etc) para outro objeto — sem cópia.

```cpp
std::string a = "texto muito grande";
std::string b = std::move(a);  // movimento
```

Após o `std::move`, o conteúdo de `a` é "roubado" por `b`. `a` fica em um estado **válido mas não definido** (normalmente, vazio). Se voçê acessar a não terá uma excessão, porém não pode confiar no valor que encontrará lá, já que agora ele está indefinido.

---

## 🛠 Quando usar `std::move`?

Sempre que você **não precisar mais de um objeto**, pode movê-lo:

```cpp
void Process(std::string text);
Process(std::move(name));
```

Em vez de copiar `name`, você move os dados internamente. Isso evita duplicação e é mais rápido.

---

## 🧠 Por que isso importa na Unreal?

A Unreal fornece a macro `MoveTemp()` para facilitar o movimento de objetos temporários ou não mais usados:

```cpp
void SetName(FString&& InName) 
{
    Name = MoveTemp(InName);
}
```

Isso é mais eficiente do que copiar `InName` para `Name`.

---

## ⚠️ Cuidado!

- Nunca use `std::move` em objetos que você **ainda vai usar** depois.
- Mover objetos altera seu estado — use com consciência.
- Não abuse: mover é útil, mas nem sempre necessário.

---

## ✅ Resumo

| Operação   | Semântica | Custo         | Uso típico                         |
|------------|-----------|---------------|------------------------------------|
| Cópia      | `T(const T&)` | Alto (duplica) | Quando você **precisa da cópia**   |
| Movimento  | `T(T&&)`      | Baixo (transfere) | Quando **não vai mais usar o original** |

---

### Pratique

- Veja onde você pode usar `std::move` para otimizar objetos temporários.
- Analise funções da Unreal que recebem `T&&` ou usam `MoveTemp`.

Cada detalhe desses pode fazer diferença em jogos de alta performance!
