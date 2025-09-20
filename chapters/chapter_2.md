
# Table of Contents

1.  [Variables and Functions](#orgb07c083)
    1.  [Example](#orgd667ab3)
    2.  [why #+name:](#orgdef7c36)
    3.  [Pattern Matching and Let](#org99befe5)
2.  [Functions](#orgd59b7a8)
    1.  [Anonymous Function](#orgdcb1056)
    2.  [Multiargument function](#org8741dff)
        1.  [Multiargument and function calling equivalence](#org2ee7abc)
        2.  [Currying](#orgd2d4950)
        3.  [Recursive Function](#org5601f96)
        4.  [Prefix Operator and Infix Operator](#org3ac52c9)
        5.  [Declaring Functions with Function](#org41769a8)
        6.  [Labelled Arguments](#org9a5ad36)
        7.  [Optional Arguments](#orgb901d43)
        8.  [Optional Arguments and Partial Application](#org072b7d5)
3.  [Quiz](#org5e250e7)



<a id="orgb07c083"></a>

# Variables and Functions

    let variable := expresion1 in expression2

    let x = 1
    let variable = 5 + 1 in 6

This gives a warning ! why ? any idea ?
This first evaluates expr1 and then evaluates expr2 with variable bound to whatever value was produced by the evaluation of expr1

    (fun binding -> binding + 24) 12

    let binding = 12 in binding + 24

these two functions are literally the same
so lets take a workflow the binding is first bound to whatever expre1 is evaluated to and then the value is then applied to expressions 2

the value returns a warning because the body does not take
int -> int = <fun>

it takes &rsquo;a -> int so what we can always call a function with anything and return int
but this is a warning because binding gets assigned to something and then in expresion 1 it doesn tuse the binding thus the warning

    let _warning = 1 in 2

suppreses the warning

-   Theoretical Framing

This snippet illustrates the ****pipeline paradigm**** in functional programming.
A value flows through a sequence of transformations, where each stage:

1.  Takes an input,
2.  Applies a transformation,
3.  Passes the result forward.


<a id="orgd667ab3"></a>

## Example

    let languages = "OCaml,Perl,C++,C";;
    let combined_string = let language_array = String.split languages ~on:',' in
                          String.concat ~sep:"-" language_array

Here:

-   The ****initial value**** is the string of languages.
-   \`String.split\` performs a **structural decomposition** (breaking the value into components).
-   \`String.concat\` performs a **recombinatory transformation** (assembling components into a new form).

The theoretical essence lies not in the specific operations but in the ****principle of sequencing transformations****:
values flow through black-boxed functions, each producing a refinement until the final result emerges.

    let x = 1 in let y = x + 1 in let x = 3 + y in x + 12

Ya right we will always use ^ such codes in production code right? here is a more relatively good example

    (* Dummy types *)
    type connection = string
    type result = string
    
    
    let open_connection uri : connection =
      "Connection(" ^ uri ^ ")"
    
    let run_query (conn : connection) (sql : string) : result =
      "Ran [" ^ sql ^ "] on " ^ conn
    
    let close_connection (conn : connection) : unit =
      print_endline ("Closing " ^ conn)
    
    let () =
      let conn = open_connection "db://localhost" in
      let result = run_query conn "SELECT * FROM users" in
      close_connection conn;
      print_endline ("Result = " ^ result)

\*\*


<a id="orgdef7c36"></a>

## why #+name:

it variable if you cant vary anything A function can be applied to different inputs, and thus its variables will take on different values, even without mutation.


<a id="org99befe5"></a>

## Pattern Matching and Let

List.unzip
Building on the $in$ command we have

    let upcase_first_entry line =
        let (first :: rest) = String.split ~on:',' line in
        String.concat ~sep:"," (String.uppercase first :: rest);;

Question :: upcase<sub>first</sub><sub>entry</sub> &ldquo;&rdquo; what happens with the input.
If that is the case will it ever require the first block? And then why is the compiler saying its an warning.

$""::[]$
first :: rest

    #require "base";;
    open Base;;
    let upcase_first_entry line =
        match String.split ~on:',' line with
        | [] -> assert false
        | first::rest -> String.concat ~sep:"," (String.uppercase first :: rest);;
    
    upcase_first_entry "foo,bar"


<a id="orgd59b7a8"></a>

# Functions


<a id="orgdcb1056"></a>

## Anonymous Function

Formally, this can be described as a chain of compositions:

<div class="LaTeX" id="org6cfb360">
<p>

</p>

<p>
\[
\texttt{let } x = e_1 \texttt{ in } e_2
\quad \equiv \quad
(\lambda x.\ e_2)\ e_1
\]
</p>

</div>

<div class="LaTeX" id="orga7ac399">
<p>
\[
</p>

\begin{aligned}
\texttt{let } x = e_1 \texttt{ in } e_2
&\;\equiv\; (\lambda x.\ e_2)\ e_1 \\[6pt]
\texttt{let } y = e_3 \texttt{ in } e_4
&\;\equiv\; (\lambda y.\ e_4)\ e_3 \\[6pt]
\texttt{let } z = e_5 \texttt{ in } e_6
&\;\equiv\; (\lambda z.\ e_6)\ e_5
\end{aligned}

<p>
\]
</p>

</div>

<div class="LaTeX" id="orgbc7a67f">
\begin{aligned}
\text{Currying} \quad
& \texttt{let f x y = e} \;\equiv\; \texttt{let f = } \lambda x.\lambda y.\, e \\[6pt]

\end{aligned}

</div>

    let x = 1 in let y = 2 in let z = 3 in x + y + z


<a id="org8741dff"></a>

## Multiargument function


<a id="org2ee7abc"></a>

### Multiargument and function calling equivalence

(x->y->z->a) can be interpreeted as a function that takes x,y,z and returns a
or can be thought of as
x that returns a function which takes y returns another function which takes z and returns a

Question ::

    let f a b = (fun a -> fun b -> abs( a - b))

what should be the output of this function?

f (a,b) = abs(a - b)
f (3,2)


<a id="orgd2d4950"></a>

### Currying

1.  TODO Partial Application does have an extra cost What is that?

2.  Currying


<a id="org5601f96"></a>

### Recursive Function

    let rec running_sum array =
        match array with
        | [] -> 0
        | first :: rest -> first + running_sum rest

for recursion you can use

    let rec is_even n =
      if n = 0 then true else is_odd (n - 1)
    and is_odd n =
      if n = 0 then false else is_even (n - 1)

TODO Maybe if there are dependencies across iterations we may not be able to do.
TODO why &rsquo;rec&rsquo;

<div class="LaTeX" id="org9569acd">
<p>
\section*{Problem (Drunkard and the Pit)}
</p>

<p>
A drunkard starts at position \(0\) on a number line. There is a pit at position
\(N \in \mathbb{Z}_{\ge 1}\). The drunkard moves in \(\emph{moves}\). In each move
he chooses a forward jump length \(s \in \{1,2,3\}\) and does the following:
</p>

\begin{enumerate}
    \item He first goes forward by $s$ steps.
    \item If during this forward part he reaches or passes the pit
    (i.e., position $\ge N$), he falls into the pit immediately and the move ends (no slip-back).
    \item Otherwise, he then slips back exactly $2$ steps.
\end{enumerate}

<p>
\textbf{Task.} Determine the minimum number of moves required to fall into the pit
as a function of \(N\).
</p>

<p>
\subsection*{Optional: Recurrence}
</p>

<p>
Let \(T(N)\) be the minimum number of moves to reach the pit at \(N\). Then
</p>

<p>
\[
T(N) =
</p>

\begin{cases}
1, & N \leq 3, \\[6pt]
1 + \min\limits_{s \in \{1,2,3\}} T\bigl(N - (s-2)\bigr), & N \geq 4,
\end{cases}

<p>
\]
</p>

<p>
with the convention that if \(s \in \{1,2,3\}\) and \(s \geq N\), then \(T(N)=1\).
</p>

<p>
\subsection*{Answer (Closed Form)}
</p>

<p>
The optimal strategy is to always choose \(s=3\) except in the final move. Hence,
</p>

<p>
\[
T(N) = \max(1,\, N-2).
\]
</p>

<p>
\subsection*{Sanity Checks}
</p>

<p>
\[
N = 1,2,3 \;\;\Rightarrow\;\; T(N)=1, \quad
N=4 \;\;\Rightarrow\;\; T(4)=2, \quad
N=5 \;\;\Rightarrow\;\; T(5)=3, \quad
N=6 \;\;\Rightarrow\;\; T(6)=4.
\]
</p>

</div>

TODO I understand that it difficult. But I was thinking if the missing base case can be found out with something like pattern matching


<a id="org3ac52c9"></a>

### Prefix Operator and Infix Operator

-   We saw this briefly in first chapter, where in we discussed that  + is a function, in fact all operations are functions

Stdlib.(+) gives more information.

-   Because its a function you can redefine it to something else as well

    let (+!) (x1,y1) (x2,y2) = (x1 + x2, y1 + y2);;
    (3,2) +! (-2,4);;

-   FACT that function calling preceedes negation so Int.MAX 3 -4 will be interpreted as (Int.MAX 3) -4
    
    Int.max 3 (-4)  (i.e., max of 3 and -4), or
    (Int.max 3) - 4  (i.e., call Int.max with one argument, then subtract 4)? And because negation has lower precedence than function calling it will actually chose this option.

1.  What OCaml actually parses: Int.max 3 -4 ⇒ ((Int.max 3) - 4)
    
            (-)
           /   \\
         App    Int(4)
        /   \\
     App     Int(3)
    /   \\

Id(Int.max)

1.  What you intended: Int.max 3 (-4) ⇒ App (App Int.max 3) (Neg 4)
    
             App
            /   \\
          App    Neg
         /   \\     \\
    Id(Int.max) Int(3) Int(4)

(Right argument is a Neg node wrapping 4, so -4 is a single integer argument.)

-   Stdlib.(|>)
    returns &rsquo;a -> (&rsquo;a -> &rsquo;b) -> &rsquo;b = <fun>

<https://stackoverflow.com/questions/30493644/ocaml-operator>

-   let (^>) x f = f x;;

String.split ~on:&rsquo;:&rsquo; path
^> List.dedup<sub>and</sub><sub>sort</sub> ~compare:String.compare
^> List.iter ~f:print<sub>endline</sub>;;

How did this suddenly become right associative?
Precedence and associativity are determined by the first character of the operator.
Since ^ > |

1.  |> is left-associative

Expression:

x |> f |> g

Parse:

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">&gt;</td>
</tr>
</tbody>
</table>

/  \\

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">&gt;</td>
</tr>
</tbody>
</table>

 /  \\    g
x    f

Which means:

(x |> f) |> g
= g (f x)

1.  ^> is right-associative

Expression:

x ^> f ^> g

Parse:

   ^>
  /  \\
x    ^>
    /  \\
   f    g

Which means:

x ^> (f ^> g)
= (f ^> g) x
= g (f x)

-   The opposite of |> is @@ (and not ^>) because we want to apply composition rules. Check for type of (@@)


<a id="org41769a8"></a>

### Declaring Functions with Function

-   function keyword

function is shorthand for writing a single-argument function that immediately pattern matches:

    let f = function
      | [] -> 0
      | x :: xs -> x + List.length xs

This is exactly the same

    let f x =
      match x with
      | [] -> 0
      | x :: xs -> x + List.length xs

NOTE in the example

    let some_or_default default = function
        | Some x -> x
        | None -> default ;;

when you call a function with default you are doing a partial application.

So function is just syntactic sugar for fun x -> match x with …


<a id="org9a5ad36"></a>

### Labelled Arguments

    let ratio ~num ~denom = Float.of_int num /. Float.of_int denom;;
    ratio 3 4

But also gives a nice warning!

    0.75

-   Bunch of Advantages of using labels.
    -   Special mention to Flexible Argument Ordering and partial application
        
            String.split ~on:':' path
            |> List.dedup_and_sort ~compare:String.compare
            |> List.iter ~f:print_endline;;

1.  Higher Order functions and Labels

    Order matters when argument is called using HOFs
    
    Here’s a crisp illustration of what you mean.
    
    Example
    
    (\* A higher-order function that takes two labelled functions \*)
    let apply f g =
      f ~x:10 (g ~y:20)
    
    (\* Two candidate functions \*)
    let f1 ~x ~y = x + y
    let g1 ~y = y / 2
    
    Now try:
    
    -   : int = 20
    
    But if you reorder the labelled arguments of f1:
    
    let f2 ~y ~x = x + y
    
    then:
    
    Error: This expression has type int but an expression was expected of type
             y:int -> &rsquo;a
    
    Here, the only change was that the higher-order function (apply) tried to call f by first applying ~x and then expecting the result to still be a function that takes ~y. With f1 (~x ~y), this matches. With f2 (~y ~x), it doesn’t, because OCaml currying “consumes” labelled arguments in the order they were defined in the function’s type. So the higher-order function must respect the order of labels in the function definition.
    
    Speculative “why”
    • Normal calls with labelled args: You can supply arguments in any order (f ~y:2 ~x:1 or f ~x:1 ~y:2) because the compiler can reorder them before evaluating the function body.
    • Higher-order functions: When you partially apply a labelled function, you’re actually fixing arguments one by one. At this point, order matters: the function’s type after applying one label depends on how the function was defined.


<a id="orgb901d43"></a>

### Optional Arguments

(\* A pretend &ldquo;static site generator&rdquo; function \*)

    let serve ?(port=8888) content =
      Printf.sprintf "Serving %s on port %d" content port
    
    (* A higher-order function that takes a 'server' function *)
    let run generator =
      (* HOF fixes some arguments *)
      (generator ~port:8080) "index.html"

Now try:

-   : string = &ldquo;Serving index.html on port 8080&rdquo;

But if you change the definition of serve to put the optional argument after the required one:

let serve2 content ?(port=8888) =
  Printf.sprintf &ldquo;Serving %s on port %d&rdquo; content port

Super Neat!


<a id="org072b7d5"></a>

### Optional Arguments and Partial Application


<a id="org5e250e7"></a>

# Quiz

    let f x y z ?op =
      let op = match op with
      | None   -> (fun n -> n)      (* default: identity *)
      | Some g -> g
      in
      op (x + y + z)
    ;;

