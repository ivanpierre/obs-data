## Why Learning NixOS is Difficult, and How to Fix It
- [[#Why NixOS]]
- [[#What Is So Hard About Learning Nix OS]]
	- [[#Wherein I Rant About The Documentation For A While]]
	- [[#The Nix Expression Language and Package Manager]]
	- [[#…So Let’s Fix the Docs…]]
- [[#The Bigger Picture]]
- [[#How to Start NixOS: an Opinionated Guide]]
    - [[#Before Installing]]
    - [[#Installing]]
    - [[#Post Install]]

About two months ago I got a new laptop. I had been using MacBook Pro laptops for almost a decade, but for a variety of reasons–reported keyboard issues, the infamous touch bar, my building dissatisfaction with macOS itself–I decided to give Linux on a recent Thinkpad a go.

As a Haskell…enthusiast, it’s hard to hang out in #haskell or anywhere there are Haskell programmers without [Nix](https://nixos.org/nix/) or [NixOS](https://nixos.org/) coming up at some point. So when I was considering what distribution to try, NixOS was an obvious choice, especially after reading a bit more about it. (“A lazy functional language for package management!” was enough to suck me in, if I’m honest.) And since I’d already tried most other major distributions out there at some point, I figured I may as well give NixOS a shot.

Alas, NixOS has a steep learning curve and is hard to get comfortable with. While I’m much better at it than I was a few months ago, I still have a ways to go before I’m a NixOS master. However, I’m at a point where I am comfortable enough with the system that I can be productive without the OS getting in the way. I know where to look and how to get answers when I don’t know how to do something. Most importantly, I’m starting to really appreciate the benefits of NixOS compared to other distributions–in particular, it blows me away how quick and easy it is to spin up a heavily customized development environment with [`nix-shell`](https://nixos.org/nix/manual/#sec-nix-shell). NixOS is a joy to use–once you get it working.

This post is my attempt to document and share my experience of getting (somewhat) comfortable in NixOS. My goal is to clarify my thoughts around this experience, share criticisms now that I’ve had some time to think about the issues I ran into, and eventually find a way to contribute more meaningfully to the NixOS documentation itself using what I’ve learned. Finally, I hope that the last section of this post can help others avoid some of the pitfalls I encountered when getting started with NixOS.

I want to be clear that, despite my complaints and despite the issues I ran into, I think NixOS is worth the trouble, and I want to see the project continue to improve and thrive.

## Why NixOS?

Briefly:

-   You appreciate immutable infrastructure and like the idea that you can roll your system back if something goes wrong
-   You’ve heard Nix is great for development, especially [Haskell development](https://github.com/Gabriel439/haskell-nix), according to all those cool [Haskell](https://www.ocharles.org.uk/blog/posts/2014-02-04-how-i-develop-with-nixos.html) hackers you are always [hanging out with](https://functor.tokyo/)
-   The idea of a lazy functional language as the way you declaratively configure your system sounds super cool to you, especially one that’s the result of a [PhD thesis](https://nixos.org/~eelco/pubs/phd-thesis.pdf)

…where “you” is “me,” and those are all the reasons **I** wanted to learn NixOS. I won’t go into it past this, there’s been enough written on why you should consider NixOS already–if you are into Haskell, check out the links above, and otherwise check out the [NixOS About page](https://nixos.org/nixos/about.html) for more reasons you may want to give NixOS a shot.

## What Is So Hard About Learning Nix(OS)?

### Wherein I Rant About The Documentation For A While

> [21:08 <ddellacosta> the docs are weirdly terrible and great at the same time](https://logs.nix.samueldr.com/nixos/2019-02-28#1999797;)  
> 21:09 <hyperfekt> this ^  
> 21:09 <jasongrossman> Yes.  
> 21:09 <illegalprime> ^ yes  

The NixOS installation manual seems to assume familiarity with Nix, and leaves you at the end of the installation process with little guidance regarding best practices or how to approach many common system setup tasks as a new user. What is the _practical_ difference between installing something using `nix-env` vs. putting it in my `configuration.nix`, and why would I do one or the other? How much should I configure software using dotfiles in my home directory vs. putting various configuration tweaks in `configuration.nix`…and why? (Hint: [home-manager](https://github.com/rycee/home-manager) exists.)

At the same time, there is a tremendous amount of confusing and redundant information scattered around–why is there a [syntax summary](https://nixos.org/nixos/manual/#sec-nix-syntax-summary) and (confusing, incomplete) instructions on how to write [my own derivation](https://nixos.org/nixos/manual/#sec-custom-packages) in the NixOS manual? I’d rather have a more in-depth explanation of the differences between installing software as the ‘system’ user vs. via `nix-env`, why I may want to use `pkgs.writeScriptBin` for adding a script vs. creating a new derivation, and the like. If necessary, we can link to the Nix docs for more on the specifics of the [Nix Expression Language](https://nixos.org/nix/manual/#ch-expression-language) or [how to write a derivation](https://nixos.org/nix/manual/#chap-writing-nix-expressions). (But, note that there’s a problem with that last section I linked to–see the issue I discuss below.)

When I first started, after I had completed basic installation and wanted to start customizing various things, I spent a week figuring out how to write my own derivation for a udev rule–an adventure unto itself–before I learned that `pkgs.writeScriptBin` exists. And the way I found out was a random comment in IRC pointing me at a [random page in the wiki](https://nixos.wiki/wiki/Nix_Cookbook).

Furthermore, when it comes to getting problematic hardware or software working, you are on your own. If you happen to be using a laptop with an NVidia Optimus card like me (note: I do **not** recommend this) hopefully you will discover the (confusingly written) [wiki page on that topic](https://nixos.wiki/wiki/Nvidia), because the [instructions in the manual](https://nixos.org/nixos/manual/#sec-x11-graphics-cards-nvidia) don’t actually apply to you–surprise! (That only cost me a few days of fumbling around trying to get X11 to start using my NVidia card.) Otherwise you will need to start hitting a search engine and cross your fingers that someone else using Nix has encountered your specific issue, or hope that you can figure out how to apply the approach a user of a different distribution has taken to NixOS (there is a LOT of this).

I have experience installing [Linux from Scratch](http://www.linuxfromscratch.org/) and ran [Gentoo](https://www.gentoo.org/) for years along with a handful of other distributions, not to mention the fact that I have experience as a programmer using Linux systems in production environments. I’m used to banging my head against obscure Linux configuration issues and compilation errors, but even I found solving some problems in NixOS challenging. A number of times this was simply because, even if documentation was present, it was conflicting, confusing, or undiscoverable.

I’m probably pretty close to the target audience for NixOS (if there is such a thing), and that’s fine–but I can’t imagine what it’d be like for someone without my experience (or stubbornness).

### The Nix Expression Language and Package Manager

The [Nix expressions](https://nixos.org/nix/manual/#chap-writing-nix-expressions) you will encounter early on using NixOS are combinations of Nix language constructs and built-in functions, global Nix packaging conventions (e.g. the “callPackage design pattern” as the [Nix Pills](https://nixos.org/nixos/nix-pills/callpackage-design-pattern.html) label it), package-specific qualified names, and, if using NixOS as a development platform for languages like Python, Rust, or Haskell, language- and platform-specific Nix packaging conventions. Taken together, this presents beginners with a deeply confusing mix of names and values which is difficult to disentangle and form a mental model of without serious effort–and much of what I’ve just mentioned is not documented anywhere outside of the code.

As Eelco Dolstra writes in [these slides](https://nixos.org/~eelco/talks/guix-feb-2018.pdf), there are a lot of ad-hoc abstractions in the [nixpkgs codebase](https://github.com/NixOS/nixpkgs). Additionally, because of the nature of how language-specific package managers need to integrate with Nix (“our nemesis,” per slide 10), you may end up with, for example, an entire [library of package-manager-specific functions undocumented outside of the code itself](https://github.com/NixOS/nixpkgs/blob/master/pkgs/development/haskell-modules/lib.nix). And that’s leaving aside how inconsistently and confusingly fundamental, ubiquitous functions like [`callPackage`](https://nixos.org/nix/manual/#sec-arguments) are documented.

If that’s not enough, the documentation has a number of mistakes, inconsistencies, and non-working examples. One that bit me early on was the [first and most basic example in the documentation](https://nixos.org/nix/manual/#ch-simple-expression) for writing a derivation. The example given does not work as-is. This was [raised as an issue in the nix repo](https://github.com/NixOS/nix/issues/2259), but _it has been open since June 27th, 2018_ with no apparent forward progress since then.

### …So Let’s Fix the Docs…?

But how do you update the NixOS documentation? Well, there’s nothing obvious on the [Support](https://nixos.org/nixos/support.html) page, which is the most obvious place to find documentation. A top-level “Documentation” link would be nice, but I can live without it. How about [Community](https://nixos.org/nixos/community.html)? Well, there’s “Contributing” but that’s all about nixpkgs, that doesn’t have anything to do with documentation, clearly. There’s a specific “Documentation” section for that, which seems mostly just redundant with the links on the Support page, but I guess that’s not a big deal.

I can create a wiki account and edit the wiki, without a lot of trouble, it would seem. But I guess I can’t contribute documentation otherwise? Who maintains the docs?

Surprise! NixOS documentation is just another package in the `nixpkgs` repo (and the Nix manual is [here](https://github.com/NixOS/nix/tree/master/doc/manual), which is arguably a bit more discoverable, but not by much). So if I am a beginner coming into NixOS, and I’ve found some mistakes or confusing language I think I could improve, I have to somehow figure out that I can find the NixOS documentation sources [here](https://github.com/NixOS/nixpkgs/tree/master/nixos/doc/manual), then I have to understand that I must make a PR for that change, which also assumes familiarity with git and github. And beyond that the process is still unclear–but apparently it’s not very efficient, considering [this](https://github.com/NixOS/nix/issues/2259), already mentioned above.

(Sorry–I should acknowledge that you _can_ easily figure out where the NixOS documentation lives in the `nixpkgs` repo as long as you’ve read all the way through to [chapter 41](https://nixos.org/nixos/manual/index.html#sec-writing-documentation) in the online documentation.)

## The Bigger Picture

There is more I could criticize–who doesn’t like to rant?–and there are other incidental factors that make Nix/NixOS hard to learn: lazy functional languages are not the easiest to build an intuition for, and eschewing the [Linux FHS](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard) certainly complicates getting comfortable with NixOS…as two examples.

It’s also important to note that there is often _not_ a standard approach to achieving certain ends using Nix. Especially when it comes to the Haskell infrastructure, I often found that there were ultimately multiple viable solutions to the problem I was trying to solve. The same is true for e.g. installing software using `nix-env` vs. adding a package to `environment.systemPackages` in `configuration.nix`, organizing your `configuration.nix` and dotfiles, etc. This is a feature, not a bug.

So while criticizing is fun and cathartic, it’s much harder to actually identify and implement solutions to these kinds of problems.

If I take a step back and imagine I were to be suddenly installed as the archetypal benevolent dictator of NixOS, faced with these issues, I think I’d enact these measures:

-   Create some kind of internal team with a mandate to improve the documentation and new-user-onboarding experience, and the authority to bypass some of the process currently required to update documentation. (The rest of these can therefore be considered the initial priorities for this team.)
-   Centralize documentation resources in a more obvious fashion, including how to update and add to the docs. Along with that effort, remove as much redundancy as possible.
-   Add some documentation around best practices for using and administering NixOS that bridges the gap between the Nix package manager manual and the NixOS manual.
-   Require reference documentation for the “internal APIs” scattered across `nixpkgs`, providing user-facing API docs for functions in `stdenv` like `callPackage` and `mkDerivation`, along with language-specific documentation for stuff like the [Haskell lib namespace](https://github.com/NixOS/nixpkgs/blob/master/pkgs/development/haskell-modules/lib.nix) I mentioned above, with the expectation that they are at least as comprehensive as those available for the [builtin functions](https://nixos.org/nix/manual/#ssec-builtins).

(And yes, I know [this](https://nixos.org/nixpkgs/manual/#sec-using-stdenv) exists and I know there are explanations of `callPackage` and much more in the Nix manual and Nix Pills–I’m talking about a consistent API reference a la Javadocs or Hackage or )

It seems like the majority of the issues I identified are a byproduct of the project being a community-driven endeavor, aside from the informal leadership of specific members of the community–with regard to documentation in particular. It’s not a result of incompetence or negligence, but reflects the challenges of taking a concept like NixOS from a dissertation through to a packaged product suitable for new users while maintaining a community-driven approach. And considering the target audience, which seems to be developers and system administrators with prior experience using Linux and a proclivity for functional programming, it’s not clear to me how much or what effort should be expended in this direction…leaving aside the question of how easy it would be to herd all the cats required to achieve any of my hypothetical dictates.

Finally, let me be clear that the community knows there are problems with the docs and that Nix/NixOS is hard to learn. Many [prominent](https://discourse.nixos.org/t/nixpkgs-library-function-documentation-doc-tests/1156) [members](https://discourse.nixos.org/t/season-of-docs-gsoc-for-documentation/2402) of the [community](https://discourse.nixos.org/t/install-report-from-new-user/1390) are committed to fixing this situation, to the best of their ability.

So rather than continuing to rant further, I’m going to switch gears, try to contribute something positive, and give you my take on how to use the _currently available_ documentation and resources to best ease yourself into NixOS.

## How to Start NixOS: an Opinionated Guide

_(as of early 2019)_

This assumes you are installing Linux as a new user of NixOS, but probably have some previous Linux experience. It assumes that you have no experience with either NixOS or the Nix package manager. I’m assuming also that you’re using this for a desktop machine, but much of my advice is probably still useful even if you have another goal for your installation.

### Before Installing

Go directly to the wiki and read the [introduction to NixOS](https://nixos.wiki/wiki/NixOS). You probably have a much better sense of NixOS after that, and if you were at all on the fence about installing it, maybe this has helped you decide.

Assuming this fills you only with enthusiasm for your future life with NixOS, then I recommend familiarizing yourself with a few more items before you dig into the install process:

-   [Introduction to the Nix package manager (wiki)](https://nixos.wiki/wiki/Nix)
-   [Cheatsheet/Comparison with Ubuntu](https://nixos.wiki/wiki/Cheatsheet)
-   [Administration of NixOS](https://nixos.org/nixos/manual/index.html#ch-running)
-   [Package Management](https://nixos.org/nix/manual/#chap-package-management)

(You can probably skip chapter 13 of the Package Management section for now, you’ll know it if you need it I expect)

I’m not imagining that you’ll absorb every bit of the documentation I’ve linked to above before installing, but simply having a sense of what is where during and immediately after the installation process will be very useful.

You may also want to poke around at the [option](https://nixos.org/nixos/options.html) and [package search](https://nixos.org/nixos/packages.html) pages simply to know those resources are there. The option search in particular will probably be crucial as you are setting your system up and customizing things to suit your needs.

This is probably also a good time to make sure you can ask questions on `#nixos` in IRC or get yourself an account on [`discourse.nixos.org`](https://discourse.nixos.org/). The community is very nice and a lot of folks are always ready to expend a lot of effort helping newbies out. If you are patient and respectful, and understand you may not always get the answer you’re looking for, you’ll get a lot of assistance.

### Installing

If you’ve gotten this far, hopefully you’ve already got a bunch of prerequisite knowledge that will head off some of the confusion and frustration I experienced the first time I installed NixOS!

(I still recommend you read to the end before you start.)

You can start by clicking the big green ‘Get NixOS’ button on the [NixOS home page](https://nixos.org/), which will lead you to the [download page](https://nixos.org/nixos/download.html). Assuming you’ve got that sorted, now is the time to start at the top of the [install manual](https://nixos.org/nixos/manual/).

Keep the links above handy, and you may want to keep a tab with the [wiki’s top page](https://nixos.wiki/) open, in particular so you can search for non-standard stuff like how to use [ZFS](https://nixos.wiki/wiki/NixOS_on_ZFS), [Nvidia Optimus cards](https://nixos.wiki/wiki/Nvidia), and more. Sometimes very useful troubleshooting information can be found in the wiki.

You’ll also probably find yourself referring to the [options search](https://nixos.org/nixos/options.html) a lot at this point. You may also find the [cheatsheet](https://nixos.wiki/wiki/Cheatsheet) and [Configuration Collection](https://nixos.wiki/wiki/Configuration_Collection) handy (searching on [‘configuration.nix’ and similar in github](https://github.com/search?utf8=%E2%9C%93&q=configuration.nix&type=) can be helpful as well when you need to find other examples of how to configure specific items).

Keep the [Arch Linux wiki](https://wiki.archlinux.org/) handy too. Considering the holes the NixOS documentation has with regard to actually configuring and using specific Linux packages, especially desktop software, the Arch Linux wiki has proven itself indispensable–in particular I found the [HiDPI](https://wiki.archlinux.org/index.php/Hidpi), [Xorg](https://wiki.archlinux.org/index.php/Xorg), and [ALSA](https://wiki.archlinux.org/index.php/Advanced_Linux_Sound_Architecture) pages very helpful, although I referred to a good number of other pages in this wiki as well during my install.

And of course, keep your search engine of choice handy for when you hit an edge case not addressed in any of the docs above.

### Post-Install

Hopefully you’ve now got a working NixOS installation and know how to take care of most common maintenance tasks you need to take care of, including installing and uninstalling software. Now may be a good time for a closer read of the [Nix manual](https://nixos.org/nix/manual) (still skipping the Installation section, probably), and the [Nix Pills](https://nixos.org/nixos/nix-pills/index.html) to really get an understanding of how it all fits together, and then branching off into specific topics, e.g. how to use [Haskell with Nix](https://github.com/Gabriel439/haskell-nix), seeing if [home-manager](https://github.com/rycee/home-manager) is up your alley, figuring out [how to play your Steam games on NixOS](https://nixos.wiki/wiki/Steam), and more.

I hope you enjoy NixOS!
