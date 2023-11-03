# Conhecendo as Ferramentas

Um programador precisa, no mínimo, um **editor de texto** e de um **compilador** (ou interpretador) da linguagem escolhida, instalado sobre o sistema operacional. Além disso, vai precisar de todas as **dependências** (bibliotecas) para o seu programa. É recomendado o uso de uma **IDE** (Ambiente Integrado de Desenvolvimento), pois aumenta consideravelmente a produtividade e o aprendizado. É recomendado também o uso de sistemas de versionamento como **git**.

**Vamos colocar a mão na massa?**

---
### Playground

[Rust Playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021)

### Instalação do Rust + IDE

(Windows)

1. Instale o [VS Code](https://code.visualstudio.com/download) - o uso do VS Code é recomendado.
1. Instale as [Ferramentas de Compilação do Visual Studio](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=16)
1. Instale o Rust (rustc  / cargo) [rust-init.exe (64 bits)](https://win.rustup.rs/x86_64). Neste momento está pronto para seguir o livro.

---

### Instalação do Scrypto

(Windows)

1. Instale o [LLVM](https://github.com/llvm/llvm-project/releases/download/llvmorg-13.0.1/LLVM-13.0.1-win64.exe)
1. Instale o [git](https://gitforwindows.org/).
1. Abra um console novo e digite: `rustup target add wasm32-unknown-unknown`
1. Instale s simulador:

```cmd
git clone https://github.com/radixdlt/radixdlt-scrypto.git
cd radixdlt-scrypto
cargo install --path ./simulator
```

---
# Conceitos da Linguagem

### - Declarações (*statements*).
### - Expressões (*expressions*).
### - Escopo.


------
#### Hello World

```Rust
fn main(){
    println!("Hello World!");
}
```

#### - Declaração de função:
```Rust
fn nome_da_funcao() { expressao }
```
#### - Função “main”.
#### - Escopo {}.
#### - Macro println!

------
### Macro *println!* 

#### - Formatando impressão:
```Rust
println!("Meu nome é {} e eu tenho {}", "Leo", 29);
```
#### - Expressão:
```Rust
println!("a + b = {}", 3+6);
```
#### - Posicional:
```Rust
println!("{0} tem um {2} e {0} tem um {1}", "Leo", "gato", "cachorro");
```
#### - Alias (apelido):
```Rust
println!("{nome} {sobrenome}", sobrenome="Magal", nome="Leo");
```
#### - Características de impressão:
```Rust
println!("binary: {:b}, Hex: {:x}, Octal: {:o}", 5, 5, 5);
```
#### - Característica Debug:
```Rust
println!("Array {:?}", [1, 2, 3]);
```

------
# Conceitos da Linguagem

### - Comentários.
### - Variáveis.
### - Tipos de dados.

### Comentários no código

#### Comentário de uma linha
```Rust
// Isto é um comentário de uma linha
```
#### Comentários em várias linhas
```Rust
/* Isso funciona mas 
Não é muito comum */
```
#### Documentando funcionalidade
```Rust
/// Isso é mais usado para documentar funcionalidade
```

------
#### Documentação de libs pelos comentários

```Rust
//! Isto é usado principalmente para documentar crates (bibliotecas)

//! # Main heading

//! ```
//! fn main() {...}
//! ```
```
https://github.com/rust-lang/rfcs/blob/master/text/1574-more-api-documentation-conventions.md


### Variáveis

#### - Declarando usando *let*
```Rust
let nome;
```
#### - Declaração com anotação de tipo
```Rust
let idade:i32;
```
#### - Declaração e atribuição
```Rust
let nome = "Leo";
let idade = 32;
```
#### - Rust é uma linguagem de tipagem forte, mas infere o tipo caso não seja declarado explicitamente.
```Rust
let amount:i64 = 8473926758472;
let amount = 8473926758472;  // error
```
#### - Nomes podem conter letras, números e _
#### - Tem que começar com uma letra ou um _
#### - Seguem a convenção snake_case
#### - Imutáveis por padrão:
```Rust
let idade = 34;
idade = 35;			// error
```
#### - Declarando mutabilidade de variáveis:
```Rust
let mut idade = 34;
idade = 35;			// ok
```
#### - *Shadowing*:
```Rust
let x = 5;
let x = x + 1;

{
    let x = x * 2;
    println!("O valor de x no escopo interno é: {}", x);
}

println!("O valor de x é: {}", x);
```
####    - Diferença entre *shadowing* e mutabilidade:
```Rust
// este código compila
let spaces = "   ";
let spaces = spaces.len(); // ok
// este código não compila
let mut spaces = "   ";
spaces = spaces.len(); // erro
```
####    - Declarando várias variáveis simultaneamente:
```Rust
let (a, b, c) = (2, 3, 4);
```


-----
### Tipos de dados
#### - Rust é *estaticamente tipada*, o que significa que o compilador tem que saber todos os tipos na compilagem
```Rust
let number = "42".parse().expect("Not a number!"); // erro
let number: u32 = "42".parse().expect("Not a number!"); // ok
``` 
#### - Tipos escalares:
Um tipo escalar representa um valor, número inteiro, número ponto flutuante, booleano ou caracter. 
####    - Inteiros:
|Length|Signed|Unsigned|
|----|----|----|
|8-bit|i8|u8|
|16-bit|i16|u16|
|32-bit|i32|u32|
|64-bit|i64|u64|
|128-bit|i128|u128|
|arch|isize|usize|

#### - Literais de inteiros:
##### sufixo de tipo: `57u8`
##### separador _ : `1_000` é o mesmo que `1000`

|Number literals|Example|
|----|----|
|Decimal|98_222|
|Hex|0xff|
|Octal|0o77|
|Binary|0b1111_0000|
|Byte (u8 only)|b'A'|

#### - Ponto Flutuante:
```Rust
let x = 2.0; // f64
let y: f32 = 3.0; // f32
let z: f32 = 2; // mismatched types error
```

#### - Booleano:
```Rust
let t = true;
let f: bool = false; // com anotação explícita
```

#### - Caracter:
```Rust
let c = 'z';
let z = 'ℤ';
let emoji_gatinho = '😻';
```

### - Tipos compostos:
#### - Tupla:
```Rust
let tupla: (i32, f64, u8) = (500, 6.4, 1);
```
##### Desestruturando a tupla:
```Rust
let tupla = (500, 6.4, 1);
let (x, y, z) = tupla;
println!("O valor de y é: {}", y);
```
##### Acessando valores dentro de uma tupla:
```Rust
let x: (i32, f64, u8) = (500, 6.4, 1);
let quinhentos = x.0;
let seis_ponto_quatro = x.1;
let um = x.2;
```
#### - Array:
```Rust
let a = [1, 2, 3, 4, 5];
let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
let a: [i32; 5] = [1, 2, 3, 4, 5];
let a = [3; 5]; // let a = [3, 3, 3, 3, 3];
```

#####    Acessando valores dentro de uma array:
```Rust
fn main() {
    let a = [1, 2, 3, 4, 5];
    let first = a[0];
    let second = a[1];
}
```
#####   Garantias de memória da Rust
#####   -> Rust abandona o programa e não permite acesso inválido à memória
```Rust
use std::io;

fn main() {
    let a = [1, 2, 3, 4, 5];

    println!("Please enter an array index.");

    let mut index = String::new();

    io::stdin()
        .read_line(&mut index)
        .expect("Failed to read line");

    let index: usize = index
        .trim()
        .parse()
        .expect("Index entered was not a number");

    let element = a[index];

    println!(
        "The value of the element at index {} is: {}",
        index, element
    );
}
```
-----
# Conceitos da Linguagem

### - Operadores
### - Strings

-----
### Operadores
#### Aritméticos
######   Operações numéricas estão implantadas em todos os tipos numéricos primitivos:
```Rust
fn main() {
    // adição
    let sum = 5 + 10;
    // subtração
    let difference = 95.5 - 4.3;
    // multiplicação
    let product = 4 * 30;
    // divisão
    let quotient = 56.7 / 32.2;
    let floored = 2 / 3; // Results in 0
    // resto
    let remainder = 43 % 5;
    // incremeno (++) ou decremento (--) não são suportados em Rust
}
```
#### Relacionais
|Operador|Nome|Exemplo|
|---|---|---|
|>|maior que|10>3=true|
|>=|maior ou igual que|"A" >= "a" = false|
|<|menor que|10<3=false|
|<=|menor ou igual que|true <= false = false|
|==|igual a|3.0 == 3.1 = false|
|!=|diferente de|'c' != 'C' = true|


#### Lógicos
|Operador|Nome|Exemplo|
|---|---|---|
|&&|AND|true && true = true|
|\|\||OR| true \|\| false = true|
|!|NOT|!true = false|

#### Bitwise Operators (&, |, ^, !, <<, >>)


----
### Strings

#### - Dois tipos:
#####    - *slices* (fatia) / string literal - é um valor passado por referência (emprestado) => imutáveis.
```Rust
let gato: &str = "Luke";
``` 
#####    - Objeto String
```Rust
let gato = String::new();
let mut gato = String::from(“Luke”); // objeto String pode ser mutável
let cachorro = "Rex".to_string();
```
##### Métodos no objeto String:
###### len
```Rust
println!("{}", gato.len());  // 4
```
###### push e push_str
```Rust
gato.push(' ');
gato.push_str("Skywalker");
println!("meu gato se chama {}", gato) // meu gato se chama Luke Skywalker
```
###### replace
```Rust
let gato = gato.replace("Skywalker","Jaguatirica");
println!("meu gato se chama {}", gato) // meu gato se chama Luke Jaguatirica
```
###### outros métodos (consultar documentação):
- split
- split_whitespace
- chars

----
### Constantes
##### Valores não podem ser alterados
```Rust
const URL: &str = "google.com";
```
##### Caixa alta por convenção
##### Anotação de tipo é mandatória
##### Shadowing é proibido
##### Podem ter escopo local ou global

----
### Controle de fluxo
##### - if
```Rust
if logical_expression {
   //functionality for true
}
```
##### - if else
```Rust
if logical_expression {
   //functionality for true
} else {
   //functionality for false
}
```
### - if else if
```Rust
if expression1 {
   //functionality for expression1 true
} else if expression2 {
   //functionality for expression1 false and expression2 true
} else {
   //functionality for both expressions false
}
```
### - if com retorno
```Rust
let res = if expr1 {
   1 //result for true = 1
} else {
   2 //result for false = 2
}
```
### - Loop for
####    Faz um loop em uma collection ou range, executando a funcionalidade para cada elemento.
```Rust
for element in collection {
   functionality
}
``` 
####    Continue pula uma iteração
####    Break sai do loop
####    Exemplo:    
```Rust
fn main() {
    for i in 1..11 {
        println!("{0} * {0} = {1}", i, i * i);
    }

    let pets = ["cat", "dog", "chihuahua", "bear", "hamster"];
    for pet in pets.iter() {
        if pet == &"chihuahua" {
            println!("{} barks too much", pet);
            continue
        }
        if pet == &"bear" {
            println!("{} is not a pet", pet);
            break
        }
        println!("I love my {}", pet);
    }

    for (pos, i) in (1..11).enumerate() {
        let square = i * i;
        let nb = pos + 1;
        println!("{0} * {0} = {1} ", nb, square);
    }
}
```
### Loop while
#### Executa o loop enquanto a condição for verdadeira
```Rust
while condition {
   ...
}
loop { // while true {
    if condition {break}
}
```
####    Continue pula uma iteração
####    Break sai do loop
### Match
```Rust
match expression {
   expr1 => { … }
   expr2 => { … }
   _ => { … }  // catch-all pattern
}
```
#### Similar a *when* ou *switch* em outras linguagens
#### Necessita ser exaustivo nas condições
#### Pode retornar um valor
#### Permitido o uso de ranges

----
# Funções