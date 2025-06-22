**Summary**

Chapter 15 introduces the Interpreter pattern, which defines a representation for a language’s grammar and uses an interpreter to evaluate sentences in the language. It shows how to build an abstract syntax tree of terminal and non-terminal expressions and interpret input in a given context.

**Concepts Map**

```mermaid
flowchart TD
  subgraph Interpreter_Pattern
    Context[Context]
    Expr[AbstractExpression]
    Terminal[TerminalExpression]
    NonTerm[NonTerminalExpression]
  end
  Expr --> Terminal
  Expr --> NonTerm
  NonTerm -->|composes| Expr
  Context -->|provides data| Expr
  Expr -->|interpret(ctx)| Value
```  

**Key Concepts**

* **Interpreter Pattern** Defines a grammar representation and interpreter for evaluating expressions.
* **AbstractExpression** Base interface declaring `interpret(context)`.
* **TerminalExpression** Handles leaf nodes (e.g., numbers, variables).
* **NonTerminalExpression** Implements grammar rules by combining subexpressions.
* **Context** Stores global information (variables, input) for interpretation.
* **Recursive Evaluation** Expression tree is traversed recursively to compute results.

**Quiz 20250622_20:00:00**

1. The Interpreter pattern’s main goal is to:
- a) Generate code
- b) Evaluate sentences in a defined grammar
- c) Create objects dynamically
- d) Manage threads

2. Terminal expressions represent:
- a) Grammar rules
- b) Leaf nodes with direct values
- c) Context storage
- d) Collections of expressions

3. Non-terminal expressions:
- a) Store global state
- b) Combine and evaluate child expressions
- c) Are never recursive
- d) Only parse input strings

4. Context in Interpreter:
- a) Holds output only
- b) Supplies runtime information like variables
- c) Defines grammar rules
- d) Implements evaluation logic

5. `interpret(context)` is implemented on:
- a) Context only
- b) TerminalExpression and NonTerminalExpression
- c) AbstractExpression only
- d) Client code

6. Recursive evaluation is needed because:
- a) Grammar is flat
- b) Expressions nest inside each other
- c) Context loops automatically
- d) Evaluation is stateful only

7. A violation of Interpreter occurs when:
- a) You use blocks instead of classes
- b) You mix parsing and evaluation in one class
- c) You define a separate context
- d) Expressions implement interpret

8. Interpreter benefits languages with:
- a) No grammar
- b) Simple fixed operations
- c) Complex, extensible grammars
- d) Only numeric data

9. To add a new grammar rule, you:
- a) Modify Context
- b) Create a new NonTerminalExpression subclass
- c) Change TerminalExpression
- d) Update AbstractExpression interface

10. Interpreter pattern is less suited for:
- a) Complex grammars
- b) One-off parsing
- c) Domain-specific languages
- d) Reusable language implementations

**Answers:**
1. b) Evaluate sentences in a defined grammar — core purpose.
2. b) Leaf nodes with direct values — terminals hold concrete values.
3. b) Combine and evaluate child expressions — non-terminals apply rules.
4. b) Supplies runtime information like variables — context role.
5. b) TerminalExpression and NonTerminalExpression — both interpret.
6. b) Expressions nest inside each other — recursive structure.
7. b) You mix parsing and evaluation in one class — violates separation.
8. c) Complex, extensible grammars — ideal use-case.
9. b) Create a new NonTerminalExpression subclass — extend grammar.
10. b) One-off parsing — overhead for simple tasks.

**Challenge**

Build a simple arithmetic expression evaluator using Interpreter pattern: support addition and multiplication. Outline classes (`NumberExpression`, `AddExpression`, `MulExpression`, `Context`), show parsing tree and evaluation of "3 + 5 * 2".

**Challenge Answer:**
```ruby
class Context; attr_reader :input; def initialize(input); @input = input; end; end
class NumberExpression
  def initialize(value); @value = value; end
  def interpret(ctx); @value; end
end
class AddExpression
  def initialize(left, right); @l, @r = left, right; end
  def interpret(ctx); @l.interpret(ctx) + @r.interpret(ctx); end
end
class MulExpression
  def initialize(left, right); @l, @r = left, right; end
  def interpret(ctx); @l.interpret(ctx) * @r.interpret(ctx); end
end
# Usage:
# parse manually: AddExpression.new(NumberExpression.new(3), MulExpression.new(NumberExpression.new(5), NumberExpression.new(2)))
expr = AddExpression.new(NumberExpression.new(3), MulExpression.new(NumberExpression.new(5), NumberExpression.new(2)))
result = expr.interpret(Context.new(nil)) # => 13
```