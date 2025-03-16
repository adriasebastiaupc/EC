## Índice
- [2.11 Punters](#211-punters)
  - [2.11.1 Declaració](#2111-declaracio)
  - [2.11.2 Inicialització](#2112-inicialitzacio)
  - [2.11.3 Operació de Desreferència](#2113-operacio-de-desferencia-indirection)
  - [2.11.4 Aritmètica de Punters](#2114-aritmetica-de-punters)
  - [2.11.5 Relació entre Punters i Vectors](#2115-relacio-entre-punters-i-vectors)

<h2 id="211-punters">2.11 Punters</h2>
<h3 id="2111-declaracio">2.11.1 Declaració</h3>

- Un **punter** és una variable que conté una **adreça de memòria**.
- En **MIPS**, un punter ocupa **32 bits**.
- Si el punter `p` conté l’adreça de la variable `v`, diem que `p` **apunta a** `v`.

#### **Exemple en C (punters globals i locals):**
```c
int *p1;  // Global

void main() {
    int *p2;  // Local
}
```

#### **Exemple en MIPS:**
```assembly
.data
p1: .word 0
p2: .word 0  # Només si és global
```

---

<h3 id="2112-inicialitzacio">2.11.2 Inicialització</h3>

- L’operador `&` retorna l’adreça d’una variable.

#### **Inicialització dins la declaració (C):**
```c
int v[100];
char a = 'E', b = 'K';
char *pglob = &a;
```
#### **Inicialització en MIPS:**
```assembly
.data
v: .space 400
a: .byte 'E'
b: .byte 'K'
pglob: .word a  # char *pglob = &a
```

#### **Inicialització dins una sentència (C):**
```c
void f() {
    pglob = &b;
}
```
#### **Inicialització en MIPS:**
```assembly
.text
f:
    la $t1, b      # &b
    la $t2, pglob  # &pglob
    sw $t1, 0($t2) # pglob = &b
```

---

### **2.11.3 Operació de Desreferència (Indirection)**
- L’operador `*` davant d’un punter retorna **el valor de la variable a la qual apunta**.

#### **Exemple en C (punter global):**
```c
char *pglob;

void g() {
    char tmp;
    tmp = *pglob;
}
```
#### **Exemple en MIPS:**
```assembly
.text
g:
    la $t2, pglob  # &pglob
    lw $t3, 0($t2) # pglob
    lb $t0, 0($t3) # *pglob
```

#### **Escriptura a un punter en C:**
```c
void g() {
    *pglob = 3;
}
```
#### **Escriptura a un punter en MIPS:**
```assembly
.text
g:
    la $t2, pglob  # &pglob
    lw $t3, 0($t2) # pglob
    li $t4, 3
    sb $t4, 0($t3) # *pglob = 3
```

---

### **2.11.4 Aritmètica de Punters**
- En C, sumar un enter `N` a un punter `p` que apunta a un tipus de `T` bytes:
  ```c
  p = p + N * T;
  ```

#### **Exemple en C:**
```c
char *p1;
int *p2;
long long *p3;

p1 = p1 + 3;
p2 = p2 + 3;
p3 = p3 + 3;
```
#### **Exemple en MIPS:**
```assembly
addiu $t1, $t1, 3   # p1 + 3 bytes
addiu $t2, $t2, 12  # p2 + 3 * 4 bytes
addiu $t3, $t3, 24  # p3 + 3 * 8 bytes
```

---

### **2.11.5 Relació entre Punters i Vectors**
- Un **vector** és en realitat un **punter** al primer element.

#### **Exemple en C:**
```c
int vec[100];
int *p;

void main() {
    p = vec;  // OK
}
```
#### **Exemple en MIPS:**
```assembly
.text
main:
    la $t0, vec  # p = vec;
```

#### **Accés a un element del vector amb punters (C):**
```c
*(p + i) = 0;  // Equivalent a: p[i] = 0;
```
#### **Exemple en MIPS:**
```assembly
.text
    la $t0, vec   # Adreça base
    li $t1, 8     # Índex
    sll $t1, $t1, 2 # Multiplicar per 4 (int)
    addu $t0, $t0, $t1
    sw $zero, 0($t0)  # p[i] = 0
```

---

