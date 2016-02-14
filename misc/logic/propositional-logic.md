% Propositional (Zeroth-Order) Logic

## Basic Connectives

### Negation (&not;)

#### Rules

Double-Negation Introduction: Given A, we can infer &not;&not;A.

Double-Negation Elimination: Given &not;&not;A, we can infer A.

#### Truth Table

<table>
  <thead>
    <tr><th>A</th><th>&not;A</th></tr>
  </thead>
  <tbody>
    <tr><td>0</td><td>1</td></tr>
    <tr><td>1</td><td>0</td></tr>
  </tbody>
</table>

### Conjuction (&and;)

#### Rules

Conjunction Introduction: Given A and given B, we can infer A&and;B.

Conjunction Elimination: Given A&and;B, we can infer A. Given A&and;B, we can infer B.

#### Truth Table

<table>
  <thead>
    <tr><th>A</th><th>B</th><th>A&and;B</th></tr>
  </thead>
  <tbody>
    <tr><td>0</td><td>0</td><td>0</td></tr>
    <tr><td>0</td><td>1</td><td>0</td></tr>
    <tr><td>1</td><td>0</td><td>0</td></tr>
    <tr><td>1</td><td>1</td><td>1</td></tr>
  </tbody>
</table>

### Disjunction (&or;)

#### Rules

Disjunction Introduction: Given A, we can infer A&or;B. Given B, we can infer A&or;B.

Disjunction Elimination: Given A&or;B, and given C assuming A, and given C assuming B, we can infer C and discharge the assumptions A and B.

#### Truth Table

<table>
  <thead>
    <tr><th>A</th><th>B</th><th>A&or;B</th></tr>
  </thead>
  <tbody>
    <tr><td>0</td><td>0</td><td>0</td></tr>
    <tr><td>0</td><td>1</td><td>1</td></tr>
    <tr><td>1</td><td>0</td><td>1</td></tr>
    <tr><td>1</td><td>1</td><td>1</td></tr>
  </tbody>
</table>

### Conditional (&rarr;)

#### Rules

Conditional Introduction (Conditional Proof): Given B assuming A, we can infer A&rarr;B and discharge the assumption A.

Conditional Elimination (Modus Ponens): Given A and given A&rarr;B, we can infer B.

Denying the Consequent (Modus Tollens): Given A&rarr;B and &not;B, we can infer &not;A.

#### Truth Table

<table>
  <thead>
    <tr><th>A</th><th>B</th><th>A&rarr;B</th></tr>
  </thead>
  <tbody>
    <tr><td>0</td><td>0</td><td>1</td></tr>
    <tr><td>0</td><td>1</td><td>1</td></tr>
    <tr><td>1</td><td>0</td><td>0</td></tr>
    <tr><td>1</td><td>1</td><td>1</td></tr>
  </tbody>
</table>

### Biconditional (&harr;)

#### Rules

Biconditional Introduction: Given A&rarr;B and given B&rarr;A, we can infer A&harr;B.

Biconditional Elimination: Given A&harr;B, we can infer (A&rarr;B)&and;(B&rarr;A)

#### Truth Table

<table>
  <thead>
    <tr><th>A</th><th>B</th><th>A&harr;B</th></tr>
  </thead>
  <tbody>
    <tr><td>0</td><td>0</td><td>1</td></tr>
    <tr><td>0</td><td>1</td><td>0</td></tr>
    <tr><td>1</td><td>0</td><td>0</td></tr>
    <tr><td>1</td><td>1</td><td>1</td></tr>
  </tbody>
</table>

## Other inference rules

### Reducto ad absurdum

Given A assuming B, and given &not;A assuming B, we can infer &not;B and discharge the assumption B.
