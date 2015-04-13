<!-- maruku -o bool_constant.html bool_constant.md -->

<style type="text/css">
pre code { display: block; margin-left: 2em; }
div { display: block; margin-left: 2em; }
ins { text-decoration: underline; background-color: #A0FFA0 }
del { text-decoration: line-through; background-color: #FFA0A0 }
table.std { border: 1pt solid black; border-collapse: collapse; width: 70%; }
table.std td { border-bottom: 1pt solid black; vertical-align: text-top; }
</style>

<table><tbody>
<tr><th>Doc. no.:</th>	<td>N4389</td></tr>
<tr><th>Date:</th>	<td>2015-02-23</td></tr>
<tr><th>Project:</th>	<td>Programming Language C++, Library Working Group</td></tr>
<tr><th>Revises:</th>	<td>N4334</td></tr>
<tr><th>Reply-to:</th>	<td>Zhihao Yuan &lt;zy at miator dot net&gt;</td></tr>
</tbody></table>

# Wording for bool_constant, revision 1

Rationale see <https://issues.isocpp.org/show_bug.cgi?id=51>.

This paper consolidates
[N4334](http://www.open-std.org/JTC1/SC22/WG21/docs/papers/2014/n4334.html)
with LWG's wording improvements to `<ratio>`.

This wording is relative to N4296.

Modify 20.10.2 &#91;meta.type.synop&#93;:

    namespace std {
      // 20.10.3, helper class:
      template <class T, T v> struct integral_constant;
<div>
<ins><tt>
      template &lt;bool B&gt;<br/>
      using bool_constant = integral_constant&lt;bool, B&gt;;<br/>
      &nbsp;<br/>
</tt></ins>
<del><tt>
      typedef integral_constant&lt;bool, true&gt; true_type;<br/>
      typedef integral_constant&lt;bool, false&gt; false_type;<br/>
</tt></del>
<ins><tt>
      typedef bool_constant&lt;true&gt; true_type;<br/>
      typedef bool_constant&lt;false&gt; false_type;<br/>
</tt></ins>
</div>
> ...

Modify 20.10.3 &#91;meta.help&#93;:

    namespace std {
      template <class T, T v>
      struct integral_constant {
> ...

      };
<div>
<del><tt>
      typedef integral_constant&lt;bool, true&gt; true_type;<br/>
      typedef integral_constant&lt;bool, false&gt; false_type;<br/>
</tt></del>
</div>

    }

> The class template `integral_constant`<ins>, alias template
> <tt>bool_constant</tt>,</ins> and its associated typedefs `true_type`
> and `false_type` are used as base classes to define the
> interface for various type traits.

Modify Table 49 in 20.10.4.3 &#91;meta.unary.prop&#93;:

<div>
<table class="std"><tbody>
<tr>
<td style="width: 40%">
<tt>
template &lt;class T&gt;<br/>
struct is_signed;<br/>
</tt>
</td>
<td style="width: 30%">
If <tt>is_arithmetic&lt;T&gt;::value</tt> is <tt>true</tt>, the same result as
<del><tt>integral_constant&lt;bool, T(-1) &lt; T(0)&gt;::value</tt></del>
<ins><tt>bool_constant&lt;T(-1) &lt; T(0)&gt;::value</tt></ins>
; otherwise, <tt>false</tt>
</td>
<td>
</td>
</tr>
<tr>
<td style="width: 40%">
<tt>
template &lt;class T&gt;<br/>
struct is_unsigned;<br/>
</tt>
</td>
<td style="width: 30%">
If <tt>is_arithmetic&lt;T&gt;::value</tt> is <tt>true</tt>, the same result as
<del><tt>integral_constant&lt;bool, T(0) &lt; T(-1)&gt;::value</tt></del>
<ins><tt>bool_constant&lt;T(0) &lt; T(-1)&gt;::value</tt></ins>
; otherwise, <tt>false</tt>
</td>
<td>
</td>
</tr>
</tbody></table>
</div>

Modify 20.11.5 &#91;ratio.comparison&#93;:

    template <class R1, class R2> struct ratio_equal
<div>
<del><tt>
      : integral_constant&lt;bool, <i>see below</i>&gt; { };<br/>
</tt></del>
<ins><tt>
      : bool_constant&lt;R1::num == R2::num &amp;&amp;
R1::den == R2::den&gt; { };<br/>
</tt></ins>
</div>

> <del>
> If <tt>R1::num == R2::num</tt> and <tt>R1::den == R2::den</tt>,
> <tt>ratio_equal&lt;R1, R2&gt;</tt>
> shall be derived from
> <tt>integral_constant&lt;bool, true&gt;</tt>
> ; otherwise it shall be derived from
> <tt>integral_constant&lt;bool, false&gt;</tt>
> .
> </del>

    template <class R1, class R2> struct ratio_not_equal
<div>
<del><tt>
      : integral_constant&lt;bool,
      !ratio_equal&lt;R1, R2&gt;::value&gt; { };<br/>
</tt></del>
<ins><tt>
      : bool_constant&lt;!ratio_equal&lt;R1, R2&gt;::value&gt; { };<br/>
</tt></ins>
</div>

    template <class R1, class R2> struct ratio_less
<div>
<del><tt>
      : integral_constant&lt;bool, <i>see below</i>&gt; { };<br/>
</tt></del>
<ins><tt>
      : bool_constant&lt;<i>see below</i>&gt; { };<br/>
</tt></ins>
</div>

> If
> <del><tt>R1::num * R2::den &lt; R2::num * R1::den</tt></del>
> <ins><tt>R1::num</tt> &times; <tt>R2::den</tt> is less than
> <tt>R2::num</tt> &times; <tt>R1::den</tt></ins>
> , `ratio_less<R1, R2>` shall be
> derived from
> <del><tt>integral_constant&lt;bool, true&gt;</tt></del>
> <ins><tt>bool_constant&lt;true&gt;</tt></ins>
> ; otherwise it shall be derived from
> <del><tt>integral_constant&lt;bool, false&gt;</tt></del>
> <ins><tt>bool_constant&lt;false&gt;</tt></ins>
> . Implementations may use other algorithms to compute this relationship to
> avoid overflow. If overflow occurs, the program is ill-formed.

<div>
<del><tt>
  template &lt;class R1, class R2&gt; struct ratio_less_equal<br/>
    : integral_constant&lt;bool, !ratio_less&lt;R2, R1&gt;::value&gt; { };<br/>
  template &lt;class R1, class R2&gt; struct ratio_greater<br/>
    : integral_constant&lt;bool, ratio_less&lt;R2, R1&gt;::value&gt; { };<br/>
  template &lt;class R1, class R2&gt; struct ratio_greater_equal<br/>
    : integral_constant&lt;bool, !ratio_less&lt;R1, R2&gt;::value&gt; { };<br/>
</tt></del>
<ins><tt>
  template &lt;class R1, class R2&gt; struct ratio_less_equal<br/>
    : bool_constant&lt;!ratio_less&lt;R2, R1&gt;::value&gt; { };<br/>
  template &lt;class R1, class R2&gt; struct ratio_greater<br/>
    : bool_constant&lt;ratio_less&lt;R2, R1&gt;::value&gt; { };<br/>
  template &lt;class R1, class R2&gt; struct ratio_greater_equal<br/>
    : bool_constant&lt;!ratio_less&lt;R1, R2&gt;::value&gt; { };<br/>
</tt></ins>
</div>

## Acknowledgments

Thanks Tony Van Eerd for bringing this to the reflector.
