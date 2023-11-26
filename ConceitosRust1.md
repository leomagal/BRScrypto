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
### Tipos primitivos de dados
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
#### - Array (estática):
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
### Loop for
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
```Rust
fn main() {
    let x_ref = {
        let x = 42;
        &x
    };
    println!("x_ref = {}", x_ref);
    // error: `x` does not live long enough
}
```
---
### Struct

Tipo de dado complexo capaz de encapsular campos nomeados com de diferentes tipos, além de métodos e funções associadas:
```Rust
struct Pessoa {
    nome: String,
    idade: u8
}

#[allow(unused_mut)]
fn main() {
  let mut fulano = Pessoa {
      nome: String::from("Fulano"),
      idade: 29
  };

  println!("{} tem {} anos", fulano.nome, fulano.idade);
}
```

Abreviatura da inicialização dos campos quando as variáveis têm o mesmo nome dos campos:
```Rust
fn build_user(email: String, username: String) -> User {
    User {
        email,  // email: email,
        username, //username: username,
        active: true,
        sign_in_count: 1,
    }
}
```

Criação de instâncias de outras instâncias com Sintaxe de Atualização
da Struct (Struct Update Syntax):

```Rust
#![allow(unused)]
fn main() {
    struct User {
        username: String,
        email: String,
        sign_in_count: u64,
        active: bool,
    }

    let user1 = User {
        email: String::from("someone@example.com"),
        username: String::from("someusername123"),
        active: true,
        sign_in_count: 1,
    };

    let user2 = User {
        email: String::from("another@example.com"),
        username: String::from("anotherusername567"),
        ..user1
    };
}
```

Estruturas sem campos nomeados (*Tuple Struct*)
```Rust
#![allow(unused)]
fn main() {
    struct Cor(i32, i32, i32);
    struct Ponto(i32, i32, i32);

    let preto = Cor(0, 0, 0);
    let origem = Ponto(0, 0, 0);
}
``` 

Estruturas não tem o *trait Display* implementado por padrão, é necessário derivar do *trait Debug*:
```Rust
#[derive(Debug)]
struct Retangulo {
    comprimento: u32,
    largura: u32,
}

fn main() {
    let rect1 = Retangulo { comprimento: 50, largura: 30 };

    println!("rect1 é {:?}", rect1);
}
```

Definindo métodos:
```Rust
struct Retangulo {
    comprimento: u32,
    largura: u32,
}

impl Retangulo {
    fn area(&self) -> u32 {
        self.comprimento * self.largura
    }
}
```
Outro recurso útil dos blocos impl é que podemos definir funções dentro dos blocos impl que não recebem self como um parâmetro. Estas são chamadas de funções associadas porque elas estão associados com a struct. Elas ainda são funções, não métodos, porque elas não têm uma instância da struct para trabalhar. Você já usou a função associada String::from.

Funções associadas são usados frequentemente para construtores que retornam uma nova instância da struct.
```Rust
impl Rectangle {
    fn square(size: u32) -> Rectangle {
        Rectangle { length: size, width: size }
    }
}
```

---
### Enums

Enums permitem definir um tipo por meio da enumeração de seus possíveis tipos/valores, determinados variantes:
```Rust
enum Network {
    Olympia,
    Babylon
}

fn main() {
    let network = Network::Babylon;
    match network {
        Network::Olympia => println!("Olympia"),
        Network::Babylon => println!("Babylon"),
    }
}
```

Enums também podem ter dados associados às suas variantes e métodos definidos em blocos `impl`:
```Rust
#![allow(unused)]
enum Mensagem {
    Sair,
    Mover { x: i32, y: i32 },
    Escrever(String),
    MudarCor(i32, i32, i32),
}

impl Mensagem {
    fn invocar(&self) {
        match self {
            Mensagem::Escrever(mensagem) => {
                println!("{}", mensagem)
            },
            _ => {}
        }
    }
}

fn main() {
    let m = Mensagem::Escrever(String::from("olá"));
    m.invocar();
}
```
---
#### Option\<T\>

Enum `Option<T>` é uma enum definida pela biblioteca padrão. O tipo Option é muito utilizado, pois engloba um cenário muito comum, em que um valor pode ser algo ou pode não ser nada, já que Rust não tem o valor nulo (null).
```Rust
enum Option<T> {
    None,
    Some(T),
}
```
A enum `Option<T>` é tão útil que ela já vem inclusa no prelúdio: você não precisa trazê-la explicitamente para o seu escopo. Além disso, o mesmo ocorre com suas variantes: você pode usar `Some` e `None` diretamente sem prefixá-las com `Option::`. `Option<T>` continua sendo uma enum como qualquer outra, e `Some(T)` e `None` ainda são variantes do tipo `Option<T>`.

