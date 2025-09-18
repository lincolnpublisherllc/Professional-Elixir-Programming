Professional Elixir Programming — Code Pack README

This README explains how the extracted code is organized, how file names and extensions were chosen, and how to run the examples locally. It also notes a few pitfalls that come from pulling code out of a Word document.

What’s inside

One folder per chapter:

Chapter_01/
Chapter_02/
…
Chapter_15/
Introduction/            # present only if the intro had code
00_Unknown/              # rare: items the parser could not place
README.md


Each folder contains multiple code snippets saved in reading order:

ch01_code_01.ex, ch01_code_02.ex, …

Config blocks use .exs

Shell commands use .sh

Docker snippets are saved as Dockerfile_01, Dockerfile_02, etc.

Total files extracted: 403
Files are grouped by where they appear in the book, not by project boundaries.

How to use the code
Elixir and OTP versions

Most examples should work on recent versions. If you are starting fresh, install:

Elixir 1.16 or newer

Erlang/OTP 26 or newer

If you hit a version error, try Elixir 1.14–1.17 with OTP 25–27.

Running a single .ex file
elixir path/to/file.ex

Running interactive snippets

Lines that look like iex> ... are interactive examples. Open IEx:

iex


Then type or paste the iex> lines without the prompt.

Mix projects

Some chapters include fragments that belong in a Mix project. If you see defmodule MyApp.Application or use Mix.Config or dependencies, create a project:

mix new my_app
cd my_app


Move the relevant files into:

lib/ for .ex

config/config.exs and friends for .exs

Then:

mix deps.get
mix compile
mix test   # if the chapter showed tests
mix run

Shell scripts and commands

Files with .sh are shell commands collected from the book. Run them as individual lines or:

bash path/to/file.sh


Review each command first, especially anything that writes files, installs packages, or runs Docker.

Docker snippets

Files named Dockerfile_XX are fragments. To test one:

Create a folder, copy the Dockerfile there, and name it Dockerfile.

Add any files it expects (for example a Mix project).

Build and run:

docker build -t myapp .
docker run --rm -p 4000:4000 myapp

Naming rules and detection

Files are numbered in the order they appear in each chapter.

Extensions are inferred:

.ex for Elixir modules and most code

.exs for configs and scripts that use Mix.Config or runtime configuration

.sh for terminal commands such as mix new, iex -S mix, asdf install, etc.

Dockerfile_* for Docker examples

If the book showed only a fragment, you will get just that fragment. You may need to wrap it in a module or place it inside a Mix project to run.

Known limitations

Pulling code out of a Word document is imperfect. Here is what to watch for:

Line breaks and indentation
Word sometimes hides soft line breaks. I normalized obvious breaks, but verify indentation in multi-line examples like case, receive, or pipelines.

Prompts and commentary
iex> prompts, shell prompts, and inline comments are kept when they were clearly part of the snippet. If a line looks like explanatory prose, it was left out.

Project context
Files that reference MyApp.Repo, endpoint, or dependencies expect a Mix project with the right mix.exs. Set those up before running.

Chapter numbering from the TOC
Word TOCs can merge chapter numbers with page numbers. I corrected obvious cases so code lands in Chapter_01 to Chapter_15. You may still find a few items in 00_Unknown if the heading was ambiguous.

Dependencies
If a snippet uses libraries like Ecto, Phoenix, Plug, Benchee, AMQP, etc., add those to mix.exs manually and run mix deps.get.

Quick recipes
Turn a few snippets into a runnable project
mix new demo
cd demo

# move files
mv /path/to/Chapter_03/ch03_code_03.ex lib/demo.ex
mv /path/to/Chapter_03/ch03_code_04.exs config/config.exs

# add deps if needed
# edit mix.exs, then:
mix deps.get
mix run

Run a one-off script with dependencies
mix new scratch --sup
cd scratch
# add {:httpoison, "~> 2.0"} or whatever you need in mix.exs
mix deps.get
# put your script in scripts/run.exs
elixir -r _build/dev/lib/*/ebin -S mix run scripts/run.exs

Test an IEx pipeline from the book
iex -S mix
# paste the iex> lines without the prompt

Troubleshooting

module Foo is not available
The file is missing or not compiled. Put it in lib/ and run mix compile.

no function clause matching or undefined function
Likely a fragment. Check the surrounding lines in the chapter and add the missing definitions.

It compiles, but runtime fails
Look for missing configs in config/*.exs or missing apps in mix.exs under applications or extra_applications.

Unicode or smart quotes
If any curly quotes slipped in from Word, replace them with straight quotes.

Reorganizing or renaming

If you want the files grouped by topic instead of by chapter, or renamed using real module names, say how you want it arranged. Examples:

One Mix project per chapter with all that chapter’s snippets wired together

A single umbrella with apps like ecto_examples, phoenix_examples, genserver_examples

Rename files based on their first module name

I can also generate a searchable index that maps each file to the exact paragraph number or a short excerpt from the book to help you trace context.

At-a-glance manifest (sample)

A tiny sample from early chapters. The full manifest is in the root README.md inside the code folder.

Chapter_01/ch01_code_01.ex      # defmodule and a simple function
Chapter_01/ch01_code_02.sh      # mix new demo && cd demo
Chapter_01/ch01_code_03.exs     # config :logger, level: :info
Chapter_02/ch02_code_01.ex      # GenServer skeleton with handle_call
Chapter_02/Dockerfile_01        # FROM elixir:1.16-alpine …

License and attribution

These files are extracted from your provided manuscript for study and reference. Use them under the same terms you use for your book materials.
