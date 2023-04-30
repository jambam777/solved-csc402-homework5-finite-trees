Download Link: https://assignmentchef.com/product/solved-csc402-homework5-finite-trees
<br>
Covers:  F#:  Finite trees

For this assignment create a file called HW5.fs.  All of your code for this assignment should be placed in this .fs file.

If you are using JetBrains Rider as your IDE then you can create the file and runnable project as you did for HW4:  Click File-&gt;New… .  Select “Console Application” under .NET Core.  Give the solution a name (e.g. “HW5” without the quotes). The dialog box will fill in the same name (e.g. “HW5”) as the name of the project.  Click “Create”.  Rider will create a file named Program.fs under the HW5 project.

Once again you will submit your work to Mimir.  To prepare for this, perform the following three additional steps:

<ul>

 <li>Right-click on Program.fs file , select Edit-&gt;Rename…, and change the file’s name to HW5.fs.</li>

 <li>Put the declaration <strong>module HW5</strong> at the top of HW5.fs. Also <strong>open System</strong> there, as well.</li>

 <li>At the bottom of HW5.fs file delete the [&lt;EntryPoint&gt;] directive and all of the code that constitutes the main function.</li>

</ul>

From Canvas you can download HW5Test.fs and include it in your project in order to test your code.  (Make sure that HW5Test.fs appears <em>after/below</em> HW5.fs in your project explorer.)




<strong>Problem 1 (Evaluating expressions )</strong>  Consider the following type and function definitions from Section 6.2:

type Fexpr =     | Const of float

| X

| Add of Fexpr * Fexpr

| Sub of Fexpr * Fexpr

| Mul of Fexpr * Fexpr

| Div of Fexpr * Fexpr

| Sin of Fexpr

| Cos of Fexpr

| Log of Fexpr

| Exp of Fexpr




let rec toString = function     | Const x      -&gt; string x

| X            -&gt; “x”

| Add(fe1,fe2) -&gt; “(” + (toString fe1) + “)” + “+” + “(” + (toString fe2) + “)”

| Sub(fe1,fe2) -&gt; “(” + (toString fe1) + “)” + “-” + “(” + (toString fe2) + “)”

| Mul(fe1,fe2) -&gt; “(” + (toString fe1) + “)” + “*” + “(” + (toString fe2) + “)”

| Div(fe1,fe2) -&gt; “(” + (toString fe1) + “)” + “/” + “(” + (toString fe2) + “)”

| Sin fe       -&gt; “sin(” + (toString fe) + “)”

| Cos fe       -&gt; “cos(” + (toString fe) + “)”

| Log fe       -&gt; “log(” + (toString fe) + “)”

| Exp fe       -&gt; “exp(” + (toString fe) + “)”




Define a function eval of type float -&gt; Fexpr -&gt; float.  The task of eval is to evaluate an Fexpr at a particular value for x.  (Hint:  This function very similar to <sub>toString</sub>, except you have an additional parameter of type <sub>float</sub>, and the function returns a <sub>float</sub>.)

&gt; eval 3.14159 (Sin (Div(X, Const 2.0)));; val it : float = 1.0<strong>            </strong>

<strong>Problem 2 (Tree traversal )</strong>  Consider the following type and function definitions from Section 6.4:

type BinTree&lt;‘a&gt; =

| Leaf

| Node of BinTree&lt;‘a&gt; * ‘a * BinTree&lt;‘a&gt;




let rec preOrder = function

| Leaf    -&gt; []

| Node(tl,x,tr) -&gt; x::(preOrder tl) @ (preOrder tr)

let rec inOrder = function

| Leaf          -&gt; []

| Node(tl,x,tr) -&gt; (inOrder tl) @ [x] @ (inOrder tr)

let rec postOrder = function

| Leaf          -&gt; []

| Node(tl,x,tr) -&gt; (postOrder tl) @ (postOrder tr) @ [x]




Define a function levelOrder of type BinTree&lt;‘a&gt; -&gt; ‘a list.  The task of levelOrder is to return a list of the tree elements according to a level-order traversal.  This involves traversing the tree level-by-level, starting at the root, and proceeding left-to-right along each level.  For example, recall the following tree <em>t4</em> from Figure 6.11 in the textbook:




t4;;

val it : BinTree&lt;int&gt; =

Node

(Node (Node (Leaf, -3, Leaf), 0, Node (Leaf, 2, Leaf)), 5,      Node (Leaf, 7, Leaf))

levelOrder t4;;

val it : int list = [5; 0; 7; -3; 2]

