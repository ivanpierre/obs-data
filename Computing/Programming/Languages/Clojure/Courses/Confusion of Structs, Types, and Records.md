# [Confusion of Structs, Types, and Records](https://old.reddit.com/r/Clojure/comments/qkg9za/confusion_of_structs_types_and_records/)

- deftype and defprotocol are for interop with host platform class type and OO facilities (e.g. fast polymorphic dispatch)  
- defrecord is a map-like datatype backed by host platform class type, does not support namespaced keywords, it is faster than clojure maps if your keys are known statically and don't have namespace.  
- clojure.core maps are your bread and butter default type, refactor to records for perf sensitive hot spots. Keys are often namespaced  
- defmulti is a higher level polymorphism construct than protocols that doesn't use host platform polymorphism and thus isn't as limited – e.g. host OOP can only dispatch polymorphically on instance of first parameter whereas defmulti can dispatch on any/all parameters; defmulti also has variable arity and feels like a clojure function  
- protocols vs multimethods is an eternal debate, it doesn't matter which you default to and makes little difference for most things

![No alternative text description for this image](media/No_alternative_text_description_for_this_image.jpg)