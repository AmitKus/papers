# <small> Gradient descent variants: </small>
 
##

There are 3 variants of gradient descent 

1. Batch gradient descent:
    - Computes gradient of the cost function w.r.t to the parameters $\theta$ for the entire training dataset:
    - $\theta = \theta - \eta.\triangledown_{\theta}J(\theta)$
    - Computational and memory intensive
    - Doesn't allow online model update
    - Guaranteed to converge to the global minimum for convex error surfaces.

##

2. Stochastic gradient descent:
   - Performs a parameter update for $each$ training example $x^i$ and label $y^i$:
   - $\theta = \theta - \eta.\triangledown_{\theta}J(\theta; x^i; y^i)$
   - SGD performs frequent update with a high variance that cause the objective function to fluctuate heavily.
   - The fluctuations help SGD to jump to better local minima
   - When we slowly decrease the learning rate, SGD shows same convergence behavior as batch gradient descent.

##

3. Mini-batch gradient descent:
    - Performs an update for every mini-batch
    - $\theta = \theta - \eta.\triangledown_{\theta}J(\theta; x^{(i:i+n)}; y^{(i:i+n)})$
    - Typical batch size between 50 and 256
    - reduces the variance of the parameter updates which can lead to more stable convergence
    - typically the algorithm of choice when training NN


# <small> Challenges with vanilla Mini-batch gradient descent. </small>

##

1. Choosing a proper learning rate
2. Learning rate schedules: need to be defined in advance
3. The same learning rate applies to all parameters.
   1. In case of sparse data or features having different frequencies, we might not want to update all of them to the same extent but perform a larger update for rarely occurring features.
4. Getting stuck on saddle points.
   1. Saddle points are usually surrounded by a plateau of same error.


# <small> Modifications to Gradient descent optimization algorithms </small>

##

* Momentum: Momentum is a method that helps accelerate SGD in the relevant direction and dampens oscillations. 
  * $v_t = \gamma v_{t-1} + \eta\triangledown_{\theta}J\left( \theta \right)$
  * $\theta = \theta - v_t$
  * $\gamma \approx 0.9$
* Intuition: The momentum term increases for dimensions whose gradients point in the same directions and reduces updates for dimensions whose gradients change directions. Ball rolling down the hill accumulating momentum, becoming faster and faster on the way.

##

* Nesterov accelerated gradient (NAG): Update on the momentum term.
  *  $v_t = \gamma v_{t-1} + \eta\triangledown_{\theta}J\left( \theta - \gamma v_{t-1}\right)$
  *  $\theta = \theta - v_t$

##

* Adagrad: Adagrad is an algorithm for gradient-based optimization that does just this: It adapts the learning rate to the parameters, performing larger updates for infrequent and smaller updates for frequent parameters.
  *  It is well-suited for dealing with sparse data.
  *  $g_{t,i} = \triangledown_{\theta_t} J\left( \theta_{t,i} \right)$
  *  $\theta_{t+1,i} = \theta_{t,i} - \eta g_{t,i}$
  *  $\theta_{t+1,i} = \theta_{t,i} - \frac{\eta}{\sqrt{G_{t,ii} + \epsilon}} g_{t,i}$
  *  Where $G_t$ is a diagonal matrix where each diagonal elementi, $ii$s the sum of the squares of the gradients w.r.t. $\theta_i$ up to time step.

##

  *  One of Adagrad’s main benefits is that it eliminates the need to manually tune the learning rate.
  *  Adagrad’s main weakness is its accumulation of the squared gradients in the denominator: Since every added term is positive, the accumulated sum keeps growing during training. This in turn causes the learning rate to shrink and eventually become infinitesimally small, at which point the algorithms no longer able to acquire additional knowledge.

##

Adadelta: 
- It's an extension of Adagrad that seeks to reduce its aggressive, monotonically decreasing learning rate. 
- Instead of accumulating all past squared gradients, Adadelta restricts the window of accumulated past gradients to some fixed size $w$.
- With Adadelta, we do not even need to set a default learning rate, as it has been eliminated from theupdate rule
- $\Delta \theta_t = -\frac{RMS[\Delta \theta]_{t-1}}{RMS[g]_t}$
- $\theta_{t+1} = \theta_t + \Delta \theta_t$

##

Adam (Adaptive Moment Estimation):
- Another method that computes adaptive learning rates for each parameter.
- In addition to storing an exponentially decaying average of past squared gradients $v_t$ like Adadelta and RMSprop, Adam also keeps an exponentially decaying average of past gradients $m_t$, similar to momentum:
- $m_t = \beta_1 m_{t-1} + (1 - \beta_1) g_t$
- $v_t = \beta_2 v_{t-1} + (1 - \beta_2) g_t^2$
- $\hat{m_t} = \frac{m_t}{1 - \beta_1^t}$
- $\hat{v_t} = \frac{v_t}{1 - \beta_2^t}$
- $m_t$ and $v_t$ are estimates of the first moment (mean) and the second moment (the uncentered variance) of the gradients respectively.
- Update rule: $\theta_{t+1} = \theta_t - \frac{\eta}{\sqrt{v_t} + \epsilon}\hat{m_t}$

## 

Nadam (Nesterov-accelerated Adaptive Moment Estimation):
- Adam = RMSprop + momentum
- RMSprop contributes the exponentially decaying average of past squared gradients $v_t$
- momentum accounts for the exponentially decaying average of past gradients $m_t$.
- Nesterov accelerated gradient (NAG) is superior to vanilla momentum.
- Nadam = Adam + NAG


# <small> Which optimizer to use? </small>

##

- Sparse input data: one of the adaptive learning-rate methods
- RMSprop, Adadelta, and Adam are very similar algorithms that do well in similar circumstances.
- Adam might be the best overall choice.

# <small> More strategies to optimize SGD </small>

##

- Curriculum learning: supplying the training examples in a meaningful order may actually lead to improved performance and better convergence.
- Batch normalization: 
  - Starting with normalized parameter values (zero mean and unit variance), we loose the normalization as training progresses.
  - BN reestablishes these normalizations for every mini-batch 
  - Can also act as regularizer, reducing the need for dropout.
- Early stopping:
  - Early stopping (is) beautiful free lunch
- Gradient noise:
  - Add Gaussian noise to each gradient update: $g_{t,i} = g_{t,i} + N(0, \sigma_t^2)$