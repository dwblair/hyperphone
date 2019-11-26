# hyperphone

Experimenting with two-way audio using [hyperpipe](https://github.com/mafintosh/hyperpipe).

## Requirements

- arecord (standard on Ubuntu 18.04+) or other audio recorder that can stream data to stdout on the commandline
- aplay (standard on Ubuntu 18.04+) or other audio player that can play audo streams that are piped to it on the commandline
- [hyperpipe](https://github.com/mafintosh/hyperpipe)

## Audio "sender side" setup

On the **sender** side, run:

```
arecord | hyperpipe ./h1.db -t
```

This will begin recording an audio stream, which will be stored locally in a database (here we've named it "h1.db") and also streamed to a p2p networked 'hypercore' feed over the internet.  The address of this hypercore feed is given by a 64-character hypercore key that is printed to the command line.  You'll see something like:

```
Recording WAVE 'stdin' : Unsigned 8 bit, Rate 8000 Hz, Mono
f84fd19e0e76a1001ac250a4720bda1202cf6fda33683507c02ae9451a7457f1
```

The above hypercore key ("f84fd18e...") will be different in your case (it is randomly generated when the command first runs, but is associated with the particular database you've written to your computer now).

Your computer will be recording from the default mic set up on your system, and piping the audio output to this hyperpipe 'key' address.

## Audio "receiver side" setup

On the **receiver** side, run:

```
hyperpipe ./r1.db [hypercore key generated on sender side above] | aplay
```

Using the key in our example above, this would look like:

```
hyperpipe ./r1.db f84fd19e0e76a1001ac250a4720bda1202cf6fda33683507c02ae9451a7457f1 | aplay
```

This should take the live-streamed audio being recorded in "arecord" on the  **sender** side, and pipe it into "aplay" on the receiver side, playing it on the default audio output of your receiver computer.


