# Battle

## General notes

- Frames are twice as fast.
- Does not show message during animation
- Updates health at the end of each animation
- Opponents sprite is flipped

## Catching

- "threw a poke ball" and ball anim
- cloud "anim" (shows cloud for 2 frames)
- wobble anim n times
- outcome
    - on lose
        - cloud "anim"
        - show mon "almost had it"
        - fled
    - on win
        - stars anim ~5 frames
        - "was caught"

## Animations

- wobble has x=20 +/- 1, y=12, 4 frames each
- throw has 6 frames
- always wobbles once (? pretty sure)
- can wobble 3 times and break out

## RNG stuff

Seems like there's a percent chance each wobble based on its HP for it to break out.
If it reaches 3 wobbles without breaking out, it's caught.

