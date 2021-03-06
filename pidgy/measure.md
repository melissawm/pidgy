# Measure interactive computing activities

To an organization, human compute time bears an important cost
and programming represents a small part of that cycle.

    def load_ipython_extension(shell):

The `metadata` module assists in collecting metadata about the interactive compute process.
It appends the metadata atrribute to the shell.

        shell.measure = Measure(shell=shell).register()

<!--

    import dataclasses, ast, pidgy
    try: from . import base
    except: import base

-->

    @dataclasses.dataclass
    class Measure(base.Extension, ast.NodeTransformer):
        definitions: list = dataclasses.field(default_factory=list)
        def pre_execute(self):
            self.definitions = []

        def visit_FunctionDef(self, node):
            self.definitions.append(node.name)
            return node

        visit_ClassDef = visit_FunctionDef

<!--

    def unload_ipython_extension(shell):
        shell.metadata.unregister()

-->
