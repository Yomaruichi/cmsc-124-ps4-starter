# The Search and the Stack

The trace for P07 shows how Prolog searches for the grandchildren of tom. The grandparent rule first chooses a child of tom, then looks for a child of that person. Prolog follows the goals from left to right and tries the parent facts in their written order. This explains why ann appears before pat.

In Activity 15's runtime stack model, each Call line represents entering a goal and pushing a frame. Call: grandparent(tom,X) starts the main goal, while the indented Call lines enter its parent goals. Exit represents a successful return, and Fail represents returning after exhausting the available alternatives. These returns correspond to popping active frames in the simplified stack model. However, an Exit does not mean Prolog has finished searching for every possible answer.

The first parent goal binds the intermediate person to liz. The next goal, parent(liz,X), fails because liz has no children in the knowledge base. Prolog then returns to the earlier choice and tries bob. From there, parent(bob,X) first binds X to ann, producing the first successful grandparent answer.

A Redo resumes a saved choice point. Unlike an ordinary returned frame, a choice point preserves enough search information to try another alternative. Prolog also undoes bindings made after that choice. After the first answer, Redo: parent(bob,ann) allows X to become pat instead. Once both children are exhausted, the remaining Fail lines show the search ending.

Clause order matters differently for reach and ancestor because their recursive searches have different structures. The first reach clause immediately calls reach again before checking an edge. This repeats indefinitely, so Prolog never reaches the direct edge clause. Putting the direct edge clause first allows some answers to appear, although the graph's cycle can still prevent exhaustive search from terminating.

The family graph has no cycle, and ancestor reads a parent fact before recursing. Each recursive step moves to a later generation, eventually reaching someone without children. Swapping its clauses therefore changes which answers appear first, but both orders still finish and find the same descendants.

```text
== trace P07 ==
Call: grandparent(tom,X)
  Call: parent(tom,_)
  Exit: parent(tom,liz)
  Call: parent(liz,X)
  Fail: parent(liz,X)
  Redo: parent(tom,liz)
  Exit: parent(tom,bob)
  Call: parent(bob,X)
  Exit: parent(bob,ann)
Exit: grandparent(tom,ann)
answer 1: X=ann
Redo: grandparent(tom,ann)
  Redo: parent(bob,ann)
  Exit: parent(bob,pat)
Exit: grandparent(tom,pat)
answer 2: X=pat
Redo: grandparent(tom,pat)
  Redo: parent(bob,pat)
  Fail: parent(bob,X)
  Redo: parent(tom,bob)
  Fail: parent(tom,_)
Fail: grandparent(tom,X)
== 2 answers ==
```
