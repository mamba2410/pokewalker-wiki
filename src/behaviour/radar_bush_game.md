# Radar Bush Game

## Walker behaviour

- Choose mon
  - check for event
    - check if we already have mon
      - check steps
      - check percent
      - exclamation level = 3+rand()%2
  - check option a > b > c
  - check steps
  - check percent (same rand() val)
  - exclamation level = 3-idx+rand()%2
  - always have at least option c
- mark active timer = 5 ticks
- timeout timer = 0x40
- active bush = rand()%4
- substate user choose bush
  - dec mark timer
  - if mark timer up, dec timeout timer
  - on enter, if cursur is on active bush, move to substate handle ok bush
  - else move to failed global state
  - set message timeout to 10 ticks
- substate radar handle ok bush
  - dec message timeout
  - if exclamation level == chosen level
    - set show message
    - reset bar animation frame
    - switch to battle start state
  - mark active timer = 16+rand()/stuff
  - inc exclamation level
  - active bush = rand()%4
  - switch to choosing state
- substate user failure (I don't know if this ever gets called, on failure we go to separate global state)
  - show received message (??)
  - wait for input
  - send to splash
- substate battle start
  - 4 ticks to black out screen
  - send to battle state
  