---
title: Enigma Machine
date: "2022-07-03"
thumb: "enigmamachine.png"
pos: "center"
lang: "c++"
---

<script>
    import MDVideo from "$components/MDVideo.svelte"
    import Collapse from "$components/Collapse.svelte";
    import EnigmaStruct from "$snippets/enigmastruct.md";
    import RotorEncryption from "$snippets/rotorencryption.md";
    import EnigmaEncryption from "$snippets/enigmaencryption.md";
    import ButtonLink from "$components/ButtonLink.svelte";
    import Button from "$components/Button.svelte";
</script>

<svelte:head>
<title>DavidB | Enigma Machine</title>
</svelte:head>

<Button href="/">
</Button>

# Enigma Machine Project
<section>

## Background
I got the inspiration to make an Enigma Machine after seeing the movie "The Imitation Game" and watching a few Computerphile youtube-videos that talked about how the machine worked and how it was cracked. My goal with the project was to make a machine that had rotorwheels you could manually rotate and swap out for different configs and to have a working lampboard that shows the encrypted letter. I did not look at any existing algorithms, instead I limited myself to the following resources:
<br>
<ButtonLink href="https://www.101computing.net/enigma-machine-emulator/">
    Emulator
</ButtonLink>
<ButtonLink href="https://www.ciphermachinesandcryptology.com/en/enigmatech.htm">
    Article
</ButtonLink>
<ButtonLink href="https://en.wikipedia.org/wiki/Enigma_rotor_details">
    Wikipedia page
</ButtonLink>

</section>

<section>

## How it works
A typical machine has an input board, plugboard, lampboard, three RotorWheels, and a reflector. The rotorwheels have unique configs but they behave like rotating substitution ciphers, so RotorWheel I in position 0 might take an A as input and output it as K, whereas another wheel might output the same as X. The wheels can be placed in any of the three positions, and the initial rotation offset and ringsetting (which shifts the wiring around) can also be changed.

<MDVideo width="640" height="360" src="/projectmedia/enigmamachine_rotorwheel.mp4" />

The first wheel rotates every time a key is pressed, and when it reaches a so called "turnover notch position" it will rotate the next wheel, which in turn will rotate the last wheel when it reaches it's own turnover position, similarly to an odometer. The rotation behaves like an index offset, which makes it so pressing the same key multiple times won't output the same letter multiple times in a row. At the end there is a "reflector" which has pairs of letters mapped together (where a regular wheel would map A to B but not have B mapped to A, the reflector maps A to B and B to A).


<Collapse title="Encryption example step by step">

- 1\. A is pressed
- 2\. Wheel 1 rotates and signal is sent from keyboard to Wheel 1
- 3\. Wheel 1 receives A and outputs C
- 4\. Wheel 2 receives C and outputs D
- 5\. Wheel 3 receives D and outputs F
- 6\. Reflector receives F and outputs S
- 7\. Wheel 3 receives S and outputs S
- 8\. Wheel 2 receives S and outputs E
- 9\. Wheel 1 receives E and outputs B
- 10\. **Lamp with letter B lights up**

<br>

- 1\. A is pressed
- 2\. Wheel 1 rotates and signal is sent from keyboard to Wheel 1
- 3\. Wheel 1 receives A and outputs D
- 4\. ...
- 10\. **Lamp with letter D lights up**

<br>

- 1\. A is pressed
- 2\. Wheel 1 rotates and signal is sent from keyboard to Wheel 1
- 3\. Wheel 1 receives A and outputs E
- 4\. ...
- 10\. **Lamp with letter Z lights up**

</Collapse>

## Implementation
For easy access and quick access to the interactible input key objects, lamps and the plugboard ports I chose to use a hashmap with structs.
I simulate the rotation of the wheels by incrementing an index offset, and the ring setting with a different offset which I use to change what letter gets returned by the encryption.
When a key is pressed I call EncodeLetter with the alphabet position of the letter as parameter. 
<Collapse title="EnigmaComponents struct">

<EnigmaStruct />

</Collapse>

<Collapse title="Individual Rotor encryption">

<RotorEncryption />
</Collapse>

<Collapse title="Machine encryption">

<EnigmaEncryption />

</Collapse>

</section>