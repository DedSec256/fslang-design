# F# RFC FS-1020 - Support byref returns

The design suggestion [Support byref returns to match C# 7](https://fslang.uservoice.com/forums/245727-f-language/suggestions/11125137-expand-support-for-byref-to-match-c-7) has been marked "approved".
This RFC covers the detailed proposal for this suggestion.

* [ ] Details: [under discussion](https://github.com/fsharp/FSharpLangDesign/issues/FILL-ME-IN)
* [ ] Implementation: [In progress](https://github.com/Microsoft/visualfsharp/pull/FILL-ME-IN)


# Summary
[summary]: #summary

C# is adding support for byref locals and returns (see https://github.com/dotnet/roslyn/issues/118, slated for milestone 1.3). This will result in many libraries that expose these features (which the CLR already supports), but methods with such return types aren't currently usable from F#. F# already supports byref locals, but doesn't support implementing byref-returning methods nor does it support calling byref-returning methods.

At a minimum, F# should support calling byref-returning-methods (e.g. SomeRefReturningMethod(x,y,z) <- w), since C# users will be creating methods like these.

It would be nice if on top of that base level of support F# also supported declaring such methods, using the same safety rules that C# is using (e.g. the only refs that are safe to return are those that point to values stored on the heap or existing refs that are passed into the method).


# Motivation
[motivation]: #motivation

To ensure F# can interop with .NET code returning byrefs, and that high-performance address-returning code can be authored in F#.

# Detailed design
[design]: #detailed-design

Here are “safe to return” rules, essentially ensuring that a method does not return variables from its own frame:
 
* byrefs to fields in heap-allocated classes and records are safe to return
* byref-valued parameters are safe to return
* byref to fields in structs are safe to return if a byref to the struct is safe to return
* “this” is not safe to return from struct members
* a byref returned from another method is safe to return if all byrefs passed to that method as formal parameters are safe to return. 

Another thing to mention is the generality of the feature – byref returns are supported on everything that is a sugared kind of a method as well. There are byref returning delegates, properties and indexers. 


# Feature Interactions

TBD

# Drawbacks
[drawbacks]: #drawbacks

The main drawback is the additional surface area added to F#.

# Alternatives
[alternatives]: #alternatives

The C# team have done the design for this feature.  This is achieving parity.

# Unresolved questions
[unresolved]: #unresolved-questions

None