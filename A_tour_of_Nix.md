#Nix 
# A tour of Nix (1/35)
- ショートカット
	- `shift + return` で実行

</br>
</br>
</br>
</br>
</br>

# How it works (2/35)
```nix
# code goes here
{
  v="understood";
}
```

</br>
</br>
</br>
</br>
</br>

# Hello World (3/35)
> [!todo]
> ```nix
> # output "Hello World"
> let 
>   h = "Hello";
>   w = "World";
> in
> {
>   helloWorld = h + X + X;
> }
> ```
 
> [!done]
> ```nix
> let 
>   h = "Hello";
>   w = "World";
> in
> {
>   helloWorld = h + w;
> }
> ```

> [!info]
> ## Let
> ```nix
> let 
> 	# bindings
> 	# ここに変数が入る。
> in
> {
> 	# body
> 	# ここに属性(attribute)が入る。
> }
> V```


</br>
</br>
</br>
</br>
</br>

# String (4/35)
> [!info]
> ## 文字列内にattributeを入れる
> `"${attribute}"`
> attributeは変数みたいなもの。
> nixはattribute setの塊を持つ。
> keyとvalueを持つ。

> [!todo]
> ```nix
> # Complete ex0, ex1 and ex2 to each print 'Hello World'.
> let 
>   h = "Hello";
> in
> {
>   ex0 = "${h} X";
>   ex1 = "${h + X + X}";
>   ex2 = ''${h + X + X}'';
> }
> ```

> [!done]
> ```nix
> let 
>   h = "Hello";
>   s = " ";
>   w = "World";
> in
> {
>   ex0 = "${h} World";
>   ex1 = "${h + s + w}";
>   ex2 = ''${h + s + w}'';
> }
> ```

</br>
</br>
</br>
</br>
</br>


# Multi line string (5/35)
> [!info]
> ## 複数行の文字列 `'' xxx ''`
> ```
> ''
> ${attribute}
> '' 
> ```
> マルチラインのなかにも

> [!todo]
> ```nix
> # Format ex0 as wanted by the solution checker!
>  
> let 
>   user = "mrNix";
>   pass = "99supersecret";
>   vip = true;
>   vipString = if vip == true then ''vip: XXX '' else XXX
> in
> {
>   ex0 = ''
>   ${user}
>     password: XXX
>     ${vipString}
>   '';
> }
> ```

> [!done]
> ```nix
> # Format ex0 as wanted by the solution checker!
>  
> let 
>   user = "mrNix";
>   pass = "99supersecret";
>   vip = true;
>   vipString = if vip == true then ''vip: "true" '' else "false";
> in
> {
>   ex0 = ''
>   ${user}
>     password: ${pass}
>     ${vipString}
>   '';
> }
> ```

</br>
</br>
</br>
</br>
</br>

# Integer to string (6/35)
> [!info]
> ## Function(関数)
> https://noogle.dev
> https://noogle.dev/f/builtins/toString
```nix
let 
  h = "Strings";
  v = 4;
in
{
  helloWorld = "${h} ${v} the win!";
}
```
```nix
let 
  h = "Strings";
  v = 4;
in
{
  helloWorld = "${h} ${toString v} the win!";
}
```

</br>
</br>
</br>
</br>
</br>

