Hotkeys, read all about it!

## Backstory
I recently purchased [the ultimate hacking keyboard](https://ultimatehackingkeyboard.com). Currently, the trackball module is still in design. In a way, this is a good thing. I am now inspired to really use the keyboard as designed and move away from a traditional mouse. The `caps lock` is replaced with `mouse` as a different way of navigating. For example, hold or double tap `mouse` and move the arrows (using the remapped arrow keys). This may sound like a steep learning curve already. To be honest, this is the first ergo split mechanical whatever other term keyboard I have tried that is the most intuitive and well thought out. The parts that _seem_ like a steep learning curve are actually not because of the design.

What I realized is there are many key bindings or ways of moving around a computer that do not actually require a mouse. [The ultimate hacking keyboard](https://ultimatehackingkeyboard.com) made it more obvious and functional while still keeping the base layout of a standard keyboard the same. It is the first mechanical ergo keyboard I was able to use out of the box. Some of the other ergo keyboards are so customizable, I did not know where to begin and became stressed out trying to simply type my name. This keyboard is still customizable but I have found there were only two keys I needed to remap (swapping the placement of the `fn` and `command` keys). I did play around with making my own custom key bindings but that was more to test out the software. In which, the software works as expected and is easy to use.

Ok great, I'm making progress with this new keyboard and to be honest, every other keyboard is sub-standard to me at this point. I switched to a regular keyboard to feel the difference and it lasted less than 5 seconds.

I can admit, moving around the pointer using the keyboard and arrows can be a bit annoying. Even with the "speed booster" (I forget what it is called but it is the equivalent of faster scrolling speed), this keyboard makes me want to figure out how to get to things without needing the pointer. This journey has also exposed Accessibility to me in ways that I never thought I would care about.

## `tabindex` matters!
It is honestly fantastic to tab your through a website and get where you need. Even without `tabindex`, a general tabbing journey can _sometimes_ get you where you need to go. Something that really through me off was why `tab`ing through web applications worked in Chrome but not Safari. Hmmm is this a cross browser support thing? Thank goddess I am a Backend Software Engineer because I do not miss dealing with this fun. However, this really bothered me and I wanted to get to the bottom of it. I have some fullstack vanilla ruby on rails applications laying around, let me see what I can figure out. Hmmm ok, ya, I was able to reproduce on my machine, `tab`ing (without actually having a `tabindex` set) works in Chrome but not Safari.

Lo and behold, it is not a cross-browser support issue it is an Apple Default Issue. I find Apple to _generally_ have sensible defaults. In this case, major fail (-1)! See https://stackoverflow.com/a/1914496/7477016 for resolution.

## `hotkeys`, let's do this!
Ok, I can tab my way through web applications but sometimes that does not feel efficient either. Sure, many native applications have keyboard shortcuts but I rarely see web applications with such. I noticed the email service [hey](https://hey.com) has very obvious hotkeys. Designed with Accessibility in mind (+1). Being a rails developer and reading plenty of DHH writings on really going for out of the box html and seemingly avoiding JavaScript at all costs. I thought there must be an html attribute for this. Turns out there is. It is also turns out the implementation of `accesskey` is pretty poor and JavaScript is actually encouraged as an alternative for this https://webaim.org/techniques/keyboard/accesskey#standard.

Lets Browser Inspect [hey](https://hey.com) and see who won in html `accesskey` vs JS `hotkey`. Lo and behold, JS wins it with (hotkey)[https://github.com/github/hotkey].

Out of curiosity, lets see how `accesskey` vs `hotkey` works.

## accesskey implementation using [rails](https://rubyonrails.org)
```
<%= link_to("Foo Bars", foo_bar_path, accesskey: "1") %>

# generates
<a accesskey="1" href="/foobars">Foo Bars</a>
```

Ok clean enough. However, if I wanted to provide additional guidance on how to use, the key maps are different on Windows vs Mac, see https://www.w3schools.com/tags/att_global_accesskey.asp. That would require generating dynamic text based of Operating System (OS) detection, having an awkward looking legend somewhere or something probably not very clean.

## hotkeys on [rails](https://rubyonrails.org)

First Install [hotkeys](https://github.com/github/hotkey). I browsed through the code repository and this is a very light weight package (+1).
```
link_to(sanitize("Foo Bars <kbd>^1</kbd>"), foo_bar_path, data: { hotkey: "Control+1" })

# generates
<a data-hotkey="Control+1" href="/foobars">Foo Bars <kbd>^1</kbd></a>
```

Now I can provide a consistent keyboard shortcut with a simple text regardless of a users Operating System!

There are plenty of things to consider before implementing hotkeys. GitHub even provides references in [hotkeys](https://github.com/github/hotkey) regarding things to consider. I think the most important is actually providing indicators for what the keymaps are.

### Respect
- To [GitHub](https://github.com) for open sourcing a package that is very straight forward and fun to use!
- To [markdown-to-medium](https://github.com/yoshuawuyts/markdown-to-medium). I was surprised Medium does not support markdown! Kudos to Open Source for solving!
