Impartial two-player game, which means the state of the game itself determines the winning or losing the game. Also, the game has the perfect information that is no information is hidden from the players. Finally it is assumed that the game is finite, at some point one of the player will be in losing position. 

Since there is no draw, we can classify all the game states into either winning or losing. 

---
## Nim

> [!note] Theorem
> The current player has a winning strategy if and only if the xor-sum of the pile sizes is non-zero. The xor-sum of a sequence a is a1 ^ a2 ^ a3… ^ an.  

> [!seealso] proof
> The presence of the symmetric strategic for the opponent. Once in a position with the xor-sum equal to zero, the player won’t be able to make it non-zero in the long term. 
> 
> Let xor-sum  s = 0, consider any move. This move reduces the size of a pile x to y. t = s ^ x ^ y = 0 ^ x ^ y = x ^ y
> 
> Is s is not zero, 
> T = s ^ x ^ y, here we need to find out in which state y should be to make T 0 to make it to the losing state (in order to win). We then can conclude y = s ^ x to make T = 0. 


### Sprague-Grundy Theorem

Consider a state v of a two player impartial game and let vi be the states reachable from it. To this state, we can assign fully equivalent game of Nim with one pile of size x. the number x is called the grundy value or nim value of state v. 

The number can be found in the following recursive way, 
```
x = mex {x1, ......., xk}
```
Xi is the grundy value for state vi and the function mex is the smallest non negative integer not found in the given set. Grundy value being 0 means a state is losing.

> [!note] Proof
> For vertices without a move, the value x is the mex of an empty set, 0. 
> Consider any other vertex v. we assume the corresponding value x is already calculated.
> Then, we define P to be mex of the x values. Then we know for any integer that are within 0 and P-1,  there exists a reachable vertex with one grundy value. This means v is equivalent to the game state of Nim with one more pile of size P! Since P is the mex of the reachable values from the initial state! 