# Functions: Introduction (7/35)
> [!todo]
>  Write a function that consumes 3 Strings and combines them to one.
```nix

let
  f = "f";
  o = "o";
  func = a: b: c: XXX; 
in
{
  foo = func f o "o";
}
```
> [!info]
> ```nix
> pattern: body
> ```
> Pattern - 引数のパターンと値の受け取り方を表す。
> 	３つのパターンがある
> 	- single identifier
> 	- set pattern
> 	- @-pattern
> [Language Constructs - Nix Reference Manual](https://nix.dev/manual/nix/2.22/language/constructs#functions)

```nix
let
  f = "f";
  o = "o";
  func = a: b: c: a+b+c; # この関数はa,b,cの引数をとり、a+b+cを行う。
in
{
  foo = func f o "o";
}
```

</br>
</br>
</br>
</br>
</br>

# Functions: Your first function! (8/35)
> [!todo]
> - implement
> 	- min
> 		- `min` takes two integer values and returns the smaller one
> 	- max
> 		- `max` takes two integer values and returns the bigger one
> ```nix
>  let
>   min = XX # modify these
>   max = XX # two lines only
>  in
>  {
>    ex0 = min 5 3;
>    ex1 = max 9 4;
>  }
> ```
> > [!hint]
> >  if () then X else Y

> [!done]
> ```nix
> let
>   min = a: b: if (a < b) then a else b;
>   max = a: b: if (a > b) then a else b;
> in
> {
>   ex0 = min 5 3;
>   ex1 = max 9 4;
> }
> ```


</br>
</br>
</br>
</br>
</br>

# Functions: attribute sets arguments (9/35)
> [!todo]
> 
> ```nix
> let
>   f = "f";
>   o = "o";
>   b = "b";
>   func = {a ? f, b , c }: a+b+c; # only modify this line!
> in
> rec {
>   foo = func {b="o"; c=o;}; # must evaluate to "foo"
>   bar = func {a=b; c="r";}; # must evaluate to "bar"
>   foobar = func {a=foo;b=bar;}; # must evaluate to "foobar"
> }
> ```
> > [!hint]
> > - set arguments
> > 	- default value
> > 		- `?`でdefault valueを設定する。
> > 		- その引数はオプションとなり、値がない場合はデフォルトの値が入る
> > - rec

> [!done]
> ```nix
> let
>   f = "f";
>   o = "o";
>   b = "b";
>   func = {a ? f, b ? "a" , c ? "" }: a+b+c; # オプションを設定した
> in
> rec {
>   foo = func {b="o"; c=o;};
>   bar = func {a=b; c="r";};
>   foobar = func {a=foo;b=bar;};
> }
> ```


</br>
</br>
</br>
</br>
</br>

# Functions: the @-pattern (10/35)
> [!todo]
> **foo** must evaluate to 'foo'
> **foobar** must evaluate to 'foobar'
> ```nix
> let
>   arguments = {a="f"; b="o"; c=X; d=X;}; #only modify this line
>   func = {a, b, c, ...}: a+b+c; 
>   func2 = args@{a, b, c, ...}: a+b+c+args.d;
> in
> {
>   #the argument d is not used 
>   foo = func arguments;
>   #now the argument d is used
>   foobar = func2 arguments;
> }
> ```
> > [!hint]
> > ## @-pattern
> > attribute setから特定のattributeを取り出す場合は明示的にするひつようがある


> [!done]
> ```nix
> let
>   arguments = {a="f"; b="o"; c="o"; d="bar";}; # only modify this line
>   func = {a, b, c, ...}: a+b+c; 
>   func2 = args@{a, b, c, ...}: a+b+c+args.d;
> in
> {
>   # the argument d is not used 
>   foo = func arguments;
>   # now the argument d is used
>   foobar = func2 arguments;
> }
> 
> ```
> > [!note]
> > Nixでは、関数に属性セットを渡す際、その属性セットに定義されていない追加の属性を含めることができます。このような追加の属性にアクセスするために@-パターンを使用します。

</br>
</br>
</br>
</br>
</br>

# Functions: the @-pattern II (11/35)
> [!todo]
> evaluate to 'foobar'
> ```nix
> let
>   func = {a, b, ...}@bargs: if a == "foo" then
>     b + bargs.c else b + bargs.x + bargs.y;
> in
> {
>   # complete next line so it evaluates to "foobar"
>   foobar = func {a="bar"; XXX # ONLY EDIT THIS LINE
> }
> ```

> [!done]
> ```nix
> let
>   func = {a, b, ...}@bargs: if a == "foo" then
>     b + bargs.c else b + bargs.x + bargs.y;
> in
> {
>   # complete next line so it evaluates to "foobar"
>  foobar = func {a="bar"; b="foo"; x="bar"; y="";};
> }
> ```
> > [!note]
> > ### `bargs@{a, b, ...}`と`{a, b, ...}@bargs`の等価性
> > `bargs@{a, b, ...}`と`{a, b, ...}@bargs`は、どちらも同じ意味を持ち、元の属性セット全体と特定の属性にアクセスするための方法です。これらの構文は完全に同じ効果を持ちます。

</br>
</br>
</br>
</br>
</br>

# Partial application (12/35)
> [!todo]
> solve each question: `ex00`, `ex01`, ... by only modifying X with a `value` of type `Int`
> ```nix
> let
>   b = 1;
>   fu0 = (x: x);
>   fu1 = (x: y: x + y) 4;
>   fu2 = (x: y: (2 * x) + y);
> in
> rec {
>   ex00 = fu0 X;     # must return 4
> #  ex01 = (fu1) X;   # must return 5
> #  ex02 = (fu2 X ) X; # must return 7
> #  ex03 = (fu2 X );   # must return <LAMBDA>s >
> #  ex04 = ex03 X;    # must return 7
> #  ex05 = (n: x: (fu2 x n)) X X; # must return 7
> }
> ```
>> [!hint]
>> **Note:** If you see `ex03 = <LAMBDA>;` after `run`, this means that `ex03` is bound to a `function`.

> [!info] ChatGPT
> ## Currying vs Partial Application (関数型言語の概念)
>> [!question]
>> 意味わかってない
>>> [!todo]
>>> https://zenn.dev/luma/articles/introduction-currying-and-partial-apply
>>> 深くは読んでない。
>> 
>> 引数が満ちるまでその引数を受け取るための関数を返し続ける？
>> 一旦、無視
> ## それぞれの関数の意味
> ```nix
>   fu0 = (x: x);
> # これはidentity function(恒等関数)とよばれる関数
> # 入力の値をそのまま返す関数。
> 
>   fu1 = (x: y: x + y) 4;
> # これは２つのくくりにわけることができる。
> # ()の中の関数
> 	# xとy を受取りx+yを返す。
> # `(関数) 4` これは関数の引数、４をすでに受け取っている関数
> 
>   fu2 = (x: y: (2 * x) + y);
> ```

> [!done]
> ```nix
> let
>   b = 1;
>   fu0 = (x: x);
>   fu1 = (x: y: x + y) 4;
>   fu2 = (x: y: (2 * x) + y);
> in
> rec {
>   ex00 = fu0 4;     # must return 4
>   ex01 = (fu1) 1;   # must return 5
>   ex02 = (fu2 2) 3; # must return 7
>   ex03 = (fu2 2);   # must return <LAMBDA>s
> 
> # ここがpartial application
>   ex04 = ex03 3;    # must return 7
>   ex05 = (n: x: (fu2 x n)) 3 2; # must return 7
> }
> ```

</br>
</br>
</br>
</br>
</br>

# Partial application II(13/35)
> [!todo]
> A must evaluate to 'HappyFunctionsAreCalled' 
> ```nix
> let
>   arguments = {a="Happy"; b="Awesome";};
> 
>   func = {a, b}: {d, b, c}: a+b+c+d;
> in
> {
>   A = func arguments XXX # only modify this line
> }
> 
> ```

> [!done]
> ```nix
> let
>   arguments = {a="Happy"; b="Awesome";};
> 
>   func = {a, b}: {d, b, c}: a+b+c+d;
> in
> {
>   A = func arguments {d="Called"; b="Functions"; c="Are";};
> }
> ```
> > [!note]
> > 複数のsetを引数に取ることができる。
> > この例では２つめのsetが1つめのsetの中の変数を上書きする。

</br>
</br>
</br>
</br>
</br>

# Attribute sets: rec (14/35)
- `attribute` = `name` + `value`
- `rec`がある場合とない場合の違いは？

> [!question]
> イマイチわかってない。
> 次の章以降でわかってくるかも？

</br>
</br>
</br>
</br>
</br>

# Attribute sets: examples 1 (15/35)
> [!todo]
> Make all `ex0` up to `ex7` evaluate to true.
> ```nix
> with import <nixpkgs> { };
> with stdenv.lib;
> let
>   attr = {a="a"; b = 1; c = true;};
>   s = "b";
> in
> {
>   #replace all X, everything should evaluate to true
>   ex0 = isAttrs X;
> #  ex1 = attr.a == X;
> #  ex2 = attr.${s} == X;
> #  ex3 = attrVals ["c" "b"] attr == [ XXX ];
> #  ex4 = attrValues attr == [ XXX ];
> #  ex5 = builtins.intersectAttrs attr {a="b"; d=234; c="";} 
> #    == { X = X; X = X;};
> #  ex6 = removeAttrs attr ["b" "c"] == { XXX };
> #  ex7 = ! attr ? a == XXX;
> }
> ``` 
> > [!hint]
> > ## 初見の関数
> > - `isAttrs e` 
> > - `attrVals set`
> > - `attrValues set`
> > - `builtins.intersectAttrs e1 e2`
> > 	- Return a set consisting of the attributes in the set *e2* which have the same name as some attribute in *e1*.
> > 	  特定のsetを返す、それは_e2_のAttributeのうち、_e1_と共通のキーを持つAttributeのみを含んだset.
> > 	- Performs in O(n log m) where n is the size of the smaller set and m the larger set's size.
> > 	```nix
> >      with import <nixpkgs> { };
> >      with stdenv.lib;
> >      let
> >        attr = {a="a"; b = 1; c = true;};
> >      in
> >      {
> >        ex5 = builtins.intersectAttrs attr {a="b"; d=234; c="";};
> >      }
> > 	```
> > 	```bash
> > 	ex5 = { a = "b"; c = ""; };
> > 	```
> > - `removeAttrs`
> > - `?`
> >     - Has attribute
> > 	```nix
> >      with import <nixpkgs> { };
> >      with stdenv.lib;
> >      let
> >        attr = {a="a"; b = 1; c = true;};
> >      in
> >      {
> >         ex7 = ! attr ? a == XXX;
> >      }
> > 	```

> [!done]
> ```nix
> with import <nixpkgs> { };
> with stdenv.lib;
> let
>   attr = {a="a"; b = 1; c = true;};
>   s = "b";
> in
> {
>   #replace all X, everything should evaluate to true
>   ex0 = isAttrs attr;
>   ex1 = attr.a == "a";
>   ex2 = attr.${s} == 1;
>   ex3 = attrVals ["c" "b"] attr == [ true 1 ];
>   ex4 = attrValues attr == [ "a" 1 true ];
>   ex5 = builtins.intersectAttrs attr {a="b"; d=234; c="";} 
>     == { a = "b"; c = "";};
>   ex6 = removeAttrs attr ["b" "c"] == { a="a";};
>   ex7 = ! attr ? a == false;
> }
> ``` 
>> [!info] ex7, Operatorの優先度
>> Has attribute`?` > Logical negation`!` > Equality`==`

|Name|Syntax|Associativity|Precedence|
|---|---|---|---|
|[Attribute selection](https://nix.dev/manual/nix/2.22/language/operators#attribute-selection)|_attrset_ `.` _attrpath_ [ `or` _expr_ ]|none|1|
| . . . | . . .|. . .|. . .|
|[Has attribute](https://nix.dev/manual/nix/2.22/language/operators#has-attribute)|_attrset_ `?` _attrpath_|none|4|
| . . . | . . .|. . .|. . .|
|Logical negation (`NOT`)|`!` _bool_|none|8|
| . . . | . . .|. . .|. . .|
|[Equality](https://nix.dev/manual/nix/2.22/language/operators#equality)|_expr_ `==` _expr_|none|11|
| . . . | . . .|. . .|. . .|
> https://nix.dev/manual/nix/2.22/language/operators


</br>
</br>
</br>
</br>
</br>

# Attribute sets: examples 2 (16/35)
> [!todo]
> Make `ex0` and `ex1` to evaluate to true
> ```nix
> let
>   list = [ { name = "foo"; value = 123; }
>            { name = "bar"; value = 456; } ];
>   string = ''{"x": [1, 2, 3], "y": null}'';
> in 
> {
>   ex0 = builtins.listToAttrs list == { XXX };
>   ex1 = builtins.fromJSON string == { XXX };
> }
> ```

> [!done]
> ```nix
> let
>   list = [ { name = "foo"; value = 123; }
>            { name = "bar"; value = 456; } ];
>   string = ''{"x": [1, 2, 3], "y": null}'';
> in
> {
>   ex0 = builtins.listToAttrs list == {foo = 123; bar = 456;};
>   ex1 = builtins.fromJSON string == {x = [1 2 3]; y = null;};
> }
> ```

</br>
</br>
</br>
</br>
</br>

# Attribute sets: examples 3 (17/35)
> [!todo]
> make `exBounus` true
> ```nix
> let
>   attrSetBonus = {f = {add = (x: y: x + y);
>                        mul = (x: y: x * y);};
>                   n = {one = 1; two = 2;};};
> in
> rec {
>   # Bonus: use only the attrSetBonus to solve this one
>   exBonus = 5 == XX ( XX XX XX ) XX;
> }
> ```

> [!done]
> ```nix
> let
>   attrSetBonus = {f = {add = (x: y: x + y);
>                        mul = (x: y: x * y);};
>                   n = {one = 1; two = 2;};};
> in
> rec {
>   exBonus = 5 == attrSetBonus.f.add ( attrSetBonus.f.mul attrSetBonus.n.two attrSetBonus.n.two) attrSetBonus.n.one;
> }
> ```

</br>
</br>
</br>
</br>
</br>

# Attribute sets: merging (18/35)
> [!todo]
> ```nix
> let
>   x = { a="bananas"; b= "pineapples"; };
>   y = { a="kakis"; c ="grapes";};
>   z = { a="raspberrys"; c= "oranges"; };
> 
>   func = {a, b, c ? "another secret ingredient"}: "A drink of: " + 
>     a + ", " + b + " and " + c;
> in
> rec {
>   ex00=func ( x );  
>   # hit 'run', you need the output to solve this!
>   # ex01=func (y // X );  
>   # ex02=func (x // { X="lychees";});
>   # ex03=func (X // x // z);
> }
> ```
> > [!hint]
> > Update operator `//`
> > 優先度は以下のリンクから
> > https://nix.dev/manual/nix/2.22/language/operators

> [!done]
> ```nix
> let
>   x = { a="bananas"; b= "pineapples"; };
>   y = { a="kakis"; c ="grapes";};
>   z = { a="raspberrys"; c= "oranges"; };
> 
>   func = {a, b, c ? "another secret ingredient"}: "A drink of: " + 
>     a + ", " + b + " and " + c;
> in
> rec {
>   ex00=func ( x );  
>   # hit 'run', you need the output to solve this!
>   ex01=func (y // x );  
>   ex02=func (x // { c="lychees";});
>   ex03=func ({} // x // z);
> }
> ```
> ```output
> { ex00 = "A drink of: bananas, pineapples and another secret ingredient"; ex01 = "A drink of: bananas, pineapples and grapes"; ex02 = "A drink of: bananas, pineapples and lychees"; ex03 = "A drink of: raspberrys, pineapples and oranges"; }
> ```

</br>
</br>
</br>
</br>
</br>

# Attribute sets and boolean (19/35)
> [!todo]
> Remove the `#` to uncomment the exercises as you proceed.
> ```nix
> let
>   attrSet = {x = "a"; y = "b"; b = {t = true; f = false;};};
>   attrSet.c = 1;
>   attrSet.d = null;
>   attrSet.e.f = "g";
> in
> rec {
>   # boolean
>   ex0 = attrSet.b.t;
>   # equal
>   ex01 =  "a" == attrSet.XX; 
>   # unequal 
>   ex02 = !("b" != attrSet.XX );
>   # and/or/neg
>   ex03 = ex01 && !ex02 || ! attrSet.XX;
>   # implication
>  ex04 = true -> attrSet.XX;
>  ex05 = attrSet.XX ? e;
> }
> ``` 

|Name|Syntax|Associativity|Precedence|
|---|---|---|---|
|[Attribute selection](https://nix.dev/manual/nix/2.22/language/operators#attribute-selection)|_attrset_ `.` _attrpath_ [ `or` _expr_ ]|none|1|
|Function application|_func_ _expr_|left|2|
|...|...|...|...|
|[Has attribute](https://nix.dev/manual/nix/2.22/language/operators#has-attribute)|_attrset_ `?` _attrpath_|none|4|
|...|...|...|...|
|Logical negation (`NOT`)|`!` _bool_|none|8|
|[Update](https://nix.dev/manual/nix/2.22/language/operators#update)|_attrset_ `//` _attrset_|right|9|
|...|...|...|...|
|[Equality](https://nix.dev/manual/nix/2.22/language/operators#equality)|_expr_ `==` _expr_|none|11|
|Inequality|_expr_ `!=` _expr_|none|11|
|Logical conjunction (`AND`)|_bool_ `&&` _bool_|left|12|
|Logical disjunction (`OR`)|_bool_ `\|\|` _bool_|left|13|
|...|...|...|...|
|[Logical implication](https://nix.dev/manual/nix/2.22/language/operators#logical-implication)|_bool_ `->` _bool_|right|14|


> [!done]
> ```nix
> let
>   attrSet = {x = "a"; y = "b"; b = {t = true; f = false;};};
>   attrSet.c = 1;
>   attrSet.d = null;
>   attrSet.e.f = "g";
> in
> rec {
>   # boolean
>   ex0 = attrSet.b.t;
>   # equal
>   ex01 =  "a" == attrSet.x; 
>   # unequal 
>   ex02 = !("b" != attrSet.y);
>   # and/or/neg
>   ex03 = ex01 && !ex02 || ! attrSet.XX;
>   # implication
>  ex04 = true -> attrSet.XX;
>  ex05 = attrSet.XX ? e;
> }
> ``` 

</br>
</br>
</br>
</br>
</br>

# Lists (20/35)

> [!todo]
> replace all X, everything should evaluate to true
> ```nix
> with import <nixpkgs> { };
>  > with stdenv.lib;
> let
>   list = [2 "4" true false {a = 27;} false 3];
>   f = x: isString x;
>   s = "foobar";
> in
> {
>   ex00 = isList X;
> #  ex01 = elemAt list 2 == X;
> #  ex02 = length list == X;
> #  ex03 = last list == X;
> #  ex04 = filter f list == [ XX ];
> #  ex05 = head list == X;
> #  ex06 = tail list == [ XXX ];
> #  ex07 = remove true list == [ XXX ];
> #  ex08 = toList s == [ XXX ];
> #  ex09 = take 3 list == [ XXX ];
> #  ex10 = drop 4 list == [ XXX ];
> #  ex11 = unique list == [ XXX ];
> #  ex12 = list ++ ["x" "y"] == [ XXX ];
> }
> ```

> [!done]
> ```nix
> with import <nixpkgs> { };
> with stdenv.lib;
> let
>   list = [2 "4" true false {a = 27;} false 3];
>   f = x: isString x;
>   s = "foobar";
> in
> {
>   ex00 = isList list;
>   ex01 = elemAt list 2 == true;
>   ex02 = length list == 7;
>   ex03 = last list == 3;
>   ex04 = filter f list == [ "4" ];
>   ex05 = head list == 2;
>   ex06 = tail list == [ "4" true false {a = 27;} false 3 ];
>   ex07 = remove true list == [ 2 "4" false {a = 27;} false 3 ];
>   ex08 = toList s == [ "foobar" ];
>   ex09 = take 3 list == [ 2 "4" true ];
>   ex10 = drop 4 list == [ {a = 27;} false 3 ];
>   ex11 = unique list == [ 2 "4" true false {a = 27;} 3 ];
>   ex12 = list ++ ["x" "y"] == [ 2 "4" true false {a = 27;} false 3 "x" "y" ];
> }
> ```

> [!note]
> ## `stdenv.lib`
> - `tail list`
> 	- Return the list without its first item; abort evaluation if the argument isn’t a list or is an empty list.
> - `take i list`
> 	- 特定の配列を返す。この配列はもとの配列の先頭からi個の要素を持つ？？？
> 	- 公式で確認する必要がある。


</br>
</br>
</br>
</br>
</br>

# Lists & attribute sets (21/35)

>[!todo]
> ```nix
> let
>   attrSet = {x = "a"; y = "b"; b = {t = true; f = false;};};
>   attrSet.c = 1;
>   attrSet.d = 2;
>   attrSet.e.f = "g";
> 
>   list1 = [attrSet.c attrSet.d];
>   list2 = [attrSet.x attrSet.y];
> 
> in
> {
>   #List concatenation.
>   ex0 = ["a" "b" 1 2] == XX ++ XX;
> }
> ```

> [!done]
> ```nix
> let
>   attrSet = {x = "a"; y = "b"; b = {t = true; f = false;};};
>   attrSet.c = 1;
>   attrSet.d = 2;
>   attrSet.e.f = "g";
> 
>   list1 = [attrSet.c attrSet.d];
>   list2 = [attrSet.x attrSet.y];
> 
> in
> {
>   #List concatenation.
>   ex0 = ["a" "b" 1 2] == list2 ++ list1;
> }
> ```
> > [!note]
> > 配列の要素をattributeを使って定義している。

</br>
</br>
</br>
</br>
</br>

# inherit / import / with (22/35)
> [!info]
> - `with`
>   - attributeの中身を直接参照できるようになる
>   ```nix
>    let 
>      as = { x = "foo"; y = "bar"; };
>    in 
>      with as; x + y
>   ```
>   - in other words, `as.x`, `as.y`のような書き方が省略できる。
> 	  - バグのリスクもあるので取り扱いのスコープは小さく
> 	  - pkgsの文字とよくみる。`pkgs.XXX`を省略する意図
> - `import`
> 	- **ChatGPTに聞いた**
>      - ### 具体的な例と説明
>      以下のコード例を説明します：
>         ```nix
>         # sample.nix
>         rec {   x = 123;   y = import ./foo.nix; }
>         ```
>         ```nix
>         # foo.nix
>         {   a = "hello";   b = 456; }
>         ```
>         上記のコードが実行されると、次のような属性セットが生成されます：
>         ```nix
>         {   x = 123;   y = {     a = "hello";     b = 456;   }; }
>         ```
>         これにより、`x`は`123`に、`y`は`{ a = "hello"; b = 456; }`という属性セットに設定されます。 
> - `inherit`
>     - Nixにおける`inherit`キーワードは、現在のスコープ内で定義されている属性を、そのまま属性セットに取り込むために使用されます。これにより、既存の変数や定義済みの属性を再利用することができます。
>       > [!question]
>       > よくわかってねぇ
>       > よく見るが、使う本質がわからんので、未来の自分に実際のコードで理解できることを祈る。

> [!todo]
> ```nix
> let 
>   myImport = import <nixpkgs> {};
>   x = 123; 
>   as = { a = "foo"; b = "bar"; };
>   
> in with as; { 
>   inherit x; #example
>   #fix line below: we want a and b in this scope
>   inherit X; 
>   #also fix this line
>   z = XXX.lib.isBool true;
> }
> ```

> [!done]
> ```nix
> let 
>   myImport = import <nixpkgs> {};
>   x = 123; 
>   as = { a = "foo"; b = "bar"; };
>   
> in with as; { 
>   inherit x; #example
>   # fix line below: we want a and b in this scope
>   inherit a b; 
>   # also fix this line
>   z = myImport.lib.isBool true;
> }
> ```


</br>
</br>
</br>
</br>
</br>

# Scoping of attributes in let/with (23/35)

</br>
</br>
</br>
</br>
</br>

# Typing system (24/35)

</br>
</br>
</br>
</br>
</br>

# Assertions (25/35)

</br>
</br>
</br>
</br>
</br>

# Debugging packaging of nano (26/35)

</br>
</br>
</br>
</br>
</br>

# Map (27/35)

</br>
</br>
</br>
</br>
</br>

# mapAttrs (28/35)

</br>
</br>
</br>
</br>
</br>

# Fold: Introduction (29/35)

</br>
</br>
</br>
</br>
</br>

# Reimplementation: reverseList (30/35)

</br>
</br>
</br>
</br>
</br>

# Fold: Implementing 'map' (31/35)

</br>
</br>
</br>
</br>
</br>

# Reimplementations: attrVals (32/35)

</br>
</br>
</br>
</br>
</br>

# Reimplementations: attrValues (33/35)

</br>
</br>
</br>
</br>
</br>

# Reimplementations: catAttrs (34/35)

</br>
</br>
</br>
</br>
</br>

# The end (35/35)

</br>
</br>
</br>
</br>
</br>

