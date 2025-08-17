# A ReImplementation of Rollup with typescript first support.

## Why not rollup?
Rollup is mainly designed to address the needs of bundling ECMAScript Modules while most Projects today get often Authored in Typescript.
And even the People That Author in native ECMAScript but do depend on Node Packages do use the types of the Packages they Consume.

often this leads to complex project configurations and issues like the default export amibugity. This bundler Aims to Produce Modules
that fullfill the module-sync target defintion of the package.json it describes a entrypoint that is require and import compatible.
It is Mainly a ESModule without TLA Top Level Await usage. Such a module when bundled is indeed also a CJS Module. Thats why the both 
module types are compatible. There are rare Edge cases when loaders or other global vars get patched by eg CJS Telemetry implementations and so on.

Long Story short for 99.9999999% of cases this bundler automagicaly creates good software and can even refactor bad dependencys. It can even bundle
types. and it supports loading phases see: https://github.com/tc39/proposal-defer-import-eval.

This can also turn codebases that depend on this often none needed optimizations into clean once without that defer so that you can control the load
via service workers and script tags in the right order as usal done on frontend where module splitting is done per page and not per module and often 
its not needed to have a indipendent js module per page a design failure of eg: webpack where you ship everything first and most time is not needed.

This can split eg on property access patterns of your frontend module code and this way lead to much cleaner codebases.