A sintaxe do `<T>`` é um parâmetro de tipo genérico. Por ora, tudo que você precisa saber é que `<T>` significa que a variante `Some` da enum `Option` pode conter um dado de qualquer tipo.

##### Controle de fluxo com if let

Digamos que eu tenha uma variavel que armazena algum valor inteiro:<br>
```Rust
let algum_valor_u8 = Some(0u8);
```

O bloco de código a seguir execvuta algo apenas se o valor é igual a três:
```Rust
match algum_valor_u8 {
    Some(3) => println!("O valor é 3"),
    _ => (),
}
```

E este bloco é absolutamente equivalente ao anterior porém mais conciso:
```Rust
if let Some(3) = algum_valor_u8 {
    println!("O valor é 3");
}
```
---
#### Result\<T, E\>

Outro enum incluso no prelúdio, muito útil no tratamento de erros. 
```Rust
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let f = File::open("hello.txt");

    let f = match f {
        Ok(file) => file,
        Err(ref error) if error.kind() == ErrorKind::NotFound => {
            match File::create("hello.txt") {
                Ok(fc) => fc,
                Err(e) => {
                    panic!(
                        "Tentou criar um arquivo e houve um problema: {:?}",
                        e
                    )
                },
            }
        },
        Err(error) => {
            panic!(
                "Houve um problema ao abrir o arquivo: {:?}",
                error
            )
        },
    };
}
```
A condição `if error.kind() == ErrorKind::NotFound` é chamada de um *match guard*: é uma condição extra dentro de uma linha de `match` que posteriormente refina o padrão da linha. Essa condição deve ser verdadeira para o código da linha ser executado; caso contrário a análise de padrões vai continuar considerando as próximas linhas no `match`. O `ref` no padrão é necessário para que o `error` não seja movido para a condição do guard, mas meramente referenciado por ele (no contexto de um padrão, `&` corresponde a uma referência e nos dá seu valor, enquanto `ref` corresponde a um valor e nos dá uma referência a ele).

Embora o uso de padrões match com Result seja útil para erros recuperáveis, há atalhos para o panic:
```Rust
use std::fs::File;

fn main() {
    let f = File::open("hello.txt").unwrap();
}
```

ou:
```Rust
use std::fs::File;

fn main() {
    let f = File::open("hello.txt").expect("Falhou ao abrir hello.txt");
}
```

O uso do enum `Result` facilita a propagação de erros por funções:
```Rust
#![allow(unused)]
use std::io;
use std::io::Read;
use std::fs::File;

fn read_username_from_file() -> Result<String, io::Error> {
    let f = File::open("hello.txt");

    let mut f = match f {
        Ok(file) => file,
        Err(e) => return Err(e),
    };

    let mut s = String::new();

    match f.read_to_string(&mut s) {
        Ok(_) => Ok(s),
        Err(e) => Err(e),
    }
}
```

O operador `?` pode ser usado como atalho, ao invés do `match`:
```Rust
#![allow(unused)]
use std::io;
use std::io::Read;
use std::fs::File;

fn read_username_from_file() -> Result<String, io::Error> {
    let mut f = File::open("hello.txt")?;
    let mut s = String::new();
    f.read_to_string(&mut s)?;
    Ok(s)
}
```

---
### Coleções
Coleções são variáveis que podem conter múltiplos valores. Diferente dos tipos embutidos array e tupla, os dados que essas coleções apontam estão guardados na heap, que significa que a quantidade de dados não precisa ser conhecida em tempo de compilação e pode aumentar ou diminuir conforme a execução do programa. Vetores, HasshMaps e Strings são exemplos de coleções.

#### Vetores
Criando um novo vetor:
```Rust
let v: Vec<i32> = Vec::new();
let v = vec![1, 2, 3];
```

Modificando um vetor:
```Rust
let mut v = vec![1, 2, 3];
v.push(4);
```

Lendo elementos do vetor:
```Rust
let v = vec![1, 2, 3, 4, 5];
let terceiro: &i32 = &v[2];
let terceiro: Option<&i32> = v.get(2);

