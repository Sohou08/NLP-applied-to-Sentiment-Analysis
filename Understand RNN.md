## How does RNN work ? 

The idea behind RNNs is to make use of sequential information. In a traditional neural network we assume that all inputs (and outputs) are independent of each other. But for many tasks that’s a very bad idea. If you want to predict the next word in a sentence you better know which words came before it. RNNs are called recurrent because they perform the same task for every element of a sequence, with the output being depended on the previous computations. Another way to think about RNNs is that they have a “memory” which captures information about what has been calculated so far. 

Here is what a typical RNN looks like described too how our recurrent network is designed :


![Untitled Diagram](https://user-images.githubusercontent.com/51121757/83359559-d9915100-a372-11ea-8656-b5f063d66c43.png)
The neural network cannot work directly on text-strings so we must convert it somehow. There are two pre-processing step required before feed the neural network. The first one is called “tokenizer” which converts words to integer. The second one is an integrated part of the neural network itself called the "embedding-layer”. This one converts each integer-token from the previous step into a vector values. 
That is necessary because for example in your case the integer-tokens may take on values between 0 and 10000 for a vocabulary of 10000 words. RNN alone cannot work on values in such a wide range. The embedding-layer is trained as a part of the RNN and will learn to map words with similar semantic meanings to similar embedding-vectors

To better understand how the RNN work, we will describe the input as a text word. But don’t forget that this is the pre-processing values (embedding) which is used.

The input sequence generally called RU1 (recurrent unit) receive a text-word. The initial memory-state of the RU1 is reset to zero internally by Keras / TensorFlow every time a new sequence begins. In our case, the word "this" is input to the RU1 which uses its internal state (initialized to zero) and its gate to calculate the new state. The next word is also processed by the same as the first and so forth. 

Therefore as there is not much meaning in the words "this is" so the RU1 probably doesn't save anything important in its internal state from seeing these words. But in terms of process, instead of outputting a single summary value at the end of the sequence, we use the output of RU1 for every time-step. This creates a new sequence that can be used as input for the next recurrent unit RU2. 
When it sees the third word "not" the RU1 has learned that it may be important for determining the overall sentiment of the input-text, so it needs to be stored in the memory-state of the RU1, which can be used later when the RU1 sees the word "good" in time-step 6.
The same process is repeated for the second layer and this creates a new output sequence which is then input to the third layer's recurrent unit RU3. 

When the entire sequence has been processed, you obtains an output vector (RU3) of the last layer  which summarizes what it has seen in the input sequence. Then a fully-connected layer with a Sigmoid activation require to be used in order to get a single value between 0 and 1. These values will be interpreted respectively as the sentiment either being negative (values close to 0) or positive (values close to 1).


We’re going to set up a small test in a movies data for better comprehension of these concept. 