Tip – The following pseudocode from Wikipedia (<a href="https://en.wikipedia.org/wiki/Tree_traversal">https://en.wikipedia.org/wiki/Tree_traversal</a><a href="https://en.wikipedia.org/wiki/Tree_traversal">)</a> gives an algorithm for level-order traversal:

<strong>levelorder</strong>(root)   q := empty queue

q.enqueue(root)   <strong>while</strong> not q.empty <strong>do</strong>     node := q.dequeue()     visit(node)     <strong>if</strong> node.left ≠ <strong>null then</strong>

q.enqueue(node.left)     <strong>if</strong> node.right ≠ <strong>null then</strong>

q.enqueue(node.right)

Note – The above pseudocode uses a couple of assignment statements.  However, we have not covered assignment statements in F#.  They exist in F# but are not covered until Chapter 8 of the textbook.  Also, assignment statements involve changing the value of a variable.  This is not considered part of <em>pure</em> functional programming.  We are trying to learn to program in a functional style here and, in fact, any computable function can be defined in a functional style.  So do this problem without assignment statements.  We can translate the idea behind the above pseudocode into concise, purely functional code if we use recursion rather than a loop.

Suggestion – Use a recursive helper function to define <sub>levelOrder</sub>.  I’ll call the helper

levelOrderHelper.  The type of the helper can be BinTree&lt;‘a&gt; list -&gt; ‘a list, in

which case it takes a list of BinTrees and returns the elements of the BinTrees listed in level order.  So a full level order computation on t4 might proceed roughly like this:

levelOrder




= levelOrderHelper [                                                              ]

= 5 :: levelOrderHelper [                                                         ; 7]




= 5 :: 0 :: levelOrderHelper[7 ; -3 ; 2]

= 5 :: 0 :: 7 :: levelOrderHelper[-3 ; 2]

= 5 :: 0 :: 7 :: -3 :: levelOrderHelper[2]

= 5 :: 0 :: 7 :: -3 :: 2 :: levelOrderHelper[]

= 5 :: 0 :: 7 :: -3 :: 2 :: []

= [5;0;7;-3;2]

<strong> </strong>

It’s okay to put Leaf values into your list/queue as you go into recursion as long as you do not put them into your result when you take them off the queue.  If you proceed in the way described above then you will need to consider three cases in your helper function: (1) when the parameter is an empty list, (2) when the parameter is a non-empty list with a Leaf value at the front, and (3) when the parameter is a non-empty list with a Node value at the front.  You can use the :: and @ operators to implement the right-hand side of your patterns.  (Review the textbook’s code for the preOrder, inOrder, and postOrder traversals, which I copied and pasted into the beginning of this problem description.)




<strong>Problem 3 (Creating a SizedBinTree – 8 points)</strong>  Consider the following type and function definitions:

type SizedBinTree&lt;‘a&gt; =

| SizedLeaf

| SizedNode of SizedBinTree&lt;‘a&gt; * ‘a * SizedBinTree&lt;‘a&gt; * int




let size = function     | SizedLeaf -&gt; 0

| SizedNode(tl,x,tr,sz) -&gt; sz




A SizedBinTree is supposed to be the same as a BinTree except that each node in the tree has an integer that gives the number of elements in that node’s subtree.

Write function binTree2SizedBinTree of type BinTree&lt;‘a&gt; -&gt; SizedBinTree&lt;‘a&gt;  that takes a <sub>BinTree</sub> and returns the corresponding <sub>SizedBinTree</sub>.  The following evaluation uses the example <sub>BinTree</sub> t4 shown in Problem 2.




binTree2SizedBinTree t4;; val it : SizedBinTree&lt;int&gt; =

SizedNode

(SizedNode

(SizedNode (SizedLeaf, -3, SizedLeaf, 1), 0,

SizedNode (SizedLeaf, 2, SizedLeaf, 1), 3), 5,  &lt;== Note: this 5 is an element

SizedNode (SizedLeaf, 7, SizedLeaf, 1), 5)         &lt;== Note: this 5 is a size




<strong>Problem 4 (Searching a SizedBinTree – 8 points)</strong>  The List module has a List.item function that can be used the get the i-th element from a list.  Of course, the indexing starts at 0.

List.item;; val it : (int -&gt; ‘a list -&gt; ‘a)

List.item 3 [‘a’;’b’;’c’;’d’;’e’];; val it : char = ‘d’

If the index is out of range the function throws an exception.

List.item 5 [‘a’;’b’;’c’;’d’;’e’];;

System.ArgumentException: The index was outside the range of elements in the list.

(Parameter ‘index’)    at Microsoft.FSharp.Collections.ListModule.Item[T](Int32 index, FSharpList`1 list) in F:workspace_work1ssrcfsharpFSharp.Corelist.fs:line 141

at &lt;StartupCode$FSI_0006&gt;<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="3c12187a6f75630c0c0c0a12515d55527c">[email protected]</a>()

Stopped due to error

We can easily implement our own custom versions of this function.  The following version returns an option: <sub> </sub>

let rec getOptionFromList index = function

| [] -&gt; None

| x::xs -&gt; if index = 0                then Some x

else getOptionFromList (index – 1) xs

val getOptionFromList : index:int -&gt; _arg1:’a list -&gt; ‘a option




getOptionFromList 3 [‘a’;’b’;’c’;’d’;’e’];; val it : char option = Some ‘d’




getOptionFromList 5 [‘a’;’b’;’c’;’d’;’e’];; val it : char option = None




In Problem 3 you wrote a function to create <sub>SizedBinTrees</sub>. Now write a function getOptionFromSizedBinTree to return the i-th element in a SizedBinTree.

val getOptionFromSizedBinTree :    index:int -&gt; _arg1:SizedBinTree&lt;‘a&gt; -&gt; ‘a option

In the following examples <em>t4</em> is again the example from Figure 6.11:




getOptionFromSizedBinTree 1 (binTree2SizedBinTree t4);; val it : int option = Some 0




getOptionFromSizedBinTree 5 (binTree2SizedBinTree t4);; val it : int option = None







Your <sub>getOptionFromSizedBinTree </sub>function should run in O(h) time, where h is the height of the tree.  So it <em>should not</em> in general traverse the entire tree.  Instead, do a binary search for the appropriate element.  This is easy if you take advantage of the size property of the nodes.  For example, suppose you are trying to get an element from <em>t4</em>:










The root Node has size=5 (besides the fact that it has element 5).  So if the index that you are searching is negative or ≥ 5, then you know immediately that the search will fail.  But suppose that the index is in the range [0,…,4].  Then look at the left child.  It has a size=3.  So if the index that you are searching is &lt; 3, you know that the element that you want is in the left tree (so recurse left).  If the index that you are searching is exactly 3, then you know that the root element is the one that you want.  However, if the index that you are searching is &gt; 3, then the element is in the right tree (recurse right).  Note that if you recurse right in this circumstance then, in the recursion, you should reduce the index by 3+1=4 because by recursing right you are bypassing 4 elements.







<strong>Problem 5 (Sum of circuit components  – 4 pts)</strong>  Recall the following definitions from the <em>Tree Recursion</em> subsection of Section 6.7 <em>Electrical circuits</em>:

type Circuit&lt;‘a&gt; =

| Comp of ‘a

| Ser of Circuit&lt;‘a&gt; * Circuit&lt;‘a&gt;

| Par of Circuit&lt;‘a&gt; * Circuit&lt;‘a&gt;




let rec circRec (c,s,p) = function

| Comp x -&gt; c x     | Ser (c1,c2) -&gt;

s (circRec (c,s,p) c1) (circRec (c,s,p) c2)

| Par (c1, c2) -&gt;

p (circRec (c,s,p) c1) (circRec (c,s,p) c2)




let count circ = circRec ((fun _ -&gt; 1), (+), (+)) circ : int

let resistance =

<table width="628">

 <tbody>

  <tr>

   <td width="102">    circRec (</td>

   <td width="526"> </td>

  </tr>

  <tr>

   <td colspan="2" width="628">               (fun r -&gt; r),</td>

  </tr>

 </tbody>

</table>

(+),

(fun r1 r2 -&gt; 1.0/(1.0/r1 + 1.0/r2)))

In the above, a <sub>Circuit</sub> is defined as a tree.  The interesting part is <sub>circRect</sub>, which is a higher-order generic function that can be used to iterate through circuit to accumulate a result.  The <sub>circRec</sub> function takes two parameters:  a triple and a circuit.  The triple consists of three functions:  c, s, and p, which are used to accumulate the result when the recursion reaches a Comp, Ser, or Par, respectively.  The count and resistance functions above show particular applications of <sub>circRec</sub>:  <sub>count</sub> returns the number of components in a circuit and <sub>resistance</sub> returns the resistance of a circuit.

For example, if <sub>cmp</sub> is the circuit from Figure 6.16 in the textbook,

then we have:

let cmp = Ser(Par(Comp 0.25, Comp 1.0), Comp 1.5);; val cmp : Circuit&lt;float&gt; = Ser (Par (Comp 0.25, Comp 1.0), Comp 1.5)

count cmp;; val it : int = 3

resistance cmp;; val it : float = 1.7

Use <sub>circRec</sub> to define a function <sub>sum</sub> that returns the sum of the resistances in the individual components of a circuit.

sum;; val it : (Circuit&lt;float&gt; -&gt; float) = &lt;fun:<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="d1b8a591e2fce0e0">[email protected]</a>&gt;

sum cmp;; val it : float = 2.75

<strong>Problem 6 (Sum along cheapest path  – 4 pts)</strong>  Repeat the previous problem, but this time your function should return the sum of components along the cheapest path that starts at a component and ends at the root.  Call it <sub>sumAlongCheapestPath</sub>.

sumAlongCheapestPath;;

val it : (Circuit&lt;float&gt; -&gt; float) = &lt;fun:<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="640d102457495554">[email protected]</a>&gt;

sumAlongCheapestPath cmp;;

val it : float = 1.75