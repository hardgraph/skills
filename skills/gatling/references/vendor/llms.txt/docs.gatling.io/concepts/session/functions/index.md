
Sometimes, you might want to dynamic parameters that are too complex to compute for Gatling EL.
Most Gatling SDK methods can also be passed a function to compute your parameter value programmatically.

{{< alert warning >}}
Those functions are executed in Gatling's shared threads, so you must absolutely avoid performing long blocking operations in there, such as remote API calls.
{{< /alert >}}

{{< alert warning >}}
Remember that the [Gatling SDK components are merely definitions]({{< ref "/reference/glossary#sdk" >}}). They only are effective when chained with other SDK components and ultimately passed to the `setUp`. **In particular, they have no effect when used inside functions.**
{{< /alert >}}

## Syntax

Those functions always take a `Session` parameter, so you can extract previously stored data.

The generic signature of these functions is:

* In Java and Kotlin: `Session -> T`
* In Scala: `Expression[T]` is an alias for `Session => Validation[T]`. Values can implicitly lifted in `Validation`.

{{< include-code "function-sample" >}}

{{< alert warning >}}
(Scala Only): For more information about `Validation`, please check out the [Validation reference]({{< ref "validation" >}}).
{{< /alert >}}
