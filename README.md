# LiquiFhir Features

## For loop

```
{% for name in Patient.name %}
{% endfor %}
```

## If Else
```
{% if Patient.name == 'kevin' %}
{% elsif Patient.name == 'anonymous' %}
{% else %}
{% endif %}
```

## Case
```
{% case Patient.identifier.use %}
  {% when 'usual' %}
  {% when 'official' %}
  {% else %}
{% endcase %}
```

## Variables
```
The patient's name is {{ Patient.name.family }}

```

## Whitespace Control

In Liquid, you can include a hyphen in your tag syntax `{{-`, `-}}`, `{%-`, and `-%}` to strip whitespace from the left or right side of a rendered tag.
Normally, even if it doesn’t output text, any line of Liquid in your template will still output a blank line in your rendered HTML:

```
{% assign my_variable = "tomato" %}
{{ my_variable }}
```

Notice the blank line before “tomato” in the rendered template:

output:

```

Tomato
```

By including hyphens in your `assign` tag, you can strip the generated whitespace from the rendered template:

```
{%- assign my_variable = "tomato" -%}
{{ my_variable }}
```

output:

```
tomato
```






## Adding FhirPath

In the example `{% if Patient.name == 'kevin' %}`
name returns a list. How do we compare that using `==`?

1. This is an error
2. Any of the repetitions should match
3. **Prefer this**: We don't use Liquid's operators (==) but instead require
   that the expression be a boolean result of a FhirPath expression, meaning the statement has to look like:
   `{% if Patient.name = "kevin" %}`

To distinguish between FhirPath and Liquid, you can enclose the FhirPath expressions in brackets. In the example below `humanname.family` is the FhirPath expression, of which the result will be capatilized using Liquid's capitalize function.

```
{% for given in (humanname.given) | capitalize %}
    {{ given }}
{% endfor %}
```

## Using Templates

Users may want to re-use certain templates. For example, in the Patient resource, there are two elements that describe an address: one for the patient, and one for a patient's contact. 

```
{% for nextAddress in Patient.address %}
  <tr>
    <td>Address ({{ nextAddress.use }})</td>
    <td>
       {% template(address, nextAddress) %}
    </td>
  </tr>
{% endfor %}
```
calls the template in ```address.narrative``` which looks like this with `nextAddress` as input parameter.

```
{% for line in address.line %}
  {% line %}<br/>
{% endfor %}
{%- address.city -%}
{%- address.state -%}
{%- address.postalCode -%}<br/>
{% address.country %}
```

## Adding Markdown or AciiDoc?
As we expect most of the narrative will be generated from content inside the structured part of the resource, and no large amounts of text are expected to be added to that, we currently do not support an editing language like Markdown or AsciiDoc, but only HTML.

If we want to support something like that in the future, we decided on using the CommonMark flavour of Markdown. 