<!-- maruku -o bool_constant.html bool_constant.md -->

<style type="text/css">
pre code { display: block; margin-left: 2em; }
div { display: block; margin-left: 2em; }
ins { text-decoration: none; font-weight: bold; background-color: #A0FFA0 }
del { text-decoration: line-through; background-color: #FFA0A0 }
</style>

<table><tbody>
<tr><th>Doc. no.:</th>	<td>Nnnnn</td></tr>
<tr><th>Date:</th>	<td>2014-11-21</td></tr>
<tr><th>Project:</th>	<td>Programming Language C++, Library Working Group</td></tr>
<tr><th>Reply-to:</th>	<td>Zhihao Yuan &lt;zy at miator dot net&gt;</td></tr>
</tbody></table>

# Wording for bool_constant

Rationale see <https://issues.isocpp.org/show_bug.cgi?id=51>.

This wording is relative to N4140.

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

    }

> The class template `integral_constant`<ins>, alias template
> <tt>bool_constant</tt>,</ins> and its associated typedefs `true_type`
> and `false_type` are used as base classes to define the
> interface for various type traits.

Replace all occurrences of "`integral_constant<bool, `"
with "`bool_constant`" in 20.11.5 &#91;ratio.comparison&#93;.
