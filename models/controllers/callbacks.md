## Callbacks

Rails supports 3 types of callbacks: before, after, around.
Such callbacks are called just prior or and/or just after the execution of actions.

If before action callback returns false, then processing of the callback chain terminates,
and the action is not run. A callback may also render output or redirect requests, in which case the
original action never gets invoked.

Callback declarations also accept blocks and the names of classes. If a block is specified, it will be called
with the current controller as a parameter. 
If a class is given, it's filter() class method will be called with the controller as a parameter.

By default, callbacks apply to all actions in a controller (and any subclasses of controller).
You can modify this with options:

- :only - it takes one or more actions on which the callback is invoked
- :except - it lists actions to be excluded from callback

The `before_aciton` and `after_action` declarations append to the controller's chain of callbacks.
Use the variants `prepend_before_action()` and `prepend_after_action()` to put callbacks at the front of the chain.

After callback can be used for compressing the response if the user's browser supports it.

Around callbacks: if callback code invokes `yield`, the action is executed. 
When the action completes, the callback code continues executing.
If the callback code never invokes `yield`, the action is not run.

The benefit of around callbacks is that they can retain context across the invokation of the action.

As well as passing `around_action` the name of a method, you can pass it a block or a filter class.

If you don't want a particular callback to run in a child controller, 
you can override the default processing with the `skip_before_action` and `skip_after_action` declarations.
This accept the `:only` and `:except` params.

Also you can use `skip_action` to skip any action callback (before, around, after).
However, it works only for callbacks that were specified as the (symbol) name of a method.


# Helpers

A helper is simply a module containing methods that assist a view. Helper methods are output-centric.
They exist to generate HTML (or XML, or Javascript) - a helper extends the befavior of a template.
