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
- Declarações
- Expressões
- Escopos
- Comentários
- E/S
- Variáveis e mutabilidade
- Tipo de dados
- Operadores
- Controle de Fluxo
- Funções
- 

-----
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

------
### Macro *println!* 

#### - Formatando impressão:
```Rust
fn main() {
    println!("Meu nome é {} e eu tenho {}", "Leo", 29);   
}
```
#### - Expressão:
```Rust
fn main() {
    println!("a + b = {}", 3+6);    
}
```
#### - Posicional:
```Rust
fn main() {
    println!("{0} tem um {2} e {0} tem um {1}", "Leo", "gato", "cachorro");
}
```
#### - Alias (apelido):
```Rust
fn main() {
    println!("{nome} {sobrenome}", sobrenome="Magal", nome="Leo");
}
```
#### - Traits de impressão:
```Rust
fn main() {
    println!("binary: {:b}, Hex: {:x}, Octal: {:o}", 5, 5, 5);    
}
```
#### - Trait Debug:
```Rust
main() {
    println!("Array {:?}", [1, 2, 3]);    
}
```
------
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
fn main() {
    let nome;    
}
```
#### - Declaração com anotação de tipo
```Rust
fn main() {
    let idade:i32;   
}
```
#### - Declaração e atribuição
```Rust
fn main() {
    let nome = "Leo";
    let idade = 32;    
}
```
#### - Rust é uma linguagem de tipagem forte, mas infere o tipo caso não seja declarado explicitamente.
```Rust
fn main() {
    let amount:i64 = 8473926758472;
    let amount = 8473926758472;  // error    
}
```
#### - Nomes de váriáveis: 
- Podem conter letras, números e _
- Tem que começar com uma letra ou um _
- Seguem a convenção snake_case
#### - Imutáveis por padrão:
```Rust
fn main() {
    let idade = 34;
    idade = 35;			// error    
}
```
#### - Declarando mutabilidade de variáveis:
```Rust
fn main() {
    let mut idade = 34;
    idade = 35;			// ok    
}
```
#### - *Shadowing*:
```Rust
fn main() {
    let x = 5;
    let x = x + 1;

    {
        let x = x * 2;
        println!("O valor de x no escopo interno é: {}", x);
    }
    println!("O valor de x é: {}", x);    
}

```
####    - Diferença entre *shadowing* e mutabilidade:
```Rust
// este código compila
fn main() {
    let spaces = "   ";
    let spaces = spaces.len(); // ok    
}
// este código não compila
fn main() { 
    let mut spaces = "   ";
    spaces = spaces.len(); // erro 
}
```
####    - Declarando várias variáveis simultaneamente:
```Rust
fn main() {
    let (a, b, c) = (2, 3, 4);
    println!("a = {}, b = {}, c = {}", a, b, c);    
}
```


-----
### Tipos de dados
#### - Rust é *estaticamente tipada*, o que significa que o compilador tem que saber todos os tipos na compilagem
```Rust
fn main() {
    let number = "42".parse().expect("Not a number!"); // erro
    let number: u32 = "42".parse().expect("Not a number!"); // ok    
}
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
fn main() {
    let x = 2.0; // f64
    let y: f32 = 3.0; // f32
    let z: f32 = 2; // mismatched types error    
}
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
### Escopo e posse de variáveis
#### Escopo
###### {} define um bloco de código. 
```Rust
fn main() {
    let x = 5;
    let y = {
        let x = 3; // este x só existe dentro
        x + 1 // este x será destruído
    };
    println!("O valor de y é: {}", y);
    println!("O valor de x é: {}", x); // x será destruído
}
```

###### Um bloco é uma ou mais expressões que retornam um valor.
```Rust
fn main() {
    // let resposta = 42;
    let resposta = { 42 }; // isto é equivalente
    println!("A resposta é {}", resposta);
    let tres_ao_quadrado = {
        let y = 3;
        y * y // repare na ausência da ; no final da última expressão => retorno
    };
    println!("Três ao quadrado é {}", tres_ao_quadrado);
    let nulo = {};
    println!("O sentido da vida é {:?}", nulo)
}
```

---
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
### Posse de Variáveis
#### Heap vs Pilha
- Tipos primitivos tem tamanho conhecido em tempo de compilação, são armazenados na pilha e implementam o trait `Copy` (cópia rasa). 
- Tipos com tamanho desconhecido em tempo de compilação implementam o trait `Clone` ("cópia profunda"), logo não podem ser implicitamente copiados.
- Quando uma função recebe uma variável passada por valor, ela toma posse dessa variável, que é destruída ao fim do seu escopo (logo não podem ser referenciadas novamente).
```Rust
fn print_num(n: i32) {
    println!("{} number", n);
}

fn print_string(s: String) {
    println!("string: {}", s);
}

fn print_str(s: &str) {
    println!("str: {}", s);
}

fn main() {
    let n: i32 = 42;
    print_num(n); // `n` é copiado
    print_num(n); // ok
    let s = "Magal";
    print_str(s); // s é copiado
    print_str(s); // ok
    let s = String::from("Magal");
    let s2 = s.clone(); // s é clonado para s2
    print_string(s); // s é movido
    // print_string(s); //isso não compila
    print_string(s2); // ok
}
```

