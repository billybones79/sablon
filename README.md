# Sablon

[![Build Status](https://travis-ci.org/senny/sablon.svg?branch=master)](https://travis-ci.org/senny/sablon)

Is a document template processor based on `docx`. Tags are represented using
MailMerge fields.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'sablon'
```


## Usage

```ruby
require "sablon"
template = Sablon.template(File.expand_path("~/Desktop/template.docx"))
context = {
  title: "Fabulous Document",
  technologies: ["Ruby", "Markdown", "ODF"]
}
template.render_to_file File.expand_path("~/Desktop/output.docx"), context
```


### Writing Templates

Sablon templates are normal word documents (`.docx`) sprinkled with MergeFields
to perform operations. The following section will use the notation `«=title»` to
refer to [Word MailMerge](http://en.wikipedia.org/wiki/Mail_merge) fields.

#### Inserting content

The most basic operation is to insert content. The contents of a context
variable can be inserted using a field like:

```
«=title»
```

It's also possible to call a method on a context object using:

```
«=post.title»
```


#### Conditionals

Sablon can render parts of the template conditonally based on the value of a
context variable. Conditional fields are inserted around the content.

```
«technologies:if»
    ... arbitrary document markup ...
«technologies:endIf»
```

This will render the enclosed markup only if the expression is truthy.
Note that `nil`, `false` and `[]` are considered falsy. Everything else is
truthy.

For more complex conditionals you can use a predicate like so:

```
«body:if(present?)»
    ... arbitrary document markup ...
«body:endIf»
```

#### Loops

Loops repeat parts of the document.

```
«technologies:each(technology)»
    ... arbitrary document markup ...
    ... use `technology` to refer to the current item ...
«technologies:endEach»
```

Loops can be used to repeat table rows or list enumerations. The fields need to
be placed in within table cells or enumeration items enclosing the rows or items
to repeat. Have a look at the
[example template](test/fixtures/sablon_sample.docx) for more details.


#### Nesting

It is possible to nest loops and conditionals.


### Examples

[This test](test/sablon_test.rb) is a good example of what Sablon can do for
you. Make sure to look at the [template](test/fixtures/sablon_template.docx) and
the corresponding [output](test/fixtures/sablon_sample.docx).


## Contributing

1. Fork it ( https://github.com/[my-github-username]/sablon/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request

## Inspiration

The following projects address a similar goal and inspired the work on Sablon:

* [ruby-docx-templater](https://github.com/jawspeak/ruby-docx-templater)
* [docx_mailmerge](https://github.com/annaswims/docx_mailmerge)
