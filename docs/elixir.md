# Elixir

- To generate a new project `mix new <project-name>`
- `mix help` will list all the tasks
- For generated projects:
    - code goest into `lib` 
    - `mix.exs` is similar to package.json
- Files with `.ex` extension are meant to be compiled
- Files with `.exs` extension can be ran as scripts
- Elixir uses modules, one module per file
- Convention is to put modules in a directory under lib (`lib/app_name/`)
- Then de module will be call `defmodule APPNAME.MODULENAME`
- An :atom is a name whose value is the same as the key (think constants in redux actions)

To run an elixir file you can do:
    - `elixir <file-path>`
The file will be compiled in memory and run in the Erlang Virtual Machine

## Atoms as Map Keys
- Elixir atoms are prefixed by a colon. I.E.
 `%{ :method => "GET", :path => "/wildthings" }`
- However, it's so common to use atoms as keys that Elixir gives us a shortcut. `%{ method: "GET", path: "/wildthings" }`
- If the keys are anything but atoms, you must use the general => form. For example, here's a map with strings as keys: `%{ "method" => "GET", "path" => "/wildthings" }`
- If the keys are atoms, so to get the values associated with the keys we can use the square-bracket syntax: `conv[:method]` or the dot notation `conv.method`
- If a key does not exist and we use the square brackets notation we will get `nil` if we use the dot notation we will get an error (so prefer the dot notation)
- If the keys are strings, the dot notation does not work only the square brackets (and we need to include the quotes) `conv["method"]`
- All data structures in elixir are Immutable, but we can rebind variables, let's change a key in the conv object: `Map.put(conv, :resp_body, "Hello")`. This won't change conv, it will return a new object with the changed value. But we can rebind `conv` -> `conv = Map.put(conv, :resp_body, "Hello")` 
- That is a common operation so there's this shortcut for it: `conv = %{ conv | resp_body: "Hello"}` (But it will only modify keys that already exist in the map, not create new ones)


## Pattern Matching
- `=` is know as the match operator elixir always tries to make the left part the same as the right side
- If there's a variable on the left and a value on the right, then the variable will be assigned the value to make the expression match
- If you don't want to bind the left side you can use the pin operator `^a = 2`
- Remember that elixir will only try to bind the variables on the left side
- We can use pattern matching to check values or extract them 
- If you are not interested in a value you can use the underscore `_` to match anything 
- To concatenate strings we use the `<>` operator, so we can also pattern match strings by doing `"/bear/" <> id = "/bear/1"` `1` will be binded to `id`

## IEX
- You can also use `iex` as a REPL
- Once inside iex to compile (and run) a module use `c "<module-path>"` the module will be run and made available
- You can also do `iex <module-path>` to start iex with the module available
- `iex -S mix` will compile and make available all the modules in the project
- To recompile inside iex you can run `r <module-name>`
- You can see methods available in an object i.e. `h String.` (then hit Tab to see available methods)

## Logging
- `IO.inspect <data>` prints and return a data structure

## Functions
- There's a shortened syntax for functions `def log(data), do: IO.inspect data`
- Function clauses are functions with the same name and arity, elixir will match the arguments and run the function that matches

```elixir
    def route(conv, "/wildthings") do
        %{ conv | resp_body: "Bears, Lions and Tigers"}
    end

    def route(conv, "/bears") do
        %{ conv | resp_body: "Teddy, Smokey and Paddington"}
    end
```

- Depending on the second argument we will run one or another version of the function.

- We could also do the clause by pattern matching
```elixir
def route(%{method: "GET", path: "/wildthings"} = conv) do
  %{ conv | resp_body: "Bears, Lions, Tigers" }
end

def route(%{method: "GET", path: "/bears"} = conv) do
  %{ conv | resp_body: "Teddy, Smokey, Paddington" }
end
```

- You can create functions that are private to a module by using `defp` instead of `dev`

