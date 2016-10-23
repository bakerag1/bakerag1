---
layout: post
title: maven dependency scope
---
# Keeping your code portable with Maven dependency scope
If you, as you should, try to keep your code portable by avoiding imports of implementations of standards, such as JAX-RS, JPA, or Bean Validation, you might find yourself accidentally importing the wrong packages, because they are both on your classpath. It is all to easy to accidentally bring these dependencies into your code, they frequently have the same name and the compiler will suggest them. Maven has a clever parameter in the dependency model that many don't know, don't use, or don't understand. It's called *scope* and is a familiar term to programmers, but they seem to forget that maven dependencies, as with variables, you should try to limit their scope. The names of the scope aren't very intuitive, but they make sense with a little thought.

**Compile** is the default scope and provides the dependency in every phase of your project, compile, test, and package (the deployable artifact). When you add a dependency, try to avoid this one. You will need it quite often, but it's worth thinking twice about it. For utilities like Apache Commons, you can't avoid it. For anything with "-rt" in the artifact name, you should be able to move away from compile.

**Provided** is very common if you are deploying to a robust application server, like Tommee+ or Weblogic. This keeps the dependency on your classpath for compile and test, but leaves it out of the deployable. If you can use this scope, you can feel free to use bulky, excessive bundles, since none of it is packaged, but only if you need to actually code against this. API's that are included in your **runtime** libraries get this scope so you don't get duplication. So, you can mark your JPA API with `provided`, and the implementation bundle, as `runtime`.

**Runtime** should also get a lot of use. It puts the dependency in your class path only during test and package. This is for projects deploying to a thin server. It prevents you from coding to an implementation, but allows you to have an implementation in your tests, for things you don't want to mock, and as part of your deployment. Be careful about what you add as runtime, you don't want your deployment to use up too much space.

**Test** is an easy one, if you only need the dependency during testing, mark the scope as test. JUnit, easymock, and so on, are only needed during testing. If you are writing a test and have to add a dependency to make it work, mark it as test, you can change it later, if it turns out to be needed elsewhere.

**System** is like **provided** but not fulfilled by your artifact repository, instead, you supply the location on the filesystem where the library can be found. This should usually be avoided, as it's inherently more difficult for a new developer to get up to speed, but you may have reason now and again.
