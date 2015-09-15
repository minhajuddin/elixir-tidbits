title: How to convert a map into a string representation?
tags: inspect, struct, object, string, convert, debug
alternate_titles:
  - How to convert a struct into a string?
  - How to convert an object into a string?
  - How to inspect a variable?
  - What is the equivalent of ruby's #inspect?
  - How to log a struct using Logger?
  - How to log an object using Logger?
---

If you quickly want to log or inspect a struct in elixir, you would try something
like the code below. However, that would fail spectacularly, because puts
expects the input argument to implement the String.Chars protocol.

~~~elixir
iex(1)> x = %{name: "Mujju Zainab"}
iex(2)> IO.puts x
** (Protocol.UndefinedError) protocol String.Chars not implemented for %{awesome: "test"}
    (elixir) lib/string/chars.ex:3: String.Chars.impl_for!/1
    (elixir) lib/string/chars.ex:17: String.Chars.to_string/1
    (elixir) lib/io.ex:426: IO.puts/2
iex(2)>
~~~


Whenever you want to do this just pass it through `inspect` and it will convert the input
argument(whatever it may be) to a string.

~~~elixir
iex(3)> x = %{name: "Zainab"}
iex(4)> xv = inspect(x)
"%{name: \"Zainab\"}"
iex(5)> IO.puts xv
%{name: "Zainab"}
:ok
iex(6)>
~~~


## From the elixir documentation

~~~
                          def inspect(arg, opts \\ [])
Inspect the given argument according to the Inspect protocol. The second
argument is a keywords list with options to control inspection.

Options

inspect/2 accepts a list of options that are internally translated to an
Inspect.Opts struct. Check the docs for Inspect.Opts to see the supported
options.

Examples

┃ iex> inspect(:foo)
┃ ":foo"
┃
┃ iex> inspect [1, 2, 3, 4, 5], limit: 3
┃ "[1, 2, 3, ...]"
┃
┃ iex> inspect("olá" <> <<0>>)
┃ "<<111, 108, 195, 161, 0>>"
┃
┃ iex> inspect("olá" <> <<0>>, binaries: :as_strings)
┃ "\"olá\\0\""
┃
┃ iex> inspect("olá", binaries: :as_binaries)
┃ "<<111, 108, 195, 161>>"
┃
┃ iex> inspect('bar')
┃ "'bar'"
┃
┃ iex> inspect([0|'bar'])
┃ "[0, 98, 97, 114]"
┃
┃ iex> inspect(100, base: :octal)
┃ "0o144"
┃
┃ iex> inspect(100, base: :hex)
┃ "0x64"

Note that the inspect protocol does not necessarily return a valid
representation of an Elixir term. In such cases, the inspected result must
start with #. For example, inspecting a function will return:

┃ inspect fn a, b -> a + b end
┃ #=> #Function<...>
~~~
