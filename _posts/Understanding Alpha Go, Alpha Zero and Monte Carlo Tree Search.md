# Understanding Alpha Go, Alpha Zero and Monte Carlo Tree Search

## Basic

* [Search: Games, Minimax, and Alpha-Beta](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-034-artificial-intelligence-fall-2010/lecture-videos/lecture-6-search-games-minimax-and-alpha-beta/) - lecture by Patrick H. Winston
* [Monte Carlo Tree Search](http://mcts.ai/about/)

## Monte Carlo Tree Search

### Upper Confidence Bound applied to trees(UCT)

$$
\mathbb{UCT}(v_i, v) = \frac{Q(v_i)}{N(v_i)} + c \sqrt{\frac{\log(N(v))}{N(v_i)}}
$$

​	where $v_i$ is the child node of $v$, $c$ is the trade-off factor of exploration and exploitation. $Q(v)$ is the total simulation reward and $N(v)$ is the total number of visits.

​	The searching procedure is expanded by UCT policy. In UCT, the first item is called win ratio, which is **exploitation** component, and the second one is **exploration** component.



## Alpha Go & Alpha Zero





## References

[Monte Carlo Tree Search – beginners guide](https://int8.io/monte-carlo-tree-search-beginners-guide/)

