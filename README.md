<div align="center">
    <a href="#" target="_blank">
        <img src="https://image.flaticon.com/icons/svg/1179/1179774.svg" alt="Opto Logo" width="140" height="140"></img>
    </a>
</div>


<h2 align="center">OPTO</h2>

--------------

### DESCRIPTION

Opto is an optimizer and compiler generator for transforming SSA WebAssembly and compiling it to machine code. Opto comes with a term rewriting language.

For now the focus will be on interpreting the term rewriting language.

--------------

### PROJECT STRUCTURE

```
- interpreter
- terms
    - wat_to_wat
    - wat_to_x86
```

-------------

### A SINGLE TERM REWRITING LANGUAGE

The goal is to be less academic and more high-level. Term rewriting is all about pattern matching.

##### Constant Propagation Example

```
// wat -> wat

@ignore_spaces() // Special pattern directive.
val := \d+ // Pattern.

allow_constants = 1 // Double. The only supported primitive type.
constants = {} // Hashmap. The only supported DS.

constant_fold := // Mapping Pattern.
    "(#ty.const :val) (local.set #name)" => {
        constants[#name] = (:val, #ty)
    }

constant_propagation :=
    "(local.get #name)" => {
        (#val, #ty) = constants[#name]
        "(#ty.const #val)"
    }

apply_constants (:expr) { // Functions. They take patterns as arguments.
    proof (allow_constants = 0) { // A proof statement.
        constant_fold()
        constant_propagation()
    }
}

apply_constants()

// Checking if a rewrite is a result of constant_propagation
proof (:constant_propagation) {
    @print("There is a constant here", :constant_propagation)
}
```


--------------

### FUTURE CLI

- Generate an x86 optimizer and compiler

```bash
./optogen --target-triple=x86-none-darwin --optfile=rules.opto -o compiler
```

- Optimize wasm file and generate x86 executable code

```bash
./compiler add.wasm
./add
```

### DESIGN REFERENCES

- Binaryen
- Cranelift
- LLVM

--------------

### LINKS

- https://www.sciencedirect.com/science/article/pii/S1571066108001473
