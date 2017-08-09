# 8 - Pattern Recognition Through Time

## Pattern Matching Dolphin Whistles

Dolphin whistles serve as an identifier challenges include:

- Different base frequencies
- Warped in Time

### Different base frequencies

On solution is looking at delta frequency rather than directly at frequency

### Warping in Time

Thad us equivalent to Ttthhhhaaaadddd

One method is looking at Euclidean distance - take a faster sequence that has fewer samples and either 0-pad it or interpolate it to stretch it out. This isn't really sufficient.

### Dynamic Time Warp

Plot dynamic frequency from 2 signals along 2 axes. Match points along the axes to the point that best matches on the opposite axis. If the next option is equivalent try to stick as close to the diagonal as possible.

Calculating the Euclidean distance on the DTW can result in much smaller distances than zero padding

### Sakoe Chiba Bounds

DTW has a problem where high degree of warping could cause two signals that are actually different to have a much smaller distance than desired.

This can be "fixed" by forcing bounds on how much warping can occur

Bounds determined empirically and tested via cross validation

If you have different sections that have more or less variability you can try to dynamically set the Sakoe Chiba bounds however this can be complicated to train.

## HMM

HMMs are a useful tool for looking at pattern recognition through time.

They have states that represent a set of observed phenomena and transitions that describe how we can  move from one state to another.

First order Markov Models only depend on previous state and not a history of states.

What's hidden? We don't know which state or model produced the output. Each state can yield certain outputs and we determine which sequence of states based on how likely they were to produce an output.

Stick tally, context, stochastic grammars, and boosting improve performance.

### HMM Representation

A common representation is Markov chain. X_0 is a starting state and can transition to successive states X_1... X_t. Has a certain probability of Output represented by E_1... E_t.

Alternative representation has left to right sequence with self transitions (can't go backwards). Emission probabilities is the probability distribution of output values within a given state. Transition probabilities are the probabilities of transitioning from one state to another.

1 / (1 - p) where p is the transition probability gives expected time in a state.

### Sign Language recognition

In sign language HMMs can be designed around delta-X and delta-Y of hand position. Even when using a seemingly poor feature like delta Y to differentiate I and We in sign language, differences transition probabilities and emission distribution can yield large difference in overall probability of a signal "fitting" a model.

## Viterbi Trellis

Recognition using HMMs

1. Consider trellis representing all possible sequence of states to produce the output. (Constrain start to initial state and ending to end state)
2. Fill in transition  probabilities.
3. Fill in emission probability for each state producing the output at each sequence.

Various ways to determine best path:

- Greedy algorithm picks the highest expected valued (transition probability * s' emission probability)

### Training Viterbi

1. Start off splitting data into equal groups
2. Use EM to reallocate clusters based on means and standard deviations.
3. Repeat until convergence.


### Baum Welch

Similar to EM, but each data point contributes to each state proportionately to the probability of being in the state.

Rabainers theorem, Thad's Master thesis, looking at an HMM toolkit like HTK are good resources.

## Additional considerations for HMM

### Multidimensional data

Add extra dimensions to probabilities to deal with multi-dimensional data.

### Mixture of Gaussians

What if data isn't Gaussian? Central limit states data ought to be Gaussian but sometimes it isn't!

Mixture of Gaussians work well. With enough Gaussians you can fit any data! But to avoid overfitting visualization techniques can be used to check data and what's going on. Three Gaussians as a limit is a good rule of thumb.

### HMM Topologies 

HMMs don't need to be strictly left to right in a Markov chain. You could allow for loops in the chain. This is to account for repeating states i.e. in sign language repeating a portion of the movement can be done as an equivalent to verbal "umm" placeholders. Cross validation techniques with random independently selected test sets to settle on the best probabilities in the more complicated topology.

It's also possible for some signs to have one handed and two handed versions. It can be easier to recognize these as separate gestures, but have them map to the same "word"

### Phrase Recognition

Even with simple states per gesture, small vocab and strict grammar, there is a large number of states because you can the states for different words are arranged along one axis and time series along the other axis. This can be difficult to keep in memory (amounts to a breadth first search)

This be improved upon by using stochastic beam search - focus on high probability paths but don't drop low probability paths as they could eventually lead to good outcomes (i.e. if the person faltered in the first few signs like a stutter). paths are kept with probability proportional it's likelihood.

### Context training

Transitions between "I Need" is not the same as starting with need. Ideally you have enough training data to not only train on each individual words, but to also train on common phrases in a combined 6 states.

In spoken language this is co-articulation between two phoneme.

You can train on two-three word co-signs if you have a large enough data set (going through and doing Baum-Welch etc.) as the benefits can be significant. 

### Statistical Grammar

Bias recognition based on occurrence in data

i.e. if "I want" occurs 4% vs "I need" occurring 10% than bias "I need" when doing pattern recognition.

This can be applied in situations that have a language like nature even if language isn't readily apparent

### State Tying

Tying states between different words together can effectively provide extra data for training a particular state or sub-word. This can be done by identifying states that have a similar means and variances and potentially using domain knowledge i.e. thinking about the actual motion in sign language and seeing whether they do in fact appear similar.

### Segmentally Boosted HMMs

If you have many input features into HMMs, by themselves they can create a lot of noise that ultimately hurts the HMM. However boosting can be used to fix this.

Start by training HMMs normally via all of the features. Then identify for each state which features best differentiate a particular state from the rest of the states. Then weight the features based on the state.

This can be applied to generative and discriminative.

## Using HMMs to generate data

Early on there was the hope that HMMs could be used to generate data i.e. generate speech as well as discriminate speech. However it turned out to not be a good idea. This is due to HMM doesn't have a sense of continuity within the state.

To fix this we can  use more states to better order the output, but that would lead to over fitting. A better method is to use HMMs to model which state we are in and then use a different system to generate the data when we are in a particular state to generate the output data.
