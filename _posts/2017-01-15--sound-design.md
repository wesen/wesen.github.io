---
layout: post
title: Sound design notes
category: articles
---

New year, and the new year's resolution is to post at least twice a month.
Procrastination is already kicking in, so this is mostly a recap of things
I have been experimenting with in the last few weeks.

I have gone back to spending a couple of hours everyday working in the DAW
or making sound design. I have been using massive, razor, molekular and form,
as well as laying out a few ideas in maschine and starting to sequence a track in S1.
I have also been patching quite a bit in Reaktor Blocks, an environment I still feel
a bit uncomfortable working with.

Attending the boston rust meetup also made me write a VST in rust, which was actually successful
(a surprise given how unproductive hack meetups can be), and it is definitely something
I want to pursue in the future.

## Massive sound design

Massive has grown to be one of my favourite synths, and one I now know how to navigate quite well.
It is good that NI is now pursuing a relatively standardized modulation scheme in their new
synthesizers, all of which are reaktor ensembles.

The main takeaway from the patches I have been making is to always set up macro knobs,
as they are a config file of sorts for the patch.
Not making macro assignments leads to patches being mostly dead when recalled a few weeks later.
The added advantage for NI synths is that the macro knobs are automatically mapped to the
first control page in Maschine.

I have made a bunch of pads and a bunch of basses, trying to keep focused on something usable melodically
instead of just straight weird sounding. You can download <a href="{{ site.url }}{{ site.baseurl }}/files/2017-01-15--massive-patches.zip">the massive patches bhere</a>.

## Razor sound design

One of my favourite synths to build for is razor, because it is so clean sounding and so easy to digress into weird ethereal soundscapes.
Nothing special to note here, and I really appreciate the new snapshot system in reaktor that doesn't require you to save the ensemble anymore,
but instead stores your presets as nrkt files in your Documents folder. You can download <a href="{{ site.url }}{{ site.baseurl }}/files/2017-01-15--razor-patches.zip">the razor patches bhere</a>.

## Molekular sound design

Realizing that a lot of minimal techno is creating interesting textures and rhythms with delays and reverb,
I decided to revive my love for molekular, and design more presets from scratch. I haven't got enough presets to publish yet, but it is an amazing effect/instrument.
This time around I had no procrastination problem in creating morph targets and macro knob assignments.

## Form sound design

It took me a while to warm up to this synthesizer, especially with its high CPU usage, but it turns out to be an instamagic turn a sample into a warm sounding bass or pluck or pad.
The effect section is fairly standard, but the oscillator waveshaping section is magic. The modulation scheme is very close to Razor/Molekular and thus quite intuitive.
A nice added feature is that you can select a name for your macro knobs from a variety of predefined labels. You can download <a href="{{ site.url }}{{ site.baseurl }}/files/2017-01-15--form-patches.zip">the form patches bhere</a>.

## Reaktor blocks patching

I have been increasing the amount of Reaktor blocks patches, tagging them with date and making a fair amount
of presets for them. I made a few monophonic synths, blocks allowing me to add grit / weird effects that
are more less fun to put together using vsts.
One of those synths called <a href="https://www.native-instruments.com/en/reaktor-community/reaktor-user-library/entry/show/10783/">monarkdust</a>
is very useful to make nice warm crackly synth leads.

I have been experimenting more with sequencers,
but still get lost easily when not writing out how things are patched together.
I realize that the XY ensemble for example writes down simple modulation / pitch inputs
in the titles of the individual modules.
I will also try to add "macro" knobs to the patches to make it easier to remember what is
nice to tweak. Not entirely sure if that is going to bypass the blocks approach (modulation wires),
or I will add some CV manipulation blocks.

It is much easier to keep focus on minimal techno when making sequencer driven modular tracks,
and I will try to focus on that in the next few weeks. Here is a simple sketch using a blocks patch and molekular:

<a class="embedly-card" data-card-key="20f0cfdf827a49e4b11f6cf34aeedf08" data-card-controls="0" href="https://clyp.it/zyk1jtlx">blocks phys modelling techno</a>

## Techno making

One of the things I want to pursue is get away from the sample mashing workflow
I have been using last year, which resulted in maximalist melodic/vocal sample
techno tracks, to a more focused and minimal sound. Of course, this is much harder
than it sounds. Here are the sketches that are not entire rubbish I produced over the
last month.

I want to take at least a few of those to full tracks. I have start on one of them,
and ended up making a full mess of it in terms of progression. Things are modulated too fast,
too much, without a clear direction. The best is probably to scrap the entire track into
the scratchpad, and start afresh.

<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

<a class="embedly-card" data-card-key="20f0cfdf827a49e4b11f6cf34aeedf08" data-card-controls="0" href="https://clyp.it/4w0vnzk0">idea 1 - heagonz3</a>

<a class="embedly-card" data-card-key="20f0cfdf827a49e4b11f6cf34aeedf08" data-card-controls="0" href="https://clyp.it/lkztzqjn">idea 2 - hexganoz</a>

<a class="embedly-card" data-card-key="20f0cfdf827a49e4b11f6cf34aeedf08" data-card-controls="0" href="https://clyp.it/3apxxkam">idea 3 motorbeat2</a>

<a class="embedly-card" data-card-key="20f0cfdf827a49e4b11f6cf34aeedf08" data-card-controls="0" href="https://clyp.it/m5izpda5">idea 4 - motorbeatring</a>

<a class="embedly-card" data-card-key="20f0cfdf827a49e4b11f6cf34aeedf08" data-card-controls="0" href="https://clyp.it/yjcdroue">idea 5 - teczno</a>

<a class="embedly-card" data-card-key="20f0cfdf827a49e4b11f6cf34aeedf08" data-card-controls="0" href="https://clyp.it/es53cap4">idea 6 - teczno2</a>

<a class="embedly-card" data-card-key="20f0cfdf827a49e4b11f6cf34aeedf08" data-card-controls="0" href="https://clyp.it/00sw0e0s">idea 7 - oonz</a>

<a class="embedly-card" data-card-key="20f0cfdf827a49e4b11f6cf34aeedf08" data-card-controls="0" href="https://clyp.it/v2nwxub4">idea 8 - carioca</a>

<a class="embedly-card" data-card-key="20f0cfdf827a49e4b11f6cf34aeedf08" data-card-controls="0" href="https://clyp.it/lrvvcjlq">idea 10 - bass zhiffle </a>

<a class="embedly-card" data-card-key="20f0cfdf827a49e4b11f6cf34aeedf08" data-card-controls="0" href="https://clyp.it/p1aq351i">idea 11 - boomb</a>

<a class="embedly-card" data-card-key="20f0cfdf827a49e4b11f6cf34aeedf08" data-card-controls="0" href="https://clyp.it/cdxw3hfw">idea 12 - boom</a>
