# AMD-easy-import


(this is mostly a note to myself)

[AMD](https://github.com/amdjs/amdjs-api/wiki/AMD) seems to defacto way to define modules on a browser (requirejs, browserify, webpack, others I'm sure)

There's this one thing that drives me nuts:

    define [
      "static/lib/ModuleOne"
      "static/lib/ModuleTwo"
      "external/minified/SomethingElse"
    ], (
      ModuleOne
      {SubmoduleOne, SubmoduleTwo}
      SomethingElse
      BrokenImport
    ) ->
       # code
       
After you import more than a few things it kind of explodes. And changing/adding things in some messier codebases usually ends with fingerprints all over my screen as I try to ensure everything lines up.

In personal project I've started doing this little hack:

    dependencies = 
       ModuleOne: "static/lib/ModuleOne"
       _ModuleTwo: "static/lib/ModuleTwo"
       SomethingElse: "external/minified/SomethingElse"

    define (_v for _, _v of dependencies), () ->
      _i=0; @[_k] = arguments[_i++] for _k of dependencies

      {SubmoduleOne, SubmoduleTwo} = _ModuleTwo
      
It's not perfect, but I don't have any off-by-one-line-bugs causing weird behavior in unexpected places anymore.
