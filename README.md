# ID Generator Macros

[![Version](https://img.shields.io/github/v/release/Nuhser/jinja-id-generators)](https://github.com/Nuhser/jinja-id-generators/releases/latest)
[![Last commit](https://img.shields.io/github/last-commit/Nuhser/jinja-id-generators)](https://github.com/Nuhser/jinja-id-generators/commits/master/)
[![GitHub Repo stars](https://img.shields.io/github/stars/Nuhser/jinja-id-generators)](#)
<br/>
[![HACS](https://img.shields.io/badge/HACS-Default-41BDF5.svg?logo=home-assistant-community-store)](https://github.com/hacs/default)

Lately, I found myself in need of generating (pseudo-)unique IDs in Home Assistant. Therefore, I created a few macros which allow you to generate IDs inside of templates.

There are macros for IDs using only numbers, hexadecimal characters, only letters, or even completely custom characters. The pattern of those IDs can be customized as well. You can use the default pattern (3 blocks with 8 characters each, separated by a hyphen) or define your own (e.g. `#XXXXX` or `XXX.XXX`).

## Installation

<details>

<summary>Without HACS</summary>

<br>

1. Download the contents from [id_generators.jinja](https://github.com/Nuhser/jinja-id-generators/blob/master/id_generators.jinja) from the master-branch.
2. Add the file to your `<config>/custom_templates`-folder.
3. Reload your configuration from within the developer tools in Home Assistant.
4. The template should now be ready for use.

</details>

<details>

<summary>With HACS</summary>

<br>

1. Search for "*ID Generators*" in HACS.
2. Download the repository via HACS.
3. The template should now be ready for use.

You can also use this button to add the repository to your Home Assistant instance via HACS:

[![Open your Home Assistant instance and open a repository inside the Home Assistant Community Store.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=Nuhser&repository=jinja-id-generators&category=Template)

</details>

## General Information

### ID Patterns

All of the macros have a parameter called `pattern`. This parameter defines the format and length of the generated ID. The parameter is optional. If not used, the generators will use the default pattern of **3 blocks with 8 characters each separated by a hyphen**. For example:

```text
12345678-12345678-12345678
```

To change this, set the `pattern` to a different string. Each character that should be replaced randomly during the generation of the ID should be a `$`. The default pattern looks like this:

```text
$$$$$$$$-$$$$$$$$-$$$$$$$$
```

Each of the `$` in the pattern above would be replaced by a random character from the list of characters available for the given generator-macro.

>[!IMPORTANT]
> Due to the way the pattern is replaced with random characters, it's not possible to have a `$` as a symbol in the finished ID. This symbol would be replaced during generation.

### Randomness

The random IDs are going to change every time you change something about the template in which they are used as the macro inside the template will be newly evaluated every time the template is rendered.

You can see this in the template test environment inside Home Assistant's developer tools. The generated ID in the output will change every time if you create a template in here which uses one of the ID generators and refresh the page, remove one of the curly brackets and add it again or change the template in any other way.

## Macro Documentation

### id_dec

Generates an ID using the given pattern (or the default pattern if no pattern is specified) using only **numbers from 0 to 9**.

#### Syntax

`id_dec(pattern='$$$$$$$$-$$$$$$$$-$$$$$$$$')`

| Param | Type | Explanation |
| :----- | :---- | :----------- |
| `pattern` | `string` | Can be used to define a custom pattern for the generated ID (see above). (*Default:* `DEFAULT_PATTERN`) |

#### Examples

**Input**

```jinja2
{%- from 'id_generators.jinja' import id_dec -%}
{{ id_dec() }}
{{ id_dec(pattern='#$$$$$$')}}
```

**Example Output**

> Yours might be different due to randomness.

```text
75752723-69849259-97443507
#870118
```

*********************

### id_hex

Generates an ID using the given pattern (or the default pattern if no pattern is specified) using **hexadecimal characters (0-9 & a-f)**.

#### Syntax

`id_hex(pattern='$$$$$$$$-$$$$$$$$-$$$$$$$$')`

| Param | Type | Explanation |
| :----- | :---- | :----------- |
| `pattern` | `string` | Can be used to define a custom pattern for the generated ID (see above). (*Default:* `DEFAULT_PATTERN`) |

#### Examples

**Input**

```jinja2
{%- from 'id_generators.jinja' import id_hex -%}
{{ id_hex() }}
{{ id_hex(pattern='$$$.$$$')}}
```

**Example Output**

> Yours might be different due to randomness.

```text
e42d2901-898054f3-e8091959
826.aca
```

*********************

### id_alpha

Generates an ID using the given pattern (or the default pattern if no pattern is specified) using only **letters (a-z)**.

#### Syntax

`id_alpha(pattern='$$$$$$$$-$$$$$$$$-$$$$$$$$')`

| Param | Type | Explanation |
| :----- | :---- | :----------- |
| `pattern` | `string` | Can be used to define a custom pattern for the generated ID (see above). (*Default:* `DEFAULT_PATTERN`) |

#### Examples

**Input**

```jinja2
{%- from 'id_generators.jinja' import id_alpha -%}
{{ id_alpha() }}
{{ id_alpha(pattern='$$$_$$$_$$$')}}
```

**Example Output**

> Yours might be different due to randomness.

```text
wscfzwsp-zzkmabci-qmuhwvxu
vyh_oej_cbs
```

*********************

### id_alphanum

Generates an ID using the given pattern (or the default pattern if no pattern is specified) using **numbers and letters (0-9 & a-z)**.

#### Syntax

`id_alphanum(pattern='$$$$$$$$-$$$$$$$$-$$$$$$$$')`

| Param | Type | Explanation |
| :----- | :---- | :----------- |
| `pattern` | `string` | Can be used to define a custom pattern for the generated ID (see above). (*Default:* `DEFAULT_PATTERN`) |

#### Examples

**Input**

```jinja2
{%- from 'id_generators.jinja' import id_alphanum -%}
{{ id_alphanum() }}
{{ id_alphanum(pattern='$$$$$$$$$$')}}
```

**Example Output**

> Yours might be different due to randomness.

```text
sekhaaj9-2quryih1-p7eeiid4
9op7xnfqbz
```

*********************

### id_custom

Generates an ID using the given pattern (or the default pattern if no pattern is specified) using characters from a custom list.

#### Syntax

`id_custom(characters, pattern='$$$$$$$$-$$$$$$$$-$$$$$$$$')`

| Param | Type | Required | Explanation |
| :----- | :---- | :--- | :----------- |
| `characters` | `list` | Yes | A list of characters from which the generator will choose randomly during ID generation. *Longer strings are possible as well (see example 2 below).* |
| `pattern` | `string` | No (*Default:* `DEFAULT_PATTERN`) | Can be used to define a custom pattern for the generated ID (see above). |

#### Examples

**Input**

```jinja2
{%- from 'id_generators.jinja' import id_custom -%}
{{ id_custom(['a', 'b', 1, 2, 3]) }}
{{ id_custom(characters=['foo', 'bar', 'hello', 'world'], pattern='$ $ $ $')}}
```

**Example Output**

> Yours might be different due to randomness.

```text
b31b3aba-b21b13bb-11121113
hello foo foo bar
```
