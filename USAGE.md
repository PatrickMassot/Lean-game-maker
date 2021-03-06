# Lean Game Maker usage guide
See [lean-game-skeleton](https://github.com/kbuzzard/lean-game-skeleton) for a boilerplate to start making your game. 
Read on to see more details.

## Configuration file
To start from scratch, make a Lean project and add a file named `game_config.toml` to project root.
The format of this file is mostly self-explanatory.

```
name = "**name of the game**"
version = "**"
extra_files = "extras"
intro = "path_to_the_intro_page"

[[worlds]]
name = "**name of the first world**"
id = 1
levels = [
	"path_to_the_first_level",
	"path_to_the_second_level",
	"path_to_the_third_level",
	"path_to_the_fourth_level",
]

[[worlds]]
name = "**name of the second world**"
id = 2
parents = [1]
levels = [
	"path_to_the_first_level",
	"path_to_the_second_level",
]

[[worlds]]
name = "**name of the third world**"
id = 3
parents = [1, 2]
levels = [
	"path_to_the_first_level",
	"path_to_the_second_level",
	"path_to_the_third_level",
]
```

The value of `extra_files` should be the address to a directory.
This directory will be copied into the output folder.
You can use it to store images and extra files that you want to appear in the game.

Each worlds has a unique `id` and these numbers must start at one and increase by one at each world.
Each world can have zero or more parents.
The ids of the parents must be strictly smaller than the world's id.



## Lean files
The intro page and each level are Lean files.
Inside each Lean file, use the following format to add a title to each level or world:

```lean
-- Level name : name_of_the_level
```
To add comments, hints and lemmas, use this format :

```lean
/-
Comment here
-/

/- Hint : title_of_the_hint
Content of the hint
-/

/- Lemma
Description of the lemma
-/
lemma ... :=
begin
  ...
end
```
You can use Markdown in comments and hints. It will be compiled with [showdown](http://demo.showdownjs.com/).
Theorems and examples are similar to lemmas. The only difference is that examples are shown with full solution in the webpage, but lemmas and theorems should be solved by the player.
Note that only the first lemma or theorem that appears in any level is playable.
Every theorem, lemma or example will be added to the side bar in the following levels.
To prevent a lemma from appearing in the side bar, use the following format :

```lean
/- Lemma : no-side-bar
Description of the lemma
-/
lemma ... :=
begin
  ...
end
```
The same goes for theorems and examples.
You can also add description of tactics to the side bar by

```lean
/- Tactic : name_of_the_tactic
Description of the tactic
-/
```
This description will be in the side bar from that level onward.
If you want to add a statement to the list of theorem statements in the side bar without any mention in the main page, use the format :

```lean
/- Axiom : name_of_the_axiom
statement_of_the_axiom
-/
```
This will appear in the side bar from that level onward.

If a line in not contained in a comment, lemma, theorem, example or tactic, it will be shown directly in the game. If such a line ends with ` -- hide`, it will not be shown. Alternatively, you can put a few lines inside blocks of the following format.
```lean
-- begin hide
** comment and lean code here **
-- end hide
```

## Making the game

After preparing the configuration file and the Lean files, go to the root folder of your Lean project and run
```bash
make-lean-game
```
This will make the game in the `html` folder in the Lean project directory.
In this folder, there will be a file named `library.zip` that contains the `.olean` files.
Making this file takes a few seconds.
If you're changing the fomatting, but the name of the Lean files and their lean content hasn't changed.
You can run
```bash
make-lean-game --nolib
```
This way the Lean-game-maker will not generate `library.zip`, so the command runs faster.

## Lean Server
To make an interactive webpage, the javascript Lean server is used. In this repository, javascript servers for Lean 3.4.1 and Lean 3.4.2 are provided. If you're working with a different version, you need to add the required files to `src/interactive_interface/lean_server`. You would need three files, named `lean_js_js.js`, `lean_js_wasm.js` and `lean_js_wasm.wasm`.