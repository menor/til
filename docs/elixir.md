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

## Modules
- Modules can have attributtes
- Module attributes are defined at the top level
- You can use attributes to store metadata about the module
- i.e. `@moduledoc` to document the module documentation
- `@doc` documents the function underneath
- We can use module attributes as constants
- It's best to declare the attributes first thing in the file, since they are bound at compile time
- A single elixir file can define multiple modules
- All modules are defined at top level, using dots in the name is just a convenient way to namespace `App.Module`
- To call other module functions in a module you can:
    - Call them including the module name `App.Module.function`
    - Import the module `import App.Module` and `function` will be available (it imports the entire module)
    - Import only the functions we need by using only: `import App.Module, only: [function]`


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
- The cons operator can pattern match the first element of a list and the rest of them like this `[head | tail] = [1, 2, 3, 4, 5]` then `head`  will have the value `1` and tails will have `[2, 3, 4, 5]`
- We can continue calling it until there's only one value left, then the head will be the value, and tail will be an empty list

## Guards
- We can add guards to a function so it will only run when cerain conditions match. i.e.
`def get_storm(id) when is_integer(id) do ... end` will only run if the id is an integer and will throw an error if not

## Error Handling
- Many methods in elixir return an tuple with two o rmore elements, the first one being `:ok` or `:err` we can then pattern match on those to handle errors:
```elixir
    def route(%{ method: "GET", path: "/about" <> id } = conv) do
        case File.read("../../pages/about.html") do
            {:ok, content} ->
                %{ conv | status: 200, resp_body: content} 

            {:error, reason} ->
                %{ conv | status: 500, resp_body: "File error: #{reason}"} 
        end
        %{ conv | status: 200, resp_body: "File"}
    end
```

- Maybe it's better to design that conditional using clause functions:
```elixir
    def route(%{ method: "GET", path: "/about" } = conv) do
        file =
            Path.expand("../../pages", __DIR__)
            |> Path.join("about.html")
            |> File.read
            |> handle_file(conv)
    end

    def handle_file({:ok, content}, conv) do
        %{ conv | status: 200, resp_body: content} 
    end

    def handle_file({:error, :enoent}, conv) do
        %{ conv | status: 404, resp_body: "File not found"} 
    end

    def handle_file({:error, reason}, conv) do
        %{ conv | status: 500, resp_body: "File error: #{reason}"} 
    end
```

## IEX
- You can also use `iex` as a REPL
- Once inside iex to compile (and run) a module use `c "<module-path>"` the module will be run and made available
- You can also do `iex <module-path>` to start iex with the module available
- `iex -S mix` will compile and make available all the modules in the project
- To recompile inside iex you can run `recompile()`
- To execute a module inside iex you can run `r <module-name>`
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
- Another way to shorten functions specially when piping is the capture syntax so you can turn this `Enum.sort(fn(b1, b2) -> Storm.order(b1, b2) end)` into `Enum.sort(&Storm.order(&1, &2))`
- You can make this even shorter by replacing the references at the end by the arity of the function you want to invoke `Enum.sort(&Storm.order/2)`
- You can also use the & operator to capture expressions. From `Enum.map([1,2,3], fn(x) -> x * 3 end)` to `Enum.map([1,2,3], &(&1 * 3))`
- The result of `&(&1 * 3)` is an anonymous function
- Alternatively, you can capture the expression as an anonymous function, bind it to a variable, and then pass the function to the higher-order map function:
```elixir
triple = &(&1 * 3)
Enum.map([1,2,3], triple)
```

## Regular Expressions
- Here's how to define a regular expression literal in Elixir `~r{regexp}`
- The ~r is called a sigil and the braces { } are delimiters for the regular expression itself.
- Example regex: `regex = ~r{\/(\w+)\?id=(\d+)}` it matches a literal / character followed by one or more word characters, followed by the literal `?id=` followed by one or more digits.
- The match method on the Regex module will return a boolean
- We can capture matching values in a regular expression like this: `regex = ~r{\/(?<thing>\w+)\?id=(?<id>\d+)}`
- Notice we added ?<thing> before the word characters and ?<id> before the digit characters. This says we want to capture the word characters as thing and the digit characters as id.
- Now we can call the named_captures function which returns the given captures as a map: 
```elixir
Regex.named_captures(regex, path)
%{"id" => "1", "thing" => "bears"}
```
- If no captures are found it will return `nil`

## Structs
- Maps are generic data structures, a struct provides more structure
- We define a struct in its own module (only one struct per module)
```elixir
defmodule Servy.Conv do
    defstruct [ method: "", path: ""]
end
```

- `method` and `path` are the key names, and the empty strings are the default values
- Since the array is the only argument to defstruct, we can remove the square brackets:
```elixir
defmodule Servy.Conv do
    defstruct method: "", path: ""
end
```
- We can create maps like this `map = %{}`

- To create a struct we need to give the name of the struct:
`conv = %Servy.Conv{}` if we want to override defaults `conv = %Servy.Conv{method: "GET", path: "/bears"}`
- The type constraint is checked at compile time
- We can use dot notation `conv.path` to access key from a struct, not the square brackets `conv[:path]`

## Parsing requests
We can use `URI.decode_query` to get an map of key values from the request body, in this map the keys are strings not atoms. It's not a good idea to convert these into atoms, cause this takes data from users and atoms are not garbage collected.