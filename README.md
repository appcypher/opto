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
wat_to_x86 = target(wat -> x86) {
    $state = wat.$state

    x86_imm =
        i32const (..) -> imm ($state.last())
}

simple_opt = target(x86 -> x86) {
    wat_to_x86.x86_imm;

    fold_zero =
        add (_, "0", "0") -> imm ("0")
}

transform =
    wat_to_x86; simple_opt
```

TERMS

```
;; target x86

const = [0-9]+
imm = const
```

```
;; target wat
$state = Array<const>

const = [0-9]+
i32const = "i32.const" const ($state.push(const))
```

EXAMPLE
```
for(int i = 0; i < x.length(); i++) { ... } ->
{ final int sz = x.length(); for(int i = 0; i < sz; i++) { ... } }
```

```
hoist =
    for (_, cond (_, _, simple_expr::constant), ...rest) -> {
        y: const_assignment ("x", simple_expr),
        for (_, cond ( _, _, $y), ...rest }
    }

```

OTHER THINGS THAT NEED TO BE EXPRESSIBLE
- Recursion and spread to allow expressing any logic
- Dependencies
- Type safety preservation
- Attributes based on behavior, e.g constant

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
