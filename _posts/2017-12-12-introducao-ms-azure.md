---
layout:     post
title:      Introdu��o ao Microsoft Azure
date:       2017-12-12 10:48:19
summary:    See what the different elements looks like.
categories: Azure
thumbnail: cogs
tags:
 - azure
 - cloud
---
# Introdu��o

Diante dos avanços tecnológicos que inserem diversos desafios no nosso dia-a-dia, é preciso estar de olho nas mudanças tecnológicas, antes mesmo de sua consolidação no mercado.

A fim de auxiliar nesta questão, eu desenvolvi um guia de introdução ao Azure, a nuvem pública da Microsoft.

Você vai conhecer os principais componentes do Azure e nos próximos artigos você vai acompanhar uma série, explorando com mais detalhes cada um dos itens explicados neste guia introdutório.

Importante ressaltar que esses artigos servem como base nos estudos para a certificação 70-533.

#Portal de gerenciamento

O Azure possui uma interface web que nos permite administrar e gerenciar praticamente todos os recursos disponíveis, incluindo máquinas virtuais, databases, web apps, redes virtuais, etc.

Imagine um console único de fácil utilização, que além de simplificar o processo de implementação e o gerenciamento dos recursos em nuvem, também fornece a possibilidade de personalizar seu portal de acordo com as suas necessidades, por exemplo:

    Criar vários dashboards;
    Inserir nomes de acordo com o seu propósito;
    Compartilhar os dashboards.

Veja aqui um dashboard com algumas customizações

**Note** - This article is a derivative of ["See pixyll in action"][1], taken from the lovely jekyll theme [pixyll][4].

All links are easy to [locate and discern](#), yet don't detract from the harmony
of a paragraph. The _same_ goes for italics and __bold__ elements. Even the the strikeout
works if <del>for some reason you need to update your post</del>. For consistency's sake,
<ins>The same goes for insertions</ins>, of course.

### Code, with syntax highlighting

Code blocks use the [peppermint][2] theme.

{% highlight ruby %}
class Awesome < ActiveRecord::Base
  include EvenMoreAwesome

  validates_presence_of :something
  validates :email, email_format: true

  def initialize(email, name = nil)
    self.email = email
    self.name = name
  end
end
{% endhighlight %}

```html
<!DOCTYPE html>
<title>Title</title>

<style>body {width: 500px;}</style>

<script type="application/javascript">
  function $init() {return true;}
</script>

<body>
  <p checked class="title" id='title'>Title</p>
  <!-- here goes the rest of the page -->
</body>
```

# Headings!

They're responsive, and well-proportioned (in `padding`, `line-height`, `margin`, and `font-size`).

##### They draw the perfect amount of attention

This allows your content to have the proper informational and contextual hierarchy. Yay.

### There are lists, too

  * Apples
  * Oranges
  * Potatoes
  * Milk

  1. Mow the lawn
  2. Feed the dog
  3. Dance

### Images look great, too

![Thumper](https://i.imgur.com/DMCHDqF.jpg)


### Stylish blockquotes included

You can use the markdown quote syntax, `>` for simple quotes.

> Lorem ipsum dolor sit amet, consectetur adipiscing elit. Suspendisse quis porta mauris.

### LaTeX support

The default math delimiters are \$\$. Hence `$$ E = m \cdot c^2 $$` yields $$ E = m \cdot c^2 $$

And here's something more fancy:

$$ \zeta(s) = \frac{1}{\Gamma(s)} \int \limits_0^\infty x^{s-1} \sum_{n=1}^\infty e^{-nx} \mathrm{d}x = \frac{1}{\Gamma(s)} \int \limits_0^\infty \frac{x^{s-1}}{e^x - 1} \mathrm{d}x $$


### There's more being added all the time

Checkout the [Github repository][3] to request,
or add, features.

Happy writing.

[1]: http://pixyll.com/jekyll/pixyll/2014/06/10/see-pixyll-in-action/
[2]: https://noahfrederick.com/log/lion-terminal-theme-peppermint/
[3]: https://github.com/jacobtomlinson/carte-noire
[4]: http://pixyll.com/
