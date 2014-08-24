---
layout: post
title: "Music Producers should now know how to code"
description: "My brother is starting his career as a music producer. He called me because he needed help with JavaScript"
category: blog
---

I received a texto from my brother [@Yoann_Saunier](http://twitter.com/yoann_saunier) asking me if I knew about Java. He is a music producer, from the duet [Faggy Dux](http://www.faggydux.fr) so I found it quite weird.

Turns out that he is working on composing the music of a French TV series in Logic Pro X and needed a way
to reproduce the midi signal of channel 1 to all the other channels from 2 to 16. And Logic Pro gently
gave him a text editor to write code in!

It's not really Java, it's JavaScript, but I can't blame him for making the mistake. Here is the code
we pair programmed:

```js
/*
With Scripter, you can use JavaScript to create your own custom MIDI processing
effects, including slider controls for real-time interaction.

For detailed information about about using Scripter, including code samples,
see the MIDI plug-ins chapter of the Logic Pro X Instruments or
MainStage 3 Instruments manual.
*/

// This code will replicate all ControlChange events (modulation, etc.) read
// on channel 1 to all the channels from 2 to 15.
function HandleMIDI(event)
{
    if (event instanceof ControlChange && event.channel == 1) {
        for (var i=2; i<=16; i++)
        {
            var cc = new ControlChange;
            cc.channel = i;
            cc.number = event.number;
            cc.value = event.value;
            cc.trace();
            cc.send();
        }
    }
    event.trace();
    event.send();
}
```

For programmers, this is quite straigthforward, but for my brother, it was
like the graal. He told me that he lost 3 hours on the Internet searching if someone
had already done this, and just found a guy selling a bunch of scripts for 40 bucks. Well,
I guess musicians will need their stackoverflow quite soon.
