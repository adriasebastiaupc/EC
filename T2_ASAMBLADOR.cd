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
  - `auto`: Local, dura fins al final de la subrutina.
  - `extern`: Global multi-file, dura fins al final del programa.
  - `static`:
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
