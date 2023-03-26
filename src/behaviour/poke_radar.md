# PokeRadar

## Choosing pokemon

- Checks for event pokemon first
  - if rand() < chance, choose event mon
  - else choose route 3 mon
- get new rand()
- Check for normal pokemon
  - start at option A (rarest)
  - check for steps
  - check if same rand() < chance
  - if both pass, choose that option mon
  - else continue
- substate_y = index+1 (probably found pokemon id)
- substate_b = randomly add 1 to index+1
  - This means that even if it chose eg 0, there's a 50%
    chance it'll actually be 1 instead
  - Same applies for event mon? Skips event check?