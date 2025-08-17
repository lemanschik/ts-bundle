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



## Understand Packaging

### tsconfig
Has overlap with package.json it can hornor and use package.json when moduleResolution is configured to use it else it can ignore that.
So it is one level above the NPM System and allows u to use other resolve methods eg ts aliases while it can also mix that with export aliases
defined in package.json so a highly hacky thing. The Most Best Resolution depending on if you use Node Packages or not is "Bundler" and "Node" (for backward Compat but should get avoided) esnext (Most clean) and Classic(simple does imported.js => imported.d.ts mapping)

### package.json
Has overlap with native Modules. Nativ ECMAScript modules do depend on analyzeable pathes they do not need anything from package.json and get confused when depending on its features.

## Understand Module Authoring
Most best is you Author your Module in Clean ECMAScript and do not add own Types. You will in general always Work with Objects from the Nativ environment so using the types of the Environment is enough if you code clean.
The only kind of types that you can introduce are classes and const assignments as also real complex property access patterns eg records you should always depend on a function that introduces the type. And this function should check
if the type of the passed value of the parm does match that type or a type that can get coreoced into that type. weg Array.from() does so and many other Nativ Platform functions. new String(1) === "1", new Number("1") === 1

avoid TLA Top Level Await avoid any code execution in the Module it self only on module.property() a function should get executed avoid assignment like const me = ()(); do not depend on Module Scope whide set Constants.
Always create a module.property scope around that. 

This avoids the need for optimizations like defer while it also opens up the usage of such patterns. so the Module Consumer can choose if he defers our Module or not. Methods like defer should only get used by the developer that codes 
the most outer part of the software so the Module Integrator your job as module author is to factor a clean Module. that can get used by some one to compose and integrate it into a bigger component of modules and only he knows what should get loaded when and when you depend on such functions to make your module usefull then create a indipendent entrypoint that does that and keep most of the code free from that loading logic hacks.

