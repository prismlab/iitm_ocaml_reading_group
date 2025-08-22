
# Table of Contents

1.  [Base and Core](#org689e1fe)
2.  [Basic Example](#org02ae1fe)
3.  [Let Binding](#orgede063d)
4.  [Functions and Type Inference](#org02d308c)
    1.  [Inferring Generic Types](#orgb5223c9)
5.  [Tuples, Lists, Options, and Pattern Matching](#orgf8a8a7c)
    1.  [Tuples](#org033cc28)
    2.  [Lists](#org3980625)
        1.  [The List Module](#orgd2bf6a1)
        2.  [Constructing with ::](#org04a261b)
        3.  [List Patterns Using Match](#org96ea208)
        4.  [Recursive List Functions](#orga6edd26)
        5.  [Options](#orgf845315)
        6.  [Records and Variants](#org40e0c6c)
6.  [Imperative Programming](#org1349eb4)
    1.  [Arrays](#orgc9f4416)
    2.  [Mutable Record Fields](#orgef7c18d)
    3.  [Iter Versus Map](#orgf82a680)
    4.  [Refs](#org2ae2ff3)
    5.  [Let and In](#org2fabe72)
    6.  [For and While](#org19bd901)



<a id="org689e1fe"></a>

# Base and Core

The Base layer of OCaml is intentionally kept minimal, while the Core library builds on top of it by introducing additional data structures and abstractions. As a result, Core takes longer to compile and produces larger executables.
This design pattern is not unique to OCaml: a similar separation exists in Java, where `java.lang` contains essential classes while `java.util` provides extended utilities.
In cold-start scenarios (for example, serverless Java functions such as AWS Lambda), developers often prefer minimal class loading. In such cases, a lightweight layer like Base is preferable to the more feature-rich Core.


<a id="org02ae1fe"></a>

# Basic Example

    3 + 4 ;;

This expression demonstrates:

-   The type of the expression, which is `int`.
-   The result of the expression, which is `7`.
-   The use of `Stdlib.(+)` for integer addition.
-   The distinction from `Stdlib.(+.)`, which is reserved for floating-point addition.

OCaml syntax can sometimes appear noisier, and aggressive inlining is possible in certain scenarios.
TODO: Provide an explicit example showing whether expressions such as `(- (+ 1 2) 3)` behave as expected under inlining.


<a id="orgede063d"></a>

# Let Binding

    let x = 1 + 2;
    let Case = 1; (* This is not allowed. *)

In OCaml, capitalization is reserved for constructors. This convention exists because variables are intended to be bound values, while constructors are intended to construct values in algebraic data types. This distinction becomes especially important in pattern matching, which will be explored later.


<a id="org02d308c"></a>

# Functions and Type Inference

A function can be defined simply as follows:

-   `let square x = x * x` demonstrates both function definition and type inference.
-   Writing `rhombus x = x * x` without the keyword `let` produces an error, which illustrates the necessity of explicit binding.
-   Multi-argument functions such as `function x y z` show how arguments can be curried.
-   The expression `ratio x y = x /. y` requires a floating-point division operator and, in some cases, a module import for floating-point operations.

    let sum_if_true test first second =
      (if test first then first else 0) + (if test second then second else 0)

The above function uses branching expressions. Adding type annotations clarifies the intended types:

    let sum_if_true (test : int -> bool) (first : int) (second : int) =
      (if test first then first else 0) + (if test second then second else 0)


<a id="orgb5223c9"></a>

## Inferring Generic Types

    let x = x = x

This expression highlights OCaml’s support for parametric polymorphism.
TODO: Discuss the difference between type errors (detected at compile time) and exceptions (raised at runtime).


<a id="orgf8a8a7c"></a>

# Tuples, Lists, Options, and Pattern Matching


<a id="org033cc28"></a>

## Tuples

A tuple groups values together. For example, `int * string` and `int * int` are common tuple types.
Extracting components can be done via pattern matching:

    let distance (x1,y1) (x2,y2) =
      Float.sqrt ((x1 -. x2) ** 2. +. (y1 -. y2) ** 2.)


<a id="org3980625"></a>

## Lists


<a id="orgd2bf6a1"></a>

### The List Module

A list is a sequence of values of the same type. For example:

    #use "topfind";;
    #require "base,stdio";;
    open Base;;
    open Stdio;;

Using `List.map l ~f:String.length` applies a function to each element of the list.


<a id="org04a261b"></a>

### Constructing with ::

The operator `::` constructs lists and is right-associative.
Concatenation uses the `@` operator.
TODO: Clarify the distinction between semicolons and commas in list syntax.


<a id="org96ea208"></a>

### List Patterns Using Match

Pattern matching allows deconstruction of lists. The operator `@` concatenates lists.
TODO: Provide an explicit example of pattern matching equivalent to Lisp’s `car` and `cdr`.


<a id="orga6edd26"></a>

### Recursive List Functions

    let rec sum l =
      match l with
      | [] -> 0
      | car :: cdr -> car + sum cdr

It is important to cover all possible branches in a `match` expression to avoid compiler warnings.
For example:

    let sum l =
      match l with
      | [] -> 0
      | first :: second :: rest -> first + second

At the top level, lists reduce to two possible forms: `[]` or `head :: tail`. The compiler uses this knowledge to check whether all branches have been covered.


<a id="orgf845315"></a>

### Options

Options are union types defined as `Some` or `None`.
Pattern matching allows safe handling of optional values:

    let downcase_extension filename =
      match String.rsplit2 ~on:'.' filename with
      | None -> filename
      | Some (base,ext) -> base ^ "." ^ String.lowercase ext


<a id="org40e0c6c"></a>

### Records and Variants

Records are tuples with named fields:

    type point2d = { x : float; y : float }

Fields can be pruned or updated.
Consider geometric data types:

    type circle_desc  = { center: point2d; radius: float }
    type rect_desc    = { lower_left: point2d; width: float; height: float }
    type segment_desc = { endpoint1: point2d; endpoint2: point2d }
    
    type scene_element =
      | Circle  of circle_desc
      | Rect    of rect_desc
      | Segment of segment_desc

Functions can then check membership within shapes:

    let is_inside_scene_element point scene_element =
      let open Float.O in
      match scene_element with
      | Circle { center; radius } ->
        distance center point < radius
      | Rect { lower_left; width; height } ->
        point.x > lower_left.x && point.x < lower_left.x + width
        && point.y > lower_left.y && point.y < lower_left.y + height
      | Segment _ -> false

Variants represent sum types, where each constructor encapsulates a specific value form.
TODO: Introduce polymorphic comparators and anonymous functions in more detail.


<a id="org1349eb4"></a>

# Imperative Programming

Although OCaml is by default a functional language (where state cannot be mutated), it also supports imperative programming when needed.


<a id="orgc9f4416"></a>

## Arrays

    let numbers = [| 1; 2; 3; 4 |] ;;
    numbers.(2) <- 4 ;;
    numbers

The unit type `()` is used to communicate the occurrence of side effects, similar to `void` in other languages.


<a id="orgef7c18d"></a>

## Mutable Record Fields

Mutable fields allow in-place updates:

    type person = {
      name : string;
      mutable age : int;
    }
    
    let alice = { name = "Alice"; age = 20 }
    
    alice.age <- 21 ;;


<a id="orgf82a680"></a>

## Iter Versus Map

`List.iter` applies a function to each element for its side effects, whereas `List.map` produces a new list with transformed elements:

    List.iter ["a"; "b"] ~f:(fun s ->
      Stdio.print_endline (String.uppercase s))
    
    List.map ["a"; "b"] ~f:(fun s -> String.uppercase s)


<a id="org2ae2ff3"></a>

## Refs

References encapsulate mutable values. They are dereferenced using `!`:

    let sum list =
      let sum = ref 0 in
      List.iter list ~f:(fun x -> sum := !sum + x);
      !sum ;;


<a id="org2fabe72"></a>

## Let and In

The `in` keyword defines a scope:

    let x = 7 in
    let y = 3 in
    let x = x + y in
    x + y

TODO: Insert a short quiz question here to test whether readers understand lexical scoping.


<a id="org19bd901"></a>

## For and While

    (* For loop: print numbers from 1 to 5 *)
    let () =
      for i = 1 to 5 do
        Printf.printf "For loop: i = %d\n" i
      done;
    
    (* While loop: print numbers from 5 down to 1 *)
    let j = ref 5 in
    while !j > 0 do
      Printf.printf "While loop: j = %d\n" !j;
      j := !j - 1
    done

