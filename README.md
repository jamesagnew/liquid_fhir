# Liquid Features

## For loop
```
{% for name in Patient.name %}
{% endfor %}
```

# If Else
```
{% if Patient.name == 'kevin' %}
{% elsif Patient.name == 'anonymous' %}
{% else %}
{% endif %}
```

# Case
```
{% case Patient.identifier.use %}
  {% when 'usual' %}
  {% when 'official' %}
  {% else %}
{% endcase %}
```

# Variables
```
The patient's name is {{ Patient.name.family }}
```

# Adding FluentPath

In the example `{% if Patient.name == 'kevin' %}`
name returns a list. How do we compare that using `==`?

1. This is an error
2. Any of the repetitions should match
3. **Prefer this**: We don't use Liquid's operators (==) but instead require
   that the expression be a boolean result of a FluentPath expression, meaning the statement has to look like:
   `{% if Patient.name = "kevin" %}`

# Adding Markdown

What are the considerations if we want to add
Markdown or AsciiDoc to the source? Which one
do we use? Do we process the Markdown
