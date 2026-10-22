# TensorLib-MLIR

TensorLib-MLIR is a transpiler for [TensorLib](https://github.com/leanprover/TensorLib) to MLIR.

It is intended to be invoked on existing TensorLib code and emit MLIR.

For example, given the following Lean code:

```lean
let t1 := (Tensor.ofFloat32List! [1.0, 2.0, 3.0, 4.0]).reshape! (Shape.mk [2, 2])
let t2 := (Tensor.ofFloat32List! [5.0, 6.0, 7.0, 8.0]).reshape! (Shape.mk [2, 2])

let t3 <- match matmul2 t1 t2 with
  | .ok result => pure result
  | .error msg => throw (IO.userError msg)
```

The intended output would resemble the following MLIR (illustrative, not yet
able to generate such code.):

```mlir
%A = arith.constant dense<[[1.0, 2.0], [3.0, 4.0]]> : tensor<2x2xf32>
%B = arith.constant dense<[[5.0, 6.0], [7.0, 8.0]]> : tensor<2x2xf32>
%C = arith.constant dense<0.0> : tensor<2x2xf32>

%result = linalg.matmul
    ins(%A, %B : tensor<2x2xf32>, tensor<2x2xf32>)
    outs(%C : tensor<2x2xf32>) -> tensor<2x2xf32>
```

## State of the project

Early development: investigating Lean compiler representations and MLIR lowering.

Currently in progress by @handel-fan.
