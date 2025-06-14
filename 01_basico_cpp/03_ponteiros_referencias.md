# Ponteiros e Referências em C++

Em C++, tanto **ponteiros** quanto **referências** são formas de acessar variáveis de forma indireta. Embora tenham semelhanças, há diferenças importantes no uso, segurança e flexibilidade de cada um.

---

## 🔗 Referências

Uma referência é um **apelido** para uma variável existente. Ela **não pode ser nula** e **não pode ser trocada** depois de inicializada.

```cpp
int value = 10;
int& alias = value; // alias é um apelido para value
alias = 20;
// agora value é 20 também
```

Quando dizemos que uma referência não pode ser trocada, queremos dizer que depois que uma referência é inicializada para se referir a uma variável específica, ela não pode mais apontar para outra variável.
```cpp
int a = 10;
int b = 20;

int& ref = a;  // ref agora é um apelido para 'a'
ref = b;       // ⚠️ isso NÃO troca o alvo da referência!
               // Isso apenas atribui o valor de 'b' a 'a'
```
Referências são ideais quando você quer passar variáveis para funções **sem cópia**, mas com segurança.

---

## 🔗 Ponteiros

Um ponteiro armazena o **endereço** de memória de uma variável. Pode ser nulo e pode apontar para diferentes lugares ao longo do tempo.

```cpp
int value = 10;
int* ptr = &value; // ptr aponta para value
*ptr = 20;
// agora value é 20

int b = 30;
ptr = &b;  // Agora ptr aponta para b
```

Ponteiros são mais flexíveis, mas exigem cuidado com **endereços inválidos** ou **acessos nulos**.

---

## 🧠 Comparativo Rápido

| Aspecto             | Ponteiro (`T*`) | Referência (`T&`) |
|---------------------|------------------|--------------------|
| Pode ser nulo       | Sim              | Não                |
| Pode ser reassociado| Sim              | Não                |
| Uso em arrays        | Sim              | Não diretamente     |
| Sintaxe              | Mais verbosa     | Mais limpa          |

---

## 📌 Quando usar cada um?

- **Use referência (`T&` ou `const T&`)** quando quiser evitar cópia, mas manter segurança.
- **Use ponteiro (`T*`)** quando precisar de:
  - valor nulo como sinal de ausência
  - alterar para onde está apontando
  - trabalhar com arrays ou buffers manuais

---

## 🧪 Exemplo de uso em função

### Referência:

```cpp
void ApplyDamage(int& health)
{
    health -= 10;
}
```

### Ponteiro:

```cpp
void ApplyDamage(int* health)
{
    if (health != nullptr)
    {
        *health -= 10;
    }
}
```

---

## 🧠 E na Unreal Engine?

- Referência é amplamente usada em funções como:

```cpp
void SetActorLocation(const FVector& NewLocation);
```

- Ponteiros são usados em funções que podem receber `nullptr`, como:

```cpp
void Possess(APawn* InPawn);
```

---

## ✅ Resumo

- Use **referência** quando o valor **deve existir** e não será trocado.
- Use **ponteiro** quando o valor **pode ser nulo** ou você precisa manipular o endereço diretamente.
