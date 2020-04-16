# tree-sitter-arcy

A minimal grammar for the Janet programming language.

## Status

Subject to change, grammar still evolving.

## Prerequisites

* [emsdk](https://emscripten.org/docs/getting_started/downloads.html#installation-instructions) -- emscripten via homebrew seems to work for macos
* node >= 12 (nvm recommended) -- recently tested 12.9.1, 12,16,1

## Fine Print

* The instructions below assume emsdk has been installed, but `emcc` (tool that can be used to compile to wasm) is not necessarily on one's `PATH`.  If an appropriate `emcc` is on one's `PATH` (e.g. emscripten installed via homebrew), the emsdk steps (e.g. `source ~/src/emsdk/emsdk_env.sh`) below may be ignored.

* `node-gyp` (tool for compiling native addon modules for Node.js) may fail on machines upgraded to macos Catalina. [This document](https://github.com/nodejs/node-gyp/blob/master/macOS_Catalina.md) may help cope with such a situation.

## Initial Setup

Suppose typical development sources are stored under `~/src`.

```
# clone repository
cd ~/src
git clone https://github.com/sogaiu/tree-sitter-arcy
cd tree-sitter-arcy

# create / populate
#   `node_modules` with dependencies
#   `src` with tree-sitter .c goodness
#   `build`
#   `build/Release` and build `tree_sitter_arcy_binding.node`
npm install

# included in previous command
#npx tree-sitter generate
#npx node-gyp configure
#npx node-gyp rebuild
```

## Grammar Development

Hack on grammar and interactively test.

```
# prepare emsdk (specifically emcc) for building .wasm
source ~/src/emsdk/emsdk_env.sh

# edit grammar.js using some editor

# rebuild tree-sitter stuff and invoke web-ui for interactive testing
npx tree-sitter generate && \
npx node-gyp rebuild && \
npx tree-sitter build-wasm && \
npx tree-sitter web-ui

# in appropriate browser window, paste code in left pane

# examine results in right pane -- can even click on nodes

# find errors and loop back to edit step above...
```

Parse individual files.

```
# create and populate sample code file for parsing named `sample.clj`

# parse sample file
npx tree-sitter parse sample.clj

# examine output similar to web-ui, but less convenient
```

## Measure Performance

```
# single measurement
npx tree-sitter parse --time sample.clj

# mutliple measurements with `multitime`
multitime -n10 -s1 npx tree-sitter parse --time --quiet sample.clj
```

## Build .wasm

Assuming emsdk is installed appropriately under `~/src/emsdk`.

```
# prepare emsdk (specifically emcc) for use
source ~/src/emsdk/emsdk_env.sh

# create `tree-sitter-arcy.wasm`
npx tree-sitter build-wasm
```

## Resources

* [Guide to your first Tree-sitter grammar](https://gist.github.com/Aerijo/df27228d70c633e088b0591b8857eeef)
* [tree-sitter](http://tree-sitter.github.io/tree-sitter/)

## Acknowledgments

* Aerijo - Guide to your first Tree-sitter grammar
* bakpakin - janet
* GrayJack - a better-looking attempt at tree-sitter-janet
* maxbrunsfeld - tree-sitter and related
