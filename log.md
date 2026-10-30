# Learning log

## Week 1
- Mon: environment setup
- Tue: Git basics
- Wed: GitHub repo

Q: Why does each Value store its children and the operation that made it?
A: The terms children can be a bit misleading. I like to think of it as parents, because in forqrd propagation these are actually nodes that precede the "current value node". It's imporant to store this data in the Value because this is how we know "Where" to backpropagate from the Value node. The operation tells us "How" we backpropagate. 

Q: What does x.grad mean? (Hint: the effect of a small nudge in x on the final output.)
A: It's the global effect of a  small nudge in x to the loss fucntion. If it's negative, incresing x decreases the loss.


Q: For out = a * b, what are a.grad and b.grad in terms of a.data, b.data, and out.grad? Why is it “local derivative times the incoming gradient”?
a.grad is d(out)/da = d(out)/d(a*b) d(a*b)/d(a) = out.grad * data.b
In this case the local derivative is data.b and the incoming gradient is out.grad



Q: Why do gradients accumulate with +=? Make up a tiny example like b = a + a.
The reason is that, if it was =, each time the node was evaluated it would reset the over-write the grad property of the node. But in cases where a certain node is visited more than once, it needs to accumulate the gradients, same as in the derivative of multivariable functions. For example, in the case of b = a + a, if we evaluated b.backward() wit = instead of +=, we would get that a.grad = 1 and then a.grad = 1, which means a.grad = 1. Of course it's incorrect, since the derivatve of b with respect to a is 2. If we change it from = to =+ we can accumulate the two 1s, to have 2.


Q: Why do we need a topological sort before calling _backward()?
A: In order for the x._backward() to work, all of the gradient propogating from it must be detremined before we make the calculation. This notion of order (before/after a node) requires a topological sort of the nodes in the net, otherwise, the direction of the propagation won't be defined. 

