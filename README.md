<div align="center">
    <a href="#" target="_blank">
        <img src="https://image.flaticon.com/icons/svg/1179/1179774.svg" alt="Opto Logo" width="140" height="140"></img>
    </a>
</div>


<h2 align="center">OPTO</h2>

--------------

### DESCRIPTION

Opto is an optimizer and compiler generator for transforming SSA WebAssembly and compiling it to machine code. Opto comes with a term rewriting language for rapid prototyping.

--------------

### PLATFORM SPECIFIC INFORMATION

- Integer and Floating-point Registers
- Instructions
- SIMD operations
- ABI (call conv, exception)
- Interrupts
- Object file format
- Debuginfo
- Target triple (based on Cranelift's)

--------------

### DESIGN

- Wasm-like SSA-based IR
- Attributes
    - Parameters
    - Return
    - Function Signature
    - Module
- TBAA types
- Types
    - Struct
    - Array
    - i32, i64, f32, f64

--------------

### PROJECT STRUCTURE

```
terms_rewriter_generator
terms
    - webassembly
    - assembly_code
    - machine_code
    - object_format
```

Term rewiter generates a compiler that takes in WebAssembly and immediately generates Assembly, Machine, or Object code.
The term language is turing complete and it can be used to specify phases of compilation.

-------------

### LANGUAGES

TERM RULES

```
[ OPT LEVEL 0 ]
target(wat -> x86) {
    i32const = wat.i32const -> imm: [1.state.top()]
}


[ OPT LEVEL 1 ]
target(x86 -> x86) {
    t = x86.add -> x86.add iff 1[1] = 1[2]
}
```

TERMS

```
;; target x86

const = [0-9]+
imm = const
```

```
;; target wat
[state] = array of const

const = [0-9]+
i32const = "i32.const" const [state.push(const)]
```
--------------

### CLI

- Generate an x86 optimizer and compiler

```bash
./optogen --target-triple=x86-none-darwin --optfile=rules.opto -o compiler
```

- Optimize wasm file and generate x86 executable code

```bash
./compiler add.wasm
./add
```

--------------

### OUT OF SCOPE

- Linker
- Non CPU processors
- Compiler Frontend

--------------

### DESIGN REFERENCES

- Binaryen
- LLVM

--------------

### LINKS

- https://www.sciencedirect.com/science/article/pii/S1571066108001473