let nao_existe = &v[100]; // pânico
let nao_existe = v.get(100);  // possibilidade de tratar o erro
```

Fatiando um vetor:
```Rust
let v = vec![1,2,3,4,5,6,7,8,9,10];
let de_dois_a_cinco: &i32 = &v[1..5];
let de_um_a_seis: &i32 = &v[..6];
let de_um_a_seis_alternativa: &[i32] = &v[..5=];
let de_dois_ao_ultimo: &[i32] = &v[1..];
let emppresta_o_vetor_todo: &[i32] = &v[..];
```

Iterando Valores do Vetor:
```Rust
let v = vec![100, 32, 57];
for i in &v {
    println!("{}", i);
}
```

#### Strings
Strings são complexas e cada linguagem as implementa de forma diferente. Rust implementa strigs como coleções de caracteres UTF-8m, similar a um vetor, mas com algumas ressalvas. Ao contrário de tipos primitivos como i32, etc, o caractere utf-8 não tem um tamanho fixo (pode variar de um a quatro bytes). Além disso, alguns alfabetos tem caracteres que se fundem em outros. Por conta disso, **indexar strings em Rust é ilegal**. Rust obriga o programador a tomar decisões logo no início do processo de desenvolvimento em relação aos caracteres UTF-8, de forma a evitar erros futuros com os caracteres não ASCII. Feliemente, o tipo String tem já uma série de métodos para todo tipo de manipulação.

Indexar strings é ilegal, mas fatiar strings é permitido:
```Rust
let mut s = String::from("hello world");
// let h = &s[0]; Isto é ilegal, não compila
let world = &s[6..11]; // ok
``` 

Mas pode gerar um panic:
```Rust
let s = String::from("Здравствуйте");
let hello = &s[0..4];
```
```
$ cargo run
   Compiling collections v0.1.0 (file:///projects/collections)
    Finished dev [unoptimized + debuginfo] target(s) in 0.43s
     Running `target/debug/collections`
thread 'main' panicked at 'byte index 1 is not a char boundary; it is inside 'З' (bytes 0..2) of `Здравствуйте`', src/main.rs:4:14
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

Ao invés de indexar uma String, vc pode iterar seus caracteres e localizar os índices usando métodos de fatiamento:
```Rust
let s = String::from("hello");
let third_character = s.chars().nth(1);
assert_eq!(third_character, Some('e'));
let s = "💖😍💖💖💖";
let third_character = s.chars().nth(1);
assert_eq!(third_character, Some('😍'));
```

Vc pode converter literais de string ou fatias em objetos String:
```Rust
let my_string = String::from("hello");
let my_string_literal = "hello";
let mystring = my_string_literal.to_string();
let mystring = "hello".to_string();
```

Vc pode "somar" (concatenar) strings com operador +:
```Rust
let s1 = String::from("Hello, ");
let s2 = String::from("world!");
let s3 = format!("{s1}{s2}");
let s4 = s1 + &s2; //s1 será movido e não poderá mais ser usado
// println!("{s1"); não funcionaria aqui 
println!("s2 = {s2}");
println!("\"{s3}\" = \"{s4}\"");
```

Vale estudar os métodos `chars()` e `bytes()` como também dar uma olhada nos inúmeros métodos disponíveis para o tipo String:
(Documentação completa do objeto String)[https://doc.rust-lang.org/beta/std/string/struct.String.html]

#### HashMap<K,V>
Hasmaps são pares chave/valor. Diferentes linguagens tem diferentes nomes para isso, como 'map', 'dictionary', 'hashtable', etc. Úteis quando vc precisa procurar itens baseado em uma chave, ao invés de um índice.
Chaves tem o tipo genérico K e o valor o tipo genérico V. O HashMap usa um função de hashing para mapear de forma segura na memória as chaves e os valores. 

Criando um HashMap e acessando seus itens:
```Rust
let mut pontuacao = HashMap::new();
pontuacao.insert(String::from("Flamengo"), 50);
pontuacao.insert(String::from("Santos"), 10);
let time = String::from("Flamengo");
let pontos_do_time = pontuacao.get(&time).copied().unwrap_or(0);
```

Sobre *Ownership* e HasMaps: para tipos primitivos, que implementam `Copy`, os valores são copiados, porém, para objetos como String, o valor será movido para dentro do HashMap:
```Rust
let chave = String::from("Flamengo");
let valor = 50u8;
println!("{chave}, {valor}");
let mut pontuacao = HashMap::new();
pontuacao.insert(chave, valor);
// println!("{chave}, {valor}"); não funciona mais aqui
```

Iterando sobre um hashmap:
```Rust
for (key, value) in &pontuacao {
    println!("{}: {}", key, value);
}
```

Sobrescrevendo valores:
```Rust
pontuacao.insert(String::from("Flamengo"), 100);
```

Adicionando apenas se a chave for inexistente:
```Rust
pontuacao.entry(String::from("Flamengo")).or_insert(100);
pontuacao.entry(String::from("Fluminense")).or_insert(0);
```

Substituindo um valor baseado no antigo usando deref:
```Rust
    use std::collections::HashMap;

    let text = "hello world wonderful world";

    let mut map = HashMap::new();

    for word in text.split_whitespace() {
        let count = map.entry(word).or_insert(0); // count é um ponteiro 
        *count += 1; // count aqui é dereferenciado p/ ter seu valor modificado
    }

    println!("{:?}", map);
```