---
### Constantes
##### Valores não podem ser alterados
##### Caixa alta por convenção
##### Anotação de tipo é mandatória
##### Shadowing é proibido
##### Podem ter escopo local ou global
###### Variáveis globais (global scope)
```Rust
const PI: f32 = 3.141592; // constantes declaradas fora de qualquer função
//acessíceis dentro de qualquer escopo do projeto

fn main() {
    println!("O valor de PI é: {}", PI); 
}

fn perimetro(raio: f32) -> f32 {
    PI * 2.0 * raio 
}

fn area(raio: f32) -> f32 {
    PI * raio * raio
}

```
----
### Controle de fluxo
##### - if
```Rust
if expressao_logica {
   // funcionalidade esperada para true
}
```
##### - if else
```Rust
if expressao_logica {
   //funcionalidade esperada para true
} else {
   //funcionalidade esperada para flase
}
```
##### - if else if
```Rust
if expressao1 {
   //funcionalidade esperada para expressao1 true
} else if expressao2 {
   //funcionalidade esperada para expressao1 true and expressao2 true
} else {
   //funcionalidade esperada para ambas falsas
}
```
##### - if com retorno
```Rust
let res = if expr1 {
   1 //result true = 1
} else {
   2 //result false = 2
}
```
### - Loop for
####    Faz um loop em uma coleção, executando a funcionalidade para cada elemento.
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

    let pets = ["cat", "dog", "chihuahua", "onça", "hamster"];
    for pet in pets.iter() {
        if pet == &"chihuahua" {
            println!("{} late muito", pet);
            continue
        }
        if pet == &"onça" {
            println!("{} não é um pet", pet);
            break
        }
        println!("Eu ❤️ meu {}", pet);
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
### Funções
#### - Declaração de função:
```Rust
fn nome_da_funcao(parametro: tipo) -> tipo { expressao }
```
#### - Passada por valor
```Rust
#[allow(unused_mut)]
fn main() {
    let mut manoel = String::from("Manoel");
    modifica_in_place(manoel);
    // println!("string: {}", manoel); // não compila
    let manoel = String::from("Manoel");
    let manoel = nao_modifica_mas_retorna(manoel);
    println!("string: {}", manoel); // ok
}

fn modifica_in_place(mut s: String) { 
    s.push_str(" e Joaquim");
    println!("string: {}", s);
}

fn nao_modifica_mas_retorna (s: String) -> String {
    let mut ss = s.clone(); // clona
    ss.push_str(" e Joaquim"); // modifica in place
    ss // retorna
}
```
#### - Passada por referencia
```Rust
fn main() {
    let manoel = String::from("Manoel");
    let a = primeira_letra(&manoel);
    println!("a primeira letra de {} é: {}", manoel, a);
}

fn primeira_letra(s: &String) -> char {
    s.chars().next().unwrap()
}
```
### Referências (ponteiros) e Empréstimo (borrowing)
- Referenciar em Rust é emprestar, pois a função não toma posse da variável e não pode *dropar* o que não possui.
- São imutáveis por padrão, mutabilidade tem que ser explícita.
```Rust
fn main() {
    let mut s = String::from("texto");

    modifica(&mut s);
}

fn modifica(uma_string: &mut String) {
    uma_string.push_str(" longo");
}
```
- Podem ser dereferenciados com o operador `*` (não recomendado)
```Rust
#[allow(unused_mut)]
fn main() {
    let mut nome: &str = "Magal";
    diga_ola(&mut nome);
    println!("{}", nome);
}
fn diga_ola(s: &mut &str) {
    println!("Olá {}", *s);
    if *s == "Magal" { *s = "Leo";}
}
```
- Em um dado momento, vc pode ter um ou outro, mas não os dois:
    - uma referência mutavel
    - um número ilimitado de referências imutáveis.
```Rust
let mut s = String::from("texto");

let r1 = &mut s;
let r2 = &mut s;
-------------
error[E0499]: cannot borrow `s` as mutable more than once at a time
 --> main.rs:5:19
  |
4 |     let r1 = &mut s;
  |                   - first mutable borrow occurs here
5 |     let r2 = &mut s;
  |                   ^ second mutable borrow occurs here
6 | }
  | - first borrow ends here
```
```Rust
let mut s = String::from("texto");

let r1 = &s; // sem problema
let r2 = &s; // sem problema
let r3 = &mut s; // PROBLEMA GRANDE
-------------
error[E0502]: cannot borrow `s` as mutable because it is also borrowed as
immutable
 --> main.rs:6:19
  |
4 |     let r1 = &s; // sem problema
  |               - immutable borrow occurs here
5 |     let r2 = &s; // sem problema
6 |     let r3 = &mut s; // PROBLEMA GRANDE
  |                   ^ mutable borrow occurs here
7 | }
  | - immutable borrow ends here
```
- Referências tem que ser sempre válidas.
```Rust
fn main() {
    let referencia_para_o_nada = soltar();
}

fn soltar() -> &String {
    let s = String::from("texto");

    &s
}
-------------
error[E0106]: missing lifetime specifier
 --> main.rs:5:16
  |
5 | fn soltar() -> &String {
  |                ^ expected lifetime parameter
  |
  = help: this function's return type contains a borrowed value, but there is
  no value for it to be borrowed from
  = help: consider giving it a 'static lifetime
```