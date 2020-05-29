# til
📘 Today I Learned


## VIM

### Replacing surrounding character using vim-surround
If you have the [vim-surround](https://github.com/tpope/vim-surround) plugin installed:
- To replace surrounding symbols `cs"'` (will replace surrounding double quotes by single)
- To delete surrounding delimiters `ds(` (Will remove surrounding parentheses)


## GIT

Since git 2.13, it is possible to use different configs depending on the directory using conditional includes.

An example:

Global config `~/.gitconfig`

```
[user]
    name = John Nada
    email = john@nada.tld

[includeIf "gitdir:~/work/"]
    path = ~/work/.gitconfig
```

Work specific config `~/work/.gitconfig`

```
[user]
    email = john.nada@acme.tld
```

## Bash

### Globbing primitives
Some globbing primitives in bash are:
- `*`  matches everything
- `?`  matches any single character
- `[abd]`  matches any character from a, b or d
- `[a-d]`  matches any character from a, b, c or d

i.e.:
- `ls *1` will list all the files that end in `1`
- `ls file[0-9]` will list all the files that start with file and end with a character from 0 to 9

### Variable interpolation
The bash shell interpolates the variable if it's in double quotes, but not if it's in single quotes.
```sh
MYSTRING="a String"
MYSENTENCE="A sentence with $MYSTRING in it"

echo $ MYSENTENCE # A sentence with a String in it

MYSENTENCE='A sentence with $MYSTRING in it'
echo $ MYSENTENCE # A sentence with $MYSTRING in it
```

### Make variables readonly
To make a variable read only put `readonly` in front of it like this:
```sh
readonly MYVAR=astring
```

### Create several files in one go
It is possible to create several files with just one command by putting them between curly braces
`touch dir/{file1,file2,file3}` will create three files under dir

## Firefox

You can create different profiles to store separate history, bookmarks and add-ons, this is useful for keeping a separate work and life profile, so you don't think about live while working (or about work while living), also separate add-ons might come handy in case you want to test in a vanilla browser. To set this up in FIrefox got to `about:profiles`

## Homebrew
You can see which packages are outdated by running `brew outdated` and then upgrade them all using `brew upgrade`, you can also do this with casks by doing `brew cask outdated` and `brew cask upgrade`

## Postgres

### Debug postgres
I had the problem of postgres crashing with some weird error. To debug I did the following:
- Started postgres service through homebrew `brew services start postgresql`
- Ran `brew services list` to see to whic file the errors where logged
- Tailed the file (`tail -n 100 /usr/local/var/log/postgres.log`) and saw that the error was about the data being created by a previous version of postgres
(see stack overflow response [here](https://stackoverflow.com/a/47673746/2565132))

### Update postgres data
- To update data from previous postgres installations you can run `brew postgresql-upgrade-database` and the data will be migrated to the new version

## Productivity
### Tips
- Setting a cadence for fixing bugs (i.e. biweekly) could help with improving the codebase

## Elixir

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

### Atoms as Map Keys
- Elixir atoms are prefixed by a colon. I.E.
 `%{ :method => "GET", :path => "/wildthings" }`
- However, it's so common to use atoms as keys that Elixir gives us a shortcut. `%{ method: "GET", path: "/wildthings" }`
- If the keys are anything but atoms, you must use the general => form. For example, here's a map with strings as keys: `%{ "method" => "GET", "path" => "/wildthings" }`
- If the keys are atoms, so to get the values associated with the keys we can use the square-bracket syntax: `conv[:method]` or the dot notation `conv.method`
- If a key does not exist and we use the square brackets notation we will get `nil` if we use the dot notation we will get an error (so prefer the dot notation)
- If the keys are strings, the dot notation does not work only the square brackets (and we need to include the quotes) `conv["method"]`
- All data structures in elixir are Immutable, but we can rebind variables, let's change a key in the conv object: `Map.put(conv, :resp_body, "Hello")`. This won't change conv, it will return a new object with the changed value. But we can rebind `conv` -> `conv = Map.put(conv, :resp_body, "Hello")` 
- That is a common operation so there's this shortcut for it: `conv = %{ conv | resp_body: "Hello"}` (But it will only modify keys that already exist in the map, not create new ones)



### Pattern Matching
- `=` is know as the match operator elixir always tries to make the left part the same as the right side
- If there's a variable on the left and a value on the right, then the variable will be assigned the value to make the expression match
- If you don't want to bind the left side you can use the pin operator `^a = 2`
- Remember that elixir will only try to bind the variables on the left side
- We can use pattern matching to check values or extract them 
- If you are not interested in a value you can use the underscore `_` to match anything 
- To concatenate strings we use the `<>` operator, so we can also pattern match strings by doing `"/bear/" <> id = "/bear/1"` `1` will be binded to `id`

### IEX
- You can also use `iex` as a REPL
- Once inside iex to compile (and run) a module use `c "<module-path>"` the module will be run and made available
- You can also do `iex <module-path>` to start iex with the module available
- `iex -S mix` will compile and make available all the modules in the project
- To recompile inside iex you can run `r <module-name>`
- You can see methods available in an object i.e. `h String.` (then hit Tab to see available methods)

### Logging
- `IO.inspect <data>` prints and return a data structure

### Functions
- There's a shortened syntax for functions `def log(data), do: IO.inspect data`
- Function clauses are functions with the same name and arity, elixir will match the arguments and run the function that matches

```
    def route(conv, "/wildthings") do
        %{ conv | resp_body: "Bears, Lions and Tigers"}
    end

    def route(conv, "/bears") do
        %{ conv | resp_body: "Teddy, Smokey and Paddington"}
    end
```

- Depending on the second argument we will run one or another version of the function.

- We could also do the clause by pattern matching
```
def route(%{method: "GET", path: "/wildthings"} = conv) do
  %{ conv | resp_body: "Bears, Lions, Tigers" }
end

def route(%{method: "GET", path: "/bears"} = conv) do
  %{ conv | resp_body: "Teddy, Smokey, Paddington" }
end
```

- You can create functions that are private to a module by using `defp` instead of `dev`

## Making Work Visible Book
### Theory of Constraints
The Theory of Constraints is a way to identify the most important limitng factor (the constraint) that stands in the way of achieving a goal. And systematically improve that constraint until it is no longer the limiting factor.

### Workflow System
Use a workflow system that:
- makes work visible
- limits work in progress (WIP)
- measures and manages the flow of work
- prioritizes effectively
- makes adjustments based on learnings from feedback and metrics

### The 5 Thieves of Time
1 - Too much work in progress
2 - Unknown dependencies: Obstacles you didn't account for when planning
3 - Unplanned work:
    - Interruptions that stop you from finishing a task
4 - Conflicting Priorities:
    - Projects and tasks that conflict
    - exarcebated when you are uncertain about the most important thing to do
5 - Neglected work:
    - Partially completed work that sits idle in the bench

#### 1 - Too much work in progress
- When we start new tasks without finishing an older task, our WIP goes up and thing takes longer to do.
- Cycle Time is the amount of elapsed time that a work item spends as work-in-progress
- WIP is a leading indicator of cycle time. The more items are worked on at the same time, the more doors open up that allow dependencies and interruptions to creep in.
- _Little's Law_: The average cycle time for finishing time is calculated as the ratio betwee WIP and throughput (`average cycletime = average WIP / average throughtput`)
- Signals that too much WIP is going on:
  - Context switching is common.
  - Customers wait for long periods of time.
  - Quality suffers.
  - Irritated Stuff (Interruptions thwart deep thinking).
- Kanban sets limits fow WIP items to avoid the system being overloaded.
- The right amount of WIP is what enables us to maintain a healthy amount of work.

#### 2 - Unknown dependencies
