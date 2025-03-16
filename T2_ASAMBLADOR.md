# **Resumen - Instruccions i tipus de dades bàsics (EC)**

## **2.2 La Memòria**
- **Adreçament a nivell de byte**: La memòria es representa com un vector on cada element té una adreça i conté 8 bits (1 byte). Es poden adreçar fins a \(2^{32}\) bytes.
- **Ordenació de bytes (Endianness)**:
  - **Little-endian**: Es guarda primer el byte de menys pes en l’adreça més baixa.
  - **Big-endian**: Es guarda primer el byte de més pes en l’adreça més baixa.
  - **MIPS** no defineix una ordenació específica, però s’utilitza **little-endian** en aquesta assignatura.

---

## **2.3 Variables**
### **2.3.1 Variables locals i globals**
- **Variables en C**: Poden ser `automatic`, `extern`, `static` o `register`.
- **Visibilitat i durada**:
  - `auto`: Local, dura fins al final de la subrutina. Per defecte, les declarades dins una funció.
  - `extern`: Global multi-file, dura fins al final del programa. Per defecte, les declarades fora d'una funció.
  - `static` (no ho farem servir):
    - Aplicat a una variable local → persisteix fins al final del programa.
    - Aplicat a una variable global → restringeix la visibilitat a single-file.

#### **Exemple en C**:
```c
int g1, g2;
void main() {
    int l1, l2;
    l1 = g1;
    ...
}
```
#### **Exemple en MIPS**:
```assembly
.data
g1: .word 0
g2: .word 0
.text
.globl main
main:
    la $t0, g1
    lw $t1, 0($t0) # l1 = g1
    ...
```

### **2.3.2 Mida de les variables**
- Depend de la ISA:
  - `char` → 1 byte
  - `short` → 2 bytes
  - `int` → 4 bytes
  - `long long` → 8 bytes

### **2.3.3 Declaració i alineació de variables**
- **Directives d’assemblador** per reservar espai:
  - `.byte` → 1 byte
  - `.half` → 2 bytes
  - `.word` → 4 bytes
  - `.dword` → 8 bytes

#### **Exemple en C**:
```c
char c = 0x11;
short s = 0x2211;
int i = 0x44332211;
unsigned int ui;
long long l = 0x8877665544332211;
```
#### **Exemple en MIPS**:
```assembly
.data
c:  .byte 0x11
s:  .half 0x2211
i:  .word 0x44332211
ui: .word 0
l:  .dword 0x8877665544332211
```
- **Alineació de dades**:
  - `.half` → múltiple de 2
  - `.word` → múltiple de 4
  - `.dword` → múltiple de 8

#### **Vectors en memòria**:
```c
int v[5] = {1, 2, 3, 4, 5};
```
```assembly
v: .word 1, 2, 3, 4, 5
```
- **Reserva d’espai sense inicialitzar**:
```c
char v[100];
```
```assembly
v: .space 100 ## Inicialitza a 0
```
- **Alineació manual amb `.align n`**:
```assembly
char c;
int v[100];

c: .byte 0
.align 2
v: .space 100*4
```

---

## **2.4 Operands**
- **Principi de disseny**: "Simplicity favors regularity".
- **Modes d’adreçament**:
  - **Registre**:
    - addu rd, rs, rt → rd = rs + rt
    - subu rd, rs, rt → rd = rs - rt
  - **Immediat**:
    - addiu rt, rs, imm16 → rt = rs + SignExt(imm16) # Inmediat de 16bits en Ca2
  - **Memòria**
  - **Pseudodirecte**
  - **Relatiu al PC**
- **Registres starnards**:
- ![Registres en MIPS](img/registros_standard_mips.png)

### **2.4.1 Operands en mode registre i immediat**
- 32 registres de 32 bits.
- **Exemple en C**:
```c
void main() {
    int f, g, h, i, j;
    f = (g + h) - (i + j);
}
```
- **Exemple en MIPS**:
```assembly
addu $t0, $t1, $t2  # $t0 = g + h
addu $t1, $t3, $t4  # $t1 = i + j
subu $t0, $t0, $t1  # f = (g + h) - (i + j)
```

- **Còpia entre registres**:
```assembly
addiu $t1, $t2, 0   # $t1 = $t2
addu $t1, $zero, $t2 # Alternativa
move $t1, $t2       # Pseudoinstrucció
```

### **2.4.2 Operands literals i constants simbòliques**
- **Assignació de literals**:
```assembly
addiu $t1, $zero, 100
```
- **Assignació de valors de 32 bits**:
```assembly
li $t1, 0x44332211   # lui $at, 0x4433
                     # ori $t1, $at, 0x2211
```
- **Assignació d’adreces**:
```assembly
.data
a: .word 0
.text
.globl main
main:
    la $t0, a  # $t0 = &a
```
- **Constants simbòliques**:
```assembly
.eqv N 10
li $t0, N
```

### **2.4.3 Operands en memòria (Load i Store)**
- **Exemple d’accés a memòria**:
```assembly
.data
a: .word 0
.text
.globl main
main:
    la $t0, a
    lw $t1, 0($t0) # b = a
    sw $t1, 0($t0) # a = b
```
