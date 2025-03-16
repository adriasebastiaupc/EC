# **Resumen - Instruccions i tipus de dades bàsics (EC)**

## **2.7 Representació de caràcters en ASCII**
- Els llenguatges d’alt nivell inclouen un tipus **caràcter** per treballar amb textos.
- Es basen en una **codificació de caràcters** (en aquest cas, **ASCII**).
- **ASCII**: codificació de **7 bits**, amb **128 caràcters** (33 no imprimibles).

### **Propietats de la codificació ASCII**
- Dígits `0-9` a partir del codi `48 (0x30)`.
- Majúscules `A-Z` a partir del codi `65 (0x41)`.
- Minúscules `a-z` a partir del codi `97 (0x61)`.
- Caràcter nul (`\n` en C) és `0x00`.
- Espai (`' '`) és `0x20`, salt de línia (`'
'`) és `0x0A`.

#### **Conversió entre majúscules i minúscules**
```
char min = maj + 32;
```
#### **Conversió de digit numeric a char**
char digit_char = '0' + numero;


### **Declaració de variables ASCII**
#### **En C:**
```c
char lletra = 'R';
```
#### **En MIPS:**
```assembly
lletra: .byte 'R'
```

### **Exemple de conversió en MIPS**
```assembly
.data
c1: .byte 'A'
c2: .byte 0
.text
.globl main
main:
    la $t0, c1
    lb $t1, 0($t0)
    addiu $t1, $t1, 32  # Convertir majúscula a minúscula

    li $t1, 7
    addiu $t1, $t1, '0' # Convertir enter a ASCII
    la $t0, c2
    sb $t1, 0($t0)
```

---

## **2.8 Format de les instruccions MIPS**
- **3 formats principals:**  
  - **R (register)**  
  - **I (immediate)**  
  - **J (jump)**  

### **Exemple: codificació binària d'una instrucció**
```assembly
addu $t0, $s2, $zero
```
#### **Desglossament en binari**
```
opcode = 000000
rs ($s2) = 10010
rt ($zero) = 00000
rd ($t0) = 01000
shamt = 00000
funct = 100001

Binari: 000000|10010|00000|01000|00000|100001 = 0x02404021
```

---

## **2.9 Vectors**
- Un **vector (array en C)** és una agrupació unidimensional d’elements del mateix tipus.

### **Declaració i emmagatzematge**
#### **En C:**
```c
int vec[100];
int vec2[5] = {0, 1, 2, 3, 4};
```
#### **En MIPS:**
```assembly
.data
.align 2
vec:  .space 400  # Reserva espai per a 100 enters (100 * 4 bytes)
vec2: .word 0, 1, 2, 3, 4
```

### **Accés a un element (índex constant)**
#### **En C:**
```c
int vec[100];
int main() {
    int x = vec[3];
}
```
#### **En MIPS:**
```assembly
la $t0, vec
lw $t1, 12($t0)  # x = vec[3] (3 * 4 = 12)
```

### **Accés a un element (índex variable)**
#### **En C:**
```c
int vec[100];
int main() {
    int x, i;
    x = vec[i];
}
```
#### **En MIPS:**
```assembly
la $t0, vec       # Adreça base del vector
sll $t3, $t2, 2   # Multiplicar i per 4 (i * 4)
addu $t0, $t0, $t3 # Calcular @vec[i]
lw $t1, 0($t0)    # x = vec[i]
```

---

## **2.10 Strings**
- **Cadenes de caràcters** de mida variable.
- En **Java**: Es representen com una tupla `(longitud, array de caràcters)`.
- En **C**: Vector de caràcters **amb sentinella (`\n`)**.

### **Declaració de cadenes**
#### **En C:**
```c
char cadena[20] = "Una frase";
char cadena[20] = {'U', 'n', 'a', ' ', 'f', 'r', 'a', 's', 'e', '\n'};
```
#### **En MIPS:**
```assembly
.data
cadena: .asciiz "Una frase"  # Inclou el sentinella '\n'
```

### **Accés als elements d’una cadena**
#### **En C:**
```c
char nom[80];
int num = 0;
while (nom[num] != '\n') num++;
```
#### **En MIPS:**
```assembly
.text
.globl main
main:
    move $t0, $zero  # num = 0
    la $t1, nom
while:
    addu $t3, $t1, $t0  # @nom[0] + num * 1
    lb $t2, 0($t3)      # Carregar nom[num]
    beq $t2, $zero, end # Si és '\n', acaba
    addiu $t0, $t0, 1   # num += 1
    b while
end:
